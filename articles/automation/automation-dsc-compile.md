---
title: "Компиляция конфигураций в Azure Automation DSC | Документация Майкрософт"
description: "Обзор двух способов, с помощью которых можно компилировать конфигурации требуемого состояния (DSC): на портале Azure или с помощью Windows PowerShell. "
services: automation
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: tysonn
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 01/25/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d64ee97acdc1fb43d8f1d998c826bd6e25935e0a


---
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Компилирование конфигураций в Azure Automation DSC
Вы можете компилировать конфигурации требуемого состояния (DSC) двумя способами: на портале Azure или с помощью Windows PowerShell. Нижеприведенная таблица поможет определить, когда и какой метод использовать с учетом характеристик каждого метода. 

### <a name="azure-preview-portal"></a>Портал предварительной версии Azure
* Простейший способ с интерактивным пользовательским интерфейсом.
* Форма для предоставления значений простых параметров.
* Легко отслеживаемое состояние задания.
* Доступ с проверкой подлинности в Azure.

### <a name="windows-powershell"></a>Windows PowerShell
* Вызов из командной строки с помощью командлетов Windows PowerShell.
* Может быть добавлено в автоматизированное решение, состоящее из нескольких шагов.
* Необходимо предоставить значения простых и сложных параметров.
* Отслеживание состояния задания
* Для поддержки командлетов PowerShell необходим клиент.
* Передача данных ConfigurationData.
* Компилирование конфигураций, использующих учетные данные.

После выбора метода компиляции вы можете выполнять процедуры, описанные ниже, чтобы начать компилирование.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Компилирование конфигурации DSC с помощью портала Azure
1. В своей учетной записи автоматизации щелкните элемент **Конфигурации**.
2. Щелкните конфигурацию, чтобы открыть ее колонку.
3. Нажмите кнопку **Компилировать**.
4. Если конфигурация не имеет параметров, нужно будет подтвердить ее компилирование. Если конфигурация имеет параметры, то отобразится колонка **Compile Configuration** (Компилирование конфигурации), в которой можно указать значения параметров. Дополнительные сведения о параметрах см. в разделе <a href="#basic-parameters">**Базовые параметры**</a> ниже.
5. Откроется колонка **Задание компилирования** , где вы можете отследить статус задания компилирования, а также конфигурации узла (документы конфигурации MOF), которые это задание расположило на опрашивающем сервере Azure Automation DSC.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Компилирование конфигурации DSC с помощью Windows PowerShell
Чтобы начать компилирование с помощью Windows PowerShell, вы можете использовать [`Start-AzureRmAutomationDscCompilationJob`](https://msdn.microsoft.com/library/mt244118.aspx) . В следующем примере кода запускается компилирование конфигурации DSC под именем **SampleConfig**.

    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig" 

`Start-AzureRmAutomationDscCompilationJob` возвращает объект задания компилирования, с помощью которого вы можете отслеживать состояние. После этого вы можете использовать этот объект задания компилирования с помощью [`Get-AzureRmAutomationDscCompilationJob`](https://msdn.microsoft.com/library/mt244120.aspx), чтобы определить статус задания компилирования, или с помощью [`Get-AzureRmAutomationDscCompilationJobOutput`](https://msdn.microsoft.com/library/mt244103.aspx), чтобы просматривать его потоки (выходные данные). В следующем примере кода мы запускаем компилирование конфигурации **SampleConfig** , ждем, пока оно завершится, а затем отображаем его потоки.

    $CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

    while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)           
    {
        $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
        Start-Sleep -Seconds 3
    }

    $CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any 


## <a name="basic-parameters"></a>Базовые параметры
Объявление параметров, в том числе типов и свойств параметров, в конфигурациях DSC выполняется так же, как и в модулях Runbook службы автоматизации Azure. Дополнительные сведения о параметрах модуля Runbook см. в статье [Запуск модуля Runbook в службе автоматизации Azure](automation-starting-a-runbook.md).

Чтобы определить значения свойств в конфигурации узла **ParametersExample.sample**, созданной во время компилирования, в следующем примере используются два параметра — **FeatureName** и **IsPresent**.

    Configuration ParametersExample
    {
        param(
            [Parameter(Mandatory=$true)]

            [string] $FeatureName,

            [Parameter(Mandatory=$true)]
            [boolean] $IsPresent
        )

        $EnsureString = "Present"
        if($IsPresent -eq $false)
        {
            $EnsureString = "Absent"
        }

        Node "sample"
        {
            WindowsFeature ($FeatureName + "Feature")
            {
                Ensure = $EnsureString
                Name = $FeatureName
            }
        }
    }

Вы можете компилировать конфигурации DSC, использующие базовые параметры, на портале Azure Automation DSC или с помощью Azure PowerShell:

### <a name="portal"></a>Microsoft Azure
Чтобы ввести значения параметров на портале, нажмите кнопку **Компилировать**.

![замещающий текст](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell
Для модуля PowerShell нужны параметры в таблице [hashtable](http://technet.microsoft.com/library/hh847780.aspx) , в которой раздел соответствует имени параметра, а значение — значению параметра.

    $Parameters = @{
            "FeatureName" = "Web-Server"
            "IsPresent" = $False
    }


    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters 


Сведения о передаче учетных данных PSCredentials в качестве параметров см. в разделе <a href="#credential-assets">**Активы учетных данных**</a> ниже.

## <a name="configurationdata"></a>ConfigurationData
Параметр **ConfigurationData** позволяет при использовании PowerShell DSC отделить конфигурацию структуры от любой конфигурации среды. Дополнительные сведения о **ConfigurationData** см. в публикации блога [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) (Разница между "что" и "где" в DSC PowerShell).

> [!NOTE]
> Вы можете использовать **ConfigurationData**, когда выполняете компилирование на платформе Azure Automation DSC с помощью Azure PowerShell. Не следует использовать этот параметр на портале Azure.
> 
> 

В приведенном ниже примере конфигурации DSC параметр **ConfigurationData** используется через ключевые слова **$ConfigurationData** и **$AllNodes**. Для этого примера также понадобится [модуль **xWebAdministration**](https://www.powershellgallery.com/packages/xWebAdministration/):

     Configuration ConfigurationDataSample
     {
        Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

        Write-Verbose $ConfigurationData.NonNodeData.SomeMessage 

        Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
        {
            xWebsite Site
            {
                Name = $Node.SiteName
                PhysicalPath = $Node.SiteContents
                Ensure   = "Present"
            }
        }
    }

Вы можете компилировать конфигурацию DSC, показанную выше, с помощью PowerShell. Команда PowerShell ниже добавляет две конфигурации узла в опрашивающий сервер службы автоматизации Azure: **ConfigurationDataSample.MyVM1** и **ConfigurationDataSample.MyVM3**:

    $ConfigData = @{
        AllNodes = @(
            @{
                NodeName = "MyVM1"
                Role = "WebServer"
            },
            @{
                NodeName = "MyVM2"
                Role = "SQLServer"
            },
            @{
                NodeName = "MyVM3"
                Role = "WebServer"

            }

        )

        NonNodeData = @{
            SomeMessage = "I love Azure Automation DSC!"

        }

    } 

    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData


## <a name="assets"></a>Активы
Ссылки на активы одинаковы в конфигурациях и модулях Runbook платформы Azure Automation DSC. Дополнительную информацию см. в следующих статьях:

* [Сертификаты](automation-certificates.md)
* [Подключения](automation-connections.md)
* [Учетные данные](automation-credentials.md)
* [Переменные](automation-variables.md)

### <a name="credential-assets"></a>Активы учетных данных
Хотя конфигурации DSC в службе автоматизации Azure могут ссылаться на ресурсы учетных данных с помощью командлета **Get-AzureRmAutomationCredential**, при необходимости эти ресурсы можно передавать и в качестве параметров. Если конфигурация принимает параметр типа **PSCredential** , в качестве значения этого параметра нужно использовать имя строки ресурса учетных данных (используется в службе автоматизации Azure), а не объект PSCredential. Названный так актив учетных данных, используемых для службы автоматизации Azure, будет в фоновом режиме извлечен и передан в конфигурацию.

Чтобы обеспечить безопасность учетных данных в конфигурациях узла (документы конфигурации MOF), учетные данные нужно зашифровать в MOF-файле конфигурации узла. Служба автоматизации Azure делает больше — она зашифровывает MOF-файл целиком. Но сейчас в модуле DSC PowerShell нужно подтверждать, что учетные данные можно отображать в формате обычного текста во время создания MOF-файла конфигурации узла. Это связано с тем, что модулю DSC PowerShell неизвестно, что, когда MOF-файл будет создан с помощью задачи компилирования, служба автоматизации Azure будет шифровать его целиком.

Вы можете подтвердить в модуле DSC PowerShell, что учетные данные можно отобразить в формате обычного текста в MOF-файлах конфигурации узлов, использующих <a href="#configurationdata">**ConfigurationData**</a>. `PSDscAllowPlainTextPassword = $true` следует передать через **ConfigurationData** для каждого имени блока узла, которое отображается в конфигурации DSC и для которого нужны учетные данные.

В следующем примере показана конфигурация DSC, использующая актив учетных данных автоматизации.

    Configuration CredentialSample
    {
       $Cred = Get-AzureRmAutomationCredential -Name "SomeCredentialAsset"

        Node $AllNodes.NodeName
        { 
            File ExampleFile
            { 
                SourcePath = "\\Server\share\path\file.ext" 
                DestinationPath = "C:\destinationPath" 
                Credential = $Cred 
               }
        }
    }

Вы можете компилировать конфигурацию DSC, показанную выше, с помощью PowerShell. Команда PowerShell ниже добавляет две конфигурации узла в опрашивающий сервер службы автоматизации Azure: **CredentialSample.MyVM1** и **CredentialSample.MyVM2**.

    $ConfigData = @{
        AllNodes = @(
            @{
                NodeName = "*"
                PSDscAllowPlainTextPassword = $True
            },
            @{
                NodeName = "MyVM1"
            },
            @{
                NodeName = "MyVM2"
            }
        )
    }

    Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData



<!--HONumber=Nov16_HO3-->


