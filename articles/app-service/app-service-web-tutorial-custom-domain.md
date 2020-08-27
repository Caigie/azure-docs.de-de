---
title: 'Tutorial: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens'
description: Hier erfahren Sie, wie Sie einen vorhandenen benutzerdefinierten DNS-Domänennamen einer Web-App, einem Back-End einer mobilen App oder einer API-App in Azure App Service hinzufügen.
keywords: App Service, Azure App Service, Domänenzuordnung, Domänenname, vorhandene Domäne, Hostname
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/13/2020
ms.custom: mvc, seodec18
ms.openlocfilehash: 1496f46eb29831dfb858f061ccc00c9e3dbc2e75
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88782310"
---
# <a name="tutorial-map-an-existing-custom-dns-name-to-azure-app-service"></a>Tutorial: Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service

Von [Azure App Service](overview.md) wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt. In diesem Tutorial wird gezeigt, wie Azure App Service ein vorhandener benutzerdefinierter DNS-Name zugeordnet wird.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Zuordnen einer Unterdomäne (z.B. `www.contoso.com`) per CNAME-Eintrag
> * Zuordnen einer Stammdomäne (z.B. `contoso.com`) per A-Eintrag
> * Zuordnen einer Platzhalterdomäne (z.B. `*.contoso.com`) per CNAME-Eintrag
> * Umleiten der Standard-URL an ein benutzerdefiniertes Verzeichnis
> * Automatisieren der Domänenzuordnung mithilfe von Skripts

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Erstellen Sie eine App Service-App](/azure/app-service/), oder verwenden Sie eine App, die Sie für ein anderes Tutorial erstellt haben.
* Erwerben Sie einen Domänennamen, und stellen Sie sicher, dass Sie Zugriff auf die DNS-Registrierung für Ihren Domänenanbieter (z.B. GoDaddy) haben.

  Um beispielsweise DNS-Einträge für `contoso.com` und `www.contoso.com` hinzuzufügen, müssen Sie die DNS-Einstellungen für die Stammdomäne `contoso.com` konfigurieren können.

  > [!NOTE]
  > Falls Sie nicht über einen vorhandenen Domänennamen verfügen, können Sie erwägen, [über das Azure-Portal einen Domänennamen zu erwerben](manage-custom-dns-buy-domain.md).

## <a name="prepare-the-app"></a>Vorbereiten der App

Um einer Web-App einen benutzerdefinierten DNS-Namen zuzuordnen, muss der [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) der Web-App einen kostenpflichtigen Tarif (**Shared**, **Basic**, **Standard**, **Premium** oder **Consumption** für Azure Functions) aufweisen. In diesem Schritt stellen Sie sicher, dass sich die App Service-App innerhalb des unterstützten Tarifs befindet.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

### <a name="select-the-app-in-the-azure-portal"></a>Auswählen der App im Azure-Portal

Suchen Sie nach **App Services**, und wählen Sie diese Option aus.

![Auswählen von „App Services“](./media/app-service-web-tutorial-custom-domain/app-services.png)

Wählen Sie auf der Seite **App Services** den Namen Ihrer Azure-App aus.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

Die Verwaltungsseite der App Service-App wird angezeigt.  

<a name="checkpricing" aria-hidden="true"></a>

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

Scrollen Sie im linken Navigationsbereich der App-Seite zum Abschnitt **Einstellungen**, und wählen Sie **Hochskalieren (App Service-Plan)** .

![Menü „Zentral hochskalieren“](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Der aktuelle Tarif der App wird durch einen blauen Rahmen hervorgehoben. Vergewissern Sie sich, dass sich die App nicht im Tarif **F1** befindet. Benutzerdefiniertes DNS wird im Tarif **F1** nicht unterstützt. 

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Wenn sich der App Service-Plan nicht im Tarif **F1** befindet, schließen Sie die Seite **Hochskalieren**, und fahren Sie mit [Zuordnen eines CNAME-Eintrags](#cname) fort.

<a name="scaleup" aria-hidden="true"></a>

### <a name="scale-up-the-app-service-plan"></a>Hochskalieren des App Service-Plans

Wählen Sie einen der kostenpflichtigen Tarife aus (**D1**, **B1**, **B2**, **B3** oder einen beliebigen Tarif aus der Kategorie **Produktion**). Klicken Sie auf **Alle Optionen anzeigen**, um weitere Optionen anzuzeigen.

Klicken Sie auf **Anwenden**.

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wenn die unten angegebene Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Bestätigung des Skalierungsvorgangs](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname" aria-hidden="true"></a>

## <a name="get-domain-verification-id"></a>Abrufen der Verifizierungs-ID für eine Domäne

Wenn Sie Ihrer App eine benutzerdefinierte Domäne hinzufügen möchten, müssen Sie den Besitz der Domäne bestätigen, indem Sie bei Ihrem Domänenanbieter eine Verifizierungs-ID als TXT-Datensatz hinzufügen. Klicken Sie im linken Navigationsbereich Ihrer App-Seite unter auf **Benutzerdefinierte Domänen**. Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** den Wert unter **Verifizierungs-ID für benutzerdefinierte Domänen** für den nächsten Schritt.

![Abrufen der Verifizierungs-ID für benutzerdefinierte Domänen](./media/app-service-web-tutorial-custom-domain/get-custom-domain-verification-id.png)

> [!WARNING]
> Durch das Hinzufügen von Domänenverifizierungs-IDs zu Ihrer benutzerdefinierten Domäne können verwaiste DNS-Einträge und Unterdomänenübernahmen verhindert werden. Weitere Informationen zu dieser allgemeinen Bedrohung mit hohem Schweregrad finden Sie unter [Verhindern verwaister DNS-Einträge und Vermeiden von Unterdomänenübernahmen](../security/fundamentals/subdomain-takeover.md).

## <a name="map-your-domain"></a>Zuordnen Ihrer Domäne

Verwenden Sie für die Zuordnung eines benutzerdefinierten DNS-Namens zu App Service entweder einen **CNAME-Eintrag** oder einen **A-Eintrag**. Führen Sie die entsprechenden Schritte aus:

- [Zuordnen eines CNAME-Eintrags](#map-a-cname-record)
- [Zuordnen eines A-Eintrags](#map-an-a-record)
- [Zuordnen einer Platzhalterdomäne (mit einem CNAME-Eintrag)](#map-a-wildcard-domain)

> [!NOTE]
> Es wird empfohlen, CNAME-Einträge für alle benutzerdefinierten DNS-Namen zu verwenden, außer für Stammdomänen (z.B. `contoso.com`). Verwenden Sie A-Einträge für Stammdomänen.

### <a name="map-a-cname-record"></a>Zuordnen eines CNAME-Eintrags

Im Tutorialbeispiel fügen Sie einen CNAME-Eintrag für die Unterdomäne `www` (z.B. `www.contoso.com`) hinzu.

Falls Sie eine andere Unterdomäne als `www` verwenden, müssen Sie `www` durch Ihre Unterdomäne ersetzen (z. B. durch `sub`, wenn Ihre benutzerdefinierte Domäne `sub.constoso.com` lautet).

#### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

#### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Ordnen Sie dem Standarddomänennamen der App (`<app-name>.azurewebsites.net`, wobei `<app-name>` der Name Ihrer App ist) eine Unterdomäne zu. Erstellen Sie zwei Einträge, um eine CNAME-Zuordnung für die Unterdomäne `www` zu erstellen:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| CNAME | `www` | `<app-name>.azurewebsites.net` | Die Domänenzuordnung selbst |
| TXT | `asuid.www` | [Die zuvor abgerufene Verifizierungs-ID](#get-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid.<subdomain>` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. |

Nach dem Hinzufügen des CNAME- und des TXT-Eintrags sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record.png)

#### <a name="enable-the-cname-record-mapping-in-azure"></a>Aktivieren der Zuordnung von CNAME-Einträgen in Azure

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**.

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Fügen Sie auf der Seite **Benutzerdefinierte Domänen** der App der Liste den vollqualifizierten benutzerdefinierten DNS-Namen (`www.contoso.com`) hinzu.

Wählen Sie das Symbol **+** neben der Option **Benutzerdefinierte Domäne hinzufügen** aus.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie einen CNAME-Eintrag hinzugefügt haben, z.B. `www.contoso.com`.

Wählen Sie **Überprüfen** aus.

Die Seite **Benutzerdefinierte Domäne hinzufügen** wird angezeigt.

Stellen Sie sicher, dass der **Typ des Hostnamenseintrags** auf **CNAME (www\.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist.

Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

> [!NOTE]
> Die Bezeichnung **Nicht sicher** für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist und dass für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne abhängig vom Browser eine Warnung oder ein Fehler angezeigt wird. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service](configure-ssl-bindings.md).

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a" aria-hidden="true"></a>

### <a name="map-an-a-record"></a>Zuordnen eines A-Eintrags

Im Beispiel dieses Tutorials fügen Sie einen A-Eintrag für die Stammdomäne (z.B. `contoso.com`) hinzu.

<a name="info"></a>

#### <a name="copy-the-apps-ip-address"></a>Kopieren der IP-Adresse der App

Für die Zuordnung eines A-Eintrags benötigen Sie die externe IP-Adresse der App. Sie finden diese IP-Adresse im Azure-Portal auf der Seite **Benutzerdefinierte Domänen** der App.

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**.

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Kopieren Sie auf der Seite **Benutzerdefinierte Domänen** die IP-Adresse der App.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

#### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

#### <a name="create-the-a-record"></a>Erstellen des A-Eintrags

Erstellen Sie zwei Einträge, um einer App (in der Regel der Stammdomäne) einen A-Eintrag zuzuordnen:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| Ein | `@` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#info) | Die Domänenzuordnung selbst (`@` stellt in der Regel die Stammdomäne dar.) |
| TXT | `asuid` | [Die zuvor abgerufene Verifizierungs-ID](#get-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid.<subdomain>` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. Verwenden Sie für die Stammdomäne `asuid`. |

> [!NOTE]
> Wenn Sie zum Hinzufügen einer Unterdomäne (wie `www.contoso.com`) einen A-Eintrag anstelle eines empfohlenen [CNAME-Eintrags](#map-a-cname-record) verwenden möchten, sollten Ihr A-Eintrag und TXT-Eintrag stattdessen wie in der folgenden Tabelle aussehen:
>
> | Eintragstyp | Host | Wert |
> | - | - | - |
> | Ein | `www` | IP-Adresse aus dem Schritt [Kopieren der IP-Adresse der App](#info) |
> | TXT | `asuid.www` | `<app-name>.azurewebsites.net` |
>

Wenn die Einträge hinzugefügt werden, sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Seite der DNS-Einträge](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a" aria-hidden="true"></a>

#### <a name="enable-the-a-record-mapping-in-the-app"></a>Aktivieren der Zuordnung von A-Einträgen in der App

Fügen Sie auf der Seite **Benutzerdefinierte Domänen** der App im Azure-Portal den vollqualifizierten benutzerdefinierten DNS-Namen (z.B. `contoso.com`) der Liste hinzu.

Wählen Sie das Symbol **+** neben der Option **Benutzerdefinierte Domäne hinzufügen** aus.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie den vollqualifizierten Domänennamen ein, für den Sie den A-Eintrag konfiguriert haben, z.B. `contoso.com`.

Wählen Sie **Überprüfen** aus.

Die Seite **Benutzerdefinierte Domäne hinzufügen** wird angezeigt.

Stellen Sie sicher, dass die Option **Typ des Hostnamenseintrags** auf **A-Datensatz (beispiel.com)** festgelegt ist.

Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

![Hinzugefügter A-Eintrag](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

> [!NOTE]
> Die Bezeichnung **Nicht sicher** für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist und dass für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne abhängig vom Browser eine Warnung oder ein Fehler angezeigt wird. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service](configure-ssl-bindings.md).

Wenn Sie einen Schritt ausgelassen haben oder Ihnen zu einem früheren Zeitpunkt ein Tippfehler unterlaufen ist, wird am unteren Rand der Seite ein Überprüfungsfehler angezeigt.

![Überprüfungsfehler](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard" aria-hidden="true"></a>

### <a name="map-a-wildcard-domain"></a>Zuordnen einer Platzhalterdomäne

Im Beispiel des Tutorials ordnen Sie der App Service-App einen [DNS-Platzhalternamen](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (z.B. `*.contoso.com`) zu, indem Sie einen CNAME-Eintrag hinzufügen.

#### <a name="access-dns-records-with-domain-provider"></a>Zugreifen auf DNS-Einträge mit Domänenanbieter

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

#### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Ordnen Sie dem Standarddomänennamen der App (`<app-name>.azurewebsites.net`, wobei `<app-name>` der Name Ihrer App ist) den Platzhalternamen `*` zu. Erstellen Sie zwei Datensätze, um den Platzhalternamen zuzuordnen:

| Eintragstyp | Host | Wert | Kommentare |
| - | - | - |
| CNAME | `*` | `<app-name>.azurewebsites.net` | Die Domänenzuordnung selbst |
| TXT | `asuid` | [Die zuvor abgerufene Verifizierungs-ID](#get-domain-verification-id) | App Service greift auf den TXT-Eintrag `asuid` zu, um den Besitz der benutzerdefinierten Domäne zu überprüfen. |

Im Domänenbeispiel `*.contoso.com` ordnet der CNAME-Eintrag `<app-name>.azurewebsites.net` den Namen `*` zu.

Wenn der CNAME-Eintrag hinzugefügt wird, sieht die Seite mit den DNS-Einträgen wie im folgenden Beispiel aus:

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

#### <a name="enable-the-cname-record-mapping-in-the-app"></a>Aktivieren der Zuordnung von CNAME-Einträgen in der App

Sie können jetzt eine beliebige Unterdomäne hinzufügen, mit der der Platzhaltername der App zugeordnet wird (z. B. `sub1.contoso.com`, `sub2.contoso.com` und `*.contoso.com` zu `*.contoso.com`).

Wählen Sie im linken Navigationsbereich der App-Seite im Azure-Portal die Option **Benutzerdefinierte Domänen**.

![Menü „Benutzerdefinierte Domänen“](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Wählen Sie das Symbol **+** neben der Option **Benutzerdefinierte Domäne hinzufügen** aus.

![Hinzufügen des Hostnamens](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Geben Sie einen vollqualifizierten Domänennamen ein, der zur Platzhalterdomäne passt (z.B. `sub1.contoso.com`), und wählen Sie anschließend **Überprüfen**.

Die Schaltfläche **Benutzerdefinierte Domäne hinzufügen** ist aktiviert.

Stellen Sie sicher, dass der **Typ des Hostnamenseintrags** auf **CNAME-Eintrag (www\.beispiel.com oder eine beliebige Unterdomäne)** festgelegt ist.

Wählen Sie **Benutzerdefinierte Domäne hinzufügen**.

![Hinzufügen des DNS-Namens zur App](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Unter Umständen dauert es eine Weile, bis die neue benutzerdefinierte Domäne auf der Seite **Benutzerdefinierte Domänen** der App angezeigt wird. Aktualisieren Sie den Browser, um die Daten zu aktualisieren.

Wählen Sie das Symbol **+** erneut aus, um eine andere benutzerdefinierte Domäne hinzuzufügen, die der Platzhalterdomäne entspricht. Fügen Sie beispielsweise `sub2.contoso.com` hinzu.

![Hinzugefügter CNAME-Eintrag](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

> [!NOTE]
> Die Bezeichnung **Nicht sicher** für Ihre benutzerdefinierte Domäne bedeutet, dass diese noch nicht an ein TLS-/SSL-Zertifikat gebunden ist und dass für alle HTTPS-Anforderungen von einem Browser an Ihre benutzerdefinierte Domäne abhängig vom Browser eine Warnung oder ein Fehler angezeigt wird. Weitere Informationen zum Hinzufügen einer TLS-/SSL-Bindung finden Sie unter [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service](configure-ssl-bindings.md).

## <a name="test-in-browser"></a>Testen im Browser

Navigieren Sie zu den DNS-Namen, die Sie zuvor konfiguriert haben, z.B. `contoso.com`, `www.contoso.com`, `sub1.contoso.com` und `sub2.contoso.com`.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="resolve-404-not-found"></a>Beheben von 404 „Nicht gefunden“

Falls Sie zu der URL Ihrer benutzerdefinierten Domäne navigieren und dabei einen HTTP-Fehler vom Typ 404 (nicht gefunden) erhalten, vergewissern Sie sich mithilfe von <a href="https://www.whatsmydns.net/" target="_blank">WhatsmyDNS.net</a>, dass Ihre Domäne zur IP-Adresse Ihrer App aufgelöst wird. Ist dies nicht der Fall, liegt möglicherweise eine der folgenden Ursachen vor:

- Der konfigurierten benutzerdefinierten Domäne fehlt ein A-Eintrag und/oder ein CNAME-Eintrag.
- Im Browserclient ist die alte IP-Adresse Ihrer Domäne zwischengespeichert. Leeren Sie den Cache, und testen Sie die DNS-Auflösung erneut. Auf einem Windows-Computer können Sie den Cache mithilfe von `ipconfig /flushdns` leeren.

<a name="virtualdir" aria-hidden="true"></a>

## <a name="migrate-an-active-domain"></a>Migrieren einer aktiven Domäne

Informationen zum Migrieren einer Livewebsite und ihres DNS-Domänennamens zu App Service ohne Ausfallzeiten finden Sie unter [Migrieren einer aktiven benutzerdefinierten Domäne zu Azure App Service](manage-custom-dns-migrate-domain.md).

## <a name="redirect-to-a-custom-directory"></a>Umleitung zu einem benutzerdefinierten Verzeichnis

App Service leitet Webanforderungen standardmäßig an das Stammverzeichnis des App-Codes weiter. Manche Webframeworks starten jedoch nicht im Stammverzeichnis. [Laravel](https://laravel.com/) startet beispielsweise im Unterverzeichnis `public`. Um das DNS-Beispiel für `contoso.com` fortzusetzen: Auf eine solche App kann zwar unter `http://contoso.com/public` zugegriffen werden, es empfiehlt sich aber, `http://contoso.com` stattdessen an das Verzeichnis `public` weiterzuleiten. Dieser Schritt beinhaltet keine DNS-Auflösung, erfordert aber die Anpassung des virtuellen Verzeichnisses.

Klicken Sie hierzu im linken Navigationsbereich Ihrer Web-App-Seite auf **Anwendungseinstellungen**. 

Das virtuelle Stammverzeichnis `/` am unteren Seitenrand verweist standardmäßig auf `site\wwwroot` (also auf das Stammverzeichnis Ihres App-Codes). Ändern Sie die Angabe, um stattdessen beispielsweise auf `site\wwwroot\public` zu verweisen, und speichern Sie die Änderung.

![Anpassen des virtuellen Verzeichnisses](./media/app-service-web-tutorial-custom-domain/customize-virtual-directory.png)

Nach Abschluss des Vorgangs sollte Ihre App die korrekte Seite unter dem Stammpfad zurückgeben (z. B. `http://contoso.com`).

## <a name="automate-with-scripts"></a>Automatisieren mit Skripts

Durch die [Azure CLI](/cli/azure/install-azure-cli) oder durch [Azure PowerShell](/powershell/azure/) können Sie mithilfe von Skripts die Verwaltung von benutzerdefinierten Domänen automatisieren. 

### <a name="azure-cli"></a>Azure CLI 

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt. 

```bash 
az webapp config hostname add \
    --webapp-name <app-name> \
    --resource-group <resource_group_name> \
    --hostname <fully_qualified_domain_name>
``` 

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/cli-configure-custom-domain.md).

### <a name="azure-powershell"></a>Azure PowerShell 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Mit dem folgenden Befehl wird ein konfigurierter benutzerdefinierter DNS-Name zu einer App Service-App hinzugefügt.

```powershell  
Set-AzWebApp `
    -Name <app-name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app-name>.azurewebsites.net")
```

Weitere Informationen finden Sie unter [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](scripts/powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Zuordnen einer Unterdomäne mit einem CNAME-Eintrag
> * Zuordnen einer Stammdomäne mit einem A-Eintrag
> * Zuordnen einer Platzhalterdomäne mit einem CNAME-Eintrag
> * Umleiten der Standard-URL an ein benutzerdefiniertes Verzeichnis
> * Automatisieren der Domänenzuordnung mithilfe von Skripts

Fahren Sie mit dem nächsten Tutorial fort, um sich über das Anbinden eines benutzerdefinierten TLS-/SSL-Zertifikats an eine Web-App zu informieren.

> [!div class="nextstepaction"]
> [Schützen eines benutzerdefinierten DNS-Namens mit einer TLS-/SSL-Bindung in Azure App Service](configure-ssl-bindings.md)
