---
title: 'Azure Monitor: поддерживаемые метрики для каждого типа ресурсов | Microsoft Docs'
description: Список метрик, доступных для каждого типа ресурса в Azure Monitor.
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
# Метрики, поддерживаемые Azure Monitor
Azure Monitor обеспечивает несколько способов взаимодействия с метриками, включая создание диаграмм метрик на портале, доступ к метрикам через REST API или запрос метрик с помощью PowerShell или интерфейса командной строки. Ниже приведен полный список метрик, доступных в настоящее время в конвейере метрик Azure Monitor.

> [!NOTE]
> Другие метрики могут быть доступны на портале или при использовании интерфейсов API прежних версий. Этот список содержит только метрики, которые доступны при использовании общедоступной предварительной версии объединенного конвейера метрик Azure Monitor.
> 
> 

## Microsoft.Batch/batchAccounts
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| CoreCount |Core Count |Процент |Средняя |Среднее число ядер в учетной записи пакетной службы. |
| TotalNodeCount |Node Count |Процент |Средняя |Среднее число узлов в учетной записи пакетной службы. |
| CreatingNodeCount |Creating Node Count |Процент |Средняя |Количество создаваемых узлов. |
| StartingNodeCount |Starting Node Count |Процент |Средняя |Число узлов, которые запускаются. |
| WaitingForStartTaskNodeCount |Waiting For Start Task Node Count |Процент |Средняя |Число узлов, ожидающих завершения задачи запуска. |
| StartTaskFailedNodeCount |Start Task Failed Node Count |Процент |Средняя |Число узлов, на которых произошел сбой задачи запуска. |
| IdleNodeCount |Idle Node Count |Процент |Средняя |Количество узлов в неактивном состоянии. |
| OfflineNodeCount |Offline Node Count |Процент |Средняя |Количество узлов не в сети. |
| RebootingNodeCount |Rebooting Node Count |Процент |Средняя |Количество перезапускаемых узлов. |
| ReimagingNodeCount |Reimaging Node Count |Процент |Средняя |Число узлов, для которых пересоздается образ. |
| RunningNodeCount |Running Node Count |Процент |Средняя |Число работающих узлов. |
| LeavingPoolNodeCount |Leaving Pool Node Count |Процент |Средняя |Количество узлов, покидающих пул. |
| UnusableNodeCount |Unusable Node Count |Процент |Средняя |Число узлов, непригодных для использования. |
| TaskStartEvent |Task Start Events |Count |Всего |Общее число запущенных задач. |
| TaskCompleteEvent |Task Complete Events |Count |Всего |Общее число завершенных задач. |
| TaskFailEvent |Task Fail Events |Count |Всего |Общее число задач, завершившихся сбоем. |
| PoolCreateEvent |Pool Create Events |Count |Всего |Общее число созданных пулов. |
| PoolResizeStartEvent |Pool Resize Start Events |Count |Всего |Общее число запущенных задач изменения размера пула. |
| PoolResizeCompleteEvent |Pool Resize Complete Events |Count |Всего |Общее число завершенных задач изменения размера пула. |
| PoolDeleteStartEvent |Pool Delete Start Events |Count |Всего |Общее число запущенных задач удаления пула. |
| PoolDeleteCompleteEvent |Pool Delete Complete Events |Count |Всего |Общее число завершенных задач удаления пула. |

## Microsoft.Cache/redis
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| connectedclients |Подключенные клиенты |Count |Максимальная | |
| totalcommandsprocessed |Всего операций |Count |Всего | |
| cachehits |Попаданий в кэш |Count |Всего | |
| cachemisses |Промахов в кэше |Count |Всего | |
| getcommands |Операций считывания |Count |Всего | |
| setcommands |Операций записи |Count |Всего | |
| evictedkeys |Исключенные ключи |Count |Всего | |
| totalkeys |Всего ключей |Count |Максимальная | |
| expiredkeys |Ключи с истекшим сроком действия |Count |Всего | |
| usedmemory |Используемая память |Байты |Максимальная | |
| usedmemoryRss |RSS используемой памяти |Байты |Максимальная | |
| serverLoad |Загрузка сервера |Процент |Максимальная | |
| cacheWrite |Запись в кэш |Байт/с |Максимальная | |
| cacheRead |Чтение из кэша |Байт/с |Максимальная | |
| percentProcessorTime |ЦП |Процент |Максимальная | |
| connectedclients0 |Connected Clients (Shard 0) |Count |Максимальная | |
| totalcommandsprocessed0 |Total Operations (Shard 0) |Count |Всего | |
| cachehits0 |Cache Hits (Shard 0) |Count |Всего | |
| cachemisses0 |Cache Misses (Shard 0) |Count |Всего | |
| getcommands0 |Gets (Shard 0) |Count |Всего | |
| setcommands0 |Sets (Shard 0) |Count |Всего | |
| evictedkeys0 |Evicted Keys (Shard 0) |Count |Всего | |
| totalkeys0 |Total Keys (Node 0) |Count |Максимальная | |
| expiredkeys0 |Expired Keys (Shard 0) |Count |Всего | |
| usedmemory0 |Used Memory (Shard 0) |Байты |Максимальная | |
| usedmemoryRss0 |Used Memory RSS (Shard 0) |Байты |Максимальная | |
| serverLoad0 |Server Load (Shard 0) |Процент |Максимальная | |
| cacheWrite0 |Cache Write (Shard 0) |Байт/с |Максимальная | |
| cacheRead0 |Cache Read (Shard 0) |Байт/с |Максимальная | |
| percentProcessorTime0 |CPU (Shard 0) |Процент |Максимальная | |
| connectedclients1 |Connected Clients (Shard 1) |Count |Максимальная | |
| totalcommandsprocessed1 |Total Operations (Shard 1) |Count |Всего | |
| cachehits1 |Cache Hits (Shard 1) |Count |Всего | |
| cachemisses1 |Cache Misses (Shard 1) |Count |Всего | |
| getcommands1 |Gets (Shard 1) |Count |Всего | |
| getcommands1 |Sets (Shard 1) |Count |Всего | |
| evictedkeys1 |Evicted Keys (Shard 1) |Count |Всего | |
| totalkeys1 |Total Keys (Node 1) |Count |Максимальная | |
| expiredkeys1 |Expired Keys (Shard 1) |Count |Всего | |
| usedmemory1 |Used Memory (Shard 1) |Байты |Максимальная | |
| usedmemoryRss1 |Used Memory RSS (Shard 1) |Байты |Максимальная | |
| serverLoad1 |Server Load (Shard 1) |Процент |Максимальная | |
| cacheWrite1 |Cache Write (Shard 1) |Байт/с |Максимальная | |
| cacheRead1 |Cache Read (Shard 1) |Байт/с |Максимальная | |
| percentProcessorTime1 |CPU (Shard 1) |Процент |Максимальная | |
| connectedclients2 |Connected Clients (Shard 2) |Count |Максимальная | |
| totalcommandsprocessed2 |Total Operations (Shard 2) |Count |Всего | |
| cachehits2 |Cache Hits (Shard 2) |Count |Всего | |
| cachemisses2 |Cache Misses (Shard 2) |Count |Всего | |
| getcommands2 |Gets (Shard 2) |Count |Всего | |
| getcommands2 |Sets (Shard 2) |Count |Всего | |
| evictedkeys2 |Evicted Keys (Shard 2) |Count |Всего | |
| totalkeys2 |Total Keys (Node 2) |Count |Максимальная | |
| expiredkeys2 |Expired Keys (Shard 2) |Count |Всего | |
| usedmemory2 |Used Memory (Shard 2) |Байты |Максимальная | |
| usedmemoryRss2 |Used Memory RSS (Shard 2) |Байты |Максимальная | |
| serverLoad2 |Server Load (Shard 2) |Процент |Максимальная | |
| cacheWrite2 |Cache Write (Shard 2) |Байт/с |Максимальная | |
| cacheRead2 |Cache Read (Shard 2) |Байт/с |Максимальная | |
| percentProcessorTime2 |CPU (Shard 2) |Процент |Максимальная | |
| connectedclients3 |Connected Clients (Shard 3) |Count |Максимальная | |
| totalcommandsprocessed3 |Total Operations (Shard 3) |Count |Всего | |
| cachehits3 |Cache Hits (Shard 3) |Count |Всего | |
| cachemisses3 |Cache Misses (Shard 3) |Count |Всего | |
| getcommands3 |Gets (Shard 3) |Count |Всего | |
| setcommands3 |Sets (Shard 3) |Count |Всего | |
| evictedkeys3 |Evicted Keys (Shard 3) |Count |Всего | |
| totalkeys3 |Total Keys (Node 3) |Count |Максимальная | |
| expiredkeys3 |Expired Keys (Shard 3) |Count |Всего | |
| usedmemory3 |Used Memory (Shard 3) |Байты |Максимальная | |
| usedmemoryRss3 |Used Memory RSS (Shard 3) |Байты |Максимальная | |
| serverLoad3 |Server Load (Shard 3) |Процент |Максимальная | |
| cacheWrite3 |Cache Write (Shard 3) |Байт/с |Максимальная | |
| cacheRead3 |Cache Read (Shard 3) |Байт/с |Максимальная | |
| percentProcessorTime3 |CPU (Shard 3) |Процент |Максимальная | |
| connectedclients4 |Connected Clients (Shard 4) |Count |Максимальная | |
| totalcommandsprocessed4 |Total Operations (Shard 4) |Count |Всего | |
| cachehits4 |Cache Hits (Shard 4) |Count |Всего | |
| cachemisses4 |Cache Misses (Shard 4) |Count |Всего | |
| getcommands4 |Gets (Shard 4) |Count |Всего | |
| setcommands4 |Sets (Shard 4) |Count |Всего | |
| evictedkeys4 |Evicted Keys (Shard 4) |Count |Всего | |
| totalkeys4 |Total Keys (Node 4) |Count |Максимальная | |
| expiredkeys4 |Expired Keys (Shard 4) |Count |Всего | |
| usedmemory4 |Used Memory (Shard 4) |Байты |Максимальная | |
| usedmemoryRss4 |Used Memory RSS (Shard 4) |Байты |Максимальная | |
| serverLoad4 |Server Load (Shard 4) |Процент |Максимальная | |
| cacheWrite4 |Cache Write (Shard 4) |Байт/с |Максимальная | |
| cacheRead4 |Cache Read (Shard 4) |Байт/с |Максимальная | |
| percentProcessorTime4 |CPU (Shard 4) |Процент |Максимальная | |
| connectedclients5 |Connected Clients (Shard 5) |Count |Максимальная | |
| totalcommandsprocessed5 |Total Operations (Shard 5) |Count |Всего | |
| cachehits5 |Cache Hits (Shard 5) |Count |Всего | |
| cachemisses5 |Cache Misses (Shard 5) |Count |Всего | |
| getcommands5 |Gets (Shard 5) |Count |Всего | |
| getcommands5 |Sets (Shard 5) |Count |Всего | |
| evictedkeys5 |Evicted Keys (Shard 5) |Count |Всего | |
| totalkeys5 |Total Keys (Node 5) |Count |Максимальная | |
| expiredkeys5 |Expired Keys (Shard 5) |Count |Всего | |
| usedmemory5 |Used Memory (Shard 5) |Байты |Максимальная | |
| usedmemoryRss5 |Used Memory RSS (Shard 5) |Байты |Максимальная | |
| serverLoad5 |Server Load (Shard 5) |Процент |Максимальная | |
| cacheWrite5 |Cache Write (Shard 5) |Байт/с |Максимальная | |
| cacheRead5 |Cache Read (Shard 5) |Байт/с |Максимальная | |
| percentProcessorTime5 |CPU (Shard 5) |Процент |Максимальная | |
| connectedclients6 |Connected Clients (Shard 6) |Count |Максимальная | |
| totalcommandsprocessed6 |Total Operations (Shard 6) |Count |Всего | |
| cachehits6 |Cache Hits (Shard 6) |Count |Всего | |
| cachemisses6 |Cache Misses (Shard 6) |Count |Всего | |
| getcommands6 |Gets (Shard 6) |Count |Всего | |
| setcommands6 |Sets (Shard 6) |Count |Всего | |
| evictedkeys6 |Evicted Keys (Shard 6) |Count |Всего | |
| totalkeys6 |Total Keys (Node 6) |Count |Максимальная | |
| expiredkeys6 |Expired Keys (Shard 6) |Count |Всего | |
| usedmemory6 |Used Memory (Shard 6) |Байты |Максимальная | |
| usedmemoryRss6 |Used Memory RSS (Shard 6) |Байты |Максимальная | |
| serverLoad6 |Server Load (Shard 6) |Процент |Максимальная | |
| cacheWrite6 |Cache Write (Shard 6) |Байт/с |Максимальная | |
| cacheRead6 |Cache Read (Shard 6) |Байт/с |Максимальная | |
| percentProcessorTime6 |CPU (Shard 6) |Процент |Максимальная | |
| connectedclients7 |Connected Clients (Shard 7) |Count |Максимальная | |
| totalcommandsprocessed7 |Total Operations (Shard 7) |Count |Всего | |
| cachehits7 |Cache Hits (Shard 7) |Count |Всего | |
| cachemisses7 |Cache Misses (Shard 7) |Count |Всего | |
| getcommands7 |Gets (Shard 7) |Count |Всего | |
| setcommands7 |Sets (Shard 7) |Count |Всего | |
| evictedkeys7 |Evicted Keys (Shard 7) |Count |Всего | |
| totalkeys7 |Total Keys (Node 7) |Count |Максимальная | |
| expiredkeys7 |Expired Keys (Shard 7) |Count |Всего | |
| usedmemory7 |Used Memory (Shard 7) |Байты |Максимальная | |
| usedmemoryRss7 |Used Memory RSS (Shard 7) |Байты |Максимальная | |
| serverLoad7 |Server Load (Shard 7) |Процент |Максимальная | |
| cacheWrite7 |Cache Write (Shard 7) |Байт/с |Максимальная | |
| cacheRead7 |Cache Read (Shard 7) |Байт/с |Максимальная | |
| percentProcessorTime7 |CPU (Shard 7) |Процент |Максимальная | |
| connectedclients8 |Connected Clients (Shard 8) |Count |Максимальная | |
| totalcommandsprocessed8 |Total Operations (Shard 8) |Count |Всего | |
| cachehits8 |Cache Hits (Shard 8) |Count |Всего | |
| cachemisses8 |Cache Misses (Shard 8) |Count |Всего | |
| getcommands8 |Gets (Shard 8) |Count |Всего | |
| setcommands8 |Sets (Shard 8) |Count |Всего | |
| evictedkeys8 |Evicted Keys (Shard 8) |Count |Всего | |
| totalkeys8 |Total Keys (Node 8) |Count |Максимальная | |
| expiredkeys8 |Expired Keys (Shard 8) |Count |Всего | |
| usedmemory8 |Used Memory (Shard 8) |Байты |Максимальная | |
| usedmemoryRss8 |Used Memory RSS (Shard 8) |Байты |Максимальная | |
| serverLoad8 |Server Load (Shard 8) |Процент |Максимальная | |
| cacheWrite8 |Cache Write (Shard 8) |Байт/с |Максимальная | |
| cacheRead8 |Cache Read (Shard 8) |Байт/с |Максимальная | |
| percentProcessorTime8 |CPU (Shard 8) |Процент |Максимальная | |
| connectedclients9 |Connected Clients (Shard 9) |Count |Максимальная | |
| totalcommandsprocessed9 |Total Operations (Shard 9) |Count |Всего | |
| cachehits9 |Cache Hits (Shard 9) |Count |Всего | |
| cachemisses9 |Cache Misses (Shard 9) |Count |Всего | |
| getcommands9 |Gets (Shard 9) |Count |Всего | |
| setcommands9 |Sets (Shard 9) |Count |Всего | |
| evictedkeys9 |Evicted Keys (Shard 9) |Count |Всего | |
| totalkeys9 |Total Keys (Node 9) |Count |Максимальная | |
| expiredkeys9 |Expired Keys (Shard 9) |Count |Всего | |
| usedmemory9 |Used Memory (Shard 9) |Байты |Максимальная | |
| usedmemoryRss9 |Used Memory RSS (Shard 9) |Байты |Максимальная | |
| serverLoad9 |Server Load (Shard 9) |Процент |Максимальная | |
| cacheWrite9 |Cache Write (Shard 9) |Байт/с |Максимальная | |
| cacheRead9 |Cache Read (Shard 9) |Байт/с |Максимальная | |
| percentProcessorTime9 |CPU (Shard 9) |Процент |Максимальная | |

## Microsoft.Compute/virtualMachines
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Percentage CPU |Percentage CPU |Процент |Средняя |Процент выделенных вычислительных единиц, которые в настоящее время используются виртуальными машинами. |
| Сеть (входящий трафик) |Сеть (входящий трафик) |Байты |Всего |Число байтов, полученных через все сетевые интерфейсы виртуальных машин (входящий трафик). |
| Сеть (исходящий трафик) |Сеть (исходящий трафик) |Байты |Всего |Число байтов, переданных из всех сетевых интерфейсов виртуальных машин (исходящий трафик). |
| Скорость чтения с диска |Скорость чтения с диска |Байты |Всего |Общее число байтов, считанных с диска за период мониторинга. |
| Скорость записи на диск |Скорость записи на диск |Байты |Всего |Общее число байтов, записанных на диск за период мониторинга. |
| Disk Read Operations/Sec |Disk Read Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при чтении с диска. |
| Disk Write Operations/Sec |Disk Write Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при записи на диск. |

## Microsoft.Compute/virtualMachineScaleSets;
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Percentage CPU |Percentage CPU |Процент |Средняя |Процент выделенных вычислительных единиц, которые в настоящее время используются виртуальными машинами. |
| Сеть (входящий трафик) |Сеть (входящий трафик) |Байты |Всего |Число байтов, полученных через все сетевые интерфейсы виртуальных машин (входящий трафик). |
| Сеть (исходящий трафик) |Сеть (исходящий трафик) |Байты |Всего |Число байтов, переданных из всех сетевых интерфейсов виртуальных машин (исходящий трафик). |
| Скорость чтения с диска |Скорость чтения с диска |Байты |Всего |Общее число байтов, считанных с диска за период мониторинга. |
| Скорость записи на диск |Скорость записи на диск |Байты |Всего |Общее число байтов, записанных на диск за период мониторинга. |
| Disk Read Operations/Sec |Disk Read Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при чтении с диска. |
| Disk Write Operations/Sec |Disk Write Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при записи на диск. |

## Microsoft.Compute/virtualMachineScaleSets/virtualMachines
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Percentage CPU |Percentage CPU |Процент |Средняя |Процент выделенных вычислительных единиц, которые в настоящее время используются виртуальными машинами. |
| Сеть (входящий трафик) |Сеть (входящий трафик) |Байты |Всего |Число байтов, полученных через все сетевые интерфейсы виртуальных машин (входящий трафик). |
| Сеть (исходящий трафик) |Сеть (исходящий трафик) |Байты |Всего |Число байтов, переданных из всех сетевых интерфейсов виртуальных машин (исходящий трафик). |
| Скорость чтения с диска |Скорость чтения с диска |Байты |Всего |Общее число байтов, считанных с диска за период мониторинга. |
| Скорость записи на диск |Скорость записи на диск |Байты |Всего |Общее число байтов, записанных на диск за период мониторинга. |
| Disk Read Operations/Sec |Disk Read Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при чтении с диска. |
| Disk Write Operations/Sec |Disk Write Operations/Sec |Число/с |Средняя |Количество операций вода-вывода в секунду при записи на диск. |

## Microsoft.Devices/IotHubs
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| d2c.telemetry.ingress.allProtocol |Telemetry Message Send Attempts |Count |Всего |Число предпринятых попыток отправки в Центр Интернета вещей сообщений телеметрии из устройства в облако. |
| d2c.telemetry.ingress.success |Telemetry Messages Sent |Count |Всего |Число успешно отправленных в Центр Интернета вещей сообщений телеметрии из устройства в облако. |
| c2d.commands.egress.complete.success |Commands Completed |Count |Всего |Число команд из облака на устройство, успешно выполненных устройством. |
| c2d.commands.egress.abandon.success |Commands Abandoned |Count |Всего |Число команд из облака на устройство, прерванных устройством. |
| c2d.commands.egress.reject.success |Commands Rejected |Count |Всего |Число команд из облака на устройство, отклоненных устройством. |
| devices.totalDevices |Total Devices |Count |Всего |Число устройств, зарегистрированных в Центре Интернета вещей. |
| devices.connectedDevices.allProtocol |Подключенные устройства |Count |Всего |Число устройств, подключенных к Центру Интернета вещей. |

## Microsoft.EventHub/namespaces
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| INREQS |Входящих запросов |Count |Всего | |
| SUCCREQ |Выполненные запросы |Count |Всего | |
| FAILREQ |Невыполненные запросы |Count |Всего | |
| SVRBSY |Ошибки из-за занятости сервера |Count |Всего | |
| INTERR |Внутренние ошибки сервера |Count |Всего | |
| MISCERR |Другие ошибки |Count |Всего | |
| INMSGS |Входящие сообщения |Count |Всего | |
| OUTMSGS |Исходящие сообщения |Count |Всего | |
| EHINMBS |Входящих байт в секунду |Байт/с |Всего | |
| EHOUTMBS |Исходящих байт в секунду |Байт/с |Всего | |
| EHABL |Archive backlog messages |Count |Всего | |
| EHAMSGS |Archive messages |Count |Всего | |
| EHAMBS |Archive message throughput |Байт/с |Всего | |

## Microsoft.Logic/workflows
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| RunsStarted |Runs Started |Count |Всего |Число запущенных выполнений рабочего процесса. |
| RunsCompleted |Runs Completed |Count |Всего |Число завершенных выполнений рабочего процесса. |
| RunsSucceeded |Runs Succeeded |Count |Всего |Число успешно завершенных выполнений рабочего процесса. |
| RunsFailed |Runs Failed |Count |Всего |Число выполнений рабочего процесса, завершившихся сбоем. |
| RunsCancelled |Runs Cancelled |Count |Всего |Число отмененных выполнений рабочего процесса. |
| RunLatency |Run Latency |Секунды |Средняя |Задержка завершенных выполнений рабочего процесса. |
| RunSuccessLatency |Run Success Latency |Секунды |Средняя |Задержка успешно завершенных выполнений рабочего процесса. |
| RunThrottledEvents |Run Throttled Events |Count |Всего |Число событий регулирования действия или триггера рабочего процесса. |
| ActionsStarted |Actions Started |Count |Всего |Число запущенных действий рабочего процесса. |
| ActionsCompleted |Actions Completed |Count |Всего |Число завершенных действий рабочего процесса. |
| ActionsSucceeded |Actions Succeeded |Count |Всего |Число успешно выполненных действий рабочего процесса. |
| ActionsFailed |Actions Failed |Count |Всего |Число действий рабочего процесса, завершившихся сбоем. |
| ActionsSkipped |Actions Skipped |Count |Всего |Число пропущенных действий рабочего процесса. |
| ActionLatency |Action Latency |Секунды |Средняя |Задержка завершенных действий рабочего процесса. |
| ActionSuccessLatency |Action Success Latency |Секунды |Средняя |Задержка успешно выполненных действий рабочего процесса. |
| ActionThrottledEvents |Action Throttled Events |Count |Всего |Число событий регулирования действия рабочего процесса. |
| TriggersStarted |Triggers Started |Count |Всего |Число запущенных триггеров рабочего процесса. |
| TriggersCompleted |Triggers Completed |Count |Всего |Число завершенных триггеров рабочего процесса. |
| TriggersSucceeded |Triggers Succeeded |Count |Всего |Число успешно выполненных триггеров рабочего процесса. |
| TriggersFailed |Triggers Failed |Count |Всего |Число триггеров рабочего процесса, завершившихся со сбоем. |
| TriggersSkipped |Triggers Skipped |Count |Всего |Число пропущенных триггеров рабочего процесса. |
| TriggersFired |Triggers Fired |Count |Всего |Число активированных триггеров рабочего процесса. |
| TriggerLatency |Trigger Latency |Секунды |Средняя |Задержка завершенных триггеров рабочего процесса. |
| TriggerFireLatency |Trigger Fire Latency |Секунды |Средняя |Задержка активированных триггеров рабочего процесса. |
| TriggerSuccessLatency |Trigger Success Latency |Секунды |Средняя |Задержка успешно выполненных триггеров рабочего процесса. |
| TriggerThrottledEvents |Trigger Throttled Events |Count |Всего |Число событий регулирования триггера рабочего процесса. |

## Microsoft.Network/applicationGateways
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Пропускная способность |Пропускная способность |Байт/с |Средняя | |

## Microsoft.Search/searchServices
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Задержка |Задержка |Секунды |Средняя |Среднее время задержки службы поиска. |
| SearchLatency |Search Latency |Секунды |Средняя |Среднее время задержки поиска для службы поиска. |
| SearchQueriesPerSecond |Search queries per second |Число/с |Средняя |Число поисковых запросов в секунду для службы поиска. |
| ThrottledSearchQueriesPercentage |Throttled search queries percentage |Процент |Средняя |Процент отрегулированных поисковых запросов для службы поиска. |

## Microsoft.ServiceBus/namespaces
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| CPUXNS |CPU usage per namespace |Процент |Максимальная | |
| WSXNS |Memory size usage per namespace |Процент |Максимальная | |

## Microsoft.Sql/servers/databases
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| cpu\_percent |Процент использования ЦП |Процент |Максимальная |Процент использования ЦП |
| physical\_data\_read\_percent |Процент операций ввода/вывода данных |Процент |Максимальная |Процент операций ввода/вывода данных |
| log\_write\_percent |Log IO percentage |Процент |Максимальная |Процент использования пропускной способности ввода-вывода журнала. |
| dtu\_consumption\_percent |Процент использования DTU |Процент |Максимальная |Процент использования DTU |
| storage |Total database size |Байты |Максимальная |Total database size |
| connection\_successful |Успешные подключения |Count |Всего |Успешные подключения |
| connection\_failed |Неудачные подключения |Count |Всего |Неудачные подключения |
| blocked\_by\_firewall |Заблокировано брандмауэром |Count |Всего |Заблокировано брандмауэром |
| deadlock |Взаимоблокировки |Count |Всего |Взаимоблокировки. |
| storage\_percent |Размер базы данных в процентах |Процент |Максимальная |Размер базы данных в процентах |
| xtp\_storage\_percent |In-Memory OLTP storage percent (Preview) |Процент |Максимальная |Процент использования хранилища выполняющейся в памяти OLTP (предварительная версия). |
| workers\_percent |Workers percentage |Процент |Максимальная |Процент использования рабочих ролей. |
| sessions\_percent |Sessions percent |Процент |Максимальная |Процент использования сеансов. |
| dtu\_limit |DTU limit |Count |Максимальная |Лимит DTU. |
| dtu\_used |DTU used |Count |Максимальная |Использование DTU. |
| service\_level\_objective |Service level objective of the database |Count |Всего |Целевой уровень служб базы данных. |
| dwu\_limit |DWU limit |Count |Максимальная |Лимит DWU. |
| dwu\_consumption\_percent |DWU percentage |Процент |Максимальная |Процент использования DWU. |
| dwu\_used |DWU used |Count |Максимальная |Использование DWU. |

## Microsoft.Sql/servers/elasticPools
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| cpu\_percent |Процент использования ЦП |Процент |Максимальная |Процент использования ЦП |
| physical\_data\_read\_percent |Процент операций ввода/вывода данных |Процент |Максимальная |Процент операций ввода/вывода данных |
| log\_write\_percent |Log IO percentage |Процент |Максимальная |Процент использования пропускной способности ввода-вывода журнала. |
| dtu\_consumption\_percent |Процент использования DTU |Процент |Максимальная |Процент использования DTU |
| storage\_percent |Storage percentage |Процент |Максимальная |Процент использования хранилища. |
| workers\_percent |Workers percent |Процент |Максимальная |Процент использования рабочих ролей. |
| sessions\_percent |Sessions percent |Процент |Максимальная |Процент использования сеансов. |
| eDTU\_limit |eDTU limit |Count |Максимальная |Лимит eDTU. |
| storage\_limit |Storage limit |Байты |Максимальная |Размер хранилища. |
| eDTU\_used |eDTU used |Count |Максимальная |Использование eDTU. |
| storage\_used |Storage used |Байты |Максимальная |Используемое хранилище. |

## Microsoft.StreamAnalytics/streamingjobs
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| ResourceUtilization |Использование единиц потоковой передачи (%) |Процент |Максимальная |Использование единиц потоковой передачи (%) |
| InputEvents |Входные события |Байты |Всего |Входные события |
| InputEventBytes |Байты входного события |Байты |Всего |Байты входного события |
| LateInputEvents |События позднего ввода |Байты |Всего |События позднего ввода |
| OutputEvents |Выходные события |Байты |Всего |Выходные события |
| ConversionErrors |Ошибки преобразования данных |Байты |Всего |Ошибки преобразования данных |
| Ошибки |Ошибки среды выполнения |Байты |Всего |Ошибки среды выполнения |
| DroppedOrAdjustedEvents |Неупорядоченные события |Байты |Всего |Неупорядоченные события. |
| AMLCalloutRequests |Запросы функций |Байты |Всего |Запросы функций. |
| AMLCalloutFailedRequests |Неудачные запросы функций |Байты |Всего |Неудачные запросы функций. |
| AMLCalloutInputEvents |События функций |Байты |Всего |События функций. |

## Microsoft.Web/serverfarms
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| Процент загруженности ЦП |Процент использования ЦП |Процент |Средняя |Процент использования ЦП |
| Процент использования памяти |Процент использования памяти |Процент |Средняя |Процент использования памяти |
| Длина очереди диска |Длина дисковой очереди |Count |Всего |Длина дисковой очереди |
| Длина очереди HTTP |Длина очереди HTTP: |Count |Всего |Длина очереди HTTP: |
| Получено байт |Входящие данные: |Байты |Всего |Входящие данные: |
| Передано байт |Выходные данные |Байты |Всего |Выходные данные |

## Microsoft.Web/sites
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| CpuTime |Время ЦП: |Секунды |Всего |Время ЦП: |
| Запросы |Запросы |Count |Всего |Запросы |
| Получено байт |Входящие данные: |Байты |Всего |Входящие данные: |
| Передано байт |Выходные данные |Байты |Всего |Выходные данные |
| Http2xx |HTTP 2xx |Count |Всего |HTTP 2xx |
| Http3xx |HTTP 3xx: |Count |Всего |HTTP 3xx: |
| Http401 |HTTP 401: |Count |Всего |HTTP 401: |
| Http403 |HTTP 403: |Count |Всего |HTTP 403: |
| Http404 |HTTP 404: |Count |Всего |HTTP 404: |
| Http406 |HTTP 406: |Count |Всего |HTTP 406: |
| Http4xx |HTTP 4xx: |Count |Всего |HTTP 4xx: |
| Http5xx |Ошибки HTTP-сервера: |Count |Всего |Ошибки HTTP-сервера: |
| MemoryWorkingSet |рабочий набор памяти; |Байты |Всего |рабочий набор памяти; |
| AverageMemoryWorkingSet |средний размер рабочего набора памяти; |Байты |Средняя |средний размер рабочего набора памяти; |
| AverageResponseTime |Среднее время ответа: |Секунды |Средняя |Среднее время ответа: |

## Microsoft.Web/sites/slots
| Метрика | Отображаемое имя метрики | Единица измерения | Тип статистической обработки | Описание |
| --- | --- | --- | --- | --- |
| CpuTime |Время ЦП: |Секунды |Всего |Время ЦП: |
| Запросы |Запросы |Count |Всего |Запросы |
| Получено байт |Входящие данные: |Байты |Всего |Входящие данные: |
| Передано байт |Выходные данные |Байты |Всего |Выходные данные |
| Http2xx |HTTP 2xx |Count |Всего |HTTP 2xx |
| Http3xx |HTTP 3xx: |Count |Всего |HTTP 3xx: |
| Http401 |HTTP 401: |Count |Всего |HTTP 401: |
| Http403 |HTTP 403: |Count |Всего |HTTP 403: |
| Http404 |HTTP 404: |Count |Всего |HTTP 404: |
| Http406 |HTTP 406: |Count |Всего |HTTP 406: |
| Http4xx |HTTP 4xx: |Count |Всего |HTTP 4xx: |
| Http5xx |Ошибки HTTP-сервера: |Count |Всего |Ошибки HTTP-сервера: |
| MemoryWorkingSet |рабочий набор памяти; |Байты |Всего |рабочий набор памяти; |
| AverageMemoryWorkingSet |средний размер рабочего набора памяти; |Байты |Средняя |средний размер рабочего набора памяти; |
| AverageResponseTime |Среднее время ответа: |Секунды |Средняя |Среднее время ответа: |

## Дальнейшие действия
* [Прочитайте о метриках в Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md#monitoring-sources).
* [Создайте оповещения на основе метрик](insights-receive-alert-notifications.md).
* [Экспортируйте метрики в хранилище, концентратор событий или Log Analytics](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

<!---HONumber=AcomDC_0928_2016-->