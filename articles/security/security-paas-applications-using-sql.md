---
title: "SQL Database 및 SQL Data Warehouse를 사용하여 PaaS 웹 및 모바일 응용 프로그램 보안 | Microsoft Docs"
description: " PaaS 웹 및 모바일 응용 프로그램 보안을 위한 Azure SQL Database 및 SQL Data Warehouse 보안 모범 사례에 대해 자세히 알아봅니다.. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: b3db1281bbd37fcfbec54bdeb998e6338bb08369
ms.openlocfilehash: 39f841c0f3c8bed1bd640dfd644a58a764422aff


---
# <a name="securing-paas-web-and-mobile-applications-using-sql-database-and-sql-data-warehouse"></a>SQL Database 및 SQL Data Warehouse를 사용하여 PaaS 웹 및 모바일 응용 프로그램 보안

이 문서에서는 PaaS 웹 및 모바일 응용 프로그램 보안을 위한 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 및 [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) 보안 모범 사례에 대해 설명합니다. 이러한 모범 사례는 Azure에 대한 Microsoft와 고객의 경험에서 비롯된 것입니다.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database 및 SQL Data Warehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) 및 [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)는 인터넷 기반 응용 프로그램에 관계형 데이터베이스 서비스를 제공합니다. PaaS 배포에서 Azure SQL Database 및 SQL Data Warehouse를 사용할 때 응용 프로그램과 데이터를 보호하는 데 도움이 되는 서비스를 살펴보겠습니다.

- Azure Active Directory 인증(SQL Server 인증 대신)
- Azure SQL 방화벽
- TDE(투명한 데이터 암호화)

## <a name="best-practices"></a>모범 사례

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>중앙 집중식 ID 리포지토리를 사용하여 인증 및 권한 부여

Azure SQL Database는 다음 두 가지 인증 유형 중 하나를 사용하도록 구성할 수 있습니다.

- **SQL 인증**은 사용자 이름과 암호를 사용합니다. 데이터베이스의 논리 서버를 만들 때 사용자 이름 및 암호를 사용하여 "서버 관리자" 로그인을 지정했습니다. 이러한 자격 증명을 사용하면 해당 서버의 모든 데이터베이스에 대해 데이터베이스 소유자로 인증 할 수 있습니다.

- **Azure Active Directory 인증**은 Azure Active Directory에서 관리하는 ID를 사용하며, 관리되는 도메인과 통합된 도메인에서 지원됩니다. Azure Active Directory 인증을 사용하려면 Azure AD 사용자와 그룹을 관리할 수 있는 "Azure AD 관리자"라는 다른 서버 관리자를 만들어야 합니다. 이 관리자는 일반 서버 관리자가 할 수 있는 모든 작업을 수행할 수도 있습니다.

[Azure Active Directory 인증](../active-directory/develop/active-directory-authentication-scenarios.md)은 Azure AD(Azure Active Directory)의 ID를 사용하여 Azure SQL Database 및 SQL Data Warehouse에 연결하는 메커니즘입니다. Azure AD는 SQL Server 인증에 대한 대안을 제공하므로 데이터베이스 서버에서 사용자 ID의 확산을 중지할 수 있습니다. Azure AD 인증을 사용하면 데이터베이스 사용자 및 다른 Microsoft 서비스의 ID를 한 곳에서 집중적으로 관리할 수 있습니다. 중앙 ID 관리는 데이터베이스 사용자 관리를 위한 단일 위치를 제공하며 권한 관리를 간소화합니다.  

SQL 인증 대신 Azure AD 인증을 사용하는 이점은 다음과 같습니다.

- 한 곳에서 암호를 회전할 수 있습니다.
- 외부 Azure AD 그룹을 사용하여 데이터베이스 권한을 관리합니다.
- Azure AD에서 지원하는 통합 Windows 인증 및 다른 인증 유형을 사용하여 저장 중인 암호를 제거합니다.
- 포함된 데이터베이스 사용자를 통해 데이터베이스 수준에서 ID를 인증합니다.
- SQL Database에 연결되는 응용 프로그램에 대한 토큰 기반 인증을 지원합니다.
- 도메인 동기화 없이 로컬 Azure AD에 대해 ADFS(도메인 페더레이션) 또는 기본 사용자/암호 인증을 지원합니다.
- Azure AD는 [MFA(Multi-Factor Authentication)](../multi-factor-authentication/multi-factor-authentication.md)를 포함하는 Active Directory 유니버설 인증을 사용하는 SQL Server Management Studio를 통해 연결하도록 지원합니다. MFA는 전화 통화, 문자 메시지, 모바일 앱 알림 등의 여러 가지 간편한 검증 옵션을 제공하는 강력한 인증을 포함합니다. 자세한 내용은 [SQL 데이터베이스 및 SQL 데이터 웨어하우스를 사용한 Azure AD MFA에 대한 SSMS 지원](../sql-database/sql-database-ssms-mfa-authentication.md)을 참조하세요.

Azure AD 인증에 대한 자세한 내용은 다음을 참조하세요.

- [Azure Active Directory 인증을 사용하여 SQL 데이터베이스 또는 SQL 데이터 웨어하우스에 연결](../sql-database/sql-database-aad-authentication.md)
- [Azure SQL 데이터 웨어하우스에 대한 인증](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Azure AD 인증을 사용하는 Azure SQL DB에 대한 토큰 기반 인증 지원](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)(영문)

> [!NOTE]
> Azure Active Directory가 사용자 환경에 적합한지 확인하려면 [Azure AD 기능 및 제한 사항](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), 특히 추가 고려 사항을 참조하세요.
>
>

### <a name="restrict-access-based-on-ip-address"></a>IP 주소 기반 액세스 제한
허용 가능한 IP 주소의 범위를 지정하는 방화벽 규칙을 만들 수 있습니다. 이러한 규칙은 서버 수준과 데이터베이스 수준 모두에서 지정할 수 있습니다. 보안을 강화하고 데이터베이스의 휴대성을 높이기 위해 가능한 경우에는 항상 데이터베이스 수준 방화벽 규칙을 사용하는 것이 좋습니다. 서버 수준 방화벽 규칙은 관리자에게 가장 적합하며, 동일한 액세스를 요구하는 데이터베이스가 많을 때 각 데이터베이스를 개별적으로 구성할 필요가 없습니다.

SQL Database의 기본 원본 IP 주소를 제한하면 Azure 주소(다른 구독 및 테넌트 포함)에서 액세스할 수 있습니다. IP 주소가 인스턴스에만 액세스할 수 있도록 제한하면 됩니다. SQL 방화벽과 IP 주소 제한이 있더라도 강력한 인증이 여전히 필요합니다. 이 문서의 앞부분에서 설명한 권장 사항을 참조하세요.

Azure SQL 방화벽 및 IP 제한에 대한 자세한 내용은 다음을 참조하세요.

- [Azure SQL Database 액세스 제어](../sql-database/sql-database-control-access.md)
- [Azure SQL Database 방화벽 규칙 구성 - 개요](../sql-database/sql-database-firewall-configure.md)
- [Azure Portal을 사용하여 Azure SQL Database 서버 수준 방화벽 규칙 구성](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>미사용 데이터 암호화
[TDE(투명한 데이터 암호화)](https://msdn.microsoft.com/library/azure/bb934049)는 SQL Server, Azure SQL Database 및 Azure SQL Data Warehouse 데이터 파일을 암호화하며, 미사용 데이터 암호화라고도 합니다. 보안 시스템 설계, 중요한 자산 암호화 및 데이터베이스 서버 방화벽 구축과 같은 데이터베이스 보호에 도움이 되는 몇 가지 예방 조치를 취할 수 있습니다. 그러나 물리적 미디어(예: 드라이브 또는 백업 테이프)가 도난되는 시나리오에서는 악의적인 사용자가 데이터베이스를 복원하거나 연결하여 데이터를 찾아 볼 수 있습니다. 한 가지 해결 방법으로 데이터베이스의 중요한 데이터를 암호화하고 인증서로 데이터를 암호화하는 데 사용되는 키를 보호합니다. 이렇게 하면 키가 없는 사람이 데이터를 사용할 수 없게 되지만 이러한 종류의 보호는 사전에 계획해야 합니다.

TDE는 데이터와 로그 파일을 의미하는 미사용 데이터를 보호합니다. 다양한 산업 분야에서 제정된 많은 법률, 규정 및 지침을 준수하는 기능을 제공합니다. 이를 통해 소프트웨어 개발자는 기존 응용 프로그램을 변경하지 않고도 업계 표준 암호화 알고리즘을 사용하여 데이터를 암호화할 수 있습니다.

규정에서 명시적으로 이러한 암호화를 지정하는 경우 TDE를 사용해야 합니다. 하지만 이렇게 하더라도 정상적인 액세스 경로를 사용하는 공격자는 막을 수 없습니다. TDE는 Azure SQL에서 행과 열에 대해 제공하는 암호화 또는 응용 프로그램 수준 암호화를 통해 추가 응용 프로그램 수준 암호화를 사용해야 할 가능성이 매우 낮은 경우를 방지하는 데 사용됩니다.

선택적 데이터에는 응용 프로그램 수준 암호화도 사용해야 합니다. 데이터 독립성 문제는 올바른 국가에서 유지되는 키로 데이터를 암호화함으로써 완화될 수 있습니다. 이렇게 하면 강한 알고리즘(예: AES 256)을 사용한다고 가정하여 키를 사용하지 않고는 데이터를 해독할 수 없으므로 실수로 인한 데이터 전송에 문제가 발생하지 않도록 방지할 수 있습니다.

권한이 있는([RBAC](../active-directory/role-based-access-built-in-roles.md)) 사용자만 액세스할 수 있도록 행과 열에 대해 Azure SQL에서 제공하는 암호화를 수행하여 권한이 낮은 사용자가 열이나 행을 볼 수 없도록 합니다.

## <a name="next-steps"></a>다음 단계
이 문서에서는 PaaS 웹 및 모바일 응용 프로그램 보안을 위한 Azure SQL Database 및 SQL Data Warehouse 보안 모범 사례 모음을 소개했습니다. PaaS 배포 보안 유지에 대한 자세한 내용은 다음을 참조하세요.

- [PaaS 배포 보안](security-paas-deployments.md)
- [Azure App Services를 사용하여 PaaS 웹 및 모바일 응용 프로그램 보안](security-paas-applications-using-app-services.md)



<!--HONumber=Feb17_HO1-->


