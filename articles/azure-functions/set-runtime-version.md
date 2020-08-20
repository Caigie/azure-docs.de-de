---
title: Einstellen von Runtimeversionen von Azure Functions als Ziel
description: Azure Functions unterstützt mehrere Versionen der Runtime. Erfahren Sie, wie Sie die Runtimeversion einer in Azure gehosteten Funktions-App angeben.
ms.topic: conceptual
ms.date: 07/22/2020
ms.openlocfilehash: 74ee0d382dcd468aed118a7de330eef95b329402
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830868"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Einstellen von Runtimeversionen von Azure Functions als Ziel

Eine Funktionen-App wird für eine bestimmte Version der Azure Functions-Runtime ausgeführt. Es gibt drei Hauptversionen: [1.x, 2.x und 3.x](functions-versions.md). Standardmäßig werden Funktions-Apps in Version 3.x der Runtime erstellt. In diesem Artikel wird erläutert, wie Sie eine Funktionen-App in Azure so konfigurieren, dass sie mit der von Ihnen ausgewählten Version ausgeführt wird. Informationen zum Konfigurieren einer lokalen Entwicklungsumgebung für eine bestimmte Version finden Sie unter [Codieren und lokales Testen von Azure Functions](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Automatische und manuelle Versionsupdates

Mit Azure Functions können Sie eine bestimmte Version der Runtime als Ziel festlegen, indem Sie die Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` in einer Funktionen-App verwenden. Die Funktionen-App verwendet weiterhin die angegebene Version der Runtime, bis Sie ausdrücklich die Verwendung einer neuen Version auswählen. Wenn Sie nur die Hauptversion angeben, wird die Funktions-App automatisch auf neue Nebenversionen der Runtime aktualisiert, sobald diese verfügbar werden. Neue Nebenversionen sollten keine Breaking Changes enthalten. 

Wenn Sie eine Nebenversion (z.B. „2.0.12345“) angeben, wird die Funktions-App an diese spezifische Version angeheftet, bis Sie sie explizit ändern. Ältere Nebenversionen werden regelmäßig aus der Produktionsumgebung entfernt. Danach wird Ihre Funktions-App auf der neuesten Version anstatt auf der in `FUNCTIONS_EXTENSION_VERSION` festgelegten Version ausgeführt. Aus diesem Grund sollten Sie Probleme im Zusammenhang mit Ihrer Funktions-App, die eine bestimmte Nebenversion erfordern, schnell beheben, damit Sie stattdessen die Hauptversion als Ziel verwenden können. Das Entfernen von Nebenversionen wird in den [App Service-Ankündigungen](https://github.com/Azure/app-service-announcements/issues) erläutert.

> [!NOTE]
> Wenn Sie an eine bestimmte Hauptversion von Azure Functions anheften und dann versuchen, mit Visual Studio in Azure zu veröffentlichen, wird ein Dialogfenster angezeigt, in dem Sie aufgefordert werden, auf die neueste Version zu aktualisieren oder die Veröffentlichung abzubrechen. Um dies zu vermeiden, fügen Sie die `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>`-Eigenschaft Ihrer `.csproj`-Datei hinzu.

Wenn eine neue Version öffentlich verfügbar ist, bietet eine Eingabeaufforderung im Portal Ihnen die Möglichkeit, auf diese Version umzusteigen. Nach dem Wechsel zu einer neuen Version können Sie jederzeit mit der Anwendungseinstellung `FUNCTIONS_EXTENSION_VERSION` zu einer früheren Version zurückkehren.

Die folgende Tabelle enthält die Werte vom Typ `FUNCTIONS_EXTENSION_VERSION` für die jeweilige Hauptversion, um automatische Updates zu aktivieren:

| Hauptversion | Wert vom Typ `FUNCTIONS_EXTENSION_VERSION` |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2.x  | `~2` |
| 1.x  | `~1` |

Eine Änderung an der Runtimeversion bewirkt, dass eine Funktionen-App neu gestartet wird.

## <a name="view-and-update-the-current-runtime-version"></a>Anzeigen und Aktualisieren der aktuellen Runtimeversion

Sie können die von der Funktions-App verwendete Runtimeversion ändern. Da möglicherweise wesentliche Änderungen vorliegen, können Sie nur die Runtimeversion ändern, bevor Sie Funktionen in Ihrer Funktions-App erstellt haben. 

> [!IMPORTANT]
> Obwohl die Runtime-Version durch die Einstellung `FUNCTIONS_EXTENSION_VERSION` bestimmt wird, sollten Sie diese Änderung im Azure-Portal vornehmen und nicht durch direkte Änderung der Einstellung. Grund dafür ist, dass das Portal Ihre Änderungen überprüft und bei Bedarf weitere zugehörige Änderungen vornimmt.

### <a name="from-the-azure-portal"></a>Über das Azure-Portal

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Mit dem Azure-Portal können Sie die Runtimeversion für eine Funktions-App, die bereits Funktionen enthält, nicht ändern.

### <a name="from-the-azure-cli"></a><a name="view-and-update-the-runtime-version-using-azure-cli"></a>Über die Azure CLI

Sie können `FUNCTIONS_EXTENSION_VERSION` auch über die Azure-Befehlszeilenschnittstelle anzeigen und festlegen.

>[!NOTE]
>Da auch andere Einstellungen von der Runtimeversion beeinträchtigt werden können, sollten Sie die Version im Portal ändern. Das Portal nimmt automatisch die anderen erforderlichen Updates vor, wenn Sie die Runtimeversionen ändern, z.B. Node.js-Version und Runtimestapel.  

Zeigen Sie in der Azure-Befehlszeilenschnittstelle die aktuelle Version der Runtime mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) an.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Ersetzen Sie in diesem Code `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. 

Sie sehen `FUNCTIONS_EXTENSION_VERSION` in der folgenden Ausgabe, die zur besseren Lesbarkeit beschnitten wurde:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

Sie können die Einstellung `FUNCTIONS_EXTENSION_VERSION` in der Funktions-App mit dem Befehl [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) aktualisieren.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Ersetzen Sie `<function_app>` durch den Namen der Funktions-App. Ersetzen Sie außerdem `<my_resource_group>` durch den Namen der Ressourcengruppe für Ihre Funktions-App. Ersetzen Sie außerdem `<version>` durch eine gültige Version der Runtime 1.x oder durch `~2` für Version 2.x.

Sie können diesen Befehl an der [Azure Cloud Shell](../cloud-shell/overview.md) ausführen, indem Sie im vorangehenden Codebeispiel **Ausprobieren** auswählen. Sie können auch die [Azure-Befehlszeilenschnittstelle lokal](/cli/azure/install-azure-cli) zum Ausführen dieses Befehls verwenden, nachdem Sie sich mit [az login](/cli/azure/reference-index#az-login) angemeldet haben.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der 2.0-Runtime als Ziel in Ihrer lokalen Entwicklungsumgebung](functions-run-local.md)

> [!div class="nextstepaction"]
> [Siehe Anmerkungen zu dieser Version für Runtimeversionen](https://github.com/Azure/azure-webjobs-sdk-script/releases)
