---
title: "HDInsight에서 PowerShell과 Hadoop Pig 사용 | Microsoft Docs"
description: "Azure PowerShell을 사용하여 HDInsight의 Hadoop 클러스터에 Pig 작업을 제출하는 방법에 대해 알아봅니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cfaade8249a643b77f3d7fdf466eb5ba38143f18
ms.openlocfilehash: a795f7a8dbb02ad29c517dca6ab06575a8d48bf9
ms.lasthandoff: 03/01/2017

---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>Azure PowerShell을 사용하여 HDInsight에서 Pig 작업 실행

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

이 문서는 Azure PowerShell을 사용하여 HDInsight의 Hadoop 클러스터에 Pig 작업을 제출하는 예제를 보여 줍니다. Pig를 사용하면 매핑하고 함수를 줄이는 대신 데이터 변환을 모델링하는 언어(Pig Latin)를 사용하여 MapReduce 작업을 작성할 수 있습니다.

> [!NOTE]
> 이 문서에는 예제에 사용된 Pig Latin 문이 수행하는 작업에 대해 자세한 설명을 제공하지 않습니다. 이 예제에 사용된 Pig Latin에 대한 자세한 내용은 [HDInsight에서 Hadoop과 Pig 사용](hdinsight-use-pig.md)을 참조하세요.

## <a id="prereq"></a>필수 조건

* **Azure HDInsight 클러스터**

  > [!IMPORTANT]
  > Linux는 HDInsight 버전 3.4 이상에서 사용되는 유일한 운영 체제입니다. 자세한 내용은 [Windows에서 HDInsight 사용 중단](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)을 참조하세요.

* **Azure PowerShell이 포함된 워크스테이션**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>PowerShell을 사용하여 Pig 작업 실행

Azure PowerShell은 HDInsight에서 Pig 작업을 원격으로 실행할 수 있는 *cmdlet* 을 제공합니다. 내부적으로 PowerShell은 HDInsight 클러스터에서 실행되는 [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)으로 REST 호출을 사용합니다.

다음 cmdlet은 원격 HDInsight 클러스터에서 Pig 작업을 실행할 때 사용됩니다.

* **Login-AzureRmAccount**: Azure 구독에 대해 Azure PowerShell을 인증합니다.
* **New-AzureRmHDInsightPigJobDefinition**: 지정한 Pig Latin 문을 사용하여 *작업 정의*를 만듭니다.
* **Start-AzureRmHDInsightJob**: HDInsight로 작업 정의를 보내고, 작업을 시작하고, 작업 상태를 확인하는 데 사용할 수 있는 *작업* 개체를 반환합니다.
* **Wait-AzureRmHDInsightJob**: 작업 개체를 사용하여 작업 상태를 확인합니다. 작업이 완료될 때까지 기다리거나 대기 시간이 초과될 때까지 기다립니다.
* **Get-AzureRmHDInsightJobOutput**: 작업 출력을 검색하는 데 사용됩니다.

다음 단계는 HDInsight 클러스터에서 작업을 실행하기 위해 이러한 cmdlet을 사용하는 방법에 대해 설명합니다.

1. 편집기를 사용하여 다음 코드를 **pigjob.ps1**로 저장합니다.
   
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        # Get cluster info
        $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
        $creds=Get-Credential -Message "Enter the login for the cluster"

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasb:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"

        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds


1. 새 Windows PowerShell 명령 프롬프트를 엽니다. **pigjob.ps1** 파일의 디렉터리 위치를 변경한 다음 명령을 사용하여 스크립트를 실행합니다.
   
        .\pigjob.ps1
   
    먼저 Azure 구독에 로그인하라는 메시지가 표시될 수 있습니다. 그런 다음 HTTPs/관리자 계정 이름과 HDInsight 클러스터 암호를 묻는 메시지가 표시됩니다.

2. 작업이 완료되면 다음 텍스트와 유사한 정보가 반환됩니다.
   
        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>문제 해결

작업이 완료될 때 정보가 반환되지 않은 경우, 처리하는 동안 오류가 발생했을 수 있습니다. 이 작업에 대한 오류 정보를 보려면 **pigjob.ps1** 파일 끝에 다음 명령을 추가하고 저장한 다음 다시 실행합니다.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

이 명령은 작업을 실행할 때 서버의 STDERR에 기록된 정보를 반환하며 이 정보는 작업이 실패한 이유를 확인하는 데 도움이 될 수 있습니다.

## <a id="summary"></a>요약
이처럼 Azure PowerShell은 HDInsight 클러스터에서 Pig 작업을 실행하고, 작업 상태를 모니터링하고, 출력을 검색하는 쉬운 방법을 알려줍니다.

## <a id="nextsteps"></a>다음 단계
HDInsight의 Pig에 대한 일반적인 정보:

* [HDInsight에서 Hadoop과 Pig 사용](hdinsight-use-pig.md)

HDInsight에서 Hadoop으로 작업하는 다른 방법에 관한 정보:

* [HDInsight에서 Hadoop과 Hive 사용](hdinsight-use-hive.md)
* [HDInsight에서 Hadoop과 MapReduce 사용](hdinsight-use-mapreduce.md)


