---
title: 'Kopieren von verwalteten Datenträgern in ein Abonnement (Windows): PowerShell-Beispiel'
description: 'Azure PowerShell-Skriptbeispiel: Kopieren oder Verschieben verwalteter Datenträger in das gleiche oder ein anderes Abonnement'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 6f191eaf5c0a02f77d267999c307d34e083b6cdf
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87069187"
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell-windows"></a>Kopieren von verwalteten Datenträgern in das gleiche oder ein anderes Abonnement mit PowerShell (Windows)

Dieses Skript erstellt eine Kopie eines vorhandenen verwalteten Datenträgers im gleichen Abonnement oder einem anderen Abonnement. Der neue Datenträger wird in der gleichen Region wie der übergeordnete verwaltete Datenträger erstellt.   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen eines neuen verwalteten Datenträgers im Zielabonnement mit der ID des verwalteten Quelldatenträgers. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Enthält die Ressourcen-ID des übergeordneten Datenträgers und des Speicherorts, der mit dem Speicherort des übergeordneten Datenträgers übereinstimmt.  |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |


## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
