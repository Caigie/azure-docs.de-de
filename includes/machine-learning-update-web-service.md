---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: e6a7ee8ed497c87c08f13e09a0022b0835c952cc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82975084"
---
Verwenden Sie die `update`-Methode, um einen Webdienst zu aktualisieren. Sie können den Webdienst aktualisieren, um ein neues Modell, ein neues Eingabeskript oder neue Abhängigkeiten zu verwenden, die in einer Rückschlusskonfiguration angegeben werden können. Weitere Informationen finden Sie in der Dokumentation zu [Webservice.update](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py#update--args-).

> [!IMPORTANT]
> Wenn Sie eine neue Version eines Modells erstellen, müssen Sie jeden Dienst, der die neue Version verwenden soll, manuell aktualisieren.
>
> Sie können nicht das SDK verwenden, um einen Webdienst zu aktualisieren, der über den Azure Machine Learning-Designer veröffentlicht wurde.

**Verwenden des SDK**

Im folgenden Code wird gezeigt, wie Sie mithilfe des SDK das Modell, die Umgebung und das Eingabeskript für einen Webdienst aktualisieren:

```python
from azureml.core import Environment
from azureml.core.webservice import Webservice
from azureml.core.model import Model, InferenceConfig

# Register new model.
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

# Use version 3 of the environment.
deploy_env = Environment.get(workspace=ws,name="myenv",version="3")
inference_config = InferenceConfig(entry_script="score.py",
                                   environment=deploy_env)

service_name = 'myservice'
# Retrieve existing service.
service = Webservice(name=service_name, workspace=ws)



# Update to new model(s).
service.update(models=[new_model], inference_config=inference_config)
print(service.state)
print(service.get_logs())
```

**Verwenden der CLI**

Sie können einen Webdienst auch über die ML CLI aktualisieren. Im folgenden Beispiel wird veranschaulicht, wie Sie ein neues Modell registrieren und anschließend einen Webdienst für die Verwendung des neuen Modells aktualisieren:

```azurecli
az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --output-metadata-file modelinfo.json
az ml service update -n myservice --model-metadata-file modelinfo.json
```

> [!TIP]
> In diesem Beispiel wird ein JSON-Dokument verwendet, um die Modellinformationen vom Registrierungsbefehl an den Aktualisierungsbefehl zu übergeben.
>
> Um den Dienst so zu aktualisieren, dass für ihn ein neues Eingabeskript oder eine neue Umgebung verwendet wird, erstellen Sie eine [Rückschlusskonfigurationsdatei](/azure/machine-learning/reference-azure-machine-learning-cli#inference-configuration-schema), und geben Sie diese mit dem `ic`-Parameter an.

Weitere Informationen finden Sie in der Dokumentation zu [az ml service update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/service?view=azure-cli-latest#ext-azure-cli-ml-az-ml-service-update).