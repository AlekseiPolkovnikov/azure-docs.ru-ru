---
title: "Обработка сообщений, отправляемых с устройства Центра Интернета вещей в облако (.NET) | Документация Майкрософт"
description: "Этот учебник поможет изучить полезные шаблоны обработки сообщений, отправляемых с устройства в облако, в центре IoT"
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: 3b65a12369fbd6f0a2993d875225d49fecb4f39b


---
# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Учебник: как обрабатывать сообщения, отправляемые с устройства центра IoT в облако, с помощью .Net
[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Введение
Центр Интернета вещей Azure — это полностью управляемая служба, которая обеспечивает надежный и защищенный двунаправленный обмен данными между миллионами устройств и серверной частью приложения. В других руководствах ([Приступая к работе с Центром Интернета вещей Azure (.NET)] и [Как обрабатывать сообщения, отправляемые с устройства Центра Интернета вещей в облако, с помощью .NET][lnk-c2d]) описаны базовые функции Центра Интернета вещей по отправке сообщений между устройством и облаком.

Это руководство основано на коде, показанном в руководстве [Приступая к работе с Центром Интернета вещей Azure (.NET)]. В нем демонстрируется два масштабируемых шаблона, с помощью которых можно обрабатывать сообщения, отправляемые с устройства в облако:

* Надежное хранилище сообщений, отправляемых с устройства в облако, в [хранилище BLOB-объектов Azure]. Распространенный сценарий, подразумевающий реализацию " *холодного* " анализа, где телеметрические данные хранятся в больших двоичных объектах, которые используются в качестве входных данных для процедур анализа. Этими процедурами могут управлять такие инструменты, как [фабрика данных Azure] или стек [HDInsight (Hadoop)].
* Надежная обработка *интерактивных* сообщений, отправляемых с устройства в облако. Сообщения, отправляемые с устройства в облако, являются интерактивными, если они немедленно инициируют набор действий в серверной части приложения. Например, устройство может отправить аварийный сигнал, который активирует вставку билета в системе CRM. В отличие от сообщений *точек данных* , которые просто поступают в модуль аналитики. Например, телеметрическое сообщение о температуре с устройства, которое должно быть сохранено для последующего анализа, является сообщением точки данных.

Так как для получения сообщений, отправляемых с устройства в облако, Центр Интернета вещей Azure предоставляет конечную точку, совместимую с [концентраторами событий][lnk-event-hubs], в этом руководстве используется экземпляр класса [EventProcessorHost]. Этот экземпляр:

* надежно хранит сообщения *точек данных* в хранилище BLOB-объектов Azure;
* перенаправляет *интерактивные* сообщения, отправляемые с устройства в облако, в [очередь служебной шины Azure] для немедленной обработки.

Служебная шина помогает обеспечить надежную обработку интерактивных сообщений, так как она обеспечивает контрольные точки для каждого сообщения и дедупликацию на основе временных интервалов.

> [!NOTE]
> Экземпляр **EventProcessorHost** — только один из способов обработки интерактивных сообщений. Другие варианты включают в себя [Azure Service Fabric][lnk-service-fabric] и [Azure Stream Analytics][lnk-stream-analytics].
> 
> 

В конце этого учебника у вас будет три консольных приложения Windows:

* **SimulatedDevice**, измененная версия приложения, созданного в руководстве [Приступая к работе с Центром Интернета вещей Azure (.NET)]. Приложение отправляет сообщения точек данных с устройства в облако один раз в секунду и интерактивные сообщения с устройства в облако каждые 10 секунд. Это приложение использует протокол AMQP для обмена данными с центром Интернета вещей.
* **ProcessDeviceToCloudMessages** использует класс [EventProcessorHost] для получения сообщений от конечной точки, совместимой с концентратором событий. Затем приложение надежно сохраняет сообщения точек данных в хранилище BLOB-объектов Azure и перенаправляет интерактивные сообщения в очередь служебной шины.
* **ProcessD2CInteractiveMessages** удаляет интерактивные сообщения из очереди служебной шины.

> [!NOTE]
> Для центра IoT существуют пакеты SDK для многих платформ устройств и языков (включая C, Java и JavaScript). Чтобы узнать, как заменить имитацию устройства в этом руководстве физическим устройством, а также подключить устройства к Центру Интернета вещей Azure, см. [Центр разработчиков для Azure IoT].
> 
> 

Содержимое данного руководства можно применить в других способах использования сообщений, совместимых с концентратором событий, например в проектах [HDInsight (Hadoop)]. Дополнительные сведения см. в [разделе "Отправка сообщений с устройства в облако" руководства разработчика по Центру Интернета вещей Azure].

Для работы с этим учебником требуется:

* Microsoft Visual Studio 2015.
* Активная учетная запись Azure. <br/>Если ее нет, можно создать [бесплатную учетную запись](https://azure.microsoft.com/free/) всего за несколько минут.

Кроме того, у вас должны быть базовые знания о [службе хранилища Azure] и [документации по служебной шине Azure].

## <a name="send-interactive-messages-from-a-simulated-device-app"></a>Отправка интерактивных сообщений из приложения виртуального устройства
В этом разделе вы измените приложение виртуального устройства, созданное в руководстве [Приступая к работе с Центром Интернета вещей Azure (.NET)], чтобы отправлять в Центр Интернета вещей интерактивные сообщения от устройства в облако.

1. В Visual Studio в проекте **SimulatedDevice** добавьте в класс **Program** следующий метод:
   
    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();
   
        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);
   
        Task.Delay(10000).Wait();
      }
    }
    ```
   
    Этот метод похож на метод **SendDeviceToCloudMessagesAsync** в проекте **SimulatedDevice**. Единственные отличия — свойство **MessageId** (настраивается сейчас) и пользовательское свойство **messageType**.
    Код присваивает глобальный уникальный идентификатор (GUID) свойству **MessageId** . Служебная шина может использовать его для дедупликации получаемых сообщений. В примере используется свойство **messageType** , чтобы отличить интерактивные сообщения от сообщений точки данных. Приложение передает эти сведения в свойствах сообщения, а не в его основной части, чтобы обработчику событий не требовалось десериализовывать сообщение для маршрутизации.
   
   > [!NOTE]
   > Очень важно создать **MessageId** , используемый в коде устройства для дедупликации интерактивных сообщений. Прерывистая сетевая связь или другие сбои могут привести к многократной повторной передаче одного сообщения от этого устройства. Вместо идентификатора GUID также можно использовать семантический идентификатор сообщения, например хэш релевантных полей данных этого сообщения.
   > 
   > 
2. Добавьте в метод **Main** непосредственно перед строкой `Console.ReadLine()` следующий метод:
   
    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````
   
   > [!NOTE]
   > Для упрощения в этом руководстве не реализуются какие-либо политики повтора. В рабочем коде следует реализовать политику повтора (например, экспоненциальную задержку), как предлагается в статье MSDN [Transient Fault Handling](Обработка временного сбоя).
   > 
   > 

## <a name="process-device-to-cloud-messages"></a>Обработка сообщений с устройства в облако
В этом разделе вы создадите консольное приложение для Windows, которое обрабатывает сообщения, отправляемые с устройства в облако из центра IoT. Центр Интернета вещей предоставляет совместимую с концентратором событий конечную точку, с помощью которой приложение считывает сообщения, отправляемые с устройств в облако. В этом руководстве для обработки таких сообщений в консольном приложении используется класс [EventProcessorHost]. Дополнительные сведения о способах обработки сообщений из концентраторов событий см. в руководстве [Приступая к работе с концентраторами событий].

Сложность, с которой вы сталкиваетесь при реализации надежного хранилища сообщений точки данных или перенаправления интерактивных сообщений, заключается в том, что контроль хода выполнения обработки событий основан на потребителе сообщений. Кроме того, чтобы добиться высокой пропускной способности, при считывании из концентраторов событий необходимо установить контрольные точки в больших пакетах. При таком подходе создается возможность повторной обработки большого количества сообщений в случае возникновения сбоя и возвращения к предыдущей контрольной точке. Из этого руководства вы узнаете, как синхронизировать операции записи в службу хранилища Azure и периоды дедупликации служебной шины с помощью контрольных точек **EventProcessorHost**.

Чтобы обеспечить надежную запись сообщений в службу хранилища Azure, в примере используется функция фиксации отдельных блоков [блочных BLOB-объектов][Azure Block Blobs]. Обработчик событий накапливает сообщения в памяти, пока не наступает время создать контрольную точку. Например, после того, как максимальный размер блока в накопленном буфере сообщений достигнет 4 МБ, или после того, как истечет период дедупликации служебной шины. Затем перед установкой контрольной точки в большом двоичном объекте с помощью кода фиксируется новый блок.

Обработчик событий использует смещения сообщений концентраторов событий в качестве идентификаторов блоков. Этот механизм позволяет обработчику событий выполнить проверку дедупликации перед фиксацией нового блока в хранилище, устраняя вероятность сбоя между фиксацией блока и контрольной точкой.

> [!NOTE]
> В этом руководстве используется одна учетная запись хранения Azure для записи всех сообщений, полученных из Центра Интернета вещей. Чтобы определить, требуется ли в вашем решении использовать несколько учетных записей хранения Azure, см. статью [Целевые показатели масштабируемости и производительности службы хранилища Azure].
> 
> 

Чтобы избежать повторений при обработке интерактивных сообщений, в приложении используется функция дедупликации служебной шины. Приложение виртуального устройства присваивает каждому интерактивному сообщению уникальный идентификатор **MessageId**. Благодаря этим идентификаторам служебная шина предотвращает доставку получателям двух сообщений с одним и тем же **MessageId** в течение указанного периода дедупликации. Такая дедупликация совместно с семантикой завершения каждого сообщения, предоставляемой очередями служебной шины, значительно упрощают реализацию надежной обработки интерактивных сообщений.

Чтобы предотвратить повторную отправку сообщений до или после дедупликации, код синхронизирует механизм контрольных точек **EventProcessorHost** с периодом дедупликации очереди служебной шины. Эта синхронизация достигается путем хотя бы однократного принудительного создания контрольной точки при каждом окончании периода дедупликации (в данном учебнике этот период составляет один час).

> [!NOTE]
> В данном руководстве для обработки всех интерактивных сообщений, полученных из центра IoT, используется одна секционированная очередь служебной шины. Дополнительные сведения об использовании очередей служебной шины в соответствии с требованиями к масштабируемости решения см. в [документации по служебной шине Azure].
> 
> 

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Подготовка учетной записи хранения Azure и очереди служебной шины
Чтобы использовать класс [EventProcessorHost] , вам понадобится учетная запись хранения Azure, в которую **EventProcessorHost** будет записывать сведения о контрольных точках. Вы можете использовать существующую учетную запись хранения или создать новую, следуя инструкциям в разделе [Создайте учетную запись хранения Azure]. Запишите строку подключения учетной записи хранения Azure.

> [!NOTE]
> Копируя и вставляя строку подключения к учетной записи хранения Azure, будьте внимательны, чтобы к имени не добавились пробелы.
> 
> 

Требуется также реализовать надежную обработку интерактивных сообщений в очереди служебной шины. Можно программно создать очередь с периодом дедупликации в один час, как описано в статье об [использовании очереди служебной шины][очередь служебной шины Azure]. Кроме того, можно воспользоваться [классическим порталом Azure][lnk-classic-portal], где нужно сделать следующее:

1. В левом нижнем углу нажмите кнопку **Создать** . Затем щелкните **Службы приложений** > **Служебная шина** > **Очередь** > **Настраиваемое создание**. Введите имя **d2ctutorial**, выберите регион и воспользуйтесь имеющимся пространством имен или создайте новое. На следующей странице установите флажок **Включить обнаружение повторений** и для параметра **Временное окно журнала обнаружения повторений** задайте значение в один час. Чтобы сохранить конфигурацию очереди, щелкните значок галочки в правом нижнем углу.
   
    ![Создание очереди на портале Azure][30]
2. В списке очередей служебной шины щелкните **d2ctutorial**, а затем нажмите кнопку **Настроить**. Создайте две политики общего доступа: **send** с разрешениями **Отправка** и **listen** с разрешениями **Прослушивание**. По завершении нажмите кнопку **Сохранить** в нижней части окна.
   
    ![Настройка очереди на портале Azure][31]
3. В верхней части страницы выберите элемент **Панель мониторинга** и щелкните внизу **Сведения о подключении**. Запишите две строки подключения.
   
    ![Панель мониторинга очереди на портале Azure][32]

### <a name="create-the-event-processor"></a>Создание обработчика событий
1. Чтобы создать проект Windows на языке Visual C#, используя шаблон проекта **Консольное приложение**, выберите **Файл** > **Добавить** > **Новый проект** в текущем решении Visual Studio. Убедитесь, что указана версия платформы .NET 4.5.1 или более поздняя версия. Присвойте проекту имя **ProcessDeviceToCloudMessages** и нажмите кнопку **ОК**.
   
    ![Создание проекта в Visual Studio][10]
2. В обозревателе решений щелкните правой кнопкой мыши проект **ProcessDeviceToCloudMessages** и выберите пункт **Управление пакетами NuGet**. Откроется диалоговое окно **Диспетчер пакетов NuGet** .
3. Найдите **WindowsAzure.ServiceBus**, щелкните **Установить** и примите условия использования. Будет скачана, установлена и добавлена ссылка на [пакет NuGet служебной шины Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus) со всеми зависимостями.
4. Найдите **Microsoft.Azure.ServiceBus.EventProcessorHost**, щелкните **Установить** и примите условия использования. Будет скачана, установлена и добавлена ссылка на [пакет NuGet концентратора событий служебной шины Azure — EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) со всеми зависимостями.
5. Щелкните правой кнопкой мыши проект **ProcessDeviceToCloudMessages**, выберите команду **Добавить**, а затем щелкните **Класс**. Присвойте новому классу имя **StoreEventProcessor**, а затем нажмите кнопку **ОК**, чтобы создать класс.
6. Добавьте в начало файла StoreEventProcessor.cs следующие инструкции:
   
    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
7. Замените следующий код текстом класса:
   
    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;
   
      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;
   
      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);
   
      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);
   
      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }
   
      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }
   
      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
   
        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();
   
        return Task.FromResult<object>(null);
      }
   
      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();
   
          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];
   
            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);
   
            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }
   
          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);
   
          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }
   
      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);
   
        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);
   
        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));
   
          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);
   
          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }
   
        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);
   
        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));
   
        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }
   
      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```
   
    Класс **EventProcessorHost** вызывается для обработки сообщений из центра IoT, отправляемых с устройства в облако. В этом классе код реализует логику для надежного хранения сообщений в контейнере больших двоичных объектов и перенаправляет интерактивные сообщения в очередь служебной шины.
   
    Метод **OpenAsync** инициализирует переменную **currentBlockInitOffset**, отслеживающую текущее смещение первого сообщения, которое считал этот обработчик событий. Помните, что каждый процессор отвечает за один раздел.
   
    Метод **ProcessEventsAsync** получает пакет сообщений из Центра Интернета вещей и обрабатывает их следующим образом: интерактивные сообщения отправляются в очередь служебной шины, а сообщения точки данных добавляются в буфер памяти с именем **toAppend**. Если буфер памяти достигает предельного размера в 4 МБ или заканчивается период дедупликации (в данном учебнике — через один час после создания контрольной точки), то приложение активирует создание контрольной точки.
   
    Сначала метод **AppendAndCheckpoint** создает идентификатор blockId для добавляемого блока. В службе хранилища Azure все идентификаторы блоков должны быть одной длины, поэтому смещение дополняется начальными нулями — `currentBlockInitOffset.ToString("0000000000000000000000000")`. Если блок с таким идентификатором уже находится в большом двоичном объекте, метод перезаписывает его текущим содержимым буфера.
   
   > [!NOTE]
   > В целях упрощения кода в этом руководстве для хранения сообщений используется по одному большому двоичному объекту для каждого раздела. В реальном решении ротация файлов реализуется за счет создания дополнительных файлов после истечения заданного промежутка времени или при достижении определенного размера. Помните, что блочный BLOB-объект Azure может содержать до 195 ГБ данных.
   > 
   > 
8. В классе **Program** добавьте в начало следующие инструкции **using**:
   
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
9. Измените метод **Main** в классе **Program**, как показано ниже. Замените **{строка подключения Центра Интернета вещей}** строкой подключения **iothubowner** из руководства [Приступая к работе с Центром Интернета вещей Azure (.NET)]. Замените строку подключения к хранилищу строкой подключения, которую вы записали в начале данного раздела. Замените строку подключения служебной шины разрешениями **Отправка** для очереди **d2ctutorial**, записанными в начале этого раздела:
   
    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";
   
      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();
   
      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```
   
   > [!NOTE]
   > Для простоты в этом руководстве используется один экземпляр класса [EventProcessorHost]. Дополнительные сведения см. в [руководстве по программированию концентраторов событий].
   > 
   > 

## <a name="receive-interactive-messages"></a>Прием интерактивных сообщений
В этом разделе вам предстоит создать консольное приложение для Windows, которое принимает интерактивные сообщения из очереди служебной шины. Дополнительные сведения о том, как разработать решение с использованием служебной шины, см. в статье [Многоуровневое приложение .NET, использующее очереди служебной шины Azure][Build multi-tier applications with Service Bus].

1. В текущем решении Visual Studio создайте проект Windows на языке Visual C#, используя шаблон проекта **Консольное приложение** . Присвойте проекту имя **ProcessD2CInteractiveMessages**.
2. В обозревателе решений щелкните правой кнопкой мыши проект **ProcessD2CInteractiveMessages** и выберите пункт **Управление пакетами NuGet**. Откроется окно **Диспетчер пакетов NuGet**.
3. Найдите **WindowsAzure.ServiceBus**, щелкните **Установить** и примите условия использования. После этого будут выполнены скачивание, установка и добавление ссылки на [служебную шину Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus)со всеми ее зависимостями.
4. Добавьте следующие инструкции **using** в начало файла **Program.cs**:
   
    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```
5. Наконец, добавьте следующие строки в метод **Main** . Подставьте строку подключения с разрешениями на **прослушивание** для очереди с именем **d2ctutorial**:
   
    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");
   
    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);
   
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);
   
    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();
   
        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);
   
        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);
   
    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Запуск приложений
Теперь все готово к запуску приложений.

1. В обозревателе решений Visual Studio щелкните правой кнопкой мыши решение и выберите пункт **Настроить запускаемые проекты**. Выберите **Несколько запускаемых проектов**, а затем выберите действие **Запуск** для проектов **ProcessDeviceToCloudMessages**, **SimulatedDevice** и **ProcessD2CInteractiveMessages**.
2. Нажмите клавишу **F5** для запуска трех консольных приложений. Приложение **ProcessD2CInteractiveMessages** должно обрабатывать каждое интерактивное сообщение, отправленное из приложения **SimulatedDevice**.
   
   ![Три консольных приложения][50]

> [!NOTE]
> Чтобы увидеть обновления в большом двоичном объекте, может потребоваться уменьшить значение константы **MAX_BLOCK_SIZE** в классе **StoreEventProcessor** (например, до **1024**). Это удобное изменение, так как на достижение предельного размера блока с данными, отправляемыми приложением виртуального устройства, уходит некоторое время. Если указать меньший размер блока, то создание и обновление большого двоичного объекта выполняется быстрее. Тем не менее, наличие блоков большего размера делает приложение более масштабируемым.
> 
> 

## <a name="next-steps"></a>Дальнейшие действия
В этом руководстве мы рассмотрели надежную обработку сообщений точек данных и интерактивных сообщений, отправляемых с устройства в облако, с помощью класса [EventProcessorHost] .

В [руководстве по отправке сообщений из облака на устройства с помощью Центра Интернета вещей][lnk-c2d] описано, как отправлять сообщения на устройства из серверной части.

Примеры комплексных решений, в которых используется Центр Интернета вещей, см. в [документации по Azure IoT Suite][lnk-suite].

Дополнительные сведения о разработке решений с помощью Центра Интернета вещей см. в статье [Руководство разработчика по центру Azure IoT (IoT — Интернет вещей)].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[хранилище BLOB-объектов Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[фабрика данных Azure]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[очередь служебной шины Azure]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[разделе "Отправка сообщений с устройства в облако" руководства разработчика по Центру Интернета вещей Azure]: iot-hub-devguide-messaging.md

[службе хранилища Azure]: https://azure.microsoft.com/documentation/services/storage/
[документации по служебной шине Azure]: https://azure.microsoft.com/documentation/services/service-bus/

[Руководство разработчика по центру Azure IoT (IoT — Интернет вещей)]: iot-hub-devguide.md
[Приступая к работе с Центром Интернета вещей Azure (.NET)]: iot-hub-csharp-csharp-getstarted.md
[Центр разработчиков для Azure IoT]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Создайте учетную запись хранения Azure]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Приступая к работе с концентраторами событий]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Целевые показатели масштабируемости и производительности службы хранилища Azure]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[руководстве по программированию концентраторов событий]: ../event-hubs/event-hubs-programming-guide.md
[Обработка временного сбоя]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Build multi-tier applications with Service Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/



<!--HONumber=Nov16_HO5-->


