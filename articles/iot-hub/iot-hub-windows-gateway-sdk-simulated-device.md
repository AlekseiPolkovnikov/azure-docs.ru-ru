---
title: "Имитация устройства с помощью пакета SDK для шлюза Интернета вещей | Документация Майкрософт"
description: "Пошаговое руководство по работе с пакетом SDK для шлюза Azure IoT под управлением Windows показывает, как отправлять данные телеметрии с имитации устройства, используя пакет SDK для шлюза Azure IoT."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 00746fa67292fa6858980e364c88921d60b29460
ms.openlocfilehash: f5eeea933dbd5b63c6a8f2bd5b065a13286a4bae


---
# <a name="azure-iot-gateway-sdk--send-device-to-cloud-messages-with-a-simulated-device-app-using-windows"></a>Пакет SDK для шлюза Интернета вещей (бета-версия): отправка сообщений с устройства в облако через приложение имитации устройства с помощью Windows
[!INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Сборка и запуск примера
Перед началом работы необходимо выполнить следующие действия:

* [Настроить среду разработки][lnk-setupdevbox] для работы с пакетом SDK для Windows.
* [Создать Центр Интернета вещей][lnk-create-hub] в подписке Azure (для выполнения указаний данного пошагового руководства необходимо имя центра). Если у вас нет учетной записи, можно создать [бесплатную учетную запись][lnk-free-trial] всего за несколько минут.
* Добавьте в центр IoT два устройства и запишите их идентификаторы и ключи устройств. Чтобы добавить устройства в Центр Интернета вещей, созданный на предыдущем шаге, и получить его ключи, можно использовать такие инструменты, как [обозреватель устройств или iothub-explorer][lnk-explorer-tools].

Сборка примера

1. Откройте **командную строку разработчика для VS2015** .
2. Перейдите в корневую папку в локальной копии репозитория **azure-iot-gateway-sdk** .
3. Запустите сценарий **tools\\build.cmd**. Этот сценарий создает файл решения Visual Studio, собирает решение и проводит тесты. Решение Visual Studio находится в папке **build** локальной копии репозитория **azure-iot-gateway-sdk**.

Запуск примера

В текстовом редакторе откройте файл **samples\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** в локальной копии репозитория **azure-iot-gateway-sdk**. Этот файл настраивает модули в примере шлюза:

* Модуль **IoTHub** подключается к центру IoT. Его необходимо настроить для отправки данных в центр IoT. В частности, укажите в качестве значения **IoTHubName** имя своего Центра Интернета вещей, а в качестве значения **IoTHubSuffix** — **azure-devices.net**. Для параметра **Transport** задайте одно из следующих значений: HTTP, AMQP или MQTT. Обратите внимание, что в настоящее время только HTTP использует одно TCP-подключение для всех сообщений с устройства. Если задать значение AMQP или MQTT, то шлюз будет поддерживать отдельное TCP-подключение к Центру Интернета вещей для каждого устройства.
* Модуль **mapping** сопоставляет MAC-адреса имитаций устройств с идентификаторами устройств Центра Интернета вещей. Убедитесь в том, что значения **deviceId** совпадают с идентификаторами двух устройств, добавленных в Центр Интернета вещей, а значения **deviceKey** содержат ключи этих двух устройств.
* Модули **BLE1** и **BLE2** — это имитации устройств. Обратите внимание на то, что их MAC-адреса совпадают с адресами в модуле **mapping** .
* Модуль **Logger** регистрирует активность вашего шлюза в файл.
* Указанные ниже значения **module path** предполагают, что вы клонировали репозиторий пакета SDK для шлюза IoT в корневую папку диска **C:**. Если вы загрузили его в другое место, измените соответственно значения **module path** .
* Массив **links** в нижней части JSON-файла подключает модули **BLE1** и **BLE2** к модулю **mapping**, а модуль **mapping** — к модулю **IoTHub**. Это также гарантирует, что все сообщения будут зарегистрированы модулем **Logger** .

```
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Сохраните все изменения, внесенные в файл конфигурации.

Запуск примера

1. В командной строке перейдите в корневую папку в локальной копии репозитория **azure-iot-gateway-sdk** .
2. Выполните следующую команду:
   
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Для мониторинга сообщений, получаемых Центром Интернета вещей из шлюза, можно использовать такие инструменты, как [обозреватель устройств или iothub-explorer][lnk-explorer-tools].

## <a name="next-steps"></a>Дальнейшие действия
Если вы хотите подробнее изучить возможности пакета SDK для шлюза Интернета вещей и поэкспериментировать с примерами кода, то см. следующие учебники и ресурсы для разработчиков:

* [Пакет SDK для шлюза IoT (бета-версия): отправка сообщений с устройства в облако через реальное устройство с ОС Linux][lnk-physical-device]
* [Пакет SDK для шлюза Azure IoT][lnk-gateway-sdk]

Для дальнейшего изучения возможностей центра IoT см. следующие статьи:

* [Руководство разработчика][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 



<!--HONumber=Nov16_HO5-->


