---
title: Azure Resource Manager-Vorlage für den Log Analytics-Arbeitsbereich
description: Sie können Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren von Log Analytics-Arbeitsbereichen verwenden.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/09/2020
ms.openlocfilehash: 240a261f8dd401f36ef763e4c1274a1c0760f2dd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515647"
---
# <a name="manage-log-analytics-workspace-using-azure-resource-manager-templates"></a>Verwalten von Log Analytics-Arbeitsbereichen mithilfe von Azure Resource Manager-Vorlagen

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Sie können [Azure Resource Manager-Vorlagen](../../azure-resource-manager/templates/template-syntax.md) zum Erstellen und Konfigurieren von Log Analytics-Arbeitsbereichen in Azure Monitor verwenden. Beispiele für die Aufgaben, die Sie mit Vorlagen ausführen können:

* Erstellen eines Arbeitsbereichs, einschließlich Festlegen des Tarifs und Kapazitätsreservierung
* Hinzufügen einer Lösung
* Erstellen gespeicherter Suchvorgänge
* Erstellen einer Computergruppe
* Aktivieren der Sammlung von IIS-Protokollen auf Computern mit installiertem Windows-Agent
* Sammeln von Leistungsindikatoren von Windows- und Linux-Computern
* Sammeln von Ereignissen aus Syslog auf Linux-Computern 
* Sammeln von Ereignissen aus Windows-Ereignisprotokollen
* Sammeln von benutzerdefinierten Protokollen auf Windows-Computern
* Hinzufügen des Log Analytics-Agents auf virtuellen Azure-Computern
* Konfiguration von Log Analytics zum Indizieren der Daten, die mit der Azure-Diagnose gesammelt werden

Dieser Artikel enthält Vorlagenbeispiele, die einen Teil der Konfiguration veranschaulichen, die Sie mit Vorlagen ausführen können.

## <a name="api-versions"></a>API-Versionen

Die folgende Tabelle enthält die API-Versionen für die Ressourcen, die in diesem Beispiel verwendet werden.

| Resource | Ressourcentyp | API-Version |
|:---|:---|:---|
| Arbeitsbereich   | workspaces    | 2017-03-15-preview |
| Suchen,      | savedSearches | 2015-03-20 |
| Datenquelle | datasources   | 2015-11-01-preview |
| Lösung    | solutions     | 2015-11-01-preview |

## <a name="create-a-log-analytics-workspace"></a>Erstellen eines Log Analytics-Arbeitsbereichs

Im folgenden Beispiel wird mithilfe einer Vorlage von Ihrem lokalen Computer ein Arbeitsbereich erstellt. Die JSON-Vorlage ist so konfiguriert, dass nur der Name und der Speicherort des neuen Arbeitsbereichs erforderlich sind. Es werden Werte verwendet, die für andere Arbeitsbereichsparameter wie [Zugriffssteuerungsmodus](design-logs-deployment.md#access-control-mode), Tarif, Aufbewahrung und Kapazitätsreservierungshöhe angegeben sind.

> [!WARNING]
> Die folgende Vorlage erstellt einen Log Analytics-Arbeitsbereich und konfiguriert die Datensammlung. Dies kann eine Änderung Ihrer Abrechnungseinstellungen verursachen. Lesen Sie [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](manage-cost-storage.md), um die Abrechnung für in einem Log Analytics-Arbeitsbereich gesammelte Daten zu verstehen, bevor Sie dies in Ihrer Azure-Umgebung anwenden.

Für die Kapazitätsreservierung definieren Sie eine ausgewählte Kapazitätsreservierung für das Erfassen von Daten, indem Sie die SKU `CapacityReservation` und einen Wert in GB für die Eigenschaft `capacityReservationLevel` angeben. In der folgenden Liste sind die unterstützten Werte und das Verhalten bei der Konfiguration erläutert.

- Nachdem Sie das Reservierungslimit festgelegt haben, können Sie innerhalb von 31 Tagen nicht zu einer anderen SKU wechseln.

- Nachdem Sie den Reservierungswert festgelegt haben, können Sie ihn nur innerhalb von 31 Tagen erhöhen.

- Sie können den Wert von `capacityReservationLevel` nur in Vielfachen von 100 mit einem Maximalwert von 50000 festlegen.

- Wenn Sie die Reservierungshöhe erhöhen, wird der Timer zurückgesetzt, und Sie können ihn für weitere 31 Tage ab dieser Aktualisierung nicht mehr ändern.  

- Wenn Sie eine andere Eigenschaft für den Arbeitsbereich ändern, aber das Reservierungslimit auf derselben Höhe belassen, wird der Timer nicht zurückgesetzt. 

### <a name="create-and-deploy-template"></a>Erstellen und Bereitstellen der Vorlage

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
      "sku": {
        "type": "string",
        "allowedValues": [
          "pergb2018",
          "Free",
          "Standalone",
          "PerNode",
          "Standard",
          "Premium"
          ],
        "defaultValue": "pergb2018",
        "metadata": {
        "description": "Pricing tier: PerGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
        }
      },
      "location": {
        "type": "String",
        "allowedValues": [
        "australiacentral", 
        "australiaeast", 
        "australiasoutheast", 
        "brazilsouth",
        "canadacentral", 
        "centralindia", 
        "centralus", 
        "eastasia", 
        "eastus", 
        "eastus2", 
        "francecentral", 
        "japaneast", 
        "koreacentral", 
        "northcentralus", 
        "northeurope", 
        "southafricanorth", 
        "southcentralus", 
        "southeastasia", 
        "uksouth", 
        "ukwest", 
        "westcentralus", 
        "westeurope", 
        "westus", 
        "westus2" 
        ],
      "metadata": {
        "description": "Specifies the location in which to create the workspace."
        }
      }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "name": "[parameters('sku')]"
                },
                "retentionInDays": 120,
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            }
          }
       ]
    }
    ```

   >[!NOTE]
   >Verwenden Sie für Einstellungen zur Kapazitätsreservierung die folgenden Eigenschaften unter "sku":
   >* "name": "CapacityReservation",
   >* "capacityReservationLevel": 100

2. Bearbeiten Sie die Vorlage entsprechend Ihren Anforderungen. Erstellen Sie ggf. eine [Resource Manager-Parameterdatei](../../azure-resource-manager/templates/parameter-files.md), anstatt Parameter als Inlinewerte zu übergeben. Informationen zu den unterstützten Eigenschaften und Werten finden Sie in der Referenz [Microsoft.OperationalInsights/workspaces template](/azure/templates/microsoft.operationalinsights/2015-11-01-preview/workspaces). 

3. Speichern Sie diese Datei unter dem Namen **deploylaworkspacetemplate.json** in einem lokalen Ordner.

4. Nun können Sie die Vorlage bereitstellen. Verwenden Sie entweder PowerShell oder die Befehlszeile, um den Arbeitsbereich zu erstellen, und geben Sie dabei den Namen und Speicherort des Arbeitsbereichs als Teil des Befehls an. Der Arbeitsbereichsname muss in allen Azure-Abonnements global eindeutig sein.

   * Führen Sie bei Verwendung von PowerShell die folgenden Befehle in dem Ordner mit der Vorlage aus:
   
        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json -workspaceName <workspace-name> -location <location>
        ```

   * Führen Sie bei Verwendung der Befehlszeile die folgenden Befehle in dem Ordner mit der Vorlage aus:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json --workspaceName <workspace-name> --location <location>
        ```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung ähnlich der folgenden mit dem Ergebnis angezeigt:<br><br> ![Beispielergebnis nach abgeschlossener Bereitstellung](./media/template-workspace-configuration/template-output-01.png)

## <a name="configure-a-log-analytics-workspace"></a>Konfigurieren eines Log Analytics-Arbeitsbereichs

Das folgende Vorlagenbeispiel veranschaulicht Folgendes:

1. Hinzufügen von Lösungen zum Arbeitsbereich
2. Erstellen gespeicherter Suchvorgänge. Um sicherzustellen, dass gespeicherte Suchvorgänge nicht versehentlich von Bereitstellungen überschrieben werden, sollte in der Ressource „savedSearches“ eine eTag-Eigenschaft zum Überschreiben und Beibehalten der Idempotenz gespeicherter Suchen hinzugefügt werden.
3. Erstellen einer gespeicherten Funktion. Das eTag sollte hinzugefügt werden, um die Funktion zu überschreiben und Idempotenz aufrechtzuerhalten.
4. Erstellen einer Computergruppe
5. Aktivieren der Sammlung von IIS-Protokollen auf Computern mit installiertem Windows-Agent
6. Sammeln von Leistungsindikatoren logischer Datenträger von Linux-Computern (Prozentsatz verwendeter I-Knoten, MB frei, Prozentsatz des verwendeten Speicherplatzes, Übertragungen/s, Lesevorgänge/s, Schreibvorgänge/s)
7. Sammeln von Syslog-Ereignissen auf Linux-Computern
8. Sammeln von Fehler- und Warnereignissen aus dem Anwendungsereignisprotokoll von Windows-Computern
9. Erfassen des Leistungsindikators „Verfügbarer Arbeitsspeicher in MB“ von Windows-Computern
10. Sammeln von IIS-Protokollen und Windows-Ereignisprotokollen, die von der Azure-Diagnose in ein Speicherkonto geschrieben werden
11. Sammeln von benutzerdefinierten Protokollen auf Windows-Computern

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Workspace name"
      }
    },
    "sku": {
      "type": "string",
      "allowedValues": [
        "PerGB2018",
        "Free",
        "Standalone",
        "PerNode",
        "Standard",
        "Premium"
      ],
      "defaultValue": "pergb2018",
      "metadata": {
        "description": "Pricing tier: pergb2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
      }
    },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
      }
    },
    "immediatePurgeDataOn30Days": {
      "type": "bool",
      "defaultValue": "[bool('false')]",
      "metadata": {
        "description": "If set to true, changing retention to 30 days will immediately delete older data. Use this with extreme caution. This only applies when retention is being set to 30 days."
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "australiacentral",
        "australiaeast",
        "australiasoutheast",
        "brazilsouth",
        "canadacentral",
        "centralindia",
        "centralus",
        "eastasia",
        "eastus",
        "eastus2",
        "francecentral",
        "japaneast",
        "koreacentral",
        "northcentralus",
        "northeurope",
        "southafricanorth",
        "southcentralus",
        "southeastasia",
        "uksouth",
        "ukwest",
        "westcentralus",
        "westeurope",
        "westus",
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location in which to create the workspace."
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the storage account with Azure diagnostics output"
      }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
      "type": "string",
      "metadata": {
        "description": "The resource group name containing the storage account with Azure diagnostics output"
      }
    },
    "customLogName": {
      "type": "string",
      "metadata": {
        "description": "The custom log name"
      }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2017-03-15-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "retentionInDays": "[parameters('dataRetention')]",
        "features": {
          "immediatePurgeDataOn30Days": "[parameters('immediatePurgeDataOn30Days')]"
        },
        "sku": {
          "name": "[parameters('sku')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-03-20",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "eTag": "*",
            "category": "VMSS",
            "displayName": "VMSS Instance Count",
            "query": "Event | where Source == \"ServiceFabricNodeBootstrapAgent\" | summarize AggregatedValue = count() by Computer",
            "version": 1
          }
        },
        {
          "apiVersion": "2017-04-26-preview",
          "name": "Cross workspace function",
          "type": "savedSearches",
            "dependsOn": [
             "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
            ],
            "properties": {
              "etag": "*",
              "displayName": "failedLogOnEvents",
              "category": "Security",
              "FunctionAlias": "failedlogonsecurityevents",
              "query": "
                union withsource=SourceWorkspace
                workspace('workspace1').SecurityEvent,
                workspace('workspace2').SecurityEvent,
                workspace('workspace3').SecurityEvent,
                | where EventID == 4625"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "dataSources",
          "name": "[concat(parameters('workspaceName'), parameters('customLogName'))]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', '/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLog",
          "properties": {
            "customLogName": "[parameters('customLogName')]",
            "description": "this is a description",
            "extractions": [
              {
                "extractionName": "TimeGenerated",
                "extractionProperties": {
                  "dateTimeExtraction": {
                    "regex": [
                      {
                        "matchIndex": 0,
                        "numberdGroup": null,
                        "pattern": "((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]"
                      }
                    ]
                  }
                },
                "extractionType": "DateTime"
              }
            ],
            "inputs": [
              {
                "location": {
                  "fileSystemLocations": {
                    "linuxFileTypeLogPaths": null,
                    "windowsFileTypeLogPaths": [
                      "[concat('c:\\Windows\\Logs\\',parameters('customLogName'))]"
                    ]
                  }
                },
                "recordDelimiter": {
                  "regexDelimiter": {
                    "matchIndex": 0,
                    "numberdGroup": null,
                    "pattern": "(^.*((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9].*$)"
                  }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-03-20",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [
              "wad-iis-logfiles"
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceName": {
      "type": "string",
      "value": "[parameters('workspaceName')]"
    },
    "provisioningState": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').provisioningState]"
    },
    "source": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').source]"
    },
    "customerId": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').customerId]"
    },
    "sku": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').sku.name]"
    },
    "retentionInDays": {
      "type": "int",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').retentionInDays]"
    },
    "immediatePurgeDataOn30Days": {
      "type": "bool",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').features.immediatePurgeDataOn30Days]"
    },
    "portalUrl": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').portalUrl]"
    }
  }
}
```

### <a name="deploying-the-sample-template"></a>Bereitstellen der Beispielvorlage

So stellen Sie die Beispielvorlage bereit

1. Speichern Sie das angefügte Beispiel in einer Datei, z.B. `azuredeploy.json`. 
2. Bearbeiten Sie die Vorlage, um die gewünschte Konfiguration zu erstellen.
3. Verwenden Sie PowerShell oder die Befehlszeile zum Bereitstellen der Vorlage.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Befehlszeile

```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Resource Manager-Beispielvorlagen

Der Katalog mit Azure-Schnellstartvorlagen enthält u.a. folgende Vorlagen für Log Analytics:

* [Bereitstellen eines virtuellen Computers unter Windows mit der Log Analytics-VM-Erweiterung](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Bereitstellen eines virtuellen Computers unter Linux mit der Log Analytics-VM-Erweiterung](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Überwachen von Azure Site Recovery mit einem vorhandenen Log Analytics-Arbeitsbereich](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Überwachen von Azure-Web-Apps mit einem vorhandenen Log Analytics-Arbeitsbereich](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Hinzufügen eines vorhandenen Speicherkontos zu Log Analytics](https://azure.microsoft.com/resources/templates/oms-existing-storage-account/)

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen des Windows-Agents für virtuelle Azure-Computer mithilfe einer Resource Manager-Vorlage](../../virtual-machines/extensions/oms-windows.md).

* [Bereitstellen des Linux-Agents für virtuelle Azure-Computer mithilfe einer Resource Manager-Vorlage](../../virtual-machines/extensions/oms-linux.md).
