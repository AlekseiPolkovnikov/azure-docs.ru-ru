---
title: Информация о пакете Azure SDK для NET
description: Узнайте, что включено в пакет SDK для Azure для .NET.
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: mollybos
services: ''

ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/30/2016
ms.author: rachelap

---
# Что такое «пакет Azure SDK для .NET»?
## Обзор
Пакет Azure SDK для .NET представляет собой набор средств для Visual Studio, средств командной строки, исполняемых двоичных файлов и клиентских библиотек, помогающих при разработке, тестировании и развертывании приложений, запускаемых в Azure. Эта статья содержит подробные сведения о том, что вы получите после установки пакета SDK. Вы можете установить пакет SDK, загрузив его со страницы [Загрузки Azure](https://azure.microsoft.com/downloads/).

Пакет Azure SDK для .NET также содержит [клиентские библиотеки для использования служб Azure](http://go.microsoft.com/fwlink/?LinkId=510472). Эти библиотеки устанавливаются отдельно при использовании NuGet.

## <a id="included"></a>Что входит в состав пакета Azure SDK для .NET?
Пакет Azure SDK для .NET устанавливает следующие программы:

* [Выпуск Visual Studio Community 2015](#vwd)
* [Эмулятор хранилища Microsoft Azure](#stgemulator)
* [Средства хранилища Microsoft Azure](#stgtools)
* [Средства создания Microsoft Azure](#auth)
* [Эмулятор Microsoft Azure](#emulator)
* [Средство HDInsight для Visual Studio и драйвер ODBC для Microsoft Hive](#hdinsight)
* [Библиотеки Microsoft Azure для .NET](#libraries)
* [Пакет SDK для мобильных приложений Microsoft Azure](#mobile)
* [Установка Microsoft Azure PowerShell](#ps)
* [Пакет Microsoft Azure Tools для Microsoft Visual Studio](#tools)
* [Microsoft ASP.NET и пакет Web Tools для Visual Studio](#wte)
* [Средства озера данных Microsoft Azure для Visual Studio](#datalake)

### <a id="vwd"></a>Visual Studio Community Edition 2015
Если на компьютере не установлен Visual Studio, пакет SDK установит [Visual Studio Community Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a id="stgemulator"></a>Эмулятор хранилища Microsoft Azure
[Эмулятор хранилища Microsoft Azure](http://msdn.microsoft.com/library/hh403989.aspx) использует экземпляр SQL Server и локальную файловую систему для эмуляции хранилища Azure (запросы, таблицы, двоичные объекты), что позволяет проводить тестирование локально.

### <a id="stgtools"></a>Средства хранилища Microsoft Azure
Будет установлена программа командной строки [AzCopy](http://aka.ms/AzCopy), которую можно использовать для переноса данных в учетную запись хранения Azure и из нее.

### <a id="auth"></a>Средства создания Microsoft Azure
В их состав входят:

* [Средство командной строки CSPack](http://msdn.microsoft.com/library/gg432988.aspx) для создания пакетов развертывания.
* [Средство командной строки CSEncrypt](http://msdn.microsoft.com/library/hh404001.aspx) для шифрования паролей, используемых для доступа к экземплярам ролей облачных служб через подключение к удаленному рабочему столу.
* Исполнимые двоичные файлы, которые требуются проектам облачных служб для коммуникации со средой выполнения и диагностики. Эти файлы недоступны в пакетах NuGet.

### <a id="emulator"></a>Эмулятор Microsoft Azure
[Эмулятор Azure](http://msdn.microsoft.com/library/dn339018.aspx) имитирует среду облачной службы, чтобы вы могли тестировать проекты облачных служб на локальном компьютере перед их развертыванием в Azure.

### <a id="hdinsight"></a>Средство HDInsight для Visual Studio и драйвер ODBC для Microsoft Hive
Средства HDInsight в обозревателе серверов позволяют перемещаться по базам данных Hive и связанным хранилищам учетных записей кластеров HDInsight, создавать таблицы, а также создавать и отправлять запросы Hive. Дополнительную информацию см. в статье [Начало работы со средствами HDInsight Hadoop для Visual Studio](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

### <a id="libraries"></a>Библиотеки Microsoft Azure для .NET
В их состав входят:

* Пакеты NuGet для хранилища Azure, сервисной шины и кэширования, которые сохраняются на компьютере, чтобы в Visual Studio можно было создать новые проекты облачных служб без доступа в Интернет.
* Дополнение для Visual Studio, позволяющее запускать проекты [службы кэша](http://msdn.microsoft.com/library/dn386103.aspx) локально в Visual Studio.

### <a id="mobile"></a>Пакет SDK для мобильных приложений Microsoft Azure
Инструменты для работы с [Мобильным приложением службы приложений Azure](app-service-mobile/app-service-mobile-value-prop.md).

### <a id="ps"></a>Установка Microsoft Azure PowerShell
Azure PowerShell позволяет [автоматизировать создание и развертывание среды Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything).

### <a id="tools"></a>Пакет Microsoft Azure Tools для Microsoft Visual Studio
Это позволяет работать с ресурсами Azure, в первую очередь с облачными службами и виртуальными машинами:

* [создавать, открывать и публиковать проекты облачных служб](cloud-services/cloud-services-dotnet-get-started.md);
* [создавать пакеты развертывания для проектов облачных служб](http://msdn.microsoft.com/library/ff683672.aspx);
* [создавать виртуальные машины Azure при создании новых веб-проектов](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md);
* [создавать сценарии PowerShell при создании новых виртуальных машин](http://msdn.microsoft.com/library/dn642480.aspx);
* [просматривать настройки проекта облачных служб в окне свойств проекта Visual Studio и управлять ими;](http://msdn.microsoft.com/library/ee405486.aspx)
* просматривать [облачные службы](http://msdn.microsoft.com/library/ff683675.aspx), [виртуальные машины](http://msdn.microsoft.com/library/jj131259.aspx) и [сервисную шину](http://msdn.microsoft.com/library/jj149828.aspx) в обозревателе серверов и управлять ими;
* [удаленно запускать в режиме отладки облачные службы и виртуальные машины](http://msdn.microsoft.com/library/ff683670.aspx).
* [Автоматизация подготовки ресурсов с помощью проектов развертывания группы ресурсов Azure](https://msdn.microsoft.com/library/dn872471.aspx)

### <a id="wte"></a>Средства службы приложений Microsoft для Visual Studio
Это позволит вам работать с веб-сайтами Azure:

* [Публиковать веб-проекты на веб-сайтах Azure](app-service-web/web-sites-dotnet-get-started.md).
* [Публикация проектов консольных приложений в веб-заданиях Azure](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Создание ресурсов веб-сайтов Azure Website и баз данных SQL при создании нового веб-проекта или при его публикации](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Создание сценариев развертывания PowerShell при создании новых веб-сайтов](http://msdn.microsoft.com/library/dn642480.aspx).
* [Управление и устранение неполадок веб-сайтов Azure в обозревателе серверов](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Удаленный запуск веб-сайтов и веб-заданий в режиме отладки](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

> [!NOTE]
> Чтобы пользоваться этими возможностями, не нужно устанавливать пакет Azure SDK для .NET, так как они включены в состав обновлений Visual Studio.
> 
> 

## <a id="datalake"></a>Средства озера данных Microsoft Azure для Visual Studio
Дополнительные сведения см. в статье [Учебник. Разработка скриптов U-SQL с помощью средств озера данных для Visual Studio](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a id="notincluded"></a>Что не входит в состав пакета Azure SDK для .NET?
Есть некоторые компоненты, которые не включены в установочный пакет SDK, но могут вам понадобиться при разработке для Azure. Наиболее важные из них:

* [Клиентские библиотеки](http://go.microsoft.com/fwlink/?LinkId=510472)
  
    Пакет SDK для Azure включает клиентские библиотеки для использования служб Azure, но не все они устанавливаются при установке SDK. Если приложению необходима клиентская библиотека, которая не устанавливается с SDK, ее можно получить на сайте [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Если приложение использует клиентскую библиотеку, которая устанавливается с SDK, ее лучше всего обновить текущей версией с сайта NuGet.org.
  
      **Локальные копии клиентских библиотек** Пакет Azure SDK для .NET копирует на компьютер пакеты NuGet для некоторых клиентских библиотек Azure, таких как Хранилище, Service Bus и Кэширование. Эти клиентские библиотеки автоматически добавляются в новые проекты облачных служб, поэтому локальные пакеты NuGet позволяют Visual Studio создавать проекты даже в случае отсутствия подключения к Интернету. Клиентские библиотеки, как правило, обновляются чаще, чем выпускаются новые версии пакета SDK, поэтому на сайте NuGet.org зачастую имеются более новые версии клиентских библиотек, чем те, которые включены в пакет SDK.
  
    **Шаблоны проектов, включающие клиентские библиотеки** Только в шаблоны проектов [облачной службы Azure](cloud-services/cloud-services-dotnet-get-started.md) и [мобильной службы Azure](mobile-services/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard.md) автоматически включены некоторые клиентские библиотеки. Для того чтобы получить другие библиотеки или шаблоны, установите необходимые [пакеты клиентских библиотек NuGet](http://go.microsoft.com/fwlink/?LinkId=510472).
* [Шаблоны проектов мобильных служб Azure](mobile-services/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard.md)
  
    Шаблоны проектов мобильных служб Azure доступны только в Visual Studio 2013 с обновлением 2 и более поздних версиях. При этом недоступны в Visual Studio 2012 и более ранних версиях, а также в Visual Studio 2013 с пакетом обновлений 1 или более ранних версиях, даже после установки Azure SDK для .NET.

## <a id="faq"></a>Часто задаваемые вопросы
* [Многие компоненты Azure уже входят в состав Visual Studio. Нужно ли мне установить пакет Azure SDK для .NET?](#azinvs)
* [Мне нужна клиентская библиотека. Нужно ли устанавливать пакет Azure SDK для .NET, чтобы получить ее?](#clientlib)
* [Где можно найти предыдущие версии пакета SDK для Azure для .NET?](#olderversions)
* [Какова политика поддержки жизненного цикла пакета SDK для Azure для .NET?](#lifecycle)
* [С какими версиями гостевых ОС совместим пакет SDK для Azure для .NET?](#guestos)
* [Как удалить пакет SDK для Azure для .NET?](#uninstall)

### <a id="azinvs"></a>Многие компоненты Azure уже входят в состав Visual Studio. Нужно ли мне установить пакет Azure SDK для .NET?
Рекомендуется устанавливать пакет SDK чтобы вести разработку для Azure с использованием современных средств. Если вы не хотите устанавливать пакет SDK, вы можете сделать это, если выполняются следующие условия:

* установлено последнее [обновление для Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5);
* вы разрабатываете только для веб-сайтов или мобильных служб Azure, а не для облачных служб или виртуальных машин;
* приложение не использует службу хранилища либо оно ее использует, но у вас нет необходимости в эмуляторе хранилища или средстве AzCopy.

### <a id="clientlib"></a>Мне нужна клиентская библиотека. Нужно ли устанавливать пакет Azure SDK для .NET, чтобы получить ее?
Пакет SDK устанавливает только клиентские библиотеки, поэтому можно создать проект облачной службы, даже если вы не подключены к Интернету. Самые последние версии клиентских библиотек доступны в виде пакетов NuGet на сайте [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472). Дополнительные сведения см. в разделе [Что не входит в состав пакета Azure SDK для .NET?](#notincluded) ранее в этом же документе.

### <a id="olderversions"></a>Где можно найти предыдущие версии пакета SDK для Azure для .NET?
Предыдущие версии пакета размещены на странице загрузки [пакета Azure SDK для .NET](https://azure.microsoft.com/downloads/archive-net-downloads/).

### <a id="lifecycle"></a>Какова политика поддержки жизненного цикла пакета SDK для Azure для .NET?
См. [Политика поддержки жизненного цикла облачных служб Microsoft Azure](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

### <a id="guestos"></a>С какими версиями гостевых ОС совместим пакет SDK для Azure для .NET?
См. [Таблицу совместимости версий гостевых ОС и Azure SDK](http://msdn.microsoft.com/library/ee924680.aspx).

### <a id="uninstall"></a>Как удалить пакет SDK для Azure для .NET?
Каждый элемент, описанный в данной статье в разделе [Что входит в состав пакета SDK для Azure для .NET](#included), является отдельной программой в списке установленных программ на панели управления Windows **Программы и компоненты**. Групповое удаление данных элементов невозможно, необходимо удалять каждую программу по отдельности.

Если у вас уже установлен пакет SDK для Azure для .NET, и вы устанавливаете новую версию, нет никакой необходимости в удалении старой. В большинстве случаев установка пакета SDK обновляет существующую программу, а не добавляет новую взамен старой.

Тем не менее, если вы хотите удалить оставшуюся часть более ранней версии, в которой вы больше не нуждаетесь, удалите только программы, в которых указан номер старой версии и делайте это только при наличии той же программы более поздней версии. Например, после обновления с версии 2.5 до версии 2.6 вы сможете видеть «Инструменты Microsoft Azure для Microsoft Visual Studio 2013» и в версии 2.5, и в версии 2.6, и вы сможете удалить версию 2.5. Но «Средства создания Microsoft Azure» вы можете увидеть только в версии 2.5, и удалять их не безопасно.

Обратите внимание, что номера версий в названии программы, которые отображаются в **Программах и компонентах** могут ввести в заблуждение. Например, пакет SDK версии 2.6 включает «Пакет SDK версии 1.0 для мобильного приложения Microsoft Azure» при установке пакета SDK для Visual Studio 2013 и «Пакет SDK версии 2.0 для мобильного приложения Microsoft Azure» для Visual Studio 2015. Индикатором в данном случае является не версия пакета SDK, но индикатор версии Visual Studio, к которой применяется программа.

В данной статье не перечисляются программы, которые включались в более ранние версии пакета SDK для Azure. Имеются другие программы, которые можно удалить из предыдущих версий пакета SDK, так как в более ранние версии пакета SDK иногда включены программы, которые были пропущены в более поздних версиях. Например, версия 2.5 устанавливает «Azure Resource Manager Tools для Visual Studio», а эти средства не указываются в данной статье, так как они больше не будут отображаться как отдельная программа в **Программах и компонентах**. В этой статье перечислены только те программы, которые включены в пакет SDK для Azure для .NET версии 2.6.

> **Примечание:** некоторые программы, которые включает пакет SDK, могут также устанавливаться отдельно в других случаях и могут быть необходимы, даже если не требуется полный пакет SDK. Это же может быть верным для программ, которые были установлены в более ранних версиях пакета SDK, но были пропущены в более поздней версии пакета SDK. При удалении программ будьте внимательны и избегайте удаления того, что по-прежнему требуется на компьютере.
> 
> 

## <a id="versions"></a>Версии
Чтобы узнать, какая версия является текущей или скачать более ранние версии, см. страницу [Журнал версий пакета SDK для Azure для .NET](https://azure.microsoft.com/downloads/archive-net-downloads/).

## <a id="resources"></a>Ресурсы
Чтобы скачать пакет Azure для SDK для .NET или клиентскую библиотеку, см. [страницу скачивания Azure](https://azure.microsoft.com/downloads/).

Исходный код пакета Azure SDK для .NET, включая клиентские библиотеки, размещен по адресу [GitHub.com/Azure](https://github.com/azure/).

Справочная документация клиентских библиотек Azure см. в разделе [Справочник по Azure для .NET](https://azure.microsoft.com/documentation/api/).

<!---HONumber=AcomDC_0713_2016-->