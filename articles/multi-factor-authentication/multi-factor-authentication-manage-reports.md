---
title: "Отчеты Azure Multi-Factor Authentication"
description: "Эта статья содержит информацию о том, как использовать функцию отчетов службы Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 738f9862ce8a8e4a0d63ec94583c4c868d914686


---
# <a name="reports-in-azure-multi-factor-authentication"></a>Отчеты в службе Azure Multi-Factor Authentication
Azure Multi-Factor Authentication предоставляет несколько типов отчетов, которые могут быть полезны для вас и вашей организации. Эти отчеты доступны через портал управления Multi-Factor Authentication. Для использования портала у вас должен быть поставщик MFA или лицензия Azure MFA, Azure AD Premium или Enterprise Mobility Suite. Ниже приведен список доступных отчетов.

Чтобы получить доступ к отчетам, войдите на портал управления Azure.

| Имя | Описание |
|:--- |:--- |
| Использование |В отчетах об использовании содержится информация об общем использовании, сводный отчет по пользователям и сведения о пользователях. |
| Состояние серверов |В этом отчете отображается состояние серверов Multi-Factor Authentication, с которыми связана ваша учетная запись. |
| Журнал заблокированных пользователей |В этих отчетах отображается журнал запросов о блокировании и разблокировании пользователей. |
| Журнал обойденных пользователей |Отображается журнал запросов об обходе службы Multi-Factor Authentication в случае использования того или иного номера телефона. |
| Предупреждение о мошенничестве |Отображается история оповещений о мошенничестве, отправленных в рамках указанного диапазона дат. |
| Поставлено в очередь |Отображается список отчетов, поставленных в очередь для обработки, и их состояние. Ссылка на загрузку или просмотр отчета предоставляется по завершении его составления. |

## <a name="to-view-reports"></a>Просмотр отчетов
1. Войдите на сайт http://azure.microsoft.com
2. Выберите слева элемент Active Directory.
3. Выберите один из следующих вариантов:
   * **Вариант 1**: перейдите на вкладку "Поставщики Multi-Factor Authentication". Выберите поставщика MFA и нажмите кнопку "Управление" в нижней части окна.
   * **Вариант 2**: выберите свой каталог и перейдите на вкладку "Настройка". В разделе многофакторной проверки подлинности щелкните "Управление параметрами службы". В нижней части страницы "Параметры службы MFA" щелкните "Перейти на портал".
4. На портале управления Azure Multi-Factor Authentication в меню слева вы увидите пункт "Просмотреть отчет". В этом разделе можно выбирать описанные выше отчеты.

<center>![Облако](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Дополнительные ресурсы**

* [Для пользователей](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication в MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)




<!--HONumber=Nov16_HO3-->


