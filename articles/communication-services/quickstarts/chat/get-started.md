---
title: 'Schnellstart: Hinzufügen von Chatfunktionen zu Ihrer App'
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihrer App Chatfunktionen von Communication Services hinzufügen.
author: fanche
manager: phans
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: ed40f91c760d7c83466078b3523748175a1701ad
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944485"
---
# <a name="quickstart-add-chat-to-your-app"></a>Schnellstart: Hinzufügen von Chatfunktionen zu Ihrer App

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Steigen Sie in Azure Communication Services ein, indem Sie die Communication Services-Clientbibliothek für Chats nutzen, um Ihre Anwendung mit Echtzeitchatfunktionen zu versehen. In dieser Schnellstartanleitung wird die Clientbibliothek für Chats verwendet, um Chatthreads zu erstellen, die es Benutzern ermöglichen, miteinander zu kommunizieren. Weitere Informationen zu Chatkonzepten finden Sie in der [Dokumentation zu Chatkonzepten](../../concepts/chat/concepts.md).

::: zone pivot="programming-language-javascript"
[!INCLUDE [Chat with JavaScript client library](./includes/chat-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Chat with Python client library](./includes/chat-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Chat with Java client library](./includes/chat-java.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Chat with C# client library](./includes/chat-csharp.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Chatclients
> * Erstellen eines Threads mit zwei Benutzern
> * Senden einer Nachricht an den Thread
> * Empfangen von Nachrichten aus einem Thread
> * Entfernen von Benutzern aus einem Thread

> [!div class="nextstepaction"]
> [Ausprobieren der Hero-App für Chats](../../samples/chat-hero-sample.md)

Das könnte Sie auch interessieren:

 - Informieren Sie sich über [Chatkonzepte](../../concepts/chat/concepts.md).
 - Machen Sie sich mit der [Clientbibliothek für Chats](../../concepts/chat/sdk-features.md) vertraut.
