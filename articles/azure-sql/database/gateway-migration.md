---
title: Hinweis zur Migration des Gatewaydatenverkehrs
description: Dieser Artikel enthält Informationen zur Migration der IP-Adressen von Azure SQL-Datenbank-Gateways
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1 
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: f5e45a4625b1cf9422f7ef7e10e9080a7878172d
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84028751"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Migration des Azure SQL-Datenbank-Datenverkehrs zu neueren Gateways
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Die Azure-Infrastruktur wird kontinuierlich verbessert, und Microsoft aktualisiert immer wieder die Hardware, um das bestmögliche Kundenerlebnis sicherzustellen. In den kommenden Monaten ist geplant, in einigen Regionen Gateways hinzuzufügen, die auf neueren Hardware-Generationen basieren, Datenverkehr zu diesen Gateways zu migrieren und schließlich Gateways außer Betrieb zu nehmen, die auf älterer Hardware basieren.  

Kunden werden per E-Mail und im Azure-Portal rechtzeitig vor jeder Änderung der in jeder Region verfügbaren Gateways informiert. Aktuelle Informationen finden Sie in der Tabelle mit den [Gateway-IP-Adressen von Azure SQL-Datenbank](connectivity-architecture.md#gateway-ip-addresses).

## <a name="impact-of-this-change"></a>Auswirkungen dieser Änderung

Die erste Datenverkehrsmigration zu neueren Gateways ist für den **14. Oktober 2019** in den folgenden Regionen geplant:

- Brasilien Süd
- USA (Westen)
- Europa, Westen
- East US
- USA (Mitte)
- Südostasien
- USA Süd Mitte
- Nordeuropa
- USA Nord Mitte
- Japan, Westen
- Japan, Osten
- USA (Ost) 2
- Asien, Osten

Bei der Migration des Datenverkehrs wird die öffentliche IP-Adresse geändert, die das DNS für SQL-Datenbank auflöst.
Sie spüren die Auswirkungen in den folgenden Fällen:

- Die IP-Adresse für ein bestimmtes Gateway in Ihrer lokalen Firewall ist hartcodiert.
- Sie verfügen über Subnetze, die Microsoft.SQL als Dienstendpunkt verwenden, aber nicht mit den Gateway-IP-Adressen kommunizieren können.

In den folgenden Fällen spüren Sie die Auswirkungen nicht:

- Sie verwenden Umleitung als Verbindungsrichtlinie.
- Sie nutzen Verbindungen mit SQL-Datenbank aus Azure heraus und über Service Tags.
- Verbindungen, die mithilfe unterstützter Versionen des JDBC-Treibers für SQL Server hergestellt werden, sind nicht betroffen. Informationen zu unterstützten JDBC-Versionen finden Sie unter [Herunterladen des Microsoft JDBC-Treibers für SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Vorgehensweise, wenn Sie betroffen sind

Es wird empfohlen, ausgehenden Datenverkehr zu IP-Adressen für alle [Gateway-IP-Adressen](connectivity-architecture.md#gateway-ip-addresses) in der Region an TCP-Port 1433 und im Portbereich 11000–11999 zulassen. Diese Empfehlung bezieht sich auf Clients, die Verbindungen von lokalen Standorten herstellen, und solche, die Verbindungen über Dienstendpunkte herstellen. Weitere Informationen zu Portbereichen finden Sie unter [Verbindungsarchitektur von Azure SQL](connectivity-architecture.md#connection-policy).

Bei Verbindungen von Anwendungen, die eine ältere Version des Microsoft JDBC-Treibers als 4.0 verwenden, können Fehler bei der Zertifikatüberprüfung auftreten. Bei älteren Versionen von Microsoft JDBC muss das Feld „Antragsteller“ des Zertifikats einen allgemeinen Namen (Common Name, CN) enthalten. Stellen Sie sicher, dass die Eigenschaft „hostNameInCertificate“ auf „*.database.windows.net“ festgelegt ist, um dieses Problem zu umgehen. Weitere Informationen zum Festlegen der Eigenschaft „hostNameInCertificate“ finden Sie unter [Herstellen von Verbindungen mit einer Verschlüsselung](/sql/connect/jdbc/connecting-with-ssl-encryption).

Wenn die oben genannte Lösung nicht funktioniert, stellen Sie unter der folgenden URL eine Supportanfrage für SQL-Datenbank oder SQL Managed Instance: https://aka.ms/getazuresupport.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Verbindungsarchitektur von Azure SQL](connectivity-architecture.md).
