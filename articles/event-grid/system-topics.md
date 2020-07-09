---
title: Systemthemen in Azure Event Grid
description: Hier werden die Systemthemen in Azure Event Grid beschrieben.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: 67746ebd8a16eb02b8f02d238b0e3c0125989189
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84308267"
---
# <a name="system-topics-in-azure-event-grid"></a>Systemthemen in Azure Event Grid
Der Azure Event Grid-Dienst erstellt Systemthemen, wenn Sie ein erstes Ereignisabonnement für eine Azure-Ereignisquelle erstellen. Derzeit erstellt Event Grid keine Systemthemen für Themenquellen, die vor dem 15. März 2020 erstellt wurden. Für alle Themenquellen, die Sie nach diesem Datum erstellt haben, erstellt Event Grid automatisch Systemthemen. In diesem Artikel werden die **Systemthemen** in Azure Event Grid beschrieben.

> [!NOTE]
> Dieses Feature ist zurzeit nicht für die Azure Government-Cloud aktiviert. 

## <a name="overview"></a>Übersicht
Wenn Sie das erste Ereignisabonnement für eine Azure-Ereignisquelle wie ein Azure Storage-Konto erstellen, wird im Rahmen des Bereitstellungsprozesses des Abonnements eine zusätzliche Ressource des Typs **Microsoft.EventGrid/systemTopics** erstellt. Wenn das letzte Ereignisabonnement für die Azure-Ereignisquelle gelöscht wird, wird das Systemthema automatisch gelöscht.

Das Systemthema kann nicht für benutzerdefinierte Themenszenarios verwendet werden (z. B. Event Grid-Themen und Event Grid-Domänen). 

## <a name="name"></a>Name 
Wenn Sie zuvor ein Abonnement für ein Ereignis erstellt haben, das durch Azure-Quellen ausgelöst wurde, hat der Event Grid-Dienst automatisch ein Systemthema mit einem **zufällig generierten Namen** erstellt. Jetzt können Sie beim Erstellen des Themas im Azure-Portal einen Namen für das Systemthema angeben. Sie können diese Systemthemaressource verwenden, um Metriken und Diagnoseprotokolle zu ermitteln.

## <a name="location"></a>Standort
Für Azure-Ereignisquellen, die sich in einer bestimmten Region bzw. einem bestimmten Speicherort befinden, wird das Systemthema am gleichen Speicherort erstellt wie die Azure-Ereignisquelle. Wenn Sie beispielsweise ein Ereignisabonnement für Azure Blob Storage in der Region „USA, Osten“ erstellen, wird das Systemthema in „USA, Osten“ erstellt. Für globale Azure-Ereignisquellen wie Azure-Abonnements, Ressourcengruppen oder Azure Maps erstellt Event Grid das Systemthema an einem **globalen** Speicherort. 

## <a name="resource-group"></a>Resource group 
Im Allgemeinen wird das Systemthema in derselben Ressourcengruppe erstellt, in der sich auch die Azure-Ereignisquelle befindet. Für Ereignisabonnements, die im Bereich des Azure-Abonnements erstellt wurden, wird das Systemthema unter der Ressourcengruppe **Default-EventGrid** erstellt. Wenn die Ressourcengruppe nicht vorhanden ist, wird diese von Azure Event Grid vor dem Systemthema erstellt. 

Wenn Sie versuchen, die Ressourcengruppe mit dem Speicherkonto zu löschen, wird das Systemthema in der Liste der betroffenen Ressourcen angezeigt.  

![Ressourcengruppe löschen](./media/system-topics/delete-resource-group.png)


## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den folgenden Artikel: [Erstellen, Anzeigen und Verwalten von Systemthemen](create-view-manage-system-topics.md).