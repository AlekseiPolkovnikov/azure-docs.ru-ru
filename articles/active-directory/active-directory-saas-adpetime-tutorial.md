---
title: "Руководство по интеграции Azure Active Directory с ADP eTime | Документация Майкрософт"
description: "Узнайте, как настроить единый вход Azure Active Directory в ADP eTime."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 56e3a4ee3cc52fc2b18e78a42a65af33a61ff349


---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Учебник. Интеграция Azure Active Directory с ADP eTime
Цель этого учебника — показать, как интегрировать Azure Active Directory (Azure AD) с приложением ADP eTime.  
Интеграция ADP eTime с Azure AD обеспечивает следующие преимущества.

* С помощью Azure AD вы можете контролировать доступ к ADP eTime.
* Вы можете включить автоматический вход пользователей в ADP eTime (единый вход) с использованием учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно — через классический портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования
Чтобы настроить интеграцию Azure AD с ADP eTime, вам потребуется:

* подписка Azure AD;
* подписка ADP eTime с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.
> 
> 

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

* Не следует использовать рабочую среду при отсутствии необходимости.
* Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
Цель этого учебника — научить вас проверять единый вход в Azure AD в пробной среде.  
Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление ADP eTime из коллекции
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-adp-etime-from-the-gallery"></a>Добавление ADP eTime из коллекции
Чтобы настроить интеграцию ADP eTime с Azure AD, необходимо добавить ADP eTime из коллекции в список управляемых приложений SaaS.

**Чтобы добавить ADP eTime из коллекции, выполните следующие действия:**

1. На **классическом портале Azure**в области навигации слева щелкните **Active Directory**. 
   
    ![Active Directory][1]
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения][2]
4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Приложения][3]
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Приложения][4]
6. В поле поиска введите **ADP eTime**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)
7. В области результатов выберите **ADP eTime** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
Цель этого раздела — показать, как настроить и проверить единый вход Azure AD в ADP eTime с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в ADP eTime соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в ADP eTime.  
Чтобы установить эту связь, следует указать **имя пользователя** в Azure AD в качестве значения **имени пользователя** в ADP eTime.

Чтобы настроить и проверить единый вход Azure AD в ADP eTime, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя ADP eTime](#creating-a-adpetime-test-user)** требуется для создания пользователя Britta Simon в ADP eTime, связанного с соответствующим представлением в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD
Цель этого раздела — включить единый вход Azure AD на классическом портале Azure и настроить единый вход в приложение ADP eTime.

Приложение ADP eTime ожидает проверочные утверждения SAML в определенном формате, который требует добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана приведен пример. Утверждение всегда будет носить имя **PersonImmutableID** и иметь значение, которое было сопоставлено с ExtensionAttribute2, содержащим EmployeeID пользователя. Здесь выполняется сопоставление пользователя из Azure AD с ADP eTime по значению EmployeeID, однако его можно сопоставить с другим значением, учитывая параметры приложения. Вместе со службой поддержки ADP eTime выполните действия по использованию правильного идентификатора пользователя и его сопоставлению с утверждением **PersonImmutableID** .  

![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Чтобы настроить утверждение SAML, обратитесь в службу технической поддержки ADP eTime и запросите значение уникального идентификатора для вашего клиента. Это значение необходимо для настройки пользовательского утверждения для вашего приложения.

**Чтобы настроить единый вход Azure AD в ADP eTime, выполните следующие действия:**

1. На классическом портале Azure на странице интеграции с приложением **ADP eTime** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа][6] 
2. На странице **Как пользователи должны входить в ADP eTime** выберите **Единый вход Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 
3. На странице диалогового окна **Настройка параметров приложения** выполните следующие действия.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 

    а. В текстовое поле **URL-адрес ответа** введите URL-адрес, используемый для входа в приложение ADP eTime, по следующей схеме: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Нажмите кнопку **Далее**.

1. На странице **Настройка единого входа в ADP eTime** выполните следующие действия.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 
   
    а. Нажмите **Загрузить метаданные**и сохраните файл на свой компьютер.
   
    b. Нажмите кнопку **Далее**.
2. Чтобы настроить единый вход для приложения, обратитесь в службу технической поддержки ADP eTime по электронной почте, прикрепив скачанный файл сертификата и указав URL-адрес удаленного входа и выхода. После этого служба поддержки сможет настроить для вас интеграцию единого входа.
   
   > [!NOTE]
   > Когда специалисты службы поддержки **ADP eTime** настроят экземпляр, получите от них значение **RelayState**. Выполните указанные ниже действия, чтобы настроить его. После этой настройки проверьте интеграцию. Обратите внимание, что эта конфигурация имеет важное значение для поддержки интеграции приложения.
   > 
   > 
3. Чтобы настроить значение RelayState в Azure AD, выполните следующие действия. 
   
    а. Войдите на [портал управления Azure](https://portal.azure.com) с учетной записью администратора.
   
    b. В области навигации слева щелкните **Больше служб**. 
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)
   
    c. В текстовое поле **Поиск** введите **Azure Active Directory**, а затем щелкните связанную ссылку.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)
   
    d. Щелкните **Корпоративные приложения**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)
   
    д. В разделе **Управление** щелкните **Все приложения**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)
   
    Е. В текстовое поле **Поиск** введите **ADP eTime**, а затем щелкните связанную ссылку. 
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)
   
    ж. В разделе **Управление** щелкните **Единый вход**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)
   
    h. Выберите **Показать дополнительные параметры URL-адресов**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
   
    i. В текстовое поле **Состояние ретранслятора** введите значение в следующем формате:
   
   * рабочая среда: `https://fed.adp.com/saml/fedlanding.html?<id>` 
   * Промежуточная среда: `https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`
     
     ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)
     
     j. Сохраните параметры.
4. На классическом портале Azure выберите подтверждение конфигурации единого входа и нажмите кнопку **Далее**.
   
    ![единого входа Azure AD][10]
5. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.  
   
    ![единого входа Azure AD][11]

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на классическом портале Azure тестового пользователя с именем Britta Simon.  
В списке пользователей выберите **Britta Simon**.

![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы отобразить список пользователей, в меню вверху выберите **Пользователи**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 
4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу нажмите кнопку **Добавить пользователя**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 
5. На странице диалогового окна **Тип учетной записи пользователя** выполните следующие действия.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 
   
    а. В поле **Тип пользователя** выберите значение **Новый пользователь в вашей организации**.
   
    b. В текстовом поле **Имя пользователя** введите **BrittaSimon**.
   
    c. Нажмите кнопку **Далее**.
6. На странице диалогового окна **Профиль пользователя** выполните следующие действия.
   
   ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 
   
   а. В текстовом поле **Имя** введите **Britta**.  
   
   b. В текстовом поле **Фамилия** введите **Simon**.
   
   c. В текстовом поле **Отображаемое имя** введите **Britta Simon**.
   
   d. В списке **Роль** выберите **Пользователь**.
   
   д. Нажмите кнопку **Далее**.
7. На странице диалогового окна **Получить временный пароль** нажмите кнопку **Создать**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 
8. На странице диалогового окна **Получить временный пароль** выполните следующие действия.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 
   
    а. Запишите значение поля **Новый пароль**.
   
    b. Нажмите **Завершено**.   

### <a name="creating-a-adp-etime-test-user"></a>Создание тестового пользователя ADP eTime
Цель этого раздела — создать пользователя с именем Britta Simon в приложении ADP eTime. Обратитесь в службу поддержки ADP eTime, чтобы добавить пользователей в учетную запись ADP eTime. 

> [!NOTE]
> Чтобы создать пользователя вручную, необходимо обратиться в службу поддержки ADP eTime.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD
Цель этого раздела — позволить пользователю Britta Simon использовать единый вход Azure, предоставив ей доступ к ADP eTime.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в ADP eTime, выполните следующие действия:**

1. Чтобы открыть представление приложений, на классическом портале Azure в представлении каталога щелкните **Приложения** в меню вверху.
   
    ![Назначение пользователя][201] 
2. Из списка приложений выберите **ADP eTime**.
   
    ![Настройка единого входа](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 
3. В меню в верхней части страницы щелкните **Пользователи**.
   
    ![Назначение пользователя][203] 
4. В списке пользователей выберите **Britta Simon**.
5. На панели инструментов внизу щелкните **Назначить**.
   
    ![Назначение пользователя][205]

### <a name="testing-single-sign-on"></a>Проверка единого входа
Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  
Щелкнув элемент ADP eTime на панели доступа, вы автоматически войдете в приложение ADP eTime.

## <a name="additional-resources"></a>дополнительные ресурсы.
* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO4-->


