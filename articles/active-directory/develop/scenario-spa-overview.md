---
title: JavaScript-Szenario mit Single-Page-App – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Single-Page-Webanwendung mithilfe der Microsoft Identity Platform erstellen (Szenarioübersicht).
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 3ead0ea58c6860519f027eb6a7450df37396bd89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80885173"
---
# <a name="scenario-single-page-application"></a>Szenario: Einseitige Anwendung

Erfahren Sie, wie Sie eine Single-Page-Webanwendung (SPA) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Erste Schritte

Sie können Ihre erste Anwendung erstellen, indem Sie die Schritte des JavaScript SPA-Schnellstarts ausführen:

> [!div class="nextstepaction"]
> [Schnellstart: Single-Page-Webanwendung](./quickstart-v2-javascript.md)

## <a name="overview"></a>Übersicht

Viele moderne Webanwendungen werden als clientseitige Single-Page-Webanwendungen (SPAs) erstellt. Entwickler schreiben diese mithilfe von JavaScript oder einem SPA-Framework wie Angular, Vue.js oder React.js. Diese Anwendungen werden in einem Webbrowser ausgeführt und weisen andere Authentifizierungsmerkmale als herkömmliche serverseitige Webanwendungen auf. 

Microsoft Identity Platform ermöglicht Single-Page-Webanwendungen mithilfe des [impliziten OAuth 2.0-Flusses](./v2-oauth2-implicit-grant-flow.md) das Anmelden von Benutzern und Abrufen von Token für den Zugriff auf Back-End-Dienste oder Web-APIs. Durch den impliziten Flow kann die Anwendung ID-Token abrufen, um den authentifizierten Benutzer darzustellen, sowie Zugriffstoken, die zum Aufrufen geschützter APIs erforderlich sind.

![Single-Page-Webanwendungen](./media/scenarios/spa-app.svg)

Dieser Authentifizierungsfluss umfasst keine Anwendungsszenarios, die plattformübergreifende JavaScript-Frameworks verwenden, z. B. Electron und React-Native. Sie erfordern weitere Funktionen für die Interaktion mit den nativen Plattformen.

## <a name="specifics"></a>Besonderheiten

Zum Aktivieren dieses Szenarios für Anwendung benötigen Sie Folgendes:

* Eine Anwendungsregistrierung bei Azure Active Directory (Azure AD). Diese Registrierung umfasst das Aktivieren des impliziten Flusses und das Festlegen eines Umleitungs-URI, an den Token zurückgegeben werden.
* Sie benötigen eine Anwendungskonfiguration mit den registrierten Anwendungseigenschaften, wie z. B. der Anwendungs-ID.
* Die müssen die Microsoft-Authentifizierungsbibliothek (MSAL) für den Authentifizierungsfluss zum Anmelden und Abrufen von Token verwenden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-spa-app-registration.md)
