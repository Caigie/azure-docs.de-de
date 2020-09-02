---
title: Blobmomentaufnahmen
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie eine schreibgeschützte Blobmomentaufnahme erstellen, um Blobdaten zu einem bestimmten Zeitpunkt zu sichern.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/19/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 4c6c2774e0d71ec33449565efab797c040aa264f
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88640598"
---
# <a name="blob-snapshots"></a>Blobmomentaufnahmen

Eine Momentaufnahme ist eine schreibgeschützte Version eines Blobs, die zu einem bestimmten Zeitpunkt erstellt wird.

> [!NOTE]
> Die Blobversionsverwaltung (Vorschau) bietet eine alternative Möglichkeit zum Verwalten von früheren Versionen eines Blobs. Weitere Informationen finden Sie unter [Versionsverwaltung (Vorschau)](versioning-overview.md).

## <a name="about-blob-snapshots"></a>Informationen zu Blobmomentaufnahmen

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

Eine Momentaufnahme eines Blobs ist mit dem dazugehörigen Basisblob bis auf die Ausnahme identisch, dass an den Blob-URI ein **DateTime**-Wert angefügt ist. Hiermit wird der Zeitpunkt angegeben, zu dem die Momentaufnahme erstellt wurde. Wenn der Seitenblob-URI `http://storagesample.core.blob.windows.net/mydrives/myvhd` ist, lautet der Momentaufnahmen-URI z.B. in etwa `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Für alle Momentaufnahmen wird der URI des Basisblobs verwendet. Der einzige Unterschied zwischen dem Basisblob und der Momentaufnahme ist der angefügte **DateTime** -Wert.
>

Für ein Blob kann eine beliebige Anzahl von Momentaufnahmen vorhanden sein. Momentaufnahmen bleiben bestehen, bis sie explizit gelöscht werden. Die Löschung kann individuell oder im Rahmen des Vorgangs [Blob löschen](/rest/api/storageservices/delete-blob) für das Basisblob erfolgen. Sie können alle einem Basisblob zugeordneten Momentaufnahmen auflisten, um die aktuell vorhandenen Momentaufnahmen nachzuverfolgen.

Wenn Sie eine Momentaufnahme eines Blobs erstellen, werden seine Systemeigenschaften mit denselben Werten in die Momentaufnahme kopiert. Die Metadaten des Basisblobs werden auch in die Momentaufnahme kopiert, sofern Sie beim Erstellen keine separaten Metadaten für die Momentaufnahme angeben. Nach dem Erstellen einer Momentaufnahme kann sie gelesen, kopiert oder gelöscht, aber nicht mehr geändert werden.

Dem Basis-Blob zugeordnete Leases wirken sich nicht auf die Momentaufnahme aus. Sie können für eine Momentaufnahme keine Lease abrufen.

Für das Speichern der aktuellen Informationen und des Status eines VM-Datenträgers wird eine VHD-Datei verwendet. Sie können einen Datenträger vom virtuellen Computer trennen oder die VM herunterfahren und dann eine Momentaufnahme der VHD-Datei erstellen. Sie können diese Momentaufnahmedatei später verwenden, um die VHD-Datei zu diesem Zeitpunkt abzurufen und den virtuellen Computer neu zu erstellen.

## <a name="understand-how-snapshots-accrue-charges"></a>Grundlegendes zur Ermittlung der Gebühren für Momentaufnahmen

Durch das Erstellen einer Momentaufnahme, die eine schreibgeschützte Kopie eines BLOBs darstellt, können auf Ihrem Konto zusätzliche Gebühren für die Datenspeicherung anfallen. Wenn Sie beim Entwurf Ihrer Anwendung berücksichtigen, wie diese Gebühren auflaufen, können Sie die Kosten minimieren.

### <a name="important-billing-considerations"></a>Wichtige Überlegungen zur Abrechnung

Die folgende Liste enthält wichtige Punkte, die beim Erstellen einer Momentaufnahme zu berücksichtigen sind.

- Für Ihr Speicherkonto fallen Gebühren für eindeutige Blöcke oder Seiten an, die im Blob oder in der Momentaufnahme enthalten sind. Ihrem Konto werden erst dann zusätzliche Gebühren für Momentaufnahmen angerechnet, die einem BLOB zugeordnet sind, wenn Sie das BLOB aktualisieren, auf dem sie basieren. Sobald Sie das Basisblob aktualisieren, entstehen Abweichungen von den zugeordneten Momentaufnahmen. In diesem Fall werden für alle eindeutigen Blöcke oder Seiten in jedem Blob bzw. einer Momentaufnahme Gebühren berechnet.
- Wenn Sie einen Block innerhalb eines Block-BLOBs ersetzen, wird dieser Block anschließend als eindeutiger Block berechnet. Dies gilt auch, wenn der Block dieselbe Block-ID und dieselben Daten enthält wie in der Momentaufnahme. Nachdem ein erneuter Commit für den Block ausgeführt wurde, weicht er von seinem Pendant in den Momentaufnahmen ab, und Ihnen werden die Daten des Blocks berechnet. Gleiches gilt für eine Seite in einem Seitenblob, die mit identischen Daten aktualisiert wird.
- Wenn Sie ein Blockblob durch Aufrufen einer Methode aktualisieren, die den gesamten Inhalt des Blobs überschreibt, werden alle Blöcke im Blob ersetzt. Wenn dem Blob eine Momentaufnahme zugeordnet ist, weisen anschließend alle Blöcke im Basisblob und in der Momentaufnahme Abweichungen auf, und Ihnen werden Gebühren für alle Blöcke in beiden Blobs berechnet. Dies gilt auch, wenn die Daten im Basis-BLOB und in der Momentaufnahme identisch sind.
- Der Azure-Blob-Dienst kann nicht feststellen, ob zwei Blöcke identische Daten enthalten. Jeder hochgeladene Block, für den ein Commit ausgeführt wird, wird als eindeutig behandelt, selbst wenn die enthaltenen Daten und die Block-ID identisch sind. Da Gebühren jeweils für eindeutige Blöcke berechnet werden, ist zu berücksichtigen, dass beim Aktualisieren eines Blobs mit einer zugeordneten Momentaufnahme zusätzliche eindeutige Blöcke generiert werden, für die zusätzliche Gebühren entstehen.

### <a name="minimize-costs-with-snapshot-management"></a>Minimieren der Kosten durch Momentaufnahmenverwaltung

Es empfiehlt sich, Ihre Momentaufnahmen sorgfältig zu verwalten, um zusätzlich anfallende Gebühren zu vermeiden. Sie können die folgenden bewährten Methoden befolgen, um die beim Speichern von Momentaufnahmen anfallenden Kosten zu minimieren:

- Löschen und erstellen Sie zugehörige Momentaufnahmen für ein BLOB neu, wenn Sie das BLOB aktualisieren, selbst wenn Sie mit identischen Daten aktualisieren, es sei denn, der Anwendungsentwurf erfordert, dass die Momentaufnahmen beibehalten werden. Durch Löschen und Neuerstellen der Momentaufnahmen für ein Blob können Sie sicherstellen, dass das Blob und die Momentaufnahmen nicht voneinander abweichen.
- Wenn Sie Momentaufnahmen für ein Blob aufbewahren, sollten Sie keine Methoden aufrufen, die beim Aktualisieren des Blobs den gesamten Blob überschreiben. Aktualisieren Sie stattdessen so wenig Blöcke wie möglich, um die Kosten niedrig zu halten.

### <a name="snapshot-billing-scenarios"></a>Abrechnungsszenarien für Momentaufnahmen

Die folgenden Szenarien veranschaulichen, wie Gebühren für ein Block-BLOB und zugehörige Momentaufnahmen berechnet werden.

#### <a name="scenario-1"></a>Szenario 1

In Szenario 1 wurde das Basis-Blob nicht aktualisiert, nachdem die Momentaufnahme erstellt wurde, sodass Gebühren nur für die eindeutigen Blöcke 1, 2 und 3 berechnet werden:

![Azure Storage-Ressourcen](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>Szenario 2

In Szenario 2 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde aktualisiert. Obwohl er die gleichen Daten und dieselbe ID enthält, ist er nicht identisch mit dem Block 3 der Momentaufnahme. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>Szenario 3

In Szenario 3 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde im Basis-BLOB durch Block 4 ersetzt, die Momentaufnahme enthält aber immer noch den Block 3. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>Szenario 4

In Szenario 4 wurde das Basis-Blob vollständig aktualisiert und enthält keinen der ursprünglichen Blöcke. Daher wird das Konto für alle acht eindeutigen Blöcke belastet.

![Azure Storage-Ressourcen](./media/snapshots-overview/storage-blob-snapshots-billing-scenario-4.png)

> [!TIP]
> Vermeiden Sie das Aufrufen von Methoden, die das gesamte Blob überschreiben, und aktualisieren Sie stattdessen einzelne Blöcke, um die Kosten niedrig zu halten.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Verwalten einer Blobmomentaufnahme in .NET](snapshots-manage-dotnet.md)
- [Sichern nicht verwalteter Azure-VM-Datenträger mithilfe inkrementeller Momentaufnahmen](../../virtual-machines/windows/incremental-snapshots.md)
