---
title: "Comment utiliser le contrôle d’accès en fonction du rôle dans Gestion des API Azure | Microsoft Docs"
description: "Découvrez comment utiliser les rôles intégrés et créer des rôles personnalisés dans Gestion des API Azure"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: apimpm
ms.translationtype: Human Translation
ms.sourcegitcommit: e22bd56e0d111add6ab4c08b6cc6e51c364c7f22
ms.openlocfilehash: fa757a591d788f52d759bc24accedd3c55149ae7
ms.contentlocale: fr-fr
ms.lasthandoff: 05/19/2017


---

# <a name="how-to-use-role-based-access-control-in-azure-api-management"></a>Comment utiliser le contrôle d’accès en fonction du rôle dans Gestion des API Azure
La Gestion des API Azure utilise le contrôle d’accès en fonction du rôle (RBAC) Azure pour gérer de façon précise l’accès aux services et aux entités Gestion des API (tels que les API et les stratégies). Cet article fournit une vue d’ensemble des rôles intégrés et personnalisés dans Gestion des API. Pour plus d’informations sur la gestion de l’accès dans le portail Azure, consultez [Prise en main de la gestion des accès dans le portail Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

## <a name="built-in-roles"></a>Rôles intégrés
La Gestion des API fournit trois rôles intégrés, auxquels deux autres vont être prochainement ajoutés. Ces rôles peuvent être affectés à différentes étendues, notamment l’abonnement, le groupe de ressources et chacune des instances Gestion des API. Par exemple, si le rôle « Lecteur du service Gestion des API » est affecté à un utilisateur au niveau du groupe de ressources, l’utilisateur dispose d’un accès en lecture à toutes les instances Gestion des API du groupe de ressources. 

Le tableau ci-dessous fournit de brèves descriptions des rôles intégrés. Vous pouvez assigner ces rôles à l’aide du portail Azure ou d’autres outils, y compris Azure [PowerShell](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-powershell), [l’interface de ligne de commande](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-azure-cli) Azure et l’[API REST](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-manage-access-rest). Pour plus d’informations sur l’affectation de rôles intégrés, consultez [Utiliser les affectations de rôle pour gérer l’accès à vos ressources d’abonnement Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/).

| Rôle          | Accès en lecture<sup>[1]</sup> | Accès en écriture<sup>[2]</sup> | Création, suppression et mise à l’échelle d’un service, configuration d’un VPN et d’un domaine personnalisé | Accès au portail de publication hérité | Description
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| Collaborateur du service Gestion des API Azure | ✓ | ✓  | ✓  | ✓ | Super utilisateur. Dispose d’un accès CRUD complet aux services et entités Gestion des API (par exemple, les API, les stratégies). Dispose d’un accès au portail de publication hérité. |
| Lecteur du service Gestion des API Azure | ✓ | | || Dispose d’un accès en lecture seule aux services et entités Gestion des API. |
| Opérateur du service Gestion des API Azure | ✓ | | ✓ | | Peut gérer les services Gestion des API, mais pas les entités.|
| Éditeur du service Gestion des API Azure<sup>*</sup> | ✓ | ✓ | |  | Peut gérer les entités Gestion des API, mais pas les services.|
| Gestionnaire de contenu Gestion des API Azure<sup>*</sup> | ✓ | | | ✓ | Peut gérer le portail des développeurs. Accès en lecture seule aux services et aux entités.|

<sup>[1] Accès en lecture aux services et entités Gestion des API (par exemple, les API, les stratégies)</sup>

<sup>[2] Accès en écriture aux services et entités Gestion des API à l’exception des opérations suivantes : (1) Création, suppression et mise à l’échelle des instances (2) Configuration du VPN (3) Configuration du nom de domaine personnalisé</sup>

<sup>\* Le rôle Éditeur de service sera disponible une fois que vous aurez migré toutes les interfaces utilisateur d’administration, du portail de publication existant vers le portail Azure. Le rôle Gestionnaire de contenu sera disponible une fois que le portail de publication aura été refactorisé pour ne contenir que les fonctionnalités relatives à la gestion du portail des développeurs.</sup>  


## <a name="custom-roles"></a>Rôles personnalisés
Si aucun des rôles intégrés ne répond à vos besoins, vous pouvez créer des rôles personnalisés permettant une gestion plus précise de l’accès aux entités Gestion des API. Par exemple, vous pouvez créer un rôle personnalisé qui dispose d’un accès en lecture seule à un service Gestion des API, mais qui ne dispose d’un accès en écriture que pour une seule API. Pour plus d’informations sur les rôles personnalisés, consultez [Rôles personnalisés dans le contrôle d’accès en fonction du rôle (RBAC) Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles). 

Lorsque vous créez un rôle personnalisé, il est plus facile de commencer par l’un des rôles intégrés. Modifiez les attributs pour ajouter les propriétés Actions, NotActions ou AssignableScopes, puis enregistrez les modifications sous un nouveau rôle. L’exemple suivant commence par le rôle « Lecteur du service Gestion des API », puis crée un rôle personnalisé appelé « Éditeur d’API de calculateur ». Le rôle personnalisé ne peut être affecté qu’à une API spécifique, et ne peut donc accéder qu’à cette API. 

```
$role = Get-AzureRmRoleDefinition "API Management Service Reader Role"
$role.Id = $null
$role.Name = 'Calculator API Contributor'
$role.Description = 'Has read access to Contoso APIM instance and write access to the Calculator API.'
$role.Actions.Add('Microsoft.ApiManagement/service/apis/write')
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add('/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>')
New-AzureRmRoleDefinition -Role $role
New-AzureRmRoleAssignment -ObjectId <object ID of the user account> -RoleDefinitionName 'Calculator API Contributor' -Scope '/subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.ApiManagement/service/<service name>/apis/<api ID>'
```

## <a name="watch-a-video-overview"></a>Regarder une vidéo de présentation

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Role-Based-Access-Control-in-API-Management/player]
> 
> 

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur le contrôle d’accès en fonction du rôle dans Azure
  * [Prise en main de la gestion des accès dans le portail Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Utiliser les attributions de rôle pour gérer l’accès à vos ressources d’abonnement Azure](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/)
  * [Rôles personnalisés dans le contrôle d’accès en fonction du rôle (RBAC) Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)

