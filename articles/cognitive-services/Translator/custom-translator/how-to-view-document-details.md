---
title: Dokumentdetails – Custom Translator
titleSuffix: Azure Cognitive Services
description: Die Seite mit der Dokumentenliste zeigt die ersten zehn Dokumente in Ihrem Arbeitsbereich an. Für jedes Dokument werden Name, Paare, Typ, Sprache, Uploadzeitstempel und E-Mail-Adresse des Benutzers angezeigt, der das Dokument hochgeladen hat.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 22c982babbed1322e97127379be75e7b070ed81f
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994241"
---
# <a name="view-document-details"></a>Anzeigen der Dokumentdetails

Die Seite mit der Dokumentenliste zeigt die ersten zehn Dokumente in Ihrem Arbeitsbereich an. Für jedes Dokument werden Name, Paare, Typ, Sprache, Uploadzeitstempel und E-Mail-Adresse des Benutzers angezeigt, der das Dokument hochgeladen hat.

Klicken Sie auf ein Dokument, um die Seite mit den entsprechenden Dokumentdetails anzuzeigen. Auf der Seite mit den Dokumentdetails wird die Liste der aus dem Dokument extrahierten Sätze angezeigt.

- In der Standardeinstellung ist im Dropdownfeld die Ausgangssprache ausgewählt. Sie können jedoch auch festlegen, dass die Sätze in der Zielsprache angezeigt werden.
- Standardmäßig werden pro Seite 20 Sätze angezeigt. Sie können mit dem Steuerelement zum Paginieren zwischen den Seiten wechseln.

![Dokumentdetails](media/how-to/how-to-view-document-details.png)

## <a name="delete-a-document"></a>Löschen eines Dokuments

Der Benutzer muss der Besitzer des Arbeitsbereichs sein, um ein Dokument zu löschen. Wenn ein Dokument von einem Modell verwendet wird, das sich in einer beliebigen Phase des Trainings- oder Bereitstellungsprozesses befindet, kann das Dokument nicht gelöscht werden.

1. Navigieren Sie zur Seite „Dokumente“.
2.  Zeigen Sie mit der Maus auf einen beliebigen Dokumentdatensatz, und klicken Sie auf das Papierkorbsymbol.

    ![Löschen eines Dokuments](media/how-to/how-to-delete-document-1.png)

3.  Bestätigen Sie den Löschvorgang.

    ![Bestätigen des Löschvorgangs](media/how-to/how-to-delete-document-confirm.png)

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie, wie Sie ein [Modell trainieren](how-to-train-model.md).
