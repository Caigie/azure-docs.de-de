---
title: Verbindungsarchitektur
description: In diesem Artikel wird die Azure SQL-Verbindungsarchitektur für interne und externe Datenbankverbindungen von Azure erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: fasttrack-edit
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: 112dbea4ef54c5923c586b87be9770c2e91befd2
ms.sourcegitcommit: 0fda81f271f1a668ed28c55dcc2d0ba2bb417edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2020
ms.locfileid: "82901432"
---
# <a name="azure-sql-connectivity-architecture"></a>Verbindungsarchitektur von Azure SQL
> [!NOTE]
> Dieser Artikel gilt für den Azure SQL-Datenbankserver sowie für Datenbanken von SQL-Datenbank und SQL Data Warehouse, die auf dem Azure SQL-Datenbankserver erstellt werden. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

> [!IMPORTANT]
> Dieser Artikel gilt *nicht* für **verwaltete Azure SQL-Datenbank-Instanzen**. Informationen dazu finden Sie unter [Konnektivitätsarchitektur für eine verwaltete Instanz](sql-database-managed-instance-connectivity-architecture.md).

In diesem Artikel wird die Architektur verschiedener Komponenten erläutert, die den Netzwerkdatenverkehr an Azure SQL-Datenbank oder SQL Data Warehouse weiterleiten. Außerdem werden andere Verbindungsrichtlinien und ihre Auswirkung auf Clients beschrieben, die eine Verbindung innerhalb und außerhalb von Azure herstellen. 

## <a name="connectivity-architecture"></a>Verbindungsarchitektur

Das folgende Diagramm bietet einen allgemeinen Überblick über die Verbindungsarchitektur von Azure SQL-Datenbank.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-overview.png)

In den folgenden Schritten wird das Herstellen einer Verbindung mit einer Azure SQL-Datenbank beschrieben:

- Clients stellen eine Verbindung mit dem Gateway her, das über eine öffentliche IP-Adresse verfügt und an Port 1433 lauscht.
- Je nach effektiver Verbindungsrichtlinie leitet das Gateway den Datenverkehr an den richtigen Datenbankcluster um oder fungiert als Proxy.
- Innerhalb des Datenbankclusters wird der Datenverkehr an die entsprechende Azure SQL-Datenbank weitergeleitet.

## <a name="connection-policy"></a>Verbindungsrichtlinie

Die Azure SQL-Datenbank unterstützt diese drei Optionen zum Festlegen der Verbindungsrichtlinie für einen SQL-Datenbank-Server:

- **Umleiten (empfohlen):** Clients stellen Verbindungen direkt mit dem Knoten her, der die Datenbank hostet. Dies führt zu geringerer Latenz und verbessertem Durchsatz. Damit dieser Modus bei Verbindungen verwendet wird, müssen Clients
   - die ausgehende Kommunikation zwischen dem Client und allen IP-Adressen für Azure SQL in der Region an Ports im Bereich zwischen 11000 und 11999 zulassen. die Diensttags für SQL verwenden, um die Verwaltung zu vereinfachen.  
   - die ausgehende Kommunikation zwischen dem Client und den IP-Adressen des Azure SQL-Datenbank-Gateways an Port 1433 zulassen.

- **Proxy:** In diesem Modus werden alle Verbindungen über die Azure SQL-Datenbank-Gateways geleitet. Dies führt zu höherer Latenz und geringerem Durchsatz. Damit dieser Modus bei Verbindungen verwendet wird, müssen Clients die ausgehende Kommunikation zwischen dem Client und den IP-Adressen des Azure SQL-Datenbank-Gateways an Port 1433 zulassen.

- **Standardwert:** Diese Verbindungsrichtlinie ist nach dem Erstellen auf allen Servern aktiv, es sei denn, Sie ändern sie explizit in `Proxy` oder `Redirect`. Die Standardrichtlinie für alle Clientverbindungen, die innerhalb von Azure hergestellt werden (z. B. über einen virtuellen Azure-Computer), ist `Redirect`. Für alle Clientverbindungen, die außerhalb von Azure hergestellt werden (z. B. über Ihre lokale Arbeitsstation), lautet sie `Proxy`.

 Im Hinblick auf die niedrigere Latenz und den höheren Durchsatz wird dringend empfohlen, die Verbindungsrichtlinie `Redirect` der Verbindungsrichtlinie `Proxy` vorzuziehen. Es müssen jedoch auch die oben genannten Anforderungen erfüllt werden, sodass Netzwerkdatenverkehr zulässig ist. Wenn es sich bei dem Client um einen virtuellen Azure-Computer handelt, lässt sich dies mithilfe von Netzwerksicherheitsgruppen (NSG) mit [Diensttags](../virtual-network/security-overview.md#service-tags) erreichen. Wenn der Client eine Verbindung über eine lokale Arbeitsstation herstellt, müssen Sie sich möglicherweise an den Netzwerkadministrator wenden, um Netzwerkdatenverkehr über die Unternehmensfirewall zuzulassen.

## <a name="connectivity-from-within-azure"></a>Verbindung aus Azure

Wenn Sie eine Verbindung aus Azure herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie `Redirect`. Die Verbindungsrichtlinie `Redirect` bedeutet, dass nach Aufbau der TCP-Sitzung mit Azure SQL-Datenbank die Clientsitzung an den richtigen Datenbankcluster umgeleitet wird. Dabei wird die virtuelle Ziel-IP von der des Gateways für Azure SQL-Datenbank in die des Clusters geändert. Danach fließen alle nachfolgenden Pakete direkt an den Cluster, wobei das Gateway von Azure SQL-Datenbank umgangen wird. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Verbindung von außerhalb von Azure

Wenn Sie von außerhalb von Azure eine Verbindung herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie `Proxy`. Die Richtlinie `Proxy` bedeutet, dass die TCP-Sitzung über das Gateway von Azure SQL-Datenbank hergestellt wird und dass alle nachfolgenden Pakete über das Gateway fließen. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> Öffnen Sie zusätzlich die TCP-Ports 1434 und 14000 bis 14999, um das [Herstellen einer dedizierten Administratorverbindung](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) zu ermöglichen.


## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-Adressen vom Gateway von Azure SQL-Datenbank

Die folgende Tabelle enthält die IP-Adressen der Gateways nach Region. Wenn Sie eine Verbindung mit einer Azure SQL-Datenbank herstellen möchten, müssen Sie ein- und ausgehenden Netzwerkdatenverkehr für **alle** Gateways der Region zulassen.

Details zum Migrieren von Datenverkehr zu neuen Gateways in bestimmten Regionen finden Sie im folgenden Artikel: [Azure SQL Database Traffic-Migration auf neuere Gateways](sql-database-gateway-migration.md).


| Name der Region          | Gateway-IP-Adressen |
| --- | --- |
| Australien, Mitte    | 20.36.105.0 |
| Australien, Mitte 2   | 20.36.113.0 |
| Australien (Osten)       | 13.75.149.87, 40.79.161.1 |
| Australien, Südosten | 191.239.192.109, 13.73.109.251 |
| Brasilien Süd         | 104.41.11.5, 191.233.200.14 |
| Kanada, Mitte       | 40.85.224.249      |
| Kanada, Osten          | 40.86.226.166      |
| USA (Mitte)           | 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96, 104.208.21.1 | 
| China, Osten           | 139.219.130.35     |
| China, Osten 2         | 40.73.82.1         |
| China, Norden          | 139.219.15.17      |
| China, Norden 2        | 40.73.50.0         |
| Asien, Osten            | 191.234.2.139, 52.175.33.150, 13.75.32.4 |
| East US              | 40.121.158.30, 40.79.153.12, 191.238.6.43, 40.78.225.32 |
| USA (Ost) 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0,  191.239.224.107, 104.208.150.3 | 
| Frankreich, Mitte       | 40.79.137.0, 40.79.129.1 |
| Deutschland, Mitte      | 51.4.144.100       |
| Deutschland, Nordosten   | 51.5.144.179       |
| Indien, Mitte        | 104.211.96.159     |
| Indien, Süden          | 104.211.224.146    |
| Indien, Westen           | 104.211.160.80     |
| Japan, Osten           | 13.78.61.196, 40.79.184.8, 191.237.240.43, 40.79.192.5 | 
| Japan, Westen           | 104.214.148.156, 40.74.100.192, 191.238.68.11, 40.74.97.10 | 
| Korea, Mitte        | 52.231.32.42       |
| Korea, Süden          | 52.231.200.86      |
| USA Nord Mitte     | 23.96.178.199, 23.98.55.75, 52.162.104.33 |
| Nordeuropa         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| Norwegen, Osten          | 51.120.96.0        |
| Norwegen, Westen          | 51.120.216.0       |
| Südafrika, Norden   | 102.133.152.0      |
| Südafrika, Westen    | 102.133.24.0       |
| USA Süd Mitte     | 13.66.62.124, 23.98.162.75, 104.214.16.32   | 
| Südostasien      | 104.43.15.0, 23.100.117.95, 40.78.232.3   | 
| VAE, Mitte          | 20.37.72.64        |
| Vereinigte Arabische Emirate, Norden            | 65.52.248.0        |
| UK, Süden             | 51.140.184.11      |
| UK, Westen              | 51.141.8.11        |
| USA, Westen-Mitte      | 13.78.145.25       |
| Europa, Westen          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| USA (Westen)              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| USA, Westen 2            | 13.66.226.202      |
|                      |                    |



## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbank-Server finden Sie unter [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Weitere Informationen zum Verbindungsverhalten von Azure SQL-Datenbank für Kunden, die ADO.NET 4.5 oder höher verwenden, finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Einen allgemeinen Überblick über die Anwendungsentwicklung finden Sie unter [SQL-Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md).
