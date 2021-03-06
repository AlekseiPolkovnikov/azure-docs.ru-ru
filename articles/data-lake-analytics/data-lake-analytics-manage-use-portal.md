---
title: "Управление Azure Data Lake Analytics с помощью портала Azure | Документация Майкрософт"
description: "Узнайте, как управлять учетными записями аналитики озера данных, источниками данных, пользователями и заданиями."
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/06/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 289f6e91458a1a4799941ccea46f7ee6d1a296c4


---
# <a name="manage-azure-data-lake-analytics-using-azure-portal"></a>Управление аналитикой озера данных Azure с помощью портала Azure
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Узнайте, как управлять учетными записями, источниками данных учетной записи, пользователями и заданиями Azure Data Lake Analytics с помощью портала Azure. Для просмотра разделов, посвященных управлению с помощью других инструментов, воспользуйтесь выбором вкладок вверху страницы.

**Предварительные требования**

Перед началом работы с этим учебником необходимо иметь следующее:

* **Подписка Azure**. Ознакомьтесь с [бесплатной пробной версией Azure](https://azure.microsoft.com/pricing/free-trial/).

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Управление учетными записями
Перед выполнением любого задания аналитики озера данных необходимо иметь учетную запись аналитики озера данных. В отличие от Azure HDInsight учетная запись Data Lake Analytics оплачивается только при выполнении задания.  Вы платите только за время, когда выполняется задание.  Дополнительные сведения см. в разделе [Обзор аналитики озера данных Azure](data-lake-analytics-overview.md).  

**Создание учетной записи аналитики озера данных**

1. Выполните вход на [портал Azure](https://portal.azure.com).
2. Выберите **Создать**, **Аналитика**, а затем — **Data Lake Analytics**.
3. Введите или выберите следующие значения:
   
    ![Аналитика озера данных Azure: колонка на портале](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)
   
   * **Имя**: имя учетной записи Data Lake Analytics.
   * **Подписка**: выберите подписку Azure, которая используется для учетной записи аналитики.
   * **Группа ресурсов**: выберите существующую группу ресурсов Azure или создайте новую группу. Обычно приложения состоят из множества компонентов, например веб-приложения, базы данных, сервера базы данных, хранилища и служб сторонних поставщиков. Диспетчер ресурсов Azure позволяет вам работать с ресурсами в приложении в виде группы. в статье [Обзор диспетчера ресурсов Azure](../azure-resource-manager/resource-group-overview.md). 
   * **Расположение**. выберите центр обработки данных Azure для учетной записи аналитики озера данных. 
   * **Хранилище озера данных**: каждая учетная запись аналитики озера данных имеет зависимую учетную запись хранения озера данных. Учетная запись аналитики озера данных и зависимая учетная запись хранения озера данных должны находиться в одном центре обработки данных Azure. Следуйте инструкциям для создания новой учетной записи хранения озера данных или выберите существующую.
4. Щелкните **Создать**. Откроется начальный экран портала. На начальную панель добавляется новый элемент с меткой «Развертывание аналитики озера данных Azure». Для создания учетной записи аналитики озера данных может потребоваться некоторое время. После создания учетной записи она открывается в новой колонке.

После создания учетной записи аналитики озера данных можно добавить дополнительные учетные записи хранения озера данных и учетные записи хранения Azure. Инструкции см. в разделе [Управление источниками данных учетной записи](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

<a name="access-adla-account"></a> **Доступ и открытие учетной записи аналитики озера данных**

1. Выполните вход на [портал Azure](https://portal.azure.com/).
2. В меню слева щелкните **Data Lake Analytics**.  Если вы не видите этот пункт, то выберите **Больше служб** и щелкните **Data Lake Analytics** в категории **Аналитика**.
3. Выберите учетную запись аналитики озера данных, к которой необходимо получить доступ. Откроется учетная запись в новой колонке.

**Удаление учетной записи аналитики озера данных**

1. Выберите учетную запись аналитики озера данных, которую необходимо удалить. Инструкции см. в [этом разделе](#access-adla-account).
2. Щелкните **Удалить** из меню кнопок в верхней части колонки.
3. Введите имя учетной записи и нажмите кнопку **Удалить**.

Удаление учетной записи Data Lake Analytics не приведет к удалению зависимых учетных записей Data Lake Store. Инструкции по удалению учетных записей Data Lake Store см. в [этом разделе](../data-lake-store/data-lake-store-get-started-portal.md#delete-azure-data-lake-store-account).

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Управление источниками данных учетной записи
Аналитика озера данных в настоящее время поддерживает следующие источники данных:

* [Хранилище озера данных Azure](../data-lake-store/data-lake-store-overview.md)
* [Хранилище Azure](../storage/storage-introduction.md)

При создании учетной записи аналитики озера данных необходимо указать учетную запись хранения озера данных Azure в качестве учетной записи хранения по умолчанию. Учетная запись хранения озера данных по умолчанию используется для хранения метаданных задания и журналов аудита задания. После создания учетной записи аналитики озера данных можно добавить дополнительные учетные записи хранения озера данных и учетные записи хранения Azure. 

<a name="default-adl-account"></a>**Поиск учетной записи хранения Data Lake по умолчанию**

* Выберите учетную запись аналитики озера данных, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account). Data Lake Store по умолчанию отображается в **Основных компонентах**:
  
    ![Azure Data Lake Analytics: добавление источника данных](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-default-adl-storage-account.png)

**Добавление дополнительных источников данных**

1. Выберите учетную запись аналитики озера данных, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account).
2. Щелкните **Параметры**, а затем нажмите кнопку **Источники данных**. Здесь вы увидите учетную запись хранения озера данных по умолчанию. 
3. Щелкните **Добавить источник данных**.
   
    ![Аналитика озера данных Azure: добавление источника данных](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-add-data-source.png)
   
    Чтобы добавить учетную запись Azure Data Lake Store, требуется имя учетной записи и доступ к учетной записи, чтобы отправить ей запрос.
    Чтобы добавить хранилище BLOB-объектов Azure, требуется учетная запись хранения и ключ учетной записи, который можно найти, перейдя к учетной записи хранения на портале.

<a name="explore-data-sources"></a>**Просмотр источников данных**    

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account).
2. Щелкните **Параметры**, а затем нажмите кнопку **Обозреватель данных**. 
   
    ![Аналитика озера данных Azure: обозреватель данных](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-data-explorer.png)
3. Выберите учетную запись Data Lake Store, чтобы ее открыть.
   
    ![Аналитика озера данных Azure: обозреватель данных учетной записи хранения](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-adls.png)
   
    В каждой учетной записи хранения озера данных можно сделать следующее.
   
   * **Создать папку**: добавить новую папку.
   * **Передать**: передать файлы в учетную запись хранения с рабочей станции.
   * **Доступ**: настроить разрешения доступа.
   * **Переименовать папку**: переименовать папку.
   * **Свойства папки**: просмотреть свойства файла или папки, такие как путь WASB или WEBHDFS, время последнего изменения и т. д.
   * **Удалить папку**: удалить папку.

<a name="upload-data-to-adls"></a> **Передача файлов в учетную запись хранения озера данных**

1. На портале щелкните **Обзор** в меню слева, а затем щелкните **Data Lake Store**.
2. Выберите учетную запись хранения озера данных, в которую необходимо отправить данные. Сведения о поиске учетной записи Data Lake Storage по умолчанию см. [здесь](#default-adl-account).
3. Щелкните **Обозреватель данных** в верхнем меню.
4. Щелкните **Создать каталог** , чтобы создать новую папку, или щелкните имя папки для изменения папки.
5. Щелкните **Передать** в верхнем меню, чтобы отправить файл.

<a name="upload-data-to-wasb"></a> **Передача файлов в учетную запись хранения больших двоичных объектов Azure**

См. раздел [Отправка данных для заданий Hadoop в HDInsight](../hdinsight/hdinsight-upload-data.md).  Сведения относятся к аналитике озера данных.

## <a name="manage-users"></a>Управление пользователями
Аналитика озера данных использует контроль доступа на базе ролей в рамках Azure Active Directory. При создании учетной записи Data Lake Analytics к этой учетной записи добавляется роль "Администраторы подписки". Можно добавить дополнительных пользователей и группы безопасности со следующими ролями:

| Роль | Описание |
| --- | --- |
| Владелец |Может управлять всем, включая доступ к ресурсам. |
| Участник |Может получать доступ к порталу, отправлять и отслеживать задания. Чтобы иметь возможность отправлять задания, участнику требуется разрешение на чтение или запись в учетные записи Data Lake Store. |
| Разработчик аналитики озера данных |Может отправлять, отслеживать и отменять задания.  Этот пользователь может отменять только свои собственные задания. Он не может управлять собственной учетной записью, например добавлять пользователей, изменять разрешения или удалить учетную запись. Чтобы иметь возможность выполнять задания, пользователю требуется разрешение на чтение или запись в учетные записи хранения озера данных. |
| читатель. |Может просматривать все элементы, но не вносить изменения. |
| Пользователь DevTest Labs |Может просматривать все, а также подключать, запускать, перезагружать виртуальные машины и завершать их работу. |
| Администратор доступа пользователей |Позволяет управлять доступом пользователей к ресурсам Azure. |

Сведения о создании пользователей и групп безопасности Azure Active Directory см. в разделе [Что такое Microsoft Azure Active Directory](../active-directory/active-directory-whatis.md).

**Добавление пользователей или групп безопасности к учетной записи Data Lake Analytics**

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account).
2. Щелкните **Параметры**, а затем нажмите кнопку **Пользователи**. Можно также щелкнуть **Доступ** в заголовке окна **Основные компоненты**, как показано на следующем снимке экрана:
   
    ![Аналитика озера данных Azure: добавление пользователей в учетную запись](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-access-button.png)
3. В колонке **Пользователи** щелкните **Добавить**.
4. Выберите роль и добавьте пользователя, затем нажмите кнопку **ОК**.

**Примечание. Если этому пользователю или группе безопасности требуется отправлять задания, им также необходимо предоставить разрешение для Data Lake Store. Дополнительные сведения см. в разделе [Защита данных, хранимых в хранилище озера данных Azure](../data-lake-store/data-lake-store-secure-data.md).**

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Управление заданиями
Для выполнения любых заданий U-SQL требуется учетная запись Data Lake Analytics.  Дополнительные сведения см. в разделе [Управление учетными записями Data Lake Analytics](#manage-data-lake-analytics-accounts).

<a name="create-job"></a>**Создание задания**

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account).
2. Нажмите кнопку **Создать задание**.
   
    ![Создание заданий U-SQL для аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-create-job-button.png)
   
    Появится новая колонка, похожая на эту:
   
    ![Создание заданий U-SQL для аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-new-job.png)
   
    Для каждого задания можно настроить следующее.

    |Имя|Описание|
    |----|-----------|
    |Имя задания|Введите имя задания.|
    |Приоритет|Меньшее число имеет более высокий приоритет. Если два задания поставлены в очередь, первым выполняется задание с более низким приоритетом.|
    |Параллелизм |Максимальное количество вычислительных процессов, которые могут выполняться одновременно. Увеличение этого числа может повысить производительность, но также может увеличить расходы.|
    |Скрипт|Введите сценарий U-SQL для задания.|

    Используя тот же интерфейс, можно также просматривать связанные источники данных и добавлять дополнительные файлы в связанные источники данных. 
1. Щелкните **Передать задание** , если вы хотите отправить задание.

**Отправка задания**

См. раздел [Создание задания](#create-job).

<a name="monitor-jobs"></a>**Мониторинг заданий**

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account). На панели управления заданиями отображаются базовые сведения о задании.
   
    ![Управление заданиями U-SQL для аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs.png)
2. Щелкните **Управление заданиями** , как показано на предыдущем снимке экрана.
   
    ![Управление заданиями U-SQL для аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs-details.png)
3. Выберите задание из списка. Или щелкните **Фильтр** , чтобы найти задания:
   
    ![Фильтрация заданий U-SQL для аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-filter-jobs.png)
   
    Задания можно фильтровать по **диапазону времени**, **имени задания** и **автору**.
4. Щелкните **Повторная отправка** , если вы хотите повторно отправить задание.

**Повторная отправка задания**

См. раздел [Мониторинг заданий](#monitor-jobs).

## <a name="monitor-account-usage"></a>Мониторинг использования учетной записи
**Мониторинг использования учетной записи**

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account). На панели «Использование» отображаются сведения об использовании:
   
    ![Мониторинг использования аналитики озера данных Azure](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-monitor-usage.png)
2. Дважды щелкните область для получения дополнительных сведений.

## <a name="view-u-sql-catalog"></a>Просмотр каталога U-SQL
[Каталог U-SQL](data-lake-analytics-use-u-sql-catalog.md) используется для структурирования данных и кода, чтобы их могли совместно использовать сценарии U-SQL. Каталог обеспечивает максимальную производительность, возможную с данными в озере данных Azure. На портале Azure вы можете просмотреть каталог U-SQL.

**Просмотр каталога U-SQL**

1. Выберите учетную запись аналитики, которой необходимо управлять. Инструкции см. в [этом разделе](#access-adla-account).
2. Щелкните **Обозреватель данных** в верхнем меню.
3. Разверните узлы **Каталог**, **мастер**, затем разверните **Таблицы, **Функции с табличным значением** или **Сборки**. На следующем снимке экрана представлена одна возвращающая табличное значение функция.
   
    ![Аналитика озера данных Azure: обозреватель данных учетной записи хранения](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-catalog.png)

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-azure-resource-manager-groups"></a>Использование групп диспетчера ресурсов Azure
Обычно приложения состоят из множества компонентов, например веб-приложения, базы данных, сервера базы данных, хранилища и служб сторонних поставщиков. Azure Resource Manager позволяет работать с ресурсами в приложении в виде группы, которая называется группой ресурсов Azure. Вы можете развертывать, обновлять, отслеживать или удалять все ресурсы для приложения в рамках одной скоординированной операции. Развертывание осуществляется на основе шаблона, используемого для разных сред, в том числе для тестовой, промежуточной и рабочей. Дополнительные сведения см. в статье [Обзор диспетчера ресурсов Azure](../azure-resource-manager/resource-group-overview.md). 

Служба аналитики озера данных может включать следующие компоненты:

* Учетная запись аналитики озера данных Azure
* Требуется учетная запись хранения озера данных Azure по умолчанию
* Дополнительные учетные записи хранения озера данных Azure
* Дополнительные учетные записи хранения Azure

Можно создать все эти компоненты в одной группе управления ресурсами, чтобы ими было проще управлять.

![Аналитика озера данных Azure: учетная запись и хранилище](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Учетная запись аналитики озера данных и зависимые учетные записи хранения должны находиться в одном центре обработки данных Azure.
Однако группа управления ресурсами может находиться в другом центре обработки данных.  

## <a name="see-also"></a>Дополнительные материалы
* [Обзор аналитики озера данных Microsoft Azure](data-lake-analytics-overview.md)
* [Начало работы с аналитикой озера данных с помощью портала Azure](data-lake-analytics-get-started-portal.md)
* [Управление аналитикой озера данных Azure с помощью Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Устранение неполадок с заданиями аналитики озера данных Azure с помощью портала Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)




<!--HONumber=Nov16_HO3-->


