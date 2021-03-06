---
title: Настройка подключения между виртуальными сетями для классической модели развертывания| Microsoft Docs
description: Узнайте, как подключить между собой виртуальные сети Azure, используя PowerShell и классический портал Azure.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: cherylmc

---
# Настройка подключения между виртуальными сетями для классической модели развертывания
> [!div class="op_single_selector"]
> * [Классический портал Azure](virtual-networks-configure-vnet-to-vnet-connection.md)
> * [PowerShell и Azure Resource Manager](vpn-gateway-vnet-vnet-rm-ps.md)
> 
> 

Данная статья поможет вам создать виртуальные сети и подключить их с помощью классической модели развертывания (которую также называют управлением службами). В приведенных ниже инструкциях описывается, как с помощью классического портала Azure создать виртуальные сети и шлюзы, а с помощью PowerShell — настроить подключение типа "виртуальная сеть — виртуальная сеть". Настроить такое подключение на портале невозможно.

![Схема подключения между виртуальными сетями](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)

### Модели развертывания и инструменты для подключения между виртуальными сетями
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Подключение "виртуальная сеть–виртуальная сеть" можно настроить в обеих моделях развертывания с помощью различных средств. Дополнительные сведения приведены в таблице ниже. Мы обновляем эту таблицу по мере выпуска новых статей, моделей развертывания и дополнительных инструментов для этой конфигурации. Если статья доступна, таблица ссылается на нее напрямую.

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## О подключениях "виртуальная сеть — виртуальная сеть"
Подключение одной виртуальной сети к другой похоже на подключение виртуальной сети к локальному сайту. В обоих типах подключений используется VPN-шлюз для создания защищенного туннеля, использующего IPsec/IKE.

Подключаемые виртуальные сети могут иметь разные подписки в разных регионах. Можно комбинировать подключение "виртуальная сеть — виртуальная сеть" с многосайтовыми конфигурациями. Это позволяет настраивать топологии сети, совмещающие распределенные подключения с подключениями между виртуальными сетями.

### Что может дать связь между виртуальными сетями
Вам может потребоваться подключить виртуальные сети по следующим причинам.

* **Межрегиональная географическая избыточность и географическое присутствие**
  
  * Вы можете настроить собственную георепликацию или синхронизацию с безопасной связью без прохождения трафика через конечные точки с выходом в Интернет.
  * С помощью Azure Load Balancer и технологий кластеризации корпорации Майкрософт или сторонних производителей можно настроить высокодоступную рабочую нагрузку с геоизбыточностью в нескольких регионах Azure. Одним из важных примеров является настройка SQL Always On с группами доступности, распределенными между несколькими регионами Azure.
* **Региональные многоуровневые приложения с четкой границей изоляции**
  
  * В одном регионе можно настроить многоуровневые приложения с использованием нескольких связанных друг с другом виртуальных сетей с сильной изоляцией и защищенной связью между уровнями.
* **Обмен данными между подписками и организациями в Azure**
  
  * Если у вас есть несколько подписок Azure, то вы можете безопасно связывать рабочие нагрузки из разных подписок между виртуальными сетями.
  * Благодаря технологии защищенных сетей VPN в Azure для предприятий или поставщиков услуг возможен обмен данными между организациями.

### Часто задаваемые вопросы о подключениях "виртуальная сеть — виртуальная сеть" для классических виртуальных сетей
* Виртуальные сети могут быть в одной или разных подписках.
* Виртуальные сети могут быть в одном или разных регионах (расположениях) Azure.
* Облачная служба или конечная точка балансировки нагрузки не может распространяться на виртуальные сети, даже если они соединены друг с другом.
* Для подключения нескольких виртуальных сетей друг к другу не требуются VPN-устройства.
* Подключения между виртуальными сетями поддерживает подключение виртуальных сетей Azure. Не поддерживается подключение виртуальных машин или облачных служб, не развернутых в виртуальной сети.
* Для подключения "виртуальная сеть — виртуальная сеть" требуются шлюзы с динамической маршрутизацией. Шлюзы Azure со статической маршрутизацией не поддерживаются.
* Подключение к виртуальной сети можно использовать одновременно с многосайтовыми VPN-подключениями. Можно использовать до 10 туннелей VPN для подключения VPN-шлюза виртуальной сети к другим виртуальным сетям или локальным сайтам.
* Адресные пространства виртуальных сетей и местных сайтов локальных сетей не должны перекрываться. Перекрытие адресных пространств приведет к сбою при создании виртуальных сетей или отправке файлов конфигурации netcfg.
* Избыточные туннели между парой виртуальных сетей не поддерживаются.
* Все туннели VPN виртуальной сети, включая VPN-подключения типа "точка — сеть", совместно используют доступную пропускную способность VPN-шлюза Azure и регулируются одним соглашением об уровне обслуживания, определяющим время работы VPN-шлюзов в Azure.
* Трафик между виртуальными сетями проходит через магистральную сеть Azure.

## <a name="step1"></a>Шаг 1. Планирование диапазонов IP-адресов
Важно определить диапазоны адресов, которые будут использоваться при настройке виртуальных сетей. При настройке необходимо убедиться в том, что никакие из диапазонов виртуальных сетей или диапазонов локальных сетей, к которым они подключены, никак не перекрываются между собой.

В таблице ниже показан пример определения виртуальных сетей. Используйте указанные диапазоны только в качестве примера. Запишите диапазоны для своих виртуальных сетей. Эти сведения будут необходимы для выполнения последующих действий.

**Примеры настроек**

| Виртуальная сеть | Пространство адресов | Регион | Подключается к сайту локальной сети |
|:--- |:--- |:--- |:--- |
| VNet1 |VNet1 (10.1.0.0/16) |Западная часть США |VNet2Local (10.2.0.0/16) |
| VNet2 |VNet2 (10.2.0.0/16) |Восточная часть Японии |VNet1Local (10.1.0.0/16) |

## Шаг 2. Создание виртуальной сети VNet1
На этом шаге мы создаем виртуальную сеть VNet1. При использовании любого из примеров не забудьте подставить собственные значения. Если у вас уже есть виртуальная сеть, то можете пропустить этот шаг. Однако необходимо проверить, не перекрываются ли диапазоны IP-адресов с диапазонами вашей второй виртуальной сети или с любыми другими виртуальными сетями, к которым вы хотите подключиться.

1. Перейдите на [классический портал Azure](https://manage.windowsazure.com). В этой статье мы используем классический портал, так как некоторые обязательные параметры конфигурации еще не доступны на портале Azure.
2. В левом нижнем углу экрана щелкните **Создать** > **Сетевые службы** > **Виртуальная сеть** > **Настраиваемое создание**, чтобы запустить мастер настройки. На страницах мастера введите указанные значения.

### Информация о виртуальной сети
На странице Описание виртуальной сети укажите следующие сведения.

  ![Информация о виртуальной сети](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

* **Имя** — имя виртуальной сети. Например, VNet1.
* **Расположение** — при создании виртуальную сеть можно связать с расположением Azure (регионом). Например, если вы хотите, чтобы ваши виртуальные машины разворачивались в виртуальной сети, которая физически расположена в западной части США, выберите это расположение. Нельзя изменить расположение, связанное с виртуальной сетью, после ее создания.

### DNS-серверы и подключение VPN
На странице DNS-серверы и VPN-соединения укажите следующие сведения и нажмите стрелку "Далее" в правом нижнем углу.

  ![DNS-серверы и подключение VPN](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)

* **DNS-серверы**: введите имя и IP-адрес DNS-сервера или выберите ранее зарегистрированный DNS-сервер из раскрывающегося списка. Этот параметр не приводит к созданию DNS-сервера. Он позволяет указать DNS-сервер, который вы хотите использовать для разрешения имен в этой виртуальной сети. Если вы хотите обеспечить разрешение имен между своими виртуальными сетями, то нужно настроить DNS-сервер, а не использовать службу разрешения имен, предоставленную Azure.
* Не устанавливайте ни один из флажков для подключений типа "точка-сеть" (P2S) и "сеть-сеть" (S2S). Щелкните стрелку в правом нижнем углу, чтобы перейти к следующему экрану.

### Адресные пространства виртуальной сети
На странице "Адресное пространство виртуальной сети" укажите диапазон адресов, который вы хотите использовать для виртуальной сети. Это динамические IP-адреса (DIP), которые будут назначаться виртуальным машинам и другим экземплярам ролей, развертываемым в этой виртуальной сети.

При создании виртуальной сети, которая также будет иметь подключение к локальной сети, особенно важно выбрать диапазон, который не перекрывается с другими диапазонами, используемыми в локальной сети. В этом случае нужно обратиться за помощью к администратору сети. Он может выделить для вашей виртуальной сети диапазон IP-адресов из адресного пространства локальной сети.

  ![Страница «Адресное пространство виртуальной сети»](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

* **Адресное пространство** — включает в себя начальный IP-адрес и число адресов. Убедитесь, что указанные адресные пространства не перекрываются ни с одним из адресных пространств, которые используются в локальной сети. В этом примере для VNet1 мы используем 10.1.0.0/16.
* **Добавить подсеть** — включает в себя начальный IP-адрес и число адресов. Дополнительные подсети не обязательны, но вам может понадобиться создать отдельную подсеть для виртуальных машин со статическими адресами DIPS. Или может потребоваться разместить виртуальные машины в подсети отдельно от других экземпляров ролей.

**Установите флажок** в правом нижнем углу страницы, после чего начнется создание виртуальной сети. По завершении на странице "Сети" в поле "Состояние" появится значение "Создано".

## Шаг 3. Создание виртуальной сети VNet2
Далее повторите предыдущие шаги, чтобы создать еще одну виртуальную сеть. В дальнейшем вы подключите эти виртуальные сети между собой. Можно воспользоваться [примерами настроек](#step1) из шага 1. Если у вас уже есть виртуальная сеть, то можете пропустить этот шаг. Тем не менее необходимо проверить, не перекрываются ли диапазоны IP-адресов с диапазонами других виртуальных сетей или с локальными сетями, к которым вы хотите подключиться.

## Шаг 4. Добавление сайтов локальной сети
При создании конфигурации "виртуальная сеть — виртуальная сеть" необходимо настроить сайты локальной сети, которые отображаются на странице **Локальные сети** портала. С помощью параметров, заданных в каждом сайте локальной сети, Azure определяет способ маршрутизации трафика между виртуальными сетями. Укажите имена, которые будут использоваться для ссылки на каждый из сайтов локальной сети. Лучше всего использовать описательные имена, так как в дальнейшем вам потребуется выбирать значение из раскрывающегося списка.

Например, сеть VNet1 подключается к созданному вами сайту локальной сети с именем VNet2Local. Параметры сайта VNet2Local содержат префиксы адреса для сети VNet2, а также общедоступный IP-адрес для шлюза VNet2. VNet2 подключается к созданному вами сайту локальной сети VNet1Local, который содержит префиксы адреса для сети VNet1 и общедоступный IP-адрес для шлюза VNet1.

### <a name="localnet"></a>Добавление сайта локальной сети VNet1Local
1. В левом нижнем углу экрана щелкните **Создать** > **Сетевые службы** > **Виртуальная сеть** > **Добавить локальную сеть**.
2. На странице **Укажите сведения о локальной сети** в поле **Имя** введите имя, которое будет представлять сеть, к которой вы хотите подключиться. В этом примере можно использовать VNet1Local, чтобы сослаться на диапазоны IP-адресов и шлюз для VNet1.
3. В поле **IP-адрес устройства VPN (необязательно)** укажите любой допустимый общедоступный IP-адрес. Как правило, для VPN-устройства используется фактический внешний IP-адрес. Для конфигураций "виртуальная сеть — виртуальная сеть" используется общедоступный IP-адрес, назначенный шлюзу для вашей виртуальной сети. Но так как вы еще не создали шлюз, то можно указать любой допустимый общедоступный IP-адрес в качестве заполнителя. Не оставляйте это поле пустым, так как оно является обязательным для данной конфигурации. Позднее вы вернетесь к этим параметрам и введете соответствующий IP-адрес шлюза, после того, как Azure создаст его. Щелкните стрелку, чтобы перейти к следующему экрану.
4. На странице **Укажите адрес** введите диапазон IP-адресов и количество адресов для VNet1. Он должен точно соответствовать диапазону, который задан для VNet1. Azure использует указанный вами диапазон IP-адресов для маршрутизации трафика, предназначенного для VNet1. Установите флажок, чтобы создать локальную сеть.

### Добавление сайта локальной сети VNet2Local
Используйте описанные выше действия для создания сайта локальной сети VNet2Local. При необходимости воспользуйтесь значениями в [примерах настроек](#step1) из шага 1.

### Настройка каждой виртуальной сеть, чтобы она указывала на локальную сеть
Каждая виртуальная сеть должна указывать на соответствующую локальную сеть, в которую будет выполняться маршрутизация трафика.

#### Для VNet1
1. Перейдите на страницу **Настройка** для виртуальной сети **VNet1**.
2. В разделе подключений типа "сеть — сеть" выберите "Подключиться к локальной сети", а затем из раскрывающегося списка выберите **VNet2Local** в качестве локальной сети.
3. Сохраните параметры.

#### Для VNet2
1. Перейдите на страницу **Настройка** для виртуальной сети **VNet2**.
2. В разделе подключений типа "сеть — сеть" выберите "Подключиться к локальной сети", а затем из раскрывающегося списка выберите **VNet1Local** в качестве локальной сети.
3. Сохраните параметры.

## Шаг 5. Настройка шлюза для каждой виртуальной сети
Настройте шлюз динамической маршрутизации для каждой виртуальной сети. Данная конфигурация не поддерживает шлюзы статической маршрутизации. Если вы используете виртуальные сети, которые были настроены ранее и уже имеют шлюзы динамической маршрутизации, то вы можете пропустить этот шаг. Если ваши шлюзы используют статическую маршрутизацию, то их следует удалить и повторно создать как шлюзы с динамической маршрутизацией. В случае удаления шлюза общедоступный IP-адрес, назначенный ему, также удалится, поэтому необходимо вернуться и повторно настроить все локальные сети и VPN-устройства, указав новый общедоступный IP-адрес нового шлюза.

1. На странице **Сети** убедитесь, что в столбце состояния виртуальной сети отображается значение **Создана**.
2. В столбце **Имя** щелкните имя виртуальной сети. В этом примере мы используем VNet1.
3. На странице **Панель мониторинга** обратите внимание, что для этой виртуальной сети шлюз еще не настроен. Вы увидите, как состояние будет изменяться в процессе настройки шлюза.
4. Щелкните **Создать шлюз** и **Динамическая маршрутизация** в нижней части страницы. Когда система запрашивает подтверждение создания шлюза, нажмите кнопку «Да».
   
      ![Тип шлюза](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)
5. Обратите внимание, что во время создания шлюза его графическое представление на странице меняет цвет на желтый и появляется надпись "Создание шлюза". Обычно создание шлюза занимает около 30 минут.
6. Повторите те же действия для сети VNet2. Вам не обязательно завершать настройку шлюза первой виртуальной сети до начала создания шлюза для второй виртуальной сети.
7. Когда состояние шлюза изменится на "Подключение", общедоступный IP-адрес для каждого шлюза отобразится на панели мониторинга. Запишите IP-адрес, соответствующий каждой виртуальной сети, стараясь не перепутать их. Это IP-адреса, которые используются при редактировании заполнителей IP-адресов для VPN-устройства для каждой локальной сети.

## Шаг 6. Изменение локальной сети
1. На странице **Локальные сети** щелкните имя локальной сети, которую требуется изменить, а затем щелкните **Изменить** в нижней части страницы. В поле **IP-адрес VPN-устройства** введите IP-адрес шлюза, который соответствует сети. Например, для VNet1Local введите IP-адрес шлюза, назначенный VNet1. Щелкните стрелку в нижней части страницы.
2. На странице **Указать адресное пространство** установите флажок в нижнем правом углу без внесения изменений.

## Шаг 7. Создание VPN-подключения
После завершения всех предыдущих шагов задайте общие ключи IPsec/IKE и создайте подключение. Для этого набора действий используется только PowerShell. Выполнить их на портале невозможно. Дополнительные сведения об установке командлетов Azure PowerShell см. в статье [Установка и настройка Azure PowerShell](../powershell-install-configure.md). Обязательно скачайте последнюю версию командлетов управления службой (SM).

1. Откройте Windows PowerShell и выполните вход.
   
        Add-AzureAccount
2. Выберите подписку, в которой находятся ваши виртуальные сети.
   
        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"
3. Создайте подключения. Обратите внимание, что в приведенных ниже примерах общий ключ совпадает. Общий ключ всегда должен совпадать.

    Подключение VNet1 к VNet2

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    Подключение VNet2 к VNet1

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

1. Ожидание подключений для инициализации. После инициализации шлюз будет выглядеть как на приведенном ниже рисунке.
   
    ![Состояние шлюза — «Подключен»](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)
   
    [!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

## Дальнейшие действия
В виртуальные сети можно добавлять виртуальные машины. Дополнительные сведения см. в [документации по виртуальным машинам](https://azure.microsoft.com/documentation/services/virtual-machines/).

[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks


<!---HONumber=AcomDC_0831_2016-->