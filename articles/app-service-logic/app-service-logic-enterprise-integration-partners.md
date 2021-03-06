---
title: Сведения о партнерах и пакете интеграции Enterprise | Microsoft Docs
description: Узнайте, как использовать партнеры с пакетом интеграции Enterprise и приложениями логики.
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
ms.date: 07/08/2016
ms.author: deonhe

---
# Сведения о партнерах и пакете интеграции Enterprise
## Обзор
Перед созданием партнера вы и организация, с которой вы планируете работать, должны обменяться сведениями, которые помогут идентифицировать и проверять сообщения, отправляемые вами и этой организацией друг другу. Обсудив это и подготовившись к тому, чтобы начать деловые отношения, вы можете создать *партнер* в своей учетной записи интеграции.

## Что такое партнер?
Партнеры — это сущности, участвующие в обмене сообщениями и транзакциях "бизнес-бизнес".

## Как используются партнеры?
Партнеры используются для создания соглашений. Соглашение определяет сведения о сообщениях, которые будут передаваться между партнерами.

Перед созданием соглашения необходимо добавить по крайней мере два партнера в свою учетную запись интеграции. Одним из партнеров в соглашении должна быть ваша организация. Партнер, представляющий вашу организацию, называется **главным партнером**. Второй партнер будет представлять другую организацию, с которой ваша организация обменивается сообщениями. Второй партнер называется **партнером-гостем**. Партнером-гостем может быть другая компания или даже подразделение вашей организации.

После добавления партнеры будут использованы для создания соглашения.

Параметры получения и отправки задаются для главного партнера. Например, параметры получения в соглашении определяют, как главный партнер получает сообщения от партнера-гостя. Аналогично параметры отправки в соглашении указывают, как главный партнер отправляет сообщения партнеру-гостю.

## Как создать партнер?
На портале Azure выполните следующие действия.

1. Щелкните **Обзор**.![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)
2. Введите **integration** в поле фильтра поиска и выберите **Integration Accounts** (Учетные записи интеграции) из списка результатов. ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)
3. Выберите **учетную запись интеграции**, в которую необходимо добавить партнеры. ![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)
4. Выберите элемент **Партнеры**.![](./media/app-service-logic-enterprise-integration-partners/partner-1.png)
5. В открывшейся колонке "Партнеры" нажмите кнопку **Добавить**. ![](./media/app-service-logic-enterprise-integration-partners/partner-2.png)
6. В соответствующих полях введите **имя** партнера, выберите **квалификатор** и, наконец, введите **значение**. Это значение используется для определения документов, которые поступают в ваши приложения. ![](./media/app-service-logic-enterprise-integration-partners/partner-3.png)
7. Щелкните значок уведомления (*колокольчик*), чтобы просмотреть ход создания партнера. ![](./media/app-service-logic-enterprise-integration-partners/partner-4.png)
8. Выберите элемент **Партнеры**. Элемент обновится, и вы увидите, что количество партнеров возросло, отражая успешное добавление нового партнера. ![](./media/app-service-logic-enterprise-integration-partners/partner-5.png)
9. Выбрав элемент "Партнеры", в колонке "Партнеры" вы также увидите только что добавленный партнер. ![](./media/app-service-logic-enterprise-integration-partners/partner-6.png)

## Как изменить партнер
Выполните следующие действия, чтобы изменить партнер, который уже существует в вашей учетной записи интеграции.

1. Выберите элемент **Партнеры**.
2. В открывшейся колонке "Партнеры" выберите партнер, который вы хотите изменить.
3. Внесите необходимые изменения в элементе **Update Partner** (Обновление партнера).
4. Внеся все необходимые изменения, щелкните ссылку **Сохранить**, чтобы сохранить их. Чтобы отменить изменения, щелкните ссылку **Отмена**. ![](./media/app-service-logic-enterprise-integration-partners/edit-1.png)

## Как удалить партнер
1. Выберите элемент **Партнеры**.
2. В открывшейся колонке "Партнеры" выберите партнер, который вы хотите удалить.
3. Щелкните ссылку **Удалить**. ![](./media/app-service-logic-enterprise-integration-partners/delete-1.png)

## Дальнейшие действия
* [Узнайте больше о соглашениях](app-service-logic-enterprise-integration-agreements.md "Узнайте о соглашениях интеграции Enterprise.").

<!---HONumber=AcomDC_0803_2016-->