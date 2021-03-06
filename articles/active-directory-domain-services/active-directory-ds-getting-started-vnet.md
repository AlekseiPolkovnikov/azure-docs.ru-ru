---
title: "Доменные службы Azure AD: создание или выбор виртуальной сети | Документация Майкрософт"
description: "Приступая к работе с доменными службами Azure Active Directory"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/03/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9e933774e3b618b1584b4f24a0491eda49e42077


---
# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Создание или выбор виртуальной сети для доменных служб Azure AD
## <a name="guidelines-to-select-an-azure-virtual-network"></a>Рекомендации по выбору виртуальной сети Azure
> [!NOTE]
> **Перед началом работы**ознакомьтесь со статьей [Networking considerations for Azure AD Domain Services](active-directory-ds-networking.md)(Рекомендации по сетям для доменных служб Azure AD).
> 
> 

## <a name="task-2-create-an-azure-virtual-network"></a>Задача 2. Создание виртуальной сети Azure
Следующая задача по настройке — создать виртуальную сеть Azure c подсетью. Вам потребуется включить доменные службы Azure AD в этой подсети. Если у вас есть существующая виртуальная сеть, которую вы хотите использовать, этот шаг можно пропустить.

> [!NOTE]
> Убедитесь, что выбранная или создаваемая виртуальная сеть для использования с доменными службами Azure AD принадлежит к региону Azure, поддерживаемому доменными службами Azure AD. На странице [служб Azure по регионам](https://azure.microsoft.com/regions/#services/) можно узнать, в каких регионах Azure доступны доменные службы Azure AD.
> 
> 

Запишите имя виртуальной сети, чтобы выбрать нужную виртуальную сеть при включении доменных служб Azure AD на следующем шаге настройки.

Выполните следующие действия по настройке, чтобы создать виртуальную сеть Azure, для которой необходимо включить доменные службы Azure AD.

1. Перейдите на **классический портал Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Выберите узел **Сети** на панели слева.
   
    ![Узел "Сети"](./media/active-directory-domain-services-getting-started/networks-node.png)
3. Щелкните **СОЗДАТЬ** на панели задач в нижней части страницы.
   
    ![Узел "Виртуальные сети"](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. В узле **Сетевые службы** выберите **Виртуальная сеть**.
5. Щелкните **Быстрое создание** , чтобы создать виртуальную сеть.
   
    ![Виртуальная сеть — быстрое создание](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
6. Введите **Имя** для виртуальной сети. Также можно настроить **адресное пространство** или указать **максимальное число виртуальных машин** для этой сети. Для параметра **DNS-сервер** пока можно оставить значение "Нет". Этот параметр можно обновить после включения доменных служб Azure AD.
7. Убедитесь, что вы выбрали поддерживаемый регион Azure в раскрывающемся списке **Расположение** . На странице [служб Azure по регионам](https://azure.microsoft.com/regions/#services/) можно узнать, в каких регионах Azure доступны доменные службы Azure AD.
8. Чтобы создать виртуальную сеть, нажмите кнопку **Создать виртуальную сеть** .
   
    ![Создание виртуальной сети для доменных служб Azure AD.](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. После создания виртуальной сети выберите ее и перейдите на вкладку **Настройка**.
   
    ![Создание подсети](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. Перейдите к разделу **Адресное пространство виртуальной сети**. Щелкните **Добавить подсеть** и укажите подсеть с именем **AaddsSubnet**. Щелкните **Сохранить**, чтобы создать подсеть.
    
    ![Создание подсети для доменных служб Azure AD.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Задача 3. Включение доменных служб Azure AD
Следующая задача конфигурации — [включить доменные службы Azure AD](active-directory-ds-getting-started-enableaadds.md).




<!--HONumber=Dec16_HO2-->


