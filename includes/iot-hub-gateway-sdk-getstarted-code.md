## <a name="typical-output"></a>Типовые выходные данные
Ниже представлен пример выходных данных, записанных в файл журнала примером Hello World. Символы табуляции и перевода строки добавлены для удобочитаемости.

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Фрагменты кода
В этом разделе рассматриваются некоторые основные части кода в примере Hello World.

### <a name="gateway-creation"></a>Создание шлюза
Разработчик должен написать *процесс шлюза*. Эта программа создает внутреннюю инфраструктуру (брокер), загружает модули и настраивает все для правильной работы. Пакет SDK предоставляет функцию **Gateway_Create_From_JSON** для начальной загрузки шлюза из JSON-файла. Для использования функции **Gateway_Create_From_JSON** необходимо передать ей путь к JSON-файлу, в котором указано, какие модули нужно загрузить. 

Код для процесса шлюза можно найти в примере Hello World в файле [main.c][lnk-main-c]. В целях удобочитаемости представленный ниже фрагмент кожа содержит сокращенную версию кода для процесса шлюза. Эта программа создает шлюз и разбирает шлюз только после того, как пользователь нажмет клавишу **ВВОД** . 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
} 
```

Файл параметров JSON содержит список модулей для загрузки модулей и связи между ними.
Каждый модуль должен содержать:

* **name** — уникальное имя модуля;
* **loader** — загрузчик, который знает, как загрузить нужный модуль.  Загрузчики являются точкой расширения для загрузки разных типов модулей. Мы предоставляем загрузчики для модулей, написанных на машинном коде C, Node.js, Java и .Net. Пример Hello World использует только загрузчик для машинного кода, так как в качестве модулей здесь используются только динамические библиотеки на языке C. Сведения об использовании модулей на других языках вы найдете в описании примеров для [Node.js](https://github.com/Azure/azure-iot-gateway-sdk/blob/develop/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/azure-iot-gateway-sdk/tree/develop/samples/java_sample) и [.Net](https://github.com/Azure/azure-iot-gateway-sdk/tree/develop/samples/dotnet_binding_sample).
    * **name** — имя загрузчика, используемого для загрузки модуля;  
    * **entrypoint** — путь к библиотеке, содержащей модуль. Для Linux это файл SO, для Windows — файл DLL. Обратите внимание, что точка входа зависит от типа используемого загрузчика. Например, загрузчик Node.js использует в качестве точки входа JS-файл, загрузчик Java — путь и имя класса, а загрузчик .Net — имя сборки и имя класса.

* **args**: необходимые модулю данные конфигурации.

Далее представлен код JSON, который используется для объявления всех модулей для примера Hello World в Linux. Необходимость аргумента для модуля зависит от структуры этого модуля. В этом примере модуль ведения журнала принимает в качестве аргумента путь к выходному файлу, а модуль Hello World не принимает никакие аргументы.

```
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

JSON-файл также содержит ссылки между модулями, которые будут передаваться в брокер. Ссылка имеет два свойства:

* **source** — имя модуля из раздела `modules` или "\*".
* **sink** — имя модуля из раздела `modules`.

Каждая ссылка определяет маршрут и направление сообщения. Сообщения из модуля `source` должны быть доставлены в модуль `sink`. Для свойства `source` может быть задано значение "\*", указывающее, что `sink` может получать сообщения из любого модуля.

Далее представлен код JSON, который используется для настройки связей между модулями для примера Hello World в ОС Linux. Каждое сообщение, созданное модулем `hello_world`, будет использоваться модулем `logger`.

```
"links": 
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="hello-world-module-message-publishing"></a>Публикация сообщений модуля Hello World
Код, использованный модулем Hello World для публикации сообщений, можно найти в файле [hello_world.c][lnk-helloworld-c]. В следующем фрагменте показана измененная версия — в целях удобочитаемости добавлены комментарии и удалена часть кода для обработки ошибок:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Обработка сообщений модуля Hello World
Модулю Hello World не приходится обрабатывать сообщения, публикуемые другими модулями в брокере. В связи с этим реализация обратного вызова сообщений в модуле Hello World становится невозможной.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Публикация и обработка сообщений модуля ведения журнала
Модуль ведения журнала получает сообщения из брокера и записывает их в файл. Он никогда не публикует сообщения. Это значит, что код модуля ведения журнала никогда не вызывает функцию **Broker_Publish**.

Функция **Logger_Receive** в файле [logger.c][lnk-logger-c] представляет собой обратный вызов, который брокер вызывает для доставки сообщений в модуль ведения журнала. В следующем фрагменте показана измененная версия — в целях удобочитаемости добавлены комментарии и удалена часть кода для обработки ошибок:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Дальнейшие действия
Информацию об использовании пакета SDK для шлюза IoT см. по следующим ссылкам.

* [Пакет SDK для шлюза Интернета вещей: отправка сообщений с устройства в облако через виртуальное устройство с помощью Linux][lnk-gateway-simulated].
* [Пакет SDK для шлюза Интернета вещей Azure][lnk-gateway-sdk] на GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Dec16_HO1-->


