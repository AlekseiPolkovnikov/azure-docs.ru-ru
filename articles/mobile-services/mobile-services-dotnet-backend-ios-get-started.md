---
title: Начало работы с мобильными службами Azure для приложений iOS
description: Следуйте указаниям этого учебника, чтобы приступить к использованию мобильных служб Azure для разработки приложений iOS.
services: mobile-services
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
# <a name="getting-started"> </a>Приступая к работе с мобильными службами
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Аналогичные сведения для мобильных приложений см. в статье [Создание приложения iOS в мобильных приложениях Azure](../app-service-mobile/app-service-mobile-ios-get-started.md).
> 
> 

В этом учебнике показано, как добавить облачную серверную службу в приложение iOS с помощью мобильных служб Azure. В этом учебнике вы создадите новую мобильную службу и простое приложение *To do list*, хранящее данные приложения в новой мобильной службе. Мобильная служба использует .NET и Visual Studio для серверной бизнес-логики. Сведения о создании мобильной службы с серверной бизнес-логикой на JavaScript см. в разделе [Серверная версия JavaScript] этой статьи.

> [!NOTE]
> Для работы с этим учебником требуется учетная запись Azure. Если у вас нет учетной записи, можно зарегистрироваться для получения бесплатной пробной версии Azure и получить [бесплатные мобильные службы, которые можно использовать и после окончания пробного периода](https://azure.microsoft.com/pricing/details/mobile-services/). Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-ios-get-started%2F).
> 
> 

## <a name="create-new-service"> </a>Создание мобильной службы
[!INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Загрузка мобильной службы и приложения на локальном компьютере
После создания мобильной службы загрузите проекты, которые можно выполнять локально.

1. Щелкните только что созданную мобильную службу, затем на вкладке быстрого запуска выберите **iOS** в разделе **Выберите платформу** и разверните элемент **Создание нового приложения iOS**.
2. На компьютере Windows щелкните **Загрузить** в разделе **Загрузка и публикация службы в облаке**. При этом загружается проект Visual Studio, реализующий мобильную службу. Сохраните сжатый файл проекта на локальный компьютер и запомните путь к нему.
3. На компьютере Mac щелкните **Загрузить** в разделе **Загрузка и запуск приложения**. При этом загружается проект для примера приложения *To do list*, которое подключается к вашей мобильной службе, а также пакет Mobile Services SDK для iOS. Сохраните сжатый файл проекта на локальный компьютер и запомните путь к нему.

## Тестирование мобильной службы
[!INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Публикация мобильной службы
[!INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Запуск нового приложения для iOS
[!INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

## <a name="next-steps"> </a>Дальнейшие действия
Здесь показано, как запустить новое клиентское приложение для мобильной службы, работающей в Azure. Перед тестированием приложения iOS с мобильной службой, работающей на локальном компьютере, необходимо настроить веб-сервер и брандмауэр, чтобы разрешить доступ с компьютера разработки iOS. Дополнительные сведения см. в разделе [Настройка локального веб-сервера для подключения к локальной мобильной службе](mobile-services-dotnet-backend-how-to-configure-iis-express.md).

Узнайте, как выполнять дополнительные важные задачи в мобильных службах:

* [Приступая к работе с синхронизацией автономных данных] <br/>Узнайте, как использовать синхронизацию автономных данных для повышения скорости реагирования и надежности приложений.
* [Добавление проверки подлинности в приложение] <br/>Узнайте, как проверять подлинность пользователей приложения с помощью поставщика удостоверений.
* [Добавление push-уведомлений в приложение] <br/>Узнайте, как отправлять в приложение базовые push-уведомления.
* [Устранение неполадок серверной части .NET мобильных служб] <br/>Узнайте, как выявлять и исправлять неполадки, которые могут возникнуть в серверной части мобильных служб для .NET.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]: #getting-started
[Create a new mobile service]: #create-new-service
[Define the mobile service instance]: #define-mobile-service-instance
[Next Steps]: #next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-completed-ios.png
[1]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-vs.png

[6]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Приступая к работе с синхронизацией автономных данных]: mobile-services-ios-get-started-offline-data.md
[Добавление проверки подлинности в приложение]: mobile-services-dotnet-backend-ios-get-started-users.md
[Добавление push-уведомлений в приложение]: mobile-services-dotnet-backend-ios-get-started-push.md
[Устранение неполадок серверной части .NET мобильных служб]: mobile-services-dotnet-backend-how-to-troubleshoot.md
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[XCode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Серверная версия JavaScript]: mobile-services-ios-get-started.md

<!---HONumber=AcomDC_0727_2016-->