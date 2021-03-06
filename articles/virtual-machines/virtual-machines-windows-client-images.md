---
title: Использование образов клиента Windows в сценариях разработки и тестирования | Microsoft Docs
description: Узнайте, как использовать преимущества подписки Visual Studio для развертывания Windows 7, 8 или 10 в Azure в сценариях разработки и тестирования.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: timlt
editor: ''

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: iainfou

---
# <a name="using-windows-client-in-azure-for-dev/test-scenarios"></a>Использование клиента Windows в Azure для сценариев разработки и тестирования
В сценариях разработки и тестирования Azure можно использовать Windows 7, Windows 8 или Windows 10 при условии, что у вас есть соответствующая подписка Visual Studio (прежнее название — MSDN). В этой статье описываются требования к доступности при запуске клиента Windows в Azure и использовании образов из коллекции Azure.

## <a name="subscription-eligibility"></a>Доступность в зависимости от подписки
Активные подписчики Visual Studio (пользователи, которые приобрели лицензию на подписку Visual Studio) могут использовать клиент Windows в целях разработки и тестирования. Вы можете использовать клиент Windows на собственном оборудовании и виртуальных машинах Azure, работающих в любом типе подписки Azure. Клиент Windows не может быть развернут или использоваться в Azure в обычной рабочей среде, а также недоступен для пользователей, не являющихся активными подписчиками Visual Studio.

Для вашего удобства мы выбрали несколько образов Windows 10 из коллекции Azure, которые можно использовать для разработки и тестирования. См. раздел [Доступные предложения для разработки и тестирования](#eligible-offers). Подписчики Visual Studio с предложением любого типа также смогут [правильно подготавливать и создавать](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-разрядные образы Windows 7, Windows 8 или Windows 10, а затем [отправлять их в Azure](virtual-machines-windows-upload-image.md). Они также могут использоваться только активными подписчиками Visual Studio и только в целях разработки и тестирования.

## <a name="eligible-offers"></a>Доступные предложения
В следующей таблице перечислены идентификаторы предложений, которые доступны для развертывания Windows 10 с помощью коллекции Azure. Образы Windows 10 отображаются только для указанных ниже предложений. Подписчики Visual Studio, которым необходимо запустить клиент Windows с помощью другого типа предложения, должны [правильно подготовить и создать](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-разрядный образ Windows 7, Windows 8 или Windows 10, а затем [передать его в Azure](virtual-machines-windows-upload-image.md).

| Название предложения | Номер предложения | Доступные образы клиента |
|:--- |:---:|:---:|
| [Разработка и тестирование с оплатой по мере использования](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Подписчики Visual Studio Enterprise (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Подписчики Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Подписчики Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium с подпиской MSDN (преимущество)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Подписчики Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Подписчики Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise — разработка и тестирование](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Проверка подписки Azure
Если вы не знаете идентификатор своего предложения, его можно найти на портале Azure или на портале учетных записей.

На портале Azure идентификатор предложения подписки указан в колонке "Подписки":

![Сведения об идентификаторе предложения на портале Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

На портале учетных записей Azure просмотреть идентификатор предложения можно на [вкладке "Подписки"](http://account.windowsazure.com/Subscriptions) :

![Сведения об идентификаторе предложения на портале учетных записей Azure](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 

## <a name="next-steps"></a>Дальнейшие действия
Теперь вы можете развернуть виртуальные машины с помощью [PowerShell](virtual-machines-windows-ps-create.md), [шаблонов Resource Manager](virtual-machines-windows-ps-template.md) или [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

<!--HONumber=Oct16_HO2-->


