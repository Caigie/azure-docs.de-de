---
title: Bereitstellen einer Web-App zur räumlichen Analyse
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die räumliche Analyse in einer Webanwendung verwenden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/10/2020
ms.author: aahi
ms.openlocfilehash: 440f901f06e431c371b7445f4a04499c475c9aa1
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931765"
---
# <a name="how-to-deploy-a-people-counting-web-application"></a>Gewusst wie: Bereitstellen einer Webanwendung für die Erfassung der Personenanzahl

In diesem Artikel erfahren Sie, wie Sie räumliche Analysen in eine Web-App integrieren, die die Bewegung von Personen versteht und die Anzahl der Personen überwacht, die einen physischen Raum einnehmen. 

In diesem Lernprogramm lernen Sie Folgendes:

* Bereitstellen des Containers für die räumliche Analyse
* Konfigurieren des Vorgangs und der Kamera
* Konfigurieren der IoT Hub-Verbindung in der Webanwendung
* Bereitstellen und Testen der Webanwendung

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Grundlegende Kenntnisse der Bereitstellungskonfigurationen von Azure IoT Edge und eine Instanz von [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/)
* Ein konfigurierter [Hostcomputer](spatial-analysis-container.md)

## <a name="deploy-the-spatial-analysis-container"></a>Bereitstellen des Containers für die räumliche Analyse

Füllen Sie den [Antrag](https://aka.ms/csgate) aus, um Zugriff zum Ausführen des Containers zu erhalten. 

Befolgen Sie das [Hostcomputersetup](./spatial-analysis-container.md), um den Hostcomputer zu konfigurieren und ein IoT Edge-Gerät mit Azure IoT Hub zu verbinden. 

### <a name="deploy-an-azure-iot-hub-service-in-your-subscription"></a>Bereitstellen eines Azure IoT Hub-Diensts in Ihrem Abonnement

Erstellen Sie zunächst eine Instanz eines Azure IoT Hub-Diensts mit dem Standard-Tarif (S1) oder dem Free-Tarif (S0). Befolgen Sie diese Anweisungen, um diese Instanz mithilfe der Azure-Befehlszeilenschnittstelle zu erstellen.

Füllen Sie die erforderlichen Parameter aus:
* Abonnement: Der Name oder die ID Ihres Azure-Abonnements
* Ressourcengruppe: Legen Sie einen Namen für Ihre Ressourcengruppe fest.
* IoT-Hub-Name: Legen Sie einen Namen für Ihren IoT-Hub fest.
* IoT-Hub-Name: Der Name des erstellten IoT-Hubs 
* Name des Edgegeräts: Legen Sie einen Namen für Ihr Edgegerät fest.

```azurecli
az login
az account set --subscription <name or ID of Azure Subscription>
az group create --name "<Resource Group Name>" --location "WestUS"

az iot hub create --name "<IoT Hub Name>" --sku S1 --resource-group "test-resource-group"

az iot hub device-identity create --hub-name "<IoT Hub Name>" --device-id "<Edge Device Name>" --edge-enabled
```

### <a name="deploy-the-container-on-azure-iot-edge-on-the-host-computer"></a>Bereitstellen des Containers in Azure IoT Edge auf dem Hostcomputer

Stellen Sie den Container für die räumliche Analyse mithilfe der Azure-Befehlszeilenschnittstelle als IoT-Modul auf dem Hostcomputer bereit. Der Bereitstellungsprozess erfordert eine Bereitstellungsmanifestdatei, in der die erforderlichen Container, Variablen und Konfigurationen für Ihre Bereitstellung beschrieben werden. Sie finden ein Beispiel für ein [Bereitstellungsmanifest](https://github.com/Azure-Samples/cognitive-services-rest-api-samples/) auf GitHub. Es enthält eine grundlegende Bereitstellungskonfiguration für den Container *spatial-analysis*. 

> [!NOTE] 
> Die Container *spatial-analysis-telegraf* und *spatial-analysis-diagnostics* sind optional. Sie können diese aus der Datei *DeploymentManifest.json* entfernen. Weitere Informationen finden Sie im Artikel zu [Telemetrie und Problembehandlung](./spatial-analysis-logging.md). Ein Beispiel für die Datei *DeploymentManifest.json* finden Sie auf [GitHub](https://go.microsoft.com/fwlink/?linkid=2142179). 

### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Die meisten **Umgebungsvariablen** für das IoT Edge-Modul sind bereits in der unter dem Link oben angegebenen Beispieldatei *DeploymentManifest.json* festgelegt. Suchen Sie in der Datei nach den Umgebungsvariablen `BILLING_ENDPOINT` und `API_KEY`, wie unten gezeigt. Ersetzen Sie die Werte durch den Endpunkt-URI und den API-Schlüssel, die Sie zuvor erstellt haben. Stellen Sie sicher, dass der EULA-Wert auf „accept“ festgelegt ist. 

```json
"EULA": { 
    "value": "accept"
},

"BILLING_ENDPOINT":{ 
    "value": "<Use a key from your Computer Vision resource>"
},
"API_KEY":{
    "value": "<Use the endpoint from your Computer Vision resource>"
}
```

### <a name="configure-the-operation-parameters"></a>Konfigurieren der Vorgangsparameter

Nachdem die anfängliche Konfiguration des Containers *spatial-analysis* nun fertiggestellt ist, konfigurieren Sie im nächsten Schritt die Vorgangsparameter und fügen sie der Bereitstellung hinzu. 

Im ersten Schritt aktualisieren Sie das [Beispielbereitstellungsmanifest](https://go.microsoft.com/fwlink/?linkid=2142179) und konfigurieren die operationId für `cognitiveservices.vision.spatialanalysis-personcount`, wie unten dargestellt:


```json
"personcount": {
    "operationId": "cognitiveservices.vision.spatialanalysis-personcount",
    "version": 1,
    "enabled": true,
    "parameters": {
        "VIDEO_URL": "<Replace RTSP URL here>",
        "VIDEO_SOURCE_ID": "<Replace with friendly name>",
        "VIDEO_IS_LIVE":true,
        "DETECTOR_NODE_CONFIG": "{ \"gpu_index\": 0 }",
        "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"queue\",\"polygon\":[<Replace with your values>], \"events\": [{\"type\":\"count\"}], \"threshold\":<use 0 for no threshold.}]}"
    }
},
```

Nachdem das [Bereitstellungsmanifest](https://go.microsoft.com/fwlink/?linkid=2142179) aktualisiert wurde, befolgen Sie die Anweisungen des Kameraherstellers, um die Kamera zu installieren und die Kamera-URL, den Benutzernamen und das Kennwort zu konfigurieren. 

Legen Sie als Nächstes `VIDEO_URL` auf die RTSP-URL der Kamera und die Anmeldeinformationen für das Herstellen einer Verbindung mit der Kamera fest.

Wenn das Edgegerät über mehrere GPUs verfügt, wählen Sie die GPU aus, auf der dieser Vorgang ausgeführt werden soll. Nehmen Sie unbedingt einen Lastenausgleich für die Vorgänge vor, sodass nicht mehr als acht Vorgänge gleichzeitig auf einer einzelnen GPU ausgeführt werden.  

Konfigurieren Sie als Nächstes die Zone, in der Sie die Personen zählen möchten. Befolgen Sie zum Konfigurieren des Zonenpolygons zunächst die Anweisungen des Herstellers, um einen Frame von der Kamera abzurufen. Um einen einzelnen Vertex des Polygons zu ermitteln, wählen Sie einen Punkt im Frame aus, nehmen die x-y-Pixelkoordinaten des Punkts relativ zur linken oberen Ecke des Frames und teilen sie durch die entsprechenden Frameabmessungen. Legen Sie die Ergebnisse als x-y-Koordinaten des Vertex fest. Sie können die Konfiguration des Zonenpolygons im Feld `SPACEANALYTICS_CONFIG` festlegen.

Dies ist ein Beispiel für einen Videoframe, in dem gezeigt wird, wie die Vertexkoordinaten für einen Frame der Größe 1.920 × 1.080 berechnet werden.
![Beispielvideoframe](./media/spatial-analysis/sample-video-frame.jpg)

Sie können auch einen Konfidenzschwellenwert auswählen, ab dem erkannte Personen gezählt und Ereignisse generiert werden. Legen Sie den Schwellenwert auf 0 fest, wenn alle Ereignisse ausgegeben werden sollen.

### <a name="execute-the-deployment"></a>Ausführen der Bereitstellung

Nachdem das [Bereitstellungsmanifest](https://go.microsoft.com/fwlink/?linkid=2142179) fertig ist, verwenden Sie diesen Befehl in der Azure-Befehlszeilenschnittstelle, um den Container auf dem Hostcomputer als IoT Edge-Modul bereitzustellen.

```azurecli
az login
az extension add --name azure-iot
az iot edge deployment create --deployment-id "<deployment name>" --hub-name "<IoT Hub name>" --content DeploymentManifest.json --target-condition "deviceId='<IoT Edge device name>'"--subscription "<subscriptionId>"
```

Füllen Sie die erforderlichen Parameter aus:

* Bereitstellungsname: Wählen Sie einen Namen für diese Bereitstellung aus.
* IoT-Hub-Name: Der Name Ihrer Azure IoT Hub-Instanz
* Deployment.json: Der Name Ihrer Bereitstellungsdatei
* Name des IoT Edge-Geräts: Der Name des IoT Edge-Geräts Ihres Hostcomputers
* Abonnement: Abonnement-ID oder -name

Mit diesem Befehl wird die Bereitstellung gestartet, und Sie können den Bereitstellungsstatus in der Azure IoT Hub-Instanz im Azure-Portal anzeigen. Als Status wird möglicherweise *417: Die Bereitstellungskonfiguration des Geräts ist nicht festgelegt* angezeigt, bis das Gerät das Herunterladen der Containerimages abgeschlossen hat und gestartet wird.

### <a name="validate-that-the-deployment-was-successful"></a>Überprüfen der erfolgreichen Bereitstellung

Suchen Sie im Azure-Portal in den Einstellungen des IoT Edge-Moduls für die räumliche Analyse in Ihrer IoT Hub-Instanz nach dem *Laufzeitstatus*. Der **gewünschte Wert** und der **gemeldete Wert** für den *Laufzeitstatus* sollten `Running` lauten. Im Folgenden finden Sie Informationen dazu, wie dies im Azure-Portal dargestellt wird.

![Beispiel für die Bereitstellungsüberprüfung](./media/spatial-analysis/deployment-verification.png)

Zu diesem Zeitpunkt führt der Container für räumliche Analysen den Vorgang aus. Er gibt KI-Erkenntnisse für den Vorgang `cognitiveservices.vision.spatialanalysis-personcount` aus und leitet diese als Telemetriedaten an Ihre Azure IoT Hub-Instanz weiter. Um weitere Kameras zu konfigurieren, können Sie die Datei mit dem [Bereitstellungsmanifest](https://go.microsoft.com/fwlink/?linkid=2142179) aktualisieren und die Bereitstellung erneut ausführen.

## <a name="person-counting-web-application"></a>Webanwendung zur Personenzählung

Mit dieser Webanwendung zur Personenzählung können Sie schnell eine Beispiel-Web-App konfigurieren und in Ihrer Azure-Umgebung hosten.

### <a name="get-the-person-counting-app-container"></a>Abrufen des Containers für die App zur Personenzählung

Eine Containerform dieser App ist in Azure Container Registry verfügbar. Verwenden Sie den folgenden Docker-Pullbefehl, um sie herunterzuladen. Für das Zugriffstoken wenden Sie sich unter projectarchon@microsoft.com an Microsoft.

```bash
docker login rtvsofficial.azurecr.io -u <token name> -p <password>
docker pull rtvsofficial.azurecr.io/acceleratorapp.personcount:1.0
```

Pushen Sie den Container in Ihre Azure Container Registry-Instanz (ACR).

```bash
az acr login --name <your ACR name>

docker tag rtvsofficial.azurecr.io/acceleratorapp.personcount:1.0 [desired local image name]

docker push [desired local image name]
```

Um den Container zu installieren, erstellen Sie eine neue Azure-Web-App für Container, und geben Sie die erforderlichen Parameter an. Wechseln Sie dann zur Registerkarte **Docker**, und wählen Sie **Einzelner Container** und dann **Azure Container Registry** aus. Verwenden Sie Ihre Instanz von Azure Container Registry, in die Sie das Image zuvor gepusht haben.

![Eingeben von Imagedetails](./media/spatial-analysis/solution-app-create-screen.png)

Klicken Sie nach dem Eingeben der oben genannten Parameter auf **Überprüfen + erstellen**, und erstellen Sie die App.

### <a name="configure-the-app"></a>Konfigurieren der App 

Warten Sie, bis das Setup abgeschlossen wurde, und navigieren Sie im Azure-Portal zu Ihrer Ressource. Wechseln Sie zum Abschnitt **Konfiguration**, und fügen Sie die folgenden beiden **Anwendungseinstellungen** hinzu.

* `EventHubConsumerGroup`: der Zeichenfolgenname der Consumergruppe aus Ihrer Azure IoT Hub-Instanz. Sie können eine neue Consumergruppe in Ihrem IoT-Hub erstellen oder die Standardgruppe verwenden. 
* `IotHubConnectionString`: die Verbindungszeichenfolge für Ihre Azure IoT Hub-Instanz. Sie können sie im Abschnitt „Schlüssel“ Ihrer Azure IoT Hub-Ressource unter ![Parameter konfigurieren](./media/spatial-analysis/solution-app-config-page.png) abrufen.

Nachdem diese beiden Einstellungen hinzugefügt wurden, klicken Sie auf **Speichern**. Klicken Sie dann im linken Navigationsmenü auf **Authentifizierung/Autorisierung**, und legen Sie die gewünschte Authentifizierungsebene fest. Es wird empfohlen, Azure Active Directory (Azure AD) Express zu aktivieren. 

### <a name="test-the-app"></a>Testen der App

Wechseln Sie zur Azure-Web-App, und vergewissern Sie sich, dass die Bereitstellung erfolgreich war und die Web-App ausgeführt wird. Navigieren Sie zur konfigurierten URL `<yourapp>.azurewebsites.net`, um die ausgeführte App anzuzeigen.

![Testen der Bereitstellung](./media/spatial-analysis/solution-app-output.png)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren von Vorgängen zur räumlichen Analysen](./spatial-analysis-operations.md)
* [Protokollierung und Problembehandlung](spatial-analysis-logging.md)
* [Leitfaden zur Kameraplatzierung](spatial-analysis-camera-placement.md)
* [Leitfaden zur Platzierung von Zonen und Linien](spatial-analysis-zone-line-placement.md)
