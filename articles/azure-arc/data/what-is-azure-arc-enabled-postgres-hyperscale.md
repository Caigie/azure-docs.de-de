---
title: Was ist eine Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe?
description: Was ist eine Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe?
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 7e8746d9b29b0b4af30ba799628328acc3fd8bc0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931280"
---
# <a name="what-is-azure-arc-enabled-postgresql-hyperscale"></a>Was ist eine Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe?

Eine Azure Arc-aktivierte PostgreSQL Hyperscale-Servergruppe ist einer der Datenbankdienste, die als Teil von Azure Arc-fähigen Datendiensten verfügbar sind. Azure Arc ermöglicht das lokale Ausführen von Azure Data Services in Edge-Umgebungen und in öffentlichen Clouds mithilfe von Kubernetes und der Infrastruktur Ihrer Wahl. Das Wertversprechen von Azure Arc hat Datendienste ermöglicht, die sich um die folgenden Funktionen drehen:
- Immer aktuell
- Elastische Skalierung
- Self-Service-Bereitstellung
- Einheitliche Verwaltung
- Unterstützung für nicht verbundenes Szenario

Weitere Informationen finden Sie in den folgenden Themen:
- [Was sind Azure Arc-fähige Datendienste?](overview.md)
- [Konnektivitätsmodi und -anforderungen](connectivity.md)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="compare-solutions"></a>Vergleichen von Lösungen

In diesem Abschnitt wird beschrieben, wie Azure sich Arc-aktivierte PostgreSQL Hyperscale-Servergruppen von Azure Database for PostgreSQL Hyperscale (Citus) unterscheidet.

## <a name="azure-database-for-postgresql-hyperscale-citus"></a>Azure Database for PostgreSQL Hyperscale (Citus)

:::image type="content" source="media/postgres-hyperscale/postgresql-hyperscale.png" alt-text="Azure SQL Database for PostgreSQL Hyperscale (Citus)":::

Dies ist der Hyperscale-Formfaktor der Postgres-Datenbank-Engine, der als Database-as-a-Service in Azure (PaaS) verfügbar ist. Er wird von der Citus-Erweiterung unterstützt, die die Hyperscale-Umgebung ermöglicht. In diesem Formfaktor wird der Dienst in den Microsoft-Rechenzentren ausgeführt und von Microsoft betrieben.

## <a name="azure-arc-enabled-postgresql-hyperscale"></a>PostgreSQL Hyperscale mit Azure Arc-Aktivierung

:::image type="content" source="media/postgres-hyperscale/postgresql-hyperscale-arc.png" alt-text="PostgreSQL Hyperscale mit Azure Arc-Aktivierung":::

Dies ist der Hyperscale-Formfaktor der Postgres-Datenbank-Engine, der mit dem Azure Arc-fähigen Datendienst verfügbar ist. Er wird auch von der Citus-Erweiterung unterstützt, die die Hyperscale-Umgebung ermöglicht. In diesem Formfaktor stellen unsere Kunden die Infrastruktur bereit, die die Systeme hostet, und betreiben sie.

## <a name="next-steps"></a>Nächste Schritte
- **Erstellen**
   > **Möchten Sie diese Funktionen ausprobieren?**  
   > Mit dem [Azure Arc-Schnelleinstieg](https://github.com/microsoft/azure_arc#azure-arc-enabled-data-services) für Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) oder auf einer Azure-VM können Sie schnell die ersten Schritte unternehmen.

   - **Erstellen:**
      - [Installieren der Clienttools](install-client-tools.md)
      - [Erstellen des Azure Arc-Datencontrollers](create-data-controller.md) (erfordert zuvor die Installation der Clienttools)
      - [Erstellen einer Azure Database for PostgreSQL Hyperscale-Servergruppe in Azure Arc](create-postgresql-hyperscale-server-group.md) (erfordert zuvor die Erstellung eines Azure Arc-Datencontrollers)
- [**Weitere Informationen zu Azure Arc-fähigen Datendiensten**](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
- [**Weitere Informationen zu Azure Arc**](https://aka.ms/azurearc)
