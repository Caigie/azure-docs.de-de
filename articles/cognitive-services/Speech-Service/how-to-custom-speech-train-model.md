---
title: 'Speech-Dienst: Trainieren eines Modells für Custom Speech'
titleSuffix: Azure Cognitive Services
description: Das Training einer Spracherkennung kann die Erkennungsgenauigkeit sowohl für das Microsoft-Basismodell als auch für ein benutzerdefiniertes Modell verbessern. Ein Modell wird mithilfe von menschenmarkierten Transkriptionen und zugehörigem Text trainiert.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: 09867d6739998902ea0fd11281d1b975866c1242
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83722625"
---
# <a name="train-a-model-for-custom-speech"></a>Trainieren eines Modells für Custom Speech

Das Training einer Spracherkennung kann die Erkennungsgenauigkeit für das Microsoft-Basismodell verbessern. Ein Modell wird mithilfe von menschenmarkierten Transkriptionen und zugehörigem Text trainiert. Diese Datasets werden zusammen mit zuvor hochgeladenen Audiodaten verwendet, um das Spracherkennungsmodell zu optimieren und zu trainieren, sodass es Wörter, Phrasen, Akronyme, Namen und andere produktspezifische Begriffe erkennt. Je mehr domäneninterne Datasets Sie bereitstellen (Daten, die sich darauf beziehen, was Benutzer sagen und was erwartungsgemäß erkannt wird), desto genauer wird Ihr Modell sein, was zu einer besseren Erkennung führt. Denken Sie daran, dass Sie durch die Eingabe von unabhängigen Daten in Ihr Training die Genauigkeit Ihres Modells reduzieren oder beeinträchtigen können.

## <a name="use-training-to-resolve-accuracy-issues"></a>Lösen von Genauigkeitsproblemen durch Training

Wenn Sie mit Ihrem Modell auf Erkennungsprobleme stoßen, kann die Verwendung von menschenmarkierten Transkripten und zugehörigen Daten für zusätzliches Training zur Verbesserung der Genauigkeit beitragen. Bestimmen Sie anhand von dieser Tabelle, welches Dataset zum Beheben Ihrer Probleme verwendet werden soll:

| Anwendungsfall | Datentyp |
| -------- | --------- |
| Verbessern der Erkennungsgenauigkeit für branchenspezifisches Vokabular und entsprechende Grammatik (z. B. aus der Medizin- oder IT-Branche). | Zugehöriger Text (Sätze/Äußerungen) |
| Definieren der phonetischen und angezeigten Form eines Worts oder Begriffs mit nicht standardmäßiger Aussprache (beispielsweise Produktnamen oder Akronyme) | Zugehöriger Text (Aussprache) |
| Verbessern der Erkennungsgenauigkeit für Sprechweisen, Akzente oder bestimmte Hintergrundgeräusche. | Audio + menschenmarkierte Transkripte |

> [!IMPORTANT]
> Wenn Sie kein Dataset hochgeladen haben, beachten Sie [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md). Dieses Dokument enthält Anweisungen zum Hochladen von Daten und Richtlinien zum Erstellen von Datasets mit hoher Qualität.

## <a name="train-and-evaluate-a-model"></a>Trainieren und Bewerten eines Modells

Der erste Schritt beim Trainieren eines Modells ist das Hochladen von Trainingsdaten. Unter [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md) finden Sie schrittweise Anweisungen zum Vorbereiten von menschenmarkierten Transkriptionen und zugehörigem Text (Äußerungen und Aussprache). Nachdem Sie Trainingsdaten hochgeladen haben, folgen Sie diesen Anweisungen, um mit dem Trainieren Ihres Modells zu beginnen:

1. Melden Sie sich beim [Custom Speech-Portal](https://speech.microsoft.com/customspeech) an.
2. Navigieren Sie zu **Spracherkennung > Custom Speech > Training**.
3. Klicken Sie auf **Modell trainieren**.
4. Geben Sie als Nächstes einen **Namen** und eine **Beschreibung** für Ihr Training ein.
5. Wählen Sie aus dem Dropdownmenü für **Szenario und Basismodell** das für Ihre Domäne am besten geeignete Szenario aus. Wenn Sie sich nicht sicher sind, welches Szenario Sie wählen sollen, wählen Sie **Allgemein** aus. Das Basismodell stellt den Ausgangspunkt für das Training dar. Das neueste Modell ist in der Regel die beste Wahl.
6. Wählen Sie auf der Seite **Trainingsdaten auswählen** ein oder mehrere Audiodatasets und menschenmarkierten Transkriptionsdatasets aus, die Sie für das Training verwenden möchten.
7. Nachdem das Training abgeschlossen ist, können Sie sich für das Ausführen von Genauigkeitsprüfungen für das neu trainierte Modell entscheiden. Dieser Schritt ist optional.
8. Wählen Sie **Erstellen** aus, um ein benutzerdefiniertes Modell zu erstellen.

In der Trainingstabelle wird ein neuer Eintrag angezeigt, der diesem neu erstellten Modell entspricht. Außerdem zeigt die Tabelle den Status an: „Verarbeitung“, „Erfolgreich“ oder „Fehler“.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Bewerten der Genauigkeit eines trainierten Modells

Mithilfe dieser Dokumente können Sie die Daten untersuchen und die Modellgenauigkeit bewerten:

- [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
- [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)

Wenn Sie sich für einen Genauigkeitstest entscheiden, ist es wichtig, ein anderes akustisches Dataset auszuwählen, als Sie für Ihr Modell verwendet haben, um ein realistisches Bild von der Leistung des Modells zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen Ihres Modells](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md)
- [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
- [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)
- [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)
