---
title: "Руководство по интеграции Azure Active Directory с 15Five | Документация Майкрософт"
description: "Узнайте, как использовать 15Five вместе с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7a0a300f505d9012471679ac27373944f07fdba3
ms.openlocfilehash: ce98338e6b21eb35a17f0183f409dd54d1123bb9


---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a>Руководство. Интеграция Azure Active Directory с 15Five
Цель данного руководства — показать интеграцию Azure и 15Five. Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Подписка с поддержкой единого входа 15Five

После завершения этого руководства пользователи Azure AD, назначенные 15Five, будут иметь возможность единого входа в приложение на веб-сайте компании 15Five (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для 15Five
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")

## <a name="enabling-the-application-integration-for-15five"></a>Включение интеграции приложений для 15Five
В этом разделе показано, как включить интеграцию приложений для 15Five.

### <a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для 15Five, выполните следующие действия.
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-15five-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-15five-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-15five-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **15Five**.
   
   ![Коллекция приложений](./media/active-directory-saas-15five-tutorial/IC784668.png "Application Gallery")
7. В области результатов выберите **15Five** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![15Five](./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
   
## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить проверку подлинности в 15Five со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **15Five** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-15five-tutorial/IC784670.png "Configure single sign-on")
2. На странице **Как пользователи должны входить в 15Five?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-15five-tutorial/IC784671.png "Configure single sign-on")
3. На странице **Configure App URL** (Настройка URL-адреса приложения) в текстовом поле **URL-адрес входа в 15Five** введите свой URL-адрес в формате *https://компания.15Five.com*, а затем нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-15five-tutorial/IC784672.png "Configure App URL")
4. На странице **Настройка единого входа в 15Five** нажмите кнопку **Скачать метаданные**, а затем перешлите файл метаданных в службу поддержки 15Five.
   
   ![Настройка единого входа](./media/active-directory-saas-15five-tutorial/IC784673.png "Configure single sign-on")
   
   > [!NOTE]
   > Единый вход должна включить служба поддержки 15Five.
   > 
   > 
5. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-15five-tutorial/IC784674.png "Configure single sign-on")
   

## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Чтобы разрешить пользователям Azure AD вход в 15Five, они должны быть подготовлены для 15Five.  
В случае с 15Five подготовка выполняется вручную.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.
1. Выполните вход на корпоративном веб-сайте **15Five** в качестве администратора.
2. Откройте страницу **Управление компанией**.
   
   ![Управление компанией](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")
3. Последовательно выберите пункты **People (Люди) \> Add People (Добавить людей)**.
   
   ![Люди](./media/active-directory-saas-15five-tutorial/IC784676.png "People")
4. В разделе «Добавить новых пользователей» выполните следующие действия.
   
   ![Добавление нового пользователя](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")
   
   1. В соответствующих текстовых полях введите **имя**, **фамилию**, **должность** и **адрес электронной почты** действующей учетной записи Azure Active Directory, которую вы хотите подготовить.
   2. Нажмите кнопку **Done**(Готово).
   
   > [!NOTE]
   > Владелец учетной записи Azure AD получит по электронной почте сообщение со ссылкой для активации учетной записи.
   > 
   > 



## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-15five-perform-the-following-steps"></a>Чтобы назначить пользователей 15Five, выполните следующие действия.
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **15Five** нажмите кнопку **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-15five-tutorial/IC784678.png "Assign users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-15five-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO4-->


