---
title: Azure Virtual WAN – Übersicht | Microsoft-Dokumentation
description: Sie erhalten Informationen darüber, wie Sie die per Virtual WAN automatisierte skalierbare Konnektivität zwischen Branches einrichten, über verfügbare Regionen und Partner.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 05/14/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 8bdba64445212c564a3d4762bc8497be15f7d9a0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657006"
---
# <a name="about-azure-virtual-wan"></a>Informationen zu Azure Virtual WAN

Azure Virtual WAN ist ein Netzwerkdienst, der viele Netzwerk-, Sicherheits- und Routingfunktionen auf einer einzigen Bedienoberfläche vereint. Diese Funktionen umfassen Zweigestellenverbindungen (über die Konnektivitätsautomatisierung von Virtual WAN-Partnereinrichtungen wie SD-WAN oder VPN CPE), Site-to-Site-VPN-Konnektivität, Remotebenutzer-VPN (Point-to-Site), private Konnektivität (ExpressRoute), Konnektivität zwischen Clouds (transitive Konnektivität für virtuelle Netzwerke), VPN ExpressRoute-Interkonnektivität, Routing, Azure Firewall und Verschlüsselung für private Konnektivität. Sie müssen nicht all diese Anwendungsfälle abdecken, um mit der Nutzung von Virtual WAN beginnen zu können. Sie können einfach mit nur einem Anwendungsfall loslegen und das Netzwerk dann abhängig von der weiteren Entwicklung anpassen.

Die Virtual WAN-Architektur ist eine Hub-and-Spoke-Architektur mit integrierter Skalierung und Leistung für Zweigstellen (VPN-/SD-WAN-Geräte), Benutzer (Azure-VPN-, OpenVPN- oder IKEv2-Clients), ExpressRoute-Leitungen und virtuelle Netzwerke. Sie ermöglicht eine [Architektur mit einem globalen Transitnetzwerk](virtual-wan-global-transit-network-architecture.md), bei dem der in der Cloud gehostete Netzwerkhub Übertragungsverbindungen zwischen Endpunkten ermöglicht, die sich in unterschiedlichen Typen von Spokes befinden.

Azure-Regionen dienen als Hubs, die Sie auswählen können, um sich mit ihnen zu verbinden. Alle Hubs sind per Standard-Virtual WAN vollständig miteinander vernetzt, damit Benutzer den Microsoft-Backbone für die Any-to-Any-Konnektivität (alle Spokes) nutzen können. Für Spokekonnektivität mit SD-WAN/VPN-Geräten können Benutzer dies entweder manuell in Azure Virtual WAN einrichten oder die Partnerlösung für Virtual WAN CPE (SD-WAN/VPN) nutzen, um Konnektivität mit Azure einzurichten. Wir führen eine Liste mit Partnern, die die Automatisierung der Konnektivität mit Azure Virtual WAN unterstützen (sie bieten die Möglichkeit, die Geräteinformationen in Azure zu exportieren, die Azure-Konfiguration herunterzuladen und Konnektivität herzustellen). Weitere Informationen finden Sie in dem Artikel [Virtual WAN-Partner und -Standorte](virtual-wan-locations-partners.md).

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan1.png)

Dieser Artikel enthält eine kurze Übersicht über die Netzwerkkonnektivität in Azure Virtual WAN. Virtual WAN hat die folgenden Vorteile:

* **Integrierte Konnektivitätslösungen (Hub & Spoke):** Automatisieren Sie die Site-to-Site-Konfiguration und -Konnektivität zwischen lokalen Standorten und einem Azure-Hub.
* **Automatisierte Spoke-Einrichtung und -Konfiguration:** Verbinden Sie Ihre virtuellen Netzwerke und Workloads nahtlos mit dem Azure-Hub.
* **Intuitive Problembehandlung:** Sie können den gesamten Datenfluss in Azure anzeigen und diese Informationen nutzen, um erforderliche Aktionen durchzuführen.

## <a name="basic-and-standard-virtual-wans"></a><a name="basicstandard"></a>Virtuelle WANs des Typs „Basic“ und „Standard“

Es gibt zwei Arten virtueller WANs: Basic und Standard. Die folgende Tabelle zeigt die verfügbaren Konfigurationen für die beiden Typen.

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

Anweisungen für ein Upgrade eines virtuellen WAN finden Sie unter [Upgrade eines virtuellen WAN von Basic auf Standard](upgrade-virtual-wan.md).

## <a name="architecture"></a><a name="architecture"></a>Architektur

Informationen zur Virtual WAN-Architektur sowie zum Migrieren zu Virtual WAN finden Sie in den folgenden Artikeln:

* [Migrieren zu Azure Virtual WAN](migrate-from-hub-spoke-topology.md)
* [Architektur mit einem globalen Transitnetzwerk und Azure Virtual WAN](virtual-wan-global-transit-network-architecture.md)

## <a name="virtual-wan-resources"></a><a name="resources"></a>Virtual WAN-Ressourcen

Sie erstellen die folgenden Ressourcen, um ein virtuelles End-to-End-WAN zu konfigurieren:

* **virtualWAN:** Die Ressource „virtualWAN“ stellt eine virtuelle Überlagerung Ihres Azure-Netzwerks dar und ist eine Sammlung aus mehreren Ressourcen. Sie enthält Links zu allen virtuellen Hubs, die Teil des virtuellen WAN sein sollen. Virtual WAN-Ressourcen sind voneinander isoliert und können keinen gemeinsamen Hub enthalten. Virtuelle Hubs in Virtual WAN kommunizieren nicht miteinander.

* **Hub:** Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk. Der Hub enthält verschiedene Dienstendpunkte zum Ermöglichen von Konnektivität. In Ihrem lokalen Netzwerk (vpnsite) können Sie sich innerhalb des virtuellen Hubs mit einer VPN Gateway-Instanz verbinden, ExpressRoute-Leitungen mit einem virtuellen Hub verbinden oder sogar mobile Benutzer mit einem Point-to-Site-Gateway im virtuellen Hub verbinden. Der Hub ist der Kern Ihres Netzwerks in einer Region. Es kann nur ein Hub pro Azure-Region vorhanden sein.

  Ein Hub-Gateway ist nicht das gleiche wie ein Gateway für virtuelle Netzwerke, das Sie für ExpressRoute und VPN Gateway verwenden. Bei der Verwendung von Virtual WAN erstellen Sie beispielsweise von Ihrer lokalen Site aus keine direkte Site-to-Site-Verbindung mit Ihrem VNET. Stattdessen erstellen Sie eine Site-to-Site-Verbindung mit dem Hub. Der Datenverkehr verläuft immer über das Hub-Gateway. Dies bedeutet, dass Ihre VNETs kein eigenes Gateway für virtuelle Netzwerke benötigen. Mit Virtual WAN können Sie für Ihre VNETs über den virtuellen Hub und das virtuelle Hub-Gateway leicht eine Skalierung durchführen.

* **Virtuelle Netzwerkverbindung für Hub:** Die Ressource für die virtuelle Netzwerkverbindung für den Hub wird verwendet, um den Hub nahtlos mit Ihrem virtuellen Netzwerk zu verbinden.

* **(Vorschau) Hub-zu-Hub-Verbindung**: In einem virtuellen WAN sind alle Hubs miteinander verbunden. Dies bedeutet, dass mit einem lokalen Hub verbundene Niederlassungen, Benutzer oder VNETs mit einer anderen Niederlassung oder anderen VNETs mithilfe der vollständig vernetzten Architektur der verbundenen Hubs kommunizieren können. Sie können auch über das vernetzte Hub-zu-Hub-Framework VNETs innerhalb eines Hubs, der durch den virtuellen Hub führt, sowie VNETs zwischen Hubs verbinden.

* **Hubroutingtabelle:**  Sie können eine virtuelle Hubroute erstellen und die Route der Routingtabelle des virtuellen Hubs zuweisen. Sie können der Routingtabelle des virtuellen Hubs mehrere Routen zuweisen.

**Zusätzliche Virtual WAN-Ressourcen**

  * **Site:** Diese Ressource wird ausschließlich für Site-to-Site-Verbindungen verwendet. Die Ressource „site“ ist **vpnsite**. Sie stellt Ihr lokales VPN-Gerät und die zugehörigen Einstellungen dar. Durch die Zusammenarbeit mit einem Virtual WAN-Partner verfügen Sie über eine integrierte Lösung zum automatischen Exportieren dieser Informationen nach Azure.

## <a name="types-of-connectivity"></a><a name="connectivity"></a>Konnektivitätstypen

Virtual WAN lässt die folgenden Konnektivitätstypen zu: Site-to-Site-VPN, Benutzer-VPN (Point-to-Site) und ExpressRoute.

### <a name="site-to-site-vpn-connections"></a><a name="s2s"></a>Site-to-Site-VPN-Verbindungen

![Virtual WAN-Diagramm](./media/virtual-wan-about/virtualwan.png)

Wenn Sie in Virtual WAN eine Site-to-Site-Verbindung einrichten, können Sie mit einem verfügbaren Partner arbeiten. Wenn Sie keinen Partner verwenden möchten, können Sie die Verbindung manuell konfigurieren. Weitere Informationen finden Sie unter [Erstellen einer Site-to-Site-Verbindung per Virtual WAN](virtual-wan-site-to-site-portal.md).

#### <a name="virtual-wan-partner-workflow"></a><a name="s2spartner"></a>Workflow für Virtual WAN-Partner

Wenn Sie mit einem Virtual WAN-Partner arbeiten, ist der Workflow wie folgt:

1. Der Controller des Zweigstellengeräts (VPN/SDWAN) wird authentifiziert, um standortbezogene Informationen mithilfe eines [Azure-Dienstprinzipals](../active-directory/develop/howto-create-service-principal-portal.md) zu exportieren.
2. Der Controller des Branchgeräts (VPN/SDWAN) ruft die Azure-Konnektivitätskonfiguration ab und aktualisiert das lokale Gerät. So werden für das lokale VPN-Gerät der Konfigurationsdownload und die Bearbeitung und die Aktualisierung automatisiert.
3. Nachdem das Gerät über die richtige Azure-Konfiguration verfügt, wird eine Site-to-Site-Verbindung (zwei aktive Tunnel) mit dem Azure WAN eingerichtet. Azure unterstützt IKEv1 und IKEv2. BGP ist optional.

#### <a name="partners-for-site-to-site-virtual-wan-connections"></a><a name="partners"></a>Partner für Site-to-Site-Verbindungen in Virtual WAN

Eine Liste der verfügbaren Partner und Standorte finden Sie in dem Artikel [Virtual WAN-Partner und -Standorte](virtual-wan-locations-partners.md).

### <a name="user-vpn-point-to-site-connections"></a><a name="uservpn"></a>Benutzer-VPN-Verbindungen (Point-to-Site)

Sie können sich über eine IPsec/IKE (IKEv2)- oder OpenVPN-Verbindung mit Ihren Ressourcen in Azure verbinden. Für diese Art von Verbindung muss auf dem Clientcomputer ein VPN-Client konfiguriert sein. Weitere Informationen finden Sie unter [Herstellen einer Point-to-Site-Verbindung](virtual-wan-point-to-site-portal.md).

### <a name="expressroute-connections"></a><a name="er"></a>ExpressRoute-Verbindungen
Mit ExpressRoute können Sie Ihr lokales Netzwerk über eine private Verbindung mit Azure verbinden. Informationen zum Erstellen der Verbindung finden Sie unter [Erstellen einer ExpressRoute-Verbindung per Virtual WAN](virtual-wan-expressroute-portal.md).

## <a name="locations"></a><a name="locations"></a>Standorte

Informationen zu Standorten finden Sie in dem Artikel [Virtual WAN-Partner und -Standorte](virtual-wan-locations-partners.md).

## <a name="faq"></a><a name="faq"></a>Häufig gestellte Fragen

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN](virtual-wan-site-to-site-portal.md)
