---
title: "Создание первой фабрики данных (PowerShell) | Документация Майкрософт"
description: "В этом руководстве вы создадите образец конвейера фабрики данных Azure с помощью Azure PowerShell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/01/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: c1551b250ace3aa6775932c441fcfe28431f8f57
ms.openlocfilehash: 110a1a0ee760b5afe00f63443ee71c60c938ef55


---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a>Руководство. Создание первой фабрики данных Azure с помощью Azure PowerShell
> [!div class="op_single_selector"]
> * [Обзор и предварительные требования](data-factory-build-your-first-pipeline.md)
> * [Портал Azure](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Шаблон Resource Manager](data-factory-build-your-first-pipeline-using-arm.md)
> * [ИНТЕРФЕЙС REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

Из этой статьи вы узнаете, как создать свою первую фабрику данных с помощью Azure PowerShell.

## <a name="prerequisites"></a>Предварительные требования
* Прочтите [обзорную статью](data-factory-build-your-first-pipeline.md) и выполните **предварительные требования** .
* Чтобы установить последнюю версию Azure PowerShell на локальном компьютере, следуйте инструкциям в статье [Установка и настройка Azure PowerShell](/powershell/azureps-cmdlets-docs) .
* В этой статье рассматриваются не все командлеты фабрики данных (необязательный раздел). Полную документацию по командлетам фабрики данных см. в [этом справочнике](https://msdn.microsoft.com/library/dn820234.aspx).

## <a name="create-data-factory"></a>Создание фабрики данных
На этом этапе с помощью Azure PowerShell создается фабрика данных Azure с именем **FirstDataFactoryPSH**. Фабрика данных может иметь один или несколько конвейеров. Конвейер может содержать одно или несколько действий. Это может быть, например, действие копирования, копирующее данные из исходного хранилища данных в целевое, или действие HDInsight Hive для выполнения скрипта Hive, преобразующего входные данные. Начнем с создания фабрики данных.

1. Откройте Azure PowerShell и выполните следующую команду. Не закрывайте Azure PowerShell, пока выполняются описанные в учебнике инструкции. Если закрыть и снова открыть это окно, то придется вновь выполнять эти команды.
   * Выполните командлет `Login-AzureRmAccount` и введите имя пользователя и пароль, которые вы используете для входа на портал Azure.
   * Выполните командлет `Get-AzureRmSubscription` , чтобы просмотреть все подписки для этой учетной записи.
   * Выполните командлет `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` , чтобы выбрать подписку, с которой вы собираетесь работать. Эта подписка должна совпадать с той, которая используется на портале Azure.
2. Создайте группу ресурсов Azure с именем **ADFTutorialResourceGroup** , выполнив следующую команду:

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Некоторые действия, описанные в этом учебнике, предполагают, что вы используете группу ресурсов с именем ADFTutorialResourceGroup. Если вы используете другую группу ресурсов, укажите ее вместо ADFTutorialResourceGroup.
3. Выполните командлет **New-AzureRmDataFactory**, чтобы создать фабрику данных с именем **FirstDataFactoryPSH**.

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"

Обратите внимание на следующие моменты.

* Имя фабрики данных Azure должно быть глобально уникальным. Если появится сообщение об ошибке **Имя FirstDataFactoryPSH фабрики данных недоступно**измените это имя (например, на ваше_имя_FirstDataFactoryPSH). Используйте это имя вместо ADFTutorialFactoryPSH при выполнении шагов в этом руководстве. Ознакомьтесь с разделом [Фабрика данных — правила именования](data-factory-naming-rules.md) , чтобы узнать о правилах именования артефактов фабрики данных.
* Чтобы создать экземпляры фабрики данных, вы должны быть администратором или участником подписки Azure.
* В будущем имя фабрики данных может быть зарегистрировано в качестве DNS-имени и, следовательно, стать отображаемым.
* Если появится сообщение об ошибке**Подписка не зарегистрирована для использования пространства имен Microsoft.DataFactory**, выполните одно из следующих действий и повторите попытку публикации.

  * Чтобы зарегистрировать поставщик фабрики данных Azure, выполните следующую команду в Azure PowerShell:

          Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory

      Чтобы убедиться, что поставщик фабрики данных зарегистрирован, выполните следующую команду:

          Get-AzureRmResourceProvider
  * Войдите на [портал Azure](https://portal.azure.com) с использованием подписки Azure и откройте колонку фабрики данных или создайте на портале фабрику данных. Поставщик будет зарегистрирован автоматически.

Прежде чем создавать конвейер, необходимо создать несколько сущностей фабрики данных. Сначала создайте связанные службы, чтобы связать хранилища данных и вычисления со своим хранилищем данных, и определите входные и выходные наборы данных, которые будут представлять входные и выходные данные в связанных хранилищах. Затем создайте конвейер с действием, в котором используются эти наборы данных.

## <a name="create-linked-services"></a>Создание связанных служб
На этом шаге вы свяжете учетную запись службы хранилища Azure и используемый по запросу кластер Azure HDInsight с фабрикой данных. В этом примере учетная запись хранения Azure содержит входные и выходные данные для конвейера. Для выполнения скрипта Hive, указанного в действии конвейера, в этом примере используется связанная служба HDInsight. Определите, какие данные хранилища и службы вычислений используются в сценарии, и свяжите эти службы с фабрикой данных, создав связанные службы.

### <a name="create-azure-storage-linked-service"></a>Создание связанной службы хранения Azure
На этом шаге вы свяжете учетную запись хранения Azure с фабрикой данных. Используйте одну и ту же учетную запись хранения Azure для хранения входных и выходных данных и файла скрипта HQL.

1. Создайте в папке C:\ADFGetStarted JSON-файл с именем StorageLinkedService.json со следующим содержимым. Создайте папку ADFGetStarted, если она еще не существует.

        {
            "name": "StorageLinkedService",
            "properties": {
                "type": "AzureStorage",
                "description": "",
                "typeProperties": {
                    "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        }

    Замените **account name** именем своей учетной записи хранения Azure, а **account key** — ключом доступа к ней. Сведения о получении, просмотре, копировании и повторном создании ключей доступа к хранилищу см. в разделе [Управление учетной записью хранения](../storage/storage-create-storage-account.md#manage-your-storage-account).
2. В Azure PowerShell перейдите в папку ADFGetStarted.
3. Создать связанную службу можно с помощью командлета **New-AzureRmDataFactoryLinkedService** . В этом командлете и в других командлетах фабрики данных, которые используются в этом руководстве, требуется передача значений для параметров *ResourceGroupName* и *DataFactoryName*. Кроме того, можно использовать командлет **Get-AzureRmDataFactory**, чтобы получить объект **DataFactory** и передать этот объект без необходимости ввода параметров *ResourceGroupName* и *DataFactoryName* при каждом запуске командлета. Выполните следующую команду, чтобы назначить выходные данные командлета **Get-AzureRmDataFactory** переменной **$df**.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
4. Теперь выполните командлет **New-AzureRmDataFactoryLinkedService**, чтобы создать связанную службу **StorageLinkedService**.

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    Если вы не выполнили командлет **Get-AzureRmDataFactory** и не присвоили выходные данные переменной **$df**, вам нужно указать значения параметров *ResourceGroupName* и *DataFactoryName* следующим образом.

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json

    Если вы закроете Azure PowerShell, не завершив выполнение описанных в руководстве инструкций, при следующем запуске Azure PowerShell вам нужно будет запустить командлет **Get-AzureRmDataFactory** , чтобы выполнить эти инструкции.

### <a name="create-azure-hdinsight-linked-service"></a>Создание связанной службы Azure HDInsight
На этом шаге вы свяжете используемый по запросу кластер HDInsight с фабрикой данных. Кластер HDInsight автоматически создается в среде выполнения и удаляется после завершения обработки и простоя в течение указанного времени. Вместо используемого по запросу кластера HDInsight можно использовать собственный кластер HDInsight. Дополнительные сведения см. в статье [Связанные службы вычислений](data-factory-compute-linked-services.md).

1. Создайте в папке **C:\ADFGetStarted** JSON-файл **HDInsightOnDemandLinkedService**.json со следующим содержимым.

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }

    В следующей таблице приведены описания свойств JSON, используемых в этом фрагменте кода.

   | Свойство | Описание |
   |:--- |:--- |
   | Version (версия) |Указывает, что версия создаваемого кластера HDInsight — 3.2. |
   | ClusterSize (размер кластера) |Указывает размер кластера HDInsight. |
   | TimeToLive (срок жизни) |Указывает, сколько времени может простаивать кластер HDInsight, прежде чем он будет удален. |
   | linkedServiceName (имя связанной службы) |Указывает имя учетной записи хранения, в которой будут храниться журналы, создаваемые HDInsight. |

    Обратите внимание на следующие моменты.

   * С помощью JSON-файла фабрика данных создает кластер HDInsight **под управлением Windows**. Можно также создать кластер HDInsight **под управлением Linux**. Дополнительные сведения см. в разделе [Связанная служба Azure HDInsight по запросу](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
   * Вместо кластера HDInsight по запросу можно использовать **собственный кластер HDInsight**. См. сведения о [связанной службе Azure HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
   * Кластер HDInsight создает **контейнер по умолчанию** в хранилище BLOB-объектов, указанном в коде JSON (**linkedServiceName**). При удалении кластера HDInsight этот контейнер не удаляется. В этом весь замысел. Если используется связанная служба HDInsight по запросу, кластер HDInsight создается при каждой обработке среза данных (если не используется динамический кластер**timeToLive**). После завершения обработки кластер автоматически удаляется.

       По мере обработки новых срезов количество контейнеров в хранилище BLOB-объектов будет увеличиваться. Если эти контейнеры не используются для устранения неполадок с заданиями, удалите их — это позволит сократить расходы на хранение. Имена контейнеров указаны в формате adf**имя_фабрики_данных**-**имя_связанной_службы**-метка_даты_и_времени. Для удаления контейнеров в хранилище BLOB-объектов Azure используйте такие инструменты, как [Microsoft Storage Explorer](http://storageexplorer.com/) .

     Дополнительные сведения см. в разделе [Связанная служба Azure HDInsight по запросу](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
2. Выполните командлет **New-AzureRmDataFactoryLinkedService** , чтобы создать связанную службу с именем HDInsightOnDemandLinkedService.

        New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json

## <a name="create-datasets"></a>Создание наборов данных
На этом шаге вы создадите наборы данных, которые представляют входные и выходные данные для обработки Hive. Эти наборы данных ссылаются на службу **StorageLinkedService** , созданную ранее в ходе работы с этим руководством. Точки связанной службы указывают на учетную запись хранения Azure, а наборы данных указывают контейнер, папку и имя файла в хранилище, в котором содержатся входные и выходные данные.

### <a name="create-input-dataset"></a>Создание входного набора данных
1. Создайте в папке **C:\ADFGetStarted** JSON-файл **InputTable.json** со следующим содержимым.

        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        }

    Приведенный выше JSON-файл определяет набор данных с именем **AzureBlobInput**, представляющий входные данные для действия в конвейере. Кроме того, файл указывает, что входные данные размещаются в контейнере больших двоичных объектов **adfgetstarted** и в папке **inputdata**.

    В следующей таблице приведены описания свойств JSON, используемых в этом фрагменте кода.

   | Свойство | Описание |
   |:--- |:--- |
   | type |Для свойства типа задано значение AzureBlob, так как данные хранятся в хранилище BLOB-объектов Azure. |
   | linkedServiceName (имя связанной службы) |Ссылается на созданную ранее службу StorageLinkedService. |
   | fileName |Это необязательное свойство. Если это свойство не указано, выбираются все файлы из папки folderPath. В этом случае обрабатывается только файл input.log. |
   | type |Файлы журнала представлены в текстовом формате, поэтому мы используем значение TextFormat. |
   | columnDelimiter |Столбцы в файлах журнала разделяются запятыми (,). |
   | frequency и interval |Для свойства frequency задано значение Month, а для свойства interval — значение 1. Это означает, что срезы входных данных доступны ежемесячно. |
   | external |Это свойство имеет значение true, если входные данные не создаются службой фабрики данных. |
2. Чтобы создать набор данных фабрики данных, выполните следующую команду в Azure PowerShell:

        New-AzureRmDataFactoryDataset $df -File .\InputTable.json

### <a name="create-output-dataset"></a>Создание выходного набора данных
Теперь создайте выходной набор данных, представляющий выходные данные, которые хранятся в хранилище BLOB-объектов Azure.

1. Создайте в папке **C:\ADFGetStarted** JSON-файл **OutputTable.json** со следующим содержимым.

        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    JSON-файл определяет набор данных с именем **AzureBlobOutput**, представляющий выходные данные для действия в конвейере. Кроме того, файл указывает, что результаты хранятся в контейнере больших двоичных объектов **adfgetstarted** и в папке **partitioneddata**. В разделе **availability** указывается частота, с которой будет создаваться выходной набор данных (ежемесячно).
2. Чтобы создать набор данных фабрики данных, выполните следующую команду в Azure PowerShell:

        New-AzureRmDataFactoryDataset $df -File .\OutputTable.json

## <a name="create-pipeline"></a>Создание конвейера
На этом шаге вы создадите свой первый конвейер с действием **HDInsightHive** . Срез входных данных создается ежемесячно (frequency: Month, interval: 1), срез выходных данных создается ежемесячно, свойство scheduler для действия также указывается ежемесячно. Параметры выходного набора данных (outputs) и планировщика действия (scheduler) должны совпадать. В настоящее время расписание активируется с помощью выходного набора данных, поэтому его необходимо создать, даже если действие не создает никаких выходных данных. Если действие не принимает никаких входных данных, входной набор данных можно не создавать. Свойства, используемые в следующем фрагменте JSON, описаны в конце этого раздела.

1. Создайте в папке C:\ADFGetStarted JSON-файл MyFirstPipelinePSH.json со следующим содержимым.

   > [!IMPORTANT]
   > В JSON-файле замените свойство **storageaccountname** именем своей учетной записи хранения.
   >
   >

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "StorageLinkedService",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    Этот фрагмент создает конвейер из одного действия, использующего Hive для обработки данных в кластере HDInsight.

    Файл **partitionweblogs.hql** скрипта Hive хранится в учетной записи хранения Azure (указывается с помощью свойства scriptLinkedService с именем **StorageLinkedService**) в папке **script** в контейнере **adfgetstarted**.

    Раздел **defines** используется для настройки параметров среды выполнения, которые будут переданы в скрипт Hive в качестве значений конфигурации Hive (например, ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

    Активный период конвейера задается с помощью свойств **start** и **end**.

    В действии JSON укажите, что скрипт Hive будет выполняться в среде вычислений, указанной в свойстве **linkedServiceName**, — **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > Сведения о свойствах JSON, используемых в этом примере, см. в разделе "Конвейер JSON" статьи [Конвейеры и действия в фабрике данных Azure](data-factory-create-pipelines.md).

2. Убедитесь, что файл **input.log** отображается в папке **adfgetstarted/inputdata** в хранилище BLOB-объектов Azure, и выполните следующую команду, чтобы развернуть конвейер. Так как время в свойствах **start** и **end** задано в прошлом, а для свойства **isPaused** задано значение false, конвейер (действие в конвейере) запускается сразу после развертывания.

       New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
3. Поздравляем! Вы создали свой первый конвейер с помощью Azure PowerShell!

## <a name="monitor-pipeline"></a>Отслеживание конвейера
На этом шаге Azure PowerShell будет использоваться для мониторинга процессов в фабрике данных Azure.

1. Выполните командлет **Get-AzureRmDataFactory** и назначьте выходные данные переменной **$df**.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
2. Выполните командлет **Get-AzureRmDataFactorySlice** для получения сведений обо всех срезах в таблице **EmpSQLTable**, которая является выходной таблицей конвейера.

        Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2016-04-01

    Обратите внимание, здесь указывается то же значение StartDateTime, что и в JSON конвейера. Вы должны увидеть результат, аналогичный приведенному ниже.

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : FirstDataFactoryPSH
        DatasetName       : AzureBlobOutput
        Start             : 4/1/2016 12:00:00 AM
        End               : 4/2/2016 12:00:00 AM
        RetryCount        : 0
        State             : InProgress
        SubState          :
        LatencyStatus     :
        LongRetryCount    : 0
3. Выполните командлет **Get-AzureRmDataFactoryRun** , чтобы получить сведения о действиях, выполняемых для конкретного среза.

        Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2016-04-01

    Вы должны увидеть результат, аналогичный приведенному ниже.

        Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : FirstDataFactoryPSH
        DatasetName         : AzureBlobOutput
        ProcessingStartTime : 12/18/2015 4:50:33 AM
        ProcessingEndTime   : 12/31/9999 11:59:59 PM
        PercentComplete     : 0
        DataSliceStart      : 4/1/2016 12:00:00 AM
        DataSliceEnd        : 4/2/2016 12:00:00 AM
        Status              : AllocatingResources
        Timestamp           : 12/18/2015 4:50:33 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : RunSampleHiveActivity
        PipelineName        : MyFirstPipeline
        Type                : Script

    Вы можете выполнять этот командлет до тех пор, пока не увидите срез с состоянием **Готово** или **Сбой**. Когда срез перейдет в состояние "Готово", проверьте выходные данные в папке **partitioneddata** контейнера **adfgetstarted** в хранилище BLOB-объектов.  Создание кластера HDInsight по требованию занимает некоторое время.

    ![выходные данные](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> Создание используемого по требованию кластера HDInsight обычно занимает некоторое время (около 20 минут). Таким образом, конвейер обработает срез **примерно через 30 минут** .
>
> В случае успешной обработки среза входной файл удаляется. Если вы хотите повторно обработать срез или еще раз выполнить инструкции из руководства, передайте входной файл (input.log) в папку inputdata в контейнере adfgetstarted.
>
>

## <a name="summary"></a>Сводка
Следуя инструкциям из этого руководства, вы создали фабрику данных Azure для обработки данных путем выполнения сценария Hive в кластере Hadoop HDInsight. Вы использовали редактор фабрики данных на портале Azure для выполнения следующих действий:

1. создание **фабрики данных Azure**;
2. создание двух **связанных служб**.
   1. **Служба хранилища Azure** — связанная служба для связывания хранилища BLOB-объектов Azure, которое содержит входные и выходные файлы, с фабрикой данных.
   2. **Azure HDInsight** — связанная служба по запросу для связывания кластера HDInsight Hadoop с фабрикой данных. Фабрика данных Azure своевременно создает кластер HDInsight Hadoop для обработки входных данных и генерирования выходных данных.
3. Создание двух **наборов данных**, которые описывают входные и выходные данные для действия HDInsight Hive в конвейере.
4. Создание **конвейера** с действием **HDInsight Hive**.

## <a name="next-steps"></a>Дальнейшие действия
В этой статье описывается создание конвейера с помощью действия преобразования (действие HDInsight), которое по требованию выполняет сценарий Hive в кластере Azure HDInsight. Сведения о том, как копировать данные из хранилища BLOB-объектов Azure в SQL Azure с помощью действия копирования, см. в статье [Копирование данных из хранилища BLOB-объектов Azure в базу данных SQL с помощью фабрики данных](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>См. также
| Раздел | Описание |
|:--- |:--- |
| [Справочник по командлетам фабрики данных](https://msdn.microsoft.com/library/azure/dn820234.aspx) |См. полную документацию по командлетам фабрики данных. |
| [Действия по преобразованию данных](data-factory-data-transformation-activities.md) |В этой статье рассматриваются действия по преобразованию данных (например, преобразование HDInsight Hive, используемое в этом руководстве), поддерживаемые фабрикой данных Azure. |
| [Планирование и выполнение](data-factory-scheduling-and-execution.md) |Здесь объясняются аспекты планирования и исполнения в модели приложений фабрики данных. |
| [Конвейеры](data-factory-create-pipelines.md) |Эта статья поможет вам понять сущность конвейеров и действий в фабрике данных Azure, а также научиться с их помощью создавать комплексные рабочие процессы, управляемые данными, для конкретных бизнес-сценариев. |
| [Наборы данных](data-factory-create-datasets.md) |Эта статья поможет вам понять, что такое наборы данных в фабрике данных Azure. |
| [Мониторинг конвейеров и управление ими с помощью колонок портала Azure](data-factory-monitor-manage-pipelines.md) |В этой статье описываются мониторинг и отладка конвейеров, а также управление ими с помощью колонок портала Azure. |
| [Мониторинг конвейеров фабрики данных Azure и управление ими с помощью нового приложения по мониторингу и управлению](data-factory-monitor-manage-app.md) |В этой статье описывается мониторинг и отладка конвейеров, а также управление ими с помощью приложения мониторинга и управления. |



<!--HONumber=Dec16_HO2-->


