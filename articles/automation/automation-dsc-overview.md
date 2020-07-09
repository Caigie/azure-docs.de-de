---
title: Übersicht über Azure Automation State Configuration
description: Dieser Artikel enthält eine Übersicht über Azure Automation State Configuration.
keywords: PowerShell DSC, Desired State Configuration, Konfiguration des gewünschten Zustands, PowerShell DSC Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9880915061c0639aebe30bdb33258d7c79e155d7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836888"
---
# <a name="azure-automation-state-configuration-overview"></a>Übersicht über Azure Automation State Configuration

Azure Automation State Configuration ist ein Azure-Dienst für die Konfigurationsverwaltung, mit dem Sie PowerShell DSC-[Konfigurationen](/powershell/scripting/dsc/configurations/configurations) (Desired State Configuration) schreiben, verwalten und kompilieren können – für Knoten in der Cloud oder in einem lokalen Rechenzentrum. Darüber hinaus ermöglicht der Dienst in der Cloud einen Import von [DSC-Ressourcen](/powershell/scripting/dsc/resources/resources) und die Zuweisung von Konfigurationen zu Zielknoten. Sie können im Azure-Portal auf Azure Automation State Configuration zugreifen, indem Sie unter **Konfigurationsverwaltung** die Option **State Configuration (DSC)** auswählen. 

Mit Azure Automation State Configuration können zahlreiche Computer verwaltet werden:

- Virtuelle Azure-Computer
- Virtuelle Azure-Computer (klassisch)
- Physische/virtuelle Windows-Computer, die lokal oder in einer anderen Cloud als Azure (einschließlich AWS-EC2-Instanzen) ausgeführt werden
- Physische/virtuelle Linux-Computer, lokal, in Azure oder in einer anderen Cloud als Azure

Wenn Sie Computerkonfigurationen noch nicht in der Cloud verwalten möchten, können Sie Azure Automation State Configuration auch ausschließlich als Endpunkt für Berichte verwenden. Mit diesem Feature können Sie Konfigurationen mittels DSC festlegen (pushen) und Berichterstellungsdetails in Azure Automation anzeigen.

> [!NOTE]
> Das Verwalten von Azure-VMs mit Azure Automation State Configuration ist kostenlos möglich, wenn die installierte Version der Azure-VM-Erweiterung für DSC höher als 2.70 ist. Weitere Informationen hierzu finden Sie unter [**Automation – Preise**](https://azure.microsoft.com/pricing/details/automation/).

## <a name="why-use-azure-automation-state-configuration"></a>Gründe für Azure Automation State Configuration

Azure Automation State Configuration bietet verschiedene Vorteile gegenüber der Verwendung von DSC außerhalb von Azure. Mit diesem Dienst können Sie Skalierbarkeit über Tausende von Computern schnell und einfach von einem zentralen, sicheren Ort aus erreichen. Sie können problemlos Computer aktivieren, ihnen deklarative Konfigurationen zuweisen und Berichte dazu anzeigen, inwieweit jeder Computer mit dem gewünschten Zustand, den Sie angegeben haben, kompatibel ist.

Der Azure Automation DSC-Dienst ist für DSC, was die Azure Automation-Runbooks für PowerShell-Skripts sind. So hilft Azure Automation auf die gleiche Weise wie beim Verwalten von PowerShell-Skripts bei der Verwaltung von DSC-Konfigurationen. 

### <a name="built-in-pull-server"></a>Integrierter Pullserver

Azure Automation State Configuration bietet einen DSC-Pullserver, der mit dem [Windows-Feature DSC-Dienst](/powershell/scripting/dsc/pull-server/pullserver) vergleichbar ist. Zielknoten können Konfigurationen automatisch empfangen, die dem gewünschten Zustand entsprechen, und Informationen zur Konformität melden. Durch den integrierten Pullserver in Azure Automation entfällt die Notwendigkeit zur Einrichtung und Verwaltung Ihres eigenen Pullservers. Azure Automation eignet sich für virtuelle oder physische Windows- oder Linux-Computer in Cloud- oder lokalen Umgebungen.

### <a name="management-of-all-your-dsc-artifacts"></a>Verwalten aller DSC-Artefakte

Azure Automation State Configuration führt dieselbe Verwaltungsschicht in [PowerShell Desired State Configuration](/powershell/scripting/dsc/overview/overview) ein, die auch für die PowerShell-Skripterstellung verfügbar ist. Sie können Ihre gesamten DSC-Konfigurationen, -Ressourcen und -Zielknoten über das Azure-Portal oder über PowerShell verwalten.

![Screenshot der Azure Automation-Seite](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Importieren von Berichtsdaten in Azure Monitor-Protokolle

Mit Azure Automation State Configuration verwaltete Knoten senden detaillierte Statusberichtsdaten an den integrierten Pullserver. Sie können Azure Automation State Configuration so konfigurieren, dass diese Daten an Ihren Log Analytics-Arbeitsbereich gesendet werden. Weitere Informationen finden Sie unter [Weiterleiten von Berichtsdaten von Azure Automation State Configuration an Azure Monitor-Protokolle](automation-dsc-diagnostics.md).

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Voraussetzungen für die Verwendung von Azure Automation State Configuration

Berücksichtigen Sie die in diesem Abschnitt beschriebenen Anforderungen, wenn Sie Azure Automation State Configuration verwenden.

### <a name="operating-system-requirements"></a>Betriebssystemanforderungen

Für Knoten, auf denen Windows ausgeführt wird, werden folgende Versionen unterstützt:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

>[!NOTE]
>Die eigenständige Produkt-SKU von [Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) umfasst keine Implementierung von DSC. Sie kann daher nicht über PowerShell DSC oder Azure Automation State Configuration verwaltet werden.

Für Linux-Knoten unterstützt die DSC Linux-Erweiterung alle unter [Unterstützte Linux-Distributionen](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions) aufgeführten Linux-Distributionen.

### <a name="dsc-requirements"></a>DSC-Anforderungen

Für alle Windows-Knoten, die in Azure ausgeführt werden, wird beim Aktivieren der Computer [WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) installiert. Für Knoten, die unter Windows Server 2012 und Windows 7 ausgeführt werden, wird [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) aktiviert.

Für alle Linux-Knoten, die in Azure ausgeführt werden, wird beim Aktivieren der Computer [PowerShell DSC for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) installiert.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Konfiguration privater Netzwerke

Wenn sich Ihre Knoten in einem privaten Netzwerk befinden, sind die folgenden Ports und URLs erforderlich. Durch diese Ressourcen wird die Netzwerkkonnektivität für den verwalteten Knoten bereitgestellt. Zudem ermöglichen sie die Kommunikation von DSC mit Azure Automation.

* Port: Für ausgehenden Zugriff auf das Internet ist nur TCP 443 erforderlich
* Globale URL: * **.azure-automation.net**
* Globale URL von „US Gov Virginia“: * **.azure-automation.us**
* Agentdienst: **https://\<workspaceId\>.agentsvc.azure-automation.net**

Wenn Sie DSC-Ressourcen verwenden, die zwischen Knoten kommunizieren (z. B. [WaitFor*-Ressourcen](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)), müssen Sie auch Datenverkehr zwischen Knoten zulassen. Informationen zu diesen Netzwerkanforderungen finden Sie in der Dokumentation für die einzelnen DSC-Ressourcen.

#### <a name="proxy-support"></a>Proxyunterstützung

Proxyunterstützung für den DSC-Agent ist in Windows, Version 1809 und höher, verfügbar. Diese Option wird aktiviert, indem die Werte für die Eigenschaften `ProxyURL` und `ProxyCredential` im [Metakonfigurationsskript](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) festgelegt werden, das zum Registrieren von Knoten verwendet wird. 

>[!NOTE]
>Azure Automation State Configuration bietet keine DSC-Proxyunterstützung für frühere Windows-Versionen.

Bei Linux-Knoten unterstützt der DSC-Agent den Proxy und bestimmt die URL anhand der Variablen `http_proxy`. Weitere Informationen zur Unterstützung von Proxys finden Sie unter [Generieren von DSC-Metakonfigurationen](automation-dsc-onboarding.md#generate-dsc-metaconfigurations).

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Azure Automation State Configuration-Netzwerkbereiche und -Namespace

Es wird empfohlen, beim Definieren von Ausnahmen die unten aufgeführten Adressen zu verwenden. Für IP-Adressen können Sie die [IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) herunterladen. Diese Datei mit den jeweils aktuellen bereitgestellten Bereichen und allen anstehenden Änderungen an den IP-Adressbereichen wird wöchentlich veröffentlicht.

Wenn eines Ihrer Automation-Konten für eine bestimmte Region definiert ist, können Sie die Kommunikation mit diesem regionalen Rechenzentrum einschränken. Die folgende Tabelle enthält den DNS-Eintrag für jede Region:

| **Region** | **DNS-Eintrag** |
| --- | --- |
| USA, Westen-Mitte | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| USA Süd Mitte |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| East US    | eus-jobruntimedata-prod-su1.azure-automation.net</br>eus-agentservice-prod-1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Indien, Mitte |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan, Osten |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australien, Südosten |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| UK, Süden | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Government, Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Wenn Sie eine Liste mit IP-Adressen der Regionen anstelle ihrer Namen benötigen, können Sie die XML-Datei mit den [IP-Adressen der Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) aus dem Microsoft Download Center herunterladen.

> [!NOTE]
> Die XML-Datei mit den IP-Adressen der Azure-Rechenzentren enthält die IP-Adressbereiche, die in den Microsoft Azure-Rechenzentren verwendet werden. Die Datei enthält die Bereiche für Compute, SQL und Storage.
>
>Eine aktualisierte Datei wird wöchentlich veröffentlicht. Die Datei enthält die derzeit bereitgestellten Bereichen und alle anstehenden Änderungen an den IP-Adressbereichen. In der Datei enthaltene neue Bereiche werden frühestens nach einer Woche in den Rechenzentren verwendet. Sie sollten die neue XML-Datei jede Woche herunterladen. Führen Sie anschließend eine Aktualisierung für Ihren Standort durch, um in Azure ausgeführte Dienste ordnungsgemäß zu identifizieren. 

Benutzer von Azure ExpressRoute sollten beachten, dass diese Datei zum Aktualisieren der BGP-Ankündigung (Border Gateway Protocol) von Azure-Bereichen jeweils in der ersten Woche des Monats verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Informationen zum Aktivieren von Knoten finden Sie unter [Aktivieren von Azure Automation State Configuration](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und sie anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation State Configuration](automation-dsc-compile.md).
- Ein Anwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Einrichten von Continuous Deployment mit Chocolatey](automation-dsc-cd-chocolatey.md).
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).