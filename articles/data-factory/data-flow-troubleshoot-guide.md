---
title: Problembehandlung für Datenflüsse
description: In diesem Artikel wird beschrieben, wie Sie in Azure Data Factory Datenflussprobleme beheben.
services: data-factory
ms.author: makromer
author: kromerm
manager: anandsub
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: 2edd5b661240b6156cf8a02059b2b9a668c402f3
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83829119"
---
# <a name="troubleshoot-data-flows-in-azure-data-factory"></a>Problembehandlung für Datenflüsse in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel werden die gängigen Problembehandlungsmethoden für Datenflüsse in Azure Data Factory beschrieben.

## <a name="common-errors-and-messages"></a>Häufige Fehler und Meldungen

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Fehlercode: DF-Executor-SourceInvalidPayload
- **Meldung**: Data preview, debug, and pipeline data flow execution failed because container does not exist (Fehler beim Anzeigen von Daten in der Vorschau, beim Debuggen und bei der Datenflussausführung in der Pipeline, weil der Container nicht vorhanden ist)
- **Ursachen**: Das Dataset enthält einen Container, der im Speicher nicht vorhanden ist.
- **Empfehlung**: Stellen Sie sicher, dass der Container, auf den Sie im Dataset verweisen, vorhanden ist und darauf zugegriffen werden kann.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Fehlercode: DF-Executor-SystemImplicitCartesian

- **Meldung**: Implicit cartesian product for INNER join is not supported, use CROSS JOIN instead. (Implizites kartesisches Produkt wird für INNER JOIN nicht unterstützt. Verwenden Sie stattdessen CROSS JOIN.) Die im Join verwendeten Spalten müssen einen eindeutigen Schlüssel für Zeilen erstellen.
- **Ursachen**: Implizites kartesisches Produkt für INNER JOIN zwischen logischen Plänen wird nicht unterstützt. Wenn die im Join verwendeten Spalten den eindeutigen Schlüssel erstellen, ist mindestens eine Spalte von beiden Seiten der Beziehung erforderlich.
- **Empfehlung**: Bei nicht gleichheitsbasierten Joins müssen Sie CUSTOM CROSS JOIN verwenden.

### <a name="error-code-df-executor-systeminvalidjson"></a>Fehlercode: DF-Executor-SystemInvalidJson

- **Meldung**: JSON parsing error, unsupported encoding or multiline (JSON-Analysefehler: nicht unterstützte Codierung oder mehrzeiliger Text)
- **Ursachen**: Mögliche Probleme mit der JSON-Datei: nicht unterstützte Codierung, beschädigte Bytes oder Verwendung der JSON-Quelle als einzelnes Dokument in vielen geschachtelten Zeilen
- **Empfehlung**: Überprüfen Sie, ob die Codierung der JSON-Datei unterstützt wird. Erweitern Sie in der Quelltransformation, die ein JSON-Dataset verwendet, die Option „JSON Settings“ („JSON-Einstellungen“), und aktivieren Sie „Single Document“ („Einzelnes Dokument“).
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Fehlercode: DF-Executor-BroadcastTimeout

- **Meldung**: Broadcast join timeout error, make sure broadcast stream produces data within 60 secs in debug runs and 300 secs in job runs (Übertragungsjoin-Timeoutfehler: Stellen Sie sicher, dass der Übertragungsdatenstrom in Debugausführungen Daten innerhalb von 60 Sekunden und in Auftragsausführungen innerhalb von 300 Sekunden Daten erzeugt.)
- **Ursachen**: Das Zeitlimit für Übertragungen beträgt standardmäßig 60 Sekunden bei Debugausführungen und 300 Sekunden bei Auftragsausführungen. Der Datenstrom, der für die Übertragung ausgewählt wurde, ist anscheinend zu groß, um innerhalb dieser Zeitspanne Daten zu erzeugen.
- **Empfehlung**: Überprüfen Sie die Registerkarte „Optimieren“ auf Ihren Datenflusstransformationen auf „Join“, „Exists“ und „Lookup“. Die Standardoption für die Übertragung ist „Auto“. Wenn diese Option festgelegt ist oder wenn Sie die linke oder rechte Seite manuell unter „Fixed“ für die Übertragung festlegen, dann können Sie entweder eine größere Azure Integration Runtime-Konfiguration festlegen oder die Übertragung deaktivieren. Der empfohlene Ansatz für die optimale Leistung bei Datenflüssen besteht darin, Spark zu erlauben, mit „Auto“ zu übertragen und ein arbeitsspeicheroptimiertes Azure IR zu verwenden.

### <a name="error-code-df-executor-conversion"></a>Fehlercode: DF-Executor-Conversion

- **Meldung**: Converting to a date or time failed due to an invalid character (Fehler beim Umwandeln eines Datums oder einer Uhrzeit aufgrund eines ungültigen Zeichens)
- **Ursachen**: Die Daten haben nicht das erwartete Format.
- **Empfehlung**: Verwenden Sie den richtigen Datentyp.

### <a name="error-code-df-executor-invalidcolumn"></a>Fehlercode: DF-Executor-InvalidColumn

- **Meldung**: Column name needs to be specified in the query, set an alias if using a SQL function (Spaltenname muss in der Abfrage angegeben werden. Legen Sie einen Alias fest, wenn Sie eine SQL-Funktion verwenden.)
- **Ursachen**: Es wurde kein Spaltenname angegeben.
- **Empfehlung**: Legen Sie einen Alias fest, wenn Sie SQL-Funktionen wie min()/max() etc. verwenden.

### <a name="error-code-getcommand-outputasync-failed"></a>Fehlercode: GetCommand OutputAsync failed (Fehler bei GetCommand OutputAsync)

- **Meldung**: During Data Flow debug and data preview: GetCommand OutputAsync failed (Beim Debuggen des Datenflusses und Anzeigen von Daten in der Vorschau: Fehler bei GetCommand OutputAsync mit ...)
- **Ursachen**: Dies ist ein Fehler beim Back-End-Dienst. Sie können den Vorgang wiederholen und auch die Debugsitzung neu starten.
- **Empfehlung**: Wenn das Problem durch Wiederholung und Neustart nicht behoben werden kann, wenden Sie sich an den Kundensupport.

### <a name="error-code-hit-unexpected-exception-and-execution-failed"></a>Fehlercode: Unerwartete Ausnahme, Fehler bei der Ausführung

- **Meldung**: Während der Ausführung der Datenflussaktivität: Unerwartete Ausnahme, Fehler bei der Ausführung.
- **Ursachen**: Dies ist ein Fehler beim Back-End-Dienst. Sie können den Vorgang wiederholen und auch die Debugsitzung neu starten.
- **Empfehlung**: Wenn das Problem durch Wiederholung und Neustart nicht behoben werden kann, wenden Sie sich an den Kundensupport.

## <a name="general-troubleshooting-guidance"></a>Allgemeine Anleitungen zur Problembehandlung

1. Überprüfen Sie den Status der Datasetverbindungen. Besuchen Sie in jeder Quell-und Senkentransformation den verknüpften Dienst für jedes verwendete Dataset, und testen Sie die Verbindungen.
1. Überprüfen Sie den Status der Datei- und Tabellenverbindungen aus dem Datenfluss-Designer. Aktivieren Sie das Debuggen, und klicken Sie auf „Datenvorschau“ für Ihre Quelltransformationen, um sicherzustellen, dass Sie auf die Daten zugreifen können.
1. Wenn in der Datenvorschau keine Probleme erkennbar sind, wechseln Sie zum Pipeline-Designer, und fügen Sie den Datenfluss in eine Pipelineaktivität ein. Debuggen Sie die Pipeline in einem End-to-End-Test.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:
*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Frageseite von Microsoft Q&A (Fragen und Antworten)](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md)
