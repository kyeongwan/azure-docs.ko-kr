---
title: "Azure AD B2C 사용량 보고 API 샘플 및 정의 | Microsoft Docs"
description: "B2C 테넌트 사용자, 인증 및 MFA에 대한 보고서를 가져오는 방법에 대한 가이드 및 샘플입니다."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: joroja
translationtype: Human Translation
ms.sourcegitcommit: 274ed196cc7159e77f6de4d84328c3607b155ee9
ms.openlocfilehash: 9bb528aa0172fb7179b5498be89aee9a92b788f8
ms.lasthandoff: 02/11/2017


---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>보고 API를 통해 Azure AD B2C에서 사용량 보고서에 액세스

Azure Active Directory B2C는 ID 공급자에 응용 프로그램의 제품군의 모든 최종 사용자에게 로그인 및 MFA 기반 인증을 제공합니다.  테넌트에서 등록된 사용자의 수, 등록하는 데 사용된 공급자 및 유형별 인증 수를 알고 있다면 다음과 같은 질문에 답변할 수 있습니다.
* 지난 10일 동안 각 유형의 ID 공급자(예: Microsoft 계정, LinkedIn)에서 등록한 사용자는 몇 명인가요?
* 지난 달에 성공적으로 완료된 Multi-Factor Authentications은 몇 번인가요?
* 이번 달에 완료된 로그인 기반 인증은 몇 번인가요? 일일? 응용 프로그램 당?
* 내 B2C 테넌트 작업의 예상된 월별 비용을 예측하려면 어떻게 해야 하나요?

이 문서에서는 청구 작업과 가장 밀접하게 관련된 보고서에 중점을 둡니다. 이는 사용자 수, 청구 가능 로그인 기반 인증 수 및 Multi-Factor Authentication 수에 기반합니다.


## <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Azure AD Reporting API에 액세스하기 위한 필수 구성 요소
시작하기 전에 [Azure AD Reporting API에 액세스하기 위한 필수 구성 요소](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)를 완료해야 합니다.  응용 프로그램을 만들고, 그 암호를 가져오고, Azure AD B2C 테넌트의 보고서에 대한 액세스 권한을 부여합니다. *Bash 스크립트* 및 *Python 스크립트* 예제도 여기에 제공됩니다.

## <a name="powershell-script"></a>PowerShell 스크립트
이 스크립트에서는 **TimeStamp** 매개 변수 및 **-ApplicationId** 필터를 사용하여&4;개의 사용량 보고서를 보여 줍니다.

```
# This script will require the Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.windows.net"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
       $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>사용량 보고서 정의
**tenantUserCount** – 최근 30일 내에 일일 ID 공급자의 유형별 테넌트 사용자 수를 개수합니다. (필요에 따라 TimeStamp 필터는 지정된 날짜에서 현재 날짜까지 사용자 수를 제공합니다.) 보고서에서는 다음을 제공합니다.
 * TotalUserCount = 모든 사용자 개체의 수
 * OtherUserCount = AAD 디렉터리 사용자의 수(B2C가 아닌 사용자)
 * LocalUserCount = B2C 테넌트에 로컬인 자격 증명을 사용하여 만든 B2C 사용자 계정의 수
 * AlternateIdUserCount** = 외부 ID 공급자로 등록된 B2C 사용자의 수(예: facebook, Microsoft 계정, 다른 AAD 테넌트 - OrgId라고도 함)

**b2cAuthenticationCountSummary** – 일별 및 유형별로 구분된 인증 흐름에서 최근 30일 동안 청구 가능한 인증의 수 일일 합계

**b2cAuthenticationCount** -시간 내의 인증 수를 계산합니다. 기본값은 최근 30일입니다.  (선택 사항: 시작 및 종료 TimeStamp는 원하는 수의 특정 기간을 정의합니다.) 출력은 StartTimeStamp(이 테넌트의 작업 중 가장 빠른 날짜) 및 EndTimeStamp(최신 업데이트)를 포함합니다.

**b2cMfaRequestCountSummary** - 일별 및 유형별로 구분된 MFA(SMS 또는 음성)에서 Multi-Factor Authentication의 수 일일 합계


## <a name="limitations"></a>제한 사항
* 사용자 수 데이터는 24-48시간마다 새로 고쳐집니다.  인증은 하루에도 여러 번 업데이트됩니다.
* ApplicationId 필터를 사용할 경우 다음 조건 중 하나로 인해 보고서 응답이 비어 있을 수 있습니다.
 * 응용 프로그램 ID는 테넌트에 존재하지 않습니다. 정확한지 확인하세요.
 * 응용 프로그램 ID가 있지만 보고 기간에서 데이터를 찾지 못했습니다. 날짜 시간 매개 변수를 검토합니다.


## <a name="next-steps"></a>다음 단계
### <a name="estimating-your-azure-ad-monthly-bill"></a>Azure AD 월별 청구서를 예측합니다.
[사용 가능한 최근 Azure AD B2C 가격 책정](https://azure.microsoft.com/pricing/details/active-directory-b2c/)과 결합한 경우 일별, 주별 및 월별 Azure 소비를 예측할 수 있습니다.  예상은 전체 비용에 영향을 줄 수 있는 테넌트 동작의 변경 내용을 계획할 때 특히 유용합니다.  [연결된 Azure 구독](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing)에서 실제 비용을 검토할 수 있습니다.

### <a name="options-for-other-output-formats"></a>다른 출력 형식의 옵션
```
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    

