---
title: "Часто задаваемые вопросы о служебной шине | Документация Майкрософт"
description: "Ответы на некоторые часто задаваемые вопросы о служебной шине Azure."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/04/2016
ms.author: sethm;juconway
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: b350bac36f1d46c97da37a2807ead8c9c732d69a


---
# <a name="service-bus-faq"></a>Часто задаваемые вопросы о служебной шине
В этой статье содержатся ответы на некоторые часто задаваемые вопросы о служебной шине Microsoft Azure. Вы также можете посетить страницу [Часто задаваемые вопросы о поддержке Azure](http://go.microsoft.com/fwlink/?LinkID=185083) , где содержатся общие сведения о расценках и поддержке Azure. В этой статье содержатся следующие разделы:

* [Общие вопросы об обмене сообщениями через служебную шину Azure](#general-questions-about-azure-service-bus-messaging)
* [Рекомендации по работе со служебной шиной](#service-bus-best-practices)
* [Служебная шина: цены](#service-bus-pricing)
* [Квоты на служебную шину](#service-bus-quotas)
* [Управление подпиской и пространством имен](#subscription-and-namespace-management)
* [Устранение неполадок](#service-bus-troubleshooting)

## <a name="general-questions-about-azure-service-bus-messaging"></a>Общие вопросы об обмене сообщениями через служебную шину Azure
### <a name="what-is-azure-service-bus-messaging"></a>Что такое обмен сообщениями через служебную шину Azure?
[Обмен сообщениями через служебную шину Azure](service-bus-messaging-overview.md) — это облачный механизм платформы асинхронного обмена сообщениями, который позволяет отправлять данные между несвязанными системами. Майкрософт предоставляет этот механизм в качестве службы. Это значит, что для его использования не потребуется устанавливать собственное оборудование.

### <a name="what-is-a-service-bus-namespace"></a>Что такое пространство имен служебной шины?
[Пространство имен](service-bus-create-namespace-portal.md) предоставляет контейнер для адресации ресурсов служебной шины в вашем приложении. Необходимо создать пространство имен, чтобы использовать служебную шину. Это будет одним из первых шагов.

### <a name="what-is-an-azure-service-bus-queue"></a>Что такое очередь служебной шины Azure?
[Очередь служебной шины](service-bus-queues-topics-subscriptions.md) — это сущность, в которой сохраняются сообщения. Их целесообразно использовать при наличии нескольких приложений или нескольких частей распределенного приложения, которые должны взаимодействовать друг с другом. Очередь подобна центру распределения, куда поступают несколько продуктов (сообщений), которые затем отправляются из этого расположения.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Что такое разделы и подписки служебной шины Azure?
Раздел можно представить в виде очереди. При использовании нескольких подписок он становится расширенной моделью обмена сообщениями. По сути, это инструмент связи по принципу "один ко многим". В рамках этой модели публикации и подписки (*pub/sub*) сообщение может быть отправлено из приложения в раздел с несколькими подписками, а затем получено несколькими приложениями.

### <a name="what-is-a-partitioned-entity"></a>Что такое секционированная сущность?
Обычная очередь или раздел обрабатываются одним брокером сообщений и сохраняются в одном хранилище сообщений. [Секционированная очередь или раздел](service-bus-partitioning.md) обрабатывается несколькими брокерами сообщений и хранится в нескольких хранилищах сообщений. Это означает, что общая пропускная способность секционированной очереди или раздела больше не ограничивается производительностью одного брокера сообщений или хранилища сообщений. Кроме того, при возникновении временного сбоя хранилища сообщений секционированная очередь или раздел останутся доступными.

Имейте в виду, что при использовании секционирования сущностей их упорядоченность не гарантируется. Если раздел недоступен, вы по-прежнему можете отправлять и получать сообщения из других секций.

## <a name="service-bus-best-practices"></a>Рекомендации по работе со служебной шиной
### <a name="what-are-some-azure-service-bus-best-practices"></a>Рекомендации по работе со служебной шиной Azure
* [Рекомендации по повышению производительности с помощью обмена сообщениями через брокер в служебной шине][Рекомендации по повышению производительности с помощью обмена сообщениями через брокер в служебной шине]. В этой статье описывается, как оптимизировать производительность во время обмена сообщениями через брокер.

### <a name="what-should-i-know-before-creating-messaging-entities"></a>Что необходимо знать перед созданием сущностей обмена сообщениями?
Приведенные ниже свойства очереди и раздела являются неизменяемыми. Учитывайте это при подготовке сущностей, так как эти свойства можно изменить, только создав сущность для замены.

* Размер
* Секционирование
* Сеансы:
* Обнаружение дубликатов
* экспресс-сущность.

## <a name="service-bus-pricing"></a>Сведения о ценах на служебную шину
В этом разделе содержатся ответы на некоторые часто задаваемые вопросы о ценах на использование служебной шины. Также можно посетить страницу [часто задаваемых вопросов о поддержке Azure](http://go.microsoft.com/fwlink/?LinkID=185083), где содержатся общие сведения о структуре ценообразования Microsoft Azure. Дополнительные сведения о ценах на использование служебной шины см. на странице [цен на служебную шину](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-service-bus"></a>Как выставляется цена на служебную шину?
Подробные сведения о ценах на использование служебной шины см. разделе [Сведения о ценах на использование служебной шины] [Обзор цен]. Помимо указанной платы, также взимается плата за связанные операции передачи данных, исходящих из центра обработки данных, в котором подготавливается ваше приложение.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Какое использование служебной шины считается передачей данных? А какое не считается?
Любая передача данных в пределах заданного региона Azure выполняется бесплатно. Любая передача исходящих данных за пределы региона оплачивается по тарифу 0,15 доллара США за 1 ГБ исходящих данных из Северной Америки и Европы и 0,20 доллара США за 1 ГБ для данных из Азиатско-Тихоокеанского региона. Передача входящих данных осуществляется бесплатно.

### <a name="does-service-bus-charge-for-storage"></a>Взимает ли служебная шина плату за использование хранилища?
Нет, служебная шина не взимает плату за использование хранилища. Тем не менее существует квота, ограничивающая максимальный объем данных, которые могут быть сохранены в каждой очереди или разделе. См. следующий раздел часто задаваемых вопросов.

## <a name="service-bus-quotas"></a>Квоты на служебную шину
Список ограничений и квот на служебную шину см. в этом [обзоре][Общие сведения о квотах].

### <a name="does-service-bus-have-any-usage-quotas"></a>Есть ли у служебной шины квоты использования?
По умолчанию для любой облачной службы Майкрософт устанавливается квота совокупного месячного использования в рамках всех подписок клиента. Мы понимаем, что вам может потребоваться больше, чем разрешено этими ограничениями. В таком случае вы можете в любое время обратиться в службу поддержки пользователей, чтобы мы изменили эти ограничения на основе ваших потребностей. Для служебной шины установлены следующие квоты общего использования:

* 5 миллиардов сообщений;

Хотя мы оставляем за собой право отключить учетную запись клиента, превысившего свои месячные квоты использования, перед выполнением каких-либо действий мы отправим уведомления по электронной почте, а также предпримем несколько попыток, чтобы связаться с клиентом. Клиентам, превысившим свои квоты, будут выставлены соответствующие счета.

Как и в случае с другими службами в Azure, служебная шина использует набор особых квот для обеспечения справедливого использования ресурсов. Ниже приведены квоты использования, применяемые службой.

#### <a name="queuetopic-size"></a>Размер очереди или раздела
Вы задаете максимальный размер очереди или раздела при их создании. Эта квота может иметь значение 1, 2, 3, 4 или 5 ГБ. При достижении максимального значения квоты последующие входящие сообщения отклоняются, а вызывающий код получает исключение.

#### <a name="naming-restrictions"></a>Ограничения для имен
Имя пространства имен служебной шины должно иметь длину от 6 до 50 символов. Имя очереди, раздела или подписки должно иметь длину от 1 до 50 символов.

#### <a name="number-of-concurrent-connections"></a>Количество одновременных подключений
Очереди, разделы и подписки. Максимально допустимое количество параллельных TCP-подключений в очереди, разделе или подписке равно 100. После достижения квоты запросы на дополнительные соединения отклоняются, а вызывающий код получает исключение. Для каждой фабрики обмена сообщениями служебная шина поддерживает одно TCP-подключение, если у любого из клиентов, созданного такой фабрикой обмена сообщениями, есть активная операция в состоянии ожидания или операция, завершенная менее 60 секунд назад. Операции REST не считаются параллельными TCP-подключениями.

#### <a name="number-of-topicsqueues-per-service-namespace"></a>Количество разделов или очередей на одно пространство имен службы
Максимально допустимое количество разделов или очередей (сущностей долгосрочного резервного хранения) на одно пространство имен службы равно 10 000. После достижения этой квоты последующие запросы на создание нового раздела или очереди в пространстве имен службы будут отклонены. В таком случае на классическом портале Azure появится сообщение об ошибке или вызывающий код клиента получит исключение (в зависимости от того, выполнялась ли попытка создания с использованием портала или кода клиента).

### <a name="message-size-quotas"></a>Квоты на размер сообщения
#### <a name="queuetopicsubscription"></a>Очередь, раздел и подписка
**Размер сообщения**. Каждое сообщение ограничено суммарным размером 256 КБ, включая заголовки сообщения.

**Размер заголовка сообщения**. Каждый заголовок сообщения ограничен 64 КБ.

Сообщения, размер которых превышает эти значения, будут отклонены, а вызывающий код получит исключение.

**Количество подписок на раздел**. Максимально допустимое количество подписок на один раздел равно 2000. После достижения этой квоты последующие запросы на создание дополнительных подписок для раздела будут отклонены. В таком случае на классическом портале Azure появится сообщение об ошибке или вызывающий код клиента получит исключение (в зависимости от того, выполнялась ли попытка создания с использованием портала или кода клиента).

**Количество фильтров SQL на раздел**. Максимально допустимое количество фильтров SQL на один раздел равно 2000. После достижения квоты последующие запросы на создание дополнительных фильтров для раздела отклоняются, а вызывающий код получает исключение.

**Количество фильтров корреляции на раздел**. Максимально допустимое количество фильтров корреляции на один раздел равно 100 000. После достижения квоты последующие запросы на создание дополнительных фильтров для раздела отклоняются, а вызывающий код получает исключение.

## <a name="subscription-and-namespace-management"></a>Управление подпиской и пространством имен
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Как перенести пространство имен в другую подписку Azure?
Чтобы переместить пространство имен из одной подписки Azure в другую, можно использовать команды PowerShell (приведенные [здесь][здесь]). При перемещении пространство имен уже должно быть активно. Кроме того, пользователь, который выполняет команды, должен обладать правами администратора как в исходной, так и в целевой подписках.

## <a name="service-bus-troubleshooting"></a>Устранение неполадок, связанных со служебной шиной
[Общие сведения об исключениях][Общие сведения об исключениях]

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-messaging-apis-and-their-suggested-actions"></a>Какие исключения создаются API обмена сообщениями через служебную шину Azure? Какие действия можно предпринять?
Исключения, создаваемые API обмена сообщениями, делятся на следующие категории:

* ошибка кода пользователя;
* ошибка установки и конфигурации;
* временные исключения;
* другие исключения.

В статье [Исключения обмена сообщениями служебной шины] [Общие сведения об исключениях] описываются некоторые исключения и рассматриваются действия по их устранению.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Что такое подписанный URL-адрес? На каких языках можно создавать подписи?
Подписанные URL-адреса представляют собой механизм аутентификации на базе алгоритма безопасного хэширования SHA-256 или URI. Дополнительные сведения о том, как создавать собственные подписи с использованием Node, PHP, Java и C\#, см. в статье о [подписанных URL-адресах][Подписи коллективного доступа].

## <a name="next-steps"></a>Дальнейшие действия
Дополнительную информацию об обмене сообщениями через служебную шину см. в следующих разделах.

* [Общие сведения об обмене сообщениями через служебную шину Azure уровня Premium (запись в блоге)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Общие сведения об обмене сообщениями через служебную шину Azure уровня Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Основные сведения об обмене сообщениями через служебную шину](service-bus-messaging-overview.md)
* [Обзор архитектуры служебной шины Azure](service-bus-fundamentals-hybrid-solutions.md)
* [Начало работы с очередями служебной шины](service-bus-dotnet-get-started-with-queues.md)

[Рекомендации по повышению производительности с помощью обмена сообщениями через брокер в служебной шине]: service-bus-performance-improvements.md
[Рекомендации по изолированию приложений от простоев и аварий служебной шины]: service-bus-outages-disasters.md
[Общие сведения о ценах]: https://azure.microsoft.com/pricing/details/service-bus/
[Общие сведения о квотах]: service-bus-quotas.md
[здесь]: service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription
[Общие сведения об исключениях]: service-bus-messaging-exceptions.md
[Подписи коллективного доступа]: service-bus-sas-overview.md



<!--HONumber=Nov16_HO3-->


