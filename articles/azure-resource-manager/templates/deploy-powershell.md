---
title: Bereitstellen von Ressourcen mit PowerShell und Vorlagen
description: Verwenden Sie Azure Resource Manager und Azure PowerShell, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
ms.topic: conceptual
ms.date: 06/04/2020
ms.openlocfilehash: af255e0248c029f42c9c2999ae7c0389d60c58fc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84431834"
---
# <a name="deploy-resources-with-arm-templates-and-azure-powershell"></a>Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell

Erfahren Sie, wie Ihre Ressourcen mit Azure PowerShell und Azure Resource Manager-Vorlagen (ARM) in Azure bereitgestellt werden. Weitere Informationen zu den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen finden Sie unter [Übersicht über die Vorlagenbereitstellung](overview.md).

## <a name="deployment-scope"></a>Bereitstellungsumfang

Sie können als Ziel für Ihre Bereitstellung eine Ressourcengruppe, ein Abonnement, eine Verwaltungsgruppe oder einen Mandanten verwenden. In den meisten Fällen wählen Sie die Bereitstellung in einer Ressourcengruppe aus. Verwenden Sie zum Anwenden von Richtlinien und Rollenzuweisungen in einem größeren Rahmen Abonnement-, Verwaltungsgruppen- oder Mandantenbereitstellungen. Bei Bereitstellungen in einem Abonnement können Sie eine Ressourcengruppe erstellen und ihr Ressourcen bereitstellen.

Abhängig vom Umfang der Bereitstellung verwenden Sie unterschiedliche Befehle.

* Verwenden Sie [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment), um eine **Ressourcengruppe** bereitzustellen:

  ```azurepowershell
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
  ```

* Verwenden Sie „New-AzSubscriptionDeployment“, um ein **Abonnement** bereitzustellen:

  ```azurepowershell
  New-AzSubscriptionDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](deploy-to-subscription.md).

* Verwenden Sie zum Bereitstellen in einer **Verwaltungsgruppe** das Cmdlet [New-AzManagementGroupDeployment](/powershell/module/az.resources/New-AzManagementGroupDeployment).

  ```azurepowershell
  New-AzManagementGroupDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Verwaltungsgruppenebene finden Sie unter [Erstellen von Ressourcen auf der Verwaltungsgruppenebene](deploy-to-management-group.md).

* Für die Bereitstellung in einem **Mandanten** verwenden Sie [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment).

  ```azurepowershell
  New-AzTenantDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Weitere Informationen zu Bereitstellungen auf Mandantenebene finden Sie unter [Erstellen von Ressourcen auf der Mandantenebene](deploy-to-tenant.md).

Die Beispiele in diesem Artikel verwenden Ressourcengruppenbereitstellungen.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen eine Vorlage für die Bereitstellung. Wenn Sie noch keine besitzen, laden Sie eine [Beispielvorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) aus dem Repository der Azure-Schnellstartvorlagen herunter, und speichern Sie diese. In diesem Artikel wird der lokale Dateiname **c:\MyTemplates\azuredeploy.json** verwendet.

Sofern Sie nicht Azure Cloud Shell zum Bereitstellen von Vorlagen verwenden, müssen Sie Azure PowerShell installieren und eine Verbindung mit Azure herstellen:

- **Installieren Sie Azure PowerShell-Cmdlets auf Ihrem lokalen Computer.** Weitere Informationen finden Sie unter [Erste Schritte mit Azure PowerShell](/powershell/azure/get-started-azureps).
- **Stellen Sie mithilfe von [Connect-AZAccount](/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Azure her**. Wenn Sie über mehrere Azure-Abonnements verfügen, müssen Sie möglicherweise auch [Set-AzContext](/powershell/module/Az.Accounts/Set-AzContext) ausführen. Weitere Informationen finden Sie unter [Verwenden mehrerer Azure-Abonnements](/powershell/azure/manage-subscriptions-azureps).

## <a name="deploy-local-template"></a>Bereitstellen einer lokalen Vorlage

Im folgenden Beispiel wird eine Ressourcengruppe erstellt und eine Vorlage von Ihrem lokalen Computer aus bereitgestellt. Der Name einer Ressourcengruppe darf nur alphanumerische Zeichen, Punkte, Unterstriche, Bindestriche und Klammern enthalten. Der Name kann bis zu 90 Zeichen umfassen. Der Name darf nicht mit einem Punkt enden.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

Die Bereitstellung kann einige Minuten dauern.

## <a name="deploy-remote-template"></a>Bereitstellen einer Remotevorlage

Anstatt ARM-Vorlagen auf dem lokalen Computer zu speichern, könnten Sie sie auch an einem externen Speicherort speichern. Sie können Vorlagen in einem Quellcodeverwaltungs-Repository (z.B. GitHub) speichern. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie sie auch in einem Azure-Speicherkonto speichern.

Verwenden Sie zum Bereitstellen einer externen Vorlage den **TemplateUri**-Parameter. Verwenden Sie den URI im Beispiel, um die Beispielvorlage aus GitHub bereitzustellen.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Das obige Beispiel erfordert einen URI mit öffentlichem Zugriff für die Vorlage, was in den meisten Szenarien funktioniert, da die Vorlage keine vertraulichen Daten enthalten sollte. Wenn Sie vertrauliche Daten (z.B. ein Administratorkennwort) angeben müssen, übergeben Sie diesen Wert als sicheren Parameter. Wenn Sie jedoch keinen öffentlichen Zugriff auf Ihre Vorlage wünschen, können Sie sie schützen, indem Sie sie in einem privaten Speichercontainer speichern. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token (Shared Access Signature) erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](secure-template-with-sas-token.md). Ein entsprechendes Tutorial finden Sie unter [Tutorial: Integrieren von Azure Key Vault in Ihre Bereitstellung einer ARM-Vorlage](template-tutorial-use-key-vault.md).

## <a name="preview-changes"></a>Vorschau der Änderungen

Vor dem Bereitstellen der Vorlage können Sie die Änderungen, die von der Vorlage an Ihrer Umgebung vorgenommen werden, in der Vorschau anzeigen. Überprüfen Sie anhand des [„Was-wäre-wenn“-Vorgangs](template-deploy-what-if.md), ob die Vorlage die erwarteten Änderungen vornimmt. „Was-wäre-wenn“ überprüft auch die Vorlage auf Fehler.

## <a name="deploy-from-azure-cloud-shell"></a>Bereitstellen über Azure Cloud Shell

Sie können Ihre Vorlage mithilfe von [Azure Cloud Shell](https://shell.azure.com) bereitstellen. Geben Sie zum Bereitstellen einer externen Vorlage den URI der Vorlage an. Um eine lokale Vorlage bereitzustellen, müssen Sie die Vorlage zuerst in das Speicherkonto für Ihre Cloud Shell-Instanz laden. Wählen Sie zum Hochladen von Dateien in die Shell das Menüsymbol **Dateien hochladen/herunterladen** im Shell-Fenster aus.

Zum Öffnen der Cloud Shell navigieren Sie zu [https://shell.azure.com](https://shell.azure.com), oder wählen Sie aus dem folgenden Codeabschnitt **Try-It** aus:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Um den Code in die Shell einzufügen, klicken Sie mit der rechten Maustaste in der Shell und wählen dann **Einfügen** aus.

## <a name="pass-parameter-values"></a>Übergeben von Parameterwerten

Zum Übergeben von Parameterwerten können Sie entweder Inlineparameter oder eine Parameterdatei verwenden.

### <a name="inline-parameters"></a>Inlineparameter

Geben Sie zum Übergeben von Inlineparametern die Parameternamen mit dem Befehl `New-AzResourceGroupDeployment` an. Wenn Sie beispielsweise eine Zeichenfolge und ein Array an eine Vorlage in einer Vorlage übergeben möchten, verwenden Sie Folgendes:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

Sie können auch den Inhalt einer Datei abrufen und als Inlineparameter übergeben.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Das Abrufen eines Parameterwerts aus einer Datei ist praktisch, wenn Sie Konfigurationswerte angeben müssen. Sie können beispielsweise [cloud-init-Werte für einen virtuellen Linux-Computer](../../virtual-machines/linux/using-cloud-init.md) angeben.

Wenn Sie ein Array von Objekten übergeben müssen, erstellen Sie Hashtabellen in PowerShell, und fügen Sie sie einem Array hinzu. Übergeben Sie dieses Array während der Bereitstellung als Parameter.

```powershell
$hash1 = @{ Name = "firstSubnet"; AddressPrefix = "10.0.0.0/24"}
$hash2 = @{ Name = "secondSubnet"; AddressPrefix = "10.0.1.0/24"}
$subnetArray = $hash1, $hash2
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleArray $subnetArray
```

### <a name="parameter-files"></a>Parameterdateien

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. Bei der Parameterdatei kann es sich um eine lokale Datei oder eine externe Datei mit einem erreichbaren URI handeln.

Weitere Informationen zur Parameterdatei finden Sie unter [Erstellen einer Resource Manager-Parameterdatei](parameter-files.md).

Verwenden Sie den **TemplateParameterFile**-Parameter, um eine lokale Parameterdatei zu übergeben:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Um eine externe Parameterdatei zu übergeben, verwenden Sie den **TemplateParameterUri**-Parameter:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Rollback zu einer erfolgreiche Bereitstellung, wenn ein Fehler auftritt, finden Sie unter [Rollback bei Fehler zu erfolgreicher Bereitstellung](rollback-on-error.md).
- Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
- Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).
- Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](secure-template-with-sas-token.md).
