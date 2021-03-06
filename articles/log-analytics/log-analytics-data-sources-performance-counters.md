---
title: "Log Analytics의 성능 카운터 수집 및 분석 | Microsoft Docs"
description: "성능 카운터는 Windows 및 Linux 에이전트에서 성능을 분석하기 위해 Log Analytics에 의해 수집됩니다.  이 문서는 Windows 및 Linux 에이전트에 대한 성능 카운터 컬렉션을 구성하는 방법과, OMS 리포지토리에 저장하는 방식에 대한 자세한 내용과, OMS 포털에서 분석하는 방법을 설명합니다."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 653696779e612726ed5b75829a5c6ed2615553d7
ms.openlocfilehash: 1e4b5dac9333a9bd38f6ef89ddce22c74fed06ba


---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Log Analytics의 Windows 및 Linux 성능 데이터 원본
Windows와 Linux의 성능 카운터는 하드웨어 구성 요소, 운영 체제 및 응용 프로그램의 성능에 대한 정보를 자세히 제공합니다.  Log Analytics는 장기적인 분석 및 보고를 위한 성능 데이터 집계는 물론 거의 실시간에 가까운(NRT) 분석을 위해 빈번한 간격으로 성능 카운터를 수집할 수 있습니다.

![성능 카운터](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>성능 카운터 구성
성능 카운터는 [Log Analytics 설정의 데이터 메뉴](log-analytics-data-sources.md#configuring-data-sources)에서 구성합니다.

새 OMS 작업 영역에 대한 Windows 또는 Linux 성능 카운터를 처음으로 구성하는 경우, 몇 가지 공용 카운터를 신속하게 만드는 옵션이 제공됩니다.  각 항목은 옆에 확인란과 함께 나열됩니다.  초기에 만들 카운터를 모두 선택한 후 **Add the selected performance counters**(선택한 성능 카운터 추가)를 클릭합니다.

![Windows 성능 카운터 구성](media/log-analytics-data-sources-performance-counters/configure-windows.png)

이 절차에 따라 수집할 새 Windows 성능 카운터를 추가합니다.

1. 텍스트 상자에 *object(instance)\counter* 형식으로 카운트 이름을 입력합니다.  입력을 시작하면 일치하는 공용 카운터 목록이 나타납니다.  목록에서 카운터를 선택하거나 원하는 항목을 입력할 수 있습니다.  *object\counter*를 지정하면 특정 카운터에 대한 모든 인스턴스를 반환할 수도 있습니다. 
2. **+**를 클릭하거나 **Enter**를 눌러서 카운터를 목록에 추가합니다.
3. 카운터를 추가할 때에는 해당 **샘플 간격**에 기본적으로 10초가 사용됩니다.  수집된 성능 데이터의 저장소 요구 사항을 줄이려면 높은 값으로, 최대 1800초(30분)까지 값을 변경할 수 있습니다.
4. 카운터 추가를 완료했으면 화면 맨 위에서 **저장** 단추를 눌러서 구성을 저장합니다.

![Linux 성능 카운터 구성](media/log-analytics-data-sources-performance-counters/configure-linux.png)

이 절차에 따라 수집할 새 Linux 성능 카운터를 추가합니다.

1. 기본적으로, 모든 구성 변경은 모든 에이전트로 자동 푸시됩니다.  Linux 에이전트에서, 구성 파일은 Fluentd 데이터 수집기로 전송됩니다.  각 Linux 에이전트에서 이 파일을 수동으로 수정하려면, *Apply below configuration to my Linux machines*(아래 구성을 내 Linux 컴퓨터에 적용) 확인란 선택을 해제합니다.
2. 텍스트 상자에 *object(instance)\counter* 형식으로 카운트 이름을 입력합니다.  입력을 시작하면 일치하는 공용 카운터 목록이 나타납니다.  목록에서 카운터를 선택하거나 원하는 항목을 입력할 수 있습니다.  
3. **+**를 클릭하거나 **Enter**를 눌러서 카운터를 개체의 다른 카운터 목록에 추가합니다.
4. 개체에 대한 모든 카운터에는 동일한 **샘플 간격**이 사용됩니다.  기본값은 10초입니다.  수집된 성능 데이터의 저장소 요구 사항을 줄이려면 이 값을 최대 1800초(30분)까지 변경할 수 있습니다.
5. 카운터 추가를 완료했으면 화면 맨 위에서 **저장** 단추를 눌러서 구성을 저장합니다.

## <a name="data-collection"></a>데이터 수집
Log Analytics는 카운터가 설치된 모든 에이전트에서 지정된 모든 성능 카운터를 지정된 샘플 간격으로 수집합니다.  데이터는 집계되지 않으며 OMS 구독에서 지정한 기간 동안 모든 로그 검색 보기에서 원시 데이터를 사용할 수 있습니다.

## <a name="performance-record-properties"></a>성능 레코드 속성
성능 레코드에는 **Perf** 라는 type과 다음 테이블의 속성이 포함됩니다.

| 속성 | 설명 |
|:--- |:--- |
| 컴퓨터 |이벤트가 수집된 컴퓨터입니다. |
| CounterName |성능 카운터의 이름입니다. |
| CounterPath |카운터의 전체 경로이며 형식은 \\\\\<Computer>\\object(instance)\\counter입니다. |
| CounterValue |카운터의 숫자 값입니다. |
| InstanceName |이벤트 인스턴스의 이름입니다.  인스턴스가 없으면 비어 있게 됩니다. |
| ObjectName |성능 개체의 이름입니다. |
| SourceSystem |데이터가 수집된 에이전트의 유형입니다. <br> OpsManager – Windows 에이전트, 직접 연결 또는 SCOM <br> Linux – 모든 Linux 에이전트  <br> AzureStorage – Azure 진단 |
| TimeGenerated |데이터가 샘플링된 날짜와 시간입니다. |

## <a name="sizing-estimates"></a>예상 크기 조정
 10초 간격으로 특정 카운터가 수집되는 양은 인스턴스당 일별 약 1MB입니다.  다음 수식을 사용하여 특정 카운터의 저장소 요구 사항을 예측할 수 있습니다.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>성능 레코드를 통한 로그 검색
다음 테이블에서는 성능 레코드를 검색하는 로그 검색의 다양한 예제를 제공합니다.

| 쿼리 | 설명 |
|:--- |:--- |
| Type=Perf |모든 성능 데이터 |
| Type=Perf Computer="MyComputer" |특정 컴퓨터의 모든 성능 데이터 |
| Type=Perf CounterName="Current Disk Queue Length" |특정 컴퓨터에 대한 모든 성능 데이터 |
| Type=Perf (ObjectName=Processor) CounterName="% Processor Time" InstanceName=_Total &#124; measure Avg(Average) as AVGCPU by Computer |모든 컴퓨터의 평균 CPU 사용률 |
| Type=Perf (CounterName="% Processor Time") &#124;  measure max(Max) by Computer |모든 컴퓨터의 최대 CPU 사용률 |
| Type=Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" &#124; measure Avg(Average) by InstanceName |지정된 컴퓨터의 모든 인스턴스의 평균 현재 디스크 큐 길이 |
| Type=Perf CounterName="DiskTransfers/sec" &#124; measure percentile95(Average) by Computer |모든 컴퓨터에 대한 디스크 전송/초의&95; 백분위수 |
| Type=Perf CounterName="% Processor Time" InstanceName="_Total"  &#124; measure avg(CounterValue) by Computer Interval 1HOUR |모든 컴퓨터에서 시간별 평균 CPU 사용량 |
| Type=Perf Computer="MyComputer" CounterName=%* InstanceName=_Total &#124; measure percentile70(CounterValue) by CounterName Interval 1HOUR |특정 컴퓨터에 대한 % 백분율 카운터당 시간별 70백분위수 |
| Type=Perf CounterName="% Processor Time" InstanceName="_Total"  (Computer="MyComputer") &#124; measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR |특정 컴퓨터의 시간별 평균, 최소, 최대, 75백분위수 CPU 사용량 |

## <a name="viewing-performance-data"></a>성능 데이터 보기
성능 데이터를 위해 로그 검색을 실행하면 **로그** 보기가 기본적으로 표시됩니다.  데이터를 그래픽 양식으로 보려면 **메트릭**을 클릭합니다.  자세한 그래픽 보기를 보려면 카운터 옆의 **+**을 클릭하십시오.  

![축소된 메트릭 보기](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

선택한 시간 범위가 6시간 이하인 경우 그래프가 수초마다 업데이트됩니다.  라이브 데이터가 그래프 오른쪽에 옅은 파란색으로 표시됩니다.

![라이브 데이터로 확장된 메트릭 보기](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

로그 검색의 성능 데이터를 집계하려면 [OMS에서 온디맨드 메트릭 집계 및 가상화](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/)를 참조하십시오.

## <a name="next-steps"></a>다음 단계
* 데이터 원본 및 솔루션에서 수집한 데이터를 분석하기 위해 [로그 검색](log-analytics-log-searches.md) 에 대해 알아봅니다.  
* 추가적인 시각화 및 분석을 위해, 수집된 데이터를 [Power BI](log-analytics-powerbi.md) 로 내보냅니다.




<!--HONumber=Jan17_HO4-->


