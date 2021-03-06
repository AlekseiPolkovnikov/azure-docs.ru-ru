---
title: "Автоматическая регистрация в Azure Active Directory присоединенных к домену устройств Windows | Документация Майкрософт"
description: "ИТ-администраторы могут автоматически и без предупреждений регистрировать устройства Windows, присоединенные к домену, с помощью Azure AD."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2d717f906bd4aad35cb46852baf84c4e088b953e


---
# <a name="automatic-device-registration-with-azure-active-directory-for-windows-domain-joined-devices"></a>Автоматическая регистрация в Azure Active Directory присоединенных к домену устройств Windows
ИТ-администраторы могут автоматически и без предупреждений зарегистрировать устройства Windows, присоединенные к домену, с помощью Azure Active Directory (Azure AD). Такую регистрацию удобно использовать, если вы настроили политики условного доступа на основе устройств для приложений Office 365 или приложений, которыми локально управляют службы федерации Active Directory (AD FS). Дополнительную информацию о сценариях регистрации устройств см. в разделе [Знакомство с регистрацией устройств в Azure Active Directory](active-directory-conditional-access-device-registration-overview.md).

> AZURE.NOTE Дополнительные сведения о настройке автоматической регистрации устройств см. в разделе [Настройка автоматической регистрации в Azure Active Directory присоединенных к домену устройств Windows](active-directory-conditional-access-automatic-device-registration-setup.md).
> 
> 

Автоматическая регистрация устройств в Azure AD доступна для компьютеров под управлением Windows 7 и Windows 8.1, присоединенных к домену Active Directory. Обычно это корпоративные компьютеры, которые предоставляются информационным работникам.

Чтобы приступить к регистрации присоединенных к домену устройств Windows в Azure AD, выполните предварительные требования, указанные ниже. Выполнив эти предварительные требования, настройте автоматическую регистрацию для своих присоединенных к домену устройств Windows.

## <a name="prerequisites-for-automatic-device-registration-of-domain-joined-windows-devices-with-azure-active-directory"></a>Предварительные требования для автоматической регистрации присоединенных к домену устройств Windows с помощью Azure AD
## <a name="deploy-ad-fs-and-connect-to-azure-active-directory-using-azure-active-directory-connect"></a>Развертывание AD FS и подключение к Azure Active Directory с помощью Azure Active Directory Connect
1. С помощью Azure Active Directory Connect разверните AD FS, используя Windows Server 2012 R2, и настройте федеративные отношения с Azure AD.
2. Настройка дополнительного правила для утверждений для отношений доверия с проверяющей стороной Azure AD
3. Откройте консоль управления AD FS и выберите **AD FS**>**Отношения доверия > Отношения доверия с проверяющей стороной**. Щелкните правой кнопкой мыши объект отношений доверия с проверяющей стороной платформы удостоверений Microsoft Office 365 и выберите **Изменить правила для утверждений...**
4. На вкладке **Правила преобразования выдачи** выберите **Добавить правило**.
5. Выберите **Отправка утверждений с помощью настраиваемого правила** из раскрывающегося списка шаблонов **Правило для утверждений**. Щелкните **Далее**.
6. Введите *Правило для утверждений метода проверки подлинности* в текстовом поле **Имя правила для утверждений:** .
7. Введите следующее правило для утверждений в текстовом поле **Правило для утверждений** :
   
        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);
8. Дважды щелкните **ОК** , чтобы закрыть диалоговое окно.

## <a name="configure-an-additional-azure-active-directory-relying-party-trust-authentication-class-reference"></a>Настройка ссылки класса аутентификации для отношений доверия с проверяющей стороной Azure Active Directory
На сервере федерации откройте командное окно Windows PowerShell и введите:

  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

где <RPObjectName> — имя объекта проверяющей стороны для объекта отношений доверия с проверяющей стороной Azure AD. Как правило, этот объект называется платформой удостоверений Microsoft Office 365.

## <a name="ad-fs-global-authentication-policy"></a>Глобальная политика аутентификации AD FS
Настройте глобальную основную политику аутентификации AD FS, чтобы разрешить встроенную аутентификацию Windows для интрасети (по умолчанию).

## <a name="internet-explorer-configuration"></a>Настройка Internet Explorer
Настройте следующие параметры в Internet Explorer на устройствах Windows для зоны безопасности локальной интрасети.

* Не запрашивать выбор сертификата клиента при наличии только одного сертификата: **включите**.
* Разрешить использование сценариев: **включите**.
* Автоматический вход только в зону интрасети: **установите флажок**.

Это параметры по умолчанию для зоны безопасности локальной интрасети Internet Explorer. Эти параметры можно просматривать, а также управлять ими в Internet Explorer. Для этого выберите **Свойства обозревателя** > **Безопасность** > "Локальная интрасеть" > "Другой". Вы также можете настроить эти параметры с помощью групповой политики Active Directory.

## <a name="network-connectivity"></a>Сетевое подключение
Для автоматической регистрации в Azure AD присоединенные к домену устройства Windows должны быть подключены к AD FS и контроллеру домена Active Directory. Как правило, это означает, что компьютер должен быть подключен к корпоративной сети. Это может быть проводное подключение, Wi-Fi-, VPN-подключение или подключение DirectAccess.

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Настройка обнаружения службы регистрации устройств Azure Active Directory
Устройства с Windows 7 и Windows 8.1 будут пытаться обнаружить сервер регистрации устройств, объединяя имя учетной записи пользователя с хорошо известным именем сервера регистрации устройств. Вам нужно создать запись DNS CNAME, которая указывает на запись A, связанную с вашей службой регистрации устройств Azure Active Directory. Запись CNAME должна использовать хорошо известный префикс **enterpriseregistration** , за которым следует суффикс имени участника-пользователя, используемый учетными записями пользователей в вашей организации. Если ваша организация использует несколько суффиксов имени участника-пользователя, следует создать несколько записей CNAME в DNS.

Например, если в вашей организации используется два суффикса имени участника-пользователя, @contoso.com и @region.contoso.com,, то вы создадите следующие записи DNS.

| Запись | Тип | Адрес |
| --- | --- | --- |
| enterpriseregistration.contoso.com |CNAME |enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com |CNAME |enterpriseregistration.windows.net |

## <a name="configure-automatic-device-registration-for-windows-7-and-windows-81-domain-joined-devices"></a>Настройка автоматической регистрации присоединенных к домену устройств Windows 7 и Windows 8.1
Настройте автоматическую регистрацию для своих присоединенных к домену устройств Windows 7 и Windows 8.1, используя ссылки ниже. Прежде чем продолжить, убедитесь, что выполнены предварительные требования, указанные выше.

* [Настройка автоматической регистрации присоединенных к домену устройств Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
* [Настройка автоматической регистрации присоединенных к домену устройств Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)
* [Автоматическая регистрация в Azure Active Directory присоединенных к домену устройств Windows 10](active-directory-azureadjoin-devices-group-policy.md)

## <a name="additional-notes"></a>Дополнительные замечания
Регистрация устройства в Azure AD предоставляет широкий набор возможностей. В Azure AD можно зарегистрировать как личные мобильные устройства (BYOD), так и присоединенные к домену устройства, которые принадлежат компании. Устройства можно использовать как с размещенными службами, например Office 365, так и со службами, управляемыми локально с помощью AD FS.

Компаниям, использующим и мобильные, и традиционные устройства, а также компаниям, использующим Office 365, Azure AD или другие службы Майкрософт, следует зарегистрировать свои устройства в Azure AD с помощью службы регистрации устройств Azure AD. Если в вашей компании не используют мобильные устройства, а также службы Майкрософт, такие как Office 365, Azure AD или Microsoft Intune, и используют только локальные приложения, можно выполнить регистрацию устройств в Active Directory с помощью AD FS.

Дополнительную информацию о развертывании службы регистрации устройств с помощью AD FS см. [здесь](https://technet.microsoft.com/library/dn486831.aspx).

## <a name="additional-topics"></a>Дополнительные разделы
* [Общие сведения о регистрации устройств в Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)
* [Настройка автоматической регистрации присоединенных к домену устройств Windows 7](active-directory-conditional-access-automatic-device-registration-windows7.md)
* [Настройка автоматической регистрации присоединенных к домену устройств Windows 8.1](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
* [Автоматическая регистрация в Azure Active Directory присоединенных к домену устройств Windows 10](active-directory-azureadjoin-devices-group-policy.md)




<!--HONumber=Nov16_HO3-->


