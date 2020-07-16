---
title: Überlegungen zum Cloud Solution Provider-Programm
description: Die delegierte Azure-Ressourcenverwaltung unterstützt CSP-Partner bei der Verbesserung der Sicherheit und Kontrolle durch die Aktivierung präziser Berechtigungen.
ms.date: 07/06/2020
ms.topic: conceptual
ms.openlocfilehash: 8dd40c5c4f7131554dcc3249777de9f0d92c35bb
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86131512"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse und das Programm für Cloud-Lösungsanbieter

Als [CSP](/partner-center/csp-overview)-Partner (Cloud Solution Provider) können Sie bereits auf die durch das CSP-Programm für Ihre Kunden erstellten Azure-Abonnements zugreifen, indem Sie die Funktion [AOBO (Administer On Behalf Of, Verwalten im Namen von)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) verwenden. Dieser Zugriff gestattet es Ihnen, die Abonnements Ihrer Kunden direkt zu unterstützen, zu konfigurieren und zu verwalten.

Mit [Azure Lighthouse](../overview.md) können Sie die delegierte Azure-Ressourcenverwaltung zusammen mit AOBO verwenden. Dies trägt zur Verbesserung der Sicherheit bei und reduziert unnötigen Zugriff, da Sie präzisere Berechtigungen für Ihre Benutzer vergeben können. Darüber hinaus werden eine höhere Effizienz und Skalierbarkeit ermöglicht, da Ihre Benutzer mit nur einer Anmeldung bei Ihrem Mandanten mehrere Kundenabonnements verwenden können.

> [!TIP]
> Damit die Ressourcen der Kunden geschützt sind, überprüfen und befolgen Sie unsere [empfohlenen Sicherheitsmaßnahmen](recommended-security-practices.md) sowie die [Sicherheitsanforderungen für Partner](/partner-center/partner-security-requirements).

## <a name="administer-on-behalf-of-aobo"></a>Verwalten im Namen von (AOBO)

Mit AOBO erhält jeder Benutzer mit der Rolle [Administrator-Agent](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) in Ihrem Mandanten AOBO-Zugriff auf Azure-Abonnements, die Sie durch das CSP-Programm erstellen. Alle Benutzer, die Zugriff auf Abonnements von Kunden benötigen, müssen Mitglied dieser Gruppe sein. AOBO lässt nicht die Flexibilität zu, unterschiedliche Gruppen zu erstellen, die mit verschiedenen Kunden zusammenarbeiten, oder verschiedene Rollen für Gruppen oder Benutzer zu aktivieren.

![Mandantenverwaltung mittels AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Delegierte Azure-Ressourcenverwaltung

Mit Azure Lighthouse können Sie verschiedenen Kunden oder Rollen unterschiedliche Gruppen zuweisen, wie im folgenden Diagramm dargestellt. Da Benutzer durch die delegierte Azure-Ressourcenverwaltung über die geeignete Zugriffsebene verfügen, können Sie die Anzahl der Benutzer verringern, die über die Rolle „Administrator-Agent“ verfügen (und somit über AOBO-Vollzugriff verfügen). Dies trägt zu einer höheren Sicherheit durch Einschränkung des unnötigen Zugriffs auf die Ressourcen ihrer Kunden bei. Es verleiht Ihnen außerdem mehr Flexibilität, um mehrere Kunden skaliert zu verwalten.

Das Onboarding eines Abonnements, das Sie über das CSP-Programm erstellt haben, erfolgt gemäß den Schritten, die in [Onboarding eines Kunden zu Azure Lighthouse](../how-to/onboard-customer.md) beschrieben sind. Jeder Benutzer, der in Ihrem Mandanten über die Rolle „Administrator-Agent“ verfügt, kann dieses Onboarding durchführen.

![Mandantenverwaltung mithilfe von AOBO und delegierter Azure-Ressourcenverwaltung](../media/csp-2.jpg)

> [!NOTE]
> Die [Seite **Meine Kunden** im Azure-Portal](../how-to/view-manage-customers.md) enthält jetzt einen Abschnitt **Cloud Solution Provider (Vorschau)** , in dem Abrechnungsinformationen und Ressourcen für CSP-Kunden aufgeführt werden, die die [Microsoft-Kundenvereinbarung signiert](/partner-center/confirm-customer-agreement) haben und [den Azure-Plan nutzen](/partner-center/azure-plan-get-started). Weitere Informationen finden Sie unter [Erste Schritte mit Ihrem Abrechnungskonto für eine Microsoft-Partnervereinbarung](../../cost-management-billing/understand/mpa-overview.md).
>
> In diesem Abschnitt werden möglicherweise CSP-Kunden angezeigt, unabhängig davon, ob sie auch ein Onboarding in die delegierte Azure-Ressourcenverwaltung durchgeführt haben. Wenn das Onboarding durchgeführt wurde, werden sie auch im Abschnitt **Kunden** angezeigt, wie unter [Anzeigen und Verwalten von Kunden und delegierten Ressourcen](../how-to/view-manage-customers.md) beschrieben. Ebenso muss ein CSP-Kunde nicht im Abschnitt **Cloud Solution Provider (Vorschau)** auf der Seite **Meine Kunden** angezeigt werden, damit Sie das Onboarding dieses Kunden in die delegierte Azure-Ressourcenverwaltung durchführen können.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](cross-tenant-management-experience.md).
- Erfahren Sie mehr über das [Onboarding eines Abonnements in Azure Lighthouse](../how-to/onboard-customer.md).
- Erhalten Sie Informationen über das [Programm für Cloud-Lösungsanbieter](/partner-center/csp-overview).
