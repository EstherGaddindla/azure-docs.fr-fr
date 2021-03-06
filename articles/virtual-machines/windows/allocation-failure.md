---
title: "Résolution des problèmes d’allocation de machines virtuelles Windows | Microsoft Docs"
description: "Résoudre les problèmes d’allocation pendant la création, le redémarrage ou le redimensionnement de machines virtuelles Windows dans Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: cjiang
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5313b454a6c6cdc7b9f212302351fafd69c21650
ms.contentlocale: fr-fr
ms.lasthandoff: 04/03/2017


---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a>Résoudre les problèmes d’allocation pendant la création, le redémarrage ou le redimensionnement de machines virtuelles Windows dans Azure
Quand vous créez une machine virtuelle, redémarrez des machines virtuelles ayant été arrêtées (désallouées) ou redimensionnez une machine virtuelle, Microsoft Azure alloue des ressources de calcul à votre abonnement. Vous pouvez parfois recevoir des erreurs lorsque vous effectuez ces opérations, avant même d’avoir atteint les limites de votre abonnement Azure. Cet article explique les causes de certains échecs d’allocation courants et propose des solutions possibles. Les informations qu’il contient peuvent également vous être utiles dans le cadre de la planification du déploiement de vos services. Vous pouvez également [résoudre les problèmes d’allocation pendant la création, le redémarrage ou le redimensionnement de machines virtuelles Linux dans Azure](../linux/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]


