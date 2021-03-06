---
title: "웹 브라우저를 사용하여 Azure HDInsight(Hadoop) 만들기 | Microsoft Docs"
description: "웹 브라우저와 Azure Preview 포털을 사용하여 Linux 기반 HDInsight에서 Hadoop, HBase, Storm 또는 Spark 클러스터를 만드는 방법을 알아봅니다."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/16/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 9e480c13f48e93da32ff5a3c8d3064e98fed0265
ms.openlocfilehash: 7bec6b07d8e15845f06b20f24414a0e25a955f38
ms.lasthandoff: 02/17/2017


---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Azure 포털을 사용하여 HDInsight에서 Linux 기반 클러스터 만들기
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure 포털은 Microsoft Azure 클라우드에 호스트된 서비스와 리소스에 대한 웹 기반 관리 도구입니다. 이 문서에서는 포털을 사용하여 Linux 기반 HDInsight 클러스터를 만드는 방법에 대해 알아봅니다.

## <a name="prerequisites"></a>필수 조건
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure 구독**. [Azure 무료 평가판](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)을 참조하세요.
* **최신 웹 브라우저**. Azure 포털은 HTML5 및 Javascript를 사용하며 이전 웹 브라우저에서 제대로 작동하지 않을 수 있습니다.

### <a name="access-control-requirements"></a>액세스 제어 요구 사항
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>클러스터 만들기
Azure 포털은 대부분의 클러스터 속성을 노출합니다. Azure Resource Manager 템플릿을 사용하여 이러한 많은 속성을 숨길 수 있습니다. 자세한 내용은 [Azure Resource Manager 템플릿을 사용하여 HDInsight의 Linux 기반 Hadoop 클러스터 만들기](hdinsight-hadoop-create-linux-clusters-arm-templates.md)를 참조하세요.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. **+**, **인텔리전스 + 분석** 및 **HDInsight**를 차례로 클릭합니다.
   
    ![Azure Portal에서 새 클러스터 만들기](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Azure Portal에서 새 클러스터 만들기")

3. **HDInsight** 블레이드에서 **(크기, 설정, 응용 프로그램) 사용자 지정**을 클릭한 다음 **기본 사항**을 클릭하고 다음 정보를 입력합니다.

    ![Azure Portal에서 새 클러스터 만들기](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.basics.png "Azure Portal에서 새 클러스터 만들기")

    * **클러스터 이름**입력: 이 이름은 전역적으로 고유해야 합니다.

    * **구독** 드롭다운에서 클러스터에 사용할 Azure 구독을 선택합니다.

    * **클러스터 유형**을 클릭한 후 다음을 선택합니다.
   
        * **클러스터 유형**: 어떤 유형을 선택할지 잘 모르는 경우 **Hadoop**을 선택합니다. 가장 인기 있는 클러스터 유형입니다.
     
             > [!IMPORTANT]
             > HDInsight 클러스터는 작업 부하 또는 클러스터에 대한 튜닝 기술에 해당하는 다양한 형식을 제공합니다. 하나의 클러스터에서 Storm 및 HBase 등의 여러 유형을 결합하는 클러스터를 만들기 위해 지원되는 메서드가 없습니다. 
             > 
             > 
        
        * **운영 체제**: **Linux**를 선택합니다.
        
        * **버전**: 어떤 버전을 선택할지 잘 모르는 경우 기본 버전을 사용합니다. 자세한 내용은 [HDInsight 클러스터 버전](hdinsight-component-versioning.md)을 참조하세요.
        * **클러스터 계층**: Azure HDInsight는 빅 데이터 클라우드 제품을 표준 계층 및 프리미엄 계층의 두 범주로 제공합니다. 자세한 내용은 [클러스터 계층](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers)을 참조하세요.

    * **클러스터 로그인 사용자 이름** 및 **클러스터 로그인 암호**에 관리 사용자의 사용자 이름 및 암호를 입력합니다.

    * **SSH 사용자 이름**을 입력하고 SSH 암호를 이전에 지정한 관리자 암호와 동일하게 하려면 **클러스터 로그인과 동일한 암호 사용** 확인란을 선택합니다. 그렇지 않으면 **암호** 또는 **공개 키**를 제공합니다. 이 항목은 SSH 사용자를 인증하는 데 사용됩니다. 공개 키를 사용하는 것이 권장 방식입니다. 아래쪽의 **선택** 을 클릭하여 자격 증명 구성을 저장합니다.
   
        HDInsight에서 SSH를 사용하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.
   
        * [Linux, Unix 또는 OS X의 HDInsight에서 Hadoop과 SSH 사용](hdinsight-hadoop-linux-use-ssh-unix.md)
        * [Windows의 HDInsight에서 Hadoop과 SSH 사용](hdinsight-hadoop-linux-use-ssh-windows.md)

    * **리소스 그룹**에서 새 리소스 그룹을 만들지 아니면 기존 집합을 사용할지 여부를 지정합니다.

    * 클러스터를 만들 데이터 센터 **위치**를 지정합니다.

    * **다음**을 클릭합니다.

4. **저장소** 블레이드에서 Azure Storage(WASB) 또는 Data Lake Store를 기본 저장소로 사용할지 여부를 지정합니다. 자세한 내용은 아래 테이블을 살펴보세요.

    ![Azure Portal에서 새 클러스터 만들기](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.storage.png "Azure Portal에서 새 클러스터 만들기")

    | 저장소                                      | 설명 |
    |----------------------------------------------|-------------|
    | **기본 저장소인 Azure Storage Blob**   | <ul><li>**기본 저장소 형식**으로 **Azure Storage**를 선택합니다. 그런 다음 Azure 구독의 일부인 저장소 계정을 지정하고 저장소 계정을 선택하려는 경우 **선택 방법**에서 **내 구독**을 선택할 수 있습니다. 그렇지 않으면 **선택키**를 클릭하고 Azure 구독 외부에서 선택하려는 저장소 계정에 대한 정보를 제공합니다.</li><li>**기본 컨테이너**의 경우 포털에서 제안한 기본 컨테이너 이름을 사용하거나 고유한 이름을 지정하도록 선택할 수 있습니다.</li><li>WASB를 기본 저장소를 사용하는 경우 선택적으로 **추가 저장소 계정**을 클릭하여 클러스터와 연결할 추가 저장소 계정을 지정합니다. **Azure Storage 키** 블레이드에서 **저장소 키 추가**를 클릭한 다음 Azure 구독 또는 다른 구독에서 저장소 계정 선택키를 제공하여 저장소 계정을 제공할 수 있습니다.</li><li>WASB를 기본 저장소로 사용하는 경우 선택적으로 **Data Lake Store 액세스**를 클릭하여 Azure Data Lake Store를 추가 저장소로 지정할 수 있습니다. 자세한 내용은 [Azure Portal을 사용하여 Data Lake Store로 HDInsight 클러스터 만들기](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)를 참조하세요.</li></ul> |
    | **기본 저장소로 Azure Data Lake Store** | **기본 저장소 유형**에서 **데이터 레이크 저장소**를 선택하고 지침은 [Azure Portal을 사용하여 Data Lake Store로 HDInsight 클러스터 만들기](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) 문서를 참조합니다. |
    | **외부 Metastore**                      | 필요에 따라 SQL Database를 지정하여 클러스터와 연결된 Hive 및 Oozie 메타데이터를 저장할 수 있습니다. **Hive용 SQL Database 사용**에 대해 SQL Database를 클릭하고 데이터베이스의 사용자 이름/암호를 입력합니다. Oozie 메타데이터에 대해 이러한 단계를 반복합니다.<br><br>metastores에 대한 Azure SQL Database를 사용하는 동안 몇 가지 고려 사항입니다. <ul><li>메타스토어에 사용되는 Azure SQL 데이터베이스는 Azure HDInsight를 비롯한 다른 Azure 서비스로의 연결을 허용해야 합니다. Azure SQL 데이터베이스 대시보드의 오른쪽에서 서버 이름을 클릭합니다. 이 서버는 SQL 데이터베이스 인스턴스가 실행되는 서버입니다. 서버 보기에서 **구성**을 클릭하고 **Azure 서비스**에 대해 **예**를 클릭한 다음 **저장**을 클릭합니다.</li><li>Metastore를 만들 때 클러스터를 만드는 프로세스가 실패할 수 있으므로 대시 또는 하이픈을 포함하는 데이터베이스 이름을 사용하지 않습니다.</li></ul>                                                                                                                                                                       |

    **다음**을 클릭합니다. 

5. 필요에 따라 **응용 프로그램**을 클릭하여 HDInsight 클러스터에 작동하는 응용 프로그램을 설치합니다. Microsoft, ISV(독립 소프트웨어 공급 업체) 또는 사용자가 직접 이러한 응용 프로그램을 개발할 수 있습니다. 자세한 내용은 [HDInsight 응용 프로그램 설치](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation)를 참조하세요.


6. **클러스터 크기**를 클릭하여 이 클러스터에 대해 만들어질 노드에 대한 정보를 표시합니다. 클러스터에 필요한 작업자 노드 수를 설정합니다. 클러스터의 예상 비용이 블레이드 내에 표시됩니다.
   
    ![노드 가격 책정 계층 블레이드](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.nodes.png "클러스터 노드 수 지정")
   
   > [!IMPORTANT]
   > 클러스터 만들기에서 또는 클러스터를 만든 후 확장하여 32개 이상의 작업자 노드를 계획하는 경우 최소한 코어 8개와 14GB RAM을 가진 헤드 노드 크기를 선택해야 합니다.
   > 
   > 노드 크기 및 관련된 비용에 대한 자세한 내용은 [HDInsight 가격 책정](https://azure.microsoft.com/pricing/details/hdinsight/)을 참조하세요.
   > 
   > 
   
   **다음**을 클릭하여 노드 가격 책정 구성을 저장합니다.

7. **고급 설정**을 클릭하여 **스크립트 작업**을 사용하는 등 다른 선택적 설정을 구성하고 **가상 네트워크**에 조인하여 사용자 지정 구성 요소를 설치하도록 클러스터를 사용자 지정합니다. 자세한 내용은 아래 테이블을 살펴보세요.

    ![노드 가격 책정 계층 블레이드](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.advanced.png "클러스터 노드 수 지정")

    | 옵션 | 설명 |
    |--------|-------------|
    | **스크립트 작업** | 클러스터를 만들 때 사용자 지정 스크립트를 사용하여 클러스터를 사용자 지정하려는 경우에 이 옵션을 사용합니다. 스크립트 동작에 대한 자세한 내용은 [스크립트 동작을 사용하여 HDInsight 클러스터 사용자 지정](hdinsight-hadoop-customize-cluster-linux.md)을 참조하세요. |
    | **Virtual Network** | 클러스터를 가상 네트워크에 배치하려는 경우 Azure Virtual Network 및 서브넷을 선택합니다. 가상 네트워크에 대한 특정 구성 요구 사항을 포함하여 가상 네트워크로 HDInsight를 사용하는 방법에 대한 자세한 내용은 [Azure 가상 네트워크를 사용하여 HDInsight 기능 확장](hdinsight-extend-hadoop-virtual-network.md)을 참조하세요. |

    **다음**을 클릭합니다.

8. **요약** 블레이드에서 이전에 입력한 정보를 확인한 다음 **만들기**를 클릭합니다.

    ![노드 가격 책정 계층 블레이드](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.summary.png "클러스터 노드 수 지정")
    
    > [!NOTE]
    > 클러스터를 만드는데 약간의 시간이 걸리며, 일반적으로 약 15분이 소요됩니다. 시작 보드에 있는 타일 또는 페이지 왼쪽에 있는 **알림** 항목을 사용하여 프로비전 프로세스를 확인하세요.
    > 
    > 
12. 생성 프로세스가 완료되면 시작 보드에서 클러스터 타일을 클릭하여 클러스터 블레이드를 시작합니다. 클러스터 블레이드에서는 다음 정보를 제공합니다.
    
    ![클러스터 블레이드](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.completed.png "클러스터 속성")
    
    다음을 사용하여 이 블레이드의 위쪽에 있는 아이콘을 이해합니다.
    
    * **개요** 탭은 이름, 속한 리소스 그룹, 위치, 운영 체제, 클러스터 대시보드의 URL 등 클러스터에 대한 모든 필수 정보를 제공합니다.
    * **대시보드**는 사용자를 클러스터와 연결된 Ambari 포털로 다이렉트합니다.
    * **보안 셸**: SSH를 사용하여 클러스터에 액세스하는 데 필요한 정보입니다.
    * **클러스터 크기 조정**을 통해 클러스터와 연결된 작업자 노드 수를 늘릴 수 있습니다.
    * **삭제**: HDInsight 클러스터를 삭제합니다.
    

## <a name="customize-clusters"></a>클러스터 사용자 지정
* [부트스트랩을 사용하여 HDInsight 클러스터 사용자 지정](hdinsight-hadoop-customize-cluster-bootstrap.md)을 참조하세요.
* [스크립트 동작을 사용하여 HDInsight 클러스터 사용자 지정](hdinsight-hadoop-customize-cluster-linux.md)을 참조하세요.

## <a name="delete-the-cluster"></a>클러스터 삭제
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>다음 단계
HDInsight 클러스터를 성공적으로 만들었으므로 다음을 사용하여 클러스터 작업을 수행하는 방법을 알아봅니다.

### <a name="hadoop-clusters"></a>Hadoop 클러스터
* [HDInsight에서 Hive 사용](hdinsight-use-hive.md)
* [HDInsight에서 Pig 사용](hdinsight-use-pig.md)
* [HDInsight와 함께 MapReduce 사용](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase 클러스터
* [HDInsight에서 HBase 시작](hdinsight-hbase-tutorial-get-started-linux.md)
* [HDInsight에서 HBase용 Java 응용 프로그램 개발](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm 클러스터
* [HDInsight에서 Storm용 Java 토폴로지 개발](hdinsight-storm-develop-java-topology.md)
* [HDInsight의 Storm에서 Python 구성 요소 사용](hdinsight-storm-develop-python-topology.md)
* [HDInsight에서 Storm을 사용하는 토폴로지 배포 및 모니터링](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark 클러스터
* [Scala를 사용하여 독립 실행형 응용 프로그램 만들기](hdinsight-apache-spark-create-standalone-application.md)
* [Livy를 사용하여 Spark 클러스터에서 원격으로 작업 실행](hdinsight-apache-spark-livy-rest-interface.md)
* [BI와 Spark: BI 도구와 함께 HDInsight에서 Spark를 사용하여 대화형 데이터 분석 수행](hdinsight-apache-spark-use-bi-tools.md)
* [기계 학습과 Spark: 음식 검사 결과를 예측하는 데 HDInsight의 Spark 사용](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark 스트리밍: HDInsight에서 Spark를 사용하여 실시간 스트리밍 응용 프로그램 빌드](hdinsight-apache-spark-eventhub-streaming.md)


