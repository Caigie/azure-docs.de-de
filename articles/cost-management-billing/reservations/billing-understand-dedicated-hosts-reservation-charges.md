---
title: Grundlegendes zum Rabatt für reservierte Azure Dedicated Host-Instanzen
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure-VM-Instanzen auf Azure Dedicated Host-Instanzen angewendet wird.
author: yashesvi
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: f9080d13c4358a03b3f13ffcf346af42e49c47c6
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88689455"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-azure-dedicated-hosts"></a>Anwendung des Azure-Reservierungsrabatts auf Azure Dedicated Host-Instanzen

Nach dem Erwerb einer reservierten Azure Dedicated Host-Instanz wird der Reservierungsrabatt automatisch auf dedizierte Hosts angewendet, die den Attributen und der Menge der Reservierung entsprechen. Eine Reservierung deckt die Computekosten Ihrer dedizierten Hosts ab.

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Reservierungsrabatte funktionieren nach dem Prinzip „*use-it-or-lose-it*“. Das heißt, wenn Sie für eine Stunde nicht über die entsprechenden Ressourcen verfügen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.

Wenn Sie einen dedizierten Host löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Wenn keine übereinstimmenden Ressourcen im angegebenen Reservierungsumfang gefunden werden, gehen die reservierten Stunden *verloren*.

## <a name="reservation-discount-for-dedicated-hosts"></a>Reservierungsrabatt für Dedicated Host-Instanzen

Durch eine reservierte Azure Dedicated Host-Instanz erhalten Sie einen Rabatt auf die Kosten der Computeinfrastruktur, die für Ihre dedizierten Hosts verwendet wird. Der Rabatt für Ihre dedizierten Hosts gilt unabhängig davon, ob diese von virtuellen Computern genutzt werden oder nicht. Zusätzliche Kosten, etwa für die Lizenzierung, die Netzwerknutzung oder den Speicherverbrauch durch virtuelle Computer, die auf dem dedizierten Host bereitgestellt werden, sind durch die Reservierung nicht abgedeckt.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Sollten Sie weitere Fragen haben oder Hilfe benötigen,  [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)

- [Dedizierte Azure-Hosts](https://docs.microsoft.com/azure/virtual-machines/windows/dedicated-hosts)

- [Azure Dedicated Host – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Verwalten von Azure-Reservierungen](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)

- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage)

- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)

- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)

