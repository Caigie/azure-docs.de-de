---
title: 'Verwenden von Erstellungs- und Laufzeitschlüsseln: LUIS'
description: Wenn Sie Language Understanding (LUIS) zum ersten Mal verwenden, müssen Sie keinen Erstellungsschlüssel erstellen. Wenn Sie die App veröffentlichen möchten und dann Ihren Laufzeitendpunkt verwenden, müssen Sie den Laufzeitschlüssel erstellen und ihn der App zuweisen.
services: cognitive-services
ms.topic: how-to
ms.date: 04/06/2020
ms.openlocfilehash: c566e8fe56d19856f5a577e472929b7610497d7c
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344457"
---
# <a name="create-luis-resources"></a>Erstellen von LUIS-Ressourcen

Erstellungs- und Laufzeitressourcen stellen Authentifizierung für Ihre LUIS-App und ihren Vorhersageendpunkt bereit.

<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>

Wenn Sie sich beim LUIS-Portal anmelden, können Sie wählen, wie Sie Ihre Arbeit fortsetzen möchten:

* mit einem kostenlosen [Testschlüssel](#trial-key): Es werden Erstellung und einige Vorhersageendpunktabfragen bereitgestellt.
* eine Azure-Ressource für die [LUIS-Erstellung](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne).

<a name="starter-key"></a>

## <a name="sign-in-to-luis-portal-and-begin-authoring"></a>Anmelden beim LUIS-Portal und Beginnen mit der Erstellung

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und stimmen Sie den Nutzungsbedingungen zu.
1. Beginnen Sie das Schreiben Ihrer LUIS-APP, indem Sie den Typ des LUIS-Erstellungsschlüssels auswählen, den Sie verwenden möchten: kostenloser Testschlüssel oder neuer Azure-LUIS-Erstellungsschlüssel.

    ![Typ der Language Understanding-Erstellungsressource auswählen](./media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Wenn Sie die Ressourcenauswahl getroffen haben, [erstellen Sie eine neue App](luis-how-to-start-new-app.md#create-new-app-in-luis).

## <a name="trial-key"></a>Testschlüssel

Der Testschlüssel (Startschlüssel) wird für Sie bereitgestellt. Er wird als Ihr Authentifizierungsschlüssel verwendet, um die Vorhersageendpunkt-Laufzeitumgebung abzufragen (bis zu 1000 Abfragen pro Monat).

Der Schlüssel wird im LUIS-Portal sowohl auf der Seite **Benutzereinstellungen** als auch auf den **Verwalten -> Azure-Ressourcen**-Seiten angezeigt.

Wenn Sie Ihren Vorhersageendpunkt veröffentlichen möchten, [erstellen](#create-luis-resources) Sie Erstellungs- und Vorhersagelaufzeitschlüssel, und [weisen](#assign-a-resource-to-an-app) Sie diese zu, um die Funktionalität des Startschlüssels zu ersetzen.

<a name="create-resources-in-the-azure-portal"></a>


[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="create-resources-in-azure-cli"></a>Erstellen von Ressourcen in der Azure CLI

Verwenden Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), um die einzelnen Ressourcen individuell zu erstellen.

Ressource `kind`:

* Erstellung: `LUIS.Authoring`
* Vorhersage: `LUIS`

1. Melden Sie sich bei der Azure CLI an:

    ```azurecli
    az login
    ```

    Dadurch wird ein Browser geöffnet, in dem Sie das richtige Konto auswählen und die Authentifizierung bereitstellen können.

1. Erstellen Sie eine **LUIS-Erstellungsressource** der Art `LUIS.Authoring` mit dem Namen `my-luis-authoring-resource` in der _vorhandenen_ Ressourcengruppe mit dem Namen `my-resource-group` für die Region `westus`.

    ```azurecli
    az cognitiveservices account create -n my-luis-authoring-resource -g my-resource-group --kind LUIS.Authoring --sku F0 -l westus --yes
    ```

1. Erstellen Sie eine **LUIS-Vorhersagen-Endpunktressource** der Art `LUIS` mit dem Namen `my-luis-prediction-resource` in der _vorhandenen_ Ressourcengruppe mit dem Namen `my-resource-group` für die Region `westus`. Wenn Sie einen höheren Durchsatz als den Free-Tarif wünschen, ändern Sie `F0` in `S0`. Weitere Informationen zu [Tarifen und Durchsatz](luis-limits.md#key-limits).

    ```azurecli
    az cognitiveservices account create -n my-luis-prediction-resource -g my-resource-group --kind LUIS --sku F0 -l westus --yes
    ```

    > [!Note]
    > Diese Schlüssel werden vom LUIS-Portal **nicht** verwendet, solange sie nicht im LUIS-Portal auf den **Verwalten -> Azure-Ressourcen** verwendet werden.

## <a name="assign-an-authoring-resource-in-the-luis-portal-for-all-apps"></a>Zuweisen einer Erstellungsressource im LUIS-Portal für alle Apps

Sie können eine Erstellungsressource für eine einzelne App oder für alle Apps in LUIS zuweisen. In der folgenden Vorgehensweise werden alle Apps einer einzelnen Erstellungsressource zugewiesen.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an.
1. Wählen Sie in der oberen Navigationsleiste ganz rechts Ihr Benutzerkonto aus, und wählen Sie dann **Einstellungen** aus.
1. Wählen Sie auf der Seite **Benutzereinstellungen** die Option **Erstellungsressource hinzufügen** aus, und wählen Sie dann eine vorhandene Erstellungsressource aus. Wählen Sie **Speichern** aus.

## <a name="assign-a-resource-to-an-app"></a>Zuweisen einer Ressource zu einer App

Mit der folgenden Vorgehensweise können Sie einer App eine einzelne Erstellungs- oder Vorhersageendpunktressource zuweisen.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie dann in der Liste **Meine Apps** eine App aus.
1. Navigieren Sie zur Seite **Verwalten -> Azure-Ressourcen**.

    ![„Verwalten -> Azure-Ressourcen“ im LUIS-Portal auswählen, um der App eine Ressource zuzuweisen.](./media/luis-how-to-azure-subscription/manage-azure-resources-prediction.png)

1. Wählen Sie die Registerkarte „Vorhersageressourcen“ oder „Erstellungsressource“ aus, und wählen Sie dann die Schaltfläche **Vorhersageressource hinzufügen** oder **Erstellungsressource hinzufügen** aus.
1. Wählen Sie die Felder im Formular aus, um die richtige Ressource zu finden, und wählen Sie dann **Speichern** aus.

### <a name="assign-runtime-resource-without-using-luis-portal"></a>Zuweisen einer Laufzeitressource ohne Verwendung des LUIS-Portals

Möglicherweise soll zu Automatisierungszwecken, etwa wegen einer CI/CD-Pipeline, die Zuweisung einer LUIS-Laufzeitressource zu einer LUIS-App automatisiert werden. Die folgenden Schritte müssen dazu ausgeführt werden:

1. Rufen Sie ein Azure Resource Manager-Token von dieser [Website](https://resources.azure.com/api/token?plaintext=true) ab. Dieses Token läuft ab, daher sollten Sie es sofort verwenden. Die Anforderung gibt ein Azure Resource Manager-Token zurück.

    ![Anfordern und Empfangen eines Azure Resource Manager-Tokens](./media/luis-manage-keys/get-arm-token.png)

1. Verwenden Sie das Token zum Anfordern der LUIS-Laufzeitressourcen aus verschiedenen Abonnements über die [API zum Abrufen von LUIS-Azure-Konten](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), auf die Ihr Benutzerkonto Zugriff hat.

    Diese POST-API erfordert folgende Einstellungen:

    |Header|Wert|
    |--|--|
    |`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.|
    |`Ocp-Apim-Subscription-Key`|Ihr Erstellungsschlüssel.|

    Diese API gibt ein Array von JSON-Objekten aus Ihren LUIS-Abonnements, einschließlich Abonnement-ID, Ressourcengruppe und Ressourcennamen als Kontonamen zurück. Finden Sie im Array das Element, das die LUIS-Ressource darstellt, die der LUIS-App zugewiesen werden soll.

1. Weisen Sie das Token mit der API zum [Zuweisen eines LUIS-Azure-Kontos zu einer Anwendung](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) der LUIS-Ressource zu.

    Diese POST-API erfordert folgende Einstellungen:

    |type|Einstellung|Wert|
    |--|--|--|
    |Header|`Authorization`|Der Wert von `Authorization` ist `Bearer {token}`. Beachten Sie, dass dem Tokenwert das Wort `Bearer` und ein Leerzeichen vorangestellt werden müssen.|
    |Header|`Ocp-Apim-Subscription-Key`|Ihr Erstellungsschlüssel.|
    |Header|`Content-type`|`application/json`|
    |Abfragezeichenfolge|`appid`|die LUIS-App-ID
    |Body||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Wenn diese API erfolgreich ist, wird der Status „201 – erstellt“ zurückgegeben.

## <a name="unassign-resource"></a>Aufheben der Zuweisung einer Ressource

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie dann in der Liste **Meine Apps** eine App aus.
1. Navigieren Sie zur Seite **Verwalten -> Azure-Ressourcen**.
1. Wählen Sie die Registerkarte „Vorhersageressourcen“ oder „Erstellungsressource“ aus, und wählen Sie dann die Schaltfläche **Ressourcenzuweisung aufheben** aus.

Wenn Sie die Zuweisung einer Ressource aufheben, wird diese nicht aus Azure gelöscht. Lediglich die Verknüpfung mit LUIS wird aufgehoben.

## <a name="reset-authoring-key"></a>Zurücksetzen des Authoringschlüssels

**Für Apps, die zur [Erstellungsressource migriert](luis-migration-authoring.md) wurden**: Wurde Ihr Erstellungsschlüssel kompromittiert, setzen Sie den Schlüssel für diese Erstellungsressource im Azure-Portal auf der Seite **Schlüssel** zurück.

**Für Apps, die noch nicht migriert wurden**: Der Schlüssel wird für Ihre Apps im LUIS-Portal zurückgesetzt. Wenn Sie Ihre Apps mit den Erstellungs-APIs schreiben, müssen Sie den Wert von „Ocp-Apim-Subscription-Key“ in den neuen Schlüssel ändern.

## <a name="regenerate-azure-key"></a>Erneutes Generieren von Azure-Schlüsseln

Generieren Sie die Azure-Schlüssel im Azure-Portal auf der Seite **Schlüssel** erneut.

## <a name="delete-account"></a>Konto löschen

Informationen zu den Daten, die beim Löschen Ihres Kontos gelöscht werden, finden Sie unter [Datenspeicherung und -löschung](luis-concept-data-storage.md#accounts).

## <a name="change-pricing-tier"></a>Ändern des Tarifs

1.  Suchen Sie in [Azure](https://portal.azure.com) nach Ihrem LUIS-Abonnement. Wählen Sie das LUIS-Abonnement aus.
    ![Suchen nach Ihrem LUIS-Abonnement](./media/luis-usage-tiers/find.png)
1.  Wählen Sie **Tarif** aus, um die verfügbaren Tarife anzuzeigen.
    ![Anzeigen der Tarife](./media/luis-usage-tiers/subscription.png)
1.  Wählen Sie den Tarif aus, und wählen Sie dann **Auswählen** aus, um die Änderung zu speichern.
    ![Ändern Ihres LUIS-Tarifs](./media/luis-usage-tiers/plans.png)
1.  Wenn die Änderung des Tarifs abgeschlossen ist, wird der neue Tarif in einem Popupfenster bestätigt.
    ![Überprüfen Ihres LUIS-Tarifs](./media/luis-usage-tiers/updated.png)
1. Denken Sie daran, [diesen Endpunktschlüssel](#assign-a-resource-to-an-app) auf der Seite **Publish** (Veröffentlichen) zuzuweisen und für alle Endpunktabfragen zu verwenden.

## <a name="viewing-azure-resource-metrics"></a>Anzeigen von Azure-Ressourcenmetriken

### <a name="viewing-azure-resource-summary-usage"></a>Anzeigen der Nutzungszusammenfassung von Azure-Ressourcen
Sie können in Azure LUIS-Nutzungsinformationen anzeigen. Auf der Seite **Übersicht** werden aktuelle zusammenfassende Informationen angezeigt, einschließlich Aufrufen und Fehlern. Wenn Sie eine LUIS-Endpunktanforderung ausführen, kann es bis zu fünf Minuten dauern, bis die Nutzung auf der Seite **Übersicht** angezeigt wird.

![Anzeigen der Nutzungszusammenfassung](./media/luis-usage-tiers/overview.png)

### <a name="customizing-azure-resource-usage-charts"></a>Anpassen von Nutzungsdiagrammen für Azure-Ressourcen
Metriken erlauben eine detailliertere Darstellung der Daten.

![Standardmetriken](./media/luis-usage-tiers/metrics-default.png)

Sie können Ihre Metrikdiagramme für Zeitraum und Metriktyp konfigurieren.

![Benutzerdefinierte Metriken](./media/luis-usage-tiers/metrics-custom.png)

### <a name="total-transactions-threshold-alert"></a>Warnung für Gesamttransaktions-Schwellenwert
Wenn Sie informiert werden möchten, wenn Sie einen bestimmten Transaktionsschwellenwert erreicht haben, z.B. 10.000 Transaktionen, können Sie eine Warnung erstellen.

![Standardwarnungen](./media/luis-usage-tiers/alert-default.png)

Fügen Sie eine Metrikwarnung für die Metrik der **Summe der Aufrufe** für einen bestimmten Zeitraum hinzu. Fügen Sie die E-Mail-Adressen aller Personen hinzu, die die Warnung erhalten sollen. Fügen Sie Webhooks für alle Systeme hinzu, die die Warnung erhalten sollen. Sie können auch eine Logik-App ausführen, wenn die Warnung ausgelöst wird.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Sie Versionen verwenden](luis-how-to-manage-versions.md), um den Lebenszyklus ihrer App zu steuern.
* Informieren Sie sich über die Konzepte, einschließlich der [Erstellungsressource](luis-concept-keys.md#authoring-key) und der [Mitwirkenden](luis-concept-keys.md#contributions-from-other-authors) für diese Ressource.
* Erfahren Sie, wie Sie Erstellungs- und Laufzeitressourcen [erstellen](luis-how-to-azure-subscription.md).
* Migrieren Sie zu der neuen [Erstellungsressource](luis-migration-authoring.md).
