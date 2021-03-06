---
title: Hero-Beispiel für Gruppenchats
titleSuffix: An Azure Communication Services sample overview
description: Enthält eine Übersicht über das Hero-Beispiel für Chats mit Azure Communication Services, damit Entwickler sich genauer über die Funktionsweise des Beispiels und Änderungsmöglichkeiten informieren können.
author: ddematheu
manager: nimag
services: azure-communication-services
ms.author: dademath
ms.date: 07/20/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 295c4bde64ad21a19d21fd48f2556114b26b202d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944218"
---
# <a name="get-started-with-the-group-chat-hero-sample"></a>Erste Schritte mit dem Hero-Beispiel für Gruppenchats

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

<!----
> [!WARNING]
> links to our Hero Sample repo need to be updated when the sample is publicly available.
---->

Das **Hero-Beispiel für Gruppenchats** von Azure Communication Services veranschaulicht, wie die Communication Services-Webclientbibliothek für Chats verwendet werden kann, um eine Benutzeroberfläche für Gruppenchats zu erstellen.

In dieser Beispiel-Schnellstartanleitung wird beschrieben, wie das Beispiel funktioniert, bevor wir es auf Ihrem lokalen Computer ausführen. Anschließend stellen wir das Beispiel in Azure bereit, indem wir Ihre eigenen Azure Communication Services-Ressourcen verwenden.

> [!IMPORTANT]
> [Laden Sie das Beispiel von GitHub herunter](https://github.com/Azure/Communication/tree/master/samples).

## <a name="overview"></a>Übersicht

Das Beispiel verfügt sowohl über eine clientseitige als auch eine serverseitige Anwendung. Die **clientseitige Anwendung** ist eine React/Redux-Webanwendung, für die das Fluent-UI-Framework von Microsoft verwendet wird. Diese Anwendung sendet Anforderungen an eine **serverseitige ASP.NET Core-Anwendung**, die für die clientseitige Anwendung die Verbindungsherstellung mit Azure ermöglicht. 

Das Beispiel sieht wie folgt aus:

:::image type="content" source="./media/chat/landing-page.png" alt-text="Screenshot: Landing Page der Beispielanwendung":::

Wenn Sie auf die Schaltfläche „Start a chat“ (Chat starten) klicken, ruft die Webanwendung ein Benutzerzugriffstoken aus der serverseitigen Anwendung ab. Dieses Token wird dann verwendet, um die Client-App mit Azure Communication Services zu verbinden. Nach dem Abrufen des Tokens werden Sie aufgefordert, Ihren Namen und Ihr Emoji für den Chat anzugeben. 

:::image type="content" source="./media/chat/pre-chat.png" alt-text="Screenshot: Bildschirm zur Vorbereitung des Chats in der Anwendung":::

Nachdem Sie Ihren Anzeigenamen und Ihr Emoji konfiguriert haben, können Sie der Chatsitzung beitreten. Der Hauptbereich für Chats, der die zentrale Benutzeroberfläche für Chats enthält, wird angezeigt.

:::image type="content" source="./media/chat/main-app.png" alt-text="Screenshot: Hauptbildschirm der Beispielanwendung":::

Komponenten des Hauptbildschirms für Chats:

- **Hauptbereich für Chats**: Dies ist der Hauptbereich für Chats, in dem Benutzer Nachrichten senden und empfangen können. Zum Senden von Nachrichten können Sie den Eingabebereich nutzen und dann die EINGABETASTE drücken (oder die Schaltfläche „Senden“ verwenden). Die empfangenen Chatnachrichten werden nach Absender mit dem zugehörigen Namen und Emoji kategorisiert. Im Chatbereich werden zwei Arten von Benachrichtigungen angezeigt: 1) Eingabebenachrichtigungen, wenn ein Benutzer Text eingibt, und 2) Sende- und Lesebenachrichtigungen für Nachrichten.
- **Header**: Hier werden dem Benutzer der Titel des Chatthreads und die Steuerelemente zum Umschalten der Seitenleiste für die Teilnehmer und die Einstellungen angezeigt. Darüber hinaus ist eine Schaltfläche zum Beenden der Chatsitzung vorhanden.
- **Seitenleiste**: Hier werden die Informationen zu den Teilnehmern und Einstellungen angezeigt, wenn der entsprechende Umschalter im Headerbereich verwendet wird. Die Seitenleiste für Teilnehmer enthält eine Liste mit den Teilnehmern des Chats und einen Link zum Einladen von Teilnehmern zur Chatsitzung. Mit der Seitenleiste für die Einstellungen können Sie den Titel des Chatthreads konfigurieren. 

Unten sind weitere Informationen zu den Voraussetzungen und Schritten zum Einrichten des Beispiels angegeben.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie ein Azure-Konto mit einem aktiven Abonnement. Details finden Sie auf der [Seite zum Erstellen eines kostenloses Azure-Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js (8.11.2 und höher)](https://nodejs.org/en/download/)
- [Visual Studio (2017 und höher)](https://visualstudio.microsoft.com/vs/)
- [.NET Core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2) (Stellen Sie sicher, dass Sie die Version installieren, die Ihrer Visual Studio-Instanz entspricht (32 oder 64 Bit).)
- Erstellen Sie eine Azure Communication Services-Ressource. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../quickstarts/create-communication-resource.md). Sie müssen die **Verbindungszeichenfolge** Ihrer Ressource für diese Schnellstartanleitung aufzeichnen.

## <a name="locally-deploying-the-service--client-app"></a>Lokales Bereitstellen der Dienst- und Client-App

Das Chatbeispiel mit einem Thread umfasst im Wesentlichen zwei Anwendungen: eine Client- und eine Serveranwendung.

Öffnen Sie Visual Studio mit „chat.csproj“, und starten Sie die Ausführung im Debugmodus. Der Front-End-Dienst des Chats wird gestartet. Wenn über den Browser auf die Server-App zugegriffen wird, wird Datenverkehr an den lokal bereitgestellten Front-End-Dienst des Chats umgeleitet.

Sie können das Beispiel lokal testen, indem Sie mehrere Browsersitzungen mit der URL Ihres Chats öffnen, um einen Chat mit mehreren Benutzern zu simulieren.

### <a name="before-running-the-sample-for-the-first-time"></a>Vor dem erstmaligen Ausführen des Beispiels

1. Öffnen Sie eine Instanz von PowerShell, des Windows-Terminals, einer Eingabeaufforderung oder eines gleichwertigen Tools, und navigieren Sie zu dem Verzeichnis, in dem Sie das Beispiel klonen möchten.
2. `git clone`
3. Navigieren Sie zum Ordner **Chat/ClientApp**, und führen Sie `npm run setup` aus.
   1. Falls Fehler 1 angezeigt wird, sollten Sie oben in der Ausgabe nach einer URL suchen, auf die Sie zum Autorisieren Ihres Clients zugreifen müssen. (Die URL sieht in etwa wie folgt aus: `app.vssps.visualstudio.com/oauth2/authorize?clientid=...`.) Nachdem Sie über diese URL auf die Seite zugegriffen haben, kopieren Sie den Befehl aus dem Browserfenster und führen ihn aus.
   2. Führen Sie den Befehl `npm run setup` erneut aus, nachdem Sie den vorherigen Schritt abgeschlossen haben.
4. Rufen Sie im Azure-Portal die `Connection String` (Verbindungszeichenfolge) ab. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Erstellen einer Azure Communication Services-Ressource](../quickstarts/create-communication-resource.md).
5. Fügen Sie nach dem Abrufen von `Connection String` die Zeichenfolge der Datei **Chat/appsettings.json** hinzu, die im Ordner „Chat“ enthalten ist. Geben Sie Ihre Verbindungszeichenfolge in die Variable ein: `ResourceConnectionString`.

### <a name="local-run"></a>Lokaler Testlauf

1. Navigieren Sie zum Ordner „Chat“.
2. Öffnen Sie die Projektmappe `Chat.csproj` in Visual Studio.
3. Führen Sie das Projekt `Chat` aus.*

*Der Browser wird mit „localhost:5000“ geöffnet (hier wird die Client-App vom Knoten bereitgestellt). Die App wird unter Internet Explorer nicht unterstützt.

#### <a name="troubleshooting"></a>Problembehandlung

- Lösung wird nicht erstellt. Während des npm-Installations- bzw. -Buildvorgangs werden Fehler ausgelöst.

Bereinigen/Neuerstellen der C#-Projektmappe

## <a name="publish-the-sample-to-azure"></a>Veröffentlichen des Beispiels in Azure

1. Klicken Sie mit der rechten Maustaste auf das Projekt `Chat`, und wählen Sie „Veröffentlichen“ aus.
2. Erstellen Sie ein neues Veröffentlichungsprofil, und wählen Sie Ihr Azure-Abonnement aus.
3. Fügen Sie vor dem Veröffentlichen Ihre Verbindungszeichenfolge mit `Edit App Service Settings` hinzu. Fügen Sie `ResourceConnectionString` als Schlüssel ein, und geben Sie Ihre Verbindungszeichenfolge (Kopie aus „appsettings.json“) als Wert ein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Informieren Sie sich über das [Bereinigen von Ressourcen](../quickstarts/create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- Informieren Sie sich über [Chatkonzepte](../concepts/chat/concepts.md).
- Machen Sie sich mit unserer [Chatclientbibliothek](../concepts/chat/sdk-features.md) vertraut.

## <a name="additional-reading"></a>Zusätzliche Lektüre

- [Azure Communication: Vorschau](https://github.com/Azure/communication-preview): Weitere Informationen zum Web-SDK für Chats
- [Redux](https://redux.js.org/): Clientseitige Zustandsverwaltung
- [Fluent-UI](https://developer.microsoft.com/fluentui#/): UI-Bibliothek von Microsoft
- [React](https://reactjs.org/): Bibliothek zum Erstellen von Benutzeroberflächen
- [ASP.NET Core](https://docs.microsoft.com/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-3.1&preserve-view=true): Framework für die Erstellung von Webanwendungen
