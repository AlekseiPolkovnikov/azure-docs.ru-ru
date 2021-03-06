---
title: "Включение аудита для баз данных SQL в центре безопасности Azure | Документация Майкрософт"
description: "В этом документе объясняется, как выполнить рекомендацию центра безопасности Azure по включению аудита для баз данных SQL."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0084e4e3ffd7284e237a9ed8544b191e1fb55a53


---
# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Включение аудита для баз данных SQL в центре безопасности Azure
Центр безопасности Azure порекомендует включить аудит для всех баз данных SQL, если аудит еще не включен. Аудит может помочь вам соблюсти требования нормативов, проанализировать работу с базой данных и получить представление о расхождениях и аномалиях, которые могут указывать на бизнес-проблемы или предполагаемые нарушения безопасности.

После включения аудита можно настроить параметры обнаружения угроз и электронных сообщений для получения оповещений системы безопасности. Система обнаружения угроз обнаруживает подозрительную активность в базе данных, указывающую на наличие потенциальных угроз безопасности. Это позволяет выявлять потенциальные угрозы и соответствующим образом реагировать при их возникновении.

Данная рекомендация относится только к службе SQL Azure и не охватывает службы SQL, работающие на виртуальных машинах.

> [!NOTE]
> В документе приводится обзор службы с помощью примера развертывания.  Он не является пошаговым руководством.
> 
> 

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций
1. В колонке **Рекомендации** выберите **Включить аудит для баз данных SQL**.  Откроется колонка **Включить аудит для баз данных SQL** .
   ![Enable auditing on SQL databases][1]
2. Выберите базу данных SQL для включения аудита. Откроется колонка **Аудит и обнаружение угроз**.
   ![Аудит и обнаружение угроз][2]
3. В колонке **Аудит и обнаружение угроз** в разделе **Аудит** щелкните **Вкл.**
   ![Включение аудита и обнаружения угроз][3]
4. Следуйте указаниям в разделе [Приступая к работе с системой обнаружения угроз базы данных SQL](../sql-database/sql-database-threat-detection-get-started.md) , чтобы включить и настроить обнаружение угроз, а также настроить список электронных адресов, на которые будут отправляться предупреждения системы безопасности при обнаружении аномальных действий.

## <a name="see-also"></a>Дополнительные материалы
В этой статье показано, как выполнить рекомендацию центра безопасности по включению аудита баз данных SQL. Чтобы узнать больше о защите базы данных SQL, ознакомьтесь со следующими статьями.

* [Защита Базы данных SQL](../sql-database/sql-database-security.md)

Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](security-center-policies.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как наблюдать за работоспособностью ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/) — последние новости и информация об обеспечении безопасности в Azure.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png



<!--HONumber=Nov16_HO3-->


