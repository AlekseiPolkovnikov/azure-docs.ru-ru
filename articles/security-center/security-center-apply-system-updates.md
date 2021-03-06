---
title: "Применение обновлений системы в центре безопасности Azure | Документация Майкрософт"
description: "В этом документе показано, как реализовать рекомендации центра безопасности Azure **Применить обновления системы** и **Перезагрузить после обновления системы**."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 51501cc5e344c321652982a3a7f448d84f892392


---
# <a name="apply-system-updates-in-azure-security-center"></a>Применение обновлений системы в центре безопасности Azure
Центр безопасности Azure ежедневно проверяет наличие обновлений операционной системы виртуальных машин Windows и Linux. Центр безопасности получает список доступных критических обновлений и обновлений для системы безопасности из Центра обновления Windows или служб Windows Server Update Services в зависимости от того, какая служба настроена для виртуальной машины Windows.  Центр безопасности также проверяет наличие последних обновлений для систем Linux. Если на виртуальной машине отсутствует обновление системы, центр безопасности порекомендует его применить.

> [!NOTE]
> В документе приводится обзор службы с помощью примера развертывания.  Он не является пошаговым руководством.
> 
> 

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций
1. В колонке **Рекомендации** выберите **Применить обновления системы**.
   ![Применение обновлений системы][1]
2. В открывшейся колонке **Применить обновления системы** отобразится список обновлений системы, которые отсутствуют на виртуальных машинах. Выберите виртуальную машину.
   ![Выбор виртуальной машины][2]
3. Откроется колонка со списком отсутствующих обновлений для этой виртуальной машины. Выберите обновление системы. В этом примере выберем обновление KB3156016.
   ![Отсутствующие обновления системы безопасности][3]
4. Следуйте указаниям в колонке **обновления для системы безопасности** , чтобы применить отсутствующее обновление.
   ![Security update][4]

## <a name="reboot-after-system-updates"></a>Перезагрузка после завершения обновлений системы
1. Вернитесь в колонку **Рекомендации** . После применения обновлений системы будет создана запись с названием **Перезагрузить после завершения обновлений системы**. Эта запись свидетельствует о том, что вам нужно перезагрузить виртуальную машину, чтобы завершить применение обновлений системы.
   ![Перезагрузка после завершения обновлений системы][5]
2. Выберите **Перезагрузить после завершения обновлений системы**. Откроется колонка **Ожидается перезапуск для завершения обновлений системы** со списком виртуальных машин, которые необходимо перезагрузить, чтобы завершить применение обновлений системы.
   ![Ожидание перезагрузки][6]

Перезапустите виртуальную машину из Azure, чтобы завершить процесс.

## <a name="see-also"></a>Дополнительные материалы
Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](security-center-policies.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как наблюдать за работоспособностью ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/) — публикации блога, посвященные безопасности и соответствию требованиям в Azure.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png



<!--HONumber=Nov16_HO3-->


