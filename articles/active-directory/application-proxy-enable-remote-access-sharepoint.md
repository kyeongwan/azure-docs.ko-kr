---
title: "Azure AD 앱 프록시를 통해 SharePoint에 원격 액세스를 사용하도록 설정 | Microsoft Docs"
description: "Azure AD 응용 프로그램 프록시 커넥터에 대한 기본 사항을 제공합니다."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 8436238546515b66b58b31673d54586e4a20f50f
ms.openlocfilehash: 86f10c04368d1c382939b418c6909ca807b3bf5a


---

# <a name="enable-remote-access-to-sharepoint-with-azure-ad-app-proxy"></a>Azure AD 앱 프록시를 통해 SharePoint에 원격 액세스를 사용하도록 설정

이 문서에서는 SharePoint 서버 온-프레미스를 Azure AD 응용 프로그램 프록시에 통합하는 방법에 대해 설명합니다.

> [!NOTE]
> 응용 프로그램 프록시는 Premium 또는 Basic 버전의 Azure Active Directory로 업그레이드하는 경우에만 사용할 수 있는 기능입니다. 자세한 내용은 [Azure Active Directory 버전](active-directory-editions.md)을 참조하세요.
> 

##<a name="prerequisites"></a>필수 조건

이 문서에서는 SharePoint 2013 이상이 사용자 환경에 이미 설정되어 실행되고 있다고 가정합니다. 또한 다음 필수 조건도 고려하세요.

* SharePoint에는 기본 Kerberos 지원을 포함합니다. 따라서 Azure AD 응용 프로그램 프록시를 통해 내부 사이트에 원격으로 액세스하는 사용자는 원활한 Single Sign-On 환경을 보유한다고 가정할 수 있습니다.

* SharePoint 서버에 대한 몇 가지 구성을 변경해야 합니다. 스테이징 환경을 사용하는 것이 좋습니다. 이 방법에서는 스테이징 서버로 먼저 업데이트할 수 있으므로 프로덕션으로 전환하기 전에 테스트 주기를 용이하게 할 수 있습니다.

* 게시된 URL에 SSL이 필요하므로 SharePoint용 SSL을 이미 설정했다고 가정합니다. 내부 사이트에는 SSL을 사용하도록 설정하여 해당 링크가 적절히 전송/매핑되도록 해야 합니다. SSL을 구성하지 않은 경우 SSL 설정에 대한 지침이 포함된 [SharePoint 2013용 SSL 구성](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013)을 참조하세요. 또한 커넥터 컴퓨터는 사용자가 발급한 인증서를 신뢰해야 합니다. (공개적으로 발급된 인증서에는 해당되지 않음)

##<a name="steps-to-set-up-sharepoint"></a>SharePoint 설정 단계

Azure AD 앱 프록시를 통해 SharePoint에 원격 액세스를 사용하도록 설정하려면 다음 단계를 따르세요.

**1부: SSO(Single Sign-On) 구성**

  * A단계. SharePoint 서버가 서비스 계정으로 실행 중인지 확인합니다.
  * B단계. Kerberos용 SharePoint 구성
  * C단계. SharePoint에 할당된 계정에 대한 SPN(서비스 주체 이름)을 설정합니다.
  * D단계. 커넥터가 SharePoint에 대한 신뢰할 수 있는 대리자로 설정되는지 확인합니다.

**2부: 안전한 원격 액세스 사용**

 * SharePoint 팜을 Azure AD 앱 프록시에 게시합니다.

**3부: SharePoint에서 외부 URL을 알고 있는지 확인**

 * SharePoint에서 대체 액세스 매핑을 설정합니다.

### <a name="part-1-set-up-single-sign-on-sso-to-sharepoint"></a>1부: SharePoint에 대한 SSO(Single Sign-On) 설정

고객은 백 엔드 응용 프로그램(이 경우 SharePoint 서버)에 대한 최상의 SSO 환경을 원합니다. 일반적인 Azure AD 시나리오에서, 다시 인증하라는 메시지가 표시되지 않으므로 사용자는 한 번만 인증됩니다.

Windows 인증을 사용하거나 필요로 하는 온-프레미스 응용 프로그램에 대해 Kerberos 인증 프로토콜 및 KCD(Kerberos 제한 위임)라는 기능을 사용하여 이를 수행할 수 있습니다. KCD(구성된 경우)를 통해 응용 프로그램 프록시 커넥터는 사용자가 Windows에 직접 로그인하지 않더라도 특정 사용자에 대한 windows 티켓/토큰을 얻을 수 있습니다. KCD에 대한 자세한 내용은 [Kerberos 제한 위임 개요](https://technet.microsoft.com/en-us/library/jj553400.aspx)를 참조하세요.

SharePoint 서버에 대해 설정하려면 아래 단계를 따릅니다.

**A단계: SharePoint가 로컬 시스템, 로컬 서비스 또는 네트워크 서비스가 아닌 서비스 계정으로 실행 중인지 확인**

가장 먼저 수행해야 할 작업은 SharePoint가 정의된 서비스 계정으로 실행 중인지 확인하는 것입니다. SPN(서비스 주체 이름)을 유효한 계정에 연결할 수 있으려면 이 작업을 수행해야 합니다. SPN은 Kerberos 프로토콜이 서로 다른 서비스를 식별하는 방법입니다. 나중에 KCD를 구성하는 데 필요합니다.

사이트가 정의된 서비스 계정으로 실행 중인지 확인하려면 다음을 수행합니다.

1. **SharePoint 2013 중앙 관리** 사이트를 엽니다.
2. **보안**으로 이동하고 **서비스 계정 구성**을 선택합니다.
3. **웹 응용 프로그램 풀 – SharePoint – 80**을 선택합니다. 옵션은 기본적으로 SSL을 사용하는 경우 웹 풀의 이름에 따라 약간 다를 수 있습니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. **이 구성 요소에 대한 계정을 선택하십시오.**가 **로컬 서비스** 또는 **네트워크 서비스**인 경우 계정을 만들어야 합니다. 그렇지 않은 경우 작업이 끝났으며 다음 단계를 진행하면 됩니다. 
5. **새 관리되는 계정을 등록하십시오.**를 선택합니다. 계정이 생성되면 계정을 사용하기 전에 **웹 응용 프로그램 풀**을 설정해야 합니다.

> [!NOTE]
서비스에 대해 미리 생성된 AD 계정이 있어야 합니다. 자동 암호 변경을 허용하는 것이 좋습니다. 전체 단계 집합 및 문제 해결에 대한 자세한 내용은 [SharePoint 2013에서 자동 암호 변경 구성](https://technet.microsoft.com/EN-US/library/ff724280.aspx)을 참조하세요. 

**B단계. Kerberos용 SharePoint 구성**

SharePoint 서버에 대한 SSO(Single Sign-On)를 수행하는 데 KCD를 사용하며 KCD는 Kerberos에서만 작동합니다. 

Kerberos 인증을 위한 SharePoint 사이트를 구성하려면

1. **SharePoint 2013 중앙 관리** 사이트를 엽니다.
2. **응용 프로그램 관리**로 이동하고 **웹 응용 프로그램 관리**를 선택하고 SharePoint 사이트를 선택합니다. 이 예제에서는 **SharePoint – 80**입니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)
  
3. 도구 모음에서 **인증 공급자**를 클릭합니다.
4. **인증 공급자** 상자에서 **기본 영역**을 클릭하여 설정을 확인합니다.
5. **인증 편집** 상자에서 **클레임 인증 유형**이 표시될 때까지 아래로 스크롤하고 **Windows 인증 사용** 및 **Windows 통합 인증**이 모두 선택되어 있는지 확인합니다. 
6. 드롭다운 상자에서 **협상(Kerberos)**이 선택되어 있는지 확인합니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. **인증 편집** 상자 맨 아래에서 **저장**을 클릭합니다.

**C단계: SharePoint 서비스 계정에 대한 SPN 설정**

KCD를 구성하기 전에, 위에서 구성한 서비스 계정으로 실행 중인 SharePoint 서비스를 식별해야 합니다. SPN(서비스 주체 이름)을 설정하여 이 작업을 수행합니다. 자세한 내용은 [서비스 주체 이름](https://technet.microsoft.com/en-us/library/cc961723.aspx)을 참조하세요.

SPN 형식은 다음과 같습니다.

```
<service class>/<host>:<port>
```

_서비스 클래스_는 서비스의 고유한 이름입니다. SharePoint의 경우 HTTP를 사용합니다.

_호스트_는 해당 서비스가 실행 중인 호스트의 정규화된 도메인 이름 또는 Netbios 이름입니다. SharePoint 사이트의 경우 사용 중인 IIS 버전에 따라 사이트의 URL이어야 할 수도 있습니다.

_포트_는 선택 사항입니다. SharePoint 서버에 대한 FQDN이 다음과 같은 경우:

```
sharepoint.demo.o365identity.us
```

SPN은 다음과 같습니다. 

```
HTTP/ sharepoint.demo.o365identity.us demo
```

또한 서버에서 특정 사이트에 대한 SPN을 설정해야 할 수도 있습니다. 자세한 내용은 [Kerberos 인증 구성](https://technet.microsoft.com/en-us/library/cc263449(v=office.12).aspx)을 참조하세요. "Kerberos 인증을 사용하여 웹 응용 프로그램에 대한 서비스 주체 이름 만들기" 섹션을 특히 주의하여 보세요. 

이 작업을 수행하는 가장 쉬운 방법은 사이트에 이미 있는 SPN 형식을 따르는 것입니다. 서비스 계정에 대해 등록할 SPN을 복사합니다. 다음을 수행합니다.

1. SPN으로 다른 컴퓨터에서 해당 사이트로 이동합니다. 
 이 작업을 수행하면 관련 Kerberos 티켓 집합이 컴퓨터에서 캐시됩니다. 이러한 티켓에는 탐색할 대상 사이트의 SPN이 포함되어 있습니다. [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html)라는 도구를 사용하여 해당 사이트에 대한 SPN을 끌어올 수 있습니다.
 
2. 브라우저에서 사이트에 액세스한 사용자와 같은 컨텍스트에서 실행 중인 명령 창에서 다음 명령을 실행합니다. 
```
Klist
```
3. 그러면 대상 서비스 SPN 집합이 반환됩니다. 이 예에서 강조 표시된 값은 필요한 SPN입니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)
  
4. 이제 SPN이 있으며, 웹 응용 프로그램에 대해 이전에 설정한 서비스 계정에서 SPN이 바르게 구성되어 있는지 확인해야 합니다 다음 섹션의 단계를 따릅니다.

**SPN 설정**

SPN을 설정하려면 명령 프롬프트에서 도메인 관리자로 아래 명령을 실행합니다.

```
setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
```

이 명령은 _demo\sp_svc_로 실행 중인 SharePoint 서비스 계정에 대한 SPN을 설정합니다.

_http/sharepoint.demo.o365identity.us_를 서버에 대한 SPN으로, _demo\sp_svc_를 사용자 환경의 서비스 계정으로 바꿉니다. setspn 명령으로 추가하기 전에 SPN을 검색합니다. 이 경우 **중복된 SPN 값** 오류가 표시될 수 있습니다. 이 오류가 표시되면 값이 해당 서비스 계정과 연결되어 있는지 확인합니다.

-l 옵션과 함께 Setspn 명령을 실행하여 SPN이 추가되었는지 확인할 수 있습니다. Setspn 도구에 대해 자세히 알아보려면 [Setspn](https://technet.microsoft.com/en-us/library/cc731241.aspx)을 참조하세요.

**D단계: 커넥터가 SharePoint에 대한 대리자로 신뢰를 받는지 확인**

이 단계에서는 Azure AD 앱 프록시 서비스가 사용자 ID를 SharePoint 서비스에 위임할 수 있도록 KCD를 구성합니다. 앱 프록시 커넥터에서 Azure AD에서 인증된 사용자에 대한 Kerberos 티켓을 검색할 수 있도록 하여 이 작업을 수행합니다. 그러면 서버는 컨텍스트를 대상 응용 프로그램(이 경우는 SharePoint)에 전달합니다. 

KCD를 구성하려면 각 커넥터 컴퓨터에 대해 다음 단계를 반복해야 합니다.

1. 도메인 관리자로 DC에 로그인한 후 **Active Directory 사용자 및 컴퓨터**를 엽니다.
2. 커넥터가 실행 중인 컴퓨터를 찾습니다. 이 예제에서는 동일한 SharePoint 서버입니다.
3. 컴퓨터를 두 번 클릭한 후 **위임** 탭을 클릭합니다.
4. 위임 설정이 **지정된 서비스에 대한 위임용으로만 이 컴퓨터 트러스트**로 설정되어 있는지 확인하고 **모든 인증 프로토콜 사용**을 선택합니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)
  
5. 이제 서비스 계정에 대해 이전에 만든 SPN을 추가해야 합니다. **추가** 단추를 클릭한 후 **사용자 또는 컴퓨터**를 클릭하고 이전에 만든 계정을 찾습니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

 SPN을 선택할 수 있는 목록이 표시됩니다. 위에서 설정한 것을 추가해야 합니다. 
6. 항목을 선택하고 **확인**을 클릭합니다. **확인**을 다시 클릭하여 변경 내용을 저장합니다.

### <a name="part-2-enable-remote-access-to-sharepoint"></a>2부: SharePoint에 원격 액세스를 사용하도록 설정

이제 Kerberos에 대해 SharePoint를 사용하도록 설정했고 KCD를 구성했으므로 SharePoint에 대해 SSO(Single Sign-On)를 설정할 준비가 되었습니다. 그런 다음 커넥터에서 Azure AD 응용 프로그램 프록시를 통해 원격 액세스를 위한 SharePoint를 게시할 수 있습니다.

**Azure AD 앱 프록시를 통해 SharePoint 게시**

아래 단계를 수행하려면 조직의 Azure Active Directory 계정 내에서 전역 관리자 역할의 멤버여야 합니다.

1. Azure 관리 포털 https://manage.windowsazure.com에 로그인하고 Azure AD 테넌트를 찾습니다.
2. **응용 프로그램**을 클릭한 다음 **추가**를 클릭합니다.
3. **네트워크 외부에서 액세스할 수 있는 응용 프로그램 게시**를 선택합니다. 이 옵션이 표시되지 않으면 테넌트에 Azure AD Basic 또는 Premium이 설정되어 있는지 확인합니다.
4. 결과 상자에 다음과 같이 각 옵션을 입력합니다. 
 * **이름**: 원하는 임의 값(예: _SharePoint_)
 * **내부 URL**: SharePoint 사이트의 내부 URL입니다(예: https://SharePoint/). 이 예제에서는 https를 사용해야 합니다.
 * **사전 인증 방법**: _Azure Active Directory_를 선택합니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. 앱이 게시되면 **구성** 탭을 클릭합니다.
6. **헤더에서 URL 변환** 옵션으로 아래로 스크롤합니다. 기본값은 _예_입니다. 이 값을 _아니요_로 변경합니다. 

 SharePoint에서는 사이트를 조회하기 위해 _호스트 헤더_ 값을 사용하며 이 값을 토대로 링크도 생성합니다. 이렇게 하면 SharePoint에서 생성하는 모든 링크가 외부 URL을 사용하도록 올바르게 설정된 게시된 URL인지 확인할 수 있습니다. 값을 _예_로 설정하는 방법으로도 커넥터에서 요청을 백 엔드 응용 프로그램으로 전달할 수 있습니다. 그러나 이 값을 _아니요_로 설정하면 커넥터가 내부 호스트 이름을 보내지 않고 대신, 호스트 헤더를 게시된 URL로 백 엔드 응용 프로그램에 보냅니다.

 또한 SharePoint가 이 URL을 허용하도록 보장하기 위해 SharePoint 서버에서 하나 이상의 구성을 완료해야 합니다. 다음 섹션에서 이 구성을 완료합니다.

7. **내부 인증 방법**을 _Windows 통합_으로 변경합니다. Azure AD 테넌트가 온-프레미스의 UPN에 대해 클라우드의 다른 UPN을 사용하는 경우 **위임된 로그인 ID**도 업데이트해야 합니다.
8. **내부 응용 프로그램 SPN**을 위에서 설정한 값으로 설정합니다. 예를 들면 _http/sharepoint.demo.o365identity.us_입니다.
9. 이제 응용 프로그램을 대상 사용자에 할당할 수 있습니다.

응용 프로그램은 다음과 유사해야 합니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

###<a name="part-3-ensure-sharepoint-knows-about-the-external-url"></a>3부: SharePoint에서 외부 URL을 알고 있는지 확인

수행해야 하는 마지막 단계는 SharePoint가 외부 URL을 토대로 사이트를 찾을 수 있는지 확인하여 외부 URL을 기반으로 링크를 렌더링하는 것입니다. SharePoint에 대한 대체 액세스 매핑을 구성하여 이 작업을 수행합니다.

**SharePoint 사이트에 대한 대체 이름 구성**

1. **SharePoint 2013 중앙 관리** 사이트를 엽니다.
2. **시스템 설정** 아래에서 **대체 액세스 매핑 구성**을 선택합니다. 

 그러면 **대체 액세스 매핑** 상자가 열립니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. **대체 액세스 매핑 컬렉션** 옆의 드롭다운 목록에서 **대체 액세스 매핑 컬렉션 변경**을 선택합니다.
4. 사이트를 선택합니다(예: **SharePoint – 80**).

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. 내부 URL 또는 공용 URL로 게시된 URL을 추가하도록 선택할 수 있습니다. 이 예제에서는 엑스트라넷으로 **공용 URL**을 사용합니다.
6. **엑스트라넷** 경로에서 **공용 URL 편집**을 클릭한 후 이전 단계에서처럼 응용 프로그램을 게시하는 경로를 입력합니다. 예를 들면 _https://sharepoint-iddemo.msappproxy.net_입니다.

  ![AzureAD 응용 프로그램 프록시 커넥터](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. **Save**를 클릭합니다. 

 Azure AD 응용 프로그램 프록시를 통해 SharePoint 사이트를 외부에서 액세스할 수 있습니다.

##<a name="next-steps"></a>다음 단계

[온-프레미스 응용 프로그램에 보안된 원격 액세스를 제공하는 방법](active-directory-application-proxy-get-started.md)<br>
[Azure AD 응용 프로그램 프록시 커넥터 이해](application-proxy-understand-connectors.md)<br>
[Azure AD 응용 프로그램 프록시를 통해 SharePoint 2016 및 Office Online Server 게시](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)









<!--HONumber=Feb17_HO1-->


