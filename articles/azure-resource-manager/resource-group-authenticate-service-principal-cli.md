---
title: "Создание субъекта-службы с помощью интерфейса командной строки Azure | Документация Майкрософт"
description: "Использование командной строки Azure для создания приложения Active Directory и субъекта-службы с последующим предоставлением доступа к ресурсам с управлением доступом на основе ролей. В статье показано, как выполнять проверку подлинности приложения с помощью пароля или сертификата."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/30/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: 1fd051e2955ca48936b7bb977088d88b25fd76c6


---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Использование интерфейса командной строки Azure для создания субъекта-службы и доступа к ресурсам
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-authenticate-service-principal.md)
> * [Интерфейс командной строки Azure](resource-group-authenticate-service-principal-cli.md)
> * [Портал](resource-group-create-service-principal-portal.md)
> 
> 

При наличии приложения или скрипта, которому требуется доступ к ресурсам, вы, вероятно, не захотите, чтобы этот процесс осуществлялся под вашими учетными данными. Ваша учетная запись может иметь другие разрешения, необходимые для приложения, и использование ваших учетных данных приложением при изменении обязанностей будет нежелательным. Вместо этого для приложения можно создать удостоверение, содержащее учетные данные проверки подлинности и назначения ролей. При каждом запуске приложения выполняется автоматическая проверка подлинности с этими учетными данными. В этой статье показано, как настроить использование собственных учетных данных и удостоверения для приложения с помощью [интерфейса командной строки Azure (Azure CLI) для Mac, Linux и Windows](../xplat-cli-install.md) .

В Azure CLI доступно два варианта проверки подлинности для приложения AD:

* пароль
* на основе сертификата.

В этой статье показано, как применить оба варианта в Azure CLI. Если вам нужно выполнять вход в Azure с платформы для программирования (например, Python, Ruby или Node.js), мы советуем использовать проверку подлинности на основе пароля. Прежде чем решить, какой тип проверки подлинности использовать (сертификат или пароль), просмотрите примеры проверки подлинности на разных платформах в разделе [Примеры приложений](#sample-applications) .

## <a name="active-directory-concepts"></a>Основные понятия Active Directory
В этой статье вы создадите два объекта: приложение Active Directory (AD) и субъект-службу. Приложение AD является глобальным представлением вашего приложения. Оно содержит учетные данные (идентификатор приложения, а также пароль или сертификат). Субъект-служба — это локальное представление вашего приложения в Active Directory. Она содержит назначение роли. В этой статье описывается однотенантное приложение, используемое в пределах одной организации. Обычно однотенантная архитектура используется для создания бизнес-приложений в рамках организации. В однотенантном приложении содержится одно приложение AD и один субъект-служба.

Вы, возможно, зададитесь вопросом, зачем нужно использовать оба объекта. Этот подход более рационален, когда речь идет о мультитенантных приложениях. Обычно в качестве мультитенантного приложения используется приложение на базе модели "программное обеспечение как услуга" (SaaS). Такое приложение работает в нескольких разных подписках. Мультитенантное приложение содержит одно приложение AD и несколько субъектов-служб (по одной на каждую учетную запись Active Directory, предоставляющую доступ к приложению). Сведения о настройке мультитенантного приложения см. в статье [Управление ресурсами клиента с помощью Azure Active Directory и Resource Manager](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Необходимые разрешения
Для работы с этой статьей у вас должен быть достаточный уровень разрешений в подписке в Azure Active Directory и Azure. В частности, вы должны иметь право на создание приложения в Active Directory и назначение роли субъекту-службе. 

В Active Directory ваша учетная запись должна иметь права администратора (например, **глобального администратора** или **администратора пользователей**). Если вашей учетной записи назначена роль **пользователя** , вам нужно отправить запрос администратору на повышение уровня ваших разрешений.

Согласно вашей подписке в учетной записи должен быть доступ `Microsoft.Authorization/*/Write`, который предоставляется с ролью [владельца](../active-directory/role-based-access-built-in-roles.md#owner) или [администратора доступа пользователей](../active-directory/role-based-access-built-in-roles.md#user-access-administrator). Если вашей учетной записи назначена роль **участника** , вы получите сообщение об ошибке при попытке назначить роль субъекту-службе. Как и в предыдущем случае, администратор вашей подписки должен предоставить вам достаточный уровень разрешений для доступа.

Теперь перейдите к соответствующему разделу, чтобы выполнить проверку подлинности на основе [пароля](#create-service-principal-with-password) или [сертификата](#create-service-principal-with-certificate).

## <a name="create-service-principal-with-password"></a>Создание субъекта-службы с использованием пароля
В этом разделе содержатся инструкции, которые помогут вам создать приложение AD с использованием пароля и назначить роль читателя субъекту-службе.

Давайте пошагово выполним эти инструкции.

1. Войдите в свою учетную запись.
   
        azure login
2. Создать приложение AD можно двумя способами. Вы можете создать приложение AD и субъект-службу одновременно или отдельно. Создайте их одновременно, если для вашего приложения не нужно указывать универсальный код ресурса (URI) домашней страницы и URI для идентификации. Создайте их отдельно, если вам нужно задать эти значения для веб-приложения. В этом шаге описаны оба способа.
   
   * Чтобы создать приложение AD и субъект-службу одновременно, укажите имя приложения и введите пароль, как показано в следующей команде.
     
          azure ad sp create -n exampleapp -p {your-password}     
   * Чтобы создать приложение AD отдельно, укажите имя приложения, URI домашней страницы, URI для идентификации и введите пароль, как показано в следующей команде.
     
          azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>
     
       Эта команда возвращает значение AppId. Чтобы создать субъект-службу, укажите это значение в качестве параметра в следующей команде.
     
          azure ad sp create -a <AppId>
     
     Если у вашей учетной записи нет [необходимых разрешений](#required-permissions) для Active Directory, вы увидите сообщение об ошибке с текстом «Authentication_Unauthorized» (Аутентификация отклонена) или «No subscription found in the context» (В этом контексте подписки не обнаружены).
     
     Новый субъект-служба возвращается при использовании обоих способов. При предоставлении разрешений требуется **идентификатор объекта**. Идентификатор GUID, приведенный в **именах субъектов-служб**, требуется для входа в систему. Значение этого идентификатора такое же, как и значение идентификатора приложения. В примерах приложений, это значение называется **идентификатор клиента**. 
     
      info:    Executing command ad sp create
     
     * Creating application exampleapp / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+ data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e data:    Display Name:            exampleapp data:    Service Principal Names: data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4 data:                             https://www.contoso.org/example info:    ad sp create command OK
3. Предоставьте субъекту-службе разрешения на вашу подписку. В этом примере показано, как назначить субъекту-службе роль **читателя**, которая дает разрешение на чтение всех ресурсов в подписке. Сведения о других ролях см. в статье [RBAC: встроенные роли](../active-directory/role-based-access-built-in-roles.md). Для параметра **ServicePrincipalName** укажите значение **ObjectId**, которое использовалось при создании приложения. 
   
        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   
     Если у вашей учетной записи нет достаточных разрешений для назначения ролей, вы получите сообщение об ошибке. В нем будет указано, что у вашей учетной записи **нет разрешения на выполнение действия Microsoft.Authorization/roleAssignments/write в области /subscriptions/{guid}**. 

Вот и все! Приложение AD и субъект-служба настроены. В следующем разделе показано, как выполнить вход с использованием учетных данных с помощью Azure CLI. Если необходимо использовать учетные данные в коде приложения, выполнять действия, описанные ниже, не требуется. Вы можете перейти к разделу [Примеры приложений](#sample-applications) , где приведены примеры входа с использованием идентификатора приложения и пароля. 

### <a name="provide-credentials-through-azure-cli"></a>Предоставление учетных данных через Azure CLI
Теперь следует войти в систему от имени приложения для выполнения операций.

1. При каждом входе в приложение AD в качестве субъекта-службы необходимо указать идентификатор клиента каталога. Клиент — это экземпляр Active Directory. Чтобы получить идентификатор клиента для текущей аутентифицированной подписки, используйте следующую команду:
   
        azure account show
   
     Возвращаемые данные:
   
        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...
   
     Если необходимо получить идентификатор клиента другой подписки, используйте следующую команду:
   
        azure account show -s {subscription-id}
2. Если необходимо получить идентификатор клиента для входа в систему, используйте следующую команду.
   
        azure ad sp show -c exampleapp --json
   
     Значение, используемые для входа в систему, — это идентификатор GUID, приведенный в именах субъектов-служб.
   
        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]
3. Выполните вход как субъект-служба.
   
        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   
    Появится запрос на ввод пароля. Введите пароль, который вы задали при создании приложения AD.
   
        info:    Executing command login
        Password: ********

Теперь вы прошли проверку подлинности от имени субъекта-службы для созданного вами приложения.

## <a name="create-service-principal-with-certificate"></a>Создание субъекта-службы с использованием сертификата
В этом разделе содержатся инструкции, которые помогут вам:

* создать самозаверяющий сертификат;
* создать приложение AD с сертификатом и субъектом-службой;
* назначить роль "Читатель" субъекту-службе.

Для выполнения этих действий в системе должен быть установлен [OpenSSL](http://www.openssl.org/) .

1. Создать самозаверяющий сертификат.
   
        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
2. Объедините открытый и закрытый ключи.
   
        cat privkey.pem cert.pem > examplecert.pem
3. Откройте файл **examplecert.pem** и найдите длинную последовательность символов между тегами **-----BEGIN CERTIFICATE-----** и **-----END CERTIFICATE-----**. Скопируйте данные сертификата. Во время создания субъекта-службы эти данные передаются в качестве параметра.
4. Войдите в свою учетную запись.
   
        azure login
5. Создать приложение AD можно двумя способами. Вы можете создать приложение AD и субъект-службу одновременно или отдельно. Создайте их одновременно, если для вашего приложения не нужно указывать универсальный код ресурса (URI) домашней страницы и URI для идентификации. Создайте их отдельно, если вам нужно задать эти значения для веб-приложения. В этом шаге описаны оба способа.
   
   * Чтобы создать приложение AD и субъект-службу одновременно, укажите имя приложения и данные сертификата, как показано в следующей команде.
     
          azure ad sp create -n exampleapp --cert-value <certificate data>
   * Чтобы создать приложение AD отдельно, укажите имя приложения, URI домашней страницы, URI для идентификации и данные сертификата, как показано в следующей команде.
     
          azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>
     
       Эта команда возвращает значение AppId. Чтобы создать субъект-службу, укажите это значение в качестве параметра в следующей команде.
     
          azure ad sp create -a <AppId>
     
     Если у вашей учетной записи нет [необходимых разрешений](#required-permissions) для Active Directory, вы увидите сообщение об ошибке с текстом «Authentication_Unauthorized» (Аутентификация отклонена) или «No subscription found in the context» (В этом контексте подписки не обнаружены).
     
     Новый субъект-служба возвращается при использовании обоих способов. При предоставлении разрешений требуется идентификатор объекта. Идентификатор GUID, приведенный в **именах субъектов-служб**, требуется для входа в систему. Значение этого идентификатора такое же, как и значение идентификатора приложения. В примерах приложений, это значение называется **идентификатор клиента**. 
     
      info:    Executing command ad sp create
     
     * Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+ data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7 data:    Display Name:     exampleapp data:    Service Principal Names: data:                      4fd39843-c338-417d-b549-a545f584a745 data:                      https://www.contoso.org/example info:    ad sp create command OK
6. Предоставьте субъекту-службе разрешения на вашу подписку. В этом примере показано, как назначить субъекту-службе роль **читателя**, которая дает разрешение на чтение всех ресурсов в подписке. Сведения о других ролях см. в статье [RBAC: встроенные роли](../active-directory/role-based-access-built-in-roles.md). Для параметра **ServicePrincipalName** укажите значение **ObjectId**, которое использовалось при создании приложения. 
   
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   
     Если у вашей учетной записи нет достаточных разрешений для назначения ролей, вы получите сообщение об ошибке. В нем будет указано, что у вашей учетной записи **нет разрешения на выполнение действия Microsoft.Authorization/roleAssignments/write в области /subscriptions/{guid}**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Предоставление сертификата с помощью автоматизированного сценария для интерфейса командной строки Azure
Теперь следует войти в систему от имени приложения для выполнения операций.

1. При каждом входе в приложение AD в качестве субъекта-службы необходимо указать идентификатор клиента каталога. Клиент — это экземпляр Active Directory. Чтобы получить идентификатор клиента для текущей аутентифицированной подписки, используйте следующую команду:
   
        azure account show
   
     Возвращаемые данные:
   
        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...
   
     Если необходимо получить идентификатор клиента другой подписки, используйте следующую команду:
   
        azure account show -s {subscription-id}
2. Чтобы получить отпечаток сертификата и удалить ненужные символы, используйте такую команду:
   
        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   
     Возвращается значение отпечатка следующего вида:
   
        30996D9CE48A0B6E0CD49DBB9A48059BF9355851
3. Если необходимо получить идентификатор клиента для входа в систему, используйте следующую команду.
   
        azure ad sp show -c exampleapp
   
     Значение, используемые для входа в систему, — это идентификатор GUID, приведенный в именах субъектов-служб.
   
        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]
4. Выполните вход как субъект-служба.
   
        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

После этого субъект-служба для созданного вами приложения Active Directory пройдет проверку подлинности.

## <a name="sample-applications"></a>Примеры приложений
Следующие примеры приложений демонстрируют вход в качестве субъекта-службы.

**.NET**

* [Развертывание виртуальной машины с включенным протоколом SSH на основе шаблона с помощью .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
* [Управление ресурсами и группами ресурсов Azure с помощью .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

* [Приступая к работе с ресурсами: развертывание с помощью шаблона Azure Resource Manager на Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
* [Приступая к работе с ресурсами: управление группой ресурсов на Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

* [Развертывание виртуальной машины с включенным протоколом SSH на основе шаблона на Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
* [Управление ресурсами и группами ресурсов Azure с помощью Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

* [Развертывание виртуальной машины с включенным протоколом SSH на основе шаблона на Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
* [Управление ресурсами и группами ресурсов Azure с помощью Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

* [Развертывание виртуальной машины с включенным протоколом SSH на основе шаблона на Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
* [Управление ресурсами и группами ресурсов Azure с помощью Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Дальнейшие действия
* Подробные инструкции по интеграции приложения в Azure для управления ресурсами см. в [Управление ресурсами клиента с помощью Azure Active Directory и Resource Manager](resource-manager-api-authentication.md).
* Дополнительные сведения об использовании сертификатов и Azure CLI см. в статье [Certificate-based auth with Azure Service Principals from Linux command line](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx) (Проверка подлинности на основе сертификата для субъектов-служб Azure из командной строки Linux). 




<!--HONumber=Nov16_HO3-->


