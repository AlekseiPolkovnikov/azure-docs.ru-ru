---
title: Включение и отключение мониторинга виртуальной машины Azure
description: В этой статье описывается включение или отключение мониторинга виртуальной машины Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: ''

ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss

---
# Включение и отключение мониторинга виртуальной машины Azure
В этом разделе описывается, как включить или отключить мониторинг виртуальных машин, работающих в Azure. По умолчанию мониторинг виртуальных машин Azure включен, если они развернуты из [портала Azure](https://portal.azure.com), а диаграммы мониторинга по умолчанию отображаются с 1-минутным интервалом. Мониторинг можно включить или отключить с помощью портала или интерфейса командной строки Azure для Mac, Linux и Windows (Azure CLI).

## Включение и отключение мониторинга через портал Azure
Вы можете включить мониторинг виртуальной машины Azure, который предоставляет данные о вашем экземпляре с интервалом в 1 минуту (применяются изменения хранилища). В результате на диаграммах портала или с помощью API становятся доступными подробные диагностические данные для виртуальной машины. По умолчанию мониторинг на портале Azure включен, но его можно отключить, как описано ниже. Включить мониторинг можно, когда виртуальная машина работает или остановлена.

* Откройте портал Azure по адресу **[https://portal.azure.com](https://portal.azure.com)**.
* В левой панели навигации щелкните "Виртуальные машины".
* В списке виртуальных машин выберите запущенный или остановленный экземпляр. Откроется колонка виртуальной машины.
* Щелкните "Все параметры".
* Щелкните "Диагностика".
* Измените состояние с "Вкл" на "Выкл". В этой колонке также можно выбрать уровень детализации мониторинга для виртуальной машины.

[Azure.Note] При создании новой виртуальной машины по умолчанию устанавливается переключатель "Диагностика вкл.".

![Включение и отключение мониторинга через портал Azure][1]

## Включение и отключение мониторинга с помощью интерфейса командной строки Azure
Чтобы включить мониторинг для виртуальной машины Azure, выполните следующие действия.

* Создайте файл с именем, например, PrivateConfig.json со следующим содержимым. { "storageAccountName":"учетная запись хранения для приема данных", "storageAccountKey":"ключ учетной записи" }
* Выполните следующую команду CLI Azure.
  
        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] С версии 2.0 можно переключиться на более позднюю версию, когда она появится.

Дополнительные сведения о настройке метрик мониторинга и примеры см. в документе **[Использование диагностического расширения Linux для мониторинга данных о состоянии и производительности виртуальных машин под управлением Linux](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png


<!---HONumber=AcomDC_0824_2016-->