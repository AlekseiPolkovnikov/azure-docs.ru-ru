---
title: Установка MongoDB на виртуальную машину Windows | Microsoft Docs
description: Узнайте, как установить MongoDB на виртуальную машину Azure под управлением Windows Server, созданную с помощью классической модели развертывания.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou

---
# <a name="install-mongodb-on-a-windows-vm"></a>Установка MongoDB на виртуальную машину Windows
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Описание установки и настройки MongoDB с помощью модели развертывания Resource Manager см. [в этой статье](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB][MongoDB] является популярной высокопроизводительной базой данных NoSQL с открытым кодом. В этой статье рассматривается создание виртуальной машины Windows Server с помощью [классического портала Azure][AzurePortal]. После этого перед установкой и настройкой MongoDB нужно создать и подключить к виртуальной машине диск данных. Если у вас есть виртуальная машина в Azure, которую вы хотите использовать, то можете перейти непосредственно к [установке и настройке MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Создание виртуальной машины под управлением Windows Server
Чтобы создать виртуальную машину, выполните указанные ниже действия.

[!INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> При создании виртуальной машины можно добавить конечную точку для MongoDB и настроить ее следующим образом: присвоить ей имя **Mongo**, в качестве протокола использовать **TCP**, а в качестве общедоступного и частного портов задать порт **27017**.
> 
> 

## <a name="attach-a-data-disk"></a>Присоединение диска данных
Чтобы предоставить хранилище для виртуальной машины, подключите диск данных, а затем инициализируйте, чтобы его можно было использовать в Windows. Можно подключить существующий диск или пустой диск.

[!INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Указания по инициализации диска см. в разделе "Практическое руководство. Инициализация нового диска данных в Windows Server" в статье [Подключение диска данных к виртуальной машине Windows](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Установка и запуск MongoDB на виртуальной машине
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Сводка
В этом учебнике вы научились создавать виртуальную машину под управлением Windows Server, удаленно подключаться к ней и подключать диск данных.  Вы также узнали, как установить и настроить MongoDB на виртуальной машине под управлением Windows. Теперь вы можете получить доступ к MongoDB на виртуальной машине под управлением Windows, выполнив действия, описанные в дополнительных разделах [документации по MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com



<!--HONumber=Oct16_HO2-->


