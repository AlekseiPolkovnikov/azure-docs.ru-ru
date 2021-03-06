---
title: "Импорт BACPAC-файла для создания базы данных SQL Azure | Документация Майкрософт"
description: "Создание базы данных SQL Azure посредством импорта существующего BACPAC-файла."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.devlang: NA
ms.date: 08/31/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26364edba47c4ac77b2125e067b99f87eefb434c


---
# <a name="import-a-bacpac-file-to-create-an-azure-sql-database"></a>Импорт BACPAC-файла для создания базы данных SQL Azure
**Отдельная база данных**

> [!div class="op_single_selector"]
> * [Портал Azure](sql-database-import.md)
> * [PowerShell](sql-database-import-powershell.md)
> * [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
> 
> 

В этой статье приведены указания по созданию базы данных SQL Azure из BACPAC-файла с помощью [портала Azure](https://portal.azure.com).

BACPAC-файл — это файл с расширением .bacpac, который содержит схему базы данных и данные. База данных создается на основе BACPAC-файла, импортированного из контейнера больших двоичных объектов хранилища Azure. Если в службе хранилища Azure нет BACPAC-файла, его можно создать, выполнив одно из действий, описанных в статье [Создание и экспорт BACPAC-файла из базы данных SQL Azure](sql-database-export.md).

> [!NOTE]
> База данных SQL Azure автоматически создает и обслуживает резервные копии для каждой пользовательской базы данных, которую можно восстановить. Дополнительные сведения см. в статье [Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure](sql-database-business-continuity.md).
> 
> 

Чтобы импортировать базу данных SQL из BACPAC-файла, необходимо следующее.

* Подписка Azure. 
* Сервер базы данных SQL Azure версии 12. Если у вас отсутствует сервер версии 12, создайте его в соответствии с инструкциями в следующей статье: [Создание первой базы данных SQL Azure](sql-database-get-started.md).
* BACPAC-файл базы данных, которую нужно импортировать, в контейнере больших двоичных объектов [учетной записи хранения Azure ("Стандартный")](../storage/storage-create-storage-account.md) .

> [!IMPORTANT]
> При импорте BACPAC-файла из хранилища BLOB-объектов используйте хранилище уровня "Стандартный". Импорт BACPAC-файла из хранилища уровня "Премиум" не поддерживается.
> 
> 

## <a name="select-the-server-to-host-the-database"></a>Выбор сервера для размещения базы данных
Откройте колонку SQL Server:

1. Перейдите на [портал Azure](https://portal.azure.com).
2. Выберите **Серверы SQL**.
3. Выберите сервер, на который нужно восстановить базу данных.
4. В колонке SQL Server щелкните **Импорт базы данных**, чтобы открыть колонку **Импорт базы данных**.
   
   ![Импорт базы данных][1]
5. Щелкните **Хранилище** и выберите учетную запись хранения, контейнер больших двоичных объектов и BACPAC-файл и нажмите кнопку **ОК**.
   
   ![настроить параметры хранилища][2]
6. Выберите уровень цен для новой базы данных и нажмите кнопку **Выбрать**. Импорт базы данных напрямую в пул эластичных БД не поддерживается, однако для начала можно импортировать ее в отдельную базу данных, а потом переместить ее в пул.
   
   ![выберите ценовую категорию][3]
7. Введите **имя базы данных** , создаваемой из BACPAC-файла.
8. Выберите тип аутентификации и предоставьте данные аутентификации для сервера. 
9. Нажмите кнопку **Создать** , чтобы создать базу данных из BACPAC-файла.
   
   ![создание базы данных][4]

Нажмите кнопку **Создать** , чтобы отправить в службу запрос об импорте базы данных. Операция импорта может занять некоторое время в зависимости от размера базы данных.

## <a name="monitor-the-progress-of-the-import-operation"></a>Отслеживание хода выполнения операции импорта
1. Выберите **Серверы SQL**.
2. Выберите сервер, на который нужно восстановить базу данных.
3. В колонке SQL Server в области "Операции" щелкните **Журнал импорта и экспорта**.
   
   ![журнал импорта и экспорта][5]
   ![журнал импорта и экспорта][6]

## <a name="verify-the-database-is-live-on-the-server"></a>Проверка активности базы данных на сервере
1. Щелкните **Базы данных SQL** и задайте для новой базы данных состояние **В сети**.

## <a name="next-steps"></a>Дальнейшие действия
* Чтобы научиться подключаться к импортированной базе данных SQL и отправлять к ней запросы, ознакомьтесь с разделом [Подключение к базе данных SQL с помощью SQL Server Management Studio и выполнение пробного запроса T-SQL](sql-database-connect-query-ssms.md)

<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png



<!--HONumber=Nov16_HO3-->


