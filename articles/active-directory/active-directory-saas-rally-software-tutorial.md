---
title: "자습서: Rally Software와 Azure Active Directory 통합| Microsoft Azure"
description: "Azure Active Directory에서 Rally Software를 사용하여 Single Sign-On, 자동화된 프로비전 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 9a653ac435198e89a527070a0174a1adaf830dc3
ms.openlocfilehash: 504a5723f025d7383dbec78cdd268b7c1b94a1e5
ms.lasthandoff: 01/05/2017


---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>자습서: Rally Software와 Azure Active Directory 통합
이 자습서는 Azure와 Rally Software의 통합을 보여 주기 위한 것입니다.  
이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* Rally Software 테넌트

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.

1. Rally Software에 응용 프로그램 통합 사용
2. Single Sign-On 구성
3. 사용자 프로비전 구성
4. 사용자 할당

![시나리오](./media/active-directory-saas-rally-software-tutorial/IC769525.png "시나리오")

## <a name="enabling-the-application-integration-for-rally-software"></a>Rally Software에 응용 프로그램 통합 사용
이 섹션에서는 Rally Software에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명합니다.

### <a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Rally Software에 응용 프로그램 통합을 사용하도록 설정하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
    ![Active Directory](./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.

3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
    ![응용 프로그램](./media/active-directory-saas-rally-software-tutorial/IC700994.png "응용 프로그램")

4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
    ![응용 프로그램 추가](./media/active-directory-saas-rally-software-tutorial/IC749321.png "응용 프로그램 추가")

5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
    ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-rally-software-tutorial/IC749322.png "갤러리에서 응용 프로그램 추가")

6. **검색 상자**에 **rally**를 입력합니다.
   
    ![응용 프로그램 갤러리](./media/active-directory-saas-rally-software-tutorial/IC769526.png "응용 프로그램 갤러리")

7. 결과 창에서 **Rally Software**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
    ![Rally Software](./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
   
## <a name="configuring-single-sign-on"></a>Single Sign-On 구성

이 섹션에서는 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 Rally Software에 인증할 수 있게 하는 방법을 간략하게 설명합니다.  
이 절차의 일부로 Rally Software로 인코딩된 인증서 파일을 만들어야 합니다.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Single Sign-On을 구성하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털의 **Rally Software** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-rally-software-tutorial/IC749323.png "Single Sign-On 구성")

2. **Rally에 대한 사용자 로그온 방법 선택** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
    ![Microsoft Azure AD Single Sign-on](./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD Single Sign-on")

3. **앱 URL 구성** 페이지의 **Rally Software 테넌트 URL** 텍스트 상자에 다음 패턴 "*https://\<tenant-name\>.rally.com*"을 사용하여 URL을 입력한 후 **다음**을 클릭합니다.
   
    ![앱 URL 구성](./media/active-directory-saas-rally-software-tutorial/IC769529.png "앱 URL 구성")

4. **Rally에서 Single Sign-On 구성** 페이지에서 메타데이터 다운로드를 클릭한 다음 컴퓨터에 메타데이터 파일을 저장합니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-rally-software-tutorial/IC769530.png "Single Sign-On 구성")

5. **Rally Software** 테넌트에 로그인합니다.

6. 위쪽의 도구 모음에서 **설정**을 클릭한 다음 **구독**을 선택합니다.
   
    ![구독](./media/active-directory-saas-rally-software-tutorial/IC769531.png "구독")

7. 오른쪽 상단에 있는 도구 모음에서 **작업** 단추를 클릭한 다음 **구독 편집**을 선택합니다.

8. **구독** 대화 상자 페이지에서 다음 단계를 수행하고 **저장 및 닫기**를 클릭합니다.
   
    ![인증](./media/active-directory-saas-rally-software-tutorial/IC769542.png "인증")
   
    1. 인증 드롭다운에서 **Rally 또는 SSO 인증** 을 선택합니다.
 
    2. Azure 클래식 포털의 **Rally Software에서 Single Sign-On 구성** 대화 상자 페이지에서 **ID 공급자 ID** 값을 복사한 다음 **ID 공급자 URL** 텍스트 상자에 붙여넣습니다.

    3. Azure 클래식 포털의 **Rally Software에서 Single Sign-On 구성** 대화 상자 페이지에서 **원격 로그아웃 URL** 값을 복사합니다.

9. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-rally-software-tutorial/IC769547.png "Single Sign-On 구성")
   
## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성

AAD 사용자가 로그인 할 수 있도록 Azure Active Directory 사용자 이름을 사용하여 Rally Software 응용 프로그램에 프로비전되어야 합니다.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>사용자 프로비저닝을 구성하려면
1. Rally Software 테넌트에 로그인합니다.

2. **설정 \> 사용자**를 클릭한 후 **+ 새 사용자 추가**를 클릭합니다.
   
    ![사용자](./media/active-directory-saas-rally-software-tutorial/IC781039.png "사용자")

3. 이름을 새 사용자 텍스트 상자에 입력한 다음 **세부 정보 추가**를 클릭합니다.

4. **사용자 만들기** 섹션에서 다음 단계를 수행합니다.
   
    ![사용자 만들기](./media/active-directory-saas-rally-software-tutorial/IC781040.png "사용자 만들기")
   
    1. **사용자 이름** 텍스트 상자에 프로비전할 Azure AD 사용자의 이름을 입력합니다.

    2. **이메일 주소** 텍스트 상자에 프로비전할 Azure AD 사용자의 이메일 주소를 입력합니다.

    3. **저장 후 닫기**를 클릭합니다.

> [!NOTE]
> 다른 Rally Software 사용자 계정 생성 도구 또는 Rally Software가 제공한 API를 사용하여 AAD 사용자 계정을 프로비전할 수 있습니다.
> 
> 

## <a name="assigning-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

### <a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Rally Software에 사용자를 할당하려면 다음 단계를 수행합니다.
1. Azure 클래식 포털에서 테스트 계정을 만듭니다.

2. **Rally Software** 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
    ![사용자 할당](./media/active-directory-saas-rally-software-tutorial/IC769548.png "사용자 할당")

3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
    ![예](./media/active-directory-saas-rally-software-tutorial/IC767830.png "예")

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.


