---
title: Обзор виртуального массива StorSimple | Microsoft Docs
description: Здесь описывается виртуальный массив Microsoft Azure StorSimple — интегрированное решение хранилища, которое управляет задачами хранилища на локальном виртуальном устройстве и в облачном хранилище Microsoft Azure.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''

ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/06/2016
ms.author: alkohli

---
# <a name="introduction-to-the-storsimple-virtual-array"></a>Общие сведения о виртуальном массиве StorSimple
## <a name="overview"></a>Обзор
Виртуальный массив Microsoft Azure StorSimple — это интегрированное решение хранилища, которое управляет задачами хранилища на локальном виртуальном устройстве, выполняющемся в низкоуровневой оболочке, и в облачном хранилище Microsoft Azure. Виртуальный массив (также называется локальным виртуальным устройством StorSimple) представляет собой эффективный, экономичный и легко управляемый файловый сервер или сервер iSCSI, который устраняет многие проблемы и расходы, связанные с корпоративным хранилищем и защитой данных. Виртуальный массив особенно хорошо подходит для сценариев, применяемых в удаленных офисах или филиалах.

Этот обзор посвящен виртуальному массиву. 

* Обзор устройств серии StorSimple 8000 см. в статье [Серия StorSimple 8000: решение гибридного облачного хранилища](storsimple-overview.md). 
* Дополнительные сведения об устройствах серии StorSimple 5000 или 7000 см. в [онлайн-справке по StorSimple](http://onlinehelp.storsimple.com/).

Виртуальный массив поддерживает протокол iSCSI или SMB. Он запускается в существующей инфраструктуре низкоуровневой оболочки и обеспечивает распределение по уровням в облаке, резервное копирование в облако, быстрое восстановление, восстановление на уровне элементов и аварийное восстановление.

В следующей таблице перечислены важные функции виртуального массива.

| Функция | Виртуальный массив |
| --- | --- |
| Требования для установки |Использует инфраструктуру виртуализации (Hyper-V или VMware) |
| Доступность |Один узел |
| Общая емкость (включая облако) |До 64 ТБ используемой емкости на виртуальное устройство |
| Локальная емкость |От 390 ГБ до 6,4 ТБ используемой емкости на виртуальное устройство (требуется подготовить от 500 ГБ до 8 ТБ места на диске) |
| Собственные протоколы |iSCSI или SMB |
| Целевое время восстановления (RTO) |iSCSI: менее 2 минут независимо от размера |
| Целевая точка восстановления (RPO) |Ежедневное резервное копирование и резервное копирование по требованию |
| Распределение по уровням хранилища |Использует тепловую карту, чтобы определить, какие данные следует распределить по уровням или перенести |
| Поддержка |Инфраструктура виртуализации, поддерживаемая поставщиком |
| Производительность |Зависит от базовой инфраструктуры |
| Мобильность данных |Позволяет выполнить восстановление на то же устройство или восстановление на уровне элементов (файловый сервер) |
| Уровни хранилища |Локальное хранилище низкоуровневой оболочки и облако |
| Размер общего ресурса |Многоуровневые — до 20 ТБ, локально закрепленные — до 2 ТБ |
| Размер тома |Многоуровневые — до 5 ТБ, локально закрепленные — до 500 ГБ. |
| Моментальные снимки |Отказоустойчивые |
| Восстановление на уровне элементов |Пользователи могут выполнить восстановление из общих папок |

## <a name="why-use-storsimple?"></a>Зачем использовать StorSimple?
StorSimple подключает пользователей и серверы к хранилищу Azure за считанные минуты, не изменяя приложение.

В приведенной ниже таблице описаны некоторые ключевые преимущества, которые предоставляет виртуальный массив.

| Функция | Преимущество |
| --- | --- |
| Прозрачная интеграция |Виртуальный массив поддерживает протокол iSCSI или SMB. Перемещение данных между локальным и облачным уровнями осуществляется прозрачно и незаметно для пользователей. |
| Снижение затрат на хранилище |StorSimple позволяет подготовить локальное хранилище достаточного объема в соответствии с текущими требованиями наиболее используемых "горячих" данных. По мере роста потребностей хранения StorSimple распределяет "холодные" данные по уровням экономичного облачного хранилища. Перед отправкой в облако данные дедуплицируются и сжимаются, чтобы еще больше сократить требования к хранилищу и затраты. |
| Упрощенное управление хранилищем |StorSimple обеспечивает централизованное управление в облаке, позволяя управлять несколькими устройствами с помощью диспетчера StorSimple. |
| Усовершенствованное аварийное восстановление и соответствие |StorSimple помогает ускорить аварийное восстановление, восстанавливая метаданные немедленно, а остальные данные — по мере необходимости. Это означает, что нормальная работа может быть продолжена с минимальными простоями. |
| Мобильность данных |Доступ к данным, распределенным по уровням в облаке, может быть организован и для других сайтов в целях восстановления и переноса данных. Обратите внимание, что данные можно восстанавливать только на исходный виртуальный массив. Тем не менее функции аварийного восстановления можно использовать для восстановления всего виртуального массива на другом виртуальном массиве. |

## <a name="storsimple-workload-summary"></a>Сводка по рабочим нагрузкам StorSimple
Ниже приведена сводка по поддерживаемым рабочим нагрузкам StorSimple.

| Сценарий | Рабочая нагрузка | Поддерживаются | Ограничения | Версия |
| --- | --- | --- | --- | --- |
| Совместная работа в удаленных офисах или филиалах |Общий доступ к файлам |Да |См. раздел [Максимальные ограничения для файлового сервера](storsimple-ova-limits.md). <br>См. раздел [Системные требования для поддерживаемых версий SMB](storsimple-ova-system-requirements.md). |Все версии |

## <a name="workflows"></a>Рабочие процессы
Виртуальный массив StorSimple особенно хорошо подходит для следующих рабочих процессов:

* [облачное управление хранилищем;](#cloud-based-storage-management)
* [резервное копирование независимо от расположения;](#location-independent-backup)
* [защита данных и аварийное восстановление.](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>облачное управление хранилищем;
Службу диспетчера StorSimple, запущенную на классическом портале Azure, можно использовать для управления данными, хранящимися на нескольких устройствах и в нескольких расположениях. Это особенно полезно в сценариях распределенных филиалов. Обратите внимание, что для управления виртуальными массивами и физическими устройствами StorSimple необходимо создать отдельные экземпляры службы диспетчера StorSimple. 

![облачное управление хранилищем;](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>резервное копирование независимо от расположения;
Благодаря виртуальному массиву облачные моментальные снимки позволяют получить независимые от расположения копии тома или общей папки на определенный момент времени. Облачные моментальные снимки включены по умолчанию и их нельзя отключать. Согласно одной политике ежедневного резервного копирования резервные копии всех томов и общих папок создаются в одно и то же время. При необходимости можно выполнять дополнительное специальное резервное копирование.

### <a name="data-protection-and-disaster-recovery"></a>Защита данных и аварийное восстановление
Виртуальный массив поддерживает следующие сценарии защиты данных и аварийного восстановления.

* **Восстановление тома или общего ресурса**. Используйте новый рабочий процесс для восстановления тома или общего ресурса. Этот подход можно использовать для восстановления всего тома или общей папки.
* **Восстановление на уровне элемента**. Общие ресурсы упрощают доступ к последним архивам. Можно легко восстановить отдельный файл из специальной папки .backup, доступной в облаке. Этой возможностью восстановления управляет пользователь, поэтому вмешательство администратора не требуется.
* **Аварийное восстановление**. Используйте возможность отработки отказа для восстановления всех общих ресурсов или томов на новом виртуальном массиве. Нужно создать виртуальный массив и зарегистрировать его в службе диспетчера StorSimple, а затем выполнить отработку отказа для исходного виртуального массива. Предполагается, что новый виртуальный массив использует подготовленные ресурсы. 

## <a name="virtual-array-components"></a>Компоненты виртуального массива
Виртуальный массив содержит следующие компоненты.

* [Виртуальный массив](#virtual-array) — гибридное облачное хранилище на основе виртуальной машины, подготовленной в виртуализированной среде или гипервизоре.  
* [Служба диспетчера StorSimple](#storsimple-manager-service) — расширение классического портала Azure, которое позволяет управлять одним или несколькими виртуальными устройствами StorSimple в одном веб-интерфейсе, к которому можно получить доступ в различных географических расположениях. Службу диспетчера StorSimple можно использовать для создания служб и управления ими, просмотра устройств и уведомлений и управления ими, а также для управления томами, общими папками и существующими моментальными снимками.
* [Локальный пользовательский веб-интерфейс](#local-web-user-interface) — пользовательский веб-интерфейс, используемый для настройки подключения устройства к локальной сети и его регистрации в службе диспетчера StorSimple. 
* [Интерфейс командной строки](#command-line-interface) — интерфейс Windows PowerShell, который можно использовать для запуска сеанса поддержки по виртуальному массиву.
  В следующих разделах каждый из этих компонентов описывается подробно и объясняется, каким образом решение упорядочивает данные и выделяет память, а также помогает управлять хранилищем и защищать данные.

### <a name="virtual-array"></a>Виртуальный массив
Виртуальный массив — это система хранения с одним узлом, которая предоставляет функции основного хранилища, управляет взаимодействием с облачным хранилищем и обеспечивает безопасность и конфиденциальность всех данных, хранящихся на устройстве.

Доступна одна модель виртуального массива для скачивания. Максимальная емкость массива хранения составляет до 6,4 ТБ на устройство (требование для базового хранилища — 8 ТБ) и 64 ТБ с облачным хранилищем. 

Виртуальное устройство отличается следующими особенностями.

* Оно экономично. Оно использует существующую инфраструктуру виртуализации и его можно развернуть в существующей низкоуровневой оболочке Hyper-V или VMware.
* Оно находится в центре обработки данных и его можно настроить в качестве файлового сервера или сервера iSCSI. 
* Оно интегрировано с облаком.
* Резервные копии хранятся в облаке, что упрощает аварийное восстановление и восстановление на уровне элементов. 
* Виртуальный массив можно обновлять так же, как и физическое устройство.

> [!NOTE]
> Виртуальный массив нельзя расширить. Таким образом, при создании виртуального устройства важно подготовить достаточно места для хранения. 
> 
> 

### <a name="storsimple-manager-service"></a>Служба диспетчера StorSimple
Microsoft Azure StorSimple предоставляет пользовательский веб-интерфейс (службу диспетчера StorSimple), которая позволяет централизованно управлять центром обработки данных и облачным хранилищем. Вы можете использовать службу диспетчера StorSimple для выполнения следующих задач.

* Управление несколькими виртуальными массивами StorSimple из одной службы. 
* Настройка параметров безопасности и управление ими для устройств StorSimple. (Шифрование в облаке зависит от интерфейсов API Microsoft Azure.)
* Настройка учетных данных и свойств учетной записи хранения.
* Настройка томов и общих папок и управление ими.
* Резервное копирование и восстановление данных в общих папках или томах.
* Мониторинг производительности.
* Просмотр системных параметров и идентификация возможных проблем.

Служба диспетчера StorSimple используется для ежедневного администрирования виртуального массива.

Дополнительные сведения см. в статье [Использование службы диспетчера StorSimple для администрирования устройства StorSimple](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Локальный пользовательский веб-интерфейс
Виртуальный массив содержит пользовательский веб-интерфейс, используемый для одноразовой настройки и регистрации устройства в службе диспетчера StorSimple. С его помощью можно выключать и перезапускать виртуальный массив, выполнять диагностические тесты, обновлять программное обеспечение, изменять пароль администратора устройства, просматривать системные журналы и обращаться в службу технической поддержки Майкрософт для подачи запроса на обслуживание. 

Сведения об использовании веб-интерфейса см. в статье [Использование веб-интерфейса для администрирования виртуального массива StorSimple](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Интерфейс командной строки
Содержащийся интерфейс Windows PowerShell позволяет запустить сеанс поддержки со службой технической поддержки Майкрософт, специалисты которой помогут устранить неполадки и решить проблемы, которые могут возникнуть на виртуальном устройстве.

## <a name="storage-management-technologies"></a>Технологии управления хранилищем
Помимо виртуальных массивов и других компонентов, решение StorSimple использует следующие технологии программного обеспечения для предоставления быстрого доступа к важным данным, сокращения потребления хранилища и защиты данных, хранящихся в виртуальном массиве:

* [Автоматическое распределение по уровням хранилища](#automatic-storage-tiering) 
* [локально закрепленные общие папки и тома;](#locally-pinned-shares-and-volumes)
* [сжатие и дедупликация данных, распределенных по уровням в облаке или скопированных в облако;](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [резервное копирование по расписанию и по требованию.](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Автоматическое распределение по уровням хранилища
Виртуальный массив использует новый механизм распределения по уровням для управления данными, хранящимися на виртуальном массиве и в облаке. Существует всего два уровня: локальный виртуальный массив и облачное хранилище Azure. Виртуальный массив StorSimple автоматически распределяет данные по уровням согласно тепловой карте, которая отслеживает текущее использование, время существования и связи с другими данными. Наиболее активные данные (используемые чаще всего) хранятся локально, а менее активные и неактивные данные автоматически переносятся в облако. (Все резервные копии хранятся в облаке). StorSimple настраивает и переупорядочивает данные и назначения хранилища по мере изменения шаблонов использования. Например, некоторые сведения могут стать менее активными со временем. По мере уменьшения активности они распределяются по уровням в облаке. Если те же данные снова становятся активными, выполняется распределение по уровням в массиве хранения.

Данным для определенного многоуровневой общей папки или тома всегда гарантируется собственное пространство на локальном уровне (приблизительно 10 % от общего подготовленного пространства для этой общей папки или тома). Хотя таким образом снижается доступное место для хранения на виртуальном устройстве для этих общих папок или томов, но в то же время обеспечивается независимость требований к распределению по уровням для разных общих папок или томов. Таким образом, чрезмерная загруженность рабочего процесса общей папки или тома не приведет к переносу других рабочих нагрузок в облако. 

![Автоматическое распределение по уровням хранилища](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Можно указать том как локально закрепленный, при этом данные остаются на виртуальном устройстве и никогда не перемещаются в облако. Дополнительные сведения см. в разделе [Локально закрепленные общие папки и тома](#locally-pinned-shares-and-volumes).
> 
> 

### <a name="locally-pinned-shares-and-volumes"></a>локально закрепленные общие папки и тома;
Можно создать соответствующие общие папки и тома как локально закрепленные. Эта возможность гарантирует, что данные, необходимые критически важным приложениям, остаются в виртуальном массиве и никогда не перемещаются в облако. Локально закрепленные общие папки и тома отличаются следующими особенностями. 

* Они не подвержены характерным для облака задержкам или проблемам со связью.
* Они используют преимущества облачного резервного копирования и аварийного восстановления StorSimple.

Можно восстановить локально закрепленные общие папки или тома как многоуровневые, а многоуровневые общие папки или тома как локально закрепленные. 

Дополнительные сведения о локально закрепленных томах см. в статье [Использование службы диспетчера StorSimple для управления томами](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>сжатие и дедупликация данных, распределенных по уровням в облаке или скопированных в облако;
StorSimple использует дедупликацию и сжатие данных для дальнейшего уменьшения требований к хранилищу в облаке. Дедупликация уменьшает общий объем сохраненных данных, устраняя избыточность в наборе сохраненных данных. По мере изменения сведений StorSimple пропускает неизмененные данные, захватывая только изменения. Кроме того, StorSimple сокращает объем сохраненных данных, определяя и удаляя повторяющуюся информацию. 

> [!NOTE]
> Данные, хранящиеся в виртуальном массиве, не подвергаются дедупликации или сжатию. Дедупликация и сжатие выполняются непосредственно перед отправкой данных в облако.
> 
> 

### <a name="scheduled-and-on-demand-backups"></a>резервное копирование по расписанию и по требованию.
Функции защиты данных StorSimple позволяют создавать резервные копии по требованию. Кроме того, расписание резервного копирования по умолчанию гарантирует ежедневное резервное копирование данных. Резервные копии создаются в виде добавочных моментальных снимков, которые хранятся в облаке. Моментальные снимки, которые записывают изменения только с момента последнего резервного копирования, можно создать и быстро восстановить. Они могут быть критически важными в сценариях аварийного восстановления, так как заменяют дополнительные системы хранения данных (например, систему резервного копирования на магнитную ленту) и позволяют восстанавливать данные в центр обработки данных или на альтернативные сайты, если это необходимо.

## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [подготовить портал виртуального массива](storsimple-ova-deploy1-portal-prep.md).

<!--HONumber=Oct16_HO2-->


