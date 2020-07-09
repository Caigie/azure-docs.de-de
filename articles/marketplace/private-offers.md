---
title: Private Angebote im kommerziellen Microsoft-Marketplace
description: Hier finden Sie Informationen zu privaten Angeboten im kommerziellen Microsoft-Marketplace für Herausgeber von Apps und Diensten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: vikrambmsft
ms.author: vikramb
ms.date: 04/19/2020
ms.openlocfilehash: b558cbc53ba09a86f7277df0ee5d45a9bdac56e3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121009"
---
# <a name="private-offers-in-the-microsoft-commercial-marketplace"></a>Private Angebote im kommerziellen Microsoft-Marketplace

Private Angebote im [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/) ermöglichen Herausgebern das Erstellen von Plänen, die nur für bestimmte Zielkunden sichtbar sind. In diesem Artikel werden die Optionen und Vorteile von privaten Angeboten erörtert.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Neue Chancen für Unternehmen durch private Angebote

Unternehmenskunden verwenden immer häufiger Onlinemarktplätze, um Cloudlösungen zu suchen, zu testen und zu erwerben. Mit privaten Angeboten können Herausgeber jetzt solche Marktplätze nutzen, um ausgewählten Kunden angepasste Lösungen mit Funktionen bereitzustellen, die in Unternehmen benötigt werden:

- *Ausgehandelte Preise*: Hiermit können Herausgeber Rabatte und Sonderpreise aus öffentlich verfügbaren Angeboten angeben.
- *Private Bestimmungen* ermöglichen es Herausgebern, Geschäftsbedingungen genau an die Anforderungen eines bestimmten Kunden anzupassen.
- Mit *speziellen Konfigurationen* können Herausgeber ihr Angebot an Virtual Machines-Diensten, Azure-Anwendungen und SaaS-Apps auf die Bedürfnisse einzelner Kunden ausrichten. Diese Option ermöglicht es Herausgebern auch, Vorschauzugriff auf neue Produktfunktionen bereitzustellen, bevor diese einem breiteren Publikum zugänglich gemacht werden.

Mit privaten Angeboten können Herausgeber von der globalen Verfügbarkeit und Größe eines öffentlichen Marktplatzes profitieren und erhalten gleichzeitig die Flexibilität und Steuerungsmöglichkeiten, um kundenspezifische Geschäftsabschlüsse auszuhandeln und die entsprechenden Konfigurationen zu liefern. All diese Features führen dazu, dass Unternehmen in zunehmendem Maß Cloudmarktplätze nutzen. Unternehmen können jetzt auf genau die Art und Weise einkaufen und verkaufen, die sie gewohnt sind und die sie selbst erwarten.

Private Angebote sind für Virtual Machines-Dienste, Azure-Anwendungen (implementiert als Lösungsvorlagen oder verwaltete Anwendungen) und SaaS-Apps verfügbar. 

<!--- Like public offers, private offers can be created and managed via the [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).  Customers can be granted or revoked access to private offers in minutes.
--->

## <a name="creating-private-offers-using-skus-and-plans"></a>Erstellen von privaten Angeboten mithilfe von SKUs und Plänen

Bei *neuen oder vorhandenen Angeboten mit öffentlichen SKUs oder Plänen* können Herausgeber ganz einfach neue private Variationen erstellen, indem sie neue SKUs oder Pläne erstellen und diese als „privat“ kennzeichnen.  

<!--- [Private SKUs](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) ---> 

Private SKUs und Pläne sind Komponenten eines Angebots und können nur von den ausgewählten Zielkunden angezeigt und erworben werden. Für private SKUs und Pläne können die Basisimages und/oder Angebotsmetadaten wiederverwendet werden, die bereits für öffentliche SKUs oder Pläne veröffentlicht wurden. Mit dieser Option können Herausgeber mehrere private Variationen eines öffentlichen Angebots erstellen, ohne mehrere Versionen der gleichen Basisimages und Angebotsmetadaten veröffentlichen zu müssen. Nur bei Angeboten für Virtual Machines und Azure-Anwendungen gilt: Wenn eine private SKU das gleiche Basisimage nutzt wie eine öffentliche SKU, werden alle Änderungen am Basisimage des Angebots an alle öffentlichen und privaten SKUs weitergegeben, die dieses Basisimage nutzen.

Bei *neuen Angeboten, die nur private SKUs oder Pläne umfassen*, können Herausgeber diese als beliebiges anderes Angebot erstellen und dann die SKUs oder Pläne als „privat“ kennzeichnen. Angebote, die nur private SKUs oder Pläne umfassen, sind von Kunden, denen diese Angebote nicht zugeordnet sind, weder über den kommerziellen Microsoft Marketplace noch über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) auffindbar oder erreichbar.

## <a name="targeting-customers-with-private-offers"></a>Private Angebote für bestimmte Zielkunden

Herausgeber können sowohl neue als auch vorhandene private Angebote mithilfe von Abonnementbezeichnern auf bestimmte Zielkunden ausrichten. Herausgeber mit Angeboten für Virtual Machines oder Azure-Anwendungen können die Verfügbarkeit einer privaten SKU auf eine individuelle Azure-Abonnement-ID beschränken oder eine CSV-Datei mit bis zu 20.000 Azure-Abonnement-IDs hochladen. Bei Verwendung eines privaten SaaS-App-Angebots können Herausgeber eine Mandanten-ID zuordnen, um die Verfügbarkeit eines privaten Plans zu beschränken. Dabei können sie IDs manuell zuordnen oder eine CSV-Datei mit mehreren IDs hochladen.

Sobald ein Angebot zertifiziert und veröffentlicht wurde, können Kunden mithilfe des Features zum Synchronisieren privater Abonnements innerhalb weniger Minuten in der SKU oder im Plan aktualisiert oder aus der SKU oder dem Plan entfernt werden. Auf diese Weise können Herausgeber schnell und einfach die Liste der Kunden aktualisieren, denen die private SKU oder der private Plan angezeigt wird, ohne das Angebot erneut zertifizieren oder veröffentlichen zu müssen.

## <a name="deploying-private-offers"></a>Bereitstellen von privaten Angeboten

Private Angebote sind nur über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) auffindbar und werden in [Microsoft AppSource](https://appsource.microsoft.com/) und dem [Azure Marketplace](https://azuremarketplace.microsoft.com) nicht angezeigt. Weitere Informationen zum Veröffentlichen in verschiedenen kommerziellen Marketplace-Storefronts finden Sie unter [Bestimmung Ihrer Veröffentlichungsoption](./determine-your-listing-type.md).

Nach der Anmeldung beim Azure-Portal können Kunden das Marketplace-Navigationselement auswählen, um auf ihre privaten Angebote zuzugreifen. Private Angebote werden auch in Suchergebnissen angezeigt und können wie alle anderen Angebote über die Befehlszeile oder mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden.

![[Private Angebote]](./media/marketplace-publishers-guide/private-offer.png)

Private Angebote werden auch in Suchergebnissen angezeigt. Achten Sie auf das Badge „Privat“.

>[!Note]
>Private Angebote werden bei Abonnements, die über einen Handelspartner des Cloud Solution Provider-Programms (CSP) eingerichtet wurden, nicht unterstützt.

<!---
## Next steps

To start using private offers, follow the steps in the [Private SKUs and Plans](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) guide.

--->