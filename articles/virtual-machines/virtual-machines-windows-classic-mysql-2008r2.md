---
title: Создание виртуальной машины с MySQL | Microsoft Docs
description: Создайте виртуальную машину Azure под управлением Windows Server 2012 R2 и базу данных MySQL, используя классическую модель развертывания.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: cynthn

---
# Установка MySQL на виртуальной машине под управлением Windows Server 2012 R2, созданной с помощью классической модели развертывания.
[MySQL](http://www.mysql.com) является популярной базой данных SQL с открытым исходным кодом. В этом учебнике демонстрируются установка и запуск версии сообщества MySQL 5.6.23 в качестве сервера MySQL на виртуальной машине под управлением Windows Server 2012 R2. Инструкции по установке MySQL в Linux см. в разделе [Как установить MySQL в Azure](virtual-machines-linux-mysql-install.md).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## Создание виртуальной машины под управлением Windows Server 2012 R2
Если у вас еще нет виртуальной машины под управлением Windows Server 2012 R2, создайте виртуальную машину, ориентируясь на этот [учебник](virtual-machines-windows-classic-tutorial.md).

## Присоединение диска данных
После создания виртуальной машины можно подключить дополнительный диск с данными. Рекомендуется использовать его для рабочих нагрузок. Также это позволит гарантировать свободное пространство на диске с операционной системой (C:).

Ознакомьтесь со статьей [Подключение диска данных к виртуальной машине Windows](virtual-machines-windows-classic-attach-disk.md) и следуйте изложенным в ней указаниям по подключению пустого диска. Установите для параметра кэширования узла значение **Нет** или **Только для чтения**.

## Вход на виртуальную машину
Далее вам необходимо [войти в систему виртуальной машины](virtual-machines-windows-classic-connect-logon.md), чтобы установить MySQL.

## Установка и запуск MySQL Community Server на виртуальной машине
Выполните следующие действия для установки, настройки и запуска версии сервера MySQL для сообщества.

> [!NOTE]
> Эти действия относятся к MySQL версии 5.6.23.0 для сообщества и Windows Server 2012 R2. Рабочая процедура в разных версиях MySQL или Windows Server может отличаться.
> 
> 

1. После подключения к виртуальной машине с помощью удаленного рабочего стола щелкните элемент **Internet Explorer** на начальном экране.
2. Нажмите кнопку **Сервис** в правом верхнем углу (значок шестеренки) и выберите **Свойства браузера**. Откройте вкладку **Безопасность**, щелкните значок **Надежные сайты**, а затем нажмите кнопку **Сайты**. Добавьте сайт http://*.mysql.com в список надежных сайтов. Нажмите кнопку **Закрыть**, а затем кнопку **ОК**.
3. В адресной строке Internet Explorer введите http://dev.mysql.com/downloads/mysql/.
4. Используйте сайт MySQL, чтобы найти и скачать последнюю версию установщика MySQL для Windows. При выборе установщика MySQL скачайте версию, которая содержит полный набор файлов (например, mysql-installer-community-5.6.23.0.msi размером 282,4 МБ), и сохраните файл установщика.
5. После завершения загрузки установщика нажмите **Выполнить**, чтобы запустить установку.
6. На странице **License Agreement** (Лицензионное соглашение) примите лицензионное соглашение и нажмите кнопку **Next** (Далее).
7. На странице **Choosing a Setup Type** (Выбор типа установки) выберите тип установки и нажмите кнопку **Next** (Далее). В следующих шагах предполагается, что вы выбрали вариант установки **Server only** (Только для серверов).
8. На странице **Installation** (Установка) нажмите кнопку **Execute** (Выполнить). После завершения установки нажмите кнопку **Next** (Далее).
9. На странице **Product Configuration** (Конфигурация продукта) нажмите кнопку **Next** (Далее).
10. На странице **Type and Networking** (Тип и сеть) укажите требуемый тип конфигурации и параметры подключения, включая TCP-порт (если это необходимо). Щелкните **Показать дополнительные параметры**, а затем нажмите кнопку **Далее**.
    
    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)
11. На странице **Accounts and Roles** (Учетные записи и роли) укажите надежный пароль пользователя root MySQL. При необходимости добавьте дополнительные учетные записи пользователей MySQL и нажмите кнопку **Next** (Далее).
    
    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)
12. На странице **Windows Service** (Служба Windows) укажите изменения параметров по умолчанию, позволяющие запустить сервер MySQL в качестве службы Windows, и нажмите кнопку **Next ** (Далее).
    
    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)
13. На странице **Advanced Options** (Дополнительные параметры) внесите необходимые изменения в параметры ведения журнала и нажмите кнопку **Next** (Далее).
    
    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)
14. На странице **Apply Server Configuration** (Применить конфигурацию сервера) нажмите кнопку **Execute** (Выполнить). Завершив настройку, нажмите кнопку **Finish** (Готово).
15. На странице **Product Configuration** (Конфигурация продукта) нажмите кнопку **Next** (Далее).
16. На странице **Installation Complete** (Завершение установки) щелкните **Скопировать журнал в буфер обмена**, если хотите просмотреть его позднее, и нажмите кнопку **Готово**.
17. На начальном экране введите **mysql**, а затем щелкните **MySQL 5.6 Command Line Client** (Клиент командной строки MySQL 5.6).
18. Введите пароль пользователя root, указанный на шаге 11, чтобы появилось приглашение, где можно выполнить команды для настройки MySQL. Подробные сведения о командах и синтаксисе см. в [справочных руководствах по MySQL](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).
    
    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)
19. Можно также настроить для конфигурации сервера параметры по умолчанию, например каталоги базы и данных и диски, с записями в файле C:\\Program Files (x86)\\MySQL\\MySQL Server 5.6\\my-default.ini. Дополнительные сведения см. в разделе [Значения по умолчанию для конфигурации сервера 5.1.2](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## Настройка конечных точек
Если требуется, чтобы служба сервера MySQL была доступна для клиентских компьютеров MySQL в Интернете, необходимо настроить конечную точку для TCP-порта, который прослушивает служба сервера MySQL, и создать дополнительное правило брандмауэра Windows. Это порт TCP 3306, если только вы не указали другой порт, на странице **Тип и сеть** (шаг 10 предыдущей процедуры).

> [!NOTE]
> Необходимо тщательно рассмотреть возможные последствия с точки зрения безопасности, так как это сделает службы MySQL Server доступными для всех компьютеров в Интернете. Вы можете определить набор исходных IP-адресов, которым разрешено использовать конечную точку, с помощью списка управления доступом (ACL). Дополнительные сведения см. в разделе [Настройка конечных точек виртуальной машины](virtual-machines-windows-classic-setup-endpoints.md).
> 
> 

Чтобы настроить конечную точку для службы сервера MySQL, выполните следующие действия:

1. На классическом портале Azure щелкните **Виртуальные машины**, выберите имя виртуальной машины MySQL, а затем щелкните **Конечные точки**.
2. На панели команд нажмите кнопку **Добавить**.
3. На странице **Добавление конечной точки к виртуальной машине** нажмите стрелку вправо.
4. При использовании TCP-порта MySQL по умолчанию 3306 выберите **MySQL** в поле **Имя**, а затем щелкните флажок.
5. Если вы используете другой TCP-порт, введите уникальное имя в поле **Имя**. Выберите элемент **TCP** в столбце «Протокол», введите номер порта в обоих полях **Общий порт** и **Частный порт**, а затем щелкните флажок.

## Добавление правила брандмауэра Windows для разрешения трафика MySQL
Чтобы добавить правило брандмауэра Windows, которое разрешает трафик MySQL из Интернета, выполните следующую команду в командной строке Windows PowerShell с повышенными привилегиями на виртуальной машине сервера MySQL.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public



## Проверка удаленного подключения
Чтобы проверить удаленное подключение к службе сервера MySQL, запущенной на виртуальной машине Azure, необходимо сначала определить DNS-имя, соответствующее облачной службы, которая содержит эту виртуальную машину с сервером MySQL.

1. На классическом портале Azure щелкните **Виртуальные машины**, выберите имя виртуальной машины с сервером MySQL, а затем щелкните **Панель мониторинга**.
2. На панели мониторинга виртуальной машины запомните значение **DNS-имя** в разделе **Сводка**. Пример:
   
   ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)
3. На локальном компьютере, где запущен MySQL, или на клиенте MySQL выполните следующую команду для входа в качестве пользователя MySQL:
   
     mysql -u <yourMysqlUsername> -p -h <yourDNSname>
   
   Например, для имени пользователя MySQL dbadmin3 и DNS-имени testmysql.cloudapp.net для виртуальной машины используйте следующую команду:
   
     mysql -u dbadmin3 -p -h testmysql.cloudapp.net

## Дальнейшие действия
Дополнительные сведения о запуске MySQL см. в [документации по MySQL](http://dev.mysql.com/doc/).

<!---HONumber=AcomDC_0727_2016-->