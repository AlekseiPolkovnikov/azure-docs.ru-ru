---
title: Архитектура надежной службы | Microsoft Docs
description: Обзор архитектуры надежных служб с отслеживанием и без отслеживания состояния.
services: service-fabric
documentationcenter: .net
author: AlanWarwick
manager: timlt
editor: vturecek

ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/30/2016
ms.author: alanwar

---
# Архитектура надежных служб с отслеживанием и без отслеживания состояния
Надежная служба Azure Service Fabric может быть с отслеживанием или без отслеживания состояния. Каждый тип службы выполняется в конкретной архитектуре. Эти архитектуры описаны в данной статье. Дополнительные сведения о различиях между службой с отслеживанием состояния и службой без отслеживания состояния см. в разделе [Обзор надежных служб](service-fabric-reliable-services-introduction.md).

## Надежные службы с отслеживанием состояния
### Архитектура службы с отслеживанием состояния
![Схема архитектуры службы с отслеживанием состояния](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### Надежная служба с отслеживанием состояния
Надежную службу с отслеживанием состояния можно произвести из класса StatefulService или StatefulServiceBase. Оба этих базовых класса предоставляются платформой Service Fabric. Они предлагают разные уровни поддержки и абстракции, позволяющие вашей службе с отслеживанием состояния взаимодействовать с платформой Service Fabric и участвовать в качестве службы в кластере Service Fabric.

Класс StatefulService является производным от класса StatefulServiceBase. Последний делает работу служб более разноплановой, но требует более глубокого понимания функций Service Fabric. Дополнительные сведения о создании служб с помощью классов StatefulService и StatefulServiceBase см. в статьях [Обзор надежных служб](service-fabric-reliable-services-introduction.md) и [Дополнительные возможности использования модели программирования надежных служб](service-fabric-reliable-services-advanced-usage.md).

Оба базовых класса определяют продолжительность и роль реализации службы. Реализация службы может переопределить виртуальные методы обоих базовых классов, если у реализации службы есть работа в этих моментах жизненного цикла реализации или если требуется создать коммуникационный объект-прослушиватель. Обратите внимание, что, хотя в приведенной выше схеме реализация службы может реализовать собственный коммуникационный объект-прослушиватель с ICommunicationListener, все же этот прослушиватель реализует служба Service Fabric и именно его использует реализация службы.

Надежная служба с отслеживанием состояния использует преимущества Reliable Collections с помощью диспетчера надежных состояний. Надежные коллекции — это локальные структуры данных высокой надежности, то есть для вашей службы они доступны всегда, даже во время отработки отказа службы. Каждый тип надежной коллекции реализуется поставщиком надежных состояний. Дополнительные сведения о надежных коллекциях см. в статье [Обзор надежных коллекций](service-fabric-reliable-services-reliable-collections.md).

### Диспетчер надежных состояний и поставщики состояний
Диспетчер надежных состояний —это объект, который управляет поставщиками надежных состояний. Он может создавать, удалять и перечислять поставщиков надежных состояний и обеспечивать их высокую надежность. Поставщик надежных состояний — это хранимая структура данных с высокой надежностью, например словарь или очередь.

Каждый поставщик надежных состояний содержит интерфейс, с помощью которого ваша служба с отслеживанием состояния взаимодействует с поставщиком надежных состояний. Например, с помощью IReliableDictionary служба взаимодействует с надежным словарем, а с помощью IReliableQueue — с надежной очередью. Все поставщики надежных состояний реализуют интерфейс IReliableState.

Диспетчер надежных состояний содержит интерфейс IReliableStateManager, с помощью которого к диспетчеру получает доступ служба с отслеживанием состояния. Интерфейсы для поставщиков надежных состояний возвращаются при помощи класса IReliableStateManager.

Диспетчер надежных состояний использует архитектуру подключаемого модуля, поэтому возможно динамическое подключение новых типов надежных коллекций.

Надежный словарь и надежная очередь основаны на реализации высокопроизводительного дифференциального хранилища с контролем версий.

### Репликатор транзакций
Репликатор транзакций обеспечивает одинаковость состояния службы (то есть состояния в надежных коллекциях и диспетчере надежных состояний) во всех репликах, которые используют вашу службу. Кроме того, он обеспечивает хранение состояния в журнале. Диспетчер надежных состояний взаимодействует с репликатором транзакций через закрытый механизм.

Чтобы передавать состояния на другие реплики экземпляра службы, репликатор транзакций использует сетевой протокол. Благодаря этому все реплики получают актуальную информацию о состоянии.

Репликатор транзакций хранит информацию о состоянии в журнале, поэтому в случае аварийного завершения работы узла или процесса эта информация не исчезает. Интерфейс работы с журналом работает с помощью закрытого механизма.

### Журнал
Компонент журнала дает возможность пользоваться постоянным высокопроизводительным хранилищем, которое можно оптимизировать для того, чтобы стала возможна запись на вращающийся или твердотельный накопитель. Журнал устроен так, чтобы постоянные хранилища (например, жесткие диски) располагались локально по отношению к узлам, использующим службу с отслеживанием состояния. По сравнению с удаленным постоянным хранилищем, которое не является локальным по отношению к узлу, это снижает задержки и увеличивает пропускную способность.

Компонент журнала использует несколько типов файлов журнала. Существует общий файл журнала для всего узла, который используют все реплики, поскольку он обеспечивает наименьшую задержку и высокую пропускную способность для хранения данных о состоянии. По умолчанию этот общий журнал помещается в рабочем каталоге узла Service Fabric, но его также можно настроить для размещения в другом месте, желательно на диске, зарезервированном только для общего журнала. Кроме того, каждая реплика для службы имеет выделенный файл журнала, который находится в рабочем каталоге службы. Способ настройки размещения выделенного журнала в другом месте не предусмотрен.

Информация о состоянии реплики попадает в общий журнал лишь на время. На постоянное хранение эта информация помещается в выделенный файл журнала. Информация о состоянии сначала записывается в общий файл журнала, а затем в фоновом режиме перемещается в выделенный файл журнала. Таким образом, при записи в общий журнал задержки минимизируются, а пропускная способность достигает пика. Это позволяет службе работать быстрее.

Операции чтения и записи в общем журнале выполняются посредством прямого ввода-вывода в предварительно выделенном на диске месте для общего файла журнала. Для оптимального использования места на диске с выделенными журналами выделенный файл журнала создается как разреженный файл NTFS. Обратите внимание, что это вызовет избыточное выделение места на диске и операционная система будет показывать, что выделенные файлы журнала используют гораздо больше дискового пространства, чем на самом деле.

Кроме минимального интерфейса, работающего в режиме пользователя, журнал создается как работающий в режиме ядра драйвер. Работая как драйвер в режиме ядра, журнал может повысить производительность использующих его служб до максимума.

Дополнительные сведения о настройке журнала см. в статье [Настройка надежных служб с отслеживанием состояния](service-fabric-reliable-services-configuration.md).

## Надежная служба без отслеживания состояния
### Архитектура службы без отслеживания состояния
![Схема архитектуры службы без отслеживания состояния](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### Надежная служба без отслеживания состояния
Реализации службы без отслеживания состояния являются производными от класса StatelessService или StatelessServiceBase. При этом второй обеспечивает дополнительную гибкость. Оба базовых класса определяют время существования и роль службы.

Реализация службы может переопределить виртуальные методы обоих базовых классов, если у службы есть работа в этих моментах жизненного цикла службы или если требуется создать коммуникационный объект-прослушиватель. Обратите внимание, что, хотя в приведенной выше схеме служба может реализовать собственный коммуникационный объект-прослушиватель с ICommunicationListener, все же этот прослушиватель реализует служба Service Fabric и именно его использует реализация службы.

Дополнительные сведения о создании служб с помощью классов StatelessService и StatelessServiceBase см. в статьях [Обзор надежных служб](service-fabric-reliable-services-introduction.md) и [Дополнительные возможности использования модели программирования надежных служб](service-fabric-reliable-services-advanced-usage.md).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Дальнейшие действия
Дополнительные сведения о Service Fabric см. в таких статьях:

[Обзор надежных служб](service-fabric-reliable-services-introduction.md)

[Быстрый запуск](service-fabric-reliable-services-quick-start.md)

[Обзор надежных коллекций](service-fabric-reliable-services-reliable-collections.md)

[Продвинутое использование надежных служб](service-fabric-reliable-services-advanced-usage.md)

[Настройка надежных служб](service-fabric-reliable-services-configuration.md)

<!---HONumber=AcomDC_0406_2016-->