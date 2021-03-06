---
title: "Добавление соединителя OneDrive в приложения логики | Документация Майкрософт"
description: "Обзор соединителя OneDrive с параметрами интерфейса API REST"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9c6213f0dcb66ae0c53d716abfe84868b87585f1


---
# <a name="get-started-with-the-onedrive-connector"></a>Начало работы с соединителем OneDrive
Подключитесь к OneDrive для управления файлами, включая передачу, получение, удаление файлов и многое другое. 

С помощью OneDrive вы можете: 

* создавать рабочие процессы, сохраняя файлы в OneDrive, или обновлять имеющиеся файлы; 
* использовать триггеры для запуска рабочего процесса при создании или обновлении файлов в OneDrive;
* использовать действия по созданию, удалению файлов и т. д. Например, после получения нового сообщения электронной почты Office 365 с вложением (триггер) может создаваться файл в OneDrive (действие).

В этой статье содержатся сведения об использовании соединителя OneDrive в приложении логики, а также перечислены предоставляемые им триггеры и действия.

> [!NOTE]
> Эта версия статьи предназначена для общедоступного выпуска приложений логики. 
> 
> 

Дополнительные сведения о приложениях логики см. в статье, посвященной [приложениям логики](../app-service-logic/app-service-logic-what-are-logic-apps.md), и [руководстве по созданию приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-onedrive"></a>Создание подключения к OneDrive
Чтобы обеспечить доступ приложения логики к какой-либо службе, сначала необходимо создать *подключение* к этой службе. Таким образом вы установите соединение между приложением логики и другой службой. Например, чтобы подключиться к OneDrive, сначала необходимо создать соответствующее *подключение*. Чтобы создать подключение, введите учетные данные, которые используются для доступа к определенной службе. Для создания подключения к OneDrive необходимо использовать учетные данные учетной записи OneDrive.

### <a name="create-the-connection"></a>Создание подключения
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Использование триггера
Триггер — это событие, которое можно использовать для запуска рабочего процесса, определенного в приложении логики. Триггеры опрашивают службу с определенным интервалом и частотой. [Дополнительные сведения о триггерах](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Чтобы открыть список триггеров, в текстовом поле приложения логики введите onedrive.  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Выберите триггер **When a file is modified** (При изменении файла). Если подключение уже существует, нажмите кнопку "Выбрать", чтобы указать папку.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Если появится запрос на вход, введите учетные данные для входа, чтобы создать подключение. Дополнительные сведения о [создании подключения](connectors-create-api-onedrive.md#create-the-connection) см. в разделе выше. 
   
   > [!NOTE]
   > В этом примере приложение логики запускается при обновлении файла в выбранной папке. Чтобы увидеть результаты триггера, добавьте другое действие, которое отправляет сообщение электронной почты. Например, добавьте действие Office 365 Outlook *Отправить сообщение электронной почты* для отправки вам сообщения электронной почты после обновления файла. 
   > 
   > 
3. Нажмите кнопку **Изменить** и задайте **частоту** и **интервал**. Например, если требуется, чтобы триггер выполнял опрос каждые 15 минут, задайте для параметра **Частота** значение **Минута**, а для параметра **Интервал** — **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Сохраните** изменения, нажав соответствующую кнопку в левом верхнем углу панели инструментов. Приложение логики сохранено и теперь может быть включено автоматически.

## <a name="use-an-action"></a>Использование действий
Действие — это операция, выполняемая рабочим процессом, определенным в приложении логики. [Дополнительные сведения о действиях](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Щелкните знак "плюс". Отобразятся следующие команды: **Добавить действие**, **Добавить условие** или **Еще**.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Выберите **Добавить действие**.
3. Чтобы открыть список всех доступных действий, в текстовом поле введите onedrive.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. В этом примере для OneDrive мы выберем действие **Создать файл**. Если подключение уже существует, укажите **путь к папке**, в которую необходимо поместить файл, введите **имя файла** и выберите тип **содержимого файла**.  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Если появится запрос на предоставление сведений о подключении, введите их, чтобы создать подключение. Эти свойства описаны в разделе о [создании подключения](connectors-create-api-onedrive.md#create-the-connection) в этой статье. 
   
   > [!NOTE]
   > В этом примере мы создадим файл в папке OneDrive. Чтобы создать файл OneDrive, можно использовать выходные данные другого триггера. Например, добавьте триггер Office 365 Outlook *When a new email arrives* (При получении новой почты). Затем, чтобы создать файл в OneDrive, добавьте действие OneDrive *Создать файл*, для которого заданы поля "Вложения" и "Тип содержимого" в ForEach. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)
   > 
   > 
5. **Сохраните** изменения, нажав соответствующую кнопку в левом верхнем углу панели инструментов. Приложение логики сохранено и теперь может быть включено автоматически.

## <a name="technical-details"></a>Технические сведения
## <a name="triggers"></a>триггеры;
| Триггер | Description (Описание) |
| --- | --- |
| [При создании файла](connectors-create-api-onedrive.md#when-a-file-is-created) |Запускает поток при создании файла в папке. |
| [При изменении файла](connectors-create-api-onedrive.md#when-a-file-is-modified) |Запускает поток при изменении файла в папке. |

## <a name="actions"></a>Действия
| Действие | Description (Описание) |
| --- | --- |
| [Получение метаданных файла](connectors-create-api-onedrive.md#get-file-metadata) |Извлекает метаданные файла. |
| [Обновление файла](connectors-create-api-onedrive.md#update-file) |Обновляет файл. |
| [Удаление файла](connectors-create-api-onedrive.md#delete-file) |Удаляет файл. |
| [Получение метаданных файла с помощью пути](connectors-create-api-onedrive.md#get-file-metadata-using-path) |Извлекает метаданные файла с помощью пути. |
| [Получение содержимого файла с помощью пути](connectors-create-api-onedrive.md#get-file-content-using-path) |Извлекает содержимое файла с помощью пути. |
| [Получение содержимого файла](connectors-create-api-onedrive.md#get-file-content) |Извлекает содержимое файла. |
| [Создание файла](connectors-create-api-onedrive.md#create-file) |Создает файл. |
| [Копирование файла](connectors-create-api-onedrive.md#copy-file) |Копирует файл в OneDrive. |
| [Вывод списка файлов в папке](connectors-create-api-onedrive.md#list-files-in-folder) |Извлекает список файлов и вложенных папок в папке. |
| [Вывод списка файлов в корневой папке](connectors-create-api-onedrive.md#list-files-in-root-folder) |Извлекает список файлов и вложенных папок в корневой папке. |
| [Извлечение архива в папку](connectors-create-api-onedrive.md#extract-archive-to-folder) |Извлекает файл архива в папку (например, ZIP-файл). |

### <a name="action-details"></a>Сведения о действиях
В этом разделе приведены сведения о каждом действии, включая обязательные и необязательные входные свойства, а также соответствующие выходные данные, связанные с соединителем.

#### <a name="get-file-metadata"></a>Получение метаданных файла
Извлекает метаданные файла. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| id* |Файл |Выбор файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |string |

#### <a name="update-file"></a>Обновление файла
Обновляет файл. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| id* |Файл |Выбор файла |
| body* |содержимое файла; |Содержимое файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |строка |

#### <a name="delete-file"></a>Удаление файла
Удаляет файл. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| id* |Файл |Выбор файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="get-file-metadata-using-path"></a>Получение метаданных файла с помощью пути
Извлекает метаданные файла с помощью пути. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| path* |Путь к файлу |Выбор файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |строка |

#### <a name="get-file-content-using-path"></a>Получение содержимого файла с помощью пути
Извлекает содержимое файла с помощью пути. 

| Имя свойства | Отображаемое имя | Описание |
| --- | --- | --- |
| path* |Путь к файлу |Выбор файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="get-file-content"></a>Получение содержимого файла
Извлекает содержимое файла. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| id* |Файл |Выбор файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="create-file"></a>Создание файла
Создает файл. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| folderPath* |Путь к папке |Выбор папки |
| name* |Имя файла |Имя файла |
| body* |содержимое файла; |Содержимое файла |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |string |

#### <a name="copy-file"></a>Копирование файла
Копирует файл в OneDrive. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| source* |URL-адрес исходного файла |URL-адрес исходного файла |
| destination* |Путь к конечному файлу |Путь к конечному файлу, включая имя конечного файла |
| overwrite |Перезаписать? |Перезаписывает конечный файл, если задано значение "true" |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |string |

#### <a name="when-a-file-is-created"></a>При создании файла
Запускает поток при создании файла в папке. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| folderId* |Папка |Выбор папки |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="when-a-file-is-modified"></a>При изменении файла
Запускает поток при изменении файла в папке. 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| folderId* |Папка |Выбор папки |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
Отсутствует.

#### <a name="list-files-in-folder"></a>List files in folder (Вывод списка файлов в папке)
Извлекает список файлов и вложенных папок в папке.

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| id* |Папка |Выбор папки |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |строка |

#### <a name="list-files-in-root-folder"></a>List files in root folder (Вывод списка файлов в корневой папке)
Извлекает список файлов и вложенных папок в корневой папке. 

Для этого вызова параметры отсутствуют.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |строка |

#### <a name="extract-archive-to-folder"></a>Извлечение архива в папку
Извлекает файл архива в папку (например, ZIP-файл). 

| Имя свойства | Отображаемое имя | Description (Описание) |
| --- | --- | --- |
| source* |Путь к исходному файлу архива |Путь к файлу архива |
| destination* |Путь к конечной папке |Путь для извлечения содержимого архива |
| overwrite |Перезаписать? |Перезаписывает конечные файлы, если задано значение "true" |

Звездочка (*) означает, что свойство является обязательным.

##### <a name="output-details"></a>Сведения о выходных данных
BlobMetadata

| Имя свойства | Тип данных |
| --- | --- |
| id |string |
| Имя |string |
| displayName |string |
| path |string |
| LastModified |string |
| Размер |целое число |
| MediaType |string |
| IsFolder |Логическое |
| ETag |string |
| FileLocator |строка |

## <a name="http-responses"></a>Ответы HTTP
В следующей таблице приведены ответы на действия и триггеры, а также их описания.  

| Имя | Описание |
| --- | --- |
| 200 |ОК |
| 202 |Принято |
| 400 |Ошибка запроса |
| 401 |Не авторизовано |
| 403 |Запрещено |
| 404 |Не найдено |
| 500 |Внутренняя ошибка сервера. Произошла неизвестная ошибка |
| по умолчанию |Операция завершилась ошибкой. |

## <a name="next-steps"></a>Дальнейшие действия
[Создание приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md). Чтобы узнать, какие еще соединители доступны в Logic Apps, просмотрите [список интерфейсов API](apis-list.md).




<!--HONumber=Nov16_HO3-->


