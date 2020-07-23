---
title: Aktivieren der Protokollierung in Azure Machine Learning
description: Erfahren Sie, wie Sie die Protokollierung in Azure Machine Learning unter Verwendung sowohl des Python-Standardpakets für die Protokollierung als auch der SDK-spezifischen Funktionen aktivieren.
ms.author: larryfr
author: BlackMist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 03/05/2020
ms.custom: tracking-python
ms.openlocfilehash: c8f9923f9a5c4954fbc4682f464100b18eb469eb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536197"
---
# <a name="enable-logging-in-azure-machine-learning"></a>Aktivieren der Protokollierung in Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Das Python-SDK für Azure Machine Learning gestattet Ihnen die Aktivierung der Protokollierung unter Verwendung sowohl des Python-Standardpakets für die Protokollierung als auch der SDK-spezifischen Funktionen für die lokale Protokollierung sowie die Protokollierung in Ihrem Arbeitsbereich im Portal. Protokolle bieten Entwicklern Echtzeitinformationen zum Anwendungszustand und können bei der Diagnose von Fehlern oder Warnungen helfen. In diesem Artikel lernen Sie verschiedene Möglichkeiten kennen, um die Protokollierung in den folgenden Bereichen zu aktivieren:

> [!div class="checklist"]
> * Trainieren von Modellen und Computeziele
> * Erstellung von Images
> * Bereitgestellte Modelle
> * Python-Einstellungen für `logging`

[Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md). Weitere Informationen zum SDK finden Sie im [Handbuch](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="training-models-and-compute-target-logging"></a>Protokollierung für das Trainieren von Modellen und Computeziele

Es gibt mehrere Möglichkeiten zum Aktivieren der Protokollierung während des Modelltrainingsprozesses, und die unten gezeigten Beispielen illustrieren gängige Entwurfsmuster. Sie können ausführungsbezogene Daten ganz einfach in Ihrem Arbeitsbereich in der Cloud protokollieren, indem Sie die `start_logging`-Funktion in der `Experiment`-Klasse verwenden.

```python
from azureml.core import Experiment

exp = Experiment(workspace=ws, name='test_experiment')
run = exp.start_logging()
run.log("test-val", 10)
```

Zusätzliche Protokollierungsfunktionen finden Sie in der Referenzdokumentation zur [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)-Klasse.

Verwenden Sie zum Aktivieren der lokalen Protokollierung des Anwendungszustands während des Trainingsfortschritts den Parameter `show_output`. Das Aktivieren der ausführlichen Protokollierung erlaubt es Ihnen, Details aus dem Trainingsprozess sowie Informationen zu allen Remoteressourcen oder Computezielen anzuzeigen. Verwenden Sie den folgenden Code, um die Protokollierung bei Experimentübermittlung zu aktivieren.

```python
from azureml.core import Experiment

experiment = Experiment(ws, experiment_name)
run = experiment.submit(config=run_config_object, show_output=True)
```

Sie können denselben Parameter auch in der `wait_for_completion`-Funktion der resultierende Ausführung verwenden.

```python
run.wait_for_completion(show_output=True)
```

Das SDK unterstützt außerdem die Verwendung des Python-Standardpakets für die Protokollierung in bestimmten Szenarien für das Training. Im folgenden Beispiel wird die Protokollierungsstufe `INFO` in einem `AutoMLConfig`-Objekt aktiviert.

```python
from azureml.train.automl import AutoMLConfig
import logging

automated_ml_config = AutoMLConfig(task='regression',
                                   verbosity=logging.INFO,
                                   X=your_training_features,
                                   y=your_training_labels,
                                   iterations=30,
                                   iteration_timeout_minutes=5,
                                   primary_metric="spearman_correlation")
```

Sie können auch den Parameter `show_output` verwenden, wenn Sie ein permanentes Computeziel erstellen. Geben Sie den Parameter in der `wait_for_completion`-Funktion an, um die während der Erstellung des Computeziels zu aktivieren.

```python
from azureml.core.compute import ComputeTarget

compute_target = ComputeTarget.attach(
    workspace=ws, name="example", attach_configuration=config)
compute.wait_for_completion(show_output=True)
```

## <a name="logging-for-deployed-models"></a>Protokollierung für bereitgestellte Modelle

Um Protokolle aus einem zuvor bereitgestellten Webdienst abzurufen, laden Sie den Dienst, und verwenden Sie die `get_logs()`-Funktion. Die Protokolle können ausführliche Informationen zu allen Fehlern enthalten, die während der Bereitstellung aufgetreten sind.

```python
from azureml.core.webservice import Webservice

# load existing web service
service = Webservice(name="service-name", workspace=ws)
logs = service.get_logs()
```

Sie können auch benutzerdefinierte Stapelüberwachungen für Ihren Webdienst protokollieren, indem Sie Application Insights aktivieren, das Ihnen die Überwachung von Anforderungs-/Antwortzeiten, Fehlerraten und Ausnahmen ermöglicht. Rufen Sie die `update()`-Funktion für einen vorhandenen Webdienst auf, um Application Insights zu aktivieren.

```python
service.update(enable_app_insights=True)
```

Weitere Informationen finden Sie unter [Überwachen und Erfassen von Daten von ML-Webdienst-Endpunkten](how-to-enable-app-insights.md).

## <a name="python-native-logging-settings"></a>Native Protokollierungseinstellungen für Python

Bestimmte Protokolle im SDK können einen Fehler enthalten, der Sie anweist, den Protokolliergrad auf DEBUG festzulegen. Um den Protokolliergrad festzulegen, fügen Sie Ihrem Skript den folgenden Code hinzu.

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Erfassen von Daten von ML-Webdienst-Endpunkten](how-to-enable-app-insights.md)
