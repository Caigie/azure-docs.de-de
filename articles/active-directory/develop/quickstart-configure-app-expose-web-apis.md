---
title: Konfigurieren einer App, um Web-APIs verfügbar zu machen – Microsoft Identity Platform | Azure
description: Es wird beschrieben, wie Sie eine Anwendung so konfigurieren, dass eine neue Berechtigung bzw. ein Bereich und eine Rolle bereitgestellt werden, um die Anwendung für Clientanwendungen verfügbar zu machen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 08/14/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 263eb531466e26ed6069dc889c17e2632aa9ed20
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87799411"
---
# <a name="quickstart-configure-an-application-to-expose-web-apis"></a>Schnellstart: Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs

Sie können eine Web-API entwickeln und sie Clientanwendungen zur Verfügung stellen, indem Sie [Berechtigungen/Bereiche](developer-glossary.md#scopes) und [Rollen](developer-glossary.md#roles) verfügbar machen. Eine ordnungsgemäß konfigurierte Web-API wird auf die gleiche Weise wie andere Microsoft-Web-APIs (einschließlich der Graph-API und der Office 365-APIs) zur Verfügung gestellt.

In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Anwendung so konfigurieren, dass ein neuer Bereich bereitgestellt wird, um diesen für Clientanwendungen verfügbar zu machen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Sie sind über die Unterstützung von [Berechtigungen und Zustimmung](v2-permissions-and-consent.md) informiert. Hiermit sollten Sie vertraut sein, wenn Sie Anwendungen erstellen, die von anderen Benutzern oder Anwendungen verwendet werden müssen.
* Sie verfügen über einen Mandanten, unter dem Anwendungen registriert wurden.
  * Wenn Sie keine Apps registriert haben, sollten Sie sich darüber informieren, [wie Sie Anwendungen bei der Microsoft Identity Platform registrieren](quickstart-register-app.md).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Anmelden beim Azure-Portal und Auswählen der App

Sie müssen die folgenden Schritte ausführen, bevor Sie die App konfigurieren können:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie mit Ihrem Konto auf mehrere Mandanten zugreifen können, klicken Sie rechts oben auf Ihr Konto, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** und anschließend **App-Registrierungen** aus.
1. Wählen Sie die Anwendung aus, die Sie konfigurieren möchten. Nachdem Sie die App ausgewählt haben, wird die anwendungsspezifische Seite **Übersicht** oder die Hauptseite für die Registrierung angezeigt.
1. Wählen Sie aus, welche Methode Sie verwenden möchten (Benutzeroberfläche oder Anwendungsmanifest), um einen neuen Bereich verfügbar zu machen:
    * [Verfügbarmachen eines neuen Bereichs über die Benutzeroberfläche](#expose-a-new-scope-through-the-ui)
    * [Verfügbarmachen eines neuen Bereichs oder einer Rolle über das Anwendungsmanifest](#expose-a-new-scope-or-role-through-the-application-manifest)

## <a name="expose-a-new-scope-through-the-ui"></a>Verfügbarmachen eines neuen Bereichs über die Benutzeroberfläche

[![Zeigt, wie eine API über die Benutzeroberfläche verfügbar gemacht wird](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png#lightbox)

Machen Sie einen neuen Bereich wie folgt über die Benutzeroberfläche verfügbar:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Eine API verfügbar machen**.

1. Wählen Sie **Bereich hinzufügen**.

1. Wenn Sie keinen **Anwendungs-ID-URI** festgelegt haben, wird eine entsprechende Aufforderung angezeigt. Geben Sie Ihren Anwendungs-ID-URI ein, oder verwenden Sie den bereits vorhandenen URI, und wählen Sie dann **Speichern und fortfahren**.

1. Geben Sie die Informationen zu Ihrem Bereich ein, wenn die Seite **Bereich hinzufügen** angezeigt wird:

    | Feld | BESCHREIBUNG |
    |-------|-------------|
    | **Bereichsname** | Geben Sie einen aussagekräftigen Namen für Ihren Bereich ein.<br><br>Beispiel: `Employees.Read.All`. |
    | **Zum Einwilligen berechtigte Personen** | Wählen Sie aus, ob für diesen Bereich die Einwilligung von Benutzern ausreicht oder ob die Einwilligung eines Administrators erforderlich ist. Wählen Sie für umfassendere Berechtigungen die Option **Nur Administratoren**. |
    | **Anzeigename der Administratoreinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die für Administratoren angezeigt wird.<br><br>Zum Beispiel, `Read-only access to Employee records` |
    | **Beschreibung der Administratoreinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die für Administratoren angezeigt wird.<br><br>Zum Beispiel, `Allow the application to have read-only access to all Employee data.` |

    Geben Sie auch Werte für die folgenden Felder ein, wenn auch Benutzer die Einwilligung für Ihren Bereich erteilen können:

    | Feld | BESCHREIBUNG |
    |-------|-------------|
    | **Anzeigename der Benutzereinwilligung** | Geben Sie einen aussagekräftigen Namen für Ihren Bereich ein, der Benutzern angezeigt wird.<br><br>Zum Beispiel, `Read-only access to your Employee records` |
    | **Beschreibung der Benutzereinwilligung** | Geben Sie eine aussagekräftige Beschreibung für Ihren Bereich ein, die Benutzern angezeigt wird.<br><br>Zum Beispiel, `Allow the application to have read-only access to your Employee data.` |

1. Legen Sie den **Zustand** fest, und wählen Sie **Bereich hinzufügen**, wenn Sie fertig sind.

1. (Optional) Soll App-Benutzern keine Einwilligungsaufforderung für die von Ihnen festgelegten Bereiche angezeigt werden, können Sie die Clientanwendung vorab für den Zugriff auf Ihre Web-API autorisieren. Sie sollten *nur* die Clientanwendungen vorab autorisieren, denen Sie vertrauen, da Ihre Benutzer keine Möglichkeit haben, die Einwilligung abzulehnen.
    1. Wählen Sie unter **Autorisierte Clientanwendungen** die Option **Eine Clientanwendung hinzufügen** aus.
    1. Geben Sie den Wert für **Anwendungs-ID (Client)** der Clientanwendung ein, die Sie vorab autorisieren möchten, beispielsweise den einer zuvor registrierten Webanwendung.
    1. Wählen Sie unter **Autorisierte Bereiche**die Bereiche aus, für die Sie die Einwilligungsaufforderung unterdrücken möchten, und wählen Sie dann **Anwendung hinzufügen** aus.

    Die Client-App ist jetzt eine vorab autorisierte Client-App, und Benutzer werden bei der Anmeldung nicht zur Einwilligung aufgefordert.

1. Führen Sie die Schritte aus, mit denen Sie [Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde](#verify-the-web-api-is-exposed-to-other-applications).

## <a name="expose-a-new-scope-or-role-through-the-application-manifest"></a>Verfügbarmachen eines neuen Bereichs oder einer Rolle über das Anwendungsmanifest

[![Verfügbarmachen eines neuen Bereichs mit der oauth2Permissions-Sammlung im Manifest](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png#lightbox)

Machen Sie einen neuen Bereich oder einer Rolle wie folgt über das Anwendungsmanifest verfügbar:

1. Wählen Sie auf der Seite **Übersicht** der App den Abschnitt **Manifest**. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest im Portal bearbeiten können (**Bearbeiten**). Optional können Sie **Herunterladen** wählen und das Manifest lokal bearbeiten und dann **Hochladen** verwenden, um es wieder auf Ihre Anwendung anzuwenden.

    Im folgenden Beispiel wird veranschaulicht, wie Sie den neuen Bereich `Employees.Read.All` für die Ressource/API verfügbar machen können, indem Sie der Sammlung `oauth2Permissions` das folgende JSON-Element hinzufügen.

      ```json
      {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
      }
      ```

   > [!NOTE]
   > Der Wert `id` muss programmgesteuert oder mit einem Tool zum Generieren von GUIDs, z.B. [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx), generiert werden. Die `id` stellt einen eindeutigen Bezeichner für den Bereich dar, der von der Web-API bereitgestellt wird. Nachdem ein Client richtig mit Berechtigungen zum Zugreifen auf Ihre Web-API konfiguriert wurde, wird dafür von Azure AD ein OAuth 2.0-Zugriffstoken ausgestellt. Wenn der Client die Web-API aufruft, wird das Zugriffstoken präsentiert, für das der Bereichsanspruch (scp) auf die Berechtigungen festgelegt ist, die in der dazugehörigen Anwendungsregistrierung angefordert werden.
   >
   > Sie können später bei Bedarf zusätzliche Bereiche verfügbar machen. Berücksichtigen Sie, dass Ihre Web-API verschiedene Berechtigungen verfügbar machen kann, die einer Vielzahl von unterschiedlichen Funktionen zugeordnet sind. Ihre Ressource kann den Zugriff auf die Web-API zur Laufzeit steuern, indem die Bereichsansprüche (`scp`) im erhaltenen OAuth 2.0-Zugriffstoken ausgewertet werden.

1. Klicken Sie abschließend auf **Speichern**. Ihre Web-API ist jetzt so konfiguriert, dass sie von anderen Anwendungen im Verzeichnis verwendet werden kann.
1. Führen Sie die Schritte aus, mit denen Sie [Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde](#verify-the-web-api-is-exposed-to-other-applications).

## <a name="verify-the-web-api-is-exposed-to-other-applications"></a>Überprüfen, ob die Web-API für andere Anwendungen verfügbar gemacht wurde

1. Wechseln Sie zurück zu Ihrem Azure AD-Mandanten, wählen Sie **App-Registrierungen** aus, und suchen Sie nach der Clientanwendung, die Sie konfigurieren möchten.
1. Wiederholen Sie die Schritte unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md).
1. Wenn Sie zum Schritt [API auswählen](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis) gelangen, wählen Sie Ihre Ressource (die Web-API-App-Registrierung) aus.
    * Wenn Sie die Web-API-App-Registrierung mithilfe des Azure-Portal erstellt haben, wird Ihre API-Ressource auf der Registerkarte **Meine APIs** aufgelistet.
    * Wenn Sie Visual Studio die Erstellung der Web-API-App-Registrierung während der Projekterstellung erlaubt haben, wird Ihre API-Ressource auf der Registerkarte **Von meiner Organisation verwendete APIs** aufgeführt.

Nach der Auswahl der Web-API-Ressource sollte der neue Bereich angezeigt werden, der für Clientberechtigungsanforderungen verfügbar ist.

## <a name="more-on-the-application-manifest"></a>Weitere Informationen zum Anwendungsmanifest

Das Anwendungsmanifest dient als Mechanismus zum Aktualisieren der Anwendungsentität, die alle Attribute der Identitätskonfiguration einer Azure AD-Anwendung definiert. Weitere Informationen zur Anwendungsentität und zum dazugehörigen Schema finden Sie in der [Dokumentation zur Anwendungsentität der Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). Der Artikel enthält umfassende Referenzinformationen zu den Anwendungsentitätselementen, die verwendet werden, um Berechtigungen für Ihre API anzugeben, z.B.:

* Das appRoles-Mitglied, bei es sich um eine Sammlung von [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type)-Entitäten handelt, die zum Definieren der [Anwendungsberechtigungen](developer-glossary.md#permissions) für eine Web-API verwendet werden.
* Das oauth2Permissions-Mitglied, bei dem es sich um eine Sammlung von [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type)-Entitäten handelt, die zum Definieren der [delegierten Berechtigungen](developer-glossary.md#permissions) für eine Web-API verwendet werden.

Weitere Informationen zu Anwendungsmanifestkonzepten im Allgemeinen finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](reference-app-manifest.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die anderen Schnellstartanleitungen zur App-Verwaltung an:

* [Registrieren einer Anwendung bei der Microsoft Identity Platform](quickstart-register-app.md)
* [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md)
* [Ändern der von einer Anwendung unterstützten Konten](quickstart-modify-supported-accounts.md)
* [Entfernen einer bei der Microsoft Identity Platform registrierten Anwendung](quickstart-remove-app.md)

Um mehr über die beiden Azure AD-Objekte, die eine registrierte Anwendung darstellen, und die Beziehung zwischen ihnen zu erfahren, lesen Sie [Anwendungsobjekte und Dienstprinzipalobjekte](app-objects-and-service-principals.md).

Weitere Informationen zu den Brandingrichtlinien, die Sie bei der Entwicklung von Anwendungen mit Azure Active Directory verwenden sollten, finden Sie unter [Brandingrichtlinien für Anwendungen](howto-add-branding-in-azure-ad-apps.md).
