---
title: "백업에서 가상 컴퓨터 복원 | Microsoft Docs"
description: "복구 지점에서 Azure 가상 컴퓨터를 복원하는 방법 알아보기"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "백업 복원; 복원하는 방법; 복구 지점;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: trinadhk; jimpark;
translationtype: Human Translation
ms.sourcegitcommit: 7de8d98f24ea01012b8fa3a326e1d4ad3f04099a
ms.openlocfilehash: 186929c1193f63ecda6fd4851bb66c8430ab99ab
ms.lasthandoff: 02/17/2017


---
# <a name="restore-virtual-machines-in-azure"></a>Azure의 가상 컴퓨터 복원
> [!div class="op_single_selector"]
> * [Azure 포털에서 VM 복원](backup-azure-arm-restore-vms.md)
> * [클래식 포털에서 VM 복원](backup-azure-restore-vms.md)
> 
> 

다음 단계를 사용하여 Azure 백업 자격 증명에 저장된 백업에서 새 VM에 가상 컴퓨터를 복원합니다.

## <a name="restore-workflow"></a>워크플로 복원
### <a name="step-1-choose-an-item-to-restore"></a>1단계: 복원할 항목 선택
1. **보호된 항목** 탭으로 이동하고 새 VM에 복원할 가상 컴퓨터를 선택합니다.
   
    ![보호된 항목](./media/backup-azure-restore-vms/protected-items.png)
   
    **보호된 항목** 페이지의 **복구 지점** 열에는 가상 컴퓨터의 복구 지점 수가 표시됩니다. **가장 새로운 복구 지점** 열은 가상 컴퓨터를 복원할 수 있는 가장 최근의 백업 시간을 나타냅니다.
2. **복원**을 클릭하여 **항목 복원** 마법사를 엽니다.
   
    ![항목 복원](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>2단계: 복구 지점 선택
1. **복구 지점 선택** 화면에서 최신 복구 지점 또는 이전 시점을 복원할 수 있습니다. 마법사가 열릴 때 선택되는 기본 옵션은 *가장 새로운 복구 지점*입니다.
   
    ![복구 지점 선택](./media/backup-azure-restore-vms/select-recovery-point.png)
2. 이전 시점을 선택하려면 드롭다운에서 **날짜 선택** 옵션을 선택하고 **달력 아이콘**을 클릭하여 달력 컨트롤에서 날짜를 선택합니다. 컨트롤에서 복구 지점이 있는 모든 날짜는 밝은 회색 음영으로 채워져 있으며 사용자가 선택할 수 있습니다.
   
    ![날짜 선택](./media/backup-azure-restore-vms/select-date.png)
   
    달력 컨트롤에서 날짜를 클릭하면 해당 날짜에 사용 가능한 복구 지점이 아래의 복구 지점 표에 나타납니다. **시간** 열은 스냅숏이 만들어진 시간을 나타냅니다. **유형** 열은 복구 지점의 [일관성](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) 을 표시합니다. 테이블 머리글의 괄호 안에는 해당 날짜에 사용할 수 있는 복구 지점의 수가 표시됩니다.
   
    ![복구 지점](./media/backup-azure-restore-vms/recovery-points.png)
3. **복구 지점** 표에서 복구 지점을 선택하고 다음 화살표를 클릭하여 다음 화면으로 이동합니다.

### <a name="step-3-specify-a-destination-location"></a>3단계: 대상 위치 지정
1. **복원 인스턴스 선택** 화면에서 가상 컴퓨터를 복원할 위치에 대한 세부 정보를 지정합니다.
   
   * 가상 컴퓨터 이름 지정: 가상 컴퓨터 이름은 주어진 클라우드 서비스 내에서 고유해야 합니다. 기본 VM 덮어쓰기를 지원하지 않습니다. 
   * VM에 대한 클라우드 서비스 선택: VM을 만들기 위한 필수 항목입니다. 기존 클라우드 서비스를 사용하거나 새 클라우드 서비스 만들기를 선택할 수 있습니다.
     
        선택한 클라우드 서비스 이름은 전역적으로 고유해야 합니다. 일반적으로 클라우드 서비스 이름은 [cloudservice].cloudapp.net 형식으로 공용 URL과 연결됩니다. Azure에서는 이름이 이미 사용된 경우 새 클라우드 서비스를 만들 수 없습니다. 새 클라우드 서비스를 만들려는 경우 가상 컴퓨터와 동일한 이름이 지정됩니다. 이 경우 VM 이름은 연결된 클라우드 서비스에 적용할 수 있을 정도로 고유해야 합니다.
     
        선호도 그룹과 연결되지 않은 클라우드 서비스 및 가상 네트워크만 복원 인스턴스 세부 정보에 표시됩니다. [자세한 정보](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. VM에 대한 저장소 계정 선택: VM을 만들기 위한 필수 항목입니다. Azure 백업 자격 증명 모음과 동일한 지역에 있는 기존 저장소 계정 중에서 선택할 수 있습니다. 영역 중복 또는 프리미엄 저장소 형식의 저장소 계정은 지원되지 않습니다.
   
    지원되는 구성의 저장소 계정이 없는 경우 복원 작업을 시작하기 전에 지원되는 구성의 저장소 계정을 만드세요.
   
    ![가상 네트워크 선택](./media/backup-azure-restore-vms/restore-sa.png)
3. 가상 네트워크 선택: VM을 만들 때 가상 컴퓨터의 가상 네트워크(VNET)를 선택해야 합니다. 복원 UI는 이 구독 내에서 사용할 수 있는 모든 VNET을 표시합니다. 복원된 VM의 VNET을 선택하는 것은 필수가 아닙니다. VNET을 적용하지 않더라도 인터넷을 통해 복원된 가상 컴퓨터에 연결할 수 있습니다.
   
    선택한 클라우드 서비스가 가상 네트워크와 연결 되면 가상 네트워크를 변경할 수 없습니다.
   
    ![가상 네트워크 선택](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. 서브넷 선택: VNET에 서브넷이 있는 경우 기본적으로 첫 번째 서브넷이 선택됩니다. 드롭다운 옵션 중에서 원하는 서브넷을 선택합니다. 서브넷 세부 정보를 보려면 [포털 홈페이지](https://manage.windowsazure.com/)의 네트워크 확장, **가상 네트워크** 로 차례로 이동한 다음 가상 네트워크를 선택하고 구성으로 드릴다운하여 서브넷 세부 정보를 확인합니다.
   
    ![서브넷 선택](./media/backup-azure-restore-vms/select-subnet.png)
5. 마법사에서 **제출** 아이콘을 클릭하여 세부 정보를 제출하고 복원 작업을 만듭니다.

## <a name="track-the-restore-operation"></a>복원 작업 추적
복원 마법사에 모든 정보를 입력하고 제출하면 Azure 백업이 복원 작업을 추적하는 작업을 만듭니다.

![복원 작업 생성 중](./media/backup-azure-restore-vms/create-restore-job.png)

작업 만들기에 성공하면 작업이 만들어졌음을 알리는 알림 메시지가 표시됩니다. **작업 보기** 단추를 클릭하면 **작업** 탭으로 이동하여 더 자세한 정보를 볼 수 있습니다.

![복원 작업이 생성됨](./media/backup-azure-restore-vms/restore-job-created.png)

복원 작업이 완료되면 **작업** 탭에 완료된 것으로 표시됩니다.

![복원 작업 완료](./media/backup-azure-restore-vms/restore-job-complete.png)

가상 컴퓨터를 복원한 후 원래 VM에 있던 확장을 다시 설치하고 Azure 포털에서 가상 컴퓨터에 대한 [끝점을 수정](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) 해야 할 수도 있습니다.

## <a name="post-restore-steps"></a>복원 후 단계
Ubuntu와 같은 클라우드 초기화 기반 Linux 배포를 사용하는 경우 보안상의 이유로 복원 후 암호를 차단합니다. 복원된 VM에서 VMAccess 확장을 사용하여 [암호를 재설정](../virtual-machines/virtual-machines-linux-classic-reset-access.md)하세요. 복원 후 암호를 다시 설정하지 않으려면 이러한 배포에서 SSH 키를 사용하는 것이 좋습니다. 

## <a name="backup-for-restored-vms"></a>복원된 VM에 대한 백업
원래 백업한 VM과 같은 이름으로 같은 클라우드 서비스에 VM을 복원하면, 백업이 VM 사후 복원에 계속 진행됩니다. VM을 다른 클라우드 서비스에 복원하거나, 복원된 VM에 다른 이름을 지정하면, 새 VM으로 간주되어 복원된 VM에 대한 백업을 설정해야 합니다.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Azure 데이터 센터 재해 중 VM 복원
백업 자격 증명 모음을 지리적으로 중복되도록 구성해 놓은 경우, VM이 실행되는 기본 데이터 센터에 재해가 발생하면, Azure 백업을 통해 쌍을 이루는 데이터 센터에 백업한 VM을 복원할 수 있습니다. 이러한 시나리오가 발생하면, 쌍을 이루는 데이터 센터에 존재하는 저장소 계정을 선택해야 하며 나머지 복원 프로세스는 동일합니다. Azure 백업은 쌍을 이루는 지역의 계산 서비스를 사용하여 복원된 가상 컴퓨터를 생성합니다. [Azure 데이터 센터 복원력](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)에 대한 자세한 정보

## <a name="restoring-domain-controller-vms"></a>도메인 컨트롤러 VM 복원
DC(도메인 컨트롤러) 가상 컴퓨터 백업은 Azure 백업을 사용하는 지원되는 시나리오입니다. 그러나 복원 프로세스 중 몇 가지 사항을 주의해야 입니다. 복원 환경이 단일 DC 구성의 도메인 컨트롤러 VM과 다중 DC 구성의 VM에서 크기 다릅니다.

### <a name="single-dc"></a>단일 DC
Azure 포털에서 또는 PowerShell을 사용하여 다른 VM과 마찬가지로 VM을 복원할 수 있습니다.

### <a name="multiple-dcs"></a>다중 DC
다중 DC 환경을 사용하는 경우 도메인 컨트롤러는 자체적인 방법으로 데이터 동기화를 유지합니다. *적절한 예방 조치 없이*오래된 백업 시점이 복원되면 USN 롤백 프로세스가 다중 DC 환경에 손상을 일으킬 수 있습니다. 이러한 VM을 복구하는 올바른 방법은 DSRM 모드로 부팅하는 것입니다.

Azure에 DSRM 모드가 없으면 문제가 됩니다. 따라서 이러한 VM을 복원하기 위해 Azure 포털을 사용할 수 없습니다. 지원되는 유일한 복원 메커니즘은 PowerShell을 사용하는 디스크 기반 복원입니다.

> [!WARNING]
> 다중 DC 환경의 도메인 컨트롤러 VM의 경우 복원에 Azure 포털을 사용하지 마세요. PowerShell 기반 복원만 지원됩니다.
> 
> 

[USN 롤백 문제](https://technet.microsoft.com/library/dd363553) 및 수정을 위한 제안 전략에 대해 알아보세요.

## <a name="restoring-vms-with-special-network-configurations"></a>특수 네트워크 구성을 가진 Vm 복원
Azure 백업은 다음 가상 컴퓨터의 특수 네트워크 구성에 대한 백업을 지원합니다.

* 부하 분산 장치에서의 VM(내부 및 외부)
* 다중의 예약된 IP가 있는 VM
* 다중 NIC가 있는 VM

이러한 구성은 복원 중 다음의 고려 사항을 위임합니다.

> [!TIP]
> VM 사후 복원의 특수 네트워크 구성을 다시 만들려면 PowerShell 기반 복원 흐름을 사용하십시오.
> 
> 

### <a name="restoring-from-the-ui"></a>UI에서 복원:
UI에서 복원하는 동안 **항상 새 클라우드 서비스를 선택**합니다. 포털은 복원 흐름 동안 필수 매개 변수만 사용하므로 UI를 사용하여 복원된 VM이 보유하고 있는 특수 네트워크 구성을 손실할 수 있음에 유의하세요. 즉, 복원 VM은 부하 분산 장치 또는 다중 NIC 또는 다중 예약된 IP의 구성이 없는 기본 VM입니다.

### <a name="restoring-from-powershell"></a>PowerShell에서 복원:
PowerShell은 백업에서 VM 디스크만 복원하고 가상 컴퓨터를 만들지 않을 수 있습니다. 이는 앞에서 언급된 특수 네트워크 구성을 요구하는 가상 컴퓨터를 복원하는 경우에 유용합니다.

가상 컴퓨터 사후 복원 디스크를 완전히 다시 만들려면 다음이 단계를 따르십시오.

1. [Azure 백업 PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)을 사용하여 백업 자격 증명 모음에서 디스크 복원
2. PowerShell cmdlet을 사용하여 부하 분산 장치/다중 NIC/다중의 예약된 IP에 필요한 VM 구성을 만들어 원하는 구성의 VM을 만드는데 사용합니다.
   
   * [내부 부하 분산 장치 ](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * [인터넷 연결 부하 분산 장치](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)에 연결하기 위한 VM을 만듭니다.
   * [다중 NIC](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * [다중의 예약된 IP](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>다음 단계
* [문제 해결](backup-azure-vms-troubleshoot.md#restore)
* [가상 컴퓨터 관리](backup-azure-manage-vms.md)


