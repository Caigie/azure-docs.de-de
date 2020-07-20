---
title: Erstellen von Azure Machine Learning-Datasets für den Datenzugriff
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Azure Machine Learning-Datasets erstellen, um auf Ihre Daten für Machine Learning-Experimentausführungen zuzugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 06/29/2020
ms.openlocfilehash: baa238f36c41b5f494e8748cd5cd563bd212f483
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610709"
---
# <a name="create-azure-machine-learning-datasets"></a>Erstellen von Azure Machine Learning-Datasets

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning-Datasets erstellen, um auf Daten für Ihre lokalen oder remotebasierten Experimente zuzugreifen. Informationen dazu, welche Rolle Datasets im Workflow für den Datenzugriff in Azure Machine Learning spielen, finden Sie im Artikel [Datenzugriff in Azure Machine Learning](concept-data.md#data-workflow).

Azure Machine Learning-Datasets ermöglichen Folgendes:

* Aufbewahren einer einzelnen Datenkopie in Ihrem Speicher, auf die durch Datasets verwiesen wird

* Nahtloses Zugreifen auf Daten während des Modelltrainings, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssen

* Freigeben von Daten und Zusammenarbeiten mit anderen Benutzern

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Wenn Sie keines haben, erstellen Sie ein kostenloses Konto, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!NOTE]
> Einige Datasetklassen sind vom Paket [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) abhängig, das nur mit 64-Bit Python kompatibel ist. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="compute-size-guidance"></a>Leitfaden für die Computegröße

Wenn Sie ein Dataset erstellen, überprüfen Sie Ihre Computeverarbeitungsleistung und die Größe der Daten im Arbeitsspeicher. Die Größe der Daten im Speicher ist nicht mit der Größe der Daten in einem Datenrahmen identisch. Beispielsweise können Daten in CSV-Dateien sich in einem Datenrahmen um das10-fache ausdehnen, sodass aus einer 1 GB großen CSV-Datei 10 GB in einem Datenrahmen werden können. 

Der Hauptfaktor ist die Größe des Datasets im Arbeitsspeicher, d. h. als Datenrahmen. Wir empfehlen, dass Ihre Computegröße und Verarbeitungsleistung der doppelten Größe des RAM entsprechen. Wenn Ihr Datenrahmen also 10 GB groß ist, benötigen Sie ein Computeziel mit mehr als 20 GB RAM, um sicherzustellen, dass der Datenrahmen bequem in den Arbeitsspeicher passt und verarbeitet werden kann. Wenn Ihre Daten komprimiert sind, können sie sich weiter ausdehnen. 20 GB Daten mit relativ geringer Dichte, die im komprimierten Parquet-Format gespeichert sind, können sich auf etwa 800 GB im Arbeitsspeicher ausdehnen. Da in Parquet-Dateien Daten in einem Spaltenformat gespeichert werden, müssen Sie, wenn Sie nur die Hälfte der Spalten benötigen, nur etwa 400 GB in den Arbeitsspeicher laden.
 
Wenn Sie Pandas verwenden, gibt es keinen Grund für mehr als 1 vCPU, denn mehr wird nicht benötigt. Sie können über Modin und Dask/Ray auf einer einzigen Azure Machine Learning-Compute-Instanz bzw. einem Knoten leicht auf mehrere vCPUs parallelisieren und bei Bedarf auf einen großen Cluster aufskalieren, indem Sie einfach `import pandas as pd` in `import modin.pandas as pd` ändern. 
 
Wenn Sie keine ausreichend große virtuelle Datei für die Daten erhalten können, haben Sie zwei Möglichkeiten. Sie können ein Framework wie Spark oder Dask verwenden, um die Verarbeitung der Daten „außerhalb des Arbeitsspeichers“ durchzuführen, was heißt, dass der Datenrahmen partitionweise in den Arbeitsspeicher geladen und verarbeitet wird, wobei das Endergebnis am Ende gesammelt wird. Wenn dies zu langsam ist, können Sie mit Spark oder Dask zu einem Cluster aufskalieren, der dennoch interaktiv genutzt werden kann. 

## <a name="dataset-types"></a>Datasettypen

Es gibt zwei Arten von Datasettypen – abhängig davon, wie Benutzer sie beim Trainieren verwenden:

* [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) stellt Daten in einem tabellarischen Format dar, indem die bereitgestellte Datei oder Liste von Dateien analysiert wird. Dadurch erhalten Sie die Möglichkeit, die Daten in einem Pandas- oder Spark-Datenrahmen (DataFrame) zu materialisieren. Ein Objekt vom Typ `TabularDataset` kann auf der Grundlage von CSV-, TSV-, Parquet- und JSONL-Dateien sowie auf der Grundlage von SQL-Abfrageergebnissen erstellt werden. Eine umfassende Liste finden Sie unter [TabularDatasetFactory-Klasse](https://aka.ms/tabulardataset-api-reference).

* Die Klasse [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) verweist auf eine einzelne Datei oder auf mehrere Dateien in Ihren Datenspeichern oder unter öffentlichen URLs. Mithilfe dieser Methode können Sie die Dateien als FileDataset-Objekt in Ihre Compute-Instanz herunterladen oder einbinden. Die Dateien können in einem beliebigen Format vorliegen. Dies ermöglicht eine größere Bandbreite an Machine Learning-Szenarien (einschließlich Deep Learning). 

## <a name="create-datasets"></a>Erstellen von Datasets

Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Da die Daten an ihrem Speicherort verbleiben, fallen keine zusätzlichen Speicherkosten an. Sowohl Datasets des Typs `TabularDataset` als auch des Typs `FileDataset` können mithilfe des Python SDK oder von Azure Machine Learning Studio unter https://ml.azure.com erstellt werden.

Damit Azure Machine Learning auf die Daten zugreifen kann, müssen Datasets auf der Grundlage von Pfaden in [Azure-Datenspeichern](how-to-access-data.md) oder auf der Grundlage öffentlicher Web-URLs erstellt werden. 

### <a name="use-the-sdk"></a>Verwenden des SDK

So erstellen Sie Datasets auf der Grundlage eines [Azure-Datenspeichers](how-to-access-data.md) unter Verwendung des Python SDK:

1. Vergewissern Sie sich, dass Sie für den registrierten Azure-Datenspeicher über Zugriff vom Typ `contributor` oder `owner` verfügen.

2. Erstellen Sie das Dataset, indem Sie auf Pfade im Datenspeicher verweisen.

> [!Note]
> Ein Dataset kann aus mehreren Pfaden in mehreren Datenspeichern erstellt werden. Es gibt keine festen Grenzwerte für die Anzahl der Dateien oder die Datengröße, mit denen Sie ein Dataset erstellen können. Für jeden Datenpfad werden jedoch einige Anforderungen an den Speicherdienst gesendet, um zu prüfen, ob er auf eine Datei oder einen Ordner verweist. Dieser Aufwand kann zu einer Beeinträchtigung der Leistung oder zu einem Fehler führen. Ein Dataset, das auf einen Ordner mit 1000 Dateien verweist, bezieht sich auf einen Datenpfad. Es wird empfohlen, für eine optimale Leistung ein Dataset zu erstellen, das auf weniger als 100 Pfade in Datenspeichern verweist.

#### <a name="create-a-tabulardataset"></a>Erstellen eines TabularDataset-Elements

Verwenden Sie die Methode [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none--support-multi-line-false-) für die Klasse `TabularDatasetFactory`, um Dateien im CSV- oder TSV-Format zu lesen und ein nicht registriertes TabularDataset-Objekt zu erstellen. Wenn Sie Daten aus mehreren Dateien lesen, werden die Ergebnisse in einer Tabellendarstellung aggregiert. 

Mit dem folgenden Code werden der vorhandene Arbeitsbereich und der gewünschte Datenspeicher anhand des Namens abgerufen. Anschließend werden die Speicherorte von Datenspeicher und Datei dem `path`-Parameter übergeben, um ein neues TabularDataset `weather_ds` zu erstellen.

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
    
# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)

# create a TabularDataset from 3 file paths in datastore
datastore_paths = [(datastore, 'weather/2018/11.csv'),
                   (datastore, 'weather/2018/12.csv'),
                   (datastore, 'weather/2019/*.csv')]

weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

Beim Erstellen eines TabularDataset-Objekts werden Spaltendatentypen standardmäßig automatisch abgeleitet. Sollten die abgeleiteten Typen nicht Ihren Erwartungen entsprechen, können Sie den folgenden Code verwenden, um Spaltentypen anzugeben. Der Parameter `infer_column_type` ist nur für Datasets anwendbar, die aus durch Trennzeichen getrennten Dateien erstellt wurden. Weitere Informationen zu unterstützten Datentypen finden Sie [hier](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py).

> [!IMPORTANT] 
> Wenn sich Ihr Speicher hinter einem virtuellen Netzwerk oder einer Firewall befindet, wird nur die Erstellung eines Datasets über das SDK unterstützt. Achten Sie beim Erstellen Ihres Datasets darauf, dass Sie die Parameter `validate=False` und `infer_column_types=False` in Ihre `from_delimited_files()`-Methode einbeziehen. Dadurch wird die anfängliche Validierungsüberprüfung umgangen und sichergestellt, dass Sie Ihr Dataset aus diesen sicheren Dateien erstellen können. 

```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|Name|Geschlecht|Age|SibSp|Parch|Ticket|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|male|22,0|1|0|A/5 21171|7.2500||E
1|2|True|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|female|38,0|1|0|PC 17599|71.2833|C85|C
2|3|True|3|Heikkinen, Miss. Laina|female|26,0|0|0|STON/O2. 3101282|7.9250||E

Um ein Dataset aus einem In-Memory-Pandas-Dataframe zu erstellen, schreiben Sie die Daten in eine lokale Datei, z. B. eine CSV-Datei, und erstellen Sie Ihr Dataset aus dieser Datei. Der folgende Code veranschaulicht diesen Workflow.

```python
local_path = 'data/prepared.csv'
dataframe.to_csv(local_path)
upload the local file to a datastore on the cloud
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
from azureml.core import Workspace, Dataset

subscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'

workspace = Workspace(subscription_id, resource_group, workspace_name)

# get the datastore to upload prepared data
datastore = workspace.get_default_datastore()

# upload the local file from src_dir to the target_path in datastore
datastore.upload(src_dir='data', target_path='data')
# create a dataset referencing the cloud location
dataset = Dataset.Tabular.from_delimited_files(datastore.path('data/prepared.csv'))
```

Verwenden Sie die Methode [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none--query-timeout-30-) für die Klasse `TabularDatasetFactory`, um Daten aus Azure SQL-Datenbank zu lesen:

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

In TabularDataset-Objekten können Sie einen Zeitstempel auf der Grundlage einer Spalte in den Daten oder auf der Grundlage des Orts angeben, an dem die Pfadmusterdaten gespeichert sind, um ein Zeitreihenmerkmal zu aktivieren. Diese Angabe ermöglicht einfaches und effizientes Filtern nach Zeit.

Verwenden Sie die Methode [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) für die Klasse `TabularDataset`, um Ihre Zeitstempelspalte anzugeben und die Filterung nach Zeit zu ermöglichen. Weitere Informationen finden Sie unter [Demo einer tabellarischen, zeitreihenbezogenen API mit NOAA-Wetterdaten](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb).

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-a-filedataset"></a>Erstellen eines FileDataset-Elements

Verwenden Sie die Methode [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) für die Klasse `FileDatasetFactory`, um Dateien in einem beliebigen Format zu laden und ein nicht registriertes FileDataset-Objekt zu erstellen. Wenn sich Ihr Speicher hinter einem virtuellen Netzwerk oder einer Firewall befindet, legen Sie den Parameter `validate =False` in Ihrer `from_files()`-Methode fest. Dadurch wird der erste Überprüfungsschritt umgangen und sichergestellt, dass Sie Ihr Dataset aus diesen sicheren Dateien erstellen können.

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [(datastore, 'animals')]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = ['https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
             'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz']
mnist_ds = Dataset.File.from_files(path=web_paths)
```

#### <a name="on-the-web"></a>Im Web 
In den folgenden Schritten und in der Animation wird gezeigt, wie ein Dataset in Azure Machine Learning Studio (https://ml.azure.com ) erstellt wird:

![Erstellen eines Datasets mithilfe der Benutzeroberfläche](./media/how-to-create-register-datasets/create-dataset-ui.gif)

So erstellen Sie ein Dataset im Studio
1. Melden Sie sich bei https://ml.azure.com an.
1. Wählen Sie **Datasets** im Abschnitt **Assets** im linken Bereich aus. 
1. Wählen Sie **Dataset erstellen** aus, um die Quelle Ihres Datasets auszuwählen. Bei dieser Quelle kann es sich um lokale Dateien, um einen Datenspeicher oder um öffentliche URLs handeln.
1. Wählen Sie **Tabellarisch** oder **Datei** als Datasettyp aus.
1. Wählen Sie **Weiter** aus, um das Formular **Datenspeicher- und Dateiauswahl** zu öffnen. In diesem Formular wählen Sie aus, wo das Dataset nach dem Erstellen aufbewahrt werden soll, sowie welche Datendateien für Ihr Dataset verwendet werden sollen. 
1. Wählen Sie **Weiter** aus, um die Formulare **Einstellungen und Vorschau** und **Schema** auszufüllen. Sie werden basierend auf dem Dateityp auf intelligente Weise aufgefüllt, und Sie können das Dataset in diesen Formularen vor der Erstellung weiter konfigurieren. 
1. Wählen Sie **Weiter** aus, um das Formular **Details bestätigen** zu überprüfen. Überprüfen Sie Ihre Auswahl, und erstellen Sie ein optionales Datenprofil für das Dataset. Weitere Informationen zur [Datenprofilerstellung](how-to-use-automated-ml-for-ml-models.md#profile). 
1. Wählen Sie **Erstellen** aus, um die Erstellung des Datasets abzuschließen.

## <a name="register-datasets"></a>Registrieren von Datasets

Registrieren Sie Ihre Datasets bei einem Arbeitsbereich, um den Erstellungsprozess abzuschließen. Verwenden Sie die Methode [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--create-new-version-false-), um Datasets bei Ihrem Arbeitsbereich zu registrieren, damit sie für andere freigegeben und für Experimente in Ihrem Arbeitsbereich wiederverwendet werden können:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Datasets, die über Azure Machine Learning Studio erstellt werden, werden automatisch beim Arbeitsbereich registriert.

## <a name="create-datasets-with-azure-open-datasets"></a>Erstellen von Datasets mit Azure Open Datasets

[Öffentliche Azure-Datasets](https://azure.microsoft.com/services/open-datasets/) sind kuratierte öffentliche Datasets, mit denen Sie Lösungen mit maschinellem Lernen szenariospezifische Features hinzufügen können, um genauere Modelle zu erzielen. Die Datasets umfassen gemeinfreie Daten für Wetter, Volkszählungen, Feiertage, öffentliche Sicherheit und Orte, mit denen Sie Machine Learning-Modelle trainieren und Vorhersagelösungen anreichern können. Open Datasets befindet sich in der Cloud unter Microsoft Azure und ist sowohl im SDK als auch in der Benutzeroberfläche des Arbeitsbereichs enthalten.

### <a name="use-the-sdk"></a>Verwenden des SDK

Möchten Sie Datasets mit Azure Open Datasets aus dem SDK erstellen, muss das Paket mit `pip install azureml-opendatasets` installiert worden sein. Jedes einzelne Dataset wird durch seine eigene Klasse im SDK dargestellt, und bestimmte Klassen sind entweder als `TabularDataset`, als `FileDataset` oder als beides verfügbar. Eine vollständige Liste der Klassen finden Sie in der [Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py).

Bestimmte Klassen können entweder als `TabularDataset` oder als `FileDataset` abgerufen werden, sodass Sie die Dateien direkt bearbeiten und/oder herunterladen können. Andere Klassen können ein Dataset **nur** abrufen, indem sie eine der Funktionen `get_tabular_dataset()` oder `get_file_dataset()` verwenden. Im folgenden Codebeispiel sind einige Beispiele für diese Typen von Klassen gezeigt.

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

Wenn Sie ein Dataset registrieren, das aus Open Datasets erstellt wurde, werden keine Daten sofort heruntergeladen, sondern die Daten werden später, wenn sie angefordert werden (z. B. während des Trainings), aus einem zentralen Speicherort abgerufen.

### <a name="use-the-ui"></a>Verwenden der Benutzeroberfläche

Sie können Datasets auch über die Benutzeroberfläche auf der Grundlage von Open Datasets-Klassen erstellen. Wählen Sie in Ihrem Arbeitsbereich unter **Assets** die Registerkarte **Datasets** aus. Wählen Sie im Dropdownmenü **Dataset erstellen** die Option **Aus Open Datasets** aus.

![Öffnen eines Datasets über die Benutzeroberfläche](./media/how-to-create-register-datasets/open-datasets-1.png)

Wählen Sie ein Dataset aus, indem Sie die entsprechende Kachel auswählen. (Über die Suchleiste kann gefiltert werden.) Wählen Sie **Weiter** aus.

![Dataset auswählen](./media/how-to-create-register-datasets/open-datasets-2.png)

Wählen Sie einen Namen, unter dem das Dataset registriert werden soll, und filtern Sie die Daten optional mithilfe der verfügbaren Filter. Filtern Sie in diesem Fall das Dataset der gesetzlichen Feiertage nach Zeitraum (ein Jahr) und Ländercode (nur USA). Klicken Sie auf **Erstellen**.

![Dataset-Parameter festlegen und Dataset erstellen](./media/how-to-create-register-datasets/open-datasets-3.png)

Das Dataset ist nun in Ihrem Arbeitsbereich unter **Datasets** verfügbar. Sie können es auf die gleiche Weise verwenden wie andere Datasets, die Sie erstellt haben.

## <a name="version-datasets"></a>Versionsverwaltung von Datasets

Sie können ein neues Dataset unter demselben Namen registrieren, indem Sie eine neue Version erstellen. Eine Datasetversion ermöglicht es, den Zustand der Daten zu markieren, sodass Sie eine bestimmte Version des Datasets für Experimente oder zukünftige Reproduktion verwenden können. Weitere Informationen zu Datasetversionen finden Sie [hier](how-to-version-track-datasets.md).
```Python
# create a TabularDataset from Titanic training data
web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
             'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```

## <a name="access-datasets-in-your-script"></a>Zugreifen auf Datasets in Ihrem Skript

Auf registrierte Datasets kann sowohl lokal als auch remote in Computeclustern wie Azure Machine Learning Compute zugegriffen werden. Verwenden Sie für den experimentübergreifenden Zugriff auf Ihr registriertes Dataset den folgenden Code, um anhand des Namens auf Ihren Arbeitsbereich und das registrierte Dataset zuzugreifen. Die Methode [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) für die Klasse `Dataset` gibt standardmäßig die neueste Version des im Arbeitsbereich registrierten Datasets zurück.

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="access-datasets-in-a-virtual-network"></a>Zugreifen auf Datasets in einem virtuellen Netzwerk

Wenn sich Ihr Arbeitsbereich in einem virtuellen Netzwerk befindet, müssen Sie das Dataset so konfigurieren, dass die Überprüfung übersprungen wird. Weitere Informationen zur Verwendung von Datenspeichern und Datasets in virtuellen Netzwerken finden Sie unter [Netzwerkisolation während Training und Rückschluss mit privaten virtuellen Netzwerken](how-to-enable-virtual-network.md#use-datastores-and-datasets).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie mit Datasets trainiert wird](how-to-train-with-datasets.md).
* Verwenden Sie automatisiertes Machine Learning, um [mit TabularDatasets zu trainieren](https://aka.ms/automl-dataset).
* Weitere Beispiele zum Trainieren von Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
