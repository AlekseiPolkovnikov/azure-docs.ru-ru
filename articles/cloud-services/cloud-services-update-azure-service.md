---
title: Обновление облачной службы | Microsoft Docs
description: Узнайте, как обновлять облачные службы в Azure. Узнайте, как обеспечивается доступность облачной службы при ее обновлении.
services: cloud-services
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''

ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: adegeo

---
# Обновление облачной службы
## Обзор
Говоря упрощенно, обновление облачной службы, включая ее роли или гостевую ОС, состоит их трех этапов. Сначала нужно передать двоичные файлы и файлы конфигурации новой облачной службы или новой версии ОС. Затем Azure резервирует вычислительные и сетевые ресурсы для облачной службы в соответствии с требованиями новой версии облачной службы. И, наконец, Azure выполняет процесс последовательного обновления клиента до новой версии службы или гостевой ОС, сохраняя при этом доступность. В этой статье подробно описан последний шаг этого процесса — последовательное обновление.

## Обновление службы Azure
Azure распределяет экземпляры роли по логическим группам, которые называются доменами обновления. Домены обновления — это логические наборы экземпляров ролей, которые обновляются одновременно. Azure поочередно обновляет по одному домену обновления облачной службы, позволяя экземплярам в других доменах обновления продолжать обработку запросов.

Количество доменов обновления по умолчанию равно 5. Вы можете указать любое другое количество доменов обновления, включив в файл определения службы (CSDEF) атрибут upgradeDomainCount. Дополнительные сведения об атрибуте upgradeDomainCount см. в статьях [Схема WebRole](https://msdn.microsoft.com/library/azure/gg557553.aspx) и [Схема WorkerRole](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Когда вы выполняете обновление на месте, Azure обновляет наборы экземпляров ролей поочередно в соответствии с их распределением в доменах обновления. Azure одновременно обновляет все экземпляры, входящие в домен обновления: останавливает их, выполняет изменения и снова возвращает в работу. Затем Azure переходит к следующему домену обновления. Поскольку работу прекращают только те экземпляры, которые находятся в текущем домене обновления, Azure обновляет службу без существенного влияния на ее работу. Подробнее этот процесс описан в разделе [Как происходит обновление](#howanupgradeproceeds).

> [!NOTE]
> Служба Azure выполняет два типа обновления: **обновление (изменение) конфигурации** и **обновление версий программ**. Описываемые в этой статье процессы и функции работают одинаково в обоих случаях, поэтому мы будем собирательно называть их обновлением.
> 
> 

Чтобы обновление роли на месте проходило без остановки ее работы, для этой роли следует определить не менее двух экземпляров. Служба, содержащая только один экземпляр одной роли, будет недоступна во время обновления на месте.

В этом разделе мы рассмотрим следующие сведения об обновлениях Azure.

* [Допустимые изменения службы во время обновления](#AllowedChanges)
* [Как происходит обновление](#howanupgradeproceeds)
* [Откат обновления](#RollbackofanUpdate)
* [Запуск нескольких операций изменения в текущем развертывании](#multiplemutatingoperations)
* [Распределение ролей в доменах обновления](#distributiondfroles)

<a name="AllowedChanges"></a>

## Допустимые изменения службы во время обновления
Все допустимые изменения службы во время обновления приведены в следующей таблице.

| Изменения, которые можно выполнять для хостинга, служб и ролей | Обновление "на месте" | Промежуточная среда (переключение виртуального IP-адреса) | Удаление и повторное развертывание |
| --- | --- | --- | --- |
| Версия операционной системы |Да |Да |Да |
| Уровень доверия .NET |Да |Да |Да |
| Размер виртуальной машины <sup>1</sup> |Да <sup>2</sup> |Да |Да |
| Параметры хранилища Azure |Только увеличение <sup>2</sup> |Да |Да |
| Добавление и удаление ролей в службе |Да |Да |Да |
| Количество экземпляров определенной роли |Да |Да |Да |
| Количество и тип конечных точек службы |Да <sup>2</sup> |Нет |Да |
| Имена и значения параметров конфигурации |Да |Да |Да |
| Только значения параметров конфигурации |Да |Да |Да |
| Добавление сертификатов |Да |Да |Да |
| Изменение существующих сертификатов |Да |Да |Да |
| Развертывание нового кода |Да |Да |Да |

<sup>1</sup> Размер можно изменять в пределах подмножества размеров, доступных для облачной службы.

<sup>2</sup> Требуется пакет SDK Azure 1.5 или более поздней версии.

> [!WARNING]
> Изменение размера виртуальной машины приведет к уничтожению локальных данных.
> 
> 

Следующие действия не могут выполняться во время обновления:

* Изменение имени роли. Роль следует удалить, а затем добавить с новым именем.
* Изменение количества доменов обновления.
* Уменьшение размера локальных ресурсов.

Чтобы внести другие изменения в определение службы, например уменьшить размер локального ресурса, вы можете выполнить переключение виртуального IP-адреса. Дополнительные сведения см. в статье [Переключение развертывания](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## Как происходит обновление
Вы можете выбрать обновление всех ролей службы или только одной. В любом случае сначала Azure остановит, обновит и снова запустит все экземпляры всех обновляемых ролей, входящих в первый домен обновления. Когда все роли возобновят работу, экземпляры во втором домене обновления будут остановлены, обновлены и запущены. Для каждой облачной службы в один момент времени выполняется только один процесс обновления. Обновление всегда выполняется до самой последней версии облачной службы.

На следующей схеме показан процесс обновления всех ролей в службе.

![Обновление службы](media/cloud-services-update-azure-service/IC345879.png "Обновление службы")

На следующей диаграмме показан процесс обновления одной роли службы.

![Обновление роли](media/cloud-services-update-azure-service/IC345880.png "Обновление роли")

> [!NOTE]
> Если при обновлении службы количество ее экземпляров увеличивается от одного до нескольких, Azure остановит такую службу на время обновления. Это обусловлено особенностями процесса обновления. Соглашение об уровне обслуживания, гарантирующее доступность службы, применяется только для служб с несколькими экземплярами. Следующий список описывает, что происходит с данными на дисках при различных сценариях обновления службы Azure.
> 
> Перезагрузка виртуальной машины:
> 
> * C: сохраняются;
> * D: сохраняются;
> * E: сохраняются.
> 
> Перезагрузка портала:
> 
> * C: сохраняются;
> * D: сохраняются;
> * E: уничтожаются.
> 
> Пересоздание образа портала:
> 
> * C: сохраняются;
> * D: уничтожаются;
> * E: уничтожаются.
> 
> Обновление на месте:
> 
> * C: сохраняются;
> * D: сохраняются;
> * E: уничтожаются.
> 
> Миграция узла:
> 
> * C: уничтожаются;
> * D: уничтожаются;
> * E: уничтожаются.
> 
> Обратите внимание, что в приведенном выше списке диск E: обозначает корневой диск роли. Это значение не должно быть жестко запрограммированным. Для обращения к этому диску используйте переменную среды %RoleRoot%.
> 
> Чтобы сократить время простоя при обновлении службы с одним экземпляром, вы можете развернуть на промежуточном сервере новую службу с несколькими экземплярами и выполнить переключение виртуального IP-адреса.
> 
> 

В ходе автоматического обновления контроллер структуры Azure периодически проверяет работоспособность облачной службы, чтобы определить безопасный момент для перехода к следующему домену обновления. Проверка работоспособности выполняется для каждой роли отдельно с учетом экземпляров только самой последней версии (т. е. экземпляров из тех доменов обновления, для которых процесс уже начат). Чтобы служба считалась работоспособной, удовлетворительного конечного состояния должно достичь минимальное заданное количество экземпляров каждой роли.

### Время ожидания запуска экземпляра роли
Контроллер структуры будет 30 минут ожидать перехода каждого экземпляра роли в запущенное состояние. По истечении времени ожидания контроллер структуры переходит к следующему экземпляру роли.

<a name="RollbackofanUpdate"></a>

## Откат обновления
Во время обновления Azure обеспечивает определенную гибкость в управлении службами. Система принимает запросы на дополнительные операции для службы даже после того, как контроллер структуры Azure получит первичный запрос на обновление. Откат может быть выполнен только тогда, когда обновление версии или конфигурации развертывания находится в состоянии **выполнения**. Обновление версии или конфигурации считается выполняющимся, если существует хотя бы один экземпляр службы, который еще не обновлен до новой версии. Чтобы проверить возможность отката, убедитесь, что тег RollbackAllowed имеет значение True (значение этого тега возвращают операции [получить развертывание](https://msdn.microsoft.com/library/azure/ee460804.aspx) и [получить свойства облачной службы](https://msdn.microsoft.com/library/azure/ee460806.aspx)).

> [!NOTE]
> Откат имеет смысл только в случае обновления **на месте**, так как обновление путем переключения виртуального IP-адреса подразумевает замену одного работающего экземпляра службы другим.
> 
> 

Откат выполняющегося обновления влияет на развертывание следующим образом.

* Все экземпляры ролей, которые еще не были обновлены, останутся в прежнем состоянии, поскольку на них выполняется нужная версия службы.
* На все экземпляры ролей с установленными новыми версиями файла пакета (CSPKG) и/или файла конфигурации (CSCFG) будут возвращены старые версии этих файлов.

Это обеспечивается следующими функциями.

* Операция [Откатить обновление](https://msdn.microsoft.com/library/azure/hh403977.aspx). Ее можно вызвать во время обновления конфигурации (запускается операцией [Изменить конфигурацию развертывания](https://msdn.microsoft.com/library/azure/ee460809.aspx)) или версии (запускается операцией [Обновить развертывание](https://msdn.microsoft.com/library/azure/ee460793.aspx)) при условии, что хотя бы один экземпляр службы еще не был обновлен до новой версии.
* Элементы Locked и RollbackAllowed, которые возвращаются в теле ответа операциями [Получить развертывание](https://msdn.microsoft.com/library/azure/ee460804.aspx) и [Получить свойства облачной службы](https://msdn.microsoft.com/library/azure/ee460806.aspx).
  
  1. Элемент Locked (Заблокировано) позволяет определить, можно ли выполнять операции изменения в этом развертывании.
  2. Элемент RollbackAllowed (Откат разрешен) позволяет определить, можно ли выполнять операцию [Откатить обновление](https://msdn.microsoft.com/library/azure/hh403977.aspx) в этом развертывании.
  
  Чтобы выполнить откат, не обязательно проверять оба элемента (Locked и RollbackAllowed). Достаточно удостовериться, что RollbackAllowed имеет значение True. Эти элементы возвращаются только в том случае, если в запросе при вызове метода указан заголовок x-ms-version: 2011-10-01 или с более поздней версией. Подробнее о заголовках управления версиями см. в статье [Управление версиями службы управления](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Существуют ситуации, когда откат обновления невозможен. Примеры таких ситуаций представлены ниже.

* Сокращение размера локальных ресурсов — если обновление увеличивает локальные ресурсы для роли, платформа Azure запрещает выполнять откат.
* Ограничения квоты — если обновление предусматривает уменьшение масштаба, вы можете выйти за рамки квоты на вычислительные ресурсы для завершения отката. Для каждой подписки Azure установлена определенная квота на количество ядер, которые могут использовать все службы в рамках этой подписки. Если для выполнения отката обновления требуется превысить квоту подписки, такой откат будет запрещен.
* Состояние гонки — если обновление уже завершено, откат невозможен.

Давайте рассмотрим ситуацию, когда может быть полезен откат обновления. Вы запускаете операцию [Обновить развертывание](https://msdn.microsoft.com/library/azure/ee460793.aspx) в ручном режиме, чтобы управлять скоростью крупномасштабного обновления размещенной в Azure службы, которое выполняется на месте.

Во время выполнения автоматического обновления вы запускаете операцию [Обновить развертывание](https://msdn.microsoft.com/library/azure/ee460793.aspx) в ручном режиме и начинаете отслеживать домены обновления. В определенный момент, контролируя ход обновления, вы замечаете, что некоторые экземпляры роли в первых доменах обновления перестали отвечать на запросы. В этот момент вы можете вызвать операцию [Откатить обновление](https://msdn.microsoft.com/library/azure/hh403977.aspx), которая оставит без изменений необновленные экземпляры, а обновленные вернет к прежнему состоянию пакета и конфигурации службы.

<a name="multiplemutatingoperations"></a>

## Запуск нескольких операций изменения в текущем развертывании
В некоторых случаях будет удобно одновременно запустить несколько операций изменения в рамках текущего развертывания. Например, во время обновления службы вам может понадобиться внести какие-то изменения, например откатить это обновление, применить другое обновление или даже удалить развертывание. Эта необходимость может возникнуть, если обновление службы содержит ошибку в коде, из-за которой обновленные экземпляры роли постоянно завершаются сбоем. В этом случае контроллер структуры Azure не сможет завершить процесс обновления, поскольку в текущем домене обновления не будет запущено достаточное количество работоспособных экземпляров. Такое состояние называется *константной неисправностью развертывания*. Выйти из этой ситуации поможет откат обновления или применение свежего обновления поверх неисправного.

Когда первоначальный запрос на обновление службы будет принят контроллером структуры Azure, можно начинать другие операции изменения. Таким образом, чтобы начать следующую операцию изменения, вам не нужно ждать завершения предыдущей.

Запуск второй операции обновления во время первого обновления будет обработан так же, как операция отката. Если второе обновление выполняется в автоматическом режиме, первый домен обновления будет обновлен немедленно. Это может привести к тому, что одновременно будут отключены экземпляры из нескольких доменов обновления.

Существуют следующие операции изменения: [Изменить конфигурацию развертывания](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Обновить развертывание](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Обновить состояние развертывания](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Удалить развертывание](https://msdn.microsoft.com/library/azure/ee460815.aspx) и [Откатить обновление](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Две операции ([Получить развертывание](https://msdn.microsoft.com/library/azure/ee460804.aspx) и [Получить свойства облачной службы](https://msdn.microsoft.com/library/azure/ee460806.aspx)) возвращают тег блокировки Locked, который позволяет определить, можно ли запустить операции изменения в определенном развертывании.

Чтобы получить эти элементы, в запросе при вызове метода должен быть указан заголовок «x-ms-version: 2011-10-01» или с более поздней версией. Подробнее о заголовках управления версиями см. в статье [Управление версиями службы управления](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## Распределение ролей в доменах обновления
Azure равномерно распределяет экземпляры роли по заданному количеству доменов обновления, которое можно указать в файле определения службы (CSDEF). По умолчанию используется 5 доменов обновления, а максимально возможное количество равно 20. Подробнее об изменении файла определения службы см. в разделе [Схема определения службы Azure (CSDEF-файл)](cloud-services-model-and-package.md#csdef).

Например, для роли с 10 экземплярами к каждому домену обновления по умолчанию будет отнесено 2 экземпляра. Если роль содержит 14 экземпляров, в четырех доменах обновления будет по три экземпляра, а в пятом — два экземпляра.

Нумерация доменов обновления начинается с нуля: первый домен обновления имеет идентификатор 0, второй — идентификатор 1 и т. д.

Следующая схема демонстрирует распределение экземпляров в службе, содержащей две роли, если задано два домена обновления. Служба выполняет восемь экземпляров веб-роли и девять экземпляров рабочей роли.

![Распределение доменов обновления](media/cloud-services-update-azure-service/IC345533.png "Распределение доменов обновления")

> [!NOTE]
> Обратите внимание, что распределением экземпляров в домены обновления управляет Azure. Вы не можете повлиять на то, в какой домен будут включены конкретные экземпляры.
> 
> 

## Дальнейшие действия
[Управление облачными службами](cloud-services-how-to-manage.md) [Мониторинг облачных служб](cloud-services-how-to-monitor.md) [Настройка облачных служб](cloud-services-how-to-configure.md)

<!---HONumber=AcomDC_0907_2016-->