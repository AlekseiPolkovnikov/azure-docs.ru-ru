---
title: Web Apps on Azure Stack - Known Issues and Troubleshooting | Microsoft Docs
description: Detailed guidance for deploying Web Apps in Azure Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 8469e3f3-fa2f-41cc-8e3e-0fb771dc3f2f
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: anwestg
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5ea5f9a751e0a539d6c5ac445415815cd6c0612c


---
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps Resource Provider - Known Issues and Troubleshooting
> [!NOTE]
> The following information only applies to Azure Stack TP1 deployments.
> 
> 

If you experience issues while deploying or working with Web Apps on Azure Stack please refer to the guidance below.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure Stack Web Apps Not Available in the portal
![Azure Stack Web Apps Create New Web App][1]

After you have completed the [registration of the Azure Stack Web Apps Provider](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) if you cannot find the Web + Mobile blade then please try the following:

* **Logout of the portal** and **close your browser** and then in a **new browser window login to the portal** and try again.

If you still cannot see the web and mobile blade please try the following:

1. On the **Azure Stack Host Machine** in **Hyper-V Manager** locate the **xRPVM** and **connect** to the VM.
2. Open an **command prompt as Administrator** and run **IISRESET**
3. Return to the **ClientVM** and open a **new browser window**, **login to the portal** and try again.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Deployment Fails when Creating a Web App in a New Resource Group
In the first Preview of Web Apps, **all Web Apps** must be created in the same Resource Group as the Web Apps Resource Provider (**AppService-LOCAL**).  This is a known issue and will be resolved in a future preview.

## <a name="other-issues"></a>Other Issues
If you encounter other issues with Web Apps on Azure Stack please post in [the Azure Stack Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) where members of our team are monitoring posts.

<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525



<!--HONumber=Nov16_HO3-->


