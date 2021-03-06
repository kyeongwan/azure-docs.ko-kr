---
title: "Azure Log Analytics로 변경 추적 | Microsoft Docs"
description: "Log Analytics에서 변경 내용 추적 솔루션을 사용하여 사용자 환경에서 발생하는 소프트웨어 및 Windows Services 변경 내용을 식별할 수 있습니다."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: 3953a83b20ee2d1ca0035b31824ca167e92f4864
ms.lasthandoff: 02/28/2017


---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>변경 내용 추적 솔루션으로 사용자 환경에서 소프트웨어 변경 추적

이 문서를 통해 Log Analytics에서 변경 내용 추적 솔루션을 사용하여 사용자 환경의 변경 내용을 쉽게 식별할 수 있습니다. 이 솔루션은 Windows 및 Linux 소프트웨어, Windows 파일, Windows 서비스 및 Linux 데몬의 변경 내용을 추적합니다. 구성 변경 내용을 식별하면 운영 문제를 쉽게 특정할 수 있습니다.

설치된 에이전트의 유형을 업데이트하려면 이 솔루션을 설치합니다. 모니터링되는 서버에서 설치된 소프트웨어, Windows 서비스 및 Linux 데몬에 대한 변경 내용을 읽은 다음, 해당 데이터는 처리를 위해 클라우드의 Log Analytics 서비스로 보내집니다. 논리는 수신된 데이터에 적용되며 클라우드 서비스는 데이터를 기록합니다. 변경 내용 추적 대시보드의 정보를 사용하여 서버 인프라에서 수행한 변경 내용을 쉽게 확인할 수 있습니다.

## <a name="installing-and-configuring-the-solution"></a>솔루션 설치 및 구성
다음 정보를 사용하여 솔루션을 설치하고 구성합니다.

* 변경 내용을 모니터링할 각 컴퓨터에 [Windows](log-analytics-windows-agents.md), [Operations Manager](log-analytics-om-agents.md) 또는 [Linux](log-analytics-linux-agents.md) 에이전트가 있어야 합니다.
* [솔루션 갤러리에서 Log Analytics 솔루션 추가](log-analytics-add-solutions.md)에 설명된 프로세스를 사용하여 OMS 작업 영역에 변경 내용 추적 솔루션을 추가합니다.  추가 구성은 필요 없습니다.

### <a name="configure-windows-files-to-track"></a>추적할 Windows 파일 구성
다음 단계를 사용하여 Windows 컴퓨터에서 추적할 파일을 구성합니다.

1. OMS 포털에서 **설정**을 클릭합니다(기어 기호).
2. **설정** 페이지에서 **데이터**를 클릭한 다음 **Windows 파일 추적**을 클릭합니다.
3. Windows 파일 변경 내용 추적에서 추적하려는 파일의 파일 이름을 포함한 전체 경로를 입력한 다음 **추가** 기호를 클릭합니다. 예: C:\Program Files (x86)\Internet Explorer\iexplore.exe or C:\Windows\System32\drivers\etc\hosts
4. **Save**를 클릭합니다.  
   ![Windows 파일 변경 내용 추적](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="limitations"></a>제한 사항
현재 변경 내용 추적 솔루션은 다음 항목을 지원하지 않습니다.

* 폴더(디렉터리)
* 재귀
* 와일드 카드
* 경로 변수
* 네트워크 파일 시스템

기타 제한 사항은 다음과 같습니다.

* **최대 파일 크기** 열과 값은 현재 구현에서 사용되지 않습니다.
* 30분 수집 주기에서 2500개 이상의 파일을 수집하는 경우 솔루션 성능이 저하될 수 있습니다.
* 네트워크 트래픽이 많을 경우 변경 레코드를 표시하는 데 최대&6;시간이 걸릴 수 있습니다.
* 컴퓨터를 종료하는 동안 구성을 수정하면 컴퓨터는 이전 구성에 속한 파일 변경 내용을 게시할 수 있습니다.

## <a name="change-tracking-data-collection-details"></a>변경 내용 추적 데이터 수집 정보
변경 내용 추적 기능은 설정된 에이전트를 사용하여 소프트웨어 인벤토리 및 Windows 서비스 메타데이터를 수집합니다.

다음 표에서는 데이터 수집 방법 및 변경 내용 추적에 대해 데이터가 수집되는 방식에 대한 기타 세부 정보를 보여 줍니다.

| 플랫폼 | 직접 에이전트 | SCOM 에이전트 | Linux 에이전트 | Azure 저장소 | SCOM 필요? | 관리 그룹을 통해 전송되는 SCOM 에이전트 데이터 | 수집 빈도 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows 및 Linux |![예](./media/log-analytics-change-tracking/oms-bullet-green.png) |![예](./media/log-analytics-change-tracking/oms-bullet-green.png) |![예](./media/log-analytics-change-tracking/oms-bullet-green.png) |![아니요](./media/log-analytics-change-tracking/oms-bullet-red.png) |![아니요](./media/log-analytics-change-tracking/oms-bullet-red.png) |![예](./media/log-analytics-change-tracking/oms-bullet-green.png) | 변경 유형에 따라 15분부터 1시간 |

## <a name="use-change-tracking"></a>변경 내용 추적 사용
솔루션을 설치한 후, OMS의 **개요** 페이지에 있는 **변경 내용 추적** 타일을 사용하여 모니터링되는 서버에 대한 변경 내용 요약을 볼 수 있습니다.

![변경 내용 추적 타일의 이미지](./media/log-analytics-change-tracking/change-tracking-tile.png)

인프라 에 대한 변경 내용을 본 후 다음 범주에 대한 세부 정보를 드릴인투할 수 있습니다.

* 소프트웨어 및 Windows 서비스에 대한 구성 유형별 변경 내용
* 개별 서버에 대한 응용 프로그램 및 업데이트에 대한 소프트웨어 변경 내용
* 각 응용 프로그램에 대한 전체 소프트웨어 변경 내용 수
* Linux 패키지
* 개별 서버에 대한 Windows 서비스 변경 내용
* Linux 데몬 변경 내용

![변경 내용 추적 대시보드의 이미지](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![변경 내용 추적 대시보드의 이미지](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>변경 내용 유형의 변경 내용을 보려면
1. **개요** 페이지에서 **변경 내용 추적** 타일을 클릭합니다.
2. **변경 내용 추적** 대시보드에서, 변경 유형 블레이드 중 하나에서 요약 정보를 검토한 다음 하나를 클릭하여 **로그 검색** 페이지에서 항목에 대한 세부 정보를 봅니다.
3. 로그 검색 페이지에서, 시간별 결과, 자세한 결과 및 로그 검색 기록을 볼 수 있습니다. 패싯으로 필터링하여 결과 범위를 좁힐 수 있습니다.

## <a name="next-steps"></a>다음 단계
* [Log Analytics에서 로그 검색](log-analytics-log-searches.md) 을 사용하여 자세한 변경 내용 추적 데이터를 볼 수 있습니다.

