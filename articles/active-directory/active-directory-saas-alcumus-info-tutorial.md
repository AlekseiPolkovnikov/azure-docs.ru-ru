---
title: "Руководство. Интеграция Azure Active Directory с Alcumus Info Exchange | Документация Майкрософт"
description: "Узнайте, как настроить единый вход Azure Active Directory в Alcumus Info Exchange."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1a2ff324cfa65977685f6a0eee9cd398b0717519
ms.openlocfilehash: 4c22b52d1c86fefbccd452967c6250fdecaac6dd


---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Учебник. Интеграция Azure Active Directory с Alcumus Info Exchange
Цель этого учебника — показать, как интегрировать приложение Alcumus Info Exchange с Azure Active Directory (Azure AD).  
Интеграция приложения Alcumus Info Exchange с Azure AD обеспечивает следующие преимущества. 

* С помощью Azure AD вы можете контролировать доступ к Alcumus Info Exchange. 
* Вы можете включить автоматический вход пользователей в Alcumus Info Exchange (единый вход) с использованием учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно — через классический портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования
Чтобы настроить интеграцию Azure AD с Alcumus Info Exchange, вам потребуется:

* подписка [Azure AD](https://azure.microsoft.com/) ;
* подписка [Alcumus Info Exchange](http://www.alcumusgroup.com/) с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.
> 
> 

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

* Не следует использовать рабочую среду при отсутствии необходимости.
* Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Описание сценария
Цель этого учебника — научить вас проверять единый вход в Azure AD в пробной среде.  
Сценарий, описанный в этом учебнике, состоит из следующих основных блоков.

1. Добавление Alcumus Info Exchange из коллекции 
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Добавление Alcumus Info Exchange из коллекции
Чтобы настроить интеграцию Alcumus Info Exchange с Azure AD, вам нужно добавить Alcumus Info Exchange из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Alcumus Info Exchange из коллекции, выполните следующие действия:**

1. На **классическом портале Azure**в области навигации слева щелкните **Active Directory**. 
   
    ![Active Directory][1]
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения][2]
4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Приложения][3]
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Приложения][4]
6. В поле поиска введите **Alcumus Info Exchange**.
   
    ![Приложения][5]
7. В области результатов выберите **Alcumus Info Exchange** и щелкните **Завершить**, чтобы добавить приложение.
   
    ![Приложения][400]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
Цель этого раздела — показать, как настроить и проверить единый вход Azure AD в Alcumus Info Exchange с использованием тестового пользователя Britta Simon.

Для использования единого входа в Azure AD необходимо знать, какой пользователь в Alcumus Info Exchange соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Alcumus Info Exchange.  
Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения параметра **Username** (Имя пользователя) в Alcumus Info Exchange.

Чтобы настроить и проверить единый вход Azure AD в Alcumus Info Exchange, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Alcumus Info Exchange](#creating-a-alcumus-info-exchange-test-user)** требуется для создания пользователя Britta Simon в Alcumus Info Exchange, связанного с соответствующим представлением в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD
Цель этого раздела — включить единый вход Azure AD на классическом портале Azure и настроить единый вход в приложение Alcumus Info Exchange.

**Чтобы настроить единый вход Azure AD в Alcumus Info Exchange, выполните следующие действия:**

1. На классическом портале Azure на странице интеграции с приложением **Alcumus Info Exchange** нажмите кнопку **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа][6]
2. На странице **Как пользователи должны входить в Alcumus Info Exchange?** выберите параметр **Единый вход Microsoft Azure AD** и щелкните **Далее**.
   
    ![единого входа Azure AD][7]
3. В диалоговом окне на странице **Настройка параметров приложения** выполните следующие действия. 
   
    ![Единый вход в Azure AD][8]
   
    a. В текстовое поле **URL-адрес ответа** введите URL-адрес получателя, настроенный для вас службой поддержки Alcumus Info Exchange.
   
   > [!NOTE]
   > Если вы не знаете, какое значение нужно ввести, обратитесь в службу поддержки Alcumus Info Exchange по адресу [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).
   > 
   > 
   
    b. Нажмите кнопку **Далее**.
4. На странице **Настройка единого входа в Alcumus Info Exchange** щелкните **Скачать метаданные**, а затем сохраните файл метаданных локально на компьютере.
   
    ![Что такое Azure AD Connect?][9]
5. Отправьте файл метаданных службе поддержки Alcumus Info Exchange по адресу [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com)и попросите активировать для вас единый вход.
6. На классическом портале Azure подтвердите конфигурацию единого входа и нажмите кнопку **Далее**. 
   
    ![Что такое Azure AD Connect?][10]
7. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.  
   
    ![Что такое Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на классическом портале Azure тестового пользователя с именем Britta Simon.  

![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы отобразить список пользователей, в меню вверху выберите **Пользователи**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу нажмите кнопку **Добавить пользователя**. 
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 
5. На странице диалогового окна **Тип учетной записи пользователя** выполните следующие действия. 
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 
   
    а. В поле «Тип пользователя» выберите значение «Новый пользователь в вашей организации».
   
    b. В текстовом поле **Имя пользователя** введите **BrittaSimon**.
   
    c. Нажмите кнопку Далее.
6. На диалоговой странице **Профиль пользователя** выполните следующие действия: 
   
   ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 

    а. В текстовом поле **Имя** введите **Britta**.  

    b. В текстовом поле **Фамилия** введите **Simon**.

    c. В текстовом поле **Отображаемое имя** введите **Britta Simon**.

    d. В списке **Роль** выберите **Пользователь**.

    д. Нажмите кнопку **Далее**.


1. На странице диалогового окна **Получить временный пароль** нажмите кнопку **Создать**.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
2. На странице диалогового окна **Получить временный пароль** выполните следующие действия.
   
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 
   
    а. Запишите значение поля **Новый пароль**.
   
    b. Нажмите **Завершено**.   

### <a name="creating-a-alcumus-info-exchange-test-user"></a>Создание тестового пользователя Alcumus Info Exchange
Цель этого раздела — создать пользователя с именем Britta Simon в Alcumus Info Exchange.

**Чтобы создать пользователя с именем Britta Simon в Alcumus Info Exchange, выполните следующие действия:**

1. Отправьте файл метаданных службе поддержки Alcumus Info Exchange по адресу [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD
Цель этого раздела —позволить пользователю Britta Simon использовать единый вход Azure, предоставив ей доступ к Alcumus Info Exchange.

![Назначение пользователя][200]

**Чтобы назначить Britta Simon в Alcumus Info Exchange, выполните следующие действия:**

1. Чтобы открыть представление приложений, в представлении каталога на портале Azure нажмите **Приложения** в меню вверху.
   
    ![Назначение пользователя][201]
2. В списке приложений выберите **Alcumus Info Exchange**.
   
    ![Назначение пользователя][202]
3. В меню в верхней части страницы щелкните **Пользователи**.
   
    ![Назначение пользователя][203]
4. В списке пользователей выберите **Britta Simon**.
5. На панели инструментов внизу щелкните **Назначить**.
   
    ![Назначение пользователя][205]

### <a name="testing-single-sign-on"></a>Проверка единого входа
Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.  
Щелкнув элемент Alcumus Info Exchange на панели доступа, вы автоматически войдете в приложение Alcumus Info Exchange.

## <a name="additional-resources"></a>дополнительные ресурсы.
* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png



<!--HONumber=Dec16_HO4-->


