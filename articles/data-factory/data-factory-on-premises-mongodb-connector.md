---
title: "Data Factory를 사용하여 MongoDB에서 데이터 이동 | Microsoft Docs"
description: "Azure Data Factory를 사용하여 MongoDB 데이터베이스에서 데이터를 이동하는 방법에 대해 알아봅니다."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: af15b530dd512873e4534fb61d276c8c8c3a196a
ms.openlocfilehash: 2de70faa090fb3da25fec8f8946e52fcae2677d3


---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Azure Data Factory를 사용하여 MongoDB에서 데이터 이동
이 문서에서는 Azure Data Factory에서 복사 작업을 사용하여 온-프레미스 MongoDB 데이터베이스에서 다른 데이터 저장소로 데이터를 이동하는 방법에 대해 간략하게 설명합니다. 이 문서는 복사 작업과 복사 작업을 지원하는 원본/싱크 데이터 저장소 조합을 사용하여 데이터 이동의 일반적인 개요를 보여주는 [데이터 이동 작업](data-factory-data-movement-activities.md) 문서를 작성합니다.

Data Factory 서비스는 데이터 관리 게이트웨이를 사용하여 온-프레미스 MongoDB 원본에 연결을 지원합니다. [데이터 관리 게이트웨이](data-factory-data-management-gateway.md) 문서를 참조하여 데이터 관리 게이트웨이에 대해 알아보고 [온-프레미스에서 클라우드로 데이터 이동](data-factory-move-data-between-onprem-and-cloud.md) 문서를 참조하여 데이터를 이동하는 데이터 파이프라인의 게이트웨이 설정에 대 한 단계별 지침에 대해 알아보세요.

> [!NOTE]
> Azure IaaS VM에서 호스팅되는 경우에도 MongoDB에 연결하려면 게이트웨이를 사용해야 합니다. 또한 클라우드에서 호스트되는 MongoDB의 인스턴스에 연결하려고 하는 경우 IaaS VM에 게이트웨이 인스턴스를 설치할 수 있습니다.
>
>

현재 Data Factory는 다른 데이터 저장소에서 MongoDB로 데이터 이동이 아닌 MongoDB에서 다른 데이터 저장소로 데이터 이동만을 지원합니다.

## <a name="supported-versions"></a>지원되는 버전
이 MongoDB 커넥터는 MongoDB 버전 2.4, 2.6, 3.0 및 3.2를 지원합니다.

## <a name="prerequisites"></a>필수 조건
Azure Data Factory 서비스가 사용자의 온-프레미스 MongoDB 데이터베이스에 연결할 수 있도록 하려면 다음 구성 요소를 설치해야 합니다.

* 호스트하는 동일한 컴퓨터 또는 별도 컴퓨터(데이터베이스와의 리소스 경쟁을 방지하려는 경우)에 있는 데이터 관리 게이트웨이 2.0 이상. 데이터 관리 게이트웨이는 온-프레미스 데이터 원본을 클라우드 서비스에 안전하고 관리되는 방식으로 연결하는 소프트웨어입니다. 데이터 관리 게이트웨이에 대한 세부 정보는 [데이터 관리 게이트웨이](data-factory-data-management-gateway.md) 문서를 참조하세요.

    게이트웨이를 설치하면 MongoDB에 연결하는 데 사용되는 Microsoft MongoDB ODBC 드라이버가 자동으로 설치됩니다.

## <a name="copy-data-wizard"></a>데이터 복사 마법사
MongoDB 데이터베이스의 데이터를 지원되는 싱크 데이터 저장소 중 하나에 복사하는 파이프라인을 만드는 가장 쉬운 방법은 데이터 복사 마법사를 사용하는 것입니다. 데이터 복사 마법사를 사용하여 파이프라인을 만드는 방법에 대한 빠른 연습은 [자습서: 복사 마법사를 사용하여 파이프라인 만들기](data-factory-copy-data-wizard-tutorial.md)를 참조하세요.

다음 예제에서는 [Azure 포털](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) 또는 [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)을 사용하여 파이프라인을 만드는 데 사용할 수 있는 샘플 JSON 정의를 제공합니다. 이 샘플은 MongoDB 데이터베이스에서 Azure Blob 저장소로 데이터를 복사하는 방법을 보여 줍니다. 그러나 Azure Data Factory의 복사 작업을 사용하여 [여기](data-factory-data-movement-activities.md#supported-data-stores-and-formats)에 설명한 싱크로 데이터를 복사할 수 있습니다.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>샘플: MongoDB에서 Azure Blob로 데이터 복사
이 샘플은 온-프레미스 MongoDB 데이터베이스에서 Azure Blob 저장소로 데이터를 복사하는 방법을 보여 줍니다. 그러나 Azure Data Factory의 복사 작업을 사용하여 **여기**에 설명한 싱크로 [직접](data-factory-data-movement-activities.md#supported-data-stores-and-formats) 데이터를 복사할 수 있습니다.  

이 샘플에는 다음 데이터 팩터리 엔터티가 있습니다.

1. [OnPremisesMongoDb](#linked-service-properties) 형식의 연결된 서비스입니다.
2. [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) 형식의 연결된 서비스
3. [MongoDbCollection](#dataset-type-properties) 형식의 입력 [데이터 집합](data-factory-create-datasets.md)입니다.
4. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 형식의 출력 [데이터 집합](data-factory-create-datasets.md)
5. [MongoDbSource](#copy-activity-type-properties) 및 [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)를 사용하는 복사 작업의 [파이프라인](data-factory-create-pipelines.md)입니다.

샘플은 MongoDB 데이터베이스의 쿼리 결과에서 blob에 매시간 데이터를 복사합니다. 이 샘플에 사용된 JSON 속성은 샘플 다음에 나오는 섹션에서 설명합니다.

첫 번째 단계로 [데이터 관리 게이트웨이](data-factory-data-management-gateway.md) 문서의 지침에 따라 데이터 관리 게이트웨이를 설정합니다.

**MongoDB 연결된 서비스**

```JSON
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",  
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
        }
    }
}
```

**Azure 저장소 연결된 서비스**

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**MongoDB 입력 데이터 집합** “외부”: ”true”를 설정하면 테이블이 Data Factory의 외부에 있으며 Data Factory의 활동에 의해 생성되지 않는다는 사실이 Data Factory 서비스에 전달됩니다.

```JSON
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob 출력 데이터 집합**

데이터는 매시간 새 blob에 기록됩니다(frequency: hour, interval: 1). Blob에 대한 폴더 경로는 처리 중인 조각의 시작 시간에 기반하여 동적으로 평가됩니다. 폴더 경로는 시작 시간에서 연도, 월, 일 및 시간 부분을 사용합니다.

```JSON
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%M"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%d"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "%H"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**복사 작업을 포함하는 파이프라인**

파이프라인은 위의 입력 및 출력 데이터 집합을 사용하도록 구성된 복사 작업을 포함하고 매시간 실행하도록 예약됩니다. 파이프라인 JSON 정의에서 **source** 형식은 **MongoDbSource**로 설정되고 **sink** 형식은 **BlobSink**로 설정됩니다. **query** 속성에 지정된 SQL 쿼리는 과거 한 시간에서 복사할 데이터를 선택합니다.

```JSON
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```

## <a name="linked-service-properties"></a>연결된 서비스 속성
다음 테이블은 **OnPremisesMongoDB** 연결된 서비스에 특정된 JSON 요소에 대한 설명을 제공합니다.

| 속성 | 설명 | 필수 |
| --- | --- | --- |
| type |형식 속성은 **OnPremisesMongoDb** |예 |
| server |MongoDB 서버의 IP 주소 또는 호스트 이름입니다. |예 |
| 포트 |MongoDB 서버가 클라이언트 연결을 수신하는 데 사용하는 TCP 포트입니다. |선택 사항, 기본값: 27017 |
| authenticationType |Basic 또는 Anonymous입니다. |예 |
| username |MongoDB에 액세스하는 사용자 계정입니다. |예(기본 인증을 사용하는 경우) |
| password |사용자에 대한 암호입니다. |예(기본 인증을 사용하는 경우) |
| authSource |인증에 대한 자격 증명을 확인하는 데 사용하려는 MongoDB 데이터베이스의 이름입니다. |선택 사항(기본 인증을 사용하는 경우). 기본값: 관리자 계정 및 databaseName 속성을 사용하는 지정된 데이터베이스를 사용합니다. |
| databaseName |액세스하려는 MongoDB 데이터베이스의 이름입니다. |예 |
| gatewayName |데이터 저장소에 액세스하는 게이트웨이의 이름입니다. |예 |
| encryptedCredential |게이트웨이에 의해 암호화된 자격 증명입니다. |옵션 |

온-프레미스 MongoDB 데이터 원본의 자격 증명을 설정하는 방법에 대한 자세한 내용은 [데이터 관리 게이트웨이를 사용하여 온-프레미스 원본과 클라우드 간 데이터 이동](data-factory-move-data-between-onprem-and-cloud.md)을 참조하세요.

## <a name="dataset-type-properties"></a>데이터 집합 형식 속성
데이터 집합 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 집합 만들기](data-factory-create-datasets.md) 문서를 참조하세요. 구조, 가용성 및 JSON 데이터 집합의 정책과 같은 섹션이 모든 데이터 집합 형식에 대해 유사합니다(Azure SQL, Azure blob, Azure 테이블 등).

**typeProperties** 섹션은 데이터 집합의 각 형식에 따라 다르며 데이터 저장소에 있는 데이터의 위치에 대한 정보를 제공합니다. **MongoDbCollection** 데이터 집합 형식의 데이터 집합에 대한 typeProperties 섹션에는 다음 속성이 있습니다.

| 속성 | 설명 | 필수 |
| --- | --- | --- |
| collectionName |MongoDB 데이터베이스에 있는 컬렉션의 이름입니다. |예 |

## <a name="copy-activity-type-properties"></a>복사 활동 형식 속성
활동 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인 만들기](data-factory-create-pipelines.md) 문서를 참조하세요. 이름, 설명, 입력/출력 테이블, 정책 등의 속성은 모든 형식의 활동에 사용할 수 있습니다.

반면 활동의 **typeProperties** 섹션에서 사용할 수 있는 속성은 각 활동 형식에 따라 다릅니다. 복사 활동의 경우 이러한 속성은 소스 및 싱크의 형식에 따라 달라집니다.

원본이 **MongoDbSource** 형식인 경우 typeProperties 섹션에서 다음과 같은 속성을 사용할 수 있습니다.

| 속성 | 설명 | 허용되는 값 | 필수 |
| --- | --- | --- | --- |
| 쿼리 |사용자 지정 쿼리를 사용하여 데이터를 읽습니다. |SQL-92 쿼리 문자열입니다. 예: select * from MyTable. |아니요(**데이터 집합**의 **collectionName**이 지정된 경우) |

## <a name="schema-by-data-factory"></a>Data Factory에서의 스키마
Azure Data Factory 서비스는 컬렉션에 있는 최신 100개의 문서를 사용하여 MongoDB 컬렉션에서 스키마를 유추합니다. 이러한 100개의 문서에 전체 스키마가 포함되어 있지 않는 경우, 일부 열은 복사 작업 중 무시될 수 있습니다.

## <a name="type-mapping-for-mongodb"></a>MongoDB에 대한 형식 매핑
[데이터 이동 활동](data-factory-data-movement-activities.md) 문서에서 설명한 것처럼 복사 작업은 다음 2단계 접근 방법을 사용하여 원본 형식에서 싱크 형식으로 자동 형식 변환을 수행합니다.

1. 네이티브 원본 형식에서 .NET 형식으로 변환
2. .NET 형식에서 네이티브 싱크 형식으로 변환

MongoDB에 데이터를 이동하는 경우 MongoDB 형식에서 .NET 형식으로 이동에 다음 매핑이 사용됩니다.

| MongoDB 형식 | .NET Framework 형식 |
| --- | --- |
| 이진 |Byte[] |
| Boolean |Boolean |
| Date |DateTime |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |문자열 |
| 문자열 |문자열 |
| UUID |Guid |
| Object |중첩 구분 기호로 “_”를 사용한 평면화된 열에 다시 정규화 |

> [!NOTE]
> 가상 테이블을 사용한 배열 지원에 대해 알아보려면 아래의 [가상 테이블을 사용하는 복합 형식에 대한 지원](#support-for-complex-types-using-virtual-tables) 섹션을 참조하세요.
>
>

현재 DBPointer, JavaScript, 최대/최소 키, 정규식, 기호, 타임 스탬프, 그리고 정의되지 않은 MongoDB 데이터 형식은 지원되지 않습니다.

## <a name="support-for-complex-types-using-virtual-tables"></a>가상 테이블을 사용하는 복합 형식에 대한 지원
Azure Data Factory는 기본 제공 ODBC 드라이버를 사용하여 MongoDB 데이터베이스에 연결하고 데이터를 복사합니다. 서로 다른 형식의 문서 간에 배열 또는 개체와 같은 복합 형식의 경우, 드라이버는 해당 가상 테이블에 데이터를 다시 정규화합니다. 특히, 테이블에 그러한 열이 포함되어 있으면 드라이버는 다음 가상 테이블을 생성합니다.

* **기본 테이블**: 복합 형식 열을 제외하고 실제 테이블과 동일한 데이터가 포함되어 있습니다. 기본 테이블은 나타내는 실제 테이블과 동일한 이름을 사용합니다.
* **가상 테이블** : 각 복합 형식 열에 대해 생성되며, 중첩된 데이터를 확장합니다. 가상 테이블 이름은 실제 테이블의 이름, 구분 기호 “_” 그리고 배열 및 개체 이름을 사용하여 지정합니다.

가상 테이블은 실제 테이블의 데이터를 나타내며, 드라이버가 정규화되지 않은 데이터에 액세스할 수 있도록 합니다. 자세한 내용은 아래의 예제 섹션을 참조하세요. 가상 테이블을 쿼리 및 조인하여 MongoDB 배열의 콘텐츠에 액세스할 수 있습니다.

[복사 마법사](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) 를 사용하여 가상 테이블을 비롯한 MongoDB 데이터베이스의 테이블 목록을 표시하고 내부의 데이터를 미리 볼 수 있습니다. 또한 복사 마법사에서 쿼리를 생성하고 결과가 유효한지 확인할 수도 있습니다.

### <a name="example"></a>예
예를 들어, 아래 "ExampleTable"은 MongoDB 테이블로, 각 셀의 개체 배열이 있는 하나의 열(송장)과 스칼라 형식의 배열이 있는 하나의 열(등급)이 있습니다.

| _id | 고객 이름 | 송장 | 서비스 수준 | 등급 |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{송장_id:”123”, 항목:”토스터”, 가격:”456”, 할인:”0.2”}, {송장_id:”124”, 항목:”오븐”, 가격: ”1235”, 할인: ”0.2”}] |Silver |[5,6] |
| 2222 |XYZ |[{송장_id:”135”, 항목:”냉장고”, 가격: ”12543”, 할인: ”0.0”}] |Gold |[1,2] |

드라이버는 이 단일 테이블을 나타내는 여러 개의 가상 테이블을 생성합니다. 첫 번째 가상 테이블은 아래에 표시된 "ExampleTable"이라는 기본 테이블입니다. 기본 테이블에는 모든 원본 테이블의 데이터가 있지만, 배열의 데이터는 생략되었으며 가상 테이블에서 확장됩니다.

| _id | 고객 이름 | 서비스 수준 |
| --- | --- | --- |
| 1111 |ABC |Silver |
| 2222 |XYZ |Gold |

다음 표는 예제에서 원본 배열을 나타내는 가상 테이블을 나타냅니다. 이들 테이블은 다음을 포함합니다.

* 원본 배열의 행에 해당하는 원본 기본 키 열에 역참조(_id 열을 통해)
* 원본 배열 내에서 데이터의 위치 표시
* 배열 내의 각 요소에 대해 확장된 데이터

테이블 "ExampleTable_Invoices":

| _id | ExampleTable_Invoices_dim1_idx | 송장_id | 항목 | 가격 | 할인 |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |토스터 |456 |0.2 |
| 1111 |1 |124 |오븐 |1235 |0.2 |
| 2222 |0 |135 |냉장고 |12543 |0.0 |

테이블 "ExampleTable_Ratings":

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>성능 및 튜닝
Azure Data Factory의 데이터 이동(복사 작업) 성능에 영향을 주는 주요 요소 및 최적화하는 다양한 방법에 대해 알아보려면 [복사 작업 성능 및 조정 가이드](data-factory-copy-activity-performance.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계
온-프레미스 데이터 저장소에서 Azure 데이터 저장소로 데이터를 이동시키는 데이터 파이프라인을 만드는 단계별 지침은 [온-프레미스 및 클라우드 간 데이터 이동](data-factory-move-data-between-onprem-and-cloud.md) 문서를 참조하세요.



<!--HONumber=Feb17_HO2-->


