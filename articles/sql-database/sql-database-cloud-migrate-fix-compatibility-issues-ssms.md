---
title: "Устранение проблем совместимости базы данных SQL Server с помощью SQL Server Management Studio перед миграцией в базу данных SQL | Документация Майкрософт"
description: "База данных SQL Microsoft Azure, миграция базы данных, совместимость, мастер миграции SQL Azure"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5f7d3544-b07e-415a-a2ae-96e49bf5d756
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 355353fb15a00860573699cc652543b61c62c2c1


---
# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Устранение проблем совместимости базы данных SQL Server с помощью SQL Server Management Studio перед миграцией в базу данных SQL
> [!div class="op_single_selector"]
> * С помощью [мастера миграции SQL Azure (SAMW)](sql-database-cloud-migrate-fix-compatibility-issues.md)
> * Использование [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * Использование [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> 
> 

Опытные пользователи могут устранить проблемы совместимости базы данных SQL Server с помощью SQL Server Management Studio перед миграцией в базу данных SQL Azure.

> [!IMPORTANT]
> Чтобы обеспечить синхронизацию с обновлениями Microsoft Azure и Базой данных SQL, рекомендуется всегда использовать последнюю версию Management Studio. [Обновите среду SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="using-sql-server-management-studio"></a>Использование среды SQL Server Management Studio
Среда Management Studio позволяет устранять проблемы совместимости, используя команды Transact-SQL, например **ALTER DATABASE**. Этот метод в основном предназначен для опытных пользователей, которые уверенно используют Transact-SQL в действующей базе данных. В противном случае рекомендуется использовать SSDT. 

## <a name="next-steps"></a>Дальнейшие действия
* [Последняя версия SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Последняя версия SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
* [Перенос совместимой базы данных SQL Server в Базу данных SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Возможности базы данных SQL Azure](sql-database-features.md)
* [Частично или полностью неподдерживаемые функции Transact-SQL.](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Миграция баз данных не на основе SQL Server с помощью помощника по миграции SQL Server).](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->


