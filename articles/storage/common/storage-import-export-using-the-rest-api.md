---
title: Verwenden der REST-API des Azure Import/Export-Diensts | Microsoft-Dokumentation
description: Erfahren Sie, wo Sie Ressourcen (einschließlich Anleitungs- und Referenzmaterial) zur Verwendung der REST-API des Azure Import/Export-Diensts finden.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: ae80c00fd81d0966167d2adf00764a0b1089c163
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514115"
---
# <a name="using-the-azure-importexport-service-rest-api"></a>Verwenden der REST-API des Azure Import/Export-Diensts

Der Microsoft Azure Import/Export-Dienst macht eine REST-API verfügbar, um die programmgesteuerte Kontrolle von Import-/Exportaufträgen zu ermöglichen. Mithilfe der REST-API können Sie alle Import-/Exportvorgänge durchführen, die mit dem [Azure-Portal](https://portal.azure.com/) möglich sind. Außerdem können Sie mit der REST-API bestimmte präzise Vorgänge durchführen (z. B. Abfragen des prozentualen Fortschritts eines Auftrags), die im Azure-Portal derzeit nicht verfügbar sind.

Eine Übersicht über den Import/Export-Dienst und ein Tutorial, in dem die Verwendung des Portals zum Erstellen und Verwalten von Import- und Exportaufträgen beschrieben wird, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](../storage-import-export-service.md).

## <a name="service-endpoints"></a>Dienstendpunkte

Der Azure Import/Export-Dienst ist ein Ressourcenanbieter für Azure Resource Manager und bietet am folgenden HTTPS-Endpunkt zum Verwalten von Import-/Exportaufträgen einen Satz von REST-APIs:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versionsverwaltung

Anforderungen an den Import/Export-Dienst müssen den `api-version`-Parameter angeben und dessen Wert auf `2016-11-01` festlegen.

## <a name="importexport-service-operations"></a>Vorgänge mit dem Import/Export-Dienst

[Erstellen eines Importauftrags](../storage-import-export-creating-an-import-job.md)

[Erstellen eines Exportauftrags](../storage-import-export-creating-an-export-job.md)

[Abrufen von Statusinformationen für einen Auftrag](storage-import-export-retrieving-state-info-for-a-job.md)

[Aufzählen von Aufträgen](../storage-import-export-enumerating-jobs.md)

[Abbrechen und Löschen von Aufträgen](storage-import-export-cancelling-and-deleting-jobs.md)

[Sichern von Laufwerkmanifesten](../storage-import-export-backing-up-drive-manifests.md)

[Diagnose und Fehlerbehebung für Import/Export-Aufträge](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Nächste Schritte

* [Speicherimport/-export – REST](/rest/api/storageimportexport)
