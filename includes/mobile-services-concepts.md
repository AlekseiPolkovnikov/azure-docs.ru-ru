## <a name="what-is"></a>Что такое мобильные службы?
Мобильные службы Azure — это платформа разработки высокомасштабируемых приложений для мобильных устройств, которая позволяет добавлять расширенные функциональные возможности в приложения для мобильных устройств с помощью Azure.

Возможности мобильных служб:

* **Создание собственных и кроссплатформенных приложений** — подключите свои приложения iOS, Android, Windows или кроссплатформенные приложения Xamarin либо Cordova (Phonegap) к серверной мобильной службе с помощью собственных пакетов SDK.  
* **Отправка push-уведомлений пользователям** — отправляйте push-уведомления пользователям своего приложения.
* **Аутентификация пользователей** — используйте популярных поставщиков удостоверений, например Facebook и Twitter, для аутентификации пользователей приложения.
* **Хранение данных в облаке** — храните данные пользователей в базе данных SQL (по умолчанию) или в Mongo DB, DocumentDB, таблицах Azure и больших двоичных объектах Azure. 
* **Создание приложений с возможностью автономной работы и синхронизации** — создайте приложения, работающие в автономном режиме, и используйте мобильные службы для синхронизации данных в фоновом режиме.
* **Мониторинг и масштабирование приложений** — наблюдайте за использованием приложения и масштабируйте серверную часть по мере роста запросов. 

## <a name="concepts"></a>Основные понятия мобильных служб
Ниже перечислены важные функции и основные понятия в Мобильных службах.

* **Ключ приложения:** уникальное значение, которое используется для ограничения доступа к вашей мобильной службе от случайных клиентов; «ключ» не является токеном безопасности и не используется для аутентификации пользователей приложения.    
* **Серверная часть:** экземпляр мобильной службы, который поддерживает работу приложения. Мобильная служба реализуется в виде проекта веб-API ASP.NET (*серверная часть .NET*) или проекта Node.js (*серверная часть JavaScript*).
* **Поставщик удостоверений:** внешняя служба, являющаяся доверенной для мобильных служб, которая аутентифицирует пользователей приложения. В число поддерживаемых поставщиков входят: Facebook, Twitter, Google, учетная запись Майкрософт и Azure Active Directory. 
* **Push-уведомление**: запущенное службой сообщение, отправляемое для зарегистрированного устройства или пользователя с помощью Центров уведомлений Azure.
* **Масштабируемость**: возможность увеличить (за дополнительную плату) вычислительную мощность, производительность и хранилище, когда ваше приложение становится более популярным.
* **Запланированное задание**: пользовательский код, который запускается по заранее заданному расписанию или по запросу.

Дополнительную информацию см. в разделе [Основные понятия мобильных служб](../articles/mobile-services/mobile-services-concepts-links.md).

<!---HONumber=AcomDC_0309_2016-->