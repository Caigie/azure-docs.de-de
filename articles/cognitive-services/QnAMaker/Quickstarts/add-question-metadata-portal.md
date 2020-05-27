---
title: 'Schnellstart: Hinzufügen von Fragen und Antworten im QnA Maker-Portal'
description: In diesem Schnellstart erfahren Sie, wie Sie Frage-Antwort-Sätze mit Metadaten hinzufügen, damit Ihre Benutzer die richtige Antwort auf ihre Frage finden können.
ms.topic: quickstart
ms.date: 05/26/2020
ms.openlocfilehash: fc8bac425723e9194c447f1cf6ee13547c09d772
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873692"
---
# <a name="quickstart-add-questions-and-answer-with-qna-maker-portal"></a>Schnellstart: Hinzufügen von Fragen und Antworten im QnA Maker-Portal

Fügen Sie nach der Erstellung einer Wissensdatenbank Frage-Antwort-Sätze (Question and Answer, QnA) mit Metadaten zum Filtern der Antwort hinzu. Die Fragen in der folgenden Tabelle hängen alle mit Azure-Diensteinschränkungen zusammen, wobei sich aber jede auf einen anderen Azure-Dienst bezieht.

<a name="qna-table"></a>

|Set|Fragen|Antwort|Metadaten|
|--|--|--|--|
|1|`How large a knowledge base can I create?`<br><br>`What is the max size of a knowledge base?`<br><br>`How many GB of data can a knowledge base hold?` |`The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`|`service=qna_maker`<br>`link_in_answer=true`|
|2|`How many knowledge bases can I have for my QnA Maker service?`<br><br>`I selected a Azure Cognitive Search tier that holds 15 knowledge bases, but I can only create 14 - what is going on?`<br><br>`What is the connection between the number of knowledge bases in my QnA Maker service and the Azure Cognitive Search service size?` |`Each knowledge base uses 1 index, and all the knowledge bases share a test index. You can have N-1 knowledge bases where N is the number of indexes your Azure Cognitive Search tier supports.`|`service=search`<br>`link_in_answer=false`|

Sobald Metadaten zu einem Frage-Antwort-Paar hinzugefügt wurden, kann die Clientanwendung folgende Aktionen ausführen:

* Antworten anfordern, die nur bestimmten Metadaten entsprechen
* Alle Antworten empfangen, die Antworten jedoch in Abhängigkeit von den Metadaten für die einzelnen Antworten nachverarbeiten


## <a name="prerequisites"></a>Voraussetzungen

* Absolvieren Sie den [vorherigen Schnellstart](./create-publish-knowledge-base.md).

## <a name="sign-in-to-the-qna-maker-portal"></a>Anmelden beim QnA Maker-Portal

1. Melden Sie sich beim [QnA Maker-Portal](https://www.qnamaker.ai) an.

1. Wählen Sie die vorhandene Wissensdatenbank aus der [vorherigen Schnellstartanleitung](../how-to/create-knowledge-base.md) aus.

## <a name="add-additional-alternatively-phrased-questions"></a>Hinzufügen zusätzlicher Fragen mit alternativen Formulierungen

Die aktuelle Wissensdatenbank enthält die Frage-Antwort-Paare für die QnA Maker-Problembehandlung. Diese Sätze wurden erstellt, als die URL während des Erstellungvorgangs zur Wissensdatenbank hinzugefügt wurde.

Beim Importieren dieser URL wurde nur eine Frage mit einer Antwort erstellt. Fügen Sie in diesem Verfahren weitere Fragen hinzu.

1. Verwenden Sie auf der Seite **Edit** (Bearbeiten) das Suchfeld über den Frage-Antwort-Sätzen, um die Frage `How large a knowledge base can I create?` zu suchen.

1. Wählen Sie in der Spalte **Question** (Frage) die Option **+ Add alternative phrasing** (Alternative Formulierung hinzufügen) aus, und fügen Sie anschließend die neuen Formulierungen hinzu, die in der folgenden Tabelle angegeben sind.

    |Alternative Formulierung|
    |--|
    |`What is the max size of a knowledge base?`|
    |`How many GB of data can a knowledge base hold?`|

1. Wählen Sie **Save and train** (Speichern und trainieren) aus, um die Wissensdatenbank erneut zu trainieren.

1. Wählen Sie **Test** (Testen) aus, und geben Sie eine Frage ein, die einem der neuen alternativen Ausdrücke ähnelt, aber nicht genau denselben Wortlaut hat:

    `What GB size can a knowledge base be?`

    Die richtige Antwort wird im Markdownformat zurückgegeben:

    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`

    Wenn Sie unter der zurückgegebenen Antwort **Inspect** (Überprüfen) auswählen, sehen Sie, dass mehrere Antworten zur Frage passen, aber nicht mit der gleichen hohen Zuverlässigkeit.

    Fügen Sie nicht alle möglichen Kombinationen alternativer Formulierungen hinzu. Wenn Sie [aktives Lernen](../how-to/improve-knowledge-base.md) für QnA Maker aktivieren, werden alternative Formulierungen gesucht, die Ihre Wissensdatenbank am besten unterstützen, um die Anforderungen Ihrer Benutzer zu erfüllen.

1. Wählen Sie erneut **Test** (Testen) aus, um das Testfenster zu schließen.

## <a name="add-metadata-to-filter-the-answers"></a>Hinzufügen von Metadaten zum Filtern der Antworten

Durch das Hinzufügen von Metadaten zu einem Frage-Antwort-Satz kann Ihre Clientanwendung gefilterte Antworten anfordern. Dieser Filter wird angewendet, bevor [die erste und zweite Rangfolge](../concepts/query-knowledge-base.md#ranker-process) angewendet wird.

1. Fügen Sie den zweiten Frage-Antwort-Satz ohne Metadaten aus der [ersten Tabelle in dieser Schnellstartanleitung](#qna-table) hinzu, und fahren Sie dann mit den folgenden Schritten fort:

1. Wählen Sie **View options** (Optionen anzeigen) und anschließend **Show metadata** (Metadaten anzeigen) aus.

1. Wählen Sie für das Frage-Antwort-Paar, das Sie gerade hinzugefügt haben, die Option **Add metadata tags** (Metadatentags hinzufügen) aus, und fügen Sie dann den Namen `service` sowie den Wert `search` hinzu. Es sieht folgendermaßen aus: `service:search`.

1. Fügen Sie weitere Metadatentags mit dem Namen `link_in_answer` und dem Wert `false` hinzu. Es sieht folgendermaßen aus: `link_in_answer:false`.

1. Suchen Sie in der Tabelle `How large a knowledge base can I create?` nach der ersten Antwort.

1. Fügen Sie für die gleichen zwei Metadatentags Metadatenpaare hinzu:

    `link_in_answer` : `true`<br>
    `server`: `qna_maker`

    Sie haben jetzt zwei Fragen mit denselben Metadatentags mit unterschiedlichen Werten.

1. Wählen Sie **Save and train** (Speichern und trainieren) aus, um die Wissensdatenbank erneut zu trainieren.

1. Wählen Sie **Publish** (Veröffentlichen) aus, um zur Veröffentlichungsseite zu wechseln.
1. Wählen Sie die Schaltfläche **Publish** (Veröffentlichen) aus, um die aktuelle Wissensdatenbank auf dem Endpunkt zu veröffentlichen.
1. Fahren Sie nach der Veröffentlichung der Wissensdatenbank mit der nächsten Schnellstartanleitung fort, um zu erfahren, wie Sie eine Antwort über Ihre Wissensdatenbank generieren.

## <a name="what-did-you-accomplish"></a>Was haben Sie erreicht?

Sie haben Ihre Wissensdatenbank bearbeitet, um weitere Fragen zu unterstützen, und Name-Wert-Paare angegeben, um die Filterung während der Suche nach der besten Antwort bzw. die Nachbearbeitung nach der Rückgabe der Antworten zu unterstützen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit der nächsten Schnellstartanleitung fortfahren, löschen Sie die QnA Maker- und Bot Framework-Ressourcen im Azure-Portal.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen einer Antwort mit Postman oder cURL](get-answer-from-knowledge-base-using-url-tool.md)