---
title: "Приступая к работе со службой интеграции журналов Azure | Документация Майкрософт"
description: "Узнайте, как установить службу интеграции журналов Azure и интегрировать журналы из службы хранилища Azure, журналы аудита Azure и оповещения центра безопасности Azure."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/24/2016
ms.author: TomSh
translationtype: Human Translation
ms.sourcegitcommit: d02a99406fd22f7578cb4825447f7710df374210
ms.openlocfilehash: 1ccd7d97596b3a74efcb9a066f42b04b62600fe3


---
# <a name="get-started-with-azure-log-integration-preview"></a>Приступая к работе со службой интеграции журналов Azure (предварительная версия)
Служба интеграции журналов Azure позволяет интегрировать необработанные журналы из ресурсов Azure с локальными системами SIEM (Security Information and Event Management). С помощью такой интеграции вы можете иметь доступ ко всем своим ресурсам, локальным или облачным, на единой панели мониторинга, что позволяет выполнять статистическую обработку, сопоставление и анализ, а также предупреждать о событиях безопасности, связанных с приложениями.

В этом учебнике рассматривается установка службы интеграции журналов Azure и интеграция журналов из службы хранилища Azure, журналов аудита Azure и оповещений центра безопасности Azure. Предполагаемое время выполнения заданий этого учебника: один час.
Для работы с этим руководством необходимо следующее:

* Компьютер (локальный или в облаке) для установки службы интеграции журналов Azure. На нем должны быть установлены 64-разрядная операционная система Windows и компонент .Net 4.5.1. Этот компьютер называется **Azlog Integrator**.
Подписка Azure. Если у вас нет подписки, вы можете зарегистрироваться для использования [бесплатной учетной записи](https://azure.microsoft.com/free/).
* Для ваших виртуальных машин Azure должна быть включена система диагностики Azure. Чтобы включить диагностику для облачных служб, ознакомьтесь с разделом [Включение системы диагностики Azure в облачных службах Azure](../cloud-services/cloud-services-dotnet-diagnostics.md). Чтобы включить диагностику для виртуальной машины Azure под управлением Windows, см. статью [Включение системы диагностики Azure на виртуальной машине под управлением Windows с помощью PowerShell](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Возможность подключения Azlog Integrator к службе хранилища Azure для аутентификации и авторизации пользователей при обращении к подписке Azure.
* Для интеграции журналов виртуальных машин Azure в Azlog Integrator необходимо установить агент SIEM (например, Splunk Universal Forwarder, агент HP ArcSight Windows Event Collector или IBM QRadar WinCollect).

## <a name="deployment-considerations"></a>Рекомендации по развертыванию
При высокой интенсивности событий можно запустить несколько экземпляров Azlog Integrator. Балансировка нагрузки учетных записей хранения системы диагностики Azure для Windows *(WAD)* и число подписок, предоставляемых для экземпляров, должны соответствовать нагрузке.

На компьютере с 8 процессорами (ядрами) один экземпляр Azlog Integrator может обрабатывать около 24 млн событий в день (примерно 1 млн в час).

На компьютере с 4 процессорами (ядрами) один экземпляр Azlog Integrator может обрабатывать около 1,5 млн событий в день (примерно 62,5 тыс. в час).

## <a name="install-azure-log-integration"></a>Установка интеграции журналов Azure
Скачайте компонент [интеграции журналов Azure](https://www.microsoft.com/download/details.aspx?id=53324).

Служба интеграции журналов Azure собирает данные телеметрии с компьютера, на котором она установлена.  Собираются следующие данные телеметрии:

* исключения, возникающие при выполнении интеграции журналов Azure;
* метрики о количестве обработанных запросов и событий;
* статистические данные использования параметров командной строки Azlog.exe.

> [!NOTE]
> Сбор данных телеметрии можно отключить, сняв этот флажок.
> 
> 


## <a name="set-your-azure-environment"></a>Настройка среды Azure
1. Откройте окно командной строки и с помощью команды **cd** перейдите в каталог **c:\Program Files\Microsoft Azure Log Integration**.
2. Выполните команду Set-AzLogAzureEnvironment -Name<Cloud>.
       
       Replace the Cloud with any of the following
       AzureCloud 
       AzureChinaCloud 
       AzureUSGovernment 
       AzureGermanCloud
       
       Note that at this time, an Azlog integrator only supports integrating logs from one cloud that you choose to integrate
       
## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Интеграция журналов виртуальных машин Azure из учетных записей хранения системы диагностики Azure
1. Прежде чем продолжить интеграцию журналов Azure, просмотрите предварительные требования, перечисленные выше, чтобы убедиться, что ваша учетная запись хранения WAD собирает журналы. Не выполняйте приведенные ниже действия, если ваша учетная запись хранения WAD не собирает журналы.
2. Откройте окно командной строки и с помощью команды **cd** перейдите в каталог **c:\Program Files\Microsoft Azure Log Integration**.
3. Выполните команду следующую команду.
   
        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>
   
      Замените StorageAccountName именем учетной записи хранения Azure, настроенной для получения событий диагностики из виртуальной машины.
   
        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==
   
      Если требуется отобразить идентификатор подписки в коде XML события, добавьте идентификатор подписки к понятному имени.
   
        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>
4. Подождите 30–60 минут (может потребоваться до часа), а затем просмотрите события, извлеченные из учетной записи хранения. Для этого в Azlog Integrator щелкните **Event Viewer (Просмотр событий) > Windows Logs (Журналы Windows) > Forwarded Events (Пересланные события)**.
5. Убедитесь в том, что стандартный соединитель SIEM, установленный на компьютере, настроен для извлечения событий из папки **Пересланные события** и передачи их в ваш экземпляр SIEM. Просмотрите конфигурацию SIEM, чтобы настроить и просмотреть параметры интеграции журналов.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Что делать, если данные не отображаются в папке "Пересланные события"?
Если через час в папке **Пересланные события** не появились данные, то сделайте следующее:

1. Проверьте компьютер и убедитесь, что он имеет доступ к Azure. Чтобы проверить возможность подключения, попробуйте открыть [портал Azure](http://portal.azure.com) в браузере.
2. Убедитесь, что учетная запись пользователя **azlog** имеет разрешение на запись в папку **users\azlog**.
3. Убедитесь, что учетная запись хранения, указанная в команде **azlog source add**, отображается при выполнении команды **azlog source list**.
4. Щелкните **Event Viewer (Просмотр событий) > Windows Logs (Журналы Windows) > Application (Приложение)**, чтобы проверить наличие ошибок, о которых сообщила служба интеграции журналов Azure.

Если вы по-прежнему не видите события, то:

1. Скачайте [Microsoft Azure Storage Explorer](http://storageexplorer.com/).
2. Подключитесь к учетной записи хранения, указанной в команде **azlog source add**.
3. В Microsoft Azure Storage Explorer перейдите к таблице **WADWindowsEventLogsTable** и проверьте наличие данных. Если их нет, значит, диагностика на виртуальной машине настроена неправильно.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Интеграция оповещений центра безопасности и журналов аудита Azure
1. Откройте окно командной строки и с помощью команды **cd** перейдите в каталог **c:\Program Files\Microsoft Azure Log Integration**.
2. Выполните команду следующую команду.
   
        azlog createazureid
   
      Эта команда запрашивает имя для входа Azure. Затем она создает [субъект-службу Azure Active Directory](../active-directory/active-directory-application-objects.md) в клиентах Azure AD, содержащих подписки Azure, для которых вошедший пользователь является администратором, соадминистратором или владельцем. Команда завершится ошибкой, если пользователь является только гостем в клиенте Azure AD. Аутентификация в Azure осуществляется через Azure Active Directory (AD).  Создание субъекта-службы для службы интеграции Azlog приводит к созданию удостоверения Azure AD, которому будет предоставлен доступ на чтение из подписок Azure.
3. Выполните команду следующую команду.
   
        azlog authorize <SubscriptionID>
   
      Она предоставляет субъекту-службе, созданному на шаге 2, доступ для чтения к подписке. Если не указать SubscriptionID, то команда попытается назначить субъекту-службе роль "Читатель" для всех подписок, к которым у вас есть какой-либо доступ.
   
        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328
   
   > [!NOTE]
   > Если выполнить команду **authorize** сразу же после команды **createazureid**, то могут появиться предупреждения. После создания учетная запись Azure AD становится доступной для использования с некоторой задержкой. Чтобы избежать таких предупреждений, следует подождать около 10 секунд после выполнения команды **createazureid** и лишь затем выполнять команду **authorize**.
   > 
   > 
4. Проверьте следующие папки, в которых должны находиться JSON-файлы журнала аудита:
   
   * **c:\Users\azlog\AzureResourceManagerJson;**
   * **c:\Users\azlog\AzureResourceManagerJsonLD.**
5. Проверьте следующие папки, в которых должны находиться оповещения центра безопасности:
   
   * **c:\Users\azlog\AzureSecurityCenterJson;**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD.**
6. Перенаправьте стандартный соединитель пересылки файлов SIEM в соответствующую папку, чтобы передавать данные экземпляру SIEM. Может потребоваться сопоставить некоторые поля согласно используемому продукту SIEM.

Если у вас есть вопросы о службе интеграции журналов Azure, отправьте электронное сообщение на адрес [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Дальнейшие действия
В этом учебнике вы узнали, как установить службу интеграции журналов Azure и интегрировать журналы из службы хранилища Azure. См. также:

* [Microsoft Azure Log Integration for Azure logs (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) (Служба интеграции журналов Microsoft Azure для журналов Azure (предварительная версия)) — посетите Центр загрузки, чтобы получить дополнительные сведения, изучить требования к системе и инструкции по установке службы интеграции журналов Azure.
* [Introduction to Microsoft Azure log integration (Preview)](security-azure-log-integration-overview.md) (Введение в службу интеграции журналов Azure (предварительная версия)) — в этом документе рассказывается о службе интеграции журналов Azure, ее основных возможностях и принципах работы.
* [Azure Log Integration SIEM configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Настройка SIEM для службы интеграции журналов Azure) — в этой записи блога показано, как настроить службу интеграции журналов Azure для работы с такими решениями партнеров, как Splunk, HP ArcSight и IBM QRadar.
* [Azure log integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Служба интеграции журналов Azure: часто задаваемые вопросы) — эта статья содержит ответы на часто задаваемые вопросы об интеграции журналов Azure.
* [Интеграция оповещений центра обеспечения безопасности с помощью интеграции журналов Azure (предварительная версия)](../security-center/security-center-integrating-alerts-with-log-integration.md) — в этом документе показано, как синхронизировать оповещения центра безопасности, а также события безопасности виртуальных машин, собранные системой диагностики Azure и в журналах аудита Azure, с решением Log Analytics или SIEM.
* [New features for Azure diagnostics and Azure Audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Новые возможности системы диагностики Azure и журналов аудита Azure) — в этой записи блога рассказывается о журналах аудита Azure и других функциях, которые помогут глубже понять, как работают ваши ресурсы Azure.




<!--HONumber=Nov16_HO5-->


