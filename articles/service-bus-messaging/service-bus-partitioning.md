---
title: "Секционированные очереди и разделы | Документация Майкрософт"
description: "В этой статье описывается, как секционировать очереди и разделы служебной шины с помощью нескольких брокеров сообщений."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/02/2016
ms.author: sethm;hillaryc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 03e68436372414a25d87f50c2b057c2f8d193401


---
# <a name="partitioned-queues-and-topics"></a>Секционированные очереди и разделы
Служебная шина Azure использует несколько посредников сообщений для обработки сообщений и несколько хранилищ сообщений для их хранения. Обычная очередь или раздел обрабатываются одним брокером сообщений и сохраняются в одном хранилище сообщений. Служебная шина также позволяет разделять (секционировать) очереди или разделы между несколькими брокерами сообщений и хранилищами сообщений. Это означает, что общая пропускная способность секционированной очереди или раздела больше не ограничивается производительностью одного брокера сообщений или хранилища сообщений. Кроме того, при возникновении временного сбоя хранилища сообщений секционированная очередь или раздел останутся доступными. Секционированные очереди и разделы могут предоставлять все расширенные функции служебной шины, в том числе поддержку транзакций и сеансов.

Дополнительные сведения о внутренних компонентах служебной шины см. в разделе [Архитектура служебной шины][Архитектура служебной шины].

## <a name="how-it-works"></a>Принцип работы
Каждая секционированная очередь или раздел состоит из нескольких фрагментов. Фрагменты хранятся в разных хранилищах сообщений и обрабатываются разными посредниками сообщений. При отправке сообщения в секционированную очередь или раздел служебная шина назначает сообщение в один из фрагментов. Выбор фрагмента осуществляется произвольно или по ключу секции, который может указать отправитель.

Если клиент хочет получить сообщение из секционированной очереди или подписки секционированного раздела, служебная шина запрашивает все фрагменты сообщений и возвращает получателю первое сообщение, которое возвращается из любого хранилища. Служебная шина кэширует другие сообщения и возвращает их при получении дополнительных запросов. Клиент ничего не знает о секционировании, для него поведение секционированных очередей или разделов (например, чтение, завершение, откладывание, недоставление или предварительная выборка) аналогично поведению обычной сущности.

При отправке или получении сообщения из секционированной очереди или раздела не возникает никаких дополнительных затрат.

## <a name="enable-partitioning"></a>Включение секционирования
Для использования секционированных очередей и разделов служебной шины Azure необходимо использовать пакет SDK для Azure 2.2 или более поздней версии или указать `api-version=2013-10` в HTTP-запросах.

Можно создавать очереди и разделы служебной шины размером в 1, 2, 3, 4 или 5 ГБ (по умолчанию — 1 ГБ). Если секционирование включено, то для каждого гигабайта служебной шиной создается 16 секций. Поэтому при создании очереди размером 5 ГБ с 16 разделами максимальный размер очереди составит 80 ГБ (5 \* 16). Чтобы просмотреть максимальный размер секционированной очереди или раздела, откройте соответствующую запись на [портале Azure][портал Azure].

Создать секционированную очередь или раздел можно несколькими способами. При создании очереди или раздела из приложения можно включить секционирование очереди или раздела, задав значение **true** для свойства [QueueDescription.EnablePartitioning][QueueDescription.EnablePartitioning] или [TopicDescription.EnablePartitioning][TopicDescription.EnablePartitioning] соответственно. Эти свойства должны быть заданы при создании очереди или раздела. Для существующей очереди или раздела изменить эти свойства невозможно. Например:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Кроме того, секционированную очередь или раздел можно создать в Visual Studio или на [портале Azure][портал Azure]. При создании очереди или раздела на портале задайте значение **true** для параметра **Включить секционирование**, расположенного в колонке **Общие настройки** окна **Параметры** очереди или раздела. В Visual Studio установите флажок **Включить секционирование** в диалоговом окне **Новая очередь** или **Новый раздел**.

## <a name="use-of-partition-keys"></a>Использование ключей секции
Когда сообщение добавляется в секционированную очередь или раздел, служебная шина проверяет наличие ключа секции. Если ключ есть, то фрагмент выбирается на основе этого ключа. Если ключа нет, то фрагмент выбирается на основе внутреннего алгоритма.

### <a name="using-a-partition-key"></a>Использование ключа секции
В некоторых сценариях, например для сессий или транзакций, необходимо, чтобы сообщения хранились в определенном фрагменте. Для всех этих сценариев необходимо использовать ключ секции. Все сообщения, использующие один и тот же ключ секции, назначаются одному и тому же фрагменту. Если фрагмент временно недоступен, служебная шина возвращает ошибку.

В зависимости от сценария в качестве ключа секции используются разные свойства сообщения:

**SessionId.** Если для сообщения установлено свойство [BrokeredMessage.SessionId][BrokeredMessage.SessionId], то служебная шина использует это свойство в качестве ключа секции. Таким образом, все сообщения, которые относятся к одному сеансу, обрабатываются одним и тем же посредником сообщений. Это позволяет служебной шине гарантировать порядок сообщений и согласованность состояний сеансов.

**PartitionKey.** Если для сообщения установлено свойство [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], но не установлено свойство [BrokeredMessage.SessionId][BrokeredMessage.SessionId], то служебная шина использует свойство [PartitionKey][PartitionKey] в качестве ключа секции. Если для сообщения установлены оба свойства [SessionId][SessionId] и [PartitionKey][PartitionKey], то их значения должны совпадать. Если значение свойства [PartitionKey][PartitionKey] отличается от значения свойства [SessionId][SessionId], служебная шина возвращает исключение **InvalidOperationException**. Свойство [PartitionKey][PartitionKey] нужно использовать, если отправитель отправляет транзакционные сообщения без учета сеансов. Ключ секции гарантирует, что все сообщения, отправленные в транзакции, будут обработаны одним и тем же посредником сообщений.

**MessageId.** Если для очереди или раздела для свойства [QueueDescription.RequiresDuplicateDetection][QueueDescription.RequiresDuplicateDetection] задано значение **true**, а свойство [BrokeredMessage.SessionId][BrokeredMessage.SessionId] или [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey] не задано, то в качестве ключа секции выступает свойство [BrokeredMessage.MessageId][BrokeredMessage.MessageId]. (Обратите внимание, что библиотеки Microsoft .NET и AMQP назначают идентификатор сообщения автоматически, если отправляющее приложение этого не сделало). В этом случае все копии одного сообщения обрабатываются одним и тем же посредником сообщений. Это позволяет служебной шине находить и исключать повторяющиеся сообщения. Если свойство [QueueDescription.RequiresDuplicateDetection][QueueDescription.RequiresDuplicateDetection] не установлено в значение **true**, служебная шина не рассматривает свойство [MessageId][MessageId] в качестве ключа секции.

### <a name="not-using-a-partition-key"></a>Неиспользование ключа секции
При отсутствии ключа секции служебная шина распределяет сообщение по принципу циклического перебора всем фрагментам секционированной очереди или раздела. Если выбранный фрагмент недоступен, служебная шина назначает сообщение другому фрагменту. Таким образом, операция отправки выполняется даже при временной недоступности хранилища сообщений.

Чтобы у служебной шины было достаточно времени для того, чтобы поместить сообщение в другой фрагмент, значение свойства [MessagingFactorySettings.OperationTimeout][MessagingFactorySettings.OperationTimeout], указываемое клиентом, который отправляет сообщение, должно быть больше 15 секунд. Рекомендуется установить свойство [OperationTimeout][OperationTimeout] в значение по умолчанию 60 секунд.

Обратите внимание, что ключ секции "прикрепляет" сообщение к определенному фрагменту. Если хранилище сообщений, содержащее этот фрагмент, недоступно, служебная шина возвращает ошибку. При отсутствии ключа секции служебная шина может выбрать другой фрагмент, и операция будет выполнена успешно. Поэтому рекомендуется не указывать ключ раздела, если он не является обязательным.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Дополнительные разделы: использование транзакций и секционированных сущностей
Сообщения, отправленные в рамках транзакции, должны указать ключ секции. Это может быть одно из следующих свойств: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey] или [BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Сообщения, отправляемые в рамках одной транзакции, должны иметь один и тот же ключ секции. При попытке отправить сообщение без ключа секции в рамках транзакции служебная шина возвращает исключение **InvalidOperationException**. При попытке отправить несколько сообщений с различными ключами секции в рамках одной транзакции служебная шина возвращает исключение **InvalidOperationException**. Например:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Если установлено любое из свойств, которые служат в качестве ключа секции, служебная шина прикрепляет сообщение к определенному фрагменту. Это происходит независимо от того, используется ли транзакция. Рекомендуется не указывать ключ секции, если в этом нет необходимости.

## <a name="using-sessions-with-partitioned-entities"></a>Использование сеансов и секционированных сущностей
Чтобы отправить транзакционное сообщение в раздел или очередь, учитывающие сеансы, для этого сообщения должно быть задано свойство [BrokeredMessage.SessionId][BrokeredMessage.SessionId]. Если также установлено свойство [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], оно должно быть идентично свойству [SessionId][SessionId]. Если они различаются, служебная шина возвращает исключение **InvalidOperationException**.

В отличие от обычных (несекционированных) очередей и разделов, использовать одну транзакцию для отправки нескольких сообщений в различные сеансы невозможно. При попытке это сделать служебная шина возвращает исключение **InvalidOperationException**. Например:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Автоматическая переадресация сообщений для секционированных сущностей
Служебная шина Azure поддерживает автоматическую переадресацию сообщений в секционированные сущности, из них или между ними. Чтобы включить автоматическую переадресацию сообщений, установите свойство [QueueDescription.ForwardTo][QueueDescription.ForwardTo] исходной очереди или подписки. Если в сообщении указан ключ секции ([SessionId][SessionId], [PartitionKey][PartitionKey] или [MessageId[MessageId]]), то этот ключ секции используется для сущности назначения.

## <a name="considerations-and-guidelines"></a>Рекомендации и советы
* **Функции высокого уровня согласованности**. Если сущность использует такие функции, как сеансы, обнаружение дубликатов или явное управление ключом секционирования, то операции обмена сообщениями всегда направляются в определенные фрагменты. Если любой из фрагментов сталкивается с высоким трафиком или базовое хранилище неработоспособно, то эти операции завершаются сбоем и их доступность ограничивается. В целом уровень согласованности гораздо выше, чем при использовании несекционированных сущностей, поэтому проблемы возникают лишь в некоторой части трафика.
* **Управление**. Такие операции, как создание, обновление и удаление, должны выполняться во всех фрагментах сущности. Если какой-либо из фрагментов находится в неработоспособном состоянии, это может привести к сбою этих операций. Для операции GET требуется, чтобы сведения, например количество сообщений, были собраны для всех фрагментов. Если какой-либо из фрагментов находится в неработоспособном состоянии, состояние доступности сущности отображается как ограниченное.
* **Сообщения небольших объемов**. Чтобы получить все сообщения в таких сценариях, особенно при использовании протокола HTTP, может потребоваться выполнить несколько операций получения. Для запросов на получение внешний интерфейс выполняет операцию получения во всех фрагментах и кэширует все полученные ответы. Это кэширование повышает производительность последующего запроса на получение при том же подключении, уменьшая задержки получения. Тем не менее при наличии нескольких подключений или использовании протокола HTTP для каждого запроса устанавливается новое подключение. Таким образом, нет никакой гарантии, что запрос будет выполняться на том же узле. Если все имеющиеся сообщения блокируются и кэшируются в другом внешнем интерфейсе, операция получения возвращает значение **NULL**. Когда в конечном итоге срок действия сообщений истекает, их снова можно получать. Рекомендуется выполнить проверку активности HTTP.
* **Обзор и просмотр сообщений**. Метод PeekBatch не всегда возвращает количество сообщений, указанное в свойстве [MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx). На это есть две причины. Первая причина — объединенный размер коллекции сообщений превышает максимальный размер в 256 КБ. Вторая причина заключается в том, что если для [свойства EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) очереди или раздела задано значение **true**, то секция может содержать недостаточно сообщений для выполнения запроса. Как правило, если приложению требуется получить определенное количество сообщений, оно должно вызывать метод [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx), пока не получит желаемое количество сообщений или пока не закончатся сообщения, которые можно просмотреть. Дополнительные сведения, включая примеры кода, см. в разделах [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) или [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Новые функции
* Теперь для секционированных сущностей поддерживаются операции добавления и удаления правила. В отличие от несекционированных сущностей эти операции не поддерживаются при выполнении транзакций. 
* Теперь AMQP может использоваться для обмена сообщениями (отправка и получение) с секционированными сущностями.
* Теперь протокол AMQP поддерживается для следующих операций: [пакетная отправка](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [пакетное получение](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [получение по порядковому номеру](https://msdn.microsoft.com/library/azure/hh330765.aspx), [просмотр](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [блокировка обновления](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [планирование сообщений](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [отмена запланированных сообщений](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [добавление правила](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [удаление правила](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [блокировка обновления сеанса](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [установка состояния сеанса](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [получение состояния сеанса](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [просмотр сообщений сеанса](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx) и [перечисление сеансов](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Ограничения секционированных сущностей
В настоящее время служебная шина накладывает следующие ограничения на секционированные очереди и разделы:

* Секционированные очереди и разделы не поддерживают отправку сообщений, принадлежащих разным сеансам, в одной транзакции.
* В настоящее время служебная шина обеспечивает до 100 секционированных очередей или разделов на пространство имен. Каждая секционированная очередь или раздел учитывается в квоте в 10 000 сущностей на пространство имен (не относится к уровню "Премиум").

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о секционировании сущностей обмена сообщениями см. в обсуждении [поддержки AMQP 1.0 для секционированных очередей и разделов служебной шины][Поддержка AMQP 1.0 для секционированных очередей и разделов служебной шины]. 

[Архитектура служебной шины]: service-bus-architecture.md
[портал Azure]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
[TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
[BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
[BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
[SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
[PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
[QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
[BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
[MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
[QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
[MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
[Поддержка AMQP 1.0 для секционированных очередей и разделов служебной шины]: service-bus-partitioned-queues-and-topics-amqp-overview.md



<!--HONumber=Nov16_HO3-->


