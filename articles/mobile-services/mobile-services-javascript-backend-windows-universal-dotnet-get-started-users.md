---
title: Добавление проверки подлинности в универсальное приложение для Windows 8.1 | Microsoft Docs
description: Узнайте, как использовать мобильные службы для аутентификации пользователей приложения магазина Windows с помощью разнообразных поставщиков удостоверений, включая Google, Facebook, Twitter и корпорацию Майкрософт.
services: mobile-services
documentationcenter: windows
author: ggailey777
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 07/21/2016
ms.author: glenga

---
# Добавление проверки подлинности в универсальное приложение для Windows 8.1
[!INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Аналогичные сведения для мобильных приложений см. в статье [Добавление проверки подлинности в приложение Windows](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md).
> 
> 

В этом разделе показано, как выполнять проверку подлинности пользователей в мобильных службах Azure из универсального приложения Windows 8.1. В этом учебнике вы добавите проверку подлинности к проекту быстрого запуска, используя поставщик удостоверений, поддерживаемый мобильными службами. После выполнения успешной проверки подлинности и авторизации мобильными службами отображается значение идентификатора пользователя.

Этот учебник создан на основе краткого руководства по мобильным службам. Вам также необходимо сначала ознакомиться с учебником [Приступая к работе с мобильными службами].

> [!NOTE]
> В этом учебнике показано, как проводить проверку подлинности пользователей в приложениях для Магазина Windows и Магазина Windows Phone 8.1. Инструкции для приложения Windows Phone 8.0 или Windows Phone Silverlight 8.1 см. в этой версии раздела [Приступая к работе с проверкой подлинности в мобильных службах](mobile-services-windows-phone-get-started-users.md).
> 
> 

## <a name="register"></a> Регистрация приложения для проверки подлинности и конфигурация мобильных служб
[!INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a> Предоставление разрешений только пользователям, прошедшим проверку подлинности
[!INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)]

> [!NOTE]
> При использовании средств Visual Studio для подключения приложения к мобильной службе создаются два набора определений **MobileServiceClient** — по одному для каждой клиентской платформы. На этом этапе вы легко можете упростить созданный код, объединив определения **MobileServiceClient** в оболочке `#if...#endif` в одно определение без оболочки, которое может использоваться обеими версиями приложения. Этого не потребуется, если вы загрузили приложение быстрого запуска с [классического портала Azure].
> 
> 

## <a name="add-authentication"></a> Добавление проверки подлинности в приложение
[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]

Теперь любой пользователь, который прошел проверку подлинности с использованием ваших доверенных поставщиков удостоверений, может получить доступ к таблице *TodoItem*. Для более надежной защиты пользовательских данных необходимо также реализовать авторизацию. Для этого нужно получить идентификатор конкретного пользователя, который затем можно использовать для определения уровня доступа пользователя к указанному ресурсу.

## <a name="tokens"></a>Хранение маркера проверки подлинности в клиенте
В предыдущем примере был показан стандартный вход, который требует, чтобы клиент подключался как к поставщику удостоверений, так и к мобильной службе каждый раз, когда приложение запускается. Мало того, что этот метод неэффективен, вы можете столкнуться с проблемами, связанными с использованием приложения, если большое количество клиентов попытаются запустить приложение одновременно. Несколько лучшим подходом является проверка подлинности удостоверений с кэшированием маркера, возвращенного мобильной службой, которая будет использоваться до входа на основе поставщика.

> [!NOTE]
> Можно кэшировать маркер, выданный мобильной службой, независимо от того, используете ли вы аутентификацию, управляемую клиентом или сервером. Этот учебник использует управляемую сервером проверку подлинности.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"> </a>Дальнейшие действия
В следующем учебнике, который называется [Авторизация пользователей мобильных служб на стороне службы](mobile-services-javascript-backend-service-side-authorization.md), значение ИД пользователя, предоставляемое мобильными службами на основе пользователя, прошедшего проверку подлинности, будет использоваться для фильтрации данных, возвращаемых мобильными службами.

## См. также
* [Расширенные возможности пользователей](http://go.microsoft.com/fwlink/p/?LinkId=506605)<br/>Вы можете получить дополнительные данные пользователя, которые хранит поставщик удостоверений в мобильной службе, вызвав в серверных сценариях функцию **user.getIdentities()**.
* [Справочник принципов использования мобильных служб .NET]<br/> Узнайте больше об использовании мобильных служб с помощью клиента .NET.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #tokens
[Next Steps]: #next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Приступая к работе с мобильными службами]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md
[Get started with authentication]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-users.md
[Get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md
[Authorize users with scripts]: ../mobile-services-windows-store-dotnet-authorize-users-in-scripts.md

[классического портала Azure]: https://manage.windowsazure.com/
[Справочник принципов использования мобильных служб .NET]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Register your Windows Store app package for Microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md


<!---HONumber=AcomDC_0727_2016-->