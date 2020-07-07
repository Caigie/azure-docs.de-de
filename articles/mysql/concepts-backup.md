---
title: Sichern und Wiederherstellen – Azure Database for MySQL
description: Enthält Informationen zu automatischen Sicherungen und zur Wiederherstellung Ihres Azure Database for MySQL-Servers.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: 3a6162bb381f4e54114e3cabbf138f5b1c6aaae0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80373033"
---
# <a name="backup-and-restore-in-azure-database-for-mysql"></a>Sicherung und Wiederherstellung in Azure Database for MySQL

Für Azure Database for MySQL werden Sicherungen automatisch erstellt und in einem vom Benutzer konfigurierten lokal redundanten oder georedundanten Speicher gespeichert. Sicherungen können verwendet werden, um für Ihren Server den Stand zu einem bestimmten Zeitpunkt wiederherzustellen. Sicherungen und Wiederherstellungen sind wesentliche Bestandteile jeder Strategie für Geschäftskontinuität, da Ihre Daten so vor versehentlichen Beschädigungen und Löschungen geschützt werden.

## <a name="backups"></a>Backups

Azure Database for MySQL nimmt Sicherungen der Datendateien und der Transaktionsprotokolle vor. Abhängig von der unterstützten maximalen Speichergröße werden entweder vollständige oder differenzielle Sicherungen (Server mit maximal 4 TB Speicher) oder Sicherungsmomentaufnahmen (Server mit bis zu 16 TB Speicher) angelegt. Dank dieser Sicherungen können Sie für einen Server den Stand zu einem beliebigen Zeitpunkt wiederherstellen, der innerhalb Ihres konfigurierten Aufbewahrungszeitraums für Sicherungen liegt. Die Standardaufbewahrungsdauer für Sicherungen beträgt sieben Tage. Mit einer [optionalen Konfiguration](howto-restore-server-portal.md#set-backup-configuration) können Sie einen Zeitraum von bis zu 35 Tagen festlegen. Zur Verschlüsselung aller Sicherungen wird die AES-Verschlüsselung mit 256 Bit verwendet.

Diese Sicherungsdateien sind nicht für den Benutzer verfügbar und können nicht exportiert werden. Sie können nur für Wiederherstellungsvorgänge in Azure Database for MySQL verwendet werden. Zum Kopieren einer Datenbank können Sie [mysqldump](concepts-migrate-dump-restore.md) verwenden.

### <a name="backup-frequency"></a>Sicherungshäufigkeit

Für Server mit einer maximal unterstützten Speicherkapazität von 4 TB werden vollständige Sicherungen üblicherweise einmal pro Woche und differenzielle Sicherungen zweimal täglich durchgeführt. Für Server mit einer unterstützten Speicherkapazität von bis zu 16 TB wird mindestens einmal täglich eine Momentaufnahmesicherung durchgeführt. Transaktionsprotokollsicherungen finden in beiden Fällen alle fünf Minuten statt. Die erste Momentaufnahme der vollständigen Sicherung wird unmittelbar nach der Erstellung des Servers eingeplant. Für einen großen wiederhergestellten Server kann die erste vollständige Sicherung länger dauern. Der früheste Zeitpunkt, der für einen neuen Server wiederhergestellt werden kann, ist der Zeitpunkt, zu dem die erste vollständige Sicherung erstellt wurde. Da Momentaufnahmen sofort vorliegen, können Server, die bis zu 16 TB Speicherkapazität unterstützen, in dem Zustand wiederhergestellt werden, der zum Zeitpunkt der Erstellung vorlag.

### <a name="backup-redundancy-options"></a>Optionen für Sicherungsredundanz

Bei Azure Database for MySQL können Sie in den Tarifen „Allgemein“ und „Arbeitsspeicheroptimiert“ flexibel zwischen lokal redundantem und georedundantem Sicherungsspeicher wählen. Wenn die Sicherungen in einem georedundanten Sicherungsspeicher gespeichert werden, werden sie nicht nur in der Region vorgehalten, in der Ihr Server gehostet wird. Sie werden außerdem in einem [gekoppelten Datencenter](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) repliziert. Dies erhöht den Schutz und ermöglicht in einem Notfall die Wiederherstellung Ihres Servers in einer anderen Region. Im Tarif „Basic“ ist nur lokal redundanter Sicherungsspeicher verfügbar.

> [!IMPORTANT]
> Das Konfigurieren von lokal redundantem oder georedundantem Speicher für die Sicherung ist nur während der Erstellung des Servers zulässig. Nachdem der Server bereitgestellt wurde, können Sie die Option für die Sicherungsspeicherredundanz nicht mehr ändern.

### <a name="backup-storage-cost"></a>Kosten für Sicherungsspeicher

Bei Azure Database for MySQL werden bis zu 100% Ihres bereitgestellten Serverspeichers ohne zusätzliche Kosten als Sicherungsspeicher hinzugefügt. Dies ist normalerweise für die Aufbewahrung von Sicherungen über einen Zeitraum von sieben Tagen geeignet. Wenn zusätzlicher Sicherungsspeicher verwendet wird, wird dies in GB pro Monat berechnet.

Beispiel: Wenn Sie einen Server mit 250 GB bereitgestellt haben, verfügen Sie ohne zusätzliche Kosten über 250 GB an Sicherungsspeicher. Für Speicher, der über 250 GB hinausgeht, werden Kosten berechnet.

## <a name="restore"></a>Restore

Wenn in Azure Database for MySQL eine Wiederherstellung durchgeführt wird, wird aus den Sicherungen des ursprünglichen Servers ein neuer Server erstellt und alle auf dem Server befindlichen Datenbanken werden wiederhergestellt.

Es gibt zwei Arten der Wiederherstellung:

- Die **Point-in-Time-Wiederherstellung** ist für beide Sicherungsredundanzoptionen verfügbar. Es wird unter Verwendung einer Kombination aus vollständigen Sicherungen und Transaktionsprotokollsicherungen in der Region des ursprünglichen Servers ein neuer Server erstellt.
- Die **Geowiederherstellung** ist nur verfügbar, wenn Sie Ihren Server für georedundanten Speicher konfiguriert haben. Hierbei können Sie den Server unter Verwendung der aktuellsten Sicherung in einer anderen Region wiederherstellen.

Die geschätzte Wiederherstellungszeit hängt von verschiedenen Faktoren ab, z.B. der Datenbankgröße, Transaktionsprotokollgröße und Netzwerkbandbreite sowie der Gesamtzahl von Datenbanken, die gleichzeitig in derselben Region wiederhergestellt werden müssen. Die Wiederherstellungszeit beträgt für gewöhnlich weniger als 12 Stunden.

> [!IMPORTANT]
> Gelöschte Server **können nicht** wiederhergestellt werden. Wenn Sie den Server löschen, werden auch alle Datenbanken gelöscht, die zum Server gehören, und können nicht wiederhergestellt werden. Um Serverressourcen nach der Bereitstellung vor versehentlichem Löschen oder unerwarteten Änderungen zu schützen, können Administratoren [Verwaltungssperren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) nutzen.

### <a name="point-in-time-restore"></a>Wiederherstellung bis zu einem bestimmten Zeitpunkt

Unabhängig von Ihrer gewählten Option für die Sicherungsredundanz können Sie eine Wiederherstellung für einen beliebigen Zeitpunkt durchführen, der innerhalb Ihres Aufbewahrungszeitraums für Sicherungen liegt. Ein neuer Server wird in derselben Azure-Region wie der ursprüngliche Server erstellt. Bei der Erstellung werden die Konfiguration für den Tarif, die Computegeneration, die Anzahl von V-Kernen, die Speichergröße, den Aufbewahrungszeitraum für Sicherungen und die Option für Sicherungsredundanz des ursprünglichen Servers verwendet.

Die Point-in-Time-Wiederherstellung ist für viele Szenarien hilfreich. Beispiele hierfür sind Fälle, in denen ein Benutzer versehentlich Daten löscht oder eine wichtige Tabelle oder Datenbank entfernt oder in denen eine Anwendung aufgrund eines Defekts fälschlicherweise Daten durch fehlerhafte Daten überschreibt.

Unter Umständen müssen Sie warten, bis die nächste Transaktionsprotokollsicherung erstellt wird, bevor Sie die Wiederherstellung für einen Zeitpunkt innerhalb der letzten fünf Minuten durchführen können.

### <a name="geo-restore"></a>Geowiederherstellung

Sie können einen Server in einer anderen Azure-Region wiederherstellen, in der der Dienst verfügbar ist, wenn Sie Ihren Server für georedundante Sicherungen konfiguriert haben. Server, die bis zu 4 TB Speicherkapazität unterstützen, können in der geografisch gekoppelten Region oder in einer beliebigen Region wiederhergestellt werden, die bis zu 16 TB Speicherkapazität unterstützt. Für Server, die bis zu 16 TB Speicherkapazität unterstützen, können Geosicherungen auch in beliebigen Regionen wiederhergestellt werden, die Server mit 16 TB unterstützen. Eine Liste der unterstützten Regionen finden Sie unter [Azure Database for MySQL-Tarife](concepts-pricing-tiers.md).

Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn Ihr Server aufgrund eines Incidents in der Region, in der der Server gehostet wird, nicht verfügbar ist. Wenn Ihre Datenbankanwendung wegen eines umfangreichen Incidents in einer Region nicht mehr verfügbar ist, können Sie einen Server aus den georedundanten Sicherungen auf einem Server in einer beliebigen anderen Region wiederherstellen. Bei der Geowiederherstellung wird die aktuellste Sicherung des Servers verwendet. Zwischen der Erstellung einer Sicherung und der Replikation in einer anderen Region kommt es zu einer Verzögerung. Diese Verzögerung kann bis zu einer Stunde betragen. Folglich kann bei einem Notfall ein Datenverlust von bis zu einer Stunde auftreten.

Während der Geowiederherstellung können folgende Serverkonfigurationen geändert werden: Computegeneration, virtueller Kern, Aufbewahrungszeitraum für die Sicherung und Sicherungsredundanzoptionen. Allerdings wird während der Geowiederherstellung nicht das Ändern des Tarifs („Basic“, „Allgemein“ oder „Arbeitsspeicheroptimiert“) oder der Speichergröße unterstützt.

### <a name="perform-post-restore-tasks"></a>Durchführen der Aufgaben nach der Wiederherstellung

Nach beiden Wiederherstellungsverfahren sollten Sie die folgenden Aufgaben durchführen, um Ihre Benutzer und Anwendungen wieder in den betriebsbereiten Zustand zu versetzen:

- Umleiten von Clients und Clientanwendungen an den neuen Server, wenn der neue Server den ursprünglichen Server ersetzen soll
- Stellen Sie sicher, dass geeignete VNET-Regeln vorhanden sind, damit Benutzer eine Verbindung herstellen können. Diese Regeln werden nicht vom ursprünglichen Server kopiert.
- Sicherstellen, dass geeignete Anmeldungen und Berechtigungen auf Datenbankebene vorhanden sind
- Konfigurieren der erforderlichen Warnungen

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Geschäftskontinuität finden Sie in der  [Übersicht über die Geschäftskontinuität](concepts-business-continuity.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über das Azure-Portal finden Sie unter  [Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit dem Azure-Portal](howto-restore-server-portal.md).
- Informationen zur Wiederherstellung des Zustands zu einem bestimmten Zeitpunkt über die Azure-Befehlszeilenschnittstelle finden Sie unter  [Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit der Azure CLI](howto-restore-server-cli.md).
