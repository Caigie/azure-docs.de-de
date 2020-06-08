---
title: Migrieren zu einer Azure-Erstellungsressource
titleSuffix: Azure Cognitive Services
description: Informationen über Migrieren zu einer Azure-Erstellungsressource
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: 2c28e6c1edf4188cf3ea80c14565785dcf1dcbba
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653817"
---
# <a name="steps-to-migrate-to-the-azure-authoring-resource"></a>Schritte zum Migrieren zur Azure-Erstellungsressource

Migrieren Sie alle Apps, deren Besitzer Sie sind, über das LUIS-Portal (Language Understanding), damit in ihnen die Azure-Erstellungsressource verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

* **Optional**: Sichern Sie die Apps, die im LUIS-Portal in der Liste der Apps aufgeführt sind, indem Sie jede App exportieren oder die Export-[API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) verwenden.
* **Optional**: Speichern Sie für jede App deren Liste der Projektmitarbeiter. Allen Projektmitarbeitern kann im Rahmen des Migrationsprozesses eine E-Mail gesendet werden.
* **Erforderlich**: Sie benötigen ein [Azure-Abonnement](https://azure.microsoft.com/free/). In einem Schritt des Abonnementprozesses sind Abrechnungsinformationen erforderlich. Sie können jedoch Free-Tarife (F0-Tarife) verwenden, wenn Sie LUIS verwenden. Sie werden möglicherweise feststellen, dass Sie einen kostenpflichtigen Tarif benötigen, wenn Ihre Nutzung zunimmt.

Wenn Sie kein Azure-Abonnement haben, [registrieren Sie sich](https://azure.microsoft.com/free/).

## <a name="access-the-migration-process"></a>Zugang zum Migrationsprozess

Sie werden wöchentlich aufgefordert, Ihre Apps zu migrieren. Sie können dieses Fenster abbrechen, ohne eine Migration vorzunehmen. Wenn Sie eine Migration vor dem nächsten geplanten Zeitraum vornehmen möchten, können Sie den Migrationsvorgang über das Symbol **Azure** auf der oberen Symbolleiste des LUIS-Portals beginnen.

> [!div class="mx-imgBorder"]
> ![Migrationssymbol](./media/migrate-authoring-key/migration-button.png)

## <a name="app-owner-begins-the-migration-process"></a>App-Besitzer beginnt den Migrationsprozess

Der Migrationsprozess ist verfügbar, wenn Sie der Besitzer einer LUIS-App sind.

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und stimmen Sie den Nutzungsbedingungen zu.
1. Im Popupfenster für Migrationen können Sie die Migration fortsetzen oder später vornehmen. Wählen Sie **Jetzt migrieren** aus. Wenn Sie sich für eine spätere Migration entscheiden, haben Sie 9 Monate Zeit, um zum neuen Erstellungsschlüssel in Azure zu migrieren.

    ![Erstes Popupfenster im Migrationsprozess, wählen Sie „Jetzt migrieren“ aus.](./media/migrate-authoring-key/migrate-now.png)

1. Gibt es für eine Ihrer Apps Projektmitarbeiter, werden Sie optional aufgefordert, **diesen Mitarbeitern eine E-Mail zu senden**, in der sie über die Migration informiert werden. Dieser Schritt ist optional.

    Nachdem Sie Ihr Konto zu Azure migriert haben, sind Ihre Apps nicht mehr für Projektmitarbeiter verfügbar.

    Für jeden Projektmitarbeiter und jede App wird die Standard-E-Mail-Anwendung mit einer einfach formatierten E-Mail geöffnet. Sie können die E-Mail vor dem Senden bearbeiten.

    Die E-Mail-Vorlage enthält die genaue App-ID und den App-Namen.

    ```html
    Dear Sir/Madam,

    I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

    App Id: <app-ID-omitted>
    App name: Human Resources

    Thank you
    ```

1. Wählen Sie diese Option aus, um eine LUIS-Erstellungsressource zu erstellen, indem Sie eine vorhandene Erstellungsressource verwenden oder eine neue Erstellungsressource erstellen.

    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Erstellungsressource](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

1. Geben Sie im nächsten Fenster ihre Ressourcenschlüsselinformationen ein. Nachdem Sie die Informationen eingegeben haben, wählen Sie **Ressource erstellen** aus. Pro Abonnement können Sie pro Region über 10 kostenlose Erstellungsressourcen verfügen.

    ![Erstellungsressource erstellen](./media/migrate-authoring-key/choose-authoring-resource-form.png)

    Geben Sie beim **Erstellen einer neuen Erstellungsressource** die folgenden Informationen an:

    * **Ressourcenname**: ein von Ihnen gewählter benutzerdefinierter Name, der als Teil der URL für Ihre Abfragen für Erstellungs- und Vorhersageendpunkte verwendet wird
    * **Mandant**: der Mandant, dem Ihr Azure-Abonnement zugeordnet ist
    * **Abonnementname**: das Abonnement, unter dem die Ressource abgerechnet wird.
    * **Ressourcengruppe**: ein benutzerdefinierter Ressourcengruppenname, den Sie auswählen oder erstellen. Mit Ressourcengruppen können Sie Azure-Ressourcen für den Zugriff und die Verwaltung gruppieren.
    * **Standort**: Die Auswahl des Standorts hängt von der Auswahl der **Ressourcengruppe** ab.
    * **Tarif**: Der Tarif bestimmt die maximale Anzahl von Transaktionen pro Sekunde und Monat.

1. Überprüfen Sie Ihre Erstellungsressource, und **migrieren Sie jetzt**.

    ![Erstellungsressource erstellen](./media/migrate-authoring-key/choose-authoring-resource-and-migrate.png)

1. Wenn die Erstellungsressource erstellt ist, wird die Erfolgsmeldung angezeigt. Wählen Sie **Schließen** aus, um das Popupfenster zu schließen.

    ![Ihre Erstellungsressource wurde erfolgreich erstellt.](./media/migrate-authoring-key/migration-success.png)

    In der Liste **Meine Apps** werden die Apps angezeigt, die zur neuen Erstellungsressource migriert wurden.

    Es ist nicht erforderlich, dass Sie den Schlüssel der Erstellungsressource wissen, um Ihre Apps im LUIS-Portal weiter bearbeiten zu können. Wenn Sie Ihre Apps programmgesteuert bearbeiten möchten, benötigen Sie die Erstellungsschlüsselwerte. Diese Werte werden im LUIS-Portal auf der Seite **Verwalten-> Azure-Ressourcen** angezeigt und sind auch im Azure-Portal auf der **Schlüssel**-Seite der Ressource verfügbar.

1. Bevor Sie auf Ihre Apps zugreifen, wählen Sie das Abonnement und die LUIS-Erstellungsressource aus, um die Apps anzuzeigen, die Sie erstellen können.

    > [!div class="mx-imgBorder"]
    > ![Wählen Sie das Abonnement und die LUIS-Erstellungsressource aus, um die Apps anzuzeigen, die Sie erstellen können.](./media/create-app-in-portal-select-subscription-luis-resource.png)

## <a name="app-contributor-begins-the-migration-process"></a>App-Mitwirkender beginnt den Migrationsprozess

Befolgen Sie dieselben Schritte wie der App-Besitzer für die Migration. Der Prozess erstellt eine neue Erstellungsressource vom Typ `LUIS.Authoring`.

Sie müssen Ihr Konto migrieren, um zu migrierten Apps als Mitwirkender hinzugefügt zu werden, die sich im Besitz anderer befinden.

## <a name="after-the-migration-process-add-contributors-to-your-authoring-resource"></a>Nach dem Migrationsprozess: Hinzufügen von Mitwirkenden zu Ihrer Erstellungsressource

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Erfahren Sie, wie Sie [Mitwirkende hinzufügen](luis-how-to-collaborate.md).

## <a name="troubleshooting-errors-with-the-migration-process"></a>Problembehandlung bei Migrationsprozessfehlern

Wenn Sie während des Migrationsprozesses im LUIS-Portal eine `MissingSubscriptionRegistration`-Fehlermeldung mit einer roten Benachrichtigungsleiste empfangen, erstellen Sie eine Cognitive Service-Ressource im [Azure-Portal](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) oder in der [Azure CLI](luis-how-to-azure-subscription.md#create-resources-in-azure-cli). Erfahren Sie mehr über die [Ursachen dieses Fehlers](../../azure-resource-manager/templates/error-register-resource-provider.md#cause).

## <a name="next-steps"></a>Nächste Schritte


* Lesen Sie die [Konzepte](luis-concept-keys.md) über Erstellungs- und Laufzeitschlüssel.
* Erfahren Sie, [wie Sie Schlüssel zuweisen](luis-how-to-azure-subscription.md) und [Mitwirkende](luis-how-to-collaborate.md) hinzufügen.
