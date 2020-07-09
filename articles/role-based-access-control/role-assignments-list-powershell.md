---
title: Auflisten von Azure-Rollenzuweisungen mit Azure PowerShell – Azure RBAC
description: Hier erfahren Sie, wie Sie mit Azure PowerShell und der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC) ermitteln können, auf welche Ressourcen Benutzer, Gruppen, Dienstprinzipale und verwaltete Identitäten Zugriff besitzen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4ee6a3c09d24d6968227ef4215000888c5f4af05
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791009"
---
# <a name="list-azure-role-assignments-using-azure-powershell"></a>Auflisten von Azure-Rollenzuweisungen mit Azure PowerShell

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] In diesem Artikel wird beschrieben, wie Sie mit Azure PowerShell Rollenzuweisungen auflisten.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

> [!NOTE]
> Wenn Ihre Organisation Verwaltungsfunktionen an einen Dienstanbieter ausgelagert hat, der die [delegierte Azure-Ressourcenverwaltung](../lighthouse/concepts/azure-delegated-resource-management.md) verwendet, werden die von diesem Dienstanbieter autorisierten Rollenzuweisungen hier nicht angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

- [PowerShell in Azure Cloud Shell](/azure/cloud-shell/overview) oder [Azure PowerShell](/powershell/azure/install-az-ps)

## <a name="list-role-assignments-for-the-current-subscription"></a>Auflisten von Rollenzuweisungen für das aktuelle Abonnement

Die einfachste Möglichkeit, eine Liste aller Rollenzuweisungen im aktuellen Abonnement (einschließlich geerbte Rollenzuweisungen von Stamm- und Verwaltungsgruppen) zu erhalten, besteht in der Verwendung von [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) ohne Parameter.

```azurepowershell
Get-AzRoleAssignment
```

```Example
PS C:\> Get-AzRoleAssignment

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : Alain
SignInName         : alain@example.com
RoleDefinitionName : Storage Blob Data Reader
RoleDefinitionId   : 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
DisplayName        : Marketing
SignInName         :
RoleDefinitionName : Contributor
RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
ObjectId           : 22222222-2222-2222-2222-222222222222
ObjectType         : Group
CanDelegate        : False

...
```

## <a name="list-role-assignments-for-a-subscription"></a>Auflisten der Rollenzuweisungen für ein Abonnement

Um alle Rollenzuweisungen in einem Abonnementbereich aufzulisten, verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment). Die Abonnement-ID können Sie über das Blatt **Abonnements** im Azure-Portal oder durch Verwenden von [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription) abrufen.

```azurepowershell
Get-AzRoleAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzRoleAssignment -Scope /subscriptions/00000000-0000-0000-0000-000000000000
```

## <a name="list-role-assignments-for-a-user"></a>Liste von Rollenzuweisungen für einen Benutzer

Verwenden Sie zum Auflisten aller Rollen, die einem bestimmten Benutzer zugewiesen sind, [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

```azurepowershell
Get-AzRoleAssignment -SignInName <email_or_userprincipalname>
```

```Example
PS C:\> Get-AzRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
```

Um alle Rollen, die einem bestimmten Benutzer zugewiesen sind, und die Rollen aufzulisten, die den Gruppen des Benutzers zugewiesen sind, verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

```azurepowershell
Get-AzRoleAssignment -SignInName <email_or_userprincipalname> -ExpandPrincipalGroups
```

```Example
Get-AzRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

## <a name="list-role-assignments-for-a-resource-group"></a>Auflisten von Rollenzuweisungen für eine Ressourcengruppe

Um alle Rollenzuweisungen in einem Ressourcengruppenbereich aufzulisten, verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

```azurepowershell
Get-AzRoleAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzRoleAssignment -ResourceGroupName pharma-sales | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Alain Charon
RoleDefinitionName : Backup Operator
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales

DisplayName        : Alain Charon
RoleDefinitionName : Virtual Machine Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales
```

## <a name="list-role-assignments-for-a-management-group"></a>Auflisten von Rollenzuweisungen für eine Verwaltungsgruppe

Um alle Rollenzuweisungen in einem Verwaltungsgruppenbereich aufzulisten, verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment). Die Verwaltungsgruppen-ID befindet sich auf dem Blatt **Verwaltungsgruppen** im Azure-Portal, oder Sie können zum Abrufen auch [Get-AzManagementGroup](/powershell/module/az.resources/get-azmanagementgroup) verwenden.

```azurepowershell
Get-AzRoleAssignment -Scope /providers/Microsoft.Management/managementGroups/<group_id>
```

```Example
PS C:\> Get-AzRoleAssignment -Scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="list-role-assignments-for-classic-service-administrator-and-co-administrators"></a>Auflisten der Rollenzuweisungen für klassische Abonnementadministratoren und -Co-Administratoren

Zum Auflisten der Rollenzuweisungen für die klassischen Abonnementadministratoren und -Co-Administratoren verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

```azurepowershell
Get-AzRoleAssignment -IncludeClassicAdministrators
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Auflisten von Rollenzuweisungen für eine verwaltete Identität

1. Rufen Sie die Objekt-ID der systemseitig oder benutzerseitig zugewiesenen verwalteten Identität ab. 

    Um die Objekt-ID einer benutzerseitig zugewiesenen verwalteten Identität abzurufen, können Sie [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) verwenden.

    ```azurepowershell
    Get-AzADServicePrincipal -DisplayNameBeginsWith "<name> or <vmname>"
    ```

1. Um die Rollenzuweisungen aufzulisten, verwenden Sie [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment).

    ```azurepowershell
    Get-AzRoleAssignment -ObjectId <objectid>
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe von Azure PowerShell](role-assignments-powershell.md)
