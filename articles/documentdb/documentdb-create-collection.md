---
title: "Создание базы данных и коллекции DocumentDB | Документация Майкрософт"
description: "Узнайте, как создать базы данных NoSQL и коллекции документов JSON с помощью интерактивного портала служб Azure DocumentDB, облачной базы данных документов. Получите бесплатную пробную  версию сегодня."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b81ad2f6-df7f-4c6d-8ca9-f8a9982d647e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: f480b8155c7bee797f1fed0f80200eec500e95a2
ms.openlocfilehash: f8e0b05301b83223a7cb15d55be7001f3299213f


---
# <a name="how-to-create-a-documentdb-collection-and-database-using-the-azure-portal"></a>Создание коллекции и базы данных DocumentDB на портале Azure
Чтобы использовать Microsoft Azure DocumentDB, необходимы [учетная запись DocumentDB](documentdb-create-account.md), база данных, коллекция и документы. В этой статье описывается создание коллекции DocumentDB на портале Azure.

Не знаете, что такое коллекция? Обратитесь к статье [Что такое коллекция DocumentDB?](#what-is-a-documentdb-collection)

1. На [портале Azure](https://portal.azure.com/) на навигационной панели щелкните **DocumentDB (NoSQL)**, а затем в колонке **DocumentDB (NoSQL)** выберите учетную запись, в которую необходимо добавить коллекцию. При отсутствии учетных записей необходимо [создать учетную запись DocumentDB](documentdb-create-account.md).

   ![Снимок экрана, на котором изображены учетные записи DocumentDB на панели быстрых переходов, учетная запись в колонке "Учетные записи DocumentDB" и база данных в колонке учетной записи DocumentDB в группе связанных элементов "База данных"](./media/documentdb-create-collection/docdb-database-creation-1-2.png)

   Если служба **DocumentDB (NoSQL)** не отображается на навигационной панели, то щелкните **Больше служб**, а затем выберите **DocumentDB (NoSQL)**. При отсутствии учетных записей необходимо [создать учетную запись DocumentDB](documentdb-create-account.md).
2. В колонке **Учетная запись DocumentDB** для выбранной учетной записи щелкните **Добавить коллекцию**.

    ![Снимок экрана, на котором изображены учетные записи DocumentDB на панели быстрых переходов, учетная запись в колонке "Учетные записи DocumentDB" и база данных в колонке учетной записи DocumentDB в группе связанных элементов "База данных"](./media/documentdb-create-collection/docdb-database-creation-3.png)
3. В колонке **Добавить коллекцию** в поле **Идентификатор** введите идентификатор новой коллекции. Имя коллекции может иметь длину от 1 до 255 символов и не может содержать `/ \ # ?` или пробел. После проверки имени в поле идентификатора отображается зеленая галочка.

    ![Снимок экрана, на котором изображены кнопка "Добавить коллекцию" в колонке "База данных", параметры в колонке "Добавление коллекции" и кнопка OK — портал Azure для DocumentDB — инструмент создания облачных баз данных JSON для NoSQL](./media/documentdb-create-collection/docdb-collection-creation-5-8.png)
4. По умолчанию для параметра **Ценовая категория** задано значение **Стандартный**, поэтому вы можете настроить для коллекции пропускную способность и размер хранилища. Дополнительные сведения о ценовой категории см. в статье [Уровни производительности в DocumentDB](documentdb-performance-levels.md).  
5. Выберите **режим секционирования коллекции**: **Одна секция** или **Секционированный**.

    Для **одной секции** зарезервировано хранилище объемом 10 ГБ и предусмотрена пропускная способность от 400 до 10 000 единиц запроса в секунду (ЕЗ/с). Одна ЕЗ соответствует пропускной способности для операции чтения документа размером 1 КБ. Дополнительные сведения о единицах запроса см. в статье [Единицы запросов в DocumentDB](documentdb-request-units.md).

    **Секционированную коллекцию** можно масштабировать, получив в результате неограниченный объем хранилища (в нескольких секциях) и пропускную способность от 10 100 ЕЗ/с. На портале максимальный объем хранилища, который можно зарезервировать, равен 250 ГБ, а максимальная резервируемая пропускная способность — 250 000 ЕЗ/с. Чтобы увеличить любую из этих квот, отправьте запрос, как описано в статье [Запрос увеличения ограничения учетной записи DocumentDB](documentdb-increase-limits.md). Дополнительные сведения о секционированных коллекциях см. в разделе [Односекционные и секционированные коллекции](documentdb-partition-data.md#single-partition-and-partitioned-collections).

    По умолчанию для новой коллекции с одной секцией устанавливается пропускная способность 1000 ЕЗ/с и хранилище емкостью 10 ГБ. Для секционированной коллекции устанавливается пропускная способность 10 100 ЕЗ/с и хранилище емкостью 250 ГБ. Пропускную способность и хранилище можно изменить после создания коллекции.
6. При создании секционированной коллекции выберите для нее **ключ секции**. Для создания высокопроизводительной коллекции важно выбрать правильный ключ раздела. Дополнительные сведения о выборе ключа секции см. в разделе [Проектирование секционирования](documentdb-partition-data.md#designing-for-partitioning).
7. В колонке **База данных** создайте базу данных или используйте имеющуюся. Имя базы данных может иметь длину от 1 до 255 символов и не может содержать `/ \ # ?` или пробел. Чтобы проверить имя, щелкните вне текстового поля. После проверки имени в поле отображается зеленая галочка.
8. Нажмите **ОК** в нижней части экрана, чтобы создать коллекцию.
9. Новая коллекция теперь отображается в разделе **Коллекции** колонки **База данных**.

    ![Снимок экрана новой коллекции в колонке "База данных" — портал Azure для DocumentDB — инструмент создания облачных баз данных JSON для NoSQL](./media/documentdb-create-collection/docdb-collection-creation-9.png)
10. **Необязательно.** Чтобы изменить пропускную способность коллекции на портале, щелкните **Масштаб** в меню ресурсов.

    ![Снимок экрана с меню ресурсов, в котором выбран пункт "Масштаб"](./media/documentdb-create-collection/docdb-collection-creation-scale.png)

## <a name="what-is-a-documentdb-collection"></a>Что такое коллекция DocumentDB?
Коллекция представляет собой контейнер документов JSON и связанной логики приложения на JavaScript. Коллекция представляет собой тарифицируемый объект, и его [стоимость](documentdb-performance-levels.md) определяется уровнем подготовленной пропускной способности. Коллекции могут охватывать один или несколько разделов либо серверов и масштабироваться до практически неограниченных объемов хранилища или пропускной способности.

DocumentDB автоматически секционирует коллекции на один или несколько физических серверов. При создании коллекции можно указать подготовленную пропускную способность в единицах запросов в секунду и свойство ключа раздела. С помощью этого свойства служба DocumentDB будет распределять документы между разделами и перенаправлять запросы. Значение ключа раздела тоже выступает в качестве границы транзакции для хранимых процедур и триггеров. Каждой коллекции назначается зарезервированный объем пропускной способности, который не используется совместно с другими коллекциями в данной учетной записи. Таким образом, для приложения можно масштабировать как объем хранилища, так и пропускную способность.

Коллекции отличаются от таблиц в реляционных базах данных тем, Коллекции не используют схемы, на самом деле, DocumentDB не использует никаких схем, это база данных без схем. Поэтому вы можете хранить разные типы документов с разными схемами в одной коллекции. Вы можете использовать коллекции для хранения объектов одного типа, как и таблицы. Оптимальная модель зависит только от того, как данные совместно отображаются в запросах и транзакциях.

## <a name="other-ways-to-create-a-documentdb-collection"></a>Другие способы создания коллекции DocumentDB
Коллекции не обязательно создавать на портале — это можно сделать также с помощью [пакетов SDK для DocumentDB](documentdb-sdk-dotnet.md) и REST API.

* Пример кода C# см. в [примерах коллекции C#](documentdb-dotnet-samples.md#collection-examples).
* Пример кода Node.js см. в [примерах коллекции Node.js](documentdb-nodejs-samples.md#collection-examples).
* Пример кода Python см. в [примерах коллекции Python](documentdb-python-samples.md#collection-examples).
* Пример кода REST API см. в статье [Создание коллекции](https://msdn.microsoft.com/library/azure/mt489078.aspx).

## <a name="troubleshooting"></a>Устранение неполадок
Если элемент **Добавить коллекцию** отключен на портале Azure, это означает, что ваша учетная запись в настоящее время отключена. Обычно это происходит, когда все доступные кредиты за месяц истекли.    

## <a name="next-steps"></a>Дальнейшие действия
Следующий шаг после создания коллекция — добавление или импорт документов в коллекцию. Добавить документы в коллекцию можно несколькими способами.

* Вы можете [добавить документы](documentdb-view-json-document-explorer.md) с помощью обозревателя документов на портале.
* Вы можете [импортировать документы и данные](documentdb-import-data.md) с помощью средства миграции данных DocumentDB, которое позволяет импортировать JSON- и CSV-файлы, а также данные из SQL Server, MongoDB, табличного хранилища Azure и других коллекций DocumentDB.
* Либо можно добавить документы с помощью одного из [пакетов SDK для DocumentDB](documentdb-sdk-dotnet.md). DocumentDB содержит пакеты SDK для .NET, Java, Python, Node.js и JavaScript API. Примеры кода C#, в которых показано, как работать с документами с помощью пакета SDK .NET для DocumentDB, см. в [примерах документов C#](documentdb-dotnet-samples.md#document-examples). Примеры кода Node.js, в которых показано, как работать с документами с помощью пакета SDK Node.js для DocumentDB, см. в [примерах документов Node.js](documentdb-nodejs-samples.md#document-examples).

Добавив документы в коллекцию, [DocumentDB SQL](documentdb-sql-query.md) можно использовать для [выполнения запросов](documentdb-sql-query.md#executing-sql-queries) к документам с помощью [обозревателя запросов](documentdb-query-collections-query-explorer.md) на портале, [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) или одного из [пакетов SDK](documentdb-sdk-dotnet.md). 



<!--HONumber=Nov16_HO3-->


