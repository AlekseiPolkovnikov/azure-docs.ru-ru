---
title: "Приступая к работе с Azure AD для Cordova | Документация Майкрософт"
description: "Практическое руководство по созданию приложения Cordova, которое интегрируется с Azure AD для входа в систему и вызывает программные интерфейсы приложения, защищенные Azure AD, по протоколу OAuth."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 09/16/2016
ms.author: vittorib
translationtype: Human Translation
ms.sourcegitcommit: 1865043ca9c9019b9813f11eb4a55f7f16d79287
ms.openlocfilehash: e4f70db9a5b1675c44af0e106bd6252c243ae0b7


---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Интеграция Azure AD с приложением Apache Cordova
[!INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova позволяет разрабатывать приложения на HTML5 и JavaScript, которые могут работать на мобильных устройствах как полноценные приложения.
Microsoft Azure Active Directory позволяет разработчику добавить в приложение Cordova возможности проверки подлинности уровня предприятия. Благодаря подключаемому модулю Cordova, являющимся оболочкой пакетов SDK Azure AD для нативных приложений IOS, Android, Магазина Windows и Windows Phone, разработчик может расширить функции приложения, реализовав вход пользователей с их учетными данными Active Directory, получив доступ к интерфейсам Office 365 и Azure API, а также защитив вызовы собственного веб-API.

В данном руководстве мы будем использовать подключаемый модуль Apache Cordova для библиотеки проверки подлинности Active Directory (ADAL) с целью расширения простого приложения следующими возможностями:

* Добавив всего нескольких строк кода, можно проверить подлинность пользователя Active Directory и получить маркер для вызова Graph API Azure AD.
* Использовать этот маркер, чтобы вызвать Graph API для запроса сведений из каталога и отображения результатов.  
* Использовать кэш маркеров библиотеки ADAL, чтобы минимизировать количество запросов пользователя на ввод данных для проверки подлинности.

Для этого вам необходимо:

1. Зарегистрировать приложение в Azure AD.
2. Добавить в приложение код для запроса маркеров.
3. Добавить код, использующий маркер для запросов Graph API и отображения результатов.
4. Создать проект развертывания Cordova на всех целевых платформах, а также подключаемый модуль Cordova ADAL и протестировать решение в эмуляторах.

## <a name="0----prerequisites"></a>*0.    Предварительные требования*
Для работы с данным руководством вам потребуется:

* Клиент Azure AD, где имеется учетная запись с правами на разработку приложений.
* Среда разработки, настроенная для использования Apache Cordova.  

При наличии и того и другого приступайте непосредственно к выполнению этапа 1.

Если у вас еще нет клиента Azure AD, см. [инструкции по его получению](active-directory-howto-tenant.md).

Если на компьютере не установлена платформа Apache Cordova, установите следующие продукты:

* [Git.](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [NodeJS](https://nodejs.org/download/)
* [Cordova CLI](https://cordova.apache.org/) (можно легко установить через диспетчер пакетов NPM: `npm install -g cordova`).

Обратите внимание, что эти продукты работают и на ПК, и на Mac.

Каждая целевая платформа имеет свои требования.

* Чтобы собрать и запустить версию приложения для планшета/ПК под управлением Windows или Windows Phone, требуется:
  * [Visual Studio 2013 для Windows с обновлением 2 или более поздней версии](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express или другая версия).
* Чтобы собрать и запустить приложение для iOS, требуется:

  * Xcode 5.x или более поздняя версия. Скачайте его по адресу http://developer.apple.com/downloads или из [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * [ios-sim](https://www.npmjs.org/package/ios-sim) — позволяет запускать приложения iOS в симуляторе iOS из командной строки (можно легко установить через терминал: `npm install -g ios-sim`).
* Чтобы собрать и запустить приложение для Android, требуется:

  * Установить [пакет средств разработки Java (JDK) версии 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) или более поздней. Убедитесь, что `JAVA_HOME` (переменная среды) указывает путь установки JDK (например, C:\Program Files\Java\jdk1.7.0_75).
  * Установить [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) и добавить расположение `<android-sdk-location>\tools` (например, C:\tools\Android\android-sdk\tools) в переменную среды `PATH`.
  * Открыть диспетчер Android SDK (например, через терминал: `android`) и установить:
  * *Android 5.0.1 (API уровня 21)* ;
  * *Android SDK Build-tools* версии 19.1.0 или более поздней;
  * *Android Support Repository* (дополнительно).

  Android SDK по умолчанию не содержит эмулятор. Если необходимо запустить приложение Android в эмуляторе, создать эмулятор можно, выполнив команду `android avd` из терминала, а затем нажав кнопку *Create...* (Создать). Рекомендуемый *уровень API* — 19 или выше. Дополнительные сведения об эмуляторе Android и параметрах создания см. в статье [Create and Manage Virtual Devices](http://developer.android.com/tools/help/avd-manager.html) (Создание и управление виртуальными устройствами).

## <a name="1----register-an-application-with-azure-ad"></a>*1.    Регистрация приложения в Azure AD*
Следует отметить, что **этот этап не является обязательным**. В руководстве представлены заранее подготовленные значения, позволяющие наблюдать работу примера без какой-либо подготовки в вашем клиенте. Однако все же рекомендуется пройти данный этап и ознакомиться с процессом, так как это потребуется вам при создании собственных приложений.

Azure AD предоставляет маркеры только известным приложениям. Прежде чем использовать службу Azure AD из приложения, вы должны создать для него запись в собственном клиенте.  Чтобы зарегистрировать новое приложение в клиенте:

1. Войдите на [портал Azure](https://portal.azure.com).
2. В правом верхнем углу щелкните учетную запись и в списке **Каталог** выберите клиент Active Directory, где есть разрешения администратора.
3. В фильтре поиска введите **регистрация приложений**.
4. Щелкните **Регистрация приложений** и нажмите кнопку **Добавить**.
5. Следуя инструкциям на экране, создайте **собственное клиентское приложение**. Несмотря на то что приложения Cordova основаны на HTML, мы создаем собственное клиентское приложение, поэтому параметр `Native Client Application` должен быть выбран. В противном случае приложение не будет работать. Введите понятное имя для приложения и выберите Native в качестве типа приложения. **URI перенаправления** — это универсальный код ресурса (URI), который используется для возврата маркеров приложению, например `http://MyDirectorySearcherApp`. Щелкните **Создать**, чтобы создать приложение.
6. На портале Azure выберите приложение, щелкните **Параметры** и выберите **Свойства**.
7. Найдите значение идентификатора приложения и скопируйте его в буфер обмена.
8. Настройте разрешения для приложения. В меню "Параметры" выберите раздел "Необходимые разрешения", щелкните **Добавить**, а затем — **Выбор API** и выберите Windows Azure Active Directory (это API AADGraph). Затем щелкните **Выбор разрешений** и выберите Access the directory as the signed-in user (Доступ к каталогу от имени пользователя, выполнившего вход).

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. Клонирование примера репозитория приложений, необходимого для данного учебника*
Введите следующую команду из оболочки или командной строки:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Создание приложения Cordova*
Существует несколько способов создания приложений Cordova. В данном руководстве используется интерфейс командной строки Cordova (CLI).
Введите следующую команду из оболочки или командной строки:

     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Команда создаст структуру папок и шаблон для проекта Cordova, скопировав содержимое начального проекта во вложенную папку www.
Перейдите в новую папку DirSearchClient.

    cd .\DirSearchClient

Добавьте подключаемый модуль whitelist, необходимый для вызова Graph API.

     cordova plugin add cordova-plugin-whitelist

Затем добавьте все платформы, которые необходимо поддерживать. Чтобы получить работающий образец, необходимо выполнить хотя бы одну из приведенных ниже команд. Следует отметить, что вы не сможете выполнить эмуляцию iOS в Windows или Windows/Windows Phone в Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Наконец, добавьте в проект библиотеку ADAL для подключаемого модуля Cordova.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. Добавление кода проверки подлинности пользователей и получение маркеров из Azure AD*
Приложение, которое разрабатывается в данном руководстве, предоставляет базовую функцию поиска в каталоге, где конечный пользователь может ввести псевдоним любого пользователя в каталоге и просмотреть некоторые его основные атрибуты.  Начальный проект содержит определение базового пользовательского интерфейса приложения (в файле www/index.html) и шаблона, который реализует циклы обработки основных событий приложения, привязку интерфейса пользователя и логику отображения результатов (в файле www/js/index.js). Единственное, что остается сделать вам, — это добавить логику, реализующую задачи по работе с удостоверениями.

Первое, что необходимо сделать, — это вставить в код значения протокола, используемые Azure AD для идентификации вашего приложения и целевых ресурсов. Позже эти значения будут использоваться для создания запросов маркеров. Вставьте следующий фрагмент в самое начала файла index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Значения `redirectUri` и `clientId` должны совпадать со значениями, описывающими приложение в Azure AD. Их можно найти на вкладке "Настройка" на портале Azure, как описано на этапе 1 данного руководства.
Примечание: если вы решили не регистрировать новое приложение в собственном клиенте, то можно просто вставить вышеупомянутые предварительно настроенные значения, как они есть, что позволит вам увидеть работу образца, хотя вам всегда придется указывать собственные значения для производственных приложений.

Далее необходимо добавить код фактического запроса маркера. Вставьте следующий фрагмент кода между определениями `search ` и `renderdata `.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Рассмотрим эту функцию, разбив ее на две основные части.
Данный образец предназначен для работы с любым клиентом, в отличие от того, который привязывается к определенному клиенту. Он использует конечную точку "/common", которая позволяет пользователю ввести данные любой учетной записи во время проверки подлинности и направляет запрос соответствующему клиенту.
Первая часть метода проверяет кэш ADAL на наличие хранимых маркеров, и, если таковые имеются, то используется клиент, от которого поступил данный маркер, для повторной инициализации ADAL. Необходимо избегать дополнительных запросов к пользователю, поскольку использование конечной точки "/common" всегда приводит к просьбе пользователю ввести новые данные учетной записи.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Вторая часть метода выполняет непосредственно запрос маркера.
Метод `acquireTokenSilentAsync` запрашивает ADAL вернуть маркер для указанного ресурса без отображения диалогового окна ввода учетных данных. Диалоговое окно не отображается, если в кэше уже имеется подходящий хранимый маркер доступа или, если существует маркер обновления, который может использоваться для получения нового токена доступа без запроса пользователя.
Если получить действительный маркер не удается, будет вызвана функция `acquireTokenAsync` , которая через диалоговое окно запросит у пользователя данные для проверки подлинности.

```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Теперь, когда у нас есть маркер, мы можем, наконец, вызвать Graph API и выполнить требуемую операцию поиска. Вставьте следующий фрагмент сразу под определением `authenticate` .

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Файлы начальной точки содержат соответствующую поддержку для ввода псевдонима пользователя в текстовое поле. Данный метод использует значение псевдонима для создания запроса, сочетания его с маркером доступа, отправки в Graph API и анализа результатов. Метод renderData, который уже присутствует в файле начальной точки, отвечает за отображение результатов.

## <a name="4-run"></a>*4. Запуск*
Приложение, наконец готово к запуску! Процесс работы очень прост: после запуска приложения введите в текстовое поле псевдоним нужного пользователя, а затем нажмите кнопку. Будет предложено ввести учетные данные для проверки подлинности. После успешной проверки подлинности и успешного поиска будут отображены атрибуты искомого пользователя. Последующие запуски будут выполнять поиск без отображения запроса благодаря присутствию в кэше маркера, полученного ранее.
Конкретные этапы запуска приложения зависят от платформы.

#### <a name="windows-10"></a>Windows 10
   Планшет или ПК: `cordova run windows --archs=x64 -- --appx=uap`

   Мобильное устройство (требуется мобильное устройство Windows 10, подключенное к ПК): `cordova run windows --archs=arm -- --appx=uap --phone`

   **Примечание.** Во время первого запуска, возможно, потребуется выполнить вход для предоставления системе лицензии разработчика. Дополнительные сведения см. в статье [Получение лицензии разработчика](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

#### <a name="windows-81-tabletpc"></a>Планшет или ПК под управлением Windows 8.1
   `cordova run windows`

   **Примечание.** Во время первого запуска, возможно, потребуется выполнить вход для предоставления системе лицензии разработчика. Дополнительные сведения см. в статье [Получение лицензии разработчика](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

#### <a name="windows-phone-81"></a>Windows Phone 8.1:
   Запустить приложение на подключенном устройстве: `cordova run windows --device -- --phone`

   Запустить приложение в эмуляторе по умолчанию: `cordova emulate windows -- --phone`

   Используйте `cordova run windows --list -- --phone` для просмотра всех доступных целевых объектов и `cordova run windows --target=<target_name> -- --phone` —для запуска приложения на конкретном устройстве или эмуляторе (например, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

#### <a name="android"></a>Android:
   Запустить приложение на подключенном устройстве: `cordova run android --device`

   Запустить приложение в эмуляторе по умолчанию: `cordova emulate android`

   **Примечание.** Убедитесь, что экземпляр эмулятора создан с помощью *диспетчера AVD*, как описано в разделе *Предварительные требования*.

   Используйте `cordova run android --list` для просмотра всех доступных целевых объектов и `cordova run android --target=<target_name>` —для запуска приложения на конкретном устройстве или эмуляторе (например, `cordova run android --target="Nexus4_emulator"`).

#### <a name="ios"></a>iOS:
   Запустить приложение на подключенном устройстве: `cordova run ios --device`

   Запустить приложение в эмуляторе по умолчанию: `cordova emulate ios`

   **Примечание.** Убедитесь, что пакет `ios-sim` установлен для запуска в эмуляторе. Дополнительные сведения см. в разделе *Предварительные требования*.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Используйте `cordova run --help` для просмотра дополнительных параметров сборки и запуска.

Для справки следует отметить, что готовый пример (без ваших значений конфигурации) находится [здесь](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Теперь вы можете приступить к более сложным сценариям (и более содержательным).  Можно попробовать:

[Защита веб-API с помощью Azure AD для Node.JS >>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]



<!--HONumber=Nov16_HO5-->


