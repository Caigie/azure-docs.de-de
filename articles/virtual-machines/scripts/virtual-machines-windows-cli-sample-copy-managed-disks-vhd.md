---
title: 'Kopieren eines verwalteten Datenträgers in ein Speicherkonto: Windows-CLI-Beispiel'
description: 'Azure CLI-Beispiel: Exportieren oder Kopieren eines verwalteten Datenträgers in ein Speicherkonto'
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
ms.date: 09/17/2018
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: 17ca58a8c03def3565bf7e38099d84cf767d333c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010092"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Exportieren/Kopieren eines verwalteten Datenträgers in ein Speicherkonto per Azure CLI

Mit diesem Skript wird die zugrunde liegende VHD eines verwalteten Datenträgers in ein Speicherkonto in derselben oder einer anderen Region exportiert. Zuerst wird der SAS-URI des verwalteten Datenträgers generiert und anschließend verwendet, um die VHD in ein Speicherkonto zu kopieren. Verwenden Sie dieses Skript, um Ihre verwalteten Datenträger für regionale Erweiterungen zu kopieren.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für einen verwalteten Datenträger und zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto mithilfe des SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az disk grant-access](/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Generiert eine schreibgeschützte Shared Access Signature (SAS), die verwendet wird, um die zugrunde liegende VHD-Datei in ein Speicherkonto zu kopieren oder lokal herunterzuladen  |
| [az storage blob copy start](/cli/azure/storage/blob/copy) | Kopiert ein Blob asynchron aus einem Speicherkonto in ein anderes |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
