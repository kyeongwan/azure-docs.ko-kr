---
title: "2단계: Machine Learning 실험에 데이터 업로드 | Microsoft Docs"
description: "예측 솔루션 연습 개발의 2단계: Azure 기계 학습 스튜디오로 저장된 공용 데이터를 업로드합니다."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: a9ebbbdc431a34553de04e920efbbc8c2496ce5f
ms.openlocfilehash: 2c44b51d9c832116bf77758144725d2ed3f6e422


---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>연습 2단계: Azure 기계 학습 실험에 기존 데이터 업로드
[Azure 기계 학습에서 예측 분석 솔루션 개발](machine-learning-walkthrough-develop-predictive-solution.md)

1. [기계 학습 작업 영역 만들기](machine-learning-walkthrough-1-create-ml-workspace.md)
2. **기존 데이터 업로드**
3. [새 실험 만들기](machine-learning-walkthrough-3-create-new-experiment.md)
4. [모델 학습 및 평가](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [웹 서비스 배포](machine-learning-walkthrough-5-publish-web-service.md)
6. [웹 서비스 액세스](machine-learning-walkthrough-6-access-web-service.md)

- - -
신용 위험에 대한 예측 모델을 개발하려면 모델을 학습 후 테스트할 수 있는 데이터가 필요합니다. 이 연습에서는 UC Irvine Machine Learning 리포지토리의 "UCI Statlog(독일 신용 데이터) 데이터 집합"을 사용합니다. 이 데이터 집합은   
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

여기서는 **german.data**라는 파일을 사용합니다. 로컬 하드 드라이브로 이 파일을 다운로드하세요.  

이 데이터 집합에는 지난 1000명의 신용대출 신청자에 대한 20개 변수 행이 포함되어 있습니다. 이러한 20개 변수는 각 대출 신청자에 대한 특성을 파악하는 기능의 데이터 집합(기능 벡터)을 나타냅니다. 각 행의 추가 열은 신청자의 계산된 신용 위험을 나타내며, 700명의 신청자는 신용 위험이 낮은 것으로, 300명의 신청자는 위험이 높은 것으로 파악되었습니다.

UCI 웹 사이트에서는 이 데이터에 대한 기능 벡터의 특성을 설명합니다. 여기에는 재무 정보, 신용 기록, 고용 상태 및 개인 정보가 포함됩니다. 각 지원자에 대해 신용 위험이 낮은지 아니면 높은지 여부를 나타내는 이진 등급이 제공되었습니다. 

이 데이터를 사용하여 예측 분석 모델의 학습을 진행합니다. 학습을 완료하면 모델에서 새로운 개인에 대한 기능 벡터를 허용하여 신용 위험이 낮거나 높은지 예측할 수 있습니다.  

그런데 한 가지 흥미로운 변형이 있습니다. 데이터 집합에 대한 설명에서는 지원자가 실제로 신용 위험이 높은데 낮은 신용 위험으로 잘못 분류하면 낮은 신용 위험을 높은 것으로 잘못 분류하는 경우보다 금융 기관의 비용이 5배 더 커진다고 말합니다. 이 실험에서 이런 점을 고려할 수 있는 간단한 방법은 신용 위험이 높은 사람을 나타내는 항목을 5배 복제하는 것입니다. 그런 다음 모델에서 높은 신용 위험을 낮은 것으로 잘못 분류하는 경우 각 복제마다 한 번씩 분류 오류를 5번 수행합니다. 그러면 학습 결과에서 이 오류의 비용이 증가됩니다.  

## <a name="convert-the-dataset-format"></a>데이터 집합 형식 변환
원래 데이터 집합은 공백으로 구분된 형식을 사용합니다. 기계 학습 스튜디오는 CSV(쉼표로 구분된 값) 파일에서 더 원활하게 작동하므로 공백을 쉼표로 바꿔서 데이터 집합을 변환합니다.  

여러 가지 방법으로 이 데이터를 변환할 수 있습니다. 한 가지 방법은 다음 Windows PowerShell 명령을 사용하는 것입니다.   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

또 다른 방법은 Unix sed 명령을 사용하는 것입니다.  

    sed 's/ /,/g' german.data > german.csv  

이 두 방법에서는 모두 실험 시 쉼표로 구분된 데이터 버전인 **german.csv** 파일을 사용합니다.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>기계 학습 스튜디오에 데이터 집합 업로드
데이터를 CSV 형식으로 변환한 후 기계 학습 스튜디오에 업로드해야 합니다. 

1. Machine Learning Studio 홈 페이지를 엽니다([https://studio.azureml.net](https://studio.azureml.net)). 

2. 창의 왼쪽 상단 모서리에 있는 ![메뉴][1]를 클릭하고 **Azure Machine Learning**을 클릭하고 **Studio**를 선택한 다음 로그인합니다.

3. 창 아래쪽에서 **+새로 만들기** 를 클릭합니다.

4. **데이터 집합**을 선택합니다.

5. **로컬 파일에서**를 선택합니다.

    ![로컬 파일에서 데이터 집합 추가][2]

6. **새 데이터 집합 업로드** 대화 상자에서 **찾아보기**를 클릭하여 앞서 만든 **german.csv** 파일을 찾습니다.

7. 데이터 집합의 이름을 입력합니다. 이 연습에서는 데이터 집합을 "UCI 독일 신용 카드 데이터"라고 합니다.

8. 데이터 형식으로 **머리글 없는 일반 CSV 파일(.nh.csv)**을 선택합니다.

9. 원하는 경우 설명을 추가합니다.

10. **확인** 확인 표시를 클릭합니다.  

    ![데이터 집합 업로드][3]

그러면 데이터가 실험에 사용할 수 있는 데이터 집합 모듈에 업로드됩니다.

스튜디오 창 왼쪽의 **데이터 집합** 탭을 클릭하여 스튜디오에 업로드된 데이터 집합을 관리할 수 있습니다.

![데이터 집합 관리][4]

다른 데이터 형식을 실험으로 가져오는 방법에 대한 자세한 내용은 [Azure Machine Learning Studio로 학습 데이터 가져오기](machine-learning-data-science-import-data.md)를 참조하세요.

**다음 단계: [새 실험 만들기](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png



<!--HONumber=Dec16_HO3-->


