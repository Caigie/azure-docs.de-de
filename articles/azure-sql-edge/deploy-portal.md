---
title: Bereitstellen von Azure SQL Edge (Vorschau) mithilfe des Azure-Portals
description: Hier erfahren Sie, wie Sie Azure SQL Edge (Vorschau) über das Azure-Portal bereitstellen.
keywords: SQL Edge bereitstellen
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 43359b66ba747dba7b3294d022a2c1aa2a3e624c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233242"
---
# <a name="deploy-azure-sql-edge-preview"></a>Bereitstellen von Azure SQL Edge (Vorschau) 

Azure SQL Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und Azure IoT Edge-Bereitstellungen. Azure SQL Database Edge bietet die Möglichkeit zur Erstellung eines Hochleistungs-Datenspeichers und einer Verarbeitungsschicht für IoT-Anwendungen und -Lösungen. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Erstellen eines Azure SQL Edge-Moduls durch Azure IoT Edge über das Azure-Portal beginnen.

## <a name="before-you-begin"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.
* Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
* Erstellen Sie einen [Azure IoT Hub](../iot-hub/iot-hub-create-through-portal.md).
* Registrieren Sie ein [IoT Edge-Gerät über das Azure-Portal](../iot-edge/how-to-register-device-portal.md).
* Bereiten Sie das IoT Edge-Gerät zum [Bereitstellen des IoT Edge-Moduls über das Azure-Portal](../iot-edge/how-to-deploy-modules-portal.md) vor.

> [!NOTE]
> Informationen zum Bereitstellen einer Azure Linux-VM als IoT Edge-Gerät finden Sie in dieser [Schnellstartanleitung](../iot-edge/quickstart-linux.md).

## <a name="deploy-sql-edge-module-from-azure-marketplace"></a>Bereitstellen eines SQL Edge-Moduls über Azure Marketplace

Azure Marketplace ist ein Onlinemarktplatz für Anwendungen und Dienste, in dem sie eine breite Palette an Unternehmensanwendungen und -lösungen durchsuchen können, die für die Ausführung unter Azure, einschließlich [IoT Edge-Modulen](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules), zertifiziert und optimiert sind. Azure SQL Edge kann über den Marketplace auf einem Edgegerät bereitgestellt werden.

1. Suchen Sie das Azure SQL Edge-Modul im Azure Marketplace.<br><br>

   ![SQL Edge im Marketplace](media/deploy-portal/find-offer-marketplace.png)

2. Wählen Sie den Softwareplan aus, der Ihren Anforderungen am besten entspricht, und klicken Sie auf **Erstellen**. <br><br>

   ![Auswählen des richtigen Softwareplans](media/deploy-portal/pick-correct-plan.png)

3. Geben Sie auf der Seite „Zielgeräte für IoT Edge-Modul“ die folgenden Details an, und klicken Sie auf **Erstellen**.

   |**Feld**  |**Beschreibung**  |
   |---------|---------|
   |Subscription  |  Das Azure-Abonnement, unter dem der IoT Hub erstellt wurde |
   |IoT Hub   |  Der Name des IoT Hubs, auf dem das IoT Edge-Gerät registriert wird. Wählen Sie dann die Option „Für Gerät bereitstellen“ aus.|
   |Name des IoT Edge-Geräts  |  Der Name des IoT Edge-Geräts, auf dem SQL Edge bereitgestellt würde |

4. Navigieren Sie auf der Seite **Module festlegen** zum Abschnitt für Bereitstellungsmodule, und klicken Sie für das SQL Edge-Modul auf **Konfigurieren**. 

5. Geben Sie im Bereich **Benutzerdefinierte IoT Edge-Module** die gewünschten Werte für die Umgebungsvariablen an, und/oder passen Sie die Erstellungsoptionen und gewünschten Eigenschaften für das Modul an. Eine vollständige Liste der unterstützten Umgebungsvariablen finden Sie unter [SQL Server Container-Umgebungsvariablen](/sql/linux/sql-server-linux-configure-environment-variables/).

   |**Parameter**  |**Beschreibung**|
   |---------|---------|
   | Name | Der Name für das Modul. |
   |SA_PASSWORD  | Geben Sie ein sicheres Kennwort für das SQL Edge-Administratorkonto an. |
   |MSSQL_LCID   | Legt die Sprach-ID fest, die für SQL Server verwendet werden soll. Beispielsweise steht „1036“ für „Französisch“. |
   |MSSQL_COLLATION | Legt die Standardsortierung für SQL Server fest. Diese Einstellung setzt die Standardzuordnung der Sprach-ID (LCID) für die Sortierung außer Kraft. |

   > [!NOTE]
   > Ändern oder aktualisieren Sie nicht den **Image-URI** oder die **ACCEPT_EULA**-Einstellungen für das Modul.

6. Aktualisieren Sie im Bereich **Benutzerdefinierte IoT Edge-Module** den gewünschten „container create options“-Wert für den **Hostport**. Wenn Sie mehrere SQL DB Edge-Module bereitstellen müssen, aktualisieren Sie auch die „mounts“-Option, um ein neues Paar aus Quelle und Ziel für das persistente Volume zu erstellen. Weitere Informationen zu Bereitstellungen und Volumes finden Sie auf der Seite zum [Verwenden von Volumes](https://docs.docker.com/storage/volumes/) in der Docker-Dokumentation. 

   ```json
       {
         "HostConfig": {
           "Binds": [
             "sqlvolume:/sqlvolume"
           ],
           "PortBindings": {
             "1433/tcp": [
               {
                 "HostPort": "1433"
               }
             ]
           },
           "Mounts": [
             {
               "Type": "volume",
               "Source": "sqlvolume",
               "Target": "/var/opt/mssql"
             }
           ]
         },
         "Env": [
           "MSSQL_AGENT_ENABLED=TRUE",
           "MSSQL_PID=Developer"
         ]
       }
   ```

7. Aktualisieren Sie im Bereich **Benutzerdefinierte IoT Edge-Module** den Wert für *Gewünschte Eigenschaften für Modulzwilling festlegen*, um den Speicherort des SQL-Pakets und die Informationen zum Stream Analytics-Auftrag einzubeziehen. Diese beiden Felder sind optional und sollten verwendet werden, wenn Sie das SQL Edge-Modul mit einer Datenbank und einem Streamingauftrag bereitstellen möchten.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

8. Legen Sie im Bereich **Benutzerdefinierte IoT Edge-Module** den Wert für *Neustartrichtlinie* auf „Immer“ und für *Gewünschter Status* auf „Wird ausgeführt“ fest.
9. Klicken Sie im Bereich **Benutzerdefinierte IoT Edge-Module** auf **Speichern**.
10. Klicken Sie auf der Seite **Module festlegen** auf **Weiter**.
11. Geben Sie auf der Seite **Module festlegen** im Abschnitt **Route angeben (optional)** die Routen für eine Kommunikation des Typs „Modul zu Modul“ oder „Modul zu IoT Edge Hub“ an. Lesen Sie dazu [Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](../iot-edge/module-composition.md).
12. Klicken Sie auf **Weiter**.
13. Klicken Sie auf **Submit**(Senden).

In diesem Schnellstart haben Sie ein SQL Edge-Modul auf einem IoT Edge-Gerät bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Machine Learning und künstliche Intelligenz mit ONNX in SQL Edge](onnx-overview.md)
- [Entwickeln einer End-to-End-IoT-Lösung mit SQL Edge unter Verwendung von IoT Edge](tutorial-deploy-azure-resources.md)