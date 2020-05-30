---
title: Azure Load Balancer-Komponenten
description: Übersicht über die Azure Load Balancer-Komponenten
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2020
ms.author: allensu
ms.openlocfilehash: 84857315e4b6b4375ed5b78520b4c6ff0d66751a
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684982"
---
# <a name="azure-load-balancer-components"></a>Azure Load Balancer-Komponenten

Azure Load Balancer besteht aus ein paar Komponenten. Diese können in Ihrem Abonnement über das Azure-Portal, die Azure-Befehlszeilenschnittstelle, Azure PowerShell oder Vorlagen konfiguriert werden.

## <a name="frontend-ip-configuration"></a>Front-End-IP-Konfiguration<a name = "frontend-ip-configurations"></a>

Die IP-Adresse Ihres Azure Load Balancers. Sie ist der Kontaktpunkt für Clients. Diese IP-Adressen sind möglich:

- **Öffentliche IP-Adresse**
- **Private IP-Adresse**

Die Art der IP-Adresse bestimmt den **Typ** des erstellten Lastenausgleichs. Wenn Sie eine private IP-Adresse auswählen, wird ein interner Lastenausgleich erstellt. Bei Auswahl einer öffentlichen IP-Adresse wird ein öffentlicher Lastenausgleich erstellt.

|  | Öffentlicher Load Balancer  | Interner Lastenausgleich |
| ---------- | ---------- | ---------- |
| Front-End-IP-Konfiguration| Öffentliche IP-Adresse | Private IP-Adresse|
| BESCHREIBUNG | Bei einem öffentlichen Lastenausgleich werden die öffentliche IP-Adresse und der Port des eingehenden Datenverkehrs der privaten IP-Adresse und dem Port der VM zugeordnet. Für den Antwortdatenverkehr von der VM führt der Lastenausgleich eine Zuordnung in umgekehrter Richtung durch. Sie können bestimmte Typen von Datenverkehr auf verschiedene VMs oder Dienste verteilen, indem Sie Lastenausgleichsregeln anwenden. Sie können zum Beispiel die Netzwerklast von Webanforderungen auf mehrere Webserver verteilen.| Ein interner Lastenausgleich verteilt Datenverkehr auf Ressourcen, die sich in einem virtuellen Netzwerk befinden. Azure schränkt den Zugriff auf die Front-End-IP-Adressen eines virtuellen Netzwerks ein, für die ein Lastausgleich vorgenommen wird. Front-End-IP-Adressen und virtuelle Netzwerke werden nie direkt für einen Internetendpunkt verfügbar gemacht. Interne Branchenanwendungen werden in Azure ausgeführt. Auf sie wird aus Azure oder von lokalen Ressourcen aus zugegriffen. |
| Unterstützte SKUs | Basic, Standard | Basic, Standard |

![Beispiel für einen mehrstufigen Lastenausgleich](./media/load-balancer-overview/load-balancer.png)

## <a name="backend-pool"></a>Back-End-Pool

Die Gruppe virtueller Computer oder Instanzen in einer VM-Skalierungsgruppe, von denen die eingehende Anforderung verarbeitet wird. Für eine kosteneffiziente Skalierung zur Bewältigung großer Mengen an eingehendem Datenverkehr empfiehlt es sich in der Regel, dem Back-End-Pool weitere Instanzen hinzuzufügen.

Die Konfiguration des Lastenausgleichs wird automatisch angepasst, wenn Sie Instanzen hoch- oder herunterskalieren. Durch Hinzufügen virtueller Computer zum bzw. Entfernen virtueller Computer aus dem Back-End-Pool wird der Lastenausgleich ohne zusätzliche Vorgänge neu konfiguriert. Der Back-End-Pool wird für jeden beliebigen virtuellen Computer im virtuellen Netzwerk verwendet.

Halten Sie bei der Gestaltung Ihres Back-End-Pools die Anzahl der einzelnen Back-End-Pool-Ressourcen so gering wie möglich, um die Dauer von Verwaltungsvorgängen zu optimieren. Es gibt keinen Unterschied in der Datenebenenleistung oder -skalierung.

## <a name="health-probes"></a>Integritätstests

Mithilfe eines Integritätstest wird der Integritätsstatus der Instanzen im Back-End-Pool ermittelt. Wenn Sie einen Load Balancer erstellen, müssen Sie einen Integritätstest konfigurieren, mit dem Ihr Load Balancer ermitteln kann, ob eine Instanz fehlerfrei ist, um dann Datenverkehr an diese weiterzuleiten.

Sie können den gewünschten Fehlerschwellenwert für Ihre Integritätstests definieren. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhaften Instanzen. Ein Testfehler wirkt sich nicht auf vorhandene Verbindungen aus. Die Verbindung bleibt so lange bestehen, bis die Anwendung:

- den Flow beendet
- eine Leerlauftimeout auftritt
- der virtuelle Computer heruntergefahren wird

Load Balancer verfügt über verschiedene Integritätstesttypen für Endpunkte: TCP, HTTP und HTTPS.

Vom Load Balancer im Tarif „Basic“ werden keine HTTPS-Tests unterstützt. Vom Load Balancer im Tarif „Basic“ werden alle TCP-Verbindungen (einschließlich aktiver Verbindungen) beendet.

## <a name="load-balancing-rules"></a>Lastenausgleichsregeln

Mithilfe einer Lastenausgleichsregel wird definiert, wie eingehender Datenverkehr auf **alle** Instanzen innerhalb des Back-End-Pools verteilt werden soll. Eine Lastenausgleichsregel ordnet mehreren Back-End-IP-Adressen und Ports eine bestimmte Front-End-IP-Konfiguration und einen Port zu.

Wenn Sie z. B. Datenverkehr an Port 80 (oder einen anderen Port) Ihrer Front-End-IP-Adresse an Port 80 aller Ihrer Back-End-Instanzen umleiten möchten, verwenden Sie eine Lastenausgleichsregel, um dies zu erreichen.

## <a name="inbound-nat-rules"></a>Eingehende NAT-Regeln

Eine NAT-Regel für eingehenden Datenverkehr leitet eingehenden Datenverkehr, der an eine ausgewählte Kombination aus Front-End-IP-Adresse und -Port gesendet wird, an einen **bestimmten** virtuellen Computer oder eine bestimmte Instanz im Back-End-Pool weiter. Der Portweiterleitung liegt die gleiche hashbasierte Verteilung zugrunde wie dem Lastenausgleich.

Beispielsweise, wenn Sie RDP-Sitzungen (Remote Desktop Protocol, Remotedesktopprotokoll) oder SSH-Sitzungen (Secure Shell) zum Trennen von VM-Instanzen innerhalb eines Back-End-Pools verwenden möchten. Ports können mehrere interne Endpunkte unter derselben Front-End-IP-Adresse zugeordnet werden. Sie können die Front-End-IP-Adressen verwenden, um für Ihre VMs die Remoteverwaltung ohne zusätzliche Jumpbox durchzuführen.

## <a name="outbound-rules"></a>Ausgangsregeln

Eine Ausgangsregel konfiguriert die Netzwerkadressenübersetzung (Network Address Translation, NAT) für ausgehenden Datenverkehr für alle virtuellen Computer oder Instanzen, die vom Back-End-Pool identifiziert wurden. Dadurch können Instanzen im Back-End (ausgehend) mit dem Internet oder anderen Endpunkten kommunizieren.

Vom Lastenausgleich im Tarif „Basic“ werden keine Ausgangsregeln unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den ersten Schritten mit einer Load Balancer-Instanz finden Sie unter [Schnellstart: Erstellen einer Load Balancer Standard-Instanz für den Lastenausgleich virtueller Computer im Azure-Portal](quickstart-load-balancer-standard-public-portal.md).
- Weitere Informationen zu [Azure Load Balancer](load-balancer-overview.md).
- Informieren Sie sich über [öffentliche IP-Adressen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).
- Informieren Sie sich über [private IP-Adressen](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses).
- Informationen zu [Standard Load Balancer und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md)
- Weitere Informationen zu [Diagnosen für Standard Load Balancer](load-balancer-standard-diagnostics.md).
- Informationen zur [TCP-Zurücksetzung bei Leerlauf](load-balancer-tcp-reset.md).
- Informationen zu [Load Balancer Standard mit Lastenausgleichsregeln für HA-Ports](load-balancer-ha-ports-overview.md)
- Informationen zur Verwendung von [Load Balancer mit mehreren Front-End-IP-Konfigurationen](load-balancer-multivip-overview.md).
- Weitere Informationen zu [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).
- Informieren Sie sich über [Testtypen](load-balancer-custom-probe-overview.md#types).
- Lesen Sie weitere Informationen zu [Load Balancer-Grenzwerten](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#load-balancer).
- Erfahren Sie mehr über die Verwendung der [Portweiterleitung](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-port-forwarding-portal).
- Erfahren Sie mehr über [Load Balancer-Ausgangsregeln](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview).
