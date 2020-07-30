---
title: Aktivieren von Azure Monitor für VMs mit Azure Policy
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs für mehrere Azure-VMs oder VM-Skalierungsgruppen mithilfe von Azure Policy aktivieren.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 7d3c4e0f4bd34f996bb39426af39a692a6f79c5c
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298367"
---
# <a name="enable-azure-monitor-for-vms-by-using-azure-policy"></a>Aktivieren von Azure Monitor für VMs mit Azure Policy

In diesem Artikel erfahren Sie, wie Sie Azure Monitor für VMs mithilfe von Azure Policy für Azure-VMs, Azure-VM-Skalierungsgruppen und Azure Arc-Computer aktivieren. Am Ende dieses Prozesses haben Sie die Aktivierung der Log Analytics- und Dependency-Agents erfolgreich konfiguriert und nicht konforme virtuelle Computer identifiziert.

Um Azure Monitor für VMs für alle Ihre Azure-VMs oder VM-Skalierungsgruppen zu erkennen, zu verwalten und zu aktivieren, können Sie entweder Azure Policy oder Azure PowerShell verwenden. Azure Policy ist die von uns empfohlene Methode, da Sie Richtliniendefinitionen so verwalten können, dass Ihre Abonnements effektiv gesteuert werden, um eine durchgängige Konformität und automatische Aktivierung neu bereitgestellter VMs zu gewährleisten. Aufgaben dieser Richtliniendefinitionen:

* Stellen den Log Analytics-Agent und Dependency-Agent bereit
* Melden Konformitätsergebnisse
* Beheben nicht konforme VMs

Wenn Sie diese Aufgaben mit Azure PowerShell oder einer Azure Resource Manager-Vorlage ausführen möchten, lesen Sie [Aktivieren von Azure Monitor für VMs mit Azure PowerShell oder einer Resource Manager-Vorlage](vminsights-enable-at-scale-powershell.md).

## <a name="prerequisites"></a>Voraussetzungen
Vor der Verwendung einer Richtlinie zum Integrieren Ihrer Azure-VMS und VM-Skalierungsgruppen in die Azure-Überwachung für VMs müssen Sie die VMInsights-Lösung in dem Arbeitsbereich aktivieren, den Sie zum Speichern der Überwachungsdaten verwenden möchten. Diese Aufgabe kann über die Seite **Erste Schritte** in Azure Monitor auf der Registerkarte **Andere Onboardingoptionen** abgeschlossen werden.  Wählen Sie die Option **Konfigurieren eines Arbeitsbereichs** aus, woraufhin Sie aufgefordert werden, den zu konfigurierenden Arbeitsbereich auszuwählen.

![Konfigurieren des Arbeitsbereichs](media/vminsights-enable-at-scale-policy/configure-workspace.png)

Sie können den Arbeitsbereich auch durch Auswahl von **Über Richtlinie aktivieren** und dann Auswahl der Symbolleistenschaltfläche **Arbeitsbereich konfigurieren** konfigurieren.  Dann wird die VMInsights-Lösung im ausgewählten Arbeitsbereich installiert, sodass der Arbeitsbereich die von den virtuellen Computern und VM-Skalierungsgruppen, die Sie mithilfe der Richtlinie aktivieren, gesendeten Überwachungsdaten speichern kann. 

![Über Richtlinie aktivieren](media/vminsights-enable-at-scale-policy/enable-using-policy.png)

## <a name="manage-policy-coverage-feature-overview"></a>Übersicht über das Feature „Richtlinienabdeckung“

Die Richtlinienabdeckung von Azure Monitor für VMs vereinfacht die bedarfsorientierte Erkennung, Verwaltung und Aktivierung der Initiative **Aktivieren von Azure Monitor für VMs**, die die zuvor genannten Richtliniendefinitionen einschließt. Um auf diese Funktion zuzugreifen, wählen Sie **Andere Onboardingoptionen** auf der Registerkarte **Erste Schritte** in Azure Monitor für VMS aus. Wählen Sie **Richtlinienabdeckung verwalten** aus, um die Seite **Azure Monitor für VMs – Richtlinienabdeckung** zu öffnen.

![Azure Monitor für VMs-Registerkarte „Erste Schritte“](./media/vminsights-enable-at-scale-policy/get-started-page.png)

Hier können Sie die Abdeckung der Initiative für alle Ihre Verwaltungsgruppen und Abonnements überprüfen und verwalten. Sie können auch erfahren, wie viele VMs in den einzelnen Verwaltungsgruppen und Abonnements vorhanden sind und welchen Konformitätsstatus sie haben.

![Azure Monitor für VMs-Seite „Richtlinienabdeckung“](media/vminsights-enable-at-scale-policy/manage-policy-page-01.png)

Diese Informationen sind nützlich, um Ihr Governance-Szenario für Azure Monitor für VMs an einem zentralen Ort zu planen und zu realisieren. Während Azure Policy eine Konformitätsansicht bereitstellt, sobald eine Richtlinie oder Initiative einem Geltungsbereich zugeordnet ist, können Sie auf dieser neuen Seite feststellen, wo die Richtlinie oder Initiative nicht zugewiesen ist, und sie direkt zuweisen. Bei allen Aktionen wie dem Zuweisen, Anzeigen und Bearbeiten erfolgt eine direkte Umleitung zu Azure Policy. Die Seite **Azure Monitor für VMs – Richtlinienabdeckung** ist eine erweiterte und integrierte Umgebung ausschließlich für die Initiative **Aktivieren von Azure Monitor für VMs**.

Auf dieser Seite können Sie auch Ihren Log Analytics-Arbeitsbereich für Azure Monitor für VMs konfigurieren, wo die *VMInsights*-Lösung installiert wird.

![Azure Monitor für VMs, Arbeitsbereich konfigurieren](media/vminsights-enable-at-scale-policy/manage-policy-page-02.png)

Diese Option bezieht sich nicht auf Richtlinienaktionen. Sie dient zur Bereitstellung einer einfachen Möglichkeit, die [Voraussetzungen](vminsights-enable-overview.md) für die Aktivierung von Azure Monitor für VMs zu erfüllen.  

### <a name="what-information-is-available-on-this-page"></a>Welche Informationen sind auf dieser Seite verfügbar?

Die folgende Tabelle enthält eine Aufschlüsselung der auf der Seite „Richtlinienabdeckung“ gezeigten Informationen und ihrer Interpretation.

| Funktion | BESCHREIBUNG | 
|----------|-------------| 
| **Umfang** | Verwaltungsgruppen und Abonnements, auf die Sie Zugriff haben oder für die Sie den Zugriff geerbt haben, mit der Möglichkeit, die Hierarchie der Verwaltungsgruppen aufzuschlüsseln.|
| **Rolle** | Ihre Rolle für den Umfang, die entweder „Leser“, „Besitzer“ oder „Mitwirkender“ sein kann. In einigen Fällen kann das Feld leer sein, um anzuzeigen, dass Sie zwar Zugriff auf das Abonnement haben, aber nicht auf die Verwaltungsgruppe, zu der es gehört. Die Informationen in anderen Spalten sind je nach Rolle verschieden. Die Rolle ist entscheidend für das Festlegen der Daten, die Sie sehen können, und der Aktionen, die Sie durchführen können, wenn Sie Richtlinien oder Initiativen (Besitzer) zuweisen, bearbeiten oder die Konformität prüfen. |
| **VMs gesamt** | Die Anzahl der VMs in diesem Umfang. Für eine Verwaltungsgruppe ist dies eine Summe von VMs, die unter den Abonnements oder der untergeordneten Verwaltungsgruppe geschachtelt sind. |
| **Zuweisungsabdeckung** | Prozentsatz der VMs, die durch die Richtlinie oder Initiative abgedeckt sind. |
| **Zuweisungsstatus** | Informationen zum Status Ihrer Richtlinien- oder Intitiativenzuweisung. |
| **Konforme VMs** | Anzahl der VMs, die gemäß der Richtlinie oder Initiative konform sind. Für die Initiative **Aktivieren von Azure Monitor für VMs** ist dies die Anzahl der VMs mit sowohl dem Log Analytics-Agent als auch dem Dependency-Agent. In einigen Fällen kann das Feld leer sein, da keine Zuweisung, keine VMs oder nicht genügend Berechtigungen vorhanden sind. Informationen werden unter **Konformitätszustand** angezeigt. |
| **Compliance** | Der Gesamtwert für die Konformität ist die Summe der einzelnen Ressourcen, die konform sind, geteilt durch die Summe aller einzelnen Ressourcen. |
| **Konformitätszustand** | Informationen zum Konformitätszustand für Ihre Richtlinien- oder Initiativenzuweisung.|

Wenn Sie die Richtlinie oder Initiative zuweisen, kann der in der Zuweisung ausgewählte Umfang der aufgelistete Umfang oder eine Teilmenge davon sein. Angenommen, Sie haben eine Zuweisung für ein Abonnement (Richtlinienumfang) und nicht für eine Verwaltungsgruppe (Abdeckungsumfang) vorgenommen. In diesem Fall gibt der Wert von **Zuweisungsabdeckung** die VMs im Umfang der Richtlinie oder Initiative dividiert durch die VMs im Abdeckungsumfang an. In einem anderen Fall haben Sie möglicherweise einige VMs, Ressourcengruppen oder ein Abonnement aus dem Richtlinienumfang ausgeschlossen. Wenn der Wert leer ist, bedeutet dies, dass entweder die Richtlinie oder Initiative nicht vorhanden ist oder Sie keine Berechtigung haben. Informationen werden unter **Zuweisungsstatus** angezeigt.

## <a name="enable-by-using-azure-policy"></a>Aktivieren mithilfe von Azure Policy

So aktivieren Sie Azure Monitor für VMs mithilfe von Azure Policy in Ihrem Mandanten

- Zuweisen der Initiative zu einem Bereich: Verwaltungsgruppe, Abonnement oder Ressourcengruppe
- Überprüfen und Beheben der Konformitätsergebnisse

Weitere Informationen zur Zuweisung von Azure Policy finden Sie unter [Azure Policy – Übersicht](../../governance/policy/overview.md#assignments), und arbeiten Sie die [Übersicht zu Verwaltungsgruppen](../../governance/management-groups/overview.md) durch, bevor Sie fortfahren.

### <a name="policies-for-azure-vms"></a>Richtlinien für Azure-VMs

Die Richtliniendefinitionen für eine Azure-VM sind in der folgenden Tabelle aufgeführt.

|Name |BESCHREIBUNG |type |
|-----|------------|-----|
|Aktivieren von Azure Monitor für VMs |Hiermit aktivieren Sie Azure Monitor für die virtuellen Computer in dem angegebenen Bereich (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. |Initiative |
|Überwachen der Bereitstellung des Dependency-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Dependency-Agents für Linux-VMs |Hiermit stellen Sie den Dependency-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Dependency-Agents für Windows-VMs |Hiermit stellen Sie den Dependency-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Log Analytics-Agents für Linux-VMs |Hiermit stellen Sie den Log Analytics-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Log Analytics-Agents für Windows-VMs |Hiermit stellen Sie den Log Analytics-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |


### <a name="policies-for-hybrid-azure-arc-machines"></a>Richtlinien für Azure Arc-Hybridcomputer

Die Richtliniendefinitionen für Azure Arc-Hybridcomputer sind in der folgenden Tabelle aufgeführt.

|Name |BESCHREIBUNG |type |
|-----|------------|-----|
| [Vorschau]: Log Analytics agent should be installed on your Linux Azure Arc machines (Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein) |Bei dieser Richtlinie werden Azure Arc-Hybridcomputer, bei denen es sich um Linux-VMs handelt, als nicht konform gemeldet, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
| [Vorschau]: Log Analytics agent should be installed on your Windows Azure Arc machines (Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein) |Bei dieser Richtlinie werden Azure Arc-Hybridcomputer, bei denen es sich um Windows-VMs handelt, als nicht konform gemeldet, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
| [Vorschau]: Deploy Dependency agent to hybrid Linux Azure Arc machines (Dependency-Agent für Linux-basierte Azure Arc-Hybridcomputer bereitstellen) |Hiermit stellen Sie den Dependency-Agent für Linux-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
| [Vorschau]: Deploy Dependency agent to hybrid Windows Azure Arc machines (Dependency-Agent für Windows-basierte Azure Arc-Hybridcomputer bereitstellen) |Hiermit stellen Sie den Dependency-Agent für Windows-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
| [Vorschau]: Deploy Log Analytics agent to Linux Azure Arc machines (Log Analytics-Agent für Linux-basierte Azure Arc-Computer bereitstellen) |Hiermit stellen Sie den Log Analytics-Agent für Linux-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
| [Vorschau]: Deploy Log Analytics agent to Windows Azure Arc machines (Log Analytics-Agent für Windows-basierte Azure Arc-Computer bereitstellen) |Hiermit stellen Sie den Log Analytics-Agent für Windows-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |


### <a name="policies-for-azure-virtual-machine-scale-sets"></a>Richtlinien für Azure-VM-Skalierungsgruppen

Die Richtliniendefinitionen für eine Azure-VM-Skalierungsgruppe sind in der folgenden Tabelle aufgeführt.

|Name |BESCHREIBUNG |type |
|-----|------------|-----|
|Aktivieren von Azure Monitor für VM-Skalierungsgruppen |Hiermit aktivieren Sie Azure Monitor für VM-Skalierungsgruppen im angegebenen Umfang (Verwaltungsgruppe, Abonnement oder Ressourcengruppe). Akzeptiert den Log Analytics-Arbeitsbereich als Parameter. Hinweis: Wenn die Upgraderichtlinie für Skalierungsgruppen auf „Manuell“ festgelegt ist, wenden Sie die Erweiterung auf alle VMs in der Gruppe an, indem Sie für diese ein Upgrade durchführen. In der CLI lautet der Befehl `az vmss update-instances`. |Initiative |
|Überwachen der Bereitstellung des Dependency-Agents in VM-Skalierungsgruppen – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VM-Skalierungsgruppen als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Überwachen der Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VM-Skalierungsgruppen als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Dependency-Agents für Linux-VM-Skalierungsgruppen |Hiermit stellen Sie den Dependency-Agent für Linux-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Dependency-Agents für Windows-VM-Skalierungsgruppen |Hiermit stellen Sie den Dependency-Agent für Windows-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Log Analytics-Agents für Linux-VM-Skalierungsgruppen |Hiermit stellen Sie den Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |
|Bereitstellen des Log Analytics-Agents für Windows-VM-Skalierungsgruppen |Hiermit stellen Sie den Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |Richtlinie |

Die eigenständige Richtlinie (nicht in der Initiative enthalten) wird hier beschrieben:

|Name |BESCHREIBUNG |type |
|-----|------------|-----|
|Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt |Meldet VMs als nicht konform, wenn sie keine Protokolle an den in der Richtlinien- oder Initiativenzuweisung angegebenen Log Analytics-Arbeitsbereich senden. |Richtlinie |

### <a name="assign-the-azure-monitor-initiative"></a>Zuweisen der Azure Monitor-Initiative

Um die Richtlinienzuweisung auf der Seite **Azure Monitor für VMs – Richtlinienabdeckung** zu erstellen, führen Sie die folgenden Schritte aus. Informationen zum Ausführen dieser Schritte finden Sie unter  [Erstellen einer Richtlinienzuweisung im Azure-Portal](../../governance/policy/assign-policy-portal.md).

Wenn Sie die Richtlinie oder Initiative zuweisen, kann der in der Zuweisung ausgewählte Umfang der hier aufgelistete Umfang oder eine Teilmenge davon sein. Angenommen, Sie haben eine Zuweisung für das Abonnement (Richtlinienumfang) und nicht für die Verwaltungsgruppe (Abdeckungsumfang) vorgenommen. In diesem Fall gibt der Abdeckungsprozentsatz die VMs im Umfang der Richtlinie oder Initiative dividiert durch die VMs im Abdeckungsumfang an. In einem anderen Fall haben Sie möglicherweise einige VMs oder Ressourcengruppen oder ein Abonnement aus dem Richtlinienumfang ausgeschlossen. Wenn der Wert leer ist, bedeutet dies, dass entweder die Richtlinie oder Initiative nicht vorhanden ist oder Sie keine Berechtigungen haben. Informationen werden unter **Zuweisungsstatus** angezeigt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im Azure-Portal die Option **Überwachen** aus. 

3. Wählen Sie im Bereich **Insights** die Option **Virtuelle Computer** aus.
 
4. Wählen Sie die Registerkarte **Erste Schritte** aus. Wählen Sie auf der Seite die Option **Richtlinienabdeckung verwalten** aus.

5. Wählen Sie entweder eine Verwaltungsgruppe oder ein Abonnement in der Tabelle aus. Wählen Sie den **Bereich** aus, indem Sie auf die Auslassungspunkte (...) klicken. In dem Beispiel schränkt ein Bereich die Richtlinienzuweisung zur Durchsetzung auf eine Gruppierung virtueller Computer ein.

6. Auf der Seite **Azure Policy-Zuweisung** ist sie mit der Initiative **Aktivieren von Azure Monitor für VMs** vorab ausgefüllt. 
    Das Feld **Zuweisungsname** wird automatisch mit dem Namen der Initiative aufgefüllt, den Sie aber ändern können. Geben Sie ggf. auch eine Beschreibung ein. Das Feld **Zugewiesen von** wird automatisch mit dem angemeldeten Benutzer ausgefüllt. Dieser Wert ist optional.

7. (Optional) Um eine oder mehrere Ressourcen aus dem Bereich zu entfernen, wählen Sie **Ausschlüsse** aus.

8. Wählen Sie in der Dropdownliste **Log Analytics-Arbeitsbereich** für die unterstützte Region einen Arbeitsbereich aus.

   > [!NOTE]
   > Wenn der Arbeitsbereich außerhalb des Bereichs der Zuweisung liegt, erteilen Sie der Prinzipal-ID der Richtlinienzuweisung *Log Analytics-Mitwirkender*-Berechtigungen. Geschieht dies nicht, tritt möglicherweise ein Fehler bei der Bereitstellung auf, z.B. `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...` Lesen Sie zum Erteilen des Zugriffs, [wie Sie die verwaltete Identität manuell konfigurieren](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  Das Kontrollkästchen **Verwaltete Identität** ist aktiviert, da die zugewiesene Initiative eine Richtlinie mit dem Effekt *deployIfNotExists* umfasst.
    
9. Wählen Sie in der Dropdownliste **Manage Identity location** (Speicherort der Identität verwalten) die passende Region aus.

10. Wählen Sie **Zuweisen** aus.

Nachdem Sie die Zuweisung erstellt haben, werden auf der Seite **Azure Monitor für VMs – Richtlinienabdeckung** die Angaben **Zuweisungsabdeckung**, **Zuweisungsstatus**, **Konforme VMs** und **Konformitätszustand** aktualisiert, um die Änderungen widerzuspiegeln. 

In der folgenden Matrix sind der Initiative alle möglichen Konformitätszustände zugeordnet.  

| Konformitätszustand | BESCHREIBUNG | 
|------------------|-------------|
| **Konform** | Auf allen VMs im Umfang sind der Log Analytics- und der Dependency-Agent bereitgestellt.|
| **Nicht konform** | Nicht auf allen VMs im Umfang sind der Log Analytics- und der Dependency-Agent bereitgestellt, was möglicherweise Korrekturen erforderlich macht.|
| **Nicht gestartet** | Eine neue Zuweisung wurde hinzugefügt. |
| **Sperre** | Sie haben nicht genügend Rechte für die Verwaltungsgruppe.<sup>1</sup> | 
| **Leer** | Es ist keine Richtlinie zugewiesen. | 

<sup>1</sup> Wenn Sie keinen Zugriff auf die Verwaltungsgruppe haben, bitten Sie einen Besitzer, Ihnen Zugriff zu gewähren. Sie können auch die Konformität anzeigen und Zuweisungen über die untergeordneten Verwaltungsgruppen oder Abonnements verwalten. 

In der folgenden Tabelle ist der Initiative jeder mögliche Zuweisungsstatus zugeordnet.

| Zuweisungsstatus | BESCHREIBUNG | 
|------------------|-------------|
| **Erfolgreich** | Auf allen VMs im Umfang sind der Log Analytics- und der Dependency-Agent bereitgestellt.|
| **Warning** | Das Abonnement gehört zu keiner Verwaltungsgruppe.|
| **Nicht gestartet** | Eine neue Zuweisung wurde hinzugefügt. |
| **Sperre** | Sie haben nicht genügend Rechte für die Verwaltungsgruppe.<sup>1</sup> | 
| **Leer** | Es sind keine VMs vorhanden, oder es wurde keine Richtlinie zugewiesen. | 
| **Aktion** | „Richtlinie zuweisen“ oder „Zuweisung bearbeiten“. | 

<sup>1</sup> Wenn Sie keinen Zugriff auf die Verwaltungsgruppe haben, bitten Sie einen Besitzer, Ihnen Zugriff zu gewähren. Sie können auch die Konformität anzeigen und Zuweisungen über die untergeordneten Verwaltungsgruppen oder Abonnements verwalten.

## <a name="review-and-remediate-the-compliance-results"></a>Überprüfen und Warten der Compliance-Ergebnisse

Das folgende Beispiel gilt für eine Azure-VM, trifft aber auch auf VM-Skalierungsgruppen zu. Informationen zum Überprüfen von Konformitätsergebnissen finden Sie unter [Identifizieren nicht konformer Ressourcen](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Wählen Sie auf der Seite **Azure Monitor für VMs – Richtlinienabdeckung** in der Tabelle entweder eine Verwaltungsgruppe oder ein Abonnement aus. Wählen Sie **Konformität anzeigen** aus, indem Sie auf die Auslassungszeichen (...) klicken.   

![Richtlinienkonformität für Azure-VMs](./media/vminsights-enable-at-scale-policy/policy-view-compliance.png)

Auf der Grundlage der in der Initiative enthaltenen Richtlinien werden VMs in den folgenden Szenarien als nicht konform gemeldet:

* Der Log Analytics-Agent oder der Dependency-Agent wurde nicht bereitgestellt.  
    Dieses Szenario ist typisch für einen Bereich mit vorhandenen virtuellen Computern. Um das Problem abzumildern, [erstellen Sie Wartungstasks](../../governance/policy/how-to/remediate-resources.md) für eine nicht konforme Richtlinie, um die erforderlichen Agents bereitzustellen.  
    - Bereitstellen des Dependency-Agents für Linux-VMs
    - Bereitstellen des Dependency-Agents für Windows-VMs
    - Bereitstellen des Log Analytics-Agents für Linux-VMs
    - Bereitstellen des Log Analytics-Agents für Windows-VMs

* Das VM-Image (Betriebssystem) wird in der Richtliniendefinition nicht identifiziert.  
    Die Kriterien der Bereitstellungsrichtlinie schließen nur VMs ein, die aus bekannten Azure VM-Images bereitgestellt werden. Überprüfen Sie anhand der Dokumentation, ob das Betriebssystem der VM unterstützt wird. Ist das nicht der Fall, müssen Sie die Bereitstellungsrichtlinie duplizieren und sie aktualisieren oder ändern, damit das Image konform wird.  
    - Überwachen der Bereitstellung des Dependency-Agents – VM-Image (Betriebssystem) nicht aufgelistet
    - Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet

* VMs melden sich nicht am angegebenen Log Analytics-Arbeitsbereich an.  
    Es ist möglich, dass sich einige VMs im Bereich der Initiative bei einem anderen Log Analytics-Arbeitsbereich anmelden, als dem, der in der Richtlinienzuweisung angegeben ist. Diese Richtlinie ist ein Tool, um zu bestimmen, welche VMs an einen nicht kompatiblen Arbeitsbereich berichten.  
    - Überwachen des Log Analytics-Arbeitsbereichs für VM – Berichtskonflikt

## <a name="edit-an-initiative-assignment"></a>Bearbeiten einer Initiativenzuweisung

Nachdem Sie einer Verwaltungsgruppe oder einem Abonnement eine Initiative zugewiesen haben, können Sie diese jederzeit bearbeiten, indem Sie die folgenden Eigenschaften ändern:

- Zuweisungsname
- BESCHREIBUNG
- Zugewiesen von
- Log Analytics-Arbeitsbereich
- Ausnahmen

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Überwachung für Ihre virtuellen Computer aktiviert wurde, stehen diese Informationen für die Analyse mit Azure Monitor für VMs zur Verfügung. 

- Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Azure Monitor für VMs – Zuordnung anzeigen](vminsights-maps.md). 

- Informationen zum Erkennen von Engpässen und der Gesamtauslastung im Hinblick auf die Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md). 
