---
title: Konnektivitätsmodi und -anforderungen
description: Erläutert Konnektivitätsoptionen für Azure Arc-fähige Datendienste zwischen Ihrer Umgebung und Azure.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: conceptual
ms.openlocfilehash: 44c1c1860cbea20a7a00da5a396e4d82d79efd8b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931621"
---
# <a name="connectivity-modes-and-requirements"></a>Konnektivitätsmodi und -anforderungen

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connectivity-modes"></a>Konnektivitätsmodi

Es gibt mehrere Optionen für den Grad der Konnektivität zwischen Ihrer Azure Arc-fähigen Datendienstumgebung und Azure. Da Ihre Anforderungen abhängig von der Geschäftsrichtlinie, behördlichen Regelungen oder der Verfügbarkeit von Netzwerkverbindungen mit Azure variieren, können Sie zwischen den folgenden Konnektivitätsmodi wählen.

Azure Arc-fähige Datendienste bieten Ihnen die Möglichkeit, in zwei verschiedenen „Konnektivitätsmodi“ eine Verbindung mit Azure herzustellen: direkt verbunden und indirekt verbunden.  Dies bietet Ihnen die Flexibilität, zu entscheiden, wie viele Daten an Azure gesendet werden und wie Benutzer mit dem Arc-Datencontroller interagieren. Abhängig vom ausgewählten Konnektivitätsmodus können einige Funktionen von Azure Arc-fähigen Datendiensten verfügbar oder nicht verfügbar sein.

Wichtig: Wenn die Azure Arc-fähigen Datendienste direkt mit Azure verbunden sind, können Benutzer die [Azure Resource Manager-APIs](/rest/api/resources/), die Azure CLI und das Azure-Portal verwenden, um die Azure Arc-Datendienste zu betreiben. Das Verhalten im direkt verbundenen Modus ist ähnlich wie bei der Verwendung eines beliebigen anderen Azure-Diensts mit Bereitstellung/Aufhebung der Bereitstellung, Skalierung, Konfiguration usw. im Azure-Portal.  Wenn die Azure Arc-fähigen Datendienste indirekt mit Azure verbunden sind, zeigt das Azure-Portal eine schreibgeschützte Ansicht an. Sie können das Inventar von verwalteten SQL-Instanzen und Postgres Hyperscale-Instanzen anzeigen, die Sie bereitgestellt haben, sowie Details zu diesen Instanzen, aber Sie können im Azure-Portal keine Aktionen für diese ausführen.  Im indirekt verbundenen Modus müssen alle Aktionen lokal mithilfe von Azure Data Studio, der Azure Data CLI oder nativen Kubernetes-Tools wie kubectl ausgeführt werden.

Darüber hinaus können Azure Active Directory und rollenbasierte Azure-Zugriffssteuerung nur im direkt verbundenen Modus verwendet werden, da eine Abhängigkeit von einer kontinuierlichen und direkten Verbindung mit Azure besteht, um diese Funktionalität bereitzustellen.

Schließlich sind einige mit Azure verbundene Dienste nur dann verfügbar, wenn sie direkt erreichbar sind, wie die Azure Defender-Sicherheitsdienste, Container Insights und „Azure Backup/Blobspeicher“.

Derzeit wird in der Vorschau nur der indirekt verbundene Modus unterstützt.  Der direkt verbundene Modus ist für die Zukunft geplant.  Es gibt einen denkbaren Modus „Nie verbunden“, der aber zurzeit nicht unterstützt wird.

||**Indirekt verbunden**|**Direkt verbunden**|**Nie verbunden**|
|---|---|---|---|
|**Beschreibung**|Der indirekt verbundene Modus bietet die meisten Verwaltungsdienste lokal in Ihrer Umgebung ohne direkte Verbindung mit Azure.  Eine minimale Datenmenge muss _nur_ zu Inventur- und Abrechnungszwecken an Azure gesendet werden. Sie wird in eine Datei exportiert und mindestens ein Mal pro Monat in Azure hochgeladen.  Es ist keine direkte oder kontinuierliche Verbindung mit Azure erforderlich.  Einige Features und Dienste, die eine Verbindung mit Azure erfordern, sind nicht verfügbar.|Der direkt verbundene Modus bietet alle verfügbaren Dienste, wenn eine direkte Verbindung mit Azure hergestellt werden kann. Verbindungen werden stets _aus_ Ihrer Umgebung mit Azure initiiert und verwenden Standardports und -protokolle, z. B. HTTPS/443.|Es können keine Daten an oder aus Azure gesendet werden.|
|**Aktuelle Verfügbarkeit**| In der Vorschau verfügbar.|Als Vorschau für die Zukunft geplant.|Wird derzeit nicht unterstützt.|
|**Typische Anwendungsfälle**|Lokale Rechenzentren, die aufgrund von Geschäfts- oder gesetzlichen Compliancerichtlinien oder aufgrund von Bedenken hinsichtlich externer Angriffe oder Datenexfiltration keine Konnektivität in die oder aus der Datenregion des Rechenzentrums zulassen.  Typische Beispiele: Finanzinstitute, Gesundheitswesen, Behörden. <br/><br/>Edge-Standorte, an denen der Edge-Standort in der Regel über keine Verbindung mit dem Internet verfügt.  Typische Beispiele: Öl-/Gas- oder militärische Anwendungen.  <br/><br/>Edge-Standorte, die über zeitweilig auftretende Verbindungen mit langen Ausfallzeiten verfügen.  Typische Beispiele: Stadien, Kreuzfahrtschiffe. | Organisationen, die öffentliche Clouds verwenden.  Typische Beispiele: Azure, AWS oder Google Cloud.<br/><br/>Edge-Standorte, an denen eine Internetverbindung normalerweise vorhanden und zulässig ist.  Typische Beispiele: Einzelhandelsgeschäfte, Produktion.<br/><br/>Unternehmensrechenzentren mit restriktiveren Richtlinien für die Konnektivität in die bzw. aus der Datenregion des Rechenzentrums mit dem Internet.  Typische Beispiele: Nicht regulierte Unternehmen, kleine und mittelständische Unternehmen|Wirklich „luftdichte“ Umgebungen, in denen unter keinen Umständen Daten aus der Datenumgebung gesendet oder in sie eingehen dürfen. Typische Beispiele: streng geheime Regierungseinrichtungen.|
|**Senden von Daten an Azure**|Es gibt drei Möglichkeiten, wie die Abrechnungs- und Inventardaten an Azure gesendet werden können:<br><br> 1) Daten werden durch einen automatisierten Prozess, der sowohl mit der sicheren Datenregion als auch mit Azure verbunden ist, aus der Datenregion exportiert.<br><br>2) Daten werden durch einen automatisierten Prozess innerhalb der Datenregion aus der Datenregion exportiert, automatisch in eine weniger sichere Region kopiert, und ein automatisierter Prozess in der weniger sicheren Region lädt die Daten in Azure hoch.<br><br>3) Daten werden von einem Benutzer innerhalb der sicheren Region manuell exportiert, manuell aus der sicheren Region herausgebracht und manuell in Azure hochgeladen. <br><br>Bei den ersten beiden Optionen handelt es sich um einen automatisierten kontinuierlichen Prozess, der so geplant werden kann, dass er häufig ausgeführt wird, sodass es nur minimale Verzögerungen bei der Übertragung von Daten in Azure gibt, abhängig ausschließlich von der verfügbaren Konnektivität mit Azure.|Daten werden automatisch und kontinuierlich an Azure gesendet.|Daten werden nie an Azure gesendet.|

## <a name="feature-availability-by-connectivity-mode"></a>Funktionsverfügbarkeit nach Konnektivitätsmodus

|**Feature**|**Indirekt verbunden**|**Direkt verbunden**|
|---|---|---|
|**Automatische Hochverfügbarkeit**|Unterstützt|Unterstützt|
|**Self-Service-Bereitstellung**|Unterstützt<br/>Die Erstellung kann über Azure Data Studio, über die Azure Data CLI oder native Kubernetes-Tools (Helm, kubectl, oc usw.) oder mithilfe einer Azure Arc-fähigen Kubernetes GitOps-Bereitstellung durchgeführt werden.|Unterstützt<br/>Neben den Erstellungsoptionen im Modus „Indirekt verbunden“ können Sie zum Erstellen auch das Azure-Portal, Azure Resource Manager-APIs, die Azure CLI oder ARM-Vorlagen verwenden. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**
|**Elastische Skalierbarkeit**|Unterstützt|Unterstützt<br/>**Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Abrechnung**|Unterstützt<br/>Abrechnungsdaten werden in regelmäßigen Abständen exportiert und an Azure gesendet.|Unterstützt<br/>Abrechnungsdaten werden automatisch und kontinuierlich an Azure gesendet und in nahezu Echtzeit angezeigt. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Bestandsverwaltung**|Unterstützt<br/>Inventardaten werden in regelmäßigen Abständen exportiert und an Azure gesendet.|Unterstützt<br/>Inventardaten werden automatisch und kontinuierlich an Azure gesendet und in nahezu Echtzeit angezeigt. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Automatische Upgrades und Patches**|Unterstützt<br/>Der Datencontroller muss entweder direkt auf Microsoft Container Registry (MCR) zugreifen können, oder die Containerimages müssen aus MCR gepullt und in eine lokale private Containerregistrierung gepusht werden, auf die der Datencontroller Zugriff besitzt.|Unterstützt<br/>**Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Regelmäßige Sicherung und Wiederherstellung**|Unterstützt<br/>Automatische lokale Sicherung und Wiederherstellung.|Unterstützt<br/>Zusätzlich zur automatisierten lokalen Sicherung und Wiederherstellung können Sie _optional_ Sicherungen für die langfristige Aufbewahrung außerhalb des Standorts an Azure Backup senden. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Überwachung**|Unterstützt<br/>Lokale Überwachung mit Grafana- und Kibana-Dashboards.|Unterstützt<br/>Zusätzlich zu den lokalen Überwachungsdashboards können Sie _optional_ Überwachungsdaten und Protokolle an Azure Monitor senden, um mehrere Standorte an einem Ort angemessen zu überwachen. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Authentifizierung**|Verwenden Sie einen lokalen Benutzernamen bzw. ein lokales Kennwort für die Datencontroller- und Dashboardauthentifizierung. Verwenden Sie SQL- und Postgres-Anmeldungen oder-Active Directory für Verbindungen mit Datenbankinstanzen.  Verwenden Sie K8s-Authentifizierungsanbieter für die Authentifizierung bei der Kubernetes-API.|Zusätzlich zu den Authentifizierungsmethoden für den indirekt verbundenen Modus können Sie _optional_ Azure Active Directory verwenden. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)**|Verwenden Sie Kubernetes RBAC mit der Kubernetes-API. Verwenden Sie SQL und Postgres RBAC für Datenbankinstanzen.|Sie können optional eine Integration in Azure Active Directory für RBAC ausführen. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Azure Defender**|Nicht unterstützt|In Zukunft geplant|

## <a name="connectivity-requirements"></a>Konnektivitätsanforderungen

**Für einige Funktionen ist eine Verbindung mit Azure erforderlich.**

**Die gesamte Kommunikation mit Azure wird immer aus Ihrer Umgebung initiiert.** Dies gilt auch für Vorgänge, die von einem Benutzer im Azure-Portal initiiert werden.  In diesem Fall gibt es einen Task, der in Azure in die Warteschlange eingereiht wird.  Ein Agent in Ihrer Umgebung initiiert die Kommunikation mit Azure, um zu ermitteln, welche Tasks in der Warteschlange enthalten sind, führt die Tasks aus und meldet den Status/Abschluss/Fehler an Azure.

|**Datentyp**|**Richtung**|**Erforderlich/optional**|**Zusätzliche Kosten**|**Erforderlicher Modus**|**Hinweise**|
|---|---|---|---|---|---|
|**Containerimages**|Microsoft Container Registry -> Kunde|Erforderlich|Nein|Indirekt oder direkt|Containerimages stellen die Methode zum Verteilen der Software dar.  In einer Umgebung, die eine Verbindung mit Microsoft Container Registry (MCR) über das Internet herstellen kann, können die Containerimages direkt aus MCR gepullt werden.  Für den Fall, dass die Bereitstellungsumgebung über keine direkte Verbindung verfügt, können Sie die Images aus MCR pullen und in eine private Containerregistrierung in der Bereitstellungsumgebung pushen.  Zum Zeitpunkt der Erstellung können Sie den Erstellungsprozess so konfigurieren, dass der Pullvorgang aus der privaten Containerregistrierung anstatt aus MCR erfolgt. Dies gilt auch für automatisierte Updates.|
|**Ressourceninventar**|Kundenumgebung-> Azure|Erforderlich|Nein|Indirekt oder direkt|Ein Inventar von Datencontrollern und Datenbankinstanzen (PostgreSQL und SQL) wird in Azure für Abrechnungszwecke und auch zu Zwecken der Erstellung eines Inventars aller Datencontroller und Datenbankinstanzen an einem Ort verwaltet, was besonders nützlich ist, wenn Sie mehrere Umgebungen mit Azure Arc-Datendiensten nutzen.  Wenn Instanzen bereitgestellt werden bzw. ihre Bereitstellung aufgehoben wird, sie horizontal hoch- und herunterskaliert oder zentral hoch- oder herunterskaliert werden, wird das Inventar in Azure aktualisiert.|
|**Abrechnungstelemetriedaten**|Kundenumgebung-> Azure|Erforderlich|Nein|Indirekt oder direkt|Die Nutzung von Datenbankinstanzen muss zu Abrechnungszwecken an Azure gesendet werden.  Während des Vorschauzeitraums fallen keine Kosten für Azure Arc-fähige Datendienste an.|
|**Überwachungsdaten und -protokolle**|Kundenumgebung-> Azure|Optional|Möglicherweise abhängig von der Datenmenge (siehe [Azure Monitor-Preise](https://azure.microsoft.com/en-us/pricing/details/monitor/))|Indirekt oder direkt|Möglicherweise möchten Sie die lokal erfassten Überwachungsdaten und -protokolle an Azure Monitor senden, um Daten aus mehreren Umgebungen an einem Ort zu aggregieren und auch Azure Monitor-Dienste wie Warnungen zu nutzen, die Daten in Azure Machine Learning zu verwenden usw.|
|**Rollenbasierte Azure-Zugriffssteuerung (RBAC)**|Kundenumgebung -> Azure -> Kundenumgebung|Optional|Nein|Nur direkt|Wenn Sie Azure für RBAC verwenden möchten, muss Konnektivität mit Azure jederzeit hergestellt werden.  Wenn Sie Azure für RBAC nicht verwenden möchten, können Sie lokale Kubernetes-RBAC verwenden.  **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Azure Active Directory (AD)**|Kundenumgebung -> Azure -> Kundenumgebung|Optional|Möglicherweise, aber vielleicht zahlen Sie bereits für Azure AD|Nur direkt|Wenn Sie Azure AD für die Authentifizierung verwenden möchten, muss Konnektivität mit Azure jederzeit hergestellt werden. Wenn Sie Azure AD nicht für die Authentifizierung verwenden möchten, können Sie Active Directory-Verbunddienste (ADFS) über Active Directory verwenden. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Sicherungen/Wiederherstellungen**|Kundenumgebung -> Azure -> Kundenumgebung|Optional|Ja, für Speicherkosten|Nur direkt|Möglicherweise möchten Sie Sicherungen, die lokal erstellt wurden, an Azure Backup für die langfristige, externe Aufbewahrung von Sicherungen senden und sie zur Wiederherstellung in die lokale Umgebung zurückholen. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Azure Defender-Sicherheitsdienste**|Kundenumgebung -> Azure -> Kundenumgebung|Optional|Ja|Nur direkt|**Ausstehende Verfügbarkeit des direkt verbundenen Modus**|
|**Bereitstellungs- und Konfigurationsänderungen aus dem Azure-Portal**|Kundenumgebung -> Azure -> Kundenumgebung|Optional|Nein|Nur direkt|Bereitstellungs- und Konfigurationsänderungen können lokal mithilfe von Azure Data Studio oder der azdata CLI erfolgen.  Im direkt verbundenen Modus können Sie auch Konfigurationsänderungen aus dem Azure-Portal bereitstellen und vornehmen. **Ausstehende Verfügbarkeit des direkt verbundenen Modus**|


## <a name="details-on-internet-addresses-ports-encryption-and-proxy-server-support"></a>Details zu Internetadressen, Ports, Verschlüsselung und Proxyserverunterstützung

Derzeit wird in der Vorschauphase nur der indirekt verbundene Modus unterstützt.  In diesem Modus sind nur drei Verbindungen für Dienste erforderlich, die im Internet verfügbar sind.  Alle HTTPS-Verbindungen mit Azure und Microsoft Container Registry werden mithilfe von SSL/TLS mithilfe von offiziell signierten und überprüfbaren Zertifikaten verschlüsselt.

|**Name**|**Verbindungsquelle**|**Verbindungsziel**|**Protokoll**|**Port**|**Kann Proxy verwenden**|**Authentifizierung**|**Hinweise**|
|---|---|---|---|---|---|---|---|
|**Microsoft Container Registry (MCR)**|Das Kubernetes-Kubelet auf jedem der Kubernetes-Knoten, das die Containerimages pullt.|`mcr.microsoft.com`|HTTPS|443|Ja|Keine|Microsoft Container Registry hostet die Azure Arc-fähigen Datendienst-Containerimages.  Sie können diese Images aus MCR pullen und in eine private Containerregistrierung pushen und den Datencontroller-Bereitstellungsprozess dann so konfigurieren, dass die Containerimages aus dieser privaten Containerregistrierung gepullt werden.|
|**Azure Resource Manager-APIs**|Ein Computer, auf dem Azure Data Studio, die Azure Data CLI oder die Azure CLI ausgeführt wird, der mit Azure verbunden ist.|`login.microsoftonline.com`<br/>`management.azure.com`<br/>`san-af-eastus-prod.azurewebsites.net`<br/>`san-af-eastus2-prod.azurewebsites.net`<br/>`san-af-australiaeast-prod.azurewebsites.net`<br/>`san-af-centralus-prod.azurewebsites.net`<br/>`san-af-westus2-prod.azurewebsites.net`<br/>`san-af-westeurope-prod.azurewebsites.net`<br/>`san-af-southeastasia-prod.azurewebsites.net`<br/>`san-af-koreacentral-prod.azurewebsites.net`<br/>`san-af-northeurope-prod.azurewebsites.net`<br/>`san-af-westeurope-prod.azurewebsites.net`<br/>`san-af-uksouth-prod.azurewebsites.net`<br/>`san-af-francecentral-prod.azurewebsites.net`|HTTPS|443|Ja|Azure Active Directory|Azure Data Studio, die Azure Data CLI und die Azure CLI stellen eine Verbindung mit den Azure Resource Manager-APIs her, um für einige Funktionen Daten an Azure zu und aus Azure abzurufen.|
|**Azure Monitor-APIs**|Ein Computer, auf dem die Azure Data CLI oder die Azure CLI ausgeführt wird, der Überwachungsmetriken oder -protokolle in Azure Monitor hochlädt.|`login.microsoftonline.com`<br/>`management.azure.com`<br/>`*.ods.opinsights.azure.com`<br/>`*.oms.opinsights.azure.com`<br/>`*.monitoring.azure.com`|HTTPS|443|Ja|Azure Active Directory|Azure Data Studio, die Azure Data CLI und die Azure CLI stellen eine Verbindung mit den Azure Resource Manager-APIs her, um für einige Funktionen Daten an Azure zu und aus Azure abzurufen.|

> **Hinweis:** Zurzeit werden alle HTTPS/443-Verbindungen des Browsers mit den Grafana- und Kibana-Dashboards und von der Azure Data CLI mit der Datencontroller-API mit selbstsignierten Zertifikaten mit SSL verschlüsselt.  In Zukunft wird eine Funktion verfügbar sein, mit der Sie Ihre eigenen Zertifikate für die Verschlüsselung dieser SSL-Verbindungen bereitstellen können.

Verbindungen aus Azure Data Studio und der Azure Data CLI mit dem Kubernetes-API-Server verwenden die von Ihnen eingerichtete Kubernetes-Authentifizierung und -Verschlüsselung.  Jeder Benutzer, der Azure Data Studio und die Azure Data CLI verwendet, muss über eine authentifizierte Verbindung mit der Kubernetes-API verfügen, um viele der Aktionen im Zusammenhang mit Azure Arc-fähigen Datendiensten ausführen zu können.
