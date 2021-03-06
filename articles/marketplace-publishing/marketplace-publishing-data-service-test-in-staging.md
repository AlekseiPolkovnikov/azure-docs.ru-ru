---
title: "Тестирование предложения службы данных для Marketplace | Документация Майкрософт"
description: "Узнайте, как протестировать предложение службы данных для Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: daf2017bfbab4250e3f1481f592e0e858c77f121


---
# <a name="testing-your-data-service-offer-in-staging"></a>Тестирование предложения службы данных в промежуточной среде
> [!IMPORTANT]
> **В настоящее время мы больше не подключаем новые издатели служб данных. Новые службы данных не будут утверждены для добавления в список.** Дополнительные сведения о публикации бизнес-приложения SaaS на AppSource см. [здесь](https://appsource.microsoft.com/partners). Дополнительные сведения о публикации приложений IaaS или службы разработчика в Azure Marketplace см. [здесь](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

После выполнения первых двух шагов из статьи [Создание учетной записи разработчика Майкрософт](marketplace-publishing-accounts-creation-registration.md) и [руководства по публикации службы данных для Azure Marketplace](marketplace-publishing-data-service-creation.md) вы готовы выставить свое предложение в Azure Marketplace. В этом разделе описывается первый промежуточный шаг под названием "Промежуточное развертывание".

Промежуточное развертывание подразумевает развертывание предложения в частной песочнице, в которой вы можете проверить все его функциональные возможности и убедиться в их работе перед публикацией в рабочей среде. Предложение будет выглядеть в промежуточной среде точно так же, как на компьютере клиента после развертывания.

## <a name="step-1-pushing-your-offer-to-staging"></a>Шаг 1. Промежуточное развертывание предложения
Промежуточное развертывание позволяет протестировать предложение, прежде чем оно станет доступным для будущих подписчиков.  Вы увидите, как ваше предложение будет выглядеть и работать для тех, кто подпишется на вашу службу.  

  ![рисунок](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Войдите на [портал публикации](https://publish.windowsazure.com)
2. Выберите **Службы данных** в левом окне навигации.
3. Выберите предложение, которое нужно переместить в промежуточную среду. Появится экран, показанный выше.
4. Нажмите кнопку **Переместить в промежуточную среду** .  
5. Если с предложением есть проблемы, которые нужно устранить до размещения в промежуточной среде, отобразится их список.  Исправьте эти проблемы, щелкнув каждую их них в списке. Завершив исправления, нажмите кнопку **Переместить в промежуточную среду** еще раз.

Если проблем с предложением нет, то появится всплывающее окно, показанное ниже.  

Если вы не планируете или не хотите открывать общий доступ к своему предложению на портале Azure (в настоящее время ресурсы ограничены), то просто закройте всплывающее окно.

Для тестирования службы данных на портале Azure (помимо портала DataMarket) требуется идентификатор подписки Azure.  Этот идентификатор подписки указывает учетную запись, с помощью которой можно тестировать ваше предложение.  

Вырежьте и вставьте идентификатор подписки и установите флажок, чтобы продолжить.

  ![рисунок](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Эти идентификаторы подписки Azure требуются только для тестирования и размещения в промежуточной среде на [портале управления Azure](https://manage.windowsazure.com). Они не нужны для тестирования в Azure Marketplace.
> 
> 

На следующем экране выделенный желтым значок "Выполняется" показывает, что идет публикация. Перевод в промежуточную среду занимает от 10 до 15 минут.  Если прошло больше времени, сначала обновите страницу в браузере (в IE нажмите клавишу F5).  В редких случаях, когда предложение переводится в промежуточную среду больше часа, щелкните ссылку для связи с нами, чтобы сообщить о проблеме.

  ![рисунок](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

По завершении перевода в промежуточную среду значок "Выполняется" перестанет двигаться, а состояние предложения изменится на "Промежуточная среда".  Теперь все готово для тестирования предложения.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Шаг 2. Тестирование предложения в промежуточной среде в DataMarket
Щелкните ссылку после текста **"Просмотреть предложение службы..."** , чтобы открыть экран, который увидит подписчик, когда предложение будет опубликовано в рабочей среде и появится в DataMarket.

  ![рисунок](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Проверьте каждый из 12 элементов, отмеченных выше, и убедитесь, что все эмблемы, цены и транзакции, текст, изображения, документы и ссылки верны и работают правильно.  На этом этапе стоит проверить, что все тестовые значения, введенные при создании предложения, были заменены фактическими значениями.

1. Эмблема предложения
2. Название предложения
3. Имя издателя и ссылка на веб-сайт компании
4. Поисковые категории для предложения
5. Ссылка, по которой подписчики смогут получить поддержку по предложению
6. Контекстное описание предложения
7. План предложения, в котором отражены сведения о выставлении счетов
8. Ссылка на код реализации
9. Примеры изображений, иллюстрирующих использование предложения данных
10. Метаданные ввода-вывода для каждой службы в составе предложения
11. Условия использования предложения
12. Предварительная версия данных предложения

Наконец, убедитесь, что служба будет работать через Datamarket, щелкнув ссылку ПРОСМОТР НАБОРА ДАННЫХ.  В средстве, которое мы называем "обозревателем служб", откроется новое окно, где можно просмотреть результаты запроса к службе.  В этом окне можно ввести необходимые параметры и просмотреть результаты запроса к службе.   Также показан URL-адрес для запроса.  

> [!NOTE]
> Проверьте текстовое описание службы, отображенное вверху.  И если предложение включает вызовы нескольких служб, с помощью вкладок внизу перейдите к следующей службе для проверки.
> 
> 

## <a name="next-step"></a>Дальнейшие действия
Если у вас возникают проблемы и требуется помощь для их устранения, обратитесь в [службу поддержки издателей Azure](http://go.microsoft.com/fwlink/?LinkId=272975).

Если все в порядке и вы готовы к публикации предложения, ознакомьтесь с документацией [Запросить утверждение для перемещения в рабочую среду](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>См. также
* [Приступая к работе: как опубликовать предложение в Azure Marketplace](marketplace-publishing-getting-started.md)




<!--HONumber=Nov16_HO3-->


