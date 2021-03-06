---
title: Вызов настраиваемого интерфейса API из приложения логики
description: Использование настраиваемого интерфейса API, размещенного в службе приложений, с приложениями логики
author: stepsic-microsoft-com
manager: dwrede
editor: ''
services: logic-apps
documentationcenter: ''

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2016
ms.author: stepsic

---
# Использование настраиваемого интерфейса API, размещенного в службе приложений, с приложениями логики
Несмотря на то что в приложениях логики предусмотрен широкий набор соединителей (более 40), может возникнуть необходимость вызвать настраиваемый интерфейс API для выполнения собственного кода. С точки зрения простоты и поддержки масштабирования одним из самых удобных способов размещения собственного пользовательского веб-API является использование службы приложений. В этой статье рассматривается вызов веб-API, размещенного в веб-приложении службы приложений, веб-приложении или мобильном приложении.

Сведения о создании API в виде триггера или действия в приложении логики см. в [этой статье](app-service-logic-create-api-app.md).

## Развертывание веб-приложения
Прежде всего необходимо развернуть интерфейс API в качестве веб-приложения службы приложений. Инструкции по базовому развертыванию см. в статье [Создание веб-приложения ASP.NET](../app-service-web/web-sites-dotnet-get-started.md). Хотя из приложения логики можно вызвать любой интерфейс API, для получения наилучших результатов рекомендуется добавить метаданные Swagger для легкой интеграции с действиями приложений логики. Подробные сведения можно найти в разделе [Добавление метаданных Swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### Настройки API
Чтобы конструктор приложений логики проанализировал метаданные Swagger, важно включить CORS и установить свойства APIDefinition веб-приложения. Это легко сделать в портале Azure. Просто откройте колонку настроек веб-приложения и в разделе API установите в качестве значения параметра "Определение API" URL-адрес файла swagger.json (обычно это https://{name}.azurewebsites.net/swagger/docs/v1). Кроме того, добавьте политику CORS для "*", чтобы разрешить запросы от конструктора приложений логики.

## Вызов API
Если вы задали свойства CORS и свойства определения API на портале приложений логики, вы сможете легко добавить настраиваемые действия API для своего потока. В конструкторе вы можете выбрать для просмотра свои веб-сайты подписки, чтобы вывести список веб-сайтов с заданным URL-адресом Swagger. Также можно использовать действие "HTTP + Swagger", чтобы указать на Swagger и вывести список доступных действий и входных данных. Наконец, всегда можно создать запрос с помощью действия HTTP для вызова любых API, даже тех, которые не имеют или не предоставляют документа Swagger.

Однако если вы хотите защитить свой API, то можете воспользоваться одним из следующих способов:

1. Не требует изменения кода. Для защиты API без изменения кода или повторного развертывания можно использовать Azure Active Directory.
2. Активируйте в коде API обычную проверку подлинности, проверку подлинности AAD или проверку подлинности на основе сертификата.

## Защита вызовов API без изменения кода
В этом разделе вы создадите два приложения Azure Active Directory: одно для приложения логики, а другое – для веб-приложения. При вызовах к веб-приложению будет выполняться проверка подлинности с помощью субъекта-службы (идентификатора клиента и секрета), связанного с приложением AAD для вашего приложения логики. Вы также включите ИД приложения в определение приложения логики.

### Часть 1. Настройка удостоверения приложения для приложения логики
Приложение логики использует это удостоверение для проверки подлинности в Active Directory. Настройка выполняется для каталога *один раз*. Например, вы можете использовать одно удостоверение для всех приложений логики или создавать отдельные удостоверения для каждого приложения логики. Для настройки можно использовать пользовательский интерфейс и PowerShell.

#### Создание удостоверения приложения на классическом портале Azure
1. Перейдите к разделу [Active Directory на классическом портале Azure](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) и выберите каталог, который используется для веб-приложения.
2. Откройте вкладку **Приложения**.
3. Выберите команду **Добавить** на панели команд в нижней части страницы.
4. Присвойте удостоверению имя и нажмите стрелку «Далее».
5. Введите уникальную строку в формате домена в два текстовых поля и щелкните галочку.
6. Перейдите на вкладку **Настройка** приложения.
7. Скопируйте **идентификатор клиента**. Он будет использоваться в приложении логики.
8. В разделе **Ключи** щелкните **Выбрать продолжительность** и выберите 1 год или 2 года.
9. Нажмите кнопку **Сохранить** в нижней части экрана и подождите несколько секунд.
10. Скопируйте ключ в поле. Он также будет использоваться в вашем приложении логики.

#### Создание удостоверения приложения с помощью PowerShell
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Обязательно скопируйте **идентификатор тенанта**, **идентификатор приложения** и пароль.

### Часть 2. Защита веб-приложения с помощью удостоверения приложения AAD
Если вы уже развернули веб-приложение, то можете легко включить его защиту на портале. Также можно включить авторизацию в рамках развертывания с помощью диспетчера ресурсов Azure.

#### Включение авторизации на портале Azure
1. Перейдите в веб-приложение и щелкните **Параметры** на панели команд.
2. Щелкните **Авторизация и проверка подлинности**.
3. **Включите** ее.

На этом этапе приложение будет создано автоматически. Идентификатор клиента этого приложения потребуется в части 3, поэтому выполните следующие действия.

1. Перейдите в раздел [Active Directory на классическом портале Azure](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) и выберите свой каталог.
2. Найдите приложение, используя поле поиска.
3. Щелкните его в списке.
4. Откройте вкладку **Настройка**.
5. Вы увидите **идентификатор клиента**.

#### Развертывание веб-приложения с помощью шаблона диспетчера ресурсов Azure
Прежде всего необходимо создать приложение для вашего веб-приложения. Оно должно отличаться от приложения, которое используется приложением логики. Повторите действия, описанные в части 1, используя для **HomePage** и **IdentifierUris** фактический https://**URL** вашего веб-приложения.

> [!NOTE]
> При создании приложения для веб-приложения необходимо использовать [классический портал Azure](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), так как командлеты PowerShell не поддерживают настройку необходимых разрешений для входа пользователей на веб-сайт.
> 
> 

После получения идентификатора клиента и тенанта включите следующий код в шаблон развертывания в качестве вложенного ресурса веб-приложения.

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Чтобы автоматически развернуть пустое веб-приложение и приложение логики, которые используют AAD, нажмите следующую кнопку.

[![Развертывание в Azure](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Полный шаблон см. в статье [Вызовы настраиваемого интерфейса API, размещенного в службе приложений и защищенного AAD, из приложения логики](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).

### Часть 3. Заполнение раздела авторизации в приложении логики
В разделе **Авторизация** действия **HTTP** необходимо указать: `{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Элемент | Описание |
| --- | --- |
| type |Тип проверки подлинности. Для проверки подлинности ActiveDirectoryOAuth это значение равно ActiveDirectoryOAuth. |
| tenant |Идентификатор тенанта, используемый для идентификации тенанта AD. |
| audience |Обязательный параметр. Ресурс, к которому вы выполняете подключение. |
| clientID |Идентификатор клиента для приложения Azure AD. |
| secret |Обязательный параметр. Секретные данные клиента, запрашивающего маркер. |

В приведенном шаблоне эти параметры уже настроены. Однако если вы создаете приложение логики полностью самостоятельно, вам потребуется включить весь раздел авторизации.

## Защита API в коде
### Проверка подлинности с помощью сертификата
Вы можете использовать сертификаты клиента для проверки входящих запросов к веб-приложению. Сведения о настройке кода см. в статье [Настройка взаимной проверки подлинности TLS для веб-приложения](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

В разделе *авторизации* необходимо указать: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Элемент | Описание |
| --- | --- |
| type |Обязательный параметр. Тип проверки подлинности. Для клиентских SSL-сертификатов используйте значение ClientCertificate. |
| pfx |Обязательный параметр. Содержимое PFX-файла с кодировкой Base64. |
| password |Обязательный параметр. Пароль для доступа к PFX-файлу. |

### Обычная проверка подлинности
Вы можете использовать обычную проверку подлинности (например, имя пользователя и пароль) для проверки входящих запросов. Обычная проверка подлинности – это универсальный вариант, который подходит для приложений, написанных на разных языках программирования.

В разделе *Авторизация* необходимо указать: `{"type": "basic","username": "test","password": "test"}`.

| Элемент | Описание |
| --- | --- |
| type |Обязательный параметр. Тип проверки подлинности. Для обычной проверки подлинности используйте значение Basic. |
| username |Обязательный параметр. Имя пользователя для проверки подлинности. |
| password |Обязательный параметр. Пароль для проверки подлинности. |

### Обработка проверки подлинности AAD в коде
По умолчанию проверка подлинности Azure Active Directory, которую можно включить на портале, не обеспечивает детального уровня авторизации. Например, она разрешает использование API не отдельным пользователям или приложениям, а на уровне конкретного тенанта.

Если вы хотите предоставить доступ к API только приложению логики, то можете извлечь заголовок, который содержит токен JWT, и проверить, кто является вызывающей стороной. Все запросы, не удовлетворяющие требованиям к вызывающей стороне, можно отклонить.

Если вы хотите реализовать проверку подлинности полностью в коде, не используя портал, ознакомьтесь со статьей [Использование Active Directory для проверки подлинности в службе приложения Azure](../app-service-web/web-sites-authentication-authorization.md).

Для создания удостоверения приложения для вашего приложения логики и вызова API вам потребуется выполнить перечисленные выше шаги.

<!---HONumber=AcomDC_0803_2016-->