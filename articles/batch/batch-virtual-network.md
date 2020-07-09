---
title: Bereitstellen eines Pools in einem virtuellen Netzwerk
description: Hier erfahren Sie, wie Sie einen Batch-Pool in einem virtuellen Netzwerk erstellen, damit Computeknoten sicher mit anderen VMs (z. B. Dateiserver) im Netzwerk kommunizieren können.
ms.topic: how-to
ms.date: 04/03/2020
ms.custom: seodec18
ms.openlocfilehash: 559cf3bc145deeed78b91def9d36211f885005d6
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83797520"
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Erstellen eines Azure Batch-Pools in einem virtuellen Netzwerk

Beim Erstellen eines Azure Batch-Pools können Sie einen Pool in einem von Ihnen angegebenen Subnetz eines [virtuellen Azure-Netzwerks](../virtual-network/virtual-networks-overview.md) (VNET) bereitstellen. In diesem Artikel wird erklärt, wie Sie einen Batch-Pool in einem VNET einrichten.

## <a name="why-use-a-vnet"></a>Gründe für die Verwendung eines VNETs

Ein Azure Batch-Pool verfügt über Einstellung, durch die Computeknoten miteinander kommunizieren können, z.B. zum Ausführen von Tasks mit mehreren Instanzen. Für diese Einstellungen ist kein separates VNET erforderlich. Allerdings können die Knoten standardmäßig nicht mit VMs kommunizieren, die Teil des Batch-Pools sind, wie etwa Lizenzserver und Dateiserver. Um die sichere Kommunikation zwischen Computeknoten und VMs oder lokalen Netzwerken zu ermöglichen, können Sie den Pool in einem Subnetz eines Azure VNETs bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* **Authentifizierung**. Zur Verwendung eines Azure VNETs muss die Batch-Client-API die Azure Active Directory-Authentifizierung verwenden. Die Azure Batch-Unterstützung für Azure AD ist unter [Authentifizieren von Lösungen des Azure Batch-Diensts mit Active Directory](batch-aad-auth.md) dokumentiert.

* **Ein Azure VNET** Im folgenden Abschnitt finden Sie Informationen zu den Anforderungen und der Konfiguration des VNET. Um ein VNET mit mindestens einem Subnetz vorzubereiten, können Sie das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle (CLI) oder andere Methoden verwenden.
  * Um ein auf Azure Resource Manager basiertes VNET zu erstellen, lesen Sie die Informationen unter [Create, change, or delete a virtual network](../virtual-network/manage-virtual-network.md#create-a-virtual-network) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks). Ein auf Resource Manager basierendes VNET wird für neue Bereitstellungen empfohlen und wird nur für Pools in der Konfiguration des virtuellen Computers unterstützt.
  * Um ein klassischen VNET zu erstellen, können Sie sich unter [Erstellen eines (klassischen) virtuellen Netzwerks mit mehreren Subnetzen](../virtual-network/create-virtual-network-classic.md) informieren. Ein klassisches VNET wird nur für Pools in der Cloud Services-Konfiguration unterstützt.

## <a name="vnet-requirements"></a>VNET-Anforderungen

[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]

## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Erstellen eines Pools mit einem VNET im Portal

Wenn Sie Ihr VNET erstellt und einem Subnetz zugewiesen haben, können Sie einen Batch-Pool mit diesem VNET erstellen. Führen Sie die folgenden Schritte aus, um einen Pool im Azure-Portal zu erstellen: 

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto. Das Konto muss sich im gleichen Abonnement und der gleichen Region wie die Ressourcengruppe befinden, die das VNET enthält, das Sie verwenden möchten.
2. Wählen Sie links im Fenster **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie im Fenster **Pools** den Befehl **Hinzufügen** aus.
4. Wählen Sie im Fenster **Pool hinzufügen** die Option, die Sie verwenden möchten, in der Dropdownliste **Imagetyp** aus.
5. Wählen Sie **Verleger/Angebot/SKU** für Ihr benutzerdefiniertes Image aus.
6. Geben Sie die übrigen erforderlichen Einstellungen an, einschließlich **Knotengröße**, **Ziel für dedizierte Knoten** und **Knoten mit niedriger Priorität**, sowie alle gewünschten optionalen Einstellungen.
7. Wählen Sie unter **Virtuelles Netzwerk** das virtuelle Netzwerk und Subnetz aus, die Sie verwenden möchten.

   ![„Pool hinzufügen“ mit virtuellem Netzwerk](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Benutzerdefinierte Routen für erzwungenes Tunneln

Ihr Unternehmen erfordert möglicherweise zu Überprüfungs- und Protokollierungszwecken das (erzwungene) Weiterleiten von Internetdatenverkehr vom Subnetz zurück zu ihrem lokalen Standort. Möglicherweise ist die Tunnelerzwingung für das Subnetz in Ihrem VNET aktiviert.

Um sicherzustellen, dass die Computeknoten Ihres Azure Batch-Pools in einem VNET funktionieren, in dem die Tunnelerzwingung aktiviert ist, müssen Sie folgende [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md) (User-Defined Routes, UDR)für dieses Subnetz hinzufügen:

* Der Batch-Dienst muss für die zeitliche Planung von Tasks mit den Computeknoten des Pools kommunizieren. Um diese Kommunikation zu ermöglichen, fügen Sie eine UDR für jede IP-Adresse hinzu, die vom Batch-Dienst in der Region Ihres Batch-Kontos verwendet werden. Informationen dazu, wie Sie die Liste mit IP-Adressen des Batch-Diensts abrufen, finden Sie unter [Lokale Diensttags](../virtual-network/service-tags-overview.md).

* Stellen Sie sicher, dass ausgehender Datenverkehr an Azure Storage (also URLs im Format `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` und `<account>.blob.core.windows.net`) nicht über Ihr lokales Netzwerkgerät blockiert wird.

Wenn Sie eine UDR hinzufügen, definieren Sie die Route für jedes zugehörige Batch-IP-Adresspräfix, und legen Sie den **Typ des nächsten Hops** auf **Internet** fest. Sehen Sie sich folgendes Beispiel an:

![Benutzerdefinierte Route](./media/batch-virtual-network/user-defined-route.png)

> [!WARNING]
> IP-Adressen des Batch-Diensts können sich im Laufe der Zeit ändern. Um einen Ausfall aufgrund einer Änderung der IP-Adresse zu verhindern, empfiehlt es sich, einen regelmäßigen Prozess zum automatischen Aktualisieren der IP-Adressen des Batch-Diensts einzurichten und diese in der Routingtabelle auf dem neuesten Stand zu halten.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
- Informationen zum Erstellen einer benutzerdefinierten Route finden Sie unter [Erstellen einer benutzerdefinierten Route – Azure-Portal](../virtual-network/tutorial-create-route-table-portal.md).
