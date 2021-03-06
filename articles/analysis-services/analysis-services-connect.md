---
title: "Получение данных из служб Azure Analysis Services | Документация Майкрософт"
description: "Узнайте, как подключиться к серверу служб Analysis Services в Azure и получить от него данные."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/28/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 193c939065979dc48243d31e7f97cd87d96bf9a8
ms.openlocfilehash: 6b65689c75247af81e916730f125ff39bb49489f


---
# <a name="get-data-from-azure-analysis-services"></a>Получение данных из служб Azure Analysis Services
Когда вы создадите Azure сервер и развернете в него табличную модель, пользователи вашей организации смогут подключиться к нему для использования данных.

Службы Azure Analysis Services поддерживают подключение клиентов с помощью [обновленных объектных моделей](#client-libraries), включая ТОМ, объекты AMO, Adomd.Net или MSOLAP, для подключения к серверу с использованием XMLA. Например, Power BI, Power BI Desktop, Excel или любое стороннее клиентское приложение, которое поддерживает объектные модели.

## <a name="server-name"></a>имя сервера;
При создании сервера служб Analysis Services в Azure вам нужно указать уникальное имя сервера и регион, в котором он будет создан. При указании имени сервера в подключении используется следующая схема именования:

```
<protocol>://<region>/<servername>
```
 Здесь protocol — это строка **asazure**, region — URI-адрес региона, в котором был создан сервер (например, westus.asazure.windows.net для западной части США), а servername — это имя, уникальное в пределах указанного региона.

## <a name="get-the-server-name"></a>Получение имени сервера
Прежде чем подключаться к серверу, необходимо получить его имя. На **портале Azure** выберите сервер, а затем щелкните **Обзор** > **Имя сервера** и скопируйте имя сервера. Если к этому серверу подключаются и другие корпоративные пользователи, вы можете сообщить им это имя сервера. Указывая имя сервера, необходимо использовать полный путь.

![Получение имени сервера в Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="connect-in-power-bi-desktop"></a>Подключение в Power BI Desktop
> [!NOTE]
> Эта функция предоставляется в предварительной версии.
> 
> 

1. В [Power BI Desktop](https://powerbi.microsoft.com/desktop/) последовательно выберите **Получить данные** > **Базы данных** > **Azure Analysis Services**.
2. В поле **Сервер** вставьте из буфера обмена имя сервера.
3. В поле **База данных** вставьте имя базы данных табличной модели или перспективы, к которой нужно подключиться, если вы знаете это имя. В противном случае это поле можно оставить пустым. Базу данных или перспективу можно будет выбрать на следующем экране.
4. Оставьте стандартное значение **Подключение в реальном времени**, затем нажмите **Подключиться**. Если система предложит ввести учетную запись, введите учетную запись организации.
5. В разделе **Навигатор** разверните узел сервера и выберите модель или перспективу, к которой вы хотите подключиться, затем нажмите **Подключиться**. Щелкнув модель или перспективу, вы можете отобразить все объекты для этого представления.

## <a name="connect-in-power-bi"></a>Подключение в Power BI
1. Создайте файл Power BI Desktop с возможностью активного подключения к нужной модели на сервере.
2. В [Power BI](https://powerbi.microsoft.com) последовательно выберите **Получить данные** > **Файлы**. Найдите и выберите созданный файл.

## <a name="connect-in-excel"></a>Подключение в Excel
Подключиться к серверу служб Analysis Services Azure из Excel можно с использованием функции Get Data (в Excel 2016) или Power Query (в более ранних версиях). [Поставщик MSOLAP.7](https://aka.ms/msolap) является обязательным. Подключение с помощью мастера импорта таблиц в Power Pivot не поддерживается.

1. В Excel 2016 на ленте **Данные** последовательно выберите **Получить внешние данные** > **Из других источников** > **Из служб Analysis Services**.
2. В мастере подключения к данным вставьте имя сервера из буфера обмена в поле **Имя сервера**. Затем для параметра **Учетные данные** выберите значение **Использовать следующие имя пользователя и пароль**, введите имя пользователя, например nancy@adventureworks.com,, и пароль для него.
   
    ![Подключение в Excel: вход](./media/analysis-services-connect/aas-connect-excel-logon.png)
3. В разделе **Выбор базы данных и таблицы** выберите нужную базу данных, а также модель или перспективу, и нажмите кнопку **Готово**.
   
    ![Подключение в Excel: выбор модели](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Строка подключения
При подключении к службам Azure Analysis Services с помощью модели табличного объекта используйте следующие форматы строки подключения:

###### <a name="integrated-azure-active-directory-authentication"></a>Встроенная проверка подлинности Azure Active Directory
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Встроенная проверка подлинности извлечет данные из кэша учетных данных Azure Active Directory, если он доступен. В противном случае отобразится окно входа в Azure.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Проверка подлинности Azure Active Directory с использованием имени пользователя и пароля
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Клиентские библиотеки
При подключении к службам Analysis Services Azure из Excel или через другие интерфейсы (TOM, AsCmd, ADOMD.NET и пр.) может потребоваться установка последней версии клиентской библиотеки поставщика. Получите последнюю версию:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Дальнейшие действия
[Управление службами Analysis Services](analysis-services-manage.md)




<!--HONumber=Nov16_HO3-->


