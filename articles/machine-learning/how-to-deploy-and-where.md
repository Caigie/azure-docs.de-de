---
title: Wie und wo Modelle bereitgestellt werden
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie und wo Sie Ihre Azure Machine Learning-Modelle bereitstellen können, einschließlich Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge und Field Programmable Gate Arrays.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2020
ms.custom: seoapril2019, tracking-python
ms.openlocfilehash: ee116d668b9c351ecf5b130a39e418a3da8fc053
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536384"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Bereitstellen von Modellen mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie, wie Sie Ihr Machine Learning-Modell als Webdienst in der Azure-Cloud oder auf Azure IoT Edge-Geräten bereitstellen.

Der Workflow ist sehr ähnlich, unabhängig vom [Bereitstellungsort](#target) Ihres Modells:

1. Registrieren des Modells.
1. Vorbereiten der Bereitstellung. (Geben Sie Ressourcen, Nutzung, Computeziel an.)
1. Bereitstellen des Modells auf dem Computeziel.
1. Testen des bereitgestellten Modells (das auch als „Webdienst“ bezeichnet wird).

Weitere Informationen zu den am Bereitstellungsworkflow beteiligten Konzepten finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

- Ein Modell. Wenn Sie über kein trainiertes Modell verfügen, können Sie die Modell- und Abhängigkeitsdateien verwenden, die in [diesem Tutorial](https://aka.ms/azml-deploy-cloud) bereitgestellt werden.

- Die [Azure CLI-Erweiterung für den Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](tutorial-setup-vscode-extension.md).

## <a name="connect-to-your-workspace"></a>Herstellen einer Verbindung mit Ihrem Arbeitsbereich

Der folgende Code veranschaulicht, wie Sie mithilfe von Informationen, die in der lokalen Entwicklungsumgebung zwischengespeichert sind, eine Verbindung mit einem Azure Machine Learning-Arbeitsbereich herstellen:

+ **Verwenden des SDK**

   ```python
   from azureml.core import Workspace
   ws = Workspace.from_config(path=".file-path/ws_config.json")
   ```

  Weitere Informationen zur Verwendung des SDKs, um eine Verbindung mit einem Arbeitsbereich herzustellen, finden Sie in der Dokumentation zum [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace).

+ **Verwenden der CLI**

   Verwenden Sie bei Verwendung der CLI den Parameter `-w` oder `--workspace-name`, um den Arbeitsbereich für den Befehl anzugeben.

+ **Verwendung von Visual Studio Code**

   Wenn Sie Visual Studio Code verwenden, wählen Sie den Arbeitsbereich über eine grafische Benutzeroberfläche aus. Weitere Informationen finden Sie unter [Bereitstellen und Verwalten von Modellen](how-to-manage-resources-vscode.md#endpoints) in der Dokumentation zur Visual Studio Code-Erweiterung.

## <a name="register-your-model"></a><a id="registermodel"></a> Registrieren Ihres Modells

Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell im Arbeitsbereich registrieren. Nachdem Sie die Dateien registriert haben, können Sie das registrierte Modell herunterladen oder bereitstellen und alle Dateien empfangen, die Sie registriert haben.

> [!TIP]
> Wenn Sie ein Modell registrieren, geben Sie den Pfad eines Cloudspeicherorts (aus einem Trainingslauf) oder eines lokalen Verzeichnisses an. Dieser Pfad wird lediglich dazu verwendet, die Dateien während des Registrierungsprozesses für das Hochladen zu finden. Er muss nicht mit dem Pfad identisch sein, der im Eingabeskript verwendet wird. Weitere Informationen finden Sie unter [Suchen nach Modelldateien im Eingabeskript](#load-model-files-in-your-entry-script).

Machine Learning-Modelle werden in Ihrem Azure Machine Learning-Arbeitsbereich registriert. Das Modell kann aus Azure Machine Learning oder aus einer anderen Quelle stammen. Wenn Sie ein Modell registrieren, können Sie optional Metadaten zum Modell bereitstellen. Die Wörterbücher `tags` und `properties`, die Sie auf eine Modellregistrierung anwenden, können dann zum Filtern von Modellen verwendet werden.

Die folgenden Beispiele veranschaulichen das Registrieren eines Modells.

### <a name="register-a-model-from-an-experiment-run"></a>Registrieren eines Modells aus einer Experimentausführung

Die Codeausschnitte in diesem Abschnitt veranschaulichen, wie ein Modell aus einem Trainingslauf (Trainingsausführung) registriert wird:

> [!IMPORTANT]
> Damit Sie diese Codeausschnitte verwenden können, müssen Sie zuvor einen Trainingslauf ausgeführt haben und Zugriff auf das `Run`-Objekt (SDK-Beispiel) oder den Ausführungs-ID-Wert haben (CLI-Beispiel). Weitere Informationen zum Trainieren von Modellen finden Sie unter [Einrichten und Verwenden von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

+ **Verwenden des SDK**

  Wenn Sie das SDK verwenden, um ein Modell zu trainieren, können Sie entweder ein [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py)-Objekt oder ein [AutoMLRun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun)-Objekt empfangen. Dies hängt davon ab, wie Sie das Modell trainiert haben. Jedes Objekt kann verwendet werden, um ein Modell zu registrieren, das von einer Experimentausführung erstellt wurde.

  + Registrieren Sie ein Modell über ein `azureml.core.Run`-Objekt:
 
    ```python
    model = run.register_model(model_name='sklearn_mnist',
                               tags={'area': 'mnist'},
                               model_path='outputs/sklearn_mnist_model.pkl')
    print(model.name, model.id, model.version, sep='\t')
    ```

    Der `model_path`-Parameter verweist auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `model_path` auf den Pfad eines Ordners fest, der die Dateien enthält. Weitere Informationen finden Sie in der Dokumentation zum [Run.register-Modell](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-).

  + Registrieren Sie ein Modell über ein `azureml.train.automl.run.AutoMLRun`-Objekt:

    ```python
        description = 'My AutoML Model'
        model = run.register_model(description = description,
                                   tags={'area': 'mnist'})

        print(run.model_id)
    ```

    In diesem Beispiel sind die Parameter `metric` und `iteration` nicht angegeben. Daher wird die Iteration mit der besten primären Metrik registriert. Der von der Ausführung zurückgegebene `model_id`-Wert wird anstelle eines Modellnamens verwendet.

    Weitere Informationen finden Sie in der Dokumentation zu [AutoMLRun.register-Modell](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun#register-model-model-name-none--description-none--tags-none--iteration-none--metric-none-).

+ **Verwenden der CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid --tag area=mnist
  ```

  [!INCLUDE [install extension](../../includes/machine-learning-service-install-extension.md)]

  Der `--asset-path`-Parameter verweist auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `--asset-path` auf den Pfad eines Ordners fest, der die Dateien enthält.

+ **Verwendung von Visual Studio Code**

  Registrieren Sie Modelle mit beliebigen Modelldateien oder -ordnern über die [Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md#deploy-the-model)-Erweiterung.

### <a name="register-a-model-from-a-local-file"></a>Registrieren eines Modells aus einer lokalen Datei

Sie können ein Modell registrieren, indem Sie den lokalen Pfad des Modells bereitstellen. Sie können den Pfad eines Ordners oder einer einzelnen Datei angeben. Mit dieser Methode können Sie Modelle registrieren, die mit dem Azure Machine Learning trainiert und dann heruntergeladen wurden. Mit dieser Methode können Sie auch Modelle registrieren, die außerhalb von Azure Machine Learning trainiert wurden.

[!INCLUDE [trusted models](../../includes/machine-learning-service-trusted-model.md)]

+ **Verwenden des SDK und von ONNX**

    ```python
    import os
    import urllib.request
    from azureml.core.model import Model
    # Download model
    onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
    urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
    os.system('tar xvzf mnist.tar.gz')
    # Register model
    model = Model.register(workspace = ws,
                            model_path ="mnist/model.onnx",
                            model_name = "onnx_mnist",
                            tags = {"onnx": "demo"},
                            description = "MNIST image classification CNN from ONNX Model Zoo",)
    ```

  Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `model_path` auf den Pfad eines Ordners fest, der die Dateien enthält.

+ **Verwenden der CLI**

  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  Um mehrere Dateien in die Modellregistrierung aufzunehmen, legen Sie `-p` auf den Pfad eines Ordners fest, der die Dateien enthält.

**Geschätzter Zeitaufwand**: Ungefähr 10 Sekunden.

Weitere Informationen finden Sie in der Dokumentation zur [Model-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Weitere Informationen zur Arbeit mit Modellen, die außerhalb des Azure Machine Learning trainiert wurden, finden Sie unter [Verwenden eines vorhandenen Modells mit Azure Machine Learning](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="single-versus-multi-model-endpoints"></a>Endpunkte mit einem oder mehreren Modellen
Azure ML unterstützt die Bereitstellung von einzelnen oder mehreren Modellen hinter einem einzelnen Endpunkt.

Endpunkte mit mehreren Modellen verwenden einen freigegebenen Container, um mehrere Modelle zu hosten. Dies trägt zur Senkung der Gesamtkosten bei, verbessert die Auslastung und ermöglicht es Ihnen, Module zu Ensembles zusammenzufügen. Modelle, die Sie in Ihrem Bereitstellungsskript angeben, werden auf dem Datenträger des bereitstellenden Containers eingebunden und zur Verfügung gestellt – Sie können sie bei Bedarf in den Arbeitsspeicher laden und auf der Grundlage des spezifischen Modells, das zum Zeitpunkt der Bewertung angefordert wird, bewerten.

Ein E2E-Beispiel, das zeigt, wie mehrere Modelle hinter einem einzelnen containerisierten Endpunkt verwendet werden können, finden Sie in [diesem Beispiel](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/deploy-multi-model).

## <a name="prepare-to-deploy"></a>Vorbereiten der Bereitstellung

Um das Modell als Dienst bereitzustellen, benötigen Sie die folgenden Komponenten:

* **Definieren der Rückschlussumgebung**. Diese Umgebung kapselt die Abhängigkeiten, die zur Ausführung Ihres Modells für Rückschlüsse erforderlich sind.
* **Definieren des Bewertungscodes**. Dieses Skript akzeptiert Anforderungen, bewertet die Anforderungen über Verwenden des Modells und gibt die Ergebnisse zurück.
* **Definieren der Rückschlusskonfiguration**. Die Rückschlusskonfiguration gibt die Umgebungskonfiguration, das Eingabeskript und andere Komponenten an, die zur Ausführung des Modells als Dienst benötigt werden.

Nachdem Sie über die erforderlichen Komponenten verfügen, können Sie ein Profil des Diensts erstellen, der als Ergebnis der Bereitstellung Ihres Modells erstellt wird, um seine CPU- und Speicheranforderungen zu verstehen.

### <a name="1-define-inference-environment"></a>1. Definieren der Rückschlussumgebung

Eine Rückschlusskonfiguration beschreibt, wie der Webdienst, der Ihr Modell enthält, eingerichtet werden kann. Sie wird später verwendet, wenn Sie das Modell bereitstellen.

Bei der Rückschlusskonfiguration werden Azure Machine Learning-Umgebungen verwendet, um die Softwareabhängigkeiten zu definieren, die für Ihre Bereitstellung benötigt werden. Mit Umgebungen können Sie die Softwareabhängigkeiten, die für das Training und die Bereitstellung erforderlich sind, erstellen, verwalten und wiederverwenden. Sie können eine Umgebung aus benutzerdefinierten Abhängigkeitsdateien erstellen oder eine der zusammengestellten Azure Machine Learning-Umgebungen verwenden. Das folgende YAML-Beispiel zeigt eine Conda-Abhängigkeitsdatei für den Rückschluss. Beachten Sie, dass Sie „azureml-defaults“ mit einer Version größer oder gleich 1.0.45 als pip-Abhängigkeit angeben müssen, da sie die Funktionalität enthält, die zum Hosten des Modells als Webdienst erforderlich ist. Wenn Sie automatische Schemagenerierung verwenden möchten, muss Ihr Eingangsskript auch die `inference-schema`-Pakete importieren.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - scikit-learn=0.20.0
  - pip:
      # You must list azureml-defaults as a pip dependency
    - azureml-defaults>=1.0.45
    - inference-schema[numpy-support]
```

> [!IMPORTANT]
> Wenn die Abhängigkeit sowohl über Conda als auch über pip (von PyPi) verfügbar ist, empfiehlt Microsoft die Verwendung der Conda-Version, da Conda-Pakete in der Regel mit vorgefertigten Binärdateien geliefert werden, welche die Zuverlässigkeit der Installation erhöhen.
>
> Weitere Informationen finden Sie unter [Grundlegendes zu Conda und pip](https://www.anaconda.com/understanding-conda-and-pip/).
>
> Überprüfen Sie mithilfe des `conda search <package-name>`-Befehls, ob ihre Abhängigkeit über Conda verfügbar ist, oder verwenden Sie die Paketindizes unter [https://anaconda.org/anaconda/repo](https://anaconda.org/anaconda/repo) und [https://anaconda.org/conda-forge/repo](https://anaconda.org/conda-forge/repo).

Sie können die Abhängigkeitsdatei verwenden, um ein Umgebungsobjekt zu erstellen und es zur späteren Verwendung in Ihrem Arbeitsbereich zu speichern:

```python
from azureml.core.environment import Environment
myenv = Environment.from_conda_specification(name = 'myenv',
                                             file_path = 'path-to-conda-specification-file'
myenv.register(workspace=ws)
```

### <a name="2-define-scoring-code"></a><a id="script"></a> 2. Definieren des Bewertungscodes

Das Eingangsskript empfängt an einen bereitgestellten Webdienst übermittelte Daten und übergibt sie an das Modell. Anschließend nimmt es die vom Modell zurückgegebene Antwort entgegen und gibt diese an den Client zurück. *Das Skript ist auf Ihr Modell zugeschnitten*. In ihm muss die Struktur der vom Modell erwarteten und zurückgegebenen Daten bekannt sein.

Das Skript enthält zwei Funktionen zum Laden und Ausführen des Modells:

* `init()`: Diese Funktion lädt üblicherweise das Modell in ein globales Objekt. Diese Funktion wird nur ein Mal ausgeführt, wenn der Docker-Container für Ihren Webdienst gestartet wird.

* `run(input_data)`: Diese Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Für Ein- und Ausgaben der Ausführung wird in der Regel JSON für die Serialisierung und Deserialisierung verwendet. Sie können auch mit binären Rohdaten arbeiten. Sie können die Daten transformieren, bevor Sie sie an das Modell senden oder an den Client zurückgeben.

#### <a name="load-model-files-in-your-entry-script"></a>Laden von Modelldateien in ein Eingabeskript

Es gibt zwei Möglichkeiten, in einem Eingabeskript nach Modellen zu suchen:
* `AZUREML_MODEL_DIR`: Eine Umgebungsvariable, die den Pfad zu dem Speicherort des Modells enthält
* `Model.get_model_path`: Eine API, die den registrierten Modellnamen verwendet, um den Pfad zur Modelldatei zurückzugeben

##### <a name="azureml_model_dir"></a>AZUREML_MODEL_DIR

AZUREML_MODEL_DIR ist eine Umgebungsvariable, die während der Dienstbereitstellung erstellt wird. Sie können diese Umgebungsvariable verwenden, um den Speicherort der bereitgestellten Modelle zu ermitteln.

In der folgenden Tabelle ist der Wert von AZUREML_MODEL_DIR in Abhängigkeit von der Anzahl der bereitgestellten Modelle beschrieben:

| Bereitstellung | Wert der Umgebungsvariablen |
| ----- | ----- |
| Einzelnes Modell | Der Pfad zu dem Ordner, der das Modell enthält |
| Mehrere Modelle | Der Pfad zu dem Ordner, der alle Modelle enthält. Die Modelle befinden sich nach Name und Version in diesem Ordner (`$MODEL_NAME/$VERSION`). |

Während der Modellregistrierung und -bereitstellung werden Modelle in den Pfad „AZUREML_MODEL_DIR“ eingefügt, und die ursprünglichen Dateinamen werden beibehalten.

Um den Pfad zu einer Modelldatei in Ihrem Eingabeskript abzurufen, kombinieren Sie die Umgebungsvariable mit dem Dateipfad, nach dem Sie suchen.

**Beispiel für ein einzelnes Modell**
```python
# Example when the model is a file
model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_regression_model.pkl')

# Example when the model is a folder containing a file
file_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'my_model_folder', 'sklearn_regression_model.pkl')
```

**Beispiel für mehrere Modelle**

In diesem Szenario werden zwei Modelle unter dem Arbeitsbereich registriert:

* `my_first_model`: Enthält eine Datei (`my_first_model.pkl`), und es gibt nur eine Version (`1`).
* `my_second_model`: Enthält eine Datei (`my_second_model.pkl`), und es gibt die zwei Versionen `1` und `2`.

Wenn der Dienst bereitgestellt wurde, werden beide Modelle im Bereitstellungsvorgang verfügbar gemacht:

```python
first_model = Model(ws, name="my_first_model", version=1)
second_model = Model(ws, name="my_second_model", version=2)
service = Model.deploy(ws, "myservice", [first_model, second_model], inference_config, deployment_config)
```

In dem Docker-Image, das den Dienst hostet, enthält die `AZUREML_MODEL_DIR`-Umgebungsvariable das Verzeichnis, in dem sich die Modelle befinden.
In diesem Verzeichnis befindet sich jedes Modell im Verzeichnispfad von `MODEL_NAME/VERSION`. Dabei ist `MODEL_NAME` der Name des registrierten Modells, und `VERSION` ist die Version des Modells. Die Dateien, aus denen das registrierte Modell besteht, werden in diesen Verzeichnissen gespeichert.

In diesem Beispiel lauten die Pfade `$AZUREML_MODEL_DIR/my_first_model/1/my_first_model.pkl` und `$AZUREML_MODEL_DIR/my_second_model/2/my_second_model.pkl`.


```python
# Example when the model is a file, and the deployment contains multiple models
first_model_name = 'my_first_model'
first_model_version = '1'
first_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), first_model_name, first_model_version, 'my_first_model.pkl')
second_model_name = 'my_second_model'
second_model_version = '2'
second_model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), second_model_name, second_model_version, 'my_second_model.pkl')
```

##### <a name="get_model_path"></a>get_model_path

Wenn Sie ein Modell registrieren, geben Sie einen Modellnamen ein. Dieser wird dazu verwendet, das Modell in der Registrierung zu verwalten. Sie verwenden diesen Namen mit der [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-)-Methode, um den Pfad der Modelldatei(en) im lokalen Dateisystem abzurufen. Wenn Sie einen Ordner oder eine Sammlung von Dateien registrieren, gibt diese API den Pfad des Verzeichnisses zurück, das diese Dateien enthält.

Wenn Sie ein Modell registrieren, geben Sie ihm einen Namen. Der Name entspricht dem Ort, an dem sich das Modell befindet (entweder lokal oder während der Dienstbereitstellung).

#### <a name="optional-define-model-web-service-schema"></a>(Optional) Definieren eines Schemas für einen Modellwebdienst

Um automatisch ein Schema für Ihren Webdienst zu generieren, stellen Sie ein Beispiel der Eingabe und/oder Ausgabe im Konstruktor für eines der definierten Typobjekte bereit. Der Typ und das Beispiel werden verwendet, um das Schema automatisch zu erstellen. Azure Machine Learning erstellt dann während der Bereitstellung eine [OpenAPI](https://swagger.io/docs/specification/about/)-Spezifikation (Swagger) für den Webdienst.

Diese Typen werden derzeit unterstützt:

* `pandas`
* `numpy`
* `pyspark`
* Python-Standardobjekt

Um Schemagenerierung zu verwenden, schließen Sie das Open-Source-`inference-schema`-Paket in Ihre Abhängigkeitsdatei ein. Weitere Informationen zu diesem Paket finden Sie unter [https://github.com/Azure/InferenceSchema](https://github.com/Azure/InferenceSchema). Definieren Sie in den Variablen `input_sample` und `output_sample` das Format für das Ein- und Ausgabebeispiel. (Die Variablen stellen das Anforderungs- und das Antwortformat für den Webdienst dar.) Verwenden Sie diese Beispiele in den Decorator-Elementen der Ein- und Ausgabefunktion für die Funktion `run()`. Im folgenden scikit-learn-Beispiel wird Schemagenerierung verwendet.

##### <a name="example-entry-script"></a>Beispiel für ein Eingangsskript

Das folgende Beispiel zeigt, wie JSON-Daten akzeptiert und zurückgegeben werden:

```python
#Example: scikit-learn and Swagger
import json
import numpy as np
import os
from sklearn.externals import joblib
from sklearn.linear_model import Ridge

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # AZUREML_MODEL_DIR is an environment variable created during deployment. Join this path with the filename of the model file.
    # It holds the path to the directory that contains the deployed model (./azureml-models/$MODEL_NAME/$VERSION).
    # If there are multiple models, this value is the path to the directory containing all deployed models (./azureml-models).
    # Alternatively: model_path = Model.get_model_path('sklearn_mnist')
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')
    # Deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

##### <a name="power-bi-compatible-endpoint"></a>Power BI-kompatibler Endpunkt 

Im folgenden Beispiel wird veranschaulicht, wie Eingabedaten als `<key: value>`-Wörterbuch definiert werden, indem ein Datenrahmen (DataFrame) verwendet wird. Diese Methode wird dazu unterstützt, den bereitgestellten Webdienst aus Power BI zu nutzen. ([Erfahren Sie mehr darüber, wie der Webdienst aus Power BI genutzt werden kann](https://docs.microsoft.com/power-bi/service-machine-learning-integration).)

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # Replace filename if needed.
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'model_file.pkl')
    # Deserialize the model file back into a sklearn model.
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    # This is a decimal type sample. Use the data type that reflects this column in your data.
    "input_name_1": 5.1,
    # This is a string type sample. Use the data type that reflects this column in your data.
    "input_name_2": "value2",
    # This is an integer type sample. Use the data type that reflects this column in your data.
    "input_name_3": 3
}])

# This is an integer type sample. Use the data type that reflects the expected result.
output_sample = np.array([0])

# To indicate that we support a variable length of data input,
# set enforce_shape=False
@input_schema('data', PandasParameterType(input_sample, enforce_shape=False))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # You can return any data type, as long as it is JSON serializable.
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Weitere Beispiele finden Sie in den folgenden Skripts:

* [PyTorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/pytorch)
* [TensorFlow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/ml-frameworks/tensorflow)
* [Keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* [AutoML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features)
* [ONNX](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* [Binärdaten](#binary)
* [CORS](#cors)

### <a name="3-define-inference-configuration"></a><a id="script"></a> 3. Definieren der Rückschlusskonfiguration
    
Im folgenden Beispiel wird das Laden einer Umgebung aus Ihrem Arbeitsbereich und die anschließende Verwendung mit der Rückschlusskonfiguration veranschaulicht:

```python
from azureml.core.environment import Environment
from azureml.core.model import InferenceConfig


myenv = Environment.get(workspace=ws, name='myenv', version='1')
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                   environment=myenv)
```

Weitere Informationen zu Umgebungen finden Sie unter [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md).

Weitere Informationen zur Rückschlusskonfiguration finden Sie in der Dokumentation der [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse.

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit einer Rückschlusskonfiguration finden Sie unter [Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages](how-to-deploy-custom-docker-image.md).

#### <a name="cli-example-of-inferenceconfig"></a>CLI-Beispiel für InferenceConfig

[!INCLUDE [inference config](../../includes/machine-learning-service-inference-config.md)]

Der folgende Befehl veranschaulicht, wie Sie ein Modell mithilfe über die Befehlszeilenschnittstelle (Call-Level-Interface, CLI) bereitstellen:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In diesem Beispiel gibt die Konfiguration die folgenden Einstellungen an:

* Die Angabe, dass für das Modell Python erforderlich ist
* Das [Eingangsskript](#script), das zum Verarbeiten von an den bereitgestellten Dienst gesendeten Webanforderungen verwendet wird.
* Die Conda-Datei, in der die Python-Pakete beschrieben sind, die für Rückschlüsse erforderlich sind

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit einer Rückschlusskonfiguration finden Sie unter [Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages](how-to-deploy-custom-docker-image.md).

### <a name="4-optional-profile-your-model-to-determine-resource-utilization"></a><a id="profilemodel"></a> 4. (Optional) Erstellen eines Profils Ihres Modells zur Bestimmung der Ressourcenverwendung

Nachdem Sie Ihr Modell registriert und die anderen für die Bereitstellung erforderlichen Komponenten vorbereitet haben, können Sie die CPU und den Speicher bestimmen, die der bereitgestellte Dienst benötigt. Die Profilerstellung testet den Dienst, der Ihr Modell ausführt, und gibt Informationen wie CPU-Auslastung, Speicherauslastung und Antwortlatenz zurück. Sie bietet auch eine Empfehlung für die CPU und den Speicher auf der Grundlage der Ressourcenverwendung.

Um ein Profil Ihres Modells erstellen zu können, benötigen Sie Folgendes:
* Ein registriertes Modell.
* Eine Rückschlusskonfiguration basierend auf Ihrem Eingabeskript und der Definition der Rückschlussumgebung.
* Ein einspaltiges Tabellendataset, bei dem jede Zeile eine Zeichenfolge enthält, die Beispielanforderungsdaten darstellt.

> [!IMPORTANT]
> An dieser Stelle unterstützen wir nur die Profilerstellung von Diensten, die erwarten, dass ihre Anforderungsdaten eine Zeichenfolge sind. Beispiel: string serialized json, text, string serialized image, usw. Der Inhalt der einzelnen Zeilen des Datasets (Zeichenfolge) wird in den Hauptteil der HTTP-Anforderung eingefügt und an den Dienst gesendet, der das Modell zur Bewertung kapselt.

Nachfolgend finden Sie ein Beispiel dafür, wie Sie ein Eingabedataset konstruieren können, um ein Profil eines Diensts zu erstellen, der erwartet, dass seine eingehenden Anforderungsdaten serialisierten JSON-Code enthalten. In diesem Fall haben Sie ein Dataset erstellt, das auf 100 Instanzen desselben Anforderungsdateninhalts basiert. In realen Szenarien wird empfohlen, dass Sie größere Datasets mit verschiedenen Eingaben verwenden, insbesondere wenn die Nutzung bzw. das Verhalten Ihrer Modellressourcen von Eingaben abhängig ist.

```python
import json
from azureml.core import Datastore
from azureml.core.dataset import Dataset
from azureml.data import dataset_type_definitions

input_json = {'data': [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                       [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]}
# create a string that can be utf-8 encoded and
# put in the body of the request
serialized_input_json = json.dumps(input_json)
dataset_content = []
for i in range(100):
    dataset_content.append(serialized_input_json)
dataset_content = '\n'.join(dataset_content)
file_name = 'sample_request_data.txt'
f = open(file_name, 'w')
f.write(dataset_content)
f.close()

# upload the txt file created above to the Datastore and create a dataset from it
data_store = Datastore.get_default(ws)
data_store.upload_files(['./' + file_name], target_path='sample_request_data')
datastore_path = [(data_store, 'sample_request_data' +'/' + file_name)]
sample_request_data = Dataset.Tabular.from_delimited_files(
    datastore_path, separator='\n',
    infer_column_types=True,
    header=dataset_type_definitions.PromoteHeadersBehavior.NO_HEADERS)
sample_request_data = sample_request_data.register(workspace=ws,
                                                   name='sample_request_data',
                                                   create_new_version=True)
```

Sobald Sie das Dataset mit den Beispielanforderungsdaten bereit haben, erstellen Sie eine Rückschlusskonfiguration. Die Rückschlusskonfiguration basiert auf der „score.py“ und der Umgebungsdefinition. Im folgenden Beispiel wird veranschaulicht, wie die Rückschlusskonfiguration erstellt und die Profilerstellung ausgeführt wird:

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.dataset import Dataset


model = Model(ws, id=model_id)
inference_config = InferenceConfig(entry_script='path-to-score.py',
                                   environment=myenv)
input_dataset = Dataset.get_by_name(workspace=ws, name='sample_request_data')
profile = Model.profile(ws,
            'unique_name',
            [model],
            inference_config,
            input_dataset=input_dataset)

profile.wait_for_completion(True)

# see the result
details = profile.get_details()
```

Der folgende Befehl veranschaulicht, wie ein Modell mithilfe der CLI profiliert wird:

```azurecli-interactive
az ml model profile -g <resource-group-name> -w <workspace-name> --inference-config-file <path-to-inf-config.json> -m <model-id> --idi <input-dataset-id> -n <unique-name>
```

> [!TIP]
> Um die durch die Profilerstellung zurückgegebenen Informationen beizubehalten, verwenden Sie Tags oder Eigenschaften für das Modell. Die Verwendung von Tags oder Eigenschaften speichert die Daten mit dem Modell in der Modellregistrierung. Die folgenden Beispiele zeigen das Hinzufügen eines neuen Tags, das die Informationen `requestedCpu` und `requestedMemoryInGb` enthält:
>
> ```python
> model.add_tags({'requestedCpu': details['requestedCpu'],
>                 'requestedMemoryInGb': details['requestedMemoryInGb']})
> ```
>
> ```azurecli-interactive
> az ml model profile -g <resource-group-name> -w <workspace-name> --i <model-id> --add-tag requestedCpu=1 --add-tag requestedMemoryInGb=0.5
> ```

## <a name="deploy-to-target"></a>Bereitstellen auf dem Ziel

Zum Bereitstellen der Modelle wird die Bereitstellungskonfiguration für die Rückschlusskonfiguration verwendet. Der Bereitstellungsprozess ist unabhängig vom Computeziel ähnlich. Eine Bereitstellung in Azure Kubernetes Service (AKS) ist geringfügig anders, da Sie eine Referenz zum AKS-Cluster bereitstellen müssen.

### <a name="choose-a-compute-target"></a>Auswählen eines Computeziels

Sie können die folgenden Computeziele bzw. Computeressourcen verwenden, um Ihre Webdienstbereitstellung zu hosten:

[!INCLUDE [aml-compute-target-deploy](../../includes/aml-compute-target-deploy.md)]

> [!NOTE]
> * ACI eignet sich nur für kleine Modelle mit einer Größe von <1 GB. 
> * Wir empfehlen die Verwendung von AKS auf einem einzelnen Knoten für den Dev-Test von größeren Modellen.

### <a name="define-your-deployment-configuration"></a>Definieren Ihrer Bereitstellungskonfiguration

Bevor Sie Ihr Modell bereitstellen, müssen Sie die Bereitstellungskonfiguration definieren. *Die Bereitstellungskonfiguration ist spezifisch für das Computeziel, das den Webdienst hosten wird.* Beispielsweise müssen Sie, wenn Sie ein Modell lokal bereitstellen, den Port angeben, an dem der Dienst Anforderungen akzeptiert. Die Bereitstellungskonfiguration ist kein Bestandteil Ihres Eingabeskripts. Sie wird verwendet, um die Merkmale des Computeziels zu definieren, von dem das Modell und das Eingabeskript gehostet werden.

Möglicherweise müssen Sie auch die Computeressource erstellen. Dies ist beispielsweise der Fall, wenn Sie noch keine AKS-Instanz (Azure Kubernetes Service) haben, die mit Ihrem Arbeitsbereich verknüpft ist.

Die folgende Tabelle enthält ein Beispiel für das Erstellen einer Bereitstellungskonfiguration für jedes Computeziel:

| Computeziel | Beispiel für eine Bereitstellungskonfiguration |
| ----- | ----- |
| Lokal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Die Klassen für lokale, Azure Container Instances- und AKS-Webdienste können aus `azureml.core.webservice` importiert werden:

```python
from azureml.core.webservice import AciWebservice, AksWebservice, LocalWebservice
```

### <a name="securing-deployments-with-tls"></a>Sichern von Bereitstellungen mit TLS

Weitere Informationen zum Sichern einer Webdienstbereitstellung finden Sie unter [Aktivieren von TLS und Bereitstellen](how-to-secure-web-service.md#enable).

### <a name="local-deployment"></a><a id="local"></a> Lokale Bereitstellung

Um ein Modell lokal bereitzustellen, müssen Sie Docker auf Ihrem lokalen Computer installiert haben.

#### <a name="using-the-sdk"></a>Verwenden des SDK

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Weitere Informationen finden Sie in der Dokumentation zu [LocalWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

#### <a name="using-the-cli"></a>Verwenden der CLI

Wenn Sie ein Modell mit der CLI bereitstellen möchten, verwenden Sie den folgenden Befehl. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells:

```azurecli-interactive
az ml model deploy -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Weitere Informationen finden Sie in der Dokumentation zu [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

### <a name="understanding-service-state"></a>Grundlegendes zum Dienstzustand

Während der Modellimplementierung kann es vorkommen, dass sich der Dienstzustand während der vollständigen Bereitstellung ändert.

In der folgenden Tabelle werden die verschiedenen Dienstzustände beschrieben:

| Webservice-Zustand | BESCHREIBUNG | Endgültiger Zustand?
| ----- | ----- | ----- |
| Im Übergang | Der Dienst wird gerade bereitgestellt. | Nein |
| Fehlerhaft | Der Dienst wurde bereitgestellt, ist aber zurzeit nicht erreichbar.  | Nein |
| Nicht planbar | Der Dienst kann derzeit aufgrund fehlender Ressourcen nicht bereitgestellt werden. | Nein |
| Fehler | Der Dienst konnte aufgrund eines Fehlers oder Absturzes nicht bereitgestellt werden. | Ja |
| Healthy | Der Dienst ist fehlerfrei und der Endpunkt ist verfügbar. | Ja |

### <a name="compute-instance-web-service-devtest"></a><a id="notebookvm"></a> Compute-Instanz-Webdienst (Entwicklung/Test)

Siehe [Bereitstellen eines Modells auf einer Azure Machine Learning Studio-Compute-Instanz](how-to-deploy-local-container-notebook-vm.md)

### <a name="azure-container-instances-devtest"></a><a id="aci"></a> Azure Container Instances (Entwicklung/Test)

Lesen Sie dazu [Bereitstellen in Azure Container Instances](how-to-deploy-azure-container-instance.md).

### <a name="azure-kubernetes-service-devtest-and-production"></a><a id="aks"></a>Azure Kubernetes Service (Entwicklung/Test und Produktion)

Lesen Sie dazu [Bereitstellen in Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md).

### <a name="ab-testing-controlled-rollout"></a>A/B-Tests (kontrollierter Rollout)
Weitere Informationen finden Sie unter [Kontrollierter Rollout von ML-Modellen](how-to-deploy-azure-kubernetes-service.md#deploy-models-to-aks-using-controlled-rollout-preview).

## <a name="consume-web-services"></a>Nutzen von Webdiensten

Jeder bereitgestellte Webdienst stellt einen REST-Endpunkt bereit, sodass Sie Clientanwendungen in jeder beliebigen Programmiersprache erstellen können.
Wenn Sie die schlüsselbasierte Authentifizierung für Ihren Dienst aktiviert haben, müssen Sie einen Dienstschlüssel als Token in Ihrem Anforderungsheader angeben.
Wenn Sie die tokenbasierte Authentifizierung für Ihren Dienst aktiviert haben, müssen Sie ein Azure Machine Learning-JWT (JSON Web Token) als Bearertoken in Ihrem Anforderungsheader angeben. 

Der primäre Unterschied besteht darin, dass **Schlüssel statisch sind und manuell neu generiert werden können** und **Token bei Ablauf aktualisiert werden müssen**. Die schlüsselbasierte Authentifizierung wird für Webdienste unterstützt, die über Azure Container Instances und Azure Kubernetes Service bereitgestellt werden. Die tokenbasierte Authentifizierung ist **nur** für Azure Kubernetes Service-Bereitstellungen verfügbar. Weitere Informationen sowie spezifische Codebeispiele finden Sie in der [Vorgehensweise](how-to-setup-authentication.md#web-service-authentication) zur Authentifizierung.

> [!TIP]
> Sie können das JSON-Schemadokument abrufen, nachdem Sie den Dienst bereitgestellt haben. Verwenden Sie die [swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri)-Eigenschaft des bereitgestellten Webdiensts (beispielsweise `service.swagger_uri`), um den URI für die Swagger-Datei des lokalen Webdiensts abzurufen.

### <a name="request-response-consumption"></a>Nutzung von Anforderung/Antwort

Es folgt ein Beispiel, wie Sie Ihren Dienst in Python aufrufen können:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]
elif service.token_auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_token()[0]

print(headers)

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

response = requests.post(
    service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Weitere Informationen finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).

### <a name="web-service-schema-openapi-specification"></a>Webdienst Schema (OpenAPI-Spezifikation)

Wenn Sie automatische Schemagenerierung mit Ihrer Bereitstellung verwendet haben, können Sie die Adresse der OpenAPI-Spezifikation für den Dienst mithilfe der [swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri)-Eigenschaft abrufen. (Beispiel: `print(service.swagger_uri)`.) Verwenden Sie eine GET-Anforderung, oder öffnen Sie den URI in einem Browser, um die Spezifikation abzurufen.

Das folgende JSON-Dokument ist ein Beispiel für ein Schema (OpenAPI-Spezifikation), das für eine Bereitstellung generiert wurde:

```json
{
    "swagger": "2.0",
    "info": {
        "title": "myservice",
        "description": "API specification for Azure Machine Learning myservice",
        "version": "1.0"
    },
    "schemes": [
        "https"
    ],
    "consumes": [
        "application/json"
    ],
    "produces": [
        "application/json"
    ],
    "securityDefinitions": {
        "Bearer": {
            "type": "apiKey",
            "name": "Authorization",
            "in": "header",
            "description": "For example: Bearer abc123"
        }
    },
    "paths": {
        "/": {
            "get": {
                "operationId": "ServiceHealthCheck",
                "description": "Simple health check endpoint to ensure the service is up at any given point.",
                "responses": {
                    "200": {
                        "description": "If service is up and running, this response will be returned with the content 'Healthy'",
                        "schema": {
                            "type": "string"
                        },
                        "examples": {
                            "application/json": "Healthy"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        },
        "/score": {
            "post": {
                "operationId": "RunMLService",
                "description": "Run web service's model and get the prediction output",
                "security": [
                    {
                        "Bearer": []
                    }
                ],
                "parameters": [
                    {
                        "name": "serviceInputPayload",
                        "in": "body",
                        "description": "The input payload for executing the real-time machine learning service.",
                        "schema": {
                            "$ref": "#/definitions/ServiceInput"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "The service processed the input correctly and provided a result prediction, if applicable.",
                        "schema": {
                            "$ref": "#/definitions/ServiceOutput"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        }
    },
    "definitions": {
        "ServiceInput": {
            "type": "object",
            "properties": {
                "data": {
                    "type": "array",
                    "items": {
                        "type": "array",
                        "items": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                }
            },
            "example": {
                "data": [
                    [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ]
                ]
            }
        },
        "ServiceOutput": {
            "type": "array",
            "items": {
                "type": "number",
                "format": "double"
            },
            "example": [
                3726.995
            ]
        },
        "ErrorResponse": {
            "type": "object",
            "properties": {
                "status_code": {
                    "type": "integer",
                    "format": "int32"
                },
                "message": {
                    "type": "string"
                }
            }
        }
    }
}
```

Weitere Informationen finden Sie in der [OpenAPI-Spezifikation](https://swagger.io/specification/).

Ein Hilfsprogramm, das Clientbibliotheken aus der Spezifikation erstellen kann, finden Sie unter [swagger-codegen](https://github.com/swagger-api/swagger-codegen).

### <a name="batch-inference"></a><a id="azuremlcompute"></a> Batchrückschluss
Azure Machine Learning Compute-Ziele werden von Azure Machine Learning erstellt und verwaltet. Sie können für Batchvorhersagen über Azure Machine Learning-Pipelines verwendet werden.

Eine exemplarische Vorgehensweise zu Batchrückschlüssen mit Azure Machine Learning Compute finden Sie unter [Verwenden von Azure Machine Learning-Pipelines für Batchbewertung](tutorial-pipeline-batch-scoring-classification.md).

### <a name="iot-edge-inference"></a><a id="iotedge"></a> IoT Edge-Rückschluss
Unterstützung für die Bereitstellung auf Edge-Geräten befindet sich in der Vorschauphase. Weitere Informationen finden Sie unter [Bereitstellen von Azure Machine Learning als IoT Edge-Modul](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a name="update-web-services"></a><a id="update"></a> Aktualisieren von Webdiensten

[!INCLUDE [aml-update-web-service](../../includes/machine-learning-update-web-service.md)]

## <a name="continuously-deploy-models"></a>Fortlaufendes Bereitstellen von Modellen

Sie können Modelle fortlaufend bereitstellen, indem Sie die Machine Learning-Erweiterung für [Azure DevOps](https://azure.microsoft.com/services/devops/) verwenden. Sie können die Machine Learning-Erweiterung für Azure DevOps verwenden, um eine Bereitstellungspipeline auszulösen, wenn ein neues Machine Learning-Modell in einem Azure Machine Learning-Arbeitsbereich registriert wurde.

1. Registrieren Sie sich für [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), was eine kontinuierliche Integration und Bereitstellung Ihrer Anwendung auf jeder Plattform oder in jeder Cloud ermöglicht. (Beachten Sie, dass Azure Pipelines nicht mit [Machine Learning-Pipelines](concept-ml-pipelines.md#compare) identisch ist.)

1. [Erstellen eines Azure DevOps-Projekts.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Installieren Sie die [Machine Learning-Erweiterung für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList).

1. Richten Sie mit Dienstverbindungen eine Dienstprinzipalverbindung mit Ihrem Azure Machine Learning-Arbeitsbereich ein, damit Sie auf Ihre Artefakte zugreifen können. Wechseln Sie zu „Projekteinstellungen“, wählen Sie **Dienstverbindungen** aus, und wählen Sie dann **Azure Resource Manager** aus:

    [![Azure Resource Manager auswählen](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png)

1. Wählen Sie in der Liste **Bereichsebene** die Option **AzureMLWorkspace** aus, und geben Sie dann die restlichen Werte ein:

    ![AzureMLWorkspace auswählen](./media/how-to-deploy-and-where/resource-manager-connection.png)

1. Um Ihr Machine Learning-Modell kontinuierlich über Azure Pipelines bereitzustellen, wählen Sie unter „Pipelines“ die Option **Freigabe** (release) aus. Fügen Sie ein neues Artefakt hinzu, und wählen Sie dann das **AzureML Model**-Artefakt und die Dienstverbindung aus, die Sie zuvor erstellt haben. Wählen Sie das Modell und die Version aus, um eine Bereitstellung auszulösen:

    [![AzureML Model auswählen](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Aktivieren Sie den Modelltrigger auf Ihrem Modellartefakt. Wenn Sie den Trigger aktivieren, wird jedes Mal, wenn die angegebene Version (dies ist die neueste Version) dieses Modells in Ihrem Arbeitsbereich registriert wird, eine Azure DevOps-Releasepipeline ausgelöst.

    [![Modelltrigger aktivieren](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

Weitere Beispielprojekte und Beispiele finden Sie in diesen Beispielrepositorys in GitHub:

* [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

## <a name="download-a-model"></a>Herunterladen eines Modells
Wenn Sie Ihr Modell herunterladen möchten, um es in Ihrer eigenen Ausführungsumgebung zu verwenden, können Sie dazu folgende SDK-/CLI-Befehlen ausführen:

SDK:
```python
model_path = Model(ws,'mymodel').download()
```

Über die CLI:
```azurecli-interactive
az ml model download --model-id mymodel:1 --target-dir model_folder
```

## <a name="preview-no-code-model-deployment"></a>(Vorschau) Modellimplementierung ohne Code

Die Modellimplementierung ohne Code ist derzeit in der Vorschauphase und unterstützt die folgenden Frameworks für maschinelles Lernen:

### <a name="tensorflow-savedmodel-format"></a>Tensorflow SavedModel-Format
Tensorflow-Modelle müssen im **SavedModel-Format** registriert werden, um mit der Modellbereitstellung ohne Code zu funktionieren.

Informationen zum Erstellen von „SavedModel“ finden Sie unter [diesem Link](https://www.tensorflow.org/guide/saved_model).

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='flowers',                        # Name of the registered model in your workspace.
                       model_path='./flowers_model',                # Local Tensorflow SavedModel folder to upload and register as a model.
                       model_framework=Model.Framework.TENSORFLOW,  # Framework used to create the model.
                       model_framework_version='1.14.0',            # Version of Tensorflow used to create the model.
                       description='Flowers model')

service_name = 'tensorflow-flower-service'
service = Model.deploy(ws, service_name, [model])
```

### <a name="onnx-models"></a>ONNX-Modelle

Die Registrierung und Bereitstellung von ONNX-Modellen wird für alle ONNX-Rückschlussgraphen unterstützt. Schritte zur Vor- und Nachverarbeitung werden derzeit nicht unterstützt.

Nachfolgend sehen Sie ein Beispiel der Registrierung und Implementierung eines MNIST ONNX-Modells:

```python
from azureml.core import Model

model = Model.register(workspace=ws,
                       model_name='mnist-sample',                  # Name of the registered model in your workspace.
                       model_path='mnist-model.onnx',              # Local ONNX model to upload and register as a model.
                       model_framework=Model.Framework.ONNX ,      # Framework used to create the model.
                       model_framework_version='1.3',              # Version of ONNX used to create the model.
                       description='Onnx MNIST model')

service_name = 'onnx-mnist-service'
service = Model.deploy(ws, service_name, [model])
```

Informationen zum Bewerten eines Modells finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service). Viele ONNX-Projekte verwenden protobuf-Dateien, um Trainings- und Validierungsdaten kompakt zu speichern. Dadurch kann es schwierig sein, das vom Dienst erwartete Datenformat zu erkennen. Als Modellentwickler sollten Sie Folgendes für Ihre Entwickler dokumentieren:

* Eingabeformat (JSON- oder Binärformat)
* Eingabedatenform und -typ (z. B. ein Array von Gleitkommazahlen in der Form [100,100,3])
* Domäneninformationen (z. B. bei einem Bild der Farbraum, die Komponentenreihenfolge und die Angabe, ob die Werte normalisiert sind)

Wenn Sie Pytorch verwenden, enthält der [Export von Modellen aus PyTorch zu ONNX](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb) die Details zur Konvertierung und zu den Einschränkungen. 

### <a name="scikit-learn-models"></a>Scikit-learn-Modelle

Für alle integrierten Scikit-learn-Modelltypen wird keine Modellimplementierung ohne Code unterstützt.

Nachfolgend sehen Sie ein Beispiel der Registrierung und Implementierung eines Scikit-learn-Modells ohne zusätzlichen Code:

```python
from azureml.core import Model
from azureml.core.resource_configuration import ResourceConfiguration

model = Model.register(workspace=ws,
                       model_name='my-sklearn-model',                # Name of the registered model in your workspace.
                       model_path='./sklearn_regression_model.pkl',  # Local file to upload and register as a model.
                       model_framework=Model.Framework.SCIKITLEARN,  # Framework used to create the model.
                       model_framework_version='0.19.1',             # Version of scikit-learn used to create the model.
                       resource_configuration=ResourceConfiguration(cpu=1, memory_in_gb=0.5),
                       description='Ridge regression model to predict diabetes progression.',
                       tags={'area': 'diabetes', 'type': 'regression'})
                       
service_name = 'my-sklearn-service'
service = Model.deploy(ws, service_name, [model])
```

HINWEIS:  Modelle, die predict_proba unterstützen, verwenden diese Methode standardmäßig. Sie können den POST-Text wie unten beschrieben ändern, um dies für die Verwendung von Vorhersagen außer Kraft zu setzen:
```python
import json


input_payload = json.dumps({
    'data': [
        [ 0.03807591,  0.05068012,  0.06169621, 0.02187235, -0.0442235,
         -0.03482076, -0.04340085, -0.00259226, 0.01990842, -0.01764613]
    ],
    'method': 'predict'  # If you have a classification model, the default behavior is to run 'predict_proba'.
})

output = service.run(input_payload)

print(output)
```

HINWEIS:  Diese Abhängigkeiten sind im vordefinieren Scikit-learn-Container für Rückschlüsse enthalten:

```yaml
    - dill
    - azureml-defaults
    - inference-schema[numpy-support]
    - scikit-learn
    - numpy
    - joblib
    - pandas
    - scipy
    - sklearn_pandas
```

## <a name="package-models"></a>Paketmodelle

In einigen Fällen möchten Sie möglicherweise ein Docker-Image erstellen, ohne das Modell bereitzustellen (beispielsweise wenn Sie [in Azure App Service bereitstellen](how-to-deploy-app-service.md) möchten). Oder Sie möchten das Image herunterladen und in einer lokalen Docker-Installation ausführen. Es könnte sogar vorkommen, dass Sie die für die Imageerstellung verwendeten Dateien herunterladen, untersuchen und ändern und das Image dann manuell erstellen möchten.

Diese Schritte können Sie bei der Modellpaketerstellung ausführen. Dabei werden alle Ressourcen, die zum Hosten eines Modells als Webdienst benötigt werden, in einem Paket zusammengefasst, und Sie erhalten die Möglichkeit, entweder ein vollständig erstelltes Docker-Image oder die zum Erstellen eines solchen Images erforderlichen Dateien herunterzuladen. Es gibt zwei Möglichkeiten, die Modellpaketerstellung zu verwenden:

**Herunterladen eines Modellpakets:** Laden Sie ein Docker-Image herunter, in dem das Modell und die anderen Dateien enthalten sind, die dazu erforderlich sind, es als Webdienst zu hosten.

**Generieren eines Dockerfile:** Laden Sie das Dockerfile, das Modell, das Eingabeskript und andere Ressourcen herunter, die zum Erstellen eines Docker-Images benötigt werden. Sie können dann die Dateien untersuchen oder Änderungen daran vornehmen, bevor Sie das Image lokal erstellen.

Beide Pakete können verwendet werden, um ein lokales Docker-Image abzurufen.

> [!TIP]
> Das Erstellen eines Pakets ist mit dem Bereitstellen eines Modells vergleichbar. Sie verwenden ein registriertes Modell und eine Rückschlusskonfiguration.

> [!IMPORTANT]
> Damit Sie ein vollständig erstelltes Image herunterladen oder ein Image lokal erstellen können, muss [Docker](https://www.docker.com) in Ihrer Entwicklungsumgebung installiert sein.

### <a name="download-a-packaged-model"></a>Herunterladen eines Modellpakets

Im folgenden Beispiel wird ein Image erstellt, das in der Azure-Containerregistrierung für Ihren Arbeitsbereich registriert wird:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Nachdem Sie ein Paket erstellt haben, können Sie `package.pull()` verwenden, um das Image per Pullvorgang in Ihre lokale Docker-Umgebung zu übertragen. In der Ausgabe dieses Befehls wird der Name des Images angezeigt. Beispiel: 

`Status: Downloaded newer image for myworkspacef78fd10.azurecr.io/package:20190822181338`. 

Nachdem Sie das Modell heruntergeladen haben, verwenden Sie den `docker images`-Befehl, um die lokalen Images aufzulisten:

```text
REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
myworkspacef78fd10.azurecr.io/package    20190822181338      7ff48015d5bd        4 minutes ago       1.43 GB
```

Um einen lokalen Container auf Basis dieses Images zu starten, verwenden Sie den folgenden Befehl, um einen benannten Container aus der Shell oder Befehlszeile zu starten. Ersetzen Sie den `<imageid>`-Wert durch die Image-ID, die vom Befehl `docker images` zurückgegeben wurde.

```bash
docker run -p 6789:5001 --name mycontainer <imageid>
```

Mit diesem Befehl wird die aktuelle Version des Images mit dem Namen `myimage` gestartet. Der Befehl ordnet den lokalen Port 6789 dem Port des Containers zu, an dem der Webdienst lauscht (5001). Außerdem weist der Befehl dem Container den Namen `mycontainer` zu, wodurch ein Beenden des Containers vereinfacht wird. Nachdem der Container gestartet wurde, können Sie Anforderungen an `http://localhost:6789/score` senden.

### <a name="generate-a-dockerfile-and-dependencies"></a>Generieren eines Dockerfile und Generieren von Abhängigkeiten

Im folgenden Beispiel wird gezeigt, wie Sie das Dockerfile, das Modell und andere Ressourcen herunterladen, die erforderlich sind, um ein Image lokal zu erstellen. Mit dem Parameter `generate_dockerfile=True` ist angegeben, dass Sie kein vollständig erstelltes Image, sondern die Dateien herunterladen möchten.

```python
package = Model.package(ws, [model], inference_config, generate_dockerfile=True)
package.wait_for_creation(show_output=True)
# Download the package.
package.save("./imagefiles")
# Get the Azure container registry that the model/Dockerfile uses.
acr=package.get_container_registry()
print("Address:", acr.address)
print("Username:", acr.username)
print("Password:", acr.password)
```

Mit diesem Code werden die Dateien heruntergeladen, die zum Erstellen des Images im Verzeichnis `imagefiles` benötigt werden. Das Dockerfile, das zu den gespeicherten Dateien gehört, verweist auf ein Basisimage in einer Azure-Containerregistrierung. Wenn Sie das Image in Ihrer lokalen Docker-Installation erstellen, müssen Sie die Adresse, den Benutzernamen und das Kennwort verwenden, um die Authentifizierung bei der Registrierung vorzunehmen. Führen Sie die folgenden Schritte aus, um das Image durch Verwenden einer lokalen Docker-Installation zu erstellen:

1. Verwenden Sie in einer Shell- oder Befehlszeilensitzung den folgenden Befehl, um für Docker die Authentifizierung bei Azure-Containerregistrierung durchzuführen. Ersetzen Sie `<address>`, `<username>` und `<password>` durch die Werte, die mit `package.get_container_registry()` abgerufen wurden.

    ```bash
    docker login <address> -u <username> -p <password>
    ```

2. Verwenden Sie den folgenden Befehl, um das Image zu erstellen. Ersetzen Sie `<imagefiles>` durch den Pfad des Verzeichnisses, in dem die Dateien mit `package.save()` gespeichert wurden.

    ```bash
    docker build --tag myimage <imagefiles>
    ```

    Mit diesem Befehl wird der Imagename auf `myimage` festgelegt.

Verwenden Sie den Befehl `docker images`, um zu überprüfen, ob das Image erstellt wurde. In der Liste sollte das Image `myimage` angezeigt werden:

```text
REPOSITORY      TAG                 IMAGE ID            CREATED             SIZE
<none>          <none>              2d5ee0bf3b3b        49 seconds ago      1.43 GB
myimage         latest              739f22498d64        3 minutes ago       1.43 GB
```

Verwenden Sie den folgenden Befehl, um basierend auf diesem Image einen neuen Container zu starten:

```bash
docker run -p 6789:5001 --name mycontainer myimage:latest
```

Mit diesem Befehl wird die aktuelle Version des Images mit dem Namen `myimage` gestartet. Der Befehl ordnet den lokalen Port 6789 dem Port des Containers zu, an dem der Webdienst lauscht (5001). Außerdem weist der Befehl dem Container den Namen `mycontainer` zu, wodurch ein Beenden des Containers vereinfacht wird. Nachdem der Container gestartet wurde, können Sie Anforderungen an `http://localhost:6789/score` senden.

### <a name="example-client-to-test-the-local-container"></a>Beispielclient zum Testen des lokalen Containers

Der folgende Code ist ein Beispiel für einen Python-Client, der mit dem Container verwendet werden kann:

```python
import requests
import json

# URL for the web service.
scoring_uri = 'http://localhost:6789/score'

# Two sets of data to score, so we get two results back.
data = {"data":
        [
            [ 1,2,3,4,5,6,7,8,9,10 ],
            [ 10,9,8,7,6,5,4,3,2,1 ]
        ]
        }
# Convert to JSON string.
input_data = json.dumps(data)

# Set the content type.
headers = {'Content-Type': 'application/json'}

# Make the request and display the response.
resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.text)
```

Beispielclients in anderen Programmiersprachen finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).

### <a name="stop-the-docker-container"></a>Beenden des Docker-Containers

Verwenden Sie zum Beenden des Containers den folgenden Befehl über eine andere Shell- oder Befehlszeileninstanz:

```bash
docker kill mycontainer
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.
Verwenden Sie zum Löschen eines registrierten Modells `model.delete()`.

Weitere Informationen finden Sie in der Dokumentation zu [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) und [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

<a id="advanced-entry-script"></a>
## <a name="advanced-entry-script-authoring"></a>Erweiterte Eingabeskripterstellung

<a id="binary"></a>

### <a name="binary-data"></a>Binärdaten

Wenn Ihr Modell Binärdaten (beispielsweise ein Bild) akzeptiert, müssen Sie die für Ihre Bereitstellung verwendete Datei `score.py` so ändern, dass sie HTTP-Rohanforderungen akzeptiert. Um Rohdaten zu akzeptieren, verwenden Sie die `AMLRequest`-Klasse in Ihrem Eingangsskript und fügen der `run()`-Funktion den `@rawhttp`-Decorator hinzu.

Hier ist ein Beispiel für eine Datei `score.py`, die Binärdaten akzeptiert:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse


def init():
    print("This is init()")


@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example just returns the posted data as the response.
        return AMLResponse(reqBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Die `AMLRequest`-Klasse befindet sich im `azureml.contrib`-Namespace. Elemente in diesem Namespace ändern sich häufig, während wir daran arbeiten, den Dienst zu verbessern. Alles in diesem Namespace sollte als Vorschau betrachtet werden, die von Microsoft nicht vollständig unterstützt wird.
>
> Wenn Sie dies in Ihrer lokalen Entwicklungsumgebung testen müssen, können Sie die Komponenten mit dem folgenden Befehl installieren:
>
> ```shell
> pip install azureml-contrib-services
> ```

Die Klasse `AMLRequest` ermöglicht Ihnen nur den Zugriff auf die unformatiert bereitgestellten Daten in der „score.py“, es gibt keine clientseitige Komponente. Von einem Client aus werden die Daten ganz normal bereitgestellt. Der folgende Python-Code liest z. B. eine Imagedatei und stellt die Daten bereit:

```python
import requests
# Load image data
data = open('example.jpg', 'rb').read()
# Post raw data to scoring URI
res = requests.post(url='<scoring-uri>', data=data, headers={'Content-Type': 'application/octet-stream'})
```

<a id="cors"></a>

### <a name="cross-origin-resource-sharing-cors"></a>Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)

Ressourcenfreigabe zwischen verschiedenen Ursprüngen ist eine Möglichkeit, um die Anforderung eingeschränkter Ressourcen auf einer Webseite aus anderen Domänen zuzulassen. CORS funktioniert über HTTP-Header, die mit der Clientanforderung gesendet und mit der Dienstantwort zurückgegeben werden. Weitere Informationen zu CORS und gültigen Headern finden Sie in Wikipedia unter [Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Um Ihre Modellimplementierung für die Unterstützung von CORS zu konfigurieren, verwenden Sie die `AMLResponse`-Klasse in Ihrem Eingangsskript. Diese Klasse ermöglicht es Ihnen, die Header für das Antwortobjekt festzulegen.

Im folgenden Beispiel wird der `Access-Control-Allow-Origin`-Header für die Antwort aus dem Eingangsskript festgelegt:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs.
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real-world solution, you would load the data from reqBody
        # and send it to the model. Then return the response.

        # For demonstration purposes, this example
        # adds a header and returns the request body.
        resp = AMLResponse(reqBody, 200)
        resp.headers['Access-Control-Allow-Origin'] = "http://www.example.com"
        return resp
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Die `AMLResponse`-Klasse befindet sich im `azureml.contrib`-Namespace. Elemente in diesem Namespace ändern sich häufig, während wir daran arbeiten, den Dienst zu verbessern. Alles in diesem Namespace sollte als Vorschau betrachtet werden, die von Microsoft nicht vollständig unterstützt wird.
>
> Wenn Sie dies in Ihrer lokalen Entwicklungsumgebung testen müssen, können Sie die Komponenten mit dem folgenden Befehl installieren:
>
> ```shell
> pip install azureml-contrib-services
> ```


> [!WARNING]
> Azure Machine Learning leitet nur POST- und GET-Anforderungen an die Container weiter, die den Bewertungsdienst ausführen. Dies kann zu Fehlern durch Browser führen, die OPTIONS-Anforderungen für CORS-Preflightanforderungen verwenden.
> 

## <a name="next-steps"></a>Nächste Schritte

* [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md)
* [Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI](how-to-troubleshoot-deployment.md)
* [Verwenden von TLS zum Absichern eines Webdiensts mit Azure Machine Learning](how-to-secure-web-service.md)
* [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md)
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [Erstellen von Ereigniswarnungen und Triggern für Modellbereitstellungen](how-to-use-event-grid.md)

