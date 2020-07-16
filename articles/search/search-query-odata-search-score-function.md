---
title: Referenz für die OData-Funktion „search.score“
titleSuffix: Azure Cognitive Search
description: Syntax und Referenzdokumentation für die Verwendung der search.score-Funktion in Azure Cognitive Search-Abfragen.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: c31304228d9629b0df7f7511ecca2616b4891ee7
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206962"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>OData-Funktion `search.score` in der kognitiven Azure-Suche

Wenn Sie eine Abfrage ohne den [Parameter **$orderby**](search-query-odata-orderby.md) an die kognitive Azure-Suche senden, werden die zurückgegebenen Ergebnisse in absteigender Reihenfolge nach Relevanz sortiert. Selbst wenn Sie **$orderby** verwenden, wird die Relevanzbewertung standardmäßig verwendet, um Verbindungen aufzuheben. Manchmal ist es jedoch sinnvoll, die Relevanzbewertung als erstes Sortierkriterium und einige andere Kriterien als Entscheidungshilfe zu verwenden. Die `search.score`-Funktion ermöglicht dies.

## <a name="syntax"></a>Syntax

Die Syntax für `search.score` in **$orderby** ist `search.score()`. Die Funktion `search.score` akzeptiert keine Parameter. Sie kann mit dem Spezifizierer `asc` oder `desc` für die Sortierreihenfolge verwendet werden, genau wie jede andere Klausel im Parameter **$orderby**. Sie kann an einer beliebigen Stelle in der Liste der Sortierkriterien platziert werden.

## <a name="example"></a>Beispiel

Sortieren von Hotels in absteigender Reihenfolge nach `search.score` und `rating` und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten, sodass von zwei Hotels mit identischen Bewertungen das nächstgelegene zuerst aufgeführt wird:

```odata-filter-expr
    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

## <a name="next-steps"></a>Nächste Schritte  

- [Übersicht über die OData-Ausdruckssprache für Azure Cognitive Search](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für Azure Cognitive Search](search-query-odata-syntax-reference.md)
- [Suchen von Dokumenten &#40;REST-API für die kognitive Azure-Suche&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
