---
title: "Устранение проблем совместимости базы данных SQL Server перед миграцией в базу данных SQL | Документация Майкрософт"
description: "База данных SQL Microsoft Azure, миграция базы данных, совместимость, мастер миграции SQL Azure, SSDT"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7ff52877-5b63-4adc-aa1a-689669a1146e
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
ms.openlocfilehash: e09c60111286681928ee1dd0b08fade7a102d6f2


---
# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Использование SQL Server Data Tools для Visual Studio для переноса базы данных SQL Server в базу данных SQL Azure
> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

В этой статье вы узнаете, как найти и устранить проблемы совместимости базы данных SQL Server с помощью SQL Server Data Tools для Visual Studio, прежде чем выполнить миграцию в базу данных SQL Azure.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Использование SQL Server Data Tools для Visual Studio
SQL Server Data Tools для Visual Studio (SSDT) позволяет импортировать схему базы данных в проект базы данных Visual Studio для анализа. Для анализа укажите целевую платформу для проекта как Базу данных SQL версии 12, а затем соберите проект. Если построение выполнено успешно, база данных совместима. Если построение завершается неудачей, вы можете устранить ошибки SSDT (или одним из других инструментов, рассмотренных в этой статье). После успешного построения проекта вы можете опубликовать его в качестве копии базы данных-источника. Затем можно использовать функцию сравнения данных в SSDT, чтобы скопировать данные из базы данных-источника в базу данных, совместимую с SQL Azure версии 12. Затем вы можете выполнить миграцию обновленной базы данных. Чтобы использовать этот параметр, загрузите [последнюю версию SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![Схема переноса VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> Если требуется перенести только схему, можно опубликовать ее в Базе данных SQL Azure непосредственно из Visual Studio. Этот метод используется, если схема баз данных требует больше изменений, чем может обработать мастер миграции.
> 
> 

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Обнаружение проблем совместимости с помощью SQL Server Data Tools для Visual Studio
1. Откройте **обозреватель объектов SQL Server** в Visual Studio. Выберите **Добавить SQL Server** для подключения к экземпляру SQL Server, содержащему базу данных для переноса. Найдите базу данных в обозревателе объектов, щелкните ее правой кнопкой мыши и выберите команду **Создать новый проект…**     
   
   ![Новый проект](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
2. В настройках параметров импорта установите флажок **Импортировать только объекты области приложения**. Снимите флажки для импорта упоминаемых имен входа, разрешений и параметров базы данных.    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    
3. Нажмите кнопку **Начать** , чтобы приступить к импорту базы данных и создать проект, который будет содержать файл сценария T-SQL для каждого объекта в базе данных. Файлы сценариев вложены в папки проекта.    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    
4. В обозревателе решений Visual Studio щелкните правой кнопкой мыши проект базы данных и выберите "Свойства". На странице **Параметры проекта** необходимо настроить целевую платформу для базы данных SQL Microsoft Azure версии 12.    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
5. Щелкните проект правой кнопкой мыши и выберите пункт **Сборка** , чтобы выполнить сборку проекта.    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
6. **Список ошибок** отображает каждую несовместимость. В этом случае имя пользователя NT AUTHORITY\NETWORK SERVICE является несовместимым. Поскольку оно несовместимо, его можно закомментировать или удалить (и разрешить последствия удаления данного имени входа и роли из решения базы данных).     
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    

## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Устранение проблем совместимости с помощью SQL Server Data Tools для Visual Studio
1. Дважды щелкните первый скрипт, чтобы открыть сценарий в окне запроса, закомментировать его, а затем выполнить.     
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)
2. Повторяйте эту процедуру для каждого сценария, имеющего несовместимости, пока все ошибки не будут устранены.    
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
3. Устранив все ошибки, щелкните правой кнопкой мыши проект и выберите команду **Опубликовать**. Будет создана и опубликована копия базы данных-источника (настоятельно рекомендуется использовать именно копию, по крайней мере первоначально).     
   
   * В зависимости от версии SQL Server источника (более ранней, чем SQL Server 2014) перед публикацией может потребоваться сбросить целевую платформу проекта, чтобы обеспечить возможность развертывания.     
   * Если вы переносите старую базу данных SQL Server, не добавляйте в проект какие-либо функции, которые не поддерживаются исходным сервером SQL Server, до тех пор, пока база данных не перенесена в более новую версию SQL Server.     
     
     ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
     
     ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
4. В обозревателе объектов SQL Server щелкните правой кнопкой мыши базу данных и выберите пункт **Сравнение данных**. Сравнение проекта с исходной базой данных поможет вам понять, какие изменения были внесены с помощью мастера. Выберите базу данных Azure SQL версии 12 и нажмите кнопку **Готово**.    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    
5. Просмотрите обнаруженные различия и нажмите кнопку **Обновить цель** для переноса данных из базы данных-источника в базу данных Azure SQL версии 12.     
   
   ![замещающий текст](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
6. Выберите метод развертывания. Ознакомьтесь с разделом [Перенос совместимой базы данных SQL Server в Базу данных SQL](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Дальнейшие действия
* [Последняя версия SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Последняя версия SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Возможности базы данных SQL Azure](sql-database-features.md)
* [Частично или полностью неподдерживаемые функции Transact-SQL.](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant (Миграция баз данных не на основе SQL Server с помощью помощника по миграции SQL Server).](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Nov16_HO4-->


