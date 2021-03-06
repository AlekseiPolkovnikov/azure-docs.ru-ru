---
title: "Восстановление хранилища данных SQL Azure (REST API) | Документация Майкрософт"
description: "Задачи REST API для восстановления хранилища данных SQL."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eed4442c5bd7a955e8d380bcb84cd62cd07eca05


---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Восстановление хранилища данных SQL Azure (REST API)
> [!div class="op_single_selector"]
> * [Обзор][Обзор]
> * [Портал][Портал]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Из этой статьи вы узнаете, как восстановить хранилище данных SQL Azure с помощью REST API.

## <a name="before-you-begin"></a>Перед началом работы
**Проверьте ресурсы DTU.**  Каждое хранилище данных SQL размещается на сервере SQL Server (например, myserver.database.windows.net), которому выделена квота DTU по умолчанию.  Перед восстановлением хранилища данных SQL убедитесь, что у сервера SQL Server осталась достаточная квота DTU для восстанавливаемой базы данных. Чтобы узнать, как вычислить необходимое количество DTU или запросить дополнительные единицы DTU, ознакомьтесь с разделом [Создание запроса в службу поддержки для хранилища данных SQL][Создание запроса в службу поддержки для хранилища данных SQL].

## <a name="restore-an-active-or-paused-database"></a>Восстановление активной или приостановленной базы данных
Процедура восстановления базы данных

1. Получите список точек восстановления базы данных с помощью операции Get Database Restore Points.
2. Начните восстановление с помощью операции [Create Database Restore Request][Create Database Restore Request].
3. Отследите состояние восстановления с помощью операции [Database Operation Status][Database Operation Status].

> [!NOTE]
> Чтобы настроить базу данных после восстановления, см. раздел [Настройка базы данных после восстановления][Настройка базы данных после восстановления].
> 
> 

## <a name="restore-a-deleted-database"></a>Восстановление удаленной базы данных.
Восстановление удаленной базы данных.

1. Создайте список всех удаленных баз данных, доступных для восстановления, с помощью операции [List Restorable Dropped Databases][List Restorable Dropped Databases].
2. Получите подробные сведения об удаленной базе данных, которую необходимо восстановить, с помощью операции [Get Restorable Dropped Database][Get Restorable Dropped Database].
3. Начните восстановление с помощью операции [Create Database Restore Request][Create Database Restore Request].
4. Отследите состояние восстановления с помощью операции [Database Operation Status][Database Operation Status].

> [!NOTE]
> Чтобы настроить базу данных после восстановления, см. раздел [Настройка базы данных после восстановления][Настройка базы данных после восстановления].
> 
> 

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о функциях, обеспечивающих непрерывность бизнес-процессов в выпусках базы данных SQL Azure, см. в статье [Обзор непрерывности бизнес-процессов в базе данных SQL Azure][Обзор непрерывности бизнес-процессов в базе данных SQL Azure].

<!--Image references-->

<!--Article references-->
[Обзор непрерывности бизнес-процессов в базе данных SQL Azure]: ../sql-database/sql-database-business-continuity.md
[Создание запроса в службу поддержки для хранилища данных SQL]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Настройка базы данных после восстановления]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Как установить и настроить Azure PowerShell]: ./powershell-install-configure.md
[Обзор]: ./sql-data-warehouse-restore-database-overview.md
[Портал]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create Database Restore Request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database Operation Status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get Restorable Dropped Database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List Restorable Dropped Databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[портала Azure]: https://portal.azure.com/
[Установщик веб-платформы Майкрософт]: https://aka.ms/webpi-azps



<!--HONumber=Nov16_HO3-->


