---
title: "Управление производительностью приложений с помощью Azure Application Insights | Документация Майкрософт"
description: "Отслеживайте производительность и использование работающего веб-приложения.  Обнаруживайте, рассматривайте и диагностируйте проблемы."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 379721d1-0f82-445a-b416-45b94cb969ec
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 8c5324742e42a1f82bb3031af4380fc5f0241d7f
ms.openlocfilehash: c719a82e6c2ae46080811190f9ca7783414f38f4


---
# <a name="application-performance-management-with-application-insights"></a>Управление производительностью приложений с помощью Application Insights
Application Insights — это расширяемая служба управления производительностью приложений (APM) для веб-разработчиков. Используйте ее для мониторинга вашего работающего веб-приложения. Она автоматически обнаруживает аномалии производительности. Эта служба включает мощные аналитические средства, которые помогут вам диагностировать проблемы и понять, что пользователи фактически делают в вашем приложении.  Эта служба помогает постоянно улучшать производительность и удобство использования. Application Insights работает с приложениями на самых разнообразных платформах, включая .NET, Node.js и J2EE, с локальным или облачным размещением. Эта служба также интегрируется с процессом разработки и выполнения операций и содержит точки подключения ко многим другим средствам.

![Составляйте диаграммы по действиям пользователей или детализируйте конкретные события.](./media/app-insights-overview/00-sample.png)

[Просмотрите этот ознакомительный анимированный ролик](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## <a name="how-does-it-work"></a>Как это работает?
Вы устанавливаете небольшой пакет инструментирования в приложение и настраиваете ресурс Application Insights на портале Microsoft Azure. Пакет инструментирования отслеживает приложение и отправляет данные телеметрии на портал. Портал отображает статистические диаграммы и предоставляет мощные средства, чтобы помочь вам диагностировать любые проблемы.

![Инструментирование Application Insights в приложении отправляет данные телеметрии в ресурс Application Insights на портале Azure.](./media/app-insights-overview/01-scheme.png)

В Application Insights есть несколько [стандартных модулей инструментирования](app-insights-configuration-with-applicationinsights-config.md) для сбора разных данных телеметрии, включая время ответов на запросы, исключения или вызовы зависимостей. Вы также можете [написать код для отправки пользовательской телеметрии](app-insights-api-custom-events-metrics.md) на портал.

### <a name="whats-the-overhead"></a>Увеличение нагрузки
Влияние на производительность вашего приложения очень мало. Вызовы отслеживания не приводят к блокировке, выполняются в пакетном режиме и отправляется в отдельном потоке.

## <a name="what-does-it-do"></a>Функция
Служба Application Insights предназначена для команды разработчиков. Она позволяет получить сведения о производительности и использовании приложения. Служба предоставляет:

Типы данных телеметрии:

* частота HTTP-запросов, время ответа, частота успешных выполнений;
* частота вызовов зависимостей (HTTP и SQL), время ответа, частота успешных выполнений;
* трассировки исключений из сервера и клиента;
* трассировки журналов диагностики;
* количество просмотров страниц, количество пользователей и сеансов, время загрузки браузера, исключения;
* частота вызовов AJAX, время ответа, частота успешных выполнений;
* счетчики производительности сервера;
* пользовательская телеметрия клиента и сервера;
* сегментация с учетом расположения клиента, версии браузера, версии ОС, экземпляра сервера, пользовательских измерений и многого другого;
* Тесты доступности

Средства диагностики и анализа:

* интеллектуальные и настраиваемые вручную оповещения о частоте сбоев, доступности и других показателях;
* диаграммы сводных показателей за определенный период времени;
* поиск экземпляров запросов, исключений, пользовательских событий, трассировок журналов, количества просмотров страниц, зависимостей и вызовов AJAX по журналам диагностики;
* аналитика — эффективный язык запросов для телеметрии;
* панели мониторинга — на них формируются диаграммы, необходимые для отслеживания компонентов приложения.

## <a name="how-do-i-use-it"></a>Как использовать службу?
### <a name="monitor"></a>Монитор
Установите Application Insights в веб-приложении, настройте доступность веб-тестов и:

* настройте панель мониторинга для комнаты своей команды, чтобы следить за нагрузкой, скоростью реагирования и производительностью зависимостей, загрузки страниц и вызовов AJAX;
* узнавайте, какие запросы выполняются медленнее всех и какие чаще всего не выполняются;
* просматривайте Live Stream при развертывании новых выпусков, чтобы сразу же узнавать о любом снижении производительности.

### <a name="diagnose"></a>Диагностика
При получении предупреждения или обнаружении проблемы:

* сопоставляйте сбои с исключениями, вызовами зависимостей и трассировками;
* проверяйте журналы трассировки и копии стека.

### <a name="assess"></a>Оценка
Оцените эффективность каждой новой развертываемой функции:

* планируйте измерение того, как пользователи используют новые UX или бизнес-функции;
* записывайте пользовательскую телеметрию в свой код, чтобы регистрировать использование в журнале;
* основывайте каждый цикл разработки на объективных данных телеметрии.

## <a name="get-started"></a>Приступая к работе
Application Insights — одна из многих служб, размещенных в Microsoft Azure, и данные телеметрии отправляются в нее для анализа и представления. Поэтому, чтобы приступить к каким-либо действиям, вам потребуется подписка на [Microsoft Azure](http://azure.com). Плата за регистрацию не взимается, и если выбрать [тарифный план](https://azure.microsoft.com/pricing/details/application-insights/) "Базовый" Application Insights, то плата не будет взиматься, пока ваше приложение не начнет значительно использовать ресурсы. Если у вашей организации уже есть подписка, в нее можно добавить вашу учетную запись Майкрософт.

Начать работу можно несколькими способами. Начните с того, который вам лучше подходит. Остальные можно использовать позже.

* **Во время выполнения — инструментирование веб-приложения на сервере.**  Не допускает обновление кода. Требуется административный доступ на сервер.
  * [**Локальные или размещенные на виртуальной машине службы IIS**](app-insights-monitor-performance-live-website-now.md);
  * [**веб-приложения или виртуальные машины Azure**](app-insights-monitor-performance-live-website-now.md);
  * [**J2EE**](app-insights-java-live.md).
* **Во время разработки — добавление Application Insights в код.**  Таким образом вы получаете возможность записывать пользовательскую телеметрию, а также инструментировать серверные и классические приложения.
  * [Visual Studio](app-insights-asp-net.md) 2013 с обновлением 2 или более поздняя версия.
  * Java в [Eclipse](app-insights-java-eclipse.md) или [другие средства](app-insights-java-get-started.md);
  * [Node.js](app-insights-nodejs.md)
  * [другие платформы.](app-insights-platforms.md)
* **[Инструментирование веб-страниц](app-insights-javascript.md)** для получения сведений о просмотрах страниц, вызовах AJAX и других данных телеметрии на стороне клиента.
* **[Тесты доступности](app-insights-monitor-web-app-availability.md)** с наших серверов для регулярной проверки связи с вашим веб-сайтом.

> [!NOTE]
> Возможно, на этом этапе вы захотите приступить к практике. Но если вас интересуют другие возможности Application Insights, продолжайте читать эту статью.
>
>

## <a name="explore-metrics"></a>Изучение метрик
Запустите приложение либо в режиме отладки на компьютере разработки, либо развернув его на сервере, и используйте его какое-то время. Затем войдите на [портал Azure](https://portal.azure.com).

Перейдите к колонке обзора Application Insights для приложения.

![Колонка "Обзор"](./media/app-insights-overview/01-find.png)

Обзор позволяет мгновенно увидеть, как работает приложение. Можно сравнить нагрузку (с точки зрения частоты запросов) со временем реагирования приложения на запросы. В случае несоразмерного увеличения времени ответа при росте нагрузки может потребоваться выделить больше ресурсов для приложения. Если после развертывания новой сборки отображается больше неудачных ответов, то можно выполнить откат.

#### <a name="get-more-detail"></a>Получение дополнительных сведений
Щелкните любую диаграмму, чтобы получить более информативный набор диаграмм. Например, диаграмма времени отклика сервера ведет к диаграммам, отображающим частоту запросов, время отклика и время отклика зависимостей (то есть служб, которые вызывает приложение).  

![Колонка "Серверы"](./media/app-insights-overview/04.png)

Диаграмма зависимостей удобна, так как позволяет узнать, отвечают ли базы данных и интерфейсы API REST, которые использует приложение, должным образом или приводят к задержкам.

#### <a name="customize-a-chart"></a>Настройка диаграммы
Попробуйте изменить одну из приведенных диаграмм. Например, если веб-приложение выполняется в коллекции экземпляров сервера, то можно сравнить время отклика на разных экземплярах сервера.

![Изменение диаграммы](./media/app-insights-overview/05.png)

1. Наведите указатель мыши на диаграмму и щелкните "Изменить".
2. Выберите метрику. На диаграмме можно отобразить несколько метрик, но только в определенных сочетаниях. Может потребоваться отменить какую-либо метрику, прежде чем можно будет выбрать нужную вам.
3. Используйте функцию "Группировать по" для сегментирования метрики по свойствам. В этом примере мы отображаем отдельные строки для различных значений времени ответа.

    Обратите внимание, что необходимо выбрать допустимое свойство для метрики, иначе на диаграмме не будут отображаться данные.
4. Выберите тип диаграммы. Диаграмма с областями и линейчатая диаграмма отображаются с накоплением, что удобно, когда используется тип агрегирования "Сумма".

[Дополнительные сведения об изучении метрик](app-insights-metrics-explorer.md).

## <a name="search-instance-data"></a>Поиск данных экземпляров
Для изучения проблемы полезно проверить определенные экземпляры события.

Пощелкайте диаграмму метрики, чтобы найти данные экземпляров с помощью соответствующих фильтров и диапазона времени. Например, пощелкайте количество запросов к серверу, чтобы просмотреть отчеты по отдельным запросам.

Или можно обратиться непосредственно к данным экземпляра, используя функцию поиска на странице обзора.

![Поиск экземпляров](./media/app-insights-overview/06.png)

Используйте фильтры, чтобы сосредоточиться на определенных типах событий и выбранных значениях свойств.

![Фильтрация по свойствам](./media/app-insights-overview/07.png)

Щелкните "…", чтобы увидеть полный список свойств или открыть другие события, связанные с тем же запросом. В этом примере у невыполненного запроса имеется связанный отчет об исключениях.

![Связанные элементы и сведения о свойствах](./media/app-insights-overview/08.png)

Откройте событие (в этом примере это связанное исключение) — и вы сможете создать рабочий элемент (при использовании Visual Studio Team Services для отслеживания задач).

![Создание рабочего элемента](./media/app-insights-overview/09.png)

## <a name="analytics"></a>Аналитика
[Аналитика](app-insights-analytics.md) — еще более эффективная функция поиска и анализа, с помощью которой можно писать SQL-подобные запросы к данным телеметрии, позволяющие искать конкретные проблемы или компилировать статистическую информацию.

![Аналитика](./media/app-insights-overview/10.png)

Откройте окно руководства, чтобы просмотреть и выполнить примеры запросов к своим данным, или ознакомьтесь с более полным [пошаговым руководством](app-insights-analytics-tour.md). Вы можете использовать запросы, которые предлагает IntelliSense, либо воспользоваться [полным справочником по языку](app-insights-analytics-reference.md).

Запросы обычно начинаются с имени потока данных телеметрии, например запросов, исключений или зависимостей. Откройте панель схемы слева, чтобы просмотреть список доступных потоков телеметрии. Запрос — это конвейер [операций запроса](app-insights-analytics-reference.md#queries-and-operators). Например, `where` — логический фильтр, а `project` вычисляет новые свойства. `summarize` агрегирует экземпляры, группируя их по определяемым вами функциям, а затем применяет статистические функции к сгруппированным данным.

Результаты могут быть [преобразованы в таблицы или диаграммы разных типов](app-insights-analytics-tour.md#charting-the-results).

## <a name="custom-telemetry"></a>Пользовательская телеметрия
Встроенная функция телеметрии, предоставляемая после установки Application Insights, позволяет анализировать количество, частоту успешных выполнений и время отклика веб-запросов к вашему приложению, а также зависимости, то есть вызовы из приложения к SQL и интерфейсам REST API. Вы также получите трассировки исключений и (при наличии монитора состояний на сервере) счетчики производительности системы. Если добавить фрагмент кода клиента в веб-страницы, то вы получите количество просмотров страниц и время загрузки страниц, исключения клиентов, частоту успешных вызовов AJAX и частоту ответов.

Анализ всех этих данных телеметрии позволит многое узнать о производительности и использовании приложения. Но иногда этого недостаточно. Может потребоваться отслеживать длину очереди, чтобы можно было настроить производительность. Или следить за количеством продаж и разделять их по расположению. Или узнать на стороне клиента, как часто пользователи нажимают определенную кнопку, чтобы настроить взаимодействие с пользователем.

В [API Application Insights](app-insights-api-custom-events-metrics.md) доступны вызовы `TrackEvent(name)` и `TrackMetric(name, value)` для отправки пользовательских событий и метрик. Существуют эквивалентные вызовы для стороны клиента.

Например, если ваша веб-страница — это одностраничная игра, то можно вставить строки в соответствующие места, чтобы вести журнал побед и поражений пользователей.

    appInsights.trackEvent("WinGame");
    ...
    appInsights.trackEvent("LoseGame");

Затем можно будет отобразить на диаграмме количества событий, группируя их по имени.

![Аналитика](./media/app-insights-overview/11.png)

### <a name="log-traces"></a>Трассировки журналов
В целях диагностики предоставляется пользовательское событие `TrackTrace(message)` , которое можно использовать для трассировки выполнения. В функциях поиска и аналитики вы можете выполнять поиск содержимого сообщения, которое может быть длиннее имени события.

Если вы уже используете платформу ведения журналов, например Log4Net, NLog, System.Diagnostic.Trace или Log4J, то эти вызовы трассировки может записывать Application Insights, и они появятся рядом с другими данными телеметрии. Средства Visual Studio автоматически добавляют соответствующий модуль пакета SDK.

## <a name="profiling-your-live-site"></a>Профилирование активного сайта

Не имеете представления, куда девается время? Профилировщик Application Insights будет отслеживать вызовы HTTP к активному веб-сайту и показывать, какие функции в коде выполнялись дольше всего. Сейчас профилировщик находится на этапе ограниченной предварительной версии. Вы можете [зарегистрироваться, чтобы попробовать его](https://aka.ms/AIProfilerPreview).

## <a name="dashboards"></a>Панели мониторинга
Многие приложения состоят из нескольких компонентов, таких как веб-служба и один или несколько внутренних обработчиков. Каждый компонент будет отслеживаться отдельным ресурсом Application Insights. Если ваша система работает в Azure, то, возможно, вы также используете и отслеживаете такие службы, как концентраторы событий и машинное обучение.

Чтобы отслеживать всю систему, можно выбрать наиболее важные диаграммы из различных приложений и закрепить их на [панели мониторинга](app-insights-dashboards.md)Azure, что позволит непрерывно следить за всей системой.

![Панели мониторинга](./media/app-insights-overview/12.png)

На самом деле можно создать несколько панелей инструментов. Например, можно создать панель мониторинга комнаты команды для наблюдения за общей работоспособностью системы, панель мониторинга для проектирования, нацеленную на использование различных функций, отдельные панели мониторинга для тестируемых компонентов и т. д.  

Панели мониторинга, как и ресурсы, могут совместно использоваться участниками команды.

## <a name="development-in-visual-studio"></a>Разработка в Visual Studio
Если вы используете среду Visual Studio для разработки приложения, то вы обнаружите в ней несколько встроенных инструментов Application Insights.

### <a name="diagnostic-search"></a>Поиск по журналу диагностики
В окне поиска отображаются события, которые были записаны в журнал. (Если во время настройки Application Insights вы вошли в Azure, эти же события вы сможете найти на портале.)

![Щелкните проект правой кнопкой мыши и последовательно выберите пункты "Application Insights" и "Поиск".](./media/app-insights-overview/34.png)

Бесплатный полнотекстовый поиск охватывает все поля в журнале событий. Например, можно выполнить поиск по фрагменту URL-адреса страницы, а также по значению свойства (например, город клиента) или ключевым словам в журнале трассировки.

Выберите любое событие, чтобы подробно просмотреть его свойства.

Кроме того, чтобы диагностировать невыполненные запросы или исключения, вы можете открыть вкладку "Связанные элементы".

![](./media/app-insights-overview/41.png)

### <a name="diagnostics-hub"></a>Концентратор диагностики
Концентратор диагностики (в Visual Studio 2015 или более поздней версии) отображает данные телеметрии сервера Application Insights при их создании. Это средство работает, даже если вы установили пакет SDK без подключения к ресурсу на портале Azure.

![Откройте окно средств диагностики и проверьте события Application Insights.](./media/app-insights-overview/31.png)

### <a name="exceptions"></a>Исключения
Если вы [настроили отслеживание исключений](app-insights-asp-net-exceptions.md), отчеты об исключениях будут отображаться в окне поиска.

Щелкните исключение, чтобы просмотреть трассировку стека. Если код приложения открыт в среде Visual Studio, можно щелкнуть трассировку стека в соответствующей строке кода.

![Трассировка стека исключений](./media/app-insights-overview/17.png)

Кроме того, в строке группы связанных элементов кода над каждым методом приведено количество исключений, зарегистрированных Application Insights за последние 24 часа.

![Трассировка стека исключений](./media/app-insights-overview/21.png)

### <a name="local-monitoring"></a>Локальный мониторинг
(Относится к Visual Studio 2015 с обновлением 2) Если ваш пакет SDK не отправляет данные телеметрии на портал Application Insights (то есть в файле ApplicationInsights.config нет ключа инструментирования), окно диагностики отображает данные телеметрии, полученные в ходе последнего сеанса отладки.

Это предпочтительно, если вы уже опубликовали предыдущую версию своего приложения. Кроме того, не рекомендуется, чтобы данные телеметрии, полученные на ваших сеансах отладки, смешивались с данными телеметрии на портале Application Insights, полученными от опубликованного приложения.

Это также полезно, если у вас есть [данные пользовательской телеметрии](app-insights-api-custom-events-metrics.md) , которые необходимо отладить перед отправкой на портал.

* *Сначала мы полностью настроили Application Insights для отправки данных телеметрии на портал. Но теперь нужно, чтобы данные телеметрии отображались только в Visual Studio.*

  * В параметрах окна поиска можно включить поиск локальной диагностики, который будет выполняться, даже если ваше приложение отправляет данные телеметрии на портал.
  * Чтобы остановить отправку данных телеметрии на портал, закомментируйте строку `<instrumentationkey>...` в файле ApplicationInsights.config. Когда данные телеметрии будут готовы к отправке на портал, раскомментируйте ее.

### <a name="trends"></a>Тренды
Тренды — это средство в Visual Studio для визуализации того, как изменяется поведение приложения со временем.

Нажмите кнопку **Обзор трендов телеметрии** на панели инструментов Application Insights или в окне поиска Application Insights. Выберите один из пяти стандартных запросов, чтобы приступить к работе. Анализировать разные наборы данных можно на основе типов данных телеметрии, диапазонов времени и других свойств.

Чтобы найти аномалии в данных, выберите один из вариантов аномалий в раскрывающемся списке "Тип представления". Параметры фильтрации в нижней части окна позволяют легко находить конкретные подмножества данных телеметрии.

![Тренды](./media/app-insights-overview/51.png)

## <a name="releasing-a-new-build"></a>Выпуск новой сборки
### <a name="live-metrics-stream"></a>Динамический поток метрик
Динамический поток метрик показывает метрики приложения прямо в данный момент, почти в реальном времени, с задержкой в 1 секунду. Это очень полезно при выпуске новой сборки, когда необходимо убедиться, что все работает правильно, а также при изучении инцидентов в режиме реального времени.

![В колонке обзора щелкните Live Stream (Динамический поток).](./media/app-insights-overview/live-stream.png)

В отличие от обозревателя метрик, динамический поток метрик отображает фиксированный набор метрик. Данные сохраняются только на то время, пока они отображаются на диаграмме, а затем удаляются.

### <a name="annotations"></a>аннотации
[Заметки к выпуску](app-insights-annotations.md) на диаграммах метрик показывают, где развернута новая сборка. С их помощью легко увидеть, повлияли ли ваши изменения на производительность приложения. Их может автоматически создавать [система сборки Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). Кроме того, [их можно создать в PowerShell](#create-annotations-from-powershell).

![Пример заметок с видимой корреляцией с временем ответа сервера](./media/app-insights-overview/00.png)

Заметки к выпуску являются функцией службы облачной сборки и выпуска Visual Studio Team Services.



## <a name="alerts"></a>Оповещения
Если с приложением что-то не так, можно немедленно узнать об этом.

Application Insights предоставляет три типа оповещений, которые доставляются по электронной почте.

### <a name="proactive-diagnostics"></a>Упреждающая диагностика
[Упреждающая диагностика](app-insights-proactive-failure-diagnostics.md) настраивается автоматически без вашего вмешательства. Если у сайта достаточный объем трафика, то вы получите электронное сообщение в случае увеличения числа невыполненных запросов, нехарактерного для времени суток или частоты запросов. Такое оповещение содержит диагностические сведения.

Вот пример оповещения.

![Пример умного оповещения, содержащий связанный со сбоем анализ кластера](./media/app-insights-overview/proactive-alert.png)

Второй тип упреждающего обнаружения позволяет обнаруживать взаимосвязи между ошибками и такими факторами, как расположение, ОС клиента или тип браузера.

### <a name="metric-alerts"></a>Оповещения о метриках
Вы можете настроить [оповещения о метриках](app-insights-alerts.md) , сообщающие, что какая-либо метрика (например, метрика количества сбоев, объема памяти или количества просмотров страницы) достигла порогового значения за определенное время.

![В обозревателе метрик выберите последовательно «Правила предупреждений», «Добавить предупреждение»](./media/app-insights-overview/appinsights-413setMetricAlert.png)

### <a name="availability"></a>Доступность
[Веб-тесты доступности](app-insights-monitor-web-app-availability.md) отправляют запросы к сайту с наших серверов, расположенных по всему миру. Они сообщают, когда сайт недоступен в Интернете или медленно отвечает на запросы.

![Пример веб-теста](./media/app-insights-overview/appinsights-10webtestresult.png)

## <a name="export-and-api"></a>Экспорт и API
Данные телеметрии с портала Application Insights можно получить несколькими способами.

* Для поиска и извлечения данных, в том числе выполнения запросов аналитики, можно использовать [REST API доступа к данным](https://dev.applicationinsights.io/). 
* Экспортируйте [запросы аналитики на панели мониторинга Power BI](app-insights-export-power-bi.md) и просматривайте результаты на визуализациях Power BI с возможностью автоматического обновления.
* [Непрерывный экспорт](app-insights-export-telemetry.md) — идеальное решение, если вам нужно сохранить большую часть данных телеметрии на более длительный срок хранения, чем стандартный.
* Таблицы [метрик](app-insights-metrics-explorer.md#export-to-excel), результаты поиска и [аналитики](app-insights-analytics.md) можно экспортировать в электронную таблицу Excel.

![Просмотр данных в Power BI](./media/app-insights-overview/210.png)

## <a name="data-management"></a>Управление данными
Существуют определенные ограничения для использования Application Insights, которые в некоторой степени зависят от выбранной ценовой схемы. Основные ограничения относятся к:

* частоте данных телеметрии в минуту;
* количеству точек данных в месяц;
* сроку хранения данных.

[Выборка](app-insights-sampling.md) — это механизм, позволяющий сократить затраты и избежать регулирования. Он удаляет некоторую часть данных телеметрии, оставляя репрезентативную выборку. Связанные элементы (например, исключения и запросы, породившие их) сохраняются или отклоняются вместе. Для приложений ASP.NET выборка выполняется автоматически. В противном случае ее можно настроить для приема данных на портале.

## <a name="next-steps"></a>Дальнейшие действия
Приступите к работе во время выполнения с помощью:

* [сервера IIS;](app-insights-monitor-performance-live-website-now.md)
* [сервера J2EE.](app-insights-java-live.md)

Приступите к работе во время разработки с помощью:

* [ASP.NET](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Node.js](app-insights-nodejs.md)

## <a name="support-and-feedback"></a>Поддержка и обратная связь
* Вопросы и проблемы
  * [Устранение неполадок][qna]
  * [Форум MSDN](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)
  * [Stackoverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
  * [Получение поддержки Developer](app-insights-get-dev-support.md)
* Ваши предложения:
  * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Блог:
  * [Блог Application Insights](https://azure.microsoft.com/blog/tag/application-insights)

## <a name="videos"></a>Видеоролики
[![Анимационное ознакомительное видео](./media/app-insights-overview/video-front-1.png)](https://www.youtube.com/watch?v=fX2NtGrh-Y0)

> [!VIDEO https://channel9.msdn.com/Series/ConnectOn-Demand/218/player]
>
> [!VIDEO https://channel9.msdn.com/Series/ConnectOn-Demand/231/player]
>
> [!VIDEO https://channel9.msdn.com/Series/ConnectOn-Demand/222/player]
>
> [Ознакомительная анимация](https://www.youtube.com/watch?v=fX2NtGrh-Y0)
>
>

<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[клиент]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[платформы]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md



<!--HONumber=Nov16_HO4-->


