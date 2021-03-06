---
title: "Отправка кроссплатформенных уведомлений пользователям с помощью центров уведомлений (ASP.NET)"
description: "Узнайте, как использовать шаблоны центров уведомлений для отправки в одном запросе независимых от платформы уведомлений, предназначенных для всех платформ."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0cc620324c75ed6ffee26fe442014d61673ba1e6


---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Отправка кроссплатформенных уведомлений пользователям с помощью центров уведомлений
В предыдущем руководстве под названием [Уведомление пользователей с помощью Центров уведомлений] вы научились отправлять push-уведомления на все устройства, зарегистрированные конкретным пользователем, прошедшим проверку. В этом учебнике для отправки уведомления на каждую поддерживаемую платформу клиента требовалось несколько запросов. Концентраторы уведомлений поддерживают шаблоны, которые позволяют указать, каким образом конкретное устройство должно получать уведомления. Это упрощает отправку кроссплатформенных уведомлений. В этой статье рассказывается, как воспользоваться преимуществами шаблонов для отправки в одном запросе независимых от платформы уведомлений, предназначенных для всех платформ. Дополнительные сведения о шаблонах см. в статье [Общие сведения о Центрах уведомлений][Шаблоны].

> [!NOTE]
> Центры уведомлений позволяют устройству зарегистрировать несколько шаблонов с одним и тем же тегом. В этом случае входящее сообщение, предназначенное для этого тега, приводит к отправке на устройство нескольких уведомлений, по одному для каждого шаблона. Это дает возможность отображения одного сообщения в нескольких визуальных уведомлениях, например в виде значка и в виде всплывающего уведомления в Магазине Windows.
> 
> 

Выполните следующие действия для отправки кроссплатформенных уведомлений с использованием шаблонов:

1. В обозревателе решений в Visual Studio разверните папку **Контроллеры** , затем откройте файл RegisterController.cs.
2. Найдите в методе **Post** блок кода, создающий регистрацию, и замените содержимое `switch` следующим кодом:
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Этот код вызывает метод, используемый на конкретной платформе для регистрации шаблонов вместо собственной регистрации. Уже имеющиеся регистрации изменять не нужно, поскольку регистрация шаблонов является производной от собственной регистрации.
3. В контроллере **Уведомления** замените метод **sendNotification** следующим кодом:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Этот код отправляет уведомление одновременно для всех платформ без необходимости указания собственных полезных данных. Центры уведомлений создают и доставляют правильные полезные данные для каждого устройства с указанным значением *тега* в соответствии с зарегистрированными шаблонами.
4. Повторно опубликуйте серверный проект WebApi.
5. Снова запустите клиентское приложение и убедитесь, что регистрация прошла успешно.
6. (Необязательно) Разверните клиентское приложение на втором устройстве, а затем запустите это приложение.
   
    Обратите внимание, что на каждом из устройств отображается уведомление.

## <a name="next-steps"></a>Дальнейшие действия
Теперь, когда вы завершили работу с данным учебником, вы можете узнать больше о центрах уведомлений и шаблонах в следующих разделах:

* **[Использование Центров уведомлений для передачи экстренных новостей]** <br/>Демонстрация другого сценария для использования шаблонов.
* **[Общие сведения о Центрах уведомлений][Шаблоны]**<br/>содержится более подробная информация о шаблонах.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push-уведомления для пользователей ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push-уведомления для пользователей мобильных служб]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express для Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Использование Центров уведомлений для передачи экстренных новостей]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[центры уведомлений Azure]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Уведомление пользователей с помощью Центров уведомлений]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Шаблоны]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Инструкции по использованию Центров уведомлений для Магазина Windows]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx



<!--HONumber=Nov16_HO3-->


