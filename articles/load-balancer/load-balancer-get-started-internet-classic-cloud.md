---
title: "Приступая к работе по созданию балансировщика нагрузки для Интернета по классической модели развертывания для облачных служб | Документация Майкрософт"
description: "Сведения о создании балансировщика нагрузки для Интернета по классической модели развертывания для облачных служб"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/17/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 171d5cd41d900b83c22e1db4bc514471a3d4b556

---

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Приступая к работе по созданию балансировщика нагрузки для Интернета для облачных служб

[!INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

В этой статье рассматривается классическая модель развертывания. Вы также можете [узнать, как создать балансировщик нагрузки для Интернета с помощью диспетчера ресурсов Azure](load-balancer-get-started-internet-arm-cli.md).

Облачные службы автоматически настраиваются с помощью подсистемы балансировки нагрузки. Их также можно настроить, используя модель службы.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Создайте балансировщик нагрузки при помощи CSDEF-файла

Вы можете обновить облачную службу с помощью пакета Azure SDK для .NET 2.5. Настройки конечных точек для облачных служб создаются в CSDEF-файле [определения службы](https://msdn.microsoft.com/library/azure/gg557553.aspx).

В следующем примере показано, как настроить файл servicedefinition.csdef для облачного развертывания.

В фрагменте CSDEF-файла, созданного путем облачного развертывания, можно увидеть внешнюю конечную точку, настроенную для использования портов HTTP через порты 10000, 10001 и 10002.

```xml
    <ServiceDefinition name=“Tenant“>
       <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
      </WorkerRole>
    </ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a>Проверка состояния работоспособности подсистемы балансировки нагрузки для облачных служб

Пример зонда работоспособности:

```xml
    <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
    </LoadBalancerProbes>
```

Балансировщик нагрузки объединяет информацию о конечной точке и информацию о зонде для создания URL-адреса в формате http://{DIP of VM}:80/Probe.aspx, который можно использовать для запроса на проверку работоспособности службы.

Служба обнаруживает периодические зонды с одного и того же IP-адреса. Это запрос зонда работоспособности, поступающий от узла, на котором запущена виртуальная машина. Служба должна отправить ответ с кодом состояния HTTP 200, что подтверждает для подсистемы балансировки нагрузки, что служба работоспособна. Любой другой код состояния HTTP (например, 503) непосредственно выводит виртуальную машину из ротации.

С помощью определения зонда можно контролировать частоту отправки зондов. В описанном выше случае подсистема балансировки нагрузки зондирует конечную точку каждые 5 секунд. Если в течение 10 секунд не приходит положительный ответ (два интервала зонда), предполагается, что зонд завершился сбоем, и виртуальная машина перестает использоваться. Аналогично, если получен положительный ответ для службы, которая не используется, сразу же выполняется ее автоматический запуск. Если состояние службы постоянно меняется (с рабочего на нерабочее и наоборот), подсистема балансировки нагрузки может отложить повторный запуск службы до получения определенного количества ответов, подтверждающих ее работоспособность.

Чтобы получить дополнительную информацию о [зонде работоспособности](https://msdn.microsoft.com/library/azure/jj151530.aspx), см. схему определения службы.

## <a name="next-steps"></a>Дальнейшие действия

[Приступая к настройке внутренней подсистемы балансировки нагрузки](load-balancer-get-started-ilb-arm-ps.md)

[Настройка режима распределения подсистемы балансировки нагрузки](load-balancer-distribution-mode.md)

[Настройка параметров времени ожидания простоя TCP для подсистемы балансировки нагрузки](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


