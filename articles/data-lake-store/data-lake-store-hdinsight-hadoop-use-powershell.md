---
title: Создание кластеров HDInsight с хранилищем озера данных с помощью Azure PowerShell | Microsoft Docs
description: Создание кластеров HDInsight для работы с озером данных Azure с помощью Azure PowerShell
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun

ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/01/2016
ms.author: nitinme

---
# Создание кластера HDInsight с хранилищем озера данных с помощью Azure PowerShell
> [!div class="op_single_selector"]
> * [Использование портала](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
> 
> 

Узнайте, как с помощью Azure PowerShell настроить кластер HDInsight (Hadoop, HBase или Storm) с доступом к хранилищу озера данных Azure. Важные сведения, которые следует учитывать при работе с данным выпуском.

* **В кластерах Spark (Linux) и Hadoop/Storm (Windows и Linux)** хранилище озера данных может использоваться только как дополнительная учетная запись хранения. Учетной записью хранения по умолчанию для таких кластеров по-прежнему будут BLOB-объекты хранилища Azure (WASB).
* **Для кластеров HBase (Windows и Linux)** хранилище озера данных можно использовать как хранилище по умолчанию или дополнительное хранилище.

> [!NOTE]
> Необходимо учитывать следующие важные замечания.
> 
> * Создание кластеров HDInsight с доступом к хранилищу озера данных доступно только при использовании HDInsight версии 3.2 и 3.4 (для кластеров Hadoop, HBase и Storm, как для Windows, так и для Linux). Для кластеров Spark в ОС Linux этот параметр доступен только для кластеров HDInsight 3.4.
> * Как упоминалось выше, хранилище озера данных доступно как хранилище по умолчанию для кластеров одних типов (HBase) и как дополнительное хранилище для кластеров других типов (Hadoop, Spark, Storm). Использование хранилища озера данных в качестве дополнительной учетной записи хранения не влияет на производительность или возможность выполнять чтение и запись в хранилище из кластера. В сценарии, при котором хранилище озера данных используется в качестве дополнительного хранилища, относящиеся к кластеру файлы (журналы и т. д.) записываются в хранилище по умолчанию (большие двоичные объекты Azure), а данные, которые необходимо обработать, могут храниться в учетной записи хранилища озера данных.
> 
> 

В этой статье мы подготовим кластер Hadoop, в котором хранилище озера данных будет дополнительным хранилищем.

Настройка в HDInsight хранилища озера данных с помощью PowerShell состоит из нескольких этапов:

* создание хранилища озера данных Azure;
* настройка проверки подлинности для доступа к хранилищу озера данных на основе ролей;
* создание кластера HDInsight с проверкой подлинности в хранилище озера данных;
* выполнение тестового задания в кластере.

## Предварительные требования
Перед началом работы с этим учебником необходимо иметь следующее:

* **Подписка Azure.**. См. [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Настройте свою подписку Azure** для использования общедоступной предварительной версии Data Lake Store. См. [инструкции](data-lake-store-get-started-portal.md#signup).
* **Пакет SDK Windows**. Его можно установить [отсюда](https://dev.windows.com/ru-RU/downloads). Пакет используется для создания сертификата безопасности.

## См. статью "Установка Azure PowerShell 1.0 и более поздних версий".
Сначала необходимо удалить версии 0.9x Azure PowerShell. Чтобы узнать установленную версию PowerShell, выполните следующую команду в окне PowerShell:

    Get-Module *azure*

Чтобы удалить старую версию, откройте панель управления, запустите компонент **Программы и компоненты** и удалите установленную версию, если она предшествует PowerShell 1.0.

Существует два основных варианта установки Azure PowerShell.

* [Коллекция PowerShell](https://www.powershellgallery.com/). Выполните следующие команды из интегрированной среды сценариев с повышенными привилегиями PowerShell или консоли Windows PowerShell с повышенными привилегиями:
  
        # Install the Azure Resource Manager modules from PowerShell Gallery
        Install-Module AzureRM
        Install-AzureRM
  
        # Install the Azure Service Management module from PowerShell Gallery
        Install-Module Azure
  
        # Import AzureRM modules for the given version manifest in the AzureRM module
        Import-AzureRM
  
        # Import Azure Service Management module
        Import-Module Azure
  
    Дополнительные сведения см. в статье [Коллекция PowerShell](https://www.powershellgallery.com/).
* [Установщик веб-платформы Майкрософт (WebPI)](http://aka.ms/webpi-azps). Если вы установили Azure PowerShell 0.9.x, появится запрос для удаления версии 0.9.x. Если вы установили модули Azure PowerShell из коллекции PowerShell, установщик требует удалить эти модули перед установкой, чтобы обеспечить целостность среды PowerShell Azure. Инструкции см. в разделе [Установка Azure PowerShell 1.0 с помощью установщика веб-платформы](https://azure.microsoft.com/blog/azps-1-0/).

Обновления для установщика веб-платформы будут выпускаться ежемесячно. Обновления для коллекции PowerShell будут выпускаться на постоянной основе. Если вы решите установить коллекцию PowerShell, она станет основным источником всего нового и лучшего в Azure PowerShell.

## Создание хранилища озера данных Azure
Чтобы создать хранилище озера данных, сделайте следующее.

1. На своем компьютере откройте новое окно Azure PowerShell и введите следующий фрагмент кода. Когда вам будет предложено войти, введите учетные данные администратора или владельца подписки.
   
        # Log in to your Azure account
        Login-AzureRmAccount
   
        # List all the subscriptions associated to your account
        Get-AzureRmSubscription
   
        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>
   
        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
   
   > [!NOTE]
   > Если при регистрации поставщика ресурсов хранилища озера данных появляется сообщение об ошибке, похожее на `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, это может означать, что ваша подписка отсутствует в списке разрешений для хранилища озера данных Azure. Убедитесь, что подписка Azure для общедоступной предварительной версии Data Lake Store включена, выполнив указанные [инструкции](data-lake-store-get-started-portal.md#signup).
   > 
   > 
2. Учетная запись хранения озера данных Azure связывается с группой ресурсов Azure. Для начала создайте группу ресурсов Azure.
   
        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"
   
    ![Создание группы ресурсов Azure](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Создание группы ресурсов Azure")
3. Создайте учетную запись хранилища озера данных Azure. Имя новой учетной записи должно содержать только строчные буквы и цифры.
   
        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"
   
    ![Создание учетной записи озера данных Azure](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Создание учетной записи озера данных Azure")
4. Убедитесь, что учетная запись создана.
   
        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName
   
    Результат должен иметь значение **True**.
5. Отправьте пример данных в озеро данных Azure. Позже мы проверим, доступны ли эти данные из кластера HDInsight. Если у вас нет под рукой подходящих для этих целей данных, передайте папку **Ambulance Data** из [репозитория Git для озера данных Azure](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## Настройка проверки подлинности для доступа к хранилищу озера данных на основе ролей
Каждая подписка Azure связана со службой Azure Active Directory. Пользователи и службы, получающие доступ к ресурсам подписки через классический портал Azure или API Azure Resource Manager, сначала должны пройти аутентификацию в соответствующей службе Azure Active Directory. Доступ к подпискам и службам Azure предоставляется путем назначения соответствующей роли для ресурса Azure. В случае со службами субъект-служба идентифицирует службу в Azure Active Directory (AAD). В этом разделе мы расскажем, как предоставить службе приложения, в частности HDInsight, доступ к ресурсу Azure (созданной ранее учетной записи хранилища озера данных Azure). Для этого с помощью Azure PowerShell мы создадим для приложения субъект-службу и назначим ему роли.

Чтобы настроить для озера данных Azure проверку подлинности в Active Directory, вам необходимо сделать следующее:

* Создание самозаверяющего сертификата
* создать приложение в Azure Active Directory и субъект-службу.

### Создание самозаверяющего сертификата
Прежде чем выполнять дальнейшие действия, описанные в этом разделе, убедитесь, что на вашем компьютере установлен [пакет SDK Windows](https://dev.windows.com/ru-RU/downloads). Вам также необходимо создать каталог, например **C:\\mycertdir**, в котором будет создан сертификат.

1. В окне PowerShell перейдите в расположение, где установлен пакет SDK Windows (обычно это `C:\Program Files (x86)\Windows Kits\10\bin\x86`), и с помощью служебной программы [MakeCert][makecert] создайте самозаверяющий сертификат и закрытый ключ. Используйте такие команды:
   
        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')
   
        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048
   
    Вам будет предложено ввести пароль для закрытого ключа. После успешного выполнения команды в указанном каталоге сертификатов должны появиться файлы **CertFile.cer** и **mykey.pvk**.
2. С помощью служебной программы [Pvk2Pfx][pvk2pfx] преобразуйте созданные программой MakeCert PVK- и CER-файлы в PFX-файл. Выполните следующую команду:
   
        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>
   
    При появлении соответствующего запроса введите указанный ранее пароль для закрытого ключа. Значение, указываемое для параметра **-po** — это пароль, связанный с PFX-файлом. После успешного выполнения команды вы должны увидеть в указанном каталоге сертификата файл CertFile.pfx.

### Создание приложения в Azure Active Directory и субъекта-службы
Следуя описаниям, приведенным в этом разделе, вы создадите субъект-службу для приложения Azure Active Directory, назначите роль субъекту-службе и пройдете проверку подлинности в качестве субъекта-службы, используя сертификат. Чтобы создать приложение в Azure Active Directory, выполните следующие команды.

1. В окне консоли PowerShell вставьте из буфера обмена следующие командлеты. Укажите для свойства **-DisplayName** уникальное значение. Значения свойств **-HomePage** и **- IdentiferUris** — это заполнители, которые не проверяются.
   
        $certificateFilePath = "$certificateFileDir\CertFile.pfx"
   
        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file
   
        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)
   
        $rawCertificateData = $certificatePFX.GetRawCertData()
   
        $credential = [System.Convert]::ToBase64String($rawCertificateData)
   
        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate
   
        $applicationId = $application.ApplicationId
2. С помощью идентификатора приложения создайте субъект-службу.
   
        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId
   
        $objectId = $servicePrincipal.Id
3. Предоставьте субъекту-службе доступ к созданному ранее хранилищу озера данных.
   
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
   
    При появлении соответствующего запроса введите **Y** для подтверждения.

## Создание кластера HDInsight с проверкой подлинности в хранилище озера данных
В этом разделе мы создадим кластер HDInsight Hadoop. В этом выпуске кластер HDInsight и хранилище озера данных должны быть в одном расположении (Восток США 2).

1. Сначала получите идентификатор клиента подписки. Позже он вам понадобится.
   
        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. В этом выпуске в кластере Hadoop хранилище озера данных может использоваться только как дополнительное хранилище кластера. Хранилищем по умолчанию по-прежнему будут BLOB-объекты хранилища Azure (WASB). Поэтому мы сначала создадим учетную запись хранения и контейнеры хранилища, необходимые для кластера.
   
        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name
   
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS
   
        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Создайте кластер HDInsight, выполнив следующие командлеты.
   
        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential
   
        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password
   
    В случае успешного выполнения командлета должен появиться такой результат:
   
        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## Выполнение тестовых заданий в кластере HDInsight
После настройки кластера HDInsight выполните в нем тестовые задания, чтобы проверить, доступно ли ему хранилище озера данных. Для этого запустите образец задания Hive, создающего таблицу с данными, которые вы ранее отправили в хранилище озера данных.

### Кластер Linux
В этом разделе вы подключитесь к кластеру по SSH и выполните пример запроса Hive. Windows не предоставляет встроенный клиент SSH. Рекомендуется использовать **PuTTY**, который можно скачать по адресу: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Дополнительные сведения об использовании PuTTY см. в разделе [Использование SSH с Hadoop на основе Linux в HDInsight из Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. После подключения запустите интерфейс командной строки Hive с помощью следующей команды:
   
        hive
2. Используя интерфейс командной строки, введите следующие инструкции, чтобы создать таблицу с именем **vehicles** с помощью примера данных в хранилище озера данных.
   
        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;
   
    Должен отобразиться результат, аналогичный приведенному ниже:
   
        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

### Кластер Windows
С помощью следующих командлетов выполните запрос Hive. Этот запрос создает таблицу на основе данных в хранилище озера данных, а затем в созданной таблице выполняет запрос на выборку.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Командлеты возвращают следующий результат. Если **ExitValue** имеет значение 0, это означает, что задание выполнилось успешно.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Извлеките выходные данные из задания с помощью следующего командлета.

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Результат будет выглядеть так:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## Доступ к хранилищу озера данных с помощью команд HDFS
Настроив в кластере HDInsight параметры для работы с хранилищем озера данных, используйте для доступа к хранилищу команды оболочки HDFS.

### Кластер Linux
В этом разделе вы подключитесь к кластеру по SSH и выполните команды HDFS. Windows не предоставляет встроенный клиент SSH. Рекомендуется использовать **PuTTY**, который можно скачать по адресу: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Дополнительные сведения об использовании PuTTY см. в разделе [Использование SSH с Hadoop на основе Linux в HDInsight из Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

После подключения используйте следующую команду файловой системы HDFS для получения списка файлов в хранилище озера данных.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Эта команда должна показать файл, который вы ранее отправили в хранилище озера данных.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

С помощью команды `hdfs dfs -put` вы можете отправить в хранилище озера данных некоторые файлы, а затем с помощью команды `hdfs dfs -ls` проверить, успешно ли они передались.

### Кластер Windows
1. Перейдите на новый [портал Azure](https://portal.azure.com).
2. Последовательно щелкните **Обзор** и **Кластеры HDInsight**, а затем выберите созданный кластер HDInsight.
3. В колонке кластера нажмите кнопку **Удаленный рабочий стол**, а затем в колонке **Удаленный рабочий стол** щелкните **Подключиться**.
   
    ![Удаленное подключение к кластеру HDI](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Создание группы ресурсов Azure")
   
    При появлении соответствующего запроса введите учетные данные, которые вы указали для пользователя удаленного рабочего стола.
4. Во время удаленного сеанса запустите Windows PowerShell и, используя команды файловой системы HDFS, отобразите список файлов в хранилище озера данных Azure.
   
         hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
   
    Эта команда должна показать файл, который вы ранее отправили в хранилище озера данных.
   
        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv
   
    С помощью команды `hdfs dfs -put` вы можете отправить в хранилище озера данных некоторые файлы, а затем с помощью команды `hdfs dfs -ls` проверить, успешно ли они передались.

## См. также
* [Портал: создание кластера HDInsight для работы с хранилищем озера данных](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

<!---HONumber=AcomDC_0914_2016-->