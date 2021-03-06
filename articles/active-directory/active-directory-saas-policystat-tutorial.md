---
title: "Руководство по интеграции Azure Active Directory с PolicyStat | Документация Майкрософт"
description: "Узнайте, как использовать PolicyStat с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 738184f4a253201a9aa7581e03d269a06d7cf48a


---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Учебник. Интеграция Azure Active Directory с PolicyStat
Цель данного учебника — показать интеграцию Azure и PolicyStat.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* действующая подписка Azure;
* Клиент PolicyStat.

По завершении работы с этим руководством пользователи Azure AD, назначенные в PolicyStat, смогут выполнять единый вход в приложение на веб-сайте PolicyStat компании (вход, инициированный поставщиком услуг) или следуя указаниям в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для PolicyStat
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")

## <a name="enabling-the-application-integration-for-policystat"></a>Включение интеграции приложений для PolicyStat
В этом разделе показано, как включить интеграцию приложений для PolicyStat.

### <a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для PolicyStat, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-policystat-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-policystat-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-policystat-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **PolicyStat**.
   
   ![Коллекция приложений](./media/active-directory-saas-policystat-tutorial/IC808627.png "Application Gallery")
7. В области результатов выберите **PolicyStat** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
   
   ## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в PolicyStat со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.  
Приложение PolicyStat ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию **атрибутов токена SAML** .  
На следующем снимке экрана приведен пример.

![Атрибуты](./media/active-directory-saas-policystat-tutorial/IC808628.png "Attributes")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На классическом портале Azure на странице интеграции с приложением **PolicyStat** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-policystat-tutorial/IC808629.png "Configure Single Sign-On")
2. На странице **Как пользователи должны входить в PolicyStat?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-policystat-tutorial/IC808630.png "Configure Single Sign-On")
3. На странице **Настроить параметры приложения** в текстовом поле **URL-адрес входа** введите URL-адрес, используемый пользователями для входа в приложение PolicyStat (например, *https://demo-azure.policystat.com*), и нажмите кнопку **Далее**.
   
   ![Настройка параметров приложения](./media/active-directory-saas-policystat-tutorial/IC808631.png "Configure App Settings")
4. На странице **Настройка единого входа в PolicyStat** щелкните **Скачать метаданные**, а затем сохраните файл метаданных на своем компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-policystat-tutorial/IC808632.png "Configure Single Sign-On")
5. В другом окне веб-браузера войдите на сайт PolicyStat своей компании в качестве администратора.
6. Щелкните вкладку **Admin** (Администрирование), а затем щелкните **Single Sign-On Configuration** (Конфигурация единого входа) в левой области навигации.
   
   ![Меню «Администратор»](./media/active-directory-saas-policystat-tutorial/IC808633.png "Administrator Menu")
7. В разделе **Setup** (Настройка) установите флажок **Enable Single Sign-on Integration** (Включить интеграцию единого входа).
   
   ![Конфигурация единого входа](./media/active-directory-saas-policystat-tutorial/IC808634.png "Single Sign-On Configuration")
8. Щелкните **Configure Attributes** (Настроить атрибуты), а затем в разделе **Configure Attributes** (Настройка атрибутов) сделайте следующее.
   
   ![Конфигурация единого входа](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")
   
   1. В текстовом поле **Username Attribute** (Атрибут имени пользователя) введите значение **uid**.
   2. В текстовом поле **First Name Attribute** (Атрибут имени) введите значение **firstname**.
   3. В текстовом поле **Last Name Attribute** (Атрибут фамилии) введите значение **lastname**.
   4. В текстовом поле **Email Attribute** (Атрибут электронной почты) введите значение **emailaddress**.
   5. Нажмите кнопку **Сохранить изменения**.
9. Щелкните **Your IDP Metadata** (Метаданные вашего поставщика удостоверений), а затем в разделе **Your IDP Metadata** (Метаданные вашего поставщика удостоверений) сделайте следующее.
   
   ![Конфигурация единого входа](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")
   
   1. Откройте скачанный файл метаданных, скопируйте его содержимое и вставьте его в текстовое поле **Метаданные поставщика удостоверений** .
   2. Нажмите кнопку **Сохранить изменения**.
10. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-policystat-tutorial/IC771723.png "Configure Single Sign-On")
11. 1. В меню в верхней части экрана выберите пункт **Атрибуты** to open the **SAML Token Атрибуты** .
    
    ![Атрибуты](./media/active-directory-saas-policystat-tutorial/IC795920.png "Attributes")
12. Чтобы добавить обязательные сопоставления атрибутов, выполните следующие действия.
    
    ![Атрибуты](./media/active-directory-saas-policystat-tutorial/IC804823.png "Attributes")
    
    1. Щелкните **добавить атрибут пользователя**.
    2. В текстовом поле **Имя атрибута** введите значение **uid**.
    3. В текстовом поле **Значение атрибута** выберите **ExtractMailPrefix()**.
    4. Из списка **Почта** выберите пункт **User.mail**.
    5. Нажмите **Завершено**.
       ##<a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Чтобы пользователи Azure AD могли выполнять вход в PolicyStat, они должны быть подготовлены для PolicyStat.  
PolicyStat поддерживает подготовку пользователей «на лету». Это означает, что вам не надо вручную добавлять пользователей PolicyStat.  
Пользователи будут добавляться автоматически первом входе с помощью единого входа.

> [!NOTE]
> Вы можете использовать любые другие инструменты создания учетных записей пользователя PolicyStat или API, предоставляемые PolicyStat для подготовки учетных записей пользователя AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Чтобы назначить пользователей PolicyStat, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **PolicyStat** щелкните **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-policystat-tutorial/IC808636.png "Assign Users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-policystat-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


