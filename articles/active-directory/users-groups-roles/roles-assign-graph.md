---
title: Zuweisen von Azure AD-Administratorrollen mit Microsoft Graph-API | Microsoft-Dokumentation
description: Zuweisen und Entfernen von Azure AD-Administratorrollen mit der Graph-API in Azure Active Directory
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 04/29/2020
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44299a55424f9b0338ee49d2742aeedf16db22e8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84732088"
---
# <a name="assign-custom-admin-roles-using-the-microsoft-graph-api-in-azure-active-directory"></a>Zuweisen von benutzerdefinierten Administratorrollen mithilfe von Microsoft Graph-API in Azure Active Directory 

Mithilfe von Microsoft Graph-API können Sie automatisieren, wie Benutzerkonten Rollen zugewiesen werden sollen. In diesem Artikel werden POST-, GET- und DELETE-Vorgänge für Rollenzuweisungen (roleAssignments) behandelt.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Stellen Sie über ein globales Administratorkonto oder als Privileged Identity-Administrator eine Verbindung mit Ihrer Azure AD-Organisation her, um Rollen zuzuweisen oder zu entfernen.

## <a name="post-operations-on-roleassignment"></a>POST-Vorgänge für RoleAssignment

HTTP-Anforderung zum Erstellen einer Rollenzuweisung zwischen einem Benutzer und einer Rollendefinition

POST

``` HTTP
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
Content-type: application/json
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/"
}
```

Antwort

``` HTTP
HTTP/1.1 201 Created
```

HTTP-Anforderung zum Erstellen einer Rollenzuweisung, bei der der Prinzipal oder die Rollendefinition nicht vorhanden ist

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":" 2142743c-a5b3-4983-8486-4532ccba12869",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/"
}
```

Antwort

``` HTTP
HTTP/1.1 404 Not Found
```

HTTP-Anforderung zum Erstellen einer einzelnen Rollenzuweisung mit Ressourcenbereich für eine integrierte Rollendefinition.

> [!NOTE] 
> Integrierte Rollen weisen heute eine Beschränkung auf, mit der sie nur auf den organisationsweiten Bereich „/“ oder auf den Bereich „/AU/*“ begrenzt werden können. Die Beschränkung auf einen einzelnen Ressourcenbereich ist für integrierte Rollen nicht möglich, kann für benutzerdefinierte Rollen jedoch verwendet werden.

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"194ae4cb-b126-40b2-bd5b-6091b380977d",
    "resourceScopes":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
}
```

Antwort

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Provided authorization scope is not supported for built-in role definitions."},
            "values":
            [
                {
                    "item":"scope",
                    "value":"/ab2e1023-bddc-4038-9ac1-ad4843e7e539"
                }
            ]
        }
    }
}
```

## <a name="get-operations-on-roleassignment"></a>GET-Vorgänge für RoleAssignment

HTTP-Anforderung zum Abrufen einer Rollenzuweisung für einen bestimmten Prinzipal

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=principalId eq ‘<object-id-of-principal>’
```

Antwort

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":"/"
} ,
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":"/"
}
```

HTTP-Anforderung zum Abrufen einer Rollenzuweisung für eine bestimmte Rollendefinition

GET

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Antwort

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":"/"
}
```

HTTP-Anforderung zum Abrufen einer Rollenzuweisung anhand der ID

GET

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Antwort

``` HTTP
HTTP/1.1 200 OK
{ 
    "id":"mhxJMipY4UanIzy2yE-r7JIiSDKQoTVJrLE9etXyrY0-1",
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"10dae51f-b6af-4016-8d66-8c2a99b929b3",
    "resourceScopes":"/"
}
```

## <a name="delete-operations-on-roleassignment"></a>DELETE-Vorgänge für RoleAssignment

HTTP-Anforderung zum Löschen einer Rollenzuweisung zwischen einem Benutzer und einer Rollendefinition

Delete

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Antwort
``` HTTP
HTTP/1.1 204 No Content
```

HTTP-Anforderung zum Löschen einer Rollenzuweisung, die nicht mehr vorhanden ist

Delete

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Antwort

``` HTTP
HTTP/1.1 404 Not Found
```

HTTP-Anforderung zum Löschen einer Rollenzuweisung einer integrierten Rollendefinition

Delete

``` HTTP
GET https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments/lAPpYvVpN0KRkAEhdxReEJC2sEqbR_9Hr48lds9SGHI-1
```

Antwort

``` HTTP
HTTP/1.1 400 Bad Request
{
    "odata.error":
    {
        "code":"Request_BadRequest",
        "message":
        {
            "lang":"en",
            "value":"Cannot remove self from built-in role definitions."},
            "values":null
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollen und zur Zuweisung von Administratorrollen finden Sie unter [Zuweisen von Administratorrollen](directory-assign-admin-roles.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
