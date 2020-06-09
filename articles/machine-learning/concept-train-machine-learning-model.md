---
title: Erstellen und Trainieren von Modellen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, mit welchen Methoden Sie das Modell mit Azure Machine Learning trainieren können. Estimators bieten eine einfache Möglichkeit, mit beliebten Frameworks wie Scikit-learn, TensorFlow, Keras, PyTorch und Chainer zu arbeiten. Machine Learning-Pipelines erleichtern die Planung unbeaufsichtigter Ausführungen, die Nutzung heterogener Computeumgebungen und die Wiederverwendung von Teilen Ihres Workflows. Außerdem bieten Laufzeitkonfigurationen eine differenzierte Kontrolle der Computeziele, auf denen der Trainingsprozess ausgeführt wird.
services: machine-learning
ms.service: machine-learning
author: Blackmist
ms.author: larryfr
ms.subservice: core
ms.topic: conceptual
ms.date: 05/13/2020
ms.openlocfilehash: 99e2c878443b9a4256eec495429dbe57a88557d0
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683012"
---
# <a name="train-models-with-azure-machine-learning"></a>Trainieren von Modellen mit Azure Machine Learning

Azure Machine Learning bietet verschiedene Methoden zum Trainieren von Modellen, von Code-First-Lösungen mit dem SDK bis zu Low-Code-Lösungen wie automatisiertes ML und der visuelle Designer. Anhand der folgenden Liste können Sie ermitteln, welche Trainingsmethode für Sie die richtige ist:

+ [Azure Machine Learning SDK für Python](#python-sdk): Das Python SDK bietet verschiedene Möglichkeiten, Modelle mit jeweils unterschiedlichen Funktionen zu trainieren.

    | Trainingsmethode | BESCHREIBUNG |
    | ----- | ----- |
    | [Laufzeitkonfiguration](#run-configuration) | Eine **generische Möglichkeit zum Trainieren von Modellen** besteht darin, ein Trainingsskript und eine Laufzeitkonfiguration zu verwenden. Die Laufzeitkonfiguration stellt die zum Konfigurieren der Trainingsumgebung, erforderlichen Informationen bereit, in der Ihr Modell trainiert wird. Sie können eine Laufzeitkonfiguration, Ihr Trainingsskript und ein Computeziel (die Trainingsumgebung) verwenden und einen Trainingsauftrag ausführen. |
    | [Automatisiertes maschinelles Lernen](#automated-machine-learning) | Mithilfe des automatisierten maschinellen Lernens können Sie **Modelle ohne umfassende Data Science- oder Programmierkenntnisse trainieren**. Für Personen mit Data Science- und Programmierungshintergrund bietet es eine Möglichkeit, Zeit und Ressourcen zu sparen, indem die Algorithmusauswahl und die Hyperparameteroptimierung automatisiert werden. Bei der Verwendung des automatisierten maschinellen Lernens müssen Sie sich nicht um die Definition einer Laufzeitkonfiguration kümmern. |
    | [Schätzfunktionen](#estimators) | Mithilfe der Estimator-Klassen können **Modelle auf Grundlage beliebter Frameworks für maschinelles Lernen einfach trainiert werden**. Es gibt Estimator-Klassen für **Scikit-learn**, **PyTorch**, **TensorFlow**, **Chainer** und **Ray RLlib**. Es gibt auch einen allgemeinen Estimator, der mit Frameworks verwendet werden kann, die noch keine dedizierte Estimator-Klasse aufweisen. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden. |
    | [Machine Learning-Pipeline](#machine-learning-pipeline) | Pipelines sind keine andere Trainingsmethode, sondern eine **Möglichkeit, einen Workflow mit modularen, wiederverwendbaren Schritten zu definieren**, die Training als Teil des Workflows enthalten können. Machine Learning-Pipelines unterstützen die Verwendung des automatisierten maschinellen Lernens sowie die Verwendung von Estimators und der Laufzeitkonfiguration zum Trainieren von Modellen. Da Pipelines nicht speziell auf das Training ausgerichtet sind, variieren die Gründe für den Einsatz einer Pipeline stärker als die anderen Trainingsmethoden. Im Allgemeinen können Sie eine Pipeline in folgenden Situationen verwenden:<br>* Sie möchten **unbeaufsichtigte Prozesse planen**, z. B. zeitintensive Trainingsaufträge oder Datenaufbereitungen.<br>* Sie möchten **mehrere Schritte** verwenden, die über heterogene Computeressourcen und Speicherorte hinweg koordiniert sind.<br>* Sie möchten die Pipeline als **wiederverwendbare Vorlage** für bestimmte Szenarien verwenden, z. B. für erneutes Training oder Batchbewertungen.<br>* **Nachverfolgung und Versionierung von Datenquellen, Eingaben und Ausgaben** für Ihren Workflow.<br>* Ihr Workflow wird von **verschiedenen Teams implementiert, die unabhängig voneinander an bestimmten Schritten arbeiten**. Die Schritte können dann in einer Pipeline zusammengeführt werden, um den Workflow zu implementieren. |

+ [Azure Machine Learning SDK für Python](#r-sdk): Das SDK verwendet das Paket reticulate zur Bindung an das Python-SDK von Azure Machine Learning. Dadurch erhalten Sie Zugriff auf die im Python-SDK implementierten Kernobjekte und -methoden aus jeder R-Umgebung.

+ **Designer**: Azure Machine Learning-Designer (Vorschauversion) bietet einen einfachen Einstiegspunkt in das maschinelle Lernen zum Erstellen von Proof of Concepts oder für Benutzer mit wenig Programmiererfahrung. Sie ermöglicht es Ihnen, Modelle per Drag & Drop über eine webbasierte Benutzeroberfläche zu trainieren. Sie können Python-Code als Teil des Designs verwenden oder Modelle trainieren, ohne Code zu schreiben.

+ **CLI**: Die Befehlszeilenschnittstelle für das maschinelle Lernen stellt Befehle für häufige Aufgaben mit Azure Machine Learning bereit und wird häufig für **Skripting- und Automatisierungsaufgaben** verwendet. Nachdem Sie z. B. ein Trainingsskript oder eine Pipeline erstellt haben, können Sie mit der Befehlszeilenschnittstelle einen Trainingsdurchlauf starten, der sich nach einem Zeitplan oder nach der Aktualisierung der für das Training verwendeten Datendateien richtet. Für Trainingsmodelle werden Befehle bereitgestellt, die Trainingsaufträge übermitteln. Sie kann Aufträge über Laufzeitkonfigurationen oder Pipelines übermitteln.

Jede dieser Trainingsmethoden kann verschiedene Arten von Computeressourcen für das Training verwenden. Zusammenfassend werden diese Ressourcen als [__Computeziele__](concept-azure-machine-learning-architecture.md#compute-targets) bezeichnet. Ein Computeziel kann ein lokaler Computer oder eine Cloudressource sein, wie beispielsweise Azure Machine Learning Compute, Azure HDInsight oder ein virtueller Remotecomputer.

## <a name="python-sdk"></a>Python SDK

Das Azure Machine Learning SDK für Python ermöglicht es Ihnen, Workflows für maschinelles Lernen mit Azure Machine Learning zu erstellen und auszuführen. Sie können mit dem Dienst über eine interaktive Python-Sitzung, Jupyter Notebooks, Visual Studio Code oder eine andere integrierte Entwicklungsumgebung (IDE) interagieren.

* [Was ist das Azure Machine Learning SDK für Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Installieren/Aktualisieren des SDKs](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning](how-to-configure-environment.md)

### <a name="run-configuration"></a>Laufzeitkonfiguration

Ein allgemeiner Trainingsauftrag mit Azure Machine Learning kann mit der [Laufzeitkonfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) definiert werden. Die Laufzeitkonfiguration wird dann zusammen mit Ihren Trainingsskripts verwendet, um ein Modell auf einem Computeziel zu trainieren.

Sie können mit einer Laufzeitkonfiguration für Ihren lokalen Computer beginnen und dann bei Bedarf zu einer Laufzeitkonfiguration für ein cloudbasiertes Computeziel wechseln. Wenn Sie das Computeziel ändern, wird nur die von Ihnen verwendete Laufzeitkonfiguration geändert. Eine Ausführung protokolliert auch Informationen zum Trainingsauftrag wie Eingaben, Ausgaben und Protokolle.

* [Was ist eine Laufzeitkonfiguration?](concept-azure-machine-learning-architecture.md#run-configurations)
* [Tutorial: Trainieren Ihres ersten ML-Modells](tutorial-1st-experiment-sdk-train.md)
* [Beispiele: Jupyter Notebook-Beispiele für Trainingsmodelle](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training)
* [Vorgehensweise: Einrichten und Verwenden von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)

### <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

Definieren Sie Iterationen, Hyperparametereinstellungen, Featurebereitstellungen und andere Einstellungen. Während des Trainings testet Azure Machine Learning verschiedene Algorithmen und Parameter gleichzeitig. Das Training wird beendet, sobald es die von Ihnen definierten Beendigungskriterien erfüllt. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden.

> [!TIP]
> Zusätzlich zum Python SDK können Sie automatisiertes maschinelles Lernen auch über [Azure Machine Learning-Studio](https://ml.azure.com) verwenden.

* [Was ist automatisiertes maschinelles Lernen?](concept-automated-ml.md)
* [Tutorial: Erstellen Ihres ersten Klassifizierungsmodells mit automatisiertem maschinellen Lernen](tutorial-first-experiment-automated-ml.md)
* [Tutorial: Vorhersagen von Preisen für Taxifahrten mit automatisiertem maschinellen Lernen](tutorial-auto-train-models.md)
* [Beispiele: Jupyter Notebook-Beispiele für automatisiertes maschinelles Lernen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
* [Vorgehensweise: Konfigurieren automatisierter ML-Experimente in Python](how-to-configure-auto-train.md)
* [Vorgehensweise: Automatisches Trainieren eines Modells für die Zeitreihenprognose](how-to-auto-train-forecast.md)
* [Vorgehensweise: Erstellen, Untersuchen und Bereitstellen von Experimenten mit automatisiertem maschinellem Lernen mit Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md)

### <a name="estimators"></a>Schätzfunktionen

Estimators vereinfachen das Training von Modellen mit beliebten ML-Frameworks. Wenn Sie **Scikit-learn**, **PyTorch**, **TensorFlow**, **Chainer** oder **Ray RLlib** verwenden, sollten Sie einen Schätzer für das Training verwenden. Es gibt auch einen allgemeinen Estimator, der mit Frameworks verwendet werden kann, die noch keine dedizierte Estimator-Klasse aufweisen. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden.

* [Was sind Estimators?](concept-azure-machine-learning-architecture.md#estimators)
* [Tutorial: Trainieren von Bildklassifikationsmodellen mit MNIST-Daten und Scikit-learn mithilfe von Azure Machine Learning](tutorial-train-models-with-aml.md)
* [Beispiele: Jupyter Notebook-Beispiele zur Verwendung von Estimators](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)
* [Vorgehensweise: Erstellen von Estimators im Training](how-to-train-ml-models.md)

### <a name="machine-learning-pipeline"></a>Machine Learning-Pipeline

Machine Learning-Pipelines können die zuvor genannten Trainingsmethoden (Laufzeitkonfiguration, Estimators und automatisiertes maschinelles Lernen) nutzen. Bei Pipelines geht es mehr um die Erstellung eines Workflows, daher umfassen sie mehr als nur das Training von Modellen. In einer Pipeline können Sie ein Modell mit automatisiertem maschinellen Lernen, Estimators oder Laufzeitkonfigurationen trainieren.

* [Was sind ML-Pipelines in Azure Machine Learning?](concept-ml-pipelines.md)
* [Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning SDK](how-to-create-your-first-pipeline.md)
* [Tutorial: Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md)
* [Beispiele: Jupyter Notebook-Beispiele für Machine Learning-Pipelines](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines)
* [Beispiele: Pipeline mit automatisiertem maschinellen Lernen](https://aka.ms/pl-automl)
* [Beispiele: Pipeline mit Estimators](https://aka.ms/pl-estimator)

## <a name="r-sdk"></a>R SDK

Mit dem R-SDK können Sie die Programmiersprache R mit Azure Machine Learning verwenden. Das SDK verwendet das Paket reticulate zur Bindung an das Python-SDK von Azure Machine Learning. Dadurch erhalten Sie Zugriff auf die im Python-SDK implementierten Kernobjekte und -methoden aus jeder R-Umgebung.

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Tutorial: Erstellen eines Modells für logistische Regression](tutorial-1st-r-experiment.md)
* [Azure Machine Learning SDK für R – Referenz](https://azure.github.io/azureml-sdk-for-r/index.html)

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning-Designer

Der Designer ermöglicht es Ihnen, Modelle über eine Drag & Drop-Schnittstelle in Ihrem Webbrowser zu trainieren.

+ [Was ist der Designer?](concept-designer.md)
+ [Tutorial: Prognostizieren von Fahrzeugpreisen](tutorial-designer-automobile-price-train-score.md)
+ [Regression: Preisprognose](how-to-designer-sample-regression-automobile-price-basic.md)
+ [Klassifizierung: Einkommensprognose](how-to-designer-sample-classification-predict-income.md)
+ [Klassifizierung: Vorhersage von Kundenabwanderung, Kauflust und Up-Selling](how-to-designer-sample-classification-churn.md)
+ [Klassifizierung mit benutzerdefiniertem R-Skript: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
+ [Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)

## <a name="many-models-solution-accelerator"></a>Many Models Solution Accelerator (Projektmappenbeschleuniger für viele Modelle)

Der [Many Models Solution Accelerator](https://aka.ms/many-models) (Preview) (Projektmappenbeschleuniger für viele Modelle (Vorschau)) baut auf Azure Machine Learning auf und ermöglicht Ihnen Training, Betrieb und Verwaltung von hunderten oder sogar tausenden von Machine Learning-Modellen.

Die Erstellung eines Modells __für jede Instanz oder jede Einzelperson__ kann in den folgenden Szenarien z. B. zu verbesserten Ergebnissen führen:

* Vorhersage der Umsätze für jedes einzelne Geschäft
* Vorausschauende Wartung für Hunderte von Ölquellen
* Anpassen einer Erfahrung für einzelne Benutzer

Weitere Informationen finden Sie auf GitHub unter [Many Models Solution Accelerator](https://aka.ms/many-models) (Projektmappenbeschleuniger für viele Modelle).

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Die Machine Learning-Befehlszeilenschnittstelle ist eine Erweiterung für die Azure-Befehlszeilenschnittstelle. Es bietet plattformübergreifende CLI-Befehle für die Arbeit mit Azure Machine Learning. In der Regel verwenden Sie die Befehlszeilenschnittstelle zur Automatisierung von Aufgaben, z. B. das Training eines Machine Learning-Modells.

* [Verwenden der CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md)
* [MLOps unter Azure](https://github.com/microsoft/MLOps)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Einrichten von Trainingsumgebungen](how-to-set-up-training-targets.md).
