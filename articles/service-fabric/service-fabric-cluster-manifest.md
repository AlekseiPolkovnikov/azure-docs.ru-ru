---
title: "Настройка автономного кластера | Документация Майкрософт"
description: "В этой статье описывается настройка автономного или частного кластера Service Fabric."
services: service-fabric
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2016
ms.author: dkshir
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b19d1ed96cf8e294cb105fa62f8623b28e3cc7fc


---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Параметры конфигурации для автономного кластера Windows
В этой статье описывается настройка автономного кластера Service Fabric с помощью файла ***ClusterConfig.JSON***. С помощью этого файла можно указать для изолированного кластера сведения об узлах Service Fabric и их IP-адресах, о различных типах узлов кластера, настройках безопасности, а также о топологии сети с точки зрения доменов сбоя и доменов обновления.

Несколько примеров файла ClusterConfig.JSON скачиваются на ваш рабочий компьютер при [скачивании автономного пакета Service Fabric](service-fabric-cluster-creation-for-windows-server.md#downloadpackage). Примеры с *DevCluster* в имени помогут вам создать кластер с тремя узлами на одном и том же компьютере (например, это могут быть логические узлы). По крайней мере один узел должен быть помечен как первичный. Данный кластер удобен для среды разработки или тестирования, но не предназначен для использования в качестве рабочего кластера. Примеры с *MultiMachine* в имени помогут вам создать кластер рабочего уровня, каждый узел которого размещен на отдельном компьютере. Число первичных узлов такого кластера будет основано на [уровне надежности](#reliability).

1. Примеры *ClusterConfig.Unsecure.DevCluster.JSON* и *ClusterConfig.Unsecure.MultiMachine.JSON* показывают, как создать незащищенный тестовый или рабочий кластер, соответственно. 
2. Примеры *ClusterConfig.Windows.DevCluster.JSON* и  *ClusterConfig.Windows.MultiMachine.JSON* показывают, как создать тестовый или рабочий кластер, защищенный с помощью [системы безопасности Windows](service-fabric-windows-cluster-windows-security.md).
3. Примеры *ClusterConfig.X509.DevCluster.JSON* и *ClusterConfig.X509.MultiMachine.JSON* показывают, как создать тестовый или рабочий кластер, защищенный с помощью [системы безопасности на основе сертификатов X509](service-fabric-windows-cluster-x509-security.md). 

Теперь рассмотрим различные разделы файла ***ClusterConfig.JSON***, приведенные ниже.

## <a name="general-cluster-configurations"></a>Общие параметры кластера
Сюда входит целый ряд параметров кластера, как показано в следующем фрагменте кода JSON.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Для кластера Service Fabric можно задать любое понятное имя, присвоив его переменной **name**. **ClusterConfigurationVersion** — номер версии кластера. Его следует увеличивать при каждом обновлении кластера Service Fabric. Тем не менее следует оставить значение переменной **apiVersion** по умолчанию.

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>Узлы в кластере
Можно настроить узлы в кластере Service Fabric с помощью раздела **nodes** , как показано в следующем фрагменте кода.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Кластер Service Fabric должен содержать по крайней мере 3 узла. Можно добавить дополнительные узлы в этот разделе в соответствии с вашей конфигурацией. В следующей таблице описаны параметры конфигурации для каждого узла.

| **Параметр узла** | **Описание** |
| --- | --- |
| nodeName |Узлу можно присвоить любое понятное имя. |
| iPAddress |Узнать IP-адрес узла можно, открыв командное окно и введя `ipconfig`. Запишите этот IPv4-адрес и назначьте его переменной **iPAddress** . |
| nodeTypeRef |Узлам можно назначить разные типы узла. [Типы узлов](#nodetypes) определены в следующем разделе. |
| faultDomain |Домены сбоя позволяют администраторам кластера определять физические узлы, на которых из-за наличия общих физических зависимостей могут одновременно возникать сбои. |
| upgradeDomain |Домены обновления описывают наборы узлов, которые почти одновременно завершают работу, чтобы выполнить обновления Service Fabric. Так как они не ограничены какими-либо физическими требованиями, вы можете выбрать узлы, которые следует назначить тем или иным доменам обновления. |

## <a name="cluster-properties"></a>Свойства кластера
Раздел **properties** в файле ClusterConfig.JSON используется для настройки кластера следующим образом.

<a id="reliability"></a>

### <a name="reliability"></a>Надежность
Раздел **ReliabilityLevel** определяет количество копий системных служб, которые могут выполняться на первичных узлах кластера. Это позволяет повысить надежность этих служб и, следовательно, кластера. Этой переменной можно задать значение *Bronze*, *Silver*, *Gold* или *Platinum*, чтобы разрешить выполнение 3, 5, 7 или 9 копий этих служб, соответственно. См. следующий пример.

    "reliabilityLevel": "Bronze",

Обратите внимание, что так как на первичном узле выполняется одна копия системных служб, вам потребуется как минимум 3 первичных узла для уровня надежности *Bronze*, 5 для уровня надежности *Silver*, 7 для уровня надежности *Gold* и 9 для уровня надежности *Platinum*.

### <a name="diagnostics"></a>Диагностика
В разделе **diagnosticsStore** можно настроить параметры, чтобы включить диагностику и устранение неполадок узлов и кластера, как показано в следующем фрагменте кода. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Переменная **metadata** представляет собой описание диагностики кластера и может быть задана в соответствии с вашей конфигурацией. Эти переменные помогают собирать журналы трассировки событий Windows, аварийные дампы и данные счетчиков производительности. Прочитайте статьи [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) и [Трассировка событий Windows](https://msdn.microsoft.com/library/ms751538.aspx), чтобы больше узнать о журналах трассировки событий Windows. Все журналы, в том числе [аварийные дампы](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) и данные [счетчиков производительности](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx), можно перенаправить в папку **perfCtrConnectionString** на вашем компьютере. Для хранения данных диагностики можно также использовать *AzureStorage* . Ниже приведен фрагмент кода.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Безопасность
Раздел **security** необходим для защиты автономного кластера Service Fabric. В следующем фрагменте кода показана часть этого раздела.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Переменная **metadata** представляет собой описание защищенного кластера и может быть задана в соответствии с вашей конфигурацией. Переменные **ClusterCredentialType** и **ServerCredentialType** определяют тип безопасности, реализуемой кластером и узлами. Можно задать значение *X509* для защиты на основе сертификатов или *Windows* для защиты на основе Azure Active Directory. Остальная часть раздела **security** будет зависеть от выбранного типа безопасности. Прочитайте статью [Защита автономного кластера под управлением Windows с помощью сертификатов](service-fabric-windows-cluster-x509-security.md) или [Защита автономного кластера под управлением Windows с помощью системы безопасности Windows](service-fabric-windows-cluster-windows-security.md), чтобы узнать, как заполнить остальную часть раздела **security**.

<a id="nodetypes"></a>

### <a name="node-types"></a>Типы узлов
В разделе **nodeTypes** описывается тип узлов в кластере. Для кластера нужно указать по крайней мере один тип узла, как показано в следующем фрагменте кода. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**name** — понятное имя этого конкретного типа узла. Чтобы создать узел данного типа, присвойте его понятное имя переменной **nodeTypeRef** для этого узла, как упоминалось [выше](#clusternodes). Для каждого типа узла определите конечные точки подключения, которые будут использоваться. Для этих конечных точек подключения можно выбрать любой номер порта, если он не конфликтует с другими конечными точками в данном кластере. В кластере с несколькими узлами будет один или несколько первичных узлов (т. е. **isPrimary** имеет значение *true*), в зависимости от значения [**reliabilityLevel**](#reliability). Прочитайте статью [Рекомендации по планированию загрузки кластера Service Fabric](service-fabric-cluster-capacity.md), чтобы больше узнать о значениях **nodeTypes** и **reliabilityLevel** и о том, что такое первичный и непервичный типы узлов. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Конечные точки, используемые для настройки типов узлов
* *clientConnectionEndpointPort* — порт, используемый клиентом для подключения к кластеру при использовании клиентских API. 
* *clusterConnectionEndpointPort* — порт, по которому узлы взаимодействуют друг с другом.
* *leaseDriverEndpointPort* — порт, используемый драйвером аренды кластера для определения того, активны ли еще узлы. 
* *serviceConnectionEndpointPort* — порт, используемый приложениями и службами, развернутыми на узле, для связи с клиентом Service Fabric на данном узле.
* *httpGatewayEndpointPort* — порт, используемый Service Fabric Explorer для подключения к кластеру.
* *ephemeralPorts* переопределяет [динамические порты, используемые операционной системой](https://support.microsoft.com/kb/929851). Service Fabric будет использовать часть из них в качестве портов приложений, а остальные будут доступны для операционной системы. Кроме того, этот диапазон будет сопоставлен с существующим диапазоном в операционной системе, поэтому для любых целей вы можете использовать диапазоны, указанные в примерах файлов JSON. Необходимо убедиться в том, что разница между номерами начального и конечного портов составляет не менее 255. Если это различие слишком мало, могут возникнуть конфликты, так как этот диапазон также используется для операционной системы. Чтобы увидеть настроенный динамический диапазон портов, запустите `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* — порты, которые будут использоваться приложениями Service Fabric. Они должны быть подмножеством диапазона *ephemeralPorts*, достаточным для обеспечения потребностей приложений в конечных точках. Service Fabric будет использовать их каждый раз, когда потребуются новые порты, а также разрешать их использование в брандмауэре. 
* *reverseProxyEndpointPort* — это необязательная конечная точка обратного прокси-сервера. Дополнительные сведения см. в статье [Обратный прокси-сервер Service Fabric](service-fabric-reverseproxy.md). 

### <a name="other-settings"></a>Другие параметры
В разделе **fabricSettings** можно задать корневые каталоги для данных и журналов Service Fabric. Их можно настроить только во время создания исходного кластера. Ниже приведен фрагмент кода из этого раздела.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Рекомендуется использовать несистемный диск (без ОС) в качестве FabricDataRoot и FabricLogRoot, так как он обеспечивает большую надежность и независимость от сбоев операционной системы. Обратите внимание, что если настроить только корневой каталог данных, то корневой каталог файлов журнала будет помещен на один уровень ниже корневого каталога данных.

## <a name="next-steps"></a>Дальнейшие действия
Завершив настройку файла ClusterConfig.JSON в соответствии с конфигурацией автономного кластера, вы можете развернуть этот кластер, следуя указаниям в статье [Создание кластера Azure Service Fabric в локальной системе или облаке](service-fabric-cluster-creation-for-windows-server.md), а затем перейти к [визуализации кластера с помощью Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).




<!--HONumber=Nov16_HO3-->


