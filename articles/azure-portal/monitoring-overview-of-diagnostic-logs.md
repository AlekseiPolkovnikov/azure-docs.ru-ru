---
title: Обзор журналов диагностики Azure | Microsoft Docs
description: Узнайте, что такое журналы диагностики Azure и как их использовать для просмотра событий, происходящих в ресурсе Azure.
author: johnkemnetz
manager: rboucher
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics

ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: johnkem

---
# Обзор журналов диагностики Azure
**Журналы диагностики Azure** генерируются ресурсом; они содержат подробные и своевременные данные о работе этого ресурса. Содержимое этих журналов зависит от типа ресурса (например, журналы событий Windows — это одна из категорий журналов диагностики для виртуальных машин, а журналы больших двоичных объектов, таблиц и очередей — это журналы диагностики для учетных записей хранения). Эти журналы отличаются от [журнала действий (раньше он назывался журналом аудита или операционным журналом)](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), который содержит информацию об операциях с ресурсами в рамках подписки. Не все ресурсы поддерживают описанный здесь новый тип журналов диагностики. Типы ресурсов, которые поддерживают новые журналы диагностики, перечислены ниже в списке поддерживаемых служб.

![Логическое расположение журналов диагностики](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## Что можно делать с журналами диагностики
Ниже описано несколько доступных операций с журналами диагностики.

* Сохранение журналов в **учетную запись хранения** для аудита или проверки вручную. В **параметрах диагностики** вы также можете задать время хранения (в днях).
* [Потоковая передача журналов в **концентраторы событий**](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) для обработки в сторонней службе или пользовательском аналитическом решении, например PowerBI.
* Анализ журналов с помощью [OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md)

## Параметры диагностики
Журналы диагностики для всех ресурсов, кроме вычислительных, настраиваются с помощью параметры диагностики. **Параметры диагностики** для ресурса определяют следующие настройки.

* Куда будут отправляться журналы диагностики (учетная запись хранения, концентраторы событий и (или) OMS Log Analytics).
* Какие категории журналов будут отправляться.
* Как долго будет храниться в учетной записи хранения каждая категория журналов (если указать здесь 0 дней, журналы будут храниться неограниченно долго). В противном случае нужно указать значение от 1 до 2 147 483 647. Если политики хранения заданы, но хранение журналов в учетной записи отключено (например, выбраны только варианты концентраторов событий или OMS), политики хранения не будут применены.

Эти параметры легко настраиваются в колонке диагностики для ресурса на портале Azure, а также с помощью Azure PowerShell, интерфейса командной строки или [REST API Insights](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!WARNING]
> Для журналов диагностики и метрик для вычислительных ресурсов (например, виртуальных машин или Service Fabric) доступен [отдельный механизм настройки и выбора выходных данных](../azure-diagnostics.md).
> 
> 

## Как включить сбор журналов диагностики
Сбор журналов диагностики можно включить во время или после создания ресурса в колонке ресурса на портале. Журналы диагностики можно также включить в любой момент с помощью Azure PowerShell, интерфейса командной строки или REST API Insights.

> [!TIP]
> При работе с некоторыми ресурсами эти инструкции могут требовать корректировки. Изучите связи на схеме, представленной в конце этой страницы, чтобы разобраться в дополнительных действиях, которые могут потребоваться при использовании некоторых типов ресурсов.
> 
> 

[В этой статье показано, как использовать шаблон ресурсов для включения параметров диагностики при создании ресурса](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md)

### Включение журналов диагностики на портале
Чтобы включить журналы диагностики на портале Azure при создании некоторых типов ресурсов, сделайте следующее.

1. Нажмите кнопку **Создать** и выберите ресурс, который вас интересует.
2. Настроив основные параметры и выбрав размер, в колонке **Параметры** раздела **Мониторинг** выберите значение **Включено** и укажите учетную запись хранения для передачи журналов диагностики. Отправка диагностических данных в учетную запись хранения тарифицируется по стандартным расценкам на хранение и передачу данных.
   
   ![Включение журналов диагностики при создании ресурса](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3. Нажмите **ОК**, чтобы создать ресурс.

Чтобы включить журналы диагностики на портале Azure после создания ресурса, сделайте следующее.

1. Перейдите к колонке ресурса и откройте колонку **Диагностика**.
2. Щелкните **Включено** и выберите учетную запись хранения и (или) концентратор событий.
   
   ![Включение журналов диагностики после создания ресурса](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3. В разделе **Журналы** выберите, какие **категории журналов** вы хотите собирать или передавать.
4. Щелкните **Сохранить**.

### Программное включение журналов диагностики
Чтобы включить журналы диагностики с помощью Azure PowerShell, используйте следующие команды.

Эта команда включает отправку журналов диагностики в учетную запись хранения:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

StorageAccountID — это идентификатор ресурса учетной записи хранения, в которую будут отправляться журналы.

Эта команда включает потоковую передачу журналов диагностики в концентратор событий:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

ServiceBusRuleID — это строка в таком формате: `{service bus resource ID}/authorizationrules/{key name}`.

Чтобы включить отправку журналов диагностики в рабочую область Log Analytics, используйте следующую команду.

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

Идентификатор рабочей области Log Analytics можно получить на портале Azure.

Можно объединять эти параметры, чтобы получить несколько вариантов вывода.

Чтобы включить журналы диагностики с помощью интерфейса командной строки Azure, используйте следующие команды.

Эта команда включает отправку журналов диагностики в учетную запись хранения:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

StorageAccountID — это идентификатор ресурса учетной записи хранения, в которую будут отправляться журналы.

Эта команда включает потоковую передачу журналов диагностики в концентратор событий:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

ServiceBusRuleID — это строка в таком формате: `{service bus resource ID}/authorizationrules/{key name}`.

Чтобы включить отправку журналов диагностики в рабочую область Log Analytics, используйте следующую команду.

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

Идентификатор рабочей области Log Analytics можно получить на портале Azure.

Можно объединять эти параметры, чтобы получить несколько вариантов вывода.

Изменение параметров диагностики с помощью API REST Insights описывается в [этом документе](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## Управление параметрами диагностики на портале
Чтобы убедиться, что для всех ресурсов правильно настроены параметры диагностики, можно перейти к колонке **Мониторинг** на портале и открыть колонку **Журналы диагностики**.

![Колонка "Журналы диагностики" на портале](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Чтобы найти колонку "Мониторинг" может понадобиться щелкнуть "Больше служб".

В этой колонке можно просматривать и фильтровать все ресурсы, поддерживающие журналы диагностики, чтобы узнать, включена ли для них диагностика и в какую учетную запись хранения, концентратор событий и (или) рабочую область Log Analytics передаются эти журналы.

![Результаты в колонке "Журналы диагностики" на портале](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Если щелкнуть ресурс, отобразятся все журналы, сохраненные в учетной записи хранения, и вы сможете отключить диагностику или изменить ее параметры. Щелкните значок скачивания, чтобы скачать журналы за определенный период времени.

![Колонка "Журналы диагностики" одного ресурса](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [!NOTE]
> Это представление будет содержать журналы диагностики, которые можно будет скачать, только если вы настроили параметры для их сохранения в учетную запись хранения.
> 
> 

## Поддерживаемые службы и схемы для журналов диагностики
Схема для журналов диагностики зависит от типа ресурса и категории журнала. Ниже приведены поддерживаемые службы и схемы.

| служба | Схемы и документы |
| --- | --- |
| Программный балансировщик нагрузки |[Служба анализа журналов для балансировщика нагрузки Azure (предварительная версия)](../load-balancer/load-balancer-monitor-log.md) |
| группы сетевой безопасности; |[Аналитика журналов для групп безопасности сети](../virtual-network/virtual-network-nsg-manage-log.md) |
| Шлюзы приложений |[Ведение журнала диагностики для шлюза приложений](../application-gateway/application-gateway-diagnostics.md) |
| хранилище ключей; |[Ведение журнала хранилища ключей Azure](../key-vault/key-vault-logging.md) |
| поиск Azure; |[Включение и использование аналитики поискового трафика](../search/search-traffic-analytics.md) |
| Хранилище озера данных |[Доступ к журналам диагностики Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Аналитика озера данных |[Доступ к журналам диагностики для Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Приложения логики |Нет схемы. |
| Пакетная служба Azure |Нет схемы. |
| Служба автоматизации Azure |Нет схемы. |

## Поддерживаемые категории журнала для каждого типа ресурса
| Тип ресурса | Категория | Отображаемое имя категории |
| --- | --- | --- |
| Microsoft.Automation/automationAccounts |JobLogs |Журналы заданий |
| Microsoft.Automation/automationAccounts |JobStreams |Потоки заданий |
| Microsoft.Batch/batchAccounts |ServiceLog |Журналы служб |
| Microsoft.DataLakeAnalytics/accounts |Аудит |Журналы аудита |
| Microsoft.DataLakeAnalytics/accounts |Запросы |Журналы запросов |
| Microsoft.DataLakeStore/accounts |Аудит |Журналы аудита |
| Microsoft.DataLakeStore/accounts |Запросы |Журналы запросов |
| Microsoft.KeyVault/vaults |AuditEvent |Журналы аудита |
| Microsoft.Logic/workflows |WorkflowRuntime |События диагностики среды выполнения рабочего процесса |
| Microsoft.Network/networksecuritygroups |NetworkSecurityGroupEvent |Событие группы безопасности сети |
| Microsoft.Network/networksecuritygroups |NetworkSecurityGroupRuleCounter |Счетчик правил группы безопасности сети |
| Microsoft.Network/networksecuritygroups |NetworkSecurityGroupFlowEvent |Событие потока правил группы безопасности сети |
| Microsoft.Network/loadBalancers |LoadBalancerAlertEvent |События оповещения балансировщика нагрузки |
| Microsoft.Network/loadBalancers |LoadBalancerProbeHealthStatus |Состояние работоспособности балансировщика нагрузки |
| Microsoft.Network/applicationGateways |ApplicationGatewayAccessLog |Журнал доступа к шлюзу приложений |
| Microsoft.Network/applicationGateways |ApplicationGatewayPerformanceLog |Журнал производительности шлюза приложений |
| Microsoft.Network/applicationGateways |ApplicationGatewayFirewallLog |Журнал брандмауэра шлюза приложений |
| Microsoft.Search/searchServices |OperationLogs |Журналы операций |
| Microsoft.ServerManagement/nodes |RequestLogs |Журналы запросов |
| Microsoft.StreamAnalytics/streamingjobs |Выполнение |Выполнение |
| Microsoft.StreamAnalytics/streamingjobs |Разработка |Разработка |

## Дальнейшие действия
* [Потоковая передача журналов диагностики в **концентраторы событий**](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Изменение параметров диагностики с помощью REST API Insights](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Анализ журналов с помощью OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md)

<!---HONumber=AcomDC_0928_2016-->