---
title: "Учебник. Интеграция Azure Active Directory с Innotas | Документация Майкрософт"
description: "Узнайте, как использовать Innotas вместе с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: eb9e9c2c-4b09-4177-bbab-423fd657448e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5d686f1ec3ec7a0ec5062e6da231ace8b47dbd83


---
# <a name="tutorial-azure-active-directory-integration-with-innotas"></a>Руководство. Интеграция Azure Active Directory с Innotas
Цель данного руководства — показать интеграцию Azure и Innotas.  
Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Клиент Innotas

После завершения этого руководства пользователи Azure AD, назначенные Innotas, будут иметь возможность единого входа в приложение на веб-сайте компании Innotas (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для Innotas
2. Настройка единого входа
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-innotas-tutorial/IC777331.png "Scenario")

## <a name="enabling-the-application-integration-for-innotas"></a>Включение интеграции приложений для Innotas
В этом разделе показано, как включить интеграцию приложений для Innotas.

### <a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для Innotas, выполните следующие действия.
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-innotas-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-innotas-tutorial/IC700994.png "Applications")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
   ![Добавить приложение](./media/active-directory-saas-innotas-tutorial/IC749321.png "Add application")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
   ![Добавить приложение из коллекции](./media/active-directory-saas-innotas-tutorial/IC749322.png "Add an application from gallerry")
6. В **поле поиска** введите **Innotas**.
   
   ![Коллекция приложений](./media/active-directory-saas-innotas-tutorial/IC777332.png "Application gallery")
7. В области результатов выберите **Innotas** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![Innotas](./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
   
   ## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить проверку подлинности в Innotas со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **Innotas** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-innotas-tutorial/IC777334.png "Configure single sign-on")
2. На странице **Как пользователи должны входить в Innotas?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Настройка единого входа](./media/active-directory-saas-innotas-tutorial/IC777335.png "Configure single sign-on")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа в Innotas** введите свой URL-адрес в формате *https://\<имя_клиента\>.Innotas.com*, а затем нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-innotas-tutorial/IC777336.png "Configure app URL")
4. На странице **Настройка единого входа в Innotas** нажмите кнопку **Скачать метаданные**, а затем сохраните файл данных локально в формате **c:\\InnotasMetaData.xml**.
   
   ![Настройка единого входа](./media/active-directory-saas-innotas-tutorial/IC777337.png "Configure single sign-on")
5. Передайте этот файл метаданных в группу поддержки Innotas. Группа поддержки осуществляет настройку единого входа.
6. Выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-innotas-tutorial/IC777338.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей

Элемент действия для настройки подготовки пользователей в Innotas отсутствует.  
Когда назначенный пользователь пытается войти в Innotas с помощью панели доступа, Innotas проверяет, существует ли данный пользователь.  
Если учетная запись пользователя отсутствует, Innotas автоматически создает ее.

## <a name="assigning-users"></a>Назначение пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-innotas-perform-the-following-steps"></a>Чтобы назначить пользователей Innotas, выполните следующие действия.
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **Innotas** нажмите кнопку **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-innotas-tutorial/IC777339.png "Assign users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-innotas-tutorial/IC767830.png "Yes")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


