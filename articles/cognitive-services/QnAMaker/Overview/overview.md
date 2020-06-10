---
title: Was ist der QnA Maker-Dienst?
description: QnA Maker ist ein cloudbasierter NLP-Dienst zur mühelosen Erstellung einer natürlichen Konversationsebene für Ihre Daten. Er kann verwendet werden, um für eine beliebige Eingabe in natürlicher Sprache die am besten geeignete Antwort aus Ihrer benutzerdefinierten Wissensdatenbank (Knowledge Base, KB) zu finden.
ms.topic: overview
ms.date: 05/26/2020
ms.openlocfilehash: d2ff2d789f2ea1ae6018d95ef1d880da87b4ff74
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994866"
---
# <a name="what-is-the-qna-maker-service"></a>Was ist der QnA Maker-Dienst?

[!INCLUDE [TLS 1.2 enforcement](../../../../includes/cognitive-services-tls-announcement.md)]

QnA Maker ist ein cloudbasierter NLP-Dienst (Natural Language Processing, Verarbeitung natürlicher Sprache) zur mühelosen Erstellung einer natürlichen Konversationsebene für Ihre Daten. Er kann verwendet werden, um für eine beliebige Eingabe in natürlicher Sprache die am besten geeignete Antwort aus Ihrer benutzerdefinierten Wissensdatenbank (Knowledge Base, KB) zu finden.

Bei einer Clientanwendung für QnA Maker handelt es sich um eine beliebige Konversationsanwendung, die mit einem Benutzer in natürlicher Sprache kommuniziert, um eine Frage zu beantworten. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.

## <a name="when-to-use-qna-maker"></a>Einsatzgebiete von QnA Maker

* **Statische Informationen:** Verwenden Sie QnA Maker, wenn Ihre Antwort-Wissensdatenbank statische Informationen enthält. Diese Wissensdatenbank ist speziell auf Ihre Anforderungen zugeschnitten und wurde anhand von Dokumenten wie [PDFs und URLs](../concepts/content-types.md) erstellt.
* **Bereitstellung der gleichen Antwort für eine Anforderung, eine Frage oder einen Befehl:** Wenn verschiedene Benutzer die gleiche Frage stellen, erhalten sie die gleiche Antwort.
* **Filterung statischer Informationen auf der Grundlage von Metainformationen:** Fügen Sie [Metadatentags](../how-to/metadata-generateanswer-usage.md) hinzu, um zusätzliche Filteroptionen bereitzustellen, die für die Benutzer Ihrer Clientanwendung und die Informationen relevant sind. Zu den gängigen Metadateninformationen zählen [Geplauder](../how-to/chit-chat-knowledge-base.md), Inhaltstyp oder -format, Inhaltszweck und Inhaltsaktualität.
* **Verwaltung einer Botkonversation, die statische Informationen beinhaltet:** Ihre Wissensdatenbank liefert eine Antwort für den Konversationstext oder Befehl eines Benutzers. Ist die Antwort Teil eines vordefinierten Konversationsablaufs (dargestellt in Ihrer Wissensdatenbank mit [Mehrfachdurchlauf-Kontext](../how-to/multiturn-conversation.md)), kann der Bot diesen Ablauf problemlos bereitstellen.

## <a name="use-qna-maker-knowledge-base-in-a-chat-bot"></a>Verwenden der QnA Maker-Wissensdatenbank in einem Chatbot

Nach der Veröffentlichung einer QnA Maker Wissensdatenbank sendet eine Clientanwendung eine Frage an den Endpunkt Ihrer Wissensdatenbank und erhält die Ergebnisse in Form einer JSON-Antwort. Eine gängige Clientanwendung für QnA Maker ist ein Chatbot.

![Stellen einer Frage an einen Bot und Erhalten einer Antwort aus dem Inhalt der Wissensdatenbank](../media/qnamaker-overview-learnabout/bot-chat-with-qnamaker.png)

|Schritt|Aktion|
|:--|:--|
|1|Die Clientanwendung sendet die frei formulierte _Frage_ des Benutzers „How do I programmatically update my Knowledge Base?“ (Wie kann ich meine Wissensdatenbank programmgesteuert aktualisieren?) an den Endpunkt Ihrer Wissensdatenbank.|
|2|QnA Maker verwendet die trainierte Wissensdatenbank, um die korrekte Antwort sowie ggf. weitere Folgeaufforderungen bereitzustellen, die zur Eingrenzung der Suche nach der besten Antwort beitragen. QnA Maker gibt eine Antwort im JSON-Format zurück.|
|3|Die Clientanwendung entscheidet auf der Grundlage der JSON-Antwort über den weiteren Verlauf der Konversation. Mögliche Entscheidungen wären etwa, die beste Antwort anzuzeigen und weitere Optionen zu präsentieren, um die Suche nach der besten Antwort einzugrenzen. |
|||

## <a name="what-is-a-knowledge-base"></a>Was ist eine Wissensdatenbank?

QnA Maker [importiert Ihre Inhalte](../concepts/knowledge-base.md) in eine Wissensdatenbank, die aus Frage-Antwort-Paaren besteht. Im Zuge des Importvorgangs werden Informationen zur Beziehung zwischen den Teilen Ihrer strukturierten und teilweise strukturierten Inhalte extrahiert, um Beziehungen zwischen den Frage-Antwort-Paaren zu implizieren. Sie können diese Frage-Antwort-Paare bearbeiten oder neue hinzufügen.

Der Inhalt des Frage-Antwort-Paars umfasst Folgendes:
* Alle alternativen Formen der Frage
* Metadatentags zum Filtern von Antwortoptionen während der Suche
* Folgeaufforderungen zum weiteren Optimieren der Suche

![Beispielfrage und -antwort mit Metadaten](../media/qnamaker-overview-learnabout/example-question-and-answer-with-metadata.png)

Nach der Veröffentlichung Ihrer Wissensdatenbank sendet eine Clientanwendung eine Benutzerfrage an Ihren Endpunkt. Ihr QnA Maker-Dienst verarbeitet die Frage und gibt die beste Antwort zurück.

## <a name="create-manage-and-publish-to-a-bot-without-code"></a>Erstellen, Verwalten und Veröffentlichen für einen Bot ohne Code

Eine Wissensdatenbank kann vollständig über das QnA Maker-Portal erstellt werden. Sie können Dokumente in ihrer aktuellen Form in Ihre Wissensdatenbank importieren. Diese Dokumente (häufig gestellte Fragen, Handbücher, Arbeitsblätter, Webseiten oder Ähnliches) werden in Frage-Antwort-Paaren konvertiert. Jedes Paar wird nach Folgeaufforderungen analysiert und mit anderen Paaren verknüpft. Das endgültige _Markdownformat_ unterstützt eine hochwertige Darstellung mit Bildern und Links.

Veröffentlichen Sie die Wissensdatenbank nach der Bearbeitung ganz ohne Programmieraufwand für einen funktionierenden [Azure-Web-App-Bot](https://azure.microsoft.com/services/bot-service/). Testen Sie Ihren Bot im [Azure-Portal](https://portal.azure.com), oder setzen Sie die Entwicklung nach dem Herunterladen fort.

## <a name="search-quality-and-ranking-provides-the-best-possible-answer"></a>Bestmögliche Antwort auf der Grundlage von Suchqualität und Bewertung

Das QnA Maker-System basiert auf einem Bewertungsansatz mit mehreren Ebenen. Die Daten werden in Azure Search gespeichert. Dies ist auch gleichzeitig die erste Bewertungsebene. Die besten Ergebnisse von Azure Search werden dann an das NLP-Neubewertungsmodell von QnA Maker übergeben, um die endgültigen Ergebnisse und die Zuverlässigkeitsbewertung zu generieren.

## <a name="qna-maker-improves-the-conversation-process"></a>Verbessern des Konversationsprozesses mit QnA Maker

QnA Maker bietet Eingabeaufforderungen mit Mehrfachdurchläufen und aktives Lernen, um Sie bei der Verbesserung Ihrer grundlegenden Frage-Antwort-Paare zu unterstützen.

**Eingabeaufforderungen mit Mehrfachdurchläufen** ermöglichen die Verknüpfung von Frage-Antwort-Paaren. Diese Verknüpfung ermöglicht es der Clientanwendung, eine Top-Antwort zurückzugeben, und stellt weitere Fragen bereit, um die Suche nach einer abschließenden Antwort einzugrenzen.

Nachdem die Wissensdatenbank Benutzerfragen über den veröffentlichten Endpunkt erhalten hat, wendet QnA Maker **aktives Lernen** auf diese Fragen aus der Praxis an, um Änderungen an Ihrer Wissensdatenbank vorzuschlagen, die zur Verbesserung der Qualität beitragen.

## <a name="development-lifecycle"></a>Lebenszyklus der Entwicklung

QnA Maker bietet Erstellungs-, Trainings- und Veröffentlichungsfunktionen sowie Zusammenarbeitsberechtigungen und lässt sich somit in den gesamten Entwicklungszyklus integrieren.

> [!div class="mx-imgBorder"]
> ![Konzeptuelle Darstellung des Entwicklungszyklus](../media/qnamaker-overview-learnabout/development-cycle.png)


## <a name="how-do-i-start"></a>Wie kann ich anfangen?

**Schritt 1:** Erstellen Sie eine QnA Maker-Ressource über das [Azure-Portal](https://portal.azure.com).

**Schritt 2:** Erstellen Sie eine Wissensdatenbank über das Portal von [QnA Maker](https://www.qnamaker.ai). Fügen Sie [Dateien und URLs](../concepts/content-types.md) hinzu, um die Wissensdatenbank zu erstellen.

**Schritt 3:** Veröffentlichen Sie Ihre Wissensdatenbank, und testen Sie sie an Ihrem benutzerdefinierten Endpunkt unter Verwendung von [cURL oder Postman](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md).

**Schritt 4:** Greifen Sie in der Clientanwendung programmgesteuert auf den Endpunkt Ihrer Wissensdatenbank zu. Die Clientanwendung verarbeitet die JSON-Antwort, um dem Benutzer die beste Antwort anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte
QnA Maker bietet alles, was Sie zum Erstellen, Verwalten und Bereitstellen Ihrer benutzerdefinierten Wissensdatenbank benötigen.

> [!div class="nextstepaction"]
> [Überprüfen der aktuellen Änderungen](../whats-new.md)
