---
title: Azure Application Insights für JavaScript-Web-Apps
description: Rufen Sie die Anzahl der Seitenaufrufe und Sitzungen, Webclientdaten und Single-Page-Anwendungen (SPA) ab, und verfolgen Sie Verwendungsmuster. Erkennen Sie Ausnahmen und Leistungsprobleme in JavaScript-Web-Apps.
ms.topic: conceptual
author: Dawgfan
ms.author: mmcc
ms.date: 09/20/2019
ms.openlocfilehash: 50ce0d57ec7395c69bf65e41b67f0cb005a43cb8
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854982"
---
# <a name="application-insights-for-web-pages"></a>Application Insights für Webseiten

Informieren Sie sich über die Leistung und Nutzung Ihrer Webseite oder App. Wenn Sie [Application Insights](app-insights-overview.md) Ihrem Seitenskript hinzufügen, erhalten Sie Zeitangaben zu Seitenladevorgängen und AJAX-Aufrufen, Anzahl und Details von Browserausnahmen und AJAX-Fehlern sowie die Anzahl von Benutzern und Sitzungen. Diese Informationen können jeweils nach Seite, Clientbetriebssystem und Browserversion, geografischer Position und anderen Dimensionen segmentiert werden. Sie können Warnungen für die Fehleranzahl oder das langsame Laden von Seiten festlegen. Und indem Sie Ablaufverfolgungsaufrufe in JavaScript-Code einfügen, können Sie nachverfolgen, wie die verschiedenen Funktionen Ihre Webseitenanwendung genutzt werden.

Application Insights kann mit allen Webseiten verwendet werden. Hierfür müssen Sie lediglich einen kurzen JavaScript-Codeabschnitt hinzufügen. Wenn Ihr Webdienst [Java](java-get-started.md) oder [ASP.NET](asp-net.md) ist, können Sie die serverseitigen SDKs zusammen mit dem clientseitigen JavaScript SDK verwenden, um die Leistung Ihrer App vollständig zu verstehen.

## <a name="adding-the-javascript-sdk"></a>Hinzufügen des JavaScript SDK

1. Zuerst benötigen Sie eine Application Insights-Ressource. Wenn Sie noch keine Ressource und keinen Instrumentierungsschlüssel haben, folgen Sie den Anweisungen unter [Erstellen einer neuen Ressource](create-new-resource.md).
2. Kopieren Sie den Instrumentierungsschlüssel aus der Ressource, an die Ihre JavaScript-Telemetriedaten gesendet werden sollen.
3. Fügen Sie die Application Insights JavaScript SDK Ihrer Webseite oder App über eine der beiden folgenden Optionen hinzu:
    * [npm-Setup](#npm-based-setup)
    * [JavaScript-Codeausschnitt](#snippet-based-setup)

> [!IMPORTANT]
> Verwenden Sie nur eine Methode, um Ihrer Anwendung das JavaScript SDK hinzuzufügen. Wenn Sie das NPM-Setup verwenden, verwenden Sie nicht den Codeausschnitt und umgekehrt.

> [!NOTE]
> Beim NPM-Setup wird das JavaScript SDK als eine Abhängigkeit in Ihrem Projekt installiert, wodurch IntelliSense aktiviert wird, während mit dem Codeausschnitt das SDK zur Laufzeit abgerufen wird. Beide unterstützen die gleichen Funktionen. Entwickler, die weitere benutzerdefinierte Ereignisse und Konfigurationen wünschen, entscheiden sich in der Regel für das NPM-Setup, während sich Benutzer, die eine schnelle Aktivierung der standardmäßig verfügbaren Webanalyse wünschen, für den Codeausschnitt entscheiden.

### <a name="npm-based-setup"></a>npm-basiertes Setup

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView(); // Manually call trackPageView to establish the current user/session/pageview
```

### <a name="snippet-based-setup"></a>Ausschnittbasiertes Setup

Wenn Ihre App npm nicht verwendet, können Sie Ihre Webseiten mit Application Insights direkt instrumentieren, indem Sie diesen Codeausschnitt am Anfang jeder Ihrer Seiten einfügen. Vorzugsweise sollte es das erste Skript in Ihrem `<head>`-Abschnitt sein, damit potenzielle Probleme mit allen Ihren Abhängigkeiten überwacht werden können. Wenn Sie die Blazor-Server-App verwenden, fügen Sie den Codeausschnitt am Anfang der Datei `_Host.cshtml` im Abschnitt `<head>` ein.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Senden von Telemetriedaten an das Azure-Portal

Standardmäßig sammelt das Application Insights JavaScript SDK automatisch eine Reihe von Telemetrieelementen, die bei der Ermittlung der Integrität Ihrer Anwendung und der zugrunde liegenden Benutzeroberfläche hilfreich sind. Dazu gehören:

- **Nicht abgefangene Ausnahmen** in Ihrer App, einschließlich Informationen zu
    - Stapelüberwachung
    - Ausnahmedetails und Meldung, die den Fehler begleitet
    - Zeilen- und Spaltennummer des Fehlers
    - URL, bei der der Fehler ausgelöst wurde
- **Anforderungen an die Netzwerkabhängigkeit**, die von den **XHR** und **Fetch**-Anforderungen Ihrer App ausgegeben werden (die Abrufsammlung ist standardmäßig deaktiviert), einschließlich Informationen zu
    - URL der Abhängigkeitsquelle
    - Befehl und Methode, der bzw. die zum Anfordern der Abhängigkeit verwendet wird
    - Dauer der Anforderung
    - Ergebniscode und Erfolgsstatus der Anforderung
    - ID (sofern vorhanden) des Benutzers, der die Anforderung sendet
    - Korrelationskontext (falls vorhanden), in dem die Anforderung ausgegeben wird
- **Benutzerinformationen** (z.B. Speicherort, Netzwerk, IP)
- **Geräteinformationen** (z. B. Browser, Betriebssystem, Version, Sprache, Modell)
- **Sitzungsinformationen**

### <a name="telemetry-initializers"></a>Telemetrieinitialisierer
Telemetrieinitialisierer werden verwendet, um den Inhalt der gesammelten Telemetriedaten zu ändern, bevor sie vom Browser des Benutzers gesendet werden. Sie können auch verwendet werden, um zu verhindern, dass bestimmte Telemetriedaten gesendet werden, indem sie `false` zurückgeben. Ihrer Application Insights-Instanz können mehrere Telemetrieinitialisierer hinzugefügt werden, und sie werden ausgeführt, um sie hinzuzufügen.

Das Eingabeargument für `addTelemetryInitializer` ist ein Rückruf, der ein [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) als Argument verwendet und ein `boolean` oder `void`zurückgibt. Bei Rückgabe von `false` wird das Telemetrieelement nicht gesendet. Andernfalls fährt es mit dem nächsten Telemetrieinitialisierer (falls vorhanden) fort, oder es wird an den Endpunkt der Telemetriedatensammlung gesendet.

Ein Beispiel für die Verwendung von Telemetrieinitialisierern:
```ts
var telemetryInitializer = (envelope) => {
  envelope.data.someField = 'This item passed through my telemetry initializer';
};
appInsights.addTelemetryInitializer(telemetryInitializer);
appInsights.trackTrace({message: 'This message will use a telemetry initializer'});

appInsights.addTelemetryInitializer(() => false); // Nothing is sent after this is executed
appInsights.trackTrace({message: 'this message will not be sent'}); // Not sent
```

## <a name="configuration"></a>Konfiguration
Die meisten Konfigurationsfelder werden so benannt, dass sie standardmäßig auf „false“ festgelegt werden können. Alle Felder mit Ausnahme von `instrumentationKey` sind optional.

| Name | Standard | BESCHREIBUNG |
|------|---------|-------------|
| instrumentationKey | NULL | **Erforderlich**<br>Instrumentierungsschlüssel, den Sie aus dem Azure-Portal abgerufen haben. |
| accountId | NULL | Eine optionale Konto-ID, wenn Ihre App Benutzer in Konten gruppiert. Keine Leerzeichen, Kommas, Semikolons, Gleichheitszeichen oder senkrechten Striche |
| sessionRenewalMs | 1800000 | Eine Sitzung wird protokolliert, wenn der Benutzer während dieser Zeitspanne (in Millisekunden) inaktiv ist. Die Standardeinstellung ist „30 Minuten“. |
| sessionExpirationMs | 86400000 | Eine Sitzung wird protokolliert, wenn sie während dieser Zeitspanne (in Millisekunden) fortgesetzt wurde. Die Standardeinstellung ist „24 Stunden“. |
| maxBatchSizeInBytes | 10000 | Maximale Größe des Telemetriedatenbatches. Wenn ein Batch diesen Grenzwert überschreitet, wird er sofort gesendet, und ein neuer Batch wird gestartet. |
| maxBatchInterval | 15000 | Dauer der Batchverarbeitung von Telemetriedaten vor dem Senden (Millisekunden) |
| disableExceptionTracking | false | Bei „true“ werden Ausnahmen nicht automatisch gesammelt. Der Standardwert ist "false". |
| disableTelemetry | false | Bei „true“ werden Telemetriedaten nicht gesammelt oder gesendet. Der Standardwert ist "false". |
| enableDebug | false | Bei „true“ werden **interne** Debugdaten als eine Ausnahme ausgelöst **statt** protokolliert zu werden. Dies geschieht unabhängig von den SDK-Protokollierungseinstellungen. Der Standardwert ist "false". <br>***Hinweis:*** Wenn Sie diese Einstellung aktivieren, werden die Telemetriedaten beim Auftreten eines internen Fehlers verworfen. Dies kann hilfreich sein, um Probleme mit Ihrer Konfiguration oder der Nutzung des SDK schnell zu identifizieren. Wenn beim Debuggen keine Telemetriedaten verloren gehen sollen, empfiehlt es sich, `consoleLoggingLevel` oder `telemetryLoggingLevel` statt `enableDebug` zu verwenden. |
| loggingLevelConsole | 0 | Protokolliert **interne** Application Insights-Fehler in der Konsole. <br>0: aus, <br>1: Nur schwerwiegende Fehler, <br>2: Alles (Fehler und Warnungen) |
| loggingLevelTelemetry | 1 | Sendet **interne** Application Insights-Fehler als Telemetriedaten. <br>0: aus, <br>1: Nur schwerwiegende Fehler, <br>2: Alles (Fehler und Warnungen) |
| diagnosticLogInterval | 10000 | (intern) Abrufintervall (in ms) für interne Protokollierungswarteschlange |
| samplingPercentage | 100 | Prozentsatz der Ereignisse, die gesendet werden. Die Standardeinstellung ist „100“. Dies bedeutet, dass alle Ereignisse gesendet werden. Legen Sie diesen Wert fest, wenn Sie Ihre Datenobergrenze für umfangreiche Anwendungen beibehalten möchten. |
| autoTrackPageVisitTime | false | Bei „true“ wird in einem Seitenaufruf die vorherige Ansichtszeit der instrumentierten Seite nachverfolgt und in Form von Telemetriedaten gesendet. Außerdem wird ein neuer Zeitgeber für den aktuellen Seitenaufruf gestartet. Der Standardwert ist "false". |
| disableAjaxTracking | false | Bei „true“ werden AJAX-Aufrufe nicht automatisch gesammelt. Der Standardwert ist "false". |
| disableFetchTracking | true | Bei „true“ werden Anforderungen zum Abrufen nicht automatisch gesammelt. Die Standardeinstellung ist „true“. |
| overridePageViewDuration | false | Bei „true“ wird das Standardverhalten von „trackPageView“ geändert, um bei dessen Aufruf das Ende des Intervalls für die Seitenaufrufdauer aufzuzeichnen. Bei „false“ ohne Angabe eines benutzerdefinierten Zeitraums für „trackPageView“ wird die Seitenaufrufleistung mithilfe der Navigationszeit-API berechnet. Der Standardwert ist "false". |
| maxAjaxCallsPerView | 500 | Mit der Standardeinstellung „500“ wird gesteuert, wie viele Ajax-Aufrufe pro Seitenaufruf überwacht werden. Legen Sie „–1“ fest, wenn alle (unbegrenzt) Ajax-Aufrufe auf der Seite überwacht werden sollen. |
| disableDataLossAnalysis | true | Bei „false“ werden interne Absenderpuffer für Telemetriedaten beim Start auf noch nicht gesendete Elemente überprüft. |
| disableCorrelationHeaders | false | Bei „false“ fügt das SDK allen Abhängigkeitsanforderungen zwei Kopfzeilen (‚Request-Id‘ und ‚Request-Context‘) hinzu, um sie mit entsprechenden serverseitigen Anforderungen zu korrelieren. Der Standardwert ist "false". |
| correlationHeaderExcludedDomains |  | Korrelations-Header für bestimmte Domänen deaktivieren |
| correlationHeaderDomains |  | Korrelations-Header für bestimmte Domänen aktivieren |
| disableFlushOnBeforeUnload | false | Die Standardeinstellung ist „false“. Bei „true“ wird die Methode „Flush“ (Leeren) beim Auslösen eines „onBeforeUnload“-Ereignisses nicht aufgerufen. |
| enableSessionStorageBuffer | true | Die Standardeinstellung ist „true“. Bei „true“ wird der Puffer mit allen nicht gesendeten Telemetriedaten im Sitzungsspeicher gespeichert. Der Puffer wird beim Laden der Seite wiederhergestellt. |
| isCookieUseDisabled | false | Die Standardeinstellung ist „false“. Bei „true“ speichert oder liest das SDK keine Daten aus Cookies.|
| cookieDomain | NULL | Benutzerdefinierte Cookiedomäne. Diese ist hilfreich, wenn Sie Application Insights-Cookies über untergeordnete Domänen hinweg freigeben möchten. |
| isRetryDisabled | false | Die Standardeinstellung ist „false“. Bei „false“ wiederholen Sie den Vorgang für 206 (teilweise erfolgreich), 408 (Timeout), 429 (zu viele Anforderungen), 500 (interner Serverfehler), 503 (Dienst nicht verfügbar) und 0 (offline, nur wenn erkannt). |
| isStorageUseDisabled | false | Bei „true“ speichert und liest das SDK keine Daten aus dem lokalen Speicher und dem Sitzungsspeicher. Der Standardwert ist "false". |
| isBeaconApiDisabled | true | Bei „false“ sendet das SDK alle Telemetriedaten mithilfe der [Beacon-API](https://www.w3.org/TR/beacon). |
| onunloadDisableBeacon | false | Die Standardeinstellung ist „false“. Wenn die Registerkarte geschlossen ist, sendet das SDK alle verbleibenden Telemetriedaten mithilfe der [Beacon-API](https://www.w3.org/TR/beacon). |
| sdkExtension | NULL | Legt den Erweiterungsnamen „sdk“ fest. Hierbei sind nur alphabetische Zeichen zulässig. Der Erweiterungsname wird dem Tag ‚ai.internal.sdkVersion‘ als Präfix hinzugefügt (z.B. ‚ext_javascript:2.0.0‘). Der Standardwert lautet null. |
| isBrowserLinkTrackingEnabled | false | Der Standardwert ist "false". Bei „true“ wird das SDK alle Anforderungen vom Typ [Browserverknüpfung](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) nachverfolgen. |
| appId | NULL | „AppId“ wird für die Korrelation zwischen AJAX-Abhängigkeiten verwendet, die clientseitig mit den serverseitigen Anforderungen erfolgt. Wenn die Beacon-API aktiviert ist, kann sie nicht automatisch verwendet werden. Sie kann aber in der Konfiguration manuell festgelegt werden. Die Standardeinstellung ist „null“. |
| enableCorsCorrelation | false | Bei „true“ fügt das SDK allen CORS-Anforderungen zwei Kopfzeilen (‚Request-Id‘ und ‚Request-Context‘) hinzu, um ausgehende AJAX-Abhängigkeiten mit entsprechenden serverseitigen Anforderungen zu korrelieren. Die Standardeinstellung ist „false“. |
| namePrefix | nicht definiert | Ein optionaler Wert, der als „name“-Postfix für „localStorage“ und den Cookienamen verwendet wird.
| enableAutoRouteTracking | false | Routenänderungen in Single-Page-Webanwendungen (Single Page Applications, SPA) automatisch nachverfolgen. Bei „true“ sendet jede Routenänderung einen neuen Seitenaufruf an Application Insights. Hashroutenänderungen (`example.com/foo#bar`) werden ebenfalls als neue Seitenaufrufe aufgezeichnet.
| enableRequestHeaderTracking | false | Bei „true“ werden AJAX- und Fetch-Anforderungsheader nachverfolgt.Der Standardwert ist „false“.
| enableResponseHeaderTracking | false | Bei „true“ werden Antwortheader für AJAX- und Fetch-Anforderungen nachverfolgt.Der Standardwert ist „false“.
| distributedTracingMode | `DistributedTracingModes.AI` | Legt den Modus für verteilte Ablaufverfolgung fest. Wenn der AI_AND_W3C-Modus oder der W3C-Modus festgelegt ist, werden Kontextheader für die W3C-Ablaufverfolgung (traceparent/tracestate) generiert und in alle ausgehenden Anforderungen eingeschlossen. AI_AND_W3C wird für Abwärtskompatibilität mit beliebigen mit älteren Application Insights-Versionen instrumentierten Diensten bereitgestellt.

## <a name="single-page-applications"></a>Single-Page-Webanwendungen

Standardmäßig verarbeitet dieses SDK **keine** statusbasierte Routenänderung, die in Single-Page-Webanwendungen erfolgt. Um das automatische Nachverfolgen von Routenänderungen für Ihre Single-Page-Webanwendung zu aktivieren, können Sie Ihrer Setupkonfiguration `enableAutoRouteTracking: true` hinzufügen.

Wir bieten derzeit ein separates [React-Plug-In](#react-extensions) an, das Sie mit diesem SDK initialisieren können. Es wird auch die Nachverfolgung von Routenänderungen automatisch ausführen sowie [weitere React-spezifische Telemetriedaten](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md) sammeln.

> [!NOTE]
> Verwenden Sie `enableAutoRouteTracking: true` nur, wenn Sie das React-Plug-In **nicht** verwenden. Beide können bei Routenänderungen neue Seitenaufrufe senden. Wenn beide aktiviert sind, werden möglicherweise doppelte Seitenaufrufe gesendet.

## <a name="configuration-autotrackpagevisittime"></a>Konfiguration: autoTrackPageVisitTime

Wenn Sie `autoTrackPageVisitTime: true` festlegen, wird die Zeit nachverfolgt, die ein Benutzer auf jeder Seite verbringt. Bei jedem neuen Seitenaufruf wird die Zeitdauer, die der Benutzer auf der *vorherigen* Seite verbracht hat, als [benutzerdefinierte Metrik](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-custom-overview) mit dem Namen `PageVisitTime`gesendet. Diese benutzerdefinierte Metrik kann im [Metrik-Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) als eine „protokollbasierte Metrik“ angezeigt werden.

## <a name="react-extensions"></a>React-Erweiterungen

| Erweiterungen |
|---------------|
| [React](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [React Native](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Browser-/clientseitige Daten erkunden

Browser-/clientseitige Daten können angezeigt werden, indem Sie zu **Metriken** navigieren und einzelne Metriken hinzufügen, an denen Sie interessiert sind:

![](./media/javascript/page-view-load-time.png)

Sie können Ihre Daten auch aus dem JavaScript SDK über die Browseroberfläche im Portal anzeigen.

Wählen Sie **Browser** und dann **Fehler** oder **Leistung** aus.

![](./media/javascript/browser.png)

### <a name="performance"></a>Leistung

![](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Abhängigkeiten

![](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analytics

Wenn Sie Ihre vom JavaScript SDK gesammelten Telemetriedaten abfragen möchten, wählen Sie die Schaltfläche **In Protokollen anzeigen (Analytics)** aus. Wenn Sie eine `where`-Anweisung von `client_Type == "Browser"`hinzufügen, werden nur Daten aus dem JavaScript SDK angezeigt, und alle von anderen SDKs gesammelten serverseitigen Telemetriedaten werden ausgeschlossen.
 
```kusto
// average pageView duration by name
let timeGrain=5m;
let dataset=pageViews
// additional filters can be applied here
| where timestamp > ago(1d)
| where client_Type == "Browser" ;
// calculate average pageView duration for all pageViews
dataset
| summarize avg(duration) by bin(timestamp, timeGrain)
| extend pageView='Overall'
// render result in a chart
| render timechart
```

### <a name="source-map-support"></a>Unterstützung für die Quellzuordnungsdatei

Die Minimierung der minimierten Aufrufliste Ihrer Ausnahmetelemetriedaten kann im Azure-Portal nicht aufgehoben werden. Alle vorhandenen Integrationen im Bereich „Ausnahmedetails“ funktionieren aber bei der Aufrufliste, deren Minimierung gerade aufgehoben wurde.

#### <a name="link-to-blob-storage-account"></a>Verknüpfung mit Blob Storage-Konto

Sie können Ihre Application Insights-Ressource mit dem eigenen Azure Blob Storage-Container verknüpfen, um die Minimierung von Aufruflisten automatisch aufzuheben. Informationen zu den ersten Schritten finden Sie unter der [automatischen Unterstützung für Quellzuordnungen](./source-map-support.md).

### <a name="drag-and-drop"></a>Drag & Drop

1. Wählen Sie im Azure-Portal ein Element vom Typ „Ausnahmetelemetrie“ aus, um dessen „End-to-End-Transaktionsdetails“ anzuzeigen.
2. Identifizieren Sie, welche Quellzuordnungsdateien dieser Aufrufliste entsprechen. Die Quellzuordnungsdatei muss mit der Quelldatei eines Stapelrahmens übereinstimmen, die allerdings mit dem Suffix `.map` versehen ist.
3. Verschieben Sie die Quellzuordnungsdateien mit Drag & Drop in die Aufrufliste im Azure-Portal ![](https://i.imgur.com/Efue9nU.gif).

### <a name="application-insights-web-basic"></a>Application Insights Web Basic

Wenn Sie es einfacher haben möchten, können Sie stattdessen die Basisversion von Application Insights installieren.
```
npm i --save @microsoft/applicationinsights-web-basic
```
Diese Version ist mit der minimalen Anzahl von Features und Funktionen ausgestattet und basiert darauf, dass Sie sie nach Bedarf erstellen. So führt sie beispielsweise keine automatische Sammlung durch (nicht abgefangene Ausnahmen, AJAX usw.). Weil die APIs zum Senden bestimmter Arten von Telemetriedaten, wie z.B. `trackTrace`, `trackException` usw., in dieser Version nicht enthalten sind, müssen Sie Ihren eigenen Wrapper bereitstellen. Die einzige verfügbare API ist `track`. Ein [Beispiel](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) finden Sie hier.

## <a name="examples"></a>Beispiele

Ausführbare Beispiele finden Sie unter den [Beispielen für das Application Insights JavaScript SDK](https://github.com/topics/applicationinsights-js-demo).

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Upgrade von der alten Application Insights-Version

Breaking Changes in der SDK V2-Version:
- Um bessere API-Signaturen zu ermöglichen, wurden einige API-Aufrufe, wie z. B. „trackPageView“ und „trackException“, aktualisiert. Eine Ausführung in Internet Explorer 8 und früheren Versionen des Browsers wird nicht unterstützt.
- Beim Umschlag für die Telemetriedaten gibt es infolge von Datenschemaaktualisierungen Feldname- und Strukturänderungen.
- `context.operation` wurde in `context.telemetryTrace` verschoben. Einige Felder wurden ebenfalls geändert (`operation.id` --> `telemetryTrace.traceID`).
  - Zum manuellen Aktualisieren der derzeitigen PageView-ID (z. B. in SPA-Apps) verwenden Sie `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()`.
    > [!NOTE]
    > Damit die Ablaufverfolgungs-ID eindeutig bleibt, verwenden Sie dort, wo Sie zuvor `Util.newId()` verwendet haben, jetzt `Util.generateW3CId()`. Beide werden letztendlich zur Vorgangs-ID.

Wenn Sie das aktuelle Application Insights-PRODUKTIONS-SDK (1.0.20) verwenden und überprüfen möchten, ob das neue SDK in der Laufzeit funktioniert, aktualisieren Sie die URL abhängig von Ihrem aktuellen SDK-Ladeszenario.

- Download über CDN-Szenario: Aktualisieren Sie den zurzeit verwendeten Codeausschnitt so, dass er auf die folgende URL verweist:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- npm-Szenario: Rufen Sie `downloadAndSetup` auf, um das vollständige ApplicationInsights-Skript aus dem CDN herunterzuladen und es mit dem Instrumentierungsschlüssel zu initialisieren:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

Testen Sie es in der internen Umgebung, um zu überprüfen, ob die Überwachung der Telemetriedaten wie erwartet funktioniert. Wenn alles funktioniert, aktualisieren Sie Ihre API-Signaturen entsprechend der SDK V2-Version, und stellen Sie sie in Ihren Produktionsumgebungen bereit.

## <a name="sdk-performanceoverhead"></a>SDK-Leistung/Mehraufwand

Bei einer GZIP-Komprimierung von nur 25 KB und einer Initialisierung von nur ~15 ms fügt Application Insights Ihrer Website eine geringfügige Menge an Ladezeit hinzu. Wenn Sie den Codeausschnitt verwenden, werden minimale Komponenten der Bibliothek schnell geladen. In der Zwischenzeit wird das vollständige Skript im Hintergrund heruntergeladen.

Während des Downloads des Skripts aus dem CDN wird die gesamte Nachverfolgung Ihrer Seite in die Warteschlange eingereiht. Sobald die asynchrone Initialisierung des heruntergeladenen Skripts abgeschlossen ist, werden alle Ereignisse, die in die Warteschlange eingereiht wurden, nachverfolgt. Infolgedessen gehen während des gesamten Lebenszyklus Ihrer Seite keine Telemetriedaten verloren. Dieser Setupvorgang stellt für Ihre Seite ein nahtloses Analysesystem bereit, das für Ihre Benutzer unsichtbar ist.

> Zusammenfassung:
> - **25 KB** – mit GZIP komprimiert
> - **15 ms** – Gesamtinitialisierungszeit
> - **Null** – Nachverfolgung während des Lebenszyklus der Seite hat gefehlt.

## <a name="browser-support"></a>Browserunterstützung

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Chrome (neueste Version) ✔ |  Firefox (neueste Version) ✔ | Internet Explorer ab Version 9 und Microsoft Edge ✔ | Opera (neueste Version) ✔ | Safari (neueste Version) ✔ |

## <a name="open-source-sdk"></a>Open Source SDK

Das Application Insights JavaScript SDK ist Open Source. Wenn Sie den Quellcode anzeigen oder zum Projekt beitragen möchten, besuchen Sie das [offizielle GitHub-Repository](https://github.com/Microsoft/ApplicationInsights-JS).

## <a name="next-steps"></a><a name="next"></a> Nächste Schritte
* [Nutzung nachverfolgen](usage-overview.md)
* [Benutzerdefinierte Ereignisse und Metriken](api-custom-events-metrics.md)
* [Erstellen-Messen-Lernen](usage-overview.md)
