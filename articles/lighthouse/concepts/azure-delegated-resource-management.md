---
title: Delegierte Azure-Ressourcenverwaltung
description: Die delegierte Azure-Ressourcenverwaltung ist ein wichtiger Bestandteil von Azure Lighthouse, der Dienstanbietern eine agile und präzise Verwaltung von delegierten Ressourcen in großem Umfang ermöglicht.
ms.date: 05/28/2020
ms.topic: conceptual
ms.openlocfilehash: bbe3c28cdcd252755b8350eaa5d2e72044981174
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86120890"
---
# <a name="azure-delegated-resource-management"></a>Delegierte Azure-Ressourcenverwaltung

Die delegierte Azure-Ressourcenverwaltung ist eine der Hauptkomponenten von [Azure Lighthouse](../overview.md). Mit der delegierten Azure-Ressourcenverwaltung können Dienstanbieter die Kundenengagement- und Onboardingerfahrung vereinfachen und gleichzeitig delegierte Ressourcen bedarfsorientiert mit Agilität und Genauigkeit verwalten.

## <a name="what-is-azure-delegated-resource-management"></a>Was ist delegierte Azure-Ressourcenverwaltung?

Die delegierte Azure-Ressourcenverwaltung ermöglicht die logische Projektion von Ressourcen von einem Mandanten auf einen anderen Mandanten. Hierdurch können autorisierte Benutzer in einem Azure Active Directory (Azure AD)-Mandanten Verwaltungsvorgänge über verschiedene Azure AD-Mandanten hinweg durchführen, die ihren Kunden gehören. Dienstanbieter können sich bei ihrem eigenen Azure AD-Mandanten anmelden und sind autorisiert, in delegierten Kundenabonnements und Ressourcengruppen zu arbeiten. Dadurch können sie Verwaltungsvorgänge im Namen ihrer Kunden durchführen, ohne sich bei den einzelnen Kundenmandanten anmelden zu müssen.

> [!NOTE]
> Die delegierte Azure-Ressourcenverwaltung kann auch [in einem Unternehmen verwendet werden, das über mehrere eigene Azure AD-Mandanten verfügt](enterprise.md), um die mandantenübergreifende Verwaltung zu vereinfachen.

Mit der delegierten Azure-Ressourcenverwaltung können autorisierte Benutzer direkt im Kontext eines Kundenabonnements arbeiten, ohne dass Sie über ein Konto im Mandanten des Kunden verfügen oder Mitbesitzer des Mandanten des Kunden sind. Sie können ferner [auf der neuen Seite **Meine Kunden** im Azure-Portal alle delegierten Kundenabonnements anzeigen und verwalten](../how-to/view-manage-customers.md).

Die [mandantenübergreifende Erfahrung](cross-tenant-management-experience.md) hilft Ihnen, effizienter mit Azure-Verwaltungsdiensten zu arbeiten wie Azure Policy, Azure Security Center und mehr. Alle Dienstanbieteraktivitäten werden im Aktivitätsprotokoll nachverfolgt, das im Mandanten des Dienstanbieters gespeichert wird (und von Benutzern im verwaltenden Mandanten angezeigt werden kann). Dies bedeutet, dass sowohl der Kunde als auch der Dienstanbieter leicht den Benutzer identifizieren können, dem Änderungen zugeordnet sind.

Wenn Sie einen Kunden in die delegierte Azure-Ressourcenverwaltung integrieren, hat er Zugriff auf die Seite **Dienstanbieter** im Azure-Portal, auf der er [seine Angebote, Dienstanbieter und delegierten Ressourcen bestätigen und verwalten](../how-to/view-manage-service-providers.md) kann. Wenn der Kunde den Zugriff für einen Dienstanbieter jemals widerrufen möchte, kann er dies hier jederzeit tun.

Sie können [den neuen Angebotstyp für verwaltete Dienste im Azure Marketplace veröffentlichen](../how-to/publish-managed-services-offers.md), um das Onboarding von Kunden für die delegierte Azure-Ressourcenverwaltung zu vereinfachen. Alternativ können Sie [den Onboardingprozess durchführen, indem Sie Azure Resource Manager-Vorlagen bereitstellen](../how-to/onboard-customer.md).

## <a name="how-azure-delegated-resource-management-works"></a>Funktionsweise der delegierten Azure-Ressourcenverwaltung

Generell funktioniert die delegierte Azure-Ressourcenverwaltung wie folgt:

1. Als Dienstanbieter identifizieren Sie den Zugriff (Rollen), den ihre Gruppen, Dienstprinzipale oder Benutzer benötigen, um die Azure-Ressourcen des Kunden zu verwalten. Die Zugriffsdefinition enthält die Mandanten-ID des Dienstanbieters zusammen mit dem erforderlichen Zugriff für das Angebot, definiert mithilfe der **principalid**-Identitäten aus Ihrem Mandanten, die den [integrierten **roleDefinition**-Werten](../../role-based-access-control/built-in-roles.md) zugeordnet sind (Mitwirkender, Mitwirkender für virtuelle Computer, Leser usw.).
2. Sie geben diesen Zugriff an und integrieren den Kunden in die delegierte Azure-Ressourcenverwaltung auf eine der zwei Arten:
   - [Veröffentlichen eines Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md) (privat oder öffentlich), das der Kunde annimmt
   - [Bereitstellen einer Azure Resource Manager Vorlage im Mandanten des Kunden](../how-to/onboard-customer.md) für ein oder mehrere bestimmte Abonnements oder Ressourcengruppen
3. Nachdem das Onboarding des Kunden durchgeführt wurde, können sich autorisierte Benutzer bei Ihrem Dienstanbietermandanten anmelden und Verwaltungsaufgaben im vorgegebenen Kundenumfang, basierend auf dem von Ihnen definierten Zugriff, durchführen.

> [!NOTE]
> Sie können delegierte Ressourcen verwalten, die sich in unterschiedlichen [Regionen](../../availability-zones/az-overview.md#regions) befinden. Die Delegierung von Abonnements auf eine [nationale Cloud](../../active-directory/develop/authentication-national-cloud.md) und die öffentliche Azure-Cloud oder zwei separate nationale Clouds wird nicht unterstützt.

## <a name="support-for-azure-delegated-resource-management"></a>Unterstützung für delegierte Azure-Ressourcenverwaltung

Wenn Sie Hilfe im Zusammenhang mit der delegierten Azure-Ressourcenverwaltung benötigen, können Sie eine Supportanfrage im Azure-Portal öffnen. Wählen Sie als **Problemtyp** **Technisch** aus. Wählen Sie ein Abonnement aus und dann **Lighthouse** (unter **Überwachung und Verwaltung**).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](cross-tenant-management-experience.md).
- Erfahren Sie über [Angebote für verwaltete Dienste in Azure Marketplace](managed-services-offers.md).