---
title: Konfigurieren eines bestehenden virtuellen Netzwerks für eine verwaltete Instanz
description: In diesem Artikel wird beschrieben, wie Sie ein vorhandenes virtuelles Netzwerk und Subnetz konfigurieren, in dem Sie eine verwaltete Azure SQL-Datenbank-Instanz bereitstellen können.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 4e678edad2c59205e76598991b36d296404a3163
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773620"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Konfigurieren eines vorhandenen virtuellen Netzwerks für eine verwaltete Azure SQL-Datenbank-Instanz

Eine verwaltete Azure SQL-Datenbank-Instanz darf nur in einem [virtuellen Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) und in dem Subnetz bereitgestellt werden, das speziell für verwaltete Instanzen bestimmt ist. Sie können das vorhandene virtuelle Netzwerk und Subnetz verwenden, wenn diese jeweils gemäß den [Anforderungen an virtuelle Netzwerke für verwaltete Instanzen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) konfiguriert sind.

Wenn einer der folgenden Fälle auf Sie zutrifft, können Sie Ihr Netzwerk mithilfe des in diesem Artikel erläuterten Skripts überprüfen und ändern:

- Sie haben ein neues Subnetz, das noch nicht konfiguriert ist.
- Sie sind nicht sicher, ob das Subnetz den [Anforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) entspricht.
- Sie möchten überprüfen, ob das Subnetz weiterhin die [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) erfüllt, nachdem Sie Änderungen vorgenommen haben.

> [!Note]
> Sie können eine verwaltete Instanz nur in virtuellen Netzwerken erstellen, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Virtuelle Azure-Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, werden nicht unterstützt. Berechnen Sie die Subnetzgröße anhand der Richtlinien im Artikel [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](sql-database-managed-instance-determine-size-vnet-subnet.md). Die Größe des Subnetzes kann nach der Bereitstellung der darin enthaltenen Ressourcen nicht mehr geändert werden.
>
> Nachdem eine verwaltete Instanz erstellt wurde, wird das Verschieben dieser Instanz oder des VNET in eine andere Ressourcengruppe oder ein anderes Abonnement nicht unterstützt.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Überprüfen und Ändern eines vorhandenen virtuellen Netzwerks

Wenn Sie eine verwaltete Instanz in einem vorhandenen Subnetz erstellen möchten, empfehlen wir das folgende PowerShell-Skript zum Vorbereiten des Subnetzes:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/delegateSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

Mit dem Skript wird das Subnetz in drei Schritten vorbereitet:

1. Überprüfen: Das ausgewählte virtuelle Netzwerk und das Subnetz werden in Hinblick auf die Netzwerkanforderungen für verwaltete Instanzen überprüft.
2. Bestätigen: Dem Benutzer werden Änderungen angezeigt, die für die Vorbereitung des Subnetzes zur Bereitstellung der verwalteten Instanz vorgenommen werden müssen. Sie müssen dann diesen Änderungen zustimmen.
3. Vorbereiten: Das virtuelle Netzwerk und das Subnetz werden ordnungsgemäß konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial zum Erstellen eines virtuellen Netzwerks, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank aus einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Lesen Sie bei Problemen im Zusammenhang mit dem DNS den Artikel [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
