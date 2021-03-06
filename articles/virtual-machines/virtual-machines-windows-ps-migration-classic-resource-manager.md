---
title: "PowerShell을 사용하여 Resource Manager로 마이그레이션 | Microsoft Docs"
description: "이 문서에서는 플랫폼 지원 방식의 Azure PowerShell 명령을 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스를 마이그레이션하는 과정을 안내합니다."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: e90036d97451b271451d0ba5845c788ac05d7abf
ms.openlocfilehash: 4253d60a8a12877a3c5dac073bd06d70d020ccdc
ms.lasthandoff: 02/10/2017


---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Azure PowerShell을 사용하여 클래식에서 Azure Resource Manager로 IaaS 리소스 마이그레이션
이러한 단계에서는 Azure PowerShell 명령을 사용하여 클래식 배포 모델의 laaS(Infrastructure as a Service) 리소스를 Azure Resource Manager 배포 모델로 마이그레이션하는 방법을 보여 줍니다. 

원한 경우 [Azure CLI(Azure 명령줄 인터페이스)](virtual-machines-linux-cli-migration-classic-resource-manager.md)를 사용하여 리소스를 마이그레이션할 수도 있습니다.

* 지원되는 마이그레이션 시나리오에 대한 배경 정보를 보려면 [클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션](virtual-machines-windows-migration-classic-resource-manager.md)을 참조하세요. 
* 자세한 지침 및 마이그레이션 연습에 대해서는 [클래식에서 Azure Resource Manager로의 플랫폼 지원 마이그레이션에 대한 기술 정보](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)를 참조하세요.
* [가장 일반적인 마이그레이션 오류 검토](virtual-machines-migration-errors.md)

## <a name="step-1-plan-for-migration"></a>1단계: 마이그레이션 계획
클래식에서 Resource Manager로 IaaS 리소스 마이그레이션을 평가하는 몇 가지 모범 사례가 있습니다.

* [지원/지원되지 않는 기능 및 구성](virtual-machines-windows-migration-classic-resource-manager.md)을 읽어봅니다. 지원되지 않는 구성 또는 기능을 사용하는 가상 컴퓨터가 있다면, 해당 구성/기능 지원이 발표되기를 기다리는 것이 좋습니다. 필요에 따라서는 마이그레이션이 가능하도록 해당 기능을 제거하거나 구성 외부로 이동할 수도 있습니다.
* 현재 인프라 및 응용 프로그램을 배포하는 스크립트를 자동화한 경우 마이그레이션을 위해 해당 스크립트를 사용하여 유사한 테스트 설정을 만들어봅니다. 또는 Azure 포털을 사용하여 샘플 환경을 설정할 수도 있습니다.

> [!IMPORTANT]
> Application Gateway는 현재 클래식에서 Resource Manager로 마이그레이션될 수 없습니다. Application Gateway를 사용하여 클래식 가상 네트워크를 마이그레이션하려면 커밋 작업을 실행하여 네트워크를 이동하기 전에 게이트웨이를 제거합니다(Application Gateway를 삭제하지 않고 준비 단계를 실행할 수 있음). 마이그레이션을 완료한 후 Azure Resource Manager에서 게이트웨이를 다시 연결합니다. 게이트웨이 및 ExpressRoute 회로가 같은 구독에 있는 경우에 ExpressRoute 게이트웨이를 마이그레이션하려면 지원에 문의해야 합니다. 다른 구독에서 ExpressRoute 회로에 연결하는 ExpressRoute 게이트웨이를 마이그레이션할 수 없습니다. 이러한 경우에 ExpressRoute 게이트웨이를 제거하고 가상 네트워크를 마이그레이션한 다음 게이트웨이를 다시 만듭니다.
> 
> 

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>2단계: Azure PowerShell 최신 버전 설치
Azure PowerShell을 설치하는 두 가지 주요 옵션으로 [PowerShell 갤러리](https://www.powershellgallery.com/profiles/azure-sdk/) 또는 [WebPI(웹 플랫폼 설치 관리자)](http://aka.ms/webpi-azps)가 있습니다. WebPI는 매월 업데이트를 수신합니다. PowerShell 갤러리는 지속적으로 업데이트를 수신합니다. 이 문서는 Azure PowerShell 버전 2.1.0을 기반으로 합니다.

설치 지침은 [Azure PowerShell 설치 및 구성 방법](/powershell/azureps-cmdlets-docs)을 참조하세요.

<br>

## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>3단계: 구독 설정 및 마이그레이션에 등록
먼저, PowerShell 프롬프트를 시작합니다. 마이그레이션의 경우 클래식 및 Resource Manager에 대한 환경을 설정해야 합니다.

Resource Manager 모델에 대한 계정으로 로그인합니다.

```powershell
    Login-AzureRmAccount
```

다음 명령으로 사용 가능한 구독을 가져옵니다.

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

현재 세션에 대한 Azure 구독을 설정합니다. 이 예제에서는 기본 구독 이름을 **내 Azure 구독**으로 설정합니다. 예제 구독 이름을 사용자 고유의 이름으로 바꿉니다. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> 등록은 일회성 단계이지만 마이그레이션 전에 한 번은 수행해야 합니다. 등록하지 않으면 다음과 같은 오류 메시지가 표시됩니다. 
> 
> *BadRequest : 구독이 마이그레이션에 대해 등록되지 않았습니다.* 
> 
> 

다음 명령을 사용하여 마이그레이션 리소스 공급자에 등록합니다.

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

등록이 완료될 때까지&5;분 정도 기다려 주세요. 다음 명령을 사용하여 승인 상태를 확인할 수 있습니다.

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

계속 진행하기 전에 RegistrationState가 `Registered` 인지 확인합니다. 

이제 클래식 모델에 대한 계정으로 로그인합니다.

```powershell
    Add-AzureAccount
```

다음 명령으로 사용 가능한 구독을 가져옵니다.

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

현재 세션에 대한 Azure 구독을 설정합니다. 이 예제에서는 기본 구독을 **내 Azure 구독**으로 설정합니다. 예제 구독 이름을 사용자 고유의 이름으로 바꿉니다. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>4단계: 현재 배포의 Azure 지역 또는 VNET에 Azure Resource Manager 가상 컴퓨터 코어가 충분한지 확인
다음 powershell 명령을 사용하여 Azure Resource Manager에 있는 현재 코어 수를 확인할 수 있습니다. 코어 할당량에 대한 자세한 내용은 [제한 및 Azure Resource Manager](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)를 참조하세요. 

이 예제에서는 **미국 서부** 지역의 사용 가능 여부를 확인합니다. 예제 지역 이름을 사용자 고유의 이름으로 바꿉니다. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>5단계: IaaS 리소스를 마이그레이션하는 명령 실행
> [!NOTE]
> 여기에 설명된 모든 작업은 idempotent 방식입니다. 지원되지 않는 기능 또는 구성 오류 이외의 문제가 발생하는 경우 준비, 중단 또는 커밋 작업을 다시 시도하는 것이 좋습니다. 그러면 플랫폼이 해당 작업을 다시 시도합니다.
> 
> 

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>클라우드 서비스에서(가상 네트워크가 아닌) 가상 컴퓨터 마이그레이션
다음 명령을 사용하여 클라우드 서비스 목록을 가져와서 마이그레이션할 클라우드 서비스를 선택합니다. 클라우드 서비스의 VM이 가상 네트워크에 있거나 웹 또는 작업자 역할을 포함하는 경우 명령을 오류 메시지를 반환합니다.

```powershell
    Get-AzureService | ft Servicename
```

클라우드 서비스에 대한 배포 이름을 가져옵니다. 이 예제에서는 서비스 이름이 **내 서비스**입니다. 예제 서비스 이름을 사용자 고유의 서비스 이름으로 바꿉니다. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

마이그레이션을 위해 클라우드 서비스의 가상 컴퓨터를 준비합니다. 두 가지 옵션 중 선택할 수 있습니다.

* **옵션 1. VM을 플랫폼에서 만든 가상 네트워크에 마이그레이션**
  
    먼저 다음 명령을 사용하여 클라우드 서비스를 마이그레이션할 수 있는지 유효성을 검사합니다.
  
    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```
  
    위의 명령은 마이그레이션을 차단하는 경고와 오류를 표시합니다. 유효성 검사가 성공하면 **준비** 단계로 넘어갈 수 있습니다.
  
    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **옵션 2. Resource Manager 배포 모델에서 기존 가상 네트워크로 마이그레이션**
  
    이 예제에서는 리소스 그룹 이름을 **myResourceGroup**, 가상 네트워크 이름을 **myVirtualNetwork**, 서브넷 이름을 **mySubNet**으로 설정합니다. 예제의 이름을 사용자 고유의 리소스 이름으로 바꿉니다.
  
    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```
  
    먼저 다음 명령을 사용하여 클라우드 서비스를 마이그레이션할 수 있는지 유효성을 검사합니다.
  
    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```
  
    위의 명령은 마이그레이션을 차단하는 경고와 오류를 표시합니다. 유효성 검사에서 마이그레이션이 가능한 것으로 확인되면 다음 준비 단계를 진행할 수 있습니다.
  
    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

위의 옵션 중 하나를 사용할 경우 준비 작업에 성공적으로 수행되면 VM의 마이그레이션 상태를 쿼리합니다. `Prepared` 상태인지 확인합니다.

이 예제에서는 VM 이름을 **myVM**으로 설정합니다. 예제 이름을 사용자 고유의 VM 이름으로 바꿉니다.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

PowerShell 또는 Azure 포털을 사용하여 준비된 리소스에 대한 구성을 확인합니다. 마이그레이션할 준비가 되지 않았으며 이전 상태로 되돌아가려면 다음 명령을 사용합니다.

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

준비된 구성이 양호하면 계속 진행하고 다음 명령을 사용하여 리소스를 커밋합니다.

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>가상 네트워크에서 가상 컴퓨터 마이그레이션
가상 네트워크에서 가상 컴퓨터를 마이그레이션하려면 네트워크를 마이그레이션합니다. 가상 컴퓨터는 네트워크와 함께 자동으로 마이그레이션됩니다. 마이그레이션할 가상 네트워크를 선택합니다. 

이 예제에서는 가상 네트워크 이름을 **myVnet**으로 설정합니다. 예제 가상 네트워크 이름을 사용자 고유의 이름으로 바꿉니다. 

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> 가상 네트워크에 웹 또는 작업자 역할이 포함되어 있거나 지원되지 않는 구성을 포함하는 VM이 있으면 유효성 검사 오류 메시지가 표시됩니다.
> 
> 

먼저 다음 명령을 사용하여 가상 네트워크를 마이그레이션할 수 있는지 유효성을 검사합니다.

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

위의 명령은 마이그레이션을 차단하는 경고와 오류를 표시합니다. 유효성 검사에서 마이그레이션이 가능한 것으로 확인되면 다음 준비 단계를 진행할 수 있습니다.

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Azure PowerShell 또는 Azure 포털을 사용하여 준비된 가상 컴퓨터에 대한 구성을 확인합니다. 마이그레이션할 준비가 되지 않았으며 이전 상태로 되돌아가려면 다음 명령을 사용합니다.

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

준비된 구성이 양호하면 계속 진행하고 다음 명령을 사용하여 리소스를 커밋합니다.

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>저장소 계정 마이그레이션
가상 컴퓨터 마이그레이션이 완료되면 저장소 계정을 마이그레이션하는 것이 좋습니다.

다음 명령을 사용하여 마이그레이션을 위한 각 저장소 계정을 준비합니다. 이 예제에서 저장소 계정 이름은 **myStorageAccount**입니다. 예제 이름을 사용자 고유의 저장소 계정 이름으로 바꿉니다. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Azure PowerShell 또는 Azure 포털을 사용하여 준비된 저장소 계정에 대한 구성을 확인합니다. 마이그레이션할 준비가 되지 않았으며 이전 상태로 되돌아가려면 다음 명령을 사용합니다.

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

준비된 구성이 양호하면 계속 진행하고 다음 명령을 사용하여 리소스를 커밋합니다.

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>다음 단계
* 마이그레이션에 대한 자세한 내용은 [클래식에서 Azure Resource Manager로 IaaS 리소스의 플랫폼 지원 마이그레이션](virtual-machines-windows-migration-classic-resource-manager.md)을 참조하세요.
* Azure PowerShell을 사용하여 추가 네트워크 리소스를 Resource Manager로 마이그레이션하려면 [Move-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Move-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx) 및 [Move-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx)과 비슷한 단계를 사용합니다.
* Azure 리소스를 클래식에서 Resource Manager로 마이그레이션하는 데 사용할 수 있는 오픈 소스 스크립트에 대해서는 [Azure Resource Manager로 마이그레이션하기 위한 커뮤니티 도구](virtual-machines-windows-migration-scripts.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


