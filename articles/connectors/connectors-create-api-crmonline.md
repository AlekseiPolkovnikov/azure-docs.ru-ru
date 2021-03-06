---
title: "Добавление соединителя Dynamics CRM Online в приложения логики | Документация Майкрософт"
description: "Создание приложений логики с помощью службы приложений Azure. Поставщик подключений Dynamics CRM Online предоставляет API для работы с сущностями в Dynamics CRM Online."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 71f9dd111ebdbe885f33d162b2ea320dfaa167bb
ms.openlocfilehash: 317d146dec09cf3239a72c9af471257ce98c458d


---
# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Приступая к работе с соединителем Dynamics CRM Online
Подключение к Dynamics CRM Online позволяет создавать записи, обновлять элементы и выполнять многие другие действия. С помощью CRM Online можно:

* формировать бизнес-процессы на основе данных, получаемых из CRM Online; 
* использовать действия, которые удаляют запись, получают сущности и т. д. Эти действия получают ответ и делают выходные данные доступными для использования другими действиями. Например, при обновлении элемента в CRM можно отправлять сообщение электронной почты с помощью Office 365.

В этой статье содержатся сведения об использовании соединителя Dynamics CRM Online в приложении логики, а также перечислены предоставляемые им триггеры и действия.

> [!NOTE]
> Эта версия статьи предназначена для общедоступного выпуска приложений логики.
> 
> 

Дополнительные сведения о приложениях логики см. в статье, посвященной [приложениям логики](../app-service-logic/app-service-logic-what-are-logic-apps.md), и [руководстве по созданию приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Подключение к Dynamics CRM Online
Чтобы обеспечить доступ приложения логики к какой-либо службе, сначала необходимо создать *подключение* к этой службе. Таким образом вы установите соединение между приложением логики и другой службой. Например, чтобы подключиться к Dynamics, необходимо сначала создать *подключение* к Dynamics CRM Online. Чтобы создать подключение, введите учетные данные, которые используются для доступа к определенной службе. Для создания подключения к Dynamics необходимо использовать учетные данные учетной записи Dynamics CRM Online.

### <a name="create-the-connection"></a>Создание подключения
> [!INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]
> 
> 

## <a name="use-a-trigger"></a>Использование триггера
Триггер — это событие, которое можно использовать для запуска рабочего процесса, определенного в приложении логики. Триггеры опрашивают службу с определенным интервалом и частотой. [Дополнительные сведения о триггерах](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Чтобы открыть список триггеров, в текстовом поле приложения логики введите "dynamics".  
   
    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Выберите **Dynamics CRM Online - When a record is created** (Dynamics CRM Online — при создании записи). Если подключение уже существует, выберите организацию и сущность в раскрывающемся списке.
   
    ![](./media/connectors-create-api-crmonline/select-organization.png)
   
    Если появится запрос на вход, введите учетные данные для входа, чтобы создать подключение. Дополнительные сведения о [создании подключения](connectors-create-api-crmonline.md#create-the-connection) см. в разделе выше. 
   
   > [!NOTE]
   > В этом примере приложение логики выполняется при создании записи. Чтобы увидеть результаты триггера, добавьте другое действие, которое отправляет сообщение электронной почты. Например, добавьте действие Office 365 *Отправить сообщение электронной почты* для отправки вам электронного сообщения после создания новой записи. 
   > 
   > 
3. Нажмите кнопку **Изменить** и задайте **частоту** и **интервал**. Например, если требуется, чтобы триггер выполнял опрос каждые 15 минут, задайте для параметра **Частота** значение **Минута**, а для параметра **Интервал** — **15**. 
   
    ![](./media/connectors-create-api-crmonline/edit-properties.png)
4. **Сохраните** изменения, нажав соответствующую кнопку в левом верхнем углу панели инструментов. Приложение логики сохранено и теперь может быть включено автоматически.

## <a name="use-an-action"></a>Использование действий
Действие — это операция, выполняемая рабочим процессом, определенным в приложении логики. [Дополнительные сведения о действиях](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Щелкните знак "плюс". Отобразятся следующие команды: **Добавить действие**, **Добавить условие** или **Еще**.
   
    ![](./media/connectors-create-api-crmonline/add-action.png)
2. Выберите **Добавить действие**.
3. Чтобы открыть список всех доступных действий, в текстовом поле введите "dynamics".
   
    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)
4. В нашем примере мы выберем действие **Dynamics CRM Online - Update a record** (Dynamics CRM Online — обновить запись). Если подключение уже существует, задайте **Название организации**, **Имя сущности** и другие свойства.  
   
    ![](./media/connectors-create-api-crmonline/sample-action.png)
   
    Если появится запрос на предоставление сведений о подключении, введите их, чтобы создать подключение. Эти свойства описаны в разделе о [создании подключения](connectors-create-api-crmonline.md#create-the-connection) в этой статье. 
   
   > [!NOTE]
   > В нашем примере мы обновляем существующую запись в CRM Online. Для обновления записи можно использовать выходные данные другого триггера. Например, добавьте триггер SharePoint *When an existing item is modified* (При изменении существующего элемента). Затем добавьте действие CRM Online *Update a record* (Обновить запись), которое использует поля SharePoint, чтобы обновить существующую запись в CRM Online. 
   > 
   > 
5. **Сохраните** изменения, нажав соответствующую кнопку в левом верхнем углу панели инструментов. Приложение логики сохранено и теперь может быть включено автоматически.

## <a name="technical-details"></a>Технические сведения
## <a name="triggers"></a>триггеры;
| Триггер | Описание |
| --- | --- |
| [When a record is created](connectors-create-api-crmonline.md#when-a-record-is-created) |Активирует поток при создании объекта в CRM. |
| [When a record is updated](connectors-create-api-crmonline.md#when-a-record-is-updated) |Активирует поток при изменении объекта в CRM. |
| [When a record is deleted](connectors-create-api-crmonline.md#when-a-record-is-deleted) |Активирует поток при удалении объекта в CRM. |

## <a name="actions"></a>Действия
| Действие | Описание |
| --- | --- |
| [List records](connectors-create-api-crmonline.md#list-records) |Эта операция получает сведения о записях сущности. |
| [Create a new record](connectors-create-api-crmonline.md#create-a-new-record) |Эта операция создает новую запись сущности. |
| [Get record](connectors-create-api-crmonline.md#get-record) |Эта операция получает сведения об указанной записи сущности. |
| [Delete a record](connectors-create-api-crmonline.md#delete-a-record) |Эта операция удаляет запись из коллекции сущностей. |
| [Update a record](connectors-create-api-crmonline.md#update-a-record) |Эта операция обновляет существующую запись сущности. |

### <a name="trigger-and-action-details"></a>Сведения о триггерах и действиях
В этом разделе приведены сведения о каждом триггере и действии, включая обязательные и необязательные входные свойства, а также соответствующие выходные данные, связанные с соединителем.

#### <a name="when-a-record-is-created"></a>When a record is created (При создании записи)
Активирует поток при создании объекта в CRM. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| $skip |Число пропусков |Количество пропускаемых записей (значение по умолчанию — 0) |
| $top |Максимальное число записей |Максимальное количество получаемых записей (значение по умолчанию — 256) |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-updated"></a>When a record is updated (При обновлении записи)
Активирует поток при изменении объекта в CRM. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| $skip |Число пропусков |Количество пропускаемых записей (значение по умолчанию — 0) |
| $top |Максимальное число записей |Максимальное количество получаемых записей (значение по умолчанию — 256) |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="when-a-record-is-deleted"></a>When a record is deleted (При удалении записи)
Активирует поток при удалении объекта в CRM. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| $skip |Число пропусков |Количество пропускаемых записей (значение по умолчанию — 0) |
| $top |Максимальное число записей |Максимальное количество получаемых записей (значение по умолчанию — 256) |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="list-records"></a>List records (Отобразить список записей)
Эта операция получает сведения о записях сущности. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| $skip |Число пропусков |Количество пропускаемых записей (значение по умолчанию — 0) |
| $top |Максимальное число записей |Максимальное количество получаемых записей (значение по умолчанию — 256) |
| $filter |Запрос фильтра |Запрос фильтра ODATA для ограничения возвращаемых записей |
| $orderby |Упорядочить по |Запрос orderBy ODATA для указания порядка записей |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
| --- | --- |
| value |array |

#### <a name="create-a-new-record"></a>Создание записи
Эта операция создает новую запись сущности. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="get-record"></a>Get record (Получить запись)
Эта операция получает сведения об указанной записи сущности. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор элемента |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="delete-a-record"></a>Delete a record (Удалить запись)
Эта операция удаляет запись из коллекции сущностей. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор элемента |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

#### <a name="update-a-record"></a>Изменение записи
Эта операция обновляет существующую запись сущности. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| dataset* |Название организации |Название организации CRM, например Contoso |
| table* |Имя сущности |Имя сущности |
| id* |Идентификатор записи |Укажите идентификатор записи |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

## <a name="http-responses"></a>Ответы HTTP
Действия и триггеры могут возвращать один или несколько кодов состояния HTTP, которые приведены ниже. 

| Имя | Описание |
| --- | --- |
| 200 |ОК |
| 202 |Принято |
| 400 |Ошибка запроса |
| 401 |Не авторизовано |
| 403 |Запрещено |
| 404 |Не найдено |
| 500 |Внутренняя ошибка сервера. Произошла неизвестная ошибка. |
| по умолчанию |Операция завершилась ошибкой. |

## <a name="next-steps"></a>Дальнейшие действия
[Создание приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md). Чтобы узнать, какие еще соединители доступны в Logic Apps, просмотрите [список интерфейсов API](apis-list.md).




<!--HONumber=Nov16_HO3-->


