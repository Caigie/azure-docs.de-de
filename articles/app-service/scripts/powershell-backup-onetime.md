---
title: 'Mit PowerShell: Sichern einer App'
description: Hier erfahren Sie, wie Sie mit Azure PowerShell die Bereitstellung und Verwaltung von App Service automatisieren. In diesem Beispiel wird gezeigt, wie Sie eine App sichern.
author: msangapu-msft
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: e17a7ffeb9d9f58e3acfdf4569554637baaaa4c9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004913"
---
# <a name="back-up-a-web-app-using-powershell"></a>Sichern einer Web-App mithilfe von PowerShell

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und anschließend eine einmalige Sicherung für sie. 

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen. 

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Erstellt ein Speicherkonto. |
| [New-AzStorageContainer](/powershell/module/az.storage/new-AzStoragecontainer) | Erstellt einen Azure-Speichercontainer. |
| [New-AzStorageContainerSASToken](/powershell/module/az.storage/new-AzStoragecontainersastoken) | Generiert ein SAS-Token für einen Azure-Speichercontainer.  |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Erstellt die Web-App. |
| [New-AzWebAppBackup](/powershell/module/az.websites/new-azwebappbackup) | Erstellt eine Sicherung für eine Web-App. |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Ruft eine Liste der Sicherungen für eine Web-App ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
