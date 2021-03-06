---
title: 'Azure Security Center: Hauptdashboard oder Übersichtsseite'
description: Enthält eine Beschreibung der Features auf der Übersichtsseite von Security Center.
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: conceptual
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 637f7dde63d562f71aa31561a960ed3502a80229
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930343"
---
# <a name="azure-security-centers-overview-page"></a>Azure Security Center-Übersichtsseite

Wenn Sie Azure Security Center öffnen, wird als Erstes die Übersichtsseite angezeigt. 

:::image type="content" source="media/overview-page/overview.png" alt-text="Security Center-Übersichtsseite":::

Ermitteln und bewerten Sie die Sicherheit Ihrer Workloads, und identifizieren und mindern Sie die Risiken auf der Seite mit der Security Center-Übersicht.

Diese Seite enthält eine einheitliche Übersicht über den Sicherheitsstatus Ihrer Hybrid Cloud-Workloads. Darüber hinaus werden darauf Sicherheitswarnungen, Informationen zur Abdeckung und andere Infos angezeigt.


## <a name="features-of-the-overview-page"></a>Features der Übersichtsseite

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Leiste am oberen Bildschirmrand auf der Security Center-Übersichtsseite":::

Die **obere Menüleiste** enthält Folgendes:
- **Abonnements**: Sie können die Liste mit den Abonnements anzeigen und filtern, indem Sie diese Schaltfläche auswählen. Die Anzeige wird angepasst, um den Sicherheitsstatus der ausgewählten Abonnements widerzuspiegeln.
- **Neuigkeiten**: Öffnet die [Versionshinweise](release-notes.md), damit Sie sich über den neuesten Stand der neuen Features, Fehlerbehebungen und veralteten Funktionen informieren können.
- **Allgemeine Zahlenwerte** für die verbundenen Cloudkonten, um den Kontext zu den Informationen in den darunter angeordneten Kacheln anzuzeigen. Darüber hinaus wird die Anzahl von aktiven Empfehlungen und Warnungen angezeigt.
    Informieren Sie sich darüber, wie Sie Ihre [AWS-Konten](quickstart-onboard-aws.md) und [GCP-Projekte](quickstart-onboard-gcp.md) verbinden.


In der Mitte der Seite befinden sich **vier zentrale Kacheln**, die jeweils mit einem dedizierten Dashboard mit weiteren Details verlinkt sind:
- **Sicherheitsbewertung**: Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko. [Weitere Informationen](secure-score-security-controls.md)
- **Compliance**: Security Center stellt basierend auf fortlaufenden Bewertungen Ihrer Azure-Umgebung Informationen zu Ihrem Konformitätsstatus (Compliance) bereit. Mit Security Center werden Risikofaktoren in Ihrer Hybrid Cloud-Umgebung anhand der bewährten Methoden für die Sicherheit analysiert. Diese Bewertungen sind Konformitätskontrollen zugeordnet, die aus dem unterstützten Standardsatz stammen. [Erfahren Sie mehr](security-center-compliance-dashboard.md).
- **Azure Defender**: Dies ist die in Security Center integrierte Cloud Workload Protection Platform (CWPP), die einen erweiterten, intelligenten Schutz Ihrer Azure- und Hybridworkloads ermöglicht. Auf der Kachel wird die Abdeckung für Ihre verbundenen Ressourcen (für die derzeit ausgewählten Abonnements) und die letzten Warnungen angezeigt, die je nach Schweregrad mit einer bestimmten Farbe gekennzeichnet sind. [Weitere Informationen](azure-defender.md)
- **Bestand**: Auf der Kachel werden die Anzahl von nicht überwachten VMs und ein einfaches Barometer für Ihre Ressourcen angezeigt, die mit Security Center überwacht werden. [Weitere Informationen](asset-inventory.md)


Der Bereich **Insights** enthält angepasste Elemente für Ihre Umgebung, die beispielsweise folgende Informationen liefern:
- Ihre Ressourcen mit den meisten Angriffen
- Ihre Sicherheitskontrollen mit dem höchsten Potenzial für die Erhöhung Ihrer Sicherheitsbewertung
- Aktive Empfehlungen mit den meisten betroffenen Ressourcen
- Aktuelle Blogbeiträge von Azure Security Center-Experten

## <a name="next-steps"></a>Nächste Schritte

Auf dieser Seite wurde die Übersichtsseite von Security Center beschrieben. Verwandte Informationen

- [Untersuchen und Verwalten Ihrer Ressourcen mit dem Ressourcenbestand und Verwaltungstools](asset-inventory.md)
- [Secure Score in Azure Security Center](secure-score-security-controls.md)