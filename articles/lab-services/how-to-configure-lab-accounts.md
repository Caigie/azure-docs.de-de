---
title: Konfigurieren des automatischen Herunterfahrens von VMs in Azure Lab Services
description: In diesem Artikel wird beschrieben, wie Sie das automatische Herunterfahren von VMs im Lab-Konto konfigurieren.
ms.topic: article
ms.date: 08/17/2020
ms.openlocfilehash: 8647aed0e66993b8a7b8e5c0a42c8ceabbb1fb9e
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798447"
---
# <a name="configure-automatic-shutdown-of-vms-for-a-lab-account"></a>Konfigurieren des automatischen Herunterfahrens von virtuellen Computern für ein Labkonto

Sie können mehrere Features zur Kostenkontrolle beim automatischen Herunterfahren aktivieren, um proaktiv zusätzliche Kosten zu verhindern, wenn die virtuellen Computer nicht aktiv genutzt werden. Die Kombination der folgenden drei Features zum automatischen Herunterfahren und Trennen deckt den größten Teil der Fälle ab, in denen Benutzer die Ausführung der virtuellen Computer versehentlich nicht beenden:
 
- Automatisches Trennen der Benutzer von virtuellen Computern, die vom Betriebssystem als Computer im Leerlauf erkannt werden (nur Windows).
- Automatisches Herunterfahren virtueller Computer, wenn Benutzer die Verbindung trennen (Windows und Linux).
- Automatisches Herunterfahren von virtuellen Computern, die gestartet wurden, mit denen jedoch von den Benutzern keine Verbindung hergestellt wurde.

Weitere Informationen zu den Features zum automatischen Herunterfahren finden Sie im Abschnitt [Maximieren der Kostenkontrolle durch Einstellungen zum automatischen Herunterfahren](cost-management-guide.md#automatic-shutdown-settings-for-cost-control).

## <a name="enable-automatic-shutdown"></a>Aktivieren des automatischen Herunterfahrens

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur Seite **Labkonto**.
1. Wählen Sie im linken Menü **Labeinstellungen** aus.
1. Wählen Sie die Einstellungen für das automatische Herunterfahren aus, die für Ihr Szenario geeignet sind.  

    > [!div class="mx-imgBorder"]
    > ![Einstellung für automatisches Herunterfahren in einem Labkonto](./media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)
    
    Diese Einstellungen gelten für alle im Labkonto erstellten Labs. Ein Lab-Ersteller (Lehrer/Dozent) kann diese Einstellung auf Lab-Ebene außer Kraft setzen. Die Änderung an dieser Einstellung im Labkonto wirkt sich nur auf Labs aus, die nach der Änderung erstellt werden.

    Deaktivieren Sie zum Deaktivieren der Einstellungen das Kontrollkästchen auf dieser Seite. 

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie ein Labbesitzer diese Einstellung auf Labebene konfigurieren oder außer Kraft setzen kann, finden Sie unter [Konfigurieren des automatischen Herunterfahrens von virtuellen Computern für ein Lab](how-to-enable-shutdown-disconnect.md).
