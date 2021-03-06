---
title: Создание и передача виртуального жесткого диска с ОС Linux на основе CentOS в Azure
description: Узнайте, как создать и передать виртуальный жесткий диск (VHD-файл) Azure, содержащий операционную систему Linux на основе CentOS
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/09/2016
ms.author: szarkos

---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Подготовка виртуальной машины на основе CentOS для Azure
* [Подготовка виртуальной машины CentOS 6.x для Azure](#centos-6x)
* [Подготовка виртуальной машины CentOS 7.0+ для Azure](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Предварительные требования
В этой статье предполагается, что вы уже установили операционную систему CentOS Linux (или аналогичную производную) на виртуальный жесткий диск. Существует несколько средств для создания VHD-файлов, например решение для виртуализации, такое как Hyper-V. Инструкции см. в разделе [Установка роли Hyper-V и настройка виртуальной машины](http://technet.microsoft.com/library/hh846766.aspx).

**Замечания по установке CentOS**

* Дополнительные сведения о подготовке Linux для Azure см. в разделе [Общие замечания по установке Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes).
* Формат VHDX не поддерживается в Azure, поддерживается только **фиксированный VHD**.  Можно преобразовать диск в формат VHD с помощью диспетчера Hyper-V или командлета convert-vhd.
* При установке системы Linux рекомендуется использовать стандартные разделы, а не LVM (как правило, значение по умолчанию во многих дистрибутивах). Это позволит избежать конфликта имен LVM при клонировании виртуальных машин, особенно если диск с OC может быть подключен к другой ВМ в целях устранения неполадок.  Для дисков данных можно использовать [LVM](virtual-machines-linux-configure-lvm.md) или [RAID](virtual-machines-linux-configure-raid.md).
* NUMA не поддерживается для виртуальных машин большего размера из-за ошибки ядра Linux в версиях ниже 2.6.37. Эта проблема влияет в основном на дистрибутивы, в которых используется исходное ядро Red Hat 2.6.32. При ручной установке агента Azure Linux (waagent) NUMA автоматически отключается в конфигурации GRUB для ядра Linux. Дополнительные сведения описаны далее.
* Не настраивайте раздел подкачки на диске с ОС. Можно настроить агент Linux для создания файла подкачки на временном диске ресурсов.  Дополнительные сведения описаны далее.
* Все VHD-диски должны иметь размер, кратный 1 МБ.

## <a name="centos-6.x"></a>CentOS 6.x
1. В диспетчере Hyper-V выберите виртуальную машину.
2. Щелкните **Подключение** , чтобы открыть окно консоли для виртуальной машины.
3. Удалите NetworkManager, выполнив следующую команду:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Примечание.** Если пакет еще не установлен, эта команда завершится с сообщением об ошибке. Это ожидаемое поведение.
4. Создайте файл с именем **network** в каталоге `/etc/sysconfig/`. Файл должен содержать следующий текст:
   
       NETWORKING=yes
       HOSTNAME=localhost.localdomain
5. Создайте файл с именем **ifcfg-eth0** в каталоге `/etc/sysconfig/network-scripts/`. Файл должен содержать следующий текст:
   
       DEVICE=eth0
       ONBOOT=yes
       BOOTPROTO=dhcp
       TYPE=Ethernet
       USERCTL=no
       PEERDNS=yes
       IPV6INIT=no
6. Переместите или удалите правила udev, чтобы не создавать статические правила для интерфейса Ethernet. Эти правила приводят к появлению проблем при клонировании виртуальной машины в Microsoft Azure или Hyper-V:
   
       # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
       # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Убедитесь, что сетевая служба запускается во время загрузки, выполнив следующую команду:
   
        # sudo chkconfig network on
8. **Только CentOS 6.3**. Установите драйверы для служб интеграции Linux.
   
    **Важно. Это действие действительно только для CentOS версии 6.3 и более ранних версий.**  В CentOS 6.4 и более поздних версиях службы интеграции Linux *уже доступны в стандартном ядре*.
   
   * Следуйте инструкциям по установке на [странице загрузки служб интеграции Linux](https://www.microsoft.com/en-us/download/details.aspx?id=46842) и установите RPM в вашем образе.  
9. Установите пакет python-pyasn1, выполнив следующую команду:
   
        # sudo yum install python-pyasn1
10. Если вы хотите использовать зеркала OpenLogic, размещенные в центрах обработки данных Azure, замените файл /etc/yum.repos.d/CentOS-Base.repo следующими репозиториями.  При этом также будет добавлен репозиторий **[openlogic]** , включающий пакеты для агента Linux для Azure:
    
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
    
        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
    
    **Примечание.** Далее в этом руководстве предполагается, что вы используете по крайней мере репозиторий [openlogic], который далее будет применяться для установки агента Linux для Azure.
11. Добавьте следующую строку в файл /etc/yum.conf:
    
        http_caching=packages
    
    **В CentOS 6.3** добавьте следующую строку:
    
        exclude=kernel*
12. Отключите модуль yum fastestmirror, внеся изменения в файл /etc/yum/pluginconf.d/fastestmirror.conf, затем в разделе `[main]`введите следующее:
    
        set enabled=0
13. Выполните следующую команду для очистки текущих метаданных yum:
    
        # yum clean all
14. **В CentOS 6.3**обновите ядро с помощью следующей команды:
    
        # sudo yum --disableexcludes=all install kernel
15. Измените строку загрузки ядра в конфигурации grub, чтобы включить дополнительные параметры ядра для Azure. Для этого откройте файл /boot/grub/menu.lst в текстовом редакторе и убедитесь, что ядро по умолчанию включает следующие параметры:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
    
    Это также гарантирует отправку всех сообщений консоли на первый последовательный порт, что может помочь технической поддержке Azure в плане отладки. Это приведет к отключению NUMA в связи с неисправностью версии ядра, используемого CentOS 6.
    
    Помимо вышесказанного, рекомендуется *удалить* следующие параметры:
    
        rhgb quiet crashkernel=auto
    
    Графическая и "тихая" загрузка бесполезны в облачной среде, в которой нам нужно, чтобы все журналы отправлялись на последовательный порт.
    
    При желании можно настроить параметр `crashkernel` , однако учитывайте, что он сокращает объем доступной памяти в виртуальной машине на 128 МБ или более, что может оказаться проблемой в виртуальных машинах небольшого размера.
16. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки.  Обычно это сделано по умолчанию.
17. Установите агент Linux для Azure, выполнив следующую команду:
    
        # sudo yum install WALinuxAgent
    
    Обратите внимание, что установка пакета WALinuxAgent приведет к удалению пакетов NetworkManager и NetworkManager-gnome, если они еще не были удалены, как описано в шаге 2.
18. Не создавайте пространство подкачки на диске с ОС.
    
    Агент Linux для Azure может автоматически настраивать пространство подкачки с использованием диска на локальном ресурсе, подключенном к виртуальной машине после подготовки для работы в среде Azure. Следует отметить, что локальный диск ресурсов является *временным* диском и должен быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. предыдущий шаг) измените следующие параметры в /etc/waagent.conf соответствующим образом:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
19. Выполните следующие команды, чтобы отменить подготовку виртуальной машины и подготовить ее в Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
20. В диспетчере Hyper-V выберите **Действие -> Завершение работы**. Виртуальный жесткий диск Linux готов к передаче в Azure.

- - -
## <a name="centos-7.0+"></a>CentOS 7.0+
**Изменения в CentOS 7 (и аналогичных производных версиях)**

Подготовка виртуальной машины CentOS 7 для Azure в значительной степени аналогична версии CentOS 6, однако есть несколько важных отличий:

* Пакет NetworkManager больше не конфликтует с агентом Azure Linux. Этот пакет устанавливается по умолчанию, и мы рекомендуем не удалять его.
* В качестве загрузчика по умолчанию теперь используется GRUB2, поэтому изменилась процедура правки параметров ядра (см. ниже).
* XFS теперь является файловой системой по умолчанию. При желании можно продолжать использование файловой системы ext4.

**Этапы настройки**

1. В диспетчере Hyper-V выберите виртуальную машину.
2. Щелкните **Подключение** , чтобы открыть окно консоли для виртуальной машины.
3. Создайте файл с именем **network** в каталоге `/etc/sysconfig/`. Файл должен содержать следующий текст:
   
       NETWORKING=yes
       HOSTNAME=localhost.localdomain
4. Создайте файл с именем **ifcfg-eth0** в каталоге `/etc/sysconfig/network-scripts/`. Файл должен содержать следующий текст:
   
       DEVICE=eth0
       ONBOOT=yes
       BOOTPROTO=dhcp
       TYPE=Ethernet
       USERCTL=no
       PEERDNS=yes
       IPV6INIT=no
5. Переместите или удалите правила udev, чтобы не создавать статические правила для интерфейса Ethernet. Эти правила приводят к появлению проблем при клонировании виртуальной машины в Microsoft Azure или Hyper-V:
   
       # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Убедитесь, что сетевая служба запускается во время загрузки, выполнив следующую команду:
   
        # sudo chkconfig network on
7. Установите пакет python-pyasn1, выполнив следующую команду:
   
        # sudo yum install python-pyasn1
8. Если вы хотите использовать зеркала OpenLogic, размещенные в центрах обработки данных Azure, замените файл /etc/yum.repos.d/CentOS-Base.repo следующими репозиториями.  При этом также будет добавлен репозиторий **[openlogic]** , включающий пакеты для агента Linux для Azure:
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
   
        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
   
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    **Примечание.** Далее в этом руководстве предполагается, что вы используете по крайней мере репозиторий [openlogic], который далее будет применяться для установки агента Linux для Azure.

1. Выполните следующую команду, чтобы очистить текущие метаданные yum и установить все обновления:
   
       # sudo yum clean all
       # sudo yum -y update
2. Измените строку загрузки ядра в конфигурации grub, чтобы включить дополнительные параметры ядра для Azure. Для этого откройте файл /etc/default/grub в текстовом редакторе и измените параметр `GRUB_CMDLINE_LINUX` , например:
   
       GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Это также гарантирует отправку всех сообщений консоли на первый последовательный порт, что может помочь технической поддержке Azure в плане отладки. Также будут отключены новые соглашения CentOS 7 об именовании для сетевых адаптеров. Помимо вышесказанного, рекомендуется *удалить* следующие параметры:
   
       rhgb quiet crashkernel=auto
   
   Графическая и "тихая" загрузка бесполезны в облачной среде, в которой нам нужно, чтобы все журналы отправлялись на последовательный порт.
   
   При желании можно настроить параметр `crashkernel` , однако учитывайте, что он сокращает объем доступной памяти в виртуальной машине на 128 МБ или более, что может оказаться проблемой в виртуальных машинах небольшого размера.
3. После внесения изменений в файл "/etc/default/grub", как указано выше, выполните следующую команду для повторного создания конфигурации grub:
   
       # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
4. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки.  Обычно это сделано по умолчанию.
5. **Если образ был создан из VMWare, VirtualBox или KVM** , добавьте модули Hyper-V в initramfs:
   
   Измените файл `/etc/dracut.conf`, добавив в него следующее содержимое:
   
       add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   Выполните сборку initramfs заново:
   
       # dracut –f -v
6. Установите агент Linux для Azure, выполнив следующую команду:
   
       # sudo yum install WALinuxAgent
       # sudo systemctl enable waagent
7. Не создавайте пространство подкачки на диске с ОС.
   
   Агент Linux для Azure может автоматически настраивать пространство подкачки с использованием диска на локальном ресурсе, подключенном к виртуальной машине после подготовки для работы в среде Azure. Следует отметить, что локальный диск ресурсов является *временным* диском и должен быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. предыдущий шаг) измените следующие параметры в /etc/waagent.conf соответствующим образом:
   
       ResourceDisk.Format=y
       ResourceDisk.Filesystem=ext4
       ResourceDisk.MountPoint=/mnt/resource
       ResourceDisk.EnableSwap=y
       ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
8. Выполните следующие команды, чтобы отменить подготовку виртуальной машины и подготовить ее в Azure:
   
       # sudo waagent -force -deprovision
       # export HISTSIZE=0
       # logout
9. В диспетчере Hyper-V выберите **Действие -> Завершение работы**. Виртуальный жесткий диск Linux готов к передаче в Azure.

## <a name="next-steps"></a>Дальнейшие действия
Теперь виртуальный жесткий диск CentOS Linux можно использовать для создания новых виртуальных машин Azure. Если вы загружаете VHD-файл в Azure впервые, обратитесь к шагам 2 и 3 в статье [Создание и загрузка виртуального жесткого диска, содержащего операционную систему Linux](virtual-machines-linux-classic-create-upload-vhd.md).

<!--HONumber=Oct16_HO2-->


