---
title: Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure NetApp Files Momentaufnahmen für ein Volume erstellen oder aus einer Momentaufnahme auf einem neuen Volume wiederherstellen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/03/2020
ms.author: b-juche
ms.openlocfilehash: ed13c61646bd2a6672b613964507d291a69a6821
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483600"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Verwalten von Momentaufnahmen mithilfe von Azure NetApp Files

Mithilfe von Azure NetApp Files können Sie eine Momentaufnahme bei Bedarf für ein Volume manuell erstellen oder aus einer Momentaufnahme auf einem neuen Volume wiederherstellen. Der Azure NetApp Files-Dienst erstellt Momentaufnahmen von Volumes nicht automatisch.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Erstellen einer Momentaufnahme bei Bedarf für ein Volume

Momentaufnahmen können nur bei Bedarf erstellt werden. Richtlinien für Momentaufnahmen werden derzeit nicht unterstützt.

1.  Klicken Sie auf dem Blatt „Volumes“ auf **Momentaufnahmen**.

    ![Zu Momentaufnahmen navigieren](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Klicken Sie auf **+ Momentaufnahme hinzufügen**, um feine Momentaufnahme bei Bedarf für ein Volume zu erstellen.

    ![Momentaufnahme hinzufügen](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Geben Sie im Fenster „Neue Momentaufnahme“ der neu erstellten Momentaufnahme einen Namen an.   

    ![Neue Momentaufnahme](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Klicken Sie auf **OK**. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Wiederherstellen einer Momentaufnahme auf einem neuen Volume

Derzeit können Sie eine Momentaufnahme nur auf einem neuen Volume wiederherstellen. 
1. Wechseln Sie vom Blatt „Volume“ zum Blatt **Momentaufnahmen verwalten**, um die Liste der Momentaufnahmen anzuzeigen. 
2. Wählen Sie die Momentaufnahme aus, die wiederhergestellt werden soll.  
3. Klicken Sie mit der rechten Maustaste auf den Namen der Momentaufnahme, und wählen Sie im Menü die Option **Auf neuem Volume wiederherstellen** aus.  

    ![Wiederherstellen von Momentaufnahme auf neuem Volume](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Geben Sie im Fenster „Neues Volume“ die folgenden Informationen für das neue Volume an:  
    * **Name**   
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.  
        
        Der Name muss innerhalb einer Ressourcengruppe eindeutig sein. Er muss mindestens drei Zeichen lang sein.  Er darf beliebige alphanumerische Zeichen enthalten.

    * **Dateipfad**     
        Geben Sie den Dateipfad zum Erstellen des Exportpfads für das neue Volume an. Der Exportpfad dient zum Einbinden und Zugreifen auf das Volume.   
        
        Das Einbindungsziel ist der Endpunkt der IP-Adresse des NFS-Diensts. Es wird automatisch generiert.   
        
        Der Dateipfadname darf nur Buchstaben, Zahlen und Bindestriche („-“) enthalten. Er muss 16 bis 40 Zeichen umfassen. 

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.

    *   **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNet) an, von dem aus Sie auf das Volume zugreifen möchten.  
        Das von Ihnen angegebene VNET muss über ein an Azure NetApp Files delegiertes Subnetz verfügen. Sie können nur aus demselben VNET aus auf Azure NetApp Files zugreifen oder per VNET-Peering von einem VNET aus, das sich in der gleichen Region befindet wie das Volume. Sie können über Express Route von Ihrem lokalen Netzwerk aus auf das Volume zugreifen. 

    * **Subnetz**  
        Geben Sie das Subnetz an, das Sie für das Volume verwenden möchten.  
        Das von Ihnen angegebene Subnetz muss an den Azure NetApp Files-Dienst delegiert werden. Sie können ein neues Subnetz erstellen, indem Sie **Neu erstellen** unter dem Feld „Subnetz“ auswählen.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. Klicken Sie auf **OK**.   
    Das neue Volume, auf dem die Momentaufnahme wiederhergestellt wurde, wird auf dem Blatt „Volumes“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
