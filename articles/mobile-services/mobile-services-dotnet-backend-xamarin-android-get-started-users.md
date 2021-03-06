---
title: Приступая к работе с проверкой подлинности в мобильных службах для приложений Xamarin для Android | Microsoft Docs
description: Узнайте, как использовать мобильные службы для аутентификации пользователей приложения Xamarin для Android с помощью разнообразных поставщиков удостоверений, включая Google, Facebook, Twitter и корпорацию Майкрософт.
services: mobile-services
documentationcenter: xamarin
author: lindydonna
manager: dwrede
editor: mollybos

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/21/2016
ms.author: donnam

---
# Приступая к работе с проверкой подлинности в мобильных службах
[!INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Аналогичные сведения для мобильных приложений см. в статье [Добавление проверки подлинности в приложение Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md).
> 
> 

В этом разделе показано, как выполнять проверку подлинности пользователей в мобильных службах Azure в приложении. В этом учебнике вы добавите проверку подлинности к проекту быстрого запуска, используя поставщик удостоверений, поддерживаемый мобильными службами. После выполнения успешной проверки подлинности и авторизации мобильными службами отображается значение идентификатора пользователя.

В этом учебнике рассматриваются следующие основные шаги для включения проверки подлинности в приложении:

1. [Регистрация приложения для проверки подлинности и настройка мобильных служб]
2. [Предоставление разрешений на таблицу только пользователям, прошедшим проверку подлинности]
3. [Добавление проверки подлинности в приложение]

Этот учебник создан на основе краткого руководства по мобильным службам. Вам также необходимо сначала ознакомиться с учебником [Приступая к работе с мобильными службами].

## <a name="register"></a>Регистрация приложения для проверки подлинности и настройка мобильных служб
[!INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[!INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

## <a name="permissions"></a>Предоставление разрешений только пользователям, прошедшим проверку подлинности
[!INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

<ol start="6">
<li><p>В Visual Studio или Xamarin Studio запустите проект клиента на устройстве или симуляторе. Убедитесь, что после запуска приложения возникает необработанное исключение с кодом состояния 401 (неавторизованный).</p>
       <p>Это происходит, потому что приложение пытается получить доступ к мобильным службам как пользователь, не прошедший проверку подлинности, а таблица <em>TodoItem</em> теперь требует выполнения проверки подлинности.</p></li>
</ol>

Далее приложение будет обновлено таким образом, что оно станет производить аутентификацию учетных данных пользователей, прежде чем запрашивать ресурсы из мобильной службы.

## <a name="add-authentication"></a>Добавление проверки подлинности в приложение
1. Добавьте в класс **TodoActivity** следующее свойство:
   
            private MobileServiceUser user;
2. Добавьте в класс **TodoActivity** следующий метод:
   
            private async Task Authenticate()
            {
                try
                {
                    user = await client.LoginAsync(this, MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}", user.UserId), "Logged in!");
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
            }
   
    При этом создается новый метод для обработки процесса проверки подлинности. Пользователь прошел проверку подлинности с помощью имени входа в Facebook. Открывается диалоговое окно, в котором отображается идентификатор пользователя, прошедшего проверку подлинности.
   
   > [!NOTE]
   > Если у вас поставщик удостоверений, отличный от Facebook, замените значение, передаваемое в метод **LoginAsync** выше, одним из следующих: *MicrosoftAccount*, *Twitter*, *Google* или *WindowsAzureActiveDirectory*.
   > 
   > 
3. Добавьте в метод **onCreate** следующую строку после кода, который формирует экземпляр объекта `MobileServiceClient`.
   
        await Authenticate(); // add this line
   
    Этот вызов запускает процесс проверки подлинности и асинхронно ждет его.
4. В меню **Запуск** щелкните **Начать отладку**, чтобы запустить приложение и выполнить вход с помощью выбранного поставщика удостоверений.
   
       После успешного входа в систему приложение должно работать без ошибок, а вы должны быть в состоянии выполнять запросы мобильных служб и обновлять данные.

<!-- ## <a name="next-steps"> </a>Next steps

In the next tutorial, [Service-side authorization of Mobile Services users][Authorize users with scripts], you will take the user ID value provided by Mobile Services based on an authenticated user and use it to filter the data returned by Mobile Services.
 -->

<!-- Anchors. -->
[Регистрация приложения для проверки подлинности и настройка мобильных служб]: #register
[Предоставление разрешений на таблицу только пользователям, прошедшим проверку подлинности]: #permissions
[Добавление проверки подлинности в приложение]: #add-authentication
[Next Steps]: #next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Приступая к работе с мобильными службами]: mobile-services-dotnet-backend-xamarin-android-get-started.md
[Get started with authentication]: mobile-services-dotnet-backend-xamarin-android-get-started-users.md
[Get started with push notifications]: mobile-services-dotnet-backend-xamarin-android-get-started-push.md
[Authorize users with scripts]: mobile-services-dotnet-backend-service-side-authorization.md
[JavaScript and HTML]: mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md

<!---HONumber=AcomDC_0727_2016-->