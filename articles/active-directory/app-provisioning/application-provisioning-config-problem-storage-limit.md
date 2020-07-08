---
title: Problem beim Speichern von Administratoranmeldeinformationen beim Konfigurieren einer Azure AD-Katalog-App
description: Problembehandlung für häufige Probleme beim Konfigurieren der Benutzerbereitstellung für eine bereits im Azure AD-Anwendungskatalog aufgeführte Anwendung
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 4f47954f3f4943846cab2dd9a38fd310abce3469
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84782244"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Problem beim Speichern von Administratoranmeldeinformationen während des Konfigurierens der Benutzerbereitstellung in einer Anwendung aus dem Azure Active Directory-Katalog 

Beim Konfigurieren der [automatischen Benutzerbereitstellung](user-provisioning.md) für eine Unternehmensanwendung über das Azure-Portal kann Folgendes passieren:

* Die für die Anwendung eingegebenen **Administratoranmeldeinformationen** sind gültig, und die Schaltfläche **Verbindung testen** funktioniert. Die Anmeldeinformationen können jedoch nicht gespeichert werden, und das Azure-Portal gibt eine generische Fehlermeldung zurück.

Wenn für die Anwendung auch das SAML-basierte einmalige Anmelden konfiguriert ist, ist der Fehler höchstwahrscheinlich darauf zurückzuführen, dass das Azure AD-interne, anwendungsspezifische Speicherlimit für Zertifikate und Anmeldeinformationen überschritten wurde.

Azure AD bietet derzeit eine maximale Speicherkapazität von 1.024 Byte für alle Zertifikate, geheimen Token, Anmeldeinformationen und die dazugehörigen Konfigurationsdaten für eine einzelne Instanz einer Anwendung (in Azure AD auch als Dienstprinzipaldatensatz bezeichnet).

Wenn das SAML-basierte einmalige Anmelden konfiguriert ist, wird das zum Signieren der SAML-Token verwendete Zertifikat hier gespeichert – und beansprucht häufig bereits mehr als die Hälfte des verfügbaren Speicherplatzes.

Die geheimen Token, URIs, Benachrichtigungs-E-Mail-Adressen, Benutzernamen und Kennwörter, die während der Einrichtung der Benutzerbereitstellung eingegeben werden, können jeweils zur Überschreitung des Speicherlimits führen.

## <a name="how-to-work-around-this-issue"></a>Problemumgehung 

Dieses Problem lässt sich auf zwei Arten umgehen:

1. **Verwenden Sie zwei Instanzen der Kataloganwendung: eine für einmaliges Anmelden und eine für die Benutzerbereitstellung.** Als Beispiel soll die Kataloganwendung [LinkedIn Elevate](../saas-apps/linkedinelevate-tutorial.md) dienen: Sie können LinkedIn Elevate über den Katalog hinzufügen und für einmaliges Anmelden konfigurieren. Für die Bereitstellung fügen Sie dann eine weitere LinkedIn Elevate-Instanz über den Azure AD-App-Katalog hinzu und nennen sie „LinkedIn Elevate (Bereitstellung)“. Konfigurieren Sie für diese zweite Instanz die [Bereitstellung](../saas-apps/linkedinelevate-provisioning-tutorial.md), aber nicht das einmalige Anmelden. Bei Verwendung dieser Problemumgehung müssen beiden Anwendungen die gleichen Benutzer und Gruppen [zugewiesen](../manage-apps/assign-user-or-group-access-portal.md) werden. 

2. **Verringern Sie die Menge der gespeicherten Konfigurationsdaten.** Alle Daten, die im Abschnitt [Administratoranmeldeinformationen](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) der Registerkarte „Bereitstellung“ eingegeben werden, werden am gleichen Ort gespeichert wie das SAML-Zertifikat. Kürzungen sind zwar möglicherweise nicht bei allen Daten möglich, einige optionale Konfigurationsfelder wie etwa die **E-Mail-Adresse für Benachrichtigungen** können jedoch entfernt werden.

## <a name="next-steps"></a>Nächste Schritte
[Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](user-provisioning.md)
