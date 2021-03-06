---
title: "Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных | Документация Майкрософт"
description: "С помощью этого учебника вы создадите конвейер фабрики данных Azure с действием копирования при помощи мастера копирования, поддерживаемого фабрикой данных."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 3205077236dd44253b3fa36d6eace36fb307871e
ms.openlocfilehash: 11754bbe534638d8321f509d7d82e025c667176c


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных
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

**Мастер копирования** фабрики данных Azure позволяет легко и быстро создать конвейер, реализующий сценарий приема и перемещения данных. Поэтому рекомендуется использовать мастер в качестве первого шага при создании примера конвейера для сценария перемещения данных. В этом руководстве показано, как создать фабрику данных Azure, запустить мастер копирования и выполнить шаги, чтобы предоставить сведения о сценарии приема и перемещения данных. После завершения работы в мастере он автоматически создаст конвейер с действием копирования, позволяющим перенести данные из хранилища BLOB-объектов Azure в базу данных SQL Azure. Дополнительные сведения о действии копирования см. в статье [Перемещение данных с помощью действия копирования](data-factory-data-movement-activities.md). 

> [!IMPORTANT]
> Прежде чем начать работу с этим руководством, изучите статью на вкладке [Обзор и предварительные требования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) и выполните все **необходимые действия**.
> 
> 

## <a name="create-data-factory"></a>Создание фабрики данных
На этом шаге с помощью портала Azure создается фабрика данных с именем **ADFTutorialDataFactory**.

1. Войдите на [портал Azure](https://portal.azure.com), нажмите кнопку **+ Создать** в верхнем левом углу, откройте раздел **аналитики** и щелкните **Фабрика данных**. 
   
   ![Создать -> Фабрика данных](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. В колонке **Создать фабрику данных** выполните следующие действия.
   
   1. Введите **ADFTutorialDataFactory** в поле **Имя**.
       Имя фабрики данных Azure должно быть глобально уникальным. При возникновении ошибки **Имя фабрики данных ADFTutorialDataFactory недоступно**измените имя этой фабрики данных (например, на вашеимяADFTutorialDataFactory) и попробуйте создать ее снова. Ознакомьтесь с разделом [Фабрика данных — правила именования](data-factory-naming-rules.md) , чтобы узнать о правилах именования артефактов фабрики данных.  
      
       ![Имя фабрики данных недоступно](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
      
      > [!NOTE]
      > В будущем имя фабрики данных может быть зарегистрировано в качестве DNS-имени и, следовательно, стать отображаемым.
      > 
      > 
   2. Выберите свою **подписку Azure**.
   3. Для группы ресурсов выполните одно из следующих действий. 
      
      - а) выберите **Использовать существующую** и укажите имеющуюся группу ресурсов;
      - выберите **Создать** и введите имя для группы ресурсов.
         
          Некоторые действия, описанные в этом руководстве, предполагают, что для группы ресурсов используется имя **ADFTutorialResourceGroup**. Сведения о группах ресурсов см. в статье, где описывается [использование групп ресурсов для управления ресурсами Azure](../azure-resource-manager/resource-group-overview.md).
   4. Укажите **расположение** фабрики данных.
   5. Установите флажок **Закрепить на панели мониторинга** в нижней части колонки.  
   6. Щелкните **Создать**.
      
       ![Создать колонку "Фабрика данных"](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. После создания вы увидите колонку **Фабрика данных**, как показано на рисунке ниже.
   
   ![Домашняя страница фабрики данных](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>Запуск мастера копирования
1. Чтобы запустить **мастер копирования**, на домашней странице фабрики данных щелкните **Копирование данных**. 
   
   > [!NOTE]
   > Если веб-браузер завис на действии "Авторизация...", отключите параметр или снимите флажок **Block third party cookies and site data** (Блокировать сторонние файлы cookie и данные сайта). Либо оставьте флажок и создайте исключение для адреса **login.microsoftonline.com**, а затем попробуйте запустить мастер еще раз.
   > 
   > 
2. Вот что нужно сделать на странице **Свойства** :
   
   1. В качестве **имени задачи** введите **CopyFromBlobToAzureSql**.
   2. Введите **описание** (необязательно).
   3. Измените **дату и время начала** и **дату и время завершения** таким образом, чтобы датой завершения был сегодняшний день, а дата начала была на пять дней раньше текущей даты.  
   4. Нажмите кнопку **Далее**.  
      
      ![Средство копирования — страница "Свойства"](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. На странице **Source data store** (Хранилище данных источников) щелкните плитку **Хранилище BLOB-объектов Azure**. На этой странице можно указать хранилище данных источников для задачи копирования. Вы можете использовать существующую связанную службу хранилища данных или указать новое хранилище данных. Чтобы использовать существующую связанную службу, щелкните элемент **FROM EXISTING LINKED SERVICES** (Из существующих связанных служб) и выберите нужную службу. 
   
    ![Средство копирования — страница "Хранилище данных источников"](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. Вот что нужно сделать на странице **Specify the Azure Blob storage account** (Указание учетной записи хранилища BLOB-объектов Azure).
   
   1. В качестве **имени связанной службы** введите **AzureStorageLinkedService**.
   2. Убедитесь, что в качестве **метода выбора учетной записи** выбран вариант **From Azure subscriptions** (Из подписок Azure).
   3. Выберите свою **подписку Azure**.  
   4. Выберите **учетную запись хранения Azure** из списка учетных записей хранения Azure, доступных в выбранной вами подписке. Кроме того, вы можете вручную настроить параметры учетной записи хранения. Для этого в качестве **метода выбора учетной записи** выберите вариант **Ввести вручную** и нажмите кнопку **Далее**. 
      
      ![Средство копирования — указание учетной записи хранилища BLOB-объектов Azure](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. Вот что нужно сделать на странице **Choose the input file or folder** (Выбор входного файла или папки).
   
   1. Перейдите к папке **adftutorial** .
   2. Выберите **emp.txt** и нажмите кнопку **Выбрать**.
   3. Нажмите кнопку **Далее**. 
      
      ![Средство копирования — выбор входного файла или папки](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. На странице **Choose the input file or folder** (Выбор входного файла или папки) нажмите кнопку **Далее**. Не устанавливайте флажок **Binary copy**(Двоичное копирование). 
   
    ![Средство копирования — выбор входного файла или папки](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. На странице **File format settings** (Параметры формата файла) отображаются разделители и схема, которые мастер определяет автоматически при анализе файла. Можно также ввести разделители вручную, чтобы остановить автоопределение с помощью мастера копирования или переопределить. Проверив разделители и просмотрев предварительные данные, нажмите кнопку **Далее**. 
   
    ![Средство копирования — параметры формата файла](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. На странице целевого хранилища данных выберите **База данных SQL Azure** и нажмите кнопку **Далее**.
   
    ![Средство копирования — выбор целевого хранилища](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Вот что нужно сделать на странице **Specify the Azure SQL database** (Указание базы данных SQL Azure).
   
   1. В поле **Имя подключения** введите **AzureSqlLinkedService**.
   2. В качестве **метода выбора базы данных или сервера** должен быть выбран вариант **From Azure subscriptions** (Из подписок Azure).
   3. Выберите свою **подписку Azure**.  
   4. Выберите **имя сервера** и **базу данных**.
   5. Введите **имя пользователя** и **пароль**.
   6. Нажмите кнопку **Далее**.  
      
      ![Средство копирования — указание базы данных SQL Azure](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. На странице **Сопоставление таблицы** выберите **emp** в поле **Назначение** раскрывающегося списка, щелкните **стрелку вниз** (необязательно), чтобы отобразить схему и просмотреть данные.
    
     ![Средство копирования — сопоставление таблиц](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. На странице **Сопоставление схемы** нажмите кнопку **Далее**.
    
    ![Средство копирования — сопоставление схем](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. На странице **Performance settings** (Параметры производительности) нажмите кнопку **Далее**. 
    
    ![Средство копирования — параметры производительности](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Просмотрите сведения на странице **Сводка** и нажмите кнопку **Готово**. Мастер создаст две связанные службы, два набора данных (входной и выходной) и один конвейер в фабрике данных (из которой вы запустили мастер копирования). 
    
    ![Средство копирования — параметры производительности](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Запуск приложения для отслеживания и управления
1. На странице **развертывания** щелкните ссылку: `Click here to monitor copy pipeline`.
   
   ![Средство копирования — развертывание выполнено](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. Чтобы получить сведения о способах отслеживания созданного вами конвейера, изучите инструкции в статье [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md) . Щелкните значок **обновления** в списке **Окна действий**, чтобы просмотреть срез. 
   
   ![Приложение мониторинга](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
   
   > [!NOTE]
   > Щелкните кнопку **Обновить** внизу списка **Окна действий**, чтобы увидеть последнее состояние. Этот список не обновляется автоматически. 
   > 
   > 

## <a name="see-also"></a>См. также
| Раздел | Описание |
|:--- |:--- |
| [Действия перемещения данных](data-factory-data-movement-activities.md) |В этой статье содержится информация о действии копирования, которое использовалось в этом руководстве. |
| [Планирование и исполнение с использованием фабрики данных](data-factory-scheduling-and-execution.md) |Здесь объясняются аспекты планирования и исполнения в модели приложений фабрики данных. |
| [Конвейеры](data-factory-create-pipelines.md) |Эта статья поможет вам понять сущность конвейеров и действий в фабрике данных Azure, а также научиться с их помощью создавать комплексные рабочие процессы, управляемые данными, для конкретных бизнес-сценариев. |
| [Наборы данных](data-factory-create-datasets.md) |Эта статья поможет вам понять, что такое наборы данных в фабрике данных Azure. |
| [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md) |В этой статье описывается мониторинг и отладка конвейеров, а также управление ими с помощью приложения мониторинга и управления. |




<!--HONumber=Dec16_HO1-->


