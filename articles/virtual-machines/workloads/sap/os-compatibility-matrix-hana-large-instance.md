---
title: Betriebssystem-Kompatibilitätsmatrix für SAP HANA (große Instanzen) | Microsoft-Dokumentation
description: In der Kompatibilitätsmatrix wird die Kompatibilität verschiedener Betriebssystemversionen mit verschiedenen Hardwaretypen (große Instanzen) dargestellt.
services: virtual-machines-linux
documentationcenter: ''
author: sasarava
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2020
ms.author: sasarava
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3fbc6c7b8811f3cf46b4f31387c2181c8d085e39
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684883"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>Kompatible Betriebssysteme für HANA (große Instanzen)

## <a name="hana-large-instance-type-i"></a>Große HANA-Instanz, Typ I     
  | Betriebssystem | Verfügbarkeit        | SKUs                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Wird nicht mehr angeboten | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Verfügbar           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |

  
### <a name="persistent-memory-skus"></a>SKUs für persistenten Speicher
  | Betriebssystem | Verfügbarkeit | SKUs                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Verfügbar    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>Große HANA-Instanz, Typ II     
  |  Betriebssystem       | Verfügbarkeit        | SKUs                                                              |
  |-------------------------|---------------------|-------------------------------------------------------------------|
  | SLES 12 SP2             | Wird nicht mehr angeboten | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP3             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP4             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP5             | Verfügbar           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  
## <a name="related-documents"></a>Verwandte Dokumente

- Weitere Informationen zu verfügbaren SKUs finden Sie [hier](hana-available-skus.md).
- Informationen zum Upgraden des Betriebssystems finden Sie [hier](os-upgrade-hana-large-instance.md).
  

  
  
