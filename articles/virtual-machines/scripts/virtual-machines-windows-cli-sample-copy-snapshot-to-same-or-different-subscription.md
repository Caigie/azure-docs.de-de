---
title: 'Kopieren der Momentaufnahme eines verwalteten Datenträgers in ein Abonnement: CLI-Beispiel, Windows-VM'
description: 'Azure CLI-Skriptbeispiel: Kopieren (Verschieben) der Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle auf einem virtuellen Windows-Computer'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 05b73bf62bfce456e65fde153379690bc9823e6a
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056736"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli-on-a-windows-vm"></a>Kopieren der Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement mithilfe der Befehlszeilenschnittstelle auf einem virtuellen Windows-Computer

Dieses Skript kopiert eine Momentaufnahme eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement. Verwenden Sie dieses Skript für folgende Szenarien:

1. Migrieren einer Momentaufnahme in Storage Premium (Premium_LRS) zu Storage Standard (Standard_LRS oder Standard_ZRS), um Ihre Kosten zu senken
1. Migrieren einer Momentaufnahme aus lokal redundantem Speicher (Premium_LRS, Standard_LRS) zu zonenredundantem Speicher (Standard_ZRS), um von der höheren Zuverlässigkeit von ZRS-Speicher zu profitieren
1. Verschieben einer Momentaufnahme in ein anderes Abonnement in der gleichen Region zur längeren Aufbewahrung

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen einer Momentaufnahme im Zielabonnement mit der ID der Quellmomentaufnahme. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az snapshot show](/cli/azure/snapshot) | Ruft alle Eigenschaften einer Momentaufnahme anhand der Eigenschaften für den Namen und die Ressourcengruppe der Momentaufnahme ab. Die ID-Eigenschaft wird verwendet, um die Momentaufnahme in ein anderes Abonnement zu kopieren.  |
| [az snapshot create](/cli/azure/snapshot) | Kopiert eine Momentaufnahme durch Erstellen einer Momentaufnahme in einem anderen Abonnement mit der ID und dem Namen der übergeordneten Momentaufnahme  |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
