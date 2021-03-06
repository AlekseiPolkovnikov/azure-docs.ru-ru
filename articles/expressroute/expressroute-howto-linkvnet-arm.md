---
title: "Связывание виртуальной сети с каналом ExpressRoute с помощью PowerShell | Документация Майкрософт"
description: "В этом документе содержатся общие сведения о связывании виртуальных сетей с каналами ExpressRoute с помощью модели развертывания Resource Manager и PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: ganesr
translationtype: Human Translation
ms.sourcegitcommit: 99d5facce236b82ea84c708edf5e934a0d69919c
ms.openlocfilehash: e362ef6d35316df0410ae84933df27639ab8c562


---
# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Связывание виртуальной сети с каналом ExpressRoute
> [!div class="op_single_selector"]
> * [Портал Azure — Resource Manager](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell — Resource Manager](expressroute-howto-linkvnet-arm.md)
> * [PowerShell — классическая модель](expressroute-howto-linkvnet-classic.md)
> 
> 

Эта статья поможет вам связать виртуальные сети с каналами Azure ExpressRoute с помощью модели развертывания Resource Manager и PowerShell. Виртуальные сети могут входить в одну и ту же подписку или в разные подписки.

**О моделях развертывания Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Предварительные требования для настройки
* Вам потребуется последняя версия модулей Azure PowerShell (минимум 1.0). Дополнительные сведения об установке командлетов PowerShell см. в статье [Как установить и настроить Azure PowerShell](../powershell-install-configure.md).
* Прежде чем приступить к настройке, необходимо изучить [предварительные требования](expressroute-prerequisites.md), [требования к маршрутизации](expressroute-routing.md) и [рабочие процессы](expressroute-workflows.md).
* Вам потребуется активный канал ExpressRoute. 
  * Следуйте инструкциям, чтобы [создать канал ExpressRoute](expressroute-howto-circuit-arm.md) и включить его на стороне поставщика услуг подключения. 
  * Убедитесь, что для вашего канала настроен частный пиринг Azure. Инструкции по маршрутизации см. в статье [Настройка маршрутизации](expressroute-howto-routing-arm.md). 
  * Для создания сквозного подключения обязательно настройте частный пиринг Azure, а также пиринг BGP между вашей сетью и сетью Майкрософт.
  * Вам необходимо создать и полностью подготовить виртуальную сеть и шлюз виртуальной сети. Следуйте инструкциям по созданию [VPN-шлюза](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), но обязательно используйте `-GatewayType ExpressRoute`.

К стандартному каналу ExpressRoute можно подключить не более 10 виртуальных сетей. Если используется стандартный канал ExpressRoute, все виртуальные сети должны находиться в одном геополитическом регионе. 

Если включить надстройку ExpressRoute Premium, вы сможете подключить к каналу ExpressRoute больше виртуальных сетей, включая сети из других геополитических регионов. Дополнительную информацию о надстройке Premium см. в разделе [Вопросы и ответы](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Подключение к каналу виртуальной сети в той же подписке
Вы можете связать шлюз виртуальной сети с каналом ExpressRoute, используя следующий командлет. Убедитесь в наличии шлюза виртуальной сети и его готовности к связыванию, прежде чем выполнять командлет.

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Подключение к каналу виртуальной сети в другой подписке
Канал ExpressRoute может совместно использоваться несколькими подписками. На рисунке ниже схематично показан способ совместного использования каналов ExpressRoute несколькими подписками.

Каждое маленькое облако внутри большого облака представляет подписки, принадлежащие различным подразделениям одной организации. Любое подразделение в организации может использовать свою собственную подписку для развертывания служб. Кроме того, оно может совместно использовать один выделенный канал ExpressRoute для подключения к корпоративной сети. Владельцем канала ExpressRoute может выступать одно подразделение (в данном примере — ИТ-подразделение). Другие подписки в организации могут использовать канал ExpressRoute.

> [!NOTE]
> Плата за подключение выделенного канала ExpressRoute и использование полосы пропускания будет взиматься с владельца выделенного канала. Полоса пропускания распределяется между всеми виртуальными сетями.
> 
> 

![Подключение между подписками](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Администрирование
*Владельцем канала* выступает уполномоченный опытный пользователь ресурса канала ExpressRoute. Владелец канала может создавать разрешения, которые могут быть активированы *пользователями канала*. *Пользователи канала* являются владельцами шлюзов виртуальных сетей (которые находятся в разных подписках с каналом ExpressRoute). *Пользователи канала* могут активировать разрешения (по одному разрешению для каждой виртуальной сети).

*Владелец канала* имеет право изменить или отменить разрешение в любое время. Отмена разрешения приводит к удалению всех связывающих подключений из подписки, доступ к которой был отменен.

### <a name="circuit-owner-operations"></a>Действия владельца канала

**Создание разрешения**

Владелец канала создает разрешение. Это приводит к созданию ключа разрешения, который может использоваться пользователем канала для подключения шлюзов виртуальной сети к каналу ExpressRoute. Разрешение действительно только для одного подключения.

В следующем фрагменте показано создание разрешения с помощью командлета.

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"


Ответ будет содержать ключ и состояние разрешения.

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**Просмотр разрешений**

Владелец канала может просмотреть все разрешения, выданные для определенного канала, выполнив следующий командлет.

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit


**Добавление разрешений**

Владелец канала может добавлять разрешения с помощью следующего командлета.

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit


**Удаление разрешений**

Владелец канала может отзывать (удалять) разрешения, выданные пользователю, с помощью следующего командлета.

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

**Действия пользователя канала**

Пользователь канала должен получить от владельца канала идентификатор однорангового узла и ключ разрешения. Ключ разрешения представляет собой идентификатор GUID.

Код узла можно проверить с помощью следующей команды.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

**Активация разрешений на подключение**

Пользователь канала может активировать разрешение на связь, выполнив следующий командлет.

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

**Освобождение разрешений на подключение**

Разрешение можно освободить, удалив подключение, связывающее канал ExpressRoute и виртуальную сеть.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об ExpressRoute см. в статье [Вопросы и ответы по ExpressRoute](expressroute-faqs.md).




<!--HONumber=Nov16_HO3-->


