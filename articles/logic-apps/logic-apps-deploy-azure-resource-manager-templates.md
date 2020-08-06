---
title: Bereitstellen von Logik-App-Vorlagen
description: Erfahren Sie, wie Sie Azure Resource Manager-Vorlagen bereitstellen, die für Azure Logic Apps erstellt wurden.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/01/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: d3ef4275e5b309bb499338fe90c0f527aeaeb71f
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501507"
---
# <a name="deploy-azure-resource-manager-templates-for-azure-logic-apps"></a>Bereitstellen von Azure Resource Manager-Vorlagen für Azure Logic Apps

Nachdem Sie eine Azure Resource Manager-Vorlage für Ihre Logik-App erstellt haben, können Sie Ihre Vorlage folgendermaßen bereitstellen:

* [Azure portal](#portal)
* [Visual Studio](#visual-studio)
* [Azure PowerShell](#powershell)
* [Azure-Befehlszeilenschnittstelle](#cli)
* [Azure-Ressourcen-Manager-REST-API](../azure-resource-manager/templates/deploy-rest.md)
* [Azure DevOps](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Bereitstellen über das Azure-Portal

Um eine Logik-App-Vorlage automatisch in Azure bereitzustellen, können Sie die folgende Schaltfläche **In Azure bereitstellen** auswählen. Darüber werden Sie im Azure-Portal angemeldet und zur Eingabe von Informationen zu Ihrer Logik-App aufgefordert. Sie können dann alle notwendigen Änderungen an der Logik-App-Vorlage oder den Parametern vornehmen.

[![In Azure bereitstellen](./media/logic-apps-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Sie werden beispielsweise nach der Anmeldung beim Azure-Portal zur Eingabe der folgenden Informationen aufgefordert:

* Name des Azure-Abonnements
* Gewünschte Ressourcengruppe
* Speicherort der Logik-App
* Der Name Ihrer Logik-App
* Test-URI
* Annahme der angegebenen Bedingungen

Weitere Informationen finden Sie in den folgenden Themen:

* [Übersicht: Automatisieren der Bereitstellung für Azure Logik-Apps mit Azure Resource Manager-Vorlagen](logic-apps-azure-resource-manager-templates-overview.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und dem Azure-Portal](../azure-resource-manager/templates/deploy-portal.md)

<a name="visual-studio"></a>

## <a name="deploy-with-visual-studio"></a>Bereitstellen mit Visual Studio 2013

Um eine Logik-App-Vorlage aus einem Azure-Ressourcengruppenprojekt bereitzustellen, das Sie mithilfe von Visual Studio erstellt haben, befolgen Sie diese [Schritte zum manuellen Bereitstellen Ihrer Logik-App](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure) in Azure.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Bereitstellen mit Azure PowerShell

Für die Bereitstellung in einer bestimmten *Azure-Ressourcengruppe* verwenden Sie den folgenden Befehl:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Weitere Informationen finden Sie in den folgenden Themen:

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md)
* [`New-AzResourceGroupDeployment`](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Für die Bereitstellung in einer bestimmten *Azure-Ressourcengruppe* verwenden Sie den folgenden Befehl:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Weitere Informationen finden Sie in den folgenden Themen:

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](../azure-resource-manager/templates/deploy-cli.md)
* [`az group deployment create`](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Bereitstellen mit Azure DevOps

Um Logik-App-Vorlagen bereitzustellen und Umgebungen zu verwalten, verwenden Teams häufig ein Tool wie [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) in [Azure DevOps](/azure/devops/user-guide/what-is-azure-devops-services). Azure Pipelines enthält eine [Aufgabe für die Bereitstellung von Azure-Ressourcengruppen](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2), die jeder Build- oder Releasepipeline hinzugefügt werden kann. Für die Autorisierung zur Bereitstellung und Generierung der Releasepipeline benötigen Sie außerdem ein Azure Active Directory (AD)-[Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md). Weitere Informationen zum [Verwenden von Dienstprinzipalen mit Azure Pipelines](/azure/devops/pipelines/library/connect-to-azure).

Weitere Informationen zu Continuous Integration und Continuous Deployment (CI/CD) für Azure Resource Manager-Vorlagen mit Azure Pipelines finden Sie in den folgenden Themen und Beispielen:

* [Integrieren von Resource Manager-Vorlagen in Azure Pipelines](../azure-resource-manager/templates/add-template-to-azure-pipelines.md)
* [Tutorial: Continuous Integration von Azure Resource Manager-Vorlagen mit Azure Pipelines](../azure-resource-manager/templates/deployment-tutorial-pipeline.md).
* [Beispiel: Herstellen einer Verbindung mit Azure Service Bus-Warteschlangen aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Herstellen einer Verbindung mit Azure-Speicherkonten aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Einrichten einer Funktions-App-Aktion für Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Beispiel: Herstellen einer Verbindung mit einem Integrationskonto aus Azure Logic Apps und Bereitstellen mit Azure Pipelines in Azure DevOps](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)
* [Beispiel: Orchestrieren von Azure Pipelines mithilfe von Azure Logic Apps](/samples/azure-samples/azure-logic-apps-pipeline-orchestration/azure-devops-orchestration-with-logic-apps/)

Hier finden Sie die allgemeinen Schritte für die Verwendung von Azure Pipelines:

1. Erstellen Sie in Azure Pipelines eine leere Pipeline.

1. Wählen Sie die Ressourcen, die Sie für die Pipeline benötigen, wie z.B. Ihre Logik-App-Vorlage und Ihre Parameterdateien für die Vorlagen, die Sie manuell oder im Rahmen des Build-Prozesses generieren.

1. Suchen Sie die Aufgabe **Bereitstellung einer Azure-Ressourcengruppe** und fügen Sie sie zum Agent-Auftrag hinzu.

   ![Hinzufügen der Aufgabe „Bereitstellung einer Azure-Ressourcengruppe“](./media/logic-apps-deploy-azure-resource-manager-templates/add-azure-resource-group-deployment-task.png)

1. Führen Sie die Konfiguration mit einem [Dienstprinzipal](/azure/devops/pipelines/library/connect-to-azure) durch.

1. Fügen Sie Verweise auf Ihre Logik-App-Vorlage und Parameterdateien für die Vorlage hinzu.

1. Erstellen Sie nach Bedarf weitere Schritte im Freigabeprozess für andere Umgebungen, automatisierte Tests oder genehmigende Personen.

<a name="authorize-oauth-connections"></a>

## <a name="authorize-oauth-connections"></a>Autorisieren von OAuth-Verbindungen

Nach der Bereitstellung funktioniert Ihre Logik-App vollständig mit gültigen Parametern. Allerdings müssen Sie weiterhin jede OAuth-Verbindung autorisieren, um gültige Zugriffstoken zum [Authentifizieren Ihrer Anmeldeinformationen](../active-directory/develop/authentication-vs-authorization.md) zu generieren. Sie können OAuth-Verbindungen folgendermaßen autorisieren:

* Zur automatisierten Bereitstellung können Sie ein Skript verwenden, das Zustimmung für jede OAuth-Verbindung enthält. Ein Beispielskript hierfür finden Sie auf GitHub im Projekt [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth).

* Um OAuth-Verbindungen manuell zu autorisieren, öffnen Sie Ihre Logik-App über das Azure-Portal oder in Visual Studio im Logik-App-Designer. Autorisieren Sie im Designer alle erforderlichen Verbindungen.

Wenn Sie zum Autorisieren von Verbindungen stattdessen einen Azure AD-[Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) verwenden, informieren Sie sich darüber, wie Sie [Dienstprinzipalparameter in Ihrer Logik-App-Vorlage angeben](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überwachen von Logik-Apps](../logic-apps/monitor-logic-apps.md)
