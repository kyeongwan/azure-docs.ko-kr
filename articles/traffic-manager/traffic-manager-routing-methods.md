---
title: "Traffic Manager - 트래픽 라우팅 방법 | Microsoft Docs"
description: "이 문서는 트래픽 관리자에서 사용하는 다양한 트래픽 라우팅 방법을 이해하는 데 도움이 됩니다."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 993408c9d008a94dfd4004ed6826a2fe6180b20c
ms.openlocfilehash: aa3c77c0bf9db1f875dd992c4eb7a494af58c400

---

# <a name="traffic-manager-traffic-routing-methods"></a>트래픽 관리자 트래픽 라우팅 방법

Azure Traffic Manager는 다양한 서비스 끝점에 네트워크 트래픽을 라우팅하는 방법을 결정하는 세 가지 트래픽 라우팅 메서드를 지원합니다. Traffic Manager는 트래픽 라우팅 메서드를 수신한 각 DNS 쿼리에 적용합니다. 트래픽 라우팅 메서드는 DNS 응답에서 반환된 끝점을 결정합니다.

Traffic Manager에 대한 Azure Resource Manager 지원은 클래식 배포 모델과는 다른 용어를 사용합니다. 다음 테이블은 Resource Manager와 클래식 용어 간의 차이점을 보여줍니다.

| Resource Manager 용어 | 클래식 용어 |
| --- | --- |
| 트래픽 라우팅 메서드 |부하 분산 메서드 |
| 우선 순위 메서드 |장애 조치 메서드 |
| 가중치 적용 메서드 |라운드 로빈 메서드 |
| 성능 메서드 |성능 메서드 |

고객의 의견에 따라 용어를 변경하여 명확성을 높이고 일반적인 오해를 줄였습니다. 기능에는 차이가 없습니다.

트래픽 관리자에서 사용할 수 있는 세 가지 트래픽 라우팅 방법이 있습니다.

* **우선 순위:** 모든 트래픽에 대해 기본 서비스 끝점을 사용하고 기본 또는 백업 끝점을 사용할 수 없을 때 백업을 제공하려면 '우선 순위'를 선택합니다.
* **가중:** 여러 끝점에 균일하게 또는 정의한 가중치에 따라 트래픽을 분산하려면 '가중'을 선택합니다.
* **성능:** 끝점이 서로 다른 지역에 있고 최종 사용자가 가장 짧은 네트워크 대기 시간을 기준으로 "가장 가까운" 끝점을 사용하게 하려는 경우 성능을 선택합니다.

모든 Traffic Manager 프로필에는 끝점 상태 및 자동 끝점 장애 조치(Failover)의 모니터링이 포함됩니다. 자세한 내용은 [트래픽 관리자 끝점 모니터링](traffic-manager-monitoring.md)을 참조하세요. 단일 트래픽 관리자 프로필은&1;가지 트래픽 라우팅 방법만 사용할 수 있습니다. 언제든지 프로필에 대해 다른 트래픽 라우팅 방법을 선택할 수 있습니다. 1분 안에 변경 내용이 적용되며 가동 중지는 발생하지 않습니다. 중첩 트래픽 관리자 프로필을 사용하여 트래픽 라우팅 방법을 결합할 수 있습니다. 중첩을 통해 크고 복잡한 응용 프로그램의 요구 사항을 충족할 수 있는 정교하고 유연한 트래픽 라우팅 구성이 가능합니다. 자세한 내용은 [중첩 트래픽 관리자 프로필](traffic-manager-nested-profiles.md)을 참조하세요.

## <a name="priority-traffic-routing-method"></a>우선 순위 트래픽 라우팅 방법

종종 조직에서는 기본 서비스가 중단될 경우에 하나 이상의 백업 서비스를 배포하여 해당 서비스에 대한 안정성을 제공합니다. '우선 순위' 트래픽 라우팅 메서드를 사용하여 Azure 고객은 이러한 장애 조치(Failover) 패턴을 쉽게 구현할 수 있습니다.

![Azure Traffic Manager '우선 순위' 트래픽 라우팅 메서드][1]

Traffic Manager 프로필은 우선 순위로 정렬된 서비스 끝점 목록을 포함합니다. 기본적으로 Traffic Manager는 모든 트래픽을 기본(가장 높은 우선 순위) 끝점으로 전송합니다. 기본 끝점을 사용할 수 없는 경우 Traffic Manager는 두 번째 끝점에 트래픽을 라우팅합니다. 목록의 기본 끝점 및 보조 끝점을 모두 사용할 수 없는 경우 트래픽이 세 번째 끝점 등으로 전송됩니다. 끝점의 가용성은 구성된 상태(사용 또는 사용 안 함) 및 지속적인 끝점 모니터링을 기반으로 합니다.

### <a name="configuring-endpoints"></a>끝점 구성

Azure Resource Manager에서 각 끝점에 대해 '우선 순위' 속성을 사용하여 끝점 우선 순위를 명시적으로 구성합니다. 이 속성은 1에서 1000 사이의 값입니다. 값이 낮을수록 더 높은 우선 순위를 나타냅니다. 끝점은 우선 순위 값을 공유할 수 없습니다. 이 속성을 설정하는 작업은 선택 사항입니다. 생략하면 끝점 순서에 따른 기본 우선 순위를 사용합니다.

기본 인터페이스를 사용하면 끝점 우선 순위는 암시적으로 구성됩니다. 우선 순위는 프로필 정의에서 끝점이 나열된 순서를 기반으로 합니다.

## <a name="weighted-traffic-routing-method"></a>가중 트래픽 라우팅 방법

'가중' 트래픽 라우팅 메서드를 사용하면 균등하게 트래픽을 분산하거나 미리 정의된 가중치를 사용할 수 있습니다.

![Azure Traffic Manager '가중' 트래픽 라우팅 메서드][2]

가중 트래픽 라우팅 메서드에서는 Traffic Manager 프로필 구성에서 각 끝점에 가중치를 할당합니다. 가중치는 1에서 1000 사이의 정수입니다. 이 매개 변수는 선택 사항입니다. 생략되면 Traffic Manager는 '1'이라는 기본 가중치를 사용합니다.

Traffic Manager는 수신한 각 DNS 쿼리에 대해 사용 가능한 끝점을 임의로 선택합니다. 끝점을 선택하는 확률은 사용 가능한 모든 끝점에 할당된 가중치를 기반으로 합니다. 모든 끝점 결과에서 동일한 가중치를 사용하면 균등하게 트래픽이 분포됩니다. 특정 끝점에 더 높은(또는 더 낮은) 가중치를 적용하면 해당 끝점이 DNS 응답에서 더(또는 덜) 자주 반환됩니다.

가중 메서드를 사용하면 다음과 같은 몇 가지 유용한 시나리오를 사용할 수 있습니다.

* 점진적 응용 프로그램 업그레이드: 새 끝점으로 라우팅할 트래픽의 백분율을 할당하고 시간 경과에 따라 점진적으로 트래픽을 100%까지 늘립니다.
* Azure에 응용 프로그램 마이그레이션: Azure 끝점 및 외부 끝점으로 프로필을 만듭니다. 새 끝점을 선호하도록 끝점의 가중치를 조정합니다.
* 추가 용량을 위한 클라우드 버스트: 트래픽 관리자 프로필을 통해 온-프레미스 배포를 클라우드로 신속하게 확장합니다. 클라우드에 추가 용량이 필요한 경우 끝점을 더 추가하거나 사용하도록 설정하고 각 끝점으로 전송되는 트래픽 양을 지정할 수 있습니다.

새 Azure Portal에서는 가중 트래픽 라우팅의 구성을 지원합니다. 가중치는 클래식 포털에서 구성할 수 없습니다. Resource Manager 및 이전 버전의 Azure PowerShell, CLI 및 REST API를 사용하여 가중치를 구성할 수 있습니다.

클라이언트가 DNS 이름을 확인하는 데 사용하는 클라이언트 및 재귀 DNS 서버에서 DNS 응답을 캐시하는 것을 이해하는 것이 중요합니다. 이 캐싱은 가중 트래픽 분산에 영향을 줄 수 있습니다. 클라이언트 및 재귀 DNS 서버의 수가 큰 경우 트래픽 분산은 예상대로 작동합니다. 그러나 클라이언트 또는 재귀 DNS 서버의 수가 적을 경우 캐싱은 트래픽 배포를 상당히 왜곡시킬 수 있습니다.

일반 사용 사례는 다음과 같습니다.

* 개발 및 테스트 환경
* 응용 프로그램 간 통신
* 일반적인 재귀 DNS 인프라를 공유하는 좁은 사용자 기반을 목표로 하는 응용 프로그램(예: 프록시를 통해 연결하는 회사의 직원)

이러한 DNS 캐싱 효과는 Azure 트래픽 관리자만이 아니라 모든 DNS 기반 트래픽 라우팅 시스템에 공통적으로 적용됩니다. 경우에 따라 DNS 캐시를 명시적으로 지우면 문제가 해결될 수도 있습니다. 대체 트래픽 라우팅 방법이 더 적절한 경우도 있습니다.

## <a name="performance-traffic-routing-method"></a>성능 트래픽 라우팅 방법

전 세계적으로 둘 이상의 위치에 끝점을 배포하면 트래픽을 사용자에게 '가장 가까운' 위치로 라우팅하여 많은 응용 프로그램의 응답성을 향상시킬 수 있습니다. '성능' 트래픽 라우팅 메서드는 이 기능을 제공합니다.

![Azure Traffic Manager '성능' 트래픽 라우팅 메서드][3]

'가장 가까운' 끝점이 반드시 지리적 거리를 기준으로 가장 가깝게 지정되는 것은 아닙니다. 대신, '성능' 트래픽 라우팅 메서드는 네트워크 대기 시간을 측정하여 가장 가까운 끝점을 결정합니다. Traffic Manager에는 IP 주소 범위와 각 Azure 데이터 센터 간의 왕복 시간을 추적하는 인터넷 대기 시간 테이블이 있습니다.

Traffic Manager는 인터넷 대기 시간 테이블에서 들어오는 DNS 요청의 원본 IP 주소를 찾습니다. Traffic Manager는 해당 IP 주소 범위에 대한 가장 낮은 대기 시간을 Azure 데이터 센터에서 사용 가능한 끝점을 선택한 다음 DNS 응답에서 해당 끝점을 반환합니다.

[Traffic Manager 작동 방식](traffic-manager-how-traffic-manager-works.md)에 설명된 대로 Traffic Manager는 클라이언트에서 직접 DNS 쿼리를 수신하지 않습니다. 대신, DNS 쿼리는 클라이언트를 사용하도록 구성한 재귀 DNS 서비스에서 제공됩니다. 따라서 '가장 가까운' 끝점을 결정하는 데 사용되는 IP 주소는 클라이언트의 IP 주소가 아니라 재귀 DNS 서비스의 IP 주소입니다. 실제로 이 IP 주소는 클라이언트에 유용한 프록시입니다.

Traffic Manager는 인터넷 대기 시간 테이블을 정기적으로 업데이트하여 전세계 인터넷 및 새 Azure 지역에서 변경 내용을 고려합니다. 그러나 응용 프로그램 성능은 인터넷을 통한 부하에서 실시간 변형에 따라 달라 집니다. 성능 트래픽 라우팅은 지정된 서비스 끝점에 대한 부하를 모니터링하지 않습니다. 그러나 끝점을 사용할 수 없는 경우 Traffic Manager가 DNS 쿼리 응답에서 이를 반환하지 않습니다.

주의할 사항:

* 프로필이 동일한 Azure 지역에서 여러 끝점을 포함하는 경우 Traffic Manager는 트래픽을 해당 지역에서 사용할 수 있는 끝점에 균등하게 분산합니다. 지역 내에서 다른 트래픽 분산을 원하는 경우 [중첩 Traffic Manager 프로필](traffic-manager-nested-profiles.md)을 사용할 수 있습니다.
* 지정된 Azure 지역에서 활성화된 모든 끝점의 성능이 저하된 경우 Traffic Manager는 그 다음으로 가까운 끝점 대신 사용할 수 있는 다른 모든 끝점에 트래픽을 분산합니다. 이 논리는 그 다음으로 가까운 끝점을 오버로드하지 않고 연속 오류가 발생하지 않도록 방지합니다. 기본 장애 조치 순서를 정의하려는 경우 [중첩된 Traffic Manager 프로필](traffic-manager-nested-profiles.md)을 사용합니다.
* 외부 끝점 또는 중첩 끝점에서 성능 트래픽 라우팅 방법을 사용하는 경우 해당 끝점의 위치를 지정해야 합니다. 배포에 가장 가까운 Azure 지역을 선택합니다. 이러한 위치는 인터넷 대기 시간 테이블에서 지원되는 값입니다.
* 끝점을 선택하는 알고리즘은 결정적입니다. 동일한 클라이언트에서 반복되는 DNS 쿼리는 동일한 끝점으로 전달됩니다. 일반적으로 클라이언트는 이동할 때 다른 재귀 DNS 서버를 사용합니다. 클라이언트는 다른 끝점으로 라우팅될 수 있습니다. 라우팅은 인터넷 대기 시간 테이블의 업데이트에 의해서도 영향을 받을 수 있습니다. 따라서 성능 트래픽 라우팅 메서드는 클라이언트가 항상 동일한 끝점으로 라우팅되는 것을 보장하지 않습니다.
* 인터넷 대기 시간 표가 변경될 경우 일부 클라이언트가 다른 끝점으로 보내진다는 것을 알 수 있습니다. 이 라우팅 변경 내용은 현재 대기 시간 데이터에 따라 더 정확합니다. 인터넷이 계속해서 진화함에 따라, 이러한 업데이트는 성능 트래픽 라우팅의 정확성을 유지하는 데 필수적입니다.

## <a name="next-steps"></a>다음 단계

[Traffic Manager endpoint monitoring](traffic-manager-monitoring.md)

[트래픽 관리자 프로필을 만드는](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



<!--HONumber=Jan17_HO1-->


