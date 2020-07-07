---
title: Verweigern des Zugriffs auf öffentliche Netzwerke – Azure-Portal – Azure Database for MySQL
description: Erfahren Sie mehr über das Konfigurieren des Verweigerns des Zugriffs auf öffentliche Netzwerke für Ihre Azure Database for MySQL-Instanz im Azure-Portal.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: b5f93f3a3583900810ca75f925c6a88df9102652
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "79372680"
---
# <a name="deny-public-network-access-in-azure-database-for-mysql-using-azure-portal"></a>Verweigern des öffentlichen Netzwerkzugriff in Azure Database for MySQL im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie eine Azure Database for MySQL-Instanz so konfigurieren können, dass alle öffentlichen Konfigurationen verweigert und nur Verbindungen über private Endpunkte zugelassen werden, um die Netzwerksicherheit weiter zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

* Eine [Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)-Instanz

## <a name="set-deny-public-network-access"></a>Festlegen von „Zugriff auf öffentliches Netzwerk verweigern“

Gehen Sie wie folgt vor, um „Zugriff auf öffentliches Netzwerk verweigern“für den MySQL-Server zu aktivieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren vorhandenen Azure Database for MySQL-Server aus.

1. Klicken Sie auf der Seite des MySQL-Servers unter **Einstellungen** auf **Verbindungssicherheit**, um die Seite zur Konfiguration der Verbindungssicherheit zu öffnen.

1. Wählen Sie in **Zugriff auf öffentliches Netzwerk verweigern** die Option **Ja** aus, um den öffentlichen Zugriff auf Ihren MySQL-Server zu verweigern.

    ![Azure Database for MySQL: Netzwerkzugriff verweigern](./media/howto-deny-public-network-access/setting-deny-public-network-access.PNG)

1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

1. Eine Benachrichtigung bestätigt, dass die Verbindungssicherheitseinstellung erfolgreich aktiviert wurde.

    ![Azure Database for MySQL: Netzwerkzugriff – Erfolg](./media/howto-deny-public-network-access/setting-deny-public-network-access-success.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).