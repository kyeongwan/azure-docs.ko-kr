---
title: "빠른 시작 가이드: Machine Learning 권장 사항 API | Microsoft 문서"
description: "Azure 기계 학습 권장 사항 - 빠른 시작 가이드"
services: cognitive-services
documentationcenter: 
author: luiscabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 7f90fe5e-c0bf-4c16-9624-49118adf9069
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: 2acb7a96b04ba457fefe811765b17ee869de52bc
ms.openlocfilehash: 86d297fd902eb1eb8af159bc684ed119feef2b30


---
# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Cognitive 서비스 Recommendations API에 대한 빠른 시작 가이드
이 문서에서는 [Recommendations API](http://go.microsoft.com/fwlink/?LinkId=759710)를 사용하도록 서비스나 응용 프로그램을 등록하는 방법에 대해 설명합니다.
Recommendations API에 대한 자세한 내용은 [여기](http://go.microsoft.com/fwlink/?LinkId=759709)서 확인할 수 있습니다. 이 가이드 전체에서 [권장 사항 API 참조](http://go.microsoft.com/fwlink/?LinkId=759348) 및 [권장 사항 UI 빠른 시작 가이드](cognitive-services-recommendations-ui-intro.md)를 참조하세요.

<a name="Overview"></a>

### <a name="general-overview"></a>일반 개요
이 문서는 단계별 가이드입니다. 모델을 교육하는 데 필요한 단계를 안내하고 프로덕션 환경의 모델을 사용할 수 있는 리소스를 제공하는 것이 목적입니다.

이 연습에는 30분 정도가 소요됩니다.

[Recommendations API](http://go.microsoft.com/fwlink/?LinkId=759710)를 사용하려면 다음 단계를 수행해야 합니다.

1. 모델 만들기 - 모델은 사용 데이터, 카탈로그 데이터 및 권장 사항 모델의 컨테이너입니다.
2. 카탈로그 데이터 가져오기 - 카탈로그에는 항목에 대한 메타데이터 정보가 포함되어 있습니다.
3. 사용 데이터 가져오기 - 다음 두 가지 방법의 하나로(또는 둘 다) 사용 데이터를 업로드할 수 있습니다.
   * 사용 데이터를 포함하는 파일을 업로드합니다.
   * 데이터 취득 이벤트를 전송합니다.
     일반적으로 초기 권장 사항 모델(부트스트랩)을 만들고 시스템에서 데이터 취득 형식을 사용하여 충분한 데이터를 수집할 때까지 초기 모델을 사용할 수 있도록 사용 파일을 업로드합니다.
4. 권장 사항 모델 작성 - 이 단계는 권장 사항 시스템이 모든 사용 데이터를 이용하여 권장 사항 모델을 만드는 비동기 작업입니다. 이 작업은 데이터 크기 및 빌드 구성 매개 변수에 따라 몇 분이나 몇 시간이 걸릴 수 있습니다. 빌드를 트리거하면 빌드 ID가 제공됩니다. 이 빌드 ID를 사용하여 권장 사항 소비를 시작하기 전에 빌드 프로세스가 종료된 시간을 확인합니다.
5. 권장 사항 소비 – 특정 항목 또는 항목 목록에 대한 권장 사항을 가져옵니다.

<a name="GetStarted"></a>

### <a name="lets-get-started"></a>이제 시작하겠습니다.
먼저 권장 사항 모델을 만들겠습니다. 그런 다음 모델이 생성한 결과를 사용하여 전자 상거래 사이트에 권장 사항을 적용하는 방법을 안내해 드리겠습니다.

<a name="Ex1Task1"></a>

#### <a name="task-1---signing-up-for-the-recommendations-api"></a>작업 1 - Recommendations API에 등록
이 작업에서는 Recommendations API 서비스에 등록하고, 권장 사항 모델을 만듭니다.

1. **Azure 포털**[http://portal.azure.com/](http://portal.azure.com/) 으로 이동하여 Azure 계정으로 로그인합니다.
2. **+ 새로 만들기**를 클릭합니다.
3. **인텔리전스** 옵션을 선택합니다.
4. **Cognitive 서비스 API** 제품을 선택합니다.
   이 제품을 사용하여 모든 Cognitive Services API(얼굴, 텍스트 분석, 컴퓨터 비전 등)에 대한 구독을 시작할 수 있습니다. 오늘은 Recommendations API를 집중적으로 살펴보겠습니다.
5. Cognitive Services API 방문 페이지에서 권장 사항 구독의 **계정 이름** 을 입력합니다. (예: "MyRecommendations"). 이름에 공백이 있으면 안 됩니다.
6. **API 형식**에서 **권장 사항**을 선택합니다.
7. **가격 책정 계층**에서 요금제를 선택합니다. 월별 10,000트랜잭션당 **무료** 계층을 선택할 수 있습니다. 이 요금제는 무료이므로 시스템을 처음 시작하는 분들에게 적합합니다. 프로덕션으로 전환되면 요청 볼륨을 고려하여 그에 따라 적절하게 요금제를 변경하는 것이 좋습니다. (참고: 한 번에 하나의 무료 계층 구독을 가질 수 있습니다)
8. **리소스 그룹**을 선택하거나 아직 리소스 그룹이 없는 경우 새 리소스 그룹을 만듭니다.
9. 만들기 대화 상자에서 다른 요소를 변경할 수 있습니다. 오늘날 리소스 공급자는 미국 데이터 센터에서만 지원된다는 점에 주의하셔야 합니다.
10. 선택을 마치면 **만들기**를 클릭합니다.
11. 몇 분 정도 기다리면 리소스가 배포됩니다.
    리소스가 배포된 후에는 API를 사용하기 위한 기본 및 보조 키를 제공하는 **설정** 블레이드의 **키** 섹션으로 이동할 수 있습니다.  기본 키를 복사해 둡니다. 첫 번째 모델을 만들 때 필요합니다.

<a name="Ex1Task2"></a>

#### <a name="task-2---did-you-bring-your-data"></a>작업 2 - 데이터를 가져오셨습니까?
Recommendations API는 좋은 제품을 추천하기 위해 카탈로그 및 트랜잭션을 통해 학습합니다. 즉 제품에 대한 좋은 데이터(**카탈로그** 파일)와 흥미로운 소비 패턴을 찾을 수 있도록 충분히 큰 트랜잭션 집합(**사용 현황**)을 제공해야 합니다.

1. 일반적으로 이러한 정보에 대한 트랜잭션 데이터베이스를 쿼리하게 됩니다.
   이러한 데이터베이스가 없는 분들을 위해 Microsoft 스토어 트랜잭션 데이터 기반의 샘플 데이터를 준비해 두었습니다.
   
   이 데이터베이스는 [여기](http://aka.ms/RecoSampleData)서 다운로드할 수 있습니다. 로컬 컴퓨터의 폴더에 MsStoreData.Zip 파일을 복사하고 압축을 풉니다.
   
   > [!NOTE]
   > 작업 3에서 다운로드하여 실행하시는 샘플 코드는 이미 그 안에 샘플 데이터가 포함되어 있으므로 이 작업은 선택 사항입니다.  즉, 이 작업을 통해 보다 현실적인 데이터 집합을 다운로드하고 Recommendations API에 대한 입력을 더욱 완벽하게 이해할 수 있습니다.
   > 
   > 
2. 이제 카탈로그 파일에 살펴보겠습니다. 데이터를 복사한 위치로 이동합니다.
   **메모장**에서 카탈로그 파일을 엽니다.
   
   카탈로그 파일이 매우 간단한 것을 알 수 있습니다. 카탈로그 파일은 `<itemid>,<item name>,<product category>`
   
   > AAA-04294,OfficeLangPack 2013 32/64 E34 Online DwnLd,Office  <br>
   > AAA-04303,OfficeLangPack 2013 32/64 ET Online DwnLd,Office  <br>
   >  C9F-00168,KRUSELL Kiruna Flip Cover for Nokia Lumia 635 - Camel,Accessories
   > 
   > 
   
   카탈로그 파일이 훨씬 더 풍부할 수 있습니다. 예를 들어 제품에 대한 메타데이터(*항목 기능*)를 추가할 수 있습니다. 카탈로그 형식에 대한 자세한 내용은 API 참조의 [카탈로그 형식](http://go.microsoft.com/fwlink/?LinkID=760716) 섹션을 참조하세요.
3. 사용 현황 데이터로 같은 작업을 수행해 보겠습니다. 사용 날짜는 `<User Id>,<Item Id>,<Time Stamp>,<Event>`형식입니다.
   
   > 00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase 0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase 0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase 00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase 0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase 00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase
   > 
   > 

앞쪽 세 개의 요소는 필수 사항입니다. 이벤트 유형은 선택 사항입니다. 이 항목에 대한 자세한 내용은 [사용 현황 형식](http://go.microsoft.com/fwlink/?LinkID=760712) 을 참조하세요.

> **얼마나 많은 데이터가 필요합니까?**
> 
> <p>
> 필요한 데이터 양은 사용 현황 데이터 자체에 따라 크게 달라집니다. 이 시스템은 사용자가 다양한 항목을 구매할 때 학습합니다. FBT 같은 일부 빌드의 경우 동일한 트랜잭션에서 어떤 항목이 구매되는지 알아야 합니다. 이것을 *동시 발생*이라고 합니다. 그간의 경험에 따르면 대부분의 항목이 20개 이상의 트랜잭션에 포함되는 것이 좋습니다. 예를 들어 카탈로그에 10,000개의 항목이 있다면 그보다 20배 많은 트랜잭션, 다시 말해서 약 200,000 트랜잭션이 있는 것이 좋습니다.
> 다시 말씀 드리지만, 이는 경험에 근거하였습니다. 각자 본인의 데이터로 실험이 필요합니다.
> </p>
> 
> 

<a name="Ex1Task3"></a>

#### <a name="task-3---creating-a-recommendations-model"></a>작업 3 - 권장 사항 모델 만들기
이제 계정과 데이터가 있으니, 첫 번째 모델을 만들어 보겠습니다.

이 작업에서는 샘플 응용 프로그램을 사용하여 첫 번째 모델을 만들겠습니다.

1. 무엇보다도 [Recommendations API 참조](http://go.microsoft.com/fwlink/?LinkId=759348)를 알고 계셔야 합니다.
2. [샘플 응용 프로그램](http://go.microsoft.com/fwlink/?LinkID=759344) 을 로컬 폴더에 다운로드합니다.
3. Visual Studio에서 **C#** 폴더에 있는 **RecommendationsSample.sln** 솔루션을 엽니다.
4. **SampleApp.cs** 파일을 엽니다. 이 파일에서 다음 단계를 살펴봅니다.
   
   * 모델 만들기
   * 카탈로그 파일 추가
   * 사용 현황 파일 추가
   * 모델에 대한 빌드 트리거
   * 항목 쌍을 기반으로 권장 사항 가져오기
     <p></p>
5. **AccountKey** 필드의 값을 작업 1의 키로 변경합니다.
6. 솔루션을 단계적으로 진행하면 모델이 어떻게 생성되는지 알 수 있습니다.
7. 방금 다운로드한 카탈로그 및 사용 현황 파일을 바꿔서 Microsoft 스토어 또는 책 추천에 대한 새 모델을 만들어 보세요. 모델 이름뿐 아니라 권장 사항을 요청하는 항목도 변경해야 합니다.
8. 모델이 만들어지면 프로덕션 환경에서 권장 사항을 요청할 때 필요하므로 **모델 ID**를 기록해 두세요.

> 빌드 형식 및 빌드의 품질을 평가하는 방법에 대해 [여기](cognitive-services-recommendations-buildtypes.md)에서 알아봅니다.
> 

<a name="Ex1Task4"></a>

### <a name="putting-your-model-in-production"></a>모델을 프로덕션 환경에 배치!
모델을 만들고 권장 사항을 사용하는 방법을 알아보았으니, 이제 모델을 웹 사이트, 모바일 응용 프로그램에 배치하거나 CRM 또는 ERP 시스템과 통합할 차례입니다.
물론 각 구현은 서로 다를 것입니다. Recommendations API는 웹 서비스로 요청되므로 여러 환경에서 간편하게 호출할 수 있어야 합니다.

>  [!IMPORTANT]
>  공용 클라이언트(예: 전자 상거래 사이트)에서 권장 사항을 표시하려는 경우 권장 사항을 제공하는 프록시 서버를 만들어야 합니다. API 키가 외부(신뢰할 수 없는) 엔터티에 노출되지 않아야 하므로 이 작업이 중요합니다.

다음은 권장 사항을 사용할 수 있는 위치의 예입니다.

### <a name="product-page"></a>제품 페이지
**항목 추천**

<p>모델이 구매 데이터를 학습한 경우 고객은 *원본 항목을 구매한 사람이 흥미를 보일 만한 제품을 탐색*할 수 있습니다.</p>
<p>모델이 클릭 데이터를 학습한 경우 고객은 *원본 항목을 방문한 사람이 방문할 만한 제품을 탐색*할 수 있습니다. 이 유형의 모델은 비슷한 항목을 반환할 수 있습니다.</p>

**자주 함께 구매됨 추천**

<p>자주 함께 구매됨 빌드를 교육하여 이 항목과 함께 구입할 가능성이 있는 항목을 가져올 수 있습니다.</p>

### <a name="check-out-page"></a>체크아웃 페이지
**항목 추천**

<p>권장 사항 모델이 항목 목록을 입력으로 가져올 수 있습니다. 따라서 바구니의 모든 항목을 입력으로 전달하여 권장 사항을 가져올 수 있습니다.
이 경우 모델이 바구니에 있는 모든 항목을 고려하여 그와 관련이 있는 권장 사항을 제공 합니다.
</p>

### <a name="landing-page"></a>방문 페이지
**사용자 추천**

<p>
권장 사항 모델이 사용자 ID를 입력으로 가져올 수 있습니다.  이렇게 하면 해당 사용자의 트랜잭션 기록을 사용하여 지정된 사용자에게 개인화된 권장 사항이 제공됩니다.
</p>

[항목 권장 사항 가져오기 문서](http://go.microsoft.com/fwlink/?LinkID=760719)를 참조하세요.

<a name="Ex1Task6"></a>

### <a name="whats-next"></a>다음 작업
여기까지 잘 따라오신 분들에게 박수를 보냅니다! 자세히 알아보려면 완전한 [권장 사항 API 참조](http://go.microsoft.com/fwlink/?LinkId=759348)를 방문하시고, 질문이 있다면 주저 말고 언제든지 mlapi@microsoft.com으로 문의하세요.




<!--HONumber=Nov16_HO5-->


