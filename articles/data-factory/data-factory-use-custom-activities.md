---
title: "Использование настраиваемых действий в конвейере фабрики данных Azure"
description: "Узнайте, как создавать пользовательские действия и использовать их в конвейере фабрики данных Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 2a04aa8a37ca73ee5a5231e049c572b307a5a1fc
ms.openlocfilehash: defb6869fb2abab369ac5fd9cb62de810c85e57f


---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Использование настраиваемых действий в конвейере фабрики данных Azure
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

Существует два типа действий, которые можно использовать в конвейере фабрики данных Azure.

* [Действия перемещения данных](data-factory-data-movement-activities.md) для перемещения данных между [поддерживаемыми хранилищами данных](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
* [Действия преобразования данных](data-factory-data-transformation-activities.md) для преобразования и обработки данных с помощью вычислений, например: в Azure HDInsight, пакетной службе Azure и машинном обучении Azure. Например: HDInsight Hive и выполнение пакетной службы машинного обучения.  

Если необходимо переместить данные из хранилища данных, не поддерживаемого фабрикой данных Azure, или в такое хранилище, можно создать пользовательское действие .NET с собственной логикой перемещения данных и использовать это действие в конвейере.

Аналогично, если вам нужно преобразовать или обработать данные способом, который не поддерживается фабрикой данных Azure, можно создать пользовательское действие с собственной логикой обработки данных и использовать это действие в конвейере.

> [!NOTE]
> В настоящее время шлюз управления данными поддерживает действие копирования и действие хранимой процедуры только в фабрике данных. Невозможно использовать шлюз из настраиваемого действия для доступа к локальным источникам данных.
>
>

Можно настроить запуск настраиваемого действия .NET с помощью **пакетной службы Azure** или **кластера Azure HDInsight**.   

В следующем руководстве содержатся пошаговые инструкции по созданию пользовательского действия .NET и его использованию в конвейере. В этом пошаговом руководстве используется связанная **пакетная служба Azure** . Чтобы использовать вместо нее пакетную службу Azure HDInsight, создайте связанную службу типа **HDInsight** (если вы используете собственный кластер HDInsight) или **HDInsightOnDemand** (если хотите создать кластер HDInsight по запросу с помощью фабрики данных), а затем используйте ее в разделе activity определения JSON конвейера (**linkedServiceName**). Дополнительные сведения об использовании Azure HDInsight для выполнения пользовательского действия приведены в разделе [Использование связанных служб Azure HDInsight](#use-azure-hdinsight-linked-services) .

> [!IMPORTANT]
> Задайте версию 4.5.2 платформы .NET Framework в качестве целевой для своего проекта настраиваемого действия в Visual Studio. Фабрика данных не поддерживает настраиваемые действия, скомпилированные для более поздних версий, чем .NET Framework 4.5.2.   
>
>

## <a name="walkthrough"></a>Пошаговое руководство
### <a name="prerequisites"></a>Предварительные требования
* Visual Studio 2012/2013/2015
* Скачайте и установите [пакет SDK Azure для .NET][azure-developer-center].

### <a name="azure-batch-prerequisites"></a>Предварительные требования для пакетной службы Azure
В этом руководстве вы запустите свои настраиваемые действия .NET с помощью пакетной службы Azure как вычислительные ресурсы. В статье [Основные сведения о пакетной службе Azure][batch-technical-overview] представлен обзор пакетной службы Azure. Статья [Начало работы с библиотекой пакетной службы Azure для .NET][batch-get-started] поможет быстро приступить к работе с пакетной службой Azure.

Для этого руководства вам понадобится создать учетную запись пакетной службы Azure с пулом виртуальных машин. Для этого выполните следующие действия:

1. Создание **учетной записи пакетной службы Azure** на [портале Azure](http://manage.windowsazure.com). Инструкции см. в статье [Создание учетной записи пакетной службы Azure на портале Azure][batch-create-account]. Запишите ключ и имя учетной записи Пакетной службы Azure.

    Для создания учетной записи пакетной службы Azure можно также воспользоваться командлетом [New-AzureBatchAccount][new-azure-batch-account]. Подробные инструкции по использованию этого командлета см. в записи блога [Using Azure PowerShell to Manage Azure Batch Account][azure-batch-blog] (Использование Azure PowerShell для управления учетной записью пакетной службы Azure).
2. Создайте **пул пакетной службы Azure**.

   1. На [портале Azure](https://portal.azure.com) щелкните **Обзор** в меню слева и выберите **Уч. записи пакетной службы**.
   2. Выберите свою учетную запись пакетной службы Azure, чтобы открыть колонку **Учетная запись пакетной службы** .
   3. Щелкните элемент **Пулы** .
   4. В колонке **Пулы** нажмите кнопку "Добавить" на панели инструментов, чтобы добавить пул.
      1. Введите идентификатор для пула (**идентификатор пула**). Сохраните **идентификатор пула**. Он понадобится при создании решения фабрики данных.
      2. В качестве параметра семейства операционных систем укажите **Windows Server 2012 R2** .
      3. Выберите **Ценовая категория для узлов**.
      4. Для параметра **Выделенный целевой объект** укажите значение **2**.
      5. Для параметра **Максимальное число заданий на узел** укажите значение **2**.
   5. Нажмите кнопку **ОК** , чтобы создать пул.

      Для создания пула Пакетной службы Azure можно также воспользоваться командлетом [New-AzureBatchPool](https://msdn.microsoft.com/library/mt628690.aspx) .     

### <a name="high-level-steps"></a>Пошаговые действия
1. **Создайте настраиваемое действие** для использования конвейера фабрики данных. Настраиваемое действие в этом примере будет содержать логику преобразования и обработки данных.
   1. В Visual Studio создайте проект библиотеки классов .NET, добавьте код для обработки входных данных и скомпилируйте проект.    
   2. Запакуйте все двоичные файлы и PDB-файл (необязательно) в ZIP-архив в выходной папке.    
   3. Отправьте ZIP-файл в хранилище BLOB-объектов Azure. Подробные указания см. в разделе «Создание пользовательского действия».
2. **Создайте фабрику данных Azure, которая использует пользовательское действие**:
   1. Создание фабрики данных Azure.
   2. создание связанных служб.
      1. AzureStorageLinkedService — предоставляет учетные данные хранилища для доступа к большим двоичным объектам.
      2. AzureBatchLinkedService — указывает пакетную службу Azure в качестве вычислительного ресурса.
   3. Создайте наборы данных.
      1. InputDataset — определяет контейнер хранилища и папку для входных больших двоичных объектов.
      2. OuputDataset — определяет контейнер хранилища и папку для выходных больших двоичных объектов.
   4. Создайте конвейер, который использует пользовательское действие.
   5. Запустите и протестируйте конвейер.
   6. Выполните отладку конвейера.

## <a name="create-the-custom-activity"></a>Создание пользовательского действия
Чтобы создать настраиваемое действие .NET, создайте проект **библиотеки классов .NET** с классом, который реализует интерфейс **IDotNetActivity**. У этого интерфейса есть только один метод [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) , и его сигнатура такова.

    public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)


Метод принимает четыре параметра:

* **linkedServices** — это перечисляемый список связанных служб, которые связывают источники входных и выходных данных (например, хранилище BLOB-объектов Azure) с фабрикой данных. В этом примере присутствует только одна связанная служба (служба хранилища Azure), которая используется для входных и выходных данных.
* **наборы данных**. Это свойство является перечисляемым списком наборов данных. Этот параметр можно использовать для получения расположений и схем, которые определяются входными и выходными наборами данных.
* **activity** — Это свойство представляет текущую вычислительную сущность, в данном случае пакетную службу Azure.
* **logger** — это объект, который позволяет записывать комментарии отладки, которые отображаются в виде пользовательского журнала для конвейера.

Этот метод возвращает словарь, который можно будет использовать для создания цепочки из настраиваемых действий в будущем. Эта функция еще не реализована, поэтому из метода просто возвращается пустой словарь.  

### <a name="procedure"></a>Описание процедуры
1. Создайте проект **библиотеки классов .NET** .
   <ol type="a">
     <li>Запустите <b>Visual Studio 2015</b>, <b>Visual Studio 2013</b> или <b>Visual Studio 2012</b>.</li>
     <li>Щелкните <b>Файл</b>, наведите указатель мыши на пункт <b>Создать</b> и щелкните <b>Проект</b>.</li>
     <li>Разверните раздел <b>Шаблоны</b> и выберите <b>Visual C#</b>. В этом руководстве используется язык C#, но для создания настраиваемого действия вы можете использовать любой язык .NET.;</li>
     <li>Выберите тип <b>Библиотека классов</b> из списка типов проектов справа.</li>
     <li>В поле <b>Имя</b> введите <b>MyDotNetActivity</b>.</li>
     <li>В качестве <b>расположения</b> укажите <b>C:\ADFGetStarted</b>.</li>
     <li>Нажмите кнопку <b>ОК</b> , чтобы создать проект.</li>
   </ol>
2. Щелкните **Инструменты**, наведите указатель мыши на пункт **Диспетчер пакетов NuGet** и щелкните **Консоль диспетчера пакетов**.
3. В консоли диспетчера пакетов выполните следующую команду, чтобы импортировать пакет **Microsoft.Azure.Management.DataFactories**.

     Install-Package Microsoft.Azure.Management.DataFactories
4. Импортируйте пакет NuGet для **службы хранилища Azure** в проект.

        Install-Package WindowsAzure.Storage -Version 4.3.0

    > [!NOTE]
    > Средство запуска службы фабрики данных требует пакет WindowsAzure.Storage версии 4.3. Если добавить ссылку в проекте настраиваемого действия в сборку службы хранилища Azure более поздней версии, при выполнении действия возникнет ошибка. Чтобы устранить ее, см. раздел [Изоляция домена приложения](#appdomain-isolation). 
5. Добавьте следующие инструкции с **using** в исходный файл в проекте.

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
6. Измените имя **пространства имен** на **MyDotNetActivityNS**.

        namespace MyDotNetActivityNS
7. Измените имя класса на **MyDotNetActivity** и извлеките класс из интерфейса **IDotNetActivity**, как показано в следующем фрагменте кода:

        public class MyDotNetActivity : IDotNetActivity
8. Реализуйте (добавьте) метод **Execute** интерфейса **IDotNetActivity** для класса **MyDotNetActivity** и скопируйте следующий пример кода в этот метод.

    Следующий пример подсчитывает количество вхождений поисковой фразы ("Microsoft") в каждый большой двоичный объект, связанный со срезом данных.

        /// <summary>
        /// Execute method is the only method of IDotNetActivity interface you must implement.
        /// In this sample, the method invokes the Calculate method to perform the core logic.  
        /// </summary>

        public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
        {
            // to get extended properties (for example: SliceStart)
            DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
            string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

            // to log all extended properties            
            IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
            logger.Write("Logging extended properties if any...");
            foreach (KeyValuePair<string, string> entry in extendedProperties)
            {
                logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
            }

            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            // declare dataset types
            CustomDataset inputLocation;
            AzureBlobDataset outputLocation;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
            inputLocation = inputDataset.Properties.TypeProperties as CustomDataset;

            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same
            // Azure Storage linked service for input and output.
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString;

            // To create an input storage client.
            string folderPath = GetFolderPath(inputDataset);
            string output = string.Empty; // for use later.

            // create storage client for input. Pass the connection string.
            CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
            CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

            // initialize the continuation token before using it in the do-while loop.
            BlobContinuationToken continuationToken = null;
            do
            {   // get the list of input blobs from the input storage client object.
                BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                         true,
                                         BlobListingDetails.Metadata,
                                         null,
                                         continuationToken,
                                         null,
                                         null);

                // Calculate method returns the number of occurrences of
                // the search term (“Microsoft”) in each blob associated
                   // with the data slice. definition of the method is shown in the next step.

                output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

            } while (continuationToken != null);

            // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
            // convert to blob location object.
            outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file.
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // The dictionary can be used to chain custom activities together in the future.
            // This feature is not implemented yet, so just return an empty dictionary.  

            return new Dictionary<string, string>();
        }
9. Добавьте следующие вспомогательные методы. Эти вспомогательные методы вызывает метод **Execute** . Метод **GetConnectionString** извлекает строку подключения к службе хранилища Azure, а **GetFolderPath** — расположение большого двоичного объекта. Самое главное, метод **Calculate** изолирует код, который выполняет итерацию каждого большого двоичного объекта.

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.
        /// </summary>

        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FolderPath;
        }

        /// <summary>
        /// Gets the fileName value from the input/output dataset.   
        /// </summary>

        private static string GetFileName(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FileName;
        }

        /// <summary>
        /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
        /// and prepares the output text that is written to the output blob.
        /// </summary>

        public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
        {
            string output = string.Empty;
            logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
            foreach (IListBlobItem listBlobItem in Bresult.Results)
            {
                CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
                if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
                {
                    string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                    logger.Write("input blob text: {0}", blobText);
                    string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                    var matchQuery = from word in source
                                     where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                     select word;
                    int wordCount = matchQuery.Count();
                    output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
                }
            }
            return output;
        }

    Метод GetFolderPath возвращает путь к папке, на которую указывает набор данных, а метод GetFileName — имя большого двоичного объекта или файла, на который указывает набор данных. Если параметр havefolderPath определяется с помощью переменных, таких как {Year}, {Month} {Day} и т. д, то метод возвращает строку как есть, не заменяя эти переменные соответствующими значениями на момент запуска. Дополнительные сведения о доступе к SliceStart, SliceEnd и т. д. см. в разделе [Доступ к расширенным свойствам](#access-extended-properties).    

            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "file.txt",
                    "folderPath": "adftutorial/inputfolder/",

    Метод Calculate вычисляет количество экземпляров ключевого слова Microsoft во входных файлах (в больших двоичных объектах в папке). Условие поиска (Microsoft) жестко задано в коде.
10. Скомпилируйте проект. В меню щелкните **Построить** и **Построить решение**.

    > [!IMPORTANT]
    > Задайте версию 4.5.2 платформы .NET Framework в качестве целевой для своего проекта: щелкните правой кнопкой мыши проект и выберите **Свойства**, чтобы задать целевую платформу. Фабрика данных не поддерживает настраиваемые действия, скомпилированные для более поздних версий, чем .NET Framework 4.5.2.
    >
    >
11. Запустите **проводник Windows** и перейдите к папке **bin\debug** или **bin\release** в зависимости от типа сборки.
12. Создайте ZIP-файл **MyDotNetActivity.zip**, который содержит все двоичные файлы из папки <project folder>\bin\Debug. Кроме того, можно добавить файл **MyDotNetActivity.pdb** , чтобы в случае сбоя получить дополнительные сведения, например номер строки в исходном коде, вызвавшем ошибку. Все файлы в ZIP-файле для настраиваемого действия должны размещаться на **верхнем уровне** без вложенных папок.

    ![Двоичные выходные файлы](./media/data-factory-use-custom-activities/Binaries.png)
13. Отправьте архив **MyDotNetActivity.zip** в качестве большого двоичного объекта в контейнер больших двоичных объектов **customactvitycontainer** в хранилище BLOB-объектов Azure, которое используется связанной службой **AzureStorageLinkedService** в **ADFTutorialDataFactory**.  Создайте контейнер больших двоичных объектов **customactivitycontainer** , если он еще не создан.

> [!NOTE]
> Если добавить этот проект действия .NET в решение в Visual Studio, содержащее проект фабрики данных, и добавить ссылку в проект действия .NET из проекта приложения фабрики данных, то необязательно выполнять два последних шага по созданию ZIP-файла и его добавлению в хранилище BLOB-объектов Azure. При публикации сущностей фабрики данных с помощью Visual Studio эти шаги выполняются автоматически в процессе публикации. Сведения о создании и публикации сущностей фабрики данных с помощью Visual Studio см. в статьях [Создание первого конвейера с помощью Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) и [Копирование данных из большого двоичного объекта Azure в SQL Azure](data-factory-copy-activity-tutorial-using-visual-studio.md).  
>
>

### <a name="execute-method"></a>Метод Execute
В этом разделе содержатся дополнительные сведения и примечания о коде в методе **Execute** .

1. Элементы для выполнения итерации входной коллекции находятся в пространстве имен [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx). Для итерации коллекции больших двоичных объектов необходимо использовать класс **BlobContinuationToken**. В сущности, необходимо использовать цикл do-while и маркер в качестве механизма для выхода из цикла. Дополнительные сведения см. в статье [Использование хранилища BLOB-объектов из .NET](../storage/storage-dotnet-how-to-use-blobs.md). Базовый цикл выглядит так:

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {   
            // Get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                      true,
                                      BlobListingDetails.Metadata,
                                      null,
                                      continuationToken,
                                      null,
                                      null);
            // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
        } while (continuationToken != null);

    Дополнительные сведения см. в документации по методу [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx).
2. Код для работы с набором больших двоичных объектов логически находится в цикле do-while. В методе **Execute** цикл do-while передает список больших двоичных объектов в метод **Calculate**. Этот метод возвращает строковую переменную с именем **output** , которая является результатом итерации всех больших двоичных объектов в сегменте.

   Он возвращает число вхождений условия поиска (**Microsoft**) в большом двоичном объекте, переданном в метод **Calculate**.

         output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
3. После завершения работы метода **Calculate** его результаты записываются в новый большой двоичный объект. Следовательно, для каждого обработанного набора больших двоичных объектов можно записать новый большой двоичный объект с результатами. Чтобы записать новый большой двоичный объект, сначала найдите выходной набор данных.

         // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
         Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

         // Convert to blob location object.
         outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;
4. Код также вызывает вспомогательный метод **GetFolderPath** , чтобы получить путь к папке (имя контейнера хранилища).

         folderPath = GetFolderPath(outputDataset);

   Метод **GetFolderPath** приводит объект DataSet к AzureBlobDataSet со свойством FolderPath.

         AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

         return blobDataset.FolderPath;
5. Код вызывает метод **GetFileName** , чтобы получить имя файла (имя большого двоичного объекта).  

         AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

         return blobDataset.FileName;
6. Чтобы записать имя файла, создается объект универсального кода ресурса (URI). Для возврата имени контейнера в конструкторе URI используется свойство **BlobEndpoint** . Для создания URI выходного большого двоичного объекта сочетаются путь к папке и имя файла.  

         // Write the name of the file.
         Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
7. Имя файла записано, и теперь можно записать выводимую строку из метода Calculate в новый большой двоичный объект:

         // Create a blob and upload the output text.
         CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
         logger.Write("Writing {0} to the output blob", output);
         outputBlob.UploadText(output);

## <a name="create-the-data-factory-using-azure-portal"></a>Создание фабрики данных с помощью портала Azure
В разделе **Создание настраиваемого действия** вы создали настраиваемое действие и отправили ZIP-файл с двоичными файлами и PDB-файлом в контейнер больших двоичных объектов Azure. В этом разделе вы создадите **фабрику данных** Azure с **конвейером**, использующим **настраиваемое действие**.

Входной набор данных для настраиваемого действия представляет собой большие двоичные объекты (файлы) во входной папке (adftutorial\inputfolder) в хранилище BLOB-объектов. Выходной набор данных для настраиваемого действия представляет собой выходные большие двоичные объекты в выходной папке (adftutorial\outputfolder) в хранилище BLOB-объектов.

Создайте файл **file.txt** со следующим содержимым и отправьте его в папку **adftutorial\inputfolder** (adftutorial — имя контейнера больших двоичных объектов Azure, а inputfolder — имя папки в этом контейнере).

    test custom activity Microsoft test custom activity Microsoft

Входная папка соответствует одному срезу в фабрике данных Azure, даже если она содержит два файла или более. При обработке каждого среза конвейером пользовательское действие выполняет итерацию всех больших двоичных объектов во входной папке для этого среза.

В папке adftutorial\output отображается один выходной файл с одной или несколькими строками (соответствует количеству больших двоичных объектов во входной папке).

    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.


Ниже приведены шаги, подробно описанные далее в этом разделе.

1. Создание **фабрики данных**.
2. **Связанные службы** для пула виртуальных машин пакетной службы Azure, в котором выполняется настраиваемое действие, а также служба хранилища Azure, содержащая входные и выходные большие двоичные объекты.
3. Входные и выходные **наборы данных** , которые представляют собой входные и выходные данные настраиваемого действия.
4. **Конвейер** , который использует настраиваемое действие.
5. **Фабрика данных**. Вы создадите его при публикации этих сущностей в Azure.

> [!NOTE]
> Создайте файл **file.txt** и передайте его в контейнер больших двоичных объектов, если еще не сделали это. См. инструкции выше.  
>
>

### <a name="step-1-create-the-data-factory"></a>Шаг 1. Создание фабрики данных
1. Войдите на портал Azure и выполните следующие действия:
   1. В меню слева нажмите кнопку **Создать** .
   2. В колонке **Создание** щелкните **Данные и аналитика**.
   3. Щелкните **Фабрика данных** в колонке **Аналитика данных**.
2. В колонке **Создание фабрики данных** в поле "Имя" введите **CustomActivityFactory**. Имя фабрики данных Azure должно быть глобально уникальным. Если возникнет ошибка **Имя фабрики данных CustomActivityFactory недоступно**, измените имя этой фабрики данных (например, на **ваше_имяCustomActivityFactory**) и попробуйте создать ее снова.
3. Щелкните **Имя группы ресурсов**, чтобы выбрать имеющуюся группу ресурсов, или создайте группу ресурсов.
4. Убедитесь, что используется именно те **подписка** и **регион**, в которых необходимо создать фабрику данных.
5. В колонке **Создание фабрики данных** нажмите кнопку **Создать**.
6. Созданная фабрика данных появится на **панели мониторинга** портала Azure.
7. После создания фабрики данных ее содержимое отобразится в соответствующей колонке.

### <a name="step-2-create-linked-services"></a>Шаг 2. Создание связанных служб
Связанные службы связывают хранилища данных или службы вычислений с фабрикой данных Azure. На этом этапе учетная запись службы хранилища Azure и учетная запись пакетной службы Azure будут связаны с фабрикой данных.

#### <a name="create-azure-storage-linked-service"></a>Создание связанной службы хранения Azure
1. В колонке **Фабрика данных** для **CustomActivityFactory** щелкните элемент **Создание и развертывание**. Отобразится редактор фабрики данных.
2. На панели команд щелкните **Создание хранилища данных** и выберите **Служба хранилища Azure**. В редакторе отобразится сценарий JSON для создания связанной службы хранилища Azure.

3. Замените **account name** именем своей учетной записи хранения Azure, а **account key** — ключом доступа к ней. Сведения о получении ключа доступа к хранилищу см. в разделах о [просмотре, копировании и повторном создании ключей доступа к хранилищу](../storage/storage-create-storage-account.md#manage-your-storage-account).

4. Чтобы развернуть эту службу, нажмите кнопку **Развернуть** на панели команд.

#### <a name="create-azure-batch-linked-service"></a>Создание связанной пакетной службы Azure
1. В редакторе фабрики данных щелкните на панели команд **Новое вычисление** и выберите в меню **Пакет Azure**.
2. Внесите следующие изменения в сценарий JSON:

   1. В свойстве **accountName** укажите имя учетной записи пакетной службы Azure. **URL-адрес** из **колонки учетной записи пакетной службы Azure** имеет следующий формат: http://**имя_учетной_записи**.<регион>.batch.azure.com. В свойстве **batchUri** в JSON требуется **удалить заполнитель accountname** из URL-адреса и указать **accountname** в свойстве JSON **accountName**.
   2. В свойстве **accessKey** укажите ключ учетной записи пакетной службы Azure.
   3. В свойстве **poolName** укажите имя пула, созданного при выполнении предварительных требований. Вместо имени пула также можно указать идентификатор пула.
   4. В свойстве **batchUri** укажите универсальный код ресурса (URI) пакетной службы Azure. Ознакомьтесь с примечаниями о свойстве **accountName** выше. Пример: https://westus.batch.azure.com.  
   5. Для свойства **AzureStorageLinkedService** for the **linkedServiceName** укажите имя учетной записи пакетной службы Azure.

           {
             "name": "AzureBatchLinkedService",
             "properties": {
               "type": "AzureBatch",
               "typeProperties": {
                 "accountName": "myazurebatchaccount",
                 "batchUri": "https://westus.batch.azure.com",
                 "accessKey": "<yourbatchaccountkey>",
                 "poolName": "myazurebatchpool",
                 "linkedServiceName": "AzureStorageLinkedService"
               }
             }
           }

       Для свойства **poolName** можно также указать идентификатор пула вместо его имени.

      > [!NOTE]
      > Служба фабрики данных не поддерживает параметр по требованию для пакетной службы Azure, как для HDInsight. Пул пакетной службы Azure можно использовать только в фабрике данных Azure.
      >
      >

### <a name="step-3-create-datasets"></a>Шаг 3. Создание наборов данных
На этом шаге вы создадите наборы данных, которые представляют входные и выходные данные.

#### <a name="create-input-dataset"></a>Создание входного набора данных
1. В **редакторе** фабрики данных нажмите на панели инструментов кнопку **Новый набор данных** и выберите в раскрывающемся меню пункт **Хранилище BLOB-объектов Azure**.
2. Замените код JSON в правой области на следующий фрагмент кода JSON:

         {
             "name": "InputDataset",
             "properties": {
                 "type": "AzureBlob",
                 "linkedServiceName": "AzureStorageLinkedService",
                 "typeProperties": {
                     "folderPath": "adftutorial/customactivityinput/",
                     "format": {
                         "type": "TextFormat"
                     }
                 },
                 "availability": {
                     "frequency": "Hour",
                     "interval": 1
                 },
                 "external": true,
                 "policy": {}
             }
         }

   Позже в этом пошаговом руководстве вы создадите конвейер со временем начала 2015-11-16T00:00:00Z и временем окончания 2015-11-16T05:00:00Z. Данные будут создаваться почасово, поэтому мы получим 5 входных и выходных срезов (между **00**:00:00 -> **05**:00:00).

   Для параметров **frequency** и **interval** входного набора данных установлены значения **Hour** и **1**. Это означает, что входной срез данных будет создаваться каждый час. В этом примере используется тот же файл (file.txt) в папке intputfolder.

   Это значения времени начала для каждого среза, представленные системной переменной SliceStart в приведенном выше фрагменте кода JSON.
3. На панели инструментов щелкните **Развернуть**, чтобы создать и развернуть **InputDataset**. Убедитесь, что на панели заголовка редактора отображается сообщение **ТАБЛИЦА УСПЕШНО СОЗДАНА** .

#### <a name="create-an-output-dataset"></a>Создание выходного набора данных
1. В **редакторе фабрики данных** нажмите кнопку **Создать набор данных** и щелкните **Хранилище BLOB-объектов Azure** на панели команд.
2. Замените сценарий JSON в правой области на следующий:

        {
            "name": "OutputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
                "typeProperties": {
                    "fileName": "{slice}.txt",
                    "folderPath": "adftutorial/customactivityoutput/",
                    "partitionedBy": [
                        {
                            "name": "slice",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy-MM-dd-HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }

     Выходное расположение — **adftutorial/customactivityoutput/**, а имя выходного файла — ГГГГ-ММ-ДД-ЧЧ.txt (где ГГГГ, ММ, ДД и ЧЧ — год, месяц, день и час создания среза). Подробную информацию см. в [справочнике разработчика][adf-developer-reference].

    Для каждого входного среза данных создается выходной большой двоичный объект или файл. Ниже в таблице приведены имена, которые даются выходным файлам для каждого среза. Все выходные файлы создаются в одной выходной папке: **adftutorial\customactivityoutput**.

   | Срез | Время начала | Выходной файл |
   |:--- |:--- |:--- |
   | 1 |2015-11-16T00:00:00 |2015-11-16-00.txt |
   | 2 |2015-11-16T01:00:00 |2015-11-16-01.txt |
   | 3 |2015-11-16T02:00:00 |2015-11-16-02.txt |
   | 4 |2015-11-16T03:00:00 |2015-11-16-03.txt |
   | 5 |2015-11-16T04:00:00 |2015-11-16-04.txt |

    Помните, что все файлы во входной папке являются частью среза со значениями времени начала, указанными выше. Во время обработки этого среза пользовательское действие сканирует каждый файл и создает строку в выходном файле с количеством вхождений условия поиска (Microsoft). Если в папке inputfolder находятся три файла, в выходном файле будут содержаться три строки для каждого почасового среза: 2015-11-16-00.txt, 2015-11-16:01:00:00.txt и т. д.
3. На панели команд нажмите кнопку **Развернуть**, чтобы развернуть **OutputDataset**.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Создание и запуск конвейера, который использует настраиваемое действие
1. В редакторе фабрики данных, нажмите кнопку **Создать конвейер** на панели команд. Если команда не отображается, нажмите кнопку **... (многоточие)**, чтобы отобразить ее.
2. Замените сценарий JSON в правой области на следующий:

        {
          "name": "ADFTutorialPipelineCustom",
          "properties": {
            "description": "Use custom activity",
            "activities": [
              {
                "Name": "MyDotNetActivity",
                "Type": "DotNetActivity",
                "Inputs": [
                  {
                    "Name": "InputDataset"
                  }
                ],
                "Outputs": [
                  {
                    "Name": "OutputDataset"
                  }
                ],
                "LinkedServiceName": "AzureBatchLinkedService",
                "typeProperties": {
                  "AssemblyName": "MyDotNetActivity.dll",
                  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                  "PackageLinkedService": "AzureStorageLinkedService",
                  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
                  "extendedProperties": {
                    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
                  }
                },
                "Policy": {
                  "Concurrency": 2,
                  "ExecutionPriorityOrder": "OldestFirst",
                  "Retry": 3,
                  "Timeout": "00:30:00",
                  "Delay": "00:00:00"
                }
              }
            ],
            "start": "2015-11-16T00:00:00Z",
            "end": "2015-11-16T05:00:00Z",
            "isPaused": false
          }
        }

    Обратите внимание на следующие моменты.

   * Для свойства **Concurrency** установлено значение **2**, чтобы два среза параллельно обрабатывались двумя виртуальными машинами в пуле пакетной службы Azure.
   * в разделе действий содержится одно действие типа **DotNetActivity**;
   * В **AssemblyName** задано имя библиотеки DLL **MyDotNetActivity.dll**.
   * В **EntryPoint** — **MyDotNetActivityNS.MyDotNetActivity**.
   * Для параметра **PackageLinkedService** задано значение **AzureStorageLinkedService**, которое указывает на хранилище BLOB-объектов, содержащее ZIP-файл настраиваемого действия. Если для входных и выходных файлов и ZIP-файла настраиваемого действия используются разные учетные записи службы хранилища Azure, необходимо создать другую связанную службу хранилища Azure. В этой статье предполагается использование одной и той же учетной записи хранения Azure.
   * Для **PackageFile** установлено значение **customactivitycontainer/MyDotNetActivity.zip**. Используется формат <контейнер_для_zip-файла>/<имя_zip-файла.zip>.
   * Настраиваемое действие принимает **InputDataset** в качестве входных данных и **OutputDataset** — в качестве выходных данных.
   * Свойство linkedServiceNam настраиваемого действия указывает на свойство **AzureBatchLinkedService**, которое сообщает фабрике данных Azure, что необходимо запустить настраиваемое действие на виртуальных машинах пакетной службы Azure.
   * Для свойства **isPaused** по умолчанию установлено значение **false**. В этом примере конвейер запускается незамедлительно, потому что срезы приходятся на прошлое. Для этого свойства можно задать значение true, чтобы приостановить работу конвейера. Чтобы перезапустить его, нужно снова установить значение false.
   * Разница между временем **начала** и временем **окончания** составляет **пять** часов, а срезы создаются каждый час. Таким образом конвейер создает пять срезов.
3. Чтобы развернуть конвейер, нажмите кнопку **Развернуть** на панели команд.

### <a name="monitor-the-pipeline"></a>Мониторинг конвейера
1. На портале Azure в колонке "Фабрика данных" щелкните **Схема**.

    ![Плитка "Схема"](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Теперь в представлении схемы щелкните OutputDataset.

    ![Представление схемы](./media/data-factory-use-custom-activities/diagram.png)
3. Если срезы уже созданы, вы увидите пять выходных срезов данных в состоянии готовности.

   ![Выходные срезы](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Убедитесь, что выходные файлы создаются в хранилище больших двоичных объектов в контейнере **adftutorial** .

   ![выходные данные настраиваемого действия][image-data-factory-ouput-from-custom-activity]
5. Если вы откроете выходной файл, вы увидите выходные данные, похожие на следующие:

    В файле inputfolder/2015-11-16-00/file.txt найдено 2 вхождения условия поиска Microsoft.
6. Используйте [портал Azure][azure-preview-portal] или командлеты Azure PowerShell, чтобы отслеживать состояние фабрики данных, конвейеров и наборов данных. Вы можете просматривать сообщения **ActivityLogger** о настраиваемом действии в журналах (а именно — user-0.log), которые можно скачать на портале или с помощью командлетов.

   ![скачивание журналов из настраиваемого действия][image-data-factory-download-logs-from-custom-activity]

Подробные указания по мониторингу наборов данных и конвейеров см. в статье [Мониторинг конвейеров фабрики данных Azure и управление ими](data-factory-monitor-manage-pipelines.md).      

### <a name="data-factory-and-batch-integration"></a>Интеграция фабрики данных и пакетной службы
Служба фабрики данных создает в пакетной службе Azure задание с именем **adf-poolname:job-xxx**.

![Фабрика данных Azure — задания пакетной службы](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Такое задание создается для каждого запуска действия среза. При наличии 10 срезов, готовых к обработке, в рамках этого задания создаются 10 задач. Если в пуле доступны несколько вычислительных узлов, можно обрабатывать параллельно сразу несколько срезов. Если для максимального количества задач на вычислительный узел установлено значение > 1, также можно выполнять сразу несколько срезов в одной среде выполнения приложений.

![Фабрика данных Azure — задачи задания пакетной службы](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

На следующей схеме показана связь между задачами фабрики данных и пакетной службы Azure.

![Фабрика данных и пакетная служба](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="debug-the-pipeline"></a>Выполнение отладки конвейера
Отладка состоит из нескольких базовых методов:

1. Если отображается следующее сообщение об ошибке, убедитесь, что имя класса в CS-файле соответствует имени, указанному для свойства **EntryPoint** в конвейере JSON. В приведенном выше пошаговом руководстве имя класса — MyDotNetActivity, а для свойства EntryPoint в конвейере JSON указано значение MyDotNetActivityNS.**MyDotNetActivity**.

         MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly

   Если имена соответствуют, убедитесь, что все двоичные файлы размещены в **корневой папке** ZIP-файла. Это означает, что при открытии ZIP-файла все файлы должны находиться в корневой папке, а не во вложенных.   
2. Если для входного среза данных не установлено значение **Готов**, убедитесь, что структура входной папки правильная и что в ней существует файл **file.txt**.
3. В методе **Execute** настраиваемого действия используйте объект **IActivityLogger**, чтобы записывать в журнал сведения, которые помогут устранить неполадки. Сообщения, записываемые в журналы, появятся в файлах журнала пользователя (один или несколько файлов с именами user-0.log, user-1.log, user-2.log и т. д.).

   В колонке **OutputDataset** щелкните срез, чтобы открыть для него колонку **Срез данных**. Для этого среза будет указано значение **Запуски операции** . Должно выполняться лишь одно действие. Если в командной строке нажать кнопку «Запуск», можно запустить другое действие для этого среза.

   Если щелкнуть выполняемое действие, появится колонка **Подробности о выполнении операции** со списком файлов журнала. Записанные в журнал сообщения можно увидеть в файле user_0.log. Если возникнет ошибка, действие будет выполнено три раза, так как для количества повторных попыток в JSON-файле конвейера или действия задано значение 3. Если щелкнуть выполняемое действие, отобразятся файлы журнала, которые можно использовать для устранения ошибки.

   В списке файлов журнала щелкните **user-0.log**. На панели справа отображаются результаты использования метода **IActivityLogger.Write** . Если вы не видите всех сообщений, проверьте наличие дополнительных файлов журнала user_1.log, user_2.log и т. д. Если нет, сбой в коде, возможно, произошел уже после последнего сообщения, записанного в журнал.

   Следует также проверить файл **system-0.log** на наличие любых системных сообщений об ошибках или исключений.
4. Добавьте **PDB-файл** в ZIP-файл, чтобы в случае возникновения ошибки сведения об ошибке содержали, например, информацию о **стеке вызовов**.
5. Все файлы в ZIP-файле для настраиваемого действия должны размещаться на **верхнем уровне** без вложенных папок.
6. Убедитесь, что для параметров **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) и **packageLinkedService** (должен указывать на хранилище BLOB-объектов Azure, содержащее ZIP-файл) установлены правильные значения.
7. Если ошибки устранены и необходимо повторно обработать срез, в колонке **OutputDataset** щелкните срез правой кнопкой мыши и выберите пункт **Запуск**.
8. Если возникла указанная ниже ошибка, это значит, что вы используете пакет службы хранилища Azure более поздней версии, чем 4.3.0. Средство запуска службы фабрики данных требует пакет WindowsAzure.Storage версии 4.3. Сведения о том, что делать, если вам нужно использовать более позднюю версию сборки службы хранилища Azure, см. в разделе [Изоляции домена приложения](#appdomain-isolation). 

        Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 

    Если вы можете использовать пакет службы хранилища Azure версии 4.3.0, удалите существующую ссылку на пакет Azure.Storage более поздней версии, чем 4.3.0, и выполните следующую команду в консоли диспетчера пакетов Nuget. 

       Install-Package WindowsAzure.Storage -Version 4.3.0

    Создайте проект. Удалите сборку WindowsAzure.Storage более поздней версии, чем 4.3.0, из папки bin\Debug. Создайте ZIP-файл с двоичными файлами и PDB-файлом. Замените старый ZIP-файл в контейнере больших двоичных объектов (customactivitycontainer) созданным. Повторно запустите срезы, в которых произошли сбои (щелкните срез правой кнопкой мыши и выберите пункт "Выполнить").   
8. Настраиваемое действие не использует файл **app.config** из пакета, поэтому если код считывает строки подключения из файла конфигурации, то действие не работает в среде выполнения. При использовании пакетной службы Azure рекомендуется хранить все секреты в **хранилище ключей Azure**. Кроме того, следует использовать субъект-службу на основе сертификата для защиты **хранилища ключей** и переместить сертификат в пул пакетной службы Azure. В этом случае пользовательское действие .NET в среде выполнения сможет использовать секреты из хранилища ключей. Это общее решение, которое можно использовать для любого типа секретов, а не только строк подключения.

   Существует и более простое (но не самое лучшее) решение: можно создать **связанную службу SQL Azure** с параметрами строки подключения, набор данных, использующий связанную службу, и привязать его как фиктивный набор входных данных к настраиваемому действию .NET. После этого вы сможете получить доступ к строке подключения связанной службы из кода пользовательского действия — в среде выполнения она должна работать нормально.  

## <a name="update-the-custom-activity"></a>Обновление пользовательского действия
Если вы обновляете код для настраиваемого действия, создайте его и отправьте ZIP-файл, содержащий новые двоичные файлы, в службу хранилища больших двоичных объектов.

## <a name="copymove-data"></a>Копирование и перемещение данных

Действие копирования копирует данные из хранилища данных-**источника** в хранилище данных-**приемник**. В разделе [Поддерживаемые хранилища данных](data-factory-data-movement-activities.md#supported-data-stores-and-formats) приведен список хранилищ данных, которые поддерживаются в качестве источников и приемников для действия копирования.

Если необходимо переместить данные в хранилище или из хранилища данных, которое не поддерживается **действием копирования**, вы можете использовать в фабрике данных **настраиваемое действие** с собственной логикой для копирования и перемещения данных. Ознакомьтесь с [примером загрузчика данных HTTP](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) на сайте GitHub.  

## <a name="appdomain-isolation"></a>Изоляция домена приложения
В разделе с [примером перекрестного домена приложения](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) показано, как создать настраиваемое действие .NET для фабрики данных Azure, которое не ограничено версиями сборок, используемых средством запуска фабрики данных Azure (например, WindowsAzure.Storage версии 4.3.0, Newtonsoft.Json версии 6.0.x и т. д.).

## <a name="access-extended-properties"></a>Доступ к расширенным свойствам
Вы можете объявить расширенные свойства в действии JSON, как показано ниже.

    "typeProperties": {
      "AssemblyName": "MyDotNetActivity.dll",
      "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
      "PackageLinkedService": "AzureStorageLinkedService",
      "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
      "extendedProperties": {
        "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
        "DataFactoryName": "CustomActivityFactory"
      }
    },


В коде есть два расширенных свойства: **SliceStart** и **DataFactoryName**. Значение свойства SliceStart основано на системной переменной SliceStart. Список поддерживаемых системных переменных см. [в этом разделе](data-factory-scheduling-and-execution.md#data-factory-functions-and-system-variables). Значение свойства DataFactoryName жестко задано как CustomActivityFactory.

Для доступа к этим расширенным свойствам в методе **Execute** используйте код, аналогичный приведенному ниже.

    // to get extended properties (for example: SliceStart)
    DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
    string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

    // to log all extended properties                               
    IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
    logger.Write("Logging extended properties if any...");
    foreach (KeyValuePair<string, string> entry in extendedProperties)
    {
        logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
    }

## <a name="auto-scaling-feature-of-azure-batch"></a>Функция автомасштабирования пакетной службы Azure
Можно также создать пул пакетной службы Azure с использованием функции **автомасштабирования** . Например, можно создать пул пакетной службы Azure с нулем выделенных виртуальных машин и формулой автоматического масштабирования на основе числа ожидающих задач:

По одной виртуальной машине на одну ожидающую задачу (например, пять ожидающих задач -> пять виртуальных машин):

    pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
    $TargetDedicated = max(pendingTaskSampleVector);

Максимум одна виртуальная машина за один раз независимо от количества ожидающих задач:

    pendingTaskSampleVector=$PendingTasks.GetSample(600 * TimeInterval_Second);
    $TargetDedicated = (max(pendingTaskSampleVector)>0)?1:0;

Дополнительные сведения см. в статье [Автоматическое масштабирование вычислительных узлов в пуле пакетной службы Azure](../batch/batch-automatic-scaling.md).

Если в пуле используется [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx)(значение по умолчанию), пакетной службе может потребоваться 15–30 минут на подготовку виртуальной машины перед выполнением настраиваемого действия.  Если пул использует другое значение autoScaleEvaluationInterval, пакетная служба может затрачивать autoScaleEvaluationInterval плюс 10 минут.

## <a name="use-azure-hdinsight-linked-services"></a>Использование связанных служб Azure HDInsight
В пошаговом руководстве для запуска пользовательского действия был использован вычислительный ресурс пакетной службы Azure. Также можно использовать собственный кластер HDInsight или создать кластер HDInsight по требованию с помощью фабрики данных и запустить пользовательское действие на кластере HDInsight. Ниже приведены основные шаги для использования кластера HDInsight.  

1. Создайте связанную службу Azure HDInsight.   
2. Используйте связанную службу HDInsight вместо **AzureBatchLinkedService** в конвейере JSON.

Чтобы протестировать сценарий с использованием службы Azure HDInsight, может потребоваться изменить время **начала** и **окончания** для конвейера.

#### <a name="create-azure-hdinsight-linked-service"></a>Создание связанной службы Azure HDInsight
Служба фабрики данных Azure поддерживает создание кластера по запросу и использует его для обработки входных данных, чтобы создать выходные данные. Вы также можете использовать для этого собственный кластер. Когда вы используете кластер HDInsight по запросу, для каждого среза создается отдельный кластер. Если вы используете собственный кластер HDInsight, он сразу сможет обработать срез. Поэтому при использовании кластера по запросу выходные данные могут выводится не так быстро, как при использовании собственного кластера.

> [!NOTE]
> Во время выполнения экземпляр действия .NET выполняется только на одном рабочем узле в кластере HDInsight; его невозможно расширить для выполнения на нескольких узлах. Несколько экземпляров действия .NET действия могут выполняться параллельно на разных узлах кластера HDInsight.
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a>Использование кластера HDInsight по запросу
1. На **портале Azure**щелкните **Создать и развернуть** .
2. В редакторе фабрики данных щелкните **Новое вычисление** на панели команд и выберите в меню **On-demand HDInsight cluster** (Кластер HDInsight по запросу).
3. Внесите следующие изменения в сценарий JSON:

   1. В свойстве **clusterSize** укажите размер кластера HDInsight.
   2. В свойстве **timeToLive** укажите, как долго может простаивать клиент, прежде чем он будет удален.
   3. В свойстве **version** укажите версию HDInsight, которую хотите использовать. Если исключить это свойство, будет использоваться последняя версия.  
   4. В свойстве **linkedServiceName** укажите службу **AzureStorageLinkedService**, которую вы создали при изучении учебника по началу работы.

           {
               "name": "HDInsightOnDemandLinkedService",
               "properties": {
                   "type": "HDInsightOnDemand",
                   "typeProperties": {
                       "clusterSize": 4,
                       "timeToLive": "00:05:00",
                       "osType": "Windows",
                       "linkedServiceName": "AzureStorageLinkedService",
                   }
               }
           }
4. Чтобы развернуть эту службу, нажмите кнопку **Развернуть** на панели команд.

##### <a name="to-use-your-own-hdinsight-cluster"></a>Использование собственного кластера HDInsight
1. На **портале Azure**щелкните **Создать и развернуть** .
2. В **редакторе фабрики данных** щелкните **Новое вычисление** на панели команд и выберите в меню **Кластер HDInsight**.
3. Внесите следующие изменения в сценарий JSON:

   1. В свойстве **clusterUri** укажите URL-адрес кластера HDInsight. Например, https://<clustername>.azurehdinsight.net/     
   2. В свойстве **UserName** введите имя пользователя, у которого есть доступ к кластеру HDInsight.
   3. В свойстве **Password** укажите пароль этого пользователя.
   4. В свойстве **LinkedServiceName** укажите **AzureStorageLinkedService**. Вы создали связанную службу в руководстве по началу работы.
4. Чтобы развернуть эту службу, нажмите кнопку **Развернуть** на панели команд.

Дополнительные сведения см. в статье [Связанные службы вычислений](data-factory-compute-linked-services.md).

В **конвейере JSON**используйте связанную службу HDInsight (по запросу или собственную):

    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "HDInsightOnDemandLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2015-11-16T00:00:00Z",
        "end": "2015-11-16T05:00:00Z",
        "isPaused": false
      }
    }

## <a name="examples"></a>Примеры
| Образец | Результат настраиваемого действия |
| --- | --- |
| [Загрузчик данных HTTP](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |Загрузка данных из конечной точки HTTP в хранилище BLOB-объектов с помощью настраиваемого действия C# в фабрике данных Azure. |
| [Пример анализа мнений с помощью Twitter](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Вызов модели машинного обучения Azure и выполнение анализа мнений, оценки, прогнозирования и т. д. |
| [Запуск скрипта R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Вызов сценария R путем запуска RScript.exe в кластере HDInsight, где уже установлен R. |
| [Действие перекрестного домена приложения .NET](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Использование разных версий сборок, которые используются средством запуска фабрики данных. |

## <a name="see-also"></a>См. также
[Обновления фабрики данных Azure: выполнение настраиваемых действий .NET ADF с помощью Пакетной службы Azure](https://azure.microsoft.com/blog/2015/05/01/azure-data-factory-updates-execute-adf-custom-net-activities-using-azure-batch/).

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[azure-developer-center]: http://azure.microsoft.com/develop/net/
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png



<!--HONumber=Nov16_HO3-->


