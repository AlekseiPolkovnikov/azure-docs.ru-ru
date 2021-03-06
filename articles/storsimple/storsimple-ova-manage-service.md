---
title: Развертывание службы диспетчера StorSimple для виртуального массива StorSimple| Microsoft Docs
description: Сведения о создании и удалении службы диспетчера StorSimple на классическом портале Azure, а также об управлении ключом регистрации службы.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''

ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/19/2016
ms.author: alkohli

---
# Развертывание службы диспетчера StorSimple для виртуального массива StorSimple
## Обзор
Служба диспетчера StorSimple выполняется в Microsoft Azure и подключается к нескольким устройствам StorSimple. После создания службы вы можете использовать ее для управления этими устройствами на классическом портале Microsoft Azure, запущенном в браузере. Это позволит отследить все устройства, которые подключены к службе диспетчера StorSimple из одного центрального расположения, таким образом, снизив административную нагрузку.

На главной странице диспетчера StorSimple перечислены все службы диспетчера StorSimple, которые можно использовать для управления запоминающими устройствами StorSimple. Для каждой службы диспетчера StorSimple на странице диспетчера StorSimple представлена следующая информация:

* **Имя** — имя, назначенное вашей службе диспетчера StorSimple при ее создании. Имя службы невозможно изменить после ее создания.
* **Состояние** — состояние службы. Оно может быть **Работает**, **Идет создание** или **В сети**.
* **Местоположение** — географическое местоположение, в котором будет развернуто устройство StorSimple.
* **Подписка** — подписка выставления счетов, связанная с вашей службой.

Распространенные задачи, которые можно выполнить на странице диспетчера StorSimple:

* создание службы;
* удаление службы;
* Получение регистрационного ключа службы
* повторное создание ключа регистрации службы.

В этом учебнике описано, как выполнить каждую из этих задач. Сведения в этой статье применимы только к виртуальным массивам StorSimple. Дополнительные сведения о серии StorSimple 8000 см. в разделе о [развертывании службы диспетчера StorSimple](storsimple-manage-service.md).

## создание службы;
Выберите параметр **Быстрое создание**, чтобы создать службу диспетчера StorSimple, если вы хотите развернуть свое устройство StorSimple. Для создания службы требуется следующее:

* подписка с соглашением Enterprise;
* активная учетная запись хранения Microsoft Azure;
* сведения о выставлении счетов, которые используются для управления доступом.

Вы также можете создать учетную запись хранения по умолчанию во время создания службы.

Одна служба может управлять несколькими устройствами. Однако устройство не может охватить несколько служб. В большую организацию может входить несколько экземпляров служб для работы с разными подписками, организациями или даже местоположениями развернутых служб.

> [!NOTE]
> Для управления виртуальными массивами StorSimple и устройствами StorSimple серии 8000 необходимо создать отдельные экземпляры службы диспетчера StorSimple.
> 
> 

Выполните следующие действия, чтобы создать службу.

[!INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## удаление службы;
Перед удалением службы убедитесь в отсутствии подключенных устройств, которые используют ее. Если служба используется, деактивируйте подключенные устройства. Операция деактивации разорвет подключение между устройством и службой, но сохранит данные устройства в облаке.

> [!IMPORTANT]
> Операцию удаления службы невозможно обратить.
> 
> 

Чтобы удалить службу, выполните указанные ниже действия.

### Удаление службы
1. На странице **Служба диспетчера StorSimple** выберите службу, которую вы хотите удалить.
2. Щелкните **Удалить** в нижней части страницы.
3. Щелкните **Да** в уведомлении о подтверждении. Удаление службы может занять несколько минут.

## Получение регистрационного ключа службы
После успешного создания службы потребуется зарегистрировать устройство StorSimple в службе. Для регистрации первого устройства StorSimple необходим ключ регистрации службы. Для регистрации дополнительных устройств в существующей службе StorSimple потребуется ключ регистрации и ключ шифрования данных службы (который создается на первом устройстве во время регистрации). Дополнительные сведения о ключе шифрования данных службы см. в разделе [Получение ключа шифрования данных службы из локального пользовательского веб-интерфейса](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

Выполните следующие действия, чтобы получить ключ регистрации.

[!INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Сохраните ключ регистрации в безопасном расположении. Этот ключ, а также ключ шифрования данных службы потребуется для регистрации дополнительных устройств в службе. После получения ключа регистрации службы вам потребуется настроить устройство в интерфейсе Windows PowerShell для StorSimple.

## повторное создание ключа регистрации службы.
Если вам необходимо сменить ключ или при изменении списка администраторов службы потребуется повторно создать ключ регистрации службы. При повторном создании ключа новый ключ используется только для регистрации последующих устройств. Этот процесс не затронет уже зарегистрированные устройства.

Выполните следующие действия, чтобы повторно создать ключ регистрации службы.

### Повторное создание ключа регистрации службы
1. На странице **Службы диспетчера StorSimple** щелкните **Ключ регистрации**.
2. В диалоговом окне **Ключ регистрации службы** щелкните **Повторно создать**.
3. Появится сообщение подтверждения. Нажмите кнопку **ОК**, чтобы продолжить повторное создание ключа.
4. Появится новый ключ регистрации службы.
5. Скопируйте этот ключ и сохраните его для регистрации новых устройств в службе.
6. Щелкните значок галочки (![значок с изображением флажка](./media/storsimple-ova-manage-service/image7.png)), чтобы закрыть это диалоговое окно.

## Дальнейшие действия
* Узнайте, как [приступить к работе](storsimple-ova-deploy1-portal-prep.md) с виртуальным массивом StorSimple.
* Узнайте, как [администрировать устройство StorSimple](storsimple-ova-web-ui-admin.md).

<!---HONumber=AcomDC_0525_2016-->