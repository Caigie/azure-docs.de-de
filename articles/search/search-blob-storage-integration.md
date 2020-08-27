---
title: Hinzufügen der Volltextsuche zu Azure Blob Storage
titleSuffix: Azure Cognitive Search
description: Extrahieren Sie Inhalte, und versehen Sie Azure-Blobs mit Struktur, wenn Sie in Azure Cognitive Search einen Volltextsuchindex erstellen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 72d00b70cf3568466715668aa441ee295614c740
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935244"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-cognitive-search"></a>Hinzufügen der Volltextsuche zu Azure-Blobdaten mithilfe von Azure Cognitive Search

Das Durchsuchen verschiedenster in Azure Blob Storage gespeicherter Inhalte kann ein schwer zu lösendes Problem sein. Allerdings können Sie mit [Azure Cognitive Search](search-what-is-azure-search.md) den Inhalt Ihrer Blobs mit nur wenigen Klicks indizieren und durchsuchen. Azure Cognitive Search bietet eine vordefinierte Integration für die Indizierung von Blobspeicher über einen [*Blobindexer*](search-howto-indexing-azure-blob-storage.md), der der Indizierung datenquellenabhängige Funktionen hinzufügt.

## <a name="what-it-means-to-add-full-text-search-to-blob-data"></a>Bedeutung des Hinzufügens der Volltextsuche zu Blobdaten

Azure Cognitive Search ist ein Cloudsuchdienst, der Indizierungs- und Abfragemodule bereitstellt, die mit in Ihrem Suchdienst gehosteten benutzerdefinierten Indizes arbeiten. Die Zusammenstellung Ihrer durchsuchbaren Inhalte mit dem Abfragemodul in der Cloud ist aus Leistungsgründen erforderlich, damit Ergebnisse mit einer Geschwindigkeit zurückgegeben werden, die Benutzer von Suchabfragen erwarten.

Azure Cognitive Search und Azure Blob Storage sind auf Indexierungsebene integriert. Dabei werden Ihre Blobinhalte als Suchdokumente importiert, die in *invertierte Indizes* und andere Suchstrukturen indiziert werden, die Textabfragen in freier Form und Filterausdrücke unterstützen. Da Ihre Blobinhalte in einem Suchindex indiziert werden, kann beim Zugriff auf Blobinhalte die gesamte Bandbreite der Abfragefunktionen von Azure Cognitive Search genutzt werden.

Nach Erstellen und Auffüllen des Indexes existiert er unabhängig von Ihrem Blobcontainer. Sie können jedoch Indizierungsvorgänge erneut ausführen, um Ihren Index mit Änderungen am zugrunde liegenden Container zu aktualisieren. Zeitstempelinformationen zu einzelnen Blobs werden zur Erkennung von Änderungen verwendet. Sie können sich entweder für die geplante Ausführung oder die bedarfsgesteuerte Indizierung als Aktualisierungsmechanismus entscheiden.

Als Eingaben fungieren Ihre Blobs in einem einzelnen Container in Azure Blob Storage. Blobs können aus nahezu beliebigen Arten von Textdaten bestehen. Wenn Ihre Blobs Bilder enthalten, können Sie der [Blobindizierung KI-Erweiterungen hinzufügen](search-blob-ai-integration.md), um Text anhand von Bildern zu erstellen und zu extrahieren.

Die Ausgabe ist stets ein Azure Cognitive Search-Index, der für schnelles Suchen, Abrufen und Erkunden von Texten in Clientanwendungen genutzt wird. Dazwischen befindet sich die Architektur der Indizierungspipeline selbst. Die Pipeline basiert auf der *Indexer*-Funktion, die im weiteren Verlauf dieses Artikels erläutert wird.

## <a name="start-with-services"></a>Beginnen mit Diensten

Sie benötigen Azure Cognitive Search und Azure Blob Storage. In Azure Blob Storage benötigen Sie einen Container, der Quellinhalt bereitstellt.

Sie können direkt auf der Portalseite des Speicherkontos beginnen. Klicken Sie auf der linken Navigationsseite unter **Blob-Dienst** auf **Azure Cognitive Search hinzufügen**, um einen neuen Dienst zu erstellen oder einen bestehenden auszuwählen. 

Nachdem Sie Ihrem Speicherkonto Azure Cognitive Search hinzugefügt haben, können Sie den Standardprozess zum Indizieren von Blobdaten befolgen. Wir empfehlen für eine einfache erste Eingabe von Daten den Assistenten **Daten importieren** in Azure Cognitive Search. Oder rufen Sie die REST-APIs mit einem Tool wie Postman auf. Dieses Tutorial führt Sie durch die Schritte zum Aufrufen der Rest-API in Postman: [Indizieren und Durchsuchen von teilweise strukturierten Daten (JSON-Blobs) in Azure Cognitive Search](search-semi-structured-data.md). 

## <a name="use-a-blob-indexer"></a>Verwenden eines Blobindexers

Ein *Indexer* ist ein datenquellenabhängiger Subdienst mit interner Logik zum Nehmen von Stichproben von Daten, Lesen von Metadaten, Abrufen von Daten und Serialisieren von Daten aus nativen Formaten in JSON-Dokumente zum anschließenden Import. 

Blobs in Azure Storage werden mithilfe des [Blob Storage-Indexers von Azure Cognitive Search](search-howto-indexing-azure-blob-storage.md) indiziert. Sie können diesen Indexer mit dem Assistenten **Daten importieren**, mit einer REST-API oder dem .NET SDK aufrufen. Im Code verwenden Sie diesen Indexer, indem Sie den Typ festlegen und Verbindungsinformationen bereitstellen, die ein Azure-Speicherkonto zusammen mit einem Blobcontainer enthalten. Sie können Ihre Blobs unterteilen, indem Sie ein virtuelles Verzeichnis erstellen, das Sie dann als Parameter übergeben können, oder indem Sie nach einer Dateityperweiterung filtern.

Ein Indexer übernimmt die Dokumententschlüsselung und öffnet ein Blob, um den Inhalt zu inspizieren. Nach dem Herstellen einer Verbindung mit der Datenquelle ist dies der erste Schritt in der Pipeline. Bei Blobdaten werden an dieser Stelle PDF-Dateien, Office-Dokumente und andere Inhaltstypen erkannt. Die Dokumententschlüsselung mit Textextraktion erfolgt gebührenfrei. Wenn Ihre Blobs Bildinhalte enthalten, werden Bilder ignoriert, es sei denn, Sie fügen [KI-Erweiterungen](search-blob-ai-integration.md) hinzu. Die Standardindizierung gilt nur für Textinhalte.

Der Blobindexer verfügt über Konfigurationsparameter und unterstützt die Nachverfolgung von Änderungen, wenn die zugrunde liegenden Daten genügend Informationen liefern. Weitere Informationen zur Kernfunktionalität finden Sie unter [Blob Storage-Indexer in Azure Cognitive Search](search-howto-indexing-azure-blob-storage.md).

### <a name="supported-content-types"></a>Unterstützte Inhaltstypen

Wenn Sie einen Blobindexer auf einen Container anwenden, können Sie mit einer einzigen Abfrage aus den folgenden Inhaltstypen Text und Metadaten extrahieren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

### <a name="indexing-blob-metadata"></a>Indizieren der Metadaten von Blobs

Das Indizieren von benutzerdefinierten Metadaten sowie von Systemeigenschaften für jedes Blob ist ein häufig auftretendes Szenario, in dem Sie Blobs jedes Inhaltstyps unkompliziert durchsehen können. Auf diese Weise werden die Informationen für alle Blobs unabhängig vom Dokumenttyp indiziert und in einem Index in Ihrem Suchdienst gespeichert. Mit dem neuen Index können Sie dann den gesamten Blob Storage-Inhalt sortieren und filtern sowie Facetten bilden.

> [!NOTE]
> Blobindextags werden vom Blob Storage-Dienst nativ indiziert und für Abfragen verfügbar gemacht. Wenn die Schlüssel-Wert-Attribute des Blob Indizierungs- und Filterungsfunktionen erfordern, sollten Blobindextags anstelle von Metadaten genutzt werden.
>
> Weitere Informationen über den Blobindex finden Sie unter [Verwalten und Suchen von Daten in Azure Blob Storage mit dem Blobindex](../storage/blobs/storage-manage-find-blobs.md).

### <a name="indexing-json-blobs"></a>Indizieren von JSON-Blobs
Indexer können so konfiguriert werden, dass sie strukturierten Inhalt aus Blobs extrahieren, die eine JSON-Datei enthalten. Ein Indexer kann aus JSON-Blobs lesen und den strukturierten Inhalt analysieren, um ihn den entsprechenden Feldern in einem Suchdokument zuzuweisen. Indexer können außerdem Blobs annehmen, die ein Array von JSON-Objekten enthalten und jedes Element mit einem separaten Suchdokument verknüpfen. Sie können einen Analysemodus festlegen, der den Typ des vom Indexer erzeugten JSON-Objekts beeinflusst.

## <a name="search-blob-content-in-a-search-index"></a>Durchsuchen von Blobinhalten in einem Suchindex 

Die Ausgabe einer Indizierung ist ein Suchindex, der für die interaktive Erkundung mithilfe von Freitext- und gefilterten Abfragen in einer Clientanwendung verwendet wird. Für die erste Erkundung und Überprüfung von Inhalten empfehlen wir, im Portal im [Suchexplorer](search-explorer.md) die Dokumentstruktur zu untersuchen. Sie können im Suchexplorer die [einfache Abfragesyntax](query-simple-syntax.md), [vollständige Abfragesyntax](query-lucene-syntax.md) und [Filterausdruckssyntax](query-odata-filter-orderby-syntax.md) verwenden.

Eine dauerhaftere Lösung besteht darin, Abfrageeingaben zu sammeln und die Antwort als Suchergebnisse in einer Clientanwendung darzustellen. Im folgenden C#-Tutorial wird das Erstellen einer Suchanwendung erläutert: [Erstellen Ihrer ersten Anwendung in Azure Cognitive Search](tutorial-csharp-create-first-app.md).

## <a name="next-steps"></a>Nächste Schritte

+ [Hochladen, Herunterladen und Auflisten von Blobs mit dem Azure-Portal (Azure Blob Storage)](../storage/blobs/storage-quickstart-blobs-portal.md)
+ [Einrichten eines Blobindexers (Azure Cognitive Search)](search-howto-indexing-azure-blob-storage.md)