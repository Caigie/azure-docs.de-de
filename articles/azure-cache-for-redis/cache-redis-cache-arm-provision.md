---
title: Bereitstellen von Azure Cache for Redis mit Azure Resource Manager
description: Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage verwenden, um eine Azure Cache for Redis-Ressource bereitzustellen. Vorlagen werden für gängige Szenarien bereitgestellt.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 01/23/2017
ms.openlocfilehash: 0dbcbd173ce0a7c4c6a123f0644b870aa3cec2f5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85829889"
---
# <a name="create-an-azure-cache-for-redis-using-a-template"></a>Erstellen eines Azure Cache for Redis mithilfe einer Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In diesem Thema erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, die einen Azure Cache for Redis bereitstellt. Der Cache kann mit einem vorhandenen Speicherkonto verwendet werden, um Diagnosedaten aufzunehmen. Sie erfahren darüber hinaus, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Derzeit werden für alle Caches in derselben Region für ein Abonnement dieselben Diagnoseeinstellungen verwendet. Ein Aktualisieren eines Cache in der Region wirkt sich auf alle anderen Caches in der Region aus.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md). Weitere Informationen zur JSON-Syntax und den Eigenschaften für Cacheressourcentypen finden Sie unter [Microsoft.Cache resource types](/azure/templates/microsoft.cache/allversions) (Microsoft.Cache-Ressourcentypen).

Die vollständige Vorlage finden Sie unter [Azure Cache for Redis-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager-Vorlagen für den neuen [Tarif „Premium“](cache-premium-tier-intro.md) sind verfügbar. 
> 
> * [Erstellen eines Azure Cache for Redis vom Typ „Premium“ mit Clustering](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
> * [Erstellen eines Azure Cache for Redis vom Typ „Premium“ mit Datenpersistenz](https://azure.microsoft.com/resources/templates/201-redis-premium-persistence/)
> * [Erstellen eines in einem virtuellen Netzwerk bereitgestellten Premium Redis Caches](https://azure.microsoft.com/resources/templates/201-redis-premium-vnet/)
> 
> Um die neuesten Vorlagen zu ermitteln, suchen Sie in [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/) nach `Azure Cache for Redis`.
> 
> 

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen
In dieser Vorlage stellen Sie einen Azure Cache for Redis bereit, der ein vorhandenes Speicherkonto für Diagnosedaten verwendet.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter
Mit Azure Resource Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens "Parameters", der alle Parameterwerte enthält.
Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
Der Speicherort des Azure Cache for Redis. Um eine optimale Leistung zu erzielen, verwenden Sie den gleichen Speicherort wie für die App, die den Cache nutzen wird.

```json
  "redisCacheLocation": {
    "type": "string"
  }
```

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
Der Name des vorhandenen Speicherkontos, das für Diagnosen verwendet werden soll. 

```json
  "existingDiagnosticsStorageAccountName": {
    "type": "string"
  }
```

### <a name="enablenonsslport"></a>enableNonSslPort
Ein boolescher Wert, der angibt, ob Zugriff über Nicht-SSL-Ports zugelassen werden soll.

```json
  "enableNonSslPort": {
    "type": "bool"
  }
```

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Ein Wert, der angibt, ob Diagnosen aktiviert sind. Verwenden Sie ON oder OFF.

```json
  "diagnosticsStatus": {
    "type": "string",
    "defaultValue": "ON",
    "allowedValues": [
          "ON",
          "OFF"
      ]
  }
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
### <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Erstellt den Azure Cache for Redis.

```json
  {
    "apiVersion": "2015-08-01",
    "name": "[parameters('redisCacheName')]",
    "type": "Microsoft.Cache/Redis",
    "location": "[parameters('redisCacheLocation')]",
    "properties": {
      "enableNonSslPort": "[parameters('enableNonSslPort')]",
      "sku": {
        "capacity": "[parameters('redisCacheCapacity')]",
        "family": "[parameters('redisCacheFamily')]",
        "name": "[parameters('redisCacheSKU')]"
      }
    },
    "resources": [
      {
        "apiVersion": "2017-05-01-preview",
        "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
        "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
        "location": "[parameters('redisCacheLocation')]",
        "dependsOn": [
          "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
        ],
        "properties": {
          "status": "[parameters('diagnosticsStatus')]",
          "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
        }
      }
    ]
  }
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```azurepowershell
    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup
```
