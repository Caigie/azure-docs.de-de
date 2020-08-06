---
title: Übersicht über virtuelle Computer der HC-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Unterstützung der Vorschauversion für die VM-Größe der HC-Serie in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 7110f3417937b623260983a9d94e9e6834fc8fc9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077370"
---
# <a name="hc-series-virtual-machine-overview"></a>Übersicht über virtuelle Computer der HC-Serie

Die Maximierung der HPC-Anwendungsleistung auf skalierbaren Intel Xeon-Prozessoren erfordert ein durchdachtes Konzept für die Prozessplatzierung in dieser neuen Architektur. In diesem Artikel wird die entsprechende Implementierung auf virtuellen Computern der HC-Serie für HPC-Anwendungen erläutert. Der Begriff „pNUMA“ bezieht sich hier auf eine physische NUMA-Domäne; „vNUMA“ steht für eine virtualisierte NUMA-Domäne. Analog dazu bezieht sich der Begriff „pCore“ auf physische CPU-Kerne, während „vCore“ für virtualisierte CPU-Kerne verwendet wird.

Physisch handelt es sich bei einem HC-Server um zwei Intel Xeon Platinum 8168-CPUs mit jeweils 24 Kernen, wodurch sich eine Gesamtanzahl von 48 physischen Kernen ergibt. Jede CPU ist eine einzelne pNUMA-Domäne und hat einheitlichen Zugriff auf sechs DRAM-Kanäle. Intel Xeon Platinum-CPUs verfügen im Vergleich zu den Vorgängergenerationen über einen viermal größeren L2-Cache (256 KB/Kern -> 1 MB/Kern) sowie über einen kleineren L3-Cache im Vergleich zu früheren Intel-CPUs (2,5 MB/Kern -> 1,375 MB/Kern).

Die obige Topologie hat auch Auswirkungen auf die Hypervisorkonfiguration der HC-Serie. Damit der Azure-Hypervisor über genügend Platz verfügt, um ohne Beeinträchtigung des virtuellen Computers agieren zu können, werden die physischen Kerne 0–1 und 24–25 (also jeweils die ersten beiden physischen Kerne der Sockets) reserviert. Anschließend werden pNUMA-Domänen aller verbleibenden Kerne dem virtuellen Computer zugewiesen. Für den virtuellen Computer steht somit Folgendes zur Verfügung:

`(2 vNUMA domains) * (22 cores/vNUMA) = 44` Kerne pro virtuellem Computer

Dem virtuellen Computer ist nicht bekannt, dass ihm die physischen Kerne 0–1 und 24–25 nicht zugewiesen wurden. Die einzelnen vNUMA-Instanzen werden daher so verfügbar gemacht, als stünden nativ nur 22 Kerne zur Verfügung.

Bei Intel Xeon-CPUs vom Typ Platinum, Gold und Silver wird auch ein 2D-On-Die-Meshnetzwerk für die interne und externe Kommunikation mit dem CPU-Socket eingeführt. Aus Leistungs- und Konsistenzgründen wird dringend empfohlen, Prozesse fest zuzuordnen. Eine feste Prozesszuordnung funktioniert bei virtuellen Computern der HC-Serie, da der zugrunde liegende Chip unverändert für den virtuellen Gastcomputer verfügbar gemacht wird. Weitere Informationen finden Sie in diesem Artikel zur [Intel Xeon-SP-Architektur](https://bit.ly/2RCYkiE) (in englischer Sprache).

Das folgende Diagramm zeigt die Trennung zwischen den Kernen, die für den Azure-Hypervisor reserviert sind, und den Kernen für den virtuellen Computer der HC-Serie:

![Trennung zwischen den Kernen, die für den Azure-Hypervisor reserviert sind, und den Kernen für den virtuellen Computer der HC-Serie](./media/hc-series-overview/segregation-cores.png)

## <a name="hardware-specifications"></a>Hardwarespezifikationen

| Hardwarespezifikationen          | Virtueller Computer der HC-Serie                     |
|----------------------------------|----------------------------------|
| Kerne                            | 44 (HT deaktiviert)                 |
| CPU                              | Intel Xeon Platinum 8168*        |
| CPU-Frequenz (ohne AVX)          | 3,7 GHz (einzelner Kern), 2,7 bis 3,4 GHz (alle Kerne) |
| Arbeitsspeicher                           | 8 GB/Kern (insgesamt 352 GB)            |
| Lokaler Datenträger                       | 700 GB NVMe                      |
| InfiniBand                       | 100 GBit EDR Mellanox ConnectX-5** |
| Netzwerk                          | 50-GBit-Ethernet (davon 40 GBit nutzbar); Azure-SmartNIC der zweiten Generation*** |

## <a name="software-specifications"></a>Softwarespezifikationen

| Softwarespezifikationen     | Virtueller Computer der HC-Serie          |
|-----------------------------|-----------------------|
| Maximale MPI-Auftragsgröße            | 13200 Kerne (300 VMs in einer einzelnen VMSS mit singlePlacementGroup=true) |
| MPI-Unterstützung                 | MVAPICH2, OpenMPI, MPICH, Platform MPI, Intel MPI  |
| Zusätzliche Frameworks       | Unified Communication X, libfabric, PGAS |
| Azure Storage-Unterstützung       | Standard und Premium (max. vier Datenträger) |
| Betriebssystemunterstützung für SR-IOV/RDMA   | CentOS/RHEL 7.6+, SLES 12 SP4+, WinServer 2016+ |
| Azure CycleCloud-Unterstützung    | Ja                         |
| Azure Batch-Unterstützung         | Ja                         |

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über HPC-VM-Größen für [Linux](../../sizes-hpc.md) und [Windows](../../sizes-hpc.md) in Azure.

* Informieren Sie sich ausführlicher über [HPC](/azure/architecture/topics/high-performance-computing/) in Azure.
