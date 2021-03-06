---
title: Konfigurieren von Advanced Threat Protection
titleSuffix: Azure SQL Managed Instance
description: Advanced Threat Protection identifiziert anomale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank in Azure SQL Managed Instance hinweisen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 09/21/2020
ms.openlocfilehash: d81550f5c6906e409675002fde670db3734de883
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90885064"
---
# <a name="configure-advanced-threat-protection-in-azure-sql-managed-instance"></a>Konfigurieren von Advanced Threat Protection in Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Advanced Threat Protection](../database/threat-detection-overview.md) für eine [Azure SQL Managed Instance](sql-managed-instance-paas-overview.md) identifiziert anomale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder sie missbräuchlich zu verwenden. Mithilfe von Advanced Threat Protection können folgende Aktivitäten identifiziert werden: **potenzielle SQL-Einschleusung**, **Zugriff von einem ungewöhnlichen Standort oder Rechenzentrum**, **Zugriff über einen unbekannten Prinzipal oder eine potenziell schädliche Anwendung** und **Brute-Force-SQL-Anmeldeinformationen**. Weitere Informationen finden Sie im Abschnitt zu den [Advanced Threat Protection-Warnungen](../database/threat-detection-overview.md#alerts).

Sie können sich über [E-Mail-Benachrichtigungen](../database/threat-detection-overview.md#explore-detection-of-a-suspicious-event) oder im [Azure-Portal](../database/threat-detection-overview.md#explore-alerts-in-the-azure-portal) über die erkannten Bedrohungen benachrichtigen lassen.

[Advanced Threat Protection](../database/threat-detection-overview.md) ist Teil des [Azure Defender für SQL](../database/azure-defender-for-sql.md)-Angebots. Dabei handelt es sich um ein vereinheitlichtes Paket für erweiterte SQL-Sicherheitsfunktionen. Der Zugriff auf Advanced Threat Protection und dessen Verwaltung sind über das zentrale Azure Defender für SQL-Portal möglich.

##  <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Navigieren Sie zur Konfigurationsseite der Instanz von SQL Managed Instance, die geschützt werden soll. Wählen Sie unter **Sicherheit** die Option **Security Center**aus.
3. Auf der Konfigurationsseite für Azure Defender für SQL
   - Aktivieren**** Sie Azure Defender für SQL.
   - Konfigurieren Sie die **Liste der E-Mail-Empfänger**, die bei Erkennung anomaler Datenbankaktivitäten Sicherheitswarnungen erhalten sollen.
   - Wählen Sie das **Azure Storage-Konto** aus, in dem Überwachungsdatensätze zu anomalen Bedrohungen gespeichert werden.
   - Wählen Sie die **Advanced Threat Protection-Typen** aus, die Sie konfigurieren möchten. Erfahren Sie mehr über [Advanced Threat Protection-Warnungen](../database/threat-detection-overview.md).
4. Klicken Sie auf **Speichern**, um die neue oder aktualisierte Azure Defender für SQL-Richtlinie zu speichern.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Advanced Threat Protection](../database/threat-detection-overview.md).
- Informationen zu verwalteten Instanzen finden Sie unter [Was ist Azure SQL Managed Instance?](sql-managed-instance-paas-overview.md)
- Erfahren Sie mehr über [Advanced Threat Protection für Azure SQL-Datenbank](../database/threat-detection-configure.md).
- Weitere Informationen zur [Überwachung von SQL Managed Instance](https://go.microsoft.com/fwlink/?linkid=869430).
- Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
