---
title: Приступая к работе с мобильными службами Azure и Sencha
description: Следуйте указаниям этого учебника, чтобы приступить к разработке с использованием мобильных служб и платформы мобильных приложений Sencha HTML5.
services: mobile-services
documentationcenter: ''
author: ggailey777
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-sencha
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: glenga

---
# <a name="getting-started"> </a>Начало работы с мобильными службами и Sencha Touch
[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

## Обзор
В этом учебнике показано, как использовать мобильные службы Azure в приложении Sencha Touch. Вы создадите простое приложение *списка дел* с помощью системы Sencha Touch, которая использует мобильные службы, определяемые на классическом портале Azure. Этот учебник предназначен для разработчиков веб-приложений среднего и высокого уровня, которые хорошо понимают принципы работы JavaScript и знакомы с платформой Sencha Touch.

Кроме того, можно посмотреть видеоклип (см. ниже), где приведены те же действия, что и в учебнике. В этом видеоклипе Артур Кэй (Arthur Kay) объясняет, как создать приложение Sencha Touch с помощью серверного компонента мобильных служб Azure.

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-Windows-Azure-for-Sencha-Touch/player]
> 
> 

Снимок экрана завершенного приложения приведен ниже:

![][0]

## Требования
* Скачайте и установите [Sencha Touch](http://wwww.sencha.com/products/touch/download" target="_blank").
* Скачайте и установите [Sencha Cmd Tool](http://www.sencha.com/products/sencha-cmd/download" target="_blank").
* Среда выполнения Java (JRE) или набор разработки Java (при создании приложений Android)
* Пакет Ruby и SASS.

## <a name="create-new-service"> </a>Создание мобильной службы
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Создание таблицы TodoItems
После создания мобильной службы можно выполнить простые действия для быстрого запуска на классическом портале Azure, чтобы создать новую таблицу базы данных для использования в мобильной службе.

1. На [классическом портале Azure] щелкните **Мобильные службы** и выберите только что созданную мобильную службу.
2. На вкладке быстрого запуска нажмите **HTML** в разделе **Выбор платформы**, а затем разверните пункт **Создать новое приложение HTML**.
   
    ![HTML-код быстрого запуска для мобильных служб](./media/partner-sencha-mobile-services-get-started/mobile-portal-quickstart-html.png)
   
    Здесь показаны три простых шага для создания и размещения приложения HTML, подключенного к вашей мобильной службе.
   
    ![HTML-код быстрого запуска для мобильных служб](./media/partner-sencha-mobile-services-get-started/mobile-quickstart-steps-html.png)
3. Чтобы создать таблицу для хранения данных приложения, нажмите кнопку **Создание таблицы TodoItems**.
   
   > [!NOTE]
   > НЕ загружайте HTML-приложение с классического портала Azure. Вместо этого мы вручную создадим приложение Sencha Touch в следующем разделе.
   > 
   > 
4. Запишите значения **appKey** и **appUrl** на классическом портале Azure. Они нам понадобятся в других разделах данного учебника.
   
    ![ключ приложения](./media/partner-sencha-mobile-services-get-started/mobile-app-key-portal.png)
5. На вкладке **Настройка** убедитесь, что элемент `localhost` уже указан в списке **Разрешить запросы от имен узлов** в разделе **Общий доступ к ресурсам независимо от источника (CORS)**. Если этот элемент не указан, введите `localhost` в поле **Имя узла** и нажмите кнопку **Сохранить**.
   
    ![Настройка CORS для localhost](./media/partner-sencha-mobile-services-get-started/mobile-services-set-cors-localhost.png)

## Создание приложения Touch
Создание приложения на базе шаблона Sencha Touch не вызывает затруднений при использовании программы командной строки Sencha Cmd и позволяет очень быстро настроить приложение и начать работу с ним.

Из каталога с установленной платформой Touch выполните следующую команду:

    $ sencha generate app Basic /path/to/application

Это создает шаблон приложения Touch, где приложение имеет имя "Basic". Для запуска приложения просто укажите в браузере путь к приложению, и вы увидите стандартный пример приложения Touch.

## Установка расширений Sencha Touch для Azure
Расширение для Azure устанавливается вручную или в виде пакета Sencha. Вы сами решаете, какой способ использовать.

### Ручная установка
Если вы хотите добавить внешнюю библиотеку классов, в большинстве приложений Touch достаточно загрузить пакет, распаковать его в каталог приложения и настроить загрузчик Touch, указав расположение библиотеки.

Расширения Azure можно добавить в приложение вручную, выполнив следующие действия:

1. Загрузите пакет расширений Azure [здесь](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure). (Для доступа к этой области может потребоваться идентификатор Sencha Forums.)
2. Скопируйте пакет расширений Azure из каталога загрузки в нужное место и распакуйте его:
   
        $ cd /path/to/application
        $ mv /download-location/azure.zip .
        $ unzip azure.zip
   
    При этом создается каталог **azure**, содержащий весь исходный код, примеры и документацию из пакета. Исходный код будет находиться в каталоге **azure/src**.

### Установка в виде пакета Sencha
> [!NOTE]
> Этот метод можно использовать только в случае, если приложение было создано с помощью команды <code>sencha generate app</code>.
> 
> 

Все приложения, создаваемые программой командной строки Sencha Cmd, имеют в корне папку «packages». Расположение этой папки можно настроить, однако независимо от расположения папка "packages" используется в качестве хранилища всех пакетов, используемых приложением (или приложениями при создании рабочей области Sencha).

Поскольку Ext.Azure является "пакетом" Sencha Cmd, исходный код можно легко установить и включить в приложение с помощью Sencha Cmd. (Дополнительные сведения см. в разделе [Пакеты Sencha Cmd](http://docs.sencha.com/cmd/6.x/cmd_packages/cmd_packages.html).)

Чтобы загрузить и установить пакет расширений Azure из репозитория пакетов Sencha, необходимо добавить имя этого пакета в ваш файл **app.json** и выполнить построение приложения:

1. Добавьте пакет Azure в раздел "requires" файла app.json:
   
        {
            "name": "Basic",
            "requires": [
                "touch-azure"
            ]
        }
2. Повторно выполните построение приложения, используя **sencha cmd** для извлечения и установки пакета:
   
        $ sencha app build

Теперь как **sencha app build**, так и **sencha app refresh** выполняют действия, необходимые для интеграции пакета в приложение. В общем случае после изменения требований пакета требуется запустить **sencha app refresh**, чтобы метаданные, необходимые для поддержки "режима разработки", находились в актуальном состоянии.

При выполнении любой из команд Sencha скачивает и развертывает пакет в папку packages. После этого вы увидите в рабочей области папку "packages/touch-azure".

## Включение и настройка Azure
**Имя файла**: app.js

После того, как расширение Azure загружено и установлено в каталоге приложения, следует указать приложению, где можно найти исходные файлы, и запросить эти файлы:

1. Настройте расположение исходного кода для загрузчика Sencha Loader.
   
        Ext.Loader.setConfig({
               enabled : true,
               paths   : {
                   'Ext'       : 'touch/src',
                   'Ext.azure' : '/path-to/azure-for-touch/azure/src'
            }
        });
2. Запросите файлы классов Azure:
   
        Ext.application({
   
            requires: [ 'Ext.azure.Azure' ],
   
            // ...
   
        });
3. Настройка Azure
   
    Пакет Azure инициализируется путем вызова метода **Ext.Azure.init** в разделе "launch" вашего приложения. В этот метод передается объект конфигурации, содержащий учетные данные мобильных служб, а также другие учетные данные и компоненты, которые вы хотите использовать.
   
    Хотя объект конфигурации можно передать непосредственно в метод "init", рекомендуется создать свойство конфигурации приложения Sencha с именем **azure** и поместить в него всю соответствующую информацию. Затем значение этого свойства можно передать в метод Ext.Azure.init.
   
    При создании мобильной службы в Azure (см. раздел [Приступая к работе с Azure](http://senchaazuredocs.azurewebsites.net/#!/guide/getting_started)) ей назначается ключ приложения и URL-адрес. Эти сведения необходимо предоставить пакету Azure, чтобы позволить ему подключиться к службе.
   
    В этом примере показана очень простая настройка и инициализация Azure, при которой указывается только ключ приложения и URL-адрес:
   
        Ext.application({
            name: 'Basic',
   
            requires: [ 'Ext.azure.Azure' ],
   
            azure: {
                appKey: 'myazureservice-access-key',
                appUrl: 'myazure-service.azure-mobile.net'
            },
   
            launch: function() {
   
                // Call Azure initialization
   
                Ext.Azure.init(this.config.azure);
   
            }
        });
   
    Дополнительные сведения о параметрах настройки Azure см. в документации по API Ext.Azure.

Поздравляем! Приложение теперь должно иметь доступ к вашей мобильной службе.

## Построение приложения списка дел
После настройки приложения для включения в него расширения Azure и предоставления необходимых учетных данных можно перейти к созданию приложения Touch, которое использует мобильную службу для просмотра и редактирования хранящихся в службе данных списка дел.

### Настройка прокси-сервера данных Azure
**Имя файла:** app/model/TodoItem.js

Приложение Touch будет взаимодействовать с мобильной службой через прокси-сервер данных. Этот прокси-сервер выполняет всю работу по отправке запросов в мобильную службу и получению данных из нее. Используя его совместно с хранилищем и моделью данных Touch, можно переложить всю тяжелую работу по обработке удаленных данных и их получению в приложении на систему Touch.

Модели Sencha Touch предоставляют определение для записей данных, которые вы будете использовать в своем приложении. Они позволяют не только определить поля данных, но и задать конфигурацию прокси-сервера, который будет обрабатывать обмен данными между приложением и мобильной службой Azure.

В приведенном ниже коде видно, что мы определяем поля (и их типы) для модели, а также задаем конфигурацию прокси-сервера. При настройке прокси-сервера необходимо присвоить ему тип (в данном случае это "azure"), имя таблицы мобильной службы (ToDoItem) и другие дополнительные параметры. В этом примере мы включим для прокси-сервера разбиение по страницам, чтобы иметь возможность легко пролистывать элементы списка вперед и назад.

Прокси-сервер Azure автоматически установит все заголовки HTTP с помощью соответствующих операций CRUD, ожидаемых API-интерфейсом Azure (включая учетные данные проверки подлинности, если они существуют).

    Ext.define('Basic.model.TodoItem', {
        extend : 'Ext.data.Model',

        requires : [
            'Ext.azure.Proxy'
        ],

        config : {
            idProperty : 'id',
            useCache   : false,

            fields     : [
                {
                    name : 'id',
                    type : 'int'
                },
                {
                    name : 'text',
                    type : 'string'
                },
                {
                    name : 'complete',
                    type : 'boolean'
                }
            ],

            proxy : {
                type               : 'azure',
                tableName          : 'TodoItem',
                enablePagingParams : true
            }
        }
    });


### Сохранение элементов списка дел
**Имя файла**: app/store/TodoItems.js

Хранилища Sencha Touch служат для хранения коллекций записей данных (модели), которые можно использовать в качестве источников для компонентов Touch в целях отображения записей несколькими разными способами. Это могут быть таблицы, диаграммы, списки и многое другое.

Здесь мы определяем хранилище, которое будет использоваться для хранения всех элементов списка дел, которые извлекаются из мобильной службы Azure. Обратите внимание, что конфигурация хранилища содержит имя типа модели (Basic.model.TodoItem определяется выше). Это определяет структуру записей, которые будут содержаться в хранилище.

Для этого хранилища у нас также есть несколько дополнительных параметров конфигурации, например, можно задать размер страницы (8 записей) и указать, что сортировка записей для этого хранилища выполняется удаленно мобильной службой Azure (локальная сортировка внутри самого хранилища не выполняется).

    Ext.define('Basic.store.TodoItems', {
        extend : 'Ext.data.Store',

        requires : [
            'Basic.model.TodoItem'
        ],

        config : {
            model        : 'Basic.model.TodoItem',
            pageSize     : 8,
            remoteSort   : true,
            remoteFilter : true
        }
    });


### Просмотр и редактирование элементов списка дел
**Имя файла**: app/view/DataItem.js

Теперь, когда мы определили структуру каждого элемента ToDo и создали хранилище для размещения всех записей, следует подумать о том, как нужно отображать эту информацию пользователям приложения. Обычно мы отображаем информацию для пользователей с помощью **представлений**. Представление может быть одним из любого числа компонентов Touch и располагаться отдельно или в сочетании с другими такими объектами.

Приведенное ниже представление состоит из элемента ListItem, определяющего отображение каждой записи, и нескольких кнопок, позволяющие удалить каждый из элементов.

    Ext.define('Basic.view.DataItem', {
        extend : 'Ext.dataview.component.ListItem',
        xtype  : 'basic-dataitem',

        requires : [
            'Ext.Button',
            'Ext.layout.HBox',
            'Ext.field.Checkbox'
        ],

        config : {
            checkbox : {
                docked     : 'left',
                xtype      : 'checkboxfield',
                width      : 50,
                labelWidth : 0
            },

            text : {
                flex : 1
            },

            button : {
                docked   : 'right',
                xtype    : 'button',
                ui       : 'plain',
                iconMask : true,
                iconCls  : 'delete',
                style    : 'color: red;'
            },

            dataMap : {
                getText : {
                    setHtml : 'text'
                },

                getCheckbox : {
                    setChecked : 'complete'
                }
            },

            layout : {
                type : 'hbox',
                align: 'stretch'
            }
        },

        applyCheckbox : function(config) {
            return Ext.factory(config, Ext.field.Checkbox, this.getCheckbox());
        },

        updateCheckbox : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        },

        applyButton : function(config) {
            return Ext.factory(config, Ext.Button, this.getButton());
        },

        updateButton : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        }

    });


### Создание основного представления
**Имя файла**: app/view/Main.js

Теперь, когда мы определили макет отдельного элемента списка дел (см. выше), необходимо применить к этому списку, который определяет фактический перечень элементов, название приложения и кнопку для добавления новой задачи, оболочку в виде полного пользовательского интерфейса.

    Ext.define('Basic.view.Main', {
        extend : 'Ext.dataview.List',
        xtype  : 'main',

        requires : [
            'Ext.TitleBar',
            'Ext.dataview.List',
            'Ext.data.Store',
            'Ext.plugin.PullRefresh',
            'Ext.plugin.ListPaging',
            'Basic.view.DataItem'
        ],

        config : {
            store : 'TodoItems',

            useSimpleItems : false,
            defaultType    : 'basic-dataitem',

            plugins : [
                {
                    xclass          : 'Ext.plugin.PullRefresh',
                    pullRefreshText : 'Pull down to refresh!'
                },
                {
                    xclass     : 'Ext.plugin.ListPaging',
                    autoPaging : true
                }
            ],

            scrollable : {
                direction     : 'vertical',
                directionLock : true
            },

            items : [
                {
                    docked : 'top',
                    xtype  : 'titlebar',
                    title  : 'Azure Mobile - Basic Data Example'
                },
                {
                    xtype  : 'toolbar',
                    docked : 'bottom',
                    items  : [
                        {
                            xtype       : 'textfield',
                            placeHolder : 'Enter new task',
                            flex        : 1
                        },
                        {
                            xtype  : 'button',
                            action : 'add',
                            text   : 'Add'
                        }
                    ]
                }
            ]
        }
    });

### Обеспечение совместной работы
**Имя файла**: app/controller/Main.js

Заключительный этап работы над приложением заключается в обработке нажатий кнопок (удаление, сохранение и т. п.) и формировании логической схемы в качестве основы для всех этих запросов. Sencha Touch использует контроллеры, которые прослушивают такие события, и реагирует соответствующим образом.

    Ext.define('Basic.controller.Main', {
        extend : 'Ext.app.Controller',

        config : {
            refs : {
                todoField : 'main toolbar textfield',
                main      : 'main'
            },

            control : {
                'button[action=add]'    : {
                    tap : 'onAddItem'
                },
                'button[action=reload]' : {
                    tap : 'onReload'
                },

                main : {
                    activate      : 'loadInitialData',
                    itemdoubletap : 'onItemEdit'
                },

                'basic-dataitem checkboxfield' : {
                    change : 'onItemCompleteTap'
                },

                'basic-dataitem button' : {
                    tap : 'onItemDeleteTap'
                }
            }
        },

        loadInitialData : function () {
            Ext.getStore('TodoItems').load();
        },

        onItemDeleteTap : function (button, e, eOpts) {
            var store    = Ext.getStore('TodoItems'),
                dataItem = button.up('dataitem'),
                rec      = dataItem.getRecord();

            rec.erase({
                success: function (rec, operation) {
                    store.remove(rec);
                },
                failure: function (rec, operation) {
                    Ext.Msg.alert(
                        'Error',
                        Ext.util.Format.format('There was an error deleting this task.<br/><br/>    Status Code: {0}<br/>Status Text: {1}',
                        operation.error.status,
                        operation.error.statusText)
                    );
                }
            });
        },

        onItemCompleteTap : function (checkbox, newVal, oldVal, eOpts) {
            var dataItem = checkbox.up('dataitem'),
                rec      = dataItem.getRecord(),
                recVal   = rec.get('complete');

            // this check is needed to prevent an issue where multiple creates get triggered from one create
            if (newVal !== recVal) {
                rec.set('complete', newVal);
                rec.save({
                    success: function (rec, operation) {
                        rec.commit();
                    },
                    failure: function (rec, operation) {
                        // since there was a failure doing the update on the server then silently reject the change
                        rec.reject(true);
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        },

        onItemEdit : function (list, index, target, record, e, eOpts) {
            var rec = list.getSelection()[0];

            Ext.Msg.prompt('Edit', 'Rename task',
                function (buttonId, value) {
                    if (buttonId === 'ok') {
                        rec.set('text', value);
                        rec.save({
                            success: function (rec, operation) {
                                rec.commit();
                            },
                            failure: function (rec, operation) {
                                // since there was a failure doing the update on the server then reject the change
                                rec.reject();
                                Ext.Msg.alert(
                                    'Error',
                                    Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                                    operation.error.status,
                                    operation.error.statusText)
                                );
                            }
                        });
                    }
                },
                null,
                false,
                record.get('text')
            );
        },

        onReload : function () {
            Ext.getStore('TodoItems').load();
        },

        onAddItem : function () {
            var me = this,
                rec,
                store = Ext.getStore('TodoItems'),
                field = me.getTodoField(),
                value = field.getValue();

            if (value === '') {
                Ext.Msg.alert('Error', 'Please enter Task name', Ext.emptyFn);
            }
            else {
                rec = Ext.create('Basic.model.TodoItem', {
                    complete : false,
                    text     : value
                });
                //store.insert(0, rec); //insert at the top
                //store.sync();
                rec.save({
                    success: function (rec, operation) {
                        store.insert(0, rec); //insert at the top
                        field.setValue('');
                    },
                    failure: function (rec, operation) {
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error creating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        }
    });

### Сборка
**Имя файла**: app.js

Теперь осталось завершить редактирование основного файла приложения и передать информацию о моделях, хранилищах, представлениях и контроллерах, которые были определены. Исходные файлы для этих ресурсов автоматически загружаются в приложение. Наконец вызывается метод запуска, который создает и отображает главное представление приложения "Basic.main.View".

    Ext.Loader.setConfig({
        enabled : true,
        paths   : {
            'Ext'       : 'touch/src',
            'Ext.azure' : 'packages/azure/src'
        }
    });

    Ext.application({
        name : 'Basic',

        requires : [
            'Ext.MessageBox',
            'Ext.azure.Azure'
        ],

        views : [
            'Main'
        ],

        controllers : [
            'Main'
        ],

        stores : [
            'TodoItems'
        ],

        azure : {
            appUrl : 'YOUR_APP_URL.azure-mobile.net',
            appKey : 'YOUR_APP_KEY'
        },

        icon : {
            '57'  : 'resources/icons/Icon.png',
            '72'  : 'resources/icons/Icon~ipad.png',
            '114' : 'resources/icons/Icon@2x.png',
            '144' : 'resources/icons/Icon~ipad@2x.png'
        },

        isIconPrecomposed : true,

        startupImage : {
            '320x460'   : 'resources/startup/320x460.jpg',
            '640x920'   : 'resources/startup/640x920.png',
            '768x1004'  : 'resources/startup/768x1004.png',
            '748x1024'  : 'resources/startup/748x1024.png',
            '1536x2008' : 'resources/startup/1536x2008.png',
            '1496x2048' : 'resources/startup/1496x2048.png'
        },

        launch : function () {
            // Destroy the #appLoadingIndicator element
            Ext.fly('appLoadingIndicator').destroy();

            // Initialize Azure
            Ext.Azure.init(this.config.azure);

            // Initialize the main view
            Ext.Viewport.add(Ext.create('Basic.view.Main'));
        },

        onUpdated : function () {
            Ext.Msg.confirm(
                "Application Update",
                "This application has just successfully been updated to the latest version. Reload now?",
                function (buttonId) {
                    if (buttonId === 'yes') {
                        window.location.reload();
                    }
                }
            );
           }
    });

### Размещение и запуск приложения Sencha Touch
Последний раздел учебника посвящен размещению и запуску нового приложения на локальном компьютере.

1. В окне терминала перейдите к расположению, куда было распаковано приложение.
2. С помощью Sencha Cmd выполните следующие команды:
   
   * *sencha app refresh* : указывает Sencha Cmd найти все зависимости приложения и загрузить все необходимые пакеты (например, расширения [Sencha Touch для Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)).
   * *sencha web start* : запускает локальный веб-сервер для тестирования приложения.
   
   ![sencha web start](./media/partner-sencha-mobile-services-get-started/sencha-web-start.png)
3. Откройте указанный в терминале URL-адрес в веб-браузере, чтобы запустить приложение (например, http://localhost:1841.
4. В приложении введите содержательный текст, например "Завершение работы с учебником", и нажмите кнопку **Добавить**.
   
   ![new todo item](./media/partner-sencha-mobile-services-get-started/new-todo-item.png)
   
   Отправляет запрос POST в новую мобильную службу, размещенную в Azure. Данные из запроса вставляются в таблицу TodoItem.
5. На [классическом портале Azure] щелкните вкладку **Данные**, а затем щелкните таблицу TodoItems.
   
   ![Таблица элементов дел](./media/partner-sencha-mobile-services-get-started/mobile-data-tab.png)
   
   Это позволяет просматривать данные, добавленные в таблицу приложением.
   
   ![Просмотр таблицы дел](./media/partner-sencha-mobile-services-get-started/mobile-data-browse.png)

## Дальнейшие действия
Теперь, когда вы закончили изучение руководства по началу работы, узнайте, как с помощью Sencha можно выполнять дополнительные важные задачи в мобильных службах.

[Загрузите](https://github.com/arthurakay/sencha-touch-azure-example) готовый пример приложения с дополнительными стилями и функциями, чтобы ознакомиться с другими возможностями Sencha.

Затем изучите дополнительные сведения о расширениях Touch Sencha для Azure:

* Пример приложения — [пошаговое руководство](http://docs.sencha.com/touch-azure/1.0.0/#!/guide/data_filters)
* Получение помощи на [форумах Sencha](http://www.sencha.com/forum)
* Обзор [документации по Sencha](http://docs.sencha.com/)
* Использование Sencha с мобильными службами Azure: [(видео)](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-126-Using-Sencha-With-Windows-Azure-Mobile-Services)

## Дополнительные ресурсы
* [Скачать Sencha Touch](http://pages.sencha.com/touch-for-azure.html)
* [Расширения Sencha Touch для Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)

## Сводка
Представленный здесь пример приведен в пакете расширения Sencha Touch для Azure, находится в каталоге примеров и относится к примерам по основным данным. Существует несколько дополнительных примеров, демонстрирующих другие функциональные возможности этого расширения и снабженных подробными комментариями и пояснениями.

Дополнительные сведения о начале работы с Sencha Touch см. в полном наборе [руководств](http://docs.sencha.com/touch/#!/guide)

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- images -->
[0]: ./media/partner-sencha-mobile-services-get-started/finished-app.png

[классическом портале Azure]: https://manage.windowsazure.com/

<!---HONumber=AcomDC_0727_2016-->