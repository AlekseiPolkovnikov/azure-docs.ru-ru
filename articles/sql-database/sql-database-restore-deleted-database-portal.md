---
title: "Восстановление удаленной базы данных SQL Azure (портал Azure) | Документация Майкрософт"
description: "Восстановление удаленной базы данных SQL Azure (портал Azure)."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 33b0c9e6-1cd2-4fd9-9b0d-70ecf6e54821
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.date: 10/12/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b5a3f40ff8811c08d130c2a3e0a7d61aed9457d1


---
# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Восстановление удаленной базы данных SQL Azure на портале Azure
> [!div class="op_single_selector"]
> * [Обзор](sql-database-recovery-using-backups.md)
> * [**Восстановление удаленной базы данных: портал**](sql-database-restore-deleted-database-portal.md)
> * [Восстановление удаленной базы данных: PowerShell](sql-database-restore-deleted-database-powershell.md)
> 
> 

## <a name="select-the-database-to-restore"></a>Выбор базы данных для восстановления
Чтобы восстановить удаленную базу данных на портале Azure, сделайте следующее:

1. На [портале Azure](https://portal.azure.com) щелкните **Больше служб** > **Серверы SQL Server**.
2. Выберите сервер с базой данных, которую требуется восстановить.
3. Прокрутите вниз до раздела **Операции** колонки сервера и выберите **Удаленные базы данных**. ![Восстановление базы данных SQL Azure](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
4. Выберите базу данных, которую требуется восстановить.
5. Укажите имя базы данных и нажмите кнопку **ОК**.
   
   ![Восстановление базы данных SQL Azure](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)

## <a name="next-steps"></a>Дальнейшие действия
* Сведения об обеспечении непрерывности бизнес-процессов и возможные сценарии описаны в [обзоре непрерывности бизнес-процессов](sql-database-business-continuity.md)
* Чтобы узнать об автоматически создаваемых резервных копиях базы данных SQL Azure, ознакомьтесь с разделом [Общие сведения об автоматическом резервном копировании базы данных SQL](sql-database-automated-backups.md)
* Чтобы узнать об использовании автоматически создаваемых резервных копий для восстановления, ознакомьтесь с [восстановлением базы данных из резервных копий, инициируемых службой](sql-database-recovery-using-backups.md)
* Чтобы узнать о более быстрых вариантах восстановления, ознакомьтесь с [активной георепликацией](sql-database-geo-replication-overview.md)  
* Чтобы узнать об использовании автоматически создаваемых резервных копий для архивации, ознакомьтесь с [копированием базы данных](sql-database-copy.md)




<!--HONumber=Nov16_HO3-->


