---
title: "Руководство. Создание конвейера с действием копирования с помощью Visual Studio | Документация Майкрософт"
description: "С помощью этого руководства вы, используя Visual Studio, создадите конвейер фабрики данных Azure с действием копирования."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 01a6f060e6ae800b0de930c7c46ed60f73b530ac
ms.openlocfilehash: 58aae152e49a4e90822f98c9cf5ee7aad067ffa8


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Руководство. Создание конвейера с действием копирования с помощью Visual Studio
> [!div class="op_single_selector"]
> * [Обзор и предварительные требования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Мастер копирования](data-factory-copy-data-wizard-tutorial.md)
> * [Портал Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Шаблон Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [ИНТЕРФЕЙС REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

В этом руководстве рассматривается создание и мониторинг фабрики данных Azure с помощью Visual Studio. Конвейер в фабрике данных копирует данные из хранилища BLOB-объектов Azure в базу данных SQL Azure с помощью действия копирования.

Ниже приведены шаги, которые вы выполните в процессе работы с этим руководством.

1. Создание двух связанных служб: **AzureStorageLinkedService1** и **AzureSqlinkedService1**. 
   
    Связанная служба AzureStorageLinkedService1 связывает хранилище Azure, а служба AzureSqlLinkedService1 связывает базу данных SQL Azure с фабрикой данных **ADFTutorialDataFactoryVS**. Входные данные для конвейера размещены в хранилище больших двоичных объектов Azure, а выходные данные сохраняются в таблице в базе данных SQL Azure. Таким образом, можно добавить эти два хранилища данных как связанные службы в фабрику данных.
2. Создание двух наборов данных, **InputDataset** и **OutputDataset**, представляющих входные и выходные данные, которые размещаются в хранилищах данных. 
   
    Для набора данных InputDataset нужно указать контейнер BLOB-объектов, в котором находится большой двоичный объект с исходными данными. Для набора данных OutputDataset нужно указать таблицу SQL, в которой хранятся выходные данные. Нужно указать и другие свойства, такие как структура, доступность данных и политика.
3. Создание конвейера с именем **ADFTutorialPipeline** в ADFTutorialDataFactoryVS. 
   
    Для конвейера назначается **действие копирования**, которое копирует входные данные большого двоичного объекта Azure в выходную таблицу SQL Azure. Действие копирования перемещает данные в фабрике данных Azure. Это действие выполняется с помощью глобально доступной службы, обеспечивающей безопасное, надежное и масштабируемое копирование данных между разными хранилищами. Дополнительные сведения о действии копирования см. в статье [Перемещение данных с помощью действия копирования](data-factory-data-movement-activities.md). 
4. Создание фабрики данных с именем **VSTutorialFactory**. Развертывание фабрики данных и всех сущностей фабрики данных, таких как связанные службы, таблицы и конвейеры.    

## <a name="prerequisites"></a>Предварительные требования
1. Прочтите [обзорную статью](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) и выполните **предварительные требования** . 
2. Чтобы опубликовать сущности фабрики данных в фабрике данных Azure, необходимо **обладать правами администратора подписки Azure** .  
3. На вашем компьютере должны быть установлены следующие компоненты: 
   * Visual Studio 2013 или Visual Studio 2015.
   * Загрузите пакет SDK Azure для Visual Studio 2013 или Visual Studio 2015. Перейдите на [cтраницу загрузки Azure](https://azure.microsoft.com/downloads/) и щелкните **VS 2013** или **VS2015** в разделе **.NET**.
   * Скачайте последнюю версию подключаемого модуля фабрики данных Azure для Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) или [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Вы также можете обновить подключаемый модуль, выполнив следующие действия. В меню выберите **Сервис** -> **Расширения и обновления** -> **В сети** -> **Галерея Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio**(Средства фабрики данных Microsoft Azure для Visual Studio) -> **Обновить**.

## <a name="create-visual-studio-project"></a>Создание проекта Visual Studio
1. Запустите **Visual Studio 2013**. Щелкните **Файл**, наведите указатель мыши на пункт **Создать** и щелкните **Проект**. Откроется диалоговое окно **Новый проект** .  
2. В диалоговом окне **Новый проект** выберите шаблон **DataFactory** и щелкните **Empty Data Factory Project** (Пустой проект фабрики данных). Если вы не видите шаблон DataFactory, закройте Visual Studio, установите пакет SDK Azure для Visual Studio 2013 и снова откройте Visual Studio.  
   
    ![Диалоговое окно "Новый проект"](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Введите **имя** проекта, **расположение** и имя **решения**, а затем нажмите кнопку **ОК**.
   
    ![Обозреватель решений](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Создание связанных служб
Связанные службы связывают хранилища данных или службы вычислений с фабрикой данных Azure. См. список [поддерживаемых хранилищ данных](data-factory-data-movement-activities.md#supported-data-stores-and-formats) для всех источников и приемников, которые поддерживаются действием копирования. См. список [связанных служб вычислений](data-factory-compute-linked-services.md), поддерживаемых фабрикой данных. В этом руководстве не рассматривается использование служб вычислений. 

На этом шаге создаются две связанные службы: **AzureStorageLinkedService1** и **AzureSqlLinkedService1**. Связанная служба AzureStorageLinkedService1 связывает учетную запись хранения Azure, а служба AzureSqlLinkedService связывает базу данных SQL Azure с фабрикой данных **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Создание связанных служб хранилища Azure
1. Щелкните правой кнопкой мыши **Связанные службы** в обозревателе решений, наведите указатель мыши на команду **Добавить** и выберите **Новый элемент**.      
2. В диалоговом окне **Добавление нового элемента** выберите в списке пункт **Azure Storage Linked Service** (Связанная служба хранилища Azure) и нажмите кнопку **Добавить**. 
   
    ![Новая связанная служба](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Замените `<accountname>` и `<accountkey>`* именем и ключом учетной записи хранения Azure. 
   
    ![Связанная служба хранилища Azure](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Сохраните файл **AzureStorageLinkedService1.json** .

> Дополнительные сведения о свойствах файлов JSON см. в статье [Перемещение данных в большой двоичный объект Azure и из него с помощью фабрики данных Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service).
> 
> 

### <a name="create-the-azure-sql-linked-service"></a>Создание связанной службы SQL Azure
1. Щелкните правой кнопкой мыши узел **Связанные службы** в **обозревателе решений**, выберите **Добавить** и щелкните **Новый элемент**. 
2. На этот раз выберите **Azure SQL Linked Service** (Связанная служба SQL Azure) и нажмите кнопку **Добавить**. 
3. В файле **AzureSqlLinkedService1.json** замените `<servername>`, `<databasename>`, `<username@servername>` и `<password>` именем сервера Azure SQL Server, именем базы данных, именем учетной записи пользователя и паролем соответственно.    
4. Сохраните файл **AzureSqlLinkedService1.json** . 

> [!NOTE]
> Дополнительные сведения о свойствах файлов JSON см. в статье [Перемещение данных в базу данных SQL Azure и из нее с помощью фабрики данных Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-datasets"></a>Создание наборов данных
На предыдущем шаге вы создали связанные службы **AzureStorageLinkedService1** и **AzureSqlLinkedService1** для связи учетной записи хранения Azure и базы данных SQL Azure с фабрикой данных **ADFTutorialDataFactory**. На этом шаге будут определены два набора данных, **InputDataset** и **OutputDataset**, представляющие входные и выходные данные в хранилищах данных, на которые ссылаются службы AzureStorageLinkedService1 и AzureSqlLinkedService1 соответственно. Для набора данных InputDataset нужно указать контейнер BLOB-объектов, в котором находится большой двоичный объект с исходными данными. Для набора данных OutputDataset нужно указать таблицу SQL, в которой хранятся выходные данные.

### <a name="create-input-dataset"></a>Создание входного набора данных
На этом шаге создается набор данных с именем **InputDataset**. Он указывает на контейнер больших двоичных объектов в службе хранилища Azure, которая представлена связанной службой **AzureStorageLinkedService1**. Таблица — это прямоугольный набор данных. Это единственный тип набора данных, который сейчас поддерживается. 

1. Щелкните правой кнопкой мыши **Таблицы** в **обозревателе решений**, выберите **Добавить** и щелкните **Новый элемент**.
2. В диалоговом окне **Добавление нового элемента** выберите **BLOB-объект Azure** и нажмите кнопку **Добавить**.   
3. Замените текст JSON приведенным далее текстом и сохраните файл **AzureBlobLocation1.json** . 

  ```json   
  {
    "name": "InputDataset",
    "properties": {
      "structure": [
        {
          "name": "FirstName",
          "type": "String"
        },
        {
          "name": "LastName",
          "type": "String"
        }
      ],
      "type": "AzureBlob",
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
        "format": {
          "type": "TextFormat",
          "columnDelimiter": ","
        }
      },
      "external": true,
      "availability": {
        "frequency": "Hour",
        "interval": 1
      }
    }
  }
  ``` 
    Обратите внимание на следующие моменты. 
   
   * Для параметра **type** набора данных задано значение **AzureBlob**.
   * Для **linkedServiceName** задано значение **AzureStorageLinkedService**. Эта связанная служба была создана на шаге 2.
   * В качестве значения **folderPath** установлен контейнер **adftutorial**. Вы также можете указать имя большого двоичного объекта в папке, используя свойство **fileName** . Так как вы не указываете имя большого двоичного объекта, данные из всех больших двоичных объектов в контейнере считаются входными данными.  
   * Для **type** формата установлено значение **TextFormat**.
   * В этом текстовом файле содержатся два поля, **FirstName** и **LastName**, которые разделяются запятой (**columnDelimiter**).    
   * Параметр **availability** имеет значение **hourly** (параметру **frequency** присваивается значение **hour**, а параметру **interval** — значение **1**). Следовательно, служба фабрики данных будет искать входные данные в корневом каталоге указанного вами контейнера BLOB-объектов (**adftutorial**) каждый час. 
   
   Если параметр **fileName** для **входного** набора данных не задан, все файлы и большие двоичные объекты из входной папки (**folderPath**) считаются входными данными. Если указать fileName в JSON, только указанный файл или большой двоичный объект рассматриваются как входные данные.
   
   Если не указать **fileName** для **выходной таблицы**, то созданные в **folderPath** файлы получают имена в следующем формате: Data.&lt;Guid\&gt;.txt (например, Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).
   
   Чтобы динамически установить параметры **folderPath** и **fileName** на основе времени **SliceStart**, используйте свойство **partitionedBy**. В следующем примере folderPath использует год, месяц и день из SliceStart (время начала обработки среза), а в fileName используется время (часы) из SliceStart. Например, если срез создается для метки времени 2016-09-20T08:00:00, свойству folderName присваивается значение wikidatagateway/wikisampledataout/2016/09/20, а свойству fileName — значение 08.csv. 
  
    ```json   
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
    [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ```
            
> [!NOTE]
> Дополнительные сведения о свойствах файлов JSON см. в статье [Перемещение данных в большой двоичный объект Azure и из него с помощью фабрики данных Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
> 
> 

### <a name="create-output-dataset"></a>Создание выходного набора данных
На этом этапе мы создадим выходной набор данных с именем **OutputDataset**. Этот набор данных указывает на таблицу SQL в базе данных SQL Azure (представлена значением **AzureSqlLinkedService1**). 

1. Снова щелкните правой кнопкой мыши **Таблицы** в **обозревателе решений**, выберите **Добавить** и щелкните **Новый элемент**.
2. В диалоговом окне **Добавление нового элемента** выберите **SQL Azure ** и нажмите кнопку **Добавить**. 
3. Замените текст JSON приведенным далее кодом JSON и сохраните файл **AzureSqlTableLocation1.json** .

    ```json
    {
     "name": "OutputDataset",
     "properties": {
       "structure": [
         {
           "name": "FirstName",
           "type": "String"
         },
         {
           "name": "LastName",
           "type": "String"
         }
       ],
       "type": "AzureSqlTable",
       "linkedServiceName": "AzureSqlLinkedService1",
       "typeProperties": {
         "tableName": "emp"
       },
       "availability": {
         "frequency": "Hour",
         "interval": 1
       }
     }
    }
    ```
   
    Обратите внимание на следующие моменты. 
   
   * Для параметра **type** набора данных задано значение **AzureSQLTable**.
   * **linkedServiceName** имеет значение **AzureSqlLinkedService** (эта связанная служба была создана на шаге 2).
   * **tablename** имеет значение **emp**.
   * В таблице emp в базе данных есть три столбца: **ID**, **FirstName** и **LastName**. ID — это столбец для идентификаторов, поэтому здесь вам нужно указать только значения **FirstName** и **LastName**.
   * Параметр **availability** имеет значение **hourly** (параметру **frequency** присваивается значение **hour**, а параметру **interval** — значение **1**).  Служба фабрики данных каждый час создает срез выходных данных в таблице **emp** в базе данных SQL Azure.

> [!NOTE]
> Дополнительные сведения о свойствах файлов JSON см. в статье [Перемещение данных в базу данных SQL Azure и из нее с помощью фабрики данных Azure](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-pipeline"></a>Создание конвейера
На данный момент мы создали связанные службы ввода и вывода, а также таблицы. Теперь вы создадите конвейер с **действием копирования** для копирования данных из большого двоичного объекта Azure в базу данных SQL Azure. 

1. Щелкните правой кнопкой мыши **Конвейеры** в **обозревателе решений**, выберите **Добавить** и щелкните **Новый элемент**.  
2. В диалоговом окне **Добавление нового элемента** выберите **Конвейер копирования данных** и нажмите кнопку **Добавить**. 
3. Замените JSON приведенным далее кодом JSON и сохраните файл **CopyActivity1.json** .

    ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob to Azure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
             }
           ],
           "typeProperties": {
             "source": {
               "type": "BlobSource"
             },
             "sink": {
               "type": "SqlSink",
               "writeBatchSize": 10000,
               "writeBatchTimeout": "60:00:00"
             }
           },
           "Policy": {
             "concurrency": 1,
             "executionPriorityOrder": "NewestFirst",
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2015-07-12T00:00:00Z",
       "end": "2015-07-13T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
   Обратите внимание на следующие моменты.
   
   * В разделе действий доступно только одно действие, параметр **type** которого имеет значение **Copy**.
   * Для этого действия параметру input присвоено значение **InputDataset**, а параметру output — значение **OutputDataset**.
   * В разделе **typeProperties** в качестве типа источника указано **BlobSource**, а в качестве типа приемника — **SqlSink**.
   
   Замените значение свойства **start** текущей датой, а значение свойства **end** — датой следующего дня. Можно указать только часть даты и пропустить временную часть указанной даты и времени. Например, 2016-02-03, что эквивалентно 2016-02-03T00:00:00Z.
   
   Даты начала и окончания должны быть в [формате ISO](http://en.wikipedia.org/wiki/ISO_8601). Например, 2016-10-14T16:32:41Z. Время **окончания** указывать не обязательно, однако в этом примере мы будем его использовать. 
   
   Если не указать значение свойства **end**, оно вычисляется по формуле "**время начала + 48 часов**". Чтобы запустить конвейер в течение неопределенного срока, укажите значение **9999-09-09** в качестве значения свойства **end**.
   
   В примере выше получено 24 среза данных, так как они создаются каждый час.

## <a name="publishdeploy-data-factory-entities"></a>Публикация и развертывание сущностей фабрики данных
На этом этапе вы опубликуете созданные ранее сущности фабрики данных (связанные службы, наборы данных и конвейеры). Вы также укажете имя новой фабрики данных, которая будет создана для хранения этих сущностей.  

1. В обозревателе решений щелкните проект правой кнопкой мыши и выберите **Опубликовать**. 
2. Когда отобразится диалоговое окно **Вход в учетную запись Майкрософт**, введите данные учетной записи с подпиской Azure и нажмите кнопку **Войти**.
3. Вы должны увидеть следующее диалоговое окно:
   
   ![Диалоговое окно "Опубликовать"](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. На странице Configure data factory (Настройка фабрики данных) сделайте следующее: 
   
   1. Выберите **Создать новую фабрику данных** .
   2. Введите **VSTutorialFactory** в качестве **имени** фабрики данных.  
      
      > [!IMPORTANT]
      > Имя фабрики данных Azure должно быть глобально уникальным. Если во время публикации отобразится ошибка об имени фабрики данных, измените имя фабрики данных (например, на ваше_имяVSTutorialFactory) и повторите попытку публикации. Ознакомьтесь со статьей [Фабрика данных Azure — правила именования](data-factory-naming-rules.md) , чтобы узнать о правилах именования артефактов фабрики данных.        
      > 
      > 
   3. Выберите свою подписку в Azure в поле **Подписка** .
      
      > [!IMPORTANT]
      > Если подписки не отображаются, проверьте, выполнен ли вход с использованием учетной записи администратора или соадминистратора подписки.  
      > 
      > 
   4. Выберите **группу ресурсов** для создаваемой фабрики данных. 
   5. Выберите **регион** для фабрики данных. В раскрывающемся списке отображаются только те регионы, которые поддерживаются службой фабрики данных.
   6. Нажмите кнопку **Далее**, чтобы перейти на страницу **Publish Items** (Публикация элементов).
      
       ![Страница Configure data factory (Настройка фабрики данных)](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. На странице **Publish Items** (Публикация элементов) выберите все сущности фабрик данных и нажмите кнопку **Далее**, чтобы перейти на страницу **Сводка**.
   
   ![Страница Publish items (Публикация элементов)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Просмотрите сводку и нажмите кнопку **Далее** для запуска процесса развертывания и просмотра **состояния развертывания**.
   
   ![Страница "Сводка публикации"](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. На странице **Состояние развертывания** вы увидите состояние процесса развертывания. После завершения развертывания нажмите кнопку "Готово".
 
   ![Страница "Состояние развертывания"](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Обратите внимание на следующие моменты. 

* Если появится сообщение об ошибке**Подписка не зарегистрирована для использования пространства имен Microsoft.DataFactory**, выполните одно из следующих действий и повторите попытку публикации. 
  
  * В Azure PowerShell выполните следующую команду, чтобы зарегистрировать поставщик фабрики данных Azure: 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Чтобы убедиться, что поставщик фабрики данных зарегистрирован, выполните следующую команду: 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Войдите на [портал Azure](https://portal.azure.com) с использованием подписки Azure и откройте колонку фабрики данных или создайте на портале фабрику данных. Поставщик будет зарегистрирован автоматически.
* В будущем имя фабрики данных может быть зарегистрировано в качестве DNS-имени и, следовательно, стать отображаемым.

> [!IMPORTANT]
> Чтобы создать экземпляры фабрики данных, необходимо быть администратором или соадминистратором подписки Azure.
> 
> 

## <a name="summary"></a>Сводка
В этом учебнике вы создали фабрику данных Azure для копирования данных из большого двоичного объекта Azure в базу данных SQL Azure. Вы использовали Visual Studio для создания фабрики данных, связанных служб, наборов данных и конвейера. Вот обобщенные действия, которые вы выполнили в этом руководстве:  

1. Создание **фабрики данных Azure**.
2. Создание **связанных служб**.
   1. **Служба хранилища Azure** — связанная служба для связи с учетной записью хранения Azure, которая содержит входные данные.     
   2. **SQL Azure** — связанная служба для связи с базой данных SQL Azure, которая содержит выходные данные. 
3. Создание **наборов данных**, описывающих входные и выходные данные для конвейеров.
4. Создание **конвейера** с **BlobSource** в качестве источника и **SqlSink** в качестве приемника с помощью **действия копирования**. 

## <a name="use-server-explorer-to-view-data-factories"></a>Использование обозревателя серверов для просмотра фабрик данных
1. В **Visual Studio** щелкните **Вид** в меню и выберите **Обозреватель серверов**.
2. В окне обозревателя серверов разверните элементы **Azure** и **Фабрика данных**. Когда отобразится окно **Выполните вход в Visual Studio**, введите данные **учетной записи**, связанной с вашей подпиской Azure, и нажмите кнопку **Продолжить**. Введите **пароль** и нажмите кнопку **Войти**. Visual Studio пытается получить сведения обо всех фабриках данных Azure в подписке. В окне **Data Factory Task List** (Список задач фабрики данных) будет отображено состояние операции.

    ![Обозреватель серверов](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Чтобы создать проект Visual Studio на основе существующей фабрики данных, щелкните фабрику данных правой кнопкой мыши и выберите пункт «Экспорт фабрики данных в новый проект».

    ![Экспорт фабрики данных для проекта VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Обновление средств фабрик данных для Visual Studio
Чтобы обновить средства фабрики данных Azure для Visual Studio, сделайте следующее:

1. Щелкните в меню пункт **Сервис** и выберите **Расширения и обновления**. 
2. Выберите пункт **Обновления** слева, а затем — **Галерея Visual Studio**.
3. Выберите **Azure Data Factory tools for Visual Studio** (Средства фабрики данных Azure для Visual Studio) и нажмите кнопку **Обновить**. Если эта запись отсутствует, у вас уже установлена последняя версия средства. 

Инструкции по использованию портала Azure для мониторинга конвейера и наборов данных, созданных с помощью этого руководства, см. в разделе [Отслеживание конвейера](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline).

## <a name="see-also"></a>См. также
| Раздел | Описание |
|:--- |:--- |
| [Действия перемещения данных](data-factory-data-movement-activities.md) |В этой статье содержится информация о действии копирования, которое использовалось в этом руководстве. |
| [Планирование и исполнение с использованием фабрики данных](data-factory-scheduling-and-execution.md) |Здесь объясняются аспекты планирования и исполнения в модели приложений фабрики данных. |
| [Конвейеры](data-factory-create-pipelines.md) |Эта статья содержит сведения о конвейерах и действиях в фабрике данных Azure. |
| [Наборы данных](data-factory-create-datasets.md) |Эта статья поможет вам понять, что такое наборы данных в фабрике данных Azure. |
| [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md) |В этой статье описывается мониторинг и отладка конвейеров, а также управление ими с помощью приложения мониторинга и управления. |




<!--HONumber=Dec16_HO3-->


