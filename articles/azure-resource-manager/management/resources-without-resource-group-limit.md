---
title: Ressourcen ohne Beschränkung auf 800 Instanzen
description: Aufstellung der Azure-Ressourcentypen, die mehr als 800 Instanzen in einer Ressourcengruppe aufweisen können.
ms.topic: conceptual
author: davidsmatlak
ms.author: v-dasmat
ms.date: 05/04/2020
ms.openlocfilehash: 892b59b3d3e980abfcdb9cd692c2598ceb1284ad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82780928"
---
# <a name="resources-not-limited-to-800-instances-per-resource-group"></a>Ressourcen ohne Beschränkung auf 800 Instanzen pro Ressourcengruppe

Standardmäßig können Sie bis zu 800 Instanzen eines Ressourcentyps in jeder Ressourcengruppe bereitstellen. Einige Ressourcentypen sind jedoch von der Beschränkung auf 800 Instanzen ausgenommen. In diesem Artikel sind die Azure-Ressourcentypen aufgeführt, die mehr als 800 Instanzen in einer Ressourcengruppe aufweisen können. Alle anderen Ressourcentypen sind auf 800 Instanzen beschränkt.

Für einige Ressourcentypen müssen Sie sich an den Support wenden, damit die Beschränkung auf 800 Instanzen aufgehoben wird. Diese Ressourcentypen sind in diesem Artikel angegeben.

## <a name="microsoftautomation"></a>Microsoft.Automation

* automationAccounts

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

* registrations
* registrations/customerSubscriptions
* registrations/products

## <a name="microsoftbotservice"></a>Microsoft.BotService

* botServices: Standardmäßig auf 800 Instanzen beschränkt. Dieser Grenzwert kann bei Kontaktaufnahme mit dem Support erhöht werden.

## <a name="microsoftcompute"></a>Microsoft.Compute

* disks
* galleries
* galleries/images
* galleries/images/versions
* images
* snapshots
* virtualMachines

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

* containerGroups

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

* registries/buildTasks
* registries/buildTasks/listSourceRepositoryProperties
* registries/buildTasks/steps
* registries/buildTasks/steps/listBuildArguments
* registries/eventGridFilters
* registries/replications
* registries/tasks
* registries/webhooks

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

* servers

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

* servers

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

* serverGroups
* servers
* serversv2
* singleServers

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

* schedules: Standardmäßig auf 800 Instanzen beschränkt. Dieser Grenzwert kann bei Kontaktaufnahme mit dem Support erhöht werden.

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

* services

## <a name="microsofteventhub"></a>Microsoft.EventHub

* clusters
* Namespaces

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

* experimentWorkspaces

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

* autoManagedVmConfigurationProfiles
* configurationProfileAssignments
* guestConfigurationAssignments
* software
* softwareUpdateProfile
* softwareUpdates

## <a name="microsoftinsights"></a>microsoft.insights

* metricalerts

## <a name="microsoftlogic"></a>Microsoft.Logic

* integrationAccounts
* workflows

## <a name="microsoftnetapp"></a>Microsoft.NetApp

* netAppAccounts
* netAppAccounts/capacityPools
* netAppAccounts/capacityPools/volumes
* netAppAccounts/capacityPools/volumes/mountTargets
* netAppAccounts/capacityPools/volumes/snapshots

## <a name="microsoftnetwork"></a>Microsoft.Network

* applicationGatewayWebApplicationFirewallPolicies
* applicationSecurityGroups
* bastionHosts
* ddosProtectionPlans
* dnszones
* dnszones/A
* dnszones/AAAA
* dnszones/CAA
* dnszones/CNAME
* dnszones/MX
* dnszones/NS
* dnszones/PTR
* dnszones/SOA
* dnszones/SRV
* dnszones/TXT
* dnszones/all
* dnszones/recordsets
* networkIntentPolicies
* networkInterfaces
* privateDnsZones
* privateDnsZones/A
* privateDnsZones/AAAA
* privateDnsZones/CNAME
* privateDnsZones/MX
* privateDnsZones/PTR
* privateDnsZones/SOA
* privateDnsZones/SRV
* privateDnsZones/TXT
* privateDnsZones/all
* privateDnsZones/virtualNetworkLinks
* privateEndpoints
* privateLinkServices
* publicIPAddresses: Standardmäßig auf 800 Instanzen beschränkt. Dieser Grenzwert kann bei Kontaktaufnahme mit dem Support erhöht werden.
* serviceEndpointPolicies
* trafficmanagerprofiles
* virtualNetworkTaps

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

* rootResources

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

* workspaceCollections: Standardmäßig auf 800 Instanzen beschränkt. Dieser Grenzwert kann bei Kontaktaufnahme mit dem Support erhöht werden.

## <a name="microsoftrelay"></a>Microsoft.Relay

* Namespaces

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

* jobcollections

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

* Namespaces

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

* applications
* containerGroups
* gateways
* networks
* secrets
* volumes

## <a name="microsoftstorage"></a>Microsoft.Storage

* storageAccounts

## <a name="microsoftweb"></a>Microsoft.Web

* apiManagementAccounts/apis
* sites

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Übersicht der Kontingente und Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](azure-subscription-service-limits.md).
