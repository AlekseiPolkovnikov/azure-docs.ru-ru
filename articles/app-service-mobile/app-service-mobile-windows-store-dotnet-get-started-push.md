---
title: Добавление push-уведомлений в приложение универсальной платформы Windows (UWP) | Microsoft Docs
description: Узнайте, как использовать мобильные приложения службы приложений Azure и центры уведомлений Azure для отправки push-уведомлений в приложение универсальной платформы Windows (UWP).
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: adrianhall
manager: dwrede
editor: ''

ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha

---
# <a name="add-push-notifications-to-your-windows-app"></a>Добавление push-уведомлений в приложение Windows
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Обзор
В этом разделе показано, как отправлять push-уведомления в приложение универсальной платформы Windows (UWP) с помощью мобильных приложений службы приложений Azure с центрами уведомлений Azure. В этом сценарии при добавлении нового элемента серверная часть мобильного приложения отправляет push-уведомление всем приложениям для Windows, которые зарегистрированы в службе уведомлений Windows (WNS).

Этот учебник создан на основе краткого руководства по мобильным приложениям. Перед началом работы с этим учебником необходимо изучить краткое руководство [Создание приложения Windows](app-service-mobile-windows-store-dotnet-get-started.md). Если вы не используете скачанный проект сервера быстрого запуска, в проект необходимо добавить пакет расширений для push-уведомлений. Дополнительную информацию о пакетах расширений для сервера см. в статье [Работа с пакетом SDK для внутреннего сервера .NET для мобильных приложений Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="<a-name="create-hub"></a>create-a-notification-hub"></a><a name="create-hub"></a>Создание центра уведомлений
[!INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Регистрация приложения для работы с push-уведомлениями
Прежде чем вы сможете отправлять push-уведомления в приложения для Windows из Azure, необходимо отправить приложение в Магазин Windows. Затем можно будет настроить серверный проект для интеграции с WNS.

1. В обозревателе решений Visual Studio щелкните правой кнопкой мыши проект приложения UWP, щелкните **Магазин** > **Связать приложение с Магазином…**. 
   
    ![Связь приложения с Магазином Windows](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. В окне мастера нажмите кнопку **Далее**, выполните вход с помощью учетной записи Майкрософт, введите имя приложения в поле **Зарезервировать новое имя приложения** и нажмите кнопку **Зарезервировать**.
3. После успешного создания регистрации приложения выберите новое имя приложения, нажмите кнопку **Далее**, а затем кнопку **Связать**. Это добавляет необходимые регистрационные данные Магазина Windows в манифест приложения.  
4. Перейдите в [Центр разработки для Windows](https://dev.windows.com/en-us/overview), войдите с помощью учетной записи Майкрософт, щелкните "Регистрация нового приложения" в области **Мои приложения**, а затем разверните **Службы** > **Push-уведомления**. 
5. На странице **Push-уведомления** в разделе **Мобильные службы Microsoft Azure** щелкните **Live Services site** (Сайт служб Live).
6. На странице регистрации запишите значения полей **Секреты приложения** и **SID пакета**, которые вам понадобятся позже при настройке серверной части мобильного приложения. 
   
    ![Связь приложения с Магазином Windows](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)
   
   > [!IMPORTANT]
   > Секрет клиента и ИД безопасности пакета — это важные учетные данные для безопасного доступа. Не сообщайте никому эти значения и не распространяйте их вместе со своим приложением. **Идентификатор приложения** используется с секретным кодом для настройки аутентификации учетной записи Майкрософт .
   > 
   > 

## <a name="configure-the-backend-to-send-push-notifications"></a>Настройка серверной части для отправки push-уведомлений
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a name="<a-id="update-service"></a>update-the-server-to-send-push-notifications"></a><a id="update-service"></a>Обновление сервера для отправки push-уведомлений
После включения push-уведомлений в приложении необходимо обновить внутренний сервер приложения так, чтобы он отправлял push-уведомления. Используйте приведенную ниже процедуру, которая соответствует типу вашего серверного проекта &mdash; [серверный проект .NET](#dotnet) или [серверный проект Node.js](#nodejs).

### <a name="<a-name="dotnet"></a>.net-backend-project"></a><a name="dotnet"></a>Серверный проект .NET
1. В Visual Studio щелкните правой кнопкой мыши серверный проект, затем щелкните **Управление пакетами NuGet**, найдите пакет Microsoft.Azure.NotificationHubs и нажмите кнопку **Установить**. Будет установлена клиентская библиотека центров уведомлений.
2. Разверните **Контроллеры**, откройте TodoItemController.cs и добавьте следующие операторы using:
   
        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. В метод **PostTodoItem** добавьте следующий код после вызова **InsertAsync**.
   
        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
   
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;
   
        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);
   
        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);
   
            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }
   
    Этот код заставляет центр уведомлений отправить push-уведомление после вставки элемента задачи.
4. Повторная публикация серверного проекта

### <a name="<a-name="nodejs"></a>node.js-backend-project"></a><a name="nodejs"></a>Серверный проект Node.js
1. [Скачайте проект быстрого запуска](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) или воспользуйтесь [онлайн-редактором на портале Azure](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor), если вы этого еще не сделали.
2. Замените существующий код в файле todoitem.js следующим кодом:
   
        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
   
        var table = azureMobileApps.table();
   
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
   
        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";
   
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });
   
        module.exports = table;
   
    При вставке нового элемента todo будет отправляться всплывающее уведомление WNS, содержащее item.text.
3. При редактировании этого файла на локальном компьютере повторно опубликуйте серверный проект.

## <a name="<a-id="update-app"></a>add-push-notifications-to-your-app"></a><a id="update-app"></a>Добавление push-уведомлений в приложение
Затем приложение необходимо зарегистрировать для получения push-уведомлений при запуске. Если аутентификация уже включена, убедитесь, что пользователь выполняет вход прежде, чем пытается зарегистрироваться для использования push-уведомлений. Дополнительные сведения см. в разделе [Authenticate first](https://github.com/Azure-Samples/app-service-mobile-windows-quickstart/blob/master/README.md#authenticate-first) полного примера быстрого запуска.

1. Откройте файл проекта **App.xaml.cs** и добавьте следующие операторы `using`.
   
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. В том же файле добавьте следующее определение метода **InitNotificationsAsync** в класс **App**.
   
        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();
   
            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }
   
    Этот код возвращает ChannelURI для приложения из WNS, а затем регистрирует ChannelURI в мобильном приложении службы приложений.
3. В верхней части обработчика событий **OnLaunched** в файле **App.xaml.cs** добавьте в определение метода модификатор **async**, а в новый метод **InitNotificationsAsync** добавьте следующий вызов (как в приведенном ниже примере).
   
        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();
   
            // ...
        }
   
    Это обеспечит регистрацию кратковременного ChannelURI при каждом запуске приложения.
4. Перестройте проект приложения UWP. Теперь приложение готово к получению всплывающих уведомлений.

## <a name="<a-id="test"></a>test-push-notifications-in-your-app"></a><a id="test"></a>Тестирование push-уведомлений в приложении
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a name="<a-id="more"></a>next-steps"></a><a id="more"></a>Дальнейшие действия
Дополнительные сведения о push-уведомлениях:

* [Использование управляемого клиента для мобильных приложений Azure](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
  Шаблоны обеспечивают гибкость при отправке push-уведомлений локально и между различными платформами. Узнайте, как регистрировать шаблоны.
* [Работа с пакетом SDK для внутреннего сервера .NET для мобильных приложений Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-add-tags-to-a-device-installation-to-enable-push-to-tags)  
  Теги позволяют отправлять push-уведомления клиентам в зависимости от их сегмента.  Узнайте, как указать теги для определенного устройства.
* [Диагностика неполадок, связанных с push-уведомлениями](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Существуют различные причины, по которым уведомления могут теряться или не доходить до устройств. В этой статье рассказывается, как проанализировать и определить основную причину сбоев push-уведомлений. 

Мы также рекомендуем изучить одно из следующих руководств:

* [Добавление аутентификации в приложение](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Узнайте больше о проверке подлинности пользователей приложения с помощью поставщика удостоверений.
* [Включение автономной синхронизации для приложения](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Узнайте, как добавить в приложение поддержку автономной работы с помощью серверной части мобильного приложения. Автономная синхронизация позволяет пользователям взаимодействовать с мобильным приложением &mdash; просматривать, добавлять или изменять данные &mdash; даже при отсутствии подключения к сети.

<!-- Anchors. -->

<!-- URLs. -->
[портала Azure]: https://portal.azure.com/

<!-- Images. -->




<!--HONumber=Oct16_HO2-->


