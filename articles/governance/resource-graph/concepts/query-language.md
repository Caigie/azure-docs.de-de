---
title: Grundlegendes zur Abfragesprache
description: Beschreibt Resource Graph-Tabellen und die verfügbaren Kusto-Datentypen, -Operatoren und -Funktionen, die mit Azure Resource Graph verwendet werden können.
ms.date: 06/29/2020
ms.topic: conceptual
ms.openlocfilehash: 4c545a8a5113f800545660a3ea812b61711630c2
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970449"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Grundlegendes zur Azure Resource Graph-Abfragesprache

Die Abfragesprache für Azure Resource Graph unterstützt eine Reihe von Operatoren und Funktionen. Diese arbeiten und funktionieren auf Grundlage der [Abfragesprache Kusto (Kusto Query Language, KQL)](/azure/kusto/query/index). Wenn Sie mehr über die von Resource Graph verwendete Abfragesprache erfahren möchten, beginnen Sie mit dem [Tutorial für KQL](/azure/kusto/query/tutorial).

In diesem Artikel werden die von Resource Graph unterstützten Sprachkomponenten behandelt:

- [Resource Graph-Tabellen](#resource-graph-tables)
- [Benutzerdefinierte Sprachelemente in Resource Graph](#resource-graph-custom-language-elements)
- [Unterstützte KQL-Sprachelemente](#supported-kql-language-elements)
- [Escapezeichen](#escape-characters)

## <a name="resource-graph-tables"></a>Resource Graph-Tabellen

Resource Graph umfasst mehrere Tabellen für die Daten, die in Bezug auf Azure Resource Manager-Ressourcentypen und deren Eigenschaften gespeichert werden. Diese Tabellen können mit den Operatoren `join` oder `union` verwendet werden, um Eigenschaften von verknüpften Ressourcentypen zu erhalten. Es folgt eine Liste der in Resource Graph verfügbaren Tabellen:

|Resource Graph-Tabellen |BESCHREIBUNG |
|---|---|
|Ressourcen |Die Standardtabelle, wenn keine in der Abfrage definiert ist. Die meisten Resource Manager-Ressourcentypen und -Eigenschaften sind hier enthalten. |
|ResourceContainers |Umfasst die Ressourcentypen und Daten des Abonnements (in der Vorschauphase – `Microsoft.Resources/subscriptions`) und der Ressourcengruppe (`Microsoft.Resources/subscriptions/resourcegroups`). |
|AdvisorResources |Umfasst Ressourcen, die mit `Microsoft.Advisor`_in Zusammenhang stehen_. |
|AlertsManagementResources |Umfasst Ressourcen, die mit `Microsoft.AlertsManagement`_in Zusammenhang stehen_. |
|HealthResources |Umfasst Ressourcen, die mit `Microsoft.ResourceHealth`_in Zusammenhang stehen_. |
|MaintenanceResources |Umfasst Ressourcen, die mit `Microsoft.Maintenance`_in Zusammenhang stehen_. |
|SecurityResources |Umfasst Ressourcen, die mit `Microsoft.Security`_in Zusammenhang stehen_. |

Eine komplette Liste, einschließlich Ressourcentypen, finden Sie unter [ Azure Resource Graph-Tabelle und Ressourcentypreferenz](../reference/supported-tables-resources.md).

> [!NOTE]
> _Resources_ ist die Standardtabelle. Beim Abfragen der Tabelle _Resources_ muss der Tabellenname nicht angegeben werden, es sei denn, `join` oder `union` wird verwendet. Es wird jedoch empfohlen, immer die anfängliche Tabelle in die Abfrage einzubeziehen.

Verwenden Sie den Resource Graph-Explorer im Portal, um zu ermitteln, welche Ressourcentypen in den einzelnen Tabellen verfügbar sind. Sie können auch eine Abfrage verwenden (z.B. `<tableName> | distinct type`), um eine Liste von Ressourcentypen zu erhalten, die von der angegebenen Resource Graph-Tabelle unterstützt werden und in Ihrer Umgebung vorhanden sind.

Die folgende Abfrage zeigt eine einfache Verwendung von `join`. Im Abfrageergebnis sind die Spalten kombiniert, und an alle doppelten Spaltennamen aus der verknüpften Tabelle (_ResourceContainers_ in diesem Beispiel) wird **1** angehängt. Da die Tabelle _ResourceContainers_ Typen für Abonnements und Ressourcengruppen enthält, können beide Typen zum Verknüpfen der Ressource aus der Tabelle _Resources_ verwendet werden.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

Die folgende Abfrage zeigt eine komplexere Verwendung von `join`. Die Abfrage schränkt die verknüpfte Tabelle auf Abonnementressourcen ein, und mit `project` wird nur das ursprüngliche Feld _subscriptionId_ einbezogen, und das Feld _name_ wird in _SubName_ umbenannt. Durch das Umbenennen des Felds wird das Hinzufügen als _name1_ bei `join` vermieden, da das Feld bereits unter _Resources_ vorhanden ist. Die ursprüngliche Tabelle wird mit `where` gefiltert, und das folgende `project`-Element enthält Spalten aus beiden Tabellen. Das Abfrageergebnis ist ein einzelner Schlüsseltresor, der den Typ, den Namen des Schlüsseltresors und den Namen des Abonnements, in dem er sich befindet, anzeigt.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> Wenn Sie die Ergebnisse von `join` mit `project` einschränken, muss die Eigenschaft, die von `join` zum Verknüpfen der beiden Tabellen verwendet wird (_subscriptionId_ im obigen Beispiel), in `project` enthalten sein.

## <a name="resource-graph-custom-language-elements"></a>Benutzerdefinierte Sprachelemente in Resource Graph

### <a name="shared-query-syntax-preview"></a><a name="shared-query-syntax"></a>Syntax für freigegebene Abfragen (Vorschauversion)

Als Previewfunktion kann der Zugriff auf eine [freigegebene Abfrage](../tutorials/create-share-query.md) direkt in einer Resource Graph-Abfrage erfolgen. Dieses Szenario ermöglicht es, Standardabfragen als freigegebene Abfragen zu erstellen und wiederzuverwenden. Verwenden Sie die Syntax `{{shared-query-uri}}`, um eine freigegebene Abfrage innerhalb einer Resource Graph-Abfrage aufzurufen. Der URI der freigegebenen Abfrage entspricht der _Ressourcen-ID_ der freigegebenen Abfrage auf der Seite **Einstellungen** für diese Abfrage. In diesem Beispiel ist `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS` der URI für die freigegebene Abfrage.
Dieser URI verweist auf das Abonnement, die Ressourcengruppe und den vollständigen Namen der freigegebenen Abfrage, auf die in einer anderen Abfrage verwiesen werden soll. Diese Abfrage ist identisch mit der Abfrage, die unter [Tutorial: Erstellen und Freigeben einer Abfrage](../tutorials/create-share-query.md) erstellt wird.

> [!NOTE]
> Eine Abfrage, die auf eine freigegebene Abfrage verweist, kann nicht als freigegebene Abfrage gespeichert werden.

Beispiel 1: Alleiniges Verwenden der freigegebenen Abfrage

Die Ergebnisse dieser Resource Graph-Abfrage sind identisch mit denen der in der freigegebenen Abfrage gespeicherten.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
```

Beispiel 2: Einfügen der freigegebenen Abfrage als Teil einer größeren Abfrage

Bei dieser Abfrage wird zunächst die freigegebene Abfrage und dann `limit` verwendet, um die Ergebnisse weiter einzugrenzen.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
| where properties_storageProfile_osDisk_osType =~ 'Windows'
```

## <a name="supported-kql-language-elements"></a>Unterstützte KQL-Sprachelemente

Resource Graph unterstützt alle [Datentypen](/azure/kusto/query/scalar-data-types/), [Skalarfunktionen](/azure/kusto/query/scalarfunctions), [Skalaroperatoren](/azure/kusto/query/binoperators) und [Aggregationsfunktionen](/azure/kusto/query/any-aggfunction) von KQL. Es werden bestimmte [tabellarische Operatoren](/azure/kusto/query/queries) von Resource Graph unterstützt, von denen einige unterschiedliche Verhaltensweisen aufweisen.

### <a name="supported-tabulartop-level-operators"></a>Unterstützte tabellarische Operatoren und Operatoren auf oberster Ebene

Es folgt eine Liste der von Resource Graph unterstützten tabellarischen KQL-Operatoren mit spezifischen Beispielen:

|KQL |Resource Graph-Beispielabfrage |Notizen |
|---|---|---|
|[count](/azure/kusto/query/countoperator) |[Anzahl von Schlüsseltresoren](../samples/starter.md#count-keyvaults) | |
|[distinct](/azure/kusto/query/distinctoperator) |[Unterschiedliche Werte für einen bestimmten Alias anzeigen](../samples/starter.md#distinct-alias-values) | |
|[extend](/azure/kusto/query/extendoperator) |[Anzahl von virtuellen Computern nach Betriebssystemtyp](../samples/starter.md#count-os) | |
|[join](/azure/kusto/query/joinoperator) |[Schlüsseltresor mit Abonnementname](../samples/advanced.md#join) |Unterstützte Joinvarianten: [innerunique](/azure/kusto/query/joinoperator#default-join-flavor), [inner](/azure/kusto/query/joinoperator#inner-join), [leftouter](/azure/kusto/query/joinoperator#left-outer-join). Maximal drei `join`-Elemente in einer einzelnen Abfrage. Benutzerdefinierte Joinstrategien wie Broadcastjoin sind nicht zulässig. Kann innerhalb einer einzelnen Tabelle oder zwischen den Tabellen _Resources_ und _ResourceContainers_ verwendet werden. |
|[limit](/azure/kusto/query/limitoperator) |[Liste der öffentlichen IP-Adressen](../samples/starter.md#list-publicip) |Synonym für `take` |
|[mvexpand](/azure/kusto/query/mvexpandoperator) | | Legacy-Operator, verwenden Sie stattdessen `mv-expand`. _RowLimit_ maximal 400. Der Standardwert ist 128. |
|[mv-expand](/azure/kusto/query/mvexpandoperator) |[Auflisten von Cosmos DB mit bestimmten Schreibstandorten](../samples/advanced.md#mvexpand-cosmosdb) |_RowLimit_ maximal 400. Der Standardwert ist 128. |
|[order](/azure/kusto/query/orderoperator) |[Auflisten von Ressourcen nach Namen sortiert](../samples/starter.md#list-resources) |Synonym für `sort` |
|[project](/azure/kusto/query/projectoperator) |[Auflisten von Ressourcen nach Namen sortiert](../samples/starter.md#list-resources) | |
|[project-away](/azure/kusto/query/projectawayoperator) |[Entfernen von Spalten aus den Ergebnissen](../samples/advanced.md#remove-column) | |
|[sort](/azure/kusto/query/sortoperator) |[Auflisten von Ressourcen nach Namen sortiert](../samples/starter.md#list-resources) |Synonym für `order` |
|[summarize](/azure/kusto/query/summarizeoperator) |[Anzahl der Azure-Ressourcen](../samples/starter.md#count-resources) |Nur vereinfachte erste Seite |
|[take](/azure/kusto/query/takeoperator) |[Liste der öffentlichen IP-Adressen](../samples/starter.md#list-publicip) |Synonym für `limit` |
|[top](/azure/kusto/query/topoperator) |[Anzeigen der ersten fünf virtuellen Computer nach Name und BS-Typ](../samples/starter.md#show-sorted) | |
|[union](/azure/kusto/query/unionoperator) |[Vereinen von Ergebnissen aus zwei Abfragen in einem einzigen Ergebnis](../samples/advanced.md#unionresults) |Einzelne Tabelle zulässig: _T_ `| union` \[`kind=` `inner`\|`outer`\] \[`withsource=`_ColumnName_\] _Table_. Maximal drei `union`-Verzweigungen in einer einzelnen Abfrage. Fuzzyauflösung von `union`-Verzweigungstabellen ist nicht zulässig. Kann innerhalb einer einzelnen Tabelle oder zwischen den Tabellen _Resources_ und _ResourceContainers_ verwendet werden. |
|[where](/azure/kusto/query/whereoperator) |[Anzeigen von Ressourcen, die Speicher enthalten](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>Escape-Zeichen

Einige Eigenschaftennamen, etwa diejenigen, die einen `.` oder ein `$` enthalten, müssen in einer Abfrage umhüllt oder mit Escapezeichen versehen sein. Andernfalls wird der jeweilige Eigenschaftenname falsch interpretiert, sodass nicht die erwarteten Ergebnisse bereitgestellt werden.

- `.` – Umhüllen Sie den Namen der jeweiligen Eigenschaft: `['propertyname.withaperiod']`
  
  Beispielabfrage, in der die Eigenschaft _odata.type_ umhüllt ist:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` – Versehen Sie den Namen der jeweiligen Eigenschaft mit einem Escapezeichen. Welches Escapezeichen verwendet wird, hängt von der Shell, in der Ressource Graph ausgeführt wird.

  - **Bash** - `\`

    Beispielabfrage, in der die Eigenschaft _\$type_ in Bash mit einem Escapezeichen versehen ist:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** – Versehen Sie das `$`-Zeichen nicht mit einem Escapezeichen.

  - **PowerShell** - ``` ` ```

    Beispielabfrage, in der die Eigenschaft _\$type_ in PowerShell mit einem Escapezeichen versehen ist:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Erfahren Sie mehr über das [Erkunden von Ressourcen](explore-resources.md).