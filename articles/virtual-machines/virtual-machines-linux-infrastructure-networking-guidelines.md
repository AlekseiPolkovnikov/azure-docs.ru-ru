---
title: Рекомендации по сетевой инфраструктуре | Microsoft Docs
description: Изучите основные рекомендации по проектированию и реализации, касающиеся развертывания виртуальных сетей в службах инфраструктуры Azure.
documentationcenter: ''
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: iainfou

---
# Рекомендации по сетевой инфраструктуре
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Эта статья посвящена действиям по планированию виртуальной сети в Azure и подключения к существующим локальным средам.

## Рекомендации по реализации виртуальных сетей
Решения

* Какой тип виртуальной сети необходим, чтобы разместить рабочую нагрузку ИТ-среды или ИТ-инфраструктуру (облачная или распределенная)?
* Какое адресное пространство необходимо для размещения подсетей и виртуальных машин сейчас и при целесообразном расширении в будущем, если используются распределенные виртуальные сети?
* Требуется создать централизованную виртуальную сеть или отдельные виртуальные сети для каждой группы ресурсов?

Задачи

* Определите адресное пространство для создаваемой виртуальной сети.
* Определить набор подсетей и адресного пространства для каждой из них.
* Если используются распределенные виртуальные сети, определите набор адресных пространств локальной сети для локальных расположений, доступ к которым необходим виртуальным машинам в виртуальной сети.
* Обратитесь к команде специалистов по локальным сетям, чтобы обеспечить правильную настройку маршрутизации при создании распределенной виртуальной сети.
* Создайте виртуальную сеть, следуя соглашению об именовании.

## виртуальные сети;
Виртуальные сети необходимы для обмена данными между виртуальными машинами. Как и в физических сетях, для них можно определить подсети, пользовательский IP-адрес, параметры DNS, фильтры безопасности и балансировку нагрузки. Используя [VPN типа "сеть — сеть"](../vpn-gateway/vpn-gateway-topology.md) или [канал ExpressRoute](../expressroute/expressroute-introduction.md), можно подключить виртуальные сети Azure к локальным сетям. Вы можете больше узнать о [виртуальных сетях и их компонентах](../virtual-network/virtual-networks-overview.md).

Используя группы ресурсов, вы получаете возможность гибкой разработки компонентов виртуальной сети. Виртуальные машины могут подключаться к виртуальным сетям за пределами своей группы ресурсов. Распространенным подходом является создание централизованных групп ресурсов, содержащих основную сетевую инфраструктуру, которой может управлять общая команда специалистов. Виртуальные машины и их приложения развертываются в отдельные группы ресурсов. Такой подход позволяет владельцам приложений обращаться к группе ресурсов, содержащей их виртуальные машины, не открывая доступ к конфигурации виртуальных сетевых ресурсов вне этой группы.

## Подключение сайтов
### Облачные виртуальные сети
Если локальным пользователям и компьютерам не требуется постоянное подключение к виртуальным машинам в виртуальной сети Azure, то структура вашей виртуальной сети довольно проста.

![Схема виртуальной сети только для облака](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Как правило, этот подход практикуется при таких рабочих нагрузках, когда необходимо подключение к Интернету, например когда используется веб-сервер. Вы можете управлять этими виртуальными машинами с помощью подключений SSH или VPN типа "точка — сеть".

Облачные виртуальные сети Azure могут использовать любую часть пространства частных IP-адресов, так как они не подключаются к локальной сети. В качестве адресного пространства можно использовать то же частное пространство, которое используется локально.

### Распределенные виртуальные сети
Если локальным пользователям и компьютерам требуется постоянное подключение к виртуальным машинам в виртуальной сети Azure, создайте распределенную виртуальную сеть. Подключите виртуальную сеть к локальной с помощью ExpressRoute или VPN-подключения типа "сеть-сеть".

![Схема распределенной виртуальной сети](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

В этой конфигурации виртуальная сеть Azure по сути является облачным расширением локальной сети.

Так как распределенные виртуальные сети подключаются к локальной сети, они должны использовать уникальную часть адресного пространства, используемого в организации. Подобно тому как разным помещениям организации назначаются разные IP-подсети, так и Azure становится еще одним расположением в локальной сети.

Чтобы разрешить передачу пакетов из распределенной виртуальной сети в локальную сеть, необходимо настроить набор соответствующих префиксов локальных адресов в рамках определения локальной сети для виртуальной сети. В зависимости от адресного пространства виртуальной сети и набора соответствующих локальных расположений, в локальной сети может быть много префиксов адресов.

Облачную виртуальную сеть можно преобразовать в распределенную, но для этого, скорее всего, потребуется переопределение IP-адресов адресного пространства виртуальной сети и ресурсов Azure. Поэтому подумайте, действительно ли требуется подключение виртуальной сети к локальной при назначении IP-подсети.

## Подсети
Подсети позволяют упорядочивать связанные ресурсы либо логически (например, одна подсеть для виртуальных машин, связанных с одним приложением), либо физически (например, одна подсеть для каждой группы ресурсов). Кроме того, чтобы обеспечить дополнительную безопасность, вы можете использовать методы изоляции подсетей.

Для развернутых виртуальных сетей необходимо проектировать подсети по тем же правилам, которые используются для локальных ресурсов. **Azure всегда использует первые три IP-адреса в адресном пространстве для каждой подсети**. Чтобы определить количество адресов, необходимых для подсети, посчитайте виртуальные машины, которые необходимы сейчас. Рассчитайте дальнейший рост, а затем для определения размера подсети используйте таблицу ниже.

| Необходимое количество виртуальных машин | Необходимое количество битов узла | Размер подсети |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> В случае обычной локальной подсети с n битов узла максимальное количество адресов узла составляет 2<sup>n</sup> – 2. В случае подсети Azure с n битов узла максимальное количество адресов узла составляет 2<sup>n</sup> – 5 (2 плюс 3 в случае адресов, используемых Azure в каждой подсети).
> 
> 

Если выбранный размер подсети слишком мал, требуется переопределить IP-адреса виртуальных машин и повторно развернуть их в подсети.

## группы сетевой безопасности;
К трафику, который проходит через виртуальные сети, можно применить правила фильтрации с помощью групп безопасности сети. Вы можете сформировать очень точные правила фильтрации, чтобы защитить среду виртуальных сетей и управлять входящим и исходящим трафиком, исходным и конечным диапазонами IP-адресов, разрешенными портами и т. д. Группы безопасности сети могут применяться для подсетей в виртуальной сети или непосредственно для заданного виртуального сетевого интерфейса. Рекомендуется применять в виртуальной сети определенный уровень фильтрации трафика посредством группы безопасности сети. Вы можете больше узнать о [группах безопасности сети](../virtual-network/virtual-networks-nsg.md).

## Дополнительные сетевые компоненты
Как и сети в локальной физической сетевой инфраструктуре, виртуальные сети Azure могут содержать не только подсети и IP-адреса. При проектировании инфраструктуры приложений может потребоваться добавить некоторые из приведенных ниже дополнительных компонентов.

* [VPN-шлюзы](../vpn-gateway/vpn-gateway-about-vpngateways.md) позволяют соединять виртуальные сети Azure с другими виртуальными сетями Azure или локальными сетями через VPN-подключение типа "сеть-сеть". Кроме того, они дают возможность реализовывать каналы ExpressRoute для выделенных безопасных подключений и предоставлять пользователям прямой доступ через подключения VPN типа "точка-сеть".
* [Балансировщик нагрузки](../load-balancer/load-balancer-overview.md). Обеспечивает балансировку нагрузки внешнего и внутреннего трафика, если это требуется.
* [Шлюз приложений](../application-gateway/application-gateway-introduction.md). Обеспечивает балансировку нагрузки HTTP на уровне приложения, предоставляя дополнительные преимущества для веб-приложений по сравнению с развернутым балансировщиком Azure Load Balancer.
* [Диспетчера трафика](../traffic-manager/traffic-manager-overview.md). Обеспечивает распределение трафика на основе DNS, направляя пользователей к ближайшей конечной точке приложения, что позволяет размещать приложение в разных регионах вне центров обработки данных Azure.

## Дальнейшие действия
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0914_2016-->