---
title: Blockieren der Legacyauthentifizierung – Azure Active Directory
description: Erfahren Sie, wie Sie durch Blockieren der Legacyauthentifizierung mithilfe des bedingten Azure AD-Zugriffs Ihren Sicherheitsstatus verbessern.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/13/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd66bc742d0832cba5d6f302bfe30c85e2d82716
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253340"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Gewusst wie: Blockieren der Legacyauthentifizierung bei Azure AD mit bedingtem Zugriff   

Um Ihren Benutzern den einfachen Zugriff auf Ihre Cloud-Apps zu ermöglichen, unterstützt Azure Active Directory (Azure AD) eine Vielzahl von Authentifizierungsprotokollen einschließlich der Legacyauthentifizierung. Ältere Protokolle unterstützen jedoch nicht die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). MFA ist in vielen Umgebungen eine allgemeine Anforderung zum Schutz vor Identitätsdiebstahl. 

Alex Weinert, Director of Identity Security bei Microsoft, hebt in seinem Blogbeitrag vom 12. März 2020 mit dem Titel [Neue Tools zum Blockieren der Legacyauthentifizierung in Ihrer Organisation](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#) hervor, warum Organisationen die Legacyauthentifizierung blockieren sollten und welche zusätzlichen Tools Microsoft für diese Aufgabe bereitstellt:

> Damit die mehrstufige Authentifizierung (MFA) wirksam wird, müssen Sie auch die Legacyauthentifizierung blockieren. Legacyauthentifizierungsprotokolle wie POP, SMTP, IMAP und MAPI können nämlich keine MFA erzwingen und sind dadurch bevorzugte Einstiegspunkte für Angreifer, die Ihre Organisation attackieren...
> 
>... Die Zahlen zur Legacyauthentifizierung sind nach einer Analyse des Azure Active Directory (Azure AD)-Datenverkehrs ziemlich krass:
> 
> - Mehr als 99 Prozent der Kennwort-Spray-Angriffe verwenden Legacyauthentifizierungsprotokolle
> - Mehr als 97 Prozent der Angriffe in Bezug auf Anmeldeinformationen verwenden die Legacyauthentifizierung
> - Bei Azure AD-Konten in Organisationen, welche die Legacyauthentifizierung deaktiviert haben, sind 67 Prozent weniger Angriffe festzustellen als bei Organisation mit aktivierter Legacyauthentifizierung
>

Wenn Ihre Umgebung für das Blockieren der Legacyauthentifizierung bereit ist, um den Schutz Ihres Mandanten zu verbessern, können Sie dieses Ziel mit bedingtem Zugriff erreichen. In diesem Artikel wird erläutert, wie Sie die Richtlinien für bedingten Zugriff konfigurieren können, mit denen die Legacyauthentifizierung für Ihren Mandanten blockiert wird.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Folgendem vertraut sind: 

- Den [grundlegenden Konzepten](overview.md) des bedingten Azure AD-Zugriffs 
- Den [Best Practices](best-practices.md) für das Konfigurieren von Richtlinien für bedingten Zugriff im Azure-Portal

## <a name="scenario-description"></a>Beschreibung des Szenarios

Azure AD unterstützt mehrere der am häufigsten verwendeten Protokolle zur Authentifizierung und Autorisierung, einschließlich der Legacyauthentifizierung. Die Legacyauthentifizierung bezieht sich auf Protokolle, die die Standardauthentifizierung verwenden. In der Regel können diese Protokolle keinen Typ der zweistufigen Authentifizierung erzwingen. Beispiele für Apps, die auf der Legacyauthentifizierung basieren, sind:

- Ältere Microsoft Office-Apps
- Apps, die E-Mail-Protokolle wie POP, IMAP und SMTP verwenden

Eine einstufige Authentifizierung (z. B. mit Benutzername und Kennwort) reicht heutzutage nicht mehr aus. Kennwörter sind unzulänglich, weil sie leicht zu erraten sind, und wir (Menschen) sind schlecht darin, gute Kennwörter auszuwählen. Kennwörter sind auch für eine Vielzahl von Angriffen wie Phishing und Kennwort-Spray anfällig. Eine der einfachsten Maßnahmen, die Sie ergreifen können, um sich vor Kennwortsicherheitsverletzungen zu schützen, ist das Implementieren von MFA. Denn selbst wenn ein Angreifer in den Besitz des Kennworts eines Benutzers gelangt, reicht bei Verwendung von MFA das Kennwort allein nicht aus, um sich erfolgreich authentifizieren und auf die Daten zugreifen zu können.

Wie können Sie verhindern, dass Apps mit Legacyauthentifizierung auf Ressourcen Ihres Mandanten zugreifen? Es wird empfohlen, diese Apps einfach mit einer Richtlinie für bedingten Zugriff zu blockieren. Gegebenenfalls können Sie nur bestimmten Benutzern und bestimmten Netzwerkadressen die Verwendung von Apps erlauben, die auf der Legacyauthentifizierung basieren.

Richtlinien für den bedingten Zugriff werden durchgesetzt, nachdem die First-Factor-Authentifizierung abgeschlossen ist. Daher ist der bedingte Zugriff nicht als erste Abwehrmaßnahme für Szenarien wie Denial-of-Service-Angriffe (DoS) gedacht, sondern kann Signale von diesen Ereignissen (z. B. der Risikostufe für die Anmeldung, den Standort der Anforderung usw.) nutzen, um den Zugriff zu bestimmen.

## <a name="implementation"></a>Implementierung

In diesem Abschnitt wird erläutert, wie eine Richtlinie für bedingten Zugriff zum Blockieren der Legacyauthentifizierung konfiguriert wird. 

### <a name="legacy-authentication-protocols"></a>Ältere Authentifizierungsprotokolle

Die folgenden Optionen gelten als ältere Authentifizierungsprotokolle.

- Authentifiziertes SMTP: wird von POP- und IMAP-Clients zum Senden von E-Mails verwendet
- AutoErmittlung: wird von Outlook und EAS-Clients verwendet, um Postfächer in Exchange Online zu suchen und diese zu verbinden
- Exchange ActiveSync (EAS): wird zum Herstellen einer Verbindung mit Postfächern in Exchange Online verwendet
- Exchange Online PowerShell: wird zum Herstellen einer Verbindung mit Exchange Online über Remote-PowerShell verwendet Wenn Sie die Standardauthentifizierung für Exchange Online PowerShell blockieren, müssen Sie das Exchange Online PowerShell-Modul verwenden, um eine Verbindung herzustellen. Anweisungen finden Sie unter [Herstellen einer Verbindung mit Exchange Online PowerShell mithilfe der mehrstufigen Authentifizierung](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
- Exchange Web Services (EWS): eine Programmierschnittstelle, die von Outlook, Outlook für Mac und Drittanbieter-Apps verwendet wird
- IMAP4: wird von IMAP-E-Mail-Clients verwendet
- MAPI über HTTP (MAPI/HTTP): wird von Outlook 2010 und höher verwendet
- Offlineadressbuch (OAB): eine Kopie der Adressenlistensammlungen, die von Outlook heruntergeladen und verwendet werden
- Outlook Anywhere (RPC über HTTP): wird von Outlook 2016 und früher verwendet
- Outlook-Dienst: wird von der Mail- und Kalender-App für Windows 10 verwendet
- POP3: wird von POP-E-Mail-Clients verwendet
- Reporting Web Services: wird zum Abrufen von Berichtsdaten in Exchange Online verwendet
- Andere Clients: andere Protokolle, bei denen die Verwendung älterer Authentifizierungsmethoden identifiziert wird

Weitere Informationen zu diesen Authentifizierungsprotokollen und -diensten finden Sie unter[Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities).

### <a name="identify-legacy-authentication-use"></a>Identifizieren der Verwendung der Legacyauthentifizierung

Bevor Sie die Legacyauthentifizierung in Ihrem Verzeichnis blockieren können, müssen Sie zuerst wissen, ob Ihre Benutzer über Apps verfügen, die die Legacyauthentifizierung verwenden, und wie sich dies auf Ihr gesamtes Verzeichnis auswirkt. Sie können Azure AD-Anmeldungsprotokolle verwenden, um herauszufinden, ob Sie die Legacyauthentifizierung verwenden.

1. Navigieren Sie zu **Azure-Portal** > **Azure Active Directory** > **Anmeldungen**.
1. Falls die Spalte „Client-App“ nicht angezeigt wird, fügen Sie sie durch Klicken auf **Spalten** > **Client-App** hinzu.
1. **Filter hinzufügen** > **Client-App** > wählen Sie alle älteren Authentifizierungsprotokolle aus, und klicken Sie auf **Anwenden**.

Durch das Filtern werden Ihnen nur Anmeldeversuche von Legacyauthentifizierungsprotokollen angezeigt. Bei Klicken auf jeden einzelnen Anmeldeversuch werden Ihnen weitere Details angezeigt. Das **Client-App**-Feld auf der Registerkarte **Grundlegende Informationen** gibt an, welche Legacyauthentifizierungsprotokolle verwendet wurden.

Diese Protokolle geben an, welche Benutzer weiterhin von der Legacyauthentifizierung abhängig sind, und welche Anwendungen ältere Protokolle für Authentifizierungsanforderungen verwenden. Implementieren Sie für Benutzer, die in diesen Protokollen nicht aufgeführt sind und nachweislich keine Legacyauthentifizierung verwenden, eine Richtlinie für bedingten Zugriff, die nur für diese Benutzer vorgesehen ist.

### <a name="block-legacy-authentication"></a>Blockieren älterer Authentifizierungsmethoden 

In einer Richtlinie für bedingten Zugriff können Sie eine Bedingung festlegen, die an die Client-Apps gebunden ist, die für den Zugriff auf Ihre Ressourcen verwendet werden. Mit der Client-App-Bedingung können Sie den Bereich für Apps mit Legacyauthentifizierung eingrenzen, indem Sie für **Mobile Apps und Desktopclients** die Optionen **Exchange ActiveSync-Clients** und **Andere Clients** auswählen.

![Andere Clients](./media/block-legacy-authentication/01.png)

Um den Zugriff für diese Apps zu blockieren, müssen Sie **Zugriff blockieren** auswählen.

![Zugriff blockieren](./media/block-legacy-authentication/02.png)

### <a name="select-users-and-cloud-apps"></a>Auswählen von Benutzern und Cloud-Apps

Wenn Sie die Legacyauthentifizierung für Ihre Organisation blockieren möchten, denken Sie wahrscheinlich, dass Sie dies durch Auswählen der folgenden Optionen erreichen können:

- Alle Benutzer
- Alle Cloud-Apps
- Zugriff blockieren

![Zuweisungen](./media/block-legacy-authentication/03.png)

Azure verfügt über eine Sicherheitsfunktion, die Sie am Erstellen einer solchen Richtlinie hindert, weil diese Konfiguration gegen die [Best Practices](best-practices.md) für Richtlinien für bedingten Zugriff verstößt.
 
![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/04.png)

Diese Sicherheitsfunktion ist erforderlich, weil das *Blockieren aller Benutzer und aller Cloud-Apps* potenziell dazu führen kann, für Ihre gesamte Organisation die Anmeldung bei Ihrem Mandanten zu blockieren. Sie müssen mindestens einen Benutzer davon ausschließen, um die Mindestanforderung der Best Practices zu erfüllen. Sie können auch eine Verzeichnisrolle ausschließen.

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/05.png)

Sie können diese Sicherheitsfunktion erfüllen, indem Sie einen Benutzer aus der Richtlinie ausschließen. Im Idealfall sollten Sie einige [Administratorkonten für den Notfallzugriff in Azure AD](../users-groups-roles/directory-emergency-access.md) definieren und diese aus der Richtlinie ausschließen.

Wenn Sie beim Aktivieren der Richtlinie zum Blockieren der Legacyauthentifizierung den [Modus „Nur Bericht“](concept-conditional-access-report-only.md) verwenden, hat Ihre Organisation die Möglichkeit, die Auswirkungen der Richtlinie zu überwachen.

## <a name="policy-deployment"></a>Richtlinienbereitstellung

Kümmern Sie sich vor dem Einsatz der Richtlinie in der Produktionsumgebung um Folgendes:
 
- **Dienstkonten**: Identifizieren Sie Benutzerkonten, die als Dienstkonten verwendet werden oder nach Geräten, wie Telefone im Konferenzraum. Stellen Sie sicher, dass diese Konten über sichere Kennwörter verfügen, und fügen Sie sie einer ausgeschlossenen Gruppe hinzu.
- **Anmeldeberichte**: Überprüfen Sie den Anmeldebericht, und suchen Sie nach dem Datenverkehr **anderer Clients**. Identifizieren Sie die höchste Nutzung, und untersuchen Sie den Grund für die Verwendung. In der Regel wird der Datenverkehr von älteren Office-Clients generiert, die keine moderne Authentifizierung oder einige E-Mail-Apps von Drittanbietern verwenden. Erarbeiten Sie einen Plan, um die Nutzung dieser Apps zu reduzieren, oder wenn die Auswirkungen gering sind, teilen Sie Ihren Benutzern mit, dass sie diese Apps nicht mehr verwenden können.
 
Weitere Informationen finden Sie unter [Wie stellen Sie eine neue Richtlinie bereit?](best-practices.md#how-should-you-deploy-a-new-policy)

## <a name="what-you-should-know"></a>Wichtige Informationen

Das Blockieren des Zugriffs mithilfe der Bedingung **Andere Clients** blockiert auch Exchange Online PowerShell und Dynamics 365 mit Standardauthentifizierung.

Durch das Konfigurieren einer Richtlinie für **Andere Clients** wird die gesamte Organisation für bestimmte Clients blockiert, z.B. SPConnect. Dies tritt ein, weil sich ältere Clients auf unerwartete Weise authentifizieren. Dieses Problem gilt nicht für Office-Hauptanwendungen wie ältere Office-Clients.

Es kann bis zu 24 Stunden dauern, bis die Richtlinie wirksam wird.

Sie können alle verfügbaren Gewährungssteuerelemente für die Bedingung **Andere Clients** auswählen. Die Endbenutzererfahrung ist jedoch immer die gleiche: Der Zugriff ist blockiert.

Wenn Sie die Legacyauthentifizierung mit der Bedingung **Andere Clients** blockieren, können Sie auch die Geräteplattform und den Speicherort als Bedingung festlegen. Wenn Sie nur ältere Authentifizierungen für mobile Geräte blockieren möchten, legen Sie z.B. die Bedingung **Geräteplattformen** fest, indem Sie Folgendes auswählen:

- Android
- iOS
- Windows Phone

![Die Richtlinienkonfiguration wird nicht unterstützt.](./media/block-legacy-authentication/06.png)

## <a name="next-steps"></a>Nächste Schritte

- [Bestimmen der Auswirkung durch Verwendung des reinen Berichtsmodus des bedingten Zugriffs](howto-conditional-access-report-only.md)
- Wenn Sie noch nicht mit dem Konfigurieren von Richtlinien für bedingten Zugriff vertraut sind, sehen Sie sich das Beispiel unter [Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md) an.
- Weitere Informationen zur Unterstützung der modernen Authentifizierung finden Sie unter [So funktioniert die moderne Authentifizierung für Office 2013- und Office 2016-Client-Apps](/office365/enterprise/modern-auth-for-office-2013-and-2016). 
- [Einrichten eines Multifunktionsgeräts oder einer -anwendung zum Senden von E-Mails mit Office 365 und Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3)
