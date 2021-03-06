---
title: Запуск выражений C# в приложении API C# в приложении логики | Microsoft Docs
description: Приложение API C# или соединитель
services: logic-apps
documentationcenter: .net
author: jeffhollan
manager: dwrede
editor: ''

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/22/2016
ms.author: jehollan

---
# Приложение API C
> [!NOTE]
> Эта версия статьи предназначена для приложений логики со схемой версии 2014-12-01-preview.
> 
> 

Приложение API C# позволяет легко выполнять простые выражения C# *во время работы приложения логики*.

## Когда следует использовать это приложение API?
Это приложение API следует использовать, если вам нужно, чтобы жизненный цикл кода, который вы пишете, был таким же, как в приложении логики, и при этом вы *не* хотите, чтобы код вызывался в других сценариях.

Также используйте этот приложение, если вам нужны повторно используемые фрагменты кода с жизненным циклом, который зависит от приложения логики. С помощью приложения API WebJobs вы сможете создавать простые выражений кода и вызывать их из приложения логики.

И, наконец, используйте его, если вам нужно включать дополнительные пакеты, которые нужно передать соединителю в сборке (.dll) в виде двоичной строки в кодировке Base64 (например, выходные данные из хранилища больших двоичных объектов) . Если вам нужна дополнительная гибкость при работе с пакетами и сборками, лучше использовать приложение WebJob.

Используйте [Приложение API JavaScript](app-service-logic-javascript-api.md), если вам нужно писать выражения на JS.

## Создание приложения API C
Чтобы использовать приложение API C#, необходимо сначала создать экземпляр приложения API C#. Это можно сделать во время создания приложения логики или путем выбора приложения API C# в Azure Marketplace.

## Использование приложения API C# в области конструктора приложения логики
### Триггер
Вы можете создать триггер, который будет опрашивать службу приложения логики (с выбранным вами интервалом). Если триггер не возвращает никаких результатов, кроме `false`, приложение логики будет выполняться или ожидать следующего интервала опроса для повторной проверки.

Входные данные для триггера:

* **выражение C#** – выражение, которое будет оцениваться. Оно будет вызываться внутри функции и должно возвращать значение `false`, если вы не хотите, чтобы приложение логики запускалось. Выражение также может возвращать другие значения, если нужно, чтобы приложение логики запускалось. Вы сможете использовать содержимое ответа в действиях приложения логики.

Например, можно создать простой триггер, который будет выполнять приложение логики только между 15 и 30 минутой часа.

```
var d = new DateTime.Now; return (d.Minute > 15) && (d.Minute < 30);
```

### Действие
Точно так же можно указать действие для выполнения.

Входные данные для действия:

* **выражение C#** – выражение, которое будет оцениваться. Необходимо добавить в него инструкцию `return`, чтобы получить содержимое.
* **Объекты контекста** — необязательный объект контекста, который может быть передан в триггер. Вы можете определить такое количество свойств, которое вам требуется, но основой должен быть объект JObject `{ ... }`. На объекты можно ссылаться в сценарии с использованием имени ключа (значение передается в виде маркера JToken, который соответствует имени).
* **Библиотеки** — необязательный массив DLL-файлов для включения в компиляцию сценария. Массив использует следующую структуру и работает лучше всего вместе с соединителем хранилища больших двоичных объектов с DLL-файлом в качестве выходных данных.

```javascript
[{"filename": "name.dll", "assembly": {Base64StringFromConnector}, "usingstatment": "using Library.Reference;"}]
```

Предположим, вы используете триггер Office 365 **Новое сообщение электронной почты**. Этот триггер возвращает следующий объект:

```javascript
{
    ...
    "Attachments" : [
        {
            "name" : "Picture.png",
            "content" : {
                "ContentData" : "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFAQMAAAC3obSmAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURf///wAAAFXC034AAAASSURBVAjXY2BgCGBgYOhgKAAABEIBSWDJEbYAAAAASUVORK5CYII=",
                "ContentType" : "image/png",
                "ContentTransferEncoding" : "Base64"
            }
        },    
        {
            "name" : "File.txt",
            "content" : {
                "ContentData" : "Don't worry, be happy!",
                "ContentType" : "text/plain",
                "ContentTransferEncoding" : "None"
            }
        }    
    ]
}
```

При этом вам необходимо передать эти вложения в публикацию Yammer. К сожалению, схема для вложений Yammer немного отличается. Теперь вы можете выполнить синтаксический анализ внутри своего приложения логики. Для объекта контекста передайте: `@triggerBody()`, а для выражения передайте:

```javascript
JArray YammerAttachments = new JObject();
foreach(var obj in (JArray)Attachments)
{
    JObject att = new JObject();
    att["Content"] = obj["content"];
    att["FileName"] = obj["Name"];
    YammerAttachments.Add(att);    
}
return YammerAttachments;
```

Это действие возвращает объект, который вы вернули из функции в объекте результатов. Таким образом, в приложении API Yammer вы можете ссылаться на `@body('csapi').results` в качестве свойства **Вложения**.

## Дополнительные возможности соединителя
После создания соединителя его можно добавить в бизнес-процесс с помощью приложения логики. См. раздел [Что такое приложения логики?](app-service-logic-what-are-logic-apps.md).

<!--References -->

<!--Links -->
[Creating a Logic App]: app-service-logic-create-a-logic-app.md

<!---HONumber=AcomDC_0803_2016-->