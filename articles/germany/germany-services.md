---
title: Azure Deutschland – Verfügbare Dienste | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die verfügbaren Dienste in Azure Deutschland.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: chsieg
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2019
ms.author: ralfwi
ms.openlocfilehash: d0c8295f0755b174251dfd2686a24cbdaf9d500f
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89442812"
---
# <a name="available-services-in-azure-germany"></a>Verfügbare Dienste in Azure Deutschland

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

Azure Deutschland aktualisiert und erweitert seine Dienste fortlaufend nach dem Evergreeningkonzept. Die Dienste werden mit demselben Code bereitgestellt, der in der globalen Azure-Umgebung verwendet wird, und nach Prüfung auf lokale Implementierbarkeit in Azure Deutschland eingepflegt. In diesem Artikel werden die Dienste beschrieben, die zurzeit in Azure Deutschland verfügbar sind. 

>[!NOTE]
> Die aktuelle Liste der Dienste finden Sie unter [Produkte nach Region](https://azure.microsoft.com/regions/services/). 
>
>

Die in den folgenden Tabellen als Azure Resource Manager-fähig angegebenen Dienste verfügen über Ressourcenanbieter und können mithilfe von PowerShell verwaltet werden. Ausführliche Informationen zu Resource Manager-Anbietern, API-Versionen und Schemas finden Sie unter [Unterstützte Resource Manager-Dienste](../azure-resource-manager/management/resource-providers-and-types.md). Die als im Portal verfügbar angegebenen Dienste können im [Azure Deutschland-Portal](https://portal.microsoftazure.de/) verwaltet werden. 

## <a name="compute"></a>[Compute](./germany-services-compute.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| [Virtuelle Computer](./germany-services-compute.md#virtual-machines)  | Ja | Ja |
| Virtual Machine Scale Sets | Ja | Ja |
| Service Fabric | Ja | Ja |


## <a name="networking"></a>[Netzwerk](./germany-services-networking.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| [ExpressRoute](./germany-services-networking.md#expressroute-private-connectivity) | Ja | Ja |
| Virtual Network | Ja | Ja |
| [Load Balancer](./germany-services-networking.md#support-for-load-balancer) | Ja | Ja |
| [Traffic Manager](./germany-services-networking.md#support-for-traffic-manager)  | Ja | Ja |
|  [VPN Gateway](./germany-services-networking.md#support-for-vpn-gateway) | Ja | Ja |
| Application Gateway | Ja | Ja |



## <a name="storage"></a>[Storage](./germany-services-storage.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| [Storage](./germany-services-storage.md#storage) | Ja | Ja |
| StorSimple | Nein | Nein |
| Backup | Ja | Ja |
| Site Recovery | Ja | Ja |



## <a name="web-and-mobile"></a>[Web und Mobil](./germany-services-webandmobile.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| [App Service: Web-Apps](./germany-services-webandmobile.md#app-service) | Ja | Ja |
| [App Service: API-Apps](./germany-services-webandmobile.md#app-service) | Ja | Ja |
| [App Service: Mobile Apps](./germany-services-webandmobile.md#app-service) | Ja | Ja |
| Media Services | Ja | Ja |


## <a name="databases"></a>[Datenbanken](./germany-services-database.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| [SQL-Datenbank](./germany-services-database.md#sql-database) | Ja | Ja |
| Azure Synapse Analytics | Ja | Ja |
| SQL Server Stretch Database | Ja | Ja |
| [Azure Cache for Redis](./germany-services-database.md#azure-cache-for-redis) | Ja | Ja |
| Azure Cosmos DB | Ja | Ja |


## <a name="intelligence-and-analytics"></a>Informationen und Analyse

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| HDInsight | Ja | Ja |
| Machine Learning | Ja | Nein |


## <a name="internet-of-things-iot"></a>[Internet der Dinge (IoT)](./germany-services-iot.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| Event Hubs | Ja | Ja |
| IoT Hub | Ja | Ja |
| Notification Hubs | Ja | Nein |
| Stream Analytics | Ja | Ja |


## <a name="enterprise-integration"></a>Unternehmensintegration

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| Service Bus | Ja | Ja |
| StorSimple | Nein | Nein |
| SQL Server Stretch Database | Ja | Ja |



## <a name="security-and-identity"></a>[Sicherheit und Identität](./germany-services-securityandidentity.md)

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| Active Directory Free | Ja | Ja |
| Active Directory Premium | Nein | Nein |
|  [Schlüsseltresor](./germany-services-securityandidentity.md#key-vault)  | Ja | Nein |



## <a name="monitoring-and-management"></a>Überwachung und Verwaltung

| Dienst | Ressourcen-Manager | Portal |
| --- | --- | --- |
| Automation | Nein | Nein |
| Backup | Ja | Ja |
| Azure Monitor-Protokolle | Nein | Nein |
| Site Recovery | Ja | Ja |



## <a name="next-steps"></a>Nächste Schritte
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.
