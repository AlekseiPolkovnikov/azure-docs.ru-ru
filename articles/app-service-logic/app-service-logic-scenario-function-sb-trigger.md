---
title: 'Сценарий приложения логики: создание триггера служебной шины с помощью функций Azure | Microsoft Docs'
description: Использование функций Azure для создания триггера служебной шины для приложения логики
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: dwrede
editor: ''

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: jehollan

---
# Сценарий приложения логики: создание триггера служебной шины Azure с помощью функций Azure
Можно использовать функции Azure для создания триггера приложения логики, когда требуется развернуть долго выполняющиеся прослушиватели или задачи. Например, можно создать функцию, которая будет прослушивать очередь и немедленно запускать извещающим триггером приложение логики.

## Создание приложения логики
В этом примере для каждого приложения логики, которое нужно запускать, создается отдельная функция. Сначала создайте приложение логики с триггером HTTP-запроса. Функция Azure вызывает эту конечную точку при получении каждого сообщения из очереди.

1. Создайте новое приложение логики и выберите триггер **Manual - When an HTTP Request is Received** (Вручную — при получении HTTP-запроса). При желании можно указать схему JSON, которая будет использоваться для очереди сообщений. Для этого примените специальный инструмент, например [jsonschema.net](http://jsonschema.net). Вставьте полученную схему в триггер. Это поможет конструктору понять, какую форму будут иметь данные, чтобы правильней использовать свойства в рабочем процессе.
2. Добавьте дополнительные шаги, которые необходимо выполнить после получения сообщения из очереди. Например, можно отправить сообщение электронной почты с помощью Office 365.
3. Сохраните приложение логики, чтобы создать URL-адрес обратного вызова для этого триггера приложения логики. URL-адрес отобразится на карточке триггера.

![URL-адрес обратного вызова отображается на карточке триггера][1]

## Создание функции
Далее следует создать функцию, которая будет выступать в качестве триггера и прослушивать очередь.

1. На [портале функций Azure](https://functions.azure.com/signin) выберите команду **Создать функцию**, а затем выберите шаблон **ServiceBusQueueTrigger - C#**.
   
    ![портал функций Azure;][2]
2. Настройте подключение к очереди служебной шины (с использованием прослушивателя `OnMessageReceive()` из пакета SDK служебной шины Azure).
3. Напишите простую функцию для вызова конечной точки приложения логики (которую мы создали ранее), используя сообщение очереди в качестве триггера. Ниже приводится полноценный пример такой функции. В примере используется сообщение с типом данных `application/json`, но этот тип можно изменить в соответствии с потребностями.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Для проверки работы можно поместить сообщение в очередь с помощью инструмента [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Приложение логики должно запуститься сразу же, как только функция получит это сообщение.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG

<!---HONumber=AcomDC_0803_2016-->