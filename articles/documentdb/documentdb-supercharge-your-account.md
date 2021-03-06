---
title: "Расширение возможностей учетной записи DocumentDB S1 | Документация Майкрософт"
description: "Воспользуйтесь преимуществами увеличения пропускной способности в учетной записи DocumentDB S1, внеся несколько простых изменений на портале Azure."
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 6f373fb6-b0d9-4745-b17c-88e8bc5f906a
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 166e0bc7838510b52324fd9a040acd21d55118c4


---
# <a name="supercharge-your-documentdb-account"></a>Расширение возможностей учетной записи DocumentDB
Выполните следующие действия, чтобы воспользоваться преимуществами повышенной пропускной способности для учетной записи Azure DocumentDB S1. Практически без дополнительных затрат можно увеличить пропускную способность учетной записи S1 от 250 [единиц запросов в секунду](documentdb-request-units.md) до 400 единиц запросов в секунду и выше!  

> [!ВИДЕО https://channel9.msdn.com/Blogs/AzureDocumentDB/ChangeDocumentDBCollectionPerformance/player]
> 
> 

## <a name="change-to-user-defined-performance-in-the-azure-portal"></a>Переключение на пользовательскую производительность на портале Azure
1. В браузере откройте [**портал Azure**](https://portal.azure.com). 
2. Последовательно щелкните элементы **Обзор** -> **DocumentDB (NoSQL)**, затем выберите учетную запись DocumentDB для изменения.   
3. В группе связанных элементов **Базы данных** выберите базу данных, которую нужно изменить, а затем в колонке **База данных** выберите коллекцию с ценовой категорией S1.
   
      ![Снимок экрана колонки "База данных" и коллекции S1](./media/documentdb-supercharge-your-account/documentdb-change-performance-S1.png)
4. В колонке **Коллекция** щелкните **Дополнительно**, а затем выберите **Параметры**.   
5. В колонке **Параметры** щелкните **Ценовая категория**, чтобы отобразить оценку ежемесячных затрат для каждого плана. В колонке **Выбор ценовой категории** щелкните **Стандартная** и нажмите кнопку **Выбрать**, чтобы сохранить изменения.
   
      ![Снимок экрана с колонками "База данных" и "Выбор ценовой категории"](./media/documentdb-supercharge-your-account/documentdb-change-performance.png)
6. В колонке **Параметры** значение **Ценовая категория** меняется на **Стандартная** и появляется поле **Пропускная способность (единицы запросов в секунду)** со значением 400 (единиц запросов в секунду) по умолчанию. Нажмите кнопку **ОК** , чтобы сохранить изменения. 
   
   > [!NOTE]
   > Вы можете задать пропускную способность от 400 до 10 000 [единиц запросов](documentdb-request-units.md) в секунду (ЕЗ/с). **Сводка по ценам** в нижней части страницы автоматически обновляется, показывая оценку ежемесячных расходов.
   > 
   > 
   
    ![Снимок экрана колонки "Параметры", на котором показано, где можно изменить значение пропускной способности](./media/documentdb-supercharge-your-account/documentdb-change-performance-set-thoughput.png)
7. В колонке **База данных** можно проверить значение расширенной пропускной способности для коллекции. 
   
    ![Снимок экрана колонки "База данных" с измененной коллекцией](./media/documentdb-supercharge-your-account/documentdb-change-performance-confirmation.png)

Дополнительные сведения об изменениях, связанных с пользовательской и стандартной пропускной способностью, см. в записи блога [DocumentDB: Everything you need to know about using the new pricing options](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/) (DocumentDB: необходимая информация для использования новых вариантов ценообразования).

## <a name="next-steps"></a>Дальнейшие действия
Если в ходе работы выяснится, что требуется более высокая пропускная способность (больше 10 000 единиц запроса в секунду) или больший объем хранилища (больше 10 ГБ), то вы можете создать секционированную коллекцию. Сведения о создании секционированной коллекции см. в статье [Создание коллекции DocumentDB на портале Azure](documentdb-create-collection.md).




<!--HONumber=Nov16_HO3-->


