---
title: "Azure AD의 그룹 관리를 위한 Azure Active Directory PowerShell 미리 보기 cmdlet | Microsoft Docs"
description: "이 페이지에는 Azure Active Directory에서 그룹을 관리하는 데 도움이 되는 PowerShell 예제가 나와 있습니다."
keywords: "Azure AD, Azure Active Directory, PowerShell, 그룹, 그룹 관리"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 4f12fdf6732d44903fb1e971f05d0233d7aff08e


---
# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>그룹 관리를 위한 Azure Active Directory 미리 보기 cmdlet
> [!div class="op_single_selector"]
> * [Azure 포털](active-directory-groups-create-azure-portal.md)
> * [Azure 클래식 포털](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

다음 문서에서는 Azure AD(Azure Active Directory)에서 PowerShell을 사용하여 그룹을 관리하는 방법의 예제를 제공합니다.  또한 Azure AD PowerShell 미리 보기 모듈을 사용하여 설정하는 방법에 대한 정보를 제공합니다. 먼저 [Azure AD PowerShell 모듈을 다운로드](http://go.microsoft.com/fwlink/p/?LinkId=828627)해야 합니다.

## <a name="installing-the-azure-ad-powershell-module"></a>Azure AD PowerShell 모듈 설치
Azure AD PowerShell 미리 보기 모듈을 설치하려면 다음 명령을 사용합니다.

    PS C:\Windows\system32> install-module azureadpreview

미리 보기 모듈이 설치되어 있는지 확인하려면 다음 명령을 사용합니다.

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

이제 모듈에서 cmdlet 사용을 시작할 수 있습니다. Azure AD 미리 보기 모듈의 cmdlet에 대한 자세한 내용은 [온라인 참조 설명서](https://msdn.microsoft.com/library/azure/mt757216.aspx)를 참조하세요.

## <a name="connecting-to-the-directory"></a>디렉터리에 연결
Azure AD PowerShell 미리 보기 cmdlet을 사용하여 그룹 관리를 시작하기 전에 PowerShell 세션을 관리하려는 디렉터리에 연결해야 합니다. 이렇게 하려면 다음 명령을 실행합니다.

    PS C:\Windows\system32> Connect-AzureAD

이 cmdlet은 디렉터리에 액세스하는 데 사용할 자격 증명을 묻습니다. 이 예제에서는 karen@drumkit.onmicrosoft.com 을 사용하여 데모 디렉터리에 액세스합니다. 이 cmdlet은 세션이 디렉터리에 정상적으로 연결되었음을 표시하기 위한 확인을 반환합니다.

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

이제 Azure AD 미리 보기 cmdlet을 사용하여 디렉터리에서 그룹 관리를 시작할 수 있습니다.

## <a name="retrieving-groups"></a>그룹 검색
디렉터리에서 기존 그룹을 검색하려면 Get-AzureADGroups cmdlet을 사용할 수 있습니다. 디렉터리의 모든 그룹을 검색하려면 매개 변수 없이 다음 cmdlet을 사용합니다.

    PS C:\Windows\system32> get-azureadgroup

이 cmdlet은 연결된 디렉터리의 모든 그룹을 반환합니다.

-objectID 매개 변수를 사용하여 그룹의 objectID를 지정하는 특정 그룹을 검색할 수 있습니다.

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

이제 이 cmdlet은 해당 objectID가 사용자가 입력한 매개 변수의 값과 일치하는 그룹을 반환합니다.

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

-filter 매개 변수를 사용하여 특정 그룹을 검색할 수 있습니다. 이 매개 변수는 다음 예제와 같이 ODATA 필터 절을 사용하고 필터와 일치하는 모든 그룹을 반환합니다.

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Azure AD PowerShell cmdlet은 OData 쿼리 표준을 구현합니다. 자세한 내용은 [여기](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter)에서 찾을 수 있습니다.

## <a name="creating-groups"></a>그룹 만들기
디렉터리에 새 그룹을 만들려면 New-AzureADGroup cmdlet을 사용합니다. 이 cmdlet을 "Marketing"이라는 새 보안 그룹을 만듭니다.

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>그룹 업데이트
기존 그룹을 업데이트하려면 Set-AzureADGroup cmdlet을 사용합니다. 이 예제에서는 그룹 “Intune Administrators”의 DisplayName 속성을 변경합니다. 먼저 Get-AzureADGroup cmdlet을 사용하여 그룹을 찾고 DisplayName 특성을 사용하여 필터링합니다.

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

다음으로 Description 속성을 새 값인 “Intune 장치 관리자”로 변경합니다.

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

이제 해당 그룹을 다시 찾으면 새 값을 반영하도록 Description 속성이 업데이트된 것을 볼 수 있습니다.

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>그룹 삭제
디렉터리에서 그룹을 삭제하려면 다음과 같이 Remove-AzureADGroup cmdlet을 사용합니다.

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>그룹 구성원 관리
그룹에 새 구성원을 추가해야 하는 경우 Add-AzureADGroupMember cmdlet을 사용합니다. 이 명령을 수행하면 이전 예제에서 사용한 Intune 관리자 그룹에 구성원이 추가됩니다.

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-ObjectId 매개 변수는 구성원을 추가하려는 그룹의 ObjectID이며, -RefObjectId는 그룹에 구성원으로 추가하려는 사용자의 ObjectID입니다.

그룹의 기존 구성원을 가져오려면 다음 예제와 같이 Get-AzureADGroupMember cmdlet을 사용합니다.

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

이전에 추가한 그룹에 구성원을 제거하려면 다음과 같이 Remove-AzureADGroupMember cmdlet을 사용합니다.

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

사용자의 그룹 구성원 자격을 확인하려면 Select-AzureADGroupIdsUserIsMemberOf cmdlet을 사용합니다. 이 cmdlet은 구성원 자격을 확인할 사용자의 ObjectId와 구성원 자격을 확인하려는 그룹의 목록을 해당 매개 변수로 사용합니다. 그룹 목록은 "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" 형식의 복잡한 변수 형태로 제공되므로 먼저 해당 형식의 변수를 만들어야 합니다.

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

다음으로 다음 복합 변수의 "GroupIds" 특성에 확인할 groupId 값을 제공합니다.

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

이제 $g의 그룹에 대해 ObjectID가 72cd4bbd-2594-40a2-935c-016f3cfeeeea인 사용자의 그룹 구성원 자격을 확인하려는 경우 다음을 사용해야 합니다.

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


반환되는 값은 이 사용자가 구성원인 그룹의 목록입니다. 또한 Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf 또는 Select-AzureADGroupIdsServicePrincipalIsMemberOf를 사용하여 지정된 그룹 목록에 대해 연락처, 그룹 또는 서비스 주체 구성원 자격을 확인할 수도 있습니다.

## <a name="managing-owners-of-groups"></a>그룹 소유자 관리
그룹에 소유자를 추가하려면 Add-AzureADGroupOwner cmdlet을 사용합니다.

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

-ObjectId 매개 변수는 소유자를 추가하려는 그룹의 ObjectID이며, -RefObjectId는 그룹에 소유자로 추가하려는 사용자의 ObjectID입니다.

그룹의 소유자를 검색하려면 Get-AzureADGroupOwner를 사용합니다.

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

이 cmdlet을 실행하면 지정된 그룹에 대한 소유자 목록이 반환됩니다.

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

그룹에서 소유자를 제거하려면 Remove-AzureADGroupOwner를 사용합니다.

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>다음 단계
[Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260)에서 더 많은 Azure Active Directory PowerShell 설명서를 찾을 수 있습니다.

* [Azure Active Directory 그룹을 사용하여 리소스에 대한 액세스 관리](active-directory-manage-groups.md)
* [Azure Active Directory와 온-프레미스 ID 통합](active-directory-aadconnect.md)



<!--HONumber=Dec16_HO5-->


