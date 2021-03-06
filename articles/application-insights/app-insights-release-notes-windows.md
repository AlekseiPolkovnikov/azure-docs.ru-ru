---
title: "Заметки о выпуске Application Insights для Windows"
description: "Заметки о выпуске пакета SDK (не рекомендуется)."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 62115aba-f806-447b-9cca-76d0e11cfbd5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: joshweb
ROBOTS: NOINDEX,NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 8c5324742e42a1f82bb3031af4380fc5f0241d7f
ms.openlocfilehash: 6868024441e3e0e0d2fdc44499a9b1e00364b31e

---
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Заметки о выпуске для пакета SDK Application Insights для Windows Phone и Магазина Windows

> [!WARNING]
> Пакет SDK для Windows Phone и Магазина Windows больше не поддерживается. Мы советуем использовать [HockeyApp](https://www.hockeyapp.net/) для управления дистрибутивом и мониторинга мобильных и классических приложений.
>  

Пакет SDK Application Insights отправляет сведения о телеметрии вашего работающего приложения в службу [Application Insights](https://azure.microsoft.com/services/application-insights/), в которой вы можете проанализировать использование и производительность приложения.

## <a name="version-111"></a>Версии 1.1.1
### <a name="windows-sdk"></a>Пакет Windows SDK
* Исправление прекращения ответа на запросы во время сбоя при использовании пакета SDK Silverlight для Windows Phone. После этого изменения данные о любом сбое, произошедшем позже, чем через 2 секунды после вызова WindowsAppInitialier.InitializeAsync(...), будут сохранены на диск и отправлены при следующем запуске приложения. Если сбой происходит раньше, чем через 2 секунды после вызова этого метода, он игнорируется.  
* Задайте для зависимостей NuGet определенную версию ядра и Microsoft.ApplicationInsights.PersistenceChannel (вер. 1.2.3).   

### <a name="core-sdk"></a>Базовый пакет SDK
* Для управления ядром используется GitHub. Заметки о будущих выпусках базового пакета SDK можно найти на портале [GitHub](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## <a name="version-11"></a>Версия 1.1
### <a name="core-sdk"></a>Базовый пакет SDK
* В пакете SDK появился новый тип телеметрии ```DependencyTelemetry``` , который содержит данные о вызове зависимости из приложения.
* Новый метод ```TelemetryClient.TrackDependency``` позволяет отправлять данные о вызовах зависимостей из приложения.

## <a name="version-100"></a>Версия 1.0.0
### <a name="windows-app-sdk"></a>Пакет SDK для приложений Windows
* Обновлена инициализация для приложений Windows. Новый класс `WindowsAppInitializer` с методом `InitializeAsync()` обеспечивает инициализации начальной загрузки коллекции SDK. Это изменение дает более точный контроль и повышает качество инициализации приложений по сравнению с используемым ранее методом ApplicationInsights.config.
* DeveloperMode больше не устанавливается автоматически. Чтобы изменить поведение DeveloperMode, необходимо прописать его в коде.
* Пакет NuGet больше не внедряет файл ApplicationInsights.config. При добавлении пакета NuGet вручную рекомендуем использовать новый WindowsAppInitializer.
* ApplicationInsights.config считывает только `<InstrumentationKey>`; все остальные параметры игнорируются, вместо них используются параметры WindowsAppInitializer.
* Хранилище рынка собирается пакетом SDK автоматически.
* Исправлены различные ошибки, повышена стабильность работы и производительность.

### <a name="core-sdk"></a>Базовый пакет SDK
* Файл ApplicationInsights.config больше не требуется и не добавляется пакетом NuGet. Конфигурация может быть полностью задана в коде.
* Пакет NuGet больше не добавляет в решение файл целевых объектов. В связи с этим удалена автоматическая настройка DeveloperMode при отладочной сборке. DeveloperMode задается вручную с помощью кода.

## <a name="version-017"></a>Версия 0.17
### <a name="windows-app-sdk"></a>Пакет SDK для приложений Windows
* Пакет SDK для приложений Windows теперь поддерживает универсальные приложения Windows, созданные согласно Windows 10 Technical Preview и VS 2015 RC.

### <a name="core-sdk"></a>Базовый пакет SDK
* TelemetryClient по умолчанию инициализируется вместе с InMemoryChannel.
* Добавлен новый API — `TelemetryClient.Flush()`. Данный метод Flush вызовет незамедлительную блокирующую загрузку всех данных телеметрии, зарегистрированных по соответствующему клиенту. Это позволяет вручную запускать загрузку данных перед отключением системы.
* В целевой объект .Net 4.5 добавлен пакет NuGet. Этот целевой объект не имеет внешних зависимостей (зависимости BCL и EventSource удалены).

## <a name="version-016"></a>Версия 0.16
28.04.2015 (предварительная)

* Теперь пакет SDK включает поддержку целевой платформы DNX для мониторинга приложений на [платформе .NET Core](http://www.dotnetfoundation.org/NETCore5) .
* Экземпляры ```TelemetryClient``` больше не кэшируют ключ инструментирования. Теперь, если ключ инструментирования не задан в ```TelemetryClient``` явным образом, ```InstrumentationKey``` вернет значение null. Устраняет проблему при задании ```TelemetryConfiguration.Active.InstrumentationKey``` после сбора данных телеметрии. Модули телеметрии, такие как сборщик зависимостей, коллекция данных веб-запросов и сборщик счетчиков производительности будут использовать новый ключ инструментирования.

## <a name="version-015"></a>Версия 0.15
* Новое свойство ```Operation.SyntheticSource``` теперь доступно в ```TelemetryContext```. Теперь можно отметить элементы телеметрии, такие как "трафик нереального пользователя", и указать, как был создан этот трафик. Например, установив это свойство, можно легко отличить трафик автоматизации тестирования от трафика нагрузочного теста.
* Логика канала перемещена в отдельный NuGet, который называется Microsoft.ApplicationInsights.PersistenceChannel. Канал по умолчанию теперь называется InMemoryChannel
* Новый метод ```TelemetryClient.Flush``` позволяет синхронно очищать элементы телеметрии из буфера

## <a name="version-013"></a>Версия 0.13
Для более старых версий заметки о выпуске не доступны. 




<!--HONumber=Nov16_HO4-->


