---
title: 'Erstellen eines verwalteten Datenträgers auf der Grundlage einer VHD-Datei in einem Speicherkonto in einem Abonnement: PowerShell-Beispiel'
description: 'Azure PowerShell-Beispielskript: Erstellen verwalteter Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement'
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
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: d18841da4102d83dd4703bc08abe57090d3a533a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010001"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Erstellen verwalteter Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement mithilfe von PowerShell

Dieses Skript erstellt einen verwalteten Datenträger aus einer VHD-Datei in einem Speicherkonto in demselben oder einem anderen Abonnement. Sie können dieses Skript verwenden, um eine bestimmte (nicht generalisierte/mit Sysprep vorbereitete) VHD auf den verwalteten Betriebssystem-Datenträger zu importieren, damit mit ihr ein virtueller Computer erstellt werden kann. Außerdem können Sie damit eine Daten-VHD auf verwaltete Datenträger importieren. 

Erstellen Sie nicht mehrere identische verwaltete Datenträger aus einer VHD-Datei in einem kurzen Zeitraum. Um verwaltete Datenträger aus einer VHD-Datei zu erstellen, wird eine Blob-Momentaufnahme der VHD-Datei erstellt und anschließend verwendet, um verwaltete Datenträger zu erstellen. Innerhalb einer Minute kann nur eine Blob-Momentaufnahme erstellt werden, die aufgrund der Drosselung Fehler bei der Datenträgererstellung auslösen kann. Erstellen Sie eine verwaltete Momentaufnahme aus der VHD-Datei, um diese Drosselung zu vermeiden (weitere Informationen finden Sie unter [Create a snapshot from a VHD](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)). Verwenden Sie anschließend die verwaltete Momentaufnahme, um in einer kurzen Zeitspanne mehrere verwaltete Datenträger zu erstellen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer VHD in verschiedenen Abonnements zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Dies umfasst den Speichertyp, den Speicherort, die Ressourcen-ID des Speicherkontos, in dem die übergeordnete VHD und VHD-URI der übergeordneter VHD-Datei gespeichert wird. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger mit PowerShell](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
