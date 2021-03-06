---
title: "Application Insights для облачных служб Azure"
description: "Эффективное отслеживание веб-ролей и рабочих ролей с помощью Application Insights"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: article
ms.workload: tbd
ms.date: 11/02/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1022f5a2bbb9b61ce7d941de9e2a5582db22b91b


---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights для облачных служб Azure
[Приложения облачных служб Microsoft Azure](https://azure.microsoft.com/services/cloud-services/) можно отслеживать с помощью [Application Insights][start], чтобы оценить показатели доступности, производительности, сбоев и использования. Благодаря получаемым данным о производительности и эффективности работы приложения на практике вы можете принимать осознанные решения о направлении разработки в каждом жизненном цикле.

![Пример](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Перед началом работы
Что вам понадобится:

* Потребуется подписка [Microsoft Azure](http://azure.com). Войдите, используя учетную запись Майкрософт, которая уже может быть у вас для Windows, XBox Live или других облачных служб Майкрософт. 
* Инструменты Microsoft Azure 2.9 или более поздней версии.
* Developer Analytics Tools 7.10 или более поздней версии.

## <a name="quick-start"></a>Быстрый запуск
Самый быстрый и простой способ настроить мониторинг облачной службы с помощью Application Insights — выбрать этот вариант при публикации своей службы в Azure.

![Пример](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Этот вариант позволяет инструментировать приложение во время выполнения, что обеспечивает все данные телеметрии, необходимые для мониторинга запросов, исключений и зависимостей в веб-роли, а также счетчиков производительности рабочих ролей. Все диагностические трассировки, генерируемые вашим приложением, также будут отправляться в Application Insights.

Если это все, что вам нужно, то все готово. Следующие шаги — [просмотр метрик приложения](app-insights-metrics-explorer.md), [запрос данных с помощью Аналитики](app-insights-analytics.md), и, возможно, настройка [мониторинга](app-insights-dashboards.md). Вам может потребоваться настроить [тесты доступности](app-insights-monitor-web-app-availability.md) и [добавить в веб-страницы код ](app-insights-javascript.md) для мониторинга производительности в браузере.

Но можно также получить дополнительные возможности:

* отправлять данные для различных компонентов и конфигураций борки в отдельные ресурсы;
* добавить пользовательские данные телеметрии из своего приложения.

Если эти возможности представляют для вас интерес, продолжайте чтение.

## <a name="sample-application-instrumented-with-application-insights"></a>Пример приложения, инструментированного с помощью Application Insights
Взгляните на этот [пример приложения](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) , в котором служба Application Insights добавлена в облачную службу с двумя рабочими ролями, размещенными в Azure. 

Ниже описано, как адаптировать собственный проект облачной службы таким же образом.

## <a name="plan-resources-and-resource-groups"></a>Планирование ресурсов и групп ресурсов
Данные телеметрии из приложения будут сохранены, проанализированы и отображены в ресурсе Azure типа Application Insights. 

Каждый ресурс относится к группе ресурсов. Группы ресурсов используются для управления затратами, предоставления доступа участникам команды и развертывания обновлений при помощи отдельной согласованной транзакции. Например, вы можете [написать сценарий для развертывания](../resource-group-template-deploy.md) облачной службы Azure и ее ресурсов мониторинга Application Insights одной операцией.

### <a name="resources-for-components"></a>Ресурсы для компонентов
Рекомендуемая схема — создавать отдельный ресурс для каждого компонента приложения, то есть каждой веб-роли и рабочей роли. Вы можете анализировать каждый компонент отдельно, но можно создать [панель мониторинга](app-insights-dashboards.md), содержащую ключевые диаграммы для всех компонентов. Это позволит сравнивать и отслеживать их вместе. 

Альтернативной схемой является отправка данных телеметрии из нескольких ролей в один ресурс, но с [добавлением свойства измерения в каждый элемент телеметрии](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer), определяющего его исходную роль. В этой схеме диаграммы метрик (например, исключений) обычно будут отображаться вместе с показателями других ролей. Но при необходимости вы можете сегментировать диаграммы по идентификатору роли. Результаты поиска также можно фильтровать по одному измерению. Этот вариант делает более удобным одновременный просмотр информации, но может привести к некоторой путанице с ролями.

Данные телеметрии браузера обычно добавляются в тот же ресурс, что и данные соответствующей серверной веб-роли.

Поместите ресурсы Application Insights для различных компонентов в одну группу ресурсов. Это позволяет легко управлять ими одновременно. 

### <a name="separating-development-test-and-production"></a>Разделение данных для разработки, тестирования и эксплуатации
При разработке пользовательских событий для следующего компонента в то время, когда предыдущая версия еще активна, необходимо отправлять данные телеметрии для разработки в отдельный ресурс Application Insights. В противном случае будет трудно найти данные телеметрии тестирования среди всего трафика, поступающего от активного сайта.

Чтобы избежать такой ситуации, создайте отдельные ресурсы для каждой конфигурации сборки или "метки" (разработка, тестирование, эксплуатация и т. п.) своей системы. Поместите ресурсы для каждой конфигурации сборки в отдельную группу ресурсов. 

Чтобы отправлять данные телеметрии в соответствующие ресурсы, можно настроить пакет SDK для Application Insights, чтобы он собирал разные ключи инструментирования, в зависимости от конфигурации сборки. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Создание ресурса Application Insights для каждой роли
Если вы решили создать отдельный ресурс для каждой роли и, возможно, отдельный набор для каждой конфигурации сборки, то создать их проще всего будет на портале Application Insights. Если создается много ресурсов, можно [автоматизировать этот процесс](app-insights-powershell.md).

1. На [портале Azure][portal] создайте ресурс Application Insights. Для параметра типа приложения выберите приложение ASP.NET. 
   
   ![Нажмите "Создать" и "Application Insights"](./media/app-insights-cloudservices/01-new.png)
2. Обратите внимание, что каждый ресурс идентифицируется с помощью ключа инструментирования. Это может понадобиться позже, если вы захотите вручную настроить или проверить конфигурацию пакета SDK.
   
   ![Нажмите "Свойства", выберите ключ и нажмите сочетание клавиш CTRL + C](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Настройка системы диагностики Azure для каждой роли
Настройте мониторинг приложения с помощью Application Insights. Для веб-ролей это обеспечивает мониторинг производительности, оповещения и диагностику, а также анализ сведений об использовании. Для других ролей можно искать и отслеживать диагностические данные Azure, например о событиях перезапуска, показаниях счетчиков производительности и вызовах System.Diagnostics.Trace. 

1. В обозревателе решений Visual Studio в разделе &lt;Ваша_облачная_служба&gt; > "Роли" откройте свойства каждой роли.
2. В разделе **Конфигурация** выберите **Отправка диагностических данных в Application Insights** и выберите соответствующий ресурс Application Insights, созданный ранее.
   
   * Если вы решили использовать отдельный ресурс Application Insights для каждой конфигурации сборки, сначала выберите эту конфигурацию.

![В свойствах каждой роли Azure настройте Application Insights.](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Это приведет к добавлению ключей инструментирования Application Insights в файлы `ServiceConfiguration.*.cscfg`. ([Пример кода](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Если требуется изменить уровень диагностических сведений, отправляемых в Application Insights, это можно сделать, [напрямую изменив CSCFG-файлы](app-insights-azure-diagnostics.md).

## <a name="a-namesdkainstall-the-sdk-in-each-project"></a><a name="sdk"></a>Установка пакета SDK в каждый проект
Этот параметр позволяет добавить пользовательские данные бизнес-телеметрии в любую роль для более тщательного анализа того, как приложение используется и работает.

В Visual Studio добавьте пакет SDK для Application Insights в каждый проект облачного приложения.

1. Измените пакеты NuGet проекта.
   
    ![Щелкните проект правой кнопкой мыши и выберите пункт "Управление пакетами Nuget"](./media/app-insights-cloudservices/03-nuget.png)
2. **Веб-роли**: добавьте [Application Insights for Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web). Эта версия пакета SDK содержит модули, которые собирают данные HTTP-запросов и добавляют контекст сервера, например информацию о роли.
   
    **Рабочие роли**: добавьте [Application Insights for Windows Servers](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/).
   
    ![Поиск Application Insights](./media/app-insights-cloudservices/04-ai-nuget.png)
3. Настройте пакет SDK для отправки данных в ресурсы Application Insights.
   
    В соответствующей функции запуска задайте ключ инструментирования из параметра конфигурации, заданного в CSCFG-файле.
   
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Сделайте это для каждой роли в вашем приложении. См. указанные ниже примеры.
   
   * [Веб-роль](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Рабочая роль](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [При работе с веб-страницами](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13)   
4. Задайте для файла ApplicationInsights.config незамедлительное копирование в выходной каталог. 
   
    (в CONFIG-файле имеются сообщения, указывающие, куда именно поместить ключ инструментирования. Тем не менее для облачных приложений лучше задать его в CSCFG-файле. Это обеспечит правильную идентификацию роли на портале.)

#### <a name="run-and-publish-the-app"></a>Запуск и публикация приложения
Запустите приложение и войдите в Azure. Откройте созданные ресурсы Application Insights, и вы увидите отдельные точки данных в области [Поиск](app-insights-diagnostic-search.md) и объединенные данные в [обозревателе метрик](app-insights-metrics-explorer.md). 

Добавьте дополнительные данные телеметрии (см. разделы ниже), а затем опубликуйте приложение для оперативного получения отзывов о диагностике и использовании. 

#### <a name="no-data"></a>Данные отсутствуют?
* Откройте плитку [Поиск][diagnostic], чтобы просмотреть отдельные события.
* Используйте приложение, открывая различные страницы, чтобы создать некоторый объем данных телеметрии.
* Подождите несколько секунд и нажмите "Обновить".
* См. [Устранение неполадок][qna].

## <a name="view-azure-diagnostic-events"></a>Просмотр событий диагностики Azure
Где найти средства диагностики:

* Счетчики производительности отображаются в виде настраиваемых метрик. 
* Журналы событий Windows отображаются в виде трассировок и настраиваемых событий.
* Журналы приложений, журналы ETW и все журналы инфраструктуры диагностики отображаются в виде трассировок.

Чтобы просмотреть счетчики производительности и счетчики событий, откройте [обозреватель метрик](app-insights-metrics-explorer.md) и добавьте новую диаграмму:

![Диагностические данные Azure](./media/app-insights-cloudservices/23-wad.png)

Используйте функцию [Поиск](app-insights-diagnostic-search.md) для поиска в различных журналах трассировки, отправляемых системой диагностики Azure. Например, если в роли есть необработанное исключение, которое вызвало сбой и переработку этой роли, соответствующие данные появятся в канале "Приложение" журнала событий Windows. Функцию поиска можно использовать для поиска ошибки в журнале событий Windows и получения полной трассировки стека для исключения, позволяющей установить причину возникшей проблемы.

![Поиск диагностических данных Azure](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Дополнительные данные телеметрии
В следующих подразделах показано, как получить дополнительные данные телеметрии из различных аспектов приложения.

## <a name="track-requests-from-worker-roles"></a>Отслеживание запросов из рабочих ролей
В веб-ролях модуль запросов собирает данные об HTTP-запросах автоматически. Примеры переопределения поведения коллекции по умолчанию см. на странице с [примером роли MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole). 

Производительность вызовов для рабочих ролей можно регистрировать, отслеживая их таким же образом, как запросы HTTP. В Application Insights тип телеметрии запроса определяет единицу работы для указанного сервера, которую можно учесть и которая может быть выполнена или не выполнена. Во время автоматической регистрации HTTP-запросов пакетом SDK можно добавить собственный код для отслеживания запросов к рабочим ролям.

Ознакомьтесь с двумя примерами рабочих ролей, которые инструментированы для отправки отчетов по запросам: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) и [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB).

## <a name="exceptions"></a>Исключения
Способы сбора необработанных исключений, выдаваемых веб-приложениями разных типов, см. в статье [Настройка Application Insights: диагностика исключений](app-insights-asp-net-exceptions.md).

Образец веб-роли имеет контроллеры MVC5 и веб-API 2. Необработанные исключения из 2 регистрируются такими объектами:

* объектом [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs), заданным [здесь](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) для контроллеров MVC5;
* объектом [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs), заданным [здесь](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) для контроллеров веб-API 2.

Для рабочих ролей есть два способа отслеживания исключений.

* TrackException(ex);
* Если вы добавили пакет NuGet прослушивателя трассировки Application Insights, вы можете вносить исключения в журнал с помощью System.Diagnostics.Trace. [Пример кода](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Счетчики производительности
По умолчанию собираются приведенные ниже счетчики.

    * \Process(??APP_WIN32_PROC??)\% Загруженность процессора
    * \Память\доступные байты
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# Исключений в секунду
    * \Process(??APP_WIN32_PROC??)\Байт исключительного пользования
    * \Process(??APP_WIN32_PROC??)\I/O — обмен данными, байт в секунду
    * \Processor(_Total)\% Загруженность процессора

А здесь приведены счетчики, которые собираются для веб-ролей.

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Запросов в секунду    
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Время выполнения запросов
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Запросы в очереди приложений

Вы можете указать дополнительные пользовательские счетчики производительности или счетчики производительности Windows, как показано [ниже](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14)

  ![Счетчики производительности](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Коррелированная телеметрия для рабочих ролей
Если вы видите причины неудавшихся запросов или запросов с высокой задержкой, это значит, что вы обладаете широким спектром возможностей диагностики. При работе с веб-ролями пакет SDK автоматически настраивает корреляцию между связанными данными телеметрии. Для рабочих ролей вы можете использовать пользовательский инициализатор телеметрии, чтобы задать атрибут общего контекста Operation.Id, и тогда эта возможность будет доступна для всех сведений телеметрии. Вы сможете узнать причину сбоя или задержки — зависимость или ваш код — с одного взгляда. 

Этот процесс описывается далее.

* Задайте идентификатор корреляции в объекте CallContext, как показано [здесь](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36). В этом случае мы используем идентификатор запроса как идентификатор корреляции.
* Добавьте пользовательскую реализацию TelemetryInitializer, которая задает Operation.Id в наборе correlationId. См. здесь: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Добавьте пользовательский инициализатор телеметрии. Это можно сделать в файле ApplicationInsights.config или в коде, как показано [ниже](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

Вот и все! Взаимодействие с порталом уже настроено, и вы можете просматривать все связанные сведения телеметрии одновременно.

![Коррелированные данные телеметрии](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Данные телеметрии клиента
[Добавьте пакет SDK JavaScript на веб-страницы][client], чтобы получать в браузере данные телеметрии, включая просмотров страниц, время загрузки страницы, исключения скриптов, а также записывать настраиваемую телеметрию в скрипты страниц.

## <a name="availability-tests"></a>Тесты доступности
[Настройте веб-тесты][availability], которые помогут быть уверенными в том, что приложение остается работоспособным и правильно отвечает на запросы.

## <a name="display-everything-together"></a>Отображение всей информации вместе
Чтобы получить общее представление о системе, можно отобразить ключевые диаграммы мониторинга на одной [панели мониторинга](app-insights-dashboards.md). Например, можно закрепить количество запросов и сбоев для каждой роли. 

Если в системе используются другие службы Azure, такие как Stream Analytics, добавьте и их диаграммы мониторинга. 

При наличии клиентского мобильного приложения добавьте код, отправляющий пользовательские события для ключевых операций пользователей, и создайте [мост HockeyApp](app-insights-hockeyapp-bridge-app.md). Создайте запросы в [Аналитике](app-insights-analytics.md) для отображения числа событий и закрепите эти показатели на панели мониторинга.

## <a name="example"></a>Пример
[примере](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) отслеживается служба, которая имеет веб-роль и две рабочие роли.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Исключение "метод не найден" при выполнении в облачных службах Azure
Выполняется сборка для .NET 4.6? Версия 4.6 не поддерживается автоматически в ролях облачных служб Azure. [установите версию 4.6 в каждой роли](../cloud-services/cloud-services-dotnet-install-dotnet.md) .

## <a name="next-steps"></a>Дальнейшие действия
* [Настройка системы диагностики Azure для отправки данных в Application Insights](app-insights-azure-diagnostics.md)
* [Автоматизация создания ресурсов Application Insights](app-insights-powershell.md)
* [Автоматизация системы диагностики Azure](app-insights-powershell-azure-diagnostics.md)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 



<!--HONumber=Nov16_HO3-->


