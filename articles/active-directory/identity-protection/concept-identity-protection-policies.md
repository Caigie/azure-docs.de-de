---
title: Azure Active Directory Identity Protection-Richtlinien
description: Identifizieren der drei Richtlinien, die mit Identity Protection aktiviert sind
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7587ed6d414a69cff67aca9446aebf6260c99fcd
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83736499"
---
# <a name="identity-protection-policies"></a>Richtlinien für den Identitätsschutz

Azure Active Directory Identity Protection enthält drei Standardrichtlinien, die Administratoren aktivieren können. Diese Richtlinien bieten eingeschränkte Anpassungen, sind jedoch für die meisten Organisationen anwendbar. Alle Richtlinien ermöglichen das Ausschließen von Benutzern, wie z.B. [Benutzer- oder Administratorkonten für den Notfallzugriff](../users-groups-roles/directory-emergency-access.md).

![Richtlinien für den Identitätsschutz](./media/concept-identity-protection-policies/identity-protection-policies.png)

## <a name="azure-mfa-registration-policy"></a>Azure MFA-Registrierungsrichtlinie

Identity Protection kann Organisationen beim Rollout von Azure Multi-Factor Authentication (MFA) mithilfe einer Richtlinie für den bedingten Zugriff unterstützen, die die Registrierung bei der Anmeldung erfordert. Wenn Sie diese Richtlinie aktivieren, können Sie sicherstellen, dass neue Benutzer in Ihrer Organisation sich am ersten Tag für MFA registrieren. Die mehrstufige Authentifizierung (MFA) ist eine der Methoden zur Eigenwartung für Risikoereignisse in Identity Protection. Eigenwartung ermöglicht Ihren Benutzern, selbst Maßnahmen zu ergreifen, um so das Helpdesk-Telefonaufkommen zu verringern.

Weitere Informationen zu Azure Multi-Factor Authentication finden Sie im Artikel [Funktionsweise: Azure Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md).

## <a name="sign-in-risk-policy"></a>Richtlinie zum Anmelderisiko

Identity Protection analysiert Signale von jeder Anmeldung (sowohl in Echtzeit als auch offline) und berechnet eine Risikobewertung basierend auf der Wahrscheinlichkeit, dass die Anmeldung nicht vom Benutzer ausgeführt wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können den Zugriff blockieren, den Zugriff erlauben oder mehrstufige Authentifizierung anfordern, um den Zugriff zu erlauben.

Wenn Risiken erkannt werden, können die Benutzer die mehrstufige Authentifizierung zur Eigenwartung durchführen und das riskante Anmeldeereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für Azure Multi-Factor Authentication registriert haben.

### <a name="custom-conditional-access-policy"></a>Benutzerdefinierte Richtlinie für bedingten Zugriff

Administratoren können auch eine benutzerdefinierte Richtlinie für den bedingten Zugriff erstellen und das Anmelderisiko als Zuweisungsbedingung einschließen. Weitere Informationen zu Risiko als Bedingung in einer Zugriffsrichtlinie für bedingten Zugriff finden Sie im Artikel [Bedingter Zugriff: Bedingungen](../conditional-access/concept-conditional-access-conditions.md#sign-in-risk)

![Benutzerdefinierte Anmelderisikorichtlinie für den bedingten Zugriff](./media/concept-identity-protection-policies/identity-protection-custom-sign-in-policy.png)

## <a name="user-risk-policy"></a>Richtlinie zum Benutzerrisiko

Identity Protection kann das erwartete „normale“ Verhalten eines Benutzers berechnen, und Entscheidungen bezüglich des Risikos darauf basieren. Das Benutzerrisiko ist eine Berechnung der Wahrscheinlichkeit, dass eine Identität kompromittiert wurde. Administratoren können basierend auf diesem Risikobewertungssignal Entscheidungen treffen, um organisatorische Anforderungen zu erzwingen. Administratoren können mit [Azure AD Self-Service-Kennwortzurücksetzung](../authentication/howto-sspr-deployment.md) den Zugriff blockieren, den Zugriff erlauben oder den Benutzer auffordern, das Kennwort zu ändern, um Zugriff zu erhalten.

Wenn Risiken erkannt werden, können die Benutzer die Self-Service-Kennwortzurücksetzung zur Eigenwartung durchführen und das Benutzerrisikoereignis schließen, um unnötigen Aufwand für Administratoren zu vermeiden.

> [!NOTE] 
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für die Self-Service-Kennwortzurücksetzung registriert haben.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Self-Service-Kennwortzurücksetzung in Azure AD](../authentication/howto-sspr-deployment.md)

- [Aktivieren von Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)

- [Aktivieren der Registrierungsrichtlinie für die mehrstufige Authentifizierung](howto-identity-protection-configure-mfa-policy.md)

- [Aktivieren von Anmelde- und Benutzerrisikorichtlinien](howto-identity-protection-configure-risk-policies.md)
