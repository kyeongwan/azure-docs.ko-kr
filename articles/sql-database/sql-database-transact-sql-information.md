---
title: "Azure SQL Database T-SQL에서 지원되지 않는 기능 | Microsoft Docs"
description: "Azure SQL 데이터베이스에서 완전히 지원되지 않는 TRANSACT-SQL 문"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 3f9077733725174f1eed61d37d544e4f36822f6e
ms.openlocfilehash: d935571ccd18bc15baa000fb8c07fed11b66ba6c


---
# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL 데이터베이스 TRANSACT-SQL의 차이점   
응용 프로그램에서 의존하는 TRANSACT-SQL 기능은 대부분 Microsoft SQL Server와 Azure SQL 데이터베이스 모두에서 지원됩니다. 예를 들어, 데이터 형식, 연산자, 문자열, 산술, 논리 및 커서 함수 등과 같은 핵심 SQL 구성 요소는 SQL Server에서 그대로 작동합니다.

## <a name="why-some-transact-sql-is-not-supported"></a>일부 Transact-SQL이 지원되지 않는 이유
Azure SQL Database는 마스터 데이터베이스 및 운영 체제에 대한 종속성으로부터 기능을 격리하도록 만들어졌습니다. 따라서 많은 서버 수준 작업이 SQL Database에 적합하지 않습니다. Transact-SQL 문은 일반적으로 서버 수준 옵션 또는 운영 체제 구성 요소를 구성하거나 파일 시스템 구성을 지정하는 경우에 사용할 수 없습니다. 사용자 데이터베이스의 외부에 있는 기능이 필요한 경우 SQL Database 또는 다른 Azure 기능이나 서비스에서 다른 방법을 사용 가능하다는 점이 적절한 대안입니다. 

예를 들어, Always On은 활성 지역 복제로 대체됩니다. 이런 이유로 가용성 그룹과 관련된 Transact-SQL 문은 SQL Database에서 지원되지 않으며 Always On에 관련된 동적 관리 뷰도 지원되지 않습니다.  

SQL Database에서 지원되는 기능 및 지원되지 않는 기능 목록은 [Azure SQL Database 고려 사항, 지침 및 기능](sql-database-features.md)을 참조하세요.

SQL Server에서 사용되지 않는 구문은 일반적으로 SQL Database에서 지원되지 않습니다.

## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>SQL Database에서 부분적으로 지원되는 Transact-SQL 구문
SQL Database에서는 해당 SQL Server 2016 Transact-SQL 문에 있는 인수가 일부는 지원되고 일부는 지원되지 않습니다. 예를 들어 `CREATE PROCEDURE` 문은 사용할 수 있지만 `CREATE PROCEDURE`의 모든 옵션은 사용할 수 없습니다. 여기에서 전체 구문을 설명하는 것은 복잡하고 중복됩니다. 각 문의 지원되는 영역에 대한 자세한 내용은 연결된 구문 항목을 참조하세요.

- 데이터베이스: [만들기](https://msdn.microsoft.com/library/dn268335.aspx)/[데이터베이스 변경](https://msdn.microsoft.com/library/ms174269.aspx)   
- 함수: [만들기](https://msdn.microsoft.com/library/ms186755.aspx)/[함수 변경](https://msdn.microsoft.com/library/ms186967.aspx)   
- 로그인: [만들기](https://msdn.microsoft.com/library/ms189751.aspx)/[로그인 변경](https://msdn.microsoft.com/library/ms189828.aspx)   
- 저장 프로시저: [만들기](https://msdn.microsoft.com/library/ms187926.aspx)/[프로시저 변경](https://msdn.microsoft.com/library/ms189762.aspx)   
- 테이블: [만들기](https://msdn.microsoft.com/library/dn305849.aspx)/[테이블 변경](https://msdn.microsoft.com/library/ms190273.aspx)   
- 형식(사용자 지정): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)   
- 사용자: [만들기](https://msdn.microsoft.com/library/ms173463.aspx)/[사용자 변경](https://msdn.microsoft.com/library/ms176060.aspx)   
- 보기: [만들기](https://msdn.microsoft.com/library/ms187956.aspx)/[보기 변경](https://msdn.microsoft.com/library/ms173846.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>SQL Database에서 지원되지 않는 Transact-SQL 구문   
[Azure SQL Database 고려 사항, 지침 및 기능](sql-database-features.md)에서 설명한 지원되지 않는 기능과 관련된 Transact-SQL 문 외에도 다음 문 및 문 그룹이 지원되지 않습니다.
- 시스템 개체의 데이터 정렬
- 연결 관련: 끝점 문, `ORIGINAL_DB_NAME`. SQL Database는 Windows 인증을 지원하지 않으며 비슷한 Azure Active Directory 인증은 지원합니다. 최신 버전의 SSMS를 사용해야 하는 인증 형식도 있습니다. 자세한 내용은 [Azure Active Directory 인증을 사용하여 SQL Database 또는 SQL Data Warehouse에 연결](sql-database-aad-authentication.md)을 참조하세요.
- 세 개 또는 네 개의 부분으로 된 이름을 사용하여 데이터베이스 쿼리를 교차합니다. 읽기 전용 데이터베이스 간 쿼리는 [탄력적 데이터베이스 쿼리](sql-database-elastic-query-overview.md)를 통해 지원됩니다.
- 데이터베이스 간 소유권 체인, `TRUSTWORTHY` 설정
- `DATABASEPROPERTY` 대신 `DATABASEPROPERTYEX`를 사용합니다.
- `EXECUTE AS LOGIN` 대신 '사용자로 실행'을 사용합니다.
- 확장 가능 키 관리를 제외하고 암호화를 지원합니다.
- 이벤트: 이벤트, 이벤트 알림, 쿼리 알림
- 데이터베이스 파일 배치, 크기 및 Microsoft Azure에서 자동으로 관리되는 데이터베이스 파일과 관련된 구문입니다.
- Microsoft Azure 계정을 통해 관리되는 고가용성과 관련된 구문입니다. 백업, 복원, Alwayson, 데이터베이스 미러링, 로그 전달, 복구 모드에 대한 구문을 포함합니다.
- SQL Database에서 사용할 수 없는 로그 판독기를 사용하는 구문: 푸시 복제, 데이터 캡처 변경입니다. SQL Database는 푸시 복제 문서의 구독자가 될 수 있습니다.
- SQL Server 에이전트 또는 MSDB 데이터베이스를 사용하는 구문: 경고, 운영자, 중앙 관리 서버입니다. 대신 Azure PowerShell과 같은 스크립팅을 사용합니다.
- 함수: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- 전역 임시 테이블
- 하드웨어 관련 서버 설정과 관련된 구문: 메모리, 작업자 스레드 수, CPU 선호도, 추적 플래그 등입니다. 대신 서비스 수준을 사용합니다.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, `BULK INSERT` 및 네 부분으로 된 이름
- .NET Framework [CLR과 SQL Server 통합](http://msdn.microsoft.com/library/ms254963.aspx)
- 의미 체계 검색
- 서버 자격 증명. 대신 데이터베이스 범위 자격 증명을 사용합니다.
- 서버 수준 항목: 서버 역할, `IS_SRVROLEMEMBER`, `sys.login_token`입니다. 서버 수준 사용 권한의 `GRANT`, `REVOKE`, `DENY`은 사용할 수 없지만 일부는 데이터베이스 수준 사용 권한으로 대체됩니다. 몇 가지 유용한 서버 수준 DMV에는 동등한 데이터베이스 수준 DMV가 있습니다.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` 옵션 및 `RECONFIGURE`입니다. [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx)을 사용하면 일부 옵션은 사용 가능합니다.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server 감사. 대신 SQL Database 감사를 사용합니다.
- SQL Server 추적
- 추적 플래그. 일부 추적 플래그 항목은 호환 모드로 이동되었습니다.
- TRANSACT-SQL 디버깅
- 트리거: 서버 범위 또는 로그온 트리거
- `USE` 문: 데이터베이스 컨텍스트를 다른 데이터베이스로 변경하려면 새 데이터베이스에 대한 새 연결을 설정해야 합니다.

## <a name="full-transact-sql-reference"></a>전체 TRANSACT-SQL 참조
TRANSACT-SQL 문법, 사용법 및 예제에 대한 자세한 내용은 SQL Server 온라인 설명서의 [TRANSACT-SQL 참조(데이터베이스 엔진)](https://msdn.microsoft.com/library/bb510741.aspx) 를 참조하세요. 

### <a name="about-the-applies-to-tags"></a>'적용 대상' 태그 정보
Transact-SQL 참조에는 현재까지 게시된 SQL Server 버전 2008과 관련된 항목이 포함되어 있습니다. 항목 제목 밑에 있는 아이콘 모음에는 SQL Server 플랫폼 4개와 적용 가능 여부가 나타납니다. 예를 들어 가용성 그룹은 SQL Server 2012에서 도입되었습니다. [CREATE AVAILABILITY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) 항목에는 이 문이 **SQL Server 2012**부터 적용된다고 나타납니다. SQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse 또는 병렬 데이터 웨어하우스에는 이 문이 적용되지 않습니다.

경우에 따라 항목의 일반 제목이 제품에 사용될 수 있지만 제품 간에 약간의 차이가 있습니다. 차이점은 항목의 중간점에 적절히 표시됩니다.



<!--HONumber=Nov16_HO5-->


