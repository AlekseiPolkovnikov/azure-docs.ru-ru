---
title: "Обзор правил брандмауэра базы данных SQL | Документация Майкрософт"
description: "Узнайте, как настроить брандмауэр базы данных SQL с помощью правил брандмауэра уровня сервера и базы данных для управления доступом."
keywords: "брандмауэр базы данных"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: ac57f84c-35c3-4975-9903-241c8059011e
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/23/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: ae1cacf0ff003e69a16d6beac48abc36a7f18896


---
# <a name="overview-of-azure-sql-database-firewall-rules"></a>Обзор правил брандмауэра базы данных SQL Azure 
> [!div class="op_single_selector"]
> * [Обзор](sql-database-firewall-configure.md)
> * [Портал Azure](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [ИНТЕРФЕЙС REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

База данных SQL Microsoft Azure предоставляет службу реляционных баз данных для Azure и других интернет-приложений. Чтобы защитить ваши данные, брандмауэр запрещает любой доступ к серверу базы данных, пока вы не укажете компьютеры, у которых есть разрешение на доступ. Брандмауэр предоставляет доступ к базам данным на основе исходного IP-адреса каждого запроса.

Для настройки брандмауэра можно создать правила брандмауэра, которые указывают диапазон допустимых IP-адресов. Можно создавать правила брандмауэра на уровне сервера и базы данных.

* **Правила брандмауэра серверного уровня:** эти правила разрешают клиентам доступ ко всему серверу Azure SQL Server, то есть ко всем базам данных на одном логическом сервере. Эти правила хранятся в базе данных **master** . Правила брандмауэра серверного уровня можно настроить на портале или с помощью инструкций Transact-SQL. Для создания правил брандмауэра на уровне сервера с помощью портала Azure или PowerShell необходимо быть владельцем или участником подписки. Чтобы создать правило брандмауэра на уровне сервера с помощью Transact-SQL, необходимо подключиться к экземпляру базы данных SQL от имени участника уровня сервера или администратора Azure Active Directory (это означает, что правило брандмауэра на уровне сервера изначально должен создать пользователь с разрешениями на уровне Azure).
* **Правила брандмауэра уровня базы данных.** Эти правила разрешают клиентам доступ к отдельным базам данных в пределах сервера Базы данных SQL Azure. Вы можете создать такие правила для каждой базы данных, и они будут храниться в отдельных базах данных. (Правила брандмауэра уровня базы данных можно создать для базы данных **master**.) Эти правила могут быть полезны для ограничения доступа к определенным (защищенным) базам данных в одном логическом сервере. Правила брандмауэра на уровне базы данных можно настроить только с помощью инструкций Transact-SQL.

**Рекомендация.** Корпорация Майкрософт рекомендует по возможности всегда использовать правила брандмауэра на уровне базы данных, чтобы повысить уровень безопасности и портативность базы данных. Используйте правила брандмауэра серверного уровня для администраторов, если у вас много баз данных с одинаковыми требованиями к доступу и вы не хотите тратить время на настройку каждой базы данных по отдельности.

> [!Note]
> Сведения о портативных базах данных в контексте непрерывности бизнес-процессов см. в разделе [Требования к проверке подлинности для аварийного восстановления](sql-database-geo-replication-security-config.md).
>

## <a name="firewall-overview"></a>Общие сведения о брандмауэре
Первоначально весь доступ к серверу Azure SQL Server с использованием Transact-SQ блокируется брандмауэром. Чтобы использовать Azure SQL Server, вам сначала нужно указать на портале Azure одно или несколько правил брандмауэра уровня сервера, которые обеспечивают доступ к этому серверу. Используйте правила брандмауэра, чтобы определить разрешенные диапазоны IP-адресов из Интернета. Кроме того, нужно указать, разрешено ли приложениям Azure пытаться подключиться к серверу Azure SQL Server.

Чтобы выборочно предоставить доступ только к одной из баз данных на сервере Azure SQL Server, необходимо создать правило уровня базы данных для соответствующей базы данных. В правиле брандмауэра уровня базы данных укажите диапазон IP-адресов, выходящий за пределы диапазона, указанного в правиле брандмауэра уровня сервера, и убедитесь, что IP-адрес клиента попадает в диапазон, указанный в правиле уровня базы данных.

Запросы на подключение из Интернета и Azure должны сначала обрабатываться брандмауэром и только потом достигать сервера Azure SQL Server или базы данных SQL, как показано на следующей схеме.

   ![Схема, описывающая конфигурацию брандмауэра.][1]

## <a name="connecting-from-the-internet"></a>Подключение через Интернет
Когда компьютер пытается подключиться к серверу базы данных из Интернета, брандмауэр проверяет исходный IP-адрес запроса относительно полного набора правил брандмауэра.

* Если IP-адрес запроса находится в одном из диапазонов, указанных в правилах брандмауэра уровня сервера, подключение предоставляется к серверу базы данных SQL Azure.
* Если IP-адрес запроса не находится в одном из диапазонов, указанных в правиле брандмауэра уровня сервера, проверяются правила брандмауэра уровня базы данных. Если IP-адрес запроса находится в одном из диапазонов, указанных в правилах брандмауэра уровня базы данных, подключение предоставляется только к базе данных, в которой есть соответствующее правило уровня базы данных.
* Если IP-адрес запроса находится за пределами диапазонов, указанных в любом из правил брандмауэра уровня сервера или уровня базы данных, запрос на подключение отклоняется.

> [!NOTE]
> Для доступа к базе данных SQL Azure с локального компьютера убедитесь, что брандмауэр сети и локального компьютера разрешает исходящие подключения по TCP-порту 1433.
> 

## <a name="connecting-from-azure"></a>Подключение из Azure
Чтобы приложения из Azure могли подключаться к серверу SQL Azure, необходимо разрешить подключения Azure. Когда приложение из Azure пытается подключиться к серверу базы данных, брандмауэр проверяет, разрешены ли подключения Azure. Параметр брандмауэра с начальным и конечным адресом, равным 0.0.0.0, указывает, что эти подключения разрешены. Если попытка подключения не разрешена, запрос не достигает сервера базы данных SQL Azure.

> [!IMPORTANT]
> Этот параметр позволяет настроить брандмауэр так, чтобы разрешить все подключения из Azure, включая подключения из подписок других клиентов. При выборе этого параметра убедитесь, что используемое имя для входа и разрешения пользователя предоставляют доступ только авторизованным пользователям.
> 

> [!NOTE]
>  Дополнительные сведения см. в разделе **Версия 12 базы данных SQL: внешняя и внутренняя программа** статьи [Порты, кроме 1433, для ADO.NET 4.5 и Базы данных SQL версии 12](sql-database-develop-direct-route-ports-adonet-v12.md).
>  

## <a name="creating-the-first-server-level-firewall-rule"></a>Создание первого правила брандмауэра уровня сервера
Первый параметр брандмауэра уровня сервера можно создать на [портале Azure](https://portal.azure.com/) или программным путем с помощью REST API либо Azure PowerShell. Последующие правила брандмауэра уровня сервера можно создавать и контролировать с помощью этих методов и Transact-SQL. Для повышения производительности правила брандмауэра на уровне сервера временно кэшируются на уровне базы данных. Сведения об обновлении кэша см. в статье [DBCC FLUSHAUTHCACHE (Transact-SQL)](https://msdn.microsoft.com/library/mt627793.aspx). Дополнительные сведения о правилах брандмауэра уровня сервера см. в статье [Настройка правила брандмауэра уровня сервера Базы данных SQL Azure с помощью портала Azure](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Правила брандмауэра уровня базы данных
После настройки первого брандмауэра уровня сервера можно ограничить доступ к определенным базам данных. Если в правиле брандмауэра уровня базы данных указать диапазон IP-адресов, выходящий за пределы диапазона, указанного в правиле брандмауэра уровня сервера, доступ к базе данных смогут получить только те клиенты, которые имеют IP-адреса в диапазоне уровня базы данных. Для базы данных можно задать не более 128 правил брандмауэра уровня базы данных. С помощью Transact-SQL можно создать и контролировать правила брандмауэра уровня базы данных для главной и пользовательской баз данных. Дополнительные сведения о настройке правил брандмауэра на уровне базы данных см. в статье [sp_set_database_firewall_rule (База данных SQL Azure)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Программное управление правилами брандмауэра
Помимо портала Azure правилами брандмауэра можно управлять программно с помощью Transact-SQL, REST API и Azure PowerShell. В приведенных ниже таблицах описан набор команд, доступных для каждого метода.

### <a name="transact-sql"></a>Transact-SQL
| Представление каталога или хранимая процедура | Уровень | Описание |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |сервер; |Отображает текущие правила брандмауэра уровня сервера |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |сервер; |Создает или обновляет правила брандмауэра уровня сервера |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |сервер; |Удаляет правила брандмауэра уровня сервера |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |База данных |Отображает текущие правила брандмауэра уровня базы данных |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |База данных |Создает или обновляет правила брандмауэра уровня базы данных |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Базы данных |Удаляет правила брандмауэра на уровне базы данных |

### <a name="rest-api"></a>ИНТЕРФЕЙС REST API
| API | Уровень | Описание |
| --- | --- | --- |
| [Вывод списка правил брандмауэра](https://msdn.microsoft.com/library/azure/dn505715.aspx) |сервер; |Отображает текущие правила брандмауэра уровня сервера |
| [Создание правила брандмауэра](https://msdn.microsoft.com/library/azure/dn505712.aspx) |сервер; |Создает или обновляет правила брандмауэра уровня сервера |
| [Задание правила брандмауэра](https://msdn.microsoft.com/library/azure/dn505707.aspx) |сервер; |Обновляет свойства существующего правила брандмауэра уровня сервера |
| [Удаление правила брандмауэра](https://msdn.microsoft.com/library/azure/dn505706.aspx) |сервер; |Удаляет правила брандмауэра уровня сервера |

### <a name="azure-powershell"></a>Azure PowerShell
| Командлет | Уровень | Описание |
| --- | --- | --- |
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) |сервер; |Возвращает текущие правила брандмауэра уровня сервера |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) |сервер; |Создает новое правило брандмауэра уровня сервера |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) |сервер; |Обновляет свойства существующего правила брандмауэра уровня сервера |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) |сервер; |Удаляет правила брандмауэра уровня сервера |

> [!NOTE]
> Задержка вступления в силу новых настроек брандмауэра может составить до пяти минут.
> 
> 

## <a name="troubleshooting-the-database-firewall"></a>Устранение неполадок брандмауэра базы данных
Если доступ к службе базы данных SQL Microsoft Azure не работает должным образом, необходимо учитывать следующее.

* **Конфигурация локального брандмауэра.** Прежде чем компьютер сможет получить доступ к Базе данных SQL Azure, может потребоваться создать исключение брандмауэра на компьютере для TCP-порта 1433. Вам может потребоваться открыть дополнительные порты при создании подключений в пределах облака Azure. Дополнительные сведения см. в разделе **Версия 12 Базы данных SQL: внешняя и внутренняя программа** статьи [Порты, кроме 1433, для ADO.NET 4.5 и Базы данных SQL версии 12](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Преобразование сетевых адресов (NAT):** из-за применения NAT IP-адрес, используемый компьютером для подключения к Базе данных SQL Azure, может отличаться от IP-адреса, показанного в параметрах конфигурации IP-адреса компьютера. Чтобы просмотреть IP-адрес, используемый компьютером для подключения к Azure, войдите на портал и откройте вкладку **Настройка** для сервера, где размещена база данных. В разделе **Разрешенные IP-адреса** отображается **текущий IP-адрес клиента**. Щелкните **Добавить** в разделе **Разрешенные IP-адреса**, чтобы разрешить компьютеру доступ к серверу.
* **Изменения в списке разрешенных адресов еще не вступили в силу:** до вступления в силу изменений конфигурации брандмауэра Базы данных SQL Azure может присутствовать задержка до пяти минут.
* **Имя для входа не авторизовано, или использован неправильный пароль.** Если имя для входа не имеет разрешений на сервере Базы данных SQL Azure или введен неправильный пароль, то подключение к серверу Базы данных SQL Azure отклоняется. Создание параметра брандмауэра только предоставляет клиентам возможность подключения к серверу. Каждый клиент должен предоставить необходимые учетные данные безопасности. Дополнительные сведения о подготовке имен входа см. в разделе "Управление базами данных, именами входа и пользователями в базе данных SQL Azure".
* **Динамический IP-адрес:** при наличии подключения к Интернету с динамическим предоставлением IP-адресов и возникновении проблем с прохождением через брандмауэр попробуйте одно из описанных ниже решений.
  
  * Попросите поставщика услуг Интернета (ISP) назначить диапазон IP-адресов тем клиентским компьютерам, с которых осуществляется доступ к серверу Базы данных SQL Azure, а затем добавьте диапазон IP-адресов в качестве правила брандмауэра.
  * Получите статические IP-адреса для клиентских компьютеров, а затем добавьте IP-адреса как правила брандмауэра.

## <a name="next-steps"></a>Дальнейшие действия
Практические руководства по созданию правил брандмауэра уровня сервера и уровня базы данных см. в следующих разделах.

* [Настройка правила брандмауэра уровня сервера базы данных SQL Azure с помощью портала Azure](sql-database-configure-firewall-settings.md)
* [Настройка правил брандмауэра уровня сервера базы данных SQL Azure и уровня базы данных SQL Azure с помощью T-SQL](sql-database-configure-firewall-settings-tsql.md)
* [Настройка правил брандмауэра уровня сервера базы данных SQL с помощью PowerShell](sql-database-configure-firewall-settings-powershell.md)
* [Практическое руководство. Настройка брандмауэра базы данных SQL Azure с помощью REST API](sql-database-configure-firewall-settings-rest.md)

Учебник по созданию базы данных SQL доступен в статье [Руководство по базам данных SQL: создание базы данных SQL за несколько минут с помощью портала Azure](sql-database-get-started.md).
Дополнительные сведения о подключении к базе данных SQL Azure из приложений с открытым кодом или приложений сторонних производителей см. в статье [Библиотеки подключений для базы данных SQL и SQL Server](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Общие сведения о навигации по базам данных см. в статье [Проверка подлинности и авторизация в базе данных SQL: предоставление доступа](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Защита базы данных](sql-database-security-overview.md)
* [Центр обеспечения безопасности для ядра СУБД SQL Server и базы данных Azure SQL](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png



<!--HONumber=Dec16_HO4-->


