---
title: "Модель приложений Service Fabric | Документация Майкрософт"
description: "Описание моделирования и описания приложений и служб в Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/29/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 4917f58f9e179b6adca0886e7d278055e5c3d281
ms.openlocfilehash: 4218b8e066d8323444695aca3c970f4f21fadea4


---
# <a name="model-an-application-in-service-fabric"></a>Моделирование приложения в структуре службы
Эта статья содержит обзор модели приложения Azure Service Fabric, а также описание того, как определить приложение и службу через файлы манифеста, выполнить создание пакета приложения и подготовку к развертыванию.

## <a name="understand-the-application-model"></a>Сведения о модели приложения
Приложение представляет собой коллекцию составляющих его служб, которые выполняют определенные функции. Служба выполняет завершенную и отдельную функцию (она может запускаться и работать независимо от других служб) и состоит из кода, конфигурации и данных. Для каждой службы код состоит из исполняемых двоичных файлов, конфигурация состоит из параметров службы, которые могут быть загружены во время выполнения, а данные состоят из произвольных статических данных, которые должны обрабатываться рассматриваемой службой. Каждый компонент в этой иерархической модели приложения может иметь свою версию и обновляться независимо.

![Модель приложения Service Fabric][appmodel-diagram]

Тип приложения представляет собой отнесение приложения к определенной категории и состоит из пакета типов служб. Тип службы представляет собой отнесение службы к определенной категории, которая может обладать различными параметрами и конфигурациями, однако ее основная функция не изменяется. Экземпляры службы представляют собой различные вариации конфигурации служб, принадлежащих к одному типу.  

Классы (или типы) приложений и служб описываются с помощью XML-файлов (манифесты приложений и манифесты служб), которые представляют собой шаблоны, по которым создаются экземпляры приложений из хранилища образов кластера. Определение схемы для файла ServiceManifest.xml и ApplicationManifest.xml устанавливается с пакетом SDK и средствами для Service Fabric по адресу *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Код для различных экземпляров приложений будет выполняться как отдельные процессы, даже если они размещены на одном узле структуры службы. Кроме того, возможно независимое управление жизненным циклом (например, обновление) каждого экземпляра приложения. На следующей диаграмме показано, что типы приложений состоят из типов служб, которые, в свою очередь, состоят из кода, конфигурации и пакетов. Чтобы упростить схему, показаны только пакеты кода, конфигурации или данных для `ServiceType4` , хотя каждый тип службы будет включать некоторые или все из этих типов пакетов.

![Типы служб и типы приложений Service Fabric][cluster-imagestore-apptypes]

Два разных файла манифестов используются для описания приложения и служб: манифест служб и манифест приложений. Эти файлы рассматриваются в последующих разделах.

В кластере может работать один или несколько экземпляров службы определенного типа. Например, в экземплярах служб с отслеживанием состояния или в репликах достигается высокий уровень доступности за счет репликации состояния между репликами, размещенными на разных узлах кластера. Такая репликация обеспечивает избыточность, необходимую для обеспечения доступности службы, даже когда на одном из узлов кластера происходит сбой. При использовании [разделенной службы](service-fabric-concepts-partitioning.md) выполняется дальнейшее разделение ее состояния (а также алгоритмов доступа к этому состоянию) на всех узлах кластера.

На следующей диаграмме отображается отношение между приложениями и экземплярами службы, разделами и репликами.

![Разделы и реплики в службе][cluster-application-instances]

> [!TIP]
> Просмотреть структуру приложений в кластере можно с помощью обозревателя Service Fabric, доступного по адресу http://&lt;адрес_кластера&gt;:19080/Explorer. Дополнительные сведения см. в статье [Визуализация кластера с помощью обозревателя Service Fabric](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Описание службы
Манифест службы декларативно определяет тип и версию службы. Он задает метаданные службы, такие как тип службы, свойства работоспособности, метрики балансировки нагрузки, двоичные файлы службы и файлы конфигурации.  Другими словами, в нем описывается код, конфигурация и пакеты данных, из которых состоит пакет службы, для поддержки одного или нескольких типов служб. Ниже приведен простой пример манифеста служб.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Версия** представляют собой неструктурированные строки, которые не обрабатываются системой. Они используются в целях указания версии каждого компонента для последующего обновления.

В атрибуте **ServiceTypes** указывается, какие типы служб поддерживаются пакетами **CodePackages** в этом манифесте. При создании экземпляра службы в соответствии с одним из этих типов службы все пакеты кода, объявленные в этом манифесте, активируются путем запуска соответствующих точек входа. Запущенные вследствие этого процессы должны зарегистрировать поддерживаемые типы служб во время выполнения. Обратите внимание, что типы служб декларируются на уровне манифеста, а не на уровне пакета кода. Поэтому при наличии нескольких пакетов кода они все активируются при поиске системой любого из задекларированных типов служб.

**SetupEntryPoint** — это привилегированная точка входа, которая запускается с теми же учетными данными, что и структура службы (обычно это локальная учетная запись *LocalSystem* ), перед тем, как будут запущены любые другие точки входа. Исполняемый файл, указанный в точке входа **EntryPoint** , обычно является узлом службы, запускаемым на длительный срок. Наличие отдельной точки входа настройки позволяет избежать необходимости в выполнении узла службы с расширенными правами в течение длительного срока. Исполняемый файл, указанный в точке входа **EntryPoint**, запускается после успешного выхода из точки **SetupEntryPoint**. Возникающий вследствие этого процесс отслеживается и перезапускается (снова начиная с точки входа **SetupEntryPoint**), даже если произошло непредвиденное завершение его работы или сбой.

Атрибут **DataPackage** объявляет папку с именем, указанным в атрибуте **Name**, содержащим произвольные статические данные, которые должны обрабатываться процессом во время выполнения.

**ConfigPackage** объявляет папку с именем, указанным в атрибуте **Name**, которая содержит файл *Settings.xml*. Этот файл содержит разделы заданных пользователем параметров пар "ключ-значение", которые могут считываться процессом во время выполнения. Во время обновления при изменении одного только атрибута **version** для **ConfigPackage** перезапуск процесса не выполняется. Вместо этого при помощи обратного вызова в процесс передается уведомление о том, что параметры конфигурации изменились, поэтому они были перезагружены в динамическом режиме. Ниже приведен пример файла *Settings.xml*.

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [!NOTE]
> Манифест служб может содержать множество пакетов кода, конфигураций и данных. Версия каждого из них устанавливается независимо.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Описание приложения
Манифест приложения декларативно описывает тип приложения и его версию. Он также указывает метаданные композиции службы, такие как стабильные имена, схема секционирования, число экземпляров и коэффициент репликации, политика безопасности и изоляции, ограничения на размещение, переопределения конфигурации и типы входящих в состав служб. Также в нем описываются домены балансировки нагрузки, в которых размещается приложение.

Таким образом, манифест приложения описывает элементы на уровне приложения и ссылается на один или несколько манифестов службы, которые составляют тип приложения. Ниже приведен простой пример манифеста приложения.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Точно так же, как и в манифестах служб, атрибуты версии **Version** представляют собой неструктурированные строки, которые не анализируются системой. Они также используются для маркировки версии каждого из компонентов для последующих обновлений.

**ServiceManifestImport** содержит ссылки на манифесты служб, из которых состоит этот тип приложения. Импортированные манифесты служб определяют, какие типы служб допустимы для применения в приложении этого типа.

**DefaultServices** декларируются экземпляры служб, которые создаются автоматически при создании экземпляра приложения в соответствии с его типом. Службы по умолчанию используются только для удобства и после создания во всех отношениях действуют как и обычные службы. Они обновляются вместе с любыми другими службами в экземпляре приложения, а также могут быть удалены.

> [!NOTE]
> Манифест приложения может содержать несколько импортированных манифестов служб и служб по умолчанию. Каждый импортированный манифест служб может иметь свою версию.
> 
> 

Сведения об использовании разных параметров приложений и служб для отдельных сред см. в статье [Управление параметрами приложения для нескольких сред](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Создание пакета приложения
### <a name="package-layout"></a>Макет пакета
Манифест приложения, манифесты служб и другие необходимые файлы пакетов должны быть организованы в определенный макет для развертывания в кластере структуры службы. Примеры манифестов в этой статье потребовали бы организации в следующую структуру каталогов.

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Папки должны иметь имена, соответствующие значениям атрибута **Name** каждого соответствующего элемента. Например, если манифест служб содержал два пакета кода с именами **MyCodeA** и **MyCodeB**, то две папки с такими же именами будут содержать необходимые двоичные файлы для каждого пакета кода.

### <a name="use-setupentrypoint"></a>Использование SetupEntryPoint
Типичные сценарии использования **SetupEntryPoint** относятся к ситуации, когда необходимо запустить исполняемый файл перед запуском службы или выполнить операцию с повышенными привилегиями. Например:

* Настройка и инициализация переменных среды, необходимых исполняемому файлу службы. Это касается не только исполняемых файлов, написанных с использованием моделей программирования Service Fabric. Например, npm.exe нужны определенные переменные среды, настроенные для развертывания приложения node.js.
* Настройка контроля доступа посредством установки сертификатов безопасности.

### <a name="build-a-package-by-using-visual-studio"></a>Создание пакета с помощью Visual Studio
При использовании Visual Studio 2015 для создания приложения вы можете использовать команду Package, чтобы автоматически создать пакет, который будет соответствовать вышеописанному макету.

Чтобы создать пакет, щелкните правой кнопкой проект приложения в обозревателе решений и выберите команду "Пакет", как показано ниже.

![Создание пакета приложения с помощью Visual Studio][vs-package-command]

После завершения создания пакета в окне **Вывод** отобразятся сведения о расположении пакета. Обратите внимание, что шаг создания пакета выполняется автоматически при развертывании или отладке вашего приложения в Visual Studio.

### <a name="test-the-package"></a>Тестирование пакета
Структуру пакета можно проверить локально средствами PowerShell, используя команду **Test-ServiceFabricApplicationPackage** . Эта команда проверит манифест на наличие ошибок при анализе, а также все ссылки. Обратите внимание, что эта команда позволяет проверить только правильность структуры каталогов и файлов в пакете. При этом проверка содержимого пакетов кода или данных не выполняется, будет проверено только наличие всех необходимых файлов.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Эта ошибка показывает, что файл *MySetup.bat* , на который ссылается манифест служб **SetupEntryPoint** , отсутствует в пакете кода. После добавления нужного файла будет выполнена проверка приложения.

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

После успешного создания и проверки пакета приложение будет готово для развертывания.

## <a name="next-steps"></a>Дальнейшие действия
[Развертывание и удаление приложений с помощью PowerShell][10]

[Управление параметрами приложения для нескольких сред][11]

[Настройка политик безопасности для приложения][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md



<!--HONumber=Nov16_HO3-->


