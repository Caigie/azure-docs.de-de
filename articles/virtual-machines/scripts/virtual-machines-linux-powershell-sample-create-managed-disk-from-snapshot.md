---
title: 'Erstellen eines verwalteten Datenträgers auf der Grundlage einer Momentaufnahme (Linux): PowerShell-Beispiel'
description: Azure PowerShell-Skriptbeispiel – Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 2faa98ec21d260e6a7cbdf9eceba24baf4b13c61
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501132"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell-linux"></a>Erstellen eines verwalteten Datenträgers auf der Grundlage einer Momentaufnahme mithilfe von PowerShell (Linux)

Dieses Skript erstellt einen verwalteten Datenträger aus einer Momentaufnahme. Sie können es zum Wiederherstellen eines virtuellen Computers aus Momentaufnahmen von Betriebssystem und Datenträgern verwenden. Erstellen Sie verwaltete Datenträger für Betriebssystem und Daten aus den jeweiligen Momentaufnahmen und anschließend einen neuen virtuellen Computer durch Anfügen der verwalteten Datenträger. Sie können auch die Datenträger eines vorhandenen virtuellen Computers wiederherstellen, indem Sie die aus Momentaufnahmen erstellten Datenträger anfügen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um einen verwalteten Datenträger aus einer Momentaufnahme zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | Ruft Eigenschaften von Momentaufnahmen ab.  |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Erstellt die Datenträgerkonfiguration, die für die Datenträgererstellung verwendet wird. Enthält die Ressourcen-ID der übergeordneten Momentaufnahme, des Speicherorts, der mit dem Speicherort der übergeordneten Momentaufnahme übereinstimmt, sowie des Speichertyp.  |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Erstellt einen Datenträger mit Datenträgerkonfiguration, Datenträgername und Name der Ressourcengruppe, die als Parameter übergeben werden. |

## <a name="next-steps"></a>Nächste Schritte


[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
