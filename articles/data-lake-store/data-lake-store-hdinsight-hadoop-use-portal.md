---
title: "Создание кластеров HDInsight с доступом к Azure Data Lake Store с помощью портала | Документация Майкрософт"
description: "Создание кластеров HDInsight для работы с хранилищем озера данных Azure с помощью портала Azure"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/18/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 3a9a4df2d260e8eebd8621b22efdb2927e9f5ecf
ms.openlocfilehash: a4fb47f9f517d66cf0ff9fde039d7bfd8edc29eb


---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-portal"></a>Создание кластера HDInsight с хранилищем озера данных с помощью портала Azure
> [!div class="op_single_selector"]
> * [Использование портала](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Использование Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Узнайте, как с помощью портала Azure создать кластер HDInsight с доступом к Azure Data Lake Store. В поддерживаемых типах кластеров Data Lake Store можно использовать в качестве хранилища по умолчанию или дополнительной учетной записи хранения. Если Data Lake Store используется как дополнительное хранилище, в этом случае в качестве учетной записи хранения по умолчанию для кластеров по-прежнему используется Azure Storage Blob (WASB). Кроме того, относящиеся к кластеру файлы (журналы и т. д.) записываются в хранилище по умолчанию, а данные, которые необходимо обработать, могут храниться в учетной записи Data Lake Store. Использование хранилища озера данных в качестве дополнительной учетной записи хранения не влияет на производительность или возможность выполнять чтение и запись в хранилище из кластера.

Важные сведения

* Возможность создавать кластеры HDInsight с доступом к Data Lake Store в качестве хранилища по умолчанию поддерживается для версии HDInsight 3.5.

* Возможность создавать кластеры HDInsight с доступом к Data Lake Store в качестве дополнительного хранилища поддерживается для версий HDInsight 3.2, 3.4 и 3.5.

* В кластерах HBase (Windows и Linux) Data Lake Store **нельзя** использовать как хранилище по умолчанию, а также как дополнительное хранилище.

* Data Lake Store может использоваться с кластерами Storm (Windows и Linux) для записи данных из топологии Storm. Хранилище озера данных также может использоваться для хранения справочных данных, которые затем можно будет прочитать с помощью топологии Storm. Дополнительные сведения см. в разделе [Использование хранилища озера данных в топологии Storm](#use-data-lake-store-in-a-storm-topology).


## <a name="prerequisites"></a>Предварительные требования
Перед началом работы с этим учебником необходимо иметь следующее:

* **Подписка Azure**. Ознакомьтесь с [бесплатной пробной версией Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Учетная запись хранилища озера данных Azure**. Следуйте инструкциям в разделе [Приступая к работе с хранилищем озера данных Azure на портале Azure](data-lake-store-get-started-portal.md).

* **Субъект-служба Azure Active Directory**. В этом учебнике приведены инструкции по созданию субъекта-службы в Azure AD. Однако, чтобы создать субъект-службу, необходимо быть администратором Azure AD. Если вы являетесь администратором Azure AD, то можете пропустить это предварительное требование и продолжить работу с учебником.

    **Если вы не являетесь администратором Azure AD**, то вы не сможете выполнить шаги, необходимые для создания субъекта-службы. В этом случае администратор Azure AD должен сначала создать субъект-службу, после чего вы сможете создать кластер HDInsight с Data Lake Store. При создании субъекта-службы также необходимо использовать сертификат, как описано в разделе [Create a service principal with certificate](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) (Создание субъекта-службы с сертификатом).

## <a name="do-you-learn-faster-with-videos"></a>Учитесь быстрее с помощью видео?
Просмотрите следующие видео, чтобы понять, как подготовить кластеры HDInsight с доступом к хранилищу озера данных.

* [Создание кластера Azure HDInsight с доступом к хранилищу озера данных Azure.](https://mix.office.com/watch/l93xri2yhtp2)
* После настройки кластера [используйте сценарии Hive и Pig для доступа к данным в хранилище озера данных](https://mix.office.com/watch/1n9g5w0fiqv1q)

## <a name="create-an-hdinsight-cluster-with-access-to-azure-data-lake-store"></a>Создание кластера Azure HDInsight с доступом к хранилищу озера данных Azure
В этом разделе мы создадим кластер HDInsight Hadoop, использующий хранилище озера данных в качестве дополнительного хранилища. В этом выпуске в кластере Hadoop хранилище озера данных может использоваться только как дополнительное хранилище кластера. Хранилищем по умолчанию по-прежнему будут большие двоичные объекты хранилища Azure (WASB). Поэтому мы сначала создадим учетную запись хранения и контейнеры хранилища, необходимые для кластера.

1. Перейдите на новый [портал Azure](https://portal.azure.com).

2. Выполните действия, описанные в разделе [Создание кластеров Hadoop в HDInsight](../hdinsight/hdinsight-provision-clusters.md) , чтобы начать подготовку кластера HDInsight.

3. В колонке **Источник данных** колонке выберите нужное хранилище по умолчанию: Azure Storage (WASB) или Data Lake Store. Если в качестве хранилища по умолчанию будет использоваться Azure Data Lake Store, переходите сразу к следующему шагу. 

    Если в качестве хранилища по умолчанию будет использоваться Azure Storage Blob, укажите **службу хранилища Azure** как **тип первичного хранилища**. Укажите данные учетной записи хранения и контейнера хранилища, а затем в поле **Расположение** укажите **Восточная часть США 2** и щелкните **Удостоверение кластера AAD**.
    
    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Add service principal to HDInsight cluster")


4. Если в качестве хранилища по умолчанию будет использоваться Azure Data Lake Store, укажите **Azure Data Lake Store** как **тип первичного хранилища**. Выберите существующую учетную запись Azure Data Lake Store, укажите путь к корневой папке, где будут храниться связанные с кластером файлы (см. примечание ниже), укажите **восточную часть США 2** в качестве **расположения**, а затем щелкните **Идентификация кластера AAD**. Этот параметр можно использовать только с кластерами версии HDInsight 3.5. В версии HDInsight 3.5 этот параметр недоступен для типа кластеров HBase.

    На снимке экрана ниже представлен путь к корневой папке: /clusters/myhdiadlcluster, где **myhdiadlcluster** — это имя создаваемого кластера. Убедитесь, что папка **/clusters** уже существует в учетной записи Data Lake Store. Папка **myhdiadlcluster** будет создана вместе с кластером. Аналогично, если корневой путь — это /hdinsight/clusters/data/myhdiadlclute, необходимо убедиться, что папка **/hdinsight/clusters/data/** уже существует в учетной записи Data Lake Store.
        
    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Add service principal to HDInsight cluster")

5. В колонке **Удостоверение кластера AAD** можно выбрать существующий субъект-службу или создать новый. Если вы хотите использовать существующий субъект-службу, сразу переходите к следующему шагу.

    Чтобы создать субъект-службу, в колонке **Удостоверение кластера AAD** щелкните **Создать**, выберите **Субъект-служба**, а затем в колонке **Создание субъекта-службы** укажите параметры для создания субъекта-службы. В рамках этого процесса также создаются сертификат и приложение Azure Active Directory. Щелкните **Создать**.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Add service principal to HDInsight cluster")

    Вы также можете **скачать сертификат**, связанный с созданным субъектом-службой. Это нужно, если требуется использовать один субъект-службу в будущем при создании дополнительных кластеров HDInsight. Нажмите кнопку **Выбрать**.

6. Чтобы создать субъект-службу, в колонке **Удостоверение кластера AAD** щелкните **Использовать существующий**, выберите **Субъект-служба**, а затем в колонке **Выбор субъекта-службы** найдите существующий субъект-службу. Щелкните имя субъекта-службы, а затем нажмите кнопку **Выбрать**.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Add service principal to HDInsight cluster")

    В колонке **Удостоверение кластера AAD** передайте связанный с выбранной субъектом-службой сертификат (PFX-файл) и введите пароль сертификата.

6. В колонке **Удостоверение кластера AAD** щелкните **Управление доступом ADLS**. На следующей панели флажок **Выбор разрешений для файла** уже установлен по умолчанию. Этот параметр перечисляет все учетные записи Data Lake Store в вашей подписке. Щелкните учетную запись Data Lake Store, которую нужно связать с кластером, чтобы получить список файлов и папок в этой учетной записи. Затем вы можете назначить разрешения на уровне файла или папки. Если вы хотите связать разрешения на корневом уровне учетной записи, установите флажок рядом с именем учетной записи.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Add service principal to HDInsight cluster")

    > [!NOTE]
    > Если вы используете учетную запись Data Lake Store в качестве хранилища по умолчанию для кластера, вам **нужно** назначить разрешения для субъекта-службы на корневом уровне учетной записи Data Lake Store.

7. Если вы хотите назначить разрешения для файла или папки в учетной записи, выберите учетную запись Data Lake Store, чтобы просмотреть файлы и папки на следующей панели. Выберите файлы и папки и разрешения (на чтение, запись и выполнение) для них, а также укажите, будут ли эти разрешения применяться рекурсивно к дочерним элементам. Затем нажмите кнопку **Выбрать**.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "Add service principal to HDInsight cluster")

8. На следующем экране щелкните **Запустить**, чтобы назначить разрешения для субъекта-службы Azure Active Directory в учетной записи, а также выбранных файлов и папок.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-2.png "Add service principal to HDInsight cluster")

9. Назначив разрешения, нажимайте кнопку **Готово** во всех колонках, чтобы вернуться в колонку **Удостоверение кластера AAD**.

4. В колонке **Удостоверение кластера AAD** нажмите кнопку **Выбрать** и продолжайте создавать кластер, как описано в статье [Создание кластеров Hadoop в HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).

10. После подготовки кластера убедитесь, что имя субъекта-службы связано с кластером HDInsight. Для этого в колонке кластера щелкните **Удостоверение кластера AAD** , чтобы просмотреть имя связанного субъекта-службы.

    ![Добавление субъекта-службы в кластер HDInsight](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Add service principal to HDInsight cluster")

## <a name="show-me-some-examples"></a>Примеры

Подготовив кластер для работы с Data Lake Store в качестве хранилища, вы можете изучить следующие примеры с использованием кластеров HDInsight для анализа данных, хранимых в Data Lake Store.

### <a name="run-a-hive-query-against-data-stored-in-data-lake-store"></a>Выполнение запроса Hive к данным, хранящимся в Data Lake Store

Чтобы выполнить запрос Hive, можно использовать интерфейс представлений Hive, доступный на портале Ambari. Инструкции по использованию представлений Ambari Hive см. в статье [Использование представления Hive с Hadoop в HDInsight](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md). При работе с данными в Data Lake Store необходимо изменить некоторые настройки.

* В примере с созданным нами кластером, путь к данным будет таким: `adl://<data_lake_store_account_name>/azuredatalakestore.net/path/to/file`. Запрос Hive для создания таблицы из образцов данных, хранимых в учетной записи Data Lake Store, будет выглядеть так:

        CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

В приведенном выше запросе:

* `adl://hdiadlstorage.azuredatalakestore.net` — корневой элемент учетной записи Data Lake Store;
* `/clusters/myhdiadlcluster` — корневой элемент для данных кластера, указанных при создании кластера;
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` — расположение образца файла, который используется в запросе.


### <a name="use-data-lake-store-with-spark-cluster"></a>Использование хранилища озера данных с кластером Spark
Кластер Spark можно использовать для выполнения заданий Spark с данными, хранящимися в Data Lake Store. Инструкции см. в статье [Использование кластера HDInsight Spark для анализа данных в Data Lake Store](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Использование хранилища озера данных в топологии Storm
Хранилище озера данных можно использовать для записи данных из топологии Storm. Инструкции по реализации этого сценария см. в статье [Использование хранилища озера данных Azure с помощью Apache Storm в HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Дополнительные материалы
* [PowerShell: создание кластера HDInsight для работы с хранилищем озера данных](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx



<!--HONumber=Nov16_HO4-->


