---
title: 'Leitfaden zur Veröffentlichung von SaaS-Anwendungen: Kommerzieller Microsoft-Marketplace'
description: Hier werden die Anforderungen und Ressourcen für die Veröffentlichung von SaaS-Anwendungsangeboten in Microsoft AppSource und Azure Marketplace vorgestellt.
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: dsindona
ms.openlocfilehash: 4d1ee4fc0760e76af7475dd3b2dc83f306e7a7bd
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657827"
---
# <a name="saas-applications-offer-publishing-guide"></a>SaaS-Anwendungen: Leitfaden für die Veröffentlichung von Angeboten

Sie können SaaS-Anwendungen im kommerziellen Marketplace mit drei verschiedenen Aktionsaufrufen veröffentlichen: „Kontakt mit mir aufnehmen“, „Jetzt testen“ und „Jetzt kaufen“. Dieser Artikel erläutert diese drei Optionen einschließlich der jeweiligen Anforderungen. 

## <a name="offer-overview"></a>Angebotsübersicht  

SaaS-Anwendungen sind in Microsoft AppSource und Azure Marketplace verfügbar.  Beide Storefronts unterstützen das Auflisten von Angeboten, Testversionen und Transaktionsangebote.

**Liste**:  Diese Veröffentlichungsoption besteht aus dem Angebotstyp „Kontakt mit mir aufnehmen“ und wird verwendet, wenn eine Teilnahme auf Testversions- oder Transaktionsebene nicht möglich ist. Dieser Ansatz hat den Vorteil, dass Herausgeber, die eine Lösung auf dem Markt anbieten, sofort Leads gewinnen können, die zu Abschlüssen führen und zum Wachstum Ihres Geschäfts beitragen.  
**Testversion/Transaktion**:  Der Kunde hat die Möglichkeit, Ihre Lösung direkt zu kaufen oder eine Testversion für Ihre Lösung anzufordern. Die Bereitstellung einer Testversion führt zu einer stärkeren Einbindung von Kunden und ermöglicht es ihnen, Ihre Lösung vor dem Kauf in Augenschein zu nehmen. Eine Testversion steigert Ihre Verkaufschancen in den Storefronts, und die Interaktion mit den Kunden führt in der Regel zu weiteren und lukrativeren Leads. Testversionen müssen mindestens für die Dauer des Testzeitraums kostenlosen Support enthalten.  

| SaaS-Apps-Angebot | Geschäftliche Anforderungen | Technische Anforderungen |  
| --- | --- | --- |  
| **Kontaktaufnahme** | Ja | Nein |  
| **Power BI/Dynamics** | Ja | Ja (Azure AD-Integration) |  
| **SaaS-Apps**| Ja | Ja (Azure AD-Integration) |     

## <a name="saas-list"></a>SaaS-Liste

Der Aktionsaufruf für eine SaaS-Auflistung ohne Testversion und Abrechnungsfunktionalität ist „Kontakt mit mir aufnehmen“. 

Zum Auflisten einer SaaS-Anwendung müssen Sie nicht Azure Active Directory konfigurieren. 

|Requirements (Anforderungen)  |Details  |
|---------|---------|
|Ihre App ist ein SaaS-Angebot  |   Ihre Lösung ist ein SaaS-Angebot, und Sie bieten ein mehrinstanzenfähiges SaaS-Produkt an.      |


## <a name="saas-trial"></a>SaaS-Testversion

Sie stellen eine Lösung oder App mithilfe einer kostenlos zu testenden, SaaS-basierten Testversion bereit. Kostenlose Testangebote können als Testkonten mit eingeschränkter Nutzung oder begrenzter Dauer angeboten werden. 


|Requirements (Anforderungen)  |Details  |
|---------|---------|
|Ihre App ist ein SaaS-Angebot  |   Ihre Lösung ist ein SaaS-Angebot, und Sie bieten ein mehrinstanzenfähiges SaaS-Produkt an.      |
|Ihre App ist AAD-tauglich     |   Der Kunde wird zu Ihrer Domäne umgeleitet, und Sie führen direkt mit dem Kunden eine Transaktion durch.       |


## <a name="saas-trial-technical-requirements"></a>Technische Anforderungen für die SaaS-Testversion

Die technischen Voraussetzungen für SaaS-Anwendungen sind einfach. Herausgeber müssen nur für die Integration in Azure Active Directory (Azure AD) sorgen, um eine Veröffentlichung zu ermöglichen. Die Integration von Anwendungen in Azure AD ist umfassend dokumentiert, und Microsoft bietet verschiedene SDKs und Ressourcen für diesen Zweck.  

Als Erstes empfiehlt es sich, ein Abonnement speziell für die Veröffentlichung im Azure Marketplace einzurichten, sodass Sie diese Arbeit von anderen Initiativen trennen können. Danach können Sie Ihre SaaS-Anwendung in diesem Abonnement bereitstellen und mit der Entwicklungsarbeit beginnen.  

Die beste Azure Active Directory-Dokumentation sowie entsprechende Beispiele und Anleitungen finden Sie auf den folgenden Websites: 

* [Entwicklerhandbuch zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

* [Integration in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

* [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

* [Azure Roadmap – Sicherheit und Identität](https://azure.microsoft.com/roadmap/?category=security-identity)

Videotutorials finden Sie hier:

* [Azure Active Directory Authentication with Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration) (Authentifizierung über Azure Active Directory mit Vittorio Bertocci)

* [Azure Active Directory Identity Technical Briefing - Part 1 of 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 1 von 2)

* [Azure Active Directory Identity Technical Briefing - Part 2 of 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration) (Technische Anleitung zu Azure Active Directory-Identitäten – Teil 2 von 2)

* [Building Apps with Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration) (Erstellen von Apps mit Microsoft Azure Active Directory)

* [Verschiedene Microsoft Azure-Videos rund um Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

Eine kostenlose Azure Active Directory-Schulung finden Sie hier:  
* [Themenreihe über Microsoft Azure für IT-Experten: Azure Active Directory](https://mva.microsoft.com/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

Darüber hinaus bietet Azure Active Directory eine Website für die Suche nach Dienstupdates:   
* [Azure AD-Dienstupdates](https://azure.microsoft.com/updates/?product=active-directory)

## <a name="using-azure-active-directory-to-enable-trials"></a>Verwenden von Azure Active Directory, um Tests zu ermöglichen  

Microsoft authentifiziert alle Marketplace-Benutzer mit Azure AD. Wenn sich also ein authentifizierter Benutzer in Marketplace durch Ihr Testversionsangebot klickt und zu Ihrer Testumgebung umgeleitet wird, können Sie dem Benutzer ohne zusätzlichen Anmeldeschritt direkt eine Testversion bereitstellen. Das Token, das Ihre App im Rahmen der Authentifizierung von Azure AD erhält, enthält wichtige Benutzerinformationen, mit denen Sie ein Benutzerkonto in Ihrer App erstellen können, um die Bereitstellung zu automatisieren und die Wahrscheinlichkeit eines Abschlusses zu erhöhen. Weitere Informationen zu dem Token finden Sie in der [Azure AD-Tokenreferenz](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

Die Verwendung der durch Azure AD ermöglichten Ein-Klick-Authentifizierung bei Ihrer App oder Testversion hat folgende Vorteile:  
* Der Prozess zwischen Marketplace und Testumgebung wird optimiert.  
* Das Gefühl einer „produktinternen Erfahrung“ bleibt selbst dann erhalten, wenn der Benutzer vom Marketplace zu Ihrer Domäne bzw. Testumgebung umgeleitet wird.  
* Die Wahrscheinlichkeit eines Abbruchs bei der Umleitung sinkt, da keine zusätzliche Anmeldung erforderlich ist.  
* Weniger Bereitstellungshindernisse für den Großteil der Azure AD-Benutzer.  

## <a name="certifying-your-azure-ad-integration-for-marketplace"></a>Zertifizieren Ihrer Azure AD-Integration für Marketplace  

Führen Sie die Zertifizierung der Azure AD-Integration auf verschiedene Arten durch – je nachdem, ob es sich um eine Anwendung mit nur einem Mandanten oder um eine mehrinstanzenfähige Anwendung handelt und ob Sie bereits die einmalige Verbundanmeldung von Azure AD unterstützen.  

**Vorgehensweise für mehrinstanzenfähige Anwendungen:**  

Wenn Sie Azure AD bereits unterstützt, gehen Sie wie folgt vor:
1.    Registrieren Sie Ihre Anwendung im Azure-Portal
2.    Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in Azure AD, um eine Testversion mit Ein-Klick-Feature zu erhalten. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Wenn die einmalige Azure AD-Verbundanmeldung für Sie neu ist, gehen Sie wie folgt vor: 
1.  Registrieren Sie Ihre Anwendung im Azure-Portal
2.  Entwickeln Sie SSO mit Azure AD mithilfe von [OpenID Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) oder [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
3.  Aktivieren Sie das Feature für die Unterstützung der Mehrinstanzenfähigkeit in AAD, um eine Testversion mit Ein-Klick-Feature zu erhalten. Ausführlichere Informationen finden Sie [hier](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).  

**Optionen für Anwendungen mit nur einem Mandanten:**  
* Fügen Sie Ihrem Verzeichnis mithilfe von [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) Benutzer als Gastbenutzer hinzu.
* Stellen Sie Testversionen für Kunden unter Verwendung von „Contact Me“ (Kontaktaufnahme) bereit.
* Entwickeln Sie eine kundenspezifische Testversion.
* Erstellen einer mehrinstanzenfähigen Beispiel-Demo-App mit SSO

## <a name="saas-subscriptions"></a>SaaS-Abonnements

Verwenden Sie SaaS-App-Angebotstypen, um Ihren Kunden die Möglichkeit zu geben, Ihre SaaS-basierte, technische Lösung als Abonnement zu kaufen. Für Ihre SaaS-App müssen die folgenden Anforderungen erfüllt werden:
- Preis und Abrechnung des Diensts zu einem Festpreis (monatlich oder jährlich) oder mit einer benutzerbasierten Rate.
- Bereitstellung einer Upgrademethode oder Stornierung des Dienst zu einem beliebigen Zeitpunkt.
Microsoft hostet die Commerce-Transaktion. Microsoft stellt Ihrem Kunden Rechnungen in Ihrem Namen. Wenn Sie eine SaaS-App als Abonnement anbieten möchten, müssen Sie diese in die SaaS-Fulfillment-APIs integrieren.  Ihr Dienst muss die Bereitstellung, das Upgrade und das Stornieren unterstützen.

| Anforderung | Details |  
|:--- |:--- |  
|Abrechnung und Messung | Der Preis für Ihr Angebot basiert auf dem Preismodell, das Sie vor der Veröffentlichung ausgewählt haben (Pauschale oder benutzerbasiert).  Wenn Sie ein Modell mit Festpreis nutzen, können Sie auch optionale Dimensionen ergänzen, die dem Kunden für die Nutzung von Diensten berechnet werden, die nicht in der Flatrate enthalten sind. |  
|Abbruch | Ihr Angebot kann jederzeit vom Kunden storniert werden. |  
|Transaktionszielseite | Sie hosten eine Transaktion-Landing Page mit Azure-Co-Branding, auf der Benutzer ihr SaaS-Dienstkonto erstellen und verwalten können. |   
| Abonnement-API | Sie machen einen Dienst verfügbar, der mit dem SaaS-Abonnement interagiert, um Benutzerkonten und Servicepläne erstellen, aktualisieren und löschen zu können. Wichtige API-Änderungen müssen innerhalb von 24 Stunden unterstützt werden. Weniger wichtige API-Änderungen werden regelmäßig veröffentlicht. |  

>[!Note]
>Die Nutzung des CSP-Partnerkanals (Cloud Solution Provider) ist jetzt verfügbar.  Unter [Cloud Solution Providers](./cloud-solution-providers.md) finden Sie weitere Informationen zum Vermarkten Ihres Angebots über die Microsoft CSP-Partnerkanäle.

## <a name="next-steps"></a>Nächste Schritte
Falls Sie dies noch nicht getan haben,

* [Erfahren Sie mehr](https://azuremarketplace.microsoft.com/sell) über den Marketplace.

Um sich in Partner Center zu registrieren, ein neues Angebot zu erstellen oder an einem vorhandenen zu arbeiten, gehen Sie wie folgt vor:

* [Melden Sie sich bei Partner Center](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) an, um Ihr Angebot zu erstellen oder abzuschließen.
* Weitere Informationen finden Sie unter [Erstellen eines SaaS-Anwendungsangebots](./partner-center-portal/create-new-saas-offer.md).
