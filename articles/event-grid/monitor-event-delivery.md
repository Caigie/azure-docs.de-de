---
title: Überwachen der Azure Event Grid-Nachrichtenübermittlung
description: In diesem Artikel erfahren Sie, wie Sie im Azure-Portal den Übermittlungsstatus von Azure Event Grid-Nachrichten anzeigen.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 7a01ab91fe84aaa1fe55018754eddbf8b8f89643
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2020
ms.locfileid: "82890855"
---
# <a name="monitor-event-grid-message-delivery"></a>Überwachen der Event Grid-Nachrichtenübermittlung 

Dieser Artikel beschreibt, wie Sie im Portal den Status von Ereignisübermittlungen einsehen können.

Event Grid bietet permanente Übermittlung. Jede Nachricht wird für jedes Abonnement mindestens einmal übermittelt. Ereignisse werden sofort an den registrierten Webhook des jeweiligen Abonnements gesendet. Wenn ein Webhook den Eingang eines Ereignisses nicht innerhalb von 60 Sekunden nach dem ersten Übermittlungsversuch bestätigt, wiederholt Event Grid die Übermittlung des Ereignisses.

Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Übermittlungsmetriken

Das Portal zeigt Metriken für den Status der Übermittlung von Ereignisnachrichten an.

In den folgenden Themen finden Sie einige der Metriken:

* **Veröffentlichung erfolgreich**: Das Ereignis wurde erfolgreich an das Thema gesendet und mit einer Antwort des Typs 2xx verarbeitet.
* **Fehler beim Veröffentlichen**: Das Ereignis wurde an das Thema gesendet, aber mit einem Fehlercode abgelehnt.
* **Ohne Übereinstimmung**: Das Ereignis wurde erfolgreich im Thema veröffentlicht, stimmt aber mit keinem Ereignisabonnement überein. Das Ereignis wurde gelöscht.

Für Abonnements sind hier einige Metriken aufgeführt:

* **Übermittlung erfolgreich**: Das Ereignis wurde erfolgreich an den Endpunkt des Abonnements übermittelt und hat eine Antwort des Typs 2xx erhalten.
* **Übermittlungsfehler**: Jedes Mal, wenn der Dienst eine Übermittlung versucht und der Ereignishandler keinen 2xx-Erfolgscode zurückgibt, wird der Zähler **Übermittlungsfehler** erhöht. Wenn Sie versuchen, dasselbe Ereignis mehrmals zu übermitteln, und dies fehlschlägt, wird der Zähler **Übermittlungsfehler** für jeden Fehler erhöht.
* **Abgelaufene Ereignisse**: Das Ereignis wurde nicht übermittelt, und alle Wiederholungsversuche wurden gesendet. Das Ereignis wurde gelöscht.
* **Übereinstimmende Ereignisse**: Das Ereignis im Thema stimmt mit dem Ereignisabonnement überein.

    > [!NOTE]
    > Die vollständige Liste der Metriken finden Sie unter [Von Azure Event Grid unterstützte Metriken](metrics.md).

## <a name="event-subscription-status"></a>Status des Ereignisabonnements

Wenn Sie Metriken für ein Ereignisabonnement anzeigen möchten, können Sie entweder nach Abonnementtyp oder nach Abonnements für eine bestimmte Ressource suchen.

Um nach Ereignisabonnementtyp zu suchen, wählen Sie **Alle Dienste** aus.

![Auswählen von „Alle Dienste“](./media/monitor-event-delivery/all-services.png)

Suchen Sie nach **Event Grid**, und wählen Sie die Option **Event Grid-Abonnements** aus.

![Suchen nach Ereignisabonnements](./media/monitor-event-delivery/search-and-select.png)

Filtern Sie nach dem Typ des Ereignisses, des Abonnements und des Standorts. Wählen Sie **Metriken** für das Abonnement aus, um diese anzuzeigen.

![Filtern von Ereignisabonnements](./media/monitor-event-delivery/filter-events.png)

Zeigen Sie die Metriken für das Ereignisthema und Abonnement an.

![Anzeigen von Ereignismetriken](./media/monitor-event-delivery/subscription-metrics.png)

Um nach den Metriken für eine bestimmte Ressource zu suchen, wählen Sie die entsprechende Ressource aus. Wählen Sie dann **Ereignisse** aus.

![Auswählen von Ereignissen für eine Ressource](./media/monitor-event-delivery/select-events.png)

Daraufhin werden die Metriken für die Abonnements für diese Ressource angezeigt.

## <a name="custom-event-status"></a>Benutzerdefinierter Ereignisstatus

Wenn Sie ein benutzerdefiniertes Thema veröffentlicht haben, können Sie die Metriken dafür anzeigen. Wählen Sie die Ressourcengruppe für das Thema aus, und wählen Sie dann das Thema aus.

![Auswählen eines benutzerdefinierten Themas](./media/monitor-event-delivery/select-custom-topic.png)

Zeigen Sie die Metriken für das benutzerdefinierte Ereignisthema an.

![Anzeigen von Ereignismetriken](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="set-alerts"></a>Festlegen von Benachrichtigungen

Sie können für benutzerdefinierte Themen und Ereignisdomänen Warnungen zu den Themen sowie den Metriken auf Domänenebene festlegen. Klicken Sie auf dem Blatt „Übersicht“ im Ressourcenmenü auf der linken Seite auf **Warnungen**, um Warnungsregeln abzurufen, zu verwalten und zu erstellen. [Weitere Informationen zu Azure Monitor-Warnungen](../azure-monitor/platform/alerts-overview.md)

![Anzeigen von Ereignismetriken](./media/monitor-event-delivery/select-alerts.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Ereignisübermittlungen und Wiederholungen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](delivery-and-retry.md).
* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
