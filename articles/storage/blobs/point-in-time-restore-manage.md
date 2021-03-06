---
title: Aktivieren und Verwalten der Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie Point-in-Time-Wiederherstellung (Vorschau) verwenden, um Blockblobs mit einem Zustand zu einem früheren Zeitpunkt wiederherzustellen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/11/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9a4c68454807cb26ac62799b598f146680e37c42
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89230176"
---
# <a name="enable-and-manage-point-in-time-restore-for-block-blobs-preview"></a>Aktivieren und Verwalten der Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)

Sie können Point-in-Time-Wiederherstellung (Vorschau) verwenden, um Blockblobs mit einem Zustand zu einem früheren Zeitpunkt wiederherzustellen. In diesem Artikel wird beschrieben, wie Sie Point-in-Time-Wiederherstellung für ein Speicherkonto mit PowerShell aktivieren. Außerdem wird gezeigt, wie Sie einen Wiederherstellungsvorgang mit PowerShell ausführen.

Weitere Informationen sowie Informationen zum Registrieren für die Vorschau finden Sie unter [Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)](point-in-time-restore-overview.md).

> [!CAUTION]
> Point-in-Time-Wiederherstellung unterstützt nur Wiederherstellungsvorgänge für Blockblobs. Vorgänge für Container können nicht wiederhergestellt werden. Wenn Sie einen Container aus dem Speicherkonto löschen, indem Sie den Vorgang [Delete Container](/rest/api/storageservices/delete-container) während der Vorschau der Point-in-Time-Wiederherstellung aufrufen, kann dieser Container nicht mit einem Wiederherstellungsvorgang wiederhergestellt werden. Löschen Sie während der Vorschau die einzelnen Blobs, anstatt einen Container zu löschen, wenn Sie sie möglicherweise wiederherstellen möchten.

> [!IMPORTANT]
> Die Vorschau von Point-in-Time-Wiederherstellung ist nur für die Verwendung außerhalb der Produktion bestimmt. Produktions-SLAs (Service Level Agreements, Vereinbarungen zum Servicelevel) sind derzeit nicht verfügbar.

## <a name="install-the-preview-module"></a>Installieren des Vorschaumoduls

Um Azure Point-in-Time-Wiederherstellung mit PowerShell zu konfigurieren, installieren Sie zunächst Version 1.14.1-preview oder eine spätere Version des Moduls Az.Storage. Die Verwendung der aktuellen Vorschauversion wird empfohlen, die Point-in-Time-Wiederherstellung wird jedoch ab Version 1.14.1-preview unterstützt. Entfernen Sie alle anderen Versionen des Moduls Az.Storage.

Mit dem folgenden Befehl wird das Modul Az.Storage [2.0.1-preview](https://www.powershellgallery.com/packages/Az.Storage/2.0.1-preview) installiert:

```powershell
Install-Module -Name Az.Storage -RequiredVersion 2.0.1-preview -AllowPrerelease
```

Der obige Befehl erfordert die Installation von Version 2.2.4.1 oder höher von PowerShellGet. So ermitteln Sie, welche Version Sie zurzeit geladen haben
```powershell
Get-Module PowerShellGet
```
Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren von Azure PowerShell mit PowerShellGet](/powershell/azure/install-az-ps).

## <a name="enable-and-configure-point-in-time-restore"></a>Aktivieren und Konfigurieren von Point-in-Time-Wiederherstellung

Bevor Sie die Point-in-Time-Wiederherstellung aktivieren und konfigurieren, aktivieren Sie die Voraussetzungen für das Speicherkonto: vorläufiges Löschen, Änderungsfeed und Blobversionsverwaltung. Weitere Informationen zum Aktivieren dieser Funktionen finden Sie in den folgenden Artikeln:

- [Aktivieren von „Vorläufiges Löschen“ für Blobs](soft-delete-enable.md)
- [Aktivieren und Deaktivieren des Änderungsfeeds](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)

Um Azure Point-in-Time-Wiederherstellung mit PowerShell zu konfigurieren, müssen Sie den Befehl Enable-AzStorageBlobRestorePolicy aufrufen. Im folgenden Beispiel wird vorläufiges Löschen aktiviert, die Beibehaltungsdauer für vorläufiges Löschen festgelegt, der Änderungsfeed aktiviert und dann die Point-in-Time-Wiederherstellung aktiviert. Verwenden Sie vor dem Ausführen des Beispiels das Azure-Portal oder eine Azure Resource Manager-Vorlage, um auch Blobversionsverwaltung zu aktivieren.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen, wenn Sie das Beispiel ausführen:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set resource group and account variables.
$rgName = "<resource-group>"
$accountName = "<storage-account>"

# Enable soft delete with a retention of 6 days.
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RetentionDays 6

# Enable change feed.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EnableChangeFeed $true

# Enable point-in-time restore with a retention period of 5 days.
# The retention period for point-in-time restore must be at least one day less than that set for soft delete.
Enable-AzStorageBlobRestorePolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RestoreDays 5

# View the service settings.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

## <a name="perform-a-restore-operation"></a>Ausführen eines Wiederherstellungsvorgangs

Um einen Wiederherstellungsvorgang zu initiieren, rufen Sie den Befehl **Restore-AzStorageBlobRange** auf, und geben Sie dabei den Wiederherstellungspunkt als **DateTime**-Wert in UTC an. Sie können lexikografische Bereiche von Blobs zum Wiederherstellen angeben oder keinen Bereich angeben, um alle Blobs in allen Containern im Speicherkonto wiederherzustellen. Pro Wiederherstellungsvorgang werden bis zu 10 lexikografische Bereiche unterstützt. Seitenblobs und Anfügeblobs sind in der Wiederherstellung nicht enthalten. Der Wiederherstellungsvorgang kann mehrere Minuten dauern.

Beachten Sie die folgenden Regeln, wenn Sie einen Bereich von Blobs für die Wiederherstellung angeben:

- Das für den Startbereich und den Endbereich angegebene Containermuster muss mindestens drei Zeichen umfassen. Der Schrägstrich (/), der zum Trennen eines Containernamens von einem Blobnamen verwendet wird, wird bei diesem Mindestwert nicht berücksichtigt.
- Pro Wiederherstellungsvorgang können bis zu 10 Bereiche angegeben werden.
- Platzhalterzeichen werden nicht unterstützt. Sie werden als Standardzeichen behandelt.
- Sie können Blobs in den `$root`- und `$web`-Containern wiederherstellen, indem Sie sie explizit in einem Bereich angeben, der an einen Wiederherstellungsvorgang übergeben wird. Die `$root`- und `$web`-Container werden nur wiederhergestellt, wenn sie explizit angegeben werden. Andere Systemcontainer können nicht wiederhergestellt werden.

> [!IMPORTANT]
> Wenn Sie einen Wiederherstellungsvorgang ausführen, blockiert Azure Storage für die Dauer dieses Vorgangs Datenvorgänge für die Blobs in den Bereichen, die wiederhergestellt werden. Lese-, Schreib- und Löschvorgänge werden am primären Speicherort blockiert. Aus diesem Grund werden Vorgänge wie das Auflisten von Containern im Azure-Portal während des Wiederherstellungsvorgangs möglicherweise nicht erwartungsgemäß ausgeführt.
>
> Lesevorgänge aus dem sekundären Speicherort können während des Wiederherstellungsvorgangs fortgesetzt werden, wenn das Speicherkonto georepliziert wird.

### <a name="restore-all-containers-in-the-account"></a>Wiederherstellen aller Container im Konto

Wenn Sie alle Container und Blobs im Speicherkonto wiederherstellen möchten, rufen Sie den Befehl **Restore-AzStorageBlobRange** auf, wobei Sie den Parameter `-BlobRestoreRange` auslassen. Im folgenden Beispiel wird der Zustand von Containern im Speicherkonto 12 Stunden vor dem aktuellen Zeitpunkt wiederhergestellt:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddHours(-12)
```

### <a name="restore-a-single-range-of-block-blobs"></a>Wiederherstellen eines einzelnen Bereichs von Blockblobs

Um einen Bereich von Blobs wiederherzustellen, rufen Sie den Befehl **Restore-AzStorageBlobRange** auf, und geben Sie einen lexikografischen Bereich von Container- und Blobnamen für den Parameter `-BlobRestoreRange` an. Der Anfang des Bereichs ist inklusiv, das Ende des Bereichs ist exklusiv.

Wenn Sie z. B. die Blobs in einem einzelnen Container mit dem Namen *sample-container* wiederherstellen möchten, können Sie einen Bereich angeben, der mit *sample-container* beginnt und mit *sample-container1* endet. Es ist nicht erforderlich, dass die in den Anfangs- und Endbereichen genannten Container vorhanden sind. Da das Ende des Bereichs exklusiv ist, wird nur der Container mit dem Namen *sample-container* wiederhergestellt, auch wenn das Speicherkonto einen Container mit dem Namen *sample-container1* enthält:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container -EndRange sample-container1
```

Um eine Teilmenge der Blobs in einem Container für die Wiederherstellung anzugeben, verwenden Sie einen Schrägstrich (/), um den Containernamen vom Blobmuster zu trennen. Beispielsweise wählt der folgende Bereich Blobs in einem einzelnen Container aus, deren Namen mit den Buchstaben *d* bis *f* beginnen:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container/d -EndRange sample-container/g
```

Geben Sie als Nächstes den Bereich für den Befehl **Restore-AzStorageBlobRange** an. Geben Sie den Wiederherstellungspunkt an, indem Sie einen **DateTime**-Wert (in UTC) für den `-TimeToRestore`-Parameter angeben. Im folgenden Beispiel werden Blobs im angegebenen Bereich in dem Zustand wiederhergestellt, den sie 3 Tage vor dem aktuellen Zeitpunkt aufgewiesen haben:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -BlobRestoreRange $range `
    -TimeToRestore (Get-Date).AddDays(-3)
```

### <a name="restore-multiple-ranges-of-block-blobs"></a>Wiederherstellen mehrerer Bereiche von Blockblobs

Wenn Sie mehrere Bereiche von Blockblobs wiederherstellen möchten, geben Sie ein Array von Bereichen für den `-BlobRestoreRange`-Parameter an. Pro Wiederherstellungsvorgang werden bis zu 10 Bereiche unterstützt. Im folgenden Beispiel wird durch die Angabe zweier Bereiche der gesamte Inhalt von *container1* und *container4* wiederhergestellt:

```powershell
# Specify a range that includes the complete contents of container1.
$range1 = New-AzStorageBlobRangeToRestore -StartRange container1 -EndRange container2
# Specify a range that includes the complete contents of container4.
$range2 = New-AzStorageBlobRangeToRestore -StartRange container4 -EndRange container5

Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-30) `
    -BlobRestoreRange @($range1, $range2)
```

### <a name="restore-block-blobs-asynchronously"></a>Asynchrones Wiederherstellen von Blockblobs

Fügen Sie zum asynchronen Ausführen eines Wiederherstellungsvorgangs dem Aufruf von **Restore-AzStorageBlobRange** den Parameter `-AsJob` hinzu, und speichern Sie das Ergebnis des Aufrufs in einer Variable. Der Befehl **Restore-AzStorageBlobRange** gibt ein Objekt vom Typ **AzureLongRunningJob** zurück. Sie können die **Status**-Eigenschaft dieses Objekts überprüfen, um zu ermitteln, ob der Wiederherstellungsvorgang abgeschlossen wurde. Der Wert der **State**-Eigenschaft kann **Wird ausgeführt** oder **Abgeschlossen** lauten.

Im folgenden Beispiel wird gezeigt, wie Sie einen Wiederherstellungsvorgang asynchron aufrufen:

```powershell
$job = Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-5) `
    -AsJob

# Check the state of the job.
$job.State
```

Wenn Sie nach dem Ausführen auf den Abschluss des Wiederherstellungsvorgangs warten möchten, rufen Sie den Befehl [Wait-Job](/powershell/module/microsoft.powershell.core/wait-job) auf, wie im folgenden Beispiel gezeigt:

```powershell
$job | Wait-Job
```

## <a name="known-issues"></a>Bekannte Probleme
- Wenn in einer Teilmenge der Wiederherstellungen Anfügeblobs vorhanden sind, tritt bei der Wiederherstellung ein Fehler auf. Führen Sie vorläufig keine Wiederherstellungen aus, wenn Anfügeblobs im Konto vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

- [Point-in-Time-Wiederherstellung für Blockblobs (Vorschau)](point-in-time-restore-overview.md)
- [Vorläufiges Löschen](soft-delete-overview.md)
- [Änderungsfeed (Vorschau)](storage-blob-change-feed.md)
- [Blobversionsverwaltung](versioning-overview.md)
