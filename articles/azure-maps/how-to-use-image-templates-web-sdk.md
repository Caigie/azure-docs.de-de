---
title: Bildvorlagen im Azure Maps Web SDK | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie Bildvorlagen mit HTML-Markern und verschiedenen Ebenen im Microsoft Azure Maps Web SDK verwenden.
author: rbrundritt
ms.author: richbrun
ms.date: 8/6/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 28fee67ccfc1e67d89d0151c8e14bd7c0b688749
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207089"
---
# <a name="how-to-use-image-templates"></a>Verwenden von Imagevorlagen

Bilder können mit HTML-Markern und verschiedenen Ebenen innerhalb des Azure Maps Web SDK verwendet werden:

 - Symbolebenen können Punkte auf der Karte mit einem Bildsymbol rendern. Symbole können auch entlang eines Linienpfads dargestellt werden.
 - Polygon Ebenen können mit einem Füllmusterbild gerendert werden. 
 - HTML-Marker können Punkte mithilfe von Bildern und anderen HTML-Elementen rendern.

Um eine gute Leistung bei Ebenen zu gewährleisten, laden Sie die Bilder vor dem Rendern in die Sprite-Ressource des Kartenbilds. Die [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions) des SymbolLayers laden standardmäßig ein paar Markerbilder in einigen wenigen Farben in das Kartenbild-Sprite vor. Diese und weitere Markerbilder sind als SVG-Vorlagen verfügbar. Sie können zum Erstellen von Images mit benutzerdefinierten Skalen verwendet oder als primäre und sekundäre Farbe des Kunden verwendet werden. Insgesamt werden 42 Bildvorlagen bereitgestellt: 27 Zeichensymbole und 15 Füllmuster für Polygone.

Mithilfe der `map.imageSprite.createFromTemplate` -Funktion können Bildvorlagen zu den Sprite-Ressourcen des Kartenbilds hinzugefügt werden. Diese Funktion ermöglicht die Übergabe von bis zu fünf Parametern.

```javascript
createFromTemplate(id: string, templateName: string, color?: string, secondaryColor?: string, scale?: number): Promise<void>
```

Die `id` ist ein eindeutiger Bezeichner, den Sie erstellen. Die `id` wird dem Bild zugewiesen, wenn es dem Kartenbild-Sprite hinzugefügt wird. Verwenden Sie diesen Bezeichner in den Ebenen, um anzugeben, welche Bildressource gerendert werden soll. Der `templateName` gibt an, welche Bildvorlage verwendet werden soll. Die Option `color` legt die Primärfarbe des Bildes und die Optionen `secondaryColor` die Sekundärfarbe fest. Die Option `scale` skaliert die Bildvorlage, bevor sie auf den Bild-Sprite angewendet wird. Wenn das Bild auf den Bild-Sprite angewendet wird, wird es in ein PNG umgewandelt. Um ein gestochen scharfes Rendering zu gewährleisten, ist es besser, die Bildvorlage vor dem Hinzufügen zum Sprite zu vergrößern, als sie in einer Ebene zu vergrößern.

Diese Funktion lädt das Bild asynchron in den Bild-Sprite. Daher wird eine Zusage zurückgegeben, dass Sie auf den Abschluss dieser Funktion warten können.

Der folgende Code zeigt, wie Sie ein Bild aus einer der integrierten Vorlagen erstellen und mit einer Symbolebene verwenden können.

```javascript
map.imageSprite.createFromTemplate('myTemplatedIcon', 'marker-flat', 'teal', '#fff').then(function () {

    //Add a symbol layer that uses the custom created icon.
    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'myTemplatedIcon'
        }
    }));
});
```

## <a name="use-an-image-template-with-a-symbol-layer"></a>Verwenden einer Bildvorlage mit einer Symbolebene

Sobald eine Bildvorlage in den Kartenbild-Sprite geladen wurde, kann sie als Symbol in einer Symbolebene gerendert werden, indem auf die Bildressourcen-ID in der Option `image` der `iconOptions` verwiesen wird.

Das folgende Beispiel rendert eine Symbolebene mit der Bildvorlage `marker-flat` mit einer türkisfarbenen Primärfarbe und einer weißen Sekundärfarbe. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Symbolebene mit integrierter Symbolvorlage" src="//codepen.io/azuremaps/embed/VoQMPp/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/VoQMPp/'>Symbolebene mit integrierter Symbolvorlage</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="use-an-image-template-along-a-lines-path"></a>Verwenden einer Bildvorlage entlang eines Linienpfads

Sobald eine Bildvorlage in den Kartenbild-Sprite geladen wurde, kann sie entlang des Pfades einer Linie gerendert werden, indem ein LineString zu einer Datenquelle hinzugefügt und eine Symbolebene mit einer `lineSpacing`-Option verwendet wird und indem mit der `image`-Option von der `iconOptions` auf die ID der Bildressource verwiesen wird. 

Das folgende Beispiel rendert eine rosafarbene Linie auf der Karte und verwendet eine Symbolebene unter Verwendung der Bildvorlage `car` mit einer cyanblauen Primärfarbe und einer weißen Sekundärfarbe. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Linienebene mit integrierter Symbolvorlage" src="//codepen.io/azuremaps/embed/KOQvJe/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/KOQvJe/'>Linienebene mit integrierter Symbolvorlage</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Wenn die Bildvorlage nach oben zeigt, stellen Sie die Symboloption `rotation` der Symbolebene auf 90, wenn Sie möchten, dass sie in die gleiche Richtung wie die Linie zeigt.

## <a name="use-an-image-template-with-a-polygon-layer"></a>Verwenden einer Bildvorlage mit einer Polygonebene

Sobald eine Bildvorlage in den Kartenbild-Sprite geladen wurde, kann sie als ein Füllmuster in einer Polygonebene gerendert werden, indem auf die Bildressourcen-ID in der Option `fillPattern` der Ebene verwiesen wird.

Das folgende Beispiel rendert eine Polygonebene mit der Bildvorlage `dot` mit einer türkisfarbenen Primärfarbe und einer weißen Sekundärfarbe.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Füllpolygon mit integrierter Symbolvorlage" src="//codepen.io/azuremaps/embed/WVMEmz/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WVMEmz/'>Füllpolygon mit integrierter Symbolvorlage</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Durch das Einstellen der Sekundärfarbe von Füllmustern ist es einfacher zu erkennen, dass die darunterliegende Karte weiterhin das primäre Muster ausgibt. 

## <a name="use-an-image-template-with-an-html-marker"></a>Verwenden einer Bildvorlage mit einem HTML-Marker

Eine Bildvorlage kann mit der Funktion `altas.getImageTemplate` abgerufen und als Inhalt eines HTML-Markers verwendet werden. Die Vorlage kann an die Option `htmlContent` des Markers übergeben und dann mit den Optionen `color`, `secondaryColor` und `text` angepasst werden.

Das folgende Beispiel verwendet die `marker-arrow`-Vorlage mit einer roten Primärfarbe, einer rosa Sekundärfarbe und einem Textwert von „00“.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="HTML-Marker mit integrierter Symbolvorlage" src="//codepen.io/azuremaps/embed/EqQvzq/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/EqQvzq/'>HTML-Marker mit integrierter Symbolvorlage</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


> [!TIP]
> Bildvorlagen können auch außerhalb der Karte verwendet werden. Die getImageTemplate-Funktion gibt eine SVG-Zeichenfolge mit Platzhaltern zurück: `{color}`, `{secondaryColor}``{scale}`, `{text}`. Ersetzen Sie diese Platzhalterwerte, um eine gültige SVG-Zeichenfolge zu erstellen. Sie können dann entweder die SVG-Zeichenfolge direkt zum HTML-DOM hinzufügen oder sie in einen Daten-URI konvertieren und in ein Bildtag einfügen. Beispiel:
> ```JavaScript
> //Retrieve an SVG template and replace the placeholder values.
> var svg = atlas.getImageTemplate('marker').replace(/{color}/, 'red').replace(/{secondaryColor}/, 'white').replace(/{text}/, '').replace(/{scale}/, 1);
>
> //Convert to data URI for use in image tags.
> var dataUri = 'data:image/svg+xml;base64,' + btoa(svg);
> ```

## <a name="create-custom-reusable-templates"></a>Erstellen benutzerdefinierter wiederverwendbarer Vorlagen

Wenn Ihre Anwendung das gleiche Symbol mit unterschiedlichen Symbolen verwendet, oder wenn Sie ein Modul erstellen, das zusätzliche Bildvorlagen hinzufügt, können Sie diese Symbole ganz einfach aus dem Azure Maps Web SDK hinzufügen und abrufen. Verwenden Sie die folgenden statischen Funktionen für den `atlas`-Namespace.

| Name | Rückgabetyp | BESCHREIBUNG | 
|-|-|-|
| `addImageTemplate(templateName: string, template: string, override: boolean)` | | Fügt dem Atlas-Namespace eine benutzerdefinierte SVG-Bildvorlage hinzu. |
|  `getImageTemplate(templateName: string, scale?: number)`| Zeichenfolge | Ruft eine SVG-Vorlage anhand des Namens ab. |
| `getAllImageTemplateNames()` | string[] |  Ruft eine SVG-Vorlage anhand des Namens ab. |

SVG-Bildvorlagen unterstützen die folgenden Platzhalterwerte:

| Platzhalter | BESCHREIBUNG |
|-|-|
| `{color}` | Die Primärfarbe. | 
| `{secondaryColor}` | Die Sekundärfarbe. | 
| `{scale}` | Das SVG-Bild wird in ein PNG-Bild konvertiert, wenn es dem Kartenbild-Sprite hinzugefügt wird. Dieser Platzhalter kann verwendet werden, um eine Vorlage vor der Konvertierung zu skalieren, um sicherzustellen, dass sie deutlich gerendert wird. | 
| `{text}` | Die Position, an der Text gerendert werden soll, wenn er mit einem HTML-Marker verwendet wird. |

Das folgende Beispiel zeigt, wie Sie eine SVG-Vorlage als wiederverwendbare Symbolvorlage dem Azure Maps Web SDK hinzufügen können. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Hinzufügen einer benutzerdefinierten Symbolvorlage zum Atlas-Namespace" src="//codepen.io/azuremaps/embed/NQyvEX/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/NQyvEX/'>Hinzufügen einer Symbolvorlage zum Atlas-Namespace</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="list-of-image-templates"></a>Liste der Bildvorlagen

Die Tabelle listet alle derzeit im Azure Maps Web SDK verfügbaren Bildvorlagen auf. Der Vorlagenname steht oberhalb jedes Bilds. Standardmäßig ist die Primärfarbe Blau und die Sekundärfarbe Weiß. Um die Sekundärfarbe auf weißem Hintergrund besser sichtbar zu machen, sind die folgenden Bilder mit der Sekundärfarbe Schwarz versehen.

**Symbolvorlagen**

|||||
|:-:|:-:|:-:|:-:|
| Marker | Marker-Dick | Marker-Kreismarker | Marker-Flach |
|![Markersymbol](./media/image-templates/marker.png)|![Symbol „Marker-Dick“](./media/image-templates/marker-thick.png)|![Symbol „Marker-Kreis“](./media/image-templates/marker-circle.png)|![Symbol „Marker-Flach“](./media/image-templates/marker-flat.png)|
||||
| Marker-Quadrat | Marker-Quadrat-Cluster | Marker-Pfeil | Marker-Ball-Pin | 
|![Symbol „Marker-Quadrat“](./media/image-templates/marker-square.png)|![Symbol „Marker-Quadrat-Cluster“](./media/image-templates/marker-square-cluster.png)|![Symbol „Marker-Pfeil“](./media/image-templates/marker-arrow.png)|![Symbol „Marker-Ball-Pin“](./media/image-templates/marker-ball-pin.png)|
||||
| Marker-quadratisch-gerundet | Marker-Quadrat-gerundet-Cluster | Flag | Flagge-Dreieck |
| ![Symbol „Marker-quadratisch-gerundet“](./media/image-templates/marker-square-rounded.png) | ![Symbol „Marker-Quadrat-gerundet-Cluster“](./media/image-templates/marker-square-rounded-cluster.png) | ![Symbol „Flagge“](./media/image-templates/flag.png) | ![Symbol „Flagge-Dreieck“](./media/image-templates/flag-triangle.png) |
||||
| Dreieck | Dreieck-dick | Dreieck-Pfeil-hoch | Dreieck-Pfeil-links |
| ![Symbol „Dreieck“](./media/image-templates/triangle.png) | ![Symbol „Dreieck-dick“](./media/image-templates/triangle-thick.png) | ![Symbol „Dreieck-Pfeil-hoch“](./media/image-templates/triangle-arrow-up.png) | ![Symbol „Dreieck-Pfeil-links“](./media/image-templates/triangle-arrow-left.png) |
||||
| Hexagon | Hexagon-dick | Hexagon-gerundet | Hexagon-gerundet-dick |
| ![Symbol „Hexagon“](./media/image-templates/hexagon.png) | ![Symbol „Hexagon-dick“](./media/image-templates/hexagon-thick.png) | ![Symbol „Hexagon-gerundet“](./media/image-templates/hexagon-rounded.png) | ![Symbol „Hexagon-gerundet-dick“](./media/image-templates/hexagon-rounded-thick.png) |
||||
| Pin | Pin-rund | Gerundetes Quadrat | Gerundetes Quadrat-dick |
| ![Symbol „Pin“](./media/image-templates/pin.png) | ![Symbol „Pin-rund“](./media/image-templates/pin-round.png) | ![Symbol „Gerundetes Quadrat“](./media/image-templates/rounded-square.png) | ![Symbol „Gerundetes Quadrat-dick“](./media/image-templates/rounded-square-thick.png) |
||||
| Pfeil-hoch | Pfeil-hoch-dünn | Auto ||
| ![Symbol „Pfeil-hoch“](./media/image-templates/arrow-up.png) | ![Symbol „Pfeil-hoch-dünn“](./media/image-templates/arrow-up-thin.png) | ![Auto-Symbol](./media/image-templates/car.png) | |

**Vorlagen für Polygonfüllmuster**

|||||
|:-:|:-:|:-:|:-:|
| Schachbrettmuster | Schachbrettmuster-gedreht | Kreise | Kreise-Abstand |
| ![Symbol „Schachbrettmuster“](./media/image-templates/checker.png) | ![Symbol „Schachbrettmuster-gedreht“](./media/image-templates/checker-rotated.png) | ![Symbol „Kreise“](./media/image-templates/circles.png) | ![Symbol „Kreise-Abstand“](./media/image-templates/circles-spaced.png) |
|||||
| Diagonal-Linien-hoch | Diagonal-Linien-runter | Diagonal-Streifen-hoch | Diagonal-Streifen-runter |
| ![Symbol „Diagonal-Linien-hoch“](./media/image-templates/diagonal-lines-up.png) | ![Symbol „Diagonal-Linien-runter“](./media/image-templates/diagonal-lines-down.png) | ![Symbol „Diagonal-Streifen-hoch“](./media/image-templates/diagonal-stripes-up.png) | ![Symbol „Diagonal-Streifen-runter-runter“](./media/image-templates/diagonal-stripes-down.png) |
|||||
| Rasterlinien | Gedrehte Rasterlinien | Gedrehte Rasterstreifen | Füllung Kreuz |
| ![Symbol „Rasterlinien“](./media/image-templates/grid-lines.png) | ![Symbol „Gedrehte Rasterlinien“](./media/image-templates/rotated-grid-lines.png) | ![Symbol „Gedrehte Rasterstreifen“](./media/image-templates/rotated-grid-stripes.png) | ![Symbol „Füllung Kreuz“](./media/image-templates/x-fill.png) |
|||||
| Zickzack | Zickzack-vertikal | Punkte |  |
| ![Symbol „Zickzack“](./media/image-templates/zig-zag.png) | ![Symbol „Zickzack-vertikal“](./media/image-templates/zig-zag-vertical.png) | ![Symbol „Punkte“](./media/image-templates/dots.png) | |

**Vorab geladene Bildsymbole**

Die Karte lädt eine Reihe von Symbolen mithilfe der Vorlagen `marker`, `pin` und `pin-round` in das Kartenbild-Sprite. Diese Symbolnamen und ihre Farbwerte sind in der folgenden Tabelle aufgeführt.

| Symbolname | color | secondaryColor |
|-----------|-------|----------------|
| `marker-black` | `#231f20` | `#ffffff` |
| `marker-blue` | `#1a73aa` | `#ffffff` |
| `marker-darkblue` | `#003963` | `#ffffff` |
| `marker-red` | `#ef4c4c` | `#ffffff` |
| `marker-yellow` | `#f2c851` | `#ffffff` |
| `pin-blue` | `#2072b8` | `#ffffff` |
| `pin-darkblue` | `#003963` | `#ffffff` |
| `pin-red` | `#ef4c4c` | `#ffffff` |
| `pin-round-blue` | `#2072b8` | `#ffffff` |
| `pin-round-darkblue` | `#003963` | `#ffffff` |
| `pin-round-red` | `#ef4c4c` | `#ffffff` |


## <a name="try-it-now-tool"></a>Tool „Jetzt testen“

Mit dem folgenden Tool können Sie die verschiedenen integrierten Bildvorlagen auf verschiedene Weise rendern und die Primär- und Sekundärfarben sowie die Skalierung anpassen.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Optionen für Symbolvorlagen" src="//codepen.io/azuremaps/embed/NQyaaO/?height=500&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/NQyaaO/'>Symbolvorlagenoptionen</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [ImageSpriteManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagespritemanager)

> [!div class="nextstepaction"]
> [Atlas-Namespace](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-maps-typescript-latest#functions
)

In den folgenden Artikeln finden Sie weitere Codebeispiele, in denen Bildvorlagen verwendet werden können:

> [!div class="nextstepaction"]
> [Hinzufügen einer Symbolebene](map-add-pin.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen von HTML-Markern](map-add-bubble-layer.md)
