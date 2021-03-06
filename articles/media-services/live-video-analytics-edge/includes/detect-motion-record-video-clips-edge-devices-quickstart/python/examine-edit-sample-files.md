---
ms.openlocfilehash: ce5611a92e9899d64ff2117385af008c37c22c5e
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682169"
---
Bei der Erfüllung der Voraussetzungen für diese Schnellstartanleitung haben Sie den Beispielcode in einen Ordner heruntergeladen. Führen Sie die unten angegebenen Schritte aus, um den Beispielcode zu überprüfen und zu bearbeiten.

1. Navigieren Sie in Visual Studio Code zu *src/edge*. Ihre *ENV*-Datei und einige Bereitstellungsvorlagendateien werden angezeigt.

    Die Bereitstellungsvorlage verweist auf das Bereitstellungsmanifest für das Edgegerät, wobei für einige Eigenschaften Variablen verwendet werden. Die *ENV*-Datei enthält die Werte für diese Variablen.
1. Navigieren Sie zum Ordner *src/cloud-to-device-console-app*. Darin werden die Datei *appsettings.json* und einige andere Dateien angezeigt:
    * ***operations.json***: Die Liste mit den Vorgängen, die vom Programm ausgeführt werden sollen.
    * ***main.py***: Der Beispielprogrammcode. Mit diesem Code wird Folgendes durchgeführt:

        * Laden der App-Einstellungen
        * Aufrufen direkter Methoden, die vom Modul „Live Video Analytics in IoT Edge“ verfügbar gemacht werden. Sie können das Modul zum Analysieren von Livevideostreams verwenden, indem Sie die zugehörigen [direkten Methoden](../../../direct-methods.md) aufrufen. 
        * Anhalten der Ausführung, damit Sie die Ausgabe des Programms im **Terminalfenster** und die vom Modul generierten Ereignisse im **Ausgabefenster** untersuchen können
        * Aufrufen direkter Methoden zur Bereinigung der Ressourcen

1. Bearbeiten Sie die Datei *operations.json*:
    * Ändern Sie den Link zur Graphtopologie:

        `"topologyUrl" : "https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/evr-motion-files/topology.json"`
    * Bearbeiten Sie unter `GraphInstanceSet` den Namen der Graphtopologie, damit er mit dem Wert im vorherigen Link übereinstimmt:
    
      `"topologyName" : "EVRToFilesOnMotionDetection"`
    * Bearbeiten Sie außerdem die RTSP-URL so, dass sie auf die Videodatei verweist:

        `"value": "rtsp://rtspsim:554/media/lots_015.mkv"`
    * Ändern Sie unter `GraphTopologyDelete` den Namen:

        `"name": "EVRToFilesOnMotionDetection"`
