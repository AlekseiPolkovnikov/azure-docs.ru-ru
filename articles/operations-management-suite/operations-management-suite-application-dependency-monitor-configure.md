---
title: Настройка монитора зависимости приложений (ADM) в Operations Management Suite (OMS) | Microsoft Docs
description: Монитор зависимости приложений (ADM) — это решение Operations Management Suite (OMS), которое автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами.  В этой статье содержатся подробные сведения о развертывании ADM в вашей среде и приведены разнообразные сценарии его использования.
services: operations-management-suite
documentationcenter: ''
author: daseidma
manager: jwhit
editor: tysonn

ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2016
ms.author: daseidma;bwren

---
# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-(oms)"></a>Настройка монитора зависимости приложений (ADM) в Operations Management Suite (OMS)
![Значок "Управление оповещениями"](media/operations-management-suite-application-dependency-monitor/icon.png) Монитор зависимости приложений (ADM) автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. Это решение позволяет рассматривать серверы как взаимосвязанные системы, предоставляющие важные службы.  Монитор зависимости приложений отображает сведения о подключениях между серверами, процессами и портами в любой подключенной по протоколу TCP архитектуре без дополнительной настройки. Пользователям требуется только установить агент.

В этой статье подробно описывается настройка монитора зависимости приложений и подключение агентов.  Дополнительные сведения об использовании ADM см. в статье [Использование монитора зависимости приложений (ADM) в Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor.md).

> [!NOTE]
> Сейчас доступна только закрытая предварительная версия монитора зависимости приложений.  Доступ к закрытой предварительной версии решения ADM можно запросить на странице [https://aka.ms/getadm](https://aka.ms/getadm).
> 
> На время действия закрытой предварительной версии все учетные записи OMS имеют свободный доступ к решению ADM.  Узлы ADM доступны бесплатно, но данные Log Analytics для типов AdmComputer_CL и AdmProcess_CL будут тарифицироваться, как и другие решения.
> 
> После выхода общедоступная предварительная версия ADM будет доступна только пользователям бесплатных и платных версий Insight and Analytics в ценовом плане OMS.  На уровне "Бесплатный" количество узлов ADM будет ограничено до 5.  Если вы используете закрытую предварительную версию и не зарегистрированы в ценовом плане OMS, при появлении общедоступной предварительной версии ADM будет отключен. 
> 
> 

## <a name="connected-sources"></a>Подключенные источники
В следующей таблице описаны подключенные источники, которые поддерживаются решением ADM.

| Подключенный источник | Поддерживаются | Описание |
|:--- |:--- |:--- |
| [Агенты Windows](../log-analytics/log-analytics-windows-agents.md) |Да |ADM анализирует и собирает данные из компьютеров агентов Windows.  <br><br>Обратите внимание, что помимо агентов OMS для агентов Windows необходим агент зависимостей Майкрософт.  Полный список версий операционной системы см. в разделе [Поддерживаемые операционные системы](#supported-operating-systems). |
| [Агенты Linux](../log-analytics/log-analytics-linux-agents.md) |Да |ADM анализирует и собирает данные из компьютеров агентов Linux.  <br><br>Обратите внимание, что помимо агентов OMS для агентов Linux необходим агент зависимостей Майкрософт.  Полный список версий операционной системы см. в разделе [Поддерживаемые операционные системы](#supported-operating-systems). |
| [Группы управления SCOM](../log-analytics/log-analytics-om-agents.md) |Да |ADM анализирует и собирает данные из агентов Windows и Linux в подключенной группе управления SCOM. <br><br>Прямое подключение из компьютера агента SCOM к OMS не требуется. Данные пересылаются напрямую из группы управления в репозиторий OMS. |
| [Учетная запись хранения Azure](../log-analytics/log-analytics-azure-storage.md) |Нет |ADM собирает данные из компьютеров агента, поэтому данные из хранилища Azure не собираются. |

Обратите внимание, что ADM поддерживает только 64-разрядные платформы.

В ОС Windows SCOM и OMS используют Microsoft Monitoring Agent (MMA) для сбора и отправки данных мониторинга.  (Этот агент называется агентом SCOM, агентом OMS, MMA или Direct Agent в зависимости от контекста.)  SCOM и OMS предоставляют различные готовые к использованию версии MMA, и в каждой из этих версий предусмотрена возможность отправлять отчеты в SCOM, OMS или в оба решения.  В ОС Linux агент OMS для Linux собирает и отправляет данные мониторинга в OMS.  ADM можно использовать на серверах с агентами Direct Agent OMS или на серверах, подключенных к OMS через группы управления SCOM.  В этой документации мы будем называть все эти агенты — как в Linux, так и в Windows, подключенные к SCOM MG или непосредственно к OMS — "агент OMS", если имя конкретного развертывания агента не понадобится в контексте.

Агент ADM самостоятельно не передает данные и не требует внесения изменений в брандмауэры или порты.  Данные ADM всегда передаются агентом OMS в OMS, напрямую или через шлюз OMS.

![Агенты ADM](media/operations-management-suite-application-dependency-monitor/agents.png)

Если вы являетесь пользователем SCOM с группой управления, подключенной к OMS:

* если у ваших агентов SCOM есть доступ к OMS через Интернет, никаких дополнительных настроек не требуется;  
* если у агентов SCOM нет доступа к OMS через Интернет, необходимо настроить шлюз OMS для работы с SCOM.

При использовании OMS Direct Agent необходимо настроить агент OMS для подключения к OMS или шлюзу OMS.  Шлюз OMS можно скачать, перейдя по ссылке [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666).

### <a name="avoiding-duplicate-data"></a>Предотвращение дублирования данных
Если вы являетесь пользователем SCOM, не следует настраивать агенты SCOM для непосредственного обмена данными с OMS, иначе данные будут отображаться дважды.  В ADM это приведет к тому, что компьютеры будут дважды показаны в списке компьютеров.

Конфигурацию OMS следует применять только в одном из следующих расположений:

* панель Operations Management Suite консоли SCOM для управляемых компьютеров;
* конфигурация оперативной аналитики Azure в свойствах MMA.

Использование обоих вариантов конфигурации с одной и той же рабочей областью в каждой из них приведет к дублированию данных. Использование обоих вариантов конфигурации с разными рабочими областями может привести к конфликтующей конфигурации (в одной из них решение ADM включено, а в другой — отключено), что повлечет за собой неполную передачу потоков данных в ADM.  

Обратите внимание, что даже если сам компьютер не указан в конфигурации OMS консоли SCOM и группа экземпляров, например "Группа экземпляров Windows Server", активна, это может привести к тому, что компьютер получит конфигурацию OMS через SCOM.

## <a name="management-packs"></a>Пакеты управления
Если ADM активирован в рабочей области OMS, пакет управления размером 300 КБ отправляется всем агентам Microsoft Monitoring Agent в этой рабочей области.  Если агенты SCOM используются в [подключенной группе управления](../log-analytics/log-analytics-om-agents.md), пакет управления ADM будет развернут из SCOM.  Если агенты подключены напрямую, пакет управления будет доставлен с помощью OMS.

Пакет управления называется Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Он записывается в папку *%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\*.  Источник данных, используемый пакетом управления, — *%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.

## <a name="configuration"></a>Конфигурация
В дополнение к установке агента на компьютерах Windows и Linux и подключению его к OMS необходимо скачать установщик агента зависимостей из решения ADM, а затем установить его с правами привилегированного пользователя или администратора на каждом управляемом сервере.  После установки агента ADM на сервере, который отправляет отчеты в OMS, карты зависимостей ADM появятся в течение 10 минут.  Если у вас возникнут проблемы, напишите по адресу [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).

### <a name="migrating-from-bluestripe-factfinder"></a>Миграция из BlueStripe FactFinder
Монитор зависимости приложения будет доставляет технологию BlueStripe в OMS поэтапно. FactFinder по-прежнему поддерживается для имеющихся клиентов, но его больше нельзя приобрести отдельно.  Эта предварительная версия агента зависимостей может обмениваться данными только с OMS.  Если вы являетесь текущим покупателем FactFinder, определите набор тестовых серверов для ADM, управление которыми не осуществляется с помощью FactFinder. 

### <a name="download-the-dependency-agent"></a>Скачивание агента зависимостей
Кроме агента MMA и OMS для Linux, которые обеспечивают подключение между компьютером и OMS, на всех компьютерах, которые анализирует монитор зависимости приложений, необходимо установить агент зависимостей.  В ОС Linux сначала необходимо установить агент OMS для Linux, а затем агент зависимостей. 

![Плитка монитора зависимости приложений](media/operations-management-suite-application-dependency-monitor/tile.png)

Чтобы скачать агент зависимостей, на плитке **Монитор зависимости приложений** щелкните **Настроить решение**, чтобы открыть колонку **Агент зависимостей**.  В колонке "Агент зависимости" содержатся ссылки на агенты Windows и Linux. Щелкните соответствующую ссылку, чтобы скачать каждый агент. Сведения об установке агента в различных системах см. в следующих разделах.

### <a name="install-the-dependency-agent"></a>Установка агента зависимостей
#### <a name="microsoft-windows"></a>Microsoft Windows
Для установки или удаления агента требуются права администратора.

Агент зависимостей устанавливается на компьютерах Windows с файлом ADM-Agent-Windows.exe. Если запустить этот исполняемый файл без параметров, запустится мастер с указаниями для установки в интерактивном режиме.  

Используйте следующие шаги, чтобы установить агент зависимостей на каждом компьютере Windows.

1. Убедитесь, что агент OMS установлен с помощью инструкций, указанных в статье о подключении компьютеров непосредственно к OMS.
2. Скачайте агент Windows и запустите его с помощью следующей команды:<br>*ADM-Agent-Windows.exe*
3. Следуйте инструкциям мастера для установки агента.
4. Если агент зависимостей не запускается, просмотрите подробные сведения об ошибке в записях журналов. В агентах Windows каталог журнала находится в расположении *C:\Program Files\BlueStripe\Collector\logs*. 

Администратор может удалить агент зависимостей для Windows через панель управления.

#### <a name="linux"></a> Linux
Для установки или настройки агента требуется доступ с правами привилегированного пользователя.

Агент зависимости устанавливается на компьютерах Linux с файлом ADM-Agent-Linux64.bin, скриптом оболочки с самоизвлекающимся двоичным файлом. Вы можете запустить этот файл с помощью sh или добавить разрешения на выполнение в сам файл.

Используйте следующие шаги, чтобы установить агент зависимостей на каждом компьютере Linux.

1. Убедитесь, что агент OMS установлен с помощью инструкций, приведенных в статье[Подключение компьютеров Linux  к Log Analytics](https://technet.microsoft.com/library/mt622052.aspx). Его нужно установить перед агентом зависимостей Linux.
2. Установите агент зависимостей Linux с правами привилегированного пользователя, используя следующую команду:<br>*sh ADM-Agent-Linux64.bin*.
3. Если агент зависимостей не запускается, просмотрите подробные сведения об ошибке в записях журналов. В агентах Linux каталог журнала находится в расположении */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Удаление агента зависимостей в Linux
Чтобы полностью удалить агент зависимостей в Linux, необходимо удалить сам агент и прокси-сервер, который автоматически устанавливается с агентом.  Удалить и то, и другое одновременно можно с помощью следующей команды:

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Установка из командной строки
В предыдущих разделах приводятся рекомендации по установке агента монитора зависимости с использованием параметров по умолчанию.  В этом разделе приведены рекомендации по установке агента из командной строки с использованием настраиваемых параметров.

#### <a name="windows"></a>Windows
Воспользуйтесь параметрами из приведенной ниже таблицы, чтобы выполнить установку из командной строки. Чтобы просмотреть список параметров установки, запустите установщик с помощью параметра /? следующим образом.

    ADM-Agent-Windows.exe /?

| Параметр | Description (Описание) |
|:--- |:--- |
| /S |Выполняет автоматическую установку без вывода сообщений для пользователя. |

Файлы для агента зависимостей Windows размещаются в папке *C:\Program Files\BlueStripe\Collector* по умолчанию.

#### <a name="linux"></a> Linux
Воспользуйтесь параметрами из приведенной ниже таблицы, чтобы выполнить установку. Чтобы просмотреть список параметров установки, запустите программу установки с помощью параметра -help следующим образом.

    ADM-Agent-Linux64.bin -help

| Параметр и описание |
|:--- |:--- |
| -s |
| --check |

Файлы для агента зависимостей размещаются в следующих каталогах.

| Файлы | Расположение |
|:--- |:--- |
| Основные файлы |/usr/lib/bluestripe-collector |
| Файлы журналов |/var/opt/microsoft/dependency-agent/log |
| Файлы конфигурации |/etc/opt/microsoft/dependency-agent/config |
| Исполняемые файлы службы |/sbin/bluestripe-collector<br>/sbin/bluestripe-collector-manager |
| Двоичные файлы хранилища |/var/opt/microsoft/dependency-agent/storage |

## <a name="troubleshooting"></a>Устранение неполадок
Если у вас возникли проблемы с монитором зависимостей приложения, вы можете собирать информацию об устранении неполадок из нескольких компонентов, используя приведенные ниже сведения.

### <a name="windows-agents"></a>Агенты Windows
#### <a name="microsoft-dependency-agent"></a>Агент зависимостей Microsoft
Для создания данных об устранении неполадок из агента зависимостей откройте командную строку от имени администратора и запустите скрипт CollectBluestripeData.vbs, используя приведенную ниже команду.  Вы можете добавить параметр --help, чтобы отобразить дополнительные параметры.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Пакет данных поддержки хранится в каталоге %USERPROFILE% для текущего пользователя.  Вы можете использовать параметр <filename> --file, чтобы сохранить его в другом расположении.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Пакет управления агента зависимостей Microsoft для MMA
Пакет управления агента зависимостей выполняется в MMA.  Он получает данные из агента зависимостей и перенаправляет их в облачную службу ADM.

Проверьте, скачивается ли пакет управления, сделав следующее:

1. Найдите файл с именем Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp в папке C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.  
2. Если файла нет или агент подключен к группе управления SCOM, убедитесь, что он импортирован в SCOM, проверив пакеты управления в рабочей области администрирования консоли управления.

Пакет управления ADM записывает события в журнал событий Windows Operations Manager.  Этот журнал можно [найти в OMS](../log-analytics/log-analytics-log-searches.md) с помощью решения системного журнала, где можно настроить, какие файлы журнала необходимо передать.  Если включены события отладки, они записываются в журнал событий приложения с источником события *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Чтобы собрать данные трассировки, откройте окно командной строки от имени администратора, а затем выполните следующие команды: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Данные трассировки записываются в папку c:\Windows\Logs\OpsMgrTrace.  Трассировку можно остановить с помощью команды StopTracing.cmd.

### <a name="linux-agents"></a>Агенты Linux
#### <a name="microsoft-dependency-agent"></a>Агент зависимостей Microsoft
Для создания данных об устранении неполадок из агента зависимостей войдите в учетную запись с правами sudo или привилегированного пользователя и выполните следующую команду.  Вы можете добавить параметр --help, чтобы отобразить дополнительные параметры.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Пакет данных поддержки хранится в расположении /var/opt/microsoft/dependency-agent/log (если с правами привилегированного пользователя) в каталоге установки агента или в домашнем каталоге пользователя, выполнившего скрипт (если это не привилегированный пользователь).  Вы можете использовать параметр <filename> --file, чтобы сохранить его в другом расположении.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Подключаемый модуль Fluentd агента зависимостей Microsoft для Linux
Подключаемый модуль Fluentd агента зависимостей выполняется в агенте OMS для Linux.  Он получает данные из агента зависимостей и перенаправляет их в облачную службу ADM.  

Журналы записываются в следующие два файла:

* /var/opt/microsoft/omsagent/log/omsagent.log;
* /var/log/messages.

#### <a name="oms-agent-for-linux"></a>Агент OMS для Linux
Ресурс для устранения неполадок для подключения серверов Linux к OMS можно найти здесь: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md). 

Журналы для агента OMS для Linux можно найти в папке */var/opt/microsoft/omsagent/log/*.  

Журналы для omsconfig (конфигурация агента) можно найти в папке */var/opt/microsoft/omsconfig/log/*.

Журнал для компонентов OMI и SCX, которые предоставляют данные метрик производительности, находятся в папках */var/opt/omi/log/* и */var/opt/microsoft/scx/log*.

## <a name="data-collection"></a>Сбор данных
Каждый агент передает примерно 25 МБ данных в день в зависимости от сложности зависимостей системы.  Они отправляют данные зависимостей ADM каждые 15 секунд.  

Агент ADM обычно потребляет 0,1 % системной памяти и 0,1 % ресурсов ЦП системы.

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы
В следующих разделах представлены поддерживаемые операционные системы для агента зависимостей.   32-разрядные версии операционных систем не поддерживаются.

### <a name="windows-server"></a>Windows Server
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2 с пакетом обновления 1

### <a name="windows-desktop"></a>Классические приложения
* Примечание. Windows 10 еще не поддерживается.
* Windows 8.1
* Windows 8
* Windows 7

### <a name="red-hat-enterprise-linux,-centos-linux-and-oracle-linux-(with-rhel-kernel)"></a>Red Hat Enterprise Linux, CentOS Linux и Oracle Linux (с ядром RHEL)
* Поддерживаются только версии ядра по умолчанию и SMP для Linux.
* Нестандартные версии ядра, такие как PAE и Xen, не поддерживаются ни в одном дистрибутиве Linux. Например, система со строкой версии "2.6.16.21-0.8-xen" не поддерживается.
* Пользовательские ядра, включая повторные компиляции стандартных ядер, не поддерживаются.
* Ядро CentOS Plus не поддерживается.
* Oracle Unbreakable Kernel (UEK) рассматривается в разделе ниже.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Версия ОС | Версия ядра |
|:--- |:--- |
| 7.0 |3.10.0-123 |
| 7.1. |3.10.0-229 |
| 7,2 |3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Версия ОС | Версия ядра |
|:--- |:--- |
| 6,0 |2.6.32-71 |
| 6.1 |2.6.32-131 |
| 6.2 |2.6.32-220 |
| 6.3 |2.6.32-279 |
| 6.4 |2.6.32-358 |
| 6,5 |2.6.32-431 |
| 6.6 |2.6.32-504 |
| 6.7 |2.6.32-573 |
| 6,8 |2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Версия ОС | Версия ядра |
|:--- |:--- |
| 5.8 |2.6.18-308 |
| 5.9 |2.6.18-348 |
| 5.10 |2.6.18-371 |
| 5.11 |2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w/-unbreakable-kernel-(uek)"></a>Oracle Enterprise Linux с Unbreakable Kernel (UEK)
#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Версия ОС | Версия ядра |
|:--- |:--- |
| 6.2 |Oracle 2.6.32-300 (UEK R1) |
| 6.3 |Oracle 2.6.39-200 (UEK R2) |
| 6.4 |Oracle 2.6.39-400 (UEK R2) |
| 6,5 |Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 |Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5
| Версия ОС | Версия ядра |
|:--- |:--- |
| 5.8 |Oracle 2.6.32-300 (UEK R1) |
| 5.9 |Oracle 2.6.39-300 (UEK R2) |
| 5.10 |Oracle 2.6.39-400 (UEK R2) |
| 5.11 |Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server
#### <a name="suse-linux-11"></a>SUSE Linux 11
| Версия ОС | Версия ядра |
|:--- |:--- |
| 11 |2.6.27 |
| 11 SP1 |2.6.32 |
| 11 SP2 |3.0.13 |
| 11 SP3 |3.0.76 |
| 11 SP4 |3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Версия ОС | Версия ядра |
|:--- |:--- |
| 10 SP4 |2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Данные диагностики и использования
Корпорация Майкрософт автоматически собирает данные об использовании и производительности с помощью службы монитора зависимости приложений. Эти данные используются, чтобы улучшить качество, повысить безопасность и целостность службы монитора зависимости приложений. Они включают в себя сведения о конфигурации программного обеспечения, такие как версия операционной системы, а также IP-адрес, DNS-имя и имя рабочей станции, чтобы предоставить возможности для точного и эффективного устранения неполадок. Корпорация Майкрософт не собирает сведения об именах, адресах и другие контактные сведения.

Дополнительные сведения о сборе и использовании данных см. в [заявлении о конфиденциальности служб Online Services корпорации Майкрософт](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, как [использовать монитор зависимости приложений](operations-management-suite-application-dependency-monitor.md) после его развертывания и настройки.

<!--HONumber=Oct16_HO2-->


