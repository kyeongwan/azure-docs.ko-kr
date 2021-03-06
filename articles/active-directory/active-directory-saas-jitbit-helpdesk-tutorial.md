---
title: "자습서: Jitbit Helpdesk와 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory에서 Jitbit Helpdesk를 사용하여 Single Sign-On, 자동화된 프로비저닝 등을 사용하도록 설정하는 방법을 알아봅니다."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: e8dfd2e943143c88406c88d85fea6916be61a7ec
ms.openlocfilehash: 0fbadc98022f82782a39c61f8ce574cf30b1fbaa
ms.lasthandoff: 02/17/2017


---

# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>자습서: Jitbit Helpdesk와 Azure Active Directory 통합
이 자습서는 Azure와 Jitbit Helpdesk의 통합을 보여 주기 위한 것입니다.  

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 항목이 있다고 가정합니다.

* 유효한 Azure 구독
* Jitbit Helpdesk 테넌트

이 자습서를 완료하면 Jitbit Helpdesk에 할당한 Azure AD 사용자가 Jitbit Helpdesk 회사 사이트(서비스 공급자가 제공한 로그온)에서 또는 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 사용하여 응용 프로그램에 Single Sign-On으로 로그인할 수 있습니다.

이 자습서에 설명된 시나리오는 다음 구성 요소로 이루어져 있습니다.

* Jitbit Helpdesk에 응용 프로그램 통합 사용
* Single Sign-On 구성
* 사용자 프로비전 구성
* 사용자 할당

![시나리오](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "시나리오")

## <a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Jitbit Helpdesk에 응용 프로그램 통합 사용
이 섹션에서는 Jitbit Helpdesk에 응용 프로그램 통합을 사용하도록 설정하는 방법을 간략하게 설명합니다.

### <a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>Jitbit Helpdesk에 응용 프로그램 통합을 사용하도록 설정하려면 다음을 수행합니다.
1. Azure 클래식 포털의 왼쪽 탐색 창에서 **Active Directory**를 클릭합니다.
   
   ![Active Directory](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")
2. **디렉터리** 목록에서 디렉터리 통합을 사용하도록 설정할 디렉터리를 선택합니다.
3. 응용 프로그램 보기를 열려면 디렉터리 보기의 최상위 메뉴에서 **응용 프로그램** 을 클릭합니다.
   
   ![응용 프로그램](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "응용 프로그램")
4. 페이지 맨 아래에 있는 **추가** 를 클릭합니다.
   
   ![응용 프로그램 추가](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "응용 프로그램 추가")
5. **수행할 작업** 대화 상자에서 **갤러리에서 응용 프로그램 추가**를 클릭합니다.
   
   ![갤러리에서 응용 프로그램 추가](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "갤러리에서 응용 프로그램 추가")
6. **검색 상자**에 **Jitbit Helpdesk**를 입력합니다.
   
   ![응용 프로그램 갤러리](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "응용 프로그램 갤러리")
7. 결과 창에서 **Jitbit Helpdesk**를 선택하고 **완료**를 클릭하여 응용 프로그램을 추가합니다.
   
   ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
   
## <a name="configure-single-sign-on"></a>Single Sign-On 구성

이 섹션에서는 사용자가 SAML 프로토콜 기반 페더레이션을 사용하여 Azure AD의 계정으로 Jitbit Helpdesk에 인증할 수 있게 하는 방법을 간략하게 설명합니다. 에서도 확인할 수 있습니다.  

이 절차의 일부로 base-64로 인코딩된 인증서 파일을 만들어야 합니다. 이 절차를 잘 모르는 경우 [이진 인증서를 텍스트 파일로 변환하는 방법](http://youtu.be/PlgrzUZ-Y1o)을 참조하십시오.

>[!IMPORTANT]
>Jitbit Helpdesk 테넌트에 대한 Single Sign-On을 구성하기 위해 먼저 Jitbit Helpdesk 기술 지원에 문의하여 이 기능을 사용하도록 설정합니다. 
> 

**Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure 클래식 포털의 **Jitbit Helpdesk** 응용 프로그램 통합 페이지에서 **Single Sign-On 구성**을 클릭하여 **Single Sign-On 구성** 대화 상자를 엽니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Single Sign-On 구성")
2. **Jitbit Helpdesk에 대한 사용자 로그온 방법을 선택하세요.** 페이지에서 **Microsoft Azure AD Single Sign-On**을 선택하고 **다음**을 클릭합니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Single Sign-On 구성")
3. **앱 URL 구성** 페이지의 **Jitbit Helpdesk 로그인 URL** 텍스트 상자에 "*https://\<tenant-name\>.Jitbit.com*" 패턴을 사용하여 URL을 입력한 후 **다음**을 클릭합니다.
   
   ![앱 URL 구성](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "앱 URL 구성")
4. **Jitbit Helpdesk에서 Single Sign-On 구성** 페이지에서 인증서를 다운로드하려면 **인증서 다운로드**를 클릭한 다음 **c:\\Jitbit Helpdesk.cer**에 로컬로 인증서 파일을 저장합니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Single Sign-On 구성")
5. 다른 웹 브라우저 창에서 Jitbit Helpdesk 회사 사이트에 관리자로 로그인합니다.
6. 위쪽의 도구 모음에서 **관리**를 클릭합니다.
   
   ![관리](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "관리")
7. **일반 설정**을 클릭합니다.
   
   ![사용자, 회사 및 권한](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "사용자, 회사 및 권한")
8. **인증 설정** 구성 섹션에서 다음 단계를 수행합니다.
   
   ![인증 설정](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "인증 설정")
   
   1. **OneLogin**과 함께 SSO(Single Sign-On)을 사용하여 **SAML 2.0 Single Sign On 사용** 로그인을 선택합니다.
   2. Azure 클래식 포털의 **Jitbit Helpdesk에서 Single Sign-On 구성** 대화 상자 페이지에서 **서비스 공급자(SP) 제공 끝점** 값을 복사한 다음 **끝점 URL** 텍스트 상자에 붙여넣습니다.
   3. 다운로드한 인증서에서 **Base-64로 인코딩된** 파일을 만듭니다. 
      >[!TIP]
      >자세한 내용은 [이진 인증서를 텍스트 파일로 변환하는 방법](http://youtu.be/PlgrzUZ-Y1o) 
      > 
   4. Base&64;로 인코딩된 인증서를 열고, 내용을 클립보드에 복사한 다음 전체 인증서를 **X.509 인증서** 텍스트 상자에 붙여넣습니다.
   5. **변경 내용 저장**을 클릭합니다.
9. Azure 클래식 포털에서 Single Sign-On 구성 확인을 선택하고 **완료**를 클릭하여 **Single Sign-On 구성** 대화 상자를 닫습니다.
   
   ![Single Sign-On 구성](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Single Sign-On 구성")
   
## <a name="configuring-user-provisioning"></a>사용자 프로비전 구성

Azure AD 사용자가 Jitbit Helpdesk에 로그인할 수 있도록 하려면 Jitbit Helpdesk로 프로비저닝되어야 합니다.  Jitbit Helpdesk의 경우 프로비저닝은 수동 작업입니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. **Jitbit Helpdesk** 테넌트에 로그인합니다.
2. 위쪽의 메뉴에서 **관리**를 클릭합니다.
   
   ![관리](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "관리")
3. **사용자, 회사 및 사용 권한**을 클릭합니다.
   
   ![사용자, 회사 및 권한](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "사용자, 회사 및 권한")
4. **사용자 추가**를 클릭합니다.
   
   ![사용자 추가](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "사용자 추가")
5. 만들기 섹션에서 프로비저닝하려는 Azure AD 계정의 데이터를 **사용자 이름**, **전자 메일**, **이름**, **성** 텍스트 상자에 입력합니다.
   
   ![만들기](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "만들기")
6. **만들기**를 클릭합니다.

>[!NOTE]
>다른 Jitbit Helpdesk 사용자 계정 생성 도구 또는 Jitbit Helpdesk가 제공한 API를 사용하여 AAD 사용자 계정을 프로비저닝할 수 있습니다.
> 

## <a name="assign-users"></a>사용자 할당
구성을 테스트하려면 응용 프로그램 사용을 허용하려는 Azure AD 사용자를 할당하여 액세스 권한을 부여해야 합니다.

**Jitbit Helpdesk에 사용자를 할당하려면 다음 단계를 수행합니다.**

1. Azure 클래식 포털에서 테스트 계정을 만듭니다.
2. **Jitbit Helpdesk** 응용 프로그램 통합 페이지에서 **사용자 할당**을 클릭합니다.
   
   ![사용자 할당](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "사용자 할당")
3. 테스트 사용자를 선택하고 **할당**을 클릭한 다음 **예**를 클릭하여 할당을 확인합니다.
   
   ![예](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "예")

Single Sign-On 설정을 테스트하려면 액세스 패널을 엽니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.


