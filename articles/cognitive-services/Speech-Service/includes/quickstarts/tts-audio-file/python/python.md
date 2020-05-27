---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: 4429a03709d0f565d5fcf98f6999c812c0d43561
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673083"
---
## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Python 3.5 bis 3.8](https://www.python.org/downloads/)
* Das Python Speech SDK-Paket ist für die folgenden Betriebssysteme verfügbar:
    * Windows: x64 und x86.
    * Mac: macOS X Version 10.12 oder höher
    * Linux: Ubuntu 16.04/18.04, Debian 9, RHEL 7/8, CentOS 7/8 für x64.
* Führen Sie unter Linux die folgenden Befehle aus, um die erforderlichen Pakete zu installieren:

# <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2
```

# <a name="debian-9"></a>[Debian 9](#tab/debian)

```Bash
sudo apt-get update
sudo apt-get install build-essential libssl1.0.2 libasound2
```

# <a name="rhelcentos"></a>[RHEL/CentOS](#tab/rhel-centos)

```Bash
sudo yum update
sudo yum install alsa-lib openssl python3
```

> [!NOTE]
> - Befolgen Sie in RHEL/CentOS 7 die Anweisungen zum [Konfigurieren von RHEL/CentOS 7 für das Speech SDK](~/articles/cognitive-services/speech-service/how-to-configure-rhel-centos-7.md).
> - Befolgen Sie unter RHEL/CentOS 8 die Anweisungen zum [Konfigurieren von OpenSSL für Linux](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md).

---

* Unter Windows benötigen Sie [Microsoft Visual C++ Redistributable für Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) für Ihre Plattform.

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Mit diesem Befehl wird das Python-Paket über [PyPI](https://pypi.org/) für das Speech SDK installiert:

```Bash
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Support und Updates

Updates für das Speech SDK-Paket für Python werden über PyPI verteilt und in den [Versionshinweisen](~/articles/cognitive-services/Speech-Service/releasenotes.md) angekündigt.
Wenn eine neue Version verfügbar ist, können Sie mit dem Befehl `pip install --upgrade azure-cognitiveservices-speech` auf diese Version aktualisieren.
Die derzeit installierte Version können Sie der Variablen `azure.cognitiveservices.speech.__version__` entnehmen.

Falls Probleme auftreten oder ein Feature fehlt, sehen Sie sich die [Support- und Hilfeoptionen](~/articles/cognitive-services/Speech-Service/support.md) an.

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Erstellen einer Python-Anwendung, die das Speech SDK verwendet

### <a name="run-the-sample"></a>Ausführen des Beispiels

Sie können den [Beispielcode](#sample-code) aus diesem Schnellstart in eine Quelldatei (`quickstart.py`) kopieren und in Ihrer integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) oder in der Konsole ausführen:

```Bash
python quickstart.py
```

Alternativ können Sie dieses Schnellstarttutorial als [Jupyter](https://jupyter.org) Notebook aus dem [Beispielrepository für das Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) herunterladen und als Notebook ausführen.

### <a name="sample-code"></a>Beispielcode

````python
import azure.cognitiveservices.speech as speechsdk

# Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)

# Creates an audio configuration that points to an audio file.
# Replace with your own audio filename.
audio_filename = "helloworld.wav"
audio_output = speechsdk.audio.AudioOutputConfig(filename=audio_filename)

# Creates a synthesizer with the given settings
speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_output)

# Synthesizes the text to speech.
# Replace with your own text.
text = "Hello world!"
result = speech_synthesizer.speak_text_async(text).get()

# Checks result.
if result.reason == speechsdk.ResultReason.SynthesizingAudioCompleted:
    print("Speech synthesized to [{}] for text [{}]".format(audio_filename, text))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech synthesis canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        if cancellation_details.error_details:
            print("Error details: {}".format(cancellation_details.error_details))
    print("Did you update the subscription info?")
````

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Installieren und Verwenden des Speech SDK mit Visual Studio Code

1. Laden Sie eine 64-Bit-Version (3.5 bis 3.8) von [Python](https://www.python.org/downloads/) herunter, und installieren Sie sie auf Ihrem Computer.
1. Laden Sie [Visual Studio Code](https://code.visualstudio.com/Download) herunter, und installieren Sie die Software.
1. Öffnen Sie Visual Studio Code, und installieren Sie die Python-Erweiterung. Wählen Sie im Menü **Datei** > **Einstellungen** > **Erweiterungen**. Suchen Sie nach **Python**.

   ![Installieren der Python-Erweiterung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-extension.png)

1. Erstellen Sie einen Ordner zum Speichern des Projekts. Ein Beispiel ist die Verwendung von Windows-Explorer.
1. Wählen Sie in Visual Studio Code das Symbol **Datei**. Öffnen Sie dann den von Ihnen erstellten Ordner.

   ![Öffnen eines Ordners](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-open-folder.png)

1. Erstellen Sie eine neue Python-Quelldatei (`speechsdk.py`), indem Sie das Symbol „Neue Datei“ auswählen.

   ![Erstellen von Dateien](~/articles/cognitive-services/Speech-Service/media/sdk/qs-python-vscode-python-newfile.png)

1. Kopieren Sie den [Python-Code](#sample-code), fügen Sie ihn in die neu erstellte Datei ein, und speichern Sie die Datei.
1. Fügen Sie Ihre Abonnementinformationen für den Speech-Dienst ein.
1. Wenn ein Python-Interpreter ausgewählt wurde, wird er auf der linken Seite der Statusleiste am unteren Rand des Fensters angezeigt.
   Zeigen Sie andernfalls eine Liste der verfügbaren Python-Interpreter an. Öffnen Sie die Befehlspalette (<kbd>STRG+UMSCHALT+P</kbd>), und geben Sie **Python: Select Interpreter** ein. Wählen Sie einen passenden Interpreter aus.
1. Sie können das Speech SDK-Paket für Python über Visual Studio Code installieren. Führen Sie diesen Schritt aus, wenn es für den ausgewählten Python-Interpreter noch nicht installiert wurde.
   Öffnen Sie zum Installieren des Speech SDK-Pakets ein Terminal. Öffnen Sie erneut die Befehlspalette (<kbd>STRG+UMSCHALT+P</kbd>), und geben Sie **Terminal: Neues integriertes Terminal erstellen** eingeben.
   Geben Sie im Terminal, das geöffnet wird, den Befehl `python -m pip install azure-cognitiveservices-speech` bzw. den entsprechenden Befehl für Ihr System ein.
1. Klicken Sie zum Ausführen des Beispielcodes mit der rechten Maustaste an einer beliebigen Stelle im Editor. Wählen Sie **Run Python File in Terminal** (Python-Datei im Terminal ausführen) aus.
   Ihr Text wird in Sprache konvertiert und in den angegebenen Audiodaten gespeichert.

   ```console
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

Falls Sie Probleme beim Ausführen dieser Anweisungen haben, können Sie das ausführlichere [Visual Studio Code-Tutorial für Python](https://code.visualstudio.com/docs/python/python-tutorial) zurate ziehen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Weitere Informationen

- [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) (Erstellen einer benutzerdefinierten Stimme)
- [Aufzeichnen benutzerdefinierter Stimmbeispiele](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
