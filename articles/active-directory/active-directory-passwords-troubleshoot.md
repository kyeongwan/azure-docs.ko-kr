---
title: "문제 해결: Azure AD 암호 관리 | Microsoft Docs"
description: "Azure AD 암호 관리를 위한 일반적인 문제 해결 단계는 재설정, 변경, 쓰기 저장, 등록 및 도움이 필요한 경우 포함할 정보를 포함합니다."
services: active-directory
documentationcenter: 
author: asteen
manager: femila
editor: curtand
ms.assetid: 18f3dcf7-9314-4a2b-8fed-54b00c0026dd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 8a4e26b7ccf4da27b58a6d0bcfe98fc2b5533df8
ms.openlocfilehash: 3515091cf71ecb595d8c08902ff13549a9ddd2f4


---
# <a name="how-to-troubleshoot-password-management"></a>암호 관리 문제 해결 방법
> [!IMPORTANT]
> **로그인하는 데 문제가 있나요?** 그렇다면 [암호를 변경하고 재설정하는 방법은 다음과 같습니다](active-directory-passwords-update-your-own-password.md).
> 
> 

암호 관리 문제가 발생하는 경우 도움을 드립니다. 직면할 문제 대부분은 아래에서 배포 문제를 해결하려고 읽을 수 있는 몇 가지 간단한 문제 해결 단계로 해결할 수 있습니다.

* [**도움이 필요한 경우 포함할 정보**](#information-to-include-when-you-need-help)
* [**Azure 관리 포털에서 암호 관리 구성에 문제가 있음**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Azure 관리 포털에서 암호 관리 보고서에 문제가 있음**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**암호 재설정 등록 포털에서 문제가 있음**](#troubleshoot-the-password-reset-registration-portal)
* [**암호 재설정 포털에서 문제가 있음**](#troubleshoot-the-password-reset-portal)
* [**암호 쓰기 저장에 문제가 있음**](#troubleshoot-password-writeback)
  * [암호 쓰기 저장 이벤트 로그 오류 코드](#password-writeback-event-log-error-codes)
  * [암호 쓰기 저장 연결에 문제가 있음](#troubleshoot-password-writeback-connectivity)

아래의 문제 해결 단계를 시도해도 여전히 문제가 발생하면 [Azure AD 포럼](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) 에 질문을 게시하거나 지원에 문의하면 빠른 시일 내에 문제를 살펴보겠습니다.

## <a name="information-to-include-when-you-need-help"></a>도움이 필요한 경우 포함할 정보
아래 지침 관련 문제를 해결할 수 없는 경우 지원 엔지니어에 문의할 수 있습니다. 이렇게 문의할 때 다음 정보를 포함 하는 것이 좋습니다.

* **오류에 대한 일반적인 설명** – 사용자가 본 정확한 오류 메시지 정의  오류 메시지가 있는 경우 알아낸 예기치 않은 동작을 자세히 설명합니다.
* **페이지** – 오류(URL 포함)가 나타나는 경우 페이지 정의
* **날짜/시간/표준 시간대** – 오류(표준 시간대 포함)가 나타나는 경우 정확한 날짜 및 시간 정의
* **지원 코드** – 오류가 나타나는 경우 생성된 지원 코드 정의(오류를 찾으려면 오류를 재현한 후 화면 아래에서 코드 지원 링크를 클릭하고 지원 엔지니어에게 결과인 GUID를 보냄.)
  
  * 아래에서 지원 코드 없이 페이지에서 F12 키를 누르고 SID 및 CID를 검색한 후 지원 엔지니어에게 이러한 두 개의 결과를 보냅니다.
    
    ![][001]
* **사용자 ID** – 오류가 나타난 사용자의 ID 정의(예: user@contoso.com)?)
* **사용자에 대한 정보** – 페더레이션된 사용자, 동기화된 암호 해시, 클라우드 정의  AAD Premium 또는 AAD 기본 라이선스가 할당된 사용자입니까?
* **응용 프로그램 이벤트 로그** – 암호 쓰기 저장을 사용 중이고 온-프레미스 인프라에서 오류가 발생한 경우 Azure AD Connect 서버에서 응용 프로그램 이벤트 로그의 복사본을 압축하고 요청과 함께 보내주십시오.

이 정보를 포함하면 최대한 빨리 문제를 해결하는데 도움이 됩니다.

## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Azure 관리 포털에서 암호 재설정 구성 문제 해결
암호 재설정을 구성할 때 오류가 발생하면 다음 문제해결 단계를 수행하여 해결할 수 있습니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>오류 사례</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>발생한 오류 정의</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>해결 방법</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Azure 관리 포털의 <strong>구성</strong> 탭 아래에서 <strong>사용자 암호 재설정 정책</strong> 섹션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>Azure 관리 포털의 <strong>구성</strong> 탭에서 <strong>사용자 암호 재설정 정책</strong> 섹션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>이 작업을 수행하는 관리자에게 할당된 AAD Premium 또는 AAD 기본 라이선스가 없는 경우 발생할 수 있습니다. </p>
              <p>이 문제를 수정하려면 <strong>라이선스</strong> 탭으로 이동하여 해당 관리자 계정에 AAD Premium 또는 AAD 기본 라이선스를 할당하고 다시 시도하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>설명서에서 설명된 <strong>사용자 암호 재설정 정책</strong> 아래에서 구성 옵션이 보이지 않습니다.</p>
            </td>
            <td>
              <p><strong>사용자 암호 재설정 정책</strong> 섹션이 보이지만 그 아래에 나타나는 유일한 플래그는 <strong>암호 재설정 활성화한 사용자</strong> 플래그입니다.</p>
            </td>
            <td>
              <p><strong>암호 재설정 활성화한 사용자</strong> 플래그를 <strong>예</strong>로 전환하는 경우 UI의 나머지가 나타납니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>특정 구성 옵션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>예를 들어 <strong>사용자 암호 재설정 정책</strong> 섹션(또는 동일한 문제에 대한 다른 예제)을 스크롤하면 <strong>사용자가 연락처 데이터를 확인하기 전에 일수</strong> 옵션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>필요할 때까지 UI의 요소는 대부분 숨겨져 있습니다. 참조하려면 페이지의 모든 옵션을 사용하도록 설정하십시오.</p>
              <p>모든 컨트롤을 사용할 수 있는 방법에 대한 자세한 정보는 <a href="active-directory-passwords-customize.md#password-management-behavior">암호 관리 동작</a>을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p><strong>암호를 온-프레미스에 쓰기 저장</strong> 구성 옵션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>Azure 관리 포털의 <strong>구성</strong> 탭 아래에 <strong>암호를 온-프레미스에 쓰기 저장</strong> 옵션이 보이지 않습니다.</p>
            </td>
            <td>
              <p>Azure AD 연결을 다운로드하고 암호 쓰기 저장을 구성하는 경우 이 옵션을 표시합니다. 이 작업을 하는 경우 해당 옵션이 표시되고 클라우드에서 쓰기 저장을 사용하거나 사용하지 않도록 설정할 수 있습니다.</p>
              <p>이 작업을 수행하는 방법에 대한 자세한 내용은 <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Azure AD Connect에서 비밀번호 쓰기 저장 활성화</a>를 참조하세요.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Azure 관리 포털에서 암호 관리 보고서에 문제 해결
암호 관리 보고서를 사용할 때 오류가 발생하면 다음 문제해결 단계를 수행하여 해결할 수 있습니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>오류 사례</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>발생한 오류 정의</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>해결 방법</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 관리 보고서가 보이지 않습니다.</p>
            </td>
            <td>
              <p><strong>보고서</strong> 탭의 <strong>활동 로그</strong> 아래에서 <strong>암호 재설정 활동</strong> 및 <strong>암호 재설정 등록 활동</strong> 보고서가 보이지 않습니다.</p>
            </td>
            <td>
              <p>이 작업을 수행하는 관리자에게 할당된 AAD Premium 또는 AAD 기본 라이선스가 없는 경우 발생할 수 있습니다. </p>
              <p>이 문제를 수정하려면 <strong>라이선스</strong> 탭으로 이동하여 해당 관리자 계정에 AAD Premium 또는 AAD 기본 라이선스를 할당하고 다시 시도하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자 등록이 여러 번 표시됩니다</p>
            </td>
            <td>
              <p>사용자가 대체 전자 메일, 휴대폰 및 보안 질문을 등록할 때 각각 대신 별도 줄로 표시됩니다.</p>
            </td>
            <td>
              <p>현재 사용자가 등록할 때 등록 페이지에 있는 모든 것들이 등록될 것을 가정할 수 없습니다. 결과적으로, 별도 이벤트로 등록된 데이터의 각 개별 부분을 로그합니다.</p>
              <p>이 데이터를 집계하려는 경우 보고서를 다운로드하고 엑셀에서 피벗 테이블로 데이터를 열어서 더 유연하게 관리할 수 있습니다.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>암호 재설정 등록 포털에서 문제 해결
암호 재설정을 위해 사용자를 등록할 때 오류가 발생하면 다음 문제해결 단계를 수행하여 해결할 수 있습니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>오류 사례</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>발생한 오류 정의</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>해결 방법</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 재설정을 위해 디렉터리를 사용할 수 없습니다.</p>
            </td>
            <td>
              <p>관리자는 이 기능을 사용하도록 활성화하지 않습니다.</p>
            </td>
            <td>
              <p><strong>암호 재설정을 활성화한 사용자</strong> 플래그를 <strong>예</strong>로 전환하고 Azure 관리 포털 디렉터리 구성 탭에서 <strong>저장</strong>을 누릅니다. 이 작업을 수행하는 관리자에게 할당된 Azure AD Premium 또는 기본 라이선스가 있어야 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>AAD Premium 또는 AAD 기본 라이선스가 할당되지 않은 사용자입니까?</p>
            </td>
            <td>
              <p>관리자는 이 기능을 사용하도록 활성화하지 않습니다.</p>
            </td>
            <td>
              <p>Azure 관리 포털의 <strong>라이선스</strong> 아래에서 사용자에게 Azure AD Premium 또는 Azure AD 기본 라이선스를 할당합니다. 이 작업을 수행하는 관리자에게 할당된 Azure AD Premium 또는 기본 라이선스가 있어야 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>오류 처리 요청</p>
            </td>
            <td>
              <p>다음과 같이 보고하는 오류가 나타납니다. </p>
              <p>

              </p>
              <p>오류 처리 요청 </p>
              <p>암호 재설정하려는 동안.</p>
            </td>
            <td>
              <p>많은 문제로 인해 발생할 수 있지만 일반적으로 확인할 수 없는 서비스 중단 또는 구성 문제로 인해 이 오류가 발생합니다. </p>
              <p>이 오류가 나타나고 비즈니스에 영향을 주는 경우 지원에 문의하시면 되도록 빨리 도움을 드립니다. 신속한 문제해결을 돕기 위해 지원 엔지니어에게 제공해야 할 내용을 보려면 <a href="#information-to-include-when-you-need-help">도움이 필요한 경우 포함할 정보</a>를 참조하세요.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>암호 재설정 포털에서 문제 해결
사용자용 암호를 재설정할 때 오류가 발생하면 다음 문제해결 단계를 수행하여 해결할 수 있습니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>오류 사례</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>발생한 오류 정의</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>해결 방법</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 재설정을 위해 디렉터리를 사용할 수 없습니다.</p>
            </td>
            <td>
              <p>암호 재설정을 위해 계정을 사용할 수 없습니다.</p>
              <p>죄송하지만 관리자가 계정을 이 서비스와 함께 사용하도록 설정하지 않습니다. </p>
              <p>

              </p>
              <p>원하는 경우 사용자의 조직에서 관리자에게 연락하여 사용자용 암호를 재설정할 수 있습니다.</p>
            </td>
            <td>
              <p><strong>암호 재설정을 활성화한 사용자</strong> 플래그를 <strong>예</strong>로 전환하고 Azure 관리 포털 디렉터리 구성 탭에서 <strong>저장</strong>을 누릅니다. 이 작업을 수행하는 관리자에게 할당된 Azure AD Premium 또는 기본 라이선스가 있어야 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>AAD Premium 또는 AAD 기본 라이선스가 할당되지 않은 사용자입니까?</p>
            </td>
            <td>
              <p>관리자 외 계정 암호를 자동으로 재설정할 수는 없지만 조직의 관리자에게 연락할 수 있습니다.</p>
            </td>
            <td>
              <p>Azure 관리 포털의 <strong>라이선스</strong> 아래에서 사용자에게 Azure AD Premium 또는 Azure AD 기본 라이선스를 할당합니다. 이 작업을 수행하는 관리자에게 할당된 Azure AD Premium 또는 기본 라이선스가 있어야 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 재설정에 디렉터리를 사용할 수 있지만 사용자가 인증 정보를 누락했거나 형식이 잘못 되었습니다.</p>
            </td>
            <td>
              <p>암호 재설정을 위해 계정을 사용할 수 없습니다.</p>
              <p>죄송하지만 관리자가 계정을 이 서비스와 함께 사용하도록 설정하지 않습니다. </p>
              <p>

              </p>
              <p>원하는 경우 사용자의 조직에서 관리자에게 연락하여 사용자용 암호를 재설정할 수 있습니다.</p>
            </td>
            <td>
              <p>진행하기 전에 사용자가 디렉터리에서 파일에 연락처 데이터를 제대로 구성했는지 확인합니다. 디렉터리에 인증 정보를 구성하는 방법에 대한 내용은 <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">암호 재설정으로 사용되는 데이터 정의</a>를 참조하여 사용자에게 이 오류가 표시되지 않도록 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 재설정에 디렉터리를 사용할 수 있지만 두 개의 검증 단계가 필요하도록 정책이 설정된 경우 사용자만이 연락처 데이터에서 한 개의 파일을 보유합니다. </p>
            </td>
            <td>
              <p>암호 재설정을 위해 계정을 사용할 수 없습니다.</p>
              <p>죄송하지만 관리자가 계정을 이 서비스와 함께 사용하도록 설정하지 않습니다. </p>
              <p>

              </p>
              <p>원하는 경우 사용자의 조직에서 관리자에게 연락하여 사용자용 암호를 재설정할 수 있습니다.</p>
            </td>
            <td>
              <p>계속하기 전에 해당 사용자에게 둘 이상의 제대로 구성된 연락 방법(예: 휴대폰 및 사무실 전화)이 있는지를 확인합니다. 디렉터리에 인증 정보를 구성하는 방법에 대한 내용은 <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">암호 재설정으로 사용되는 데이터 정의</a>를 참조하여 사용자에게 이 오류가 표시되지 않도록 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>암호 재설정에 디렉터리를 사용할 수 있고 사용자가 올바르게 구성되어 있지만 사용자와 연결할 수 없습니다. </p>
            </td>
            <td>
              <p>아이쿠!  사용자를 연결하는 동안 예기치 않은 오류가 발생했습니다.</p>
            </td>
            <td>
              <p>이것은 임시 서비스 오류 또는 우리가 제대로 검색할 수 없는 잘못 구성한 연락처 데이터 결과입니다. 사용자가 10초 동안 기다리면 다시 시도하고 "관리자에게 문의" 링크가 표시됩니다. 다시 시도를 클릭하면 호출을 다시 발송하는 반면 "관리자에게 문의"를 클릭하면 해당 사용자 계정을 위해 암호 재설정이 수행되도록 요청하는 전자 메일 양식을 사용자, 암호 또는 전역 관리자에게(이 우선순위 대로) 전송할 것입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자는 암호 재설정 SMS 또는 전화 통화를 수신하지 않습니다</p>
            </td>
            <td>
              <p>사용자는 "내게 텍스트" 또는 "내게 전화 걸기"를 클릭하고 아무것도 수신하지 않습니다.</p>
            </td>
            <td>
              <p>이것은 디렉터리에서 형식이 잘못된 전화번호의 결과일 수 있습니다. 전화번호가 "+ ccc xxxyyyzzzzXeeee" 형식인지 확인합니다. 암호 재설정에 사용할 전화번호 형식에 대한 자세한 내용은 <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">암호 재설정에서 사용하는 데이터 정의</a>를 참조하세요.</p>
              <p>문제의 사용자에게 라우팅할 확장이 필요한 경우 디렉터리에서 하나를 지정하더라도 해당 암호 재설정은 확장을 지원하지 않습니다.(호출이 발송되기 전에 제거됨)  확장을 사용하지 않고 숫자를 사용하거나 사용자의 PBX의 전화번호에 확장을 통합하려 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자는 암호 재설정 전자 메일을 수신하지 못합니다.</p>
            </td>
            <td>
              <p>사용자는 "내게 전화 걸기"를 클릭하고 아무것도 수신하지 않습니다.</p>
            </td>
            <td>
              <p>이 문제에 대한 가장 일반적인 원인은 스팸 필터에 의해 메시지가 거부된다는 점입니다. 전자 메일에 대한 스팸, 정크, 또는 삭제된 항목 폴더를 확인하십시오.</p>
              <p>또한 메시지를 위해 올바른 전자 메일인지를 확인하십시오...많은 사람이 매우 유사한 전자 메일 주소가 있고 메시지를 받은 편지함을 잘못 확인합니다. 이러한 두 옵션이 모두 효과가 없으면 디렉터리에서 전자 메일 주소 형식이 잘못된 것일 수도 있습니다. 전자 메일 주소가 올바른지 확인하고 다시 시도하십시오. 암호 재설정에 사용할 전자 메일 주소 형식에 대한 자세한 내용은 <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">암호 재설정에서 사용하는 데이터 정의</a>를 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>필자는 암호 재설정 정책을 설정했지만 관리자 계정이 암호 재설정을 사용하는 경우 해당 정책이 적용되지 않습니다.</p>
            </td>
            <td>
              <p><strong>구성</strong> 탭의 <strong>사용자 암호 재설정 정책</strong> 섹션에서 어떤 정책을 설정했는지에 관계 없이 암호를 재설정한 관리자 계정은 전자 메일 및 휴대폰에서 암호 재설정을 위해 활성화된 동일한 옵션을 나타냅니다.</p>
            </td>
            <td>
              <p><strong>구성</strong> 탭의 <strong>사용자 암호 재설정 정책</strong> 섹션 아래에서 구성된 옵션만이 사용자의 조직에서 최종 사용자에게 적용됩니다.</p>
              <p>가장 높은 수준의 보안을 보장하기 위해 Microsoft가 관리자 암호 재설정 정책을 관리하고 제어합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자는 하루에 너무 많이 암호를 재설정하려 할 수 없습니다.</p>
            </td>
            <td>
              <p>다음과 같이 보고하는 오류가 나타납니다. </p>
              <p>

              </p>
              <p>다른 옵션을 사용하십시오.</p>
              <p>마지막 1시간에 계정 인증을 너무 많이 시도했습니다. 보안상의 이유로 다시 시도하기 전에 24시간을 대기해야 합니다. </p>
              <p>원하는 경우 사용자의 조직에서 관리자에게 연락하여 사용자용 암호를 재설정할 수 있습니다.</p>
            </td>
            <td>
              <p>짧은 시간 동안에 너무 여러 번 자신의 암호를 다시 설정하려는 사용자를 차단하는 자동 제한 메커니즘을 구현합니다. 다음과 같은 때 이것이 일어납니다.</p>
              <ol class="ordered">
                <li>
사용자는 한 시간 동안 5회에 걸쳐 전화번호의 유효성을 검사하려 합니다.<br\><br\></li>
                <li>
사용자는 한 시간 동안 5회에 걸쳐 보안 질문 게이트를 사용하려 합니다.<br\><br\></li>
                <li>
사용자가 한 시간 동안 5회에 걸쳐 동일한 사용자 계정에 대한 암호를 재설정하려 합니다.<br\><br\></li>
              </ol>
              <p>이를 해결하려면 사용자가 마지막 시도 후 24시간 동안 대기하도록 하고 사용자가 자신의 암호를 재설정합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자가 자신의 전화번호의 유효성을 검사하는 경우 오류가 보입니다.</p>
            </td>
            <td>
              <p>인증 방법을 사용하여 전화를 확인하려고 시도할 때, 사용자에게 내용의 오류를 보여줍니다.</p>
              <p>

              </p>
              <p>잘못된 전화번호를 지정합니다.</p>
            </td>
            <td>
              <p>이 오류는 입력한 휴대폰 번호와 파일에서 휴대폰 번호가 일치하지 않을 때 발생합니다.</p>
              <p>사용자가 암호 재설정을 위해 전화 기반 방법을 사용하려고 할 때 영역 및 국가 코드를 포함하여 전체 전화 번호를 입력하는지 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>오류 처리 요청</p>
            </td>
            <td>
              <p>다음과 같이 보고하는 오류가 나타납니다. </p>
              <p>

              </p>
              <p>오류 처리 요청 </p>
              <p>암호 재설정하려는 동안.</p>
            </td>
            <td>
              <p>많은 문제로 인해 발생할 수 있지만 일반적으로 확인할 수 없는 서비스 중단 또는 구성 문제로 인해 이 오류가 발생합니다. </p>
              <p>이 오류가 나타나고 비즈니스에 영향을 주는 경우 지원에 문의하시면 되도록 빨리 도움을 드립니다. 신속한 문제해결을 돕기 위해 지원 엔지니어에게 제공해야 할 내용을 보려면 <a href="#information-to-include-when-you-need-help">도움이 필요한 경우 포함할 정보</a>를 참조하세요.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>암호 쓰기 저장 문제 해결
암호 쓰기 저장을 활성화, 비활성화 또는 사용할 때 오류가 발생하면 다음 문제해결 단계를 수행하여 해결할 수 있습니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>오류 사례</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>발생한 오류 정의</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>해결 방법</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>일반 온보딩 및 시작 오류</p>
            </td>
            <td>
              <p>Azure AD 연결 컴퓨터의 응용 프로그램 이벤트 로그에서 오류 6800로 인해 온-프레미스에서 암호 재설정 서비스가 시작되지 않습니다.</p>
              <p>

              </p>
              <p>온보딩 후에 페더레이션되거나 암호 해시 동기화된 사용자는 자신의 암호를 재설정할 수 없습니다.</p>
            </td>
            <td>
              <p>암호 쓰기 저장을 사용하는 경우 동기화 엔진은 저장 라이브러리를 호출하여 클라우드 온보딩 서비스와 대화함으로써 구성(온보딩)을 수행합니다. 암호 쓰기 저장에 대한 WCF 끝점을 온보딩 또는 시작하는 동안 발생한 오류는 Azure AD 연결 컴퓨터의 이벤트 로그의 이벤트 로그에서 오류를 발생시킵니다.</p>
              <p>ADSync 서비스를 다시 시작하는 동안 쓰기 저장을 구성한 경우 WCF 끝점을 시작합니다. 그러나 끝점을 시작하는 데 실패하면 단순히 6800 이벤트를 로그하고 동기화 서비스를 시작합니다. 이 이벤트의 현재 상태는 암호 쓰기 저장 끝점이 시작되지 않았음을 의미합니다. 이 이벤트(6800)에 대한 이벤트 로그 세부 정보는 PasswordResetService 요소가 생성한 이벤트 로그 항목과 함께 끝점을 시작하지 못한 이유를 나타냅니다. 암호 쓰기 저장이 여전히 작동하지 않는 경우 이러한 이벤트 로그 오류를 검토하고 Azure AD 연결을 다시 시작합니다. 문제가 지속되면 암호 쓰기 저장을 비활성화하고 다시 활성화시킵니다.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>사용자가 암호 재설정 또는 비밀번호 쓰기 저장을 사용하도록 설정된 계정을 잠금 해제하려는 경우 작업이 실패합니다. 또한 잠금 해제 작업이 발생한 후 “동기화 엔진이 오류 hr=800700CE 반환, 메시지=파일 이름 또는 확장명이 너무 깁니다”를 포함하는 Azure AD Connect 이벤트 로그에서 이벤트가 표시됩니다.
                            </p>
            </td>
            <td>
              <p>Azure AD Connect 또는 DirSync의 이전 버전에서 업그레이드한 경우 발생할 수 있습니다. 이전 버전의 Azure AD Connect 업그레이드는 Azure AD 관리 에이전트 계정에 대해 254자의 암호를 설정합니다(최신 버전은 127자 길이 암호 설정) . 이러한 긴 암호는 AD 커넥터 가져오기 및 내보내기 작업에 사용할 수 있지만 잠금 해제 작업에서 지원되지 않습니다.
                            </p>
            </td>
            <td>
              <p>Azure AD Connect에 대한 [Active Directory 계정을 찾고](connect/active-directory-aadconnect-accounts-permissions.md#active-directory-account) 127자 이하를 포함하도록 암호를 다시 설정합니다. 그런 다음 시작 메뉴에서 **동기화 서비스**를 엽니다. **커넥터**로 이동하고 **Active Directory Connector**를 찾습니다. 이를 선택하고 **속성**을 클릭합니다. **자격 증명** 페이지로 이동하고 새 암호를 입력합니다. **확인**을 선택하여 페이지를 닫습니다.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Azure AD Connect을 설치하는 동안 쓰기 저장을 구성하는 오류입니다.</p>
            </td>
            <td>
              <p>Azure AD Connect 설치 프로세스의 마지막 단계에서 암호 쓰기 저장을 구성할 수 없다는 오류가 표시됩니다.</p>
              <p>

              </p>
              <p>Azure AD 연결 응용 프로그램 이벤트 로그는 "인증 토큰 가져오기 오류"라는 오류 32009 텍스트를 포함합니다.</p>
            </td>
            <td>
              <p>이 오류는 다음과 같은 두 경우에 발생합니다.</p>
              <ul>
                <li class="unordered">
Azure AD Connect 설치 프로세스를 시작할 때 지정된 전역 관리자 계정에 잘못된 암호를 지정했습니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Azure AD Connect 설치 프로세스를 시작할 때 지정된 전역 관리자 계정에 페더레이션된 사용자를 사용하려 했습니다.<br\><br\></li>
              </ul>
              <p>이 오류를 해결하려면 Azure AD Connect 설치 프로세스를 시작할 때 지정한 전역 관리자로 페더레이션된 계정을 사용하지 않고 지정된 암호가 올바른지 확인하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Azure AD Connect을 설치하는 동안 쓰기 저장을 구성하는 오류입니다.</p>
            </td>
            <td>
              <p>Azure AD Connect 컴퓨터 이벤트 로그는 PasswordResetService가 발생시킨 오류 32002를 포함합니다.</p>
              <p>

              </p>
              <p>오류에서 "ServiceBus에 연결 오류, 토큰 공급자가 보안 토큰을 제공할 수 없습니다... "라는 메시지가 보입니다.</p>
              <p>

              </p>
            </td>
            <td>
              <p>이 오류의 근본 원인은 온-프레미스 환경에서 실행하는 암호 재설정 서비스가 클라우드의 서비스 버스 끝점에 연결할 수 없는 데 있습니다. 이 오류는 일반적으로 특정 포트 또는 웹 주소에 대한 아웃 바운드 연결을 차단하는 방화벽 규칙에 의해 발생됩니다.</p>
              <p>

              </p>
              <p>방화벽이 다음에서 아웃 바운드 연결을 허용하는지 확인합니다.</p>
              <ul>
                <li class="unordered">
TCP 443(HTTPS)를 통한 모든 트래픽<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
다음에 아웃바운드 연결<br\><br\></li>
              </ul>
              <p>

              </p>
              <p>이러한 규칙을 업데이트한 후 Azure AD Connect 컴퓨터를 재부팅하고 암호 쓰기 저장은 작업을 다시 시작해야 합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>연결할 수 없는 암호 쓰기 저장 끝점 온-프레미스</p>
            </td>
            <td>
              <p>시간이 지난 후에 페더레이션되거나 암호 해시 동기화된 사용자는 자신의 암호를 재설정할 수 없습니다.</p>
            </td>
            <td>
              <p>일부 드문 경우에서 Azure AD Connect를 다시 시작할 때 암호 쓰기 저장 서비스를 다시 시작하지 못할 수 있습니다. 이러한 경우 먼저 암호 쓰기 저장이 활성화된 온-프레미스에 나타나는지 확인하십시오. 이렇게 하려면 Azure AD Connect 마법사 또는 Powershell를 사용해야 합니다.(위의 HowTos 섹션 참조) 기능을 사용 가능하도록 표시되면 UI 또는 PowerShell을 통해 이 기능을 활성화 또는 비활성화하도록 시도합니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">비밀번호 쓰기 저장을 활성화/비활성화하는 방법</a>에서 "2단계: 디렉터리 동기화 컴퓨터에서 비밀번호 쓰기 저장 사용 &amp; 방화벽 규칙을 구성"을 참조하세요.</p>
              <p>

              </p>
              <p>작동하지 않으면 Azure AD Connect를 완전히 제거하고 다시 설치하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>권한 오류</p>
            </td>
            <td>
              <p>페더레이션된 페더레이션되거나 암호 해시 동기화되어 자신의 암호를 재설정하려는 사용자는 서비스 문제가 나타나는 암호를 제출한 후 오류가 나타납니다.</p>
              <p>

              </p>
              <p>이 외에도 암호 재설정 작업 중 관리 에이전트와 관련한 오류가 온-프레미스 이벤트 로그에서 거부된 액세스를 표시할 수 있습니다</p>
            </td>
            <td>
              <p>이러한 오류를 이벤트 로그에 표시한 경우 AD MA 계정(구성 시 마법사에서 지정됨)이 암호 쓰기 저장에 필요한 권한이 있는지 확인합니다.</p>
              <p>

              </p>
              <p>이 권한이 부여되면 DC에서 sdprop 백그라운드 작업을 통해 권한을 주는 데 최대 1시간이 걸릴 수 있습니다.  </p>
              <p>암호 재설정을 작동하기 위해 해당 암호를 재설정하는 사용자 개체의 보안 설명자에 사용 권한을 스탬프해야 합니다. 사용자 개체에 이 사용 권한이 표시될 때까지 암호 재설정에 액세스가 거부되고 실패할 것입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Azure AD Connect 구성 마법사에서 암호 쓰기 저장을 구성하는 동안 오류가 발생했습니다. </p>
            </td>
            <td>
              <p>암호 쓰기 저장을 활성화/비활성화하는 동안 마법사에서 "MA를 찾을 수 없다는" 오류입니다</p>
            </td>
            <td>
              <p>다음과 같은 상황에서 매니페스트한 Azure AD Connect의 릴리스 버전에서는 알려진 버그가 있습니다.</p>
              <ol class="ordered">
                <li>
자격 증명을 사용하여 테넌트 abc.com(확인된 도메인)에 대한 Azure AD Connect을 구성합니다. "abc.com – AAD"라는 이름으로 AAD 커넥터가 생성되었습니다.<br\><br\></li>
                <li>
그런 다음 커넥터용 AAD 자격 증명을 (이전 동기화 UI 사용함) (동일한 테넌트지만 서로 다른 도메인 이름임)로 바꿉니다. <br\><br\></li>
                <li>
이제 암호 쓰기 저장 활성화/비활성화를 시도할 수 있습니다. 마법사는 자격 증명을 사용하여 "abc.onmicrosoft.com – AAD"으로 커넥터의 이름을 생성하고 암호 쓰기 저장 cmdlet에 전달합니다. 이 이름을 사용하여 만든 커넥터가 없기 때문에 실패합니다.<br\><br\></li>
              </ol>
              <p>이것은 최신 빌드에서 수정되었습니다. 이전 빌드를 설정한 경우 해결 방법은 기능을 활성화/비활성화하는 Powershell cmdlet을 사용하는 것입니다. 이 작업을 수행하는 방법에 대한 자세한 내용은 <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">비밀번호 쓰기 저장을 활성화/비활성화하는 방법</a>에서 "2단계: 디렉터리 동기화 컴퓨터에서 비밀번호 쓰기 저장 사용 &amp; 방화벽 규칙을 구성"을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>도메인 관리자/ 엔터프라이즈 관리자 등과 같은 특수한 그룹의 사용자용 암호를 다시 설정할 수 없습니다.</p>
            </td>
            <td>
              <p>페더레이션된 페더레이션되거나 암호 해시 동기화되어 보호된 그룹의 일부로서 자신의 암호를 재설정하려는 사용자는 서비스 문제가 나타나는 암호를 제출한 후 오류가 나타납니다.</p>
            </td>
            <td>
              <p>Active Directory에 권한이 있는 사용자는 AdminSDHolder를 사용하여 보호됩니다. 자세한 내용은 <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a>를 참조하세요. </p>
              <p>

              </p>
              <p>즉, 이러한 개체에서 보안 설명자는 AdminSDHolder에 지정된 개체와 일치하도록 정기적으로 검사하고 일치하지 않는 경우 재설정됩니다. 따라서 암호 쓰기 저장에 필요한 추가 사용 권한을 그러한 사용자에게 주지 않습니다. 이러한 사용자에게는 암호 쓰기 저장이 작동하지 않을 수 있습니다. 결과적으로 이는 AD 보안 모델을 저해하므로 이러한 그룹 내에서 사용자용 관리 암호를 지원하지 않습니다 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>개체의 재설정 작업 실패를 찾을 수 없습니다</p>
            </td>
            <td>
              <p>페더레이션된 페더레이션되거나 암호 해시 동기화되어 자신의 암호를 재설정하려는 사용자는 서비스 문제가 나타나는 암호를 제출한 후 오류가 나타납니다.</p>
              <p>

              </p>
              <p>이외에 암호 재설정 작업 중에 Azure AD Connect 서비스에서 이벤트 로그에 "개체를 찾을 수 없습니다"라는 오류가 나타날 수 있습니다.</p>
            </td>
            <td>
              <p>이 오류는 일반적으로 동기화 엔진이 AAD 커넥터 공간 또는 연결된 MV에서 사용자 개체 또는 AD 커넥터 공간 개체를 찾을 수 없음을 나타냅니다. </p>
              <p>

              </p>
              <p>이 문제를 해결하려면 Azure AD Connect의 현재 인스턴스를 통해 사용자가 온-프레미스에서 AAD에 실제로 동기화되었는지 확인하고 커넥터 공간 및 MV에서 개체의 상태를 검사합니다. AD CS 개체가 "Microsoft.InfromADUserAccountEnabled.xxx" 규칙을 통한 MV 개체의 커넥터인지 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>재설정 작업은 여러 일치 항목이 오류를 찾아내면 실패합니다</p>
            </td>
            <td>
              <p>페더레이션된 페더레이션되거나 암호 해시 동기화되어 자신의 암호를 재설정하려는 사용자는 서비스 문제가 나타나는 암호를 제출한 후 오류가 나타납니다.</p>
              <p>

              </p>
              <p>이외에 암호 재설정 작업 중에 Azure AD Connect 서비스에서 이벤트 로그에 "여러 에러를 찾았습니다"라는 오류가 나타날 수 있습니다.</p>
            </td>
            <td>
              <p>이는 MV 개체가 "Microsoft.InfromADUserAccountEnabled.xxx"를 통해 둘 이상의 연결된 AD CS 개체라는 점을 동기화 엔진이 감지했음을 나타냅니다. 이는 사용자에게 둘 이상의 포리스트에 활성화된 계정이 있음을 의미합니다. </p>
              <p>

              </p>
              <p>현재 시나리오는 암호 쓰기 저장을 지원하지 않습니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>구성 오류가 있으면 암호 작업은 실패합니다.</p>
            </td>
            <td>
              <p>구성 오류가 있으면 암호 작업은 실패합니다. 응용 프로그램 이벤트 로그는 텍스트와 함께 Azure AD Connect 오류 6329를 포함합니다. 0x8023061f (이 관리 에이전트에서 암호 동기화를 사용하지 않기 때문에 작업이 실패했습니다.)</p>
            </td>
            <td>
              <p>암호 쓰기 저장 기능이 이미 설정된&nbsp;<strong>후에</strong> Azure AD Connect이 새 AD 포리스트를 추가(또는 기존 포리스트를 제거하고 다시 추가)하여 변경하려는 경우 발생합니다. 이렇게 새로 추가된 포리스트에서 사용자용 암호 작업은 실패합니다. 이 문제를 해결하려면 포리스트 구성이 변경된 후에 비활성화 및 재활성화된 암호 쓰기 저장 기능을 완료합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>사용자가 적절하게 재설정한 암호 쓰기 저장이지만 사용자가 변경하거나 관리자가 사용자용으로 재설정한 암호 쓰기 저장은 실패합니다.</p>
            </td>
            <td>
              <p>Azure 관리 포털에서 사용자를 대신하여 암호를 재설정하려고 할 때 다음과 같은 내용의 메시지가 표시됩니다. “온-프레미스 환경에서 실행되는 암호 재설정 서비스는 관리자가 사용자 암호 재설정하도록 지원하지 않습니다. 이 문제를 해결하려면 Azure AD Connect최신 버전으로 업그레이드하십시오.”</p>
            </td>
            <td>
              <p>이는 동기화 엔진 버전이 사용된 특정 암호 쓰기 저장 작업을 지원하지 않는 경우 발생합니다. Azure AD Connect 1.0.0419.0911 이후 버전은 Azure 관리 포털에서 암호를 재설정 쓰기 저장, 암호 변경 쓰기 저장 및 관리자가 시작한 암호 재설정 쓰기 저장을 포함하는 모든 암호 관리 작업을 지원합니다.&nbsp; DirSync 1.0.6862 이후 버전이 암호 재설정 쓰기 저장을 지원합니다. 이 문제를 해결하려면 Azure AD Connect 또는 Azure Active Directory Connect의 최신 버전을 설치하는 것이 좋습니다. 조직에서 이 문제를 해결하고 암호 쓰기 저장을 최대한 활용하려면 [온-프레미스 ID 통합](connect/active-directory-aadconnect.md)을 참조하세요.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>암호 쓰기 저장 이벤트 로그 오류 코드
암호 쓰기 저장과 관련된 문제를 해결할 때 가장 좋은 방법은 Azure AD Connect 컴퓨터에서 해당 응용 프로그램 이벤트 로그를 검사하는 것입니다. 이 이벤트 로그는 암호 쓰기 저장에 관심을 둔 두 개의 소스에서 비롯된 이벤트를 포함합니다. PasswordResetService 소스는 암호 쓰기 저장 작업에 관련된 작업 및 문제를 설명합니다. ADSync 소스는 AD 환경에서 암호 설정과 관련된 작업 및 문제를 설명합니다.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>코드</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>이름/메시지</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>원본</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>설명</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>재귀 한도 초과: MMS(4924) 0x80230619 – "제한은 암호가 지정된 현재 공급자로 변경되지 않도록 방지합니다."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>이 이벤트는 암호 쓰기 저장 서비스가 암호 사용 기간, 기록, 복잡성 또는 도메인의 필터링 요구 사항에 맞지 않는 로컬 디렉터리에 암호를 설정하려고 할 때 발생 합니다.</p>
              <ul>
                <li class="unordered">
최소 암호 사용 기간을 사용하고 최근 시간의 해당 창 내에서 암호를 변경하는 경우 사용자 도메인에 지정된 보존 기간에 도달할 때까지 다시 암호를 변경할 수 없습니다. 테스트를 위해 최소 보존 기간을 0으로 설정해야 합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 기록 요구 사항을 활성화하는 경우 마지막 N 번에서 사용되지 않은 암호를 선택해야 합니다. 여기서 N은 암호 기록 설정입니다. 마지막 N 번에서 사용된 암호를 선택하는 경우 오류가 나타납니다. 테스트를 위해 기록을 0으로 설정해야 합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 복잡성 요구 사항이 있는 경우 사용자가 암호를 변경하거나 재설정하려고 할 때 이들 모두를 강제제합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 필터를 사용하고 사용자가 필터링 조건을 충족하지 않는 암호를 선택한 경우 재설정 또는 변경 작업이 실패합니다.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>동기화 엔진이 오류 hr=80230402를 반환했습니다. 메시지= 동일한 앵커를 가진 중복된 항목이 있기 때문에 개체를 가져오려는 시도가 실패했습니다.</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>이 이벤트는 여러 도메인에 동일한 사용자 ID가 설정된 경우에 발생합니다.  예를 들어 계정/리소스 포리스트를 동기화하고 각각에서 동일한 사용자 ID가 나타나고 사용된 경우 이 오류가 발생할 수 있습니다.  </p>
              <p>사용자가 고유하지 않은 앵커 특성(예: 별칭 또는 UPN)을 사용하고 두 명의 사용자가 해당하는 동일한 앵커 특성을 공유하는 경우 이 오류가 발생할 수 있습니다.</p>
              <p>이 문제를 해결하려면 도메인 내에서 중복된 사용자가 없도록 하고 각 사용자에 고유한 앵커 특성을 사용하고 있는지를 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 온-프레미스 서비스가 클라우드에서 시작한 페더레이션되거나 암호 해시 동기화된 사용자를 위해 암호 재설정 요구를 검색하도록 합니다. 이 이벤트는 쓰기 저장 작업을 재설정한 모든 암호의 첫 번째 이벤트입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 사용자가 암호 재설정 작업 중 새 암호를 선택함을 나타냅니다. 이 암호는 기업의 암호 요구 사항을 만족하고 해당 암호가 다시 로컬 AD 환경에 성공적으로 작성되었음을 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 사용자가 암호를 선택하고 암호가 온-프레미스 환경에 성공적으로 도달했지만 로컬 AD 환경에서 암호를 설정하려고 할 때 오류가 발생했음을 나타냅니다. 이 옵션은 다음과 같은 이유로 발생할 수 있습니다.</p>
              <ul>
                <li class="unordered">
사용자의 암호는 도메인에 대한 나이, 기록, 복잡성 또는 필터 요구를 충족하지 않습니다. 이를 해결하려면 완전히 새로운 암호를 시도하세요.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA 서비스 계정은 우려되는 사용자 계정에 새 암호를 설정하는 적절한 권한이 없습니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
사용자의 계정은 암호 집합 작업을 허용하지 않는 도메인 또는 엔터프라이즈 관리자와 같은 보호되는 그룹에 있습니다.<br\><br\></li>
              </ul>
              <p>이 오류를 일으킬 수 있는 다른 상황에 대한 자세한 내용은 <a href="#troubleshoot-password-writeback">암호 쓰기 저장 문제 해결</a>을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 Azure AD Connect로 암호 쓰기 저장을 사용하도록 설정하는 경우 발생하며 암호 쓰기 저장 웹 서비스에 사용자의 조직을 온보딩하기 시작했음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 온보딩 프로세스가 완료되었으며 암호 쓰기 저장 기능을 사용할 준비가 되었음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 온-프레미스 서비스가 클라우드에서 시작한 페더레이션되거나 암호 해시 동기화된 사용자를 위해 암호 변경 요구를 검색하도록 합니다. 이 이벤트는 쓰기 저장 작업을 변경한 모든 암호의 첫 번째 이벤트입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 사용자가 암호 변경 작업 중 새 암호를 선택함을 나타냅니다. 이 암호는 기업의 암호 요구 사항을 만족하고 해당 암호가 다시 로컬 AD 환경에 성공적으로 작성되었음을 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 사용자가 암호를 선택하고 암호가 온-프레미스 환경에 성공적으로 도달했지만 로컬 AD 환경에서 암호를 설정하려고 할 때 오류가 발생했음을 나타냅니다. 이 옵션은 다음과 같은 이유로 발생할 수 있습니다.</p>
              <ul>
                <li class="unordered">
사용자의 암호는 도메인에 대한 나이, 기록, 복잡성 또는 필터 요구를 충족하지 않습니다. 이를 해결하려면 완전히 새로운 암호를 시도하세요.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA 서비스 계정은 우려되는 사용자 계정에 새 암호를 설정하는 적절한 권한이 없습니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
사용자의 계정은 암호 집합 작업을 허용하지 않는 도메인 또는 엔터프라이즈 관리자와 같은 보호되는 그룹에 있습니다.<br\><br\></li>
              </ul>
              <p>이 오류를 일으킬 수 있는 다른 상황에 대한 자세한 내용은 <a href="#troubleshoot-password-writeback">암호 쓰기 저장 문제 해결</a>을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>온-프레미스 서비스는 사용자 대신 관리자에서 시작한 페더레이션되거나 암호 해시 동기화된 사용자를 위해 암호 재설정 요구를 검색합니다. 이 이벤트는 쓰기 저장 작업을 재설정한 모든 관리자가 시작한 암호의 첫 번째 이벤트입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>관리자가 스스로 시작한 암호 재설정 작업 중 새 암호를 선택합니다. 이 암호는 기업의 암호 요구 사항을 만족하고 해당 암호가 다시 로컬 AD 환경에 성공적으로 작성되었음을 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>관리자가 사용자 대신 암호를 선택하고 암호가 온-프레미스 환경에 성공적으로 도달했지만 로컬 AD 환경에서 암호를 설정하려고 할 때 오류가 발생했음을 나타냅니다. 이 옵션은 다음과 같은 이유로 발생할 수 있습니다.</p>
              <ul>
                <li class="unordered">
사용자의 암호는 도메인에 대한 나이, 기록, 복잡성 또는 필터 요구를 충족하지 않습니다. 이를 해결하려면 완전히 새로운 암호를 시도하세요.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
MA 서비스 계정은 우려되는 사용자 계정에 새 암호를 설정하는 적절한 권한이 없습니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
사용자의 계정은 암호 집합 작업을 허용하지 않는 도메인 또는 엔터프라이즈 관리자와 같은 보호되는 그룹에 있습니다.<br\><br\></li>
              </ul>
              <p>이 오류를 일으킬 수 있는 다른 상황에 대한 자세한 내용은 <a href="#troubleshoot-password-writeback">암호 쓰기 저장 문제 해결</a>을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 Azure AD Connect로 암호 쓰기 저장을 사용하지 않는 경우에 발생하며 사용자 조직을 암호 쓰기 저장 웹 서비스로 오프보딩하기 시작했음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 오프보딩 프로세스가 완료되었으며 암호 쓰기 저장 기능이 성공적으로 비활성화되었음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 오프보딩 프로세스가 성공했는지를 나타냅니다. 이는 구성하는 동안 지정된 클라우드 또는 온-프레미스 관리자 계정에서 발생한 사용 권한 오류 때문이거나 또는 암호 쓰기 저장을 비활성화하는 경우 페더레이션된 클라우드 전역 관리자를 사용하려고 했기 때문일 수 있습니다. 이 문제를 해결하려면 암호 쓰기 저장 기능을 구성하는 동안 관리 권한과 페더레이션된 계정을 사용하지 않는지를 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호 쓰기 저장 서비스를 성공적으로 시작하고 클라우드에서 암호 관리 요청을 받을 준비가 되었음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호 쓰기 저장 서비스가 중지되고 클라우드에서 모든 암호 관리 요청이 성공할 수 없음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 오프보딩 또는 온보딩 프로세스를 시작하기 위해 Azure AD Connect 설치 중에 지정된 전역 관리자에 대한 권한 부여 토큰을 성공적으로 검색했음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 클라우드에서 온-프레미스 환경에 보내야 하는 암호를 암호화하는 데 사용할 암호 암호화 키를 성공적으로 만들었음을 나타냅니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호 관리 작업을 하는 동안 알 수 없는 오류를 나타냅니다. 자세한 내용은 이벤트에서 예외 텍스트를 확인합니다. 문제가 발생하는 경우 암호 쓰기 저장 기능을 비활성화하고 재활성화하여 시도하십시오. 이렇게 해도 도움이 되지 않으면 지원 담당 엔지니어에 지정된 소식지 추적 id와 함께 이벤트 로그의 복사본을 포함합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 클라우드 암호 재설정 서비스에 연결된 오류가 있음을 나타내며 일반적으로 온-프레미스 서비스가 암호 재설정 웹 서비스에 연결할 수 없을 때 발생합니다. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 테넌트의 서비스 버스 인스턴스에 연결하는 오류가 발생했음을 나타냅니다. 이는 온-프레미스 환경에서 아웃 바운드 연결을 차단하기 때문에 발생할 수 있습니다. TCP 443을 통해 <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>으로 연결을 허용하도록 방화벽을 확인하고 다시 시도하세요. 여전히 문제가 발생하는 경우 암호 쓰기 저장 기능을 비활성화하고 재활성화하여 시도하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 웹 서비스 API에 전달 된 입력이 잘못되었음을 나타냅니다. 작업을 다시 시도합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 클라우드에서 도착하는 암호를 해독하는 오류가 발생했음을 나타냅니다. 클라우드 서비스 및 온-프레미스 환경 간에 암호 해독 키가 불일치하기 때문일 수 있습니다. 이를 해결하기 위해 온-프레미스 환경에서 암호 쓰기 저장을 비활성화하고 재활성화합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>온보딩하는 동안 온-프레미스 환경의 구성 파일에서 테넌트 관련 정보를 저장합니다. 이 이벤트는 이 파일을 저장하는 오류가 발생했거나 서비스를 시작할 때 파일을 읽는 오류가 발생함을 나타냅니다. 이 문제를 해결하려면 이 구성 파일을 다시 작성하도록 강제하여 암호 쓰기 저장을 비활성화 및 재활성화하십시오. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>사용되지 않음 –이 이벤트는 Azure AD Connect에 나타나지 않으며 쓰기 저장을 지원하는 DirSync의 초기 빌드입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>온보딩하는 동안 데이터를 클라우드에서 온-프레미스 암호 재설정 서비스로 보냅니다. 그런 다음 해당 데이터가 동기화 서비스에 전송되어 디스크에 이 정보를 안전하게 저장하기 전에 메모리 내 파일에 기록됩니다. 이 이벤트는 메모리에서 해당 데이터를 작성하거나 업데이트하는 데 문제가 있음을 나타냅니다. 이 문제를 해결하려면 이 구성을 다시 작성하도록 강제하여 암호 쓰기 저장을 비활성화 및 재활성화하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호 재설정 웹 서비스에서 잘못된 응답을 받았음을 나타냅니다. 이 문제가 해결하려면 암호 쓰기 저장 기능을 비활성화하고 재활성화하여 시도하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 Azure AD Connect 설치 중에 지정된 전역 관리자 계정에 대한 권한 부여 토큰을 가져올 수 없음을 나타냅니다. 이 오류는 전역 관리자 계정으로 지정된 잘못된 사용자 이름 또는 암호 또는 지정된 전역 관리자 계정이 페더레이션되어 발생할 수 있습니다. 이 문제를 해결 하려면 올바른 사용자 이름과 암호를 사용하여 구성을 다시 실행하고 관리자가 관리된(클라우드 전용 또는 암호 동기화된) 계정인지를 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 클라우드 서비스에서 도달한 암호 암호화 키를 생성하거나 암호를 해독할 때 오류가 발생 했음을 나타냅니다. 이 오류가 환경에 문제가 있음을 나타낼 수 있습니다. 이 문제를 해결하고 자세한 이벤트 로그에서 세부 정보를 확인합니다. 이를 해결하려면 암호 쓰기 저장 서비스를 비활성화하고 재활성화할 수 있습니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 온보딩 프로세스를 시작하기 위해 온-프레미스 서비스가 암호 재설정 웹 서비스와 제대로 통신하지 못함을 나타냅니다. 이는 테넌트에 대한 인증 토큰을 가져오는 방화벽 규칙 또는 문제 때문일 수 있습니다. 이를 해결하려면 TCP 443 및 TCP 9350-9354를 통해 <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>으로 아웃 바운드 연결을 차단하지 않았는지와 온보드에 사용하는 AAD 관리자 계정이 페더레이션되었는지를 확인합니다. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>사용되지 않음 –이 이벤트는 Azure AD Connect에 나타나지 않으며 쓰기 저장을 지원하는 DirSync의 초기 빌드입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 오프보딩 프로세스를 시작하기 위해 온-프레미스 서비스가 암호 재설정 웹 서비스와 제대로 통신하지 못함을 나타냅니다. 이는 테넌트에 대한 인증 토큰을 가져오는 방화벽 규칙 또는 문제 때문일 수 있습니다. 이를 해결하려면 443을 통해 또는 <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>으로 아웃 바운드 연결을 차단하지 않았는지와 오프보드에 사용하는 AAD 관리자 계정이 페더레이션되었는지를 확인합니다. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 테넌트의 서비스 버스 인스턴스에 연결하려고 재시도해야 함을 나타냅니다. 정상 조건에서 우려해야 하지만 여러 번 이 이벤트가 나타나는 경우, 특히 긴 대기 시간 또는 낮은 대역폭 연결이면 서비스 버스에 대한 네트워크 연결을 확인합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>암호 쓰기 저장 서비스의 상태를 모니터링하려면 5분 마다 Heartbeat 데이터를 암호 재설정 웹 서비스로 보냅니다. 이 이벤트는 클라우드 웹 서비스에 이 상태 정보를 보낼 때 오류가 발생했음을 나타냅니다. 이 상태 정보는 OII 또는 PII 데이터를 포함하지 않고 순수하게 Heartbeat 및 기본 서비스 통계이므로 클라우드에서 서비스 상태 정보를 제공할 수 있습니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 AD가 반환한 알 수 없는 오류가 있음을 나타냅니다. 이 오류에 대한 자세한 내용은 ADSync 소스에서 이벤트에 대한 Azure AD Connect 서버 이벤트 로그를 확인하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호를 재설정 또는 변경하려는 사용자를 온-프레미스 디렉터리에서 찾지 못했음을 나타냅니다. 사용자가 온-프레미스 삭제되지만 클라우드에서 삭제되지 않았을 때 또는 동기화에 문제가 있는 경우 이 문제가 발생할 수 있습니다. 자세한 정보는 마지막 동기화 실행 세부 정보 및 동기화 로그를 확인하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>암호를 재설정하거나 클라우드에서 시작된 변경을 요청한 경우 Azure AD Connect의 설치 프로세스 중에 지정된 클라우드 앵커를 사용하여 온-프레미스 환경에서 사용자에게 해당 요청을 다시 연결하는 방법을 결정합니다. 이 이벤트는 같은 클라우드 앵커 특성을 가진 온-프레미스 디렉터리에 두 사용자가 있음을 나타냅니다. 자세한 정보는 마지막 동기화 실행 세부 정보 및 동기화 로그를 확인하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 관리 에이전트 서비스 계정이 해당 계정에 새 암호를 설정할 적절한 권한을 가지지 않음을 나타냅니다. 사용자의 포리스트에서 MA 계정이 포리스트의 모든 개체에 암호 재설정 및 변경 권한이 있는지 확인합니다.  이를 수행하는 방법에 대한 자세한 내용은 <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">4단계: 적절한 Active Directory 사용 권한 설정</a>을 참조하세요.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 온-프레미스에서 사용하지 않도록 설정된 계정의 암호를 변경 또는 재설정하려고 시도했음을 나타냅니다. 계정을 활성화하고 작업을 다시 시도합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이벤트는 온-프레미스에서 잠긴 계정의 암호를 변경 또는 재설정하려고 시도했음을 나타냅니다. 사용자가 짧은 기간 동안 너무 여러 번 암호 작업을 변경 또는 재설정하려는 경우 잠겨질 수 있습니다. 계정의 잠금을 풀고 작업을 다시 시도합니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 사용자가 변경 작업을 수행할 때 잘못된 현재 암호를 지정했음을 나타냅니다. 올바른 현재 암호를 지정하고 다시 시도하십시오.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 암호 쓰기 저장 서비스가 암호 사용 기간, 기록, 복잡성 또는 도메인의 필터링 요구 사항에 맞지 않는 로컬 디렉터리에 암호를 설정하려고 할 때 발생 합니다.</p>
              <ul>
                <li class="unordered">
최소 암호 사용 기간을 사용하고 최근 시간의 해당 창 내에서 암호를 변경하는 경우 사용자 도메인에 지정된 보존 기간에 도달할 때까지 다시 암호를 변경할 수 없습니다. 테스트를 위해 최소 보존 기간을 0으로 설정해야 합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 기록 요구 사항을 활성화하는 경우 마지막 N 번에서 사용되지 않은 암호를 선택해야 합니다. 여기서 N은 암호 기록 설정입니다. 마지막 N 번에서 사용된 암호를 선택하는 경우 오류가 나타납니다. 테스트를 위해 기록을 0으로 설정해야 합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 복잡성 요구 사항이 있는 경우 사용자가 암호를 변경하거나 재설정하려고 할 때 이들 모두를 강제제합니다.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
암호 필터를 사용하고 사용자가 필터링 조건을 충족하지 않는 암호를 선택한 경우 재설정 또는 변경 작업이 실패합니다.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>이 이벤트는 Active Directory 구성 문제로 인해 암호를 다시 온-프레미스 디렉터리로 작성하는 문제가 있음을 나타냅니다. 어떤 오류가 발생했는지에 대한 자세한 내용은 ADSync 서비스로부터 온 메시지용 Azure AD Connect 컴퓨터의 응용 프로그램 이벤트 로그를 확인하십시오. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>사용되지 않음 –이 이벤트는 Azure AD Connect에 나타나지 않으며 쓰기 저장을 지원하는 DirSync의 초기 빌드입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>사용되지 않음 –이 이벤트는 Azure AD Connect에 나타나지 않으며 쓰기 저장을 지원하는 DirSync의 초기 빌드입니다.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>사용되지 않음 –이 이벤트는 Azure AD Connect에 나타나지 않으며 쓰기 저장을 지원하는 DirSync의 초기 빌드입니다.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>암호 쓰기 저장 연결 문제 해결
Azure AD Connect의 암호 쓰기 저장 구성 요소로 서비스 중단이 발생하는 경우 이 문제를 해결하기 위해 수행할 수 있는 빠른 단계는 다음과 같습니다.

* [Azure AD Connect 동기화 서비스 다시 시작](#restart-the-azure-AD-Connect-sync-service)
* [암호 쓰기 저장 기능을 비활성화 및 재활성화](#disable-and-re-enable-the-password-writeback-feature)
* [최신 Azure AD Connect 릴리스 설치](#install-the-latest-azure-ad-connect-release)
* [암호 쓰기 저장 문제 해결](#troubleshoot-password-writeback)

일반적으로 가장 빠른 방법으로 서비스를 복구하기 위해 위의 순서로 이 단계를 실행하는 것이 좋습니다.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Azure AD Connect 동기화 서비스 다시 시작
Azure AD Connect 동기화 서비스를 다시 시작하면 연결 문제 또는 서비스와 관련된 다른 일시적인 문제를 해결하는 데 도움이 됩니다.

1. 관리자로써 **Azure AD Connect**를 실행하는 서버에서 **시작**을 클릭합니다.
2. 검색 상자에 **“services.msc”**를 입력하고 **Enter** 키를 누릅니다.
3. **Microsoft Azure AD Connect** 항목을 찾습니다.
4. 서비스 항목을 마우스 오른쪽 단추로 클릭하여 **다시 시작**을 클릭하고 작업이 완료되기를 기다립니다.
   
   ![][002]

이러한 단계는 클라우드 서비스를 사용하여 연결을 다시 설정하고 발생한 중단이 해결할 수 있습니다.  동기화 서비스를 다시 시작해도 문제가 해결되지 않으면 다음 단계로 암호 쓰기 저장 기능을 비활성화 및 재활성화하는 것이 좋습니다.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>암호 쓰기 저장 기능을 비활성화 및 재활성화
다시 암호 쓰기 저장 기능을 비활성화 및 재활성화하면 연결 문제를 해결할 수 있습니다.

1. 관리자 권한으로 **Azure AD Connect 구성 마법사**를 엽니다.
2. **Azure AD에 연결** 대화 상자에서 **Azure AD 전역 관리자 자격 증명**을 입력합니다
3. **AD DS에 연결** 대화 상자에서 **AD 도메인 서비스 관리자 자격 증명**을 입력합니다
4. **고유하게 식별하는 사용자** 대화 상자에서 **다음** 단추를 클릭합니다.
5. **선택적 기능** 대화 상자에서 **암호 쓰기 저장** 확인란을 선택 취소합니다.
   
   ![][003]
6. **구성할 준비** 페이지에 도달할 때까지 아무것도 변경하지 않고 남은 대화 상자 페이지를 통해 **다음**을 클릭합니다.
7. 구성 페이지가 **암호 쓰기 저장 옵션을 사용 안함으로** 표시하는지 확인하고 변경 내용을 적용하려면 녹색 **구성** 단추를 클릭합니다.
8. **마침** 대화 상자에서 **지금 동기화** 옵션을 선택 취소한 다음 마법사를 닫기 위해 **마침**을 클릭합니다.
9. **Azure AD Connect 구성 마법사**를 다시 엽니다.
10. 서비스를 다시 활성화하는 **선택적 기능** 화면에서 **암호 쓰기 저장 옵션을 선택**했는지 확인한 후 **2~8단계를 반복**합니다.
    
    ![][004]

이러한 단계를 실행하여 클라우드 서비스를 사용하여 연결을 다시 설정하고 발생할 수 있는 중단이 해결될 수 있습니다.

암호 쓰기 저장 기능을 비활성화 및 재활성화해도 문제가 해결되지 않으면 다음 단계로 Azure AD Connect를 재설치하는 것이 좋습니다.

### <a name="install-the-latest-azure-ad-connect-release"></a>최신 Azure AD Connect 릴리스 설치
Azure AD Connect 패키지를 재설치하면 클라우드 서비스 연결 또는 로컬 AD 환경에서 암호를 관리하는 능력에 영향을 미칠 수 있는 모든 구성 문제를 해결할 것입니다.
위에서 설명한 처음 두 단계를 시도한 후에 이 단계를 수행하는 것이 좋습니다.

1. 최신 버전의 Azure AD Connect을 [여기서](connect/active-directory-aadconnect.md#install-azure-ad-connect)다운로드
2. Azure AD Connect를 이미 설치했기 때문에 Azure AD Connect 설치를 최신 버전으로 업데이트하려면 바로 업그레이드를 수행해야 합니다.
3. 다운로드한 패키지를 실행하고 화면에 나타나는 지침을 따라 Azure AD Connect 컴퓨터를 업데이트하십시오.  상자 동기화 규칙 중 사용자 지정을 하지 않으면 추가 수동 단계가 필요하지 않으며 이 경우 **업그레이드를 계속하기 전에 이를 백업하고 완료된 이후 수동으로 다시 배포**해야 합니다.

이러한 단계를 실행하여 클라우드 서비스를 사용하여 연결을 다시 설정하고 발생할 수 있는 중단이 해결될 수 있습니다.

최신 버전의 Azure AD Connect 서버를 설치해도 문제가 해결되지 않으면 최신 동기화 QFE를 설치한 후에 마지막 단계로서 암호 쓰기 저장 기능을 비활성화 및 재활성화하는 것이 좋습니다.

문제를 해결되지 않으면 [암호 쓰기 저장 문제 해결](#troubleshoot-password-writeback) 및 [Azure AD 암호 관리 FAQ](active-directory-passwords-faq.md)에서 문제를 논의했는지 확인하기 위해 살펴보는 것이 좋습니다

<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>암호 재설정 설명서에 대한 링크
다음은 모든 Azure AD 암호 재설정 설명서 페이지에 대한 링크입니다.

* **로그인하는 데 문제가 있나요?** 그렇다면 [암호를 변경하고 재설정하는 방법은 다음과 같습니다](active-directory-passwords-update-your-own-password.md).
* [**작동 방식**](active-directory-passwords-how-it-works.md) -&6;개의 다양한 구성 요소 서비스 및 기능에 대해 알아봅니다.
* [**시작하기**](active-directory-passwords-getting-started.md) -사용자가 클라우드 또는 온-프레미스 암호를 다시 설정하고 변경할 수 있는 방법에 대해 알아봅니다.
* [**사용자 지정**](active-directory-passwords-customize.md) - 모양과 느낌 및 조직의 요구에 맞게 서비스의 동작을 사용자 지정하는 방법에 대해 알아봅니다
* [**모범 사례**](active-directory-passwords-best-practices.md) - 사용자의 조직에서 신속하게 배포하고 효과적으로 암호를 관리하는 방법에 대해 알아봅니다.
* [**정보 활용**](active-directory-passwords-get-insights.md) -우리의 통합된 보고 기능에 대해 알아봅니다
* [**FAQ**](active-directory-passwords-faq.md) -자주 묻는 질문에 답변합니다.
* [**자세히 알아보기**](active-directory-passwords-learn-more.md) -서비스의 작동 원리 방식의 기술적 측면을 자세히 알아봅니다.

[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"



<!--HONumber=Feb17_HO2-->


