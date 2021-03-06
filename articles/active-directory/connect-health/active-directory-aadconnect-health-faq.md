---
title: Azure Active Directory Connect Health FAQ - Azure | Microsoft Docs
description: "이 FAQ는 Azure AD Connect Health에 대한 질문에 답변합니다. 이 FAQ는 요금 청구 모델, 기능, 제한 및 지원을 포함한 서비스 사용에 대한 질문을 다룹니다."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 6ad2194a71e0d36ba4a0b9a46ca6dbcd58b619ff
ms.openlocfilehash: 06a8b79f0740e902bb7f9412b449a98b2f0167ea
ms.lasthandoff: 02/16/2017

---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Azure AD Connect Health에 대한 질문과 대답
이 문서에는 Azure AD(Azure Active Directory) Connect Health에 대한 FAQ(질문과 대답)가 포함되어 있습니다. 이 FAQ에서는 청구 모델, 기능, 제한 및 지원을 포함한 서비스 사용 방법에 대해 다룹니다.

## <a name="general-questions"></a>일반적인 질문
**Q: 여러 Azure AD 디렉터리를 관리합니다. Azure Active Directory Premium을 사용하는 디렉터리로 전환하려면 어떻게 해야 하나요?**

오른쪽 위 모서리에 있는 현재 로그인한 **사용자 이름**을 선택하고 적절한 계정을 선택하면 여러 Azure AD 테넌트 간에 전환할 수 있습니다. 계정이 여기에 나열되어 있지 않으면 **로그아웃**을 선택한 다음 Azure Active Directory Premium을 사용하도록 설정된 디렉터리의 글로벌 관리자 자격 증명을 사용하여 로그인합니다.

**Q: Azure AD Connect Health에서 어떤 버전의 ID 역할을 지원하나요?**

다음 테이블에는 역할 및 지원되는 운영 체제 버전이 나열되어 있습니다.

|역할| 운영 체제/버전|
|--|--|
|AD FS(Active Directory Federation Services)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | 버전 1.0.9125 이상|
|AD DS(Active Directory Domain Services)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

서비스에서 제공하는 기능은 역할 및 운영 체제에 따라 다를 수 있습니다. 즉, 운영 체제 버전에 따라 일부 기능이 제공되지 않을 수도 있습니다. 자세한 내용은 기능 설명을 참조하세요.

**Q: 내 인프라를 모니터링하려면 라이선스가 몇 개 필요한가요?**

* 첫 번째 Connect Health 에이전트에는 하나 이상의 Azure AD Premium 라이선스가 필요합니다.
* 등록된 추가 에이전트에는 각각 25개의 추가 Azure AD Premium 라이선스가 필요합니다.
* 에이전트 수는 모든 모니터링된 역할(AD FS, Azure AD Connect 및/또는 AD DS)에 등록된 에이전트의 총 수와 같습니다.

라이선스 정보는 [Azure AD 가격 책정 페이지](https://aka.ms/aadpricing)에서 찾을 수 있습니다.

예제:

| 등록된 에이전트 | 필요한 라이선스 | 모니터링 구성 예제 |
| ------ | --------------- | --- |
| 1 | 1 | Azure AD Connect 서버&1;개 |
| 2 | 26| Azure AD Connect 서버 1개 및 도메인 컨트롤러 1개 |
| 3 | 51 | AD FS(Active Directory Federation Services) 서버 1개, AD FS 프록시 1개, 도메인 컨트롤러 1개 |
| 4 | 76 | AD FS 서버 1개, AD FS 프록시 1개, 도메인 컨트롤러 2개 |
| 5 | 101 | Azure AD Connect 서버 1개, AD FS 서버 1개, AD FS 프록시 1개, 도메인 컨트롤러 2개 |


## <a name="installation-questions"></a>설치 관련 질문

**Q: 개별 서버에 Azure AD Connect Health Agent를 설치하면 어떤 영향이 있나요?**

Microsoft Azure AD Connect Health 에이전트, AD FS, 웹 응용 프로그램 프록시 서버, Azure AD Connect(sycn) 서버, 도메인 컨트롤러를 설치해도 CPU, 메모리 사용량, 네트워크 대역폭 및 저장소에는 최소한의 영향만 미칩니다.

다음 숫자는 근사값입니다.

* CPU 사용량:&1;-5% 증가
* 메모리 사용량: 전체 시스템 메모리의 최대 10%

> [!NOTE]
> 에이전트가 Azure와 통신할 수 없는 경우 에이전트는 정의된 최대 한도까지 데이터를 로컬로 저장합니다. Agent는 "최소 최근 서비스" 단위로 "캐시된" 데이터를 덮어씁니다.
>
>

* Azure AD Connect Health 에이전트의 로컬 버퍼 저장소: ~20MB.
* AD FS 서버의 경우 모든 감사 데이터를 덮어쓰기 전에 처리하려면 Azure AD Connect Health 에이전트의 AD FS 감사 채널에 1,024MB(1GB)의 디스크 공간을 프로비전하는 것이 좋습니다.

**Q: Azure AD Connect Health Agent를 설치하는 동안 내 서버를 재부팅해야 하나요?**

아니요. 에이전트를 설치하는 데 서버를 재부팅할 필요는 없습니다. 그러나 일부 필수 구성 요소 설치 단계에서 서버를 재부팅해야 할 수 있습니다.

예를 들어 Windows Server 2008 R2에 .NET 4.5 Framework를 설치하는 경우 서버를 재부팅해야 합니다.

**Q: Azure AD Connect Health는 통과 HTTP 프록시를 통해 작동하나요?**

예. 진행 중인 작업의 경우 HTTP 프록시를 사용하여 아웃바운드 http 요청을 전달하도록 Health Agent를 구성할 수 있습니다.
[Health 에이전트에 대한 HTTP 프록시 구성](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)을 자세히 읽어보세요.

에이전트를 등록하는 동안 프록시를 구성해야 하는 경우 Internet Explorer 프록시 설정을 미리 수정해야 할 수도 있습니다.

1. Internet Explorer > **설정** > **인터넷 옵션** > **연결** > **LAN 설정**으로 이동합니다.
2. **사용자 LAN의 프록시 서버 사용**을 선택합니다.
3. HTTP 및 HTTPS/보안의 프록시 포트가 다른 경우 **고급**을 선택합니다.

**Q: Azure AD Connect Health는 HTTP 프록시에 연결할 때 기본 인증을 지원하나요?**

아니요. 기본 인증에 필요한 임의 사용자 이름/암호를 지정하는 메커니즘은 현재 지원되지 않습니다.

**Q: Azure AD Connect Health Agent가 작동하도록 하기 위해 열어야 하는 방화벽 포트는 무엇인가요?**

방화벽 포트 및 기타 연결 요구 사항 목록은 [요구 사항 섹션](active-directory-aadconnect-health-agent-install.md#requirements)을 참조하세요.

**Q: Azure AD Connect Health 포털에 같은 이름을 가진 두 대의 서버가 표시되는 이유는 무엇인가요?**

서버에서 에이전트를 제거해도 해당 서버가 Azure AD Connect Health 포털에서 자동으로 제거되지 않습니다. 서버에서 에이전트를 수동으로 제거하거나 서버 자체를 제거한 경우 Azure AD Connect Health 포털에서 서버 항목을 수동으로 삭제해야 합니다.

서버를 이미지로 다시 설치하거나 같은 세부 정보(예: 컴퓨터 이름)를 사용하여 새 서버를 만들 수 있습니다. Azure AD Connect Health 포털에서 이미 등록된 서버를 제거하지 않고 새 서버에 에이전트를 설치하면 이름이 같은 두 개의 항목이 표시될 수 있습니다.

이 경우 이전 서버에 소속된 항목을 수동으로 삭제해야 합니다. 이 서버에 대한 데이터는 오래된 정보입니다.

## <a name="health-agent-registration-and-data-freshness"></a>Health Agent 등록 및 데이터 새로 고침

**Q: Health Agent 등록 오류가 발생하는 일반적인 원인은 무엇이며 어떻게 해결해야 하나요?**

Health Agent는 다음과 같은 원인으로 등록에 실패할 수 있습니다.

* 방화벽이 트래픽을 차단하고 있어서 에이전트가 필수 끝점과 통신할 수 없습니다. 특히 웹 응용 프로그램 프록시 서버에서 자주 발생하는 문제입니다. 필수 끝점 및 포트에 아웃바운드 통신을 허용해야 합니다. 자세한 내용은 [요구 사항 섹션](active-directory-aadconnect-health-agent-install.md#requirements)을 참조하세요.
* 아웃바운드 통신은 네트워크 계층에서 SSL 검사를 받습니다. 이로 인해 에이전트에서 사용하는 인증서가 검사 서버/엔터티로 교체되고, 에이전트 등록을 완료하는 단계가 실패합니다.
* 사용자는 에이전트의 등록을 수행하기 위한 액세스 권한이 없습니다. 전역 관리자는 기본적으로 액세스 권한이 있습니다. [역할 기반 액세스 제어](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)를 사용하여 다른 사용자에 대한 액세스를 위임할 수 있습니다.

**Q: "Health Service 데이터가 최신 상태가 아닙니다."라는 경고가 표시됩니다. 이 문제를 어떻게 해결하나요?**

Azure AD Connect Health는&2;시간 동안 서버에서 데이터 지점을 수신하지 않으면 이 경고를 생성합니다. 이 경고가 발생하는 여러 이유가 있을 수 있습니다.

* 방화벽이 트래픽을 차단하고 있어서 에이전트가 필수 끝점과 통신할 수 없습니다. 특히 웹 응용 프로그램 프록시 서버에서 자주 발생하는 문제입니다. 필수 끝점 및 포트에 아웃바운드 통신을 허용해야 합니다. 자세한 내용은 [요구 사항 섹션](active-directory-aadconnect-health-agent-install.md#requirements)을 참조하세요.
* 아웃바운드 통신은 네트워크 계층에서 SSL 검사를 받습니다. 이로 인해 에이전트에서 사용하는 인증서가 검사 서버/엔터티로 교체되고, Azure AD Connect Health 서비스로 데이터를 업로드하는 프로세스가 실패합니다.
* 에이전트에 기본 제공된 연결 명령을 사용할 수 있습니다. [자세히 알아보기](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* 또한 에이전트는 인증되지 않은 HTTP 프록시를 통해 아웃바운드 연결을 지원합니다. [자세히 알아보기](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>작업 관련 질문
**Q: 웹 응용 프로그램 프록시 서버에 대한 감사를 사용하도록 설정해야 하나요?**

아니요, 웹 응용 프로그램 프록시 서버에서 감사를 사용할 필요가 없습니다.

**Q: Azure AD Connect Health 경고는 어떻게 해결하나요?**

Azure AD Connect Health 경고는 성공 조건에서 해결됩니다. Azure AD Connect Health 에이전트가 정기적으로 성공 조건을 검색하여 서비스에 보고합니다. 일부 경고의 경우 시간을 기준으로 경고가 제거됩니다. 즉, 동일한 오류 조건이 경고 생성으로부터 72시간 내에 관찰되지 않으면 경고가 자동으로 해결됩니다.

## <a name="migration-questions"></a>마이그레이션 관련 질문

이 섹션은 Azure AD Connect Health 데이터의 예정된 마이그레이션에 대한 알림을 받은 고객에게만 적용됩니다.

**Q: 마이그레이션이 발생한 후에 내 에이전트를 다시 등록하거나 내 알림 설정을 다시 구성해야 하나요?**

아니요. 에이전트 등록 정보 및 알림 설정은 마이그레이션의 일부로 이동됩니다.

**Q: 마이그레이션하고 얼마 후에 포털에 데이터가 표시되기 시작하나요?**

마이그레이션하고 한 시간 후에 포털에 데이터가 표시되기 시작합니다.

**Q: 기존 활성 경고는 어떻게 되나요?**

모든 해당 경고는 마이그레이션하고&1;시간 이내에 다시 활성화됩니다.

## <a name="related-links"></a>관련 링크
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health 에이전트 설치](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health 작업](active-directory-aadconnect-health-operations.md)
* [AD FS와 함께 Azure AD Connect Health 사용](active-directory-aadconnect-health-adfs.md)
* [동기화에 대한 Azure AD Connect Health 사용](active-directory-aadconnect-health-sync.md)
* [AD DS와 함께 Azure AD Connect Health 사용](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health 버전 내역](active-directory-aadconnect-health-version-history.md)

