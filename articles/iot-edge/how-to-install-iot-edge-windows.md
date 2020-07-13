---
title: Installieren von Azure IoT Edge unter Windows | Microsoft-Dokumentation
description: Anweisungen zur Azure IoT Edge-Installation unter Windows 10, Windows Server und Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: kgremban
ms.openlocfilehash: ba3e8b9d7649d56d1639f7f608d85a2da04ff74a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465557"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Installieren der Azure IoT Edge-Runtime unter Windows

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen.

Weitere Informationen zur IoT Edge-Runtime finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf Ihrem Windows-x64-System (AMD/Intel) mit Windows-Containern aufgeführt.

> [!NOTE]
> Ein bekanntes Problem des Windows-Betriebssystems verhindert den Übergang in den Energiesparmodus und den Ruhezustand, wenn IoT Edge-Module (vom Prozess isolierte Windows Nano Server-Container) ausgeführt werden. Dieses Problem wirkt sich auf die Akkulaufzeit des Geräts aus.
>
> Geben Sie als Problemumgehung den Befehl `Stop-Service iotedge` ein, um die Ausführung aller IoT Edge-Module zu beenden, bevor Sie diese Energiesparzustände verwenden.

Die Verwendung von Linux-Containern auf Windows-Systemen ist keine empfohlene oder unterstützte Produktionskonfiguration für Azure IoT Edge. Die Container können jedoch zu Entwicklungs- und Testzwecken eingesetzt werden. Weitere Informationen finden Sie unter [Verwenden von IoT Edge unter Windows zum Ausführen von Linux-Containern](how-to-install-iot-edge-windows-with-linux.md).

Informationen zur aktuellen Version von IoT Edge finden Sie auf der Website für die [Releases von IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie anhand dieses Abschnitts, ob Ihr Windows-Gerät IoT Edge unterstützt, und bereiten Sie es vor der Installation für eine Container-Engine vor.

### <a name="supported-windows-versions"></a>Unterstützte Windows-Versionen

IoT Edge für Windows erfordert Windows-Version 1809/Build 17763. Dies ist der neueste [Windows Long Term Support-Build](https://docs.microsoft.com/windows/release-information/). Im Hinblick auf die Unterstützung von Windows-SKUs informieren Sie sich, was unterstützt wird – je nachdem, ob Sie sich für Produktionsszenarien oder Entwicklungs- und Testszenarien vorbereiten:

* **Produktion:** Neueste Informationen dazu, welche Betriebssysteme zurzeit für Produktionsszenarien unterstützt werden, finden Sie unter [Von Azure IoT Edge unterstützte Systeme](support.md#operating-systems).
* **Entwickeln und Testen**: In Entwicklungs- und Testszenarien kann Azure IoT Edge mit Windows-Containern auf jeder SKU (Pro, Enterprise, Server usw.) von Windows-Build 17763 installiert werden, die das Containerfeature unterstützt.

IoT Core-Geräte müssen zur Unterstützung der IoT Edge-Runtime das optionale Feature „IoT Core-Windows-Container“enthalten. Verwenden Sie den folgenden Befehl in einer [PowerShell-Remotesitzung](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell), um zu überprüfen, ob Windows-Container auf Ihrem Gerät unterstützt werden:

```powershell
Get-Service vmcompute
```

Wenn der Dienst vorhanden ist, sollten Sie eine erfolgreiche Antwort mit dem Dienststatus erhalten, der als **running** aufgeführt wird. Wenn der Dienst `vmcompute` nicht gefunden wird, erfüllt Ihr Gerät nicht die Anforderungen für IoT Edge. Wenden Sie sich an Ihren Hardwareanbieter, um Unterstützung für dieses Feature zu erbitten.

### <a name="prepare-for-a-container-engine"></a>Vorbereitungen für eine Container-Engine

Azure IoT Edge basiert auf einer [OCI-kompatiblen](https://www.opencontainers.org/) Container-Engine. Verwenden Sie für Produktionsszenarios die Moby-Engine, die im Installationsskript enthalten ist, um Windows-Container auf Ihren Windows-Geräten auszuführen.

## <a name="install-iot-edge-on-a-new-device"></a>Installieren von IoT Edge auf einem neuen Gerät

>[!NOTE]
>Für Azure IoT Edge-Softwarepakete gelten die in den Paketen enthaltenen Lizenzbedingungen (im Verzeichnis „LICENSE“). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

Der Azure IoT Edge-Sicherheitsdaemon wird über ein PowerShell-Skript heruntergeladen und installiert. Der Sicherheitsdaemon startet dann das erste der beiden Runtimemodule, den IoT Edge-Agent. Dieser ermöglicht die Remotebereitstellung anderer Module.

>[!TIP]
>Bei IoT Core-Geräten empfehlen wir die Ausführung der Installationsbefehle über eine RemotePowerShell-Sitzung. Weitere Informationen finden Sie unter [Using PowerShell for Windows IoT](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell) (Verwenden von PowerShell für Windows IoT).

Wenn Sie die IoT Edge-Runtime zum ersten Mal auf einem Gerät installieren, müssen Sie dem Gerät eine Identität von einem IoT-Hub bereitstellen. Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Alternativ können Sie IoT Hub Device Provisioning Service verwenden, um automatisch Geräte bereitzustellen. Dieses Verfahren ist hilfreich, wenn viele Geräte eingerichtet werden müssen. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen.

In den folgenden Abschnitten werden die häufigsten Anwendungsfälle und Parameter für IoT Edge-Installationsskripts auf einem neuen Gerät erläutert.

### <a name="option-1-install-and-manually-provision"></a>Option 1: Installation und manuelle Bereitstellung

Hier stellen Sie eine **Geräteverbindungszeichenfolge** bereit, die von IoT Hub generiert wurde, um das Gerät bereitzustellen.

In diesem Beispiel wird die manuelle Installation mit Windows-Containern veranschaulicht:

1. Registrieren Sie ein neues IoT Edge-Gerät, und rufen Sie die **Geräteverbindungszeichenfolge** ab, falls Sie dies noch nicht getan haben. Notieren Sie sich diese Verbindungszeichenfolge zur späteren Verwendung in diesem Abschnitt. Sie können diesen Schritt mithilfe der folgenden Tools ausführen:

   * [Azure portal](how-to-register-device.md#register-in-the-azure-portal)
   * [Azure-Befehlszeilenschnittstelle](how-to-register-device.md#register-with-the-azure-cli)
   * [Visual Studio Code](how-to-register-device.md#register-with-visual-studio-code)

2. Führen Sie PowerShell als Administrator aus.

   >[!NOTE]
   >Verwenden Sie eine AMD64-PowerShell-Sitzung, um IoT Edge zu installieren, nicht PowerShell (x86). Wenn Sie nicht sicher sind, welchen Sitzungstyp Sie verwenden, führen Sie den folgenden Befehl aus:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. Durch den Befehl **Deploy-IoTEdge** wird überprüft, ob Ihr Windows-Computer über eine unterstützte Version verfügt. Außerdem aktiviert der Befehl das Containerfeature und lädt dann die Moby-Runtime und die IoT Edge-Runtime herunter. Der Befehl verwendet standardmäßig Windows-Container.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge
   ```

4. An diesem Punkt könnten IoT Core-Geräte möglicherweise automatisch neu starten. Andere Windows 10- oder Windows Server-Geräte könnten Sie zum Neustart auffordern. Wenn ja, starten Sie Ihr Gerät jetzt neu. Sobald Ihr Gerät bereit ist, führen Sie PowerShell erneut als Administrator aus.

5. Der Befehl **Initialize-IoTEdge** konfiguriert die IoT Edge-Runtime auf Ihrem Computer. Standardmäßig wird für den Befehl die manuelle Bereitstellung mit Windows-Containern verwendet.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge
   ```

6. Geben Sie bei Aufforderung die Geräteverbindungszeichenfolge ein, die Sie in Schritt 1 abgerufen haben. Die Geräteverbindungszeichenfolge ordnet das physische Gerät in IoT Hub einer Geräte-ID zu.

   Die Verbindungszeichenfolge hat das folgende Format und darf keine Anführungszeichen enthalten: `HostName={IoT hub name}.azure-devices.net;DeviceId={device name};SharedAccessKey={key}`

7. Überprüfen Sie mit den Schritten in [Bestätigen einer erfolgreichen Installation](#verify-successful-installation) den Status von IoT Edge auf Ihrem Gerät.

Wenn Sie ein Gerät manuell installieren und bereitstellen, können Sie zusätzliche Parameter verwenden, um die Installation folgendermaßen anzupassen:

* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können das Installationsprogramm auf ein Offlineverzeichnis verweisen.
* Sie können ein bestimmtes Containerimage für den Agent deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.

Um weitere Informationen zu diesen Installationsoptionen zu erhalten, fahren Sie mit [Alle Installationsparameter](#all-installation-parameters) fort.

### <a name="option-2-install-and-automatically-provision"></a>Option 2: Installation und automatische Bereitstellung

Hier stellen Sie das Gerät mithilfe von IoT Hub Device Provisioning Service bereit. Geben Sie die **Bereichs-ID** aus einer Instanz des Device Provisioning Service zusammen mit allen anderen Informationen ein, die für Ihren bevorzugten [Nachweismechanismus](../iot-dps/concepts-security.md#attestation-mechanism) spezifisch sind:

* [Erstellen und Bereitstellen eines simulierten IoT Edge-Geräts mit einem virtuellen TPM unter Windows](how-to-auto-provision-simulated-device-windows.md)
* [Erstellen und Bereitstellen eines simulierten IoT Edge-Geräts mit X.509-Zertifikaten](how-to-auto-provision-x509-certs.md)
* [Erstellen und Bereitstellen eines IoT Edge-Geräts mithilfe des Nachweises des symmetrischen Schlüssels](how-to-auto-provision-symmetric-keys.md)

Wenn Sie ein Gerät automatisch installieren und bereitstellen lassen, können Sie zusätzliche Parameter verwenden, um die Installation folgendermaßen anzupassen:

* Sie können direkten Datenverkehr über einen Proxyserver leiten.
* Sie können das Installationsprogramm auf ein Offlineverzeichnis verweisen.
* Sie können ein bestimmtes Containerimage für den Agent deklarieren und Anmeldeinformationen bereitstellen, wenn dieses sich in einer privaten Registrierung befindet.

Weitere Informationen zu diesen Installationsoptionen finden Sie in diesem Artikel. Sie können diesen auch überspringen und [hier](#all-installation-parameters) mehr über alle Installationsparameter erfahren.

## <a name="offline-or-specific-version-installation"></a>Offlineinstallation oder Installation einer bestimmten Version

Während der Installation werden drei Dateien heruntergeladen:

* Ein PowerShell-Skript, das die Installationsanweisungen enthält
* Eine Microsoft Azure IoT Edge-CAB-Datei, die IoT Edge-Sicherheitsdaemon (iotedged), Moby-Container-Engine und Moby-CLI enthält
* Ein Installationsprogramm für das Visual C++ Redistributable Package (VC-Runtime)

Wenn Ihr Gerät während der Installation offline ist oder wenn Sie eine bestimmte Version von IoT Edge installieren möchten, können Sie diese Dateien im Voraus auf das Gerät herunterladen. Wenn Sie die Installation durchführen möchten, verweisen Sie im Installationsskript auf das Verzeichnis, in dem die heruntergeladenen Dateien gespeichert sind. Das Installationsprogramm überprüft dieses Verzeichnis zuerst und lädt dann nur Komponenten herunter, die nicht gefunden werden konnten. Wenn alle Dateien offline verfügbar sind, können Sie die Installation ohne Internetverbindung durchführen.

Sie können auch den Parameter für den Offline-Installationspfad verwenden, um IoT Edge zu aktualisieren. Weitere Informationen finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).

1. Die aktuellen IoT Edge-Installationsdateien und die früheren Versionen finden Sie auf der Seite für [Azure IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases).

2. Suchen Sie die Version, die Sie installieren möchten, und laden Sie die folgenden Dateien aus dem Abschnitt **Assets** der Versionshinweise auf Ihr IoT-Gerät herunter:

   * IoTEdgeSecurityDaemon.ps1
   * Microsoft-Azure-IoTEdge-amd64.cab aus Release 1.0.9 oder neuer oder Microsoft-Azure-IoTEdge.cab aus Release 1.0.8 und älter.

   Microsoft-Azure-IotEdge-arm32.cab ist ab Version 1.0.9 nur für Testzwecke verfügbar. IoT Edge wird auf Windows-ARM32-Geräten derzeit nicht unterstützt.

   Es ist wichtig, das PowerShell-Skript für dasselbe Release zu verwenden, zu dem die CAB-Datei gehört, weil die Unterstützung für Features sich in jedem Release ändert.

3. Wenn die von Ihnen heruntergeladene CAB-Datei ein Suffix der Architektur enthält, benennen Sie die Datei in **Microsoft-Azure-IoTEdge.cab** um.

4. Optional können Sie auch ein Installationsprogramm für Visual C++ Redistributable herunterladen. Das PowerShell-Skript verwendet beispielsweise diese Version: [vc_redist.x64.exe](https://download.microsoft.com/download/0/6/4/064F84EA-D1DB-4EAA-9A5C-CC2F0FF6A638/vc_redist.x64.exe). Speichern Sie das Installationsprogramm auf Ihrem IoT-Gerät im selben Ordner, in dem sich die IoT Edge-Dateien befinden.

5. Für eine Installation mit Offlinekomponenten führen Sie eine [DOT-Quellentnahme](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7#script-scope-and-dot-sourcing) in der lokalen Kopie des PowerShell-Skripts durch. Verwenden Sie dann den `-OfflineInstallationPath`-Parameter als Teil des `Deploy-IoTEdge`-Befehls, und geben Sie den absoluten Pfad zum Dateiverzeichnis an. Beispiel:

   ```powershell
   . <path>\IoTEdgeSecurityDaemon.ps1
   Deploy-IoTEdge -OfflineInstallationPath <path>
   ```

   Der Bereitstellungsbefehl verwendet alle Komponenten, die im angegebenen lokalen Dateiverzeichnis gefunden werden. Wenn die CAB-Datei oder der Visual C++-Installer fehlt, versucht der Befehl, diese Komponenten herunterzuladen.

6. Führen Sie den Befehl `Initialize-IoTEdge` aus, um für Ihr Gerät eine Identität in IoT Hub anzugeben. Geben Sie entweder eine Geräteverbindungszeichenfolge für die manuelle Bereitstellung an, oder wählen Sie eine der im obigen Abschnitt zur [automatischen Bereitstellung](#option-2-install-and-automatically-provision) beschriebenen Methoden aus.

   Wenn Ihr Gerät nach der Ausführung von `Deploy-IoTEdge` neu gestartet wird, führen Sie erneut ein DOT-Quellentnahme im PowerShell-Skript durch, bevor Sie `Initialize-IoTEdge` ausführen.

Um weitere Informationen zur Offline-Installationsoption zu erhalten, fahren Sie mit [Alle Installationsparameter](#all-installation-parameters) fort.

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Überprüfen Sie den Status des IoT Edge-Diensts. Er sollte als „Wird ausgeführt“ aufgelistet werden.  

```powershell
Get-Service iotedge
```

Untersuchen Sie die Dienstprotokolle der letzten fünf Minuten. Wenn Sie die Installation der IoT Edge-Runtime gerade beendet haben, wird möglicherweise eine Liste von Fehlern aus der Zeit zwischen den Ausführungen von **Deploy-IoTEdge** und **Initialize-IoTEdge** angezeigt. Diese Fehler werden erwartet, da der Dienst versucht, zu starten, bevor er konfiguriert ist.

```powershell
. {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Führen Sie das [Tool zur Problembehandlung](troubleshoot.md#run-the-check-command) aus, um nach den häufigsten Konfigurations-und Netzwerkfehlern zu suchen.

```powershell
iotedge check
```

Das Systemmodul **$edgeHub** wird erst auf dem Gerät bereitgestellt, wenn Sie Ihr erstes Modul in IoT Edge auf dem Gerät bereitstellen. Infolgedessen wird bei der automatisierten Überprüfung ein Fehler für die Konnektivitätsprüfung `Edge Hub can bind to ports on host` zurückgegeben. Dieser Fehler kann ignoriert werden – es sei denn, er tritt nach der Bereitstellung eines Moduls auf dem Gerät auf.

Listen Sie abschließend die derzeit ausgeführten Module auf:

```powershell
iotedge list
```

Nach einer Neuinstallation sollte als einziges Modul **edgeAgent** ausgeführt werden. Nachdem Sie [IoT Edge-Module](how-to-deploy-modules-portal.md) zum ersten Mal bereitgestellt haben, wird das andere Systemmodul namens **edgeHub** ebenfalls auf dem Gerät gestartet.

## <a name="manage-module-containers"></a>Verwalten von Modulcontainern

Der IoT Edge-Dienst erfordert eine Containerengine, die auf Ihrem Gerät ausgeführt wird. Wenn Sie ein Modul auf einem Gerät bereitstellen, verwendet die IoT Edge-Runtime die Containerengine, um das Containerimage per Pullvorgang aus einer Registrierung in der Cloud abzurufen. Der IoT Edge-Dienst ermöglicht es Ihnen, mit Ihren Modulen zu interagieren und Protokolle abzurufen, aber unter Umständen möchten Sie die Containerengine verwenden, um mit dem Container selbst zu interagieren.

Weitere Informationen zu Modulkonzepten finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md).

Wenn Sie Windows-Container auf Ihrem Windows IoT Edge-Gerät ausgeführt haben, dann umfasste die IoT Edge-Installation die Moby-Containerengine. Die Moby-Engine basierte auf denselben Standards wie Docker und wurde für die parallele Ausführung auf demselben Computer wie Docker Desktop entwickelt. Wenn Sie von der Moby-Engine verwaltete Container als Ziel festlegen möchten, müssen Sie diese Engine anstelle von Docker als Ziel auswählen.

Verwenden Sie z. B. den folgenden Befehl, um alle Docker-Images aufzulisten:

```powershell
docker images
```

Ändern Sie denselben Befehl mit einem Zeiger auf die Moby-Engine, um alle Moby-Images aufzulisten:

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Die Engine-URI ist in der Ausgabe des Installationsskripts aufgeführt, oder Sie finden sie im Abschnitt für die Einstellungen der Containerruntime der Datei „config.yaml“.

![moby_runtime uri in config.yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Weitere Informationen zu Befehlen, mit denen Sie mit Containern und Images interagieren können, die auf Ihrem Gerät ausgeführt werden, finden Sie unter [Docker-Befehlszeilenschnittstellen](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="uninstall-iot-edge"></a>Deinstallieren von IoT Edge

Wenn Sie IoT Edge von Ihrem Windows-Gerät entfernen möchten, sollten Sie folgenden Befehl in einem PowerShell-Fenster ausführen, das mit Administratorrechten geöffnet wurde. Dieser Befehl entfernt die IoT Edge-Runtime, die vorhandenen Konfigurationen und die Daten der Moby-Engine.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-IoTEdge
```

Der Befehl Uninstall-IoTEdge funktioniert nicht unter Windows IoT Core. Um IoT Edge von Windows IoT Core-Geräten zu entfernen, müssen Sie Ihr Windows IoT Core-Image erneut bereitstellen.

Um weitere Informationen zu den Deinstallationsoptionen zu erhalten, verwenden Sie den Befehl `Get-Help Uninstall-IoTEdge -full`.

## <a name="verify-installation-script"></a>Überprüfen des Installationsskripts

Die in diesem Artikel angegebenen Installationsbefehle verwenden das Cmdlet „Invoke-WebRequest“ zum Anfordern des Installationsskripts von `aka.ms/iotedge-win`. Dieser Link verweist auf das Skript `IoTEdgeSecurityDaemon.ps1` aus dem aktuellsten [IoT Edge-Release](https://github.com/Azure/azure-iotedge/releases). Sie können dieses Skript oder eine Version des Skripts aus einer bestimmten Version auch herunterladen, um die Installationsbefehle auf Ihrem IoT Edge-Gerät auszuführen.

Das bereitgestellte Skript ist signiert, um die Sicherheit zu erhöhen. Sie können die Signatur überprüfen, indem Sie das Skript auf Ihr Gerät herunterladen und dann den folgenden PowerShell-Befehl ausführen:

```powershell
Get-AuthenticodeSignature "C:\<path>\IotEdgeSecurityDaemon.ps1"
```

Der Ausgabestatus lautet **Gültig**, wenn die Signatur überprüft wird.

## <a name="all-installation-parameters"></a>Alle Installationsparameter

In den vorherigen Abschnitten wurden gängige Installationsszenarios vorgestellt. Dabei wurden Beispiele angegeben, wie Sie Parameter verwenden können, um das Installationsskript anzupassen. Dieser Abschnitt enthält die Verweistabellen der allgemeinen Parameter, die zum Installieren, Aktualisieren oder Deinstallieren von IoT Edge verwendet werden.

### <a name="deploy-iotedge"></a>Deploy-IoTEdge

Der Befehl Deploy-IoTEdge lädt den IoT Edge-Sicherheitsdaemon und die zugehörigen Abhängigkeiten herunter und stellt sie bereit. Der Bereitstellungsbefehl akzeptiert diese allgemeinen Parametern neben anderen. Mit dem Befehl `Get-Help Deploy-IoTEdge -full` können Sie die vollständige Liste anzeigen.  

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** oder **Linux** | Wenn kein Containerbetriebssystem angegeben wird, ist „Windows“ der Standardwert.<br><br>Für Windows-Container verwendet IoT Edge die in der Installation enthaltene Moby-Container-Engine. Für Linux-Container müssen Sie eine Container-Engine installieren, bevor Sie mit der Installation beginnen. |
| **Proxy** | Proxy-URL | Verwenden Sie diesen Parameter, wenn Ihr Gerät die Internetverbindung über einen Proxyserver herstellen muss. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Verzeichnispfad | Wenn dieser Parameter verwendet wird, überprüft das Installationsprogramm das aufgelistete Verzeichnis auf die IoT Edge-CAB- und die VC-Runtime-MSI-Dateien, die für die Installation erforderlich sind. Alle im Verzeichnis nicht gefundenen Dateien werden heruntergeladen. Wenn sich beide Dateien im Verzeichnis befinden, können Sie IoT Edge ohne Internetverbindung installieren. Sie können diesen Parameter auch verwenden, um eine bestimmte Version zu verwenden. |
| **InvokeWebRequestParameters** | Hashtabelle mit Parametern und Werten | Während der Installation werden mehrere Webanforderungen durchgeführt. Verwenden Sie dieses Feld, um die Parameter für diese Webanforderungen festzulegen. Dieser Parameter ist bei der Konfiguration der Anmeldeinformationen für Proxyserver hilfreich. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **RestartIfNeeded** | none | Dieses Flag ermöglicht dem Bereitstellungsskript, den Computer bei Bedarf ohne Eingabeaufforderung neu zu starten. |

### <a name="initialize-iotedge"></a>Initialize-IoTEdge

Der Initialize-IoTEdge-Befehl konfiguriert IoT Edge mit Ihrer Geräte-Verbindungszeichenfolge und Ihren Betriebsdetails. Ein Großteil der durch diesen Befehl generierten Informationen wird dann in der Datei „iotedge\config.yaml“ gespeichert. Der Initialisierungsbefehl akzeptiert diese allgemeinen Parametern neben anderen. Mit dem Befehl `Get-Help Initialize-IoTEdge -full` können Sie die vollständige Liste anzeigen.

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| **Manuell** | Keine | **Switch-Parameter:** Wenn kein Bereitstellungstyp angegeben wird, ist die manuelle Bereitstellung der Standardwert.<br><br>Gibt an, dass Sie eine Geräteverbindungszeichenfolge angeben, um das Gerät manuell bereitzustellen. |
| **Dps** | Keine | **Switch-Parameter:** Wenn kein Bereitstellungstyp angegeben wird, ist die manuelle Bereitstellung der Standardwert.<br><br>Gibt an, dass Sie eine Bereichs-ID von einem Gerätebereitstellungsdienst (Device Provisioning Service, DPS) und die Registrierungs-ID Ihres Geräts angeben, damit die Bereitstellung über einen Gerätebereitstellungsdienst durchgeführt wird.  |
| **DeviceConnectionString** | Eine Verbindungszeichenfolge, die von einem IoT Edge-Gerät stammt, das in einem IoT-Hub registriert ist (in einfachen Anführungszeichen). | **Erforderlich** für die manuelle Bereitstellung. Wenn Sie in den Skriptparametern keine Verbindungszeichenfolge angeben, werden Sie dazu aufgefordert. |
| **ScopeId** | Eine Bereichs-ID, die aus der Instanz des Gerätebereitstellungsdiensts stammt, die Ihrem IoT-Hub zugeordnet ist. | **Erforderlich** für die DPS-Bereitstellung. Wenn Sie in den Skriptparametern keine Bereichs-ID angeben, werden Sie dazu aufgefordert. |
| **RegistrationId** | Eine Registrierungs-ID, die von Ihrem Gerät generiert wurde. | **Erforderlich** für die DPS-Bereitstellung, wenn TPM oder der Nachweis des symmetrischen Schlüssels verwendet wird. **Optional**, wenn der X.509-Zertifikatnachweis verwendet wird. |
| **X509IdentityCertificate** | Der URI-Pfad zum X.509-Geräteidentitätszertifikat auf dem Gerät. | **Erforderlich** für die DPS-Bereitstellung, wenn ein X.509-Zertifikatnachweis verwendet wird. |
| **X509IdentityPrivateKey** | Der URI-Pfad zum Schlüssel für das X.509-Geräteidentitätszertifikat auf dem Gerät. | **Erforderlich** für die DPS-Bereitstellung, wenn ein X.509-Zertifikatnachweis verwendet wird. |
| **SymmetricKey** | Der symmetrische Schlüssel, der bei der Verwendung von IoT Hub Device Provisioning Service für die Bereitstellung der IoT Edge-Geräteidentität verwendet wird. | **Erforderlich** für die DPS-Bereitstellung, wenn der Nachweis des symmetrischen Schlüssels verwendet wird. |
| **ContainerOs** | **Windows** oder **Linux** | Wenn kein Containerbetriebssystem angegeben wird, ist „Windows“ der Standardwert.<br><br>Für Windows-Container verwendet IoT Edge die in der Installation enthaltene Moby-Container-Engine. Für Linux-Container müssen Sie eine Container-Engine installieren, bevor Sie mit der Installation beginnen. |
| **InvokeWebRequestParameters** | Hashtabelle mit Parametern und Werten | Während der Installation werden mehrere Webanforderungen durchgeführt. Verwenden Sie dieses Feld, um die Parameter für diese Webanforderungen festzulegen. Dieser Parameter ist bei der Konfiguration der Anmeldeinformationen für Proxyserver hilfreich. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **AgentImage** | URI des IoT Edge-Agent-Images | Wenn Sie IoT Edge neu installieren, wird standardmäßig das neueste fortlaufende Tag für das IoT Edge-Agent-Image verwendet. Verwenden Sie diesen Parameter, um ein bestimmtes Tag für die Imageversion festzulegen, oder stellen Sie ein eigenes Agent-Image bereit. Weitere Informationen finden Sie unter [Grundlagen von IoT Edge-Tags](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Benutzername** | Benutzername der Containerregistrierung | Verwenden Sie diesen Parameter nur, wenn Sie den Parameter „-AgentImage“ auf einen Container in einer privaten Registrierung festlegen. Geben Sie einen Benutzernamen an, der auf die Registrierung zugreifen kann. |
| **Kennwort** | Sichere Kennwortzeichenfolge | Verwenden Sie diesen Parameter nur, wenn Sie den Parameter „-AgentImage“ auf einen Container in einer privaten Registrierung festlegen. Geben Sie das Kennwort an, mit dem auf die Registrierung zugegriffen werden kann. |

### <a name="update-iotedge"></a>Update-IoTEdge

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| **ContainerOs** | **Windows** oder **Linux** | Wenn kein Containerbetriebssystem angegeben wird, wird der Standardwert „Windows“ verwendet. Für Windows-Container wird eine Container-Engine im Rahmen der Installation installiert. Für Linux-Container müssen Sie eine Container-Engine installieren, bevor Sie mit der Installation beginnen. |
| **Proxy** | Proxy-URL | Verwenden Sie diesen Parameter, wenn Ihr Gerät die Internetverbindung über einen Proxyserver herstellen muss. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Hashtabelle mit Parametern und Werten | Während der Installation werden mehrere Webanforderungen durchgeführt. Verwenden Sie dieses Feld, um die Parameter für diese Webanforderungen festzulegen. Dieser Parameter ist bei der Konfiguration der Anmeldeinformationen für Proxyserver hilfreich. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Verzeichnispfad | Wenn dieser Parameter verwendet wird, überprüft das Installationsprogramm das aufgelistete Verzeichnis auf die IoT Edge-CAB- und die VC-Runtime-MSI-Dateien, die für die Installation erforderlich sind. Alle im Verzeichnis nicht gefundenen Dateien werden heruntergeladen. Wenn sich beide Dateien im Verzeichnis befinden, können Sie IoT Edge ohne Internetverbindung installieren. Sie können diesen Parameter auch verwenden, um eine bestimmte Version zu verwenden. |
| **RestartIfNeeded** | none | Dieses Flag ermöglicht dem Bereitstellungsskript, den Computer bei Bedarf ohne Eingabeaufforderung neu zu starten. |

### <a name="uninstall-iotedge"></a>Uninstall-IoTEdge

| Parameter | Zulässige Werte | Kommentare |
| --------- | --------------- | -------- |
| **Force** | none | Dieses Flag erzwingt die Deinstallation für den Fall, dass der vorherige Versuch der Deinstallation nicht erfolgreich war.
| **RestartIfNeeded** | none | Dieses Flag ermöglicht dem Deinstallationsskript, den Computer bei Bedarf ohne Eingabeaufforderung neu zu starten. |

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein IoT Edge-Gerät für die installierte Runtime bereitgestellt haben, können Sie [IoT Edge-Module bereitstellen](how-to-deploy-modules-portal.md).

Wenn Probleme bei der Installation von IoT Edge auftreten, finden Sie auf der Seite für die [Problembehandlung](troubleshoot.md) weitere Informationen.

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).
