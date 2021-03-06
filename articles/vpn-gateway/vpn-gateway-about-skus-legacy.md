---
title: "Références SKU héritées de passerelle de réseau virtuel Azure | Microsoft Docs"
description: "Anciennes références SKU de passerelle de réseau virtuel."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 01ef2f0309848be5a891749178196e60fff2930e
ms.contentlocale: fr-fr
ms.lasthandoff: 08/03/2017

---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Utilisation des références SKU de passerelle de réseau virtuel (anciennes références SKU)

Cet article contient des informations sur les anciennes références SKU de passerelle de réseau virtuel. Les anciennes références SKU continuent de fonctionner dans les deux modèles de déploiement pour les passerelles VPN qui ont déjà été créés. Les passerelles VPN classiques continuent à utiliser d’anciennes références SKU, aussi bien pour les passerelles existantes que pour les nouvelles passerelles. Lorsque vous créez de nouvelles passerelles VPN de gestionnaire de ressources, utilisez les références SKU des nouvelles passerelles. Pour plus d’informations sur les nouvelles références SKU, reportez-vous à la rubrique [À propos de la passerelle VPN](vpn-gateway-about-vpngateways.md).

## <a name="gateway-skus"></a>SKU de passerelle

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="estimated-aggregate-throughput-by-sku"></a>Débit agrégé estimé par SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="supported-configurations-by-sku-and-vpn-type"></a>Configurations prises en charge par référence SKU et type de VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Redimensionner une passerelle (modifier la référence SKU d’une passerelle)

Vous pouvez redimensionner la référence SKU d’une passerelle au sein de la même famille de références SKU. Par exemple, si vous avez une référence SKU standard, vous pouvez redimensionner selon une référence SKU HighPerformance. Vous ne pouvez pas redimensionner vos passerelles VPN entre d’anciennes références SKU est de nouvelles familles de références SKU. Par exemple, vous ne pouvez pas aller d’une référence SKU standard à une référence SKU VpnGw2. 

Pour redimensionner une référence SKU de passerelle pour le modèle de déploiement classique, utilisez la commande suivante :

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Pour redimensionner une référence SKU de passerelle pour le modèle de déploiement du Gestionnaire de ressources, utilisez la commande suivante :

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migration vers les nouvelles références SKU de passerelle

Si vous utilisez le modèle de déploiement du Gestionnaire de ressources, vous pouvez migrer vers les nouvelles références SKU de la passerelle. Si vous utilisez le modèle de déploiement classique, vous ne pouvez pas migrer vers les nouvelles références SKU et devez continuer à utiliser les anciennes références SKU.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les nouvelles références SKU de passerelle, voir [Références (SKU) de passerelle](vpn-gateway-about-vpngateways.md#gwsku).

Pour plus d’informations sur les paramètres de configuration, consultez [À propos des paramètres de configuration de la passerelle VPN](vpn-gateway-about-vpn-gateway-settings.md).
