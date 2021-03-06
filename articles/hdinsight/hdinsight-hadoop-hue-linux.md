---
title: Использование Hue с Hadoop в кластерах HDInsight на платформе Linux | Microsoft Docs
description: Узнайте, как установить и использовать Hue с кластерами Hadoop в HDInsight на платформе Linux.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun

ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2016
ms.author: nitinme

---
# Установка и использование Hue на кластерах HDInsight Hadoop
Узнайте, как установить Hue на кластер HDInsight на платформе Linux и задействовать туннелирование для направления запросов на Hue.

## Что такое Hue
Hue — это набор веб-приложений, используемых для взаимодействия с кластером Hadoop. Hue позволяет просматривать хранилище, связанное с кластером Hadoop (WASB в случае кластеров HDInsight), выполнять задания Hive, сценарии Pig и т. д. Установив Hue на кластер HDInsight Hadoop, вы получите доступ к следующим компонентам:

* редактор Beeswax Hive,
* Pig,
* диспетчер метахранилища,
* Oozie,
* FileBrowser (связывается с контейнером WASB по умолчанию),
* обозреватель заданий.

> [!WARNING]
> Компоненты, предоставляемые вместе с кластером HDInsight, поддерживаются в полном объеме. Техническая поддержка Майкрософт поможет вам выявить и решить проблемы, связанные с этими компонентами.
> 
> Настраиваемые компоненты получают ограниченную коммерчески оправданную поддержку, способствующую дальнейшей диагностике проблемы. В результате проблема может быть устранена, либо вас могут попросить воспользоваться доступными каналами по технологиям с открытым исходным кодом, чтобы связаться с экспертами в данной области. Можно использовать ряд сайтов сообществ, например [форум MSDN по HDInsight](https://social.msdn.microsoft.com/Forums/azure/ru-RU/home?forum=hdinsight) и [http://stackoverflow.com](http://stackoverflow.com). Кроме того, для проектов Apache есть соответствующие сайты проектов по адресу [http://apache.org](http://apache.org), например для [Hadoop](http://hadoop.apache.org/).
> 
> 

## Установка Hue с помощью действий сценария
Следующее действие сценария может использоваться для установки Hue в кластере HDInsight под управлением Linux. https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

Этот раздел содержит указания по использованию сценария при подготовке кластера с помощью портала Azure.

> [!NOTE]
> Для выполнения действий этого сценария также можно использовать Azure PowerShell, Azure CLI, пакет SDK .NET для HDInsight или шаблоны Azure Resource Manager. Действия сценария также можно применять к уже работающим кластерам. Дополнительные сведения см. в статье [Настройка кластеров HDInsight с помощью действий сценария](hdinsight-hadoop-customize-cluster-linux.md).
> 
> 

1. Начните подготовку кластера с помощью действий, описанных в статье [Подготовка кластеров HDInsight под управлением Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), но не завершайте ее.
   
   > [!NOTE]
   > Для установки Hue в кластерах HDInsight рекомендуется размер головного узла не менее A4 (8 ядер, 14 ГБ памяти).
   > 
   > 
2. В колонке **Необязательная конфигурация** выберите **Действия скрипта** и введите следующие сведения:
   
    ![Предоставление параметров действий скриптов для Hue](./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Предоставление параметров действий скриптов для Hue")
   
   * **ИМЯ**: введите понятное имя для действия сценария.
   * **УНИВЕРСАЛЬНЫЙ КОД РЕСУРСА (URI) СКРИПТА**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.
   * **ЗАГОЛОВОК**: установите флажок.
   * **РАБОЧИЙ**: оставьте это поле пустым.
   * **ZOOKEEPER**: оставьте это поле пустым.
   * **ПАРАМЕТРЫ**: оставьте это поле пустым.
3. В нижней части раздела **Действия сценария** нажмите кнопку **Выбрать**, чтобы сохранить конфигурацию. Наконец, нажмите кнопку **Выбрать** в нижней части колонки **Необязательная конфигурация**, чтобы сохранить дополнительные настройки.
4. Продолжите подготовку кластера, как описано в статье [Подготовка кластеров HDInsight под управлением Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## Использование Hue с кластерами HDInsight
Единственным способом доступа Hue к запущенному кластеру является туннелирование SSH. Туннелирование через SSH позволяет перейти непосредственно к головному узлу кластера, на котором работает Hue. После завершения подготовки кластера выполните следующие действия, чтобы начать использовать Hue в кластере HDInsight на платформе Linux.

1. Изучив статью [Использование туннелирования SSH для доступа к веб-интерфейсу Ambari, ResourceManager, JobHistory, NameNode, Oozie и другим веб-интерфейсам](hdinsight-linux-ambari-ssh-tunnel.md), создайте туннель SSH от своей клиентской системы к кластеру HDInsight, а затем настройте веб-браузер, чтобы он использовал этот туннель SSH в качестве прокси-сервера.
2. После создания SSH-туннеля и настройки браузера для отправки трафика прокси-сервера через этот туннель, найдите имя основного головного узла. Для этого нужно подключиться к кластеру по SSH через порт 22. Например, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` где **USERNAME** — это имя пользователя SSH, а **CLUSTERNAME** — имя кластера.
   
    Дополнительные сведения об использовании SSH см. в следующих документах:
   
   * [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Использование SSH с Hadoop на основе Linux в HDInsight из Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
3. После подключения используйте следующую команду, чтобы получить полное доменное имя основного головного узла.
   
        hostname -f
   
    Вы получите приблизительно такой результат:
   
        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
   
    Это имя основного головного узла, где находится веб-сайт Hue.
4. Откройте в браузере портал Hue по адресу http://HOSTNAME:8888. Замените HOSTNAME на имя, полученное на предыдущем шаге.
   
   > [!NOTE]
   > При первом входе в систему вам будет предложено создать учетную запись для входа на портал Hue. Эти учетные данные будут связаны только с порталом и не будут иметь отношения к учетным данным администратора или SSH, указанным при подготовке кластера.
   > 
   > 
   
    ![Вход на портал Hue](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Ввод учетных данных на портале Hue")

### Выполнение запроса Hive
1. На портале Hue щелкните **Query Editors** (Редакторы запросов), а затем **Hive**, чтобы открыть редактор Hive.
   
    ![Использование Hive](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Использование Hive")
2. На вкладке **Assist** (Помощь) в разделе **Database** (База данных) отобразится элемент **hivesampletable**. Это пример таблицы, входящей в состав всех кластеров Hadoop в HDInsight. В правой области введите запрос и просмотрите выходные данные на нижней вкладке **Results** (Результаты), как показано на приведенном снимке экрана.
   
    ![Выполнение запросов Hive](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Выполнение запросов Hive")
   
    Для визуального представления результатов вы можете использовать вкладку **Chart** (Диаграмма).

### Просмотр хранилища кластера
1. На портале Hue в правом верхнем углу панели меню выберите **File Browser** (Обозреватель файлов).
2. По умолчанию обозреватель открывается в каталоге **/user/myuser**. Щелкните косую черту непосредственно перед каталогом пользователя, чтобы перейти в корневую папку контейнера хранилища Azure, связанного с кластером.
   
    ![Использование обозревателя файлов](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Использование обозревателя файлов")
3. Щелкните правой кнопкой мыши файл или папку, чтобы отобразились доступные операции. Отправить файлы в текущую папку можно с помощью кнопки **Upload** (Отправить) в правом углу. Для создания новых файлов и папок используйте кнопку **New** (Создать).

> [!NOTE]
> Обозреватель файлов Hue может показывать содержимое только контейнера по умолчанию, связанного с кластером HDInsight. У вас не будет доступа к другим учетным записям хранения и контейнерам, которые вы могли связать с кластером. Тем не менее задания Hive будут иметь доступ ко всем дополнительным контейнерам, связанным с кластером. Например, если в редакторе Hive ввести команду `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net`, отобразится содержимое дополнительных контейнеров. В этой команде элемент **newcontainer** не является используемым по умолчанию контейнером, который связан с кластером.
> 
> 

## Важные сведения
1. Скрипт, который использовался для установки Hue, устанавливает его только на основной головной узел кластера.
2. Во время установки некоторые службы Hadoop (HDFS, YARN, MR2, Oozie) перезапускаются для обновления конфигурации. После того как сценарий завершает установку Hue, для запуска прочих служб Hadoop иногда требуется некоторое время. Это может повлиять на первоначальную производительность Hue. После запуска всех служб набор Hue полностью готов к работе.
3. Hue не может работать с заданиями Tez, которые по умолчанию используются в Hive. Если в качестве подсистемы выполнения в Hive вы хотите задействовать MapReduce, измените сценарий, добавив в него следующую команду.
   
     set hive.execution.engine=mr;
4. Кластеры под управлением Linux можно настроить так, чтобы службы выполнялись на основном головном узле, а Resource Manager — на дополнительном. При таком сценарии попытка использования Hue для просмотра сведений о ЗАПУЩЕННЫХ заданиях на кластере может привести к ошибкам (показаны ниже). Тем не менее после завершения заданий вы можете просматривать сведения о них.
   
   ![Ошибка портала Hue](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Ошибка портала Hue")
   
   Это известная проблема. В качестве обходного решения можно изменить Ambari, чтобы активный Resource Manager также запускался на основном головном узле.
5. Hue понимает WebHDFS, а кластеры HDInsight используют службу хранилища Azure с приставкой `wasbs://` в начале пути. Таким образом, пользовательский сценарий, используемый со сценарием действия, устанавливает службу WebWasb, совместимую с WebHDFS и предназначенную для обмена данными с WASB. Поэтому несмотря на то, что в некоторых местах на портале Hue используется надпись HDFS (например, при наведении указателя мыши на **File Browser** (Обозреватель файлов)), ее следует читать как WASB.

## Дальнейшие действия
* [Установка Giraph в кластерах HDInsight](hdinsight-hadoop-giraph-install-linux.md). Используйте настройки кластера для установки Giraph в кластерах HDInsight Hadoop. Giraph позволяет выполнять обработку графов с использованием Hadoop и может использоваться с Azure HDInsight.
* [Установка Solr в кластерах HDInsight](hdinsight-hadoop-solr-install-linux.md). Используйте настройки кластера для установки Solr в кластерах HDInsight Hadoop. Solr позволяет вести расширенный поиск по хранимым данным.
* [Установка R в кластерах HDInsight](hdinsight-hadoop-r-scripts-linux.md). Используйте настройки кластера для установки R в кластерах HDInsight Hadoop. R — это открытый язык программирования и свободная программная среда для статистических вычислений. Он предоставляет сотни встраиваемых статистических функций и собственный язык программирования, который сочетает аспекты функционального и объектно-ориентированного программирования. Этот проект также обеспечивает обширные графические возможности.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

<!---HONumber=AcomDC_0921_2016-->