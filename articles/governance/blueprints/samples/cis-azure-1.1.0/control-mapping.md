---
title: Steuerungen des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“
description: Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“ zu Azure Policy.
ms.date: 05/12/2020
ms.topic: sample
ms.openlocfilehash: b6029e147af49cfb91078c6228615c32ad2db5fe
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84167229"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Empfehlungszuordnung des Blaupausenbeispiels „CIS Microsoft Azure Foundations Benchmark“

Im folgenden Artikel erfahren Sie, wie das Azure Blueprints-Blaupausenbeispiel „CIS Microsoft Azure Foundations Benchmark“ den Empfehlungen für „CIS Microsoft Azure Foundations Benchmark“ zugeordnet wird. Weitere Informationen zu den Empfehlungen finden Sie unter [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/).

Die folgenden Zuordnungen gelten für die Empfehlungen für **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Empfehlungszuordnung navigieren.
Viele der zugeordneten Empfehlungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Preview\] Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 recommendations and deploy specific VM Extensions to support audit requirements** ([Vorschauversion] Empfehlungen für „CIS Microsoft Azure Foundations Benchmark v1.1.0“ überwachen und spezifische VM-Erweiterungen zur Unterstützung von Überwachungsanforderungen bereitstellen), und wählen Sie sie aus.

> [!IMPORTANT]
> Jede Steuerung unten ist einer oder mehreren [Azure Policy](../../../policy/overview.md)-Definitionen zugeordnet. Diese Richtlinien können Ihnen bei der [Konformitätsbewertung](../../../policy/how-to/get-compliance-data.md) mit der Steuerung helfen. Es gibt jedoch oft keine 1:1- oder vollständige Übereinstimmung zwischen einer Steuerung und einer bzw. mehreren Richtlinien. Daher bezieht sich **Konform** in Azure Policy nur auf die Richtlinien selbst und gewährleistet nicht die vollständige Konformität mit allen Anforderungen einer Steuerung. Außerdem enthält der Kompatibilitätsstandard Steuerungen, die derzeit von keiner Azure Policy-Definition abgedeckt werden. Daher ist die Konformität in Azure Policy nur eine partielle Ansicht Ihres gesamten Konformitätsstatus. Die Zuordnungen zwischen Steuerungen und Azure Policy-Definitionen für dieses Konformitätsblaupausenbeispiel können sich im Laufe der Zeit ändern. Den Änderungsverlaufs finden Sie im [GitHub-Commit-Verlauf](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md).

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1. Sicherstellen, dass die mehrstufige Authentifizierung für alle privilegierten Benutzer aktiviert ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie die Überwachung durchführen können, wenn die mehrstufige Authentifizierung für privilegierte Azure Active Directory-Konten nicht aktiviert ist.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2. Sicherstellen, dass die mehrstufige Authentifizierung für alle nicht privilegierten Benutzer aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit denen Sie die Überwachung durchführen können, wenn die mehrstufige Authentifizierung für nicht privilegierte Azure Active Directory-Konten nicht aktiviert ist.

- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3. Sicherstellen, dass keine Gastbenutzer vorhanden sind

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie die Überwachung für Gastkonten durchführen können, die ggf. entfernt werden müssen.

- Externe Konten mit Leseberechtigungen sollten aus Ihrem Abonnement entfernt werden
- Externe Konten mit Schreibberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.

## <a name="123-ensure-that-no-custom-subscription-owner-roles-are-created"></a>1.23 Sicherstellen, dass keine benutzerdefinierten Abonnementbesitzerrollen erstellt werden

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, die Sie bei der Überwachung auf benutzerdefinierte Abonnementbesitzerrollen unterstützen, die ggf. entfernt werden müssen.

- Es dürfen keine benutzerdefinierten Abonnementbesitzerrollen vorhanden sein.

## <a name="21-ensure-that-standard-pricing-tier-is-selected"></a>2.1. Sicherstellen, dass der Tarif „Standard“ ausgewählt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Netzwerke und virtuelle Computer überwachen können, für die der Standard-Tarif von Security Center nicht aktiviert ist.

- Standard-Tarif von Security Center muss ausgewählt sein

## <a name="22-ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>2.2. Sicherstellen, dass „Automatische Bereitstellung des Überwachungs-Agents“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die automatische Bereitstellung des Log Analytics-Agents aktiviert ist.

- Für Ihr Abonnement muss die automatische Bereitstellung des Log Analytics-Überwachungs-Agents aktiviert sein

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Systemupdates überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf virtuellen Computern Systemupdates installiert werden.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Betriebssystem-Sicherheitsrisiken überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie eine Überwachung auf unbehandelte Sicherheitsrisiken von virtuellen Computern durchführen können.

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Endpoint Protection überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der Endpunktschutz auf den virtuellen Computern aktiviert ist.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Datenträgerverschlüsselung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträger von virtuellen Computern verschlüsselt sind.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="27-ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>2.7. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Netzwerksicherheitsgruppen überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie virtuelle Computer mit Internetzugriff schützen können.

- Auf virtuelle Computer mit Internetzugang müssen Empfehlungen zur adaptiven Netzwerkhärtung angewendet werden.

## <a name="29-ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>2.9. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Next Generation Firewall-Überwachung aktivieren“ nicht deaktiviert ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie Subnetze und virtuelle Computer vor Bedrohungen schützen können, indem Sie den Zugriff einschränken. Die Security Center-Richtlinie, auf die von dieser CIS Microsoft Azure Foundations Benchmark-Empfehlung verwiesen wird, wurde durch zwei neue Empfehlungen ersetzt. Die unten aufgeführten Richtlinien gelten für die neuen Empfehlungen.

- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Virtuelle Computer mit Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Sicherheitsrisikobewertung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Sicherheitsrisiken erkannt und beseitigt werden.

- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.

## <a name="211-ensure-asc-default-policy-setting-monitor-storage-blob-encryption-is-not-disabled"></a>2.11. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Speicherblobverschlüsselung überwachen“ nicht deaktiviert ist

Die Azure Storage-Verschlüsselung wird für alle neuen und vorhandenen Speicherkonten aktiviert und kann nicht deaktiviert werden. (Hierbei handelt es sich um eine Azure-Standardfunktion ohne Richtlinienzuweisung.)

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „JIT-Netzwerkzugriff überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie den Zugriff auf virtuelle Computer steuern können.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.

## <a name="213-ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>2.13. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „Monitor Adaptive Application Whitelisting“ (Whitelist für adaptive Anwendungssteuerung überwachen) nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die adaptive Anwendungssteuerung auf den virtuellen Computern aktiviert ist.

- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="214-ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>2.14. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „SQL-Überwachung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung aktiviert ist.

- Die Überwachung in SQL Server muss aktiviert werden.

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15. Sicherstellen, dass die ASC-Standardrichtlinieneinstellung „SQL-Verschlüsselung überwachen“ nicht deaktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für SQL-Datenbanken Transparent Data Encryption aktiviert ist.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="216-ensure-that-security-contact-emails-is-set"></a>2.16. Sicherstellen, dass „E-Mail-Adressen für Sicherheitskontakt“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- Für Ihr Abonnement muss eine E-Mail-Adresse als Sicherheitskontakt angegeben sein

## <a name="217-ensure-that-security-contact-phone-number-is-set"></a>2.17. Sicherstellen, dass für den Sicherheitskontakt die „Telefonnummer“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- Für Ihr Abonnement muss eine Telefonnummer für den Sicherheitskontakt angegeben werden

## <a name="218-ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>2.18. Sicherstellen, dass „E-Mail-Benachrichtigung für Warnungen mit hohem Schweregrad senden“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- E-Mail-Benachrichtigung zu Warnungen mit hohem Schweregrad muss aktiviert sein

## <a name="219-ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>2.19. Sicherstellen, dass „E-Mails auch an Abonnementbesitzer senden“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Sicherheitsbenachrichtigungen richtig aktiviert sind.

- E-Mail-Benachrichtigung des Abonnementbesitzers bei Warnungen mit hohem Schweregrad muss aktiviert sein

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1. Sicherstellen, dass „Sichere Übertragung erforderlich“ aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die unsichere Verbindungen zulässig sind.

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7. Sicherstellen, dass die Standard-Netzwerkzugriffsregel für Speicherkonten auf „Verweigern“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die uneingeschränkter Zugriff zulässig ist.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen

## <a name="38-ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>3.8. Sicherstellen, dass „Vertrauenswürdige Microsoft-Dienste“ für den Speicherkontozugriff aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie Speicherkonten überwachen können, für die der Zugriff über vertrauenswürdige Microsoft-Dienste nicht zulässig ist.

- Speicherkonten sollten Zugriff von vertrauenswürdigen Microsoft-Diensten zulassen

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1. Sicherstellen, dass „Überwachung“ auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung aktiviert ist. 

- Die Überwachung in SQL Server muss aktiviert werden.

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2. Sicherstellen, dass „AuditActionGroups“ in der Überwachungsrichtlinie für eine SQL Server-Instanz ordnungsgemäß festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die SQL Server-Überwachung richtig konfiguriert ist.

- In den SQL-Überwachungseinstellungen sollten Aktionsgruppen zur Erfassung kritischer Aktivitäten konfiguriert sein.

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3. Sicherstellen, dass die Aufbewahrungsdauer für die Überwachung auf über 90 Tage festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass SQL Server-Protokolle mindestens 90 Tage lang aufbewahrt werden.

- SQL Server-Instanzen sollten mit einer Überwachungsaufbewahrungsdauer von über 90 Tagen konfiguriert werden.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4. Sicherstellen, dass „Advanced Data Security“ für eine SQL Server-Instanz auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Advanced Data Security auf Servern mit SQL-Datenbank und verwalteten SQL-Instanzen aktiviert ist.

- Advanced Data Security muss für Ihre SQL-Server aktiviert werden.
- Advanced Data Security muss für Ihre verwalteten SQL-Instanzen aktiviert werden.

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5. Sicherstellen, dass „Bedrohungstypen für Erkennung“ auf „Alle“ festgelegt ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Advanced Threat Protection auf Servern mit SQL-Datenbank und verwalteten SQL-Instanzen ordnungsgemäß konfiguriert ist.

- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für SQL Server auf „Alle“ festgelegt werden.
- Advanced Threat Protection-Typen sollten in den Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen auf „Alle“ festgelegt werden.

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6. Sicherstellen, dass „Warnungen senden an“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Benachrichtigungen für Advanced Data Security richtig aktiviert sind.

- Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7. Sicherstellen, dass „E-Mail-Dienst und Co-Administratoren“ aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Benachrichtigungen für Advanced Data Security richtig aktiviert sind.

- E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer sollten in den erweiterten Einstellungen für Datensicherheit in SQL Server aktiviert werden.
- In den Advanced Data Security-Einstellungen für die verwaltete SQL-Instanz müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8. Sicherstellen, dass der Azure Active Directory-Administrator konfiguriert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für Computer mit SQL Server ein Azure Active Directory-Administrator bereitgestellt wird.

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9. Sicherstellen, dass „Datenverschlüsselung“ für eine SQL-Datenbank auf „Ein“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für SQL-Datenbanken Transparent Data Encryption aktiviert ist.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10. Sicherstellen, dass die TDE-Schutzvorrichtung der SQL Server-Instanz mit BYOK (also unter Verwendung eines eigenen Schlüssels) verschlüsselt ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass die TDE-Schutzvorrichtung für Server mit SQL-Datenbank und verwalteten SQL-Instanzen mit Ihrem eigenen Schlüssel verschlüsselt ist.

- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.

## <a name="411-ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>4.11. Sicherstellen, dass „SSL-Verbindung erzwingen“ für den MySQL-Datenbankserver auf „AKTIVIERT“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für MySQL-Datenbankserver TLS-/SSL-Verbindungen erzwungen werden.

- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein

## <a name="412-ensure-server-parameter-log_checkpoints-is-set-to-on-for-postgresql-database-server"></a>4.12 Sicherstellen, dass der Serverparameter „log_checkpoints“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass PostgreSQL-Datenbankserver Prüfpunkte protokollieren.

- Die Protokollierung von Prüfpunkten sollte für PostgreSQL-Datenbankserver aktiviert sein.

## <a name="413-ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>4.13. Sicherstellen, dass „SSL-Verbindung erzwingen“ für den PostgreSQL-Datenbankserver auf „AKTIVIERT“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für PostgreSQL-Datenbankserver TLS-/SSL-Verbindungen erzwungen werden.

- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein

## <a name="414-ensure-server-parameter-log_connections-is-set-to-on-for-postgresql-database-server"></a>4.14 Sicherstellen, dass der Serverparameter „log_connections“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass PostgreSQL-Datenbankserver Verbindungen protokollieren.

- Die Protokollierung von Verbindungen sollte für PostgreSQL-Datenbankserver aktiviert sein.

## <a name="415-ensure-server-parameter-log_disconnections-is-set-to-on-for-postgresql-database-server"></a>4.15 Sicherstellen, dass der Serverparameter „log_disconnections“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass PostgreSQL-Datenbankserver Verbindungstrennungen protokollieren.

- Verbindungstrennungen sollten für PostgreSQL-Datenbankserver protokolliert werden.

## <a name="416-ensure-server-parameter-log_duration-is-set-to-on-for-postgresql-database-server"></a>4.16 Sicherstellen, dass der Serverparameter „log_duration“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass PostgreSQL-Datenbankserver die Dauer abgeschlossener Anweisungen protokollieren.

- Die Protokollierung der Dauer sollte für PostgreSQL-Datenbankserver aktiviert sein.

## <a name="417-ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>4.17 Sicherstellen, dass der Serverparameter „connection_throttling“ für den PostgreSQL-Datenbankserver auf „EIN“ festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie das Risiko von Brute-Force-Angriffen auf PostgreSQL-Datenbankserver reduzieren können.

- Die Verbindungsdrosselung muss für PostgreSQL-Datenbankserver aktiviert sein

## <a name="419-ensure-that-azure-active-directory-admin-is-configured"></a>4.19. Sicherstellen, dass der Azure Active Directory-Administrator konfiguriert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für Computer mit SQL Server ein Azure Active Directory-Administrator bereitgestellt wird. CIS Microsoft Azure Foundations Benchmark enthält diese Empfehlung. Es handelt sich aber um ein Duplikat von [Empfehlung 4.8](#48-ensure-that-azure-active-directory-admin-is-configured).

- Ein Azure Active Directory-Administrator sollte für SQL-Server-Instanzen bereitgestellt werden

## <a name="511-ensure-that-a-log-profile-exists"></a>5.1.1. Sicherstellen, dass ein Protokollprofil vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass für alle Azure-Abonnements ein Protokollprofil vorhanden ist. 

- Azure-Abonnements benötigen ein Protokollprofil für das Aktivitätsprotokoll

## <a name="512-ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>5.1.2. Sicherstellen, dass die Speicherdauer für Aktivitätsprotokolle auf mindestens 365 Tage festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Aktivitätsprotokolle mindestens ein Jahr lang aufbewahrt werden.

- Das Aktivitätsprotokoll muss mindestens ein Jahr lang aufbewahrt werden

## <a name="513-ensure-audit-profile-captures-all-the-activities"></a>5.1.3. Sicherstellen, dass mit dem Überwachungsprofil alle Aktivitäten erfasst werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass das Protokollprofil richtig konfiguriert ist.

- Das Azure Monitor-Protokollprofil muss Protokolle für die Kategorien „write“, „delete“ und „action“ erfassen

## <a name="514-ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>5.1.4. Sicherstellen, dass mit dem Protokollprofil die Aktivitätsprotokolle für alle Regionen erfasst werden (einschließlich global)

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass das Protokollprofil richtig konfiguriert ist.

- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen

## <a name="516-ensure-the-storage-account-containing-the-container-with-activity-logs-is-encrypted-with-byok-use-your-own-key"></a>5.1.6 Sicherstellen, dass das Speicherkonto, das den Container mit Aktivitätsprotokollen enthält, per BYOK (Bring Your Own Key) verschlüsselt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Speicherkonten, die Aktivitätsprotokolle enthalten, per BYOK verschlüsselt sind.

- Speicherkonto mit dem Container mit Aktivitätsprotokollen muss per BYOK verschlüsselt sein

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7. Sicherstellen, dass die Protokollierung für Azure Key Vault aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Diagnoseprotokolle für Schlüsseltresore aktiviert sind.

- Diagnoseprotokolle in Key Vault sollten aktiviert sein.

## <a name="521-ensure-that-activity-log-alert-exists-for-create-policy-assignment"></a>5.2.1 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Richtlinienzuweisung erstellen“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte Richtlinienvorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="522-ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group"></a>5.2.2 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Netzwerksicherheitsgruppe erstellen oder aktualisieren“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="523-ensure-that-activity-log-alert-exists-for-delete-network-security-group"></a>5.2.3 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Netzwerksicherheitsgruppe löschen“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="524-ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group-rule"></a>5.2.4 Sicherstellen, dass eine Aktivitätsprotokollwarnung für die Regel „Netzwerksicherheitsgruppe erstellen oder aktualisieren“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="525-ensure-that-activity-log-alert-exists-for-the-delete-network-security-group-rule"></a>5.2.5 Sicherstellen, dass eine Aktivitätsprotokollwarnung für die Regel „Netzwerksicherheitsgruppe löschen“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="526-ensure-that-activity-log-alert-exists-for-create-or-update-security-solution"></a>5.2.6 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Sicherheitslösung erstellen oder aktualisieren“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte Sicherheitsvorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="527-ensure-that-activity-log-alert-exists-for-delete-security-solution"></a>5.2.7 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Sicherheitslösung löschen“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte Sicherheitsvorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="528-ensure-that-activity-log-alert-exists-for-create-or-update-or-delete-sql-server-firewall-rule"></a>5.2.8 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „SQL Server-Firewallregel erstellen oder aktualisieren bzw. löschen“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="529-ensure-that-activity-log-alert-exists-for-update-security-policy"></a>5.2.9 Sicherstellen, dass eine Aktivitätsprotokollwarnung für „Sicherheitsrichtlinie aktualisieren“ vorhanden ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass spezifische Aktivitätsprotokollwarnungen vorhanden sind.

- Für bestimmte Sicherheitsvorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="61-ensure-that-rdp-access-is-restricted-from-the-internet"></a>6.1 Sicherstellen, dass der RDP-Zugriff aus dem Internet eingeschränkt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der RDP-Zugriff eingeschränkt ist.

- RDP-Zugriff über das Internet blockieren

## <a name="62-ensure-that-ssh-access-is-restricted-from-the-internet"></a>6.2 Sicherstellen, dass der SSH-Zugriff aus dem Internet eingeschränkt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der SSH-Zugriff eingeschränkt ist.

- SSH-Zugriff über das Internet blockieren

## <a name="65-ensure-that-network-watcher-is-enabled"></a>6.5. Sicherstellen, dass Network Watcher aktiviert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Network Watcher für alle Regionen mit bereitgestellten Ressourcen aktiviert ist. Für diese Richtlinie wird ein Parameterarray benötigt, in dem alle zutreffenden Regionen angegeben sind. Der Standardwert in dieser Richtlinien-Initiativendefinition lautet „eastus“.

- Network Watcher muss aktiviert sein

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1. Sicherstellen, dass der Betriebssystem-Datenträger verschlüsselt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträgerverschlüsselung auf den virtuellen Computern aktiviert ist.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2. Sicherstellen, dass reguläre Datenträger verschlüsselt sind

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die Datenträgerverschlüsselung auf den virtuellen Computern aktiviert ist.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.

## <a name="73-ensure-that-unattached-disks-are-encrypted"></a>7.3. Sicherstellen, dass „nicht angefügte Datenträger“ verschlüsselt sind

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass nicht angefügte Datenträger verschlüsselt sind.

- Nicht angefügte Datenträger müssen verschlüsselt werden

## <a name="74-ensure-that-only-approved-extensions-are-installed"></a>7.4. Sicherstellen, dass nur genehmigte Erweiterungen installiert werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass nur genehmigte Erweiterungen für virtuelle Computer installiert werden. Für diese Richtlinie wird ein Parameterarray benötigt, in dem alle genehmigten Erweiterungen für virtuelle Computer angegeben sind. Diese Richtlinien-Initiativendefinition enthält die vorgeschlagenen Standardwerte, die von Kunden überprüft werden sollten. 

- Es dürfen nur genehmigte VM-Erweiterungen installiert werden

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5. Sicherstellen, dass die neuesten Betriebssystempatches für alle virtuellen Computer angewendet werden

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf virtuellen Computern Systemupdates installiert werden.

- Systemupdates sollten auf Ihren Computern installiert sein.

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6. Sicherstellen, dass Endpoint Protection für alle virtuellen Computer installiert ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass der Endpunktschutz auf den virtuellen Computern aktiviert ist.

- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen

## <a name="84-ensure-the-key-vault-is-recoverable"></a>8.4. Sicherstellen, dass der Schlüsseltresor wiederhergestellt werden kann

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Schlüsseltresorobjekte nach dem versehentlichen Löschen wiederhergestellt werden können.

- Key Vault-Objekte müssen wiederherstellbar sein

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5. Aktivieren der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Kubernetes Service

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass die rollenbasierte Zugriffssteuerung in Kubernetes-Dienstclustern zum Verwalten der Berechtigungen verwendet wird.

- Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.

## <a name="91-ensure-app-service-authentication-is-set-on-azure-app-service"></a>9.1 Sicherstellen, dass die App Service-Authentifizierung für Azure App Service festgelegt ist

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass Anforderungen für App Service-Apps authentifiziert werden.

- Authentifizierung sollte für Ihre API-App aktiviert werden
- Authentifizierung sollte für Ihre Funktions-App aktiviert werden
- Authentifizierung sollte für Ihre Web-App aktiviert werden

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2. Sicherstellen, dass die Web-App den gesamten HTTP-Datenverkehr in Azure App Service an HTTPS umleitet

Mit dieser Blaupause wird eine [Azure Policy](../../../policy/overview.md)-Definition zugewiesen, mit der Sie sicherstellen können, dass auf Webanwendungen nur über sichere Verbindungen zugegriffen werden kann.

- Zugriff auf Webanwendung nur über HTTPS gestatten

## <a name="93-ensure-web-app-is-using-the-latest-version-of-tls-encryption"></a>9.3 Sicherstellen, dass die Web-App die neueste Version der TLS-Verschlüsselung verwendet

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste TLS-Version verwenden.

- In Ihrer API-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Funktions-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Web-App sollte die neueste TLS-Version verwendet werden.

## <a name="94-ensure-the-web-app-has-client-certificates-incoming-client-certificates-set-to-on"></a>9.4 Sicherstellen, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die Web-App auf „Ein“ festgelegt ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass eine Web-App nur für Clients mit gültigen Zertifikaten erreichbar ist.

- Stellen Sie sicher, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die API-App auf „Ein“ festgelegt ist.
- Stellen Sie sicher, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die Funktions-App auf „Ein“ festgelegt ist.
- Stellen Sie sicher, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die Web-App auf „Ein“ festgelegt ist.

## <a name="95-ensure-that-register-with-azure-active-directory-is-enabled-on-app-service"></a>9.5 Sicherstellen, dass „Registrierung bei Azure Active Directory“ für die API-App aktiviert ist

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps eine verwaltete Identität verwenden.

- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die API-App aktiviert ist.
- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die Funktions-App aktiviert ist.
- Stellen Sie sicher, dass „Registrierung bei Azure Active Directory“ für die Web-App aktiviert ist.

## <a name="96-ensure-that-net-framework-version-is-the-latest-if-used-as-a-part-of-the-web-app"></a>9.6 Sicherstellen, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste Version von .NET Framework verwenden.

- Stellen Sie sicher, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste .NET Framework-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.

## <a name="97-ensure-that-php-version-is-the-latest-if-used-to-run-the-web-app"></a>9.7 Sicherstellen, dass die neueste PHP-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste Version von PHP verwenden.

- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste PHP-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.

## <a name="98-ensure-that-python-version-is-the-latest-if-used-to-run-the-web-app"></a>9.8 Sicherstellen, dass die neueste Python-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste Version von Python verwenden.

- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste Python-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.

## <a name="99-ensure-that-java-version-is-the-latest-if-used-to-run-the-web-app"></a>9.9 Sicherstellen, dass die neueste Java-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste Version von Java verwenden.

- Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der API-App verwendet wird.
- Sicherstellen, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Funktions-App verwendet wird
- Stellen Sie sicher, dass die neueste Java-Version angegeben ist, wenn sie als Teil der Web-App verwendet wird.

## <a name="910-ensure-that-http-version-is-the-latest-if-used-to-run-the-web-app"></a>9.10 Sicherstellen, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird

Mit dieser Blaupause werden [Azure Policy](../../../policy/overview.md)-Definitionen zugewiesen, mit denen Sie sicherstellen können, dass Web-Apps die neueste Version von HTTP verwenden.

- Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der API-App verwendet wird.
- Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der Funktions-App verwendet wird.
- Stellen Sie sicher, dass die neueste HTTP-Version angegeben ist, wenn sie zum Ausführen der Web-App verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit der Steuerungszuordnung der Blaupause „CIS Microsoft Azure Foundations Benchmark“ vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr über diese Blaupause zu erfahren, oder weisen Sie die Initiative über das Azure-Portal unter „Azure Policy“ zu:

> [!div class="nextstepaction"]
> [Blaupause „CIS Microsoft Azure Foundations Benchmark“ – Übersicht](./index.md)
> [Blaupause „CIS Microsoft Azure Foundations Benchmark“ – Bereitstellungsschritte](./deploy.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).