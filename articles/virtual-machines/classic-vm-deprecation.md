---
title: Klassische Azure-VMs werden ab dem 1. März 2023 nicht mehr unterstützt
description: Der Artikel bietet einen allgemeinen Überblick über das Auslaufen klassischer VMs.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 7488ef45d665d95a28f69b7af887b98dd5a76376
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84678374"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>Migrieren Ihrer IaaS-Ressourcen zu Azure Resource Manager vor dem 1. März 2023 

2014 haben wir IaaS in Azure Resource Manager eingeführt und die Funktionen seitdem kontinuierlich weiter ausgebaut. Da [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) jetzt über vollständige IaaS-Funktionen und andere Erweiterungen verfügt, haben wir am 28. Februar 2020 begonnen, die Verwaltung von IaaS-VMs über Azure Service Manager einzustellen. Am 1. März 2023 wird die Unterstützung dieser Funktionen vollständig beendet. 

Derzeit nutzen etwa 90% der IaaS-VMs Azure Resource Manager. Wenn Sie IaaS-Ressourcen über Azure Service Manager (ASM) nutzen, sollten Sie ab sofort mit der Planung der Migration beginnen und diesen Vorgang bis zum 1. März 2023 abschließen, um zu [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/) zu wechseln.

Für klassische VMs gilt die [moderne Lebenszyklusrichtlinie](https://support.microsoft.com/help/30881/modern-lifecycle-policy) für die Außerbetriebnahme.

## <a name="how-does-this-affect-me"></a>Welche Folgen hat das für mich? 

1) Seit dem 28. Februar 2020 können Kunden, die im Februar 2020 keine IaaS-VMs über Azure Service Manager (ASM) verwendet haben, keine klassischen VMs mehr erstellen. 
2) Ab dem 1. März 2023 können Kunden keine IaaS-VMs mehr über Azure Service Manager starten. VMs, die zu diesem Zeitpunkt ausgeführt werden oder zugeordnet sind, werden beendet, und die Zuordnung wird aufgehoben. 
2) Am 1. März 2023 werden alle Abonnements, die noch nicht zu Azure Resource Manager migriert wurden, über unseren Zeitplan für das Löschen der verbleibenden klassischen VMs informiert.  

Die folgenden Azure-Dienste und -Funktionen sind von dieser Aussonderung **NICHT** betroffen: 
- Cloud Services 
- Speicherkonten, die **nicht** von klassischen VMs genutzt werden 
- Virtuelle Netzwerke (VNETs), die **nicht** von klassischen VMs genutzt werden 
- Sonstige klassische Ressourcen

## <a name="what-actions-should-i-take"></a>Welche Aktionen zieht das für mich nach sich? 

- Beginnen Sie noch heute mit der Planung Ihre Migration zu Azure Resource Manager. 

- Weitere Informationen zur Migration Ihrer klassischen virtuellen [Linux](./linux/migration-classic-resource-manager-plan.md)- und [Windows](./windows/migration-classic-resource-manager-plan.md)-Computer zu Azure Resource Manager finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

- Zusätzliche Informationen finden Sie unter [Häufig gestellte Fragen zur Migration vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq).

- Wenden Sie sich bei technischen Fragen oder Problemen und für die Aufnahme Ihres Abonnements in die Whitelist an den [Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

- Wenn Sie sonstige Fragen haben, die nicht bei den häufig gestellten Fragen aufgeführt sind und bei denen es sich nicht um Feedback handelt, können Sie unten einen Kommentar schreiben.
