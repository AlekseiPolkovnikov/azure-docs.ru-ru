---
title: Мониторинг приложений в службе приложений Azure
description: Узнайте, как осуществлять мониторинг приложений в службе приложений Azure с помощью портала Azure.
services: app-service
documentationcenter: ''
author: btardif
manager: wpickett
editor: mollybos

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal

---
# Мониторинг приложений в службе приложений Azure
[Служба приложений](http://go.microsoft.com/fwlink/?LinkId=529714) предоставляет встроенные средства мониторинга на [портале Azure](https://portal.azure.com). Они позволяют просматривать **квоты**, **метрики** приложения и план службы приложений, а также настраивать получение **оповещений** и автоматическое **масштабирование** приложения на основе метрик.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Общие сведения о квотах и метриках
### Квоты
На приложения, размещенные в службе приложений, распространяются *ограничения* на использование ресурсов. Эти ограничения определены в рамках **плана службы приложений**, связанного с приложением.

Если приложение используется в режиме плана **Бесплатный** или **Общий**, ограничения на использование ресурсов выражены в виде **квот**.

Если приложение используется в режиме плана **Базовый**, **Стандартный** или **Премиум**, ограничения выражаются в виде **размера** выделенных ресурсов ("Малый", "Средний" или "Большой") и **числа экземпляров** (1, 2, 3 и т. д.) **плана службы приложений**.

Для приложений, работающих в режиме плана **Бесплатный** или **Общий**, предоставляются следующие **квоты**:

* **CPU (Short)** (ЦП (короткий интервал)):
  * объем ресурсов ЦП, которые может потребить приложение в течение 3 минут. Эта квота повторно назначается каждые 3 минуты;
* **CPU (Day)** (ЦП (на день)):
  * объем ресурсов ЦП, которые может потребить приложение в течение одного дня. Эта квота повторно назначается каждые 24 часа в полночь (в формате UTC);
* **Память**
  * объем ресурсов ЦП, которые может потребить приложение;
* **Пропускная способность**
  * объем исходящей пропускной способности, который может использовать приложение в течение одного дня. Эта квота повторно назначается каждые 24 часа в полночь (в формате UTC);
* **Filesystem** (Файловая система):
  * общий объем доступного пространства для хранения.

Для приложений, настроенных для работы в режиме плана **Базовый**, **Стандартный** и **Премиум**, доступна только квота **Filesystem** (Файловая система).

Дополнительные сведения об определенных квотах, ограничениях и функциях, доступных для разных номеров SKU службы приложений, см. в статье [Подписка Azure, границы, квоты и ограничения службы](../azure-subscription-service-limits.md#app-service-limits).

#### Принудительное применение квот
Если квота **CPU (short)** (ЦП (короткий интервал)), **CPU (Day)** (ЦП (на день)) или **Пропускная способность** будет превышена, работа приложения будет остановлена до повторного назначения квоты. В течение этого времени все входящие запросы будут завершаться с ошибкой **HTTP 403**. ![][http403]

В случае превышения квоты на **память** приложение будет перезапущено.

Если будет превышена квота на **файловую систему**, все операции записи, в том числе и записи в журналы, будут завершаться сбоем.

Квоту на использование можно увеличить или удалить из приложения путем изменения плана службы приложений.

### Метрики
**Метрики** предоставляют сведения о приложении или поведении плана службы приложений.

Для **приложения** доступны следующие метрики:

* **Среднее время ответа**:
  * среднее время обработки запроса приложением (в миллисекундах);
* **средний размер рабочего набора памяти;**
  * средний объем памяти (в МиБ/с), используемый приложением;
* **Время ЦП**:
  * количество времени ЦП (в секундах), используемого приложением. Дополнительные сведения об этих метриках см. в разделе о [времени и проценте использования ЦП](#cpu-time-vs-cpu-percentage);
* **Входящие данные**:
  * объем входящей пропускной способности, используемый приложением (в МиБ/с);
* **Выходные данные**
  * объем исходящей пропускной способности, используемый приложением (в МиБ/с);
* **HTTP 2xx**
  * Количество запросов, для которых возвращается ошибка HTTP с кодом состояния больше (равно) 200, но меньше 300.
* **HTTP 3xx**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния больше (равно) 300, но меньше 400;
* **HTTP 401**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния 401;
* **HTTP 403**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния 403;
* **HTTP 404**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния 404;
* **HTTP 406**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния 406;
* **HTTP 4xx**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния больше (равно) 400, но меньше 500;
* **Ошибки HTTP-сервера**:
  * количество запросов, для которых возвращается ошибка HTTP с кодом состояния больше (равно) 500, но меньше 600;
* **рабочий набор памяти;**
  * текущий объем используемой приложением памяти (в МиБ/с);
* **Requests (Запросы)**
  * общее количество запросов, для которых возвращается ошибка HTTP (независимо от кода состояния).

Для **плана службы приложений** доступны следующие метрики.

> [!NOTE]
> Метрики плана службы приложений поддерживаются только в рамках плана **Базовый**, **Стандартный** и **Премиум**.
> 
> 

* **Процент использования ЦП**
  * средний процент использования ЦП всеми экземплярами плана;
* **Процент использования памяти**
  * средний процент использование памяти всеми экземплярами плана;
* **Входящие данные**:
  * средний показатель использования входящей пропускной способности всеми экземплярами плана;
* **Выходные данные**
  * средний показатель использования исходящей пропускной способности всеми экземплярами плана;
* **Длина дисковой очереди**
  * среднее число запросов на чтение и запись, поставленных в очередь в хранилище. Большая длина очереди на диске замедляет производительность приложения из-за избыточных дисковых операции ввода-вывода;
* **Длина очереди HTTP**:
  * среднее число запросов HTTP, которые ставятся в очередь перед выполнением. Большая или увеличивающая длина очереди HTTP является признаком интенсивной нагрузки плана.

### Время и процент использования ЦП
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Существуют 2 метрики, которые отражают использование ЦП: **Время ЦП** и **Процент ЦП**.

Метрику **Время ЦП** рекомендуется использовать для приложений, настроенных для работы в режиме плана **Бесплатный** или **Общий**. Это связанно с тем, что квота на использование ЦП приложением, предоставленная в этих планах, определяется в минутах.

В свою очередь, метрику **Процент ЦП** рекомендуется использовать для приложений, настроенных для работы в режиме плана **Базовый**, **Стандартный** и **Премиум**, так как эти приложения можно масштабировать. Кроме того, эта метрика позволяет оценить общее потребление ресурсов ЦП всеми экземплярами.

## Степень детализации метрик и политика их хранения
Метрики для приложения и плана службы приложений записываются и обрабатываются в службе в соответствии со следующей степенью детализации и политиками хранения.

* Метрики со степенью детализации до **минут** хранятся **48 часов**.
* Метрики со степенью детализации до **часов** хранятся **30 дней**.
* Метрики со степенью детализации до **дней** хранятся **90 дней**.

## Мониторинг квот и метрик на портале Azure
Состояние **квот** и **метрик**, влияющих на приложение, можно просмотреть на [портале Azure](https://portal.azure.com).

![][quotas] Чтобы просмотреть сведения о **квотах**, выберите "Параметры" > **Квоты**. В UX отображаются следующие сведения о квотах: имя, интервал повторного назначения, текущие ограничение и значение.

![][metrics] Доступ к сведениям о **метриках** можно получить прямо в колонке ресурсов. Здесь также можно настроить отображение диаграммы. Для этого необходимо **щелкнуть** диаграмму и выбрать **Изменить диаграмму**. Этот параметр позволяет задать **диапазон времени**, **тип диаграммы** и отображаемые **метрики**.

Дополнительные сведения о метриках см. в статье [Отслеживание метрик службы](../azure-portal/insights-how-to-customize-monitoring.md).

## Оповещения и автомасштабирование
Приложение и план службы приложений поддерживают получение оповещений на основе метрик. Дополнительные сведения о настройках этой возможности см. в статье [Получение уведомлений об оповещениях](../azure-portal/insights-receive-alert-notifications.md).

Приложения службы приложений, настроенные для работы в режиме плана "Базовый", "Стандартный" и "Премиум", поддерживают возможность **автомасштабирования**. Это позволяет настроить правила мониторинга метрик плана службы приложений и изменить число экземпляров, благодаря чему можно получить дополнительные ресурсы или же сэкономить средства в случае избыточной подготовки. Дополнительные сведения об автомасштабировании см. в статье [Масштабирование числа экземпляров вручную или автоматически](../azure-portal/insights-how-to-scale.md) и [Рекомендации по автомасштабированию Azure Insights](../monitoring-and-diagnostics/insights-autoscale-best-practices.md).

> [!NOTE]
> Чтобы приступить к работе со службой приложений Azure до создания учетной записи Azure, перейдите к разделу [Пробное использование службы приложений](http://go.microsoft.com/fwlink/?LinkId=523751), где вы можете быстро создать кратковременное веб-приложение начального уровня в службе приложений. Никаких кредитных карт и обязательств.
> 
> 

## Изменения
* Указания по изменениям при переходе от веб-сайтов к службе приложений см. в разделе [Служба приложений Azure и ее влияние на существующие службы Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

[fzilla]: http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]: http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png

<!---HONumber=AcomDC_0914_2016-->