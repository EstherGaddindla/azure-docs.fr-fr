---
title: "Supprimer le lien d’un compte Azure Automation dans Log Analytics | Microsoft Docs"
description: "Cet article décrit comment supprimer le lien votre compte Azure Automation dans un espace de travail OMS."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 02/07/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 7cd65cd34846122ff14f6d5df61e4f61a7c1ac4f
ms.openlocfilehash: 56b09c2cfc14813b5efcb364c580787fec1bf639

---

# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Comment supprimer le lien de votre compte Automation dans un espace de travail Log Analytics

Azure Automation s’intègre dans Log Analytics non seulement pour assurer une surveillance proactive de vos travaux de Runbook sur l’ensemble de vos comptes Automation, mais il est indispensable lorsque vous importez les solutions suivantes qui dépendent de Log Analytics :

* [Gestion des mises à jour](../operations-management-suite/oms-solution-update-management.md)
* [Suivi des modifications](../log-analytics/log-analytics-change-tracking.md)
* [Démarrer/arrêter des machines virtuelles pendant les heures creuses](automation-solution-vm-management.md)
 
Si vous ne souhaitez plus intégrer votre compte Automation dans Log Analytics, vous pouvez supprimer son lien directement dans le portail Azure.  Avant toute chose, vous devez supprimer les solutions mentionnées précédemment. Sinon, la procédure ne pourra pas aboutir.  Consultez la rubrique relative à la solution que vous avez importée pour comprendre sa procédure de suppression.  

Après avoir supprimé ces solutions, vous pouvez effectuer les étapes suivantes pour supprimer le lien de votre compte Automation.

## <a name="unlink-workspace"></a>Supprimer le lien de votre espace de travail

1. Dans le portail Azure, ouvrez votre compte Automation puis, dans le panneau de ce dernier, sélectionnez **Unlink workspace (Supprimer le lien de l’espace de travail)**.<br><br> ![Option Unlink workspace (Supprimer le lien de l’espace de travail)](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Dans le panneau Unlink workspace (Supprimer le lien de l’espace de travail), cliquez sur **Unlink workspace (Supprimer le lien de l’espace de travail)**.<br><br> ![Panneau Unlink workspace (Supprimer le lien de l’espace de travail)](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Vous recevez une invite de confirmation de la suppression.<br><br>
3. Pour suivre la progression de la suppression du lien de votre espace de travail Log Analytics dans Azure Automation, sélectionnez **Notifications** dans le menu.

Si vous avez utilisé la solution de gestion de la mise à jour, vous pouvez (si vous le souhaitez) supprimer les éléments suivants qui ne sont plus nécessaires après la suppression de la solution.

* Planifications de mise à jour.  Chacune aura un nom correspondant aux déploiements de mise à jour que vous avez créés.

* Groupes de travail hybrides créés pour la solution.  Chacun a un nom similaire à machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8.

Si vous avez utilisé la solution Démarrer/arrêter des machines virtuelles pendant les heures creuses, vous pouvez (si vous le souhaitez) supprimer les éléments suivants qui ne sont plus nécessaires après la suppression de la solution.

* Start and stop VM runbook schedules (Démarrer et arrêter les planifications de Runbook de machine virtuelle) 
* Start and stop VM runbooks (Démarrer et arrêter les Runbooks de machine virtuelle)
* Variables   

## <a name="next-steps"></a>Étapes suivantes

Pour reconfigurer votre compte Automation et l’intégrer dans OMS Log Analytics, consultez [Transférer l’état d’un travail et des flux de travail d’Automation vers Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md). 


<!--HONumber=Feb17_HO2-->


