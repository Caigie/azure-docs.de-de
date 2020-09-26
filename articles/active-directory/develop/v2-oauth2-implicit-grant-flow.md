---
title: OAuth 2.0-Flow für die implizite Genehmigung – Microsoft Identity Platform | Azure
description: Schützen Sie Single-Page-Apps mit dem impliziten Fluss von Microsoft Identity Platform.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: fbe74b62352babf7a1fdd93bf19a6e1475e3f032
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85553579"
---
# <a name="microsoft-identity-platform-and-implicit-grant-flow"></a>Microsoft Identity Platform und der implizit gewährte Datenfluss

Mit dem Microsoft Identity Platform-Endpunkt können Sie Benutzer sowohl mit persönlichen Konten als auch mit Geschäfts-, Schul- oder Unikonten von Microsoft bei Single-Page-Apps anmelden. Bei Single-Page-Apps und anderen JavaScript-Apps, die hauptsächlich im Browser ausgeführt werden, gibt es in Bezug auf die Authentifizierung einige interessante Herausforderungen:

* Die Sicherheitsmerkmale dieser Apps unterscheiden sich grundlegend von herkömmlichen serverbasierten Webanwendungen.
* Zahlreiche Autorisierungsserver und Identitätsanbieter unterstützen keine CORS-Anforderungen.
* Umleitungen auf ganzseitige Browserseiten stören die Benutzererfahrung erheblich.

Bei diesen Anwendungen (Angular, Ember.js, React.js usw.) unterstützt Microsoft Identity Platform den impliziten OAuth 2.0-Genehmigungsablauf. Der implizite Fluss wird in der [OAuth 2.0-Spezifikation](https://tools.ietf.org/html/rfc6749#section-4.2) beschrieben. Der größte Vorteil besteht darin, dass die App Token aus Microsoft Identity Platform abrufen kann, ohne dass die Anmeldeinformationen für den Back-End-Server ausgetauscht werden müssen. Dadurch kann die App den Benutzer anmelden, die Sitzung aufrechterhalten und Token für andere Web-APIs abrufen. All dies geschieht innerhalb des Client-JavaScript-Codes. Bei der Verwendung des impliziten Flusses gibt es einige wichtige Sicherheitsaspekte zu beachten, insbesondere in Bezug auf [Clients](https://tools.ietf.org/html/rfc6749#section-10.3) und [Identitätswechsel](https://tools.ietf.org/html/rfc6749#section-10.3).

In diesem Artikel wird beschrieben, wie Sie direkt mit dem Protokoll in Ihrer Anwendung programmieren.  Es wird stattdessen empfohlen, ggf. die unterstützten Microsoft Authentication Libraries (MSAL) zu verwenden, um [Token zu erhalten und gesicherte Web-APIs aufzurufen](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows).  Sehen Sie sich auch die [Beispiel-Apps an, die MSAL verwenden](sample-v2-code.md).

Wenn Sie jedoch in Ihrer Single-Page-App keine Bibliothek verwenden und Protokollmeldungen selbst senden möchten, führen Sie die folgenden allgemeinen Schritte aus.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geeignete Szenarien für die implizite OAuth2-Gewährung

Gemäß Deklaration der OAuth2-Spezifikation wurde die implizite Gewährung entwickelt, um die Verwendung von Benutzer-Agent-Anwendungen (JavaScript-Anwendungen, die in einem Browser ausgeführt werden) zu ermöglichen. Solche Anwendungen zeichnen sich insbesondere dadurch aus, dass für den Zugriff auf Serverressourcen (üblicherweise eine Web-API) und für die entsprechende Aktualisierung der Benutzeroberfläche der Anwendung JavaScript-Code verwendet wird. Denken Sie dabei z.B. an Anwendungen wie Gmail oder Outlook Web Access: Wenn Sie eine Nachricht aus Ihrem Posteingang auswählen, ändert sich nur der Bereich, in dem die Nachricht dargestellt wird, während der Rest der Seite unverändert bleibt. Im Gegensatz dazu führt bei konventionellen, auf Umleitungen basierenden Web-Apps jede Benutzerinteraktion zu einem ganzseitigen Postback sowie zum ganzseitigen Rendern der neuen Serverantwort.

Extrembeispiele für Anwendungen mit der JavaScript-basierten Methode sind die sogenannten Single-Page-Webanwendungen (Single-Page Applications, SPAs). Diese Anwendungen sind nur für die Versorgung einer HTML-Startseite und des dazugehörigen JavaScript-Codes konzipiert. Alle nachfolgenden Interaktionen werden mithilfe von Web-API-Aufrufen über JavaScript ausgeführt. Hybride Methoden, bei denen Anwendungen größtenteils Postback-orientiert sind, aber auch einzelne JS-Aufrufe ausführen, sind allerdings nichts Ungewöhnliches, und die Informationen zur Verwendung impliziter Abläufe sind auch für diese Methoden relevant.

Auf Umleitungen basierende Anwendungen schützen ihre Anforderungen in der Regel mithilfe von Cookies. Bei JavaScript-Anwendungen funktioniert diese Methode jedoch nicht so gut. Cookies können nur für die Domäne verwendet werden, für die sie erstellt wurden. JavaScript-Aufrufe können dagegen an andere Domänen weitergeleitet werden. Das kommt in der Praxis auch recht häufig vor – etwa bei Anwendungen, die die Microsoft Graph-API, Office-API oder Azure-API aufrufen: Diese befinden sich alle außerhalb der Domäne, von der aus die Anwendung bereitgestellt wird. Bei JavaScript-Anwendungen geht der Trend zunehmend dahin, dass kein Back-End vorhanden ist und sie zur Implementierung der Geschäftsfunktion zu 100 % auf Web-APIs von Drittanbietern basieren.

Aktuell wird zum Schutz von Web-API-Aufrufen die Verwendung der OAuth2-Bearertoken-Methode empfohlen. Bei dieser Methode wird jeder Aufruf von einem OAuth2-Zugriffstoken begleitet. Die Web-API untersucht das eingehende Zugriffstoken und gewährt dem angeforderten Vorgang Zugriff, wenn das Token in den erforderlichen Bereichen gefunden wurde. Der implizite Ablauf ist eine praktische Methode, mit der JavaScript-Anwendungen Zugriffstoken für eine Web-API beziehen können. Gegenüber Cookies bietet er diverse Vorteile:

* Token können zuverlässig abgerufen werden, ohne dass ursprungsübergreifende Aufrufe erforderlich sind. Die obligatorische Registrierung des Umleitungs-URI, an den Token zurückgegeben werden, stellt sicher, dass Token nicht verschoben werden.
* JavaScript-Anwendungen können beliebig viele Zugriffstoken für beliebig viele Web-APIs beziehen. Hierbei gelten keine Domänenbeschränkungen.
* HTML5-Features wie Sitzungen oder lokale Speicher ermöglichen es, das Zwischenspeichern von Token und die Lebensdauerverwalltung vollständig zu steuern, wobei die Verwaltung von Cookies für die App nicht transparent ist.
* Zugriffstoken sind für websiteübergreifende Anforderungsfälschungen (CSRF) nicht anfällig.

Bei der impliziten Gewährung werden insbesondere aus Sicherheitsgründen keine Aktualisierungstoken ausgegeben. Ein Aktualisierungstoken ist weniger eng umgrenzt als ein Zugriffstoken und gewährt weitaus mehr Rechte, weshalb der Schaden bei Verlust auch größer sein kann. Beim impliziten Ablauf werden Token in der URL übermittelt, weshalb die Gefahr eines Abfangens höher ist als bei der Gewährung über einen Autorisierungscode.

JavaScript-Anwendungen verfügen jedoch über andere Möglichkeiten, um Zugriffstoken zu erneuern, bei denen der Benutzer nicht wiederholt zur Eingabe der Anmeldeinformationen aufgefordert wird. Zum Ausführen neuer Tokenanforderungen in Bezug auf den Autorisierungsendpunkt von Azure AD kann die Anwendung ein ausgeblendetes iFrame verwenden: Wenn der Browser über eine aktive Sitzung mit der Azure AD-Domäne (d.h. über ein Sitzungscookie) verfügt, kann die Authentifizierungsanforderung ausgeführt werden, ohne dass eine Benutzerinteraktion erforderlich ist.

Bei diesem Modell kann die JavaScript-Anwendung Zugriffstoken unabhängig erneuern und sogar neue Token für eine neue API beziehen (sofern der Benutzer dem vorher zugestimmt hat). Dadurch entfällt das zusätzliche Beziehen, Verwalten und Schützen eines hochwertigen Artefakts (etwa ein Aktualisierungstoken). Das Artefakt, das die Verlängerung im Hintergrund ermöglicht (das Azure AD-Sitzungscookie), wird außerhalb der Anwendung verwaltet. Ein weiterer Vorteil dieser Methode: Ein Benutzer kann sich über eine beliebige, bei Azure AD angemeldete Anwendung, die auf einer beliebigen Browserregisterkarte ausgeführt wird, von Azure AD abmelden. Daraufhin wird das Azure AD-Sitzungscookie gelöscht, sodass die JavaScript-Anwendung für den abgemeldeten Benutzer keine Token mehr erneuern kann.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Eignet sich die implizite Gewährung für meine App?

Die implizite Genehmigung stellt ein höheres Risiko dar als andere Genehmigungen, die besonders zu beachtenden Bereiche sind jedoch gut dokumentiert (z. B. unter [Missbräuchliches Verwenden von Zugriffstoken zum Annehmen der Identität des Ressourcenbesitzers im impliziten Flow][OAuth2-Spec-Implicit-Misuse] und [Überlegungen zum OAuth 2.0-Bedrohungsmodell und zur Sicherheit][OAuth2-Threat-Model-And-Security-Implications]). Das höhere Risiko liegt größtenteils darin begründet, dass Anwendungen aktiviert werden, die aktiven Code ausführen, der von einer Remoteressource direkt für einen Browser bereitgestellt wird. Wenn Sie eine SPA-Architektur planen, über keine Back-End-Komponenten verfügen oder eine Web-API über JavaScript aufrufen möchten, empfiehlt sich die Verwendung des impliziten Ablaufs für den Tokenabruf.

Falls es sich bei Ihrer Anwendung dagegen um einen nativen Client handelt, ist der implizite Ablauf weniger empfehlenswert. Da bei nativen Clients keine Azure AD-Sitzungscookies vorhanden sind, hat Ihre Anwendung keine Möglichkeit, eine Sitzung mit langer Laufzeit aufrechtzuerhalten. Das bedeutet, dass Ihre Anwendung beim Abrufen von Zugriffstoken für neue Ressourcen immer wieder Eingabeaufforderung an den Benutzer richtet.

Auch wenn Sie eine Webanwendung mit einem Back-End entwickeln, die eine API über den Back-End-Code nutzen soll, ist der implizite Ablauf nicht die beste Wahl. Bei anderen Methoden haben Sie deutlich mehr Steuerungsmöglichkeiten. Bei der OAuth2-Gewährung mit Clientanmeldeinformationen haben Sie beispielsweise die Möglichkeit, Token zu beziehen, die die Berechtigungen für die Anwendung widerspiegeln (im Gegensatz zu Benutzerdelegierungen). Das bedeutet unter anderem, dass der Client den programmgesteuerten Zugriff auf Ressourcen verwalten kann, auch wenn ein Benutzer nicht aktiv in eine Sitzung eingebunden ist. Darüber hinaus gewährleisten Gewährungen dieser Art eine höhere Sicherheit. So durchlaufen Zugriffstoken etwa niemals den Browser des Benutzers und können dadurch gar nicht erst im Browserverlauf gespeichert werden. Die Clientanwendung kann außerdem beim Anfordern eines Tokens eine strenge Authentifizierung durchführen.

[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

## <a name="protocol-diagram"></a>Protokolldiagramm

Das folgende Diagramm zeigt, wie der gesamte implizite Anmeldevorgang aussieht, und in den folgenden Abschnitten wird jeder Schritt im Detail beschrieben.

![Diagramm: Impliziter Anmeldevorgang](./media/v2-oauth2-implicit-grant-flow/convergence-scenarios-implicit.svg)

## <a name="send-the-sign-in-request"></a>Senden der Anmeldeanforderung

Zur anfänglichen Anmeldung des Benutzers bei Ihrer App können Sie eine [OpenID Connect](v2-protocols-oidc.md)-Authentifizierungsanforderung senden und ein `id_token` vom Microsoft Identity Platform-Endpunkt abrufen.

> [!IMPORTANT]
> Um ein ID-Token und/oder ein Zugriffstoken erfolgreich anfordern zu können, muss für die App-Registrierung im [Azure-Portal auf der Seite „App-Registrierungen“](https://go.microsoft.com/fwlink/?linkid=2083908) der entsprechende Flow zur impliziten Genehmigung aktiviert sein. Zu diesem Zweck wählen Sie im Abschnitt **Implizite Genehmigung** die Option **ID-Token** und/oder **Zugriffstoken** aus. Wenn sie nicht aktiviert ist, wird ein Fehler des Typs `unsupported_response` zurückgegeben: **The provided value for the input parameter ‚response_type‘ is not allowed for this client. Expected value is ‚code‘.** (Der angegebene Wert für den Eingabeparameter ‚response_type‘ ist für diesen Client nicht zulässig. Erwarteter Wert: ‚code‘.)

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Klicken Sie zum Testen der Anmeldung mit dem impliziten Fluss auf <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a> Nach der Anmeldung sollte der Browser mit einem `id_token` in der Adressleiste zu `https://localhost/myapp/` umgeleitet werden.
>

| Parameter | type | BESCHREIBUNG |
| --- | --- | --- |
| `tenant` | required |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner. Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| `client_id` | required | Die Anwendungs-ID (Client-ID), die Ihrer App im [Azure-Portal auf der Seite „App-Registrierungen“](https://go.microsoft.com/fwlink/?linkid=2083908) zugewiesen wurde. |
| `response_type` | required |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten. Kann auch den Antworttyp `token` enthalten. Mithilfe von `token` kann Ihre App ein Zugriffstoken direkt vom Autorisierungsendpunkt abrufen, ohne dass eine zweite Anforderung an den Autorisierungsendpunkt erforderlich ist. Wenn Sie den Antworttyp `token` verwenden, muss der `scope`-Parameter einen Bereich enthalten, der angibt, für welche Ressource das Token ausgestellt wird (z. B. „user.read“ in Microsoft Graph).  |
| `redirect_uri` | empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| `scope` | required |Eine durch Leerzeichen getrennte Liste von [Bereichen](v2-permissions-and-consent.md). Für OpenID Connect (id_tokens) muss der Bereich `openid` enthalten sein, der auf der Zustimmungsbenutzeroberfläche die Berechtigung „Anmeldung in Ihrem Namen“ ergibt. Optional können Sie auch die Bereiche `email` und `profile` einschließen, um Zugriff auf zusätzliche Benutzerdaten zu erhalten. Sie können in diese Anforderung auch andere Bereiche aufnehmen, um die Zustimmung für verschiedene Ressourcen anzufordern, wenn ein Zugriffstoken angefordert wird. |
| `response_mode` | optional |Gibt die Methode an, die zum Senden des resultierenden Tokens zurück an Ihre App verwendet werden soll. Der Standardwert nur für ein Zugriffstoken ist „query“, aber „fragment“, wenn die Anforderung ein „id_token“ enthält. |
| `state` | empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](https://tools.ietf.org/html/rfc6749#section-10.12). Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| `nonce` | required |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten sein wird. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. Nur erforderlich, wenn ein „id_token“ angefordert wird. |
| `prompt` | optional |Gibt den Typ der erforderlichen Benutzerinteraktion an. Zu diesem Zeitpunkt sind die einzigen gültigen Werte „login“, „none“, „select_account“ und „consent“. `prompt=login` zwingt den Benutzer, die Anmeldeinformationen bei dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich. `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird. Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der Microsoft Identity Platform-Endpunkt einen Fehler zurück. `prompt=select_account` sendet den Benutzer an eine Kontoauswahl, in der alle in der Sitzung gespeicherten Konten angezeigt werden. `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| `login_hint`  |optional |Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, nachdem sie den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahiert haben.|
| `domain_hint` | optional |Wenn dieser Parameter vorhanden ist, wird der E-Mail-basierte Ermittlungsvorgang übersprungen, den der Benutzer auf der Anmeldeseite durchläuft, und so die Benutzerfreundlichkeit verbessert. Dieser Parameter wird häufig bei branchenspezifischen Apps verwendet, die in einem einzigen Mandanten ausgeführt werden, in dem sie einen Domänennamen innerhalb eines bestimmten Mandanten bereitstellen.  Dadurch wird der Benutzer an den Verbundanbieter für diesen Mandanten weitergeleitet.  Beachten Sie, dass sich Gäste dann nicht bei dieser Anwendung anmelden können.  |

Zu diesem Zeitpunkt wird der Benutzer dazu aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen. Der Microsoft Identity Platform-Endpunkt stellt auch sicher, dass der Benutzer den Berechtigungen zugestimmt hat, die im `scope`-Abfrageparameter angegeben sind. Wenn der Benutzer **keiner** Berechtigung zugestimmt hat, wird er dazu aufgefordert, den erforderlichen Berechtigungen zuzustimmen. Weitere Informationen finden Sie unter [Berechtigungen, Zustimmung und mehrinstanzenfähigen Apps](v2-permissions-and-consent.md).

Sobald der Benutzer authentifiziert ist und seine Zustimmung erteilt hat, gibt der Microsoft Identity Platform-Endpunkt mithilfe der Methode im festgelegten `response_mode`-Parameter eine Antwort auf dem angegebenen `redirect_uri` an Ihre App zurück.

#### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort mithilfe von `response_mode=fragment` und `response_type=id_token+token` sieht wie folgt aus, wobei die Zeilenumbrüche der Lesbarkeit dienen:

```HTTP
GET https://localhost/myapp/#
&token_type=Bearer
&expires_in=3599
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | BESCHREIBUNG |
| --- | --- |
| `access_token` |Ist enthalten, wenn `token` in `response_type` enthalten ist. Das von der App angeforderte Zugriffstoken. Das Zugriffstoken sollte nicht decodiert oder anderweitig untersucht werden, es sollte als nicht transparente Zeichenfolge behandelt werden. |
| `token_type` |Ist enthalten, wenn `token` in `response_type` enthalten ist. Ist immer `Bearer`. |
| `expires_in`|Ist enthalten, wenn `token` in `response_type` enthalten ist. Gibt für die Zwischenspeicherung den Gültigkeitszeitraum des Tokens in Sekunden an. |
| `scope` |Ist enthalten, wenn `token` in `response_type` enthalten ist. Gibt die Bereiche an, für die das Zugriffstoken gültig ist. Umfasst eventuell nicht alle angeforderten Bereiche, wenn sie nicht auf den Benutzer anwendbar sind (im Fall von reinen Azure AD-Bereichen, die angefordert werden, wenn ein persönliches Konto für die Anmeldung verwendet wird). |
| `id_token` | Ein signiertes JSON Web Token (JWT). Die App kann die Segmente dieses Tokens decodieren, um Informationen zum angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich in Bezug auf Autorisierungs- und Sicherheitsgrenzen aber nicht darauf verlassen. Weitere Informationen zu ID-Token finden Sie unter [`id_token reference`](id-tokens.md). <br> **Hinweis:** Wird nur bei Anforderung des `openid`-Bereichs bereitgestellt. |
| `state` |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### <a name="error-response"></a>Fehlerantwort

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```HTTP
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | BESCHREIBUNG |
| --- | --- |
| `error` |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| `error_description` |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

## <a name="getting-access-tokens-silently-in-the-background"></a>Automatisches Abrufen von Zugriffstoken im Hintergrund

Nachdem Sie den Benutzer bei der Single-Page-App angemeldet haben, können Sie Zugriffstoken zum Aufrufen der von der Microsoft Identity Platform gesicherten Web-APIs automatisch abrufen, z. B. [Microsoft Graph](https://developer.microsoft.com/graph). Auch wenn Sie mithilfe des Antworttyps `token` bereits ein Token erhalten haben, können Sie diese Methode zum Abrufen von Token für zusätzliche Ressourcen verwenden, ohne den Benutzer zur erneuten Anmeldung umzuleiten.

Im herkömmlichen OpenID Connect/OAuth-Fluss senden Sie dazu eine Anforderung an den Microsoft Identity Platform-Endpunkt `/token`. Der Microsoft Identity Platform-Endpunkt unterstützt jedoch keine CORS-Anforderungen, daher kommen AJAX-Aufrufe zum Abrufen und Aktualisieren von Token nicht infrage. Stattdessen können Sie den impliziten Fluss in einem ausgeblendeten IFrame verwenden, um neue Token für andere Web-APIs zu erhalten:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&response_mode=fragment
&state=12345
&nonce=678910
&prompt=none
&login_hint=myuser@mycompany.com
```

Weitere Informationen zu den Abfrageparametern in der URL finden Sie unter [Senden der Anmeldeanforderung](#send-the-sign-in-request).

> [!TIP]
> Kopieren und fügen Sie die folgende Anforderung in eine Browserregisterkarte ein. (Vergessen Sie dabei nicht, den Wert `login_hint` durch den richtigen Wert für den Benutzer zu ersetzen.)
>
>`https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&login_hint={your-username}`
>

Dank des Parameters `prompt=none` ist diese Anforderung entweder erfolgreich oder sie schlägt direkt fehl und kehrt zu Ihrer Anwendung zurück. Eine erfolgreiche Antwort wird an Ihre App an den angegebenen Umleitungs-URI (`redirect_uri`) gesendet. Dabei wird die im Parameter `response_mode` angegebene Methode verwendet.

#### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort mit `response_mode=fragment` sieht wie folgt aus:

```HTTP
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fdirectory.read
```

| Parameter | BESCHREIBUNG |
| --- | --- |
| `access_token` |Ist enthalten, wenn `token` in `response_type` enthalten ist. Das von der Anwendung angeforderte Zugriffstoken, in diesem Fall für Microsoft Graph. Das Zugriffstoken sollte nicht decodiert oder anderweitig untersucht werden, es sollte als nicht transparente Zeichenfolge behandelt werden. |
| `token_type` | Ist immer `Bearer`. |
| `expires_in` | Gibt für die Zwischenspeicherung den Gültigkeitszeitraum des Tokens in Sekunden an. |
| `scope` | Gibt die Bereiche an, für die das Zugriffstoken gültig ist. Umfasst eventuell nicht alle angeforderten Bereiche, wenn sie nicht auf den Benutzer anwendbar sind (im Fall von reinen Azure AD-Bereichen, die angefordert werden, wenn ein persönliches Konto für die Anmeldung verwendet wird). |
| `id_token` | Ein signiertes JSON Web Token (JWT). Ist enthalten, wenn `id_token` in `response_type` enthalten ist. Die App kann die Segmente dieses Tokens decodieren, um Informationen zum angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich in Bezug auf Autorisierungs- und Sicherheitsgrenzen aber nicht darauf verlassen. Weitere Informationen zu ID-Token (id_token) finden Sie in der [`id_token`-Referenz](id-tokens.md). <br> **Hinweis:** Wird nur bei Anforderung des `openid`-Bereichs bereitgestellt. |
| `state` |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### <a name="error-response"></a>Fehlerantwort

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann: Im Fall von `prompt=none` wird folgender Fehler erwartet:

```HTTP
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | BESCHREIBUNG |
| --- | --- |
| `error` |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| `error_description` |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Wenn Sie diesen Fehler in der IFrame-Anforderung erhalten, muss sich der Benutzer erneut anmelden, um ein neues Token abzurufen. Diesen Fall können Sie so behandeln, wie es für Ihre Anwendung am sinnvollsten ist.

## <a name="refreshing-tokens"></a>Aktualisieren von Token

Die implizite Gewährung stellt keine Aktualisierungstoken bereit. `id_token`s und `access_token`s laufen nach kurzer Zeit ab. Ihre App muss daher diese Token in regelmäßigen Abständen aktualisieren. Zum Aktualisieren beider Tokentypen können Sie die oben erwähnte verborgene IFrame-Anforderung unter Verwendung des Parameters `prompt=none` ausführen, um das Verhalten der Microsoft Identity Platform zu steuern. Wenn Sie ein neues `id_token` erhalten möchten, verwenden Sie unbedingt `id_token` in `response_type` und `scope=openid` sowie einen `nonce`-Parameter.

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldungsanforderung

Die OpenID Connect `end_session_endpoint` ermöglicht Ihrer App das Senden einer Anforderung an den Microsoft Identity Platform-Endpunkt zum Beenden der Sitzung eines Benutzers und zum Löschen von Cookies, die vom Microsoft Identity Platform-Endpunkt festgelegt wurden. Um einen Benutzer vollständig von einer Webanwendung abzumelden, muss Ihre App ihre eigene Sitzung mit dem Benutzer beenden (in der Regel durch Löschen eines Tokencaches oder von Cookies) und dann den Browser zu folgender Adresse umleiten:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parameter | type | BESCHREIBUNG |
| --- | --- | --- |
| `tenant` |required |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner. Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| `post_logout_redirect_uri` | empfohlen | Die URL, zu der der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll. Dieser Wert muss einem der Umleitung-URIs entsprechen, die für die Anwendung registriert sind. Wenn keine Angabe erfolgt, wird dem Benutzer vom Microsoft Identity Platform-Endpunkt eine allgemeine Meldung angezeigt. |

## <a name="next-steps"></a>Nächste Schritte

* Wechseln Sie zu den [MSAL JS-Beispielen](sample-v2-code.md), um in die Programmierung einzusteigen.

[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
