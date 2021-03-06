---
title: "Руководство по интеграции Azure Active Directory с SmarterU | Документация Майкрософт"
description: "Узнайте, как использовать SmarterU вместе с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и многого другого."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f8658df8871ce3bdca4724448c2232ea326fce6


---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Учебник. Интеграция Azure Active Directory с SmarterU
Цель данного учебника — показать интеграцию Azure и SmarterU.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Клиент SmarterU.

После завершения этого руководства пользователи Azure AD, назначенные SmarterU, будут иметь возможность единого входа в приложение на веб-сайте компании SmarterU (вход, инициированный поставщиком услуг) или с помощью указаний из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для SmarterU
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

## <a name="enabling-the-application-integration-for-smarteru"></a>Включение интеграции приложений для SmarterU
В этом разделе показано, как включить интеграцию приложений для SmarterU.

### <a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для SmarterU, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-smarteru-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-smarteru-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **SmarterU**.
   
   ![Коллекция приложений](./media/active-directory-saas-smarteru-tutorial/IC777321.png "Application fallery")
7. В области результатов выберите **SmarterU** и щелкните **Завершить**, чтобы добавить приложение.
   
   ![SmarterU](./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

## <a name="configuring-single-sign-on"></a>Настройка единого входа
В этом разделе показано, как разрешить пользователям проходить аутентификацию в SmarterU со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure AD на странице интеграции с приложением **SmarterU** нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-smarteru-tutorial/IC777323.png "Configure Single Sign-On")
2. На странице **Как пользователи должны входить в SmarterU** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-smarteru-tutorial/IC777324.png "Configure Single Sign-On")
3. На странице **Настройка единого входа в SmarterU** щелкните **Скачать метаданные**, чтобы скачать метаданные, а затем сохраните файл данных локально как **c:\\SmarterUMetaData.cer**.
   
   ![Настройка единого входа](./media/active-directory-saas-smarteru-tutorial/IC777325.png "Configure Single Sign-On")
4. В другом окне веб-браузера войдите на сайт SmarterU своей компании в качестве администратора.
5. На панели инструментов вверху щелкните **Параметры учетной записи**.
   
   ![Параметры учетной записи](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")
6. На странице конфигурации учетной записи выполните следующие действия.
   
   ![Внешняя авторизация](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")
   
   1. Установите флажок **Включить внешнюю авторизацию**.
   2. В разделе **Master Login Control** (Управление универсальным именем для входа) щелкните вкладку **SmarterU**.
   3. В разделе **User Default Login** (Имя для входа пользователей по умолчанию) щелкните вкладку **SmarterU**.
   4. Выберите **Включить Okta**.
   5. Скопируйте содержимое скачанного файла метаданных и вставьте его в текстовое поле **Метаданные Okta** .
   6. Щелкните **Сохранить**.
7. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-smarteru-tutorial/IC777328.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей
Чтобы пользователи Azure AD могли входить в SmarterU, их необходимо подготовить для SmarterU.  
В случае SmarterU подготовка выполняется вручную.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Чтобы подготовить учетные записи пользователей, выполните следующие действия:
1. Выполните вход в клиент **SmarterU** .
2. Перейдите в раздел **Пользователи**.
3. В разделе «Пользователи» выполните следующие действия:
   
   ![Новый пользователь](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")
   
   1. Щелкните **+ Пользователь**.
   2. Введите соответствующие значения атрибутов учетной записи Azure AD в следующие текстовые поля: **Primary Email** (Основной электронный адрес), **Employee ID** (Идентификатор сотрудника), **Password** (Пароль), **Verify Password** (Проверка пароля), **Given Name** (Имя) и **Surname** (Фамилия).
   3. Нажмите **Активный**.
   4. Щелкните **Сохранить**.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователей SmarterU или API, предоставляемые SmarterU для подготовки учетных записей пользователей AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Чтобы назначить пользователей SmarterU, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **SmarterU** нажмите кнопку **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-smarteru-tutorial/IC777330.png "Assign Users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-smarteru-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


