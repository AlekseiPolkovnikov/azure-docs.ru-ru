---
title: Неполадки при перезапуске или изменении размера виртуальной машины | Microsoft Docs
description: Устранение неполадок в классическом развертывании при перезагрузке или изменении размера существующей виртуальной машины Linux в Azure.
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue

ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 09/20/2016
ms.devlang: na
ms.author: delhan

---
# Устранение неполадок в классическом развертывании при перезагрузке или изменении размера существующей виртуальной машины Linux в Azure.
> [!div class="op_single_selector"]
> * [Классический](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
> * [Диспетчер ресурсов](virtual-machines-linux-restart-resize-error-troubleshooting.md)
> 
> 

Когда вы запускаете остановленную виртуальную машину Azure или изменяете размер существующей виртуальной машины Azure, часто возникает ошибка выделения ресурсов. Это происходит, когда кластер или регион не имеют доступных ресурсов или не поддерживают запрашиваемый размер виртуальной машины.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Сбор журналов аудита
Для устранения неполадок прежде всего соберите журналы аудита, чтобы определить ошибку, связанную с этой проблемой.

На портале Azure последовательно выберите **Обзор** > **Виртуальные машины** > *имя вашей виртуальной машины Linux* > **Настройки** > **Журналы аудита**.

## Проблема: ошибка во время запуска остановленной виртуальной машины
При попытке запустить остановленную виртуальную машину отображается сообщение об ошибке выделения.

### Причина:
Запрос на запуск остановленной виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не имеет свободного места для выполнения запроса.

### Способы устранения:
* Создайте новую облачную службу и свяжите ее с регионом или виртуальной сетью на основе региона, но не с территориальной группой.
* Удалите остановленную виртуальную машину.
* Повторно создайте виртуальную машину в новой облачной службе с помощью дисков.
* Запустите вновь созданную виртуальную машину.

Если при попытке создания облачной службы появляется сообщение об ошибке, повторите попытку позже или измените регион для облачной службы.

> [!IMPORTANT]
> Новая облачная служба будет иметь новое имя и новый виртуальный IP-адрес. Поэтому эти данные следует изменить для всех зависимостей, которые используют информацию о существующей облачной службе.
> 
> 

## Проблема: ошибка при изменении размера существующей виртуальной машины
При попытке изменить размер существующей виртуальной машины отображается сообщение об ошибке выделения.

### Причина:
Запрос на изменение размера виртуальной машины нужно выполнять в исходном кластере, в котором размещена облачная служба. Но этот кластер не поддерживает запрашиваемый размер виртуальной машины.

### Способы устранения:
Уменьшите запрашиваемый размер виртуальной машины и повторите запрос на изменение размера.

* Последовательно выберите **Просмотреть все** > **Виртуальные машины (классика)** > *имя вашей виртуальной машины* > **Настройки** > **Размер**. Подробные инструкции см. в статье [Перезапуск виртуальной машины](https://msdn.microsoft.com/library/dn168976.aspx).

Если размер виртуальной машины уменьшить невозможно, выполните следующие действия.

* Создайте новую облачную службу, не связанную с территориальной группой или с виртуальной сетью, которая связана с территориальной группой.
* Создайте в этой службе новую виртуальную машину большего размера.

Вы можете объединить все виртуальные машины в одной облачной службе. Если существующая облачная служба связана с виртуальной сетью на основе региона, вы можете подключить новую облачную службу к существующей виртуальной сети.

Если существующая облачная служба не связана с виртуальной сетью на основе региона, следует удалить виртуальные машины из существующей облачной службы и воссоздать их в новой облачной службе с помощью дисков. Не забывайте, что новая облачная служба будет иметь новое имя и новый виртуальный IP-адрес. Следовательно, эти данные потребуется обновить для всех зависимостей, которые используют эту информацию о существующей облачной службе.

## Дальнейшие действия
При возникновении проблем во время создания виртуальной машины Linux в Azure см. статью, посвященную [устранению неполадок в развертывании](virtual-machines-linux-troubleshoot-deployment-new-vm.md).

<!---HONumber=AcomDC_0921_2016-->