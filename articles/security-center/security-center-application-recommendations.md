---
title: "Защита приложений в центре безопасности Azure | Документация Майкрософт"
description: "В этом документе рассматриваются рекомендации в центре безопасности Azure, которые помогают защитить приложения Azure и обеспечить соответствие политикам безопасности."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 174546d59124296711731de6c8d8929bada56baf
ms.openlocfilehash: dda826daa18182c1d415037faed5e832680392e1


---
# <a name="protecting-your-applications-in-azure-security-center"></a>Защита приложений в центре безопасности Azure
Центр безопасности Azure анализирует состояние безопасности ресурсов Azure. Когда центр безопасности выявляет потенциальные уязвимости в системе безопасности, он создает рекомендации по настройке необходимых элементов управления.  Рекомендации относятся к следующим типам ресурсов Azure: виртуальные машины (ВМ), сети, SQL и приложения.

В этой статье рассматриваются рекомендации, которые касаются приложений.  Они сфокусированы в основном на развертывании брандмауэра приложения.  В таблице ниже приведены справочные сведения по доступным рекомендациям, касающимся приложений, и действиям, выполняемым при их применении.

## <a name="available-application-recommendations"></a>Доступные рекомендации по приложениям
| Рекомендации | Описание |
| --- | --- |
| [Добавление брандмауэра веб-приложения](security-center-add-web-application-firewall.md) |Рекомендует выполнить развертывание брандмауэра веб-приложения (WAF) для конечных точек веб-службы. Вы можете защитить несколько веб-приложений в центре обеспечения безопасности, добавив эти приложения в существующие развернутые службы WAF. Устройства WAF (созданные с помощью модели развертывания Resource Manager) должны быть развернуты в отдельной виртуальной сети. Устройства WAF (созданные с помощью классической модели развертывания) ограничены заданной группой безопасности сети. В будущем эта поддержка будет расширена до полностью настраиваемого развертывания устройства WAF (в классической модели). |
| [Завершение подготовки защиты приложений](security-center-add-web-application-firewall.md#finalize-application-protection) |Для завершения настройки брандмауэра веб-приложения трафик должен перенаправляться в модуль WAF. В соответствии с этой рекомендацией в установку будут внесены необходимые изменения. |

## <a name="see-also"></a>Дополнительные материалы
Дополнительные сведения о рекомендациях, которые относятся к другим типам ресурсов Azure, см. в следующих статьях:

* [Защита виртуальных машин в центре безопасности Azure.](security-center-virtual-machine-recommendations.md)
* [Защита сети в центре безопасности Azure](security-center-network-recommendations.md)
* [Защита службы SQL Azure в центре безопасности Azure.](security-center-sql-service-recommendations.md)

Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](security-center-policies.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.



<!--HONumber=Nov16_HO3-->


