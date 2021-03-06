---
title: Узнайте, как создать соглашение AS2 для пакета интеграции Enterprise.
description: Как создать соглашение AS2 для пакета интеграции Enterprise | Служба приложений Microsoft Azure
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: deonhe

---
# Интеграция Enterprise с AS2
## Создание соглашения AS2
Чтобы использовать корпоративные компоненты в приложениях логики, необходимо сначала создать соглашения.

### Вот что понадобится для начала работы
* В подписке Azure должна быть определена [учетная запись интеграции](app-service-logic-enterprise-integration-accounts.md).
* В учетной записи интеграции должны быть определены по крайней мере два [партнера](app-service-logic-enterprise-integration-partners.md).

> [!NOTE]
> При создании соглашения содержимое его файла должно соответствовать типу соглашения.
> 
> 

После [создания учетной записи интеграции](app-service-logic-enterprise-integration-accounts.md) и [добавления партнеров](app-service-logic-enterprise-integration-partners.md) можно создать соглашение, выполнив следующие действия.

### На домашней странице портала Azure
Войдите на [портал Azure](http://portal.azure.com "Портал Azure").

1. Выберите **Обзор** в меню слева.

> [!TIP]
> Если вы не видите ссылку **Обзор**, может потребоваться сначала развернуть это меню. Для этого щелкните ссылку **Показать меню**, расположенную в верхнем левом углу свернутого меню.
> 
> 

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)

1. Введите *integration* в поле фильтра поиска и выберите **Integration Accounts** (Учетные записи интеграции) из списка результатов. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
2. В открывшейся колонке **Integration Accounts** (Учетные записи интеграции) выберите учетную запись интеграции для создания соглашения. Если вы не видите списков учетных записей интеграции, то [создайте первый из них](app-service-logic-enterprise-integration-accounts.md "Все об учетных записях интеграции"). ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
3. Выберите элемент **Соглашение**. Если вы не видите элемент "Соглашение", то добавьте соглашение. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)
4. В открывшейся колонке "Соглашения" нажмите кнопку **Добавить**. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)
5. В открывшейся колонке "Соглашения" введите **имя** соглашения, затем задайте значения параметров **Host Partner** (Главный партнер), **Host Identity** (Идентификатор главного партнера), **Guest Partner** (Партнер-гость), **Guest Identity** (Идентификатор гостя). ![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)

Ниже приведены некоторые сведения, которые могут оказаться полезными при настройке параметров соглашения.

| Свойство | Описание |
| --- | --- |
| "Host Partner" (Главный партнер) |Для соглашения нужно указать главного партнера и партнера-гостя. Главный партнер представляет организацию, которая настраивает соглашение. |
| "Host Identity" (Идентификатор главного партнера) |Идентификатор главного партнера. |
| "Guest Partner" (Партнер-гость) |Для соглашения нужно указать главного партнера и партнера-гостя. Партнер-гость представляет организацию, которая имеет деловые отношения с главным партнером. |
| "Guest Identity" (Идентификатор гостя) |Идентификатор партнера-гостя. |
| "Receive Settings" (Параметры получения) |Эти свойства применяются ко всем сообщениям, получаемым посредством соглашения. |
| "Send Settings" (Параметры отправки) |Эти свойства применяются ко всем сообщениям, отправляемым посредством соглашения. |

Итак, продолжим.

1. Выберите **Receive Settings** (Параметры получения), чтобы настроить обработку сообщений, получаемых посредством данного соглашения.
   
   * При необходимости можно переопределить свойства во входящем сообщении. Чтобы сделать это, установите флажок **Override message properties** (Переопределить свойства сообщения).
   * Установите флажок **Message should be signed** (Сообщение должно быть подписано), чтобы требовать наличие подписи во всех входящих сообщениях. Если этот флажок установлен, то также потребуется выбрать **сертификат**, который будет использоваться для проверки подписи сообщений.
   * Кроме того, можно потребовать шифровать сообщения. Чтобы сделать это, установите флажок **Message should be encrypted** (Сообщение должно быть зашифровано). Затем потребуется выбрать **сертификат**, который будет использоваться для расшифровки входящих сообщений.
   * Можно также потребовать, чтобы сообщения были сжаты. Чтобы сделать это, установите флажок **Message should be compressed** (Сообщение должно быть сжато). ![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)

Если вы хотите узнать больше о том, какие параметры получения следует включить, ознакомьтесь со следующей таблицей.

| Свойство | Описание |
| --- | --- |
| "Override message properties" (Переопределить свойства сообщения) |Установите этот флажок, чтобы указать, что свойства в полученных сообщениях могут быть переопределены. |
| "Message should be signed" (Сообщение должно быть подписано) |Установите этот флажок, чтобы требовать наличие цифровой подписи в сообщениях. |
| "Message should be encrypted" (Сообщение должно быть зашифровано) |Установите этот флажок, чтобы требовать шифрование сообщений. Незашифрованные сообщения будут отклонены. |
| "Message should be compressed" (Сообщение должно быть сжато) |Установите этот флажок, чтобы требовать сжатие сообщений. Несжатые сообщения будут отклонены. |
| "MDN Text" (Текст MDN) |Это уведомление MDN по умолчанию, отправляемое отправителю сообщения. |
| "Send MDN" (Отправлять MDN) |Включите этот параметр, чтобы отправлять уведомления MDN. |
| "Send signed MDN" (Отправлять MDN с подписью) |Включите этот параметр, чтобы требовать отправки MDN с подписью. |
| "MIC Algorithm" (Алгоритм контроля целостности учетных данных) | |
| "Send asynchronous MDN" (Отправлять асинхронные MDN) |Включите этот параметр, чтобы требовать асинхронную отправку сообщений. |
| URL-адрес |Это URL-адрес, на который будут отправляться сообщения. |

Давайте продолжим.

1. Выберите **Send Settings** (Параметры отправки), чтобы настроить обработку сообщений, отправляемых посредством данного соглашения. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)

Если вы хотите узнать больше о том, какие параметры отправки следует включить, ознакомьтесь со следующей таблицей.

| Свойство | Описание |
| --- | --- |
| "Enable message signing" (Включить подписание сообщений) |Установите этот флажок, чтобы включить подписание всех сообщений, отправляемых посредством соглашения. |
| "MIC Algorithm" (Алгоритм контроля целостности учетных данных) |Выберите алгоритм, используемый для подписания сообщений. |
| Сертификат |Выберите сертификат, используемый для подписания сообщений. |
| "Enable message encryption" (Включить шифрование сообщений) |Установите этот флажок для шифрования всех сообщений, отправляемых посредством данного соглашения. |
| "Encryption Algorithm" (Алгоритм шифрования) |Выберите алгоритм для шифрования сообщений. |
| "Unfold HTTP headers" (Развернуть заголовки HTTP) |Установите этот флажок для разворачивания заголовка HTTP content-type в одну строку. |
| "Request MDN" (Запрашивать MDN) |Установите этот флажок, чтобы запрашивать уведомления MDN для всех сообщений, отправляемых посредством данного соглашения. |
| "Request signed MDN" (Запрашивать подписанные MDN) |Включите этот параметр, чтобы требовать наличие подписи у всех уведомлений MDN, отправляемых в это соглашение. |
| "Request asynchronous MDN" (Запрашивать асинхронные MDN) |Включите этот параметр, чтобы запрашивать отправку асинхронных уведомлений MDN в данное соглашение. |
| URL-адрес |URL-адрес, по которому будут отправляться уведомления MDN. |
| "Enable NRR" (Включить NRR) |Установите этот флажок, чтобы включить неотрекаемость от получения. |

Все почти готово.

1. Выберите элемент **Соглашения** в колонке "Integration Account" (Учетная запись интеграции), и вы увидите новое соглашение в списке. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

<!---HONumber=AcomDC_0803_2016-->