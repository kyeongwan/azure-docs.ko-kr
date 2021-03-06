---
title: "Azure 네트워킹 인프라 지침 - Linux | Microsoft Docs"
description: "Azure 인프라 서비스에서 가상 네트워킹을 배포하기 위한 핵심 디자인 및 구현 지침에 대해 알아봅니다."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a7ebd5da-3c62-45e8-ad90-6c73a37ebeb1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 84b64fd79da641987d5346d90bb77bde154b58c4
ms.openlocfilehash: a519c101e24a340078adcfde3e5733db71630aea


---
# <a name="azure-networking-infrastructure-guidelines"></a>Azure 네트워킹 인프라 지침
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

이 문서에서는 Azure 내의 가상 네트워킹 및 기존 온-프레미스 환경 간 연결에 필요한 계획 단계를 집중적으로 살펴봅니다.

## <a name="implementation-guidelines-for-virtual-networks"></a>가상 네트워크에 대한 구현 지침
의사 결정:

* IT 작업 또는 인프라를 호스트하는 데 필요한 가상 네트워크는 어떤 유형인가(클라우드 전용 또는 크로스-프레미스)?
* 크로스-프레미스 가상 네트워크의 경우, 현재 서브넷 및 VM을 호스트하고 향후 타당한 확장에 주소 공간이 얼마나 필요한가?
* 각 리소스 그룹에 대해 중앙 집중식 가상 네트워크를 만들려고 하나? 아니면 개별 가상 네트워크를 만들려고 하나?

작업:

* 생성할 가상 네트워크에 대한 주소 공간을 정의합니다.
* 각각에 대한 주소 공간 및 서브넷 집합을 정의합니다.
* 크로스-프레미스 가상 네트워크의 경우, 가상 네트워크의 VM이 도달에 필요한 온-프레미스 위치의 로컬 네트워크 주소 공간 집합을 정의합니다.
* 크로스-프레미스 가상 네트워크를 만들 때 온-프레미스 네트워킹 팀과 협력하여 적절한 라우팅이 구성되도록 합니다.
* 명명 규칙을 사용하여 가상 네트워크를 만듭니다.

## <a name="virtual-networks"></a>가상 네트워크
가상 네트워크는 VM(가상 컴퓨터) 간 통신을 지원하는 데 필요합니다. 물리적 네트워크의 경우와 마찬가지로 서브넷, 사용자 지정 IP 주소, DNS 설정, 보안 필터링 및 부하 분산을 정의할 수 있습니다. [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) 또는 [Express 경로 회로](../expressroute/expressroute-introduction.md)를 사용하여 Azure 가상 네트워크를 온-프레미스 네트워크에 연결할 수 있습니다. [가상 네트워크 및 해당 구성 요소](../virtual-network/virtual-networks-overview.md)에 대해 자세히 읽어보세요.

리소스 그룹을 사용하여 보다 유연하게 가상 네트워킹 구성 요소를 디자인할 수 있습니다. VM은 자체 리소스 그룹 외부의 가상 네트워크에 연결할 수 있습니다. 일반적인 디자인 방법은 일반 팀에서 관리할 수 있는 핵심 네트워킹 인프라를 포함하는 중앙 집중식 리소스 그룹을 만듭니다. VM 및 해당 응용 프로그램을 별도의 리소스 그룹에 배포하는 것입니다. 이 방법을 사용하면 응용 프로그램 소유자가 좀 더 광범위한 가상 네트워킹 리소스 구성에 대한 액세스 권한을 얻지 않고도 VM을 포함하는 리소스 그룹에 액세스할 수 있습니다.

## <a name="site-connectivity"></a>사이트 연결
### <a name="cloud-only-virtual-networks"></a>클라우드 전용 가상 네트워크
온-프레미스 사용자 및 컴퓨터가 Azure 가상 네트워크에서 VM에 지속적인 연결을 필요로 하지 않는 경우, 상당히 직관적으로 가상 네트워크를 디자인할 수 있습니다.

![기본 클라우드 전용 가상 네트워크 다이어그램](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

이 방법은 일반적으로 인터넷 기반 웹 서버와 같은 인터넷 연결 작업을 위한 것입니다. SSH 또는 지점-사이트 간 VPN 연결을 사용하여 이러한 VM을 관리할 수 있습니다.

온-프레미스 네트워크에 연결하지 않기 때문에 Azure 전용 가상 네트워크는 개인 IP 주소 공간의 일부를 사용할 수 있습니다. 주소 공간은 사용 중인 온-프레미스와 동일한 개인 공간일 수 있습니다.

### <a name="cross-premises-virtual-networks"></a>크로스-프레미스 가상 네트워크
온-프레미스 사용자 및 컴퓨터가 Azure 가상 네트워크에서 가상 컴퓨터에 지속적인 연결을 필요로 하지 않는 경우, 크로스-프레미스 가상 네트워크를 만듭니다. 가상 네트워크를 Express 경로 또는 사이트 간 VPN 연결을 사용하여 온-프레미스 네트워크에 연결합니다.

![프레미스 간 가상 네트워크 다이어그램](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

이 구성에서 Azure 가상 네트워크는 기본적으로 온-프레미스 네트워크의 클라우드 기반 확장입니다.

크로스-프레미스 가상 네트워크는 온-프레미스 네트워크에 연결되므로 조직에서 사용하는 고유한 주소 공간 일부를 사용해야 합니다. 여러 다른 회사에 특정 IP 서브넷이 할당되게 되는 동일한 방식에서, 사용자가 네트워크를 확장하면 Azure는 다른 위치가 됩니다.

크로스-프레미스 가상 네트워크에서 온-프레미스 네트워크로 이동하는 패킷을 허용하려면 가상 네트워크에 대한 로컬 네트워크 정의의 일부로 관련 온-프레미스 주소 접두사의 집합을 구성해야 합니다. 가상 네트워크의 주소 공간 및 관련 온-프레미스 위치의 집합에 따라 로컬 네트워크의 많은 주소 접두사가 될 수 있습니다.

클라우드 전용 가상 네트워크를 크로스-프레미스 가상 네트워크로 변환할 수 있지만 가상 네트워크 주소 공간 및 Azure 리소스를 다시 IP해야 할 수 있습니다. 따라서 IP 서브넷을 할당할 때 가상 네트워크를 온-프레미스 네트워크에 연결해야 할지를 신중히 고려해야 합니다.

## <a name="subnets"></a>서브넷
서브넷을 사용하면 관련된 리소스를 로컬로(예: 동일한 응용 프로그램에 관련된 VM에 대한 하나의 서브넷) 또는 물리적으로(예: 리소스 그룹당 하나의 서브넷) 조직할 수 있습니다. 또한 보안 강화를 위해 서브넷 격리 기술을 이용할 수도 있습니다.

크로스-프레미스 가상 네트워크의 경우 온-프레미스 리소스에 사용하는 동일한 규칙으로 서브넷을 설계해야 합니다. **Azure는 항상 각 서브넷에 해당 주소 공간의 처음&3;개의 IP 주소를 사용합니다**. 서브넷에 필요한 주소 수를 확인하려면 지금 필요한 VM 수를 계산하여 시작합니다. 향후 성장을 예측한 후 다음 표를 사용하여 서브넷의 크기를 결정합니다.

| 필요한 VM 수 | 필요한 호스트 비트 수 | 서브넷 크기 |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> 일반적인 온-프레미스 서브넷의 경우, n개의 호스트 비트를 포함하는 서브넷에 대한 호스트 주소의 최대 수는 2<sup>n</sup> – 2입니다. Azure 서브넷의 경우, n개의 호스트 비트를 포함하는 서브넷에 대한 호스트 주소의 최대 수는 2<sup>n</sup> – 5입니다(Azure가 각 서브넷에서 사용하는 주소에 대해 2+3).
> 
> 

서브넷 크기를 너무 작게 선택하면 해당 서브넷에서 VM을 다시 IP하고 다시 배포해야 합니다.

## <a name="network-security-groups"></a>네트워크 보안 그룹
네트워크 보안 그룹을 사용하여 가상 네트워크를 따라 흐르는 트래픽에 필터링 규칙을 적용할 수 있습니다. 가상 네트워킹 환경 보안 유지를 위해 세밀한 필터링 규칙을 작성하여 인바운드 및 아웃바운드 트래픽, 소스 및 대상 IP 범위, 허용 포트 등을 제어할 수 있습니다. 가상 네트워크 내의 서브넷이나 지정된 가상 네트워크 인터페이스에 직접 네트워크 보안 그룹을 적용할 수 있습니다. 가상 네트워크의 트래픽을 필터링하는 일정 수준의 네트워크 보안 그룹을 유지하는 것이 좋습니다. [네트워크 보안 그룹](../virtual-network/virtual-networks-nsg.md)에 대해 자세히 읽어보세요.

## <a name="additional-network-components"></a>네트워크 구성 요소 추가
온-프레미스 물리적 네트워킹 인프라의 경우와 마찬가지로, Azure 가상 네트워킹에는 서브넷 및 IP 주소 지정 이상의 기능이 포함될 수 있습니다. 응용 프로그램 인프라를 디자인할 경우 다음과 같은 일부 추가 구성 요소를 포함하려고 할 수 있습니다.

* [VPN 게이트웨이](../vpn-gateway/vpn-gateway-about-vpngateways.md) - 사이트 간 VPN 연결을 통해 Azure 가상 네트워크를 다른 Azure 가상 네트워크에 연결하거나 온-프레미스 네트워크에 연결합니다. 전용, 보안 연결을 위해 Express 경로 연결을 구현합니다. 지점 및 사이트 간 VPN 연결로 사용자 직접 액세스를 제공할 수도 있습니다.
* [부하 분산 장치](../load-balancer/load-balancer-overview.md) - 필요에 따라 외부 및 내부 트래픽에 대한 부하 분산을 제공합니다.
* [Application Gateway](../application-gateway/application-gateway-introduction.md) - 응용 프로그램 계층의 HTTP 부하 분산을 통해 단순히 Azure 부하 분산 장치를 배포하는 것보다는, 웹 응용 프로그램을 위한 몇 가지 추가적인 혜택을 제공합니다.
* [트래픽 관리자](../traffic-manager/traffic-manager-overview.md) - DNS 기반 트래픽 분산을 통해 최종 사용자를 사용 가능한 가장 가까운 응용 프로그램 끝점으로 보낼 수 있도록 지원하므로 Azure 데이터 센터의 응용 프로그램을 여러 다른 지역에 호스트할 수 있습니다.

## <a name="next-steps"></a>다음 단계
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Jan17_HO4-->


