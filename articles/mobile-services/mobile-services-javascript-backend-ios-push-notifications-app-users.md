---
title: Рассылка push-уведомлений проверенным пользователям приложения в iOS (серверная часть JavaScript)
description: Сведения об отправке push-уведомлений конкретным адресатам
services: mobile-services,notification-hubs
documentationcenter: ios
author: krisragh
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/21/2016
ms.author: krisragh

---
# Рассылка push-уведомлений проверенным пользователям
[!INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Аналогичные сведения для мобильных приложений см. в статье [Практическое руководство. Отправка push-уведомлений пользователю, прошедшему проверку подлинности, с помощью тегов](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#push-user).
> 
> 

В этом разделе показано, как отправлять push-уведомления пользователю, прошедшему проверку подлинности, на iOS-устройстве. Перед этим учебником сперва выполните задания учебников [Приступая к работе с проверкой подлинности] и [Приступая к работе с push-уведомления].

В этом учебнике вы узнаете, как требовать от пользователей проверку подлинности, регистрироваться в концентраторе уведомлений для рассылки push-уведомлений и обновлять серверные скрипты для отправки этих уведомлений только пользователям, прошедшим проверку подлинности.

## <a name="register"></a>Обновление службы с учетом требования по проверке подлинности для регистрации
[!INCLUDE [mobile-services-javascript-backend-push-notifications-app-users](../../includes/mobile-services-javascript-backend-push-notifications-app-users.md)]

Замените функцию `insert` следующим кодом и нажмите кнопку **Сохранить**. Этот скрипт вставки использует тег идентификатора пользователя для отправки push-уведомления на всех зарегистрированные приложения iOS от пользователя, который вошел в систему:

```
// Get the ID of the logged-in user.
var userId = user.userId;

function insert(item, user, request) {
    request.execute();
    setTimeout(function() {
        push.apns.send(userId, {
            alert: "Alert: " + item.text,
            payload: {
                "Hey, a new item arrived: '" + item.text + "'"
            }
        });
    }, 2500);
}
```

## <a name="update-app"></a>Обновление приложения с учетом требования по входу в систему перед регистрацией
[!INCLUDE [mobile-services-ios-push-notifications-app-users-login](../../includes/mobile-services-ios-push-notifications-app-users-login.md)]

## <a name="test"></a>Тестирование приложения
[!INCLUDE [mobile-services-ios-push-notifications-app-users-test-app](../../includes/mobile-services-ios-push-notifications-app-users-test-app.md)]

<!-- Anchors. -->
[Updating the service to require authentication for registration]: #register
[Updating the app to log in before registration]: #update-app
[Testing the app]: #test
[Next Steps]: #next-steps


<!-- URLs. -->
[Приступая к работе с проверкой подлинности]: mobile-services-ios-get-started-users.md
[Приступая к работе с push-уведомления]: mobile-services-javascript-backend-ios-get-started-push.md
[Mobile Services .NET How-to Conceptual Reference]: mobile-services-ios-how-to-use-client-library.md

<!---HONumber=AcomDC_0727_2016-->