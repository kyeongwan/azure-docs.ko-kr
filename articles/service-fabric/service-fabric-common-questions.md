---
title: "Microsoft Azure Service Fabric에 대한 일반적인 질문 | Microsoft Docs"
description: "다음은 Service Fabric에 대해 자주 묻는 몇 가지 질문과 그에 대한 답변입니다."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/19/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 102be620e8812cc551aebafe7c8df4e4eac0ae90
ms.openlocfilehash: 2ad3bd7b846693c637fd843383802651a619b128

---


# <a name="commonly-asked-service-fabric-questions"></a>Service Fabric에 대해 자주 묻는 질문

Service Fabric으로 수행할 수 있는 작업 및 사용 방법에 대한 여러 가지 자주 묻는 질문이 있습니다. 이 문서에서는 자주 묻는 질문 및 그에 대한 답변을 설명합니다.

## <a name="cluster-setup-and-management"></a>클러스터 설정 및 관리

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions"></a>여러 Azure 지역에 걸쳐서 클러스터를 만들 수 있나요?

당장은 아니지만, 조사를 진행하는 데 받는 일반적인 요청입니다.

코어 Service Fabric 클러스터링 기술은 Azure 지역에 대해 어떠한 정보도 알지 못하며 서로 네트워크로 연결되어 있기만 하다면 전 세계 어디서나 실행되는 컴퓨터를 결합하는 데 사용할 수 있습니다. 하지만 Azure에서 Service Fabric 클러스터 리소스는 클러스터가 작성된 가상 컴퓨터 크기 집합이므로 지역에 국한됩니다. 또한 멀리 떨어져 있는 컴퓨터 간에 일관성 있는 데이터 복제를 제공하는 것은 본질적인 도전입니다. 사용자는 지역 간 클러스터를 지원하기 전에 성능을 확실히 예측하고 허용할 수 있기를 원합니다.

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Service Fabric 노드에서 OS 업데이트를 자동으로 수신하나요?

당장은 아니지만, 이것 역시 우리가 제공하고자 하는 일반적인 요청입니다.

OS 업데이트 문제는 일반적으로 컴퓨터를 재부팅해야 하며 이로 인해 일시적인 가용성 손실이 발생합니다. Service Fabric에서 해당 서비스에 대한 트래픽을 다른 노드로 리디렉션하므로 이것 자체는 문제가 아닙니다. 하지만 OS 업데이트가 클러스터 간에 조정되지 않는다면 많은 노드가 한 번에 다운될 가능성이 있습니다. 이러한 동시 재부팅은 서비스 또는 적어도 특정 파티션(상태 저장 서비스)에 대한 총체적인 가용성 손실을 유도할 수 있습니다.

향후에는 완전 자동화되고 업데이트 도메인 간에 조정되는 OS 업데이트 정책을 지원하여 재부팅하거나 다른 예기치 않은 오류가 발생해도 가용성이 유지되도록 할 것입니다.

그 동안에는 클러스터 관리자가 수동으로 각 노드의 패치를 안전한 방식으로 시작할 수 있는 [스크립트를 제공](https://blogs.msdn.microsoft.com/azureservicefabric/2017/01/09/os-patching-for-vms-running-service-fabric/)합니다.

### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Service Fabric 클러스터의 최소 크기는 어떻게 되나요? 작으면 안되는 이유는 무엇인가요?

프로덕션 워크로드를 시행하는 Service Fabric 클러스터에 지원되는 최소 크기는&5;개 노드입니다. 개발/테스트 시나리오에 대해&3;개의 노드 클러스터를 지원합니다.

Service Fabric 클러스터는 이름 지정 서비스 및 장애 조치(Failover) 관리자를 포함한 일련의 상태 저장 시스템 서비스를 실행하므로 이러한 최소 크기가 존재합니다. 이러한 클러스터에 어떤 서비스가 배포되었고 현재 호스트되는 위치를 추적하며 강력한 일관성을 따릅니다. 한편 강력한 일관성은 해당 서비스 상태로 특정 업데이트를 위해 *쿼럼*을 획득하는 기능에 의존하는데, 여기서 쿼럼은 지정된 서비스에 대한 복제본(N/2 +1)의 엄격한 다수성을 나타냅니다.

배경 정보로 몇 가지 가능한 클러스터 구성을 살펴보겠습니다.

**한 개 노드**: 이 옵션에서는 어떤 이유로든 단일 노드 손실이 전체 클러스터의 손실을 의미하므로 고가용성을 제공하지 않습니다.

**두 개 노드**: 두 노드(N = 2) 간에 배포된 서비스의 쿼럼은 2(2/2 + 1 = 2)입니다. 단일 복제본이 손실되면 쿼럼을 만들 수 없습니다. 서비스 업그레이드를 수행하려면 복제본을 일시적으로 종료해야 하므로 유용한 구성이 아닙니다.

**세 개 노드**: 세 개 노드(N=3)를 사용하며 쿼럼을 만들기 위한 요건은 계속해서 두 개 노드입니다(3/2 + 1 = 2). 따라서 개별 노드를 손실하고 쿼럼을 계속 유지할 수 있습니다.

업그레이드를 안전하게 수행하고 개별 노드 오류에도 대응 가능하므로(동시에 발생하지만 않는다면) 개발/테스트에 대해 세 개 노드 클러스터 구성이 지원됩니다. 프로덕션 워크로드를 위해 이러한 동시 오류에 대해 복원력이 있어야 하므로&5;개의 노드가 필요합니다.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>비용 절감을 위해 야간/주말에 내 클러스터를 해제할 수 있나요?

일반적으로 그렇지 않습니다. Service Fabric은 임시 로컬 디스크에 상태를 저장하므로 가상 컴퓨터가 다른 호스트로 이동하는 경우 데이터가 이동하지 않습니다. 정상 작동 시에는 새 노드는 다른 노드에 의해 최신 상태가 되므로 문제가 되지 않습니다. 그러나 모든 노드를 중지하고 나중에 다시 시작하는 경우 대부분의 노드가 새 호스트에서 실행되고 시스템을 복구할 수 없을 가능성이 높습니다.

배포하기 전 응용 프로그램 테스트를 위해 클러스트를 만드는 경우 해당 클러스터를 [연속 통합/연속 배포 파이프라인](service-fabric-set-up-continuous-integration.md)의 일부로 동적으로 만드는 것이 좋습니다.

## <a name="application-design"></a>응용 프로그램 설계

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Reliable Collection의 파티션에 대해 데이터를 쿼리하는 가장 좋은 방법은 무엇인가요?

일반적으로 Reliable Collection은 더 나은 성능 및 처리량을 달성하도록 규모 확장하기 위해 [분할](service-fabric-concepts-partitioning.md)됩니다. 즉, 지정된 서비스에 대한 상태가 수십 또는 수백 대의 컴퓨터 간에 분산될 수 있습니다. 전체 데이터 집합에 대한 작업을 수행하기 위한 몇 가지 옵션이 있습니다.

- 다른 서비스의 모든 파티션을 쿼리하는 서비스를 만들어 필요한 데이터를 가져옵니다.
- 다른 서비스의 모든 파티션에서 데이터를 수신할 수 있는 서비스를 만듭니다.
- 각 서비스에서 외부 저장소로 주기적으로 데이터를 푸시합니다. 이 방법은 수행하는 쿼리가 핵심 비즈니스 논리에 포함되지 않는 경우에만 적절합니다.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>내 행위자에 대해 데이터를 쿼리하는 가장 좋은 방법은 무엇인가요?

행위자는 독립적인 상태 및 계산 단위로 설계되므로 런타임에는 행위자 상태에 대한 광범위한 쿼리를 수행하지 않는 것이 좋습니다. 전체 행위자 상태 집합에 대해 쿼리를 해야 하는 경우 다음을 고려합니다.

- 행위자 서비스를 상태 저장 Reliable Services로 바꿔서 많은 네트워크 요청으로 여러 행위자의 모든 데이터를 서비스의 여러 파티션으로 수집하도록 합니다.
- 상태를 외부 저장소로 주기적으로 푸시하도록 행위자를 설계하여 간편하게 쿼리할 수 있도록 합니다. 위에서처럼 이 방법은 수행 중인 쿼리가 런타임 동작에 대해 필요하지 않은 경우에만 필요합니다.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Reliable Collection에 저장할 수 있는 데이터는 얼마나 되나요?

Reliable Services는 일반적으로 분할되므로 저장할 수 있는 양은 클러스터에 있는 컴퓨터 수와 해당 컴퓨터에서 사용할 수 있는 메모리 양에 의해서만 제한됩니다.

예를 들어, 평균 1kb 크기의 개체를 저장하는 100개 파티션과 3개 복제본이 있는 서비스에 Reliable Collection이 있다고 가정합니다. 이제 컴퓨터당 16gb의 메모리가 있는 10개의 컴퓨터 클러스터가 있다고 가정합니다. 간단하고 무난한 작업을 위해 운영 체제 및 시스템 서비스, Service Fabric 런타임 및 사용자 서비스가 이중 6gb를 사용하고 나머지 10gb는 컴퓨터당 사용 가능(클러스터인 경우 100gb)하다고 가정합니다.

각 개체는 세 번(주에서 1번, 복제본에서 2번) 저장되어야 하며 전체 용량으로 작동할 때 컬렉션에서 약 3천 500만 개체에 대해 충분한 메모리를 포함합니다. 하지만 오류 도메인 및 업그레이드 도메인의 동시 손실에 복원력을 갖추는 것이 좋습니다. 동시 손실의 경우 용량의 약 1/3을 나타내므로 약 2천 300만 개체로 감소합니다.

이 계산에서는 다음 사항도 가정합니다.

- 파티션 간 데이터 분포는 대략적으로 균일하거나 부하 메트릭을 클러스터 리소스 관리자에게 보고합니다. 기본적으로 Service Fabric은 복제본 수에 따라 부하 분산을 수행합니다. 위의 예에서는 클러스터의 각 노드에 주 복제본 10개와 보조 복제본 20개를 배치합니다. 파티션 간에 부하가 고르게 분포된 경우에는 원활하게 작동합니다. 부하가 고르게 분포되지 않은 경우 로드를 보고하여 리소스 관리자가 작은 크기의 복제본을 함께 패킹하여 큰 복제본이 개별 노드에서 더 많은 메모리를 사용하도록 합니다.

- 문제가 되는 Reliable Services는 클러스터에서 유일한 저장 상태입니다. 여러 서비스를 클러스터에 배포할 수 있으므로 각각 해당 상태를 실행 및 관리해야 하는 리소스에 유념해야 합니다.

- 클러스터 자체는 증가 또는 감소하지 않습니다. 더 많은 컴퓨터를 추가하는 경우 개별 복제본은 컴퓨터 간에 걸쳐 있을 수 없으므로 컴퓨터 수가 서비스의 파티션 수를 초과할 때까지 Service Fabric은 복제본의 부하를 다시 조정하여 추가 용량을 활용합니다. 이와 반대로 컴퓨터를 제거하여 클러스터 크기를 줄이는 경우 복제본을 보다 조밀하게 패킹하면 전체 용량이 감소합니다.

### <a name="how-much-data-can-i-store-in-an-actor"></a>행위자에 저장할 수 있는 데이터는 얼마나 되나요?

Reliable Services와 마찬가지로 행위자 서비스에 저장할 수 있는 데이터 양은 클러스터에 있는 노드 간에 사용 가능한 총 디스크 공간 및 메모리 양에 의해서만 제한됩니다. 하지만 개별 행위자는 적은 양의 상태 및 관련 비즈니스 논리를 캡슐화하는 데 사용할 경우 가장 효과적입니다. 일반적으로 개별 행위자는 킬로바이트 단위로 측정되는 상태를 포함하게 됩니다.

## <a name="other-questions"></a>기타 질문

### <a name="how-does-service-fabric-relate-to-containers"></a>Service Fabric은 컨테이너와 어떻게 연결되나요?

컨테이너는 서비스와 해당 종속성을 패키지하는 간단한 방법을 제공하여 모든 환경에서 일관성 있게 실행되고 단일 컴퓨터에서 격리된 방식으로 작동할 수 있도록 합니다. Service Fabric은 [컨테이너에 패키지된 서비스](service-fabric-containers-overview.md)를 비롯하여 서비스를 배포 및 관리하는 방법을 제공합니다.

### <a name="are-you-planning-to-open-source-service-fabric"></a>Service Fabric의 오픈 소스 계획이 있나요?

GitHub에서 Reliable Services 및 Reliable Actors 프레임워크에 대한 오픈 소스를 계획 중이며 해당 프로젝트에 대한 참여를 허용합니다. 공지되면 [Service Fabric 블로그](https://blogs.msdn.microsoft.com/azureservicefabric/)에서 자세한 내용을 확인하세요.

현재는 Service Fabric 런타임에 대한 오픈 소스 계획은 없습니다.

## <a name="next-steps"></a>다음 단계

- [핵심 Service Fabric 개념 및 모범 사례에 대해 알아보기](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)



<!--HONumber=Jan17_HO3-->


