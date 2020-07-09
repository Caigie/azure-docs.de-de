---
title: 'Azure Virtual WAN: Benutzer-VPN-Clientprofile'
description: Diese Informationen unterstützen Sie bei der Arbeit mit der Clientprofildatei.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/18/2020
ms.author: cherylmc
ms.openlocfilehash: c64e7988094612077131029547682c7ae3d25c98
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753142"
---
# <a name="working-with-user-vpn-client-profiles"></a>Arbeiten mit Benutzer-VPN-Clientprofilen

Die heruntergeladene Profildatei enthält Informationen, die zum Konfigurieren einer VPN-Verbindung erforderlich sind. Dieser Artikel hilft Ihnen beim Abrufen und Verstehen der Informationen, die für ein Benutzer-VPN-Clientprofil erforderlich sind.

[!INCLUDE [client profiles](../../includes/vpn-gateway-vwan-vpn-profile-download.md)]

* Der Ordner **OpenVPN** enthält das Profil *ovpn*, das geändert werden muss, um den Schlüssel und das Zertifikat einzuschließen. Weitere Informationen finden Sie unter [Konfigurieren von OpenVPN-Clients für Azure VPN Gateway](../virtual-wan/howto-openvpn-clients.md#windows).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Virtual WAN-Benutzer-VPN finden Sie unter [Tutorial: Erstellen einer Benutzer-VPN-Verbindung per Azure Virtual WAN](virtual-wan-point-to-site-portal.md).
