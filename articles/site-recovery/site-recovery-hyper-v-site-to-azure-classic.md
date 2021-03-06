---
title: "Репликация между локальными виртуальными машинами Hyper-V и Azure (без VMM) с помощью службы Site Recovery | Документация Майкрософт"
description: "В этой статье описывается репликация виртуальных машин Hyper-V в Azure с помощью Azure Site Recovery, когда виртуальные машины не управляются в облаках VMM."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 8ff2423f5b546864757a75cd7af1e6c76f047b19
ms.openlocfilehash: 221027027e57413b6244c97e3e5c57d6423d94ea


---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Репликация между локальными виртуальными машинами Hyper-V и Azure (без VMM) с помощью службы Azure Site Recovery
> [!div class="op_single_selector"]
> * [Портал Azure](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell — Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Классический портал](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Вас приветствует служба Azure Site Recovery.

Site Recovery — это служба Azure, которая помогает реализовать стратегию непрерывности бизнес-процессов и аварийного восстановления (BCDR). Она координирует репликацию локальных физических серверов и виртуальных машин в облако (Azure) или дополнительный центр обработки данных. При возникновении сбоев в исходном расположении происходит отработка отказа с выполнением перехода в дополнительное расположение. Это обеспечивает доступность приложений и рабочих нагрузок. При восстановлении нормального режима работы исходного расположения происходит переключение на него. Дополнительные сведения см. в статье [Что такое Azure Site Recovery?](site-recovery-overview.md)

В этой статье описано, как выполнить репликацию локальных виртуальных машин Hyper-V в Azure с помощью Azure Site Recovery на портале Azure. В данном случае управление серверами Hyper-V не осуществляется в облаках VMM.

Комментарии или вопросы технического характера можно добавить в конце этой статьи или на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).





## <a name="site-recovery-in-the-azure-portal"></a>Служба Site Recovery на портале Azure

В Azure предлагаются две разные [модели развертывания](../resource-manager-deployment-model.md) для создания ресурсов и работы с ними: модель Azure Resource Manager и классическая модель. Кроме того, Azure предоставляет два портала — классический портал Azure и портал Azure.

В этой статье описывается развертывание на классическом портале. Классический портал можно использовать для поддержания существующих хранилищ. Новые хранилища с помощью классического портала создавать нельзя.

## <a name="site-recovery-in-your-business"></a>Преимущества службы Site Recovery для бизнеса

Организациям нужна стратегия обеспечения непрерывности бизнес-процессов и аварийного восстановления, которая определяет, как приложения и данные будут оставаться доступными при запланированных и незапланированных простоях, а также как можно максимально быстро восстановить нормальный режим работы. Преимущества службы Site Recovery:

* автономная защита бизнес-приложений, запущенных на виртуальных машинах Hyper-V;
* единое расположение для настройки, мониторинга репликации и управления ею, а также отработки отказа и восстановления;
* простая отработка отказа в Azure и восстановление размещения с переносом из Azure на серверы узла Hyper-V на локальном сайте;
* предоставление планов восстановления для нескольких виртуальных машин, что позволяет обеспечить отработку отказа рабочих нагрузок многоуровневого приложения.

## <a name="azure-prerequisites"></a>Предварительные требования Azure
* Вам потребуется учетная запись [Microsoft Azure](https://azure.microsoft.com/) . Начните с [бесплатной пробной версии](https://azure.microsoft.com/pricing/free-trial/).
* Для хранения реплицируемых данных потребуется учетная запись хранения Azure. На учетной записи необходимо включить георепликацию. Она должна находиться в том же регионе, что и хранилище Azure Site Recovery, и быть связана с той же подпиской. См. [дополнительные сведения о службе хранилища Azure](../storage/storage-introduction.md). Учтите, что перемещение учетных записей хранения, созданных с помощью [нового портала Azure](../storage/storage-create-storage-account.md), между группами ресурсов не поддерживается.
* Виртуальные машины Azure должны быть подключены к сети при отработке отказа с перемещением из основного сайта. Поэтому вам потребуется виртуальная сеть Azure.

## <a name="hyper-v-prerequisites"></a>Предварительные требования Hyper-V
* На исходном локальном сайте вам понадобится один или несколько серверов **Windows Server 2012 R2** с установленной ролью Hyper-V или **Microsoft Hyper-V Server 2012 R2**. Ниже приведены требования к этому серверу.
* Он должен содержать одну или несколько виртуальных машин.
* Он должен быть подключенным к Интернету напрямую или через прокси-сервер.
* Он должен содержать исправления, описанные в KB [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>Предварительные требования к виртуальным машинам
Виртуальные машины, которые требуется защитить, должны отвечать [требованиям к виртуальным машинам Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).

## <a name="provider-and-agent-prerequisites"></a>Предварительные требования к поставщикам и агентам
При развертывании Azure Site Recovery мы установим поставщик этой службы и агент служб восстановления Azure на каждом сервере Hyper-V. Обратите внимание на следующее.

* Рекомендуется запускать последние версии поставщика и агента. Они доступны на портале Site Recovery.
* На всех серверах Hyper-V в хранилище должны быть одинаковые версии поставщика и агента.
* Поставщик, запущенный на сервере, подключается к службе Site Recovery через Интернет. Это можно сделать без прокси-сервера с помощью параметров прокси-сервера, настроенных на сервере Hyper-V, или с помощью пользовательских параметров прокси-сервера, настраиваемых во время установки поставщика. Необходимо убедиться в том, что желаемый прокси-сервер может получить доступ к этим URL-адресам для подключения к Azure:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Кроме того, разрешите подключение к IP-адресам, перечисленным в статье [Диапазоны IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653) , и по протоколу HTTPS (443). Необходимо разрешить диапазоны IP-адресов для региона Azure, который вы планируете использовать, и для западной части США.

На рисунке показаны различные каналы связи и порты, используемые службой Site Recovery для управления и репликации

![Топология B2A](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>Шаг 1. Создание хранилища
1. Выполните вход на [Портал управления](https://portal.azure.com).
2. Разверните **Службы данных** > **Службы восстановления** и щелкните **Хранилище Site Recovery**.
3. Щелкните **Создать** > **Быстрое создание**.
4. В поле **Имя**введите понятное имя для идентификации хранилища.
5. В поле **Область**выберите географический регион для хранилища архивации. Сведения о поддерживаемых регионах см. в разделе "Географическая доступность" на странице [Расценки на службу Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Щелкните элемент **Создать хранилище**.

    ![Новое хранилище](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Проверьте строку состояния, чтобы убедиться, что хранилище успешно создано. На главной странице служб восстановления это хранилище будет указано с состоянием **Активно** .

## <a name="step-2-create-a-hyper-v-site"></a>Шаг 2. Создание узла Hyper-V
1. На странице Службы восстановления щелкните хранилище, чтобы открыть страницу быстрого запуска. Страницу быстрого запуска можно открыть и в любой другой момент, щелкнув этот значок.

    ![Быстрый запуск](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. В раскрывающемся списке выберите элемент **Между локальным узлом Hyper-V и Microsoft Azure**.

    ![Сценарий узла Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. В разделе **Создание узла Hyper-V** щелкните **Создать узел Hyper-V**. Укажите имя узла и сохраните.

    ![Узел Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-the-provider-and-agent"></a>Шаг 3. Установка поставщика и агента
Установите поставщик и агент на каждом сервере Hyper-V, содержащем виртуальные машины, которые необходимо защитить.

При установке в кластере Hyper-V выполните шаги с 5-11 на каждом узле отказоустойчивого кластера. После регистрации всех узлов и включения защиты виртуальные машины будут защищены даже при перемещении между узлами кластера.

1. В разделе **Подготовка серверов Hyper-V** щелкните **Загрузить регистрационный ключ**.
2. На странице **Загрузка регистрационного ключа** щелкните **Загрузка** рядом с узлом. Скачайте ключ в безопасное расположение, легко доступное для сервера Hyper-V. Ключ действителен в течение пяти дней после создания.

    ![Регистрационный ключ](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Щелкните **Скачать поставщик** для получения последней версии.
4. Запустите файл на каждом сервере Hyper-V, который нужно зарегистрировать в хранилище. Файл устанавливает два компонента:
   * **Поставщик Azure Site Recovery**— обеспечивает возможность связи и оркестрации между сервером Hyper-V и порталом Azure Site Recovery.
   * **Агент служб восстановления Azure**— обрабатывает передачу данных между виртуальными машинами, работающими на исходном сервере Hyper-V и в хранилище Azure.
5. В **Центре обновления Майкрософт** вы можете настроить обновления. Если этот параметр включен, то обновления поставщика и агента будут устанавливаться в соответствии с вашей политикой Центра обновления Майкрософт.

    ![Центр обновления Майкрософт](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. В поле **Установка** укажите место на сервере Hyper-V для установки поставщика и агента.

    ![Расположение установки](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. После завершения установки продолжите настройку, чтобы зарегистрировать сервер в хранилище.
8. На странице **Параметры хранилища** нажмите кнопку **Обзор** и выберите файл ключа. Укажите подписку Azure Site Recovery, имя хранилища и узла Hyper-V, к которому относится сервер Hyper-V.

    ![Регистрация сервера](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. На странице **Подключение к Интернету** укажите способ подключения поставщика к Azure Site Recovery. Выберите пункт **Use default system proxy settings** (Использовать настройки прокси-сервера по умолчанию), чтобы использовать настройки подключения к Интернету, по умолчанию заданные на сервере. Если значение не указано, будут использоваться параметры по умолчанию.

   ![Параметры Интернета](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Начнется процесс регистрации сервера в хранилище.

    ![Регистрация сервера](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. После завершения регистрации Azure Site Recovery извлекает метаданные из сервера Hyper-V, а сам сервер отображается на вкладке **Узлы Hyper-V** на странице **Серверы** в хранилище.

### <a name="install-the-provider-from-the-command-line"></a>Установка поставщика из командной строки
В качестве альтернативы можно установить поставщик Azure Site Recovery из командной строки. Этот метод следует использовать, если требуется установить поставщик на компьютере, работающем под управлением Windows Server Core 2012 R2. Выполните запуск из командной строки следующим образом:

1. Скачайте файл установки поставщика и ключ регистрации в папку. Например, C:\ASR.
2. Запустите командную строку от имени администратора и введите следующее:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Установите поставщик, выполнив следующую команду:

        C:\ASR> setupdr.exe /i
4. Для завершения регистрации выполните следующую команду:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Параметры:

* **/Credentials**: укажите расположение загруженного ключа регистрации.
* **/FriendlyName**: укажите имя для определения сервера узла Hyper-V. Это имя отобразится на портале.
* **/EncryptionEnabled**. (Необязательно.) Укажите, следует ли шифровать виртуальные машины реплик в Azure (неактивное шифрование).
* **/proxyAddress**; **/proxyport**; **/proxyUsername**; **/proxyPassword**: необязательны. (Необязательно.) Укажите параметры прокси-сервера, если необходимо использовать пользовательский прокси-сервер или выполнить проверку подлинности для существующего прокси-сервера.

## <a name="step-4-create-an-azure-storage-account"></a>Шаг 4. Создание учетной записи хранения Azure
1. В разделе **Подготовка ресурсов** выберите **Создать учетную запись хранения** для создания учетной записи хранения Azure, если таковой нет. Для учетной записи необходимо включить георепликацию. Она должна находиться в том же регионе, что и хранилище Azure Site Recovery, и быть связана с той же подпиской.

    ![Создать учетную запись хранения](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Мы не поддерживаем перемещение учетных записей хранения, созданных с помощью [нового портала Azure](../storage/storage-create-storage-account.md) , между группами ресурсов.                               2. [Перенос учетных записей хранения](../resource-group-move-resources.md) между группами ресурсов в рамках одной или нескольких подписок не поддерживается для учетных записей хранения, используемых для развертывания Site Recovery.
>
>

## <a name="step-5-create-and-configure-protection-groups"></a>Шаг 5. Создание и настройка групп защиты
Группы защиты — это логические группы виртуальных машин, которые требуется защитить, используя те же параметры защиты. Параметры, установленные для группы защиты, будут применяться ко всем виртуальным машинам, добавленным в группу.

1. В разделе **Создание и настройка групп защиты** щелкните **Создать группу защиты**. Если выполнены не все предварительные требования, выдается соответствующее сообщение. Для получения дополнительных сведений щелкните **Просмотр сведений**.
2. На вкладке **Группы защиты** добавьте группу защиты. Укажите имя, исходный узел Hyper-V, целевой ресурс **Azure**, имя подписки Azure Site Recovery и учетную запись хранения Azure.

    ![Группа защиты](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. В разделе **Параметры репликации** задайте параметр **Частота копирования**, чтобы указать, как часто следует синхронизировать разностные данные между источником и получателем. Можно выбрать значение 30 секунд, 5 минут или 15 минут.
4. В поле **Сохранять точки восстановления** укажите, сколько часов журнала восстановления следует сохранять.
5. В поле **Частота моментальных снимков, соответствующих приложению** можно указать, создавать ли снимки, использующие службу теневого копирования томов (VSS), чтобы гарантировать согласованное состояние приложений на момент создания снимка. По умолчанию они не создаются. Это значение должно быть меньше количества дополнительных точек восстановления, которое было настроено. Это поддерживается только в том случае, если виртуальная машина работает под управлением операционной системы Windows.
6. В поле **Время запуска начальной репликации** укажите, когда начальная репликация виртуальных машин в группе защиты должна отправляться в Azure.

    ![Группа защиты](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>Шаг 6. Включение защиты виртуальной машины
Добавьте виртуальные машины в группу защиты, чтобы включить для них защиту.

> [!NOTE]
>  Защита виртуальных машин под управлением Linux со статическими IP-адресами не поддерживается.
>
>

1. На вкладке **Компьютеры** для группы защиты щелкните **Добавить виртуальные машины в группу защиты, чтобы включить защиту**.
2. На странице **Включение защиты виртуальной машины** выберите виртуальные машины, которые следует защитить.

    ![Включение защиты виртуальной машины](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    Запускается задание включения защиты. Ход выполнения операции можно отслеживать на вкладке **Задания** . После запуска задачи финализации защиты виртуальная машина готова к отработке отказа.
3. После настройки защиты вы можете:

   * Просмотреть виртуальные машины, выбрав элементы **Защищенные элементы** > **Группы защиты** > *имя_группы* > **Виртуальные машины**. Подробные сведения о виртуальной машине можно просмотреть на вкладке **Свойства**.
   * Настраивать свойства отработки отказа для виртуальных машин в разделе **Защищенные элементы** > **Группы защиты** > *имя_группы* > **Виртуальные машины***имя_виртуальной_машины* > **Настроить**. Вы можете настроить:

     * **Имя**: имя виртуальной машины в Azure.
     * **Размер**: целевой размер виртуальной машины, отрабатывающей отказ.

       ![Настройка свойств виртуальной машины](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * В разделе *Защищенные элементы** > **Группы защиты** > *имя_группы_защиты* > **Виртуальные машины** *имя_виртуальной_машины* > **Настроить** настройте дополнительные параметры виртуальных машин, в том числе следующие:

     * **Сетевые адаптеры**: количество сетевых адаптеров диктуется размером, указанным для целевой виртуальной машины. Ознакомьтесь со [спецификациями размеров виртуальных машин](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) , чтобы узнать количество сетевых адаптеров в соответствии с размером виртуальной машины.

            When you modify the size for a virtual machine and save the settings, the number of network adapter will change when you open **Configure** page the next time. The number of network adapters of target virtual machines is minimum of the number of network adapters on source virtual machine and maximum number of network adapters supported by the size of the virtual machine chosen. It is explained below:


            - If the number of network adapters on the source machine is less than or equal to the number of adapters allowed for the target machine size, then the target will have the same number of adapters as the source.
            - If the number of adapters for the source virtual machine exceeds the number allowed for the target size then the target size maximum will be used.
            - For example if a source machine has two network adapters and the target machine size supports four, the target machine will have two adapters. If the source machine has two adapters but the supported target size only supports one then the target machine will have only one adapter.     
        - **Сеть Azure**: укажите сеть, в которую должна переходить виртуальная машина. Если у виртуальной машины несколько сетевых адаптеров, то все адаптеры должны быть подключены к одной сети Azure.
        - **Подсеть** : выберите для каждого сетевого адаптера на виртуальной машине подсеть в сети Azure, к которой машина должна подключаться после отработки отказа.
        - **Целевой IP-адрес**. Если сетевой адаптер исходной виртуальной машины настроен для использования статического IP-адреса, можно указать IP-адрес для целевой виртуальной машины, чтобы предотвратить изменение IP-адреса виртуальной машины после отработки отказа.  Если IP-адрес не указан, то во время отработки отказа будет назначен любой доступный адрес. Если указать адрес, который уже используется, при отработке отказа произойдет сбой.

        > [AZURE.NOTE] [Миграция сетей](../resource-group-move-resources.md) между группами ресурсов в рамках одной или нескольких подписок не поддерживается для сетей, используемых для развертывания Site Recovery.

        ![Настройка свойств виртуальной машины](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>Шаг 7. Создание плана восстановления
Чтобы протестировать развертывание, можно запустить тестовую отработку отказа для отдельной виртуальной машины или создать план восстановления для одной или нескольких виртуальных машин. См. [дополнительные сведения](site-recovery-create-recovery-plans.md) о создании плана восстановления.

## <a name="step-8-test-the-deployment"></a>Шаг 8. Тестирование развертывания
Существует два способа запуска тестовой отработки отказа в Azure.

* **Тестовая отработка отказа без использования сети Azure**. В этом случае проверяется, запустилась ли виртуальная машина в Azure. Виртуальная машина не подключается к сети Azure после отработки отказа.
* **Тестовая отработка отказа с использованием сети Azure**. В этом случае проверяется, работает ли вся среда репликации должным образом и подключаются ли виртуальные машины, для которых выполнена отработка отказа, к указанной целевой сети Azure. Обратите внимание, что для тестовой отработки отказа подсеть тестовой виртуальной машины определяется в зависимости от подсети виртуальной машины реплики. Этот режим отличается от обычной репликации, когда подсеть реплики виртуальной машины основывается на подсети исходной виртуальной машины.

Если необходимо выполнить тестовую отработку отказа без указания сети Azure, никакая подготовка не требуется.

Для запуска тестовой отработки отказа с целевой сетью Azure потребуется создать новую сеть Azure, изолированную от рабочей среды Azure (обычная практика при создании новой сети в Azure). Дополнительные сведения см. в разделе [Запуск тестовой отработки отказа](site-recovery-failover.md#run-a-test-failover).

Чтобы полностью протестировать развертывание репликации и сети, необходимо настроить инфраструктуру так, чтобы реплицируемая виртуальная машина работала должным образом. Один из способов сделать это — настроить виртуальную машину как контроллер домена с помощью DNS и реплицировать ее в Azure с помощью службы Site Recovery, чтобы создать ее в тестовой сети, выполнив тестовую отработку отказа.  [Узнайте больше](site-recovery-active-directory.md#test-failover-considerations) о рекомендациях по тестированию отработки отказа для Active Directory.

Запустите тестовую отработку отказа следующим образом:

> [!NOTE]
> Для оптимальной производительности при отработке отказа в Azure необходимо, чтобы агент Azure был установлен на защищенном компьютере. Это ускоряет загрузку и помогает диагностировать неполадки. Агент Linux можно скачать [отсюда](https://github.com/Azure/WALinuxAgent), а агент Windows — [отсюда](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

1. На вкладке **Планы восстановления** выберите план и щелкните **Тестовая отработка отказа**.
2. На **странице подтверждения тестовой отработки отказа** выберите **Нет** или конкретную сеть Azure.  Учтите, что при выборе варианта **Нет** в ходе тестовой отработки отказа будет проверяться правильность репликации виртуальной машины в Azure, но не конфигурация сети репликации.

    ![Тестовая отработка отказа](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. На вкладке **Задания** можно отследить ход выполнения отработки отказа. Кроме того, вы можете просмотреть тестовую реплику виртуальной машины на портале Azure. Если настройки предусматривают доступ к виртуальным машинам из локальной сети, можно инициировать подключение протокола удаленного рабочего стола к виртуальной машине.
4. После того как отработка отказа достигнет фазы **Полное тестирование**, нажмите **Завершить тестирование**, чтобы закончить действие. Вы можете перейти на вкладку **Задания** , чтобы отслеживать ход выполнения отработки отказа и изменение ее состояния, а также выполнять прочие необходимые действия.
5. После отработки отказа тестовая реплика виртуальной машины будет отображаться на портале Azure. Если настройки предусматривают доступ к виртуальным машинам из локальной сети, можно инициировать подключение протокола удаленного рабочего стола к виртуальной машине.

   1. Проверьте, успешно ли выполнен запуск виртуальных машин.
   2. При необходимости подключиться к виртуальной машине в Azure с использованием удаленного рабочего стола после отработки отказа, включите подключение удаленного рабочего стола на виртуальной машине до запуска тестирования отработки отказа. Кроме того, на виртуальной машине потребуется добавить конечную точку RDP. Для этого можно использовать модуль [Runbook службы автоматизации Azure](site-recovery-runbook-automation.md) .
   3. После отработки отказа, если вы используете общедоступный IP-адрес для подключения к виртуальной машине в Azure с помощью удаленного рабочего стола, убедитесь, что у вас нет политик доменов, которые препятствуют подключению к виртуальной машине с помощью общедоступного адреса.
6. После завершения тестирования выполните следующие действия:

   * Щелкните пункт **Тестовая отработка отказа завершена**. Проведите очистку тестовой среды, чтобы автоматически отключить питание и удалить тестовые виртуальные машины.
   * Щелкните элемент **Примечания** для регистрации и сохранения любых наблюдений, связанных с тестовой отработкой отказа.
7. После того как отработка отказа достигнет фазы **Полное тестирование** , выполните следующую проверку:
   1. Просмотрите реплику виртуальной машины на портале Azure. Проверьте, успешно ли выполнен запуск виртуальной машины.
   2. Если настройки предусматривают доступ к виртуальным машинам из локальной сети, можно инициировать подключение протокола удаленного рабочего стола к виртуальной машине.
   3. Щелкните **Завершить проверку** для завершения действия.
   4. Щелкните элемент **Примечания** для регистрации и сохранения любых наблюдений, связанных с тестовой отработкой отказа.
   5. Щелкните пункт **Тестовая отработка отказа завершена**. Проведите очистку тестовой среды, чтобы автоматически отключить питание и удалить тестовую виртуальную машину.

## <a name="next-steps"></a>Дальнейшие действия
Настроив и запустив развертывание, ознакомьтесь с [дополнительными сведениями](site-recovery-failover.md) об отработке отказов.



<!--HONumber=Nov16_HO4-->


