---
title: Запись образа виртуальной машины Linux для его использования в качестве шаблона | Microsoft Docs
description: Узнайте, как записать и подготовить образ виртуальной машины Azure под управлением Linux, созданной посредством модели развертывания с помощью Azure Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: danlep

---
# Как записать образ виртуальной машины Linux для его использования в качестве шаблона диспетчера ресурсов
Используйте интерфейс командной строки Azure (Azure CLI) для записи и подготовке к использованию виртуальной машины Azure под управлением Linux. После этого ее можно будет использовать как шаблон Azure Resource Manager, чтобы создавать другие виртуальные машины. В шаблоне указаны диск операционной системы и диски с данными, присоединенные к виртуальной машине. Он не включает в себя ресурсы виртуальной сети, необходимые для создания виртуальной машины Azure Resource Manager. Поэтому обычно их нужно настраивать отдельно перед созданием очередной виртуальной машины, которая использует шаблон.

> [!TIP]
> Если вы заинтересованы в создании пользовательского образа виртуальной машины Linux и его передаче в Azure для последующего создания виртуальных машин, ознакомьтесь с разделом [Передача пользовательского образа диска и создание виртуальной машины на его основе](virtual-machines-linux-upload-vhd.md).
> 
> 

## Перед началом работы
Эти инструкции предполагают, что вы уже создали виртуальную машину Azure в модели развертывания Resource Manager и настроили операционную систему, в частности, подключили диски данных и установили приложения. Настроить виртуальную машину можно сделать несколькими способами, в том числе с помощью Azure CLI. Если это еще не сделано, см. инструкции по использованию Azure CLI в режиме диспетчера ресурсов Azure:

* [Создание виртуальной машины Linux в Azure с помощью интерфейса командной строки](virtual-machines-linux-quick-create-cli.md)
* [Развертывание виртуальных машин и управление ими с помощью шаблонов диспетчера ресурсов Azure и интерфейса командной строки Azure](virtual-machines-linux-cli-deploy-templates.md)

Например, создайте в центральной части США группу ресурсов с именем *MyResourceGroup*. Затем с помощью команды **azure vm quick-create** разверните в этой группе виртуальную машину под управлением Ubuntu 14.04 LTS.

     azure vm quick-create -g MyResourceGroup -n <your-virtual-machine-name> "centralus" -y Linux -Q canonical:ubuntuserver:14.04.2-LTS:latest -u <your-user-name> -p <your-password>

Когда подготовленная виртуальная машина запустится, может потребоваться [присоединить и подключить диск данных](virtual-machines-linux-add-disk.md).

## Запись образа виртуальной машины
1. Чтобы записать образ виртуальной машины, подключитесь к ней с помощью SSH-клиента.
2. В окне SSH введите следующую команду. Выходные данные служебной программы **waagent** могут незначительно различаться в зависимости от ее версии.
   
    `sudo waagent -deprovision+user`
   
    Эта команда пытается очистить систему и сделать ее пригодной для повторной подготовки. В ходе операции выполняются такие задачи:
   
   * удаляются ключи узла SSH (если в файле конфигурации для Provisioning.RegenerateSshHostKeyPair указано значение «y»);
   * очищается конфигурация имени сервера в /etc/resolvconf;
   * удаляется пароль пользователя `root` из /etc/shadow (если в файле конфигурации для Provisioning.DeleteRootPassword указано значение «y»);
   * удаляются кэшированные аренды DHCP-клиентов.
   * возвращается имя узла localhost.localdomain;
   * удаляется последняя подготовленная учетная запись пользователя (полученная из /var/lib/waagent) и связанные с ней данные.
     
     > [!NOTE]
     > Процедура отзыва приводит к удалению файлов и данных, так как образ подготавливается к использованию. Эту команду следует выполнять только на той виртуальной машине, образ которой вы хотите записать. Отзыв не гарантирует, что из образа будет удалена вся конфиденциальная информация и что он будет готов к повторному распространению третьим сторонам.
     > 
     > 
3. Чтобы продолжить, введите **y**. Чтобы запрос на подтверждение не появлялся, добавьте параметр **-force**.
4. Введите **exit**, чтобы закрыть SSH-клиент.
   
   > [!NOTE]
   > Далее предполагается, что вы уже [установили Azure CLI](../xplat-cli-install.md) на клиентском компьютере.
   > 
   > 
5. На клиентском компьютере откройте Azure CLI и войдите в свою подписку Azure. Дополнительные сведения см. в статье [Подключение к среде Azure с использованием интерфейса командной строки Azure](../xplat-cli-connect.md).
6. Активируйте режим диспетчера ресурсов:
   
    .`azure config mode arm`
7. Остановите отозванную виртуальную машину с помощью следующей команды.
   
    .`azure vm deallocate -g <your-resource-group-name> -n <your-virtual-machine-name>`
8. Обобщите виртуальную машину:
   
    `azure vm generalize -g <your-resource-group-name> -n <your-virtual-machine-name>`
9. Теперь запишите образ и шаблон локального файла:
   
    `azure vm capture <your-resource-group-name>  <your-virtual-machine-name> <your-vhd-name-prefix> -t <path-to-your-template-file-name.json>`
   
    Эта команда создает обобщенный образ операционной системы, используя префикс имени VHD, указанный для дисков виртуальной машины. По умолчанию VHD-файлы образа создаются в учетной записи хранения, которую использует исходная виртуальная машина. (Виртуальные жесткие диски создаваемых из образа виртуальных машин будут храниться в той же учетной записи.) Параметр **-t** создает локальный шаблона JSON-файла, с помощью которого из образа можно создать новую виртуальную машину.

> [!TIP]
> Чтобы найти расположение образа, откройте шаблон JSON-файла. В разделе **storageProfile** в контейнере **system** найдите **URI** **образа**. Например, URI для образа диска ОС может выглядеть так: `https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/<your-vhd-name-prefix>-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
> 
> 

## Развертывание новой виртуальной машины из записанного образа
Теперь с помощью образа и шаблона мы создадим виртуальную машину под управлением Linux. Мы покажем, как можно создать виртуальную машину в новой виртуальной сети, используя Azure CLI и шаблон JSON-файла, созданного с помощью команды `azure vm capture`.

### Создание сетевых ресурсов
Чтобы использовать шаблон, для новой виртуальной машины сначала необходимо настроить виртуальную сеть и сетевую карту. Мы рекомендуем создать новую группу ресурсов для этих ресурсов в расположении, в котором хранится образ виртуальной машины. Выполните приведенные ниже команды, подставив нужные имена ресурсов и указав соответствующий регион Azure (вместе centralus).

    azure group create <your-new-resource-group-name> -l "centralus"

    azure network vnet create <your-new-resource-group-name> <your-vnet-name> -l "centralus"

    azure network vnet subnet create <your-new-resource-group-name> <your-vnet-name> <your-subnet-name>

    azure network public-ip create <your-new-resource-group-name> <your-ip-name> -l "centralus"

    azure network nic create <your-new-resource-group-name> <your-nic-name> -k <your-subnetname> -m <your-vnet-name> -p <your-ip-name> -l "centralus"

Чтобы развернуть виртуальную машину из образа, используя JSON-файл, который был сохранен во время записи образа, вам потребуется идентификатор сетевого адаптера. Получить идентификатор можно с помощью следующей команды:

    azure network nic show <your-new-resource-group-name> <your-nic-name>

В выходных данных **идентификатор** будет строкой следующего вида.

    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/<your-new-resource-group-name>/providers/Microsoft.Network/networkInterfaces/<your-nic-name>



### Создание развертывания
Теперь из образа и сохраненного шаблона JSON-файла создайте виртуальную машину. Для этого выполните следующую команду:

    azure group deployment create <your-new-resource-group-name> <your-new-deployment-name> -f <path-to-your-template-file-name.json>

Вам будет предложено указать имя виртуальной машины, имя и пароль администратора, а также идентификатор созданного ранее сетевого адаптера.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: mynewvm
    adminUserName: myadminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/mynewrg/providers/Microsoft.Network/networkInterfaces/mynewnic

Если развертывание пройдет успешно, вы увидите примерно такой результат:

    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "dlnewdeploy"
    + Waiting for deployment to complete
    data:    DeploymentName     : mynewdeploy
    data:    ResourceGroupName  : mynewrg
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-10-29T16:35:47.3419991Z
    data:    Mode               : Incremental
    data:    Name                Type          Value


    data:    ------------------  ------------  -------------------------------------

    data:    vmName              String        mynewvm


    data:    vmSize              String        Standard_D1


    data:    adminUserName       String        myadminuser


    data:    adminPassword       SecureString  undefined


    data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/mynewrg/providers/Microsoft.Network/networkInterfaces/mynewnic
    info:    group deployment create command OK

### Проверка развертывания
Чтобы проверить развертывание и начать использовать новую виртуальную машину, подключитесь к ней с помощью SSH-клиента. Для этого сначала определите IP-адрес созданной виртуальной машины:

    azure network public-ip show <your-new-resource-group-name> <your-ip-name>

Команда возвращает общедоступный IP-адрес. По умолчанию подключение к виртуальной машине Linux по протоколу SSH выполняется через порт 22.

## Создание дополнительных виртуальных машин с помощью шаблона
Развертывание дополнительных виртуальных машин с использованием записанного образа и шаблона выполняется так же, как описано в предыдущем разделе.

* Образ виртуальной машины должен храниться в той же учетной записи хранения, в которой будет размещен виртуальный жесткий диск виртуальной машины.
* Скопируйте шаблон JSON-файла и введите уникальное значение для **URI** каждого виртуального жесткого диска виртуальной машины.
* Создайте сетевую карту в той же или другой виртуальной сети.
* С помощью измененного шаблона JSON-файла создайте развертывание в группе ресурсов, в которой вы настраиваете виртуальную сеть.

Если вы хотите, чтобы сеть настроилась автоматически при создании виртуальной машины из образа, используйте [шаблон 101-vm-from-user-image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) с сайта GitHub. Этот шаблон создает не только виртуальную машину из пользовательского образа, но и необходимую виртуальную сеть, общедоступный IP-адрес и сетевые ресурсы. Пошаговые инструкции по использованию шаблона на портале Azure см. в статье [How to create a virtual machine from a custom image using a ARM template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/) (Как создать виртуальную машину из пользовательского образа с помощью шаблона ARM).

## Команда azure vm create
Обычно шаблон диспетчера ресурсов используют для создания виртуальной машины из образа. Однако виртуальную машину можно создать и *принудительно* — с помощью команды **azure vm create** с параметром **-Q** (**--image-urn**). В случае использования данного метода также передается параметр **-d** (**--os-disk-vhd**), чтобы указать расположение VHD-файла операционной системы для новой виртуальной машины. Он должен находиться в контейнере виртуальных жестких дисков учетной записи хранения, где хранится VHD-файл образа. Эта команда автоматически копирует VHD для новой виртуальной машины в контейнер виртуальных жестких дисков.

Прежде чем пытаться создать виртуальную машину из образа с помощью команды **azure vm create**, сделайте вот что:

1. Создайте для развертывания группу ресурсов или укажите существующую.
2. Создайте для новой виртуальной машины общедоступный IP-адрес и сетевой адаптер. Процедура создания виртуальной сети, общедоступного IP-адреса и сетевого адаптера с помощью интерфейса командной строки описана выше в этой статье. (Команда **azure vm create** может создать также и сетевую карту, но для этого потребуется указать дополнительные параметры виртуальной сети и подсети.)

Затем выполните команду, подобную указанной далее, передав URI в новый VHD-файл операционной системы и в существующий образ.

    azure vm create <your-resource-group-name> <your-new-vm-name> eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/<your-new-VM-prefix>.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/<your-image-prefix>-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u <your-admin-name> -p <your-admin-password> -f <your-nic-name>

Чтобы получить информацию о дополнительных параметрах команды, выполните команду `azure help vm create`.

## Дальнейшие действия
Сведения об управлении виртуальными машинами с помощью интерфейса командной строки см. в статье [Развертывание виртуальных машин и управление ими с помощью шаблонов диспетчера ресурсов Azure и интерфейса командной строки Azure](virtual-machines-linux-cli-deploy-templates.md).

<!---HONumber=AcomDC_0817_2016-->