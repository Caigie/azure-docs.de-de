---
title: 'Erstellen eines virtuellen Computers durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger (Windows): PowerShell'
description: 'Azure PowerShell-Skriptbeispiel: Erstellen eines virtuellen Computers durch Anfügen eines verwalteten Datenträgers als Betriebssystemdatenträger'
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8001990d4ade9986bea81f63b60832ed69024265
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509555"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell-windows"></a>Erstellen eines virtuellen Computers mit einem vorhandenen verwalteten Betriebssystemdatenträger mit PowerShell (Windows)

Dieses Skript erstellt einen virtuellen Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger. Verwenden Sie dieses Skript in den vorherigen Szenarien:
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Betriebssystemdatenträger, der von einem verwalteten Datenträger in einem anderen Abonnement kopiert wurde
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Datenträger, der aus einer spezialisierten VHD-Datei erstellt wurde 
* Erstellen eines virtuellen Computers aus einem vorhandenen verwalteten Betriebssystemdatenträger, der aus einer Momentaufnahme erstellt wurde 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Eigenschaften eines verwalteten Datenträgers abzurufen, einen verwalteten Datenträger an einen neuen virtuellen Computer anzufügen und einen virtuellen Computer zu erstellen. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzDisk](/powershell/module/az.compute/get-azdisk) | Ruft das Datenträgerobjekt basierend auf dem Namen und der Ressourcengruppe eines Datenträgers ab. Die ID-Eigenschaft des zurückgegebenen Datenträgerobjekts wird verwendet, um den Datenträger an einen neuen virtuellen Computer anzufügen. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Erstellt eine VM-Konfiguration. Diese Konfiguration umfasst Informationen wie VM-Name, Betriebssystem und Administratoranmeldeinformationen. Die Konfiguration wird während der VM-Erstellung verwendet. |
| [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) | Fügt einen verwalteten Datenträger mit der ID-Eigenschaft des Datenträgers als Betriebssystemdatenträger an einen neuen virtuellen Computer an. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Erstellt eine öffentliche IP-Adresse. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Erstellt eine Netzwerkschnittstelle. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Erstellen Sie eine VM. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

Verwenden Sie für Marketplace-Images [Set-AzVMPlan](/powershell/module/az.compute/set-azvmplan), um die Planinformationen festzulegen.

```powershell
Set-AzVMPlan -VM $VirtualMachine -Publisher $Publisher -Product $Product -Name $Bame
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
