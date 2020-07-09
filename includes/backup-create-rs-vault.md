---
title: include file
description: include file
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: include
ms.date: 10/18/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: 2c74783ea8246232cb5c4270691daf3f83fe9a30
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82204643"
---
## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Bei einem Recovery Services-Tresor handelt es sich um eine Speicherentität zum Speichern der im Laufe der Zeit erstellten Wiederherstellungspunkte. Er enthält auch die zugehörigen Sicherungsrichtlinien für die geschützten Elemente.

Führen Sie die folgenden Schritte aus, um einen Recovery Services-Tresor zu erstellen:

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie im Menü links **Alle Dienste** aus.

    ![Auswählen von „Alle Dienste“](./media/backup-create-rs-vault/click-all-services.png)

1. Geben Sie im Dialogfeld **Alle Dienste** *Recovery Services* ein. Die Liste der Ressourcen wird Ihrer Eingabe entsprechend gefiltert. Wählen Sie in der Liste der Ressourcen **Recovery Services-Tresore** aus.

    ![Eingeben und Auswählen von „Recovery Services-Tresore“](./media/backup-create-rs-vault/all-services.png)

    Die Liste mit den Recovery Services-Tresoren im Abonnement wird angezeigt.

1. Wählen Sie auf dem Dashboard **Recovery Services-Tresore** die Option **Hinzufügen** aus.

    ![Hinzufügen eines Recovery Services-Tresors](./media/backup-create-rs-vault/add-button-create-vault.png)

    Das Dialogfeld **Recovery Services-Tresor** wird geöffnet. Legen Sie Werte für **Name**, **Abonnement**, **Ressourcengruppe** und **Speicherort** fest.

    ![Konfigurieren des Recovery Services-Tresors](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Name**: Geben Sie einen Anzeigenamen ein, über den der Tresor identifiziert wird. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
   - **Abonnement**: Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist.
   - **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** und dann eine Ressource in der Dropdownliste aus. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein. Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure Resource Manager – Übersicht](../articles/azure-resource-manager/management/overview.md).
   - **Standort**: Wählen Sie die geografische Region für den Tresor aus. Um einen Tresor zum Schutz von Datenquellen zu erstellen, *muss* sich dieser in derselben Region wie die Datenquelle befinden.

      > [!IMPORTANT]
      > Wenn Sie den Speicherort der Datenquelle nicht kennen, schließen Sie das Dialogfeld. Navigieren Sie zur Liste Ihrer Ressourcen im Portal. Falls Sie über Datenquellen in mehreren Regionen verfügen, erstellen Sie für jede Region einen Recovery Services-Tresor. Erstellen Sie den Tresor am ersten Speicherort, bevor Sie den Tresor für einen anderen Speicherort erstellen. Das Angeben von Speicherkonten zum Speichern der Sicherungsdaten ist nicht erforderlich. Der Recovery Services-Tresor und Azure Backup führen diesen Schritt automatisch aus.
      >
      >

1. Wenn Sie den Recovery Services-Tresor erstellen möchten, wählen Sie **Erstellen** aus.

    ![Erstellen des Recovery Services-Tresors](./media/backup-create-rs-vault/click-create-button.png)

    Es kann einige Zeit dauern, denn Recovery Services-Tresor zu erstellen. Verfolgen Sie die Statusbenachrichtigungen im **Benachrichtigungsbereich** in der oberen rechten Ecke im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, wählen Sie **Aktualisieren**.

     ![Aktualisieren der Liste der Sicherungstresore](./media/backup-create-rs-vault/refresh-button.png)
