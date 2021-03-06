---
title: "Авторизация учетных записей разработчиков с помощью Azure Active Directory в управлении API Azure"
description: "Узнайте, как авторизовать пользователей с помощью Azure Active Directory в управлении API"
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4d1b2f7b798e4cabfaa604358c2a380e8ed04fc6


---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Авторизация учетных записей разработчиков с помощью Azure Active Directory в управлении API Azure
## <a name="overview"></a>Обзор
В этом руководстве показывается, как включить доступ к порталу разработчика для всех пользователей в одной или нескольких службах каталогов Active Directory Azure. В этом руководстве также показано, как управлять группами пользователей Azure Active Directory путем добавления внешних групп, содержащих пользователей Azure Active Directory.

> Для выполнения шагов в этом руководстве вам сначала потребуется Azure Active Directory для создания приложения.
> 
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Авторизация учетных записей разработчиков с помощью Azure Active Directory
Чтобы начать работу, щелкните **Publisher portal** (Портал издателя) на портале Azure для службы управления API. Будет открыт портал издателя службы управления API.

![Портал издателя][api-management-management-console]

> Если экземпляр службы управления API еще не создан, выполните инструкции из раздела [Создание экземпляра управления API][Создание экземпляра управления API] в статье [Начало работы со службой управления Azure API][Приступая к работе со службой управления API].
> 
> 

В меню **Управление API** слева выберите пункт **Безопасность**, а затем перейдите на вкладку **Внешние удостоверения**.

![External Identities][api-management-security-external-identities]

В меню Azure Active Directory(Внешние удостоверения). Запишите **URL-адрес перенаправления** и переключитесь в Azure Active Directory на классическом портале Azure.

![External Identities][api-management-security-aad-new]

Нажмите кнопку **Добавить**, чтобы создать приложение Azure Active Directory, а затем установите флажок **Добавить приложение, разрабатываемое моей организацией**.

![Добавление нового приложения Azure Active Directory][api-management-new-aad-application-menu]

Введите понятное имя для приложения, установите флажок **Веб-приложение и/или веб-API**и нажмите кнопку «Далее».

![Новое приложение Azure Active Directory][api-management-new-aad-application-1]

В поле **URL-адрес входа**введите URL-адрес входа на свой портал разработчика. В данном примере **URL-адрес входа** — `https://aad03.portal.current.int-azure-api.net/signin`. 

Для **URL-адреса ИД приложения**введите домен по умолчанию или пользовательский домен для Azure Active Directory и добавьте к нему уникальную строку. В данном примере используется домен по умолчанию **https://contoso5api.onmicrosoft.com** с суффиксом **/api**.

![Свойства нового приложения Azure Active Directory][api-management-new-aad-application-2]

Нажмите кнопку с галочкой, чтобы сохранить и создать новое приложение, и переключитесь на вкладку **Настройка**, чтобы настроить новое приложение.

![Новое приложение Azure Active Directory создано][api-management-new-aad-app-created]

Если для этого приложения будут использоваться несколько служб каталогов Azure Active Directory, нажмите кнопку **Да** для параметра **Приложение является мультитенантным**. По умолчанию используется значение **Нет**.

![Приложение является мультитенантным][api-management-aad-app-multi-tenant]

Скопируйте **URL-адрес перенаправления** в разделе **Azure Active Directory** на вкладке **Внешние удостоверения** портала издателя и вставьте его в текстовое поле **URL-адрес ответа**. 

![URL-адрес ответа][api-management-aad-reply-url]

Прокрутите вкладку настройки вниз, выберите раскрывающийся список **Разрешения приложения** и установите флажок **Чтение данных каталога**.

![Разрешения приложения][api-management-aad-app-permissions]

Выберите раскрывающийся список **Делегирование разрешений** и установите флажок **Включить вход и чтение профилей пользователей**.

![Делегированные разрешения][api-management-aad-delegated-permissions]

> Дополнительные сведения о приложении и делегированных разрешениях см. в статье [Доступ к Graph API][Доступ к Graph API].
> 
> 

Скопируйте **идентификатор клиента** в буфер обмена.

![идентификатор клиента][api-management-aad-app-client-id]

Вернитесь на портал издателя и вставьте **идентификатор клиента** , скопированный из конфигурации приложения Azure Active Directory.

![Идентификатор клиента][api-management-client-id]

Вернитесь в конфигурацию Azure Active Directory, щелкните раскрывающийся список **Выбрать длительность** в разделе **Ключи** и укажите интервал. В данном примере используется интервал **1 год** .

![Ключ][api-management-aad-key-before-save]

Нажмите кнопку **Сохранить** , чтобы сохранить конфигурацию и отобразить ключ. Скопируйте этот ключ в буфер обмена.

> Запишите этот ключ. После закрытия окна конфигурации Azure Active Directory нельзя будет снова отобразить ключ.
> 
> 

![Ключ][api-management-aad-key-after-save]

Вернитесь на портал издателя и вставьте скопированный ключ в текстовое поле **Секрет клиента** .

![Секрет клиента][api-management-client-secret]

**Allowed Tenants** (Разрешенные клиенты) указывается, какие каталоги имеют доступ к API экземпляра службы управления API. Укажите домены экземпляров Azure Active Directory, к которым требуется предоставить доступ. Несколько доменов можно разделять символами начала новой строки, пробелами или запятыми.

![Allowed Tenants][api-management-client-allowed-tenants]

В разделе **Разрешенные клиенты** можно указывать несколько доменов. Прежде чем какой-либо пользователь сможет выполнить вход из домена, отличного от домена, в котором было зарегистрировано приложение, глобальный администратор этого другого домена должен предоставить разрешение для доступа приложения к данным каталога. Чтобы предоставить разрешение, глобальный администратор должен войти в приложение и нажать кнопку **Принять**. В следующем примере в раздел **Разрешенные клиенты** был добавлен домен `miaoaad.onmicrosoft.com`, и глобальный администратор этого домена впервые выполняет вход.

![Разрешения][api-management-permissions-form]

> Если администратор, не являющийся глобальным, попытается выполнить вход до того, как глобальный администратор предоставит разрешения, то попытка входа завершится неудачно и появится экран сообщения об ошибке.
> 
> 

После указания требуемой конфигурации нажмите кнопку **Сохранить**.

![Сохранить][api-management-client-allowed-tenants-save]

После сохранения изменений пользователи в указанном каталоге Azure Active Directory смогут входить на портал разработчика, выполняя действия, приведенные в разделе [Вход на портал разработчика с помощью учетной записи Azure Active Directory][Вход на портал разработчика с помощью учетной записи Azure Active Directory].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Добавление внешней группы Active Directory Azure
После включения доступа для пользователей в Azure Active Directory вы можете добавлять группы Azure Active Directory в управление API, чтобы упростить управление связью разработчиков в такой группе с требуемыми продуктами.

> Чтобы настроить внешнюю группу Azure Active Directory, необходимо сначала настроить Azure Active Directory на вкладке удостоверений, выполнив процедуру, описанную в предыдущем разделе. 
> 
> 

Внешние группы Azure Active Directory добавляются на вкладке **Видимость** продукта, к которому вы хотите предоставить доступ группе. Щелкните пункт **Продукты**, а затем щелкните имя нужного продукта.

![Настройка продукта][api-management-configure-product]

Переключитесь на вкладку **Видимость** и нажмите **Добавить группы из Azure Active Directory**.

![Добавление групп][api-management-add-groups]

Выберите в раскрывающемся списке пункт **Клиент Azure Active Directory**, а затем введите имя группы, которую нужно добавить, в текстовом поле **Группы**.

![Выбор группы][api-management-select-group]

Имя этой группы можно найти в списке **Группы** для Azure Active Directory, как показано в следующем примере.

![Список групп Azure Active Directory][api-management-aad-groups-list]

Нажмите кнопку **Добавить** , чтобы проверить имя группы и добавить ее. В данном примере добавляется внешняя группа **Разработчики Contoso 5** . 

![Группа добавлена][api-management-aad-group-added]

Нажмите кнопку **Сохранить** , чтобы сохранить выбор новой группы.

После настройки группы Azure Active Directory в одном продукте ее можно включать на вкладке **Видимость** для других продуктов в этом экземпляре службы управления API.

Чтобы просмотреть и настроить свойства внешних групп после их добавления, щелкните имя группы на вкладке **Группы** .

![Управление группами][api-management-groups]

Здесь можно изменить **имя** и **описание** группы.

![Изменение группы][api-management-edit-group]

Пользователи из настроенных служб каталогов Azure Active Directory могут входить на портал разработчика, а затем просматривать и подписываться на любые группы, которые они могут видеть, выполняя инструкции из следующего раздела.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Вход на портал разработчика с помощью учетной записи Azure Active Directory
Чтобы войти на портал разработчика с помощью учетной записи Azure Active Directory, настроенной в предыдущих разделах, откройте новое окно браузера с помощью **URL-адреса входа** из конфигурации приложения Active Directory и нажмите кнопку **Azure Active Directory**.

![Портал разработчика][api-management-dev-portal-signin]

Введите учетные данные одного из пользователей в Azure Active Directory и нажмите кнопку **Вход**.

![Вход][api-management-aad-signin]

Может появиться запрос с регистрационной формой, если требуются какие-либо дополнительные сведения. Заполните эту регистрационную форму и нажмите кнопку **Регистрация**.

![Регистрация][api-management-complete-registration]

Теперь этот пользователь вошел на портал разработчика для вашего экземпляра службы управления API.

![Регистрация завершена][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[Как добавлять операции в API]: api-management-howto-add-operations.md
[Как создать и опубликовать продукт]: api-management-howto-add-products.md
[Мониторинг и аналитика]: api-management-monitoring.md
[Добавление интерфейсов API к продукту]: api-management-howto-add-products.md#add-apis
[Публикация продукта]: api-management-howto-add-products.md#publish-product
[Приступая к работе со службой управления API]: api-management-get-started.md
[Справочник по политикам службы управления API]: api-management-policy-reference.md
[Политики кэширования]: api-management-policy-reference.md#caching-policies
[Создание экземпляра управления API]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Доступ к Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Предварительные требования]: #prerequisites
[Настройка сервера авторизации OAuth 2.0 в управлении API]: #step1
[Настройка API для авторизации пользователей по протоколу OAuth 2.0]: #step2
[Тестирование авторизации пользователей по протоколу OAuth 2.0 на портале разработчика]: #step3
[Дальнейшие действия]: #next-steps

[Вход на портал разработчика с помощью учетной записи Azure Active Directory]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account




<!--HONumber=Nov16_HO3-->


