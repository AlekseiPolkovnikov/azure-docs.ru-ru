---
title: Сравнение продуктов Майкрософт для мониторинга | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) — это облачное решение Майкрософт для управления ИТ-средой, которое помогает управлять локальной и облачной инфраструктурой и защищать ее.  В этой статье определяются различные службы в составе OMS и приводятся ссылки на подробные сведения об этих службах.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren

---
# <a name="microsoft-monitoring-product-comparison"></a>Сравнение продуктов Майкрософт для мониторинга
Эта статья содержит сравнение System Center Operations Manager (SCOM) и службы Log Analytics в Operations Management Suite (OMS) с точки зрения архитектуры, логики отслеживания ресурсов и анализа собираемых данных.  Здесь приводятся основные сведения об их различиях и относительных сильных сторонах.  

## <a name="basic-architecture"></a>Базовая архитектура
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Все компоненты SCOM устанавливаются в центре обработки данных.  [Агенты устанавливаются](http://technet.microsoft.com/library/hh551142.aspx) на компьютеры Windows и Linux, управляемые с помощью SCOM.  Агенты подключаются к [серверам управления](https://technet.microsoft.com/library/hh301922.aspx) , которые взаимодействуют с хранилищем данных и базой данных SCOM.  Для подключения к серверам управления агенты используют проверку подлинности домена.  Агенты за пределами доверенного домена могут выполнять проверку подлинности на основе сертификатов или подключаться к [серверу шлюза](https://technet.microsoft.com/library/hh212823.aspx).

Для работы SCOM требуется две базы данных SQL: одна для рабочих данных, а вторая для поддержки создания отчетов и анализа данных.  На [сервере отчетов](https://technet.microsoft.com/library/hh298611.aspx) выполняются службы SQL Reporting Services для составления отчетов по данным из хранилища данных. 

SCOM может отслеживать облачные ресурсы с помощью пакетов управления для таких продуктов, как [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708) и [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Эти пакеты управления используют один или несколько локальных агентов в качестве прокси-агентов для обнаружения облачных ресурсов и выполняющихся рабочих процессов с целью определения их производительности и доступности.  Прокси-агенты также используются для [наблюдения за сетевыми устройствами](https://technet.microsoft.com/library/hh212935.aspx) и другими внешними ресурсами.

Консоль управления — это приложение Windows, которое подключается к одному из серверов управления и позволяет администраторам просматривать и анализировать собранные данные, а также настраивать среду SCOM.  Веб-консоль может размещаться на любом сервере IIS. Она позволяет анализировать данные в браузере.

![Архитектура SCOM](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Служба Log Analytics
Большинство компонентов OMS размещается в облаке Azure, что позволяет осуществлять их развертывание и управление с минимальными затратами и усилиями по администрированию.  Все данные, собранные службой Log Analytics, хранятся в репозитории OMS.

Log Analytics может собирать данные из одного из трех источников:

* физические компьютеры и виртуальные машины с ОС Windows и [агентом управления Майкрософт (MMA)](https://technet.microsoft.com/library/mt484108.aspx) или ОС Linux и [агентом Operations Management Suite для Linux](https://technet.microsoft.com/library/mt622052.aspx)  (эти компьютеры могут быть локальными или виртуальными машинами в Azure или другой облачной службе);
* учетная запись хранения Azure с данными [системы диагностики Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) , собираемыми рабочей ролью, веб-ролью или виртуальной машиной Azure;
* [подключение к группе управления SCOM](https://technet.microsoft.com/library/mt484104.aspx).  В этой конфигурации агенты взаимодействуют с серверами управления SCOM, которые доставляют данные в базу данных SCOM, из которой они затем доставляются в хранилище данных OMS.
  Администраторы анализируют собранные данные и настраивают службу Log Analytics с помощью портала OMS, который размещается в Azure и доступен из любого браузера.  Для стандартных платформ предусмотрены мобильные приложения для доступа к этим данным.

![Архитектура службы Log Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Интеграция SCOM и Log Analytics
Если SCOM используется в качестве источника данных для Log Analytics, функции обоих продуктов можно использовать в гибридной среде.  В консоли управления можно настроить управление существующими агентами SCOM средствами OMS в дополнение к выполнению пакетов управления из SCOM.  
Данные из подключенной группы управления SCOM доставляются в Log Analytics одним из четырех способов.

* События и данные производительности собираются агентом и доставляются в SCOM.  Затем серверы управления в SCOM доставляют данные в Log Analytics.
* Некоторые события, такие как журналы IIS и события безопасности, по-прежнему доставляются агентом непосредственно в Log Analytics.
* Некоторые решения будут предоставлять дополнительное программное обеспечение агенту или требовать установки программного обеспечения для сбора дополнительных данных.  Эти данные обычно будут отправляться непосредственно в Log Analytics.
* Некоторые решения будут собирать данные непосредственно с серверов управления SCOM, источником которых не является агент.  Например, [решение для управления оповещениями](https://technet.microsoft.com/library/mt484092.aspx) собирает оповещения от SCOM после их создания.

## <a name="monitoring-logic"></a>Логика мониторинга
SCOM и Log Analytics работают с похожими данными, полученными от агентов, однако имеют фундаментальные отличия в том, каким образом они определяют и реализуют логику сбора данных и анализируют собранные данные.

### <a name="operations-manager"></a>Operations Manager
Логика мониторинга для SCOM реализована в [пакетах управления](https://technet.microsoft.com/library/hh457558.aspx) , которые содержат логику для обнаружения отслеживаемых компонентов, измерения работоспособности этих компонентов и сбора данных для анализа.  Наблюдение за данными может заключаться просто в сборе счетчиков событий или производительности или включать сложную логику, реализованную в сценарии.  Пакеты управления, которые включают полный мониторинг, доступны для различных [приложений Майкрософт и сторонних поставщиков](http://go.microsoft.com/fwlink/?LinkId=82105) , а также для оборудования и сетевых устройств.  Для пользовательских приложений можно [создавать собственные пакеты управления](http://aka.ms/mpauthor) .

Пакеты управления содержат несколько рабочих процессов, каждый из которых выполняет определенные функции мониторинга, например выборку счетчика производительности, проверку состояния службы или выполнение сценария.  Каждый рабочий процесс выполняется независимо и определяет собственные результаты, например базу данных, в которую он будет выполнять запись, или необходимость создания оповещения. 

Можно переопределить параметры рабочего процесса, например интервал выполнения, пороговое значение для создания ошибки и серьезность создаваемого оповещения.  Кроме того, можно реализовать дополнительные функции, добавив собственные рабочие процессы.

![Переопределения](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Пакеты управления устанавливаются в базе данных Operations Manager и автоматически передаются агентам серверами управления.  Каждый агент автоматически скачивает пакеты управления и загружает рабочие процессы, соответствующие установленным приложениям.  Данные, собранные агентом, передаются обратно на сервер управления для вставки в базу данных и хранилище данных SCOM.  Консоль управления позволяет просматривать и анализировать эти данные с помощью настраиваемых представлений, панелей мониторинга и отчетов, входящих в пакет управления.

На следующей схеме показано распределение пакетов управления.

![Поток пакета управления](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Служба Log Analytics
#### <a name="event-and-performance-collection"></a>Сборка событий и данных производительности
Log Analytics собирает события и счетчики производительности из систем агентов с помощью таких источников, как журнал событий Windows, журналы IIS и системный журнал.  Можно определить условия для сбора данных на портале Log Analytics, а затем создать запросы журнала для анализа собранных данных.  Набор стандартных условий определяется при создании рабочей области OMS, кроме того, вы можете определить дополнительные данные для конкретных приложений. 

![Определение журналов событий в Log Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Тогда как SCOM включает много подробных рабочих процессов, которые обычно определяют конкретные условия для данных и ответных действий, Log Analytics включает более общие условия для сбора данных.  Решения и запросы журнала обеспечивают более точные условия анализа и реагирования для определенных данных в облаке после их сбора.

#### <a name="solutions"></a>Решения
Решения предоставляют дополнительную логику для сбора и анализа данных.  Решения для добавления в подписку OMS можно выбрать в каталоге решений.

![Каталог решений](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Решения в основном работают в облаке, выполняя анализ событий и счетчиков производительности, собранных в репозитории OMS.  Кроме того, они могут определять дополнительные собираемые данные, которые можно анализировать, используя запросы журнала или дополнительный пользовательский интерфейс, предоставляемый решением на панели мониторинга OMS. 

Например, [решение для отслеживания изменений](https://technet.microsoft.com/library/mt484099.aspx) обнаруживает изменения конфигурации в системах агентов и записывает события в репозиторий OMS, которые затем можно проанализировать с помощью нескольких графических представлений, обобщающих обнаруженные изменения.  Можно выполнить детализацию из сводного представления в запросы журнала, которые выводят на экран подробные данные, собранные решением.

Хотя вы можете выбрать, какие решения добавить в подписку, возможность создавать собственные решения пока недоступна.  Можно выбрать события и счетчики производительности для сбора и создать пользовательские представления, основанные на запросах журнала.

На следующей схеме показана обобщенная логика мониторинга Log Analytics.

![Поток решения Log Analytics](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Мониторинг работоспособности
### <a name="operations-manager"></a>Operations Manager
SCOM может моделировать разные компоненты приложения и предоставлять сведения о работоспособности каждого из них в режиме реального времени.  Это позволяет не только просматривать обнаруженные ошибки и данные производительности в динамике по времени, но также оценивать фактическое состояние работоспособности приложения или системы и всех их компонентов в любой момент времени.  Так как модуль проверки работоспособности SCOM учитывает периоды времени, в которые доступно приложение, он также поддерживает соглашения об уровне обслуживания (SLA), которые анализируют доступность приложения в динамике по времени и создают соответствующие отчеты.

Например, в представлении ниже в реальном времени выводятся данные работоспособности ядер СУБД SQL, отслеживаемых SCOM.  Состояние работоспособности каждой базы данных для одного из ядер СУБД отображается в нижней половине представления.

![Представление состояния](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Ниже показано окно анализатора работоспособности для одного из ядер СУБД с мониторами, которые используются для определения его общей работоспособности.  Эти мониторы определяются в пакете управления SQL и выполняются для всех ядер СУБД SQL, обнаруженных SCOM.

![Анализатор работоспособности](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Можно сочетать компоненты в нескольких системах для измерения работоспособности распределенного приложения.  Это может быть особенно удобно для бизнес-приложений, включающих несколько распределенных компонентов.  Можно создать модель, которая измеряет работоспособность каждого компонента, участвующего в доступности приложения.

Примером такого пакета управления, который предоставляет модель для анализа распределенных компонентов, является Active Directory.  Примерная схема ниже демонстрирует работоспособность всей среды и отношения между лесами, доменами и контроллерами домена.  Каждый из этих компонентов включает подкомпоненты и несколько мониторов, аналогичных мониторам в приведенном выше примере SQL.

![Представление схемы SCOM](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Служба Log Analytics
OMS не включает общую подсистему моделирования приложений или измерения их работоспособности в режиме реального времени.  Отдельные решения могут оценивать общую работоспособность конкретных служб на основе собранных данных и устанавливать настраиваемую логику на агенте для выполнения анализа в реальном времени.  Так как решения выполняются в облаке с доступом к репозиторию OMS, как правило, они обеспечивают более глубокий анализ, чем обычно выполняемый с помощью пакетов управления. 

Например, [решения оценки AD и оценки SQL](https://technet.microsoft.com/library/mt484102.aspx) выполняют анализ собранных данных и предоставляют оценку для различных аспектов среды.  Она включает рекомендации по улучшениям, которые можно реализовать для повышения доступности и производительности среды.

![Решение оценки AD](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Анализ данных
SCOM и Log Analytics предоставляют разные возможности для анализа собранных данных.  SCOM включает представления и панели мониторинга в консоли управления для анализа последних данных в различных форматах и отчетах для представления данных из хранилища данных в табличной форме.  Log Analytics предоставляет полный язык запросов журнала и интерфейс для анализа данных в репозитории OMS.  Если SCOM используется в качестве источника данных для Log Analytics, репозиторий включает данные, собранные SCOM, таким образом средства Log Analytics можно использовать для анализа данных из обеих систем.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Views
Представления в консоли управления позволяют просматривать различные типы данных, собранные SCOM в разных форматах, как правило, в виде таблиц для событий, оповещений и данных состояния или графиков для данных производительности.  Представления выполняют минимальный анализ или консолидацию данных, однако позволяют выполнять фильтрацию по определенным условиям. 

![Views](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Пакеты управления обычно предоставляют несколько представлений, поддерживающих отслеживаемые приложения или системы.  Они могут включать представления состояния для разных объектов, которые обнаруживает пакет управления, представления оповещений для обнаруженных проблем и представления производительности для счетчиков.

Представления особенно удобны для анализа текущего состояния среды, включая открытые оповещения и состояние работоспособности отслеживаемых систем и объектов.  Можно выполнить детализацию событий или данных производительности, сопровождающих конкретное оповещения, чтобы выяснить его причину. Аналогично можно просмотреть сведения о производительности и работоспособности различных компонентов приложения, чтобы оценить его текущее состояние.

#### <a name="dashboards"></a>Панели мониторинга
Панели мониторинга в консоли управления главным образом работают с теми же данными, что и представления, однако поддерживают больше настроек и могут включать расширенные возможности визуализации.  Доступен набор стандартных панелей мониторинга, которые вы можете с легкостью настроить по своему усмотрению.  Можно также использовать мини-приложение PowerShell, которое может выводить на экран данные, возвращенные запросом PowerShell.

![Панель мониторинга](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Разработчики могут добавлять собственные компоненты на панели мониторинга, включаемые в пакеты управления.  Они могут предназначаться для конкретного приложения и быть узко специализированными, как например панель мониторинга в пакете управления SQL, показанная ниже.  Эту панель мониторинга можно также использовать как шаблон для пользовательских версий.

![Панель мониторинга SQL](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Отчеты
Отчеты в SCOM анализируют данные из хранилища данных в табличной форме.  Их можно печатать и планировать для автоматической доставки в разных форматах, включая PDF, CSV и Word.  Отчеты обрабатывают данные из хранилища данных, поэтому они особенно удобны для анализа долгосрочных тенденций.

Пакеты управления, как правило, включают настраиваемые отчеты для конкретных приложений.  Вы также можете выбрать отчет из библиотеки общих отчетов, чтобы настроить его для собственных приложений или для выполнения специального анализа.

Ниже приведен пример отчета о производительности, содержащего данные, собранные с помощью пакета управления Active Directory.

![Отчет](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Служба Log Analytics
Log Analytics включает [язык запросов](https://technet.microsoft.com/library/mt484120.aspx) , который можно использовать для выполнения анализа данных из нескольких приложений без необходимости создавать настраиваемые представления или отчеты.  Так как набор OMS реализован в облаке, производительность запросов и анализа данных не ограничивается характеристиками оборудования: поддерживается анализ запросов, включающих миллионы записей. 

Запросы в Log Analytics также служат основой для других функций.  Можно сохранить запрос, экспортировать его результаты в Excel или выполнять его автоматически с регулярными интервалами и создавать оповещение, если его результаты соответствуют определенным условиям.  

![Поток запроса журнала](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Ниже приведен пример запроса Log Analytics.  В этом примере все события, в имени которых указана строка started, возвращаются и группируются по коду события.  Пользователь просто предоставляет запрос, и Log Analytics динамически создает пользовательский интерфейс для выполнения анализа.  При выборе любого элемента в списке возвращается подробные сведения о событии.

![Запрос журнала](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Помимо выполнения специального анализа, запросы в Log Analytics можно сохранить для дальнейшего использования, а также добавить в [панель мониторинга OMS](http://technet.microsoft.com/library/mt484090.aspx) , как показано в следующем примере.

![панель мониторинга OMS](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Дальнейшие действия
* Выполните развертывание [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Зарегистрируйтесь в службе [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics).  

<!--HONumber=Oct16_HO2-->


