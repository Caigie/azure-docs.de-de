---
title: Codieren der benutzerdefinierten Transformation mit Media Services v3 .NET – Azure | Microsoft-Dokumentation
description: In diesem Thema wird gezeigt, wie mit Azure Media Services v3 eine benutzerdefinierte Transformation mithilfe von .NET codiert wird.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: b5bb505df3cad7856e0b08f04949c2e56ccec1ca
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89295613"
---
# <a name="how-to-encode-with-a-custom-transform---net"></a>Codieren mit einer benutzerdefinierten Transformation – .NET

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bei der Codierung mit Azure Media Services können Sie mit einer der empfohlenen integrierten Voreinstellungen, basierend auf in der Branche bewährten Vorgehensweisen, schnell einsteigen, wie im Tutorial [Hochladen, Codieren und Streamen von Videos mithilfe von .NET](stream-files-tutorial-with-api.md) gezeigt. Sie können auch eine benutzerdefinierte Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen entwickeln.

## <a name="considerations"></a>Überlegungen

Beim Erstellen von benutzerdefinierten Voreinstellungen gelten die folgenden Überlegungen:

* Alle Werte für Höhe und Breite in AVC-Inhalt müssen ein Vielfaches von 4 sein.
* In Azure Media Services v3 werden alle Codierungsbitraten in Bits pro Sekunde angegeben. Dies unterscheidet sich von den Voreinstellungen bei unseren v2-APIs. Dort wurden Kilobits pro Sekunde (KBit/s) als Einheit verwendet. Wenn beispielsweise die Bitrate in v2 als 128 (Kilobits/Sekunde) angegeben wurde, würde sie in v3 auf 128.000 (Bits/Sekunde) festgelegt.

## <a name="prerequisites"></a>Voraussetzungen 

[Erstellen eines Media Services-Kontos](./create-account-howto.md)

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie ein GitHub-Repository auf Ihren Computer, das das vollständige .NET Core-Beispiel enthält, indem Sie den folgenden Befehl verwenden:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
Das benutzerdefinierte voreingestellte Beispiel befindet sich im Ordner [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/).

## <a name="create-a-transform-with-a-custom-preset"></a>Erstellen einer Transformation mit einer benutzerdefinierten Voreinstellung 

Beim Erstellen einer neuen [Transformation](/rest/api/media/transforms) müssen Sie angeben, was als Ausgabe generiert werden soll. Der erforderliche Parameter ist ein [TransformOutput](/rest/api/media/transforms/createorupdate#transformoutput)-Objekt, wie im folgenden Code gezeigt. Jedes **TransformOutput**-Objekt enthält eine **Voreinstellung**. Die **Voreinstellung** beschreibt die schrittweisen Anweisungen von Video- und/oder Audioverarbeitungsvorgängen, die verwendet werden sollen, um das gewünschte **TransformOutput**-Objekt zu generieren. Das folgende **TransformOutput**-Objekt erstellt benutzerdefinierte Codec- und Layerausgabeeinstellungen.

Beim Erstellen einer [Transformation](/rest/api/media/transforms) sollten Sie zunächst mit der **Get**-Methode überprüfen, ob eine solche bereits vorhanden ist. Der folgende Code zeigt dies. In Media Services v3 geben **Get**-Methoden für Entitäten **NULL** zurück, wenn die Entität nicht vorhanden ist (eine Überprüfung des Namens ohne Unterscheidung zwischen Groß-/Kleinschreibung).

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein Satz von Ausgaben definiert, der bei Verwendung dieser Transformation generiert werden soll. Zuerst fügen wir eine AacAudio-Ebene für die Audiocodierung und zwei H264Video-Ebenen für die Videocodierung hinzu. In den Videoebenen weisen wir Bezeichnungen zu, damit sie in den Ausgabedateinamen verwendet werden können. Als Nächstes soll die Ausgabe auch Miniaturansichten enthalten. Im Beispiel unten geben wir Bilder im PNG-Format an, die mit 50 % der Auflösung vom Eingabevideo generiert wurden und drei Zeitstempel aufweisen – {25 %, 50 %, 75 %} der Länge des Eingabevideos. Schließlich geben wir das Format für die Ausgabedateien an – eine für Video+Audio und eine andere für die Miniaturansichten. Weil wir mehrere H264-Ebenen haben, müssen wir Makros verwenden, die eindeutige Namen pro Ebene generieren. Wir können entweder ein `{Label}`- oder ein `{Bitrate}`-Makro verwenden. Im Beispiel wird erstere Option verwendet.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Nächste Schritte

[Streamingdateien](stream-files-tutorial-with-api.md) 
