---
title: Добавление расширительных узлов в кластер пакета HPC | Microsoft Docs
description: Сведения о расширении емкости кластера пакета HPC по запросу путем добавления экземпляров рабочих ролей, выполняемых в облачной службе.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: azure-service-management,hpc-pack

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 07/15/2016
ms.author: danlep

---
# <a name="add-on-demand-"burst"-nodes-to-an-hpc-pack-cluster-in-azure"></a>Добавление "расширительных" узлов по запросу в кластер пакета HPC в Azure
В этой статье показано, как добавить расширительные узлы Azure (экземпляры рабочей роли, запущенные в облачной службе) в качестве вычислительных ресурсов в существующий головной узел пакета HPC в Azure. С помощью расширительного узла можно увеличивать и уменьшать масштаб вычислительной мощности кластера HPC в Azure по требованию без обслуживания набора предварительно настроенных виртуальных машин вычислительных узлов.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Расширительные узлы][burst]

Действия, описанные в этой статье, помогут быстро добавить узлы Azure на облачную виртуальную машину с головным узлом пакета HPC для тестовых и экспериментальных развертываний. Процедура по существу аналогична процедуре «ускорения в Azure» и заключается в добавлении вычислительной мощности облака в локальный кластер пакета HPC. Руководство см. в разделе [Установка гибридного вычислительного кластера с помощью пакета Microsoft HPC](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Подробные инструкции и рекомендации для рабочих развертываний см. в разделе [Ускорение в Azure с помощью пакета Microsoft HPC](https://technet.microsoft.com/library/gg481749.aspx).

Рекомендации по использованию размеров экземпляров с интенсивным использованием вычислительных ресурсов для расширительных узлов см. в статье [Виртуальные машины серии A (для ресурсоемких вычислений) и серии H](virtual-machines-windows-a8-a9-a10-a11-specs.md).

## <a name="prerequisites"></a>Предварительные требования
* **Головной узел пакета HPC развернут на виртуальной машине Azure**. Можно использовать изолированную или входящую в состав большого кластера виртуальную машину головного узла. Чтобы создать изолированный головной узел, ознакомьтесь со статьей [Развертывание головного узла пакета HPC на виртуальной машине Azure](virtual-machines-windows-hpcpack-cluster-headnode.md). Чтобы ознакомиться с вариантами автоматизированного развертывания кластеров пакета HPC, прочитайте раздел [Варианты создания кластера высокопроизводительных вычислительных систем (HPC) на основе Windows и управления им в Azure с помощью пакета Microsoft HPC](virtual-machines-windows-hpcpack-cluster-options.md).
  
  > [!TIP]
  > Если для создания кластера в Azure используется [сценарий развертывания IaaS пакета HPC](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md), можно включить расширительные узлы Azure в свое автоматическое развертывание. См. примеры в этой статье.
  > 
  > 
* **Подписка Azure** — для добавления узлов Azure вы можете же выбрать ту же подписку, которая применялась для развертывания виртуальной машины головного узла, либо другую подписку (или подписки).
* **Квота ядер** — может потребоваться увеличить квоту ядер, особенно если вы решите развернуть несколько многоядерных узлов Azure. Чтобы увеличить квоту, бесплатно [отправьте запрос в службу поддержки](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) .

## <a name="step-1:-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Шаг 1. Создание облачной службы и учетной записи хранения для узлов Azure
Используйте классический портал Azure или эквивалентные инструменты для настройки следующих ресурсов, необходимых для развертывания узлов Azure.

* Новая облачная служба Azure
* Новая учетная запись хранения Azure

> [!NOTE]
> Не используйте повторно уже существующую облачную службу в подписке. 
> 
> 

**Рекомендации**

* Настройте отдельную облачную службу для каждого шаблона узла Azure, который планируется создать. Однако можно использовать одну и ту же учетную запись для нескольких шаблонов узла.
* Облачную службу и учетную запись хранения для развертывания рекомендуется размещать в одном регионе Azure.

## <a name="step-2:-configure-an-azure-management-certificate"></a>Шаг 2. Настройка сертификата управления Azure
Для добавления узлов Azure в качестве вычислительных ресурсов необходимо иметь сертификат управления на головном узле и загрузить соответствующий сертификат в подписку Azure, используемую для развертывания.

В этом сценарии вы можете выбрать **сертификат управления HPC Azure по умолчанию** , который пакет HPC автоматически устанавливает и настраивает на головном узле. Этот сертификат удобен для тестирования и экспериментальных развертываний. Чтобы использовать этот сертификат, передайте в подписку файл C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer с виртуальной машины головного узла. Чтобы отправить сертификат с помощью [классического портала Azure](https://manage.windowsazure.com), щелкните **Параметры** > **Сертификаты управления**.

Сведения о дополнительных параметрах настройки сертификата управления см. в разделе [Сценарии настройки сертификата управления Azure для расширения развертывания Azure](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3:-deploy-azure-nodes-to-the-cluster"></a>Шаг 3. Развертывание узлов Azure в кластере
Действия по добавлению и запуску узлов Azure в этом сценарии аналогичны действиям для локального головного узла. Дополнительные сведения см. в следующих разделах статьи [Шаги по развертыванию узлов Azure с помощью пакета Microsoft HPC](https://technet.microsoft.com/library/gg481758.aspx).

* Создание шаблона узла Azure
* Добавление узлов Azure в кластер HPC Windows
* Запуск (подготовка) узлов Azure

После добавления и запуска узлов их можно использовать для выполнения заданий кластера.

Если при развертывании узлов Azure возникли проблемы, ознакомьтесь со статьей [Устранение неполадок развертываний узлов Azure с помощью пакета Microsoft HPC](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Дальнейшие действия
* Если вам требуется возможность автоматического увеличения или уменьшения вычислительных ресурсов Azure в соответствии с текущей рабочей нагрузкой, ознакомьтесь со статьей [Автоматическое изменение размера ресурсов кластера пакета HPC в Azure в соответствии с рабочей нагрузкой кластера](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[расширение]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png



<!--HONumber=Oct16_HO2-->


