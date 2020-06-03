---
title: Indizieren großer Datasets mithilfe integrierter Indexer
titleSuffix: Azure Cognitive Search
description: Strategien für die Indizierung umfangreicher Daten oder rechenintensive Indizierung über den Batchmodus, Ressourcenerstellung und Techniken für die geplante, parallele und verteilte Indizierung.
manager: liamca
author: dereklegenzoff
ms.author: delegenz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/05/2020
ms.openlocfilehash: 915243fb4dbc6bb274e26261bc5741811ef24592
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82925982"
---
# <a name="how-to-index-large-data-sets-in-azure-cognitive-search"></a>Indizieren großer Datasets in der kognitiven Azure-Suche

Von Azure Cognitive Search werden [zwei grundlegende Ansätze](search-what-is-data-import.md) für das Importieren von Daten in einen Suchindex unterstützt: Sie können die Daten programmgesteuert an den Index *pushen* oder einen [Azure Cognitive Search-Indexer](search-indexer-overview.md) auf eine unterstützte Datenquelle verweisen, um die Daten zu *pullen*.

Wenn das Datenvolumen zunimmt oder die Verarbeitung geändert werden muss, stellen Sie möglicherweise fest, dass einfache oder standardmäßige Indizierungsstrategien nicht ausreichend sind. Für die kognitive Azure-Suche gibt es verschiedene Ansätze für die Aufnahme von größeren Datasets, angefangen bei der Art, wie Sie eine Uploadanforderung strukturieren, bis zur Verwendung eines quellenspezifischen Indexers für geplante und verteilte Workloads.

Die Techniken gelten auch für lang andauernde Prozesse. Insbesondere die unter [Parallelindizierung](#parallel-indexing) beschriebenen Schritte sind hilfreich für die Ausführung rechenintensiver Indizierung, z. B. Bildanalyse oder Verarbeitung natürlicher Sprache in einer [KI-Anreicherungspipeline](cognitive-search-concept-intro.md).

In den folgenden Abschnitten werden Techniken zum Indizieren großer Datenmengen unter Verwendung der Push-API und mithilfe von Indexern erläutert.

## <a name="push-api"></a>Push-API

Beim Pushen von Daten an einen Index gibt es einige wichtige Aspekte, die sich auf die Indizierungsgeschwindigkeit für die Push-API auswirken. Diese Aspekte werden im folgenden Abschnitt beschrieben. 

Zusätzlich zu den Informationen in diesem Artikel können Sie auch die Codebeispiele im [Tutorial zur Optimierung der Indizierungsgeschwindigkeiten](tutorial-optimize-indexing-push-api.md) nutzen, um mehr zu erfahren.

### <a name="service-tier-and-number-of-partitionsreplicas"></a>Dienstebene und Anzahl der Partitionen/Replikate

Die Indizierungsgeschwindigkeiten können sowohl durch Hinzufügen von Partitionen als auch durch Erhöhen der Ebene Ihres Suchdiensts verbessert werden.

Auch durch Hinzufügen zusätzlicher Replikate können die Indizierungsgeschwindigkeiten erhöht werden. Auf der anderen Seite erhöhen zusätzliche Replikate das Abfragevolumen, das von Ihrem Suchdienst verarbeitet werden kann. Replikate sind auch eine Schlüsselkomponente für den Erhalt einer [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Bevor Sie eine Partition/Replikate hinzufügen oder ein Upgrade auf eine höhere Dienstebene durchführen, berücksichtigen Sie Kosten und Zuweisungszeit. Das Hinzufügen von Partitionen kann die Indizierungsgeschwindigkeit erheblich erhöhen. Das Hinzufügen/Entfernen von Partitionen kann jedoch zwischen 15 Minuten und mehreren Stunden dauern. Weitere Informationen finden Sie in der Dokumentation unter [Anpassen der Kapazität](search-capacity-planning.md).

### <a name="index-schema"></a>Indexschema

Das Schema Ihres Indexes spielt eine wichtige Rolle bei der Indizierung von Daten. Zusätzlich hinzugefügte Felder und Feldeigenschaften (wie etwa *searchable*, *facetable* oder *filterable*) tragen zur Verlangsamung der Indizierung bei.

Im Allgemeinen wird empfohlen, Feldern nur dann zusätzliche Eigenschaften hinzuzufügen, wenn Sie diese verwenden möchten.

> [!NOTE]
> Um die Dokumentgröße niedrig zu halten, vermeiden Sie es, dem Index nicht abfragbare Daten hinzuzufügen. Bilder und andere binäre Daten können nicht direkt durchsucht werden und sollten nicht im Index gespeichert werden. Um nicht abfragbare Daten in Suchergebnisse zu integrieren, sollten Sie ein nicht durchsuchbares Feld definieren, in dem ein URL-Verweis auf die Ressource gespeichert wird.

### <a name="batch-size"></a>Batchgröße

Eines der einfachsten Verfahren für die Indizierung eines größeren Datasets ist das Senden mehrerer Dokumente oder Datensätze in einer einzelnen Anforderung. Solange die gesamte Nutzlast kleiner als 16MB ist, kann eine Anforderung bis zu 1.000 Dokumente in einem Uploadmassenvorgang verarbeiten. Diese Grenzwerte gelten unabhängig davon, ob Sie die [REST-API „Dokumente hinzufügen“](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) oder die [index-Methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) im .NET SDK verwenden. Für beide APIs würden Sie 1000 Dokumente im Text der einzelnen Anforderungen verpacken.

Wenn Sie Dokumente in Batches indizieren, verbessert sich die Indizierungsleistung erheblich. Die Bestimmung der optimalen Batchgröße für Ihre Daten ist ein wichtiger Faktor bei der Optimierung der Indizierungsgeschwindigkeit. Die optimale Batchgröße wird hauptsächlich durch die beiden folgenden Faktoren beeinflusst:
+ Indexschema
+ Datengröße

Da die optimale Batchgröße von Ihrem Index und von Ihren Daten abhängt, empfiehlt es sich, durch Testen verschiedener Batchgrößen zu ermitteln, bei welcher Größe die Indizierung für Ihr Szenario am schnellsten ist. Dieses [Tutorial](tutorial-optimize-indexing-push-api.md) enthält Beispielcode zum Testen von Batchgrößen mithilfe des .NET-SDKs. 

### <a name="number-of-threadsworkers"></a>Anzahl von Threads/Workern

Zur bestmöglichen Nutzung der Indizierungsgeschwindigkeit von Azure Cognitive Search müssen wahrscheinlich mehrere Threads verwendet werden, um Batchindizierungsanforderungen parallel an den Dienst zu senden.  

Die optimale Anzahl von Threads wird durch Folgendes bestimmt:

+ Ebene Ihres Suchdiensts
+ Anzahl der Partitionen
+ Größe Ihrer Batches
+ Schema Ihres Indexes

Sie können dieses Beispiel ändern und mit einer anderen Threadanzahl testen, um die optimale Threadanzahl für Ihr Szenario zu ermitteln. Solange Sie jedoch mehrere parallel ausgeführte Threads verwenden, sollten von einem Großteil der Effizienzsteigerungen profitieren. 

> [!NOTE]
> Wenn Sie die Ebene des Suchdiensts erhöhen oder die Partitionen vergrößern, sollten Sie auch die Anzahl der gleichzeitigen Threads erhöhen.

Im Zuge der Erhöhung der Anforderungen für den Suchdienst werden möglicherweise [HTTP-Statuscodes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) mit dem Hinweis zurückgegeben, dass die Anforderung nicht vollständig erfolgreich war. Zwei gängige HTTP-Statuscodes im Zusammenhang mit der Indizierung sind:

+ **503 Dienst nicht verfügbar**: Dieser Fehler bedeutet, dass die Auslastung des Systems sehr hoch ist und Ihre Anforderungen aktuell nicht verarbeitet werden können.
+ **207 Multi-Status**: Dieser Fehler bedeutet, dass der Vorgang für einige Dokumente erfolgreich war, bei mindestens einem Dokument aber ein Fehler aufgetreten ist.

### <a name="retry-strategy"></a>Wiederholungsstrategie 

Im Falle eines Fehlers sollten Anforderungen unter Verwendung einer [Wiederholungsstrategie mit exponentiellem Backoff](https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/implement-retries-exponential-backoff) wiederholt werden.

Anforderungen mit 503-Fehlern und anderen Fehlern werden vom .NET SDK von Azure Cognitive Search automatisch wiederholt. Für die Wiederholung bei 207-Fehlern muss allerdings eine eigene Logik implementiert werden. Eine Wiederholungsstrategie kann auch mithilfe von Open-Source-Tools wie [Polly](https://github.com/App-vNext/Polly) implementiert werden.

### <a name="network-data-transfer-speeds"></a>Datenübertragungsgeschwindigkeit im Netzwerk

Die Datenübertragungsgeschwindigkeit im Netzwerk kann ein limitierender Faktor beim Indizieren von Daten sein. Das Indizieren von Daten in ihrer Azure-Umgebung ist eine einfache Möglichkeit zum Beschleunigen der Indizierung.

## <a name="indexers"></a>Indexer

Mit [Indexern](search-indexer-overview.md) werden unterstützte Azure-Datenquellen nach durchsuchbaren Inhalten durchforstet. Mehrere Indexerfunktionen sind zwar nicht speziell für die Indizierung in großem Rahmen vorgesehen, jedoch besonders nützlich zur Aufnahme größerer Datasets:

+ Mit Schedulern können Sie die Indizierung in regelmäßige Intervalle gliedern, sodass Sie sie im Laufe der Zeit verteilen können.
+ Geplante Indizierung kann am letzten bekannten Haltepunkt fortgesetzt werden. Wenn eine Datenquelle innerhalb eines 24-Stunden-Zeitfensters nicht vollständig durchsucht wird, setzt der Indexer die Indizierung am nächsten Tag fort, unabhängig davon, wo er aufgehört hat.
+ Partitionieren von Daten in kleinere einzelne Datenquellen ermöglicht die parallele Verarbeitung. Sie können Quelldaten in kleinere Komponenten teilen, z. B. in mehrere Container in Azure Blob Storage, und anschließend mehrere entsprechende [Datenquellenobjekte](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in der kognitiven Azure-Suche erstellen, die parallel indiziert werden können.

> [!NOTE]
> Indexer sind datenquellenspezifisch, also eignet sich ein Indexeransatz nur für ausgewählte Datenquellen in Azure: [SQL-Datenbank](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Blobspeicher](search-howto-indexing-azure-blob-storage.md), [Tabellenspeicher](search-howto-indexing-azure-tables.md), [Cosmos DB](search-howto-index-cosmosdb.md).

### <a name="batch-size"></a>Batchgröße

Wie die Push-API ermöglichen auch Indexer, die Anzahl von Elementen pro Batch zu konfigurieren. Für Indexer basierend auf der [Create Indexer-REST-API](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) können Sie das `batchSize`-Argument zum Anpassen dieser Einstellung festlegen, damit die Eigenschaften besser Ihren Daten entsprechen. 

Die Standardbatchgrößen sind datenquellenspezifisch. Azure SQL-Datenbank und Azure Cosmos DB verfügen über eine Standardbatchgröße von 1000. Im Gegensatz dazu wird bei der Azure-Blobindizierung eine Batchgröße von 10 Dokumenten unter Berücksichtigung der größeren durchschnittlichen Dokumentgröße festgelegt. 

### <a name="scheduled-indexing"></a>Geplante Indizierung

Indexerzeitpläne sind ein wichtiger Mechanismus zum Verarbeiten von großen Datasets und für lang andauernde Prozesse wie Bildanalyse in einer Pipeline für die kognitive Suche. Die Indexerverarbeitung wird innerhalb von 24 Stunden ausgeführt. Wenn die Verarbeitung nicht innerhalb von 24 Stunden abgeschlossen ist, können Sie die Verhaltensweisen für die Indexerzeitplanung zu Ihrem Vorteil nutzen. 

Standardmäßig startet die geplante Indizierung zu bestimmten Zeitintervallen. Dabei wird ein Vorgang in der Regel abgeschlossen, bevor die Indizierung zum nächsten geplanten Intervall fortgesetzt wird. Wenn der Vorgang jedoch nicht innerhalb des Intervalls abgeschlossen wird, wird der Indexer beendet, da ein Timeout auftritt. Beim nächsten Intervall wird die Verarbeitung an der Stelle fortgesetzt, an der sie während des letzten Intervalls unterbrochen wurde. Dabei verfolgt das System nach, an welcher Stelle dies der Fall ist. 

Das heißt in der Praxis, dass Sie für Indexladungen, die sich über mehrere Tage erstrecken, einen 24-Stunden-Zeitplan für den Indexer festlegen können. Wenn eine Indizierung für den nächsten 24-Stunden-Zyklus fortgesetzt wird, startet diese bei dem letzten erfolgreich verarbeiteten Dokument neu. Auf diese Weise kann sich ein Indexer über mehrere Tage hinweg durch das Dokumentbacklog durcharbeiten, bis alle nicht verarbeitete Dokumente verarbeitet wurden. Weitere Informationen zu diesem Ansatz finden Sie unter [Indizieren großer Datasets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets). Weitere Informationen zum Festlegen der Zeitpläne im Allgemeinen finden Sie unter [Create Indexer-REST-API](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer) (REST-API zum Erstellen von Indexern) oder [Festlegen eines Zeitplans für Indexer in der kognitiven Azure-Suche](search-howto-schedule-indexers.md).

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>Parallele Indizierung

Eine parallele Indizierungsstrategie basiert auf der gemeinsamen Indizierung mehrerer Datenquellen, wobei jede Datenquellendefinition eine Teilmenge der Daten angibt. 

Für nicht routinemäßige, rechenintensive Indizierungsanforderungen – wie z.B. OCR bei gescannten Dokumenten in einer kognitiven Suchpipeline, bei der Bildanalyse oder Verarbeitung natürlicher Sprache – ist eine parallele Indizierungsstrategie häufig der richtige Ansatz für den Abschluss eines lang andauernden Prozesses in kürzestmöglicher Zeit. Wenn Sie Abfrageanforderungen vermeiden oder einschränken können, ist die parallele Indizierung für einen Dienst, der simultan keine Anforderungen verarbeitet, Ihre beste Strategie für das Durcharbeiten einer großen Inhaltsmenge, die langsam verarbeitet wird. 

Parallele Verarbeitung verfügt über die folgenden Elemente:

+ Verteilen Sie Ihre Quelldaten auf mehrere Container oder mehrere virtuelle Ordner innerhalb desselben Containers. 
+ Ordnen Sie jedes noch so kleine Dataset einer eigenen [Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source) zu, die an einen eignen [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer) gekoppelt ist.
+ Verweisen Sie für die kognitive Suche in jeder Indexerdefinition auf dieselbe [Fähigkeitsgruppe](https://docs.microsoft.com/rest/api/searchservice/create-skillset).
+ Schreiben Sie in denselben Suchindex für das Ziel. 
+ Legen Sie für die Ausführung aller Indexer denselben Zeitpunkt fest.

> [!NOTE]
> In Azure Cognitive Search können Sie der Indizierung oder Abfrageverarbeitung keine einzelnen Replikate oder Partitionen zuweisen. Das System bestimmt, wie Ressourcen verwendet werden. Um die Auswirkung auf die Abfrageleistung zu verstehen, können Sie die parallele Indizierung in einer Testumgebung erproben, bevor Sie diese in die Produktion überführen.  

### <a name="how-to-configure-parallel-indexing"></a>Konfigurieren der parallelen Indizierung

Für Indexer basiert die Verarbeitung der Kapazität grob auf einem Indexersubsystem für jede Diensteinheit, die von Ihrem Suchdienst verwendet wird. Für Dienste der kognitiven Azure-Suche, die mit den Tarifen „Basic“ und „Standard“ bereitgestellt werden und mindestens zwei Replikate aufweisen, sind mehrere gleichzeitige Indexer möglich. 

1. Prüfen Sie im [Azure-Portal](https://portal.azure.com) auf der Dashboardseite **Übersicht** des Search-Diensts den **Tarif**, um festzustellen, ob die parallele Indizierung notwendig ist. Sowohl der Basic- als auch der Standard-Tarif umfassen mehrere Replikate.

2. Erhöhen Sie die [Anzahl der Replikate](search-capacity-planning.md) unter **Einstellungen** > **Staffelung** um ein zusätzliches Replikat für jede Indexerworkload. Geben Sie eine ausreichende Zahl für bestehende Abfragevolumen an. Es sollten keine Abfrageworkloads für die Indizierung geopfert werden.

3. Teilen Sie Daten auf mehrere Container auf, und zwar auf einer Ebene, die von den Indexern der kognitiven Azure-Suche erreicht werden kann. Also z.B. auf mehrere Tabellen in Azure SQL-Datenbank, auf mehrere Container im Azure Blob-Speicher oder auf mehrere Sammlungen. Definieren Sie ein Datenquellobjekt pro Tabelle oder Container.

4. Erstellen Sie mehrere Indexer, und planen Sie eine parallele Ausführung:

   + Gehen Sie von einem Dienst mit sechs Replikaten aus. Konfigurieren Sie sechs Indizes, wobei jeder einer Datenquelle zugeordnet ist, die ein Sechstel des Datasets enthält, sodass Sie das gesamte Dataset auf sechs Teile aufteilen können. 

   + Richten Sie alle Indexer auf einen Index aus. Richten Sie für kognitive Suchworkloads alle Indexer auf eine Fähigkeitsgruppe aus.

   + Legen Sie innerhalb sämtlicher Indexerdefinitionen denselben Zeitplan für das Ausführungsmuster für die Runtime fest. Beispielsweise erstellt `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` für den 15.5.2018 einen Zeitplan für alle Indexer, die in Intervallen von acht Stunden ausgeführt werden.

Zum festgelegten Zeitpunkt beginnen sämtliche Indexer mit der Ausführung, dem Laden von Daten, der Anwendung von Anreicherungen (wenn Sie eine Pipeline für die kognitive Suche konfiguriert haben) und dem Schreiben in den Index. Die kognitive Azure-Suche sperrt den Index nicht für Updates. Gleichzeitige Schreibvorgänge werden verwaltet und wiederholt, wenn ein bestimmter Schreibvorgang beim ersten Mal nicht erfolgreich ausgeführt werden kann.

> [!Note]
> Wenn Sie die Anzahl der Replikate erhöhen, sollten Sie auch die Anzahl der Partitionen erhöhen, wenn zu erwarten ist, dass der Index deutlich vergrößert wird. Partitionen speichern einzelne Segmente des indizierten Inhalts. Je mehr Partitionen Sie besitzen, desto kleiner sind die einzelnen Segmente, die darin gespeichert werden müssen.

## <a name="see-also"></a>Weitere Informationen

+ [Indexer (Übersicht)](search-indexer-overview.md)
+ [Indizieren im Portal](search-import-data-portal.md)
+ [Azure SQL-Datenbank-Indexer](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB-Indexer](search-howto-index-cosmosdb.md)
+ [Azure Blob Storage-Indexer](search-howto-indexing-azure-blob-storage.md)
+ [Azure Table Storage-Indexer](search-howto-indexing-azure-tables.md)
+ [Sicherheit in der kognitiven Azure-Suche](search-security-overview.md)
