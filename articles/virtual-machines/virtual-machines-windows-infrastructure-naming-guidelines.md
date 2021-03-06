---
title: "Azure 인프라 명명 지침 - Windows | Microsoft Docs"
description: "Azure 인프라 서비스에서 이름을 지정하기 위한 핵심 디자인 및 구현 지침에 대해 알아봅니다."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 660765fa-4d42-49cb-a9c6-8c596d26d221
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 87a3d4fc849cee5d9abed1e4c32a9da583ea9516
ms.openlocfilehash: 866265bfa2eb7bcfebd8c73e14d3344a150aaf09


---
# <a name="azure-infrastructure-naming-guidelines"></a>Azure 인프라 명명 지침
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

이 문서에서는 작업 환경 전반에 걸쳐 논리적이며 쉽게 식별할 수 있는 리소스 집합을 작성할 수 있도록 다양한 Azure 리소스 전체에 명명 규칙을 적용하는 방식을 중점적으로 설명합니다.

## <a name="implementation-guidelines-for-naming-conventions"></a>명명 규칙에 대한 구현 지침
의사 결정:

* Azure 리소스에 대한 명명 규칙은 무엇인가?

작업:

* 전체 리소스에서 일관된 방식으로 사용할 수 있게 접사를 정의합니다.
* 저장소 계정 이름을 전역적으로 고유하게 정의합니다.
* 사용할 명명 규칙을 문서화하고 관련된 모든 당사자에게 제공하여 배포 간에 일관성이 유지되도록 합니다.

## <a name="naming-conventions"></a>명명 규칙
Azure에서 항목을 만들기 전에 좋은 명명 규칙이 구현되어 있어야 합니다. 명명 규칙은 모든 리소스에 예측 가능한 이름이 있어야 하며, 이는 해당 리소스의 관리와 연관된 관리 부담을 줄이는 데 도움이 됩니다.

전체 조직 또는 특정 Azure 구독이나 계정에 정의된 명명 규칙의 특정 집합을 따를 수 있습니다. 이는 조직 내의 개인이 Azure 리소스로 작업할 때 암시적 규칙을 설정하기 쉽지만, 팀이 Azure에서 프로젝트를 작업할 때 해당 모델은 잘 확장되지 않습니다.

사전에 명명 규칙 집합을 합의합니다. 규칙 집합에 영향을 미치는 명명 규칙과 관련한 고려 사항이 있습니다.

## <a name="affixes"></a>접사
명명 규칙을 정의하려는 경우에는 접사의 위치를 결정해야 합니다.

* 이름(접두사)의 시작 부분
* 이름(접미사)의 끝 부분

예를 들어 `rg` 접사를 사용할 경우 리소스 그룹에 대해 다음 두 가지 이름을 지정할 수 있습니다.

* Rg-WebApp(접두사)
* WebApp-Rg(접미사)

접사는 특정 리소스를 설명하는 다양한 측면을 참조할 수 있습니다. 다음 표에서 일반적으로 사용하는 일부 예를 보여줍니다.

| 측면 | 예 | 참고 사항 |
|:--- |:--- |:--- |
| Environment |dev, stg, prod |각 환경의 목적 및 이름에 따라 다릅니다. |
| 위치 |usw(West US), use(East US 2) |데이터 센터의 지역 또는 조직의 지역에 따라 다릅니다. |
| Azure 구성 요소, 서비스 또는 제품 |리소스 그룹은 RG, 가상 네트워크는 VNet |리소스가 지원을 제공하는 제품에 따라 다릅니다. |
| 역할 |sql, ora, sp, iis |가상 컴퓨터의 역할에 따라 다릅니다. |
| 인스턴스 |01, 02, 03 등 |둘 이상의 인스턴스가 있는 리소스의 경우. 예를들어, 클라우드 서비스의 분산된 웹 서버를 로드합니다. |

명명 규칙을 설정할 때 각 리소스 유형에 사용할 접사 및 어느 위치(접두사 및 접미사)에 사용할지 분명하게 언급해야 합니다.

## <a name="dates"></a>날짜
리소스 이름에서 만든 날짜를 확인하는 것이 중요한 경우가 많습니다. YYYYMMDD 날짜 형식을 사용하는 것이 좋습니다. 이 형식은 전체 날짜를 기록할 뿐 아니라 날짜에 대해서만 다른 이름이 있는 두 리소스를 알파벳순 및 시간순으로 동시에 정렬합니다.

## <a name="naming-resources"></a>리소스 명명
명명 규칙에서 각 리소스 형식을 정의합니다. 이 명명 규칙은 작성되는 각 리소스에 이름을 할당하는 방법을 정의하는 규칙을 포함해야 합니다. 이러한 규칙은 모든 유형의 리소스에 적용해야 합니다. 예를들어 다음과 같습니다.

* 구독
* 계정
* 저장소 계정
* 가상 네트워크
* 서브넷
* 가용성 집합
* 리소스 그룹
* 가상 컴퓨터
* 끝점
* 네트워크 보안 그룹
* 역할

이름이 참조하는 리소스를 확인하기에 충분한 정보를 제공할 수 있도록 설명이 포함된 이름을 사용해야 합니다.

## <a name="computer-names"></a>컴퓨터 이름
VM(가상 컴퓨터)을 만들 때 Microsoft Azure에서는 최대 15자의 VM 이름을 리소스 이름에 사용하도록 요구합니다. Azure는 VM에 설치된 운영 체제에 동일한 이름을 사용합니다. 하지만 이 이름이 항상 같지는 않을 수 있습니다.

운영 체제를 이미 포함하는 .vhd 이미지 파일에서 VM을 만드는 경우, Azure의 VM 이름이 VM의 운영 체제 컴퓨터 이름과 다를 수 있습니다. 이 경우 VM 관리가 더욱 어려워질 수 있으므로 권장되지 않습니다. Azure VM 리소스 이름을 해당 VM의 운영 체제에 할당하는 컴퓨터 이름과 동일한 이름으로 지정하세요.

Azure VM 이름은 기본 운영 체제 컴퓨터 이름과 동일하게 지정하는 것이 좋습니다.

## <a name="storage-account-names"></a>저장소 계정 이름
저장소 계정에는 해당 이름을 제어하는 특별한 규칙이 있습니다. 소문자와 숫자만 사용할 수 있습니다. 자세한 내용은 [저장소 계정 만들기](../storage/storage-create-storage-account.md#create-a-storage-account) 를 참조하세요. 또한 core.windows.net과 함께 저장소 계정 이름은 전역적으로 유효하고 고유한 DNS 이름이어야 합니다. 예를 들어 저장소 계정이 mystorageaccount인 경우 결과로 생성된 다음 DNS 이름이 고유해야 합니다.

* mystorageaccount.blob.core.windows.net
* mystorageaccount.table.core.windows.net
* mystorageaccount.queue.core.windows.net

## <a name="next-steps"></a>다음 단계
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Jan17_HO4-->


