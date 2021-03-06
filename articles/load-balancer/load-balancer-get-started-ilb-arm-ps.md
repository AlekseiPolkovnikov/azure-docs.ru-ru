---
title: "Создание подсистемы балансировки нагрузки в Resource Manager с помощью PowerShell | Документация Майкрософт"
description: "Узнайте, как создать внутренний балансировщик нагрузки в диспетчере ресурсов с помощью PowerShell."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 07ea7f3529d5d2e6fde07805663da7c0f3e65d86

---

# <a name="create-an-internal-load-balancer-using-powershell"></a>Создание внутреннего балансировщика нагрузки с помощью PowerShell

> [!div class="op_single_selector"]
> * [портале Azure](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Интерфейс командной строки Azure](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Шаблон](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель Resource Manager и классическая модель](../azure-resource-manager/resource-manager-deployment-model.md).  В этой статье описывается использование модели развертывания c помощью Resource Manager. Для большинства новых развертываний мы рекомендуем использовать эту модель вместо [классической](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

Ниже описана процедура создания внутреннего балансировщика нагрузки при помощи Azure Resource Manager с PowerShell. Диспетчер ресурсов Azure позволяет по отдельности настраивать элементы для создания внутренней подсистемы балансировки нагрузки, после чего на их основе создается балансировщик нагрузки.

Чтобы развернуть балансировщик нагрузки, необходимо создать и настроить следующие объекты:

* Настройка внешнего IP-адреса — настройка частного IP-адреса для входящего сетевого трафика.
* Внутренний пул адресов — настройка сетевых интерфейсов, которые будут получать трафик с распределенной нагрузкой из внешнего пула IP-адресов.
* Правила балансировки нагрузки — конфигурация исходного и локального портов для подсистемы балансировки нагрузки.
* Пробы — настройка проб для проверки состояния работоспособности экземпляров виртуальной машины.
* Входящие правила преобразования сетевых адресов (NAT) — настройка правил порта для прямого доступа к одному из экземпляров виртуальной машины.

Дополнительные сведения о настройке компонентов балансировщика нагрузки с помощью Azure Resource Manager см. в статье [Поддержка диспетчера ресурсов Azure для подсистемы балансировки нагрузки](load-balancer-arm.md).

Далее описана настройка балансировщика нагрузки для двух виртуальных машин.

## <a name="setup-powershell-to-use-resource-manager"></a>Настройка PowerShell для использования диспетчера ресурсов

Обязательно установите последнюю рабочую версию модуля Azure для PowerShell и правильно настройте PowerShell для доступа к подписке Azure.

### <a name="step-1"></a>Шаг 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Шаг 2

Проверка подписок для учетной записи

```powershell
Get-AzureRmSubscription
```

Вам будет предложено пройти проверку подлинности с вашими учетными данными.

### <a name="step-3"></a>Шаг 3.

Выберите, какие подписки Azure будут использоваться.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Создание группы ресурсов для подсистемы балансировки нагрузки

Создайте группу ресурсов (пропустите этот шаг, если вы используете существующую группу).

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Диспетчер ресурсов Azure требует, чтобы все группы ресурсов указывали расположение. Оно используется по умолчанию для ресурсов в этой группе. Убедитесь, что во всех командах создания подсистемы балансировки нагрузки используется одна группа ресурсов.

В приведенном выше примере мы создали группу ресурсов под названием «NRP RG» с расположением «Запад США».

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Создание виртуальной сети и общедоступного IP-адреса для пула IP-адресов клиентской части

Создание подсети для виртуальной сети и присвоение ее переменной $backendSubnet

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Создайте виртуальную сеть:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Создание виртуальной сети, добавление подсети lb-subnet-be к виртуальной сети NRPVNet и присвоение ее переменной $vnet

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Создание пула IP-адресов клиентской части и пула адресов серверной части

Настройка пула IP-адресов клиентской части для входящего сетевого трафика подсистемы балансировки нагрузки и пула адресов серверной части для получения распределенного сетевого трафика.

### <a name="step-1"></a>Шаг 1

Создание внешнего пула IP-адресов с помощью частного IP-адреса 10.0.2.5 для подсети 10.0.2.0/24, которая будет служить конечной точкой для входящего сетевого трафика.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>Шаг 2

Настройте пул адресов серверной части для приема входящего трафика из пула IP-адресов клиентской части:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Создание правил балансировки нагрузки, правил преобразования сетевых адресов, пробы и подсистемы балансировки нагрузки

После создания пула IP-адресов клиентской части и пула адресов серверной части необходимо создать правила, которые будут принадлежать ресурсу подсистемы балансировки нагрузки:

### <a name="step-1"></a>Шаг 1

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

В вышеприведенном примере создаются следующие элементы.

* Входящее правило NAT для направления трафика с порта 3441 на порт 3389.
* Второе входящее правило NAT для направления трафика с порта 3442 на порт 3389.
* Правило балансировки нагрузки, согласно которому весь трафик, поступающий на общедоступный порт 80, будет распределяться на локальный порт 80 в пуле адресов серверной части.
* Правило пробы, согласно которому будет проверяться доступность пути «HealthProbe.aspx».

### <a name="step-2"></a>Шаг 2

Создайте подсистему балансировки нагрузки, объединив все объекты (правила преобразования сетевых адресов, правила балансировки нагрузки, настройки проб):

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Создание сетевых интерфейсов

После создания внутренней подсистемы балансировки нагрузки необходимо определить сетевые интерфейсы, к которым будут применяться правила NAT и пробы и которые будут получать сетевой трафик с распределенной нагрузкой. В этом случае сетевой интерфейс настраивается отдельно и может быть назначен виртуальной машине не сразу.

### <a name="step-1"></a>Шаг 1

Определите ресурс виртуальной сети и подсети для создания сетевых интерфейсов:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

На этом шаге мы создаем сетевой интерфейс, которой будет принадлежать пулу серверной части подсистемы балансировки нагрузки, а затем назначаем первое правило NAT протоколу удаленного рабочего стола для данного сетевого интерфейса:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>Шаг 2

Создайте второй сетевой интерфейс с названием «LB-Nic2-BE»:

На этом шаге мы создаем второй сетевой интерфейс, присваиваем его тому же пулу серверной части подсистемы балансировки нагрузки и связываем второе правило NAT с протоколом удаленного рабочего стола:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

Результат будет аналогичным следующему:

    $backendnic1

Ожидаемые выходные данные:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Шаг 3.

Используйте команду Add-AzureRmVMNetworkInterface, чтобы присвоить сетевую карту виртуальной машине.

Пошаговые инструкции по созданию виртуальной машины и назначению сетевой карты описаны в статье [Создание виртуальной машины Windows с помощью Resource Manager и PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface"></a>Добавление сетевого интерфейса

Если виртуальная машина у вас уже есть, добавьте сетевой интерфейс, выполнив описанные ниже действия:

### <a name="step-1"></a>Шаг 1

Загрузите ресурс балансировщика нагрузки в переменную (если вы это еще не сделали). Имя переменной — $lb. Используйте имена из ресурса балансировщика нагрузки, созданного ранее.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>Шаг 2

Загрузите в переменную конфигурацию серверной части.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>Шаг 3.

Загрузите в переменную созданный ранее сетевой интерфейс. Имя переменной — $nic. Имя сетевого интерфейса совпадает с именем в приведенном выше примере.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>Шаг 4.

Измените конфигурацию серверной части в сетевом интерфейсе.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>Шаг 5

Сохраните объект сетевого интерфейса.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

После того как сетевой интерфейс будет добавлен в пул серверной части балансировщика нагрузки, он начнет получать сетевой трафик согласно правилам балансировки нагрузки для соответствующего ресурса балансировщика.

## <a name="update-an-existing-load-balancer"></a>Обновление существующего балансировщика нагрузки

### <a name="step-1"></a>Шаг 1
Используя балансировщик нагрузки из предыдущего примера, присвойте переменной $slb ссылку на объект балансировщика нагрузки, используя метод Get-AzureRmLoadBalancer.

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>Шаг 2

В следующем примере вы добавите новое входящее правило NAT для порта 81 на клиентской части и порта 8181 на серверной части, которое будет применяться к пулу существующего балансировщика нагрузки.

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>Шаг 3.

Сохраните новую конфигурацию, используя командлет Set-AzureLoadBalancer.

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Удалите балансировщика нагрузки.

Воспользуйтесь командой Remove-AzureRmLoadBalancer, чтобы удалить ранее созданного балансировщика нагрузки с именем NRP-LB в группе ресурсов NRP RG.

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Чтобы пропустить подтверждение удаления, можно использовать необязательный ключ -Force.

## <a name="next-steps"></a>Дальнейшие действия

[Настройка режима распределения подсистемы балансировки нагрузки](load-balancer-distribution-mode.md)

[Настройка параметров времени ожидания простоя TCP для подсистемы балансировки нагрузки](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO5-->


