---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/18/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: 54ccaebd84c6af308ddcfa956add7f84b6e55832
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321024"
---
Der Container bietet websocketbasierte Abfrageendpunkt-APIs, auf die über das [Speech SDK](../index.yml) zugegriffen wird. Standardmäßig verwendet das Speech SDK Online-Speech-Dienste. Um den Container verwenden zu können, müssen Sie die Initialisierungsmethode ändern.

> [!TIP]
> Wenn Sie das Sprach-SDK mit Containern verwenden, müssen Sie nicht die Azure Speech-Ressource [Abonnementschlüssel oder Authentifizierungsbearertoken](../rest-speech-to-text.md#authentication) bereitstellen.

Weitere Informationen finden Sie in den folgenden Beispielen.

# <a name="c"></a>[C#](#tab/csharp)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

in den folgenden Aufruf mit dem [Containerhost](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet):

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```

# <a name="python"></a>[Python](#tab/python)

Ändern Sie den folgenden Azure-Cloudinitialisierungsaufruf

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

---
