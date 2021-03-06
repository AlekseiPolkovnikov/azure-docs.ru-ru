---
title: "Разработка определяемых пользователем операторов U-SQL для заданий Azure Data Lake Analytics | Документация Майкрософт"
description: "Узнайте, как разрабатывать определяемые пользователем операторы (с возможностью повторного использования) для заданий аналитики озера данных. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 73d3e5577d0702a93b7f4edf3bf4e29f55a053ed
ms.openlocfilehash: 88a00c2b0a5aac85bbcaef5b21b10f44121c7d38


---
# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Разработка определяемых пользователем операторов U-SQL для заданий аналитики озера данных Azure
Узнайте, как разрабатывать определяемые пользователем операторы (с возможностью повторного использования) для заданий аналитики озера данных. Вам предстоит разработать пользовательский оператор для преобразования названий стран.

## <a name="prerequisites"></a>Предварительные требования
* Visual Studio 2015, Visual Studio 2013 с обновлением 4 или Visual Studio 2012 с установленным Visual C++.
* Пакет SDK Microsoft Azure для .NET (версии 2.5 или выше).  Его можно установить с помощью установщика веб-платформы.
* Учетная запись аналитики озера данных.  [Начало работы с аналитическим модулем озера данных Azure при помощи портала Azure](data-lake-analytics-get-started-portal.md).
* Пройдите руководство [Приступая к работе с U-SQL Studio для аналитики озера данных Azure](data-lake-analytics-u-sql-get-started.md) .
* Подключение к Azure.
* Загрузите исходные данные. Ознакомьтесь с руководством [Приступая к работе с U-SQL Studio для аналитики озера данных Azure](data-lake-analytics-u-sql-get-started.md). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Определение и использование пользовательских операторов в U-SQL
**Создание и отправка задания U-SQL**

1. В меню **Файл** выберите команду **Создать**, а затем — **Проект**.
2. Выберите тип **Проект U-SQL** .

    ![новый проект U-SQL в Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Нажмите кнопку **ОК**. Visual Studio создает решение с помощью файла Script.usql.
4. В **обозревателе решений** разверните узел Script.usql и дважды щелкните файл **Script.usql.cs**.
5. Скопируйте приведенный ниже код и вставьте его в файл.

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Откройте файл Script.usql и вставьте следующий сценарий U-SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. В **обозревателе решений** щелкните правой кнопкой мыши файл **Script.usql** и выберите команду **Build Script** (Создать сценарий).
8. В **обозревателе решений** щелкните правой кнопкой мыши файл **Script.usql** и выберите команду **Submit Script** (Отправить сценарий).
9. Если вы еще не подключились к своей подписке Azure, вам будет предложено ввести учетные данные Azure.
10. Нажмите кнопку **Submit**(Отправить). Итоги отправки и ссылка на задание появятся в окне результатов, когда операция отправки будет завершена.
11. Чтобы увидеть текущее состояние задания, нажмите кнопку «Обновить».

**Просмотр выходных данных задания**

1. В **обозревателе сервера** разверните узлы **Azure**, **Data Lake Analytics**, а также учетную запись Data Lake Analytics. Затем разверните узел **Учетные записи хранения**, щелкните хранилище по умолчанию правой кнопкой мыши и выберите **Обозреватель**.
2. Разверните узлы «Примеры» и «Выходные данные», а затем дважды щелкните **Drivers.csv**.

## <a name="see-also"></a>Дополнительные материалы
* [Приступая к работе с аналитикой озера данных с помощью PowerShell](data-lake-analytics-get-started-powershell.md)
* [Приступая к работе с аналитикой озера данных с помощью портала Azure](data-lake-analytics-get-started-portal.md)
* [Использование инструментов озера данных для Visual Studio для разработки приложений U-SQL](data-lake-analytics-data-lake-tools-get-started.md)



<!--HONumber=Nov16_HO3-->


