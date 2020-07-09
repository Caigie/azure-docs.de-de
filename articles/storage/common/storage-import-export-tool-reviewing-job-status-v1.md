---
title: Überprüfen des Status von Azure Import/Export-Aufträgen (V1) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der Protokolldateien, die beim Ausführen des Import- oder Exportauftrags erstellt werden, den Status des Import/Export-Auftrags ermitteln.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/26/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 5ab0dd7f0e0ed90c205c37ddbb7ea3a9fca74ae9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512269"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Überprüfen des Status von Azure Import/Export-Aufträgen mithilfe von Kopierprotokolldateien
Wenn der Microsoft Azure Import/Export-Dienst Laufwerke im Rahmen eines Import- oder Exportauftrags verarbeitet, schreibt er Kopierprotokolldateien in das Speicherkonto, in das bzw. aus dem Sie Blobs importieren oder exportieren. Die Protokolldatei enthält ausführliche Statusinformationen zu den einzelnen importierten oder exportierten Dateien. Die URL zu den einzelnen Kopierprotokolldateien wird zurückgegeben, wenn Sie den Status eines abgeschlossenen Auftrags abfragen. (Weitere Informationen finden Sie unter [Get Job](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get).)  

## <a name="example-urls"></a>Beispiel-URLs

Im Anschluss finden Sie Beispiel-URLs für Kopierprotokolldateien für einen Importauftrag mit zwei Laufwerken:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Informationen zum Format von Kopierprotokollen sowie eine vollständige Liste mit Statuscodes finden Sie unter [Format der Protokolldateien des Import/Export-Diensts](../storage-import-export-file-format-log.md).  

## <a name="next-steps"></a>Nächste Schritte

 * [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)   
 * [Vorbereiten von Festplatten für einen Importauftrag](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Reparieren eines Importauftrags](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparieren eines Exportauftrags](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Behandeln von Problemen mit dem Azure Import/Export-Tool](storage-import-export-tool-troubleshooting-v1.md)
