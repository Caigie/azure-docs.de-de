---
title: Senken der Kosten für Azure Synapse Analytics mit reservierter Azure-Kapazität
description: Hier erfahren Sie, wie Sie die Kosten für Azure Synapse Analytics mit reservierter Kapazität senken und so Geld sparen können.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: ca83760f09ea29ba0b21a0f7177dd3f3188c44cd
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686138"
---
# <a name="save-costs-for-azure-synapse-analytics-charges-with-reserved-capacity"></a>Senken der Kosten für Azure Synapse Analytics mit reservierter Kapazität

Mit Azure Synapse Analytics können Sie Geld sparen, indem Sie sich für eine Reservierung für Ihre cDWU-Nutzung für einen Zeitraum von einem Jahr bzw. drei Jahren entscheiden. Um reservierte Kapazität für Azure Synapse Analytics zu erwerben, müssen Sie die Azure-Region und die Laufzeit auswählen. Fügen Sie dann die Azure Synapse Analytics-SKU Ihrem Warenkorb hinzu, und wählen Sie die Menge der cDWU-Einheiten aus, die Sie erwerben möchten.

Wenn Sie eine Reservierung erwerben, wird die Nutzung von Azure Synapse Analytics, die den Reservierungsattributen entspricht, nicht mehr zu den Preisen für die nutzungsbasierte Bezahlung abgerechnet.

Eine Reservierung deckt keine mit der Azure Synapse Analytics-Nutzung verbundenen Speicher- oder Netzwerkgebühren ab.

Nach Ablauf der reservierten Kapazität werden Azure Synapse Analytics-Instanzen weiterhin ausgeführt, jedoch zu den Preisen für die nutzungsbasierte Bezahlung abgerechnet. Reservierungen werden nicht automatisch verlängert.

Informationen zu den Preisen finden Sie unter [Azure Synapse Analytics – Preise](https://azure.microsoft.com/pricing/details/synapse-analytics/).

Sie können reservierte Azure Synapse Analytics-Kapazität über das [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) erwerben. Bezahlen Sie die Reservierung [im Voraus oder monatlich](monthly-payments-reservations.md). So erwerben Sie reservierte Kapazität:

- Sie müssen über die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Bezahlung verfügen.
- Bei Enterprise-Abonnements muss im [EA-Portal](https://ea.azure.com/) die Option **Reservierte Instanzen hinzufügen** aktiviert werden. Wenn diese Einstellung deaktiviert ist, müssen Sie EA-Administrator sein.
- Für das Cloud Solution Provider-Programm (CSP) können nur die Administratoren oder Vertriebsmitarbeiter reservierte Azure Synapse Analytics-Kapazität erwerben.

Weitere Informationen zur Berechnung der Reservierung von Kapazitäten für Unternehmenskunden und Kunden mit nutzungsbasierter Bezahlung finden Sie unter [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md) und [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Auswählen der passenden Größe vor dem Kauf

Die Reservierungsgröße für Azure Synapse Analytics sollte auf der Grundlage der gesamten Compute Data Warehouse Units (cDWU), die Sie nutzen, berechnet werden. Der Kauf erfolgt in Schritten von je 100 cDWU.

Angenommen, für Ihre Gesamtnutzung von Azure Synapse Analytics benötigen Sie DW3000c. Dafür möchten Sie reservierte Kapazität erwerben. Sie sollten also eine reservierte Kapazität von 30 cDWU-Einheiten erwerben.

## <a name="buy-azure-synapse-analytics-reserved-capacity"></a>Erwerben von reservierter Azure Synapse Analytics-Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie ein Abonnement aus. Wählen Sie in der Abonnementliste das Abonnement aus, das für den Kauf der reservierten Kapazität verwendet wird. Die Zahlungsmethode für das Abonnement wird mit den Kosten für die Reservekapazität belastet. Der Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummern: MS-AZR-0017P oder MS-AZR-0148P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummern: MS-AZR-0003P oder MS-AZR-0023P) sein.
   - Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet.
   - Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.
4. Wählen Sie einen Bereich aus. Wählen Sie in der Bereichsliste einen Abonnementbereich aus.
   - **Einzelne Ressourcengruppe**: Wendet den Reservierungsrabatt nur auf die entsprechenden Ressourcen in der ausgewählten Ressourcengruppe an.
   - **Einzelnes Abonnement**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen im ausgewählten Abonnement an.
   - **Gemeinsam genutzt**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen in berechtigten Abonnements innerhalb des Abrechnungskontexts an. Für Kunden mit einem Enterprise Agreement ist der Abrechnungskontext die Registrierung. Für Kunden mit individuellen Abonnements mit nutzungsbasierten Tarifen handelt es sich beim Abrechnungsbereich um alle berechtigten Abonnements, die vom Kontoadministrator erstellt wurden.
   - Für Enterprise-Kunden ist der Abrechnungskontext die EA-Registrierung.
   - Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.
5. Wählen Sie eine Azure-Region aus, die durch die reservierte Kapazität abgedeckt ist.
6. Wählen Sie die Menge aus. Geben Sie als Menge die 100 Data Warehouse-Einheiten (cDWU) ein, die Sie erwerben möchten.    
   Bei einer Menge von 30 erhalten Sie beispielsweise 3.000 cDWU-Einheiten reservierter Kapazität pro Stunde.
7. Überprüfen Sie die Kosten für die reservierte Azure Synapse Analytics-Kapazität im Abschnitt **Kosten**.
8. Wählen Sie die Option **Kaufen**.
9. Wählen Sie **Diese Reservierung anzeigen** aus, um den Status des Kaufs anzuzeigen.

## <a name="cancel-exchange-or-refund-reservations"></a>Stornieren, Umtauschen oder Rückerstatten von Reservierungen

Reservierungen können unter bestimmten Einschränkungen storniert, umgetauscht oder rückerstattet werden. Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](exchange-and-refund-azure-reservations.md).

Der Reservierungsrabatt wird automatisch auf die Anzahl von Azure Synapse Analytics-Instanzen angewendet, die dem Umfang und der Region der reservierten Azure Synapse Analytics-Kapazität entsprechen. Sie können den Umfang der reservierten Azure Synapse Analytics-Kapazität über das [Azure-Portal](https://portal.azure.com/), PowerShell, die Befehlszeilenschnittstelle oder die API aktualisieren.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Reservierungsrabatten für Azure Synapse Analytics finden Sie unter [Anwendung von Reservierungsrabatten auf Azure Synapse Analytics](prepay-sql-data-warehouse-charges.md).

- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
  - [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md)
  - [Grundlegendes zum Rabatt für Azure-Reservierungen](understand-reservation-charges.md)
  - [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](understand-reserved-instance-usage.md)
  - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md)
