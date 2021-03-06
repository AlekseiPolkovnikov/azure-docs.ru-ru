---
title: "Обзор протокола .NET Azure AD | Документация Майкрософт"
description: "В этой статье описывается использование HTTP-сообщений для авторизации в клиенте доступа к веб-приложениям и веб-интерфейсам API с использованием Azure Active Directory и OpenID Connect."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: f1ad2206f1b2da35d4a442beb91c5fcf7cbcddf6
ms.openlocfilehash: 37673952137b3fb7f4bca2759795ce1c96a9916d


---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Предоставление доступа к веб-приложениям с помощью OpenID Connect и Azure Active Directory
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) представляет простой уровень идентификации на основе протокола OAuth 2.0. OAuth 2.0 определяет механизмы получения и использования **маркеров доступа** для доступа к защищенным ресурсам, но не содержит стандартных методов для предоставления информации об идентификации. Аутентификация OpenID Connect является расширением процесса авторизации OAuth 2.0. Она предоставляет информацию о конечном пользователе в виде маркера `id_token`, который подтверждает идентификацию пользователя и предоставляет базовые сведения о его профиле.

Мы рекомендуем использовать OpenID Connect для веб-приложений, которые размещаются на сервере и доступны через веб-браузер.

[!INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]

## <a name="authentication-flow-using-openid-connect"></a>Поток проверки подлинности при использовании OpenID Connect
Наиболее простой процесс входа содержит следующие этапы. Каждый из них подробно описан ниже.

![Поток проверки подлинности OpenID Connect](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="send-the-sign-in-request"></a>Отправка запроса на вход
Если веб-приложению требуется проверить подлинность пользователя, оно может направить такого пользователя к конечной точке `/authorize` . Этот запрос похож на первый этап [потока кода авторизации OAuth 2.0](active-directory-protocols-oauth-code.md)с несколькими важными различиями:

* Запрос должен содержать область `openid` в параметре `scope`.
* Параметр `response_type` должен включать `id_token`.
* Запрос должен содержать параметр `nonce` .

Запрос в целом будет выглядеть примерно так.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Параметр |  | Описание |
| --- | --- | --- |
| tenant |обязательно |Значение `{tenant}` в пути запроса можно использовать для того, чтобы контролировать, кто может входить в приложение.  Допустимые значения — идентификаторы клиента, например `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` или `common` для маркеров без указания клиента. |
| client_id |обязательно |Идентификатор приложения, назначенный вашему приложению при регистрации в Azure AD. Это значение можно найти на классическом портале Azure. Щелкните **Active Directory**, выберите каталог и приложение, а затем щелкните **Настройка**. |
| response_type |обязательно |Должен включать `id_token` для входа в OpenID Connect.  Параметр также может содержать другие типы response_types, например `code`. |
| scope |обязательно |Список областей с разделителями-пробелами.  При использовании протокола OpenID Connect он должен содержать область `openid`, что преобразуется в разрешение "Вход" в пользовательском интерфейсе предоставления согласия.  Для предоставления разрешений этот запрос может также включать другие области. |
| nonce |обязательно |Значение, включенное в запрос и созданное приложением, которое войдет в состав маркера `id_token` в качестве утверждения.  Приложение может проверить это значение во избежание атак с использованием воспроизведения токена.  Это значение обычно представляет собой случайную уникальную строку или глобальный уникальный идентификатор, которые можно использовать для определения источника запроса. |
| redirect_uri |рекомендуется |URI перенаправления приложения, на который можно отправлять ответы проверки подлинности для их получения приложением.  Он должен в точности соответствовать одному из URI перенаправления, зарегистрированных на портале, но иметь форму закодированного URL-адреса. |
| response_mode |рекомендуется |Указывает метод, с помощью которого следует отправлять полученный код авторизации приложению.  Поддерживаются значения `form_post` для *формы HTTP POST* или `fragment` для *фрагмента URL-адреса*.  Для веб-приложений рекомендуется использовать `response_mode=form_post` , чтобы обеспечить наиболее безопасную передачу токенов в приложение. |
| state |рекомендуется |Значение, включенное в запрос, которое также возвращается в ответе токена.  Это может быть строка любого контента.  Как правило, для [предотвращения подделки межсайтовых запросов](http://tools.ietf.org/html/rfc6749#section-10.12)используется генерируемое случайным образом уникальное значение.  Параметр "state" также используется для кодирования информации о состоянии пользователя в приложении перед созданием запроса на проверку подлинности, например информации об открытой на тот момент странице или представлении. |
| prompt |необязательный |Указывает требуемый тип взаимодействия с пользователем.  На текущий момент единственные допустимые значения — login, none и consent.  При значении `prompt=login` пользователю придется вводить учетные данные по запросу. Единый вход не сработает.  Значение `prompt=none` является противоположным — оно гарантирует, что интерактивные запросы не будут выводиться ни при каких обстоятельствах.  Если запрос не удастся завершить автоматически с использованием единого входа, то конечная точка вернет ошибку.  Если установить значение `prompt=consent`, то после входа пользователь увидит диалоговое окно согласия OAuth с запросом на предоставление разрешений приложению. |
| login_hint |необязательный |Можно применять для предварительного заполнения поля имени пользователя или электронного адреса на странице входа пользователя (если имя пользователя известно заранее).  Зачастую этот параметр используется в приложениях при повторной проверке подлинности. При этом имя пользователя извлекается во время предыдущего входа с помощью утверждения `preferred_username`. |

На текущем этапе пользователю придется ввести учетные данные и завершить проверку подлинности.

### <a name="sample-response"></a>Пример ответа
Пример ответа после проверки подлинности пользователя может выглядеть так:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Параметр | Описание |
| --- | --- |
| id_token |Маркер `id_token`, запрошенный приложением. Вы можете использовать маркер `id_token` для идентификации пользователя и запуска сеанса с пользователем. |
| state |Значение, включенное в запрос, которое также возвращается в ответе токена. Обычно для [предотвращения подделки межсайтовых запросов](http://tools.ietf.org/html/rfc6749#section-10.12)применяется генерируемое случайным образом уникальное значение.  Параметр "state" также используется для кодирования информации о состоянии пользователя в приложении перед созданием запроса на проверку подлинности, например информации об открытой на тот момент странице или представлении. |

### <a name="error-response"></a>Сообщение об ошибке
Сообщения об ошибках также можно отправлять на `redirect_uri` , чтобы приложение обрабатывало их должным образом:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Параметр | Описание |
| --- | --- |
| error |Строка кода ошибки, которую можно использовать для классификации типов возникающих ошибок и реагирования на них. |
| error_description |Конкретное сообщение об ошибке, с помощью которого разработчик может определить причину возникновения ошибки проверки подлинности. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Коды ошибок конечной точки авторизации
В таблице ниже описаны различные коды ошибок, которые могут возвращаться в параметре `error` ответа с ошибкой.

| Код ошибки | Описание | Действие клиента |
| --- | --- | --- |
| invalid_request |Ошибка протокола, например отсутствует обязательный параметр. |Исправьте запрос и отправьте его повторно. Это ошибка разработки, которая, как правило, обнаруживается во время первоначального тестирования. |
| unauthorized_client |Клиентскому приложению не разрешено запрашивать код авторизации. |Как правило, это происходит, если клиентское приложение не зарегистрировано в Azure AD или не добавлено в клиент Azure AD пользователя. Приложение может отобразить пользователю запрос с инструкцией по установке приложения и его добавлению в Azure AD. |
| access_denied |Владелец ресурса отказал в использовании |Клиентское приложение может уведомить пользователя, что не может продолжить работу без согласия пользователя. |
| unsupported_response_type |Сервер авторизации не поддерживает тип ответа в запросе. |Исправьте запрос и отправьте его повторно. Это ошибка разработки, которая, как правило, обнаруживается во время первоначального тестирования. |
| server_error |Сервер обнаружил непредвиденную ошибку. |Повторите запрос. Эти ошибки могут возникать в связи с временными условиями. Клиентское приложение может отобразить для пользователя сообщение о том, что его ответ задерживается из-за временной ошибки. |
| temporarily_unavailable |Сервер временно занят и не может обработать запрос. |Повторите запрос. Клиентское приложение может отобразить для пользователя сообщение о том, что его ответ задерживается из-за временных условий. |
| invalid_resource |Целевой ресурс недопустим, так как он не существует. Azure AD не удается найти ресурс, или он настроен неправильно. |Это означает, что ресурс, если он существует, не настроен в клиенте. Приложение может отобразить пользователю запрос с инструкцией по установке приложения и его добавлению в Azure AD. |

## <a name="validate-the-idtoken"></a>Проверка токена "id_token"
Для аутентификации пользователя недостаточно просто получить маркер `id_token`. Вам также нужно проверить подпись маркера `id_token` и утверждения в нем на соответствие с требованиями приложения. В конечной точке Azure AD для подписи токенов и проверки их правильности используются веб-токены JSON (JWTs) и шифрование с открытым ключом.

Вы можете выбрать проверку `id_token` в клиентском коде, но обычно `id_token` отправляется на проверку внутреннему серверу. После проверки подписи маркера `id_token`вам следует проверить несколько утверждений.

Вам также может потребоваться проверить дополнительные утверждения в зависимости от сценария. Ниже приведены некоторые из стандартных проверок:

* Обеспечение регистрации пользователя или организации в приложении.
* Предоставление пользователю необходимого уровня авторизации и привилегий.
* Обеспечение определенного уровня проверки подлинности, например многофакторной проверки подлинности.

После полной проверки маркера `id_token` вы можете начать сеанс с пользователем, используя утверждения из маркера `id_token` для получения сведений о пользователе в приложении. Эти сведения можно использовать для отображения, записей, авторизации и т. д. Дополнительные сведения о типах маркеров и утверждениях см. в разделе [Справочник по токенам в Azure AD](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Отправка запроса на выход
Для выхода пользователя из приложения недостаточно очистить файлы cookie или каким-то другим образом завершить сеанс пользователя.  Чтобы пользователь вышел, его также необходимо перенаправить в `end_session_endpoint` .  Если этого не сделать, то пользователь сможет повторно пройти проверку подлинности в приложении без ввода учетных данных, поскольку для него сохранится действительный сеанс единого входа на конечной точке Azure AD.

Можно просто перенаправить пользователя на адрес, указанный в параметре `end_session_endpoint` в документе метаданных OpenID Connect:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Параметр |  | Описание |
| --- | --- | --- |
| post_logout_redirect_uri |рекомендуется |URL-адрес, на который пользователя следует перенаправить после успешного выхода.  Если он не указан, то пользователю будет показано универсальное сообщение. |

## <a name="token-acquisition"></a>Получение токена
Многим веб-приложениям требуется не только выполнить вход пользователя, но и получить доступ к веб-службе от лица этого пользователя с помощью OAuth. В этом сценарии OpenID Connect используется для аутентификации пользователей и одновременного получения кода `authorization_code`, который можно использовать для получения маркеров `access_tokens` с использованием потока кода авторизации OAuth.

## <a name="get-access-tokens"></a>Получение токенов доступа
Для получения маркера доступа необходимо немного изменить приведенный выше запрос на вход.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Если включить в запрос области разрешений и применить `response_type=code+id_token`, конечная точка `authorize` обеспечит, чтобы пользователь предоставил согласие на разрешения, перечисленные в параметре запроса `scope`, а затем возвратит приложению код авторизации в обмен на маркер доступа.

### <a name="successful-response"></a>Успешный ответ
Успешный ответ с использованием метода `response_mode=form_post` выглядит следующим образом:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Параметр | Описание |
| --- | --- |
| id_token |Маркер `id_token` , запрошенный приложением. Вы можете использовать маркер `id_token` для идентификации пользователя и запуска сеанса с пользователем. |
| Код |Запрашиваемый приложением код авторизации. Приложение может использовать код авторизации для запроса маркера доступа для целевого ресурса. Срок действия кодов авторизации крайне мал и обычно истекает по прошествии порядка 10 минут. |
| state |Если запрос содержит параметр "state", в ответе должно отображаться то же значение. Приложение должно проверить, совпадают ли значения параметра "state" в запросе и ответе. |

### <a name="error-response"></a>Сообщение об ошибке
Сообщения об ошибках также можно отправлять на `redirect_uri` , чтобы приложение обрабатывало их должным образом:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Параметр | Описание |
| --- | --- |
| error |Строка кода ошибки, которую можно использовать для классификации типов возникающих ошибок и реагирования на них. |
| error_description |Конкретное сообщение об ошибке, с помощью которого разработчик может определить причину возникновения ошибки проверки подлинности. |

Описания кодов ошибок и сведения о рекомендуемых действиях в клиенте см. в разделе [Коды ошибок конечной точки авторизации](#error-codes-for-authorization-endpoint-errors).

После получения авторизации `code` и `id_token` вы можете разрешить вход пользователя и получить токены доступа от его имени.  Для входа пользователя необходимо проверить маркер `id_token` , как описано выше. Чтобы получить маркеры доступа, необходимо выполнить действия, описанные в разделе "Использование кода авторизации для запроса маркера доступа" нашей [документации по протоколу OAuth](active-directory-protocols-oauth-code.md).



<!--HONumber=Dec16_HO4-->


