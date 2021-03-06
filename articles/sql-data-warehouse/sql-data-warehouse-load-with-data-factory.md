---

title: "Загрузка данных в хранилище данных SQL Azure (фабрика данных) | Документация Майкрософт"
description: "В этом руководстве вы загрузите данные в хранилище данных SQL Azure, используя фабрику данных Azure и базу данных SQL Server в качестве источника данных."
services: sql-data-warehouse
documentationcenter: NA
author: linda33wj
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: jingwang;kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: b46ac604c10a2cb014991f3707fc3fc3b300f772
ms.openlocfilehash: 5b045ed236771919ea3f644a6b2351d54c75549b


---

# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Загрузка данных в хранилище данных SQL с помощью фабрики данных

В этом руководстве вы загрузите данные в хранилище данных SQL Azure, используя фабрику данных Azure и базу данных SQL Server в качестве источника данных. Таким образом вы сохраните данные в хранилище данных SQL.

**Оценка времени**. Для работы с этим руководством потребуется около 10–15 минут (при условии, что предварительные требования уже выполнены).

## <a name="prerequisites"></a>Предварительные требования

- Предполагается, что вы владеете основами языка Transact-SQL и умеете создавать таблицы и схемы.  

- Вам понадобится учетная запись хранения Azure. Вы можете [создать бесплатную учетную запись Azure](/pricing/free-trial/?WT.mc_id=A261C142F) или [активировать преимущества для подписчиков Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

- Вам понадобится хранилище данных SQL. Если у вас еще нет хранилища данных, создайте его, как описано в статье [Создание хранилища данных SQL Azure](sql-data-warehouse-get-started-provision.md). Чтобы оптимизировать производительность, разместите учетную запись хранения и хранилище данных в одном регионе Azure.

- Подготовьте хранилище данных для получения входящих данных. Для этого создайте одну или несколько схем таблиц. Создать скрипт для схем можно с помощью [служебной программы миграции хранилища данных SQL](sql-data-warehouse-migrate-migration-utility.md). 

## <a name="configure-a-new-data-factory"></a>Настройка новой фабрики данных
1. Войдите на [портал Azure][].
2. Найдите хранилище данных и щелкните, чтобы открыть его. 
3. В колонке **Свойства** щелкните **Загрузить данные > Фабрика данных Azure**.

    ![Запуск мастера загрузки данных](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Откроется диалоговое окно **Новая фабрика данных**. Укажите необходимые сведения или выберите существующую фабрику данных. Щелкните **Создать**.

5. В диалоговом окне **Выбор фабрики данных** параметр **Загрузить данные** выбран по умолчанию. Щелкните **Далее**, чтобы завершить создание фабрики данных. 

## <a name="configure-the-data-factory-properties"></a>Настройка свойств фабрики данных
Создав фабрику данных, переходите к настройке расписания загрузки данных. 

1. Выберите **Свойства** и укажите необходимые сведения.
2. В качестве **имени задачи** введите **DWLoadData-fromSQLServer**.
3. Нажмите кнопку **Далее**.

    ![Настройка расписания загрузки](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-data-factory-source-and-gateway"></a>Настройка источника фабрики данных и шлюза
Теперь для фабрики данных нужно указать локальную базу данных SQL Server для загрузки данных.

1. Щелкните **Источник**.
2. Выберите **SQL Server** в каталоге хранилищ данных, поддерживаемых в качестве источника данных, и нажмите кнопку **Далее**.

    ![Выбор источника SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

3. Откроется диалоговое окно **Выбор локальной базы данных SQL Server**. Заполните обязательные поля.

    - **Имя подключения**: укажите новое имя подключения.
    - **Имя сервера**: имя локального сервера SQL.
    - **Имя базы**: имя базы данных сервера SQL.
    - **Шифрование учетных данных**: нет. 
    - **Тип проверки подлинности**: выберите тип проверки подлинности, который вы используете.
    - **Имя пользователя** и **пароль**: введите имя и пароль для пользователя с правами на копирование данных.

4. В последнем поле нужно указать имя шлюза. Щелкните ссылку **Создать шлюз**, чтобы создать шлюз управления данными. Шлюз — это клиентский агент, который необходимо установить в локальной среде для копирования данных между локальными и облачными хранилищами данных. 

5. Откроется диалоговое окно **Создание шлюза**. В качестве имени введите **GatewayForDWLoading** и нажмите кнопку **Создать**.

6. Откроется диалоговое окно **Настройка шлюза**.  
    ![Запуск экспресс-установки](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

7. Щелкните ссылку **Launch express setup on this computer** (Запустить экспресс-установку на этом компьютере), чтобы скачать, установить и зарегистрировать шлюз на используемом компьютере. Ход выполнения отображается во всплывающем окне.

    Экспресс-установка по умолчанию выполняется с помощью Microsoft Edge и Internet Explorer. Если вы используете Google Chrome, сначала установите расширение ClickOnce из интернет-магазина Chrome. Или вы можете вручную скачать и установить шлюз, а затем воспользоваться ключом для регистрации.

8. Дождитесь завершения установки шлюза. После регистрации и подключения шлюза всплывающее окно закроется, а новый шлюз отобразится в поле шлюза. Нажмите кнопку **Далее**.

9. Затем вам нужно выбрать таблицы, из которых будут копироваться данные. Эти таблицы можно отфильтровать с помощью ключевых слов. Кроме того, на нижней панели доступен предварительный просмотр схемы таблиц и данных. Выбрав нужные элементы, нажмите кнопку **Далее**.

    ![Выбор таблиц](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a>Настройка целевого расположения — хранилища данных SQL

1. Щелкните **Назначение**. Сведения о подключении к хранилищу данных SQL будут заполнены автоматически.
2. Введите пароль для этого имени пользователя, а затем нажмите кнопку **Далее**.

    ![Настройка назначения](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

3. Отобразится интеллектуальное сопоставление таблиц, которое позволяет связать исходные и целевые таблицы на основе сходных имен. Добавьте
4.  сопоставление для любой таблицы — остальные сопоставятся автоматически на основе примера. 
5. Просмотрите результат и нажмите кнопку **Далее**.

    ![Сопоставление таблиц](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

6. Просмотрите сопоставление схем и проверьте наличие сообщений об ошибке. Интеллектуальное сопоставление основано на имени столбца. Если между исходным и целевым столбцами обнаружится преобразование неподдерживаемого типа данных, появится сообщение об ошибке с указанием на соответствующую таблицу.

    ![Сопоставление схем](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

7. Нажмите кнопку **Далее**.

## <a name="configure-the-performance-settings"></a>Настройка параметров производительности
В параметрах производительности настройте учетную запись хранения Azure для промежуточного хранения данных перед их загрузкой в хранилище данных SQL.

1. Щелкните **Производительность**. 
2. Выберите существующую учетную запись хранения Azure и нажмите кнопку **Далее**.

    ![Настройка промежуточного BLOB-объекта](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a>Просмотр сводных данных и развертывание конвейера

1. Щелкните **Сводка** и просмотрите сведения.
2. Нажмите кнопку **Готово**, чтобы развернуть конвейер.

    ![Развертывание фабрики данных](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Мониторинг загрузки данных

Когда развертывание завершится, в меню слева отобразится параметр **Развертывание**. 

1. Щелкните **Развертывание**.
2. Для мониторинга загрузки данных щелкните ссылку **Click here to monitor copy pipeline** (Щелкните здесь для мониторинга конвейера копирования).

    ![Отслеживание конвейера](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

3. В колонке **обозревателе ресурсов** разверните узлы конвейеров и щелкните созданный конвейер загрузки данных
4. **DWLoadData-fromSQL Server**.

    ![Представление конвейера](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

5. Щелкните конвейер, чтобы просмотреть подробные сведения о состоянии каждой таблицы, связанной с действием.

    ![Просмотр действия таблицы](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

6. Вы также можете щелкнуть действие, чтобы просмотреть данные о загрузке на панели справа, включая размер данных, строки, пропускную способность и т. д.

    ![Просмотр сведений о действии таблицы](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

7. Чтобы запустить это представление мониторинга позже, в хранилище данных SQL щелкните **Загрузить данные > Фабрика данных Azure**, выберите свою фабрику и щелкните **Monitor existing loading tasks** (Отслеживать существующие задачи загрузки).

## <a name="next-steps"></a>Дальнейшие действия

Инструкции по переносу базы данных в хранилище данных SQL см. в статье [Перенос решения в хранилище данных SQL](sql-data-warehouse-overview-migrate.md).

Дополнительные сведения о возможностях копирования фабрики данных Azure см. в статьях [Общие сведения о службе фабрики данных Azure, службе интеграции данных в облаке](../data-factory/data-factory-introduction.md) и [Перемещение данных с помощью действия копирования](../data-factory/data-factory-data-movement-activities.md).

Дополнительные сведения об использовании данных в хранилище данных SQL см. в статьях [Запросы к хранилищу данных SQL Azure (Visual Studio)](sql-data-warehouse-query-visual-studio.md) и [Визуализация данных с помощью Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[портал Azure]: https://portal.azure.com 


<!--HONumber=Nov16_HO4-->


