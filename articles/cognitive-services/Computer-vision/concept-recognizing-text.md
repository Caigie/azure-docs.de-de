---
title: Erkennung von gedrucktem/handschriftlichem Text – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Konzepte zur Erkennung von gedrucktem und handschriftlichem Text in Bildern mithilfe der Maschinelles Sehen-API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5d0a9771e5b999028996676ea72f8def3c5d63cf
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589855"
---
# <a name="recognize-printed-and-handwritten-text"></a>Erkennen von gedrucktem und handschriftlichem Text

Maschinelles Sehen bietet einige Dienste, die auf Bildern erscheinenden gedruckten oder handgeschriebenen Text erkennen und extrahieren können. Dies ist in vielen Szenarios hilfreich, z.B. beim Erstellen von Notizen, in Patientenakten, in Sicherheitsangelegenheiten und im Bankwesen. In den folgenden drei Abschnitten werden drei verschiedene Texterkennungs-APIs genauer beschrieben, von denen jede für verschiedene Anwendungsfälle optimiert wurde.

## <a name="read-api"></a>Lese-API

Die Lese-API erkennt mithilfe unserer neuesten Erkennungsmodelle Textinhalt auf einem Bild und konvertiert den erkannten Text in eine computerlesbare Zeichendatenfolge. Sie wurde für textlastige Bilder (wie digital eingescannte Dokumente) und für Bilder mit starkem Bildrauschen optimiert. Sie bestimmt, welches Erkennungsmodell für die einzelnen Textzeilen verwendet werden soll und unterstützt Bilder mit gedrucktem und handschriftlichem Text. Die Lese-API wird asynchron ausgeführt, da es bei größeren Dokumenten mehrere Minuten dauern kann, bis ein Ergebnis zurückgegeben wird.

Beim Lesevorgang werden die ursprünglichen Zeilengruppierungen der erkannten Wörter in der Ausgabe beibehalten. Zu jeder Zeile werden die Koordinaten des umgebenden Felds übertragen, und jedes Wort innerhalb der Zeile verfügt über seine eigenen Koordinaten. Wenn ein Wort mit geringer Zuverlässigkeit erkannt wurde, wird diese Information ebenfalls vermittelt. Weitere Informationen finden Sie in der [Referenzdokumentation zur Lese-API 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) oder in der [Referenzdokumentation zur Lese-API 3.0](https://aka.ms/computer-vision-v3-ref).

Der Lesevorgang kann Text in englischer, spanischer, deutscher, französischer, italienischer, portugiesischer und niederländischer Sprache erkennen.

### <a name="image-requirements"></a>Bildanforderungen

Die Lese-API kann Bilder analysieren, die folgende Anforderungen erfüllen:

- Das Bild muss im JPEG-, PNG-, BMP-, PDF- oder TIFF-Format vorliegen.
- Die Größe des Bilds muss zwischen 50 x 50 und 10.000 x 10.000 Pixel liegen. PDF-Seiten dürfen höchstens 17 x 17 Zoll groß sein.
- Die Bilddatei darf höchstens 20 MB groß sein.

### <a name="limitations"></a>Einschränkungen

Wenn Sie ein kostenloses Abonnement nutzen, verarbeitet die Lese-API nur die ersten beiden Seiten eines PDF- oder TIFF-Dokuments. Bei einem bezahlten Abonnement verarbeitet sie bis zu 200 Seiten. Beachten Sie außerdem, dass die API maximal 300 Zeilen pro Seite erkennt.

## <a name="ocr-optical-character-recognition-api"></a>OCR-API (Optical Character Recognition, optische Zeichenerkennung)

Die OCR-API (optische Zeichenerkennung) von Maschinelles Sehen ähnelt der Lese-API, wird allerdings synchron ausgeführt und wurde nicht für große Dokumente optimiert. Sie verwendet ein früheres Erkennungsmodell, funktioniert aber mit mehr Sprachen; unter [Sprachunterstützung](language-support.md#text-recognition) finden Sie eine vollständige Liste der unterstützten Sprachen.

Bei Bedarf korrigiert die OCR die Drehung des erkannten Textes, indem sie den Drehversatz in Grad um die horizontale Bildachse dreht. OCR stellt außerdem, wie in der folgenden Abbildung gezeigt, die Framekoordinaten jedes Worts bereit.

![Ein Bild wird gedreht und der darauf befindliche Text gelesen und beschrieben](./Images/vision-overview-ocr.png)

Weitere Informationen finden Sie in der [OCR-Referenzdokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc).

### <a name="image-requirements"></a>Bildanforderungen

Die OCR-API kann Bilder analysieren, die folgende Anforderungen erfüllen:

* Das Bild muss im JPEG-, PNG-, GIF- oder BMP-Format vorliegen.
* Die Größe des Eingabebilds muss zwischen 50 x 50 und 4200 x 4200 Pixel liegen.
* Der Text kann um ein beliebiges Vielfaches von 90 Grad plus einem kleinen Winkel von bis zu 40 Grad gedreht werden.

### <a name="limitations"></a>Einschränkungen

Bei Fotos, auf denen Text dominiert, kann es durch teilweise erkannte Wörter zu falsch positiven Ergebnissen kommen. Bei einigen Fotos, insbesondere bei Fotos ohne Text, kann die Genauigkeit je nach Bildtyp variieren.

## <a name="recognize-text-api"></a>Texterkennungs-API

> [!NOTE]
> Die Texterkennungs-API wird durch die Lese-API ersetzt. Die Lese-API verfügt über ähnliche Funktionen und wird aktualisiert, sodass sie PDF-, TIFF- und Dokumente mit mehreren Seiten verarbeiten kann.

Die Texterkennungs-API ähnelt OCR, wird jedoch asynchron ausgeführt und verwendet aktualisierte Erkennungsmodelle. Weitere Informationen finden Sie in der [Referenzdokumentation zur Texterkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200).

### <a name="image-requirements"></a>Bildanforderungen

Die Texterkennungs-API kann Bilder analysieren, die folgende Anforderungen erfüllen:

- Das Bild muss im JPEG-, PNG- oder BMP-Format vorliegen.
- Die Größe des Bilds muss zwischen 50 x 50 und 4200 x 4200 Pixel liegen.
- Die Bilddatei darf höchstens 4 MB groß sein.

## <a name="limitations"></a>Einschränkungen

Die Genauigkeit der Texterkennungsvorgänge hängt von der Qualität der Bilder ab. Die folgenden Faktoren können zu einem ungenauen Lesevorgang führen:

* Unscharfe Bilder
* Handschriftlicher oder kursiver Text
* Künstlerische Schriftarten
* Geringe Textgröße
* Komplexe Hintergründe, Schatten oder Blendung über dem Text oder eine perspektivische Verzerrung
* Übergroße oder fehlende Großbuchstaben am Anfang von Wörtern
* Tiefgestellter, hochgestellter oder durchgestrichener Text

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie die Anweisungen unter [Schnellstart: Extrahieren von Text (Lesen)](./QuickStarts/CSharp-hand-text.md), um die Texterkennung in einer einfachen C#-App zu implementieren.
