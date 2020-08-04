---
title: 'Erstellen eines Kubernetes-Entwicklungsbereichs: Visual Studio Code und .NET Core'
services: azure-dev-spaces
ms.date: 09/26/2018
ms.topic: tutorial
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Dev Spaces und Visual Studio Code eine .NET Core-Anwendung in Azure Kubernetes Service debuggen und schnell durchlaufen.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.openlocfilehash: 9c73c191054c9eee183a762d0a029d6c8dc431ee
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87013639"
---
# <a name="create-a-kubernetes-dev-space-visual-studio-code-and-net-core-with-azure-dev-spaces"></a>Erstellen eines Kubernetes-Entwicklungsbereichs: Visual Studio Code und .NET Core mit Azure Dev Spaces

In diesem Leitfaden lernen Sie Folgendes:

- Erstellen einer für die Entwicklung optimierten, Kubernetes-basierten Umgebung in Azure (ein _Entwicklungsbereich_)
- Iteratives Entwickeln von Code in Containern mit VS Code und der Befehlszeile
- Produktives Entwickeln und Testen Ihres Codes in einer Teamumgebung

> [!NOTE]
> **Sollten Sie einmal nicht weiterkommen**, lesen Sie den Abschnitt [Problembehandlung](troubleshooting.md).

## <a name="install-the-azure-cli"></a>Installieren der Azure CLI
Bei Azure Dev Spaces ist der Einrichtungsaufwand für die lokalen Computer minimal. Der Großteil der Konfiguration Ihres Entwicklungsbereichs wird in der Cloud gespeichert und kann gemeinsam mit anderen Benutzern genutzt werden. Laden Sie zunächst die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli?view=azure-cli-latest) (Command-Line Interface, CLI) herunter, und führen Sie sie aus.

### <a name="sign-in-to-azure-cli"></a>Anmelden bei der Azure-Befehlszeilenschnittstelle
Melden Sie sich bei Azure an. Geben Sie in einem Terminalfenster den folgenden Befehl ein:

```azurecli
az login
```

> [!NOTE]
> Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Falls Sie über mehrere Azure-Abonnements verfügen...
Führen Sie zum Anzeigen Ihrer Abonnements Folgendes aus: 

```azurecli
az account list --output table
```

Suchen Sie nach dem Abonnement, bei dem *True* für *IsDefault* angegeben ist.
Handelt es sich dabei nicht um das Abonnement, das Sie verwenden möchten, können Sie das Standardabonnement wie folgt ändern:

```azurecli
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Erstellen eines Kubernetes-Clusters mit Aktivierung für Azure Dev Spaces

Erstellen Sie an der Eingabeaufforderung die Ressourcengruppe in einer [Region, die Azure Dev Spaces unterstützt][supported-regions].

```azurecli
az group create --name MyResourceGroup --location <region>
```

Erstellen Sie mit dem folgenden Befehl einen Kubernetes-Cluster:

```azurecli
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Die Erstellung des Clusters dauert einige Minuten.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Konfigurieren des AKS-Clusters zur Verwendung von Azure Dev Spaces

Geben Sie den folgenden Azure CLI-Befehl ein. Verwenden Sie dabei die Ressourcengruppe, die Ihren AKS-Cluster enthält, und den Namen des AKS-Clusters. Der Befehl konfiguriert Ihren Cluster mit Unterstützung für Azure Dev Spaces.

   ```azurecli
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
   
> [!IMPORTANT]
> Bei der Azure Dev Spaces-Konfiguration wird der Namespace `azds` im Cluster entfernt, sofern er vorhanden ist.

## <a name="get-kubernetes-debugging-for-vs-code"></a>Erhalten von Kubernetes-Debugging für VS Code
VS Code bietet .NET Core- und Node.js-Entwicklern umfangreiche Features wie etwa Kubernetes-Debugging.

1. Installieren Sie [VS Code](https://code.visualstudio.com/Download), falls Sie noch nicht darüber verfügen.
1. Laden Sie die [VS Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)- und [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)-Erweiterung herunter, und installieren Sie sie. Klicken Sie auf der Marketplace-Seite der Erweiterung für jede Erweiterung auf „Installieren“. Wiederholen Sie diesen Vorgang in VS Code.

## <a name="create-a-web-app-running-in-a-container"></a>Erstellen einer Web-App, die in einem Container ausgeführt wird

In diesem Abschnitt erstellen Sie eine ASP.NET Core-Web-App und bereiten sie für die Ausführung in einem Container in Kubernetes vor.

### <a name="create-an-aspnet-core-web-app"></a>Erstellen einer ASP.NET Core-Web-App
Klonen Sie die [Azure Dev Spaces-Beispielanwendung](https://github.com/Azure/dev-spaces), oder laden Sie sie herunter. In diesem Artikel wird Code im Verzeichnis *samples/dotnetcore/getting-started/webfrontend* verwendet.

## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Vorbereiten von Code für die Entwicklung mit Docker und Kubernetes
Bisher verfügen Sie über eine einfache Web-App, die lokal ausgeführt werden kann. Jetzt werden Sie die App containerisieren, indem Sie Ressourcen erstellen, die den Container und die Bereitstellung der App für Kubernetes definieren. Diese Aufgabe kann ganz einfach mit Azure Dev Spaces ausgeführt werden: 

1. Starten Sie Visual Studio Code (VS Code), und öffnen Sie den Ordner `webfrontend`. (Sie können alle Standardaufforderungen zum Hinzufügen von Debugressourcen oder zum Wiederherstellen des Projekts ignorieren.)
1. Öffnen Sie in VS Code das integrierte Terminal (über das Menü **Ansicht > Integriertes Terminal**).
1. Führen Sie den folgenden Befehl aus (stellen Sie dabei sicher, dass **webfrontend** der aktuelle Ordner ist):

    ```cmd
    azds prep --enable-ingress
    ```

Mit dem Azure CLI-Befehl `azds prep` werden Docker- und Kubernetes-Ressourcen mit Standardeinstellungen generiert:
* `./Dockerfile` beschreibt das Containerimage der App und wie der Quellcode erstellt und im Container ausgeführt wird.
* In einem [Helm-Diagramm](https://docs.helm.sh) unter `./charts/webfrontend` wird veranschaulicht, wie der Container für Kubernetes bereitgestellt wird.

> [!TIP]
> Azure Dev Spaces nutzt das [Dockerfile und das Helm-Diagramm](how-dev-spaces-works-prep.md#prepare-your-code) zum Erstellen und Ausführen Ihres Codes. Sie können sie jedoch ändern, wenn Sie anpassen möchten, wie das Projekt erstellt und ausgeführt wird.

Vorerst ist es nicht notwendig, den gesamten Inhalt dieser Dateien zu verstehen. Es ist jedoch erwähnenswert, dass **dieselben Konfiguration-als-Code-Ressourcen von Kubernetes und Docker von der Entwicklung bis zur Produktion verwendet werden können, wodurch eine bessere Konsistenz in verschiedenen Umgebungen erreicht wird.**
 
Außerdem wird mit dem `prep`-Befehl eine Datei namens `./azds.yaml` generiert. Das ist die Konfigurationsdatei für Azure Dev Spaces. Sie ergänzt die Docker- und Kubernetes-Artefakte durch eine zusätzliche Konfiguration, die eine iterative Entwicklungserfahrung in Azure ermöglicht.

## <a name="build-and-run-code-in-kubernetes"></a>Erstellen und Ausführen von Code in Kubernetes
Führen Sie Ihren Code jetzt aus! Führen Sie im Terminalfenster diesen Befehl aus dem **Codestammordner** des Web-Front-Ends aus:

```cmd
azds up
```

Achten Sie auf die Ausgabe des Befehls, Sie werden während der Ausführung verschiedene Punkte bemerken:
- Quellcode wird mit dem Entwicklungsbereich in Azure synchronisiert.
- In Azure wird ein Containerimage erstellt, wie von den Docker-Objekten in Ihrem Codeordner angegeben.
- Es werden Kubernetes-Objekte erstellt, die das Containerimage verwenden, wie vom Helm-Diagramm in Ihrem Codeordner angegeben.
- Es werden Informationen zu den Endpunkten des Containers angezeigt. In unserem Fall erwarten wir eine öffentliche HTTP-URL.
- Wenn die oben genannten Phasen erfolgreich abgeschlossen sind, sollten Sie die Ausgabe von `stdout` (und `stderr`) sehen, wenn der Container startet.

> [!NOTE]
> Bei der ersten Ausführung von `up` dauern diese Schritte etwas länger, spätere Ausführungen erfolgen dann aber schneller.

### <a name="test-the-web-app"></a>Testen der Web-App
Prüfen Sie, ob die Konsolenausgabe *Anwendung gestartet* meldet, und vergewissern Sie sich, dass der Befehl `up` abgeschlossen ist:

```
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Microsoft (R) Build Engine version 15.9.20+g88f5fadfbe for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.dll
  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.Views.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:00.94
[...]
webfrontend-5798f9dc44-99fsd: Now listening on: http://[::]:80
webfrontend-5798f9dc44-99fsd: Application started. Press Ctrl+C to shut down.
```

Identifizieren Sie die öffentliche URL für den Dienst in der Ausgabe des Befehls `up`. Sie endet mit `.azds.io`. Im obigen Beispiel lautet die öffentliche URL `http://webfrontend.1234567890abcdef1234.eus.azds.io/`.

Um Ihre Web-App anzuzeigen, öffnen Sie die öffentliche URL in einem Browser. Beachten Sie auch, dass die Ausgabe von `stdout` und `stderr` an das Terminalfenster *azds trace* gestreamt wird, während Sie mit Ihrer Web-App interagieren. Sie sehen auch Nachverfolgungsinformationen für HTTP-Anforderungen, während sie das System durchlaufen. Dies erleichtert Ihnen während der Entwicklung die Nachverfolgung komplexer, sich auf mehrere Dienste beziehender Aufrufe. Die von Azure Dev Spaces hinzugefügte Instrumentierung stellt diese Anforderungsverfolgung zur Verfügung.

![Terminalfenster „azds trace“](media/get-started-netcore/azds-trace.png)

> [!NOTE]
> Zusätzlich zur öffentlichen URL können Sie die alternative URL `http://localhost:<portnumber>` verwenden, die in der Konsolenausgabe angezeigt wird. Bei Verwendung der localhost-URL kann es so aussehen, als ob der Container lokal ausgeführt wird, während er stattdessen unter AKS ausgeführt wird. Azure Dev Spaces nutzt die Kubernetes-Funktionalität *Portweiterleitung*, um den Port von „localhost“ dem Container zuzuordnen, der in AKS ausgeführt wird. Dies erleichtert die Interaktion mit dem Dienst auf dem lokalen Computer.

### <a name="update-a-content-file"></a>Aktualisieren einer Inhaltsdatei
Bei Azure Dev Spaces geht es nicht nur um die Ausführung von Code in Kubernetes: Mit diesem Dienst sollen Codeänderungen in einer Kubernetes-Umgebung in der Cloud schnell und iterativ sichtbar gemacht werden.

1. Navigieren Sie zur Datei `./Views/Home/Index.cshtml`, und ändern Sie die HTML. Beispiel: Ändern Sie [Zeile 73 mit dem Inhalt `<h2>Application uses</h2>`](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Views/Home/Index.cshtml#L73) wie folgt: 

    ```html
    <h2>Hello k8s in Azure!</h2>
    ```

1. Speichern Sie die Datei . Kurz darauf wird im Terminalfenster eine Nachricht mit dem Hinweis angezeigt, dass eine Datei im ausgeführten Container aktualisiert wurde.
1. Aktualisieren Sie die Anzeige im Browser. Daraufhin sollte auf der Webseite die aktualisierte HTML angezeigt werden.

Was ist passiert? Für Änderungen an Inhaltsdateien (etwa HTML und CSS) ist keine erneute Kompilierung in einer .NET Core-Web-App erforderlich. Ein aktiver `azds up`-Befehl synchronisiert daher automatisch alle geänderten Inhaltsdateien im ausgeführten Container in Azure, sodass alle Inhaltsänderungen direkt angezeigt werden.

### <a name="update-a-code-file"></a>Aktualisieren einer Codedatei
Die Aktualisierung von Codedateien ist etwas aufwendiger, da eine .NET Core-App aktualisierte Anwendungsbinärdateien neu erstellen und generieren muss.

1. Drücken Sie im Terminalfenster `Ctrl+C` (zum Beenden von `azds up`).
1. Öffnen Sie die Codedatei mit dem Namen `Controllers/HomeController.cs`, und bearbeiten Sie die Nachricht, die auf der Seite „Info“ angezeigt wird: `ViewData["Message"] = "Your application description page.";`.
1. Speichern Sie die Datei .
1. Führen Sie `azds up` im Terminalfenster aus. 

Mit diesem Befehl wird das Containerimage neu erstellt und das Helm-Diagramm erneut bereitgestellt. Navigieren Sie in der Web-App zum Menü „Info“, um zu sehen, wie sich Ihre Codeänderungen in der ausgeführten Anwendung auswirken.

Es gibt jedoch eine noch *schnellere Methode* für die Codeentwicklung. Sie wird im nächsten Abschnitt erläutert. 

## <a name="debug-a-container-in-kubernetes"></a>Debuggen eines Containers in Kubernetes

In diesem Abschnitt verwenden Sie VS Code zum direkten Debuggen des in Azure ausgeführten Containers. Außerdem erfahren Sie, wie Sie die Schleife zum Bearbeiten/Ausführen/Testen beschleunigen.

![Das Diagramm zeigt eine Entwicklungsschleife mit drei Phasen: Code bearbeiten, Container aktualisieren und Update anzeigen.](media/common/edit-refresh-see.png)

> [!NOTE]
> **Sollten Sie einmal nicht weiterkommen**, lesen Sie den Abschnitt [Problembehandlung](troubleshooting.md), oder hinterlassen Sie einen Kommentar auf dieser Seite.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Initialisieren von Debugressourcen mit der VS Code-Erweiterung
Sie müssen zunächst Ihr Codeprojekt konfigurieren, damit VS Code mit dem Entwicklungsbereich in Azure kommunizieren kann. Die VS Code-Erweiterung für Azure Dev Spaces enthält einen Befehl zum Einrichten der Debugkonfiguration. 

Öffnen Sie die **Befehlspalette** (über das Menü **Ansicht | Befehlspalette**), verwenden Sie die automatische Vervollständigung für die Eingabe, und wählen Sie den Befehl `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces` aus. 

Dadurch wird die Debugkonfiguration für Azure Dev Spaces unter dem `.vscode`-Ordner hinzugefügt. Dieser Befehl darf nicht mit dem Befehl `azds prep` verwechselt werden, der zum Konfigurieren des Projekts für die Bereitstellung dient.

![Screenshot: Auswählen des Befehls „Azure Dev Spaces: Vorbereiten von Konfigurationsdateien für Azure Dev Spaces“ im Fenster mit der Befehlspalette](media/common/command-palette.png)


### <a name="select-the-azds-debug-configuration"></a>Auswählen der AZDS-Debugkonfiguration
1. Klicken Sie zum Öffnen der Debugansicht auf der **Aktivitätsleiste** am Rand von VS Codeauf das Symbol „Debuggen“.
1. Wählen Sie als aktive Debugkonfiguration **.NET Core Launch (AZDS)** (.NET Core-Start (AZDS)) aus.

![Der Screenshot zeigt die obere linke Ecke des Visual Studio Code-Fensters. Das Symbol „Debuggen“ ist hervorgehoben, der linke Bereich hat den Titel „DEBUGGEN“, und in der Dropdownliste rechts vom Titel wird „.NET Core Launch (AZDS)“ (.NET Core-Start (AZDS)) angezeigt.](media/get-started-netcore/debug-configuration.png)

> [!NOTE]
> Falls in der Befehlspalette keine Azure Dev Spaces-Befehle angezeigt werden, überprüfen Sie, ob die VS Code-Erweiterung für Azure Dev Spaces installiert wurde. Achten Sie darauf, dass es sich bei dem in VS Code geöffneten Arbeitsbereich um den Ordner handelt, der „azds.yaml“ enthält.


### <a name="debug-the-container-in-kubernetes"></a>Debuggen des Containers in Kubernetes
Drücken Sie **F5**, um Ihren Code in Kubernetes zu debuggen.

Wie beim Befehl `up` wird der Code mit dem Entwicklungsbereich synchronisiert, und ein Container wird erstellt und in Kubernetes bereitgestellt. Dieses Mal ist der Debugger natürlich an den Remotecontainer angefügt.

> [!TIP]
> Die Visual Studio Code-Statusleiste ändert sich in orange, was bedeutet, dass der Debugger angefügt ist. Sie zeigt auch eine klickbare URL an, die Sie verwenden können, um Ihre Website zu öffnen.

![Screenshot: Unterer Bereich des Visual Studio Code-Fensters. Die orangefarbene Statusleiste ist die letzte Zeile. Sie enthält eine URL zum Öffnen der Website.](media/common/vscode-status-bar-url.png)

Legen Sie einen Breakpoint in einer serverseitigen Codedatei fest, beispielsweise in der `About()`-Funktion in der Quelldatei `Controllers/HomeController.cs`. Durch die Aktualisierung der Browserseite wird der Breakpoint erreicht.

Sie besitzen wie bei der lokalen Ausführung des Codes Vollzugriff auf Debuginformationen, etwa Aufrufliste, lokale Variablen, Ausnahmeinformationen usw.

### <a name="edit-code-and-refresh"></a>Bearbeiten des Codes und Aktualisieren
Nehmen Sie bei aktivem Debugger eine Codeänderung vor. Ändern Sie beispielsweise die Meldung der Seite „Info“ unter `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Speichern Sie die Datei, und klicken Sie im Bereich **Debugaktionen** auf die Schaltfläche **Neu starten**. 

![Der Bereich „Debugaktionen“ ist ein kleiner Bereich oben in der Mitte der Seite (direkt unterhalb des Seitentitels). Die Schaltfläche „Neu starten“ (ein kreisförmiger Pfeil) ist hervorgehoben. Das Hoverbild für die Schaltfläche ist „Neu starten (STRG+UMSCHALT+F5)“.](media/common/debug-action-refresh.png)

Das Neuerstellen und erneute Bereitstellen eines neuen Containerimages bei jeder vorgenommenen Codeänderung kann geraume Zeit in Anspruch nehmen. Daher kompiliert Azure Dev Spaces Code im vorhandenen Container inkrementell, um den Bearbeitungs-/Debugging-Kreislauf zu beschleunigen.

Aktualisieren Sie die Web-App im Browser, und wechseln Sie zur Seite „Info“. Daraufhin sollte Ihre benutzerdefinierte Meldung auf der Benutzeroberfläche angezeigt werden.

**Sie kennen jetzt eine Methode für die schnelle Codeiteration und das direkte Debuggen in Kubernetes!** Als Nächstes erfahren Sie, wie Sie einen zweiten Container erstellen und aufrufen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zur Entwicklung mit mehreren Diensten](multi-service-netcore.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
