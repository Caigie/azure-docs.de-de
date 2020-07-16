---
title: Versionshinweise zum Azure-Dateisynchronisierungs-Agent | Microsoft-Dokumentation
description: Versionshinweise zum Azure-Dateisynchronisierungs-Agent
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 6/26/2020
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 54a7f3f50de27747ab15f6895ebfb4f65faf5fdf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85484059"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Versionshinweise zum Azure-Dateisynchronisierungs-Agent
Mit der Azure-Dateisynchronisierung können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Ihre Windows Server-Installationen werden in einen schnellen Cache Ihrer Azure-Dateifreigabe transformiert. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen (z.B. SMB, NFS und FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält die Versionshinweise für die unterstützten Versionen des Azure-Dateisynchronisierungs-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Für den Azure-Dateisynchronisierungs-Agent werden die folgenden Versionen unterstützt:

| Meilenstein | Agent-Versionsnummer | Veröffentlichungsdatum | Status |
|----|----------------------|--------------|------------------|
| V10.1-Release: [KB4522411](https://support.microsoft.com/en-us/help/4522411)| 10.1.0.0 | 5\. Juni 2020 | Unterstützt – Flighting |
| Mai 2020 Updaterollup – [KB4522412](https://support.microsoft.com/help/4522412)| 10.0.2.0 | 19. Mai 2020 | Unterstützt |
| V10-Release: [KB4522409](https://support.microsoft.com/en-us/help/4522409)| 10.0.0.0 | 9\. April 2020 | Unterstützt |
| Updaterollup von Dezember 2019: [KB4522360](https://support.microsoft.com/help/4522360)| 9.1.0.0 | 12. Dezember 2019 | Unterstützt |
| V9-Release: [KB4522359](https://support.microsoft.com/help/4522359)| 9.0.0.0 | 2\. Dezember 2019 | Unterstützt |
| V8-Release – [KB4511224](https://support.microsoft.com/help/4511224)| 8.0.0.0 | 8\. Oktober 2019 | Unterstützt |
| Juli 2019 Updaterollup – [KB4490497](https://support.microsoft.com/help/4490497)| 7.2.0.0 | 24. Juli 2019 | Unterstützt: Agent-Version läuft am 1. September 2020 ab |
| Juli 2019 Updaterollup – [KB4490496](https://support.microsoft.com/help/4490496)| 7.1.0.0 | 12. Juli 2019 | Unterstützt: Agent-Version läuft am 1. September 2020 ab |
| V7-Release: [KB4490495](https://support.microsoft.com/help/4490495)| 7.0.0.0 | 19. Juni 2019 | Unterstützt: Agent-Version läuft am 1. September 2020 ab |
| V6-Release | 6.0.0.0–6.3.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 21. April 2020 abgelaufen. |
| Release V5 | 5.0.2.0–5.2.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 18. März 2020 abgelaufen. |
| Release V4 | 4.0.1.0 – 4.3.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 6. November 2019 abgelaufen. |
| Release V3 | 3.1.0.0 – 3.4.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 19. August 2019 abgelaufen. |
| Pre-GA-Agents | 1.1.0.0 – 3.0.13.0 | – | Nicht unterstützt – Agent-Versionen sind am 1. Oktober 2018 abgelaufen. |

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-10100"></a>Agent-Version 10.1.0.0
Die folgenden Versionshinweise gelten für Version 10.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 5. Juni 2020 veröffentlicht wurde. Sie gelten zusätzlich zu den für die Versionen 10.0.0.0 und 10.0.2.0 angegeben Versionshinweisen.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für private Azure-Endpunkte
    - Synchronisierungsdatenverkehr zum Speichersynchronisierungsdienst kann jetzt an einen privaten Endpunkt gesendet werden. Dies ermöglicht ein Tunneln über eine ExpressRoute- oder VPN-Verbindung. Weitere Informationen finden Sie unter [Konfigurieren von Netzwerkendpunkten für die Azure-Dateisynchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-networking-endpoints).
- Die Metrik „Dateien synchronisiert“ zeigt nun bei einer großen Synchronisierung den Fortschritt während der Ausführung an statt am Ende.
- Verschiedene Verbesserungen in Bezug auf die Zuverlässigkeit bei der Agent-Installation, beim Cloudtiering, bei der Synchronisierung und bei der Telemetrie.

## <a name="agent-version-10020"></a>Agent-Version 10.0.2.0
Die folgenden Versionshinweise gelten für Version 10.0.2.0 des Azure-Dateisynchronisierungs-Agents, die am 19. Mai 2020 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 10.0.0.0 angegeben sind.

In dieser Version behobenes Problem:  
- Der Storage-Synchronisierungs-Agent (FileSyncSvc) stürzt nach der Installation des Azure-Dateisynchronisierung-Agents, Version 10, häufig ab.

> [!Note]  
>Für diese Version wurde kein Test-Flight für Server durchgeführt, die automatisch aktualisiert werden, wenn eine neue Version verfügbar ist. Verwenden Sie zum Installieren dieses Updates Microsoft Update oder den Microsoft Update-Katalog (Installationsanweisungen finden Sie unter [KB4522412](https://support.microsoft.com/help/4522412)).

## <a name="agent-version-10000"></a>Agent-Version 10.0.0.0
Die folgenden Versionshinweise gelten für Version 10.0.0.0 des Azure-Dateisynchronisierungs-Agents, die am 9. April 2020 veröffentlicht wurde.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Verbesserter Synchronisierungsfortschritt im Portal
    - Bei Verwendung der Agent-Version V10 wird in Kürze der Typ der aktuell ausgeführten Synchronisierungssitzung im Azure-Portal angezeigt. Beispiel: erster Download, regulärer Download, Hintergrundrückruf (Fälle von schnellen Notfallwiederherstellungen) und ähnliches 

- Verbesserte Benutzeroberfläche des Portals für Cloudtiering
    - Für Dateien mit Tiering- oder Abruffehlern werden jetzt die Tieringfehler in den Eigenschaften des Serverendpunkts angezeigt.
    - Für Serverendpunkte stehen jetzt zusätzliche Cloudtieringinformationen bereit:
        - Größe des lokalen Caches
        - Effizienz der Cachenutzung
        - Details zur Cloudtieringrichtlinie: Volumegröße, aktueller freier Speicherplatz oder der Zeitpunkt des letzten Zugriffs auf die älteste Datei im lokalen Cache.
    - Diese Änderungen werden im Azure-Portal unmittelbar nach dem V10-Agent-Erstrelease bereitgestellt.

- Unterstützung für das Verschieben des Speichersynchronisierungsdiensts und/oder Speicherkontos in einen anderen Azure Active Directory-Mandanten (AAD)
    - Die Azure-Dateisynchronisierung unterstützt jetzt das Verschieben des Speichersynchronisierungsdiensts und/oder Speicherkontos in eine andere Ressourcengruppe, in ein anderes Abonnement oder in einen anderen Azure AD Mandanten.
    
- Sonstige Verbesserungen bei Leistung und Zuverlässigkeit
    - Die Änderungserkennung für die Azure-Dateifreigabe schlägt möglicherweise fehl, wenn ein virtuelles Netzwerk (VNET) und Firewallregeln für das Speicherkonto konfiguriert sind.
    - Geringerer Arbeitsspeicherverbrauch im Zusammenhang mit Rückrufen 
    - Verbesserte Leistung bei Verwendung des [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection)-Cmdlets
    - Verschiedene Verbesserungen im Hinblick auf die Zuverlässigkeit der Synchronisierung 
    
### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe, ein anderes Abonnement oder einen anderen Azure AD-Mandanten verschoben werden. Wenn der Speichersynchronisierungsdienst oder das Speicherkonto verschoben wurde, müssen Sie der Anwendung „Microsoft.StorageSync“ Zugriff auf das Speicherkonto gewähren (siehe [Stellen Sie sicher, dass die Azure-Dateisynchronisierung über Zugriff auf das Speicherkonto verfügt.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Beim Erstellen des Cloudendpunkts müssen sich der Speichersynchronisierungsdienst und das Speicherkonto im selben Azure AD-Mandanten befinden. Nach der Erstellung des Cloudendpunkts können der Speichersynchronisierungsdienst und das Speicherkonto in verschiedene Azure AD Mandanten verschoben werden.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.

## <a name="agent-version-9100"></a>Agent-Version 9.1.0.0
Die folgenden Versionshinweise gelten für Version 9.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 12. Dezember 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 9.0.0.0 angegeben sind.

In dieser Version behobenes Problem:  
- Fehler bei der Synchronisierung mit einem der folgenden Fehlercodes nach Upgrade auf Version 9.0 des Azure-Dateisynchronisierungs-Agents:
    - 0x8e5e044e (JET_errWriteConflict)
    - 0x8e5e0450 (JET_errInvalidSesid)
    - 0x8e5e0442 (JET_errInstanceUnavailable)

## <a name="agent-version-9000"></a>Agent-Version 9.0.0.0
Die folgenden Versionshinweise gelten für Version 9.0.0.0 des Azure-Dateisynchronisierungs-Agents (am 2. Dezember 2019 veröffentlicht).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung der Self-Service-Wiederherstellung
    - Benutzer können ihre Dateien nun mithilfe des Features „Vorherige Version“ wiederherstellen. Vor dem V9-Release wurde das Feature für vorherige Versionen auf Volumes mit aktiviertem Cloudtiering nicht unterstützt. Dieses Feature muss für jedes Volume separat aktiviert werden, auf denen ein Endpunkt mit aktiviertem Cloudtiering vorliegt. Weitere Informationen finden Sie unter  
[Self-Service-Wiederherstellung mit „Vorherige Versionen“ und VSS (Volumeschattenkopie-Dienst)](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service). 
 
- Unterstützung größerer Dateifreigaben 
    - Die Azure-Dateisynchronisierung unterstützt nun bis zu 64 TiB und 100 Millionen Dateien in einem einzelnen Synchronisierungsnamespace.  
 
- Unterstützung der Datendeduplizierung auf Windows Server 2019 
    - Die Datendeduplizierung wird nun mit aktiviertem Cloudtiering auf Windows Server 2019 unterstützt. Für die Unterstützung der Datendeduplizierung auf Volumes mit Cloudtiering muss das Windows-Update [KB4520062](https://support.microsoft.com/help/4520062) installiert sein. 
 
- Verbesserung der Mindestdateigröße für Dateien, für die das Tiering durchgeführt werden soll 
    - Die Mindestdateigröße für Dateien, für die das Tiering durchgeführt werden soll, basiert nun auf der Größe des Dateisystemclusters (das Doppelte der Größe des Dateisystemclusters). Die Standardgröße des NTFS-Dateisystemclusters beträgt beispielsweise 4 KB, weshalb die Mindestdateigröße für eine Datei, für die das Tiering durchgeführt werden soll, 8 KB beträgt. 
 
- Cmdlet zum Testen der Netzwerkkonnektivität 
    - Im Rahmen der Konfiguration der Azure-Dateisynchronisierung müssen mehrere Dienstendpunkte kontaktiert werden. Diese verfügen jeweils über einen eigenen DNS-Namen, der für den Server erreichbar sein muss. Diese URLs sind auch spezifisch für die Region, bei der ein Server registriert ist. Sobald ein Server registriert ist, kann das Cmdlet zum Testen der Konnektivität (PowerShell- und Serverregistrierungs-Hilfsprogramm) verwendet werden, um die Kommunikation mit allen für diesen Server spezifischen URLs zu testen. Dieses Cmdlet kann Sie bei der Problembehandlung unterstützen, wenn unvollständige Kommunikation den Server daran hindert, vollständig mit der Azure-Dateisynchronisierung zu arbeiten. Es kann außerdem zum Optimieren der Proxy- und Firewallkonfigurationen verwendet werden.  
 
        Führen Sie die folgenden PowerShell-Befehle aus, um den Netzwerkkonnektivitätstest auszuführen: 
 
        Import-Module "C:\Programme\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"  
        Test-StorageSyncNetworkConnectivity
 
- Entfernen der Serverendpunktverbesserung bei aktiviertem Cloudtiering 
    - Das Entfernen eines Serverendpunkts führt wie zuvor nicht dazu, dass Dateien aus der Azure-Dateifreigabe entfernt werden. Jedoch wurde das Verhalten von Analysepunkten auf dem lokalen Server geändert. Analysepunkte (Zeiger, die auf nicht lokale Dateien auf dem Server zeigen) werden nun gelöscht, wenn ein Serverendpunkt entfernt wird. Die vollständig zwischengespeicherten Dateien werden auf dem Server beibehalten. Diese Verbesserung wurde vorgenommen, um [verwaiste mehrstufige Dateien](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) bei, Entfernen eines Serverendpunkts zu verhindern. Wenn der Serverendpunkt neu erstellt wird, werden die Analysepunkte für die mehrstufigen Dateien auf dem Server neu erstellt.  
 
- Verbesserungen von Leistung und Zuverlässigkeit 
    - Die Anzahl der Fehler bei nochmaligem Aufrufen wurde verringert. Die Größe erneuter Aufrufe wird nun automatisch gemäß der Netzwerkbandbreite angepasst. 
    - Die Downloadleistung beim Hinzufügen eines neuen Servers zu einer Synchronisierungsgruppe wurde verbessert. 
    - Die Anzahl der Dateien wurde verringert, die aufgrund von Einschränkungskonflikten nicht synchronisiert werden. 
    - In bestimmten Szenarien schlagen mehrstufige Dateien fehl oder werden unerwartet abgerufen, wenn der Serverendpunktpfad ein Volumebereitstellungspunkt ist.
    
### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar. Weitere Informationen finden Sie unter [Auf mehrstufige Dateien auf dem Server kann nach dem Löschen eines Serverendpunkts nicht zugegriffen werden](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint).
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Wenn sich die Datei „pagefile.sys“ auf einem Volume befindet, für das Cloudtiering aktiviert ist, kann es zu einem Fehler beim Tiering von Dateien kommen. Die Datei „pagefile.sys“ sollte sich auf einem Volume befinden, auf dem Cloudtiering deaktiviert ist.

## <a name="agent-version-8000"></a>Agent-Version 8.0.0.0
Die folgenden Versionshinweise gelten für Version 8.0.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 8. Oktober 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Leistungsverbesserungen beim Wiederherstellen
    - Schnellere Wiederherstellungszeiten bei einer Wiederherstellung über Azure Backup. Wiederhergestellte Dateien werden auf Azure-Dateisynchronisierungsservern viel schneller wieder synchronisiert. 
- Verbesserte Benutzeroberfläche des Portals für Cloudtiering  
    - Wenn Sie mehrstufige Dateien haben, deren Abruf fehlschlägt, können Sie jetzt die Abruffehler in den Eigenschaften des Serverendpunkts anzeigen. Außerdem werden bei der Integrität der Serverendpunkte jetzt ein Fehler und Schritte zur Entschärfung angezeigt, wenn der Filtertreiber für das Cloudtiering nicht auf den Server geladen wurde.
- Einfachere Agentinstallation
    - Das PowerShell-Modul „Az\AzureRM“ ist zum Registrieren des Servers nicht mehr erforderlich, sodass die Installation einfacher und schnell durchgeführt werden kann.
- Sonstige Verbesserungen bei Leistung und Zuverlässigkeit

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar. Weitere Informationen finden Sie unter [Auf mehrstufige Dateien auf dem Server kann nach dem Löschen eines Serverendpunkts nicht zugegriffen werden](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint).
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.

## <a name="agent-version-7200"></a>Agent-Version 7.2.0.0
Die folgenden Versionshinweise gelten für Version 7.2.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 24. Juli 2019). Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 7.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Der Speichersynchronisierungs-Agent (FileSyncSvc) stürzt ab, wenn die Proxykonfiguration NULL ist.
- Der Serverendpunkt startet BCDR (Fehler: 0x80c80257 – ECS_E_BCDR_IN_PROGRESS), wenn mehrere Endpunkte auf dem Server den gleichen Namen haben.
- Verbesserte Zuverlässigkeit des Cloudtierings

## <a name="agent-version-7100"></a>Agent-Version 7.1.0.0
Die folgenden Versionshinweise gelten für Version 7.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 12. Juli 2019). Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 7.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Zugreifen auf oder Durchsuchen eines Serverendpunkt-Speicherorts per SMB ist unter Windows Server 2012 R2 langsam. 
- Höhere CPU-Auslastung nach Installation von Version 6 des Azure-Dateisynchronisierungs-Agents
- Verbesserungen bei Cloudtiering-Telemetriedaten
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung.

## <a name="agent-version-7000"></a>Agent-Version 7.0.0.0
Die folgenden Versionshinweise gelten für Version 7.0.0.0 des Azure-Dateisynchronisierungs-Agents (veröffentlicht am 19. Juni 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung größerer Dateifreigaben
    - Mit der Vorschauversion für größere Azure-Dateifreigaben erhöhen wir auch unsere Supportgrenzwerte für die Dateisynchronisierung. In diesem ersten Schritt unterstützt die Azure-Dateisynchronisierung jetzt bis zu 25 TB und 50 Millionen Dateien pro Synchronisierungsnamespace. Füllen Sie das Formular unter https://aka.ms/azurefilesatscalesurvey aus, um sich für die Vorschauversion für große Dateifreigaben zu bewerben. 
- Unterstützung für Firewalleinstellungen und Einstellungen für ein virtuelles Netzwerk in Speicherkonten
    - Azure-Dateisynchronisierung unterstützt jetzt die Firewalleinstellungen und Einstellungen für ein virtuelles Netzwerk in Speicherkonten. Informationen dazu, wie Sie Ihre Bereitstellung konfigurieren müssen, damit sie mit den Firewalleinstellungen und den Einstellungen für ein virtuelles Netzwerk funktioniert, finden Sie unter [Konfigurieren von Firewall- und VNET-Einstellungen](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings).
- PowerShell-Cmdlet, um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren
    - Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet „Invoke-AzStorageSyncChangeDetection“ verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Dieses Cmdlet ist für Szenarien vorgesehen, in denen irgendein automatisierter Prozess Änderungen in der Azure-Dateifreigabe vornimmt oder die Änderungen von einem Administrator vorgenommen werden (etwa Verschieben von Dateien und Verzeichnissen in die Freigabe). Für Endbenutzeränderungen empfiehlt es sich, den Azure-Dateisynchronisierungs-Agent auf einem virtuellen IaaS-Computer zu installieren und Endbenutzern den Zugriff auf die Dateifreigabe über den virtuellen IaaS-Computer zu ermöglichen. Auf diese Weise werden alle Änderungen schnell mit anderen Agents synchronisiert, ohne dass das Cmdlet „Invoke-AzStorageSyncChangeDetection“ verwendet werden muss. Weitere Informationen hierzu finden Sie in der Dokumentation von [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection).
- Verbessertes Portalverhalten, wenn Dateien vorliegen, die nicht synchronisiert sind
    - Wenn Sie Dateien haben, die nicht synchronisiert werden können, wird nun zwischen vorübergehenden und dauerhaften Fehlern im Portal unterschieden. Vorübergehende Fehler werden in der Regel automatisch aufgelöst, ohne dass eine Administratoraktion erforderlich ist. Beispielsweise wird eine Datei, die momentan verwendet wird, erst synchronisiert, nachdem das Dateihandle geschlossen wurde. Bei dauerhaften Fehlern wird nun die Anzahl der Dateien angezeigt, von jedem Fehler betroffen sind. Die Anzahl der dauerhaften Fehler wird auch in der „Dateien ohne Synchronisierung“-Spalte aller Serverendpunkte einer Synchronisierungsgruppe angezeigt.
- Verbesserte Azure Backup-Wiederherstellung auf Dateiebene
    - Einzelne Dateien, die mit Azure Backup wiederhergestellt werden, werden jetzt schneller erkannt und mit dem Serverendpunkt synchronisiert.
- Verbesserte Zuverlässigkeit des Cmdlets für Cloudtieringrückrufe 
    - Ähnlich wie bei Robocopy können Kunden jetzt mithilfe des Cmdlets „Invoke-StorageSyncFileRecall“ die Anzahl der Wiederholungsversuche pro Datei und die Wiederholungsverzögerung pro Datei angeben. Zuvor wurden mit diesem Cmdlet alle mehrstufigen Dateien unter einem angegebenen Pfad in zufälliger Reihenfolge abgerufen. Mit dem Parameter „new -Order“ ruft dieses Cmdlet die heißesten Daten zuerst ab und berücksichtigt die Cloudtiering-Richtlinie (Aufruf beenden, wenn die Datumsrichtlinie oder der freie Speicherplatz auf dem Volumen erfüllt ist – je nachdem, was zuerst geschieht).
- Unterstützung nur für TLS 1.2 (TLS 1.0 und 1.1 sind deaktiviert)
    - Die Azure-Dateisynchronisierung unterstützt jetzt die Verwendung von TLS 1.2 nur auf Servern, für die TLS 1.0 und 1.1 deaktiviert wurden. Vor dieser Verbesserung ist bei der Serverregistrierung ein Fehler aufgetreten, wenn TLS 1.0 und 1.1 auf dem Server deaktiviert war.
- Verschiedene Verbesserungen in Bezug auf die Leistung und Zuverlässigkeit für die Synchronisierung und das Cloudtiering
    - Dieses Release enthält mehrere Verbesserungen in Bezug auf die Zuverlässigkeit und Leistung. Mit einigen Maßnahmen soll erreicht werden, dass das Cloudtiering effizienter wird und die Azure-Dateisynchronisierung insgesamt in diesen Situationen besser funktioniert, wenn Sie einen Zeitplan für die Bandbreitendrosselung festgelegt haben.

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.

## <a name="agent-version-6300"></a>Agent-Version 6.3.0.0
Die folgenden Versionshinweise gelten für Version 6.3.0.0 des Azure-Dateisynchronisierungs-Agents, die am 27. Juni 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Zugreifen auf oder Durchsuchen eines Serverendpunkt-Speicherorts per SMB ist unter Windows Server 2012 R2 langsam 
- Höhere CPU-Auslastung nach Installation von Version 6 des Azure-Dateisynchronisierungs-Agents
- Verbesserungen bei Cloudtiering-Telemetriedaten

## <a name="agent-version-6200"></a>Agent-Version 6.2.0.0
Die folgenden Versionshinweise gelten für Version 6.2.0.0 des Azure-Dateisynchronisierungs-Agents, die am 13. Juni 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Nach dem Erstellen eines Serverendpunkts kann es zu einer hohen CPU-Auslastung kommen, wenn per Hintergrundrückruf Dateien auf den Server heruntergeladen werden
- Für Synchronisierungs- und Cloudtieringvorgänge tritt aufgrund eines Tokenablaufs ggf. der Fehler ECS_E_SERVER_CREDENTIAL_NEEDED auf
- Beim Rückruf einer Datei tritt ggf. ein Fehler auf, wenn die URL zum Herunterladen der Datei reservierte Zeichen enthält 

## <a name="agent-version-6100"></a>Agent-Version 6.1.0.0
Die folgenden Versionshinweise gelten für Version 6.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 6. Mai 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 6.0.0.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Behoben: In Windows Admin Center konnte die Version des Agents und die Konfiguration des Serverendpunkts auf Servern nicht angezeigt werden, auf denen die Version 6.0 des Azure-Dateisynchronisierungs-Agents installiert ist.

## <a name="agent-version-6000"></a>Agent-Version 6.0.0.0
Die folgenden Versionshinweise gelten für Version 6.0.0.0 des Azure-Dateisynchronisierungs-Agents, die am 22. April 2019 veröffentlicht wurde.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für automatische Updates des Agents
  - Gemäß Ihrem Feedback haben wir dem Azure-Dateisynchronisierungsserver-Agent ein Feature für automatische Updates hinzugefügt. Weitere Informationen finden Sie unter [Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#azure-file-sync-agent-update-policy).
- ACL-Unterstützung für Azure-Dateifreigabe
  - Die Azure-Dateisynchronisierung hat schon immer das Synchronisieren von ACLs zwischen Serverendpunkten unterstützt. Die ACLs wurden bisher jedoch nicht mit dem Cloudendpunkt synchronisiert (Azure-Dateifreigabe). Dieses Release unterstützt nun die Synchronisierung von ACLs zwischen Server- und Cloudendpunkten.
- Paralleles Hochladen und Herunterladen während Synchronisierungssitzungen für einen Serverendpunkt 
  - Serverendpunkte unterstützen nun das parallele Hoch- und Herunterladen von Dateien. Sie müssen also nicht mehr warten, bis ein Download abgeschlossen ist, bis Dateien in die Azure-Dateifreigabe hochgeladen werden können. 
- Neue Cloudtiering-Cmdlets für den Abruf des Volumens und des Tieringstatus
  - Mithilfe von zwei neuen, für den Server lokalen PowerShell-Cmdlets können Sie Informationen zum Cloudtiering und Dateiabruf erhalten. Damit stehen Protokollinformationen aus zwei Ereigniskanälen auf dem Server zur Verfügung:
    - „Get-StorageSyncFileTieringResult“ listet alle Dateien mit zugehörigen Pfaden auf, für die noch kein Tiering durchgeführt wurde, und führt den Grund dafür auf.
    - „Get-StorageSyncFileRecallResult“ erstellt einen Bericht über alle Dateiabrufereignisse. Jede abgerufene Datei wird zusammen mit ihrem Pfad aufgeführt. Außerdem wird angegeben, ob der Abruf erfolgreich war oder zu einem Fehler führte.
  - Standardmäßig können beide Ereigniskanäle jeweils bis zu 1 MB speichern. Sie können die Anzahl der Dateien, für die ein Bericht erstellt wird, erhöhen, indem Sie die Größe der Ereigniskanäle erhöhen.
- Unterstützung für den FIPS-Modus
  - Die Azure-Dateisynchronisierung unterstützt nun das Aktivieren des FIPS-Modus auf Servern, auf denen der Azure-Dateisynchronisierungs-Agent installiert ist.
    - Bevor Sie den FIPS-Modus auf Ihrem Server aktivieren, installieren Sie den Azure-Dateisynchronisierungs-Agent und das [PackageManagement-Modul](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) auf Ihrem Server. Wenn der FIPS-Modus bereits auf Ihrem Server aktiviert wurde, können Sie das [PackageManagement-Modul](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2)[manuell auf Ihren Server herunterladen](/powershell/scripting/gallery/how-to/working-with-packages/manual-download).
- Verschiedene Zuverlässigkeitverbesserungen für Cloudtiering und Synchronisierung

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.

## <a name="agent-version-5200"></a>Agent-Version 5.2.0.0
Die folgenden Versionshinweise gelten für Version 5.2.0.0 des Azure-Dateisynchronisierungs-Agents, die am 4. April 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 5.0.2.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Verbesserte Zuverlässigkeit bei Features für Offlinedatenübertragung und Datenübertragungsfortsetzung
- Verbesserungen bei den Synchronisierungstelemetriedaten

## <a name="agent-version-5100"></a>Agent-Version 5.1.0.0
Die folgenden Versionshinweise gelten für Version 5.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 7. März 2019 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für Version 5.0.2.0 angegeben sind.

Liste der in dieser Version behobenem Probleme:  
- Bei der Dateisynchronisierung tritt möglicherweise Fehler „0x80c8031d (ECS_E_CONCURRENCY_CHECK_FAILED)“, wenn die Änderungsenumeration auf dem Server nicht funktioniert.
- Wenn eine Synchronisierungssitzung oder eine Datei den Fehler „0x80072f78 (WININET_E_INVALID_SERVER_RESPONSE)“ empfängt, wiederholt die Synchronisierung den Vorgang nicht.
- Beim Synchronisieren von Dateien tritt möglicherweise der Fehler „0x80c80203 (ECS_E_SYNC_INVALID_STAGED_FILE)“ auf.
- Beim Zurückrufen von Dateien tritt möglicherweise eine hohe Arbeitsspeicherauslastung auf.
- Verbesserungen bei Cloudtiering-Telemetriedaten 

## <a name="agent-version-5020"></a>Agent-Version 5.0.2.0
Die folgenden Versionshinweise gelten für Version 5.0.2.0 des Azure-Dateisynchronisierungs-Agents (veröffentlicht am 12. Februar 2019).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für Azure Government-Cloud
  - Wir haben Vorschauunterstützung für die Azure Government-Cloud hinzugefügt. Dazu ist ein zugelassenes Abonnement und ein spezieller Agent-Download von Microsoft erforderlich. Wenn Sie Zugriff auf die Vorschau erhalten möchten, senden Sie uns bitte unter [AzureFiles@microsoft.com](mailto:AzureFiles@microsoft.com) eine E-Mail.
- Unterstützung für Datendeduplizierung
    - Die Datendeduplizierung wird jetzt mit aktiviertem Cloudtiering unter Windows Server 2016 und Windows Server 2019 vollständig unterstützt. Durch das Aktivieren der Deduplizierung auf einem Volume mit aktiviertem Cloudtiering können Sie weitere Dateien lokal zwischenspeichern, ohne mehr Speicher bereitstellen zu müssen.
- Unterstützung für Offlinedatenübertragung (z.B. über Data Box)
    - Migrieren Sie mühelos große Datenmengen in die Azure-Dateisynchronisierung über das von Ihnen gewählte Mittel. Sie können Azure Data Box, AzCopy und sogar Migrationsdienste von Drittanbietern auswählen. Bei Data Box benötigen Sie keine riesigen Bandbreiten, um Ihre Daten in Azure zu übertragen – senden Sie sie einfach per E-Mail dorthin! Weitere Informationen finden Sie unter [Offlinedatenübertragung – Dokumentation](https://aka.ms/AFS/OfflineDataTransfer).
- Verbesserte Synchronisierungsleistung
    - Kunden mit mehreren Serverendpunkten auf demselben Volume haben vor dieser Release möglicherweise eine langsame Synchronisierungsleistung festgestellt. Die Azure-Dateisynchronisierung erstellt einmal pro Tag eine temporäre VSS-Momentaufnahme auf dem Server, um Dateien mit offenen Handles zu synchronisieren. Jetzt unterstützt die Synchronisierung mehrere Serverendpunkte, die auf einem Volume synchronisieren, wenn eine VSS-Synchronisierungssitzung aktiv ist. Es muss nicht mehr gewartet werden, bis eine VSS-Synchronisierungssitzung abgeschlossen ist, damit die Synchronisierung auf anderen Serverendpunkten auf dem Volume fortgesetzt werden kann.
- Verbesserte Überwachung im Portal
    - Im Speichersynchronisierungsdienst-Portal wurden Diagramme hinzugefügt, um Folgendes anzuzeigen:
        - Anzahl der synchronisierten Dateien
        - Größe der übertragenen Daten
        - Anzahl der Dateien ohne Synchronisierung
        - Größe der zurückgerufenen Daten
        - Status der Serverkonnektivität
    - Weitere Informationen finden Sie unter [Überwachen der Azure-Dateisynchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring).
- Verbesserte Skalierbarkeit und Zuverlässigkeit
    - Die maximale Anzahl von Dateisystemobjekten (Verzeichnisse und Dateien) in einem Verzeichnis wurde auf 1.000.000 erhöht. Das vorherige Limit lag bei 200.000.
    - Wenn eine Übertragung bei großen Dateien unterbrochen wird, versucht die Synchronisierung, die Datenübertragung fortzusetzen, statt die Daten erneut zu übertragen. 

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoptionen „Windows Server Core“ oder „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.
- Der FIPS-Modus wird nicht unterstützt und muss deaktiviert werden. 

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, für den der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der DACL-Teil (besitzerverwaltete Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, sofern dieser größer als 2 KB ist. (Dieses Problem trifft nur zu, wenn Sie für ein einzelnes Element über mehr als ca. 40 Zugriffssteuerungseinträge verfügen.)
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Auf mehrstufige Dateien kann nicht mehr zugegriffen werden, wenn für die Dateien vor dem Löschen des Serverendpunkts kein Rückruf erfolgt. Erstellen Sie den Serverendpunkt neu, um den Zugriff auf die Dateien wiederherzustellen. Nachdem 30 Tage vergangen sind, seitdem der Serverendpunkt gelöscht wurde, oder wenn der Cloudendpunkt gelöscht wurde, sind mehrstufige Dateien, für die kein Rückruf durchgeführt wurde, nicht mehr verwendbar.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe oder ein anderes Abonnement im vorhandenen Azure AD-Mandanten verschoben werden. Wenn das Speicherkonto verschoben wird, müssen Sie dem Hybrid-Dateisynchronisierungsdienst Zugriff auf das Speicherkonto gewähren (siehe [Sicherstellen, dass die Azure-Dateisynchronisierung Zugriff auf das Speicherkonto besitzt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Die Azure-Dateisynchronisierung unterstützt nicht das Verschieben des Abonnements in einen anderen Azure AD-Mandanten.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
- Beim Anzeigen von Dateieigenschaften über einen SMB-Client sieht es aufgrund der Zwischenspeicherung von Dateimetadaten unter Umständen so aus, als wäre das Offlineattribut nicht korrekt festgelegt.
