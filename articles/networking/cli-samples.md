---
title: Azure CLI-Beispiele – Netzwerke
description: Azure CLI-Beispiele
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 397f2999821e97a1f12362e2aa2fa20b20e78596
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232405"
---
# <a name="azure-cli-samples-for-networking"></a>Azure CLI-Beispiele für Netzwerke

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| Skript | BESCHREIBUNG |
|-|-|
|**Konnektivität zwischen Azure-Ressourcen**||
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr am Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr zum Back-End-Subnetz auf MySQL und Port 3306 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP, HTTPS und SSH beschränkt. Aus dem Back-End-Subnetz ausgehender Datenverkehr an das Internet ist nicht zulässig. |
|**Lastenausgleich und Datenverkehrsrichtung**||
| [Lastenausgleich für mehrere Websites auf VMs](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Erstellt zwei VMs mit verschiedenen IP-Konfigurationen, die zu einer über Azure Load Balancer aufrufbaren Verfügbarkeitsgruppe hinzugefügt werden. |
| [Weiterleiten von Datenverkehr über mehrere Regionen hinweg für hohe Anwendungsverfügbarkeit](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Erstellt zwei App Service-Pläne, zwei Web-Apps, ein Traffic Manager-Profil und zwei Traffic Manager-Endpunkte. |
| | |
