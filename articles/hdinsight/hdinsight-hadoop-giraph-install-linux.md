---
title: "Linux 기반 HDInsight(Hadoop)에서 Giraph 설치 및 사용 | Microsoft 문서"
description: "스크립트 작업을 사용하여 Linux 기반 HDInsight 클러스터에 Giraph를 설치하는 방법에 대해 알아봅니다. 스크립트 작업을 사용하면 클러스터 구성을 변경하거나 서비스 및 유틸리티를 설치하여 생성 중 클러스터를 사용자 지정할 수 있습니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5ec4b964066687b506686709c3dc5ed5b402fbaf
ms.openlocfilehash: 699626f24e6f66f4b149feda5eb292e64d4b6647


---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>HDInsight Hadoop 클러스터에 Giraph를 설치하고 Giraph를 사용하여 대규모 그래프를 처리합니다.

클러스터를 사용자 지정하는 **스크립트 작업** 을 사용하여 Azure HDInsight의 Hadoop에서 모든 유형의 클러스터에 Giraph를 설치할 수 있습니다.

이 항목에서는 스크립트 동작을 사용하여 Giraph를 설치하는 방법을 알아봅니다. Giraph를 설치한 후 대규모 그래프를 처리하기 위해 가장 일반적인 응용 프로그램에 Giraph를 사용하는 방법도 알아보겠습니다.

> [!IMPORTANT]
> 이 문서의 단계에는 Linux를 사용하는 HDInsight 클러스터가 필요합니다. Linux는 HDInsight 버전 3.4 이상에서 사용되는 유일한 운영 체제입니다. 자세한 내용은 [Windows에서 HDInsight 사용 중단](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)을 참조하세요.

## <a name="a-namewhatisawhat-is-giraph"></a><a name="whatis"></a>Giraph란?

[Apache Giraph](http://giraph.apache.org/) 를 통해 Hadoop을 사용하여 그래프 처리를 수행할 수 있으며, Azure HDInsight에서 이를 사용할 수도 있습니다. 그래프는 인터넷과 같은 대규모 네트워크의 라우터 간 연결, 소셜 네트워크(또는 소셜 그래프)상의 사람들 간 관계 등 개체 간의 관계를 모델링합니다. 그래프 처리를 통해 그래프의 개체 간 관계를 추론하여 다음을 수행할 수 있습니다.

* 현재 관계를 기반으로 하여 잠재적인 친구 파악.

* 네트워크의 두 컴퓨터 간에 가장 짧은 경로 파악.

* 웹 페이지의 페이지 순위 계산.

> [!WARNING]
> HDInsight 클러스터와 함께 제공되는 구성 요소는 완벽하게 지원되며 Microsoft 지원은 이러한 구성 요소와 관련된 문제를 격리하고 해결하는 데 도움이 됩니다.
> 
> Giraph와 같은 사용자 지정 구성 요소는 문제 해결에 도움이 되는 합리적인 지원을 받습니다. 지원을 통해 문제를 해결하거나 해당 기술에 대한 전문 지식이 있는, 오픈 소스 기술에 대해 사용 가능한 채널에 참여하도록 요구할 수 있습니다. 예를 들어 [HDInsight에 대한 MSDN 포럼](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com)과 같은 여러 커뮤니티 사이트를 사용할 수 있습니다. Apache 프로젝트는 [http://apache.org](http://apache.org)에 프로젝트 사이트가 있습니다(예: [Hadoop](http://hadoop.apache.org/)).


## <a name="what-the-script-does"></a>스크립트가 수행하는 작업

이 스크립트는 다음 작업을 수행합니다.

* Giraph를 `/usr/hdp/current/giraph`에 설치합니다.

* 클러스터에 대한 기본 저장소(WASB)에 `giraph-examples.jar` 파일을 복사합니다. `/example/jars/giraph-examples.jar`

## <a name="a-nameinstallainstall-giraph-using-script-actions"></a><a name="install"></a>스크립트 동작을 사용하여 Giraph 설치

HDInsight 클러스터에서 Giraph를 설치하는 샘플 스크립트는 다음 위치에서 사용할 수 있습니다.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

이 섹션에서는 Azure 포털을 사용하여 클러스터를 만들면서 샘플 스크립트를 사용하는 방법에 대한 지침을 제공합니다. 

> [!NOTE]
> Azure PowerShell, Azure CLI, HDInsight .NET SDK 또는 Azure Resource Manager 템플릿을 스크립트 동작을 적용하는 데 사용할 수도 있습니다. 이미 실행 중인 클러스터에도 스크립트 동작을 적용할 수 있습니다. 자세한 내용은 [스크립트 작업을 사용하여 HDInsight 클러스터 사용자 지정](hdinsight-hadoop-customize-cluster-linux.md)을 참조하세요.

1. [Linux 기반 HDInsight 클러스터 만들기](hdinsight-hadoop-create-linux-clusters-portal.md)의 단계를 사용하여 클러스터를 만들기 시작하지만 완료하지 마세요.

2. **선택적 구성** 블레이드에서 **스크립트 동작**을 선택하고 아래 정보를 제공합니다.
   
   * **이름**: 스크립트 동작의 이름을 입력합니다.

   * **스크립트 URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

   * **HEAD**: 이 옵션 선택

   * **작업자**: 선택 취소된 상태로 둡니다.

   * **ZOOKEEPER**: 선택 취소된 상태로 둡니다.

   * **PARAMETERS**: 이 필드는 공백으로 둡니다.

3. **스크립트 동작**의 아래 쪽에서 **선택** 단추를 사용하여 구성을 저장합니다. 마지막으로 **선택적 구성** 블레이드의 아래 쪽에서 **선택** 단추를 사용하여 선택적 구성 정보를 저장합니다.

4. [Linux 기반 HDInsight 클러스터 만들기](hdinsight-hadoop-create-linux-clusters-portal.md)에서 설명한 대로 클러스터를 계속 만듭니다.

## <a name="a-nameusegiraphahow-do-i-use-giraph-in-hdinsight"></a><a name="usegiraph"></a>HDInsight에서 Giraph를 사용하는 방법

클러스터가 만들기를 완료하면 다음 단계를 사용하여 Giraph에 포함된 SimpleShortestPathsComputation 예를 실행합니다. 그래프의 개체 간 가장 짧은 경로를 찾기 위한 기본 [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) 구현을 사용합니다.

1. SSH를 사용하여 HDInsight 클러스터에 연결합니다.
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    HDInsight에서 SSH를 사용하는 방법에 대한 자세한 내용은 다음을 참조하세요.
   
   * [Linux, Unix, OS X 또는 Windows의 HDInsight에서 Linux 기반 Hadoop과 SSH 사용](hdinsight-hadoop-linux-use-ssh-unix.md)

   * [Windows의 HDInsight에서 Linux 기반 Hadoop과 SSH(PuTTY) 사용](hdinsight-hadoop-linux-use-ssh-windows.md)

2. 다음을 사용하여 **tiny_graph.txt**라는 새 파일을 만듭니다.
   
    ```
    nano tiny_graph.txt
    ```
   
    이 파일의 내용으로 다음을 사용합니다.
   
    ```
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```
   
    이 데이터는 `[source_id, source_value,[[dest_id], [edge_value],...]]` 형식을 사용하여 유향 그래프에서 개체 간 관계를 설명합니다. 각 줄은 `source_id` 개체와 하나 이상의 `dest_id` 개체 간 관계를 나타냅니다. `edge_value`는 `source_id`와 `dest\_id` 간 연결의 강도 또는 거리로 생각할 수 있습니다.
   
    개체 간 거리로 위의 값(또는 가중치)을 사용하여 그리면 다음과 같을 수 있습니다.
   
    ![원과 거리가 다른 선으로 그린 tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. 파일을 저장하려면 **Ctrl + X**, **Y**, 마지막으로 **Enter**를 차례로 사용하여 파일 이름을 적용합니다.

4. 다음을 사용하여 HDInsight 클러스터에 대한 주 저장소로 데이터를 저장합니다.
   
    ```
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. 다음 명령을 사용하여 SimpleShortestPathsComputation 예제를 실행합니다.
   
    ```
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```
   
    이 명령에 사용되는 매개 변수를 다음 테이블에서 설명합니다.
   
   | 매개 변수 | 기능 |
   | --- | --- |
   | `jar /usr/hdp/current/giraph/giraph-examples.jar` |예를 포함하는 jar 파일입니다. |
   | `org.apache.giraph.GiraphRunner` |예를 시작하는 데 사용되는 클래스입니다. |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |사용되는 예제입니다. 이 경우 그래프에서 ID 1과 다른 모든 ID 사이의 최단 경로를 계산합니다. |
   | `-ca mapred.job.tracker=headnodehost:9010` |클러스터에 대한 헤드 노드입니다. |
   | `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` |입력 데이터에 사용할 입력 형식입니다. |
   | `-vip /example/data/tiny_graph.txt` |입력 데이터 파일입니다. |
   | `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` |출력 형식입니다. 이 경우 일반 텍스트인 ID 및 값입니다. |
   | `-op /example/output/shortestpaths` |출력 위치입니다. |
   | `-w 2` |사용할 작업자의 수입니다. 이 경우 2입니다. |
   
    이 밖에 Giraph 샘플과 함께 사용된 기타 매개 변수에 대한 자세한 내용은 [Giraph 빠른 시작](http://giraph.apache.org/quick_start.html)을 참조하세요.

6. 작업이 완료되면 결과는 **/example/out/shotestpathss** 디렉터리에 저장됩니다. 출력 파일 이름은 **part-m-**으로 시작하고 첫 번째, 두 번째 파일 등을 나타내는 숫자로 끝납니다. 출력을 보려면 다음 명령을 사용합니다.
   
    ```
    hdfs dfs -text /example/output/shortestpaths/*
    ```
   
    출력은 다음과 유사합니다.
   
        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0
   
    SimpleShortestPathComputation 예제는 개체 ID 1로 시작하도록 하드 코딩되며 다른 개체에 대한 가장 짧은 경로를 찾습니다. 따라서 출력은 `destination_id distance`이며, 여기서 distance는 가장자리를 기준으로 개체 ID 1과 대상 ID 간의 거리 값(또는 가중치)입니다.
   
    이를 시각화하면 ID 1과 다른 모든 개체 간의 가장 짧은 경로를 이동하여 결과를 확인할 수 있습니다. ID 1과 ID 4 간의 가장 짧은 경로는 5입니다. <span style="color:orange">ID 1과 3</span> 사이의 총 거리와 <span style="color:red">ID 3과 4</span> 사이의 총 거리를 더한 값입니다.
   
    ![가장 짧은 경로와 함께 원으로 그린 개체](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a>다음 단계

* [HDInsight 클러스터에서 Hue 설치 및 사용](hdinsight-hadoop-hue-linux.md)입니다. Hue는 기본 저장소에서 HDInsight 클러스터를 쉽게 찾을 뿐만 아니라 Pig 및 Hive 작업을 편리하게 만들고 실행하고 저장할 수 있도록 하는 웹 UI입니다.

* [HDInsight 클러스터에 R 설치](hdinsight-hadoop-r-scripts-linux.md): 클러스터 사용자 지정을 사용하여 HDInsight Hadoop 클러스터에 R을 설치하고 사용하는 방법에 대한 지침입니다. R은 통계 계산을 위한 오픈 소스 언어 및 환경입니다. 수백 개의 기본 제공 통계 함수와 기능 및 개체 지향 프로그래밍의 측면을 결합하는 고유한 프로그래밍 언어를 제공합니다. 또한 광범위한 그래픽 기능도 제공합니다.

* [HDInsight 클러스터에 Solr 설치](hdinsight-hadoop-solr-install-linux.md)(영문). 클러스터 사용자 지정을 사용하여 HDInsight Hadoop 클러스터에 Solr을 설치합니다. Solr을 사용하면 저장된 데이터에서 강력한 검색 작업을 수행할 수 있습니다.




<!--HONumber=Feb17_HO2-->


