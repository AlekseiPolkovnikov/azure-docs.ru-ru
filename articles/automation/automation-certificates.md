---
title: "Ресурсы-контейнеры сертификатов в службе автоматизации Azure | Документация Майкрософт"
description: "Сертификаты можно безопасно сохранить в службе автоматизации Azure, чтобы к ним могли обращаться модули Runbook или конфигурации DSC для прохождения аутентификации при доступе к ресурсам Azure и сторонних производителей.  В этой статье подробно рассматриваются сертификаты и работа с ними при создании текстовых и графических модулей."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1973a3523e121414dfbebf4d00cd2d4fe2005d2f


---
# <a name="certificate-assets-in-azure-automation"></a>Сертификация активов в службе автоматизации Azure
Сертификаты можно безопасно сохранить в службе автоматизации Azure, чтобы к ним могли обращаться модули Runbook или конфигурации DSC с помощью действия **Get-AutomationCertificate** . Это позволяет создавать модули Runbook и конфигурации DSC, которые используют сертификаты для проверки подлинности или добавляют их в ресурсы Azure либо сторонних производителей.

> [!NOTE]
> Безопасные средства в службе автоматизации Azure включают учетные данные, сертификаты, подключения и зашифрованные переменные. Эти ресурсы шифруются и хранятся в службе автоматизации Azure с помощью уникального ключа, который создается для каждой учетной записи службы автоматизации. Ключ шифруется главным сертификатом и хранится в службе автоматизации Azure. Перед сохранением защищенного ресурса ключ учетной записи службы автоматизации дешифруется с помощью главного сертификата и используется для шифрования ресурса.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Командлеты Windows PowerShell
Командлеты, представленные в следующей таблице, используются для создания ресурсов сертификата службы автоматизации и управления ими с помощью Windows PowerShell. Они входят в состав [модуля Azure PowerShell](../powershell-install-configure.md) , доступного в модулях Runbook и конфигурациях DSC службы автоматизации.

| Командлеты | Описание |
|:--- |:--- |
| [Get-AzureAutomationCertificate](http://msdn.microsoft.com/library/dn913765.aspx) |Извлекает сведения о сертификате. Сам сертификат можно извлечь только с помощью действия Get-AutomationCertificate. |
| [New-AzureAutomationCertificate](http://msdn.microsoft.com/library/dn913764.aspx) |Импортирует новый сертификат в службу автоматизации Azure. |
| [Remove-AzureAutomationCertificate](http://msdn.microsoft.com/library/dn913773.aspx) |Удаляет сертификат из службы автоматизации Azure. |
| [Set-AzureAutomationCertificate](http://msdn.microsoft.com/library/dn913763.aspx) |Задает свойства для существующего сертификата, включая отправку файла сертификата и задание пароля для PFX-файла. |

## <a name="activities-to-access-certificates"></a>Действия для доступа к сертификатам
Действия в следующей таблице используются для доступа к сертификатам в модуле Runbook или конфигурации DSC.

| Действия | Описание |
|:--- |:--- |
| Get-AutomationCertificate |Получает сертификат для использования в модуле Runbook или в конфигурации DSC. |

> [!NOTE]
> Не следует использовать переменные в параметре –Name в Get-AutomationCertificate, так как это может усложнить обнаружение зависимостей между модулями Runbook или конфигурациями DSC и ресурсами-контейнерами сертификата во время разработки.
> 
> 

## <a name="creating-a-new-certificate"></a>Создание нового сертификата
При создании нового сертификата в службу автоматизации Azure передается CER- или PFX-файл. Если пометить сертификат как экспортируемый, его можно будет переместить из хранилища сертификатов службы автоматизации Azure. Если он не экспортируемый, то он будет использоваться только для подписи в модуле Runbook или конфигурации DSC.

### <a name="to-create-a-new-certificate-with-the-azure-classic-portal"></a>Создание сертификата на портале классической версии Azure
1. Из учетной записи службы автоматизации щелкните **Активы** в верхней части окна.
2. Нажмите кнопку **Добавить параметр**в нижней части окна.
3. Щелкните **Добавить учетные данные**.
4. В раскрывающемся списке **Тип учетных данных** выберите **Сертификат**.
5. Введите имя сертификата в поле **Имя** и нажмите кнопку со стрелкой вправо.
6. Перейдите к CER- или PFX-файлу.  Если выбран PFX-файл, введите пароль и укажите, разрешен ли экспорт сертификата.
7. Щелкните флажок, чтобы отправить файл сертификата и сохранить новый ресурс сертификата.

### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Создание нового сертификата на портале Azure
1. Из учетной записи службы автоматизации щелкните **Ресурсы**, чтобы открыть колонку **Ресурсы**.
2. Щелкните элемент **Сертификаты**, чтобы открыть колонку **Сертификаты**.
3. Щелкните **Добавить сертификат** в верхней части колонки.
4. Введите имя сертификата в поле **Имя** .
5. Щелкните **Выбрать файл** в разделе **Отправка файла сертификата** и перейдите к CER- или PFX-файлу.  Если выбран PFX-файл, введите пароль и укажите, разрешен ли экспорт сертификата.
6. Щелкните **Создать** для сохранения нового ресурса сертификата.

### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Создание нового сертификата с помощью Windows PowerShell
Команды, приведенные ниже в примере, демонстрируют создание нового сертификата службы автоматизации и помечают его как экспортируемый. Вследствие этого импортируется существующий PFX-файл.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force

    New-AzureAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable

## <a name="using-a-certificate"></a>Использование сертификата
Чтобы использовать сертификат, необходимо применить действие **Get-AutomationCertificate** . Нельзя использовать командлет [Get-AzureAutomationCertificate](http://msdn.microsoft.com/library/dn913765.aspx) , так как он возвращает сведения о ресурсе сертификата, но не сам сертификат.

### <a name="textual-runbook-sample"></a>Пример текстового Runbook
В следующем примере кода показано, как добавить сертификат из Runbook в облачную службу. В этом примере пароль извлекается из зашифрованной переменной службы автоматизации.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AutomationVariable –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Пример графического Runbook
Вы можете добавить **Get-AutomationCertificate** в графический компонент Runbook, щелкнув правой кнопкой мыши сертификат в области "Библиотека" графического редактора и выбрав пункт **Добавить на холст**.

![](media/automation-certificates/certificate-add-canvas.png)

На следующем рисунке показан пример использования сертификата в графическом Runbook.  Этот же пример был показан выше, он добавляет сертификат из текстового Runbook в облачную службу.  

В этом примере для действия **Send-TwilioSMS** задан параметр **UseConnectionObject**, использующий объект подключения для аутентификации в службе.  Здесь необходимо использовать [конвейерную связь](automation-graphical-authoring-intro.md#links-and-workflow) , так как последовательная связь возвращает коллекцию, содержащую один объект, который не ожидается параметром Connection.

![](media/automation-certificates/add-certificate.png)

## <a name="see-also"></a>См. также
* [Использование связей при создании графических модулей](automation-graphical-authoring-intro.md#links-and-workflow) 




<!--HONumber=Nov16_HO3-->


