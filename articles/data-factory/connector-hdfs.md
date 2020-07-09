---
title: Kopieren von Daten aus HDFS mithilfe von Azure Data Factory
description: Erfahren Sie, wie Daten aus einer Cloud- oder lokalen HDFS-Quelle mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/15/2020
ms.author: jingwang
ms.openlocfilehash: f39fb50c5044cacb04b3b147a0160dd23c9eb2d0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83657095"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Kopieren von Daten aus HDFS mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-hdfs-connector.md)
> * [Aktuelle Version](connector-hdfs.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie Daten vom HDFS-Server kopieren. Informationen zu Azure Data Factory finden Sie im [Einführungsartikel](introduction.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Der HDFS-Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

Der HDFS-Connector unterstützt insbesondere Folgendes:

- Kopieren von Dateien mithilfe der **Windows**-Authentifizierung (Kerberos) oder **anonymen** Authentifizierung
- Kopieren von Dateien mithilfe des **WebHDFS**-Protokolls oder der **integrierten DistCp**-Unterstützung
- Kopieren von Dateien im jeweiligen Zustand oder Analysieren bzw. Generieren von Dateien mit den [unterstützten Dateiformaten und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

> [!NOTE]
> Stellen Sie sicher, dass die Integration Runtime auf **ALLE** folgenden Komponenten des Hadoop-Clusters zugreifen kann: [Namenknotenserver]:[Namenknotenport] und [Datenknotenserver]:[Datenknotenport]. [Namenknotenport] ist standardmäßig 50070, [Datenknotenport] ist standardmäßig 50075.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für HDFS verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit HDFS verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **Hdfs**. | Ja |
| url |Die URL für das HDFS. |Ja |
| authenticationType | Zulässige Werte sind: **Anonym** oder **Windows**. <br><br> Um Ihre lokale Umgebung zur Verwendung der **Kerberos-Authentifizierung** für den HDFS-Connector einzurichten, lesen Sie [diesen Abschnitt](#use-kerberos-authentication-for-hdfs-connector). |Ja |
| userName |Der Benutzername für die Windows-Authentifizierung. Geben Sie für die Kerberos-Authentifizierung `<username>@<domain>.com` an. |Ja (für die Windows-Authentifizierung) |
| password |Das Kennwort für die Windows-Authentifizierung. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |Ja (für die Windows-Authentifizierung) |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Weitere Informationen finden Sie im Abschnitt [Voraussetzungen](#prerequisites). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

**Beispiel: Verwenden der anonymen Authentifizierung**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel: Verwenden der Windows-Authentifizierung**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für HDFS unter den `location`-Einstellungen in formatbasierten Datasets unterstützt:

| Eigenschaft   | BESCHREIBUNG                                                  | Erforderlich |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Die „type“-Eigenschaft unter `location` im Dataset muss auf **HdfsLocation** festgelegt werden. | Ja      |
| folderPath | Der Pfad zum Ordner. Wenn Sie Platzhalter verwenden möchten, um Ordner zu filtern, überspringen Sie diese Einstellung, und geben Sie entsprechende Aktivitätsquelleneinstellungen an. | Nein       |
| fileName   | Der Name der Datei unter dem angegebenen „folderPath“. Wenn Sie Platzhalter verwenden möchten, um Ordner zu filtern, überspringen Sie diese Einstellung, und geben Sie entsprechenden Aktivitätsquelleneinstellungen an. | Nein       |

**Beispiel:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der HDFS-Quelle unterstützt werden.

### <a name="hdfs-as-source"></a>HDFS als Quelle

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für HDFS unter den `storeSettings`-Einstellungen in der formatbasierten Kopierquelle unterstützt:

| Eigenschaft                 | BESCHREIBUNG                                                  | Erforderlich                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Die „type“-Eigenschaft unter `storeSettings` muss auf **HdfsReadSettings** festgelegt werden. | Ja                                           |
| ***Suchen Sie die zu kopierenden Dateien:*** |  |  |
| OPTION 1: statischer Pfad<br> | Kopieren Sie aus dem angegebenen Ordner/Dateipfad, der im Dataset angegeben ist. Wenn Sie alle Dateien aus einem Ordner kopieren möchten, geben Sie außerdem für `wildcardFileName` den Wert `*` an. |  |
| OPTION 2: Platzhalter<br>- wildcardFolderPath | Der Ordnerpfad mit Platzhalterzeichen, um Quellordner zu filtern. <br>Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält. <br>Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Nein                                            |
| OPTION 2: Platzhalter<br>- wildcardFileName | Der Dateiname mit Platzhalterzeichen unter dem angegebenen „folderPath/wildcardFolderPath“ für das Filtern von Quelldateien. <br>Zulässige Platzhalter sind: `*` (entspricht null oder mehr Zeichen) und `?` (entspricht null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält.  Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Ja |
| OPTION 3: eine Liste von Dateien<br>- fileListPath | Gibt an, dass ein bestimmter Satz von Dateien kopiert werden soll. Verweisen Sie auf eine Textdatei, die eine Liste der zu kopierenden Dateien enthält, und zwar eine Datei pro Zeile. Dies ist der relative Pfad zu dem im Dataset konfigurierten Pfad.<br/>Wenn Sie diese Option verwenden, dürfen Sie keinen Dateinamen im Dataset angeben. Weitere Beispiele finden Sie unter [Beispiele für Dateilisten](#file-list-examples). |Nein |
| ***Zusätzliche Einstellungen:*** |  | |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert und dort auch nicht erstellt. <br>Zulässige Werte sind **true** (Standard) und **false**.<br>Diese Eigenschaft gilt nicht, wenn Sie `fileListPath` konfigurieren. |Nein |
| modifiedDatetimeStart    | Dateifilterung basierend auf dem Attribut: Letzte Änderung. <br>Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.<br/>Diese Eigenschaft gilt nicht, wenn Sie `fileListPath` konfigurieren. | Nein                                            |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein                                            |
| ***DistCp-Einstellungen:*** |  | |
| distcpSettings | Eigenschaftengruppe bei Verwendung von HDFS DistCp | Nein |
| resourceManagerEndpoint | Der YARN-Ressourcen-Manager-Endpunkt | Ja, wenn DistCp verwendet wird |
| tempScriptPath | Ein Ordnerpfad zum Speichern von temporären DistCp-Befehlsskripts. Die Skriptdatei wird von Data Factory generiert und nach Abschluss des Kopierauftrags entfernt. | Ja, wenn DistCp verwendet wird |
| distcpOptions | Zusätzliche für den DistCp-Befehl bereitgestellte Optionen | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HdfsReadSettings",
                    "recursive": true,
                    "distcpSettings": {
                        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
                        "tempScriptPath": "/usr/hadoop/tempscript",
                        "distcpOptions": "-m 100"
                    }
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Beispiele für Ordner- und Dateifilter

Dieser Abschnitt beschreibt das sich ergebende Verhalten für den Ordnerpfad und den Dateinamen mit Platzhalterfiltern.

| folderPath | fileName             | recursive | Quellordnerstruktur und Filterergebnis (Dateien mit **Fettformatierung** werden abgerufen.) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (empty, use default) | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*`  | (empty, use default) | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*`  | `*.csv`              | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*`  | `*.csv`              | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |

### <a name="file-list-examples"></a>Beispiele für Dateilisten

In diesem Abschnitt wird das resultierende Verhalten beim Verwenden des Dateilistenpfads in der Quelle der Kopieraktivität beschrieben.

Angenommen, Sie haben die folgende Quellordnerstruktur und möchten die Dateien kopieren, deren Namen fett formatiert sind:

| Beispielquellstruktur                                      | Inhalt in „FileListToCopy.txt“                             | ADF-Konfiguration                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| root<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Metadaten<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | Datei1.csv<br>Unterordner1/Datei3.csv<br>Unterordner1/Datei5.csv | **Im Dataset:**<br>- Ordnerpfad: `root/FolderA`<br><br>**In der Quelle der Kopieraktivität:**<br>- Dateilistenpfad: `root/Metadata/FileListToCopy.txt` <br><br>Der Dateilistenpfad verweist auf eine Textdatei im selben Datenspeicher, der eine Liste der zu kopierenden Dateien enthält, und zwar eine Datei pro Zeile. Diese enthält den relativen Pfad zu dem im Dataset konfigurierten Pfad. |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>Kopieren von Daten aus einem HDFS mithilfe von DistCp

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) ist ein natives Hadoop-Befehlszeilentool, um verteilte Kopiervorgänge in einem Hadoop-Cluster auszuführen. Wenn ein DistCp-Befehl ausgeführt wird, werden zuerst alle zu kopierende Dateien aufgeführt und mehrere Zuordnungsaufträge im Hadoop-Cluster erstellt. Zudem wird für jede Zuordnung eine Binärkopie von der Quelle in der Senke erstellt.

Statt die selbstgehostete Integration Runtime auszuführen, können bei der Kopieraktivität mithilfe von DistCp Dateien im jeweiligen Zustand im Azure-Blob (einschließlich der [bereitgestellten Kopie](copy-activity-performance.md)) oder Azure Data Lake Store kopiert werden, sodass Ihr Cluster optimal zum Einsatz kommt. Die Kopieraktivität bietet einen besseren Kopierdurchsatz, insbesondere dann, wenn Sie über einen äußerst umfangreichen Cluster verfügen. Basierend auf Ihrer Konfiguration in Azure Data Factory erstellt die Kopieraktivität automatisch einen DistCp-Befehl, sendet diesen an Ihren Hadoop-Cluster und überwacht den Kopierstatus.

### <a name="prerequisites"></a>Voraussetzungen

Um mithilfe von DistCp Dateien im jeweiligen Zustand aus HDFS im Azure-Blob (einschließlich der bereitgestellten Kopie) oder Azure Data Lake Store kopieren zu können, müssen Sie sicherstellen, dass Ihr Hadoop-Cluster die folgenden Anforderungen erfüllt:

1. Die Dienste MapReduce und YARN sind aktiviert.
2. Sie verfügen über YARN Version 2.5 oder höher.
3. Der HDFS-Server ist in Ihren Zieldatenspeicher integriert, d.h. Azure-Blob oder Azure Data Lake Store:

    - Das Azure-Blobdateisystem wird nativ ab Hadoop 2.7 unterstützt. Sie müssen lediglich den JAR-Pfad in der Hadoop-Umgebungskonfiguration angeben.
    - Das Azure Data Lake Store-Dateisystem wird ab Hadoop 3.0.0-alpha1 verpackt. Wenn Ihr Hadoop-Cluster eine niedrigere Version als diese aufweist, müssen Sie JAR-Pakete im Zusammenhang mit ADLS (azure-datalake-store.jar) [hierüber](https://hadoop.apache.org/releases.html) manuell in den Cluster importieren und den JAR-Pfad in der Hadoop-Umgebungskonfiguration angeben.

4. Bereiten Sie einen temporären Ordner in HDFS vor. In diesem temp-Ordner werden DistCp-Shellskripts gespeichert, sodass Speicherplatz im KB-Bereich belegt wird.
5. Stellen Sie sicher, dass das im verknüpften HDFS-Dienst bereitgestellte Benutzerkonto über Berechtigungen zum a) Übermitteln einer Anwendung in YARN; und b) Erstellen eines Unterordners sowie zum Lesen von bzw. Schreiben in Dateien im oben genannten temp-Ordner verfügt.

### <a name="configurations"></a>Configurations

DistCp-bezogene Konfigurationen und Beispiele finden Sie im Abschnitt [HDFS als Quelle](#hdfs-as-source).

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Verwenden der Kerberos-Authentifizierung für den HDFS-Connector

Zur Einrichtung der lokalen Umgebung für die Verwendung der Kerberos-Authentifizierung im HDFS-Connector gibt es zwei Optionen. Wählen Sie die Option, die in Ihrem Fall besser passt.
* Option 1: [Beitreten zum Computer mit der selbstgehosteten Integrationslaufzeit im Kerberosbereich](#kerberos-join-realm)
* Option 2: [Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberosbereich](#kerberos-mutual-trust)

### <a name="option-1-join-self-hosted-integration-runtime-machine-in-kerberos-realm"></a><a name="kerberos-join-realm"></a>Option 1: Beitreten zum Computer mit der selbstgehosteten Integrationslaufzeit im Kerberosbereich

#### <a name="requirements"></a>Requirements (Anforderungen)

* Der Computer mit der selbstgehosteten Integrationslaufzeit muss dem Kerberosbereich beitreten und darf in keine Windows-Domäne eingebunden sein.

#### <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration

**Bezüglich der selbstgehosteten Integrationslaufzeit:**

1.  Führen Sie das **Ksetup**-Hilfsprogramm aus, um den Kerberos-KDC-Server (Key Distribution Center) und -Bereich zu konfigurieren.

    Der Computer muss als Mitglied einer Arbeitsgruppe konfiguriert werden, da sich Kerberos-Bereiche von Windows-Domänen unterscheiden. Sie erreichen dies, indem Sie den Kerberos-Bereich einrichten und einen KDC-Server hinzufügen, wie im Folgenden erläutert. Ersetzen Sie *REALM.COM* ggf. durch Ihren eigenen entsprechenden Bereich.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    Nach Ausführung dieser beiden Befehle **starten Sie den Computer neu**.

2.  Überprüfen Sie die Konfiguration mit dem **Ksetup**-Befehl. Die Ausgabe sollte wie folgt sein:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit dem Namen und Kennwort Ihres Kerberos-Prinzipals, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#linked-service-properties).

### <a name="option-2-enable-mutual-trust-between-windows-domain-and-kerberos-realm"></a><a name="kerberos-mutual-trust"></a>Option 2: Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberosbereich

#### <a name="requirements"></a>Requirements (Anforderungen)

*   Der Computer mit der selbstgehosteten Integrationslaufzeit muss einer Windows-Domäne beitreten.
*   Sie benötigen die Berechtigung zum Aktualisieren der Einstellungen des Domänencontrollers.

#### <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration

> [!NOTE]
> Ersetzen Sie REALM.COM und AD.COM im folgenden Tutorial ggf. durch Ihren eigenen Bereich und Domänencontroller.

**Auf dem KDC-Server:**

1. Bearbeiten Sie die KDC-Konfiguration in der Datei **krb5.conf** mithilfe der folgenden Konfigurationsvorlage so, dass KDC der Windows-Domäne vertraut. Standardmäßig befindet sich die Konfiguration unter **/etc/krb5.conf**.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   Führen Sie nach der Konfiguration einen **Neustart** des KDC-Diensts aus.

2. Bereiten Sie mit dem folgenden Befehl einen Prinzipal namens **krgtgt/REALM.COM\@AD.COM** auf dem KDC-Server vor:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. Fügen Sie in der HDFS-Dienstkonfigurationsdatei **hadoop.security.auth_to_local** diese Zeichenfolge hinzu: `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`.

**Auf dem Domänencontroller:**

1.  Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Richten Sie eine Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich ein. [password] ist das Kennwort für den Prinzipal **krbtgt/REALM.COM\@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Wählen Sie den in Kerberos verwendeten Verschlüsselungsalgorithmus aus.

    1. Wechseln Sie zu „Server-Manager > Gruppenrichtlinienverwaltung > Domäne > Gruppenrichtlinienobjekte > Standard- oder aktive Domänenrichtlinie“, und wählen Sie „Bearbeiten“.

    2. Wechseln Sie im Popupfenster **Gruppenrichtlinienverwaltungs-Editor** zu „Computerkonfiguration“ > „Richtlinien“ > „Windows-Einstellungen“ > „Sicherheitseinstellungen“ > „Lokale Richtlinien“ > „Sicherheitsoptionen“, und konfigurieren Sie die Einstellung **Netzwerksicherheit: Für Kerberos zulässige Verschlüsselungstypen konfigurieren**.

    3. Wählen Sie den Verschlüsselungsalgorithmus, der beim Herstellen der Verbindung mit KDC verwendet werden soll. Im Allgemeinen können Sie einfach alle Optionen auswählen.

        ![Konfigurieren von Verschlüsselungstypen für Kerberos](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Verwenden Sie den Befehl **Ksetup**, um den Verschlüsselungsalgorithmus anzugeben, der in dem bestimmten Bereich verwendet werden soll.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Erstellen Sie die Zuordnung zwischen dem Domänenkonto und dem Kerberos-Prinzipal, um den Kerberos-Prinzipal in der Windows-Domäne verwenden zu können.

    1. Starten Sie „Verwaltung > **Active Directory-Benutzer und -Computer**“.

    2. Konfigurieren Sie erweiterte Funktionen, indem Sie auf **Ansicht** > **Erweiterte Funktionen** klicken.

    3. Suchen Sie das Konto, für das Sie Zuordnungen erstellen möchten, und klicken Sie mit der rechten Maustaste, um **Namenszuordnungen** anzuzeigen. Klicken Sie dann auf die Registerkarte **Kerberos-Namen**.

    4. Fügen Sie einen Prinzipal aus dem Bereich hinzu.

        ![Sicherheitsidentität zuordnen](media/connector-hdfs/map-security-identity.png)

**Bezüglich der selbstgehosteten Integrationslaufzeit:**

* Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit Ihrem Domänenkonto oder Ihrem Kerberos-Prinzipal, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#linked-service-properties).

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="legacy-models"></a>Legacy-Modelle

>[!NOTE]
>Die folgenden Modelle werden aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das in den obigen Abschnitten erwähnte neue Modell zu verwenden, da das neue Modell nun von der ADF-Benutzeroberfläche für die Erstellung generiert wird.

### <a name="legacy-dataset-model"></a>Legacy-Datasetmodell

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf folgenden Wert festgelegt werden: **FileShare** |Ja |
| folderPath | Pfad zum Ordner. Platzhalterfilter werden unterstützt. Zulässige Platzhalter sind: `*` (entspricht Null oder mehr Zeichen) und `?` (entspricht Null oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn Ihr tatsächlicher Dateiname einen Platzhalter oder dieses Escapezeichen enthält. <br/><br/>Beispiele: Stammordner/Unterordner/. Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). |Ja |
| fileName |  **Name oder Platzhalterfilter** für die Dateien unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, verweist das Dataset auf alle Dateien im Ordner. <br/><br/>Für Filter sind folgende Platzhalter zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen).<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Ordnername einen Platzhalter oder dieses Escapezeichen enthält. |Nein |
| modifiedDatetimeStart | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Beachten Sie, dass die generelle Leistung der Datenverschiebung beeinträchtigt wird, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| modifiedDatetimeEnd | Dateifilterung basierend auf dem Attribut: Letzte Änderung. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Beachten Sie, dass die generelle Leistung der Datenverschiebung beeinträchtigt wird, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird.  Wenn `modifiedDatetimeStart` den datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist.  Wenn `modifiedDatetimeEnd` den datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| format | Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen.<br/><br/>Für das Analysieren von Dateien mit einem bestimmten Format werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs-legacy.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs-legacy.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| compression | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Folgende Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und **ZipDeflate**.<br/>Folgende Ebenen werden unterstützt: **Optimal** und **Fastest**. |Nein |

>[!TIP]
>Wenn Sie alle Dateien eines Ordners kopieren möchten, geben Sie nur **folderPath** an.<br>Wenn Sie eine einzelne Datei mit einem bestimmten Namen kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Dateinamen an.<br>Wenn Sie eine Teilmenge der Dateien eines Ordners kopieren möchten, geben Sie **folderPath** mit dem Ordner und **fileName** mit dem Platzhalterfilter an.

**Beispiel:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Legacy-Kopieraktivität: Quellenmodell

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **HdfsSource** |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Beachten Sie Folgendes: Wenn „recursive“ auf TRUE festgelegt und die Senke ein dateibasierter Speicher ist, wird ein leerer Ordner/Unterordner nicht in die Senke kopiert bzw. nicht in ihr erstellt.<br/>Zulässige Werte sind **true** (Standard) oder **false**. | Nein |
| distcpSettings | Eigenschaftengruppe bei Verwendung von HDFS DistCp | Nein |
| resourceManagerEndpoint | Der YARN-Ressourcen-Manager-Endpunkt | Ja, wenn DistCp verwendet wird |
| tempScriptPath | Ein Ordnerpfad zum Speichern von temporären DistCp-Befehlsskripts. Die Skriptdatei wird von Data Factory generiert und nach Abschluss des Kopierauftrags entfernt. | Ja, wenn DistCp verwendet wird |
| distcpOptions | Zusätzliche für den DistCp-Befehl bereitgestellte Optionen | Nein |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein |

**Beispiel: HDFS-Quelle in der Kopieraktivität unter Verwendung von DistCp**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
