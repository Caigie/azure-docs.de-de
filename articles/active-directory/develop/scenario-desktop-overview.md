---
title: Erstellen einer Desktop-App, die Web-APIs aufruft | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (Übersicht)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 92f0909660427e414264442523dba3ed2abe0142
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771858"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Szenario: Desktop-App, die Web-APIs aufruft

Erfahren Sie alles, was Sie brauchen, um eine Desktop-App zu erstellen, die Web-APIs aufruft.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="get-started"></a>Erste Schritte

Sofern nicht bereits geschehen, erstellen Sie nun Ihre erste Anwendung, indem Sie den Schritten in der .NET Desktop-Schnellstartanleitung, der Universal Windows Platform (UWP)-Schnellstartanleitung oder der Schnellstartanleitung für native macOS-Apps folgen:

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Windows Desktop-App aus](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer UWP-App aus](./quickstart-v2-uwp.md)

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API aus einer nativen macOS-App](./quickstart-v2-ios.md)

## <a name="overview"></a>Übersicht

Sie schreiben eine Desktopanwendung und möchten Benutzer bei Ihrer Anwendung anmelden sowie Web-APIs wie Microsoft Graph, andere Microsoft-APIs oder Ihre eigene Web-API aufrufen. Sie haben hierfür mehrere Möglichkeiten:

- Sie können den interaktiven Tokenabruf verwenden:

  - Wenn Ihre Desktopanwendung grafische Steuerelemente unterstützt (z. B. bei einer Windows.Form-Anwendung, einer WPF-Anwendung oder einer nativen macOS-Anwendung).
  - Wenn es sich um eine .NET Core-Anwendung handelt, und Sie zustimmen, dass die Authentifizierungsinteraktion mit Azure Active Directory (Azure AD) im Systembrowser erfolgt.

- Bei Anwendungen, die von Windows gehostet werden, kann ein Token mithilfe der integrierten Windows-Authentifizierung automatisch abgerufen werden, wenn die Anwendungen auf Computern ausgeführt werden, die zu einer Windows-Domäne gehören oder in Azure AD eingebunden sind.
- Schließlich können Sie, obwohl dies nicht empfohlen wird, in öffentlichen Clientanwendungen Benutzername und Kennwort verwenden. Dies wird noch bei einigen Szenarien (z. B. DevOps) benötigt. Die Verwendung ist allerdings mit Einschränkungen für Ihre Anwendung verbunden. Beispielsweise kann sich ein Benutzer, der die [mehrstufige Authentifizierung](../authentication/concept-mfa-howitworks.md) (bedingter Zugriff) durchführen muss, nicht darüber anmelden. Auch einmaliges Anmelden wäre für Ihre Anwendung dann nicht möglich.

  Diese Möglichkeit widerspricht außerdem den Prinzipien moderner Authentifizierung und steht nur aus Legacygründen zur Auswahl.

  ![Desktopanwendung](media/scenarios/desktop-app.svg)

- Wenn Sie ein portables Befehlszeilentool schreiben (möglicherweise eine.NET Core-Anwendung, die unter Linux oder Mac ausgeführt wird) und akzeptieren, dass die Authentifizierung an den Systembrowser delegiert wird, können Sie die interaktive Authentifizierung verwenden. .Net Core bietet keinen [Webbrowser](https://aka.ms/msal-net-uses-web-browser), daher erfolgt die Authentifizierung im Systembrowser. Die beste Option in diesem Fall ist alternativ die Verwendung des Gerätecodeflows. Dieser Flow wird auch für Anwendungen ohne Browser verwendet, z. B. für IoT-Anwendungen.

  ![Anwendungen ohne Browser](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Besonderheiten

Desktopanwendungen weisen eine Reihe von Besonderheiten auf. Diese hängen hauptsächlich davon ab, ob Ihre Anwendung die interaktive oder eine andere Authentifizierung verwendet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Desktop-App: App-Registrierung](scenario-desktop-app-registration.md)
