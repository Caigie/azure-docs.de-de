---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/14/2019
ms.author: glenga
ms.openlocfilehash: 6fd8c3c5839d4cc897caa2dff70af87980e547eb
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88206736"
---
## <a name="local-settings-file"></a>Datei für lokale Einstellungen

Die Datei „local.settings.json“ speichert App-Einstellungen, Verbindungszeichenfolgen und Einstellungen, die von lokalen Entwicklungstools verwendet werden. Einstellungen in der Datei „local.settings.json“ werden nur bei der lokalen Ausführung von Projekten verwendet. Die Datei mit den lokalen Einstellungen hat folgende Struktur:

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "<language worker>",
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>",
    "AzureWebJobs.HttpExample.Disabled": "true"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*",
    "CORSCredentials": false
  },
  "ConnectionStrings": {
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

Diese Einstellungen werden bei der lokalen Ausführung von Projekten unterstützt:

| Einstellung      | BESCHREIBUNG                            |
| ------------ | -------------------------------------- |
| **`IsEncrypted`** | Wenn diese Einstellung auf `true` festgelegt wird, werden alle Werte mithilfe eines Schlüssels des lokalen Computers verschlüsselt. Wird mit `func settings`-Befehlen verwendet. Der Standardwert ist `false`. |
| **`Values`** | Ein Array von Anwendungseinstellungen und Verbindungszeichenfolgen, die bei der lokalen Ausführung eines Projekts verwendet werden. Diese Schlüssel-Wert-Paare (Zeichenfolge-Zeichenfolge) entsprechen den Anwendungseinstellungen in Ihrer Funktions-App in Azure, etwa [`AzureWebJobsStorage`]. Viele Trigger und Bindungen verfügen über eine Eigenschaft, die auf eine App-Einstellung für die Verbindungszeichenfolge verweist, z. B. `Connection` für den [Blob Storage-Trigger](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#configuration). Für diese Eigenschaften muss eine Anwendungseinstellung im Array `Values` definiert sein. <br/>[`AzureWebJobsStorage`] ist eine erforderliche App-Einstellung für andere Trigger als HTTP. <br/>Ab Version 2.x der Functions-Runtime ist die Einstellung [`FUNCTIONS_WORKER_RUNTIME`] erforderlich, die mit Core-Tools für Ihr Projekt generiert wird. <br/> Wenn der [Azure-Speicheremulator](../articles/storage/common/storage-use-emulator.md) lokal installiert ist und Sie [`AzureWebJobsStorage`] auf `UseDevelopmentStorage=true` festlegen, verwendet Core Tools den Emulator. Der Emulator ist während der Entwicklung hilfreich, doch sollten Sie vor der Bereitstellung einen Test mit einer tatsächlichen Speicherverbindung durchführen.<br/> Werte müssen Zeichenfolgen und dürfen nicht JSON-Objekte oder Arrays sein. Einstellungsnamen dürfen weder einen Doppelpunkt (`:`) noch einen doppelten Unterstrich (`__`) enthalten. Diese Zeichen sind für die Runtime reserviert. <br/>Wenn Sie bei lokaler Ausführung eine Funktion deaktivieren möchten, fügen Sie der Sammlung `"AzureWebJobs.<FUNCTION_NAME>.Disabled": "true"` hinzu, wobei `<FUNCTION_NAME>` der Name der Funktion ist. Weitere Informationen zum Deaktivieren von Funktionen in Azure Functions finden Sie [hier](../articles/azure-functions/disable-function.md#localsettingsjson).  |
| **`Host`** | Die Einstellungen in diesem Abschnitt passen den Hostprozess von Functions bei der lokalen Ausführung von Projekten an. Diese Einstellungen sind getrennt von den host.json-Einstellungen, die auch bei der Ausführung von Projekten in Azure angewendet werden. |
| **`LocalHttpPort`** | Legt den Standardport fest, der bei der Ausführung des lokalen Functions-Host verwendet wird (`func host start` und `func run`). Die Befehlszeilenoption `--port` hat Vorrang vor dieser Einstellung. |
| **`CORS`** | Definiert die für die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zulässigen Ursprünge. Ursprünge werden als durch Trennzeichen getrennte Liste ohne Leerzeichen bereitgestellt. Den Platzhalterwert (\*) wird unterstützt, wodurch Anforderungen von einem beliebigen Ursprung zulässig sind. |
| **`CORSCredentials`** |  Wird `true` festgelegt, sind Anforderungen vom Typ `withCredentials` zulässig. |
| **`ConnectionStrings`** | Eine Auflistung. Verwenden Sie diese Sammlung nicht für die Verbindungszeichenfolgen, die von Ihren Funktionsbindungen verwendet werden. Diese Sammlung wird nur von Frameworks verwendet, die Verbindungszeichenfolgen üblicherweise aus dem Abschnitt `ConnectionStrings` einer Konfigurationsdatei abrufen, z. B. [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Verbindungszeichenfolgen in diesem Objekt werden der Umgebung mit dem Anbietertyp [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) hinzugefügt. Elemente in dieser Sammlung werden nicht mit anderen App-Einstellungen in Azure veröffentlicht. Sie müssen diese Werte explizit zur Sammlung `Connection strings` in den Einstellungen Ihrer Funktions-App hinzufügen. Bei der Erstellung einer [`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx)-Klasse in Ihrem Funktionscode sollten Sie den Verbindungszeichenfolgenwert zusammen mit den anderen Verbindungen in den **Anwendungseinstellungen** im Portal speichern. |

[AzureWebJobsStorage]: ../articles/azure-functions/functions-app-settings.md#azurewebjobsstorage
