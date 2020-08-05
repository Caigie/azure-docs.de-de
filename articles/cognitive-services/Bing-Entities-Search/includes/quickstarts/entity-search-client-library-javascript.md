---
title: 'Schnellstart: JavaScript-Clientbibliothek für die Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: f5a3b48fd6be48d468b66009a77de100962be10d
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405064"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Entitätssuche-Clientbibliothek für JavaScript mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js).

## <a name="prerequisites"></a>Voraussetzungen

* Die aktuelle Version von [Node.js](https://nodejs.org/en/download/)

* Das [Bing-Entitätssuche-SDK für Node.js](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

Installation des Bing-Entitätssuche-SDK:

1. Führen Sie `npm install ms-rest-azure` in Ihrer Entwicklungsumgebung aus.
2. Führen Sie `npm install @azure/cognitiveservices-entitysearch` in Ihrer Entwicklungsumgebung aus.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Erstellen und Initialisieren der Anwendung

1. Erstellen Sie eine neue JavaScript-Datei in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor, und fügen Sie die folgenden Anforderungen hinzu.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. Erstellen Sie mithilfe Ihres Abonnementschlüssels eine Instanz von `CognitiveServicesCredentials`. Dann erstellen Sie damit eine Instanz des Suchclients.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>Senden einer Anforderung und Erhalten einer Antwort

1. Senden Sie mit `entitiesOperations.search()` eine Suchanforderung für Entitäten. Nach dem Empfang einer Antwort geben Sie das `queryContext`-Element, die Anzahl der zurückgegebenen Ergebnisse, und die Beschreibung des ersten Ergebnisses aus.

    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App](../../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../../overview.md)