---
title: Erstellen, Ausführen und Nachverfolgen von ML-Pipelines
titleSuffix: Azure Machine Learning
description: Erstellen Sie eine Machine Learning-Pipeline mit dem Azure Machine Learning SDK für Python, und führen Sie sie aus. Verwenden Sie Machine Learning-Pipelines (ML) zum Erstellen und Verwalten von Workflows, die Machine Learning-Phasen zusammenfügen. Zu diesen Phasen gehören beispielsweise Datenaufbereitung, Modelltraining, Modellimplementierung und Rückschluss/Bewertung.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 12/05/2019
ms.custom: seodec18, tracking-python
ms.openlocfilehash: 50bd9d620fac4031ff398bb13d40b288488dda95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559384"
---
# <a name="create-and-run-machine-learning-pipelines-with-azure-machine-learning-sdk"></a>Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning SDK

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie eine [Machine Learning-Pipeline](concept-ml-pipelines.md) mit dem [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) erstellen, veröffentlichen, ausführen und verfolgen.  Verwenden Sie **ML-Pipelines**, um einen Workflow zu erstellen, der verschiedene ML-Phasen zusammenfügt. Veröffentlichen Sie diese Pipeline dann in Ihrem Azure Machine Learning-Arbeitsbereich, um später auf die Pipeline zuzugreifen oder sie für andere Personen freizugeben.  ML-Pipelines eignen sich ideal für Szenarien mit Batchbewertungen, in denen verschiedene Berechnungen ausgeführt, Schritte wiederverwendet statt wiederholt ausgeführt und ML-Workflows für andere Personen freigegeben werden.

Sie können zwar zur CI/CD-Automatisierung von ML-Aufgaben eine andere Art von Pipeline verwenden, die als [Azure-Pipeline](https://docs.microsoft.com/azure/devops/pipelines/targets/azure-machine-learning?context=azure%2Fmachine-learning%2Fservice%2Fcontext%2Fml-context&view=azure-devops&tabs=yaml) bezeichnet wird, aber diese Art von Pipeline wird nie in Ihrem Arbeitsbereich gespeichert. [Vergleichen Sie diese unterschiedlichen Pipelines](concept-ml-pipelines.md#which-azure-pipeline-technology-should-i-use).

Jede Phase einer ML-Pipeline, z. B. Datenvorbereitung und Modelltraining, kann einen oder mehrere Schritte umfassen.

Die von Ihnen erstellten ML-Pipelines sind für die Mitglieder Ihres Azure Machine Learning-[Arbeitsbereichs](how-to-manage-workspace.md) sichtbar. 

ML-Pipelines verwenden Remotecomputeziele für die Berechnung und Speicherung der mit dieser Pipeline verbundenen Zwischen- und Enddaten. Pipelines können Daten in unterstützten [Azure Storage](https://docs.microsoft.com/azure/storage/)-Speicherorten lesen und schreiben.

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

## <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie einen [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md), um Ihre gesamten Pipelineressourcen aufzunehmen.

* [Konfigurieren Sie Ihre Entwicklungsumgebung](how-to-configure-environment.md) für die Installation des Azure Machine Learning SDK, oder verwenden Sie eine [Azure Machine Learning-Compute-Instanz (Vorschau)](concept-compute-instance.md) mit bereits installiertem SDK.

Beginnen Sie mit dem Anfügen Ihres Arbeitsbereichs:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

## <a name="set-up-machine-learning-resources"></a>Einrichten von Machine Learning-Ressourcen

Erstellen Sie die Ressourcen, die zum Ausführen einer ML-Pipeline erforderlich sind:

* Richten Sie einen Datenspeicher ein, über den auf die in den Pipelineschritten benötigten Daten zugegriffen werden kann.

* Konfigurieren Sie ein `Dataset`-Objekt, um auf beständige Daten zu verweisen, die sich in einem Datenspeicher befinden oder dort zugänglich sind. Konfigurieren Sie ein `PipelineData`-Objekt für temporäre Daten, die zwischen den Pipelineschritten übermittelt werden. 

* Richten Sie die [Computeziele](concept-azure-machine-learning-architecture.md#compute-targets) ein, auf denen Ihre Pipelineschritte ausgeführt werden.

### <a name="set-up-a-datastore"></a>Einrichten eines Datenspeichers

Ein Datenspeicher speichert die Daten für den Zugriff durch die Pipeline. Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher. Sie können weitere Datenspeicher registrieren. 

Wenn Sie Ihren Arbeitsbereich erstellen, werden [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) und [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) an den Arbeitsbereich angefügt. Ein Standarddatenspeicher wird registriert, um eine Verbindung mit Azure Blob Storage herzustellen. Weitere Informationen finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträgern](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore 
def_data_store = ws.get_default_datastore()

# Get the blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")

# Get file storage associated with the workspace
def_file_store = Datastore(ws, "workspacefilestore")

```

Laden Sie Datendateien oder Verzeichnisse in den Datenspeicher hoch, damit sie von Ihren Pipelines aus zugänglich sind. In diesem Beispiel wird ein Blobspeicher als Datenspeicher verwendet:

```python
def_blob_store.upload_files(
    ["iris.csv"],
    target_path="train-dataset",
    overwrite=True)
```

Eine Pipeline besteht aus mindestens einem Schritt. Ein Schritt ist eine Einheit, die auf einem Computeziel ausgeführt wird. Schritte können Datenquellen verbrauchen und „Zwischendaten“ erzeugen. Ein Schritt kann Daten wie ein Modell, ein Verzeichnis mit Modell- und abhängigen Dateien oder temporäre Daten erstellen. Diese Daten stehen dann später für weitere Schritte in der Pipeline zur Verfügung.

Weitere Informationen zum Verbinden Ihrer Pipeline mit Ihren Daten finden Sie in den Artikeln [Zugreifen auf Daten](how-to-access-data.md) und [Registrieren von Datasets](how-to-create-register-datasets.md). 

### <a name="configure-data-using-dataset-and-pipelinedata-objects"></a>Konfigurieren von Daten mithilfe von `Dataset`-Objekten und `PipelineData`-Objekten

Sie haben gerade eine Datenquelle erstellt, auf die in einer Pipeline als Eingabe für einen Schritt verwiesen werden kann. Zum Bereitstellen von Daten für eine Pipeline wird vorzugsweise ein [Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.Dataset)-Objekt verwendet. Das `Dataset`-Objekt verweist auf Daten, die sich in einem Datenspeicher befinden oder über diesen oder über eine Web-URL zugänglich sind. Die `Dataset`-Klasse ist abstrakt. Daher erstellen Sie eine Instanz von `FileDataset` (die auf eine oder mehrere Dateien verweist) oder `TabularDataset`, die aus einer oder mehreren Dateien mit durch Trennzeichen getrennten Spalten von Daten erstellt wird.

`Dataset`-Objekte unterstützen Versionsverwaltung, Unterschiede und Zusammenfassungsstatistiken. `Dataset`s werden verzögert ausgewertet (wie Python-Generatoren), und es ist effizient, sie durch Aufteilen oder Filtern zu unterteilen. 

Sie erstellen ein `Dataset` mit Methoden wie [from_file](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) oder [from_delimited_files](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none--support-multi-line-false-).

```python
from azureml.core import Dataset

iris_tabular_dataset = Dataset.Tabular.from_delimited_files([(def_blob_store, 'train-dataset/iris.csv')])
```

Zwischendaten (oder die Ausgabe eines Schritts) werden durch ein [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py)-Objekt dargestellt. `output_data1` wird als Ausgabe eines Schritts erstellt und als Eingabe für einen oder mehrere zukünftige Schritte verwendet. `PipelineData` führt eine Datenabhängigkeit zwischen den Schritten ein und erzeugt eine implizite Ausführungsreihenfolge in der Pipeline. Dieses Objekt wird später beim Erstellen von Pipelineschritten verwendet.

```python
from azureml.pipeline.core import PipelineData

output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

Weitere Informationen und Beispielcode zum Arbeiten mit Datasets und Pipelinedaten finden Sie unter [Moving data into and between ML pipeline steps (Python)](how-to-move-data-in-out-of-pipelines.md) (Verschieben von Daten in und zwischen ML-Pipelineschritten (Python), in englischer Sprache).

## <a name="set-up-a-compute-target"></a>Einrichten von Computezielen

In Azure Machine Learning bezieht sich der Begriff __Compute__ (oder __Computeziel__) auf die Computer oder Cluster, die die Berechnungsschritte in Ihrer Machine Learning-Pipeline durchführen.   Eine vollständige Liste der Computeziele sowie Informationen zum Erstellen und Anfügen an Ihren Arbeitsbereich finden Sie unter [Computeziele für das Modelltraining](how-to-set-up-training-targets.md).  Das Vorgehen zum Erstellen und Anfügen eines Computeziels ist immer dasselbe, ganz gleich, ob Sie ein Modell trainieren oder einen Pipelineschritt ausführen. Verwenden Sie nach dem Erstellen und Anfügen des Computeziels das `ComputeTarget`-Objekt in Ihrem [Pipelineschritt](#steps).

> [!IMPORTANT]
> Das Anwenden von Verwaltungsvorgängen auf Computeziele wird innerhalb von Remoteaufträgen nicht unterstützt. Da Pipelines für maschinelles Lernen als Remoteauftrag übermittelt werden, sollten Sie innerhalb der Pipeline keine Verwaltungsvorgänge auf Computeziele anwenden.

Nachfolgend sind Beispiele zum Erstellen und Anfügen von Computezielen für Folgendes aufgeführt:

* Azure Machine Learning Compute
* Azure Databricks 
* Azure Data Lake Analytics

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Compute

Sie können eine Azure Machine Learning-Computeressource zum Ausführen Ihrer Schritte erstellen.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

compute_name = "aml-compute"
vm_size = "STANDARD_NC6"
if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size=vm_size,  # STANDARD_NC6 is GPU-enabled
                                                                min_nodes=0,
                                                                max_nodes=4)
    # create the compute target
    compute_target = ComputeTarget.create(
        ws, compute_name, provisioning_config)

    # Can poll for a minimum number of nodes and for a specific timeout.
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20)

    # For a more detailed view of current cluster status, use the 'status' property
    print(compute_target.status.serialize())
```

### <a name="azure-databricks"></a><a id="databricks"></a>Azure Databricks

Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie kann mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

Erstellen Sie vor der Verwendung einen Azure Databricks-Arbeitsbereich. Informationen zum Erstellen einer Arbeitsbereichsressource finden Sie im Dokument [Ausführen eines Spark-Auftrags in Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Geben Sie zum Anfügen von Azure Databricks als Computeziel die folgenden Informationen an:

* __Databricks-Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Name des Databricks-Arbeitsbereichs__: Der Name des Azure Databricks-Arbeitsbereichs.
* __Databricks-Zugriffstoken__: Das zur Authentifizierung bei Azure Databricks verwendete Zugriffstoken. Informationen zum Generieren eines Zugriffstokens finden Sie im Dokument [Authentifizierung](https://docs.azuredatabricks.net/dev-tools/api/latest/authentication.html).

Der folgende Code veranschaulicht, wie Sie Azure Databricks als Computeziel mit dem Azure Machine Learning SDK anfügen (__der Databricks-Arbeitsbereich muss im gleichen Abonnement wie der AML-Arbeitsbereich vorhanden sein__):

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get(
    "AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get(
    "AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get(
    "AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get(
    "AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(
        workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group=databricks_resource_group,
                                                           workspace_name=databricks_workspace_name,
                                                           access_token=databricks_access_token)
    databricks_compute = ComputeTarget.attach(
        ws,
        databricks_compute_name,
        attach_config
    )

    databricks_compute.wait_for_completion(True)
```

Ein ausführlicheres Beispiel finden Sie in einem [Beispiel-Notebook](https://aka.ms/pl-databricks) auf GitHub.

### <a name="azure-data-lake-analytics"></a><a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics ist eine umfangreiche Datenanalyseplattform in der Azure-Cloud. Sie kann mit einer Azure Machine Learning-Pipeline als Computeziel verwendet werden.

Erstellen Sie vor der Verwendung ein Azure Data Lake Analytics-Konto. Informationen zum Erstellen dieser Ressource finden Sie im Dokument [Erste Schritte mit Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Um Data Lake Analytics als Computeziel anzufügen, müssen Sie das Azure Machine Learning SDK verwenden und die folgenden Informationen angeben:

* __Computename__: Der Name, der dieser Computeressource zugewiesen werden soll.
* __Ressourcengruppe__: Die Ressourcengruppe, die das Data Lake Analytics-Konto enthält.
* __Kontoname__: Der Name des Data Lake Analytics-Kontos.

Der folgende Code veranschaulicht, wie Data Lake Analytics als Computeziel angefügt wird:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get(
    "AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get(
    "AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get(
    "AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group=adla_resource_group,
                                                     account_name=adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
        ws,
        adla_compute_name,
        attach_config
    )

    adla_compute.wait_for_completion(True)
```

Ein ausführlicheres Beispiel finden Sie in einem [Beispiel-Notebook](https://aka.ms/pl-adla) auf GitHub.

> [!TIP]
> Azure Machine Learning-Pipelines können nur ausgeführt werden, wenn die Daten im Standarddatenspeicher des Data Lake Analytics-Kontos gespeichert werden. Wenn die Daten, die Sie verwenden möchten, in einem nicht standardmäßigen Speicher gespeichert sind, können Sie die Daten mithilfe von [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) vor dem Trainieren kopieren.

## <a name="construct-your-pipeline-steps"></a><a id="steps"></a>Erstellen Ihrer Pipelineschritte

Nachdem Sie ein Computeziel erstellt und an Ihren Arbeitsbereich angefügt haben, können Sie einen Pipelineschritt definieren. Über das Azure Machine Learning SDK stehen viele integrierte Schritte zur Verfügung. Der grundlegendste dieser Schritte ist [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py), der ein Python-Skript an einem bestimmten Computeziel ausführt:

```python
from azureml.pipeline.steps import PythonScriptStep

ds_input = my_dataset.as_named_input('input1')

trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", ds_input.as_download(), "--output", output_data1],
    inputs=[ds_input],
    outputs=[output_data1],
    compute_target=compute_target,
    source_directory=project_folder,
    allow_reuse=True
)
```

Die Wiederverwendung von vorherigen Ergebnissen (`allow_reuse`) ist entscheidend, wenn Pipelines in einer Umgebung für Zusammenarbeit verwendet werden, da die Beseitigung von nicht benötigten erneuten Ausführungen die Flexibilität erhöht. Die Wiederverwendung ist das Standardverhalten, wenn „script_name“ und die Eingaben und Parameter eines Schritts gleich bleiben. Wenn die Ausgabe des Schritts wiederverwendet wird, wird der Auftrag nicht an den Computevorgang übermittelt, sondern die Ergebnisse aus der vorherigen Ausführung sind sofort für die Ausführung des nächsten Schritts verfügbar. Bei Festlegung von `allow_reuse` auf FALSE wird für diesen Schritt während der Pipelineausführung immer eine neue Ausführung generiert. 

Nachdem Sie die Schritte definiert haben, erstellen Sie die Pipeline mit einigen oder allen dieser Schritte.

> [!NOTE]
> Es werden keine Dateien oder Daten in Azure Machine Learning hochgeladen, wenn Sie die Schritte definieren oder die Pipeline erstellen.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

from azureml.pipeline.core import Pipeline

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

Im folgenden Beispiel wird das Azure Databricks-Computeziel verwendet, das weiter oben erstellt wurde: 

```python
from azureml.pipeline.steps import DatabricksStep

dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    compute_target=databricks_compute,
    allow_reuse=False
)
# List of steps to run
steps = [dbStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=steps)
```

### <a name="use-a-dataset"></a>Verwenden eines Datasets 

Aus Azure Blob Storage, Azure Files, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL-Datenbank und Azure Database for PostgreSQL erstellte Datasets können für jeden Pipelineschritt als Eingabe verwendet werden. Sie können die Ausgabe in [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py) oder [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) schreiben oder [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) verwenden, wenn Sie Daten in einen bestimmten Datenspeicher schreiben möchten. 

> [!IMPORTANT]
> Das Schreiben von Ausgabedaten in einen Datenspeicher mit PipelineData wird nur für Azure-Blob- und Azure-Dateifreigabe-Datenspeicher unterstützt. Diese Funktion wird derzeit für [ADLS Gen 2-Datenspeicher](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) nicht unterstützt.

```python
dataset_consuming_step = PythonScriptStep(
    script_name="iris_train.py",
    inputs=[iris_tabular_dataset.as_named_input("iris_data")],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Anschließend rufen Sie mit dem Wörterbuch [Run.input_datasets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#input-datasets) das Dataset in der Pipeline ab.

```python
# iris_train.py
from azureml.core import Run, Dataset

run_context = Run.get_context()
iris_dataset = run_context.input_datasets['iris_data']
dataframe = iris_dataset.to_pandas_dataframe()
```

Die Zeile `Run.get_context()` verdient besondere Beachtung. Diese Funktion ruft ein `Run`-Objekt ab, das die aktuelle experimentelle Ausführung darstellt. Im obigen Beispiel verwenden wir die Funktion zum Abrufen eines registrierten Datasets. Das `Run`-Objekt wird auch häufig verwendet, um das Experiment selbst und den Arbeitsbereich abzurufen, in dem sich das Experiment befindet: 

```python
# Within a PythonScriptStep

ws = Run.get_context().experiment.workspace
```

Weitere Details, einschließlich alternativer Verfahren zum Übergeben von Daten und Zugreifen auf Daten, finden Sie unter [Moving data into and between ML pipeline steps (Python)](how-to-move-data-in-out-of-pipelines.md) (Verschieben von Daten in und zwischen ML-Pipelineschritten (Python), in englischer Sprache).

## <a name="submit-the-pipeline"></a>Übermitteln der Pipeline

Wenn Sie die Pipeline übermitteln, prüft Azure Machine Learning die Abhängigkeiten für die einzelnen Schritte und lädt eine Momentaufnahme des als Quellverzeichnis angegebenen Ordners hoch. Wenn kein Quellverzeichnis angegeben ist, wird das aktuelle lokale Verzeichnis hochgeladen. Die Momentaufnahme wird auch als Teil des Experiments in Ihrem Arbeitsbereich gespeichert.

> [!IMPORTANT]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
>
> Weitere Informationen finden Sie unter [Momentaufnahmen](concept-azure-machine-learning-architecture.md#snapshots).

```python
from azureml.core import Experiment

# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

Wenn Sie eine Pipeline erstmals ausführen, geht Azure Machine Learning folgendermaßen vor:

* Die Momentaufnahme des Projekts wird aus dem Blobspeicher, der dem Arbeitsbereich zugeordnet ist, auf das Computeziel heruntergeladen.
* Für jeden Schritt in der Pipeline wird ein Docker-Image erstellt.
* Das Docker-Image für die einzelnen Schritte wird aus der Containerregistrierung auf das Computeziel heruntergeladen.
* Konfiguriert den Zugriff auf das `Dataset`-Objekt und das `PipelineData`-Objekt. Im Zugriffsmodus `as_mount()` wird FUSE zum Bereitstellen des virtuellen Zugriffs verwendet. Wenn das Einbinden nicht unterstützt wird oder wenn der Benutzer den Zugriffsmodus `as_download()` angegeben hat, werden die Daten stattdessen in das Computeziel kopiert.
* Der Schritt wird in dem in der Schrittdefinition angegebenen Computeziel ausgeführt. 
* Artefakte wie Protokolle, stdout und stderr, Metriken und Ausgaben, die durch den Schritt angegeben werden, werden erstellt. Diese Artefakte werden dann hochgeladen und im Standarddatenspeicher des Benutzers gespeichert.

![Diagramm zum Ausführen eines Experiments als Pipeline](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

Weitere Informationen finden Sie in der Referenz zur [Experiment-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py).

### <a name="view-results-of-a-pipeline"></a>Anzeigen der Ergebnisse einer Pipeline

Hier finden Sie die Liste aller Ihrer Pipelines und deren Ausführungsdetails in Studio:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. [Zeigen Sie den Arbeitsbereich an](how-to-manage-workspace.md#view).

1. Wählen Sie auf der linken Seite **Pipelines** aus, um alle Pipelineausführungen anzuzeigen.
 ![Liste der Machine Learning-Pipelines](./media/how-to-create-your-first-pipeline/pipelines.png)
 
1. Wählen Sie eine bestimmte Pipeline aus, um die Ausführungsergebnisse anzuzeigen.

## <a name="git-tracking-and-integration"></a>Git-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="publish-a-pipeline"></a>Veröffentlichen einer Pipeline

Sie können eine Pipeline veröffentlichen, um sie später mit verschiedenen Eingaben auszuführen. Damit der REST-Endpunkt einer bereits veröffentlichten Pipeline Parameter akzeptiert, muss die Pipeline vor der Veröffentlichung parametrisiert werden.

1. Um einen Pipelineparameter zu erstellen, verwenden Sie ein [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py)-Objekt mit einem Standardwert.

   ```python
   from azureml.pipeline.core.graph import PipelineParameter
   
   pipeline_param = PipelineParameter(
     name="pipeline_arg",
     default_value=10)
   ```

2. Fügen Sie dieses `PipelineParameter`-Objekt als Parameter zu einem der Schritte in der Pipeline wie folgt hinzu:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],
     compute_target=compute_target,
     source_directory=project_folder)
   ```

3. Veröffentlichen Sie diese Pipeline, die beim Aufruf einen Parameter akzeptiert.

   ```python
   published_pipeline1 = pipeline_run1.publish_pipeline(
        name="My_Published_Pipeline",
        description="My Published Pipeline Description",
        version="1.0")
   ```

### <a name="run-a-published-pipeline"></a>Ausführen einer veröffentlichten Pipeline

Alle veröffentlichten Pipelines weisen einen REST-Endpunkt auf. Mit dem Pipelineendpunkt können Sie eine Ausführung der Pipeline von beliebigen externen Systemen aus auslösen, einschließlich Clients, die nicht Python verwenden. Dieser Endpunkt ermöglicht „verwaltete Wiederholbarkeit“ in Szenarien für die Batchbewertung und für erneutes Training.

Zum Aufrufen der Ausführung der vorhergehenden Pipeline benötigen Sie ein Headertoken für die Azure Active Directory-Authentifizierung, wie in der Referenz zur [AzureCliAuthentication-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) beschrieben. Weitere Details finden Sie außerdem im Notebook zur [Authentifizierung in Azure Machine Learning](https://aka.ms/pl-restep-auth).

```python
from azureml.pipeline.core import PublishedPipeline
import requests

response = requests.post(published_pipeline1.endpoint,
                         headers=aad_token,
                         json={"ExperimentName": "My_Pipeline",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

## <a name="create-a-versioned-pipeline-endpoint"></a>Erstellen eines Pipelineendpunkts mit Versionsangabe

Sie können einen Pipelineendpunkt erstellen, hinter dem sich mehrere veröffentlichte Pipelines befinden. Dieser kann wie eine veröffentlichte Pipeline verwendet werden, stellt aber einen festen REST-Endpunkt bereit, wenn Sie Ihre ML-Pipelines durchlaufen und aktualisieren.

```python
from azureml.pipeline.core import PipelineEndpoint

published_pipeline = PublishedPipeline.get(workspace="ws", name="My_Published_Pipeline")
pipeline_endpoint = PipelineEndpoint.publish(workspace=ws, name="PipelineEndpointTest",
                                            pipeline=published_pipeline, description="Test description Notebook")
```

### <a name="submit-a-job-to-a-pipeline-endpoint"></a>Übermitteln eines Auftrags an einen Pipelineendpunkt

Sie können einen Auftrag an die Standardversion eines Pipelineendpunkts übermitteln:

```python
pipeline_endpoint_by_name = PipelineEndpoint.get(workspace=ws, name="PipelineEndpointTest")
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment")
print(run_id)
```

Sie können einen Auftrag auch an eine bestimmte Version übermitteln:

```python
run_id = pipeline_endpoint_by_name.submit("PipelineEndpointExperiment", pipeline_version="0")
print(run_id)
```

Das gleiche Ergebnis kann mit der REST-API erreicht werden:

```python
rest_endpoint = pipeline_endpoint_by_name.endpoint
response = requests.post(rest_endpoint, 
                         headers=aad_token, 
                         json={"ExperimentName": "PipelineEndpointExperiment",
                               "RunSource": "API",
                               "ParameterAssignments": {"1": "united", "2":"city"}})
```

### <a name="use-published-pipelines-in-the-studio"></a>Verwenden von veröffentlichten Pipelines in Studio

Sie können über Studio auch eine veröffentlichte Pipeline ausführen:

1. Melden Sie sich bei [Azure Machine Learning Studio](https://ml.azure.com) an.

1. [Zeigen Sie den Arbeitsbereich an](how-to-manage-workspace.md#view).

1. Klicken Sie links auf **Endpunkte**.

1. Wählen Sie oben **Pipelineendpunkte** aus.
 ![Liste der veröffentlichen Machine Learning-Pipelines](./media/how-to-create-your-first-pipeline/pipeline-endpoints.png)

1. Wählen Sie eine bestimmte Pipeline aus, um einen Pipelineendpunkt auszuführen, zu nutzen oder die Ergebnisse früherer Ausführungen des Pipelineendpunkts zu überprüfen.

### <a name="disable-a-published-pipeline"></a>Deaktivieren einer veröffentlichten Pipeline

Um eine Pipeline aus der Liste der veröffentlichten Pipelines auszublenden, deaktivieren Sie sie in Studio oder über das SDK:

```python
# Get the pipeline by using its ID from Azure Machine Learning studio
p = PublishedPipeline.get(ws, id="068f4885-7088-424b-8ce2-eeb9ba5381a6")
p.disable()
```

Sie können sie mit `p.enable()` wieder aktivieren. Weitere Informationen finden Sie in der Referenz zur [PublishedPipeline-Klasse](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.publishedpipeline?view=azure-ml-py).

## <a name="caching--reuse"></a>Zwischenspeichern und Wiederverwenden  

Zur Optimierung und Anpassung des Verhaltens Ihrer Pipelines können Sie einige Optionen für die Zwischenspeicherung und Wiederverwendung nutzen. Sie haben beispielsweise folgende Optionen:
+ **Deaktivieren Sie die standardmäßige Wiederverwendung der Ausgabe nach Ausführung des Schritts**, indem Sie bei der [Schrittdefinition](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)`allow_reuse=False` festlegen. Die Wiederverwendung ist entscheidend, wenn Pipelines in einer Umgebung für die Zusammenarbeit verwendet werden, weil die Beseitigung von nicht benötigten Ausführungen zu einer höheren Flexibilität führt. Sie können die Wiederverwendung aber auch deaktivieren.
+ **Erzwingen Sie die erneute Generierung der Ausgabe für alle Schritte einer Ausführung** mit `pipeline_run = exp.submit(pipeline, regenerate_outputs=False)`.

Standardmäßig ist `allow_reuse` für Schritte aktiviert, und das in der Schrittdefinition angegebene `source_directory` verfügt über einen Hashwert. Wenn das Skript für einen Schritt unverändert bleibt (`script_name`, Eingaben und Parameter), und sich nichts sonst im ` source_directory` geändert hat, wird also die Ausgabe eines vorherigen Schritts wiederverwendet, der Auftrag wird nicht an den Computevorgang übermittelt, und die Ergebnisse der vorherigen Ausführung sind stattdessen sofort für den nächsten Schritt verfügbar.

```python
step = PythonScriptStep(name="Hello World",
                        script_name="hello_world.py",
                        compute_target=aml_compute,
                        source_directory=source_directory,
                        allow_reuse=False,
                        hash_paths=['hello_world.ipynb'])
```

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [diese Jupyter-Notebooks auf GitHub](https://aka.ms/aml-pipeline-readme), um die Pipelines für maschinelles Lernen eingehender zu erkunden.
- Hinweise zu den Paketen [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) finden Sie in der SDK-Referenzhilfe.
- Tipps zum Debuggen und zur Problembehandlung von Pipelines finden Sie in der [Schrittanleitung](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]
