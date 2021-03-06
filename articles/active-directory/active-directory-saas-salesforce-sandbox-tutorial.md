---
title: "Руководство по интеграции Azure Active Directory с песочницей Salesforce | Документация Майкрософт"
description: "Узнайте, как использовать песочницу Salesforce вместе с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и многого другого."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/28/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 286f5a31a410914b32dd407055d8e0abe4c6eeda


---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Учебник. Интеграция Azure Active Directory с песочницей Salesforce
> [!TIP]
> Чтобы оставить отзыв, щелкните [здесь](http://go.microsoft.com/fwlink/?LinkId=521878).
> 
> 

Цель данного учебника — показать интеграцию Azure и песочницы Salesforce.  
Песочницы позволяет создать несколько копий организации в отдельных средах для различных целей, например для разработки, тестирования и обучения, не подвергая риску данные и приложения в рабочей организации Salesforce.  
Дополнительные сведения см. в [статье о песочницах](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US).

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Песочница на сайте Salesforce.com.

Если у вас еще нет действующей песочницы на сайте Salesforce.com, вам необходимо будет обратиться в Salesforce.

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для песочницы Salesforce
2. Настройка единого входа
3. Включение домена
4. Настройка подготовки учетных записей пользователей
5. Назначение пользователей

![Сценарий](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")

## <a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Включение интеграции приложений для песочницы Salesforce
В этом разделе показано, как включить интеграцию приложений для песочницы Salesforce.

### <a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Чтобы включить интеграцию приложений для песочницы Salesforce, выполните следующие действия:
1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
   ![Приложения](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applications")
4. Чтобы открыть **коллекцию приложений**, щелкните **Добавить приложение**, затем — **Добавить приложение для использования моей организацией**.
   
   ![Что необходимо сделать?](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "What do you want to do?")
5. В **поле поиска** введите **Salesforce Sandbox**.
   
   ![Коллекция приложений](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Application Gallery")
6. В области результатов выберите **Salesforce Sandbox** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
   ![Песочница Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce Sandbox")
   
   ## <a name="configuring-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить проверку подлинности в Salesforce со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Чтобы настроить единый вход, выполните следующие действия.
1. На странице интеграции с приложением **Salesforce Sandbox** классического портала Azure нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
   ![Настройка единого входа](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Configure single sign-on")
2. На странице **Как пользователи должны входить в Salesforce Sandbox?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
   ![Песочница Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce Sandbox")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес для входа** введите свой URL-адрес, используя следующий шаблон `http://company.my.salesforce.com`, а затем нажмите кнопку **Далее**.
   
   ![Настройка URL-адреса приложения](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Configure App URL")
4. Если вы уже настроили единый вход для другого экземпляра Salesforce Sandbox в каталоге, необходимо также настроить параметр **Идентификатор**, задав для него то же значение, что и для параметра **URL-адрес входа**. Поле **Идентификатор** можно найти, установив флажок **Показать дополнительные настройки** на странице **Настроить URL-адрес приложения** диалогового окна.
5. На странице **Настройка единого входа в Salesforce Sandbox** нажмите кнопку **Скачать сертификат**, а затем сохраните файл сертификата на компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Configure Single Sign-On")
6. В другом окне веб-браузера войдите в свою песочницу Salesforce в качестве администратора.
7. В верхнем меню щелкните **Настройка**.
   
   ![Настройка](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")
8. В области навигации слева щелкните **Security Controls** (Средства управления безопасностью), а затем — **Параметры единого входа**.
   
   ![Параметры единого входа](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Single Sign-On Settings")
9. В разделе «Параметры единого входа» выполните следующие действия:
   
   ![Параметры единого входа](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Single Sign-On Settings")
   
   а.  Установите флажок **SAML включен**.
   
   b.  Нажмите кнопку **Создать**.
10. В разделе «Параметры единого входа SAML» выполните следующие действия:
    
    ![Параметры единого входа SAML](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML Single Sign-On Settings")
    
    а.  В текстовом поле "Имя" введите имя конфигурации (например, *SPSSOWAAD\_Test*).
    
    b.  На странице **Настройка единого входа в Salesforce Sandbox** классического портала Azure скопируйте значение поля **URL-адрес издателя** и вставьте его в текстовое поле **Издатель**.
    
    В.  В текстовом поле **Идентификатор сущности** введите **https://test.salesforce.com**, если это первый экземпляр Salesforce Sandbox, добавляемый в каталог. Если вы уже добавили экземпляр Salesforce Sandbox, то для параметра **Идентификатор сущности** введите значение **URL-адрес входа** в следующем формате: `http://company.my.salesforce.com`
    
    d.  Чтобы отправить скачанный сертификат, нажмите кнопку **Обзор**.
    
    д.  В поле **SAML Identity Type** (Тип удостоверения SAML) выберите значение **Assertion contains the Federation ID from the User object** (Проверочное утверждение содержит идентификатор федерации из объекта User).
    
    Е.  В поле **SAML Identity Location** (Расположение удостоверения SAML) выберите значение **Identity is in the NameIdentifier element of the Subject statement** (Удостоверение находится в элементе NameIdentifier оператора Subject).
    
    g.  На странице **Настройка единого входа в Salesforce Sandbox** классического портала Azure скопируйте значение поля **URL-адрес удаленного входа** и вставьте его в текстовое поле **URL-адрес входа поставщика удостоверений**.
    
    h.  SFDC не поддерживает выход SAML.  Чтобы решить эту проблему, вставьте URL-адрес https://login.windows.net/common/wsfederation?wa=wsignout1.0 в текстовое поле **URL-адрес выхода поставщика удостоверений** .
    
    i.  В поле **Service Provider Initiated Request Binding** (Связывание запросов, инициированных поставщиком услуг) выберите значение **HTTP POST**.
    
    j. Щелкните **Сохранить**.
11. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
    
    ![Настройка единого входа](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Configure Single Sign-On")

## <a name="enabling-your-domain"></a>Включение домена
В этом разделе предполагается, что вы уже создали домен.  
Дополнительные сведения см. в разделе об [определении имени домена](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

### <a name="to-enable-your-domain-perform-the-following-steps"></a>Выполните следующие действия, чтобы включить домен:
1. В области навигации слева щелкните **Управление доменами**, затем — **Мой домен**.
   
   ![Мой домен](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "My Domain")
   
   > [!NOTE]
   > Убедитесь в правильности настройки домена.
   > 
   > 
2. В разделе **Login Page Settings** (Параметры страницы входа) щелкните **Изменить**, а затем для параметра **Authentication Service** (Служба проверки подлинности) выберите имя параметра единого входа SAML из предыдущего раздела, после чего щелкните **Сохранить**.
   
   ![Мой домен](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "My Domain")

После настройки домена для входа в песочницу Salesforce пользователям необходимо будет использовать URL-адрес домена.  
Чтобы получить значение URL-адреса, щелкните профиль единого входа, созданный в предыдущем разделе.

## <a name="configuring-user-provisioning"></a>Настройка подготовки учетных записей пользователей
В этом разделе показано, как включить подготовку учетных записей пользователей Active Directory для песочницы Salesforce.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.
1. На портале Salesforce в верхней области навигации выберите свое имя, чтобы открыть меню пользователя:
   
   ![Мои параметры](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "My Settings")
2. В меню пользователя выберите **Мои параметры**, чтобы открыть страницу **Мои параметры**.
3. В области навигации слева щелкните **Личный**, чтобы развернуть раздел "Личный", и щелкните **Reset My Security Token** (Сброс маркера безопасности).
   
   ![Мои параметры](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "My Settings")
4. На странице **Reset My Security Token** (Сброс токена безопасности) щелкните **Reset Security Token** (Сбросить токен безопасности), чтобы запросить электронное сообщение, содержащее ваш токен безопасности Salesforce.com.
   
   ![Новый маркер](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "New Token")
5. Проверьте папку "Входящие" электронной почты на наличие сообщения от Salesforce.com со строкой темы**salesforce.com.com security confirmation**.
6. Откройте это сообщение и скопируйте значение маркера безопасности.
7. На странице интеграции приложений **Salesforce Sandbox** классического портала управления Azure щелкните **Настроить подготовку учетных записей пользователей**, чтобы открыть диалоговое окно **Настроить подготовку учетных записей пользователей**.
   
   ![Настроить подготовку учетных записей пользователей](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Configure user provisioning")
8. На странице **Введите учетные данные песочницы Salesforce, чтобы включить автоматическую подготовку учетных записей пользователей** укажите следующие параметры конфигурации:
   
   ![Песочница Salesforce](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce Sandbox")
   
   а.  В текстовом поле **Имя пользователя администратора Salesforce Sandbox** введите имя учетной записи Salesforce Sandbox с профилем **системного администратора** в Salesforce.com.
   
   b.  В текстовом поле **Пароль администратора Salesforce Sandbox** введите пароль для этой учетной записи.
   
   c.  В текстовом поле **Токен безопасности пользователя** вставьте значение токена безопасности.
   
   d.  Щелкните **Проверить** для проверки конфигурации.
   
   д.  Нажмите кнопку **Далее**, чтобы открыть страницу **Подтверждение**.
9. На странице **Подтверждение** щелкните **Завершить**, чтобы сохранить конфигурацию.
   
   ## <a name="assigning-users"></a>Назначение пользователей

Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

### <a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Чтобы назначить пользователей песочницы Salesforce, выполните следующие действия:
1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **Salesforce Sandbox** щелкните **Назначить пользователей**.
   
   ![Назначить пользователей](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Assign users")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
   ![Да](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Yes")

Подождите 10 минут и убедитесь, что учетная запись синхронизирована с песочницей Salesforce.

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](https://msdn.microsoft.com/library/dn308586).




<!--HONumber=Nov16_HO3-->


