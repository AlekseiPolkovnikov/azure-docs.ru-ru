---
title: "Уровни служб. Производительность базы данных SQL | Документация Майкрософт"
description: "Сравнение уровней служб базы данных SQL."
keywords: "параметры базы данных, производительность базы данных"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/04/2017
ms.author: carlrab; janeng
translationtype: Human Translation
ms.sourcegitcommit: a40319d3e53c07a94bc34714ca7393c2747fb50c
ms.openlocfilehash: 340656b896763914c2f6d37c72ce1d5323d1411e


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Параметры базы данных SQL и производительность: возможности разных уровней служб
[База данных SQL Azure](sql-database-technical-overview.md) предлагает три уровня служб — **Базовый**, **Стандартный** и **Премиум** — с несколькими уровнями производительности для обработки разных рабочих нагрузок. Высокий уровень производительности предоставляет более обширный набор ресурсов, предназначенный для повышения пропускной способности. Вы можете [динамически изменять уровни служб и уровни производительности](sql-database-scale-up.md) без простоев в работе. По условиям SLA, все уровни службы — "Базовый", "Стандартный" и "Премиум" — включают такие преимущества, как бесперебойная работа в течение 99,99 %, гибкие способы обеспечения непрерывности бизнес-процессов, функции безопасности и почасовая тарификация. 

Вы можете создавать отдельные базы данных с помощью выделенных ресурсов на выбранном [уровне производительности](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Можно также управлять несколькими базами данных в [пуле эластичных БД](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus), где ресурсы совместно используются разными базами данных. Ресурсы, доступные для отдельных баз данных, выражаются в единицах транзакций базы данных (DTU), а для пулов эластичных баз данных — в эластичных единицах DTU (eDTU). Дополнительные сведения о DTU и eDTU см. в статье [Общие сведения об обычных единицах передачи данных (DTU) и единицах передачи данных в эластичной базе данных (eDTU)](sql-database-what-is-a-dtu.md). 

В обоих случаях существуют следующие уровни служб: **Базовый**, **Стандартный** и **Премиум**. 

## <a name="choosing-a-service-tier"></a>Выбор уровня службы
В следующей таблице приведены примеры уровней службы, которые лучше всего подходят для рабочих нагрузок различных приложений.

| Уровень служб | Целевые рабочие нагрузки |
| :--- | --- |
| **базовая;** | Лучше всего подходит для небольшой базы данных, для которой в определенный момент времени обычно выполняется одна активная операция. В качестве примера можно привести базы данных, используемые для разработки или тестирования, или небольшие редко используемые приложения. |
| **Стандартный** |Оптимальный вариант для облачных приложений с низкими или средними требованиями к производительности операций ввода-вывода, поддерживающий несколько параллельных запросов. В качестве примера можно привести рабочую группу или веб-приложения. |
| **Премиальный** | Разработан для большого объема транзакций с высокими требованиями к производительности операций ввода-вывода. Поддерживается параллельная работа множества пользователей. В качестве примера можно привести базы данных для поддержки критически важных приложений. |

Сначала определите, что вам нужно: запустить отдельную базу данных или сгруппировать базы данных, которые совместно используют ресурсы. Просмотрите [рекомендации по пулам эластичных БД](sql-database-elastic-pool-guidance.md). Чтобы выбрать уровень службы, сначала определите минимальный набор необходимых функций базы данных:

* максимальный размер отдельной базы данных: (до 2 ГБ для уровня "Базовый", до 250 ГБ для уровня "Стандартный" и от 500 ГБ до 1 ТБ для уровня "Премиум" при максимальной производительности);
* максимальный общий объем хранилища при использовании пула эластичных БД (117 ГБ для уровня "Базовый", 1200 ГБ для уровня "Стандартный" и 750 ГБ для уровня "Премиум");
* максимальное число баз данных в пуле (400 для уровня "Базовый", 400 для уровня "Стандартный" и 50 для уровня "Премиум");
* период хранения резервной копии базы данных (7 дней для уровня "Базовый", 35 дней для уровней "Стандартный" и "Премиум").

Выбрав минимальный уровень службы, переходите к определению уровня производительности для базы данных (в единицах DTU). В большинстве случаев для начала достаточно стандартных уровней производительности S2 и S3. Для баз данных с высокими требованиями к ЦП или операциям ввода-вывода рекомендуется начинать с уровней производительности "Премиум". На уровне "Премиум" предоставляется больше ресурсов ЦП и минимум в 10 раз больше операций ввода-вывода по сравнению с самой высокой производительностью на уровне "Стандартный".

Выбрав уровень производительности, вы сможете затем динамически увеличивать или уменьшать масштаб [отдельной базы данных](sql-database-scale-up.md) либо [пула эластичных БД](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) в процессе фактической работы. При миграции можно также использовать [калькулятор DTU](http://dtucalculator.azurewebsites.net/), чтобы рассчитать приблизительное число необходимых единиц DTU. 

>
> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Уровни служб для отдельной базы данных и уровни производительности
Для отдельных баз данных существует несколько уровней производительности в пределах каждого уровня обслуживания. У вас есть возможность выбирать уровень, который лучше всего соответствует требованиям вашей рабочей нагрузки. Если вам нужно увеличить или уменьшить масштаб, вы легко можете изменить уровни базы данных. Дополнительные сведения см. в статье [Изменение уровней служб и уровней производительности базы данных](sql-database-scale-up.md).

Вне зависимости от количества размещаемых баз данных, вашей базе данных будет выделен гарантированный набор ресурсов. Кроме того, ожидаемые показатели производительности этой базы данных останутся без изменений.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Подробное описание всех строк этой таблицы уровней служб см. в разделе [Возможности и ограничения уровней служб](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Уровни службы пула эластичных баз данных и производительность в единицах eDTU

Благодаря пулам базы данных могут иметь общий доступ к ресурсам eDTU и совместно использовать их. При этом каждой базе данных в пуле не нужно назначать определенный уровень производительности. Например, отдельная база данных в пуле с уровнем обслуживания "Стандартный" может использовать от 0 до максимального количества единиц eDTU базы данных, которое вы задали в настройках пула. Пулы позволяют нескольким базам данных с меняющейся рабочей нагрузкой эффективно использовать ресурсы eDTU, доступные всему пулу. Дополнительные сведения см. в статье о [ценах и рекомендациях по производительности пула эластичных баз данных](sql-database-elastic-pool-guidance.md).

В следующей таблице описаны характеристики уровней обслуживания пула.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Каждая база данных в пуле также соответствует характеристикам отдельной базы данных для этого пула. Например, для пулов уровня "Базовый" установлено предельное количество сеансов (4800–28 800 на пул), но для отдельных баз данных в этом пуле установлено ограничение в 300 сеансов на базу данных.


## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь с подробными сведениями о [пулах эластичных БД](sql-database-elastic-pool-guidance.md) и [целесообразности их использования](sql-database-elastic-pool-guidance.md).
* Узнайте, как [отслеживать пулы эластичных баз данных, управлять ими и изменять их размеры](sql-database-elastic-pool-manage-portal.md), а также как [отслеживать производительность отдельных баз данных](sql-database-single-database-monitor.md).
* Теперь, когда вы узнали об уровнях базы данных SQL, попробуйте [создать свою первую базу данных SQL](sql-database-get-started.md) с помощью [бесплатной учетной записи](https://azure.microsoft.com/pricing/free-trial/).




<!--HONumber=Dec16_HO3-->


