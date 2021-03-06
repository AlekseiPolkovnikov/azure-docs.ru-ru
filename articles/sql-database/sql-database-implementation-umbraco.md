---
title: "Пример использования базы данных SQL Azure: Umbraco | Документация Майкрософт"
description: "Узнайте, как Umbraco использует Базу данных SQL, чтобы быстро подготавливать и масштабировать службы для тысяч клиентов в облаке."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5243d31e-3241-4cb0-9470-ad488ff28572
ms.service: sql-database
ms.custom: app development case study; app development
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/22/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0800f04034410c3734ef0a97afd9d41cf850381b


---
# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco использует Базу данных SQL Azure, чтобы быстро подготавливать и масштабировать службы для тысяч клиентов в облаке
![Логотип Umbraco](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco — это популярная система управления содержимым (CMS) с открытым кодом, в которой можно запускать как небольшие сайты с рекламными кампаниями или брошюрами, так и сложные приложения для компаний из списка Fortune 500 и глобальные веб-сайты с мультимедийным содержимым. 

> "Сообщество разработчиков, использующих систему, достаточно велико. Это более 100 000 разработчиков на наших форумах и более 350 000 работающих сайтов на платформе Umbraco"
> 
> — Мортен Кристенсен (Morten Christensen), технический руководитель в Umbraco.
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

Чтобы упростить развертывание клиентов, компания Umbraco добавила решение Umbraco как услуга (UaaS). Это предложение SaaS (ПО как услуга), которое устраняет необходимость в локальных развертываниях, обеспечивает встроенные функции масштабирования и устраняет лишние операции управления, что позволяет разработчикам сосредоточиться на инновациях в сфере продуктов, а не управлении решениями. Umbraco может предоставить все эти преимущества на основе гибкой модели PaaS (платформа как услуга), предлагаемой Microsoft Azure.

UaaS позволяет клиентам SaaS использовать возможности Umbraco CMS, которые были ранее недоступны для них. Для этих клиентов подготавливается рабочая среда CMS, которая включает в себя рабочую базу данных. Клиенты могут добавить до двух дополнительных баз данных для среды разработки и промежуточной среды, в зависимости от своих потребностей. При запросе новой среды автоматизированный процесс назначает клиенту базу данных автоматически. Новая база данных становится готова за считанные секунды, так как она уже была предварительно подготовлена Umbraco в пуле эластичных БД Azure, содержащем доступные базы данных (см. рисунок 1).

![Жизненный цикл подготовки Umbraco](./media/sql-database-implementation-umbraco/figure1.png)

Рисунок 1. Жизненный цикл подготовки для UaaS

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Пулы эластичных БД Azure и автоматизация упрощают развертывание
Используя Базу данных SQL Azure и другие службы Azure, клиенты Umbraco могут самостоятельно подготавливать собственные среды, а Umbraco может легко отслеживать базы данных и управлять ими в рамках интуитивно понятного рабочего процесса.

1. Подготовка
   
   Umbraco содержит 200 доступных предварительно подготовленных баз данных в пулах эластичных БД. Когда новый клиент регистрируется для использования UaaS, компания Umbraco предоставляет ему новую среду CMS в псевдореальном времени, назначая базу данных из пула доступности.
   
   Когда пул доступности достигает порогового значения, создается новый пул эластичных БД, в котором предварительно подготавливаются новые базы данных для назначения клиентам по мере необходимости.
   
   Реализация полностью автоматизирована с помощью библиотек управления C# и очередей служебной шины Azure.
2. Использование
   
   Клиенты используют от одной до трех сред (рабочая среда, промежуточная среда и (или) среда разработки), каждая из которых обладает собственной базой данных. Базы данных клиента находятся в пулах эластичных баз данных, благодаря чему Umbraco обеспечивает эффективное масштабирование без необходимости подготавливать лишние ресурсы.
   
   ![Обзор проекта Umbraco](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Сведения о проекте Umbraco](./media/sql-database-implementation-umbraco/figure3.png)
   
   Рис. 2. Веб-сайт для клиентов UaaS, на котором отображен обзор и сведения о проекте
   
   База данных SQL Azure использует единицы транзакций базы данных (DTU) для представления относительный мощности, необходимой для обработки транзакций реальной базы данных. Для клиентов UaaS базы данных обычно используют 10 DTU, но каждая из них может гибко менять эту величину по запросу. Таким образом UaaS гарантирует, что у клиентов всегда будут необходимые ресурсы, даже во время пиковых нагрузок. Например, во время спортивного события в прошлое воскресенье один из клиентов UaaS наблюдал скачки нагрузки на базу данных до 100 DTU во время игры. Пулы эластичных БД Azure дают Umbraco возможность обеспечивать такие повышенные потребности без снижения производительности.
3. Монитор
   
   Umbraco отслеживает операции баз данных с помощью панелей мониторинга на портале Azure и настраиваемых оповещений по электронной почте.
4. Аварийное восстановление
   
   Azure предоставляет два варианта аварийного восстановления : активную георепликацию и геовосстановление. Выбор варианта аварийного восстановления зависит от [целей по обеспечению непрерывности бизнес-процессов](sql-database-business-continuity.md)компании.
   
   Активная георепликация обеспечивает наиболее быстрый уровень реагирования в случае простоя. С помощью активной георепликации можно создать до четырех доступных для чтения баз данных-получателей на серверах в разных регионах, чтобы затем можно было инициировать отработку отказа на любую из них в случае сбоя.
   
   Umbraco не требует использовать георепликацию, но использует преимущества геовосстановления Azure, чтобы обеспечить минимальное время простоя в случае сбоя. Геовосстановление использует резервные копии базы данных из геоизбыточного хранилища Azure. Это позволяет пользователям восстановить данные из резервной копии в случае сбоя в основном регионе.
5. Отзыв
   
   При удалении среды проекта во время очистки очереди служебной шины Azure удаляются все связанные базы данных (базы данных для разработки, а также промежуточные или динамические базы данных). Этот автоматизированный процесс возвращает неиспользуемые базы данных в пул эластичных баз данных (пул доступности) Umbraco, делая их доступными для будущей подготовки. Это обеспечивает максимально эффективное использование баз данных.

## <a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Пулы эластичных БД обеспечивают удобное масштабирование UaaS
Используя преимущества пулов эластичных баз данных Azure, Umbraco может оптимизировать производительность для своих клиентов без необходимости подготавливать чрезмерное или недостаточное число ресурсов. В настоящее время у Umbraco имеется около 3000 баз данных в 19 пулах эластичных баз данных. При необходимости компания может легко их масштабировать в соответствии потребностями любого из своих 325 000 существующих клиентов или новых клиентов, которые готовы к развертыванию CMS в облаке.

На самом деле, как утверждает Мортен Кристенсен, технический руководитель в Umbraco: "Сейчас UaaS увеличивается на 30 новых клиентов в день. Наши клиенты очень довольны удобной возможности подготавливать новые проекты за считанные секунды, мгновенно публиковать обновления для работающих сайтов из среды разработки с помощью "развертывания одним щелчком" и вносить изменения сразу же после обнаружения ошибок".

Если клиенту больше не нужна вторая и (или) третья среда, он может просто удалить их. Это позволяет освободить ресурсы, которые могут использоваться другими клиентами в пуле эластичных баз данных Umbraco.

![Архитектура развертывания Umbraco](./media/sql-database-implementation-umbraco/figure4.png)

Рис. 3. Архитектура развертывания UaaS в Microsoft Azure

## <a name="the-path-from-datacenter-to-cloud"></a>Путь от центра обработки данных к облаку
Когда разработчики Umbraco изначально приняли решение перейти на модель SaaS, они знали, что им понадобится экономичное и масштабируемое решение, чтобы создать службу.

> "Пулы эластичных баз данных идеально подходят для нашего предложения SaaS, так как мы можем увеличивать и уменьшать емкость, как захотим. Подготовка очень проста, и с нашей конфигурацией мы можем обеспечить максимально эффективное использование ресурсов"
> 
> — Мортен Кристенсен (Morten Christensen), технический руководитель в Umbraco.
> 
> 

"Мы хотели тратить время на решение проблем наших клиентов, а не управление инфраструктурой. На хотелось, чтобы нашим клиентам было легче извлечь максимальную пользу, — говорит Нильс Хартвиг (Niels Hartvigс), основатель компании Umbraco. — Сначала мы решили сами размещать серверы, но тогда планирование загрузки было бы настоящим кошмаром". По стечению обстоятельств Umbraco не нанимает администраторов баз данных, что только подчеркивает ценность предложения использовать UaaS.

Одной из важнейших целей для разработчиков Umbraco было позволить клиентам UaaS быстро и без ограничения по емкости подготавливать среды. Но предоставление выделенной размещенной службы в центрах обработки данных Umbraco потребовало бы много избыточной емкости, чтобы справляться со скачками рабочих нагрузок обработки. Это означало бы, что необходимо добавить значительную вычислительную инфраструктуру, которая простаивала бы во время обычной загрузки.

Кроме того, команда разработчиков Umbraco хотела найти решение, которое бы позволило по-максимуму повторно использовать существующий код. Миккель Мезен (Mikkel Madsen), разработчик Umbraco, рассказывает: "Мы были довольны уже привычными средствами разработки Майкрософт, такими как Microsoft SQL Server, база данных SQL Microsoft Azure, ASP.Net и IIS. Прежде чем инвестировать в облачное решение IaaS или PaaS, мы хотели убедиться в том, что оно будет поддерживать инструменты и платформы Майкрософт, чтобы нам не пришлось кардинально менять базовый код".

Определив все обязательные условия, компания Umbraco искала партнера в сфере облачных служб, решения которого обладают следующими характеристиками:

* Достаточная емкость и надежность.
* Поддержка средств разработки Майкрософт, чтобы инженерам Umbraco не пришлось полностью изобретать свои среды разработки с нуля.
* Присутствие на всех географических рынках, в которых UaaS составляет конкуренцию (предприятиям нужны гарантии быстрого доступа к данным и хранения данных в расположении, которое соответствует региональным нормативным требованиям).

## <a name="why-umbraco-chose-azure-for-uaas"></a>Почему компания Umbraco выбрала Azure для UaaS
Как говорит Мортен Кристенсен: "Рассмотрев все варианты, мы выбрали предложение Azure, так как оно соответствовало всем нашим условиям: от управляемости и масштабируемости до ознакомленности и экономической эффективности. Мы создаем среды на виртуальных машинах Azure, и у каждой среды имеется собственный экземпляр базы данных SQL Azure. Все эти экземпляры размещаются в пулах эластичных баз данных. Разделяя базы данных для среды разработки, промежуточной среды и динамической среды, мы можем предложить клиентам надежную изоляцию производительности, которая соответствует масштабу. Это огромное достижение".

Мортен продолжает: "Раньше нам приходилось вручную подготавливать серверы для баз данных в Интернете. Теперь нам даже не нужно задумываться об этом. Все автоматизировано — от подготовки до очистки".

Кроме того, Мортену нравятся возможности масштабирования, предоставляемые Azure. "Пулы эластичных баз данных идеально подходят для нашего предложения SaaS, так как мы можем увеличивать и уменьшать емкость, как захотим. Подготовка очень проста, и с нашей конфигурацией мы можем обеспечить максимально эффективное использование ресурсов" Мортен утверждает: "Простота пулов эластичных БД и гарантированные значения DTU на основе уровня служб дают нам возможность подготавливать новые пулы ресурсов по запросу. Недавно один из наших крупных клиентов наблюдал скачок нагрузки до 100 DTU в своей динамической среде. Благодаря Azure наши пулы эластичных БД предоставляют клиентам базы данных с необходимыми ресурсами в реальном времени, и на не нужно прогнозировать потребности в DTU. Проще говоря, наши клиенты получают ожидаемое время оборота, а мы можем соблюдать наши соглашения об уровне обслуживания производительности".

Миккель Мезен подводит итог: "Мы внедрили мощный алгоритм Azure, который соединяет распространенный сценарий SaaS (адаптируя новых клиентов в реальном времени согласно масштабу) с нашим шаблоном приложений (предварительно подготовленные базы данных для среды разработки и динамической среды) на основе базовой технологии (с использованием очередей служебной шины Azure и Базы данных SQL Azure)".

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Благодаря Azure UaaS превышает ожиданиям клиентов
Выбрав Azure в качестве партнера в сфере облачных служб, компания Umbraco смогла оптимизировать производительность управления содержимым клиентов UaaS, не вкладывая средства в ресурсы ИТ, которые потребовались бы для самостоятельного решения по размещению. Как говорит Мортен: "Мы ценим удобство работы разработчиков и масштабируемость, которые предоставляет Azure, и наши клиенты в восторге от полученных возможностей и надежности. В общем, для нас это было огромное достижение!"

## <a name="more-information"></a>Дополнительные сведения
* Дополнительные сведения о пулах эластичных баз данных см. в статье [Что такое пул эластичных БД Azure?](sql-database-elastic-pool.md).
* Дополнительные сведения о служебной шине Azure см. в статье [Служебная шина Azure](https://azure.microsoft.com/services/service-bus/).
* Чтобы узнать больше о веб-ролях и рабочих ролях, ознакомьтесь с [рабочими ролями](../fundamentals-introduction-to-azure.md#compute).    
* Чтобы больше узнать о виртуальных сетях Azure, ознакомьтесь с [виртуальными сетями](https://azure.microsoft.com/documentation/services/virtual-network/).    
* Чтобы узнать больше об архивации и восстановлении, ознакомьтесь с [обеспечением непрерывности бизнес-процессов](sql-database-business-continuity.md).    
* Чтобы узнать больше о мониторинге пулов, ознакомьтесь с [мониторингом пулов](sql-database-elastic-pool-manage-portal.md).    
* Чтобы узнать больше об Umbraco как службе, ознакомьтесь с [Umbraco](https://umbraco.com/cloud).




<!--HONumber=Nov16_HO3-->


