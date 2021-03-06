---
title: "Циклы в хранилище данных SQL | Документация Майкрософт"
description: "Рекомендации по применению циклов Transact-SQL и замене курсоров в хранилище данных SQL Azure для разработки решений."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 41bb17ccd175506d4436eff985c52d46fa594576


---
# <a name="loops-in-sql-data-warehouse"></a>Циклы в хранилище данных SQL
Хранилище данных SQL позволяет использовать цикл [WHILE][WHILE] для повторяющихся блоков операторов. Цикл продолжается, пока не будут выполнены указанные условия или пока код не прервет цикл с помощью ключевого слова `BREAK` . Циклы особенно полезны для замены курсоров, определенных в коде SQL. К счастью почти все курсоры, записанные в коде SQL, относятся к разряду перемотки и доступности только для чтения. Циклы [WHILE] станут отличной альтернативой, если вам нужно заменить какой-то курсор.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Применение циклов и замена курсоров в хранилище данных SQL
Прежде чем обращаться к циклам, задайте себе следующий вопрос: "Можно ли переписать этот курсор, задействовав операции, ориентированные на работу с наборами данных?" Во многих случаях можно ответить утвердительно, и часто это будет лучшим вариантом. Операция, ориентированная на работу с набором данных, нередко выполняется намного быстрее, чем итеративный метод построчного перебора.

Курсоры быстрой перемотки, доступные только для чтения, легко заменяются циклической конструкцией. Ниже приведен простой пример. Приведенный в примере код обновляет статистику для каждой таблицы в базе данных. Перебор таблиц в цикле позволяет выполнить каждую команду в последовательности.

Для начала создайте временную таблицу с уникальным номером строки, обозначающим отдельные операторы:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Затем инициализируйте переменные, необходимые для выполнения цикла:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Теперь запустите цикл последовательного выполнения операторов:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

И, наконец, удалите временную таблицу, созданной на первом этапе:

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные советы по разработке см. в статье [общие сведения о разработке][общие сведения о разработке].

<!--Image references-->

<!--Article references-->
[общие сведения о разработке]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WHILE]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->



<!--HONumber=Nov16_HO3-->


