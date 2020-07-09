---
title: Dokumentationsänderungen für SQL Server auf virtuellen Azure-Computern | Microsoft-Dokumentation
description: Erfahren Sie mehr über die neuen Funktionen und Verbesserungen für SQL Server auf Azure-VMs.
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/06/2020
ms.openlocfilehash: f87d72df977e98c60948389d794eb102ac08f8d2
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84032471"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Dokumentationsänderungen für SQL Server auf virtuellen Azure-Computern
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

In Azure können Sie einen virtuellen Computer (VM) mit einem integrierten SQL Server-Image bereitstellen. In diesem Artikel werden die Dokumentationsänderungen zusammengefasst, die mit neuen Funktionen und Verbesserungen in den neuesten Releases von [SQL Server in Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) in Zusammenhang stehen. 


## <a name="january-2020"></a>Januar 2020

| Änderungen | Details |
| --- | --- |
| **Azure Government-Support** | Es ist jetzt möglich, SQL Server-VMs mit dem SQL-VM-Ressourcenanbieter für virtuelle Computer zu registrieren, der in der [Azure Government](https://azure.microsoft.com/global-infrastructure/government/)-Cloud gehostet wird. | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Änderungen | Details |
 --- | --- |
| **Kostenloses DR-Replikat in Azure** | Sie können eine [kostenlose passive Instanz](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) für die Notfallwiederherstellung in Azure für Ihre lokale SQL Server-Instanz hosten, wenn Sie über [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3)verfügen. | 
| **Massenressourcen-Anbieterregistrierung** | Sie können jetzt eine [Massenregistrierung](sql-vm-resource-provider-bulk-register.md) für virtuelle SQL-Computer mit dem Ressourcenanbieter durchführen. | 
|**Leistungsoptimierte Speicherkonfiguration** | Sie ist nun möglich, Ihre [Speicherkonfiguration vollständig anzupassen](storage-configuration.md#new-vms), wenn Sie eine neue SQL Server-VM erstellen. |
|**Premium-Dateifreigabe für FCI** | Sie können nun eine Failoverclusterinstanz mit einer [Premium-Dateifreigabe](failover-cluster-instance-premium-file-share-manually-configure.md) anstelle der ursprünglichen Methode für [direkte Speicherplätze](failover-cluster-instance-storage-spaces-direct-manually-configure.md) erstellen. 
| **Dedizierter Azure-Host** | Sie können Ihre SQL Server-VM auf einem [dedizierten Azure-Host](dedicated-host.md) ausführen. | 
| **Verschieben einer SQL-VM in eine andere Region** | Verwenden Sie Azure Site Recovery, um [Ihre SQL Server-VM aus einer Region in eine andere zu migrieren](move-sql-vm-different-region.md). |
|  **Neue SQL-IaaS-Installationsmodi** | Die SQL Server-IaaS-Erweiterung kann nun im [Lightweight-Modus](sql-server-iaas-agent-extension-automate-management.md) installiert werden, um den SQL Server-Dienst nicht neu starten zu müssen.  |
| **Änderung der SQL Server-Edition** | Sie können nun die [Editionseigenschaft](change-sql-server-edition.md) für Ihren virtuellen SQL Server-Computer ändern. |
| **Änderungen am SQL-VM-Ressourcenanbieter** | Sie können [Ihren virtuellen SQL Server-Computer beim SQL-VM-Ressourcenanbieter registrieren](sql-vm-resource-provider-register.md) und dabei die neuen SQL-IaaS-Modi verwenden. Diese Möglichkeit umfasst auch Images mit [Windows Server 2008](sql-vm-resource-provider-register.md#management-modes).|
| **Bring-Your-Own-License-Images mit dem Azure-Hybridvorteil** | Bei über den Azure Marketplace bereitgestellten Bring-Your-Own-License-Images kann der [Lizenztyp nun in nutzungsbasierte Bezahlung](licensing-model-azure-hybrid-benefit-ahb-change.md#remarks) geändert werden.| 
| **Neue SQL Server-VM-Verwaltung im Azure-Portal** | Es gibt jetzt eine Methode, mit der Sie Ihre SQL Server-VM im Azure-Portal verwalten können. Weitere Informationen finden Sie unter [Verwalten von SQL Server-VMs über das Azure-Portal](manage-sql-vm-portal.md).  | 
| **Erweiterte Unterstützung für SQL Server 2008/2008 R2** | [Erweiterung der Unterstützung](sql-server-2008-extend-end-of-support.md) für SQL Server 2008 und SQL Server 2008 R2 durch *unverändertes* Migrieren zu einer Azure-VM. | 
| **Unterstützung von benutzerdefinierten Images** | Sie können jetzt die [SQL Server-IaaS-Erweiterung](sql-server-iaas-agent-extension-automate-management.md#installation) für benutzerdefinierte Betriebssystem- und SQL-Images installieren, die die eingeschränkte Funktionalität der [flexiblen Lizenzierung](licensing-model-azure-hybrid-benefit-ahb-change.md) bietet. Geben Sie beim Registrieren eines benutzerdefinierten SQL Server-VM-Images beim Ressourcenanbieter als Lizenztyp „AHUB“ an. Andernfalls tritt bei der Registrierung ein Fehler auf. | 
| **Unterstützung für benannte Instanzen** | Sie können jetzt die [SQL Server-IaaS-Erweiterung](sql-server-iaas-agent-extension-automate-management.md#installation) mit einer benannten Instanz verwenden, wenn die Standardinstanz ordnungsgemäß deinstalliert wurde. | 
| **Portalerweiterung** | Die Azure-Portal-Benutzeroberfläche zur Bereitstellung einer SQL Server-VM wurde überarbeitet, um die Benutzerfreundlichkeit zu verbessern. Weitere Informationen finden Sie in Kurzform im [Schnellstart](sql-vm-create-portal-quickstart.md) und ausführlicher in der [Schrittanleitung](create-sql-vm-portal.md) zum Bereitstellen einer SQL Server-VM.|
| **Verbesserung beim Portal** | Es ist jetzt möglich, mithilfe des [Azure-Portals](licensing-model-azure-hybrid-benefit-ahb-change.md#vms-already-registered-with-the-resource-provider) das Lizenzierungsmodell für einen virtuellen SQL Server-Computer von nutzungsbasierter Bezahlung in Bring-Your-Own-License zu ändern.|
| **Vereinfachung der Bereitstellung von Verfügbarkeitsgruppen mit der Azure SQL Server-VM-CLI** | Es ist jetzt einfacher denn je, eine Verfügbarkeitsgruppe auf einer SQL Server-VM in Azure bereitzustellen. Sie können mit der [Azure-Befehlszeilenschnittstelle](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) den Windows-Failovercluster, den internen Lastenausgleich und die Verfügbarkeitsgruppenlistener über die Befehlszeile erstellen. Weitere Informationen finden Sie unter [Verwenden der Azure SQL Server-VM-CLI zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einem virtuellen Azure-Computer](availability-group-az-cli-configure.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Änderungen | Details |
| --- | --- |
|  **Neuer Ressourcenanbieter für SQL Server-Cluster** | Ein neuer Ressourcenanbieter (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) definiert die Metadaten des Windows-Failoverclusters. Ein Einbinden einer SQL Server-VM in die *SqlVirtualMachineGroups* startet den Windows Server-Failovercluster-Dienst (WSFC) und verknüpft den virtuellen Computer mit dem Cluster.  |
| **Automatisiertes Einrichten einer Verfügbarkeitsgruppenbereitstellung mit Azure-Schnellstartvorlagen** |Es ist jetzt möglich, mit zwei Azure-Schnellstartvorlagen den Windows-Failovercluster zu erstellen, SQL Server-VMs in diesen einzubinden, den Listener zu erstellen und den internen Lastenausgleich zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden von Azure-Schnellstartvorlagen zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einem virtuellen Azure-Computer](availability-group-quickstart-template-configure.md). | 
| **Automatische Registrierung beim SQL-VM-Ressourcenanbieter** | SQL Server-VMs, die nach diesem Monat bereitgestellt werden, werden automatisch beim neuen SQL Server-Ressourcenanbieter registriert. SQL Server-VMs, die vor diesem Monat bereitgestellt wurden, müssen weiterhin manuell registriert werden. Weitere Informationen finden Sie unter [Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md).|
|**Neuer SQL-VM-Ressourcenanbieter** |  Ein neuer Ressourcenanbieter (Microsoft.SqlVirtualMachine) bietet eine bessere Verwaltung Ihrer SQL Server-VMs. Weitere Informationen zum Registrieren Ihrer VMs finden Sie unter [Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter](sql-vm-resource-provider-register.md). |
|**Wechsel des Lizenzierungsmodells** | Sie können nun mit der Azure-Befehlszeilenschnittstelle oder PowerShell für Ihre SQL Server-VM zwischen dem nutzungsbasierten Modell und dem Bring-Your-Own-License-Modell wechseln. Weitere Informationen finden Sie unter [Ändern des Lizenzierungsmodells für einen virtuellen SQL Server-Computer in Azure](licensing-model-azure-hybrid-benefit-ahb-change.md). | 
| &nbsp; | &nbsp; |

## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](create-sql-vm-portal.md)
* [Migrieren einer Datenbank zu SQL Server auf einem virtuellen Azure-Computer](migrate-to-vm-from-sql-server.md)
* [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Bewährte Methoden für die Leistung von SQL Server auf virtuellen Azure-Computern](performance-guidelines-best-practices.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf virtuellen Azure-Computern](application-patterns-development-strategies.md)

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](../linux/sql-vm-create-portal-quickstart.md)
* [Häufig gestellte Fragen (Linux)](../linux/frequently-asked-questions-faq.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
