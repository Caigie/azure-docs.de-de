---
title: 'Azure AD v2 – Windows-Desktop: Erste Schritte – Konfiguration'
description: Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: d82f9beecb1b558fca094c31f8c6718c990debd1
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162705"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App
In diesem Schritt müssen Sie die Anwendungs-ID Ihrem Projekt hinzufügen.

1.  Öffnen Sie `App.xaml.cs`, und ersetzen Sie die Zeile mit der `ClientId` durch:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Wie geht es weiter?

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
