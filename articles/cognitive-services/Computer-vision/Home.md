---
title: Worum handelt es sich bei maschinellem Sehen? – Maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Über den Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 05/27/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3e1c67ee91298b9e8d0c3c427988c9966771aeaa
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920556"
---
# <a name="what-is-computer-vision"></a>Worum handelt es sich bei maschinellem Sehen?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Über den Azure-Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen, die Bilder verarbeiten und Informationen anhand der für sie interessanten visuellen Features zurückgeben. Beispielsweise kann durch maschinelles Sehen ermittelt werden, ob ein Bild nicht jugendfreie Inhalte aufweist, oder es können bestimmte Marken oder Objekte oder menschliche Gesichter gesucht werden.

Sie können maschinelles Sehen in Ihrer Anwendung über ein Clientbibliothek-SDK oder durch direktes Aufrufen der REST-API nutzen. Auf dieser Seite erfahren Sie ganz allgemein, welche Möglichkeiten maschinelles Sehen bietet.

## <a name="computer-vision-for-digital-asset-management"></a>Maschinelles Sehen für Digital Asset Management (DAM)

Das maschinelle Sehen kann viele DAM-Szenarien (Digital Asset Management) unterstützen. DAM ist der Geschäftsprozess der Organisation, Speicherung und Abfrage von Rich-Media-Medienobjekten und der Verwaltung digitaler Rechte und Berechtigungen. Beispielsweise kann ein Unternehmen Bilder basierend auf sichtbaren Logos, Gesichtern, Objekten, Farben usw. gruppieren und identifizieren. Oder Sie können automatisch [Beschriftungen für Bilder generieren](./Tutorials/storage-lab-tutorial.md) und Schlüsselwörter anhängen, damit sie durchsuchbar sind. Eine All-in-One-DAM-Lösung mit Cognitive Services, Azure Cognitive Search und intelligenter Berichterstellung finden Sie im Leitfaden [Knowledge Mining Solution Accelerator](https://github.com/Azure-Samples/azure-search-knowledge-mining) auf GitHub. Weitere DAM-Beispiele finden Sie im Repository zu [Lösungsvorlagen für maschinelles Sehen](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates).

## <a name="analyze-images-for-insight"></a>Analysieren von Bildern, um Erkenntnisse zu gewinnen

Sie können Bilder analysieren, um Erkenntnisse zu visuellen Merkmalen und Eigenschaften zu gewinnen. Alle Features in der folgenden Tabelle werden von der [API für die Bildanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) bereitgestellt.

| Aktion | Beschreibung |
| ------ | ----------- |
|**[Markieren visueller Merkmale](concept-tagging-images.md)**|Erkennen und markieren Sie visuelle Merkmale in einem Bild auf der Grundlage von Tausenden von erkennbaren Objekten, Lebewesen, Landschaften und Aktionen. Wenn die Tags nicht eindeutig sind oder nicht zum Allgemeinwissen gehören, enthält die API-Antwort Hinweise, um den Kontext des Tags zu verdeutlichen. Die Markierung ist nicht auf den Hauptinhalt (etwa eine Person im Vordergrund) beschränkt, sondern bezieht auch die Umgebung (Innen- oder Außenbereich), Möbel, Werkzeuge, Pflanzen, Tiere, Zubehör, Geräte und Ähnliches mit ein.|
|**[Erkennen von Objekten](concept-object-detection.md)**| Die Objekterkennung ähnelt dem Tagging, die API gibt jedoch die Koordinaten des Begrenzungsrahmens für jedes angewendete Tag zurück. Wenn ein Bild z. B. einen Hund, eine Katze und eine Person enthält, werden diese Objekte bei der Erkennung zusammen mit ihren Koordinaten im Bild aufgelistet. Sie können diese Funktion verwenden, um weitere Beziehungen zwischen den Objekten in einem Bild zu verarbeiten. Außerdem können Sie feststellen, ob mehrere Instanzen des gleichen Tags in einem Bild enthalten sind.|
|**[Erkennen von Marken](concept-brand-detection.md)**|Erkennen Sie auf Bildern oder in Videos kommerzielle Marken auf der Grundlage einer Datenbank mit Tausenden Logos aus der ganzen Welt. Mit diesem Feature können Sie beispielsweise ermitteln, welche Marken in sozialen Medien besonders beliebt sind oder besonders gerne in Medien platziert werden.|
|**[Kategorisieren eines Bilds](concept-categorizing-images.md)**|Erkennen und kategorisieren Sie ein gesamtes Bild unter Verwendung einer [Kategorietaxonomie](Category-Taxonomy.md) mit über-/untergeordneten vererbbaren Hierarchien. Kategorien können einzeln oder in Kombination mit unseren neuen Markierungsmodellen verwendet werden.<br/>Als Sprache für die Markierung und Kategorisierung von Bildern wird derzeit nur Englisch unterstützt.|
|**[Beschreiben eines Bilds](concept-describing-images.md)**|Generieren Sie eine Beschreibung eines gesamten Bilds mit vollständigen Sätzen in lesbarer Sprache. Algorithmen für maschinelles Sehen generieren verschiedene Beschreibungen auf der Grundlage der im Bild erkannten Objekte. Die Beschreibungen werden jeweils ausgewertet, und eine Zuverlässigkeitsbewertung wird generiert. Dann wird eine Liste in der Reihenfolge von höchster Zuverlässigkeitsbewertung zu niedrigster zurückgegeben.|
|**[Erkennen von Gesichtern](concept-detecting-faces.md)** |Erkennen Sie Gesichter in einem Bild, und stellen Sie Informationen zu den einzelnen Gesichtern bereit. Maschinelles Sehen gibt für jedes erkannte Gesicht die Koordinaten, ein Rechteck, das Geschlecht und das Alter zurück.<br/>Maschinelles Sehen bietet einige Funktionen des Diensts [Gesichtserkennung](/azure/cognitive-services/face/). Der Gesichtserkennungsdienst kann für eine eingehendere Analyse (Gesichtsausdruck, Kopfhaltung und Ähnliches) verwendet werden.|
|**[Erkennen von Bildtypen](concept-detecting-image-types.md)**|Erkennen Sie Merkmale eines Bilds – also beispielsweise, ob es sich bei dem Bild um eine Strichzeichnung handelt oder wie wahrscheinlich es ist, dass es sich bei dem Bild um ein ClipArt handelt.|
|**[Erkennen domänenspezifischer Inhalte](concept-detecting-domain-content.md)**|Verwenden Sie Domänenmodelle, um domänenspezifische Inhalte (etwa berühmte Personen und Orientierungspunkte) in einem Bild zu erkennen. Wenn ein Bild also beispielsweise Personen enthält, kann maschinelles Sehen auf ein Domänenmodell für berühmte Personen zurückgreifen und so ermitteln, ob es sich bei den Personen auf dem Bild um berühmte Personen handelt.|
|**[Erkennen des Farbschemas](concept-detecting-color-schemes.md)**|Analysieren Sie die Farben eines Bilds. Maschinelles Sehen kann ermitteln, ob es sich um ein Schwarzweißbild oder um ein Farbbild handelt. Bei Farbbildern kann maschinelles Sehen außerdem die dominante Farbe sowie Akzentfarben erkennen.|
|**[Generieren einer Miniaturansicht](concept-generating-thumbnails.md)**|Analysieren Sie den Inhalt eines Bilds, um eine geeignete Miniaturansicht für das Bild zu generieren. Die Maschinelles Sehen-API generiert zunächst eine hochwertige Miniaturansicht und analysiert dann die Objekte im Bild, um den *relevanten Bereich* zu bestimmen. Anschließend wird das Bild auf den relevanten Bereich zugeschnitten. Das Seitenverhältnis der generierten Miniaturansicht kann sich bei Bedarf vom Seitenverhältnis des ursprünglichen Bilds unterscheiden.|
|**[Abrufen des relevanten Bereichs](concept-generating-thumbnails.md#area-of-interest)**|Analysieren Sie den Inhalt eines Bilds, um die Koordinaten des *relevanten Bereichs* zurückzugeben. Anstatt das Bild zuzuschneiden und eine Miniaturansicht zu generieren, gibt maschinelles Sehen die Koordinaten des Begrenzungsrahmens des Bereichs zurück, sodass die aufrufende Anwendung das ursprüngliche Bild nach Bedarf ändern kann.|

## <a name="optical-character-recognition-ocr"></a>Optische Zeichenerkennung (OCR)

Maschinelles Sehen umfasst auch Funktionen für [optische Zeichenerkennung (Optical Character Recognition, OCR)](concept-recognizing-text.md). Sie können die neue Lese-API verwenden, um gedruckten und handschriftlichen Text aus Bildern und Dokumenten zu extrahieren. Hierbei werden die neuesten Modelle verwendet, und es wird Text auf vielen verschiedenen Flächen und Hintergründen verarbeitet. Beispiele hierfür sind Belege, Poster, Visitenkarten, Briefe und Whiteboards. Die zwei OCR-APIs unterstützen das Extrahieren von gedrucktem Text in [mehreren Sprachen](./language-support.md).

## <a name="moderate-content-in-images"></a>Moderieren von Bildinhalten

Maschinelles Sehen ermöglicht die [Erkennung nicht jugendfreier Inhalte](concept-detecting-adult-content.md) in einem Bild sowie die Rückgabe einer Zuverlässigkeitsbewertung für verschiedene Klassifizierungen. Der Schwellenwert für die Kennzeichnung von Inhalten kann mithilfe eines Schiebereglers nach Bedarf angepasst werden.

## <a name="use-containers"></a>Verwenden von Containern

[Verwenden Sie Container für maschinelles Sehen](computer-vision-how-to-install-containers.md), um gedruckten und handschriftlichen Text lokal zu erkennen. Installieren Sie dazu einen standardisierten Docker-Container, der sich näher bei Ihren Daten befindet.

## <a name="image-requirements"></a>Bildanforderungen

Maschinelles Sehen kann Bilder analysieren, die folgende Anforderungen erfüllen:

- Das Bild muss im JPEG-, PNG-, GIF- oder BMP-Format vorliegen.
- Die Dateigröße muss weniger als 4 MB betragen.
- Das Bild muss größer als 50 x 50 Pixel sein.
  - Für die Lese-API muss die Größe des Bilds zwischen 50 x 50 und 10.000 x 10.000 Pixel liegen.

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Diensten müssen Entwickler, die den Maschinelles Sehen-Dienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie die folgende Schnellstartanleitung als Einführung in die Verwendung von maschinellem Sehen:

- [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp) (.NET)
- [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/client-library.md?pivots=programming-language-python) (Python)
- [Schnellstart: Verwenden der Clientbibliothek für maschinelles Sehen](./quickstarts-sdk/client-library.md?pivots=programming-language-java) (Java)
