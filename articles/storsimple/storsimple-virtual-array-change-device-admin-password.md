---
title: "Изменение пароля администратора устройства для доступа к виртуальному массиву Microsoft Azure StorSimple | Документация Майкрософт"
description: "В статье описано, как с помощью портала Azure или веб-интерфейса виртуального массива StorSimple изменить пароль администратора устройства."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/21/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: fd73672f97b4c16e49b2fad5e53042764f5793ca
ms.openlocfilehash: 4645ec88f804908916f7cf9b090376753c089119

---
# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Изменение пароля администратора для устройства виртуального массива StorSimple (предварительная версия)

## <a name="overview"></a>Обзор

При использовании интерфейса Windows PowerShell для доступа к виртуальному массиву StorSimple вам потребуется ввести пароль администратора устройства. Если подготовка и запуск устройства StorSimple выполняются впервые, по умолчанию используется пароль *Password1*. Для защиты ваших данных пароль по умолчанию становится недействительным после первого входа в систему и его необходимо будет изменить.

После развертывания устройства в рабочей среде можно в любой момент изменить пароль администратора устройства с помощью локального веб-интерфейса или портала Azure. Каждая из этих процедур описана в статье.

 ![Колонка "Устройства"](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-the-azure-portal-to-change-the-password"></a>Изменение пароля с помощью портала Azure

Чтобы изменить пароль администратора устройства с помощью портала Azure, выполните следующие действия.

#### <a name="to-change-the-device-administrator-password-via-the-azure-portal"></a>Изменение пароля администратора устройства с помощью портала Azure

1. На главной странице служб выберите службу, дважды щелкните ее имя, а затем в разделе **Управление** щелкните **Устройства**. Откроется колонка **Устройства**, в которой перечислены все устройства виртуального массива StorSimple.

2. В колонке **Устройства** дважды щелкните устройство, для которого нужно изменить пароль.

3. В колонке **Параметры** для устройства щелкните **Безопасность**.

4. В колонке **Параметры безопасности** выполните следующие действия.
   
   1. Прокрутите экран вниз, к разделу **Пароль администратора устройства** . Укажите пароль администратора длиной от 8 до 15 символов.
   2. Подтвердите пароль.
   3. Щелкните **Сохранить** в верхней части колонки.

Теперь пароль администратора устройства изменен. Этот новый пароль можно использовать для локального доступа к устройству.

![Колонка "Параметры безопасности"](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-the-local-web-ui-to-change-the-password"></a>Изменение пароля с помощью локального веб-интерфейса

Чтобы изменить пароль администратора устройства с помощью локального веб-интерфейса, выполните следующие действия.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Изменение пароля администратора устройства с помощью локального веб-интерфейса

1. В локальном веб-интерфейсе щелкните **Обслуживание** > **Изменение пароля** для своего устройства.
   
    ![изменить password1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Введите **текущий пароль**.
3. Укажите **новый пароль**. Пароль должен содержать не менее 8 символов. При этом он должен содержать как минимум три вида символов из следующих четырех: прописные буквы, строчные буквы, цифры и специальные символы.
   
    Обратите внимание, что текущий пароль не должен совпадать с 24 предыдущими.
4. Повторно введите пароль для подтверждения.
   
    ![изменить password2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. В нижней части страницы нажмите кнопку **Применить**. Теперь новый пароль применен. Если изменение пароля не произошло, появится следующее сообщение об ошибке.
   
    ![ошибочный пароль](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    После успешного обновления пароля вы получите соответствующее уведомление. Теперь новый пароль можно использовать для локального доступа к устройству.


## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [администрировать виртуальный массив StorSimple](storsimple-ova-web-ui-admin.md).




<!--HONumber=Nov16_HO4-->


