---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 07/31/2020
ms.author: rgarcia
ms.openlocfilehash: 310c0f547ee11a3243589c364755a30a84be1a25
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810169"
---
## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten:

* Sie haben die Informationen unter [Azure Spatial Anchors-Übersicht](../articles/spatial-anchors/overview.md) gelesen.
* Sie haben eine der [fünfminütigen Schnellstartanleitungen](../articles/spatial-anchors/index.yml) absolviert.
* Grundlegende Kenntnisse zu C# und Unity.
* Grundlegende Kenntnisse zu <a href="https://developers.google.com/ar/discover/" target="_blank">ARCore</a> (bei Verwendung von Android) oder <a href="https://developer.apple.com/arkit/" target="_blank">ARKit</a> (bei Verwendung von iOS).
* Einen Windows-Computer mit der **ASP.NET- und Webentwicklungs**-Workload, auf dem <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a> oder höher installiert ist.
* Das [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download)
* Mindestens ein Gerät (iOS oder Android) zum Bereitstellen und Ausführen einer App.
  * Bei Verwendung von Android benötigen Sie Folgendes:
    * <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3</a> oder höher, <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.4 (LTS)</a> und <a href="https://git-scm.com/download/win" target="_blank">Git für Windows</a> auf Ihrem Windows-Computer
    * Ein <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">für Entwickler geeignetes</a> und <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">ARCore-fähiges</a> Android-Gerät.
  * Bei Verwendung von iOS benötigen Sie Folgendes:
    * Einen macOS-Computer, auf dem <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a> oder höher, <a href="https://cocoapods.org" target="_blank">CocoaPods</a> und <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2019.4 (LTS)</a> installiert sind
    * Ein für Entwickler geeignetes <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">ARKit-kompatibles</a> iOS-Gerät.
    * Git-Installation über Homebrew. Geben Sie den folgenden Befehl in einer einzelnen Zeile am Terminal ein: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` Führen Sie dann `brew install git` aus.
