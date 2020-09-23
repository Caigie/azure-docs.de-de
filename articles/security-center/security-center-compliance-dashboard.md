---
title: 'Tutorial: Überprüfungen der Einhaltung gesetzlicher Bestimmungen: Azure Security Center'
description: 'Tutorial: Es wird beschrieben, wie Sie die Einhaltung gesetzlicher Bestimmungen verbessern, indem Sie Azure Security Center verwenden.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2019
ms.author: memildin
ms.openlocfilehash: 7f235f9255df49c096e224e1754ad2bbdf664423
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895816"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Tutorial: Verbessern der Einhaltung gesetzlicher Vorschriften
---

Mit Azure Security Center kann der Prozess zur Einhaltung von Anforderungen gesetzlicher Bestimmungen optimiert werden, indem das **Dashboard zur Einhaltung gesetzlicher Bestimmungen** verwendet wird. Im Dashboard werden über Security Center basierend auf fortlaufenden Bewertungen Ihrer Azure-Umgebung Informationen zu Ihrem Konformitätsstatus (Compliance) bereitgestellt. Mit Security Center werden Risikofaktoren in Ihrer Hybrid Cloud-Umgebung anhand der bewährten Methoden für die Sicherheit analysiert. Diese Bewertungen sind Konformitätskontrollen zugeordnet, die aus dem unterstützten Standardsatz stammen. Im Dashboard für die Einhaltung gesetzlicher Bestimmungen wird der Status aller Bewertungen in Ihrer Umgebung im Kontext eines bestimmten Standards oder einer Bestimmung angezeigt. Wenn Sie aufgrund der Empfehlungen Maßnahmen ergreifen und die Risikofaktoren Ihrer Umgebung reduzieren, verbessert sich Ihr Konformitätsstatus.

In diesem Tutorial lernen Sie Folgendes:

-   Evaluieren der Einhaltung gesetzlicher Bestimmungen mit dem entsprechenden Dashboard

-   Verbessern Ihres Konformitätsstatus durch das Ergreifen von Maßnahmen aufgrund von Empfehlungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchlaufen der in diesem Tutorial behandelten Features muss [Azure Defender](azure-defender.md) aktiviert sein. Azure Defender kann 30 Tage lang kostenlos getestet werden.

##  <a name="assess-your-regulatory-compliance"></a>Bewerten der Einhaltung gesetzlicher Bestimmungen

Security Center bewertet die Konfiguration Ihrer Ressourcen fortlaufend, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. Diese Bewertungen werden als Empfehlungen angezeigt, bei denen es hauptsächlich um die Verbesserung der Sicherheit geht. Im Dashboard zur Einhaltung gesetzlicher Bestimmungen können Sie die Konformitätsstandards mit allen Anforderungen anzeigen. Die unterstützten Anforderungen sind hierbei jeweils den entsprechenden Sicherheitsbewertungen zugeordnet. Auf diese Weise können Sie Ihren Konformitätsstatus in Bezug auf den Standard anzeigen, der auf dem Status dieser Bewertungen basiert.

Das Dashboard zur Einhaltung gesetzlicher Bestimmungen kann Ihr Augenmerk auf die Konformitätslücken lenken, die für einen wichtigen Standard oder eine Bestimmung bestehen. Aufgrund dieser genauen Anzeige können Sie Ihre Compliancebewertung in dynamischen Cloud- und Hybridumgebungen ständig überwachen.

>[!NOTE]
> Standardmäßig unterstützt Security Center die folgenden gesetzlichen Standards: Azure CIS, PCI-DSS 3.2, ISO 27001 und SOC TSP. 
>
> Das Feature [Dynamische Compliancepakete (Vorschauversion)](update-regulatory-compliance-packages.md) ermöglicht Ihnen ein Upgrade der Standards, die in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen angezeigt werden, auf die neuen *dynamischen* Pakete. Dieselbe Previewfunktion können Sie auch nutzen, um neue Compliancepakete hinzuzufügen und Ihre Compliance mit zusätzlichen Standards zu überwachen. 

1.  Wählen Sie im Hauptmenü von Security Center unter **RICHTLINIE UND KONFORMITÄT** die Option **Einhaltung gesetzlicher Bestimmungen** aus. <br>
Am oberen Rand des Bildschirms wird ein Dashboard angezeigt, das eine Übersicht über Ihren Konformitätsstatus mit den unterstützten Bestimmungen enthält. Es sind Ihr Gesamtstatus der Konformität und die Anzahl von bestandenen und nicht bestandenen Bewertungen der einzelnen Standards angegeben.

    ![Computerbeschreibung – Hohe Vertrauenswürdigkeit](./media/security-center-compliance-dashboard/compliance-dashboard.png)

2.  Wählen Sie eine Registerkarte für einen Konformitätsstandard aus, der für Sie relevant ist. Die Liste mit allen Kontrollen des Standards wird angezeigt. Sie können für eine Kontrolle jeweils die Details zu den bestandenen und nicht bestandenen Bewertungen anzeigen. Einige Kontrollen sind abgeblendet. Diesen Kontrollen sind keine Security Center-Bewertungen zugeordnet. Überprüfen Sie die Anforderungen hierfür, und führen Sie die Bewertung in Ihrer Umgebung selbst durch. Es kann sein, dass einige Anforderungen prozessbezogen und nicht technischer Art sind.

    ![Registerkarte „Konformität“](./media/security-center-compliance-dashboard/compliance-pci.png)

1. Klicken Sie auf **Bericht herunterladen**, um einen PDF-Bericht zum aktuellen Compliancestatus für einen bestimmten Standard zu generieren und herunterzuladen.

    Der Bericht enthält eine allgemeine Zusammenfassung des Compliancestatus für den ausgewählten Standard auf der Grundlage von Security Center-Bewertungsdaten. Der Bericht ist nach den Kontrollen dieses bestimmten Standards gegliedert. Der Bericht kann an Stakeholder weitergegeben und als Nachweis bei internen und externen Prüfungen genutzt werden.

    ![Download verfügbar ist](./media/security-center-compliance-dashboard/download-report.png)

## <a name="improve-your-compliance-posture"></a>Verbessern Ihres Konformitätsstatus

Mit den Informationen im Dashboard für die Einhaltung gesetzlicher Bestimmungen können Sie Ihren Konformitätsstatus verbessern, indem Sie direkt im Dashboard Lösungen für Empfehlungen finden.

1.  Klicken Sie durch die nicht bestandenen Bewertungen, die im Dashboard angezeigt werden, um jeweils die Details zu einer Empfehlung anzuzeigen. Jede Empfehlung enthält verschiedene Lösungsschritte, die ausgeführt werden müssen, um das Problem zu beheben.

1.  Sie können eine bestimmte Ressource auswählen, um weitere Details anzuzeigen und die Empfehlung für diese Ressource zu lösen. <br>Auf der Registerkarte zum **Azure CIS-Standard** können Sie beispielsweise auf die Empfehlung **Sichere Übertragung zum Speicherkonto erforderlich** klicken.

    ![Auswählen der Empfehlung **Sichere Übertragung an Speicherkonto erforderlich**](./media/security-center-compliance-dashboard/compliance-recommendation.png)

1. Wenn Sie durch die Empfehlungsinformationen klicken und eine fehlerhafte Ressource auswählen, gelangen Sie direkt zur Aktivierung der **sicheren Speicherübertragung** im Azure-Portal.

    Weitere Informationen zur Anwendung der Empfehlungen finden Sie unter [Implementieren von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).

    ![Speicherkontokonfiguration](./media/security-center-compliance-dashboard/compliance-remediate-recommendation.png)

1.  Nachdem Sie Lösungsmaßnahmen für die Empfehlungen ergriffen haben, sind die Auswirkungen im Bericht im Dashboard zur Einhaltung gesetzlicher Bestimmungen sichtbar, weil sich Ihre Compliancebewertung verbessert hat.

    > [!NOTE]
    > Die Bewertungen werden ungefähr alle 12 Stunden durchgeführt, und Sie sehen die Auswirkungen auf Ihre Konformitätsdaten jeweils erst nach Abschluss einer Bewertung.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie das Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen für folgende Zwecke verwenden:

-   Anzeigen und Überwachen Ihres Konformitätsstatus basierend auf den Standards und Bestimmungen, die für Sie wichtig sind

-   Verbessern Ihres Konformitätsstatus durch das Lösen relevanter Empfehlungen und Verfolgen der verbesserten Compliancebewertung

Mit dem Dashboard zur Einhaltung gesetzlicher Bestimmungen kann der Konformitätsprozess stark vereinfacht werden. Darüber hinaus kann eine erhebliche Verkürzung des Zeitraums erzielt werden, der für das Sammeln von Konformitätsnachweisen für Ihre Azure- und Hybridumgebung erforderlich ist.

Weitere Informationen:

-   [Aktualisieren auf dynamische Compliancepakete in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen (Vorschau)](update-regulatory-compliance-packages.md): Informieren Sie sich über diese neue Previewfunktion, mit der Sie die Standards, die in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen angezeigt werden, auf die neuen *dynamischen* Pakete aktualisieren können. Dieselbe Previewfunktion können Sie auch nutzen, um neue Compliancepakete hinzuzufügen und Ihre Compliance mit zusätzlichen Standards zu überwachen. 

-   [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.

-   [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Es wird beschrieben, wie Sie Empfehlungen in Azure Security Center nutzen, um Ihre Azure-Ressourcen zu schützen.

-   [Verbessern des Secure Score in Azure Security Center](secure-score-security-controls.md): Es wird beschrieben, wie Sie Sicherheitsrisiken und -empfehlungen priorisieren, um Ihren Sicherheitsstatus bestmöglich zu verbessern.