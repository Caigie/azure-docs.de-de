---
title: Exportieren eines Jupyter Notebook-Projekts aus Azure Notebooks (Vorschauversion)
description: Hier wird beschrieben, wie Sie schnell ein Jupyter Notebook-Projekt exportieren.
ms.topic: quickstart
ms.date: 06/29/2020
ms.openlocfilehash: a2e76f2c1956df4d0de59cfb429dc1c9dce85a62
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88892098"
---
# <a name="quickstart-export-a-jupyter-notebook-project-in-azure-notebooks-preview"></a>Schnellstart: Exportieren eines Jupyter Notebook-Projekts in Azure Notebooks (Vorschauversion)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

In dieser Schnellstartanleitung laden Sie ein Azure Notebooks-Projekt zur Verwendung in anderen Jupyter Notebook-Lösungen herunter. 

## <a name="prerequisites"></a>Voraussetzungen

Ein vorhandenes Azure Notebooks-Projekt.

## <a name="export-an-azure-notebooks-project"></a>Exportieren eines Azure Notebooks-Projekts

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com), und melden Sie sich an Ausführliche Informationen finden Sie unter [Schnellstart: Anmelden bei Azure Notebooks](quickstart-sign-in-azure-notebooks.md).

1. Wählen Sie oben auf Ihrer öffentlichen Profilseite **Meine Projekte** aus:

    ![Link „Meine Projekte“ am oberen Rand des Browserfensters](media/quickstarts/my-projects-link.png)

1. Wählen Sie ein Projekt aus.
1. Klicken Sie auf die Schaltfläche „Download“, um das Herunterladen einer ZIP-Datei auszulösen, in der Ihre gesamten Projektdateien enthalten sind.
1. Klicken Sie alternativ auf einer Projektseite auf die Schaltfläche „Projekt herunterladen“, um alle Dateien eines bestimmten Projekts herunterzuladen.

Nachdem Sie Ihre Projektdateien heruntergeladen haben, können Sie sie mit anderen Jupyter Notebook-Lösungen nutzen. Einige der in den folgenden Abschnitten beschriebenen Optionen umfassen Folgendes: 
- [Visual Studio Code](#use-notebooks-in-visual-studio-code)
- [Visual Studio Codespaces](#use-notebooks-in-visual-studio-codespaces)
- [Azure Machine Learning](#use-notebooks-with-azure-machine-learning)
- [Azure Lab Services](#use-azure-lab-services)
- [GitHub](#use-github)

## <a name="create-an-environment-for-notebooks"></a>Erstellen einer Umgebung für Notebooks

Wenn Sie eine Umgebung erstellen möchten, die der Umgebung von Azure Notebooks (Vorschauversion) entspricht, können Sie die auf GitHub bereitgestellte Skriptdatei verwenden.

1. Navigieren Sie zum [GitHub-Repository](https://github.com/microsoft/AzureNotebooks) von Azure Notebooks, oder [greifen Sie direkt auf den Umgebungsordner zu](https://aka.ms/aznbrequirementstxt).
1. Navigieren Sie an einer Eingabeaufforderung zu dem Verzeichnis, das Sie für Ihre Projekte verwenden möchten.
1. Laden Sie den Inhalt des Umgebungsordners herunter, und befolgen Sie die Anleitung in der Infodatei, um die Azure Notebooks-Paketabhängigkeiten zu installieren.


## <a name="use-notebooks-in-visual-studio-code"></a>Verwenden von Notebooks in Visual Studio Code

[VS Code](https://code.visualstudio.com/) ist ein kostenloser Code-Editor, den Sie lokal oder über eine Remote-Computeumgebung nutzen können. In Kombination mit der Python-Erweiterung ergibt sich eine vollständige Umgebung für die Python-Entwicklung, einschließlich einer umfassenden nativen Benutzeroberfläche für die Verwendung von Jupyter Notebook-Instanzen. 

![Jupyter Notebook-Unterstützung für VS Code](media/vs-code-jupyter-notebook.png)

Nach dem [Herunterladen](#export-an-azure-notebooks-project) Ihrer Projektdateien können Sie sie mit VS Code verwenden. Eine Anleitung zur Nutzung von VS Code mit Jupyter Notebooks finden Sie in den Tutorials unter [Verwenden von Jupyter Notebooks in Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support) und [Data Science in Visual Studio Code](https://code.visualstudio.com/docs/python/data-science-tutorial).

Sie können auch das [Azure Notebooks-Umgebungsskript](#create-an-environment-for-notebooks) mit Visual Studio Code verwenden, um eine Umgebung zu erstellen, die Azure Notebooks (Vorschauversion) entspricht.

## <a name="use-notebooks-in-visual-studio-codespaces"></a>Verwenden von Notebooks in Visual Studio Codespaces

Visual Studio Codespaces verfügt über in der Cloud gehostete Umgebungen, in denen Sie Ihre Notebooks mit Visual Studio Code oder Ihrem Webbrowser bearbeiten können. Sie erhalten die gleiche leistungsfähige Jupyter-Umgebung wie in VS Code, ohne dass Sie Installationen auf Ihrem Gerät durchführen müssen. Wenn Sie keine lokale Umgebung einrichten möchten und eine cloudbasierte Lösung bevorzugen, ist die Erstellung eines Codespace eine gute Option. Erste Schritte:

1. Führen Sie den [Download](#export-an-azure-notebooks-project) Ihrer Projektdateien durch.
1. [Erstellen Sie ein GitHub-Repository](https://help.github.com/github/getting-started-with-github/create-a-repo) zum Speichern Ihrer Notebooks. 
1. [Fügen Sie Ihre Dateien dem Repository hinzu](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository).
1. Richten Sie Visual Studio Codespaces über den [Browser](https://docs.microsoft.com/visualstudio/online/how-to/browser), [Visual Studio](https://docs.microsoft.com/visualstudio/online/how-to/vside) oder [Visual Studio Code](https://docs.microsoft.com/visualstudio/online/how-to/vscode) ein.

## <a name="use-notebooks-with-azure-machine-learning"></a>Verwenden von Notebooks mit Azure Machine Learning

Azure Machine Learning verfügt über eine End-to-End-Plattform für maschinelles Lernen, die es Benutzern ermöglicht, Modelle in Azure schneller zu erstellen und bereitzustellen. Azure ML ermöglicht die Ausführung von Jupyter Notebooks auf einer VM oder in einer Computingumgebung mit freigegebenem Cluster. Falls Sie für Ihre ML-Workload eine cloudbasierte Lösung mit Experimentnachverfolgung, Datasetverwaltung und weiteren Features benötigen, empfehlen wir Ihnen Azure Machine Learning. Erste Schritte mit Azure ML:

1. Führen Sie den [Download](#export-an-azure-notebooks-project) Ihrer Projektdateien durch.
1. [Erstellen Sie im Azure-Portal einen Arbeitsbereich](../machine-learning/how-to-manage-workspace.md).

   ![Erstellen eines Arbeitsbereichs](../machine-learning/media/how-to-manage-workspace/create-workspace.gif)
 
1. Öffnen Sie [Azure Studio (Vorschauversion)](https://ml.azure.com/).
1. Wählen Sie in der linken Navigationsleiste die Option **Notebooks** aus.
1. Klicken Sie auf die Schaltfläche **Dateien hochladen**, und laden Sie die Projektdateien hoch, die Sie aus Azure Notebooks heruntergeladen haben.

Weitere Informationen zu Azure ML und zum Ausführen von Jupyter Notebooks finden Sie in der [Dokumentation](../machine-learning/how-to-run-jupyter-notebooks.md). Sie können auch das Modul [Einführung in Azure Machine Learning](https://docs.microsoft.com/learn/modules/intro-to-azure-machine-learning-service/) bei Microsoft Learn verwenden.


## <a name="use-azure-lab-services"></a>Verwenden von Azure Lab Services

Mit [Azure Lab Services](https://azure.microsoft.com/services/lab-services/) können Lehrkräfte den bedarfsgesteuerten Zugriff auf vorkonfigurierte VMs für eine ganze Klasse leicht einrichten und bereitstellen. Wenn Sie nach einer Möglichkeit suchen, Jupyter Notebooks und Cloud Computing in einer maßgeschneiderten Klassenumgebung zu nutzen, ist Lab Services eine gute Option.

![image](../lab-services/media/tutorial-setup-classroom-lab/new-lab-button.png)

 Nach dem [Herunterladen](#export-an-azure-notebooks-project) Ihrer Projektdateien können Sie sie mit Azure Lab Services verwenden. Eine Anleitung zum Einrichten eines Labs finden Sie unter [Einrichten eines Labs zum Vermitteln von Data Science mit Python und Jupyter Notebooks](../lab-services/class-type-jupyter-notebook.md).

## <a name="use-github"></a>Verwenden von GitHub

Mit GitHub können Sie kostenlos und mit Quellcodeverwaltung Notebooks (und andere Dateien) speichern, für andere Benutzer freigeben und zusammen bearbeiten. Wenn Sie nach einer Möglichkeit suchen, Ihre Projekte gemeinsam zu nutzen und mit anderen Benutzern zusammenzuarbeiten, ist GitHub eine gute Option. Darüber hinaus ist auch die Kombination mit [Visual Studio Codespaces](#use-notebooks-in-visual-studio-codespaces) möglich, um eine leistungsfähige Entwicklungsumgebung zu schaffen. Erste Schritte mit GitHub

1. Führen Sie den [Download](#export-an-azure-notebooks-project) Ihrer Projektdateien durch.
1. [Erstellen Sie ein GitHub-Repository](https://help.github.com/github/getting-started-with-github/create-a-repo) zum Speichern Ihrer Notebooks. 
1. [Fügen Sie Ihre Dateien dem Repository hinzu](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository).

## <a name="next-steps"></a>Nächste Schritte

- [Python in Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)
- [Azure Machine Learning und Jupyter Notebooks](../machine-learning/how-to-run-jupyter-notebooks.md)
- [Visual Studio Codespaces](https://visualstudio.microsoft.com/services/visual-studio-codespaces/)
- [Azure Lab Services](https://azure.microsoft.com/services/lab-services/)
- [GitHub](https://help.github.com/github/getting-started-with-github/)
