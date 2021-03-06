---
title: "Разработка скриптов U-SQL с помощью средств Data Lake для Visual Studio | Документация Майкрософт"
description: "Сведения об установке средств озера данных для Visual Studio, разработке и тестировании скриптов U-SQL. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3ffeeb2e96510342abb0c0f8718273f41729cc2d


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Учебник. Приступая к работе с языком U-SQL для аналитики озера данных Azure
U-SQL — это язык, который объединяет преимущества SQL с эффективными возможностями вашего собственного кода для обработки всех данных любого масштаба. Масштабируемый распределенный запрос U-SQL позволяет эффективно анализировать данные в хранилище и в реляционных хранилищах, таких как Базы данных Azure SQL.  Он позволяет обрабатывать неструктурированные данные, применяя схему для чтения, вставки пользовательской логики и определяемых пользователем функций, и включает возможности расширения, обеспечивающие точный контроль над выполнением в масштабе. Дополнительные сведения о принципах разработки U-SQL см. в [записи блога Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Существуют некоторые отличия от ANSI SQL или T-SQL. Например, ключевые слова, такие как SELECT, должны быть из прописных букв.

Его система типов и язык выражения внутри предложения select, где предикаты и т. д. в C#.
Это означает, что типы данных — это типы C# и типы данных используют семантику C# NULL, а операции сравнения внутри предиката используют синтаксис C# (например, a == "foo").  Это также означает, что значения являются полными объектами .NET, что позволяет легко использовать любой метод для работы с объектом (например, "f o o o".Split(' ') ).

Дополнительные сведения см. в [справке по U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

### <a name="prerequisites"></a>Предварительные требования
Вам необходимо использовать [Учебник. Разработка скриптов U-SQL с помощью средств озера данных для Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

В этом учебнике выполняется задание аналитики озера данных с помощью следующего сценария U-SQL:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Этот сценарий не содержит действий преобразования. Он считывает данные из исходного файла **SearchLog.tsv**, схематизирует их и выводит набор строк в файл с именем **SearchLog-first-u-sql.csv**.

Обратите внимание на вопросительный знак рядом с типом данных поля «Длительность». Это означает, что поле «Длительность» может иметь значение null.

Некоторые основные понятия и ключевые слова в сценарии.

* **Переменные набора строк**: каждое выражение запроса, которое возвращает набор строк, может быть назначено переменной. U-SQL использует шаблон именования переменной T-SQL, например **@searchlog** , в сценарии.
    Обратите внимание, что назначение не обеспечивает принудительного выполнения. Оно просто именует выражение и дает возможность создавать более сложные выражения.
* **EXTRACT** дает возможность определить схему для чтения. Схема задается именем столбца и парой имен типа C# для каждого столбца. Она использует так называемое **средство извлечения**, например **Extractors.Tsv()**, для извлечения TSV-файлов. Можно разрабатывать пользовательские средства извлечения.
* **OUTPUT** принимает набор строк и сериализует его. Outputters.Csv() выводит файл с разделителями-запятыми в указанное место. Можно также разработать пользовательские средства вывода.
* Обратите внимание, что два пути являются относительными. Также можно использовать абсолютные пути.  Например:
  
        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
  
    Необходимо использовать абсолютный путь для доступа к файлам в связанных учетных записях хранения.  Для файлов, хранящихся в связанной учетной записи хранения Azure, используется следующий синтаксис:
  
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
  
  > [!NOTE]
  > Контейнер больших двоичных объектов Azure с разрешениями на доступ к общедоступным большим двоичным объектам или общедоступным контейнерам в настоящее время не поддерживается.
  > 
  > 

## <a name="use-scalar-variables"></a>Использование скалярных переменных
Скалярные переменные можно использовать также для упрощения обслуживания вашего сценария. Предыдущий сценарий U-SQL также можно написать следующим образом:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Преобразования наборов строк
Используйте **SELECT** для преобразования наборов строк:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Предложение WHERE использует [логическое выражение C#](https://msdn.microsoft.com/library/6a71f45d.aspx). Для создания собственных выражений и функций можно использовать язык выражений C#. Можно даже выполнить более сложную фильтрацию, сочетая их с помощью логического объединения (and) и разделения (or).

Следующий сценарий использует метод DateTime.Parse() и объединение.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Обратите внимание, что второй запрос работает на результатах первого набора строк и, таким образом, результатом является сочетание двух фильтров. Можно также повторно использовать имя переменной, и имена объединяются лексически.

## <a name="aggregate-rowsets"></a>Агрегированные наборы строк
U-SQL предоставляет знакомые предложения **ORDER BY**, **GROUP BY** и агрегаты.

Следующий запрос находит общую длительность по регионам, а затем выводит 5 результатов максимальной длительности по порядку.

Наборы строк U-SQL не сохраняют свой порядок для следующего запроса. Таким образом, чтобы упорядочить выходные данные, необходимо добавить предложение ORDER BY в OUTPUT, как показано ниже.

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Предложение ORDER BY в U-SQL необходимо использовать вместе с предложением FETCH в выражении SELECT.

Предложение HAVING в U-SQL можно использовать для ограничения выходных данных группами, которые удовлетворяют условиям HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Объединение данных
U-SQL предоставляет общие операторы объединения, такие как INNER JOIN, LEFT/RIGHT/FULL OUTER JOIN, SEMI JOIN, для объединения не только таблиц, но и любых наборов строк (даже тех, которые создаются из файлов).

Следующий сценарий объединяет searchlog с журналом впечатлений объявления и дает нам объявления для строки запроса для конкретной даты.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U-SQL поддерживает только синтаксис объединения, совместимый с ANSI: предикат Rowset1 JOIN Rowset2 ON. Старый синтаксис предиката FROM Rowset1, Rowset2 WHERE НЕ поддерживается.
Предикат в JOIN должен являться объединением равенства, а не выражением. Чтобы использовать выражение, добавьте его в предыдущее предложение SELECT набора строк. Если требуется выполнить различные сравнения, его можно переместить в предложение WHERE.

## <a name="create-databases-table-valued-functions-views-and-tables"></a>Создание баз данных, возвращающих табличные значения функций, представлений и таблиц
U-SQL позволяет использовать данные в контексте базы данных и схемы. Поэтому нет необходимости постоянно считывать или записывать файлы.

Каждый сценарий U-SQL выполняется с помощью базы данных по умолчанию (master) и схемы по умолчанию (dbo) в качестве контекста по умолчанию. Можно создать собственную базу данных или схему. Чтобы изменить контекст, используйте предложение **USE** .

### <a name="create-a-table-valued-function-tvf"></a>Создание возвращающих табличные значения функций
В предыдущем сценарии U-SQL мы повторно использовали EXTRACT для чтения из того же исходного файла. Возвращающая табличные значения функция U-SQL позволяет инкапсулировать данные для повторного использования в будущем.   

Следующий сценарий создает возвращающую табличное значение функцию с именем *Searchlog()* в базе данных и схеме по умолчанию:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Следующий сценарий показывает, как использовать возвращающую табличное значение функцию, определенную в предыдущем сценарии:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Создание представлений
Если имеется только одно выражение запроса, которое вы хотите выделить и не хотите параметрировать, можно создать представление вместо табличной функции.

Следующий сценарий создает представление с именем *SearchlogView* в базе данных и схеме по умолчанию:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Следующий сценарий демонстрирует использование определенного представления.

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>создание таблиц.
Подобно таблице реляционной базы данных, U-SQL позволяет создать таблицу с предварительно определенной схемой или же создать таблицу и вывести схему из запроса, который заполняет таблицу (CREATE TABLE AS SELECT или CTAS).

Следующий сценарий создает базу данных и две таблицы:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SearchLogDb;
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Запросы к таблицам
Вы можете отправлять запрос к таблицам (созданным в предыдущем сценарии) так же, как вы отправляете запрос к файлам данных. Вместо создания набора строк с помощью EXTRACT теперь можно непосредственно ссылаться на имя таблицы.

Сценарий преобразования, который использовался ранее, изменяется для чтения из таблиц:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Обратите внимание, что в настоящее время невозможно запустить SELECT для таблицы в том же сценарии, где создается таблица.

## <a name="conclusion"></a>Заключение
В этом учебнике рассматривается лишь небольшая часть U-SQL. В рамках данного учебника невозможно охватить все области, такие как:

* Использование CROSS APPLY для распаковки части строки, массивов и схем в строки.
* Использование секционированных наборов данных (наборов файлов и секционированных таблиц).
* Разработка определяемых пользователем операторов, таких как средства извлечения, средства вывода, процессоры, определяемые пользователем агрегаты в C#.
* Использование функций окон U-SQL.
* Управление кодом U-SQL с помощью представлений, возвращающих табличные значения функций и хранимых процедур.
* Выполнение произвольного пользовательского кода на узлах обработки.
* Подключения к базам данных SQL Azure и создание федерации запросов к ним и данным U-SQL и озера данных Azure.

## <a name="see-also"></a>Дополнительные материалы
* [Обзор аналитики озера данных Microsoft Azure](data-lake-analytics-overview.md)
* [Разработка скриптов U-SQL с помощью средств озера данных для Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Использование оконных функций U-SQL для заданий в службе аналитики озера данных Azure](data-lake-analytics-use-window-functions.md)
* [Мониторинг заданий аналитики озера данных Azure и устранение связанных с ними неполадок с помощью портала Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Сообщите нам свое мнение
* [Отправить запрос на функцию](http://aka.ms/adlafeedback)
* [Получить помощь на форумах](http://aka.ms/adlaforums)
* [Отправить отзыв о U-SQL](http://aka.ms/usqldiscuss)




<!--HONumber=Nov16_HO3-->


