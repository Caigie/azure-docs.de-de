---
title: Kopieren von Daten in Dynamics (Common Data Service)
description: Erfahren Sie, wie mithilfe einer Kopieraktivität in eine Data Factory-Pipeline Daten aus Microsoft Dynamics CRM oder Microsoft Dynamics 365 (Common Data Service) in unterstützte Senkendatenspeicher oder aus unterstützten Quelldatenspeichern nach Dynamics CRM oder Dynamics 365 kopiert werden.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 05/06/2020
ms.openlocfilehash: 255c39eac2285a23403da2db893d9de8835f7d2c
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891532"
---
# <a name="copy-data-from-and-to-dynamics-365-common-data-service-or-dynamics-crm-by-using-azure-data-factory"></a>Kopieren von Daten aus und nach Dynamics 365 (Common Data Service) oder Dynamics CRM mithilfe von Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und nach Microsoft Dynamics 365 oder Dynamics CRM zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

Sie können Daten aus Dynamics 365 (Common Data Service) oder Dynamics CRM in jeden unterstützten Senkendatenspeicher kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in Dynamics 365 (Common Data Service) oder Dynamics CRM kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser Dynamics-Connector unterstützt die Dynamics-Versionen 7.x bis 9.x sowohl online als auch lokal. Dies umfasst insbesondere Folgendes:

- Version 7.x ist Dynamics CRM 2015 zugeordnet
- Version 8.x ist Dynamics CRM 2016 und der frühen Version von Dynamics 365 zugeordnet
- Version 9.x ist der neueren Version von Dynamics 365 zugeordnet

In der folgenden Tabelle finden Sie Informationen zu den unterstützten Authentifizierungstypen und Konfigurationen für die jeweiligen Dynamics-Versionen/-Produkte. (IFD ist die Abkürzung für „Internet Facing Deployment“ [Bereitstellung mit Internetzugriff].)

| Dynamics-Versionen | Authentifizierungstypen | Beispiele für verknüpfte Dienste |
|:--- |:--- |:--- |
| Common Data Service <br> Dynamics 365 (online) <br> Dynamics CRM Online | AAD-Dienstprinzipal <br> Office365 | [Dynamics Online und AAD-Dienstprinzipal oder Office 365-Authentifizierung](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 lokal mit IFD <br> Dynamics CRM 2016 lokal mit IFD <br> Dynamics CRM 2015 lokal mit IFD | IFD | [Dynamics lokal mit IFD und IFD-Authentifizierung](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Für Dynamics 365 werden insbesondere die folgenden Anwendungstypen unterstützt:

- Dynamics 365 for Sales
- Dynamics 365 for Customer Service
- Dynamics 365 for Field Service
- Dynamics 365 for Project Service Automation
- Dynamics 365 for Marketing

Andere Anwendungstypen wie z.B. Finance and Operations, Talent usw. werden von diesem Connector nicht unterstützt.

Dieser Dynamics-Connector basiert auf [Dynamics XRM-Tools](https://docs.microsoft.com/dynamics365/customer-engagement/developer/build-windows-client-applications-xrm-tools).

>[!TIP]
>Sie können den [Dynamics AX-Connector](connector-dynamics-ax.md) verwenden, um Daten aus **Dynamics 365 for Finance and Operations** zu kopieren.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie diesen Connector bei der AAD-Dienstprinzipalauthentifizierung verwenden möchten, müssen Sie die Server-zu-Server-Authentifizierung (S2S) in Common Data Service oder Dynamics einrichten. Ausführliche Schritte finden Sie in [diesem Artikel](https://docs.microsoft.com/powerapps/developer/common-data-service/build-web-applications-server-server-s2s-authentication).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Dynamics verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Die folgenden Eigenschaften werden für den mit Dynamics verknüpften Dienst unterstützt.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 und Dynamics CRM Online

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **Dynamics**, **DynamicsCrm** oder **CommonDataServiceForApps** festgelegt werden. | Ja |
| deploymentType | Der Bereitstellungstyp der Dynamics-Instanz. Für Dynamics Online muss der Typ **Online** lauten. | Ja |
| serviceUri | Die Dienst-URL Ihrer Dynamics-Instanz, z.B. `https://adfdynamics.crm.dynamics.com` | Ja |
| authenticationType | Der Authentifizierungstyp für die Herstellung der Verbindung mit dem Dynamics-Server. Zulässige Werte sind: **AADServicePrincipal** oder **Office 365**. | Ja |
| servicePrincipalId | Geben Sie die Client-ID der Azure Active Directory-Anwendung an. | Ja, bei Verwendung der `AADServicePrincipal`-Authentifizierung |
| servicePrincipalCredentialType | Geben Sie die Art der Anmeldeinformationen für die Dienstprinzipalauthentifizierung an. Zulässige Werte sind: **ServicePrincipalKey** oder **ServicePrincipalCert**. | Ja, bei Verwendung der `AADServicePrincipal`-Authentifizierung |
| servicePrincipalCredential | Geben Sie die Anmeldeinformationen zum Dienstprinzipal an. <br>Wenn Sie `ServicePrincipalKey` als Anmeldeinformationstyp verwenden, kann `servicePrincipalCredential` eine Zeichenfolge (ADF verschlüsselt diese bei der Bereitstellung eines verknüpften Diensts) oder ein Verweis auf einen geheimen Schlüssel in Azure Key Vault sein. <br>Wenn Sie `ServicePrincipalCert` als Anmeldeinformation verwenden, sollte `servicePrincipalCredential` ein Verweis auf ein Zertifikat in Azure Key Vault sein. | Ja, bei Verwendung der `AADServicePrincipal`-Authentifizierung | 
| username | Geben Sie den Benutzernamen für die Herstellung der Verbindung mit Dynamics an. | Ja, bei Verwendung der `Office365`-Authentifizierung |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für „username“ angegeben haben. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja, bei Verwendung der `Office365`-Authentifizierung |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein für die Quelle. Ja für die Senke, wenn der mit der Quelle verknüpfte Dienst keine Integration Runtime aufweist. |

>[!NOTE]
>Der Dynamics-Connector hat zum Identifizieren Ihrer Dynamics CRM- oder 365 Online-Instanz früher die optionale Eigenschaft „organizationName“ verwendet. Diese Eigenschaft kann zwar immer noch verwendet werden, es wird jedoch empfohlen, stattdessen die neue Eigenschaft „serviceUri“ anzugeben, um eine bessere Leistung bei der Instanzermittlung zu erzielen.

**Beispiel: Dynamics Online unter Verwendung des AAD-Dienstprinzipals und der Schlüsselauthentifizierung**

```json
{  
    "name": "DynamicsLinkedService",  
    "properties": {  
        "type": "Dynamics",  
        "typeProperties": {  
            "deploymentType": "Online",  
            "serviceUri": "https://adfdynamics.crm.dynamics.com",  
            "authenticationType": "AADServicePrincipal",  
            "servicePrincipalId": "<service principal id>",  
            "servicePrincipalCredentialType": "ServicePrincipalKey",  
            "servicePrincipalCredential": "<service principal key>"
        },  
        "connectVia": {  
            "referenceName": "<name of Integration Runtime>",  
            "type": "IntegrationRuntimeReference"  
        }  
    }  
}  
```
**Beispiel: Dynamics Online unter Verwendung des AAD-Dienstprinzipals und der Zertifikatauthentifizierung**

```json
{ 
    "name": "DynamicsLinkedService", 
    "properties": { 
        "type": "Dynamics", 
        "typeProperties": { 
            "deploymentType": "Online", 
            "serviceUri": "https://adfdynamics.crm.dynamics.com", 
            "authenticationType": "AADServicePrincipal", 
            "servicePrincipalId": "<service principal id>", 
            "servicePrincipalCredentialType": "ServicePrincipalCert", 
            "servicePrincipalCredential": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<AKV reference>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<certificate name in AKV>" 
            } 
        }, 
        "connectVia": { 
            "referenceName": "<name of Integration Runtime>", 
            "type": "IntegrationRuntimeReference" 
        } 
    } 
} 
```

**Beispiel: Dynamics Online mit Office 365-Authentifizierung**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "Online",
            "serviceUri": "https://adfdynamics.crm.dynamics.com",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 und Dynamics CRM lokal mit IFD

*Die zusätzlichen Eigenschaften im Vergleich zu Dynamics Online lauten „hostName“ und „port“.*

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **Dynamics**, **DynamicsCrm** oder **CommonDataServiceForApps** festgelegt werden. | Ja |
| deploymentType | Der Bereitstellungstyp der Dynamics-Instanz. Muss für Dynamics lokal mit IFD **OnPremisesWithIfd** lauten.| Ja |
| hostName | Der Hostname des lokalen Dynamics-Servers. | Ja |
| port | Der Port des lokalen Dynamics-Servers. | Nein (Standard = 443) |
| organizationName | Der Organisationsname der Dynamics-Instanz. | Ja |
| authenticationType | Der Authentifizierungstyp für die Herstellung der Verbindung mit dem Dynamics-Server. Geben Sie für Dynamics lokal mit IFD **Ifd** an. | Ja |
| username | Geben Sie den Benutzernamen für die Herstellung der Verbindung mit Dynamics an. | Ja |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für „username“ angegeben haben. Sie können dieses Feld optional als SecureString markieren, um es sicher in ADF zu speichern, oder dieses Kennwort in Azure Key Vault speichern und von dort von der Kopieraktivität abrufen lassen, wenn Datenkopiervorgänge durchgeführt werden. Weitere Informationen finden Sie unter [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Quelle: Nein, Senke: Ja |

**Beispiel: Dynamics lokal mit IFD mit IFD-Authentifizierung**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dynamics-Dataset unterstützt werden.

Beim Kopieren von Daten aus und in Dynamics werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **DynamicsEntity**, **DynamicsCrmEntity** oder **CommonDataServiceForAppsEntity** festgelegt werden. |Ja |
| entityName | Der logische Name der abzurufenden Entität. | Nein für die Quelle (wenn „query“ in der Aktivitätsquelle angegeben ist), Ja für die Senke. |

**Beispiel:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "schema": [],
        "typeProperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von den Quell- und Senkentypen für Dynamics unterstützt werden.

### <a name="dynamics-as-a-source-type"></a>Dynamics als Quelltyp

Beim Kopieren von Daten aus Dynamics werden die folgenden Eigenschaften im Abschnitt **source** der Copy-Aktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Copy-Aktivität muss auf **DynamicsSource**, **DynamicsCrmSource** oder **CommonDataServiceForAppsSource** festgelegt werden. | Ja |
| Abfrage | FetchXML ist eine proprietäre Abfragesprache, die in Dynamics (online und lokal) verwendet wird. Siehe folgendes Beispiel. Weitere Informationen finden Sie unter [Erstellen von Abfragen mit FetchXML](https://msdn.microsoft.com/library/gg328332.aspx). | Nein (wenn „entityName“ im Dataset angegeben ist) |

>[!NOTE]
>Die PK-Spalte wird immer herauskopiert. Dies gilt auch, wenn sie nicht in der Spaltenprojektion enthalten ist, die Sie in der FetchXML-Abfrage konfigurieren.

> [!IMPORTANT]
>- Wenn Sie Daten aus Dynamics kopieren, ist die explizite Spaltenzuordnung von Dynamics zur Senke optional, sie wird jedoch dringend empfohlen, um ein deterministisches Kopierergebnis sicherzustellen.
>- Wenn Sie ein Schema in die Erstellungsbenutzeroberfläche importieren, leitet ADF das Schema ab, indem Stichproben der oberen Zeilen des Dynamics-Abfrageergebnisses entnommen werden, um die Quellspaltenliste zu initialisieren. In diesem Fall werden Spalten ohne Werte in den oberen Zeilen ausgelassen. Dasselbe Verhalten gilt für Kopiervorgänge, wenn keine explizite Zuordnung vorliegt. Sie können weitere Spalten zur Zuordnung hinzufügen und überprüfen. Dieser Vorgang wird während der Laufzeit des Kopiervorgangs berücksichtigt.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>FetchXML-Beispielabfrage

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-a-sink-type"></a>Dynamics als Senkentyp

Beim Kopieren von Daten in Dynamics werden die folgenden Eigenschaften im Abschnitt **source** der Copy-Aktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Copy-Aktivität muss auf **DynamicsSink**, **DynamicsCrmSink** oder **CommonDataServiceForAppsSink** festgelegt werden. | Ja |
| writeBehavior | Das Schreibverhalten des Vorgangs.<br/>Der zulässige Wert ist **Upsert**. | Ja |
| alternateKeyName | Geben Sie zum Ausführen von „upsert“ den für Ihre Entität definierten alternativen Schlüsselnamen an. | Nein |
| writeBatchSize | Die Zeilenanzahl der Daten, die in jedem Batch in Dynamics geschrieben werden. | Nein (Standard = 10) |
| ignoreNullValues | Gibt an, ob NULL-Werte von Eingabedaten (außer Schlüsselfeldern) während des Schreibvorgangs ignoriert werden sollen.<br/>Zulässige Werte sind **true** und **false**.<br>- **true**: Lässt die Daten im Zielobjekt unverändert, wenn Sie einen upsert- oder update-Vorgang ausführen. Fügt beim Ausführen eines insert-Vorgangs einen definierten Standardwert ein.<br/>- **false**: Aktualisiert die Daten im Zielobjekt auf NULL, wenn Sie einen upsert- oder update-Vorgang ausführen. Fügt beim Ausführen eines insert-Vorgangs einen NULL-Wert ein. | Nein (Standardwert ist „false“) |

>[!NOTE]
>Sowohl für die Senke **writeBatchSize** als auch für die Kopieraktivität **[parallelCopies](copy-activity-performance-features.md#parallel-copy)** für die Dynamics-Senke lautet der Standardwert 10. Daher werden 100 Datensätze gleichzeitig an Dynamics übermittelt.

Für Dynamics 365 online besteht ein Limit von [2 gleichzeitigen Batchaufrufen pro Organisation](https://msdn.microsoft.com/library/jj863631.aspx#Run-time%20limitations). Bei Überschreitung dieses Grenzwerts wird ein „Fehler aufgrund von Serverauslastung“ ausgelöst, bevor die erste Anforderung jemals ausgeführt wird. Sorgen Sie dafür, dass writeBatchSize den Wert 10 nicht überschreitet, um eine solche Einschränkung gleichzeitiger Aufrufe zu vermeiden.

Die optimale Kombination von **writeBatchSize** und **parallelCopies** hängt vom Schema Ihrer Entität ab, z.B. Anzahl der Spalten, Zeilengröße, Anzahl der Plug-Ins/Workflows/Workflowaktivitäten, die mit diesen Aufrufen verknüpft sind usw. Die Standardeinstellung von 10 writeBatchSize * 10 parallelCopies ist die Empfehlung gemäß Dynamics-Dienst, die für die meisten Dynamics-Entitäten funktionieren würde, jedoch möglicherweise nicht bei optimaler Leistung. Sie können die Leistung optimieren, indem Sie die Kombination in Ihren Kopieraktivitätseinstellungen anpassen.

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Datentypzuordnung für Dynamics

Beim Kopieren von Daten aus Dynamics werden die folgenden Zuordnungen von Dynamics-Datentypen zu Data Factory-Zwischendatentypen verwendet. Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

Konfigurieren Sie anhand der folgenden Zuordnungstabelle den entsprechenden Data Factory-Datentyp in der Datasetstruktur auf Grundlage des Dynamics-Quelldatentyps.

| Dynamics-Datentyp | Data Factory-Zwischendatentyp | Als Quelle unterstützt | Als Senke unterstützt |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Long | ✓ | ✓ |
| AttributeTypeCode.Boolean | Boolean | ✓ | ✓ |
| AttributeType.Customer | Guid | ✓ | |
| AttributeType.DateTime | Datetime | ✓ | ✓ |
| AttributeType.Decimal | Decimal | ✓ | ✓ |
| AttributeType.Double | Double | ✓ | ✓ |
| AttributeType.EntityName | String | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | Guid | ✓ | ✓ (mit Zuordnung eines einzelnen Ziels) |
| AttributeType.ManagedProperty | Boolean | ✓ | |
| AttributeType.Memo | String | ✓ | ✓ |
| AttributeType.Money | Decimal | ✓ | ✓ |
| AttributeType.Owner | Guid | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | Guid | ✓ | ✓ |
| AttributeType.String | String | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |

> [!NOTE]
> Die Dynamics-Datentypen AttributeType.CalendarRules, AttributeType.MultiSelectPicklist und AttributeType.PartyList werden nicht unterstützt.

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
