---
title: "Настройка виртуальной сети и шлюза для ExpressRoute на классическом портале | Документация Майкрософт"
description: "Эта статья поможет настроить виртуальную сеть для ExpressRoute с использованием классической модели развертывания и классического портала."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1206d7444f32216597a6f546c71131b2de0ec3f8


---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Создание виртуальной сети для ExpressRoute на классическом портале
В этой статье описано, как настроить виртуальную сеть и шлюз виртуальной сети для ExpressRoute с использованием классической модели развертывания и классического портала.

Инструкции для модели развертывания Resource Manager см. в статье [Создание виртуальной сети с помощью PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) и [Настройка шлюза виртуальной сети для ExpressRoute с Resource Manager и PowerShell](expressroute-howto-add-gateway-resource-manager.md).

**О моделях развертывания Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Создание классической виртуальной сети и шлюза
Следующие действия позволяют создать классическую виртуальную сеть и шлюз виртуальной сети. Если у вас уже есть классическая виртуальная сеть, переходите к разделу [Настройка существующей классической виртуальной сети](#config) далее в этой статье.

1. Перейдите на [классический портал Azure](http://manage.windowsazure.com).
2. В нижнем левом углу экрана нажмите кнопку **Создать**. В области навигации щелкните **Сетевые службы**, а затем — **Виртуальная сеть**. Нажмите кнопку **Настраиваемое создание** для запуска мастера настройки.
3. На странице **Сведения о виртуальной сети** укажите следующую информацию:
   
   * **Имя** : название виртуальной сети. Это имя виртуальной сети будет использоваться при развертывании виртуальных машин и экземпляров PaaS, поэтому не рекомендуется создавать слишком сложное имя.
   * **Расположение** : непосредственно связано с физическим расположением (регионом) ресурсов (виртуальных машин). Например, если вы хотите, чтобы виртуальные машины находились в виртуальной сети, которая физически расположена в восточной части США, выберите соответствующий регион. После создания виртуальной сети изменить связанное с ней расположение нельзя.
4. На странице **DNS-серверы и подключение VPN** укажите следующие сведения и щелкните стрелку «Далее» в правом нижнем углу. 
   
   * **DNS-серверы** : введите имя и IP-адрес DNS-сервера или выберите из контекстного меню ранее зарегистрированный DNS-сервер. Этот параметр не приводит к созданию DNS-сервера. Он позволяет указать DNS-сервер, который вы хотите использовать для разрешения имен в этой виртуальной сети.
   * **Подключение типа "сеть — сеть".** Установите флажок **Настроить подключение VPN типа "сеть-сеть"**.
   * **ExpressRoute.** Установите флажок **Использование ExpressRoute**. Этот параметр отображается, только если на предыдущем шаге вы установили флажок **Настроить подключение VPN типа "сеть — сеть"**.
   * **Локальная сеть** : необходимо создать сайт локальной сети для ExpressRoute. Однако для подключения ExpressRoute игнорируются префиксы адресов, указанные для сайта локальной сети. Вместо них для целей маршрутизации используются префиксы адресов, переданные в Майкрософт через канал ExpressRoute.<BR>Если вы уже создали локальную сеть для подключения к ExpressRoute, выберите ее в раскрывающемся списке. В противном случае выберите **Указать новую локальную сеть**.
5. Если ранее вы выбрали указание новой локальной сети, откроется страница **Подключение типа "сеть — сеть"** . Чтобы настроить локальную сеть, укажите следующие сведения и нажмите кнопку "Далее". 
   
   * **Имя** : имя площадки в локальной сети.
   * **Адресное пространство** : включает в себя начальный IP-адрес и CIDR (число адресов). Можно указать любой диапазон адресов, если он не пересекается с диапазоном адресов для виртуальной сети. Как правило, здесь указываются диапазоны адресов для локальных сетей, но для ExpressRoute эти параметры не используются. Тем не менее этот параметр является обязательным, если вы создаете локальную сеть на классическом портале.
   * **Добавить адресное пространство** : этот параметр не относится к ExpressRoute.
6. На странице **Адресные пространства виртуальной сети** укажите следующие сведения и затем нажмите кнопку с галочкой в нижнем правом углу для настройки сети. 
   
   * **Адресное пространство** : включает в себя начальный IP-адрес и число адресов. Убедитесь, что заданные адресные пространства не перекрываются ни с одним из адресных пространств, которые используются в локальной сети.
   * **Добавить подсеть** — включает в себя начальный IP-адрес и число адресов. Дополнительные подсети не требуются.
   * **Добавить подсеть шлюза** — щелкните, чтобы добавить подсеть шлюза. Подсеть шлюза используется только для шлюза виртуальной сети и необходима в этой конфигурации.<BR>Стандарт CIDR подсети шлюза (количество адресов) для ExpressRoute должен иметь значение /28 или больше (/27, /26 и т. д.). Это обеспечит достаточное количество IP-адресов в подсети для работы такой конфигурации. Если вы работаете на классическом портале и установили флажок использования ExpressRoute, портал выберет подсеть шлюза с префиксом /28.  На классическом портале невозможно изменить количество адресов CIDR. Подсеть шлюза будет отображаться на классическом портале с именем **Gateway**, хотя на самом деле созданная подсеть получит имя **GatewaySubnet**. Настоящее имя можно получить с помощью PowerShell или на портале Azure.
7. Установите флажок в нижней части страницы, после чего начнется создание виртуальной сети. По завершении работы на классическом портале Azure на странице **Сети** в разделе **Состояние** появится значение **Создано**.

## <a name="a-namegwacreate-the-gateway"></a><a name="gw"></a>Создание шлюза
1. На странице **Сети** щелкните только что созданную виртуальную сеть, а затем — **Панель мониторинга** в верхней части страницы.
2. В нижней части **панели мониторинга** щелкните **Создать шлюз** и выберите **динамическую маршрутизацию**. Щелкните **Да** , чтобы подтвердить создание шлюза.
3. В начале создания шлюза появится сообщение, информирующее о запуске шлюза. Для создания шлюза потребуется до 45 минут.
4. Привяжите вашу сеть к каналу. Следуйте указаниям в статье [Как связать виртуальные сети и каналы ExpressRoute](expressroute-howto-linkvnet-classic.md).

## <a name="a-nameconfigaconfigure-an-existing-classic-vnet-for-expressroute"></a><a name="config"></a>Настройка существующей классической виртуальной сети для ExpressRoute
Если у вас уже есть классическая виртуальная сеть, ее можно настроить для подключения к ExpressRoute на классическом портале. Параметры подключения такие же, как описано в разделах выше. Изучите эти разделы, чтобы знать нужные параметры. Если вы хотите создать одновременно действующие подключения ExpressRoute и "сеть — сеть", выполните действия из [этой статьи](expressroute-howto-coexist-classic.md). Они отличаются от процедуры, описанной в данной статье.

1. Следует сначала создать локальную сеть и только затем обновлять остальные параметры виртуальной сети. Чтобы создать новую локальную сеть, которая потребуется для настройки ExpressRoute на классическом портале, щелкните **Создать** **>** **Сетевые службы** **>** **Виртуальная сеть** **>** **Добавить локальную сеть**. Выполните инструкции мастера, чтобы создать локальную сеть.
2. На странице **Настройка** обновите остальные параметры виртуальной сети и свяжите виртуальную сеть с локальной сетью.
3. После настройки параметров перейдите к разделу этой статьи [Создание шлюза](#gw) , чтобы создать шлюз.

## <a name="next-steps"></a>Дальнейшие действия
* Если вы хотите добавить виртуальные машины в свою виртуальную сеть, см. статью [Схемы обучения для виртуальных машин](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Дополнительные сведения об ExpressRoute см. в статье [Технический обзор ExpressRoute](expressroute-introduction.md).




<!--HONumber=Nov16_HO3-->


