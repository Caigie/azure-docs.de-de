---
title: Erstellen einer Azure-Supportanfrage | Microsoft Docs
description: Kunden, die Unterstützung benötigen, können das Azure-Portal verwenden, um Self-Service-Lösungen zu finden und Supportanfragen zu erstellen und zu verwalten.
services: Azure Supportability
author: ganganarayanan
manager: scotthit
ms.assetid: fd6841ea-c1d5-4bb7-86bd-0c708d193b89
ms.service: azure-supportability
ms.topic: article
ms.date: 03/31/2020
ms.author: kfollis
ms.openlocfilehash: 0bd1191c0b92203b100b1713971119ec828352ea
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835545"
---
# <a name="how-to-create-an-azure-support-request"></a>Gewusst wie: Erstellen einer Azure-Supportanfrage

## <a name="overview"></a>Übersicht

Azure ermöglicht das Erstellen und Verwalten von Supportanfragen (auch Supporttickets genannt). In diesem Artikel werden die Erstellung und Verwaltung von Anfragen über das [Azure-Portal](https://portal.azure.com) behandelt. Anfragen können aber auch programmgesteuert mithilfe der [Azure-Supportticket-REST-API](/rest/api/support) erstellt und verwaltet werden.

> [!NOTE]
> Die Azure-Portal-URL ist für die Azure-Cloud spezifisch, in der Ihre Organisation bereitgestellt ist.
>
>* Azure-Portal zur kommerziellen Verwendung: [https://portal.azure.com](https://portal.azure.com)
>* Azure-Portal für Deutschland: [https://portal.microsoftazure.de](https://portal.microsoftazure.de).
>* Azure-Portal für die US-Regierung: [https://portal.azure.us](https://portal.azure.us).
>
>

Basierend auf Kundenfeedback haben wir die Benutzeroberfläche für Supportanfragen aktualisiert, um drei Hauptziele zu erreichen:

* **Optimierung**: Einfacheres Auffinden von Support und Problembehandlung und vereinfachtes Absenden einer Supportanfrage.
* **Integration**: Bei der Behandlung eines Problems mit einer Azure-Ressource können Sie eine Supportanfrage ganz einfach öffnen, ohne den Kontext zu wechseln.
* **Effizient**: Sammeln Sie die wesentlichen Informationen, die Ihr Supportmitarbeiter benötigt, um das Problem effizient zu lösen.

## <a name="getting-started"></a>Erste Schritte

Zu **Hilfe und Support** gelangen Sie im Azure-Portal. Die Option steht über das Menü im Azure-Portal, die globale Kopfzeile oder das Ressourcenmenü für einen Dienst zur Verfügung. Bevor Sie eine Supportanfrage einreichen können, müssen Sie über die entsprechenden Berechtigungen verfügen.

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Um eine Supportanfrage erstellen zu können, müssen Sie ein [Besitzer](../../role-based-access-control/built-in-roles.md#owner) oder [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) sein, oder Ihnen muss die Rolle [Mitwirkender für Supportanfragen](../../role-based-access-control/built-in-roles.md#support-request-contributor) auf der Abonnementebene zugewiesen sein. Zum Erstellen einer Supportanfrage ohne Abonnement, z. B. in einem Azure Active Directory-Szenario (AAD), müssen Sie ein [Administrator](../../active-directory/users-groups-roles/directory-assign-admin-roles.md) sein.

### <a name="go-to-help--support-from-the-global-header"></a>Über die globale Kopfzeile zu „Hilfe + Support“ wechseln

So starten Sie von einer beliebigen Stelle im Azure-Portal aus eine Supportanfrage

1. Wählen Sie das **?** in der globalen Kopfzeile aus. Wählen Sie dann **Hilfe + Support** aus.

   ![Hilfe und Support](./media/how-to-create-azure-support-request/helpandsupportnewlower.png)

2. Wählen Sie **Neue Supportanfrage** aus. Befolgen Sie die Anweisungen, um uns Informationen zu Ihrem Problem zu geben. Wir schlagen einige mögliche Lösungen vor, sammeln Details zum Problem und helfen Ihnen, die Supportanfrage zu übermitteln und zu verfolgen.

   ![Neue Supportanfrage](./media/how-to-create-azure-support-request/newsupportrequest2lower.png)

### <a name="go-to-help--support-from-a-resource-menu"></a>Aus einem Ressourcenmenü zu „Hilfe + Support“ wechseln

So starten Sie eine Supportanfrage in dem Kontext der Ressource, mit der Sie zurzeit arbeiten

1. Wählen Sie im Ressourcenmenü im Abschnitt **Support + Problembehandlung** die Option **Neue Supportanfrage** aus.

   ![Im Kontext](./media/how-to-create-azure-support-request/incontext2lower.png)

2. Befolgen Sie die Anweisungen, um uns Informationen zu Ihrem Problem zu geben. Wenn Sie den Supportanfrageprozess von der Ressource aus starten, sind einige Optionen für Sie vorab ausgewählt.

## <a name="create-a-support-request"></a>Erstellen einer Supportanfrage

Wir führen Sie durch einige Schritte, um Informationen zu Ihrem Problem zu sammeln und Ihnen bei dessen Behebung zu helfen. Jeder dieser Schritte wird in den folgenden Abschnitten beschrieben.

### <a name="basics"></a>Grundlagen

Im ersten Schritt des Prozesses für Supportanfragen werden grundlegende Informationen zu Ihrem Problem und Supportplan gesammelt.

Verwenden Sie auf der Registerkarte **Grundlagen** von **Neue Supportanfrage** die Selektoren, um uns erste Informationen über das Problem mitzuteilen. Zuerst identifizieren Sie einige allgemeine Kategorien für den Problemtyp und wählen das zugehörige Abonnement aus. Wählen Sie den Dienst aus, z. B. **Virtueller Computer mit Windows**. Wählen Sie die Ressource aus, z. B. den Namen Ihres virtuellen Computers. Beschreiben Sie das Problem kurz mit eigenen Worten, und fahren Sie dann mit **Problemtyp auswählen** fort, um genauere Informationen anzugeben.

![Blatt "Grundlagen"](./media/how-to-create-azure-support-request/basics2lower.png)

> [!NOTE]
> In Azure wird unbegrenzter Support für die Abonnementverwaltung bereitgestellt, einschließlich Abrechnung, Kontingentanpassungen und Kontenübertragungen. Für technischen Support benötigen Sie einen Supportplan. [Erfahren Sie mehr über Supportpläne](https://azure.microsoft.com/support/plans).
>
>

### <a name="solutions"></a>Lösungen

Nach dem Sammeln der grundlegenden Informationen zeigen wir Ihnen als Nächstes Lösungen, die Sie selbst ausprobieren können. In einigen Fällen können wir sogar eventuell eine schnelle Diagnose ausführen. Lösungen werden von Azure-Experten geschrieben und unterstützen Sie beim Beheben der am häufigsten auftretenden Probleme.

### <a name="details"></a>Details

Als Nächstes sammeln wir zusätzliche Details über das Problem. Wenn Sie in diesem Schritt ausführliche und detaillierte Informationen bereitstellen, können wir Ihre Supportanfrage besser an den richtigen Mitarbeiter weiterleiten.

Teilen Sie uns wenn möglich mit, wann das Problem begonnen hat und durch welche Schritte es hervorgerufen wird. Sie können eine Datei hochladen, z. B. eine Protokolldatei oder eine Ausgabe der Diagnose.

Nachdem wir alle Informationen zum Problem erhalten haben, können Sie auswählen, wie Sie Support erhalten möchten. Wählen Sie im Abschnitt **Supportmethode** der Registerkarte **Details** den Schweregrad der Auswirkungen aus. Geben Sie Ihre bevorzugte Kontaktmethode an, einen guten Zeitpunkt für die Kontaktaufnahme mit Ihnen sowie Ihre Supportsprache.

Füllen Sie als Nächstes den Abschnitt **Kontaktinformationen** aus, damit wir wissen, wie wir Sie kontaktieren können.

### <a name="review--create"></a>Bewerten + erstellen

Füllen Sie alle erforderlichen Informationen auf jeder Registerkarte aus, und wählen Sie dann **Überprüfen + erstellen** aus. Überprüfen Sie die Details, die Sie an den Support senden werden. Wechseln Sie zu jeder Registerkarte zurück, um ggf. Änderungen vorzunehmen. Wenn Sie damit zufrieden sind und die Supportanfrage vollständig ist, wählen Sie **Erstellen** aus.

Ein Supportmitarbeiter setzt sich mit Ihnen mithilfe der von Ihnen angeführten Methode in Verbindung. Informationen zur anfänglichen Reaktionszeit finden Sie unter [Supportumfang und Reaktionszeiten](https://azure.microsoft.com/support/plans/response/).

## <a name="all-support-requests"></a>Alle Supportanfragen

Die Details und den Status von Supportanfragen können Sie anzeigen, indem Sie zu **Hilfe + Support** >  **Alle Supportanfragen** wechseln.

![Alle Supportanfragen](./media/how-to-create-azure-support-request/allrequestslower.png)

Auf dieser Seite können Sie Supportanfragen nach **Abonnement**, **Erstellungsdatum** (UTC) und **Status** filtern. Darüber hinaus können Sie Supportanfragen auf dieser Seite sortieren und suchen.

Wählen Sie eine Supportanfrage aus, um Details anzuzeigen, einschließlich Schweregrad und des geschätzten Zeitraums, den der Supporttechniker für eine Antwort benötigen wird.

Wenn Sie den Schweregrad der Anfrage ändern möchten, können Sie **Beeinträchtigung des Geschäftsbetriebs** auswählen. Wählen Sie in einer Liste den Schweregrad aus, der zugewiesen werden soll.

> [!NOTE]
> Der maximale Schweregrad richtet sich nach Ihrem Supportplan. [Erfahren Sie mehr über Supportpläne](https://azure.microsoft.com/support/plans).
>
>
Weitere Informationen zu den Optionen für die Selbsthilfeunterstützung in Azure finden Sie in diesem Video:

> [!VIDEO https://www.youtube.com/embed/gNhzR5FE9DY]

## <a name="next-steps"></a>Nächste Schritte

* [Senden Ihres Feedbacks und Ihrer Anregungen an uns](https://feedback.azure.com/forums/266794-support-feedback)
* Kontaktaufnahme mit uns auf [Twitter](https://twitter.com/azuresupport)
* Hilfe von Kollegen über die [Microsoft Q&A-Frageseite](https://docs.microsoft.com/answers/products/azure)
* Weitere Informationen in den [Häufig gestellten Fragen zum Azure-Support](https://azure.microsoft.com/support/faq)
