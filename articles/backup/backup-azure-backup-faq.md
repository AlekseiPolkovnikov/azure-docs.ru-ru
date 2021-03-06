---
title: "Часто задаваемые вопросы о службе архивации Azure | Документация Майкрософт"
description: "Ответы на часто задаваемые вопросы о службе архивации, агенте службы архивации, резервном копировании и периоде удержания, восстановлении, безопасности, а также ответы на другие распространенные вопросы об аварийном восстановлении и резервном копировании."
services: backup
documentationcenter: 
author: markgalioto
manager: jwhit
editor: 
keywords: "резервное копирование и аварийное восстановление; служба архивации"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2016
ms.author: trinadhk; giridham; arunak; markgal; jimpark;
translationtype: Human Translation
ms.sourcegitcommit: be06f1eca1848ff6d00661cfc1166797649a98a4
ms.openlocfilehash: cb45e7113073d19c1dc3e305d7b69373bd38d84f


---
# <a name="azure-backup-service--faq"></a>Служба архивации Azure: часто задаваемые вопросы
В этой статье приведены ответы на часто задаваемые вопросы о службе архивации Azure. Специалисты сообщества отвечают на вопросы в кратчайшие сроки. Если какой-то вопрос задают часто, мы добавляем ответ на него в эту статью. В ответах обычно указываются ссылки или сведения о поддержке. Вопросы о службе архивации Azure можно задать в разделе обсуждений этой статьи или другой связанной статьи. Кроме того, их также можно задать на [форуме для обсуждений](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup-br"></a>Какие операционные системы поддерживают резервное копирование в Azure с помощью службы архивации Azure? <br/>
Служба архивации Azure поддерживает следующие операционные системы для резервного копирования файлов, папок и рабочих приложений с помощью сервера службы архивации Azure и SCDPM.

| Операционная система | платформа | SKU |
|:--- | --- |:--- |
| Windows 8 и последние пакеты обновления |64-разрядная |Корпоративная, Профессиональная |
| Windows 7 и последние пакеты обновления |64-разрядная |Максимальная, Корпоративная, Профессиональная, Домашняя расширенная, Домашняя базовая, Начальная |
| Windows 8.1 и последние пакеты обновления |64-разрядная |Корпоративная, Профессиональная |
| Windows 10 |64-разрядная |Корпоративная, Профессиональная, Домашняя |
| Windows Server 2012 R2 и последние пакеты обновления |64-разрядная |Standard, Datacenter, Foundation |
| Windows Server 2012 и последние пакеты обновления |64-разрядная |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 и последние пакеты обновления |64-разрядная |Standard, Workgroup |
| Windows Storage Server 2012 и последние пакеты обновления |64-разрядная |Standard, Workgroup |
| Windows Server 2012 R2 и последние пакеты обновления |64-разрядная |Essential |
| Windows Server 2008 R2 с пакетом обновления 1 |64-разрядная |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 с пакетом обновления 2 (SP2) |64-разрядная |Standard, Enterprise, Datacenter, Foundation |

Резервное копирование виртуальных машин Azure

* **Linux**: служба архивации Azure поддерживает весь [список дистрибутивов, рекомендуемых для использования в Azure](../virtual-machines/virtual-machines-linux-endorsed-distros.md) , за исключением CoreOS Linux.  Другие собственные дистрибутивы Linux также должны работать, если агент виртуальной машины доступен на виртуальной машине и есть поддержка Python.
* **Windows Server**: версии до Windows Server 2008 R2 не поддерживаются.

## <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Откуда можно скачать последнюю версию агента службы архивации Azure? <br/>
Последнюю версию агента для резервного копирования Windows Server, System Center DPM и клиента Windows можно скачать [отсюда](http://aka.ms/azurebackup_agent). Если вы хотите сделать резервную копию виртуальной машины, используйте агент виртуальной машины (он автоматически устанавливает нужное расширение). Как правило, агент виртуальной машины уже есть на виртуальных машинах, созданных с использованием коллекции Azure.

## <a name="which-version-of-scdpm-server-is-supported-br"></a>Какая версия сервера SCDPM поддерживается? <br/>
Рекомендуется установить [последнюю](http://aka.ms/azurebackup_agent) версию агента службы архивации Azure на последний накопительный пакет обновления SCDPM (UR11 по состоянию на август 2016 г.).

## <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>При настройке агента службы архивации Azure появляется запрос на ввод учетных данных хранилища. Ограничен ли срок действия этих учетных данных?
Да, учетные данные хранилища действительны в течение 48 часов. Если срок действия файла истек, войдите на портал Azure и скачайте новый файл учетных данных из хранилища.

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Существует ли ограничение на число хранилищ, которые можно создать в каждой подписке Azure? <br/>
Да. Начиная с сентября 2016 года можно создать 25 резервных хранилищ на подписку. Можно создать не более 25 хранилищ служб восстановления на каждый поддерживаемый регион службы архивации Azure на подписку. Если вам нужно больше хранилищ, создайте новую подписку.

## <a name="are-there-any-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Существует ли ограничение на число серверов и компьютеров, которые можно зарегистрировать в каждом хранилище? <br/>
Да, в одном хранилище можно зарегистрировать не более 50 компьютеров. Для виртуальных машин Azure IaaS это ограничение предусматривает до 200 ВМ на хранилище. Если необходимо зарегистрировать больше виртуальных машин, создайте новое хранилище.

## <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Как зарегистрировать сервер в другом центре обработки данных?<br/>
Данные резервной копии отправляются в центр обработки данных хранилища, в котором выполнена регистрация. Самый простой способ изменить ЦОД — удалить агент, установить его повторно и зарегистрироваться в новом хранилище, которое принадлежит к нужному центру обработки данных.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Что произойдет, если переименовать сервер Windows, выполняющий резервное копирование данных в службу Azure?<br/>
Если переименовать сервер, остановятся все настроенные процессы резервного копирования.
Зарегистрируйте новое имя сервера в хранилище службы архивации. После регистрации нового имени в хранилище сначала выполняется операция *полного* резервного копирования. Если вам нужно восстановить резервные копии данных из хранилища со старым именем сервера, используйте переключатель [**Другой сервер**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) в мастере **восстановления данных**.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from-br"></a>С каких типов дисков можно создавать резервные копии файлов и папок? <br/>
Резервные копии нельзя создавать для следующих дисков и томов:

* Съемные носители. Диск должен быть фиксированным. Только такой диск можно использовать в качестве источника для резервного копирования.
* Тома только для чтения. Том должен быть доступный для записи, иначе служба теневого копирования томов (VSS) не будет работать.
* Автономные тома. Том должен быть в сети, иначе служба VSS не будет работать.
* Сетевая папка. Том должен быть локальным томом сервера, иначе оперативное резервное копирование не будет выполняться.
* Тома, зашифрованные по технологии Bitlocker. Для создания резервной копии том должен быть разблокирован.
* Идентификация файловой системы. Данная версия службы онлайн-архивации поддерживает только файловую систему NTFS.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Для каких типов файлов и папок можно создавать резервные копии с моего сервера?<br/>
Поддерживаются такие типы:

* зашифрованные;
* сжатые;
* разреженные;
* сжатые и разреженные;
* жесткие связи (не поддерживаются, пропускаются);
* точки повторного анализа (не поддерживаются, пропускаются);
* зашифрованные и сжатые (не поддерживаются, пропускаются);
* зашифрованные и разреженные (не поддерживаются, пропускаются);
* сжатые потоки (не поддерживаются, пропускаются);
* разреженные потоки (не поддерживаются, пропускаются).

## <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Каков минимальный необходимый размер папки кэша? <br/>
Размер папки кэша определяет объем данных, для которых выполняется резервное копирование. Для папки кэша выделяется 5 % пространства, необходимого для хранения данных.

## <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Если у моей организации есть одно хранилище, как изолировать восстановление данных определенного сервера от других серверов?<br/>
Все серверы, зарегистрированные в одном и том же хранилище, могут восстанавливать данные резервных копий, созданные другими серверами, для которых *используется одна парольная фраза*. Если вам нужно изолировать восстановление данных резервной копии сервера от других серверов в организации, настройте для него уникальную парольную фразу. Например, серверы отдела по работе с персоналом могут использовать одну зашифрованную парольную фразу, серверы бухгалтерии — другую, а серверы-хранилища — третью.

## <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Можно ли перенести резервные копии данных или хранилище резервных копий из одной подписки в другую? <br/>
Нет. Хранилище создается на уровне подписки и после создания не может быть перенесено в другую подписку.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Работает ли агент службы архивации Azure на сервере, где используется дедупликация Windows Server 2012? <br/>
Да. При подготовке операции резервного копирования служба агента преобразует дедуплицированные данные в обычные. Затем она оптимизирует данные для резервного копирования, шифрует их и отправляет зашифрованные данные в службу онлайн-архивации.

## <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Удаляются ли переданные данные резервной копии при отмене запущенной операции резервного копирования? <br/>
Нет. Все данные, переданные в хранилище до момента отмены, остаются в хранилище. Служба архивации Azure использует механизм контрольных точек, чтобы добавлять точки в данные резервной копии во время резервного копирования. Благодаря наличию контрольных точек во время следующего резервного копирования можно проверить целостность файлов. Следующее задание выполнит добавочное резервное копирование относительно уже переданных данных. При добавочном резервном копировании передаются только новые или измененные данные, что позволяет более эффективно использовать пропускную способность сети.

Если вы отмените задание резервного копирования для виртуальной машины Azure, все переданные данные игнорируются. Следующее задание резервного копирования передает добавочные данные (изменения), произошедшие с момента последнего успешного задания.

## <a name="why-am-i-seeing-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-had-scheduled-regular-backups-previously-br"></a>Почему отображается предупреждение "Служба архивации Azure для этого сервера не настроена", хотя регулярное резервное копирование было запланировано? <br/>
Это предупреждение появляется, если параметры расписания резервного копирования, сохраненные на локальном сервере, не совпадают с параметрами в хранилище службы архивации. Восстановление сервера или параметров до известного рабочего состояния может привести к потере синхронизации между расписаниями. Если вы получили это предупреждение, [повторно настройте политику резервного копирования](backup-azure-manage-windows-server.md) и выберите команду **Запустить архивацию сейчас** , чтобы заново синхронизировать локальный сервер с Azure.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup-br"></a>Какие правила брандмауэра следует настроить для использования службы архивации Azure? <br/>
Чтобы обеспечить эффективную защиту данных, передаваемых из локального сервера и при рабочей нагрузке в Azure, рекомендуем настроить для брандмауэра обмен данными с сайтами со следующими URL-адресами:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Можно ли установить агент службы архивации Azure на виртуальной машине Azure, резервная копия которой уже создана службой архивации Azure с использованием расширения виртуальной машины? <br/>
Конечно. Служба архивации Azure предоставляет возможность резервного копирования виртуальных машин Azure с использованием расширения. Установите агент службы архивации Azure в гостевой ОС Windows, чтобы защитить ее файлы и папки.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Можно ли установить агент службы архивации Azure на виртуальной машине Azure для резервного копирования файлов и папок, расположенных во временном хранилище на виртуальной машине Azure? <br/>
Да. Установите агент службы архивации Azure в гостевой ОС Windows и создайте резервную копию файлов и папок во временном хранилище. Но учитывайте, что удаление данных из временного хранилища приведет к сбою резервного копирования. Кроме того, если удалить данные из временного хранилища, их можно восстановить только в постоянном хранилище.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>У меня установлен агент службы архивации Azure для защиты файлов и папок. Можно ли теперь установить SCDPM для работы с агентом службы архивации Azure, чтобы защитить рабочие нагрузки локальных приложений и виртуальных машин в Azure? <br/>
Чтобы использовать службу архивации Azure с System Center Data Protection Manager (DPM), сначала установите DPM, а затем — агент службы архивации Azure. Только такой порядок установки компонентов службы архивации Azure обеспечит возможность взаимодействия между агентом службы архивации Azure и DPM. Установка агента службы архивации Azure до установки DPM не рекомендуется и не поддерживается.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent-br"></a>Какой длины может быть путь к файлу, определяемый в политике резервного копирования Azure с помощью агента службы архивации Azure? <br/>
Агент службы архивации Azure использует NTFS. Длина [пути к файлам ограничивается настройками API Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Если длина пути к файлам, для которых выполняется резервное копирование, превышает ограничения Windows API, вы можете выполнить резервное копирование родительской папки или всего диска с этими файлами.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Какие символы можно использовать в пути к файлу, определяемом в политике резервного копирования Azure с помощью агента службы архивации Azure? <br>
 Агент службы архивации Azure использует NTFS. Поэтому, указывая файлы, можно использовать [символы, которые поддерживаются в NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) .  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Можно ли использовать сервер службы архивации Azure, чтобы создать резервную копию для восстановления исходного состояния физического сервера? <br/>
Да.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails-br"></a>Можно ли настроить в службе резервного копирования отправку сообщений электронной почты при сбое задания резервного копирования? <br/>
Да, в службе резервного копирования предусмотрено несколько оповещений на основе событий. Эти оповещения можно отправлять с помощью сценариев PowerShell. Полное описание см. в разделе, посвященном [настройке оповещений](backup-azure-monitor-vms.md#configure-notifications).

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Существует ли ограничение на размер каждого источника данных, для которого выполняется резервное копирование? <br/>
Нет ограничений на объем данных для резервного копирования в хранилище. Хотя служба архивации Azure ограничивает максимальный размер источника данных, речь идет об очень больших величинах. По состоянию на август 2015 г. используются следующие ограничения максимального размера источника данных для поддерживаемых операционных систем.

| Серийный номер | Операционная система | Максимальный размер источника данных |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 или более поздней версии; |54 400 ГБ |
| 2 |Windows 8 или более поздняя версия |54 400 ГБ |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 ГБ |
| 4. |Windows 7 |1700 ГБ |

В таблице ниже описано, по какому принципу определяется размер источника данных.

| Источник данных | Сведения |
|:---:|:--- |
| Том |Объем данных, для которых выполняется резервное копирование из одного тома сервера или клиентского компьютера |
| Виртуальные машины Hyper-V |Общий объем данных всех виртуальных жестких дисков виртуальной машины, для которой выполняется резервное копирование |
| База данных Microsoft SQL Server |Размер одной базы данных SQL, для которой выполняется резервное копирование |
| Microsoft SharePoint |Общий объем баз данных содержимого и конфигурации в ферме SharePoint, для которой выполняется резервное копирование |
| Microsoft Exchange |Общее количество баз данных Exchange на сервере Exchange, для которого выполняется резервное копирование |
| Восстановление исходного состояния системы и состояние системы |Каждая отдельная копия восстановления исходного состояния системы или состояния системы компьютера, для которого выполняется резервное копирование |

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-daybr"></a>Существует ли ограничение на количество запланированных заданий резервного копирования в день?<br/>
С помощью Windows Server или клиента Windows можно выполнять до трех заданий резервного копирования в день. С помощью System Center DPM можно выполнять не больше двух заданий резервного копирования в день. Для виртуальных машин IaaS резервное копирование можно выполнять один раз в день.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-ie-on-windows-server-without-dpm-br"></a>Есть ли отличия между политикой планирования DPM и Windows Server (т. е. на сервере Windows Server без DPM)? <br/>
Да. При использовании DPM можно настроить ежедневное, еженедельное, ежемесячное и ежегодное расписание. С помощью Windows Server (без DPM) можно задать только ежедневное и еженедельное расписание.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-ie-on-windows-server-without-dpmbr"></a>Есть ли отличия между политикой хранения DPM и Windows Server или клиента Windows (т. е. на сервере Windows Server без DPM)?<br/>
Нет, с помощью DPM, Windows Server и клиента Windows можно настроить ежедневную, еженедельную, ежемесячную и ежегодную политику хранения.

## <a name="can-i-configure-my-retention-policies-selectively-ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Можно ли настроить политики хранения выборочно, т. е. настроить еженедельную и ежедневную политики, но не ежегодную или ежемесячную?<br/>
Да, структура хранения службы архивации Azure обеспечивает полную гибкость в определении политики хранения в соответствии с вашими требованиями.

## <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Можно ли запланировать резервное копирование на 18:00, а политики хранения настроить на другое время?<br/>
Нет. Политики хранения можно применять только в точках резервного копирования. На изображении ниже политика хранения указывается для резервных копий, создаваемых в 00:00 и 18:00. <br/>

![Планирование резервного копирования и хранения](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Передается ли для запланированных политик хранения добавочная копия? <br/>
Нет, добавочная копия отправляется в то время, которое указано на странице расписания резервного копирования. Точки, которые могут храниться, определяются на основе политики хранения.

## <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Если резервная копия хранится в течение длительного времени, занимает ли восстановление старой точки данных больше времени? <br/>
 Нет. Время восстановления старых и новых точек является одинаковым. Каждая точка восстановления ведет себя как полная точка.

## <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Если каждая точка представляет собой полную точку, влияет ли это на общее оплачиваемое хранилище резервных копий?<br/>
Обычно продукты с точками длительного срока хранения хранят резервные данные как полные точки. Полные точки *неэффективно* используют хранилище, но их восстановление осуществляется проще и быстрее. Добавочные копии используют хранилище *эффективно* , но для них необходимо выполнять восстановление цепочки данных, что влияет на время восстановления. В архитектуре хранилища службы архивации Azure учтены все преимущества обоих подходов: вы получаете оптимальное хранилище с возможностью быстрого восстановления данных и низкими затратами на хранение. Этот подход обеспечивает эффективное использование пропускной способности (как для входящих, так и для исходящих данных). При этом использование емкости хранилища данных и время восстановления сводятся к минимуму. Дополнительные сведения об эффективности хранения [добавочных резервных копий](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) .

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Существует ли ограничение на число создаваемых точек восстановления?<br/>
Нет. Мы сняли ограничения на точки восстановления. Вы можете создать сколько угодно точек восстановления.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-upbr"></a>Почему объем данных, переданных при резервном копировании, не равен объему данных, для которых создана резервная копия?<br/>
 Все данные, для которых создается резервная копия из агента службы архивации Azure, а также из SCDPM или с сервера службы архивации Azure, сжимаются и шифруются перед передачей. После сжатия и шифрования данные, которые попадают в резервное хранилище, становятся на 30–40 % меньше.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Можно ли настроить пропускную способность для службы архивации?<br/>
 Да, чтобы настроить пропускную способность, можно использовать команду **Изменить свойства** в агенте службы архивации. Вы можете настроить объем пропускной способности и время ее использования. Пошаговая инструкция приведена в разделе **[Включить регулирование сети (необязательно)](backup-configure-vault.md#enable-network-throttling)**, в статьи [Архивация сервера Windows Server или клиента Windows в Azure с использованием модели развертывания с помощью Resource Manager].

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure-br"></a>Пропускной способности моего интернет-канала недостаточно для объема данных, для которых требуется создать резервную копию. Как можно переместить данные в определенное расположение с широким сетевым каналом, а затем передать их в Azure? <br/>
Вы можете выполнить резервное копирование данных в Azure с помощью стандартного процесса оперативного резервного копирования или использовать службу импорта и экспорта Azure, чтобы передать данные в хранилище BLOB-объектов Azure. Других способов создать резервную копию данных в службе хранилища Azure не существует. Сведения о том, как использовать службу импорта и экспорта Azure со службой архивации Azure, см. в статье [Автономное резервное копирование в службе архивации Azure](backup-azure-backup-import-export.md).

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Сколько раз можно восстанавливать данные из резервной копии в Azure?<br/>
Ограничение на количество операций восстановления из службы архивации Azure отсутствует.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveriesbr"></a>Нужно ли платить за исходящий трафик из центра обработки данных Azure при восстановлении?<br/>
 Нет. Восстановление является бесплатным, а плата за исходящий трафик не взимается.

## <a name="is-the-data-sent-to-azure-encrypted-br"></a>Шифруются ли передаваемые в Azure данные? <br/>
Да. Данные шифруются на локальном сервере, клиенте или компьютере SCDPM с помощью AES256 и передаются через защищенное соединение HTTPS.

## <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Шифруются ли резервные копии данных в Azure?<br/>
 Да. Отправленные в Azure данные остаются зашифрованными (при хранении). Мы никогда не расшифровываем резервные копии данных. При резервном копировании виртуальной машины Azure служба архивации Azure использует шифрование виртуальной машины. Например, если ваша виртуальная машина зашифрована с помощью службы шифрования дисков Azure или другой технологии шифрования, служба архивации Azure использует это шифрование для защиты данных.

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Какова минимальная длина ключа шифрования, используемого для шифрования резервных копий данных? <br/>
 Ключ шифрования должен состоять из не менее 16 символов.

## <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Что будет в случае потери ключа шифрования? Смогу ли я или корпорация Майкрософт восстановить данные? <br/>
Ключ, используемый для шифрования резервной копии данных, есть только у клиента. Мы не храним копии ключей в Azure и не имеет к ключам никакого доступа. Если клиент потеряет ключ, мы не сможем восстановить данные из резервных копий.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Как изменить расположение кэша, указанное для агента службы архивации Azure?<br/>
 Чтобы изменить расположение кэша, последовательно выполните перечисленные ниже действия.

* Остановите работу модуля архивации, выполнив следующую команду в командной строке с повышенными привилегиями:

  ```PS C:\> Net stop obengine```
* Не перемещайте файлы. Скопируйте папку пространства кэша на другой диск с достаточным объемом свободного места. Исходную папку кэша можно удалить, когда убедитесь, что резервные копии используют новый кэш.
* Обновите следующие записи реестра, указав путь к новой папке размера кэша.<br/>

| Путь к элементу реестра | Ключ реестра | Значение |
| --- | --- | --- |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Новое расположение папки кэша* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Новое расположение папки кэша* |

* Перезапустите модуль архивации, выполнив следующую команду в командной строке с повышенными привилегиями:

  ```PS C:\> Net start obengine```

  После успешного выполнения заданий архивации с использованием нового кэша вы можете удалить изначальную папку кэша.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Где можно сохранить папку кэша, чтобы агент службы архивации Azure работал должным образом?<br/>
Для папки кэша не рекомендуется использовать следующие расположения.

* Сетевая папка или съемный носитель. Папка кэша должна быть локальной по отношению к серверу, которому требуется резервное копирование с помощью оперативного резервного копирования. Сетевые расположения и съемные носители (например, USB-накопители) не поддерживаются.
* Автономные тома. Папка кэша должна быть подключена к сети, если планируется резервное копирование с помощью агента службы архивации Azure.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Существуют ли неподдерживаемые атрибуты папки кэша?<br/>
 Для папки кэша не поддерживаются следующие атрибуты и их комбинации:

* зашифрованные;
* дедупликация;
* сжатые;
* разреженные;
* точка повторного анализа.

Ни папка кэша, ни метаданные виртуального жесткого диска не содержат нужных атрибутов для агента службы архивации Azure.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Хранилища служб восстановления развернуты с помощью Resource Manager. Поддерживаются ли еще хранилища службы архивации, развернутые с помощью классической модели? <br/>
Да, хранилища службы архивации по-прежнему поддерживаются. Хранилища службы архивации можно создать на [классическом портале](https://manage.windowsazure.com). Хранилища служб восстановления можно создать на [портале Azure](https://portal.azure.com). Но мы настоятельно рекомендуем создать хранилище службы восстановления, так как все будущие улучшения будут доступны только в этом решении.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Можно ли перенести хранилище службы архивации в хранилище служб восстановления? <br/>
К сожалению, нет. В настоящее время содержимое хранилища службы архивации нельзя перенести в хранилище служб восстановления. Мы работаем над добавлением этих функциональных возможностей, но пока они недоступны.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Какие виртуальные машины поддерживают хранилища служб восстановления: классические ВМ или ВМ на основе Resource Manager? <br/>
Хранилища служб восстановления поддерживают обе модели.  В хранилище служб восстановления можно создать резервную копию классической виртуальной машины (созданной на классическом портале) или виртуальной машины Resource Manager (созданной на портале Azure).

## <a name="i-have-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Резервные копии классических виртуальных машин уже находятся в хранилище службы архивации. Можно ли переключить виртуальные машины из классического режима в режим Resource Manager и защитить их в хранилище служб восстановления?
Точки восстановления классических виртуальных машин в хранилище службы архивации не будут автоматически перемещаться в хранилище служб восстановления, если вы переключите виртуальные машины из классического режима в режим Resource Manager. Чтобы переключить резервные копии виртуальных машин, выполните следующие действия.

1. В хранилище службы архивации на вкладке **Защищенные элементы** выберите нужную виртуальную машину. Щелкните [Остановить защиту](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). *Снимите* флажок **Удаление связанных архивируемых данных**.
2. Теперь эту виртуальную машину можно перевести из классического режима в режим Resource Manager. Хранилище и сеть, связанные с виртуальной машиной, также следует перевести в режим Resource Manager.
3. Создайте хранилище служб восстановления и настройте резервное копирование для перенесенной виртуальной машины. Это можно сделать с помощью действия **Резервное копирование** в верхней части панели мониторинга хранилища. Дополнительные сведения об архивации виртуальных машин в хранилище службы восстановления см. в статье [Первое знакомство. Защита виртуальных машин в хранилище служб восстановления](backup-azure-vms-first-look-arm.md).



<!--HONumber=Nov16_HO3-->


