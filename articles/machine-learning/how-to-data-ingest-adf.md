---
title: Datenerfassung mit Azure Data Factory
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine Datenerfassungspipeline mit Azure Data Factory erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 03/01/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: b756e83f1d810007e9e9ef6cf2987c3cf60b7f7d
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852853"
---
# <a name="data-ingestion-with-azure-data-factory"></a>Datenerfassung mit Azure Data Factory

In diesem Artikel erfahren Sie, wie Sie eine Datenerfassungspipeline mit Azure Data Factory (ADF) erstellen. Diese Pipeline wird für die Erfassung von Daten zur Verwendung mit Azure Machine Learning verwendet. Mit Azure Data Factory können Sie auf einfache Weise Daten extrahieren, transformieren und laden (ETL). Nachdem die Daten transformiert und in den Speicher geladen wurden, können sie zum Trainieren Ihrer Machine Learning-Modelle verwendet werden.

Die einfache Datentransformation kann mit nativen ADF-Aktivitäten und Instrumenten wie dem [Datenfluss](https://docs.microsoft.com/azure/data-factory/control-flow-execute-data-flow-activity) durchgeführt werden. Bei komplizierteren Szenarien können die Daten mit etwas benutzerdefiniertem Code verarbeitet werden. Beispiel: Python- oder R-Code.

Es gibt verschiedene gängige Verfahren, um mit Azure Data Factory die Daten während der Erfassung zu transformieren. Jedes Verfahren hat Vor- und Nachteile, die bestimmen, ob es für einen bestimmten Anwendungsfall gut geeignet ist:

| Verfahren | Vorteile | Nachteile |
| ----- | ----- | ----- |
| ADF und Azure Functions | Geringe Wartezeit, serverloses Computing</br>Zustandsbehaftete Funktionen</br>Wiederverwendbare Funktionen | Nur geeignet für die Verarbeitung kurzzeitiger Prozesse |
| ADF und benutzerdefinierte Komponente | Paralleles Computing in großem Umfang</br>Geeignet für komplizierte Algorithmen | Umschließen von Code in einer ausführbaren Datei</br>Komplexität der Verarbeitung von Abhängigkeiten und E/A |
| ADF und Azure Databricks-Notebook | Apache Spark</br>Native Python-Umgebung | Kann teuer sein</br>Das Erstellen von Clustern nimmt zunächst Zeit in Anspruch und erhöht die Wartezeit

## <a name="adf-with-azure-functions"></a>ADF mit Azure Functions

![adf-function](media/how-to-data-ingest-adf/adf-function.png)

Azure Functions ermöglicht das Ausführen kleiner Codeelemente (Funktionen), ohne sich Gedanken über die Anwendungsinfrastruktur machen zu müssen. Bei dieser Option werden die Daten mit benutzerdefiniertem Python-Code verarbeitet, der von einer Azure-Funktion umschlossen ist. 

Die Funktion wird mit der [ADF Azure Function-Aktivität](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity) aufgerufen. Dieser Ansatz ist eine gute Option für leichte Datentransformationen. 

* Vorteile:
    * Die Daten werden auf einem serverlosen Compute mit einer relativ niedrigen Wartezeit verarbeitet
    * Die ADF-Pipeline kann eine [Durable Azure Function](/azure/azure-functions/durable/durable-functions-overview) aufrufen, die einen komplexen Datentransformationsfluss implementieren kann 
    * Die Details der Datentransformation werden durch die Azure-Funktion abstrahiert, die wiederverwendet und von anderen Stellen aus aufgerufen werden kann
* Nachteile:
    * Azure Functions muss vor der Verwendung mit ADF erstellt werden
    * Azure Functions ist nur für die kurzzeitige Datenverarbeitung geeignet

## <a name="adf-with-custom-component-activity"></a>ADF mit benutzerdefinierter Komponentenaktivität

![adf-customcomponent](media/how-to-data-ingest-adf/adf-customcomponent.png)

Bei dieser Option werden die Daten mit benutzerdefiniertem Python-Code verarbeitet, der von einer ausführbaren Datei umschlossen ist. Sie wird mit einer [ADF-Benutzerdefinierte-Komponente-Aktivität](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity) aufgerufen. Dieser Ansatz eignet sich besser für große Datenmengen als das vorherige Verfahren.

* Vorteile:
    * Die Daten werden im [Azure Batch](https://docs.microsoft.com/azure/batch/batch-technical-overview)-Pool verarbeitet, der paralleles und leistungsstarkes Computing in großem Umfang ermöglicht
    * Kann zum Ausführen komplexer Algorithmen und zur Verarbeitung großer Datenmengen verwendet werden
* Nachteile:
    * Der Azure Batch-Pool muss vor der Verwendung mit ADF erstellt werden
    * Übermäßige Entwicklung hinsichtlich der Umschließung von Python-Code in einer ausführbaren Datei. Komplexität der Verarbeitung von Abhängigkeiten und Eingabe-/Ausgabeparametern

## <a name="adf-with-azure-databricks-python-notebook"></a>ADF mit Azure Databricks Python-Notebook

![adf-databricks](media/how-to-data-ingest-adf/adf-databricks.png)

[Azure Databricks](https://azure.microsoft.com/services/databricks/) ist eine Apache Spark-basierte Analyseplattform in der Microsoft-Cloud.

Bei diesem Verfahren wird die Datentransformation von einem [Python-Notebook](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook) ausgeführt, das in einem Azure Databricks-Cluster ausgeführt wird. Dies ist wahrscheinlich der häufigste Ansatz, bei dem die volle Leistung eines Azure Databricks-Diensts genutzt wird. Er ist für die verteilte Datenverarbeitung im großen Stil konzipiert.

* Vorteile:
    * Die Daten werden über den leistungsfähigsten Azure-Dienst für die Datenverarbeitung transformiert, der von der Apache Spark-Umgebung unterstützt wird
    * Native Unterstützung von Python zusammen mit Data Science-Frameworks und Bibliotheken wie TensorFlow, PyTorch und scikit-learn
    * Es ist nicht erforderlich, den Python-Code in Funktionen oder ausführbare Module zu verpacken. Der Code funktioniert so, wie er ist.
* Nachteile:
    * Die Azure Databricks-Infrastruktur muss vor der Verwendung mit ADF erstellt werden
    * Kann je nach Azure Databricks-Konfiguration teuer sein
    * Das Hochfahren von Computeclustern aus dem „kalten“ Modus dauert einige Zeit, was eine hohe Wartezeit für die Lösung bedeutet 
    

## <a name="consuming-data-in-azure-machine-learning-pipelines"></a>In Anspruch nehmen von Daten in Azure Machine Learning-Pipelines

![aml-dataset](media/how-to-data-ingest-adf/aml-dataset.png)

Die transformierten Daten aus der ADF-Pipeline werden in einem Datenspeicher (z. B. Azure Blob) gespeichert. Azure Machine Learning kann über [Datenspeicher](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores) und [Datasets](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) auf diese Daten zugreifen.

Jedes Mal, wenn die ADF-Pipeline ausgeführt wird, werden die Daten an einer anderen Position im Speicher gespeichert. Um den Speicherort an Azure Machine Learning zu übergeben, ruft die ADF-Pipeline eine Azure Machine Learning-Pipeline auf. Beim Aufruf der ML-Pipeline werden der Datenspeicherort und die Ausführungs-ID als Parameter gesendet. Die ML-Pipeline kann dann unter Verwendung des Datenspeicherorts einen Datenspeicher bzw. ein Dataset erstellen. 

> [!TIP]
> Datasets [unterstützen die Versionsverwaltung](https://docs.microsoft.com/azure/machine-learning/how-to-version-track-datasets), sodass die ML-Pipeline eine neue Version des Datasets registrieren kann, die auf die neuesten Daten aus der ADF-Pipeline verweist.

Sobald die Daten über einen Datenspeicher oder ein Dataset zugänglich sind, können Sie sie zum Trainieren eines Machine Learning-Modells verwenden. Der Trainingsprozess könnte Teil derselben ML-Pipeline sein, die von ADF aufgerufen wird. Oder es könnte ein separater Prozess sein, z. B. ein Experiment in einem Jupyter Notebook.

Da Datasets die Versionsverwaltung unterstützen und jede Ausführung der Pipeline eine neue Version erzeugt, ist es leicht zu erkennen, welche Version der Daten zum Trainieren eines Modells verwendet wurde.

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen eines Databricks-Notebooks in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook)
* [Zugreifen auf Daten in Azure Storage-Diensten](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores)
* [Trainieren von Modellen mit Datasets in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/how-to-train-with-datasets)
* [DevOps für eine Datenerfassungspipeline](https://docs.microsoft.com/azure/machine-learning/how-to-cicd-data-ingestion)

