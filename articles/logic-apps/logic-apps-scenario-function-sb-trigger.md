---
title: "Scénario d’application logique : créer un déclencheur à l’aide d’Azure Functions et d’Azure Service Bus | Microsoft Docs"
description: "Créer une fonction pour déclencher une application logique à l’aide d’Azure Functions et d’Azure Service Bus"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: 98c78d84f3a615fae7d6785994f0db20f7a53254
ms.openlocfilehash: 013e3d29694a8daf1481e513c9c4dfc6b5da3384
ms.contentlocale: fr-fr
ms.lasthandoff: 02/10/2017


---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Scénario d’application logique : créer un déclencheur à l’aide d’Azure Functions et d’Azure Service Bus

Vous pouvez utiliser Azure Functions afin de créer un déclencheur pour une application logique lorsque vous avez besoin de déployer un écouteur ou une tâche de longue durée. Par exemple, vous pouvez créer une fonction qui écoute une file d’attente, puis déclenche immédiatement une application logique en tant que déclencheur d’émission.

## <a name="build-the-logic-app"></a>Création de l'application logique
Dans cet exemple, une fonction est en cours d’exécution pour chaque application logique à déclencher. Créez d’abord une application logique comportant un déclencheur de demande HTTP. La fonction appelle ce point de terminaison chaque fois qu’un message de la file d’attente est reçu.  

1. Créez une application logique.
2. Sélectionnez le déclencheur **Manuel – Lors de la réception d’une demande HTTP**.
   Si vous le souhaitez, vous pouvez spécifier un schéma JSON à utiliser avec le message de la file d’attente à l’aide d’un outil tel que [jsonschema.net](http://jsonschema.net). Collez le schéma dans le déclencheur. Les schémas aident le concepteur à comprendre la forme des données et à répercuter plus facilement les propriétés dans le workflow.
2. Ajoutez les étapes supplémentaires à exécuter après réception d’un message de la file d’attente. Par exemple, envoyer un courrier électronique via Office 365.  
3. Enregistrez l’application logique pour générer l’URL de rappel du déclencheur sur cette application logique. L’URL s’affiche sur la carte de déclencheur.

![L’URL de rappel s’affiche sur la carte de déclencheur][1]

## <a name="build-the-function"></a>Création de la fonction
Vous devez ensuite créer une fonction qui agit comme déclencheur et qui écoute la file d’attente.

1. Dans le [portail Azure Functions](https://functions.azure.com/signin), sélectionnez **Nouvelle fonction** puis choisissez le modèle **ServiceBusQueueTrigger - C#**.
   
    ![portail Azure Functions][2]
2. Configurez la connexion à la file d’attente Service Bus qui utilise l’écouteur `OnMessageReceive()` du SDK Azure Service Bus.
3. Écrivez une fonction de base pour appeler le point de terminaison de l’application logique (créé précédemment) en utilisant le message de la file d’attente comme déclencheur. Voici un exemple complet d’une fonction. L’exemple utilise un type de contenu de message `application/json`, mais vous pouvez le modifier si nécessaire.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Pour tester, ajoutez un message de la file d’attente via un outil tel que l [’Explorateur Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer). Vous remarquerez que l’application logique se déclenche immédiatement dès que la fonction reçoit le message.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png

