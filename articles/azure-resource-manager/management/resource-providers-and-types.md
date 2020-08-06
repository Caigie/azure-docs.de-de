---
title: Ressourcenanbieter und Ressourcentypen
description: Beschreibt die Ressourcenanbieter, die den Ressourcen-Manager, die zugehörigen Schemas und verfügbaren API-Versionen sowie die Regionen unterstützen, die die Ressourcen hosten können.
ms.topic: conceptual
ms.date: 08/29/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 581b653c6d4769f7777b0ca56f136d25443c1ae4
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87500009"
---
# <a name="azure-resource-providers-and-types"></a>Azure-Ressourcenanbieter und -typen

Beim Bereitstellen von Ressourcen müssen Sie häufig Informationen zu den Ressourcenanbietern und -typen abrufen. Wenn Sie beispielsweise Schlüssel und Geheimnisse speichern möchten, verwenden Sie den Ressourcenanbieter „Microsoft.KeyVault“. Dieser Ressourcenanbieter bietet einen Ressourcentyp mit dem Namen „vaults“ zum Erstellen des Schlüsseltresors an.

Der Name eines Ressourcentyps hat folgendes Format: **{Ressourcenanbieter}/{Ressourcentyp}** . Der Ressourcentyp für einen Schlüsseltresor ist **Microsoft.KeyVault/vaults**.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Anzeigen aller Ressourcenanbieter in Azure
* Überprüfen des Registrierungsstatus eines Ressourcenanbieters
* Registrieren eines Ressourcenanbieters
* Anzeigen von Ressourcentypen für einen Ressourcenanbieter
* Anzeigen gültiger Standorte für einen Ressourcentyp
* Anzeigen gültiger API-Versionen für einen Ressourcentyp

Diese Schritte können über das Azure-Portal, mithilfe von Azure PowerShell oder unter Verwendung der Azure-Befehlszeilenschnittstelle ausgeführt werden.

Eine Liste, die Ressourcenanbieter zu Azure-Diensten zuordnet, finden Sie unter [Ressourcenanbieter für Azure-Dienste](azure-services-resource-providers.md).

## <a name="azure-portal"></a>Azure-Portal

So zeigen Sie alle Ressourcenanbieter und den Registrierungsstatus für Ihr Abonnement an

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü des Azure-Portals **Alle Dienste** aus.

    ![„Abonnements“ auswählen](./media/resource-providers-and-types/select-all-services.png)

3. Geben Sie im Feld **Alle Dienste** **Abonnement** ein, und wählen Sie dann **Abonnements** aus.
4. Wählen Sie das Abonnement aus der Abonnentenliste aus, um es anzuzeigen.
5. Wählen Sie **Ressourcenanbieter** aus, und sehen Sie sich die Liste mit den verfügbaren Ressourcenanbietern an.

    ![Ressourcenanbieter anzeigen](./media/resource-providers-and-types/show-resource-providers.png)

6. Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten. Wählen Sie **Registrieren** aus, um einen Ressourcenanbieter zu registrieren. Im vorherigen Screenshot ist der Link **Registrieren** für **Microsoft.Blueprint** hervorgehoben.

    Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen von diesem Ressourcenanbieter vorhanden sind.

So zeigen Sie Informationen für einen bestimmten Ressourcenanbieter an

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü des Azure-Portals **Alle Dienste** aus.
3. Geben Sie im Feld **Alle Dienste** **Ressourcen-Explorer** ein, und wählen Sie dann **Ressourcen-Explorer** aus.

    ![Auswahl von „Alle Dienste“](./media/resource-providers-and-types/select-resource-explorer.png)

4. Erweitern Sie **Anbieter**, indem Sie den Pfeil nach rechts auswählen.

    ![„Anbieter“ auswählen](./media/resource-providers-and-types/select-providers.png)

5. Erweitern Sie einen Ressourcenanbieter und Ressourcentyp, die Sie anzeigen möchten.

    ![Ressourcentyp auswählen](./media/resource-providers-and-types/select-resource-type.png)

6. Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen. Der Ressourcen-Explorer zeigt gültige Standorte für den Ressourcentyp an.

    ![Standorte anzeigen](./media/resource-providers-and-types/show-locations.png)

7. Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. Der Ressourcen-Explorer zeigt gültige API-Versionen für den Ressourcentyp an.

    ![API-Versionen anzeigen](./media/resource-providers-and-types/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Die Rückgabe sieht in etwa wie folgt aus:

```output
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```output
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen von diesem Ressourcenanbieter vorhanden sind.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```output
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Ausgabe des Befehls:

```output
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht.

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Ausgabe des Befehls:

```output
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen.

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Ausgabe des Befehls:

```output
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Die Rückgabe sieht in etwa wie folgt aus:

```output
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Durch Registrieren eines Ressourcenanbieters wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. Der Gültigkeitsbereich der Registrierung ist immer das Abonnement. Viele Ressourcenanbieter werden standardmäßig automatisch registriert. Einige Ressourcenanbieter müssen Sie jedoch unter Umständen manuell registrieren. Um einen Ressourcenanbieter zu registrieren, benötigen Sie die Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Dieser Vorgang ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

```azurecli
az provider register --namespace Microsoft.Batch
```

Daraufhin wird eine Meldung mit dem Hinweis zurückgegeben, dass die Registrierung noch nicht abgeschlossen ist.

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen von diesem Ressourcenanbieter vorhanden sind.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```azurecli
az provider show --namespace Microsoft.Batch
```

Die Rückgabe sieht in etwa wie folgt aus:

```output
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Ausgabe des Befehls:

```output
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht.

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Ausgabe des Befehls:

```output
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die verhindern, dass Sie einige Regionen verwenden, die die Ressource unterstützen.

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Ausgabe des Befehls:

```output
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../templates/template-syntax.md). 
* Informationen zum Anzeigen der Vorlagenschemata für Ressourcenanbieter finden Sie unter [Vorlagenreferenz](/azure/templates/).
* Eine Liste, die Ressourcenanbieter zu Azure-Diensten zuordnet, finden Sie unter [Ressourcenanbieter für Azure-Dienste](azure-services-resource-providers.md).
* Informationen zum Anzeigen der Vorgänge für einen Ressourcenanbieter finden Sie unter [Azure-REST-API](/rest/api/).
