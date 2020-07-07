---
title: 'Azure-VPN Gateway: Konfigurieren von Paketerfassungen'
description: Hier erhalten Sie Informationen über Paketerfassungsfunktionen, die für VPN-Gateways verwendet werden können.
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/15/2019
ms.author: radwiv
ms.openlocfilehash: 6edfe0228ce4cbe21ad4ae0eb8b7316a92f1da31
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987154"
---
# <a name="configure-packet-captures-for-vpn-gateways"></a>Konfigurieren der Paketerfassung für VPN-Gateways

Probleme in Bezug auf die Konnektivität und Leistung sind häufig komplex, und erfordert viel Zeit, um die Ursache des Problems einzugrenzen. Die Paketerfassung trägt wesentlich dazu bei, die für die Ursachensuche benötigte Zeit zu verkürzen, indem das Problem auf bestimmte Teile des Netzwerks eingeschränkt wird. So wird z. B. ermittelt, ob das Problem auf der Kundenseite des Netzwerks, auf der Azure-Seite des Netzwerks oder irgendwo dazwischen liegt. Sobald das Problem eingegrenzt wurde, ist es einfacher, ein effizientes Debugging und Maßnahmen zur Problembeseitigung durchzuführen.

Für die Paketerfassung stehen verschiedene gängige Tools zur Verfügung. Eine relevante Paketerfassung ist mit diesen Tools jedoch oft sehr umständlich, insbesondere bei der Arbeit in Szenarien mit hohem Datenaufkommen. Die von einer Paketerfassung für VPN-Gateways bereitgestellten Filterfunktionen werden zu einem wichtigen Unterscheidungsmerkmal. Sie können zusätzlich zu den gängigen Tools für die Paketerfassung eine VPN-Gateway-Paketerfassung nutzen.

## <a name="vpn-gateway-packet-capture-filtering-capabilities"></a>Filterfunktionen für die VPN-Gateway-Paketerfassung

VPN-Gateway-Paketerfassungen können je nach Kundenanforderungen für das Gateway oder eine bestimmte Verbindung ausgeführt werden. Es ist außerdem möglich, Paketerfassungen für mehrere Tunnel gleichzeitig auszuführen. Sie können uni- oder bidirektionalen Datenverkehr, IKE- und ESP-Datenverkehr und interne Pakete gemeinsam mit einer Filterung für ein VPN-Gateway erfassen.

Die Verwendung von 5-Tupel-Filtern (Quellsubnetz, Zielsubnetz, Quellport, Zielport, Protokoll) und TCP-Flags (SYN, ACK, FIN, URG, PSH, RST) ist nützlich, um eine Problemisolierung bei einem hohen Datenverkehrsaufkommen durchzuführen.

Während der Ausführung der Paketerfassung können Sie nur eine Option pro Eigenschaft verwenden.

## <a name="setup-packet-capture-using-powershell"></a>Einrichten der Paketerfassung mithilfe von PowerShell

Nachfolgend finden Sie Beispiele für PowerShell-Befehle zum Starten und Beenden von Paketerfassungen. Weitere Informationen zu den Parameteroptionen (z. B. zum Erstellen von eines Filters) finden Sie in diesem PowerShell-[Dokument](https://docs.microsoft.com/powershell/module/az.network/start-azvirtualnetworkgatewaypacketcapture).

### <a name="start-packet-capture-for-a-vpn-gateway"></a>Starten der Paketerfassung für ein VPN-Gateway

```azurepowershell-interactive
Start-AzVirtualnetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName"
```

Der optionale Parameter **-FilterData** kann verwendet werden, um einen Filter anzuwenden.

### <a name="stop-packet-capture-for-a-vpn-gateway"></a>Beenden der Paketerfassung für ein VPN-Gateway

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName" -SasUrl "YourSASURL"
```

### <a name="start-packet-capture-for-a-vpn-gateway-connection"></a>Starten der Paketerfassung für eine VPN-Gatewayverbindung

```azurepowershell-interactive
Start-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName"
```

Der optionale Parameter **-FilterData** kann verwendet werden, um einen Filter anzuwenden.

### <a name="stop-packet-capture-on-a-vpn-gateway-connection"></a>Beenden der Paketerfassung für eine VPN-Gatewayverbindung

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName" -SasUrl "YourSASURL"
```

## <a name="key-considerations"></a>Wichtige Aspekte

- Das Ausführen von Paketerfassungen kann sich auf die Leistung auswirken. Denken Sie daran, die Paketerfassung zu beenden, wenn sie nicht benötigt wird.
- Die empfohlene Mindestdauer für die Paketerfassung beträgt 600 Sekunden. Eine kürzere Paketerfassung führt möglicherweise zu unvollständigen Daten aufgrund von Synchronisierungsproblemen zwischen verschiedenen Komponenten im Pfad.
- Bei der Paketerfassung werden Datendateien im PCAP-Format generiert. Verwenden Sie Wireshark oder andere allgemein verfügbare Anwendungen, um PCAP-Dateien zu öffnen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VPN-Gateways finden Sie unter [Was ist VPN Gateway?](vpn-gateway-about-vpngateways.md).
