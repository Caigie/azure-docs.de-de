---
title: Grundlegendes zu Apps und Bereitstellungen in Azure Spring Cloud
description: Der Unterschied zwischen Anwendung und Bereitstellung in Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 07/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: 9e909db0041979eb7bc4fc30bd9551382e83c488
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90892494"
---
# <a name="understand-app-and-deployment-in-azure-spring-cloud"></a>Grundlegendes zu Apps und Bereitstellungen in Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

**App** und **Bereitstellung** sind die beiden wichtigsten Konzepte im Ressourcenmodell von Azure Spring Cloud. In Azure Spring Cloud ist eine *App* eine Abstraktion einer Geschäfts-App oder eines Microservice.  Eine Version des Codes oder der Binärdatei, die als die *App* bereitgestellt wird, wird in einer *Bereitstellung* ausgeführt.  Apps werden in einer *Azure Spring Cloud-Dienstinstanz* (kurz *Dienstinstanz*) ausgeführt, wie im Folgenden beschrieben.

 ![Apps und Bereitstellungen](./media/spring-cloud-app-and-deployment/app-deployment-rev.png)

Sie können über mehrere Dienstinstanzen innerhalb eines einzelnen Azure-Abonnements verfügen, es ist jedoch am einfachsten, wenn sich alle Apps, aus denen eine Geschäfts-App oder ein Microservice besteht, innerhalb einer einzelnen Azure Spring Cloud-Dienstinstanz befinden.

Der Standardtarif der Azure Spring Cloud ermöglicht, dass eine App eine Produktionsbereitstellung und eine Stagingbereitstellung hat, sodass Sie damit auf einfache Weise eine Blue/Green-Bereitstellung durchführen können.

## <a name="app"></a>App
Die folgenden Features/Eigenschaften werden auf App-Ebene definiert.

| Enumeration | Definition |
|:--|:----------------|
| Öffentlich</br>Endpunkt | Die URL für den Zugriff auf die App. |
| Benutzerdefiniert</br>Domain | Der CNAME-Eintrag, der die benutzerdefinierte Domäne sichert. |
| Dienst</br>Bindung | Die in der Datei „function.json“ und im Attribut *ServiceBusTrigger* festgelegten Bindungskonfigurationseigenschaften. |
| Verwaltet</br>Identity | Verwaltete Entität von Azure Active Directory, die es Ihrer App erlaubt, einfach auf andere Azure AD-geschützte Ressourcen wie Azure Key Vault zuzugreifen. |
| Beständig</br>Storage | Einstellung, die es ermöglicht, dass Daten nach dem Neustart der App beibehalten werden können. |

## <a name="deployment"></a>Bereitstellung

Die folgenden Features/Eigenschaften werden auf der Bereitstellungsebene definiert und beim Wechsel der Produktions-/Stagingbereitstellung ausgetauscht.

| Enumeration | Definition |
|:--|:----------------|
| CPU | Anzahl der virtuellen Kerne pro App-Instanz. |
| Arbeitsspeicher | Einstellung, die Arbeitsspeicher für die Hochskalierung und Aufskalierung von Bereitstellungen zuordnet. |
| Instanz</br>Anzahl | Die Anzahl der App-Instanzen, manuell oder automatisch festgelegt. |
| Automatische Skalierung | Anzahl der Skalierungsinstanzen, automatisch auf vordefinierten Regeln und Zeitplänen basierend. |
| JVM</br>Optionen | Einstellung: JAVA_OPTS |
| Environment</br>Variables | Einstellungen, die für die gesamte Azure Spring Cloud-Umgebung gelten. |
| Typ</br>Version | Java 8/Java 11|

## <a name="restrictions"></a>Beschränkungen

* **Eine App muss eine Produktionsbereitstellung besitzen**: Das Löschen einer Produktionsbereitstellung ist durch die API blockiert. Sie sollte vor dem Löschen zu Staging gewechselt werden.
* **Eine App kann höchstens zwei Bereitstellungen besitzen**: Das Erstellen von mehr als zwei Bereitstellungen ist durch die API blockiert. Stellen Sie Ihre neue Binärdatei entweder in der vorhandenen Produktions- oder der Stagingbereitstellung bereit.
* **Die Bereitstellungs Verwaltung ist im Tarif „Basic“ nicht verfügbar**: Verwenden Sie den Tarif „Standard“ für die Blue/Green-Bereitstellungsfunktion.

## <a name="see-also"></a>Siehe auch
* [Einrichten einer Stagingumgebung in Azure Spring Cloud](spring-cloud-howto-staging-environment.md)
