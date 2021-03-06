---
title: "Настройка служб с помощью диспетчера кластерных ресурсов Service Fabric | Документация Майкрософт"
description: "Описание службы Service Fabric с указанием метрик, ограничений на размещение и других политик размещения."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/19/2016
ms.author: masnider
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e21127ab5c59e0acb3ffb28c44518e5baa5fe868


---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Настройка параметров диспетчера кластерных ресурсов для служб Service Fabric
Диспетчер кластерных ресурсов Service Fabric позволяет очень точно управлять правилами, регулирующими работу отдельных служб. Каждый экземпляр именованной службы может задать правила своего выделения в кластере. Он может также определить набор метрик, о которых будет сообщать, включая их важность для данной службы. Обычно настройка служб делится на три задачи:

1. настройка ограничений на размещение;
2. настройка метрик;
3. настройка расширенных политик размещения (мало распространено).

Рассмотрим их по очереди.

## <a name="placement-constraints"></a>Ограничения на размещение
Ограничения на размещение используются, чтобы контролировать, на каких именно узлах кластера может выполняться служба. Обычно вы видите определенный экземпляр именованной службы или все службы заданного типа, которые могут быть запущены только на узле определенного типа. При этом ограничения на размещение можно расширить. Вы можете задать любой набор свойств на основе типа узла, а затем выбрать их для ограничений при создании службы. Кроме того, ограничения на размещение можно динамически обновлять во время существования службы, что позволяет реагировать на изменения в кластере. Свойства заданного узла можно также обновлять динамически в кластере. Дополнительные сведения об ограничениях на размещение и их настройке см. в [этой статье](service-fabric-cluster-resource-manager-cluster-description.md#placement-constraints-and-node-properties).

## <a name="metrics"></a>Метрики
Метрики представляют собой набор ресурсов, необходимых заданному экземпляру службы, включая информацию о том, какую часть ресурса использует по умолчанию каждая реплика с отслеживанием состояния или экземпляр без отслеживания состояния этой службы. Кроме того, метрики содержат вес, который показывает, насколько важна балансировка по этой метрике для службы в случае, когда возникает необходимость компромисса.

## <a name="other-placement-rules"></a>Другие правила размещения
Существуют другие типы правил размещения, которые главным образом используются в кластерах, распределенных географически, или в других менее распространенных случаях. Они настраиваются с помощью корреляций или политик. Хотя они и не используются в большинстве сценариев, мы опишем их для полноты информации.

## <a name="next-steps"></a>Дальнейшие действия
* Метрики показывают, как диспетчер кластерных ресурсов Service Fabric управляет потреблением и емкостью в кластере. Дополнительные сведения о метриках и их настройке см. в [этой статье](service-fabric-cluster-resource-manager-metrics.md).
* Сходство — один режимов, который можно настроить для служб. Этот режим редко используется; узнать о нем подробнее можно [здесь](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* Существует много различных правил размещения, которые можно настроить для службы, чтобы реализовать дополнительные сценарии. Узнать о разных политиках размещения можно [здесь](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
* Начните с самого начала, [изучив общие сведения о диспетчере кластерных ресурсов Service Fabric](service-fabric-cluster-resource-manager-introduction.md)
* Чтобы узнать, как диспетчер кластерных ресурсов управляет нагрузкой кластера и балансирует ее, ознакомьтесь со статьей о [балансировке нагрузки](service-fabric-cluster-resource-manager-balancing.md)
* В диспетчере кластерных ресурсов много параметров для описания кластера. Чтобы узнать о них больше, ознакомьтесь с этой статьей об [описании кластера Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)




<!--HONumber=Nov16_HO3-->


