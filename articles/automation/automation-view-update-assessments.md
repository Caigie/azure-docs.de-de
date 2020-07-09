---
title: Anzeigen von Azure Automation-Updatebewertungen
description: In diesem Artikel erfahren Sie, wie Sie Updatebewertungen für Bereitstellungen der Updateverwaltung anzeigen.
services: automation
ms.subservice: update-management
ms.date: 01/21/2020
ms.topic: conceptual
ms.openlocfilehash: 321146364897d46a403bdfd6789fcb219179d88c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830598"
---
# <a name="view-update-assessments"></a>Anzeigen der Updatebewertungen

Wählen Sie in Ihrem Azure Automation-Konto **Updateverwaltung** aus, um den Status Ihrer Computer anzuzeigen.

Diese Ansicht enthält Informationen zu Ihren Computern, zu fehlenden Updates, zu Updatebereitstellungen und geplanten Updatebereitstellungen. Die Spalte **KONFORMITÄT** gibt Aufschluss darüber, wann der Computer zuletzt bewertet wurde. Die Spalte **UPDATE-AGENT-BEREITSCHAFT** gibt Aufschluss über die Integrität des Update-Agents. Liegt ein Problem vor, wählen Sie den Link aus, um zur Dokumentation für die Problembehandlung zu navigieren und Informationen zur Behebung des Problems zu erhalten.

Wenn Sie eine Protokollsuche ausführen möchten, die Informationen zum Computer, zu Updates oder zu Bereitstellungen zurückgibt, wählen Sie das entsprechende Element in der Liste aus. Der Bereich „Protokollsuche“ wird mit einer Abfrage für das ausgewählte Element geöffnet.

![Standardansicht der Updateverwaltung](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Anzeigen fehlender Updates

Wählen Sie **Fehlende Updates** aus, um eine Liste der Updates anzuzeigen, die auf Ihrem Computer nicht vorhanden sind. Jedes Update wird aufgeführt und kann ausgewählt werden. Es werden Informationen zur Anzahl von Computern, die das Update benötigen, sowie Details zum Betriebssystem und ein Link zu weiteren Informationen angezeigt. Im Bereich „Protokollsuche“ werden weitere Details zu den Updates angezeigt.

![Fehlende Updates](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="work-with-update-classifications"></a>Verwenden von Updateklassifizierungen

Die folgenden Tabellen enthalten eine Liste der unterstützten Updateklassifizierungen in der Updateverwaltung sowie eine Definition für jede Klassifikation.

### <a name="windows"></a>Windows

|Klassifizierung  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates     | Updates für bestimmte Probleme, mit denen kritische, nicht sicherheitsrelevante Fehler behoben werden.        |
|Sicherheitsupdates     | Updates für produktspezifische, sicherheitsrelevante Probleme.        |
|Updaterollups     | Eine Gruppe von Hotfixes, die zur Vereinfachung der Bereitstellung gebündelt sind.        |
|Feature Packs     | Neue Produktfeatures, die nicht im Rahmen eines Produktreleases verteilt werden.        |
|Service Packs     | Eine Gruppe von Hotfixes, die auf eine Anwendung angewendet werden.        |
|Definitionsupdates     | Updates für Viren- oder andere Definitionsdateien.        |
|Tools     | Hilfsprogramme oder Features, mit denen mindestens eine Aufgabe ausgeführt werden kann.        |
|Aktualisierungen     | Updates für Anwendungen oder Dateien, die derzeit installiert sind.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|Klassifizierung  |BESCHREIBUNG  |
|---------|---------|
|Kritische Updates und Sicherheitsupdates     | Updates für ein spezielles oder produktspezifisches, sicherheitsrelevantes Problem.         |
|Andere Updates     | Alle anderen Updates, die nicht kritisch sind oder bei denen es sich nicht um Sicherheitsupdates handelt.        |

Für Linux kann die Updateverwaltung bei der Anzeige von Bewertungsdaten zwischen kritischen Updates und Sicherheitsupdates in der Cloud unterscheiden. (Diese Granularität wird durch die Datenanreicherung in der Cloud ermöglicht.) Für das Patchen verwendet die Updateverwaltung Klassifizierungsdaten, die auf dem Computer verfügbar sind. Im Gegensatz zu anderen Distributionen verfügt CentOS in den RTM-Versionen des Produkts standardmäßig nicht über diese Informationen. Wenn Sie CentOS-Computer so konfiguriert haben, dass für den folgenden Befehl Sicherheitsdaten zurückgegeben werden, kann die Updateverwaltung basierend auf Klassifizierungen Patchvorgänge ausführen.

```bash
sudo yum -q --security check-update
```

Aktuell steht keine unterstützte Methode zur Verfügung, mit der unter CentOS eine native Verfügbarkeit von Klassifizierungsdaten implementiert werden kann. Für Kunden, die diese Funktion möglicherweise selbst implementiert haben, wird derzeit nur Support nach bestem Wissen bereitgestellt.

Sie müssen das Plug-In „yum-security“ installieren, um Updates in Version 6 von Red Hat Enterprise zu klassifizieren. Unter Red Hat Enterprise Linux 7 ist das Plug-In bereits Teil von yum selbst. Sie müssen also nichts installieren. Weitere Informationen finden Sie in [diesem Red Hat-Knowledge-Artikel](https://access.redhat.com/solutions/10021).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
