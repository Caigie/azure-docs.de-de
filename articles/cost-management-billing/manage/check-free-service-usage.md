---
title: Überwachen und Nachverfolgen der Nutzung von kostenlosen Azure-Diensten
description: Erfahren Sie, wie Sie die Nutzung von kostenlosen Diensten im Azure-Portal überprüfen. Es werden keine Gebühren für in einem kostenlosen Konto enthaltene Dienste in Rechnung gestellt, solange Sie deren Grenzwerte nicht überschreiten.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 787d54ba2050a0293957310dbc8377b83a7f7bfc
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690050"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Überprüfen der Verwendung der kostenlosen Dienste, die in Ihrem kostenlosen Azure-Konto enthalten sind

Ihnen werden keine Gebühren für kostenlose Dienste, die in einem kostenlosen Azure-Konto enthalten sind, in Rechnung gestellt, solange Sie die Grenzwerte der Dienste nicht überschreiten. Um die Grenzwerte einzuhalten, können Sie das Azure-Portal zum Nachverfolgen der Nutzung kostenloser Dienste verwenden.

## <a name="check-usage-in-the-azure-portal"></a>Überprüfen der Nutzung im Azure-Portal

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2.  Suchen Sie nach **Abonnements**.

    ![Screenshot der Suche im Portal nach Abonnements](./media/check-free-service-usage/billing-search-subscriptions.png)

3.  Wählen Sie das Abonnement aus, das Sie erstellt haben, als Sie sich für Ihr kostenloses Azure-Konto registriert haben.

4.  Scrollen Sie nach unten, um die Tabelle zur Nutzung des kostenlosen Diensts anzuzeigen.

    ![Screenshot mit Verwendung der kostenlosen Dienste](./media/check-free-service-usage/subscription-usage-free-services.png)

    Die Tabelle enthält folgende Spalten:

* **Verbrauchseinheit:** Gibt die Maßeinheit für den Dienst an, für den der Verbrauch gemessen wird.
* **Nutzung/Limit:** Nutzung und Grenzwert für die Verbrauchseinheit für den aktuellen Monat.
* **Status:** Verwendungsstatus des Diensts. Basierend auf Ihrer Nutzung kann einer der folgenden Status gelten:
  * **Nicht in Verwendung:** Sie haben diese Verbrauchseinheit nicht verwendet, oder die Verwendung der Verbrauchseinheit hat das Abrechnungssystem nicht erreicht.
  * **Überschritten am \<Date>:** Sie haben den Grenzwert für die Verbrauchseinheit am \<Date> überschritten.
  * **Überschreitung unwahrscheinlich:** Es ist unwahrscheinlich, dass Sie den Grenzwert für diese Verbrauchseinheit überschreiten.
  * **Überschreitung am \<Date>:** Sie werden den Grenzwert für die Verbrauchseinheit wahrscheinlich am \<Date> überschreiten.

> [!IMPORTANT]
>
> Kostenlose Dienste sind nur für das Abonnement verfügbar, das Sie erstellt haben, als Sie sich für Ihr kostenloses Azure-Konto registriert haben. Wenn die Tabelle mit den kostenlosen Diensten in der Abonnementübersicht nicht angezeigt wird, sind sie für das Abonnement nicht verfügbar.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- [Durchführen eines Upgrades für Ihr kostenloses Azure-Konto](upgrade-azure-subscription.md)
