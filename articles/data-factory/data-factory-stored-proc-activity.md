---
title: "Действие &quot;Хранимая процедура SQL Server&quot;"
description: "Узнайте, как с помощью действия хранимой процедуры SQL Server можно вызвать хранимую процедуру в Базе данных SQL Azure или хранилище данных SQL Azure из конвейера фабрики данных."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 1b2514e1e6f39bb3ce9d8a46f4af01835284cdcc
ms.openlocfilehash: 3510b0446cf3c1a7ffb3ff02a4d84d24ead1cae7


---
# <a name="sql-server-stored-procedure-activity"></a>Действие "Хранимая процедура SQL Server"
> [!div class="op_single_selector"]
> [Hive](data-factory-hive-activity.md)  
> [Pig](data-factory-pig-activity.md)  
> [MapReduce](data-factory-map-reduce.md)  
> [Потоковая передача Hadoop](data-factory-hadoop-streaming-activity.md)
> [Машинное обучение](data-factory-azure-ml-batch-execution-activity.md)
> [Хранимая процедура](data-factory-stored-proc-activity.md)
> [Сценарий U-SQL в Data Lake Analytics](data-factory-usql-activity.md)
> [Настраиваемое действие .NET](data-factory-use-custom-activities.md)
>
>

С помощью действия "Хранимая процедура SQL Server" в [конвейере](data-factory-create-pipelines.md) фабрики данных можно вызвать хранимую процедуру в одном из следующих хранилищ данных.

* База данных SQL Azure
* Хранилище данных SQL Azure  
* База данных SQL Server в вашей организации или на виртуальной машине Azure. Шлюз управления данными необходимо установить на том же компьютере, на котором размещена база данных, или на отдельном компьютере во избежание конкуренции за ресурсы с базой данных. Шлюз управления данными — это программное обеспечение, которое обеспечивает безопасное и управляемое подключение локальных источников данных и источников данных, расположенных на виртуальных машинах Azure, к облачным службам. Сведения о шлюзе управления данными см. в статье [Перемещение данных между локальными источниками и облаком](data-factory-move-data-between-onprem-and-cloud.md).

Данная статья основана на материалах статьи о [действиях преобразования данных](data-factory-data-transformation-activities.md) , в которой приведен общий обзор преобразования данных и список поддерживаемых действий преобразования.

## <a name="walkthrough"></a>Пошаговое руководство
### <a name="sample-table-and-stored-procedure"></a>Образец таблицы и хранимая процедура
1. Создайте следующую **таблицу** в базе данных SQL Azure с помощью SQL Server Management Studio или другого подходящего инструмента. Столбец datetimestamp содержит дату и время создания соответствующего идентификатора.

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Id — уникальный идентификатор, а столбец datetimestamp содержит дату и время создания соответствующего идентификатора.
    ![Пример данных](./media/data-factory-stored-proc-activity/sample-data.png)

   > [!NOTE]
   > В этом примере используется база данных SQL Azure, но работает она точно так же, как в случае с хранилищем данных SQL Azure и базой данных SQL Server.
   >
   >
2. Создайте следующую **хранимую процедуру**, вставляющую данные в таблицу **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS

        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

   > [!IMPORTANT]
   > **Имя** и **регистр** параметра (в этом примере — DateTime) должны соответствовать имени и регистру параметра, указанного в конвейере или действии JSON. Убедитесь, что в определении хранимой процедуры в качестве префикса для параметра используется символ **@** .
   >
   >

### <a name="create-a-data-factory"></a>Создать фабрику данных
1. Войдите на [портал Azure](https://portal.azure.com/).
2. Щелкните **Создать** в меню слева, выберите **Аналитика** и щелкните **Фабрика данных**.

    ![Новая фабрика данных](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. В колонке **Новая фабрика данных** введите **SProcDF** в поле "Имя". Имена фабрики данных Azure являются **глобально уникальными**. Для успешного создания фабрики добавьте свое имя в качестве префикса имени фабрики.

   ![Новая фабрика данных](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Выберите свою **подписку Azure**.
5. Для **группы ресурсов** выполните одно из следующих действий:
   1. Щелкните **Создать** и введите имя группы ресурсов.
   2. Щелкните **Использовать существующий** и укажите имеющуюся группу ресурсов.  
6. Укажите **расположение** фабрики данных.
7. Выберите **Закрепить на панели мониторинга**, чтобы при следующем входе фабрика данных отобразилась на панели мониторинга.
8. В колонке **Создание фабрики данных** нажмите кнопку **Создать**.
9. Созданная фабрика данных появится на **панели мониторинга** портала Azure. Ее содержимое отобразится на соответствующей странице.
   ![Домашняя страница фабрики данных](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Создание связанной службы SQL Azure
После создания фабрики данных вы создаете связанную службу SQL Azure, которая связывает вашу базу данных SQL Azure с фабрикой данных. Эта база данных содержит таблицу sampletable и хранимую процедуру sp_sample.

1. В колонке **Фабрика данных** щелкните **Создать и развернуть** для **SProcDF**, чтобы запустить редактор фабрики данных.
2. Щелкните **Новое хранилище данных** в командной строке и выберите **Azure SQL**. В редакторе отобразится сценарий JSON для создания связанной службы SQL Azure.

   ![Новое хранилище данных](media/data-factory-stored-proc-activity/new-data-store.png)
3. Внесите следующие изменения в сценарий JSON:

   1. Замените **&lt;serverName&gt;** именем сервера Базы данных SQL.
   2. Замените **&lt;databasename&gt;** базой данных, в которой создана таблица и хранимая процедура.
   3. Замените **&lt;username@servername&gt;** учетной записью пользователя, у которой есть доступ к базе данных.
   4. Замените **&lt;password&gt;** паролем к учетной записи пользователя.

      ![Новое хранилище данных](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Чтобы развернуть эту службу, нажмите кнопку **Развернуть** на панели команд. Экземпляр AzureSqlLinkedService должен отображаться в иерархическом представлении слева.

    ![иерархическое представление со связанными службами](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Создание выходного набора данных
1. Нажмите **... Дополнительно** на панели инструментов, щелкните **Новый набор данных** и выберите **Azure SQL**. **Новый набор данных** на панели команд и выберите **Azure SQL**.

    ![иерархическое представление со связанными службами](media/data-factory-stored-proc-activity/new-dataset.png)
2. Скопируйте и вставьте следующий скрипт JSON в редактор JSON.

        {                
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
3. Нажмите кнопку **Развернуть** на панели команд, чтобы развернуть набор данных. Набор данных должен отображаться в иерархическом представлении.

    ![иерархическое представление со связанными службами](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Создание конвейера с помощью действия SqlServerStoredProcedure
Теперь создадим конвейера с помощью действия SqlServerStoredProcedure.

1. Нажмите **... Дополнительно** на панели команд и выберите **Новый конвейер**.
2. Скопируйте и вставьте следующий фрагмент JSON. Для параметра **storedProcedureName** установлено значение **sp_sample**. Имя и регистр параметра **DateTime** должны совпадать с именем и регистром параметра в определении хранимой процедуры.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                 "start": "2016-08-02T00:00:00Z",
                 "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Если для параметра необходимо передать значение null, используйте синтаксис param1: null (все символы в нижнем регистре).
3. Щелкните **Развернуть** на панели инструментов для развертывания конвейера.  

### <a name="monitor-the-pipeline"></a>Мониторинг конвейера
1. Щелкните **X**, чтобы закрыть колонки редактора фабрики данных и вернуться в колонку фабрики данных. Затем щелкните **Схема**.

    ![плитка "Схема"](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. В **представлении схемы**вы увидите все конвейеры и наборы данных, используемые в этом руководстве.

    ![плитка "Схема"](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. В представлении схемы дважды щелкните набор данных **sprocsampleout**. Отобразятся срезы в состоянии "Готово". Должно отобразиться пять срезов, так как из JSON срез создается каждый час в периоде между временем начала и окончания.

    ![плитка "Схема"](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Если срез находится в состоянии **Готово**, выполните запрос **select ** from sampletable* к Базе данных SQL Azure, чтобы убедиться, что хранимая процедура вставила данные в таблицу.

   ![Выходные данные](./media/data-factory-stored-proc-activity/output.png)

   В разделе [Мониторинг конвейера](data-factory-monitor-manage-pipelines.md) представлены подробные сведения о мониторинге конвейеров фабрики данных Azure.  

> [!NOTE]
> В этом примере SprocActivitySample не содержит входных данных. Если требуется организовать это действие в цепочку с вышестоящим действием (то есть ранее выполнявшимся действием обработки), выходные данные вышестоящего действия можно использовать как входные данные в этом действии. В таком случае действие не будет выполнено, пока не завершится вышестоящее действие и не будут доступны выходные данные вышестоящих действий (в состоянии "Готово"). Входные данные нельзя использовать непосредственно в качестве параметра действия хранимой процедуры
>
>

## <a name="json-format"></a>Формат JSON
    {
        "name": "SQLSPROCActivity",
        "description": "description",
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>Свойства JSON
| Свойство | Описание | Обязательно |
| --- | --- | --- |
| name |Имя действия. |Да |
| Описание |Текст, описывающий, для чего используется действие |Нет |
| type |SqlServerStoredProcedure |Да |
| inputs |необязательный параметр. При указании входного набора данных он должен быть доступен (в состоянии "Готово") для выполнения действия хранимой процедуры. Входной набор данных не может потребляться в хранимой процедуре в качестве параметра. Он используется только для проверки зависимости перед запуском действия хранимой процедуры. |Нет |
| outputs |Для действия хранимой процедуры необходимо указать выходной набор данных. Выходной набор данных указывает **расписание** для действия хранимой процедуры (ежечасно, еженедельно, ежемесячно и т. д.). <br/><br/>Выходной набор данных должен использовать **связанную службу**, ссылающуюся на Базу данных SQL Azure, хранилище данных SQL Azure или базу данных SQL Server, в которых следует запускать хранимую процедуру. <br/><br/>Выходной набор данных может использоваться для передачи результатов хранимой процедуры для дальнейшей обработки с помощью другого действия ([цепочки действий](data-factory-scheduling-and-execution.md#run-activities-in-a-sequence)) в конвейере. Тем не менее фабрика данных не записывает выходные данные хранимой процедуры в этот набор данных автоматически. Выходные данные записывает сама хранимая процедура в таблицу SQL, на которую указывает выходной набор данных. <br/><br/>В некоторых случаях выходной набор данных может быть **фиктивным набором данных**. Он используется, только чтобы задать расписание выполнения действия хранимой процедуры. |Да |
| storedProcedureName |Укажите имя хранимой процедуры в базе данных SQL Azure или хранилище данных SQL Azure, представленной связанной службой, которую использует выходная таблица. |Да |
| storedProcedureParameters |Указываемые значения для параметров хранимой процедуры. Если для параметра необходимо передать значение null, используйте синтаксис param1: null (все символы в нижнем регистре). Чтобы научиться использовать это свойство, см. пример ниже. |Нет |

## <a name="passing-a-static-value"></a>Передача статического значения
Теперь давайте рассмотрим добавление другого столбца с именем "Scenario" в таблицу, содержащую статическое значение с именем "Document sample".

![Пример данных 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)

    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Теперь передайте параметр Scenario и значение из действия хранимой процедуры. Раздел typeProperties в предыдущем примере выглядит как следующий фрагмент кода.

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters":
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }



<!--HONumber=Nov16_HO3-->


