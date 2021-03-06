---
title: "논리 앱 콘텐츠 형식 처리 | Microsoft Docs"
description: "논리 앱이 디자인 타임 및 런타임에 콘텐츠 형식을 처리하는 방법을 이해합니다."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 84f1968a4bda08232bb260f74915ef8889b53f3c
ms.openlocfilehash: 77daea3da5bdb8de374058ef7b1ab5926ed59aa7
ms.lasthandoff: 02/27/2017


---
# <a name="logic-apps-content-type-handling"></a>논리 앱 콘텐츠 형식 처리
논리 앱을 통해 흐를 수 있는 다양한 유형의 콘텐츠(JSON, XML, 플랫 파일 및 이진 데이터 포함)가 있습니다.  모든 콘텐츠 형식이 지원되지만 논리 앱 엔진에서 기본적으로 파악되는 형식도 있고, 캐스팅 또는 변환이 필요한 경우도 있습니다.  다음 문서에서는 엔진이 다른 콘텐츠 형식을 처리하는 방법과 필요에 따라 올바르게 처리하는 방법을 설명합니다.

## <a name="content-type-header"></a>Content-Type 헤더
먼저 Logic App 내에서 사용하기 위해 변환이나 캐스팅이 필요하지 않은&2;개의 `Content-Types`인 `application/json` 및 `text/plain`을 살펴보겠습니다.

## <a name="applicationjson"></a>Application/json
워크플로 엔진은 HTTP 호출의 `Content-Type` 헤더에 의존하여 적합한 처리를 확인합니다.  콘텐츠 형식 `application/json` 을 사용한 요청은 JSON 개체로 저장되고 처리됩니다.  또한 JSON 콘텐츠는 기본적으로 캐스팅되지 않고도 구문 분석될 수 있습니다.  따라서 content-type 헤더 `application/json ` 이 있는 다음과 같은 요청은

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

값(이 경우 `bar`)을 가져오기 위한 `@body('myAction')['foo'][0]`와 같은 식을 사용하여 워크플로에서 구문 분석될 수 있습니다.  추가 캐스팅은 필요 없습니다.  JSON 데이터로 작업하지만 헤더를 지정하지 않은 경우 `@json()` 함수(예: `@json(triggerBody())['foo']`)를 사용하여 수동으로 JSON으로 캐스팅할 수 있습니다.

### <a name="schema-and-schema-generator"></a>스키마 및 스키마 생성기
요청 트리거를 통해 수신할 것으로 예상되는 페이로드에 대한 JSON 스키마를 입력할 수 있습니다. 이렇게 하면 디자이너가 요청의 콘텐츠를 사용하는 데 도움이 되는 토큰을 생성할 수 있습니다. 스키마가 준비되지 않은 경우 `Use sample payload to generate schema`를 사용하여 샘플 페이로드에서 JSON 스키마를 생성합니다.

![스키마](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="use-parse-json-action"></a>parse JSON 작업 사용
`Parse JSON` 작업을 통해 JSON 콘텐츠를 Logic App에서 사용하기 친숙한 토큰으로 구문 분석할 수 있습니다. 요청 트리거와 마찬가지로 구문 분석하려는 콘텐츠에 맞게 JSON 스키마를 입력하거나 생성할 수 있습니다. Service Bus, DocumentDb 등에서 데이터 사용을 훨씬 간편하게 하는 데 유용한 도구입니다.

![Parse JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>Text/plain
`application/json`과 마찬가지로 `text/plain`의 `Content-Type` 헤더와 함께 수신된 HTTP 메시지는 원시 형태로 저장됩니다.  또한 캐스팅 없이 후속 작업에 포함된 요청은 `Content-Type`: `text/plain` 헤더와 함께 사용됩니다.  예를 들어 플랫 파일을 사용하는 경우 다음 HTTP 콘텐츠가

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

`text/plain`으로 수신될 수 있습니다.  다음 작업에서 다른 요청의 본문으로 전송한 경우(`@body('flatfile')`) 요청에는 `text/plain` 콘텐츠 유형 헤더가 포함됩니다.  일반 텍스트 데이터로 작업하지만 헤더를 지정하지 않은 경우 `@string()` 함수(예: `@string(triggerBody())`)를 사용하여 수동으로 일반 텍스트로 캐스팅할 수 있습니다.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml, Application/octet-stream 및 변환기 함수
Logic App 엔진은 HTTP 요청 또는 응답에서 수신된 `Content-Type`을 항상 보존합니다.  즉, 캐스팅이 없는 후속 작업의 경우처럼 `Content-Type`이 `application/octet-stream`인 콘텐츠가 수신되면 `Content-Type`: `application/octet-stream`의 아웃바운드 요청이 발생합니다.  이러한 방식으로 엔진은 워크플로 전체에서 이동하는 데이터가 손실되지 않을 것임을 보장할 수 있습니다.  그러나 워크플로를 따라 진행되면서 작업 상태(입력 및 출력)가 JSON 개체에 저장됩니다.  즉, 일부 데이터 형식을 유지하기 위해 엔진은 `$content` 및 `$content-type`(자동으로 변환됨) 둘 다를 보존하는 적합한 메타데이터를 사용하여 콘텐츠를 이진 base64 인코딩 문자열로 변환합니다.  기본 제공 변환기 함수를 사용하여 콘텐츠 형식 간을 수동으로 변환할 수도 있습니다.

* `@json()` - 데이터를 `application/json`로 캐스팅합니다.
* `@xml()` - 데이터를 `application/xml`로 캐스팅합니다.
* `@binary()` - 데이터를 `application/octet-stream`로 캐스팅합니다.
* `@string()` - 데이터를 `text/plain`로 캐스팅합니다.
* `@base64()` - 콘텐츠를 base64 문자열로 변환합니다.
* `@base64toString()` - base64 인코딩 문자열을 `text/plain`으로 변환합니다.
* `@base64toBinary()` - base64 인코딩 문자열을 `application/octet-stream`으로 변환합니다.
* `@encodeDataUri()` - 문자열을 dataUri 바이트 배열로 인코딩합니다.
* `@decodeDataUri()` - dataUri를 바이트 배열로 디코딩합니다.

예를 들어 `Content-Type`: `application/xml`인 다음 HTTP 요청을 받은 경우

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

캐스팅한 후 나중에 `@xml(triggerBody())` 등의 항목과 함께 또는 `@xpath(xml(triggerBody()), '/CustomerName')` 등의 함수 내에서 사용할 수 있습니다.

## <a name="other-content-types"></a>다른 콘텐츠 형식
다른 콘텐츠 형식이 지원되고 논리 앱에서 작동되지만 `$content`를 디코딩하여 메시지 본문을 수동으로 검색해야 할 수 있습니다.  예를 들어 `application/x-www-url-formencoded` 요청에 대해 다음과 같이 트리거한 경우

```
CustomerName=Frank&Address=123+Avenue
```

일반 텍스트도, JSON도 아니므로 다음과 같이 작업에 저장됩니다.

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

여기서 `$content` 는 모든 데이터를 보존하하기 위해 base64 문자열로 인코딩된 페이로드입니다.  현재 양식 데이터에 대한 네이티브 함수가 없으므로 `@string(body('formdataAction'))`과 같은 함수를 통해 데이터에 수동으로 액세스하여 워크플로 내에서 이 데이터를 계속 사용할 수 있습니다.  나가는 요청에도 `application/x-www-url-formencoded` 콘텐츠 유형 헤더를 포함하기 위해 `@body('formdataAction')`과 같이 캐스팅 없이 작업 본문에 이를 추가할 수 있습니다.  그러나 이러한 방식은 본문이 `body` 입력의 유일한 매개 변수인 경우에만 작동합니다.  `application/json` 요청 내에서 `@body('formdataAction')`을 수행하려고 할 경우 인코딩된 암호를 전송하게 되므로 런타임 오류가 발생합니다.
