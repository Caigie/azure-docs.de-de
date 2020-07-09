---
title: Erstellen von Verwaltungsgruppen zum Organisieren von Ressourcen – Azure Governance
description: Hier erfahren Sie, wie Sie Azure-Verwaltungsgruppen zum Verwalten mehrerer Ressourcen über das Portal, mithilfe von Azure PowerShell oder mithilfe der Azure CLI erstellen.
ms.date: 04/15/2020
ms.topic: conceptual
ms.openlocfilehash: 0abecf02062dd4461274a9bc3722d46e6e7cc3d1
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220761"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Erstellen von Verwaltungsgruppen zum Organisieren und Verwalten von Ressourcen

Bei Verwaltungsgruppen handelt es sich um Container, mit denen Sie Zugriff, Richtlinien und Konformität abonnementübergreifend verwalten können. Erstellen Sie diese Container, um eine effektive und effiziente Hierarchie zu erstellen, die mit [Azure Policy](../policy/overview.md) und mit der [rollenbasierten Zugriffssteuerung (Role-Based Access Controls, RBAC) von Azure](../../role-based-access-control/overview.md) verwendet werden kann. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren von Ressourcen mit Azure-Verwaltungsgruppen](overview.md).

Es kann bis zu 15 Minuten dauern, bis die Erstellung der ersten Verwaltungsgruppe im Verzeichnis abgeschlossen ist. Bei der ersten Erstellung werden Prozesse zum Einrichten des Verwaltungsgruppendiensts in Azure für Ihr Verzeichnis ausgeführt. Sie erhalten eine Benachrichtigung, wenn der Vorgang abgeschlossen ist. Weitere Informationen finden Sie unter [Erstmalige Einrichtung von Verwaltungsgruppen](./overview.md#initial-setup-of-management-groups).

## <a name="create-a-management-group"></a>Erstellen einer Verwaltungsgruppe

Alle Azure AD-Benutzer im Mandanten können eine Verwaltungsgruppe erstellen, ohne dafür die Schreibberechtigung für die Verwaltungsgruppe zu benötigen. Diese neue Verwaltungsgruppe ist dann der Stammverwaltungsgruppe untergeordnet, und dem Ersteller wird die Rolle „Besitzer“ zugewiesen. Der Verwaltungsgruppendienst ermöglicht dies, damit Rollen nicht auf Stammebene zugewiesen werden müssen. Bei der Erstellung der Stammverwaltungsgruppe hat kein Benutzer Zugriff auf sie. Damit nicht erst nach den globalen Azure AD-Administratoren gesucht werden muss, um mit der Verwendung von Verwaltungsgruppen beginnen zu können, lassen wir die Erstellung der ersten Verwaltungsgruppen auf  
Stammebene zu.

Sie können die Verwaltungsgruppe im Portal, mit einer [Resource Manager-Vorlage](../../azure-resource-manager/templates/deploy-to-tenant.md#create-management-group), mithilfe von PowerShell oder über die Azure CLI erstellen.

### <a name="create-in-portal"></a>Erstellen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltung und Governance**.

1. Wählen Sie **Verwaltungsgruppen** aus.

1. Wählen Sie **+ Verwaltungsgruppe hinzufügen** aus.

   :::image type="content" source="./media/main.png" alt-text="Seite für die Arbeit mit Verwaltungsgruppen" border="false":::

1. Füllen Sie das Feld für die Verwaltungsgruppen-ID aus.

   - Die **ID der Verwaltungsgruppe** ist der eindeutige Bezeichner des Verzeichnisses, der zum Übermitteln von Befehlen für diese Verwaltungsgruppe verwendet wird. Dieser Bezeichner kann nach der Erstellung nicht bearbeitet werden, da er im gesamten Azure-System zum Identifizieren dieser Gruppe verwendet wird. Die [Stammverwaltungsgruppe](overview.md#root-management-group-for-each-directory) wird mit einer ID, bei der es sich um die Azure Active Directory-ID handelt, automatisch erstellt. Weisen Sie bei allen anderen Verwaltungsgruppen eine eindeutige ID zu.
   - Im Feld für den Anzeigenamen wird der Name angegeben, der im Azure-Portal angezeigt wird. Beim Erstellen der Verwaltungsgruppe gibt es ein optionales Feld für einen separaten Anzeigenamen, der jederzeit geändert werden  
     kann.

   :::image type="content" source="./media/create_context_menu.png" alt-text="Bereich „Optionen“ zum Erstellen einer neuen Verwaltungsgruppe" border="false":::

1. Wählen Sie **Speichern** aus.

### <a name="create-in-powershell"></a>Erstellen in PowerShell

Verwenden Sie bei PowerShell das Cmdlet [New-AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) zum Erstellen einer neuen Verwaltungsgruppe.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** ist ein eindeutiger Bezeichner, der erstellt wird. Diese ID wird von anderen Befehlen zum Verweisen auf diese Gruppe verwendet. Sie kann später nicht geändert werden.

Falls im Azure-Portal ein anderer Name für die Verwaltungsgruppe angezeigt werden soll, fügen Sie den Parameter **DisplayName** hinzu. Beispiel: Wenn Sie eine Verwaltungsgruppe mit dem Gruppennamen „Contoso“ und dem Anzeigenamen „Contoso Group“ erstellen möchten, verwenden Sie das folgende Cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

In den vorstehenden Beispielen wird die neue Verwaltungsgruppe unter der Stammverwaltungsgruppe erstellt. Wenn Sie eine andere Verwaltungsgruppe als übergeordnetes Element angeben möchten, verwenden Sie den Parameter **ParentId**.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Erstellen in der Azure CLI

Verwenden Sie bei der Azure CLI den Befehl [az account management-group create](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) zum Erstellen einer neuen Verwaltungsgruppe.

```azurecli-interactive
az account management-group create --name Contoso
```

Der **Name** ist ein eindeutiger Bezeichner, der erstellt wird. Diese ID wird von anderen Befehlen zum Verweisen auf diese Gruppe verwendet. Sie kann später nicht geändert werden.

Falls im Azure-Portal ein anderer Name für die Verwaltungsgruppe angezeigt werden soll, fügen Sie den Parameter **display-name** hinzu. Beispiel: Wenn Sie eine Verwaltungsgruppe mit dem Gruppennamen „Contoso“ und dem Anzeigenamen „Contoso Group“ erstellen möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

In den vorstehenden Beispielen wird die neue Verwaltungsgruppe unter der Stammverwaltungsgruppe erstellt. Wenn Sie eine andere Verwaltungsgruppe als übergeordnetes Element angeben möchten, verwenden Sie den Parameter **parent**, und geben Sie den Namen der übergeordneten Gruppe an.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links:

- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](./create.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](./manage.md)
- [Überprüfen von Verwaltungsgruppen im Azure PowerShell-Ressourcenmodul](/powershell/module/az.resources#resources)
- [Überprüfen von Verwaltungsgruppen in der REST-API](/rest/api/resources/managementgroups)
- [Überprüfen von Verwaltungsgruppen in der Azure CLI](/cli/azure/account/management-group)
