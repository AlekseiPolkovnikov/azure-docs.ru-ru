---
title: "Учебник. Интеграция Azure Active Directory с Lynda.com | Документация Майкрософт"
description: "Узнайте, как использовать Lynda.com вместе с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 68ac46371849282e4d68b581373b3510a2980744


---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Руководство. Интеграция Azure Active Directory с Lynda.com
Цель данного руководства — показать интеграцию Azure и Lynda.com.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Клиент Lynda.com

После завершения этого руководства пользователи Azure AD, назначенные Lynda.com, будут иметь возможность единого входа в приложение на веб-сайте компании Lynda.com (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для Lynda.com
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-lynda-tutorial/IC781046.png "Scenario")

## <a name="enabling-the-application-integration-for-lyndacom"></a>Включение интеграции приложений для Lynda.com
В этом разделе показано, как включить интеграцию приложений для Lynda.com.

### <a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Lynda.com, выполните следующие действия.
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-lynda-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-lynda-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-lynda-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **Lynda.com**.
   
   ![Коллекция приложений](./media/active-directory-saas-lynda-tutorial/IC777524.png "Application Gallery")
7. В области результатов выберите **Lynda.com** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![Lynda.com](./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
   
   ## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить проверку подлинности в Lynda.com со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

> [!IMPORTANT]
> Чтобы иметь возможность настроить единый вход для клиента Lynda.com, необходимо сначала обратиться в службу технической поддержки Lynda.com для включения соответствующей функции.
> 
> 

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **Lynda.com** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-lynda-tutorial/IC777526.png "Configure single sign-on")
2. На странице **Как пользователи должны входить в Lynda.com?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-lynda-tutorial/IC777527.png "Configure single sign-on")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа в Lynda.com** введите URL-адрес клиента Lynda.com (например, *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target=https://shib.lynda.com/InCommon*) и нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-lynda-tutorial/IC781047.png "Configure app URL")
4. На странице **Настройка единого входа в Lynda.com** нажмите кнопку **Скачать метаданные**, а затем сохраните файл сертификата на локальном компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-lynda-tutorial/IC777529.png "Configure single sign-on")
5. Отправьте скачанный файл метаданных в группу поддержки Lynda.com. Группа поддержки Lynda.com выполняет для вас настройку единого входа.
6. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-lynda-tutorial/IC777530.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Элемент действия для настройки подготовки пользователей в Lynda.com отсутствует.  
Когда назначенный пользователь пытается войти в Lynda.com с помощью панели доступа, Lynda.com проверяет, существует ли данный пользователь.  
Если учетная запись пользователя отсутствует, Lynda.com автоматически создает ее.

> [!NOTE]
> Вы можете использовать любые другие средства создания учетной записи пользователя Lynda.com или API, предоставляемые Lynda.com для подготовки учетных записей пользователя AAD.
> 
> 

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Чтобы назначить пользователей Lynda.com, выполните следующие действия.
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **Lynda.com** нажмите кнопку **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-lynda-tutorial/IC777531.png "Assign users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-lynda-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


