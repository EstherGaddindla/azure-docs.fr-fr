---
title: "Fonctions de modèle Azure Resource Manager - comparaison| Microsoft Docs"
description: "Décrit les fonctions à utiliser dans un modèle Azure Resource Manager pour comparer des valeurs."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.contentlocale: fr-fr
ms.lasthandoff: 08/02/2017

---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Fonctions de comparaison pour les modèles Azure Resource Manager

Resource Manager fournit plusieurs fonctions pour effectuer des comparaisons dans vos modèles.

* [equals](#equals)
* [greater](#greater)
* [greaterOrEquals](#greaterorequals)
* [less](#less)
* [lessOrEquals](#lessorequals)

## <a name="equals"></a>equals
`equals(arg1, arg2)`

Vérifie si deux valeurs sont égales.

### <a name="parameters"></a>Paramètres

| Paramètre | Requis | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Oui |entier, chaîne, tableau ou objet |Première valeur dont l’égalité est à vérifier. |
| arg2 |Oui |entier, chaîne, tableau ou objet |Deuxième valeur dont l’égalité est à vérifier. |

### <a name="return-value"></a>Valeur de retour

Retourne **True** si les valeurs sont égales ; sinon, renvoie **False**.

### <a name="remarks"></a>Remarques

La fonction equals est souvent utilisée avec l’élément `condition` pour tester si une ressource est déployée.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Exemple

L’exemple de modèle vérifie que les différents types de valeurs sont égaux. Toutes les valeurs par défaut retournent la valeur True.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

La sortie de l’exemple précédent avec les valeurs par défaut se présente comme suit :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | True |
| checkArrays | Bool | True |
| checkObjects | Bool | true |


L’exemple suivant utilise [non](resource-group-template-functions-logical.md#not) avec**égal à**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

La sortie de l’exemple précédent est :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkNotEquals | Bool | true |


## <a name="greater"></a>greater
`greater(arg1, arg2)`

Vérifie si la première valeur est supérieure à la deuxième valeur.

### <a name="parameters"></a>Paramètres

| Paramètre | Requis | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Oui |entier ou chaîne |Première valeur pour la comparaison « supérieur à ». |
| arg2 |Oui |entier ou chaîne |Seconde valeur pour la comparaison « supérieur à ». |

### <a name="return-value"></a>Valeur de retour

Retourne **True** si la première valeur est supérieure à la seconde ; sinon, renvoie **False**.

### <a name="example"></a>Exemple

L’exemple de modèle vérifie si une valeur est supérieure à l’autre.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

La sortie de l’exemple précédent avec les valeurs par défaut se présente comme suit :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |


## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Vérifie si la première valeur est supérieure ou égale à la deuxième valeur.

### <a name="parameters"></a>Paramètres

| Paramètre | Requis | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Oui |entier ou chaîne |Première valeur pour la comparaison « supérieur ou égal à ». |
| arg2 |Oui |entier ou chaîne |Seconde valeur pour la comparaison « supérieur ou égal à ». |

### <a name="return-value"></a>Valeur de retour

Retourne **True** si la première valeur est supérieure ou égale à la seconde ; sinon, renvoie **False**.

### <a name="example"></a>Exemple

L’exemple de modèle vérifie si une valeur est supérieure ou égale à l’autre.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

La sortie de l’exemple précédent avec les valeurs par défaut se présente comme suit :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |



## <a name="less"></a>less
`less(arg1, arg2)`

Vérifie si la première valeur est inférieure à la deuxième valeur.

### <a name="parameters"></a>Paramètres

| Paramètre | Requis | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Oui |entier ou chaîne |Première valeur pour la comparaison « inférieur à ». |
| arg2 |Oui |entier ou chaîne |Deuxième valeur pour la comparaison « inférieur à ». |

### <a name="return-value"></a>Valeur de retour

Retourne **True** si la première valeur est inférieure à la seconde ; sinon, renvoie **False**.

### <a name="example"></a>Exemple

L’exemple de modèle vérifie si une valeur est inférieure à l’autre.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

La sortie de l’exemple précédent avec les valeurs par défaut se présente comme suit :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |


## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Vérifie si la première valeur est inférieure ou égale à la deuxième valeur.

### <a name="parameters"></a>Paramètres

| Paramètre | Requis | Type | Description |
|:--- |:--- |:--- |:--- |
| arg1 |Oui |entier ou chaîne |Première valeur pour la comparaison « inférieur à ». |
| arg2 |Oui |entier ou chaîne |Première valeur pour la comparaison « inférieur ou égal à ». |

### <a name="return-value"></a>Valeur de retour

Retourne **True** si la première valeur est inférieure ou égale à la seconde ; sinon, renvoie **False**.

### <a name="example"></a>Exemple

L’exemple de modèle vérifie si une valeur est inférieure ou égale à l’autre.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

La sortie de l’exemple précédent avec les valeurs par défaut se présente comme suit :

| Nom | Type | Valeur |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |



## <a name="next-steps"></a>Étapes suivantes
* Pour obtenir une description des sections d’un modèle Azure Resource Manager, consultez [Création de modèles Azure Resource Manager](resource-group-authoring-templates.md).
* Pour fusionner plusieurs modèles, consultez [Utilisation de modèles liés avec Azure Resource Manager](resource-group-linked-templates.md).
* Pour itérer un nombre de fois spécifié lors de la création d'un type de ressource, consultez [Création de plusieurs instances de ressources dans Azure Resource Manager](resource-group-create-multiple.md).
* Pour savoir comment déployer le modèle que vous avez créé, consultez [Déploiement d’une application avec un modèle Azure Resource Manager](resource-group-template-deploy.md).


