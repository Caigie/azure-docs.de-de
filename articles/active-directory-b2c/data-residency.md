---
title: Regionale Verfügbarkeit und Datenresidenz
titleSuffix: Azure AD B2C
description: Hier finden Sie Informationen zur regionalen Verfügbarkeit, zur Datenresidenz und zu Azure Active Directory B2C-Vorschaumandanten.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: references_regions
ms.openlocfilehash: 46d8fb33c59fc5f0b6d844831e5ee1c937654afb
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193797"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Regionale Verfügbarkeit und Datenresidenz

Regionale Verfügbarkeit und Datenresidenz sind zwei sehr unterschiedliche Konzepte, die in Azure AD B2C anders als in den restlichen Komponenten von Azure angewendet werden. In diesem Artikel werden die Unterschiede zwischen diesen beiden Konzepten erläutert und ihre Anwendung in Azure und Azure AD B2C miteinander verglichen.

Azure AD B2C ist **grundsätzlich weltweit verfügbar**, mit der Option für **Datenresidenz** in den **USA, in Europa und in der Region „Asien-Pazifik“** .

[Regionale Verfügbarkeit](#region-availability) gibt an, wo ein Dienst für die Verwendung verfügbar ist.

[Datenresidenz](#data-residency) gibt an, wo die Benutzerdaten gespeichert werden.

## <a name="region-availability"></a>Regionale Verfügbarkeit

Azure AD B2C ist über die öffentliche Azure-Cloud weltweit verfügbar.

Dies unterscheidet sich von dem Modell der meisten anderen Azure-Dienste, in dem *Verfügbarkeit* und *Datenresidenz* gekoppelt sind. Beispiele hierfür finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/) und der Seite [Azure Active Directory B2C – Preise](https://azure.microsoft.com/pricing/details/active-directory-b2c/) von Azure.

## <a name="data-residency"></a>Datenresidenz

Benutzerdaten von Azure AD B2C werden entweder in den USA, in Europa oder in der Region „Asien-Pazifik“ gespeichert.

Die Datenresidenz wird durch das Land oder die Region bestimmt, das bzw. die Sie beim [Erstellen eines Azure AD B2C-Mandanten](tutorial-create-tenant.md) auswählen:

![Screenshot eines Vorschaumandanten](./media/data-residency/data-residency-b2c-tenant.png)

Für die folgenden Länder/Regionen werden die Daten in den **USA** gespeichert:

> USA, Kanada, Costa Rica, Dominikanische Republik, El Salvador, Guatemala, Mexiko, Panama, Puerto Rico sowie Trinidad und Tobago

Für die folgenden Länder/Regionen werden die Daten in **Europa** gespeichert:

> Algerien, Ägypten, Aserbaidschan, Bahrain, Belgien, Bulgarien, Dänemark, Deutschland, Estland, Finnland, Frankreich, Griechenland, Irland, Island, Israel, Italien, Jordanien, Kasachstan, Katar, Kenia, Kroatien, Kuwait, Lettland, Libanon, Liechtenstein, Litauen, Luxemburg, Malta, Marokko, Nordmazedonien, Montenegro, Niederlande, Nigeria, Norwegen, Oman, Österreich, Pakistan, Polen, Portugal, Rumänien, Russland, Saudi-Arabien, Schweden, Schweiz, Serbien, Slowakei, Slowenien, Spanien, Südafrika, Tschechische Republik, Tunesien, Türkei, Ukraine, Ungarn, Vereinigte Arabische Emirate, Vereinigtes Königreich, Weißrussland und Zypern.

Für die folgenden Länder/Regionen werden die Daten in der Region **Asien-Pazifik** gespeichert:

> Afghanistan, Hongkong SAR, Indien, Indonesien, Japan, Korea, Malaysia, Philippinen, Singapur, Sri Lanka, Taiwan und Thailand.

Derzeit wird die Liste um die folgenden Länder/Regionen ergänzt. Sie können vorerst Azure AD B2C verwenden, indem Sie eines bzw. eine der oben genannten Länder/Regionen auswählen.

> Argentinien, Australien, Brasilien, Chile, Ecuador, Irak, Kolumbien, Neuseeland, Paraguay, Peru, Uruguay und Venezuela.

## <a name="preview-tenant"></a>Vorschaumandant

Wenn Sie während des Vorschauzeitraums von Azure AD B2C einen B2C-Mandanten erstellt haben, ist die Wahrscheinlichkeit hoch, dass der **Mandantentyp** auf **Vorschaumandant** festgelegt ist.

In diesem Fall können Sie Ihren Mandanten NUR für Entwicklungs- und Testzwecke verwenden. Verwenden Sie einen Vorschaumandanten NICHT für Produktionsanwendungen.

Es gibt **keinen Migrationspfad** von einem B2C-Vorschaumandanten zu einem B2C-Produktionsmandanten. Sie müssen einen neuen B2C-Mandanten für Ihre Produktionsanwendungen erstellen.

Beim Löschen eines B2C-Vorschaumandanten und dem Erstellen eines B2C-Produktionsmandanten mit demselben Domänennamen können bekannte Probleme auftreten. *Sie müssen einen B2C-Produktionsmandanten mit einem anderen Domänennamen erstellen*.

![Screenshot eines Vorschaumandanten](./media/data-residency/preview-b2c-tenant.png)