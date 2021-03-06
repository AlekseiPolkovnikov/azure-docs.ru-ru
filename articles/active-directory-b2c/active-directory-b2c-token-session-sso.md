---
title: "Azure Active Directory B2C: настройка маркера, сеанса и единого входа | Документация Майкрософт"
description: "Конфигурация маркера, сеанса и единого входа в Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b37d419f799e5a56c67344ca634bdecec2f3c1f2


---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: конфигурация маркера, сеанса и единого входа
Эта возможность обеспечивает более точный контроль указанных далее параметров, осуществляемый [для каждой политики](active-directory-b2c-reference-policies.md).

1. Время существования маркеров безопасности, выдаваемых Azure Active Directory (Azure AD) B2C.
2. Время существования сеансов веб-приложения, управляемых Azure AD B2C.
3. Поведение единого входа (SSO) в нескольких приложениях и политиках в клиенте B2C.

Далее приводятся указания по использованию этой функции в клиенте B2C.

1. Выполните эти действия, чтобы [перейти к колонке функций B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) на портале Azure.
2. Щелкните **Политики входа**. *Примечание. Эту функцию можно использовать с любым типом политики, а не только с **политиками входа***.
3. Откройте политику, щелкнув ее. Например, щелкните **B2C_1_SiIn**.
4. Щелкните **Изменить** в верхней части колонки.
5. Щелкните **Конфигурация маркера, сеанса и единого входа**.
6. Внесите нужные изменения. Сведения о доступных свойствах см. в последующих разделах.
7. Нажмите кнопку **ОК**.
8. Нажмите кнопку **Сохранить** в верхней части колонки.

![Снимок экрана конфигурации маркера, сеанса и единого входа](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Конфигурация времени существования маркеров
Azure AD B2C поддерживает [протокол авторизации OAuth 2.0](active-directory-b2c-reference-protocols.md) для включения безопасного доступа к защищенным ресурсам. Для реализации этой поддержки Azure AD B2C выдает различные [маркеры безопасности](active-directory-b2c-reference-tokens.md). Ниже приведены свойства, которые можно использовать для управления временем существования маркеров безопасности, выдаваемых Azure AD B2C.

* **Время существования маркера доступа и маркера идентификатора (в минутах)**: время существования маркера носителя OAuth 2.0, используемого для получения доступа к защищенному ресурсу. На данный момент Azure AD B2C выдает только маркеры идентификаторов. Это значение также будет применяться к маркерам доступа после того, как будет добавлена их поддержка.
  * Значение по умолчанию — 60 минут.
  * Минимум (включительно) — 5 минут.
  * Максимум (включительно) — 1440 минут.
* **Время существования маркера обновления (в днях)**. Максимальный период времени, до наступления которого маркер обновления может использоваться для получения нового маркера доступа или маркера идентификатора (и при необходимости для получения нового маркера обновления, если приложению была предоставлена область `offline_access`).
  * Значение по умолчанию — 14 дней.
  * Минимум (включительно) — 1 день.
  * Максимум (включительно) — 90 дней.
* **Скользящее время существования маркера обновления (в днях)**. По истечении этого периода времени пользователь должен будет повторно пройти проверку подлинности независимо от срока действия самого последнего маркера обновления, полученного приложением. Он предоставляется, только если установлен переключатель **Ограничено**. Значение должно быть больше или равно значению **времени существования маркера обновления (в днях)**. Если установлен переключатель **Не ограничено**, указать конкретное значение нельзя.
  * Значение по умолчанию — 90 дней.
  * Минимум (включительно) — 1 день.
  * Максимум (включительно) — 365 дней.

Ниже приведено несколько вариантов использования, которые можно реализовать с помощью этих свойств.

* Предоставление пользователю возможности находиться в мобильном приложении сколь угодно долго до тех пор, пока он активно его использует. Для этого в политике входа нужно установить переключатель **Скользящее время существования маркера обновления (в днях)** в положение **Не ограничено**.
* Обеспечение соответствия нормам и требованиям безопасности путем задания соответствующих значений времени существования маркера доступа.

## <a name="session-configuration"></a>Конфигурация сеанса
Azure AD B2C поддерживает [протокол проверки подлинности OpenID Connect](active-directory-b2c-reference-oidc.md) для включения безопасного входа в веб-приложения. Ниже приведены свойства, которые можно использовать для управления сеансами веб-приложения.

* **Время существования сеанса веб-приложения (в минутах)**. Время существования файла cookie сеанса Azure AD B2C, сохраненного в браузере пользователя после успешной проверки подлинности.
  * Значение по умолчанию — 1440 минут.
  * Минимум (включительно) — 15 минут.
  * Максимум (включительно) — 1440 минут.
* **Время ожидания сеанса веб-приложения**. Если этот переключатель установлен в положение **Абсолютное**, пользователь вынужден повторно пройти проверку подлинности по истечении периода времени, заданного параметром **Время существования сеанса веб-приложения (в минутах)**. Если этот переключатель установлен в положение **Скользящее** (по умолчанию), пользователь может находиться в веб-приложении сколь угодно долго до тех пор, пока он активно его использует.

Ниже приведено несколько вариантов использования, которые можно реализовать с помощью этих свойств.

* Обеспечение соответствия нормам и требованиям безопасности путем задания соответствующих значений времени существования сеанса веб-приложения.
* Применение принудительной повторной проверки подлинности по истечении заданного периода во время взаимодействия пользователя с высокозащищенной частью веб-приложения. 

## <a name="single-sign-on-sso-configuration"></a>Конфигурация единого входа (SSO)
Если в клиенте B2C существует несколько приложений и политик, для управления взаимодействиями пользователей с ними можно использовать свойство **Конфигурация единого входа** . Этому свойству можно задать одно из указанных далее значений.

* **Клиент**. Это значение по умолчанию. Если выбрано это значение, несколько приложений и политик в клиенте B2C могут совместно использовать один и тот же сеанс пользователя. Например, когда пользователь входит в приложение Contoso Shopping, он (или она) также может легко войти в другое приложение — Contoso Pharmacy — непосредственно во время доступа к нему.
* **Приложение**. Это значение позволяет поддерживать сеанс пользователя исключительно для одного приложения и независимо от других приложений. Например, если требуется, чтобы пользователь выполнял вход в приложение Contoso Pharmacy (с теми же учетными данными), даже если он (или она) уже выполнил вход в Contoso Shopping, другое приложение на том же клиенте B2C. 
* **Политика**. Это значение позволяет поддерживать сеанс пользователя исключительно для политики и независимо от приложений, ее использующих. Например, если пользователь уже выполнил вход и прошел этап многофакторной проверки подлинности (MFA), он может получить доступ к более защищенным частям нескольких приложений, который будет действовать, пока не истечет срок действия сеанса, связанного с политикой.
* **Отключено**. Если задано это значение, пользователь должен осуществлять все действия по входу при каждом выполнении политики. Например, несколько пользователей могут зарегистрироваться в приложении (в сценарии с использованием общего рабочего стола), даже если в течение всего времени в приложении остается один пользователь.




<!--HONumber=Nov16_HO3-->


