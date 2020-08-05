---
title: Verwalten und Überwachen von MARS-Agent-Sicherungen
description: Erfahren Sie etwas über das Verwalten und Überwachen von MARS-Agent-Sicherungen (Microsoft Azure Recovery Services) mit dem Azure Backup-Dienst.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 096f47ba4a3d8d490c9cfaf43f4dafec4faf10f9
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87317347"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Verwalten von MARS-Agent-Sicherungen (Microsoft Azure Recovery Services) mit dem Azure Backup-Dienst

In diesem Artikel wird beschrieben, wie Sie Dateien und Ordner verwalten, die mit Microsoft Azure Recovery Services Agent gesichert wurden.

## <a name="modify-a-backup-policy"></a>Ändern einer Sicherungsrichtlinie

Wenn Sie die Sicherungsrichtlinie ändern, können Sie neue Elemente hinzufügen, vorhandene Elemente aus der Sicherung entfernen oder mithilfe von Ausschlusseinstellungen Dateien aus der Sicherung ausschließen.

- **Elemente hinzufügen**: Verwenden Sie diese Option nur, um neue zu sichernde Elemente hinzuzufügen. Um vorhandene Elemente zu entfernen, verwenden Sie die Option **Elemente entfernen** oder **Ausschlusseinstellungen**.  
- **Elemente entfernen**: Verwenden Sie diese Option, um Elemente aus der Sicherung auszuschließen.
  - Verwenden Sie **Ausschlusseinstellungen** statt **Elemente entfernen**, um alle Elemente in einem Volume zu entfernen.
  - Das Deaktivieren aller Auswahloptionen in einem Volume bewirkt, dass alte Sicherungen der Elemente gemäß den Aufbewahrungseinstellungen zum Zeitpunkt der letzten Sicherung aufbewahrt werden, ohne dass Änderungen vorgenommen werden können.
  - Wenn Sie diese Optionen erneut aktivieren, führt dies zu einer ersten vollständigen Sicherung, und neue Richtlinienänderungen werden nicht auf alte Sicherungen angewendet.
  - Durch das Aufheben der Auswahl des gesamten Volumes wird die letzte Sicherung aufbewahrt, ohne dass an der Aufbewahrungsrichtlinie Änderungen vorgenommen werden können.
- **Ausschlusseinstellungen**: Verwenden Sie diese Option, um bestimmte Elemente aus der Sicherung auszuschließen.

### <a name="add-new-items-to-existing-policy"></a>Hinzufügen neuer Elemente zu einer vorhandenen Richtlinie

1. Klicken Sie unter **Aktionen** auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-configure-vault/schedule-first-backup.png)

2. Wählen Sie auf der Registerkarte **Wählen Sie das Richtlinienelement aus** die Option **Ändern Sie einen Sicherungszeitplan für Ihre Dateien und Ordner** aus, und klicken Sie dann auf **Weiter**.

    ![Hinzufügen von Richtlinienelementen](./media/backup-azure-manage-mars/select-policy-items.png)

3. Wählen Sie auf der Registerkarte **Geplante Sicherung ändern oder beenden** die Option **Sicherungselemente oder -zeiten ändern** aus, und klicken Sie auf **Weiter**.

    ![Ändern oder Planen der Sicherung](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. Klicken Sie auf der Registerkarte **Elemente für die Sicherung auswählen** auf **Elemente hinzufügen**, um die Elemente hinzuzufügen, die Sie sichern möchten.

    ![Ändern oder Planen der Sicherung – Elemente hinzufügen](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. Wählen Sie im Fenster **Elemente auswählen** Dateien oder Ordner aus, die Sie hinzufügen möchten, und klicken Sie auf **OK**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-item.png)

6. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

### <a name="add-exclusion-rules-to-existing-policy"></a>Hinzufügen von Ausschlussregeln zu einer vorhandenen Richtlinie

Sie können Ausschlussregeln hinzufügen, um Dateien und Ordner zu überspringen, die nicht gesichert werden sollen. Sie können dies beim Definieren einer neuen Richtlinie oder Ändern einer vorhandenen Richtlinie durchführen.

1. Klicken Sie im Bereich „Aktionen“ auf **Sicherung planen**. Wechseln Sie zu **Elemente für die Sicherung auswählen**, und klicken Sie auf **Ausschlusseinstellungen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. Klicken Sie in **Ausschlusseinstellungen**auf **Ausschluss hinzufügen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/add-exclusion.png)

3. Durchsuchen Sie in **Auszuschließende Elemente auswählen** die Dateien und Ordnern, die Sie ausschließen möchten, wählen Sie Elemente aus, die Sie ausschließen möchten, und klicken Sie auf **OK**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-items-exclude.png)

4. Standardmäßig werden alle **Unterordner** in den ausgewählten Ordnern ausgeschlossen. Sie können dies ändern, indem Sie **Ja** oder **Nein** auswählen. Sie können die auszuschließenden Dateitypen wie unten dargestellt bearbeiten und angeben:

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/subfolders-type.png)

5. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

### <a name="remove-items-from-existing-policy"></a>Entfernen von Elementen aus vorhandener Richtlinie

1. Klicken Sie im Bereich „Aktionen“ auf **Sicherung planen**. Wechseln Sie zu **Elemente für die Sicherung auswählen**. Wählen Sie in der Liste die Dateien und Ordner aus, die Sie aus dem Sicherungszeitplan entfernen möchten, und klicken Sie auf **Elemente entfernen**.

    ![Auswählen der Elemente](./media/backup-azure-manage-mars/select-items-remove.png)

    > [!NOTE]
    > Gehen Sie mit Bedacht vor, wenn Sie ein Volume vollständig aus der Richtlinie entfernen.  Wenn Sie es erneut hinzufügen müssen, wird es als neues Volume behandelt. Die nächste geplante Sicherung wird als Erstsicherung (vollständige Sicherung) und nicht als inkrementellen Sicherung ausgeführt. Wenn Sie zu einem späteren Zeitpunkt Elemente temporär entfernen und hinzufügen müssen, empfiehlt es sich, **Ausschlusseinstellungen** anstelle von **Elemente entfernen** zu verwenden, damit statt einer vollständigen Sicherung eine inkrementelle Sicherung erfolgt.

2. Führen Sie die nachfolgenden Schritte aus, und klicken Sie zum Abschließen des Vorgangs auf **Fertig stellen**.

## <a name="stop-protecting-files-and-folder-backup"></a>Beenden des Schutzes von Dateien und Ordnern durch Sicherungen

Sie haben zwei Möglichkeiten, den Schutz von Dateien und Ordnern durch Sicherungen zu beenden:

- **Schutz beenden und Sicherungsdaten beibehalten**.
  - Mit dieser Option werden alle zukünftigen Sicherungsaufträge beendet.
  - Der Azure Backup-Dienst behält alle vorhandenen Wiederherstellungspunkte weiterhin bei.  
  - Sie können die gesicherten Daten für nicht abgelaufene Wiederherstellungspunkte wiederherstellen.
  - Mit der Option *Aktivieren Sie den Sicherungszeitplan erneut* können Sie bei Bedarf den Schutz fortsetzen. Anschließend werden die Daten basierend auf der neuen Aufbewahrungsrichtlinie aufbewahrt.
- **Schutz beenden und Sicherungsdaten löschen**.
  - Mit dieser Option werden alle zukünftigen Sicherungsaufträge zum Schutz Ihrer Daten beendet und alle Wiederherstellungspunkte gelöscht.
  - Sie erhalten eine Warn-E-Mail zur Löschung der Sicherungsdaten mit der Nachricht *Your data for this Backup item has been deleted. This data will be temporarily available for 14 days, after which it will be permanently deleted* (Die Daten für dieses Sicherungselement wurden gelöscht. Diese Daten sind 14 Tage lang verfügbar und werden anschließend endgültig gelöscht.) und der empfohlenen Aktion *Reprotect the Backup item within 14 days to recover your data.* (Schützen Sie das Sicherungselement innerhalb von 14 Tagen erneut, um die Daten wiederherzustellen.).
  - Um den Schutz fortzusetzen, stellen Sie ihn innerhalb von 14 Tagen nach dem Löschvorgang wieder her.

### <a name="stop-protection-and-retain-backup-data"></a>Schutz beenden und Sicherungsdaten beibehalten

1. Öffnen Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/mars-actions.png)
1. Wählen Sie auf der Seite **Wählen Sie das Richtlinienelement aus** die Option **Ändern Sie einen Sicherungszeitplan für Ihre Dateien und Ordner** aus, und klicken Sie auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. Wählen Sie auf der Seite **Geplante Sicherung ändern oder beenden** die Option **Stop using this backup schedule, but keep the stored backups until a schedule is activated again** (Diesen Sicherungszeitplan nicht länger verwenden, die gespeicherten Sicherungen jedoch beibehalten, bis erneut ein Sicherungszeitplan aktiviert wird) aus. Klicken Sie anschließend auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. Überprüfen Sie die Informationen in **Geplante Sicherung anhalten**, und klicken Sie auf **Fertig stellen**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. Überprüfen Sie in **Sicherungsstatus ändern**, ob der Status der geplanten Sicherungspausierung „Erfolgreich“ ist, und klicken Sie auf **Schließen**, um den Vorgang abzuschließen.

### <a name="stop-protection-and-delete-backup-data"></a>Schutz beenden und Sicherungsdaten löschen

1. Öffnen Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.
2. Wählen Sie auf der Seite **Geplante Sicherung ändern oder beenden** die Option **Sicherungszeitplan nicht länger verwenden und alle gespeicherten Sicherungen löschen** aus. Klicken Sie anschließend auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. Wählen Sie auf der Seite **Geplante Sicherung beenden** die Option **Fertig stellen** aus.

    ![Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. Sie werden aufgefordert, eine Sicherheits-PIN (persönliche Identifikationsnummer) einzugeben, die Sie manuell generieren müssen. Dazu melden Sie sich zunächst beim Azure-Portal an.
5. Navigieren Sie zu **Recovery Services-Tresor** > **Einstellungen** > **Eigenschaften**.
6. Wählen Sie unter **Sicherheits-PIN** die Option **Generieren** aus. Kopieren Sie diese PIN. Die PIN ist nur fünf Minuten lang gültig.
7. Fügen Sie die PIN in der Verwaltungskonsole ein, und klicken Sie anschließend auf **OK**.

    ![Generieren einer Sicherheits-PIN](./media/backup-azure-delete-vault/security-pin.png)

8. Auf der Seite **Sicherungsstatus ändern** wird die folgende Meldung angezeigt: *Gelöschte Sicherungsdaten werden 14 Tage lang aufbewahrt. Danach werden Sicherungsdaten dauerhaft gelöscht.*  

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nachdem Sie die lokalen Sicherungselemente gelöscht haben, führen Sie die nächsten Schritte im Portal aus.

## <a name="re-enable-protection"></a>Erneutes Aktivieren des Schutzes

Wenn Sie den Schutz beendet haben, die Daten jedoch aufbewahrt wurden, und Sie den Schutz fortsetzen möchten, können Sie durch Ändern der Sicherungsrichtlinie den Sicherungszeitplan erneut aktivieren.

1. Wählen Sie in **Aktionen** die Option **Sicherung planen** aus.
1. Wählen Sie **Aktivieren Sie den Sicherungszeitplan erneut. Sie können Sicherungselemente oder -zeiten auch ändern.** aus, und klicken Sie auf **Weiter**.<br>

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. Klicken Sie in **Elemente für Sicherung auswählen** auf **Weiter**.

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-manage-mars/re-enable-next.png)
1. Geben Sie in **Sicherungszeitplan angeben** den Sicherungszeitplan an, und klicken Sie auf **Weiter**.
1. Geben Sie in **Aufbewahrungsrichtlinie auswählen** die Aufbewahrungsdauer an, und klicken Sie auf **Weiter**.
1. Überprüfen Sie abschließend im Bildschirm **Bestätigung** die Richtliniendetails, und klicken Sie auf **Fertig stellen**.

## <a name="re-generate-passphrase"></a>Passphrase neu generieren

Eine Passphrase wird zum Verschlüsseln und Entschlüsseln von Daten während der Sicherung oder Wiederherstellung Ihres lokalen Computers mithilfe des MARS-Agents nach oder aus Azure verwendet. Wenn Sie die Passphrase verloren oder vergessen haben, können Sie sie neu generieren (vorausgesetzt, der Computer ist noch beim Recovery Services-Tresor registriert, und die Sicherung wurde konfiguriert). Führen Sie dazu die folgenden Schritte aus:

1. Wechseln Sie von der MARS-Agent-Konsole zu **Aktionsbereich** > **Eigenschaften ändern** >. Wechseln Sie dann zur **Registerkarte „Verschlüsselung“** .<br>
1. Aktivieren Sie das Kontrollkästchen **Passphrase ändern**.<br>
1. Geben Sie eine neue Passphrase ein, oder klicken Sie auf **Passphrase generieren**.
1. Klicken Sie auf **Durchsuchen**, um die neue Passphrase zu speichern.

    ![Generieren Sie die Passphrase.](./media/backup-azure-manage-mars/passphrase.png)

1. Klicken Sie auf **OK**, um die Änderungen zu übernehmen.  Wenn im Azure-Portal das [Sicherheitsfeature](./backup-azure-security-feature.md#enable-security-features) für den Recovery Services-Tresor aktiviert ist, werden Sie aufgefordert, die Sicherheits-PIN einzugeben. Führen Sie die Schritte in diesem [Artikel](./backup-azure-security-feature.md#authentication-to-perform-critical-operations) aus, um die PIN zu erhalten.<br>
1. Fügen Sie die Sicherheits-PIN aus dem Portal ein, und klicken Sie auf **OK**, um die Änderungen anzuwenden.<br>

    ![Generieren Sie die Passphrase.](./media/backup-azure-manage-mars/passphrase2.png)
1. Stellen Sie sicher, dass die Passphrase an einem alternativen Speicherort (nicht auf dem Quellcomputer) sicher gespeichert wird, vorzugsweise im Azure Key Vault. Halten Sie alle Passphrasen nach, wenn Sie über mehrere mit den MARS-Agents gesicherte Computer verfügen.

## <a name="managing-backup-data-for-unavailable-machines"></a>Verwalten von Sicherungsdaten für nicht verfügbare Computer

In diesem Abschnitt wird ein Szenario besprochen, in dem Ihr Quellcomputer, der mit MARS geschützt war, nicht mehr verfügbar ist, weil er gelöscht, beschädigt, mit Malware/Bransomware infiziert oder außer Betrieb genommen wurde.

Für diese Computer stellt der Azure Backup-Dienst sicher, dass der letzte Wiederherstellungspunkt gemäß den in der Sicherungsrichtlinie festgelegten Aufbewahrungsregeln nicht abläuft (d. h. nicht gelöscht wird). Daher ist eine sichere Wiederherstellung des Computers möglich.  Beachten Sie die folgenden Szenarien, die Sie für die gesicherten Daten ausführen können:

### <a name="scenario-1-the-source-machine-is-unavailable-and-you-no-longer-need-to-retain-backup-data"></a>Szenario 1: Der Quellcomputer ist nicht verfügbar, und Sie müssen die Sicherungsdaten nicht mehr beibehalten.

- Mithilfe der in [diesem Artikel](backup-azure-delete-vault.md#delete-protected-items-on-premises) aufgeführten Schritte können Sie die gesicherten Daten aus dem Azure-Portal löschen.

### <a name="scenario-2-the-source-machine-is-unavailable-and-you-need-to-retain-backup-data"></a>Szenario 2: Der Quellcomputer ist nicht verfügbar, und Sie müssen die Sicherungsdaten beibehalten.

Das Verwalten der Sicherungsrichtlinie für MARS erfolgt über die MARS-Konsole und nicht über das Portal. Wenn Sie die Aufbewahrungseinstellungen für vorhandene Wiederherstellungspunkte verlängern müssen, bevor diese ablaufen, müssen Sie den Computer wiederherstellen, die MARS-Konsole installieren und die Richtlinie verlängern.

- Um den Computer wiederherzustellen, führen Sie die folgenden Schritte aus:
  1. [Wiederherstellen des virtuellen Computers auf einem alternativen Zielcomputer](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)
  1. Erstellen Sie den Zielcomputer mit dem gleichen Hostnamen wie der Quellcomputer neu.
  1. Installieren Sie den Agent, und registrieren Sie sich erneut beim gleichen Tresor und mit der gleichen Passphrase.
  1. Starten Sie den MARS-Client, um die Aufbewahrungsdauer entsprechend Ihren Anforderungen zu verlängern.
- Der neu wiederhergestellte Computer, der mit MARS geschützt wird, führt weiter Sicherungen durch.  

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu unterstützten Szenarien und Einschränkungen finden Sie in der [Supportmatrix für den MARS-Agent](./backup-support-matrix-mars-agent.md).
- Erfahren Sie mehr über die [Aufbewahrung bei Richtlinien für bedarfsgesteuerte Sicherungen](backup-windows-with-mars-agent.md#set-up-on-demand-backup-policy-retention-behavior).
- Weitere häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen – Microsoft Azure Recovery Services-Agent (MARS)](backup-azure-file-folder-backup-faq.md).
