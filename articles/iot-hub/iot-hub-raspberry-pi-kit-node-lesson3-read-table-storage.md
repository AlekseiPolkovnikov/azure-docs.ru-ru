---
title: "Чтение сообщений, сохраненных в службе хранилища Azure | Документация Майкрософт"
description: "Отслеживайте сообщения, передаваемые с устройства в облако, по мере их записывания в хранилище таблиц Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "получить данные из облака, облачная служба Интернета вещей"
ms.assetid: 9965bd54-61da-479b-aaad-5604850a2be5
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: ffcb9214b8fa645a8a2378c5e7054b9f984addbb
ms.openlocfilehash: d741d9be27c17171a9161b7e0ea335a9f5fbafc0


---
# <a name="read-messages-persisted-in-azure-storage"></a>Чтение сообщений, сохраненных в службе хранилища Azure
## <a name="what-you-will-do"></a>Выполняемая задача
Отслеживание сообщений, передаваемых с устройства в облако (то есть с устройства Raspberry Pi 3 в центр Интернета вещей), по мере записывания сообщений в хранилище таблиц Azure. Если возникнут какие-либо проблемы, то решения можно найти на [странице со сведениями об устранении неполадок](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Новые знания
В этой статье вы узнаете, как использовать задачу gulp read-message для чтения сообщений, сохраненных в хранилище таблиц Azure.

## <a name="what-you-need"></a>Необходимые элементы
Прежде чем начинать, необходимо успешно выполнить инструкции статьи, посвященный [запуску на устройстве Raspberry Pi 3 примера приложения Azure для включения индикатора](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="read-new-messages-from-your-storage-account"></a>Чтение новых сообщений из учетной записи хранения
В предыдущей статье вы запустили пример приложения на устройстве Pi. Пример приложения отправлял сообщения в центр Интернета вещей Azure. Сообщения, отправленные в центр Интернета вещей, сохраняются в хранилище таблиц Azure с помощью приложения-функции Azure. Для чтения сообщений из хранилища таблиц Azure потребуется строка подключения к хранилищу Azure.

Чтобы прочитать сообщения, сохраненные в хранилище таблиц Azure, выполните следующие действия:

1. Получите строку подключения, выполнив следующую команду:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Первая команда получает имя `storage name`, которое используется во второй команде для получения строки подключения. Используйте `iot-sample` в качестве значения `{resource group name}`, если вы не меняли это значение.
2. Откройте файл конфигурации `config-raspberrypi.json` в Visual Studio Code, выполнив следующую команду.

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Замените `[Azure storage connection string]` строкой подключения, полученной на шаге 1.
4. Сохраните файл `config-raspberrypi.json`.
5. Отправьте сообщения еще раз и считайте их из хранилища таблиц Azure, выполнив следующую команду.
   
   ```bash
   gulp run --read-storage
   ```
   
   Логика чтения из хранилища таблиц Azure содержится в файле `azure-table.js`.
   
    ![gulp run --read-storage](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a>Сводка
Вы успешно подключили устройство Pi к Центру Интернета вещей в облаке и с помощью примера приложения для включения индикатора отправили сообщения с устройства в облако. Также вы использовали приложение-функцию Azure для сохранения входящих сообщений из Центра Интернета вещей в хранилище таблиц Azure. Теперь можно перейти к отправке сообщений из облака на устройство, т. е. из Центра Интернета вещей на устройство Pi.

## <a name="next-steps"></a>Дальнейшие действия
[Run the sample application to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md) (Запуск примера приложения для получения сообщений из облака на устройство)




<!--HONumber=Nov16_HO5-->


