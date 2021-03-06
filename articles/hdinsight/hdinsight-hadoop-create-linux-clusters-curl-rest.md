---
title: "cURL 및 REST를 사용하여 Azure HDInsight(Hadoop) 만들기 | Microsoft Docs"
description: "cURL, Azure Resource Manager 템플릿 및 Azure REST API를 사용하여 HDInsight 클러스터를 만드는 방법을 알아봅니다. 클러스터 유형(Hadoop, HBase 또는 Storm)을 지정하거나 스크립트를 사용하여 사용자 지정 구성 요소를 설치할 수 있습니다."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 110f3aa9ce4848c9350ea2e560205aa762decf7a
ms.openlocfilehash: c8ed0760650f7fb3c85eef4ec7f72c60bb0efd00
ms.lasthandoff: 02/21/2017


---
# <a name="create-hdinsight-clusters-using-curl-and-the-azure-rest-api"></a>cURL 및 Azure REST API를 사용하여 HDInsight 클러스터 만들기

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure Resource Manager 템플릿 및 Azure REST API를 사용하여 HDInsight 클러스터를 만드는 방법을 알아봅니다.

Azure REST API를 사용하면 HDInsight 클러스터 등과 같은 새 리소스 생성을 포함하여 Azure 플랫폼에서 호스트되는 관리 작업을 수행할 수 있습니다.

> [!IMPORTANT]
> Linux는 HDInsight 버전 3.4 이상에서 사용되는 유일한 운영 체제입니다. 자세한 내용은 [Windows에서 HDInsight 사용 중단](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure 구독**. [Azure 무료 평가판](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)을 참조하세요.

* **Azure CLI 2.0**(미리 보기). 서비스 주체는 Azure REST API에 대한 요청의 인증 토큰을 생성하는 데 사용됩니다. Azure CLI 2.0 미리 보기에 대한 자세한 내용은 [Azure CLI 2.0 시작](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)을 참조하세요.

* **cURL**. 이 유틸리티는 패키지 관리 시스템을 통해서나 [http://curl.haxx.se/](http://curl.haxx.se/)에서 다운로드하여 사용할 수 있습니다.

  > [!NOTE]
  > PowerShell을 사용하여 이 문서의 명령을 실행하는 경우 먼저 기본적으로 생성한 `curl` 별칭을 제거합니다. 이 별칭은 cURL이 아닌 Invoke-WebRequest를 사용합니다. 이 별칭을 제거하지 않으면 이 문서에 사용된 명령 중 일부에 오류가 발생할 수 있습니다.
  >
  > 이 별칭을 제거하려면 PowerShell 프롬프트에서 다음 명령을 사용합니다.
  >
  > `Remove-item alias:curl`
  >
  > 별칭을 제거한 후에는 시스템에 설치한 cURL 버전을 사용할 수 있어야 합니다.

### <a name="access-control-requirements"></a>액세스 제어 요구 사항

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-template"></a>템플릿 만들기

Azure Resource Manager 템플릿은 **리소스 그룹**과 그 안의 모든 리소스를 설명하는 JSON 문서입니다(예: HDInsight). 이 템플릿 기반 접근 방식을 사용하면 하나의 템플릿에서 HDInsight에 필요한 리소스를 정의할 수 있습니다.

다음은 [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password)의 템플릿과 매개 변수 파일의 병합기로, 암호를 사용하여 SSH 사용자 계정을 보호하는 Linux 기반 클러스터를 만듭니다.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "location": {
                       "type": "string",
                       "allowedValues": ["Central US",
                       "East Asia",
                       "East US",
                       "Japan East",
                       "Japan West",
                       "North Europe",
                       "South Central US",
                       "Southeast Asia",
                       "West Europe",
                       "West US"],
                       "metadata": {
                           "description": "The location where all azure resources are deployed."
                       }
                   },
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[parameters('location')]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[parameters('location')]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.5",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "location": {
                   "value": "North Europe"
               },
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

이 예는 이 문서의 단계에서 사용됩니다. **매개 변수** 섹션에 있는 예제 *값*을 클러스터에 대한 값으로 바꿉니다.

> [!IMPORTANT]
> 이 템플릿에서는 HDInsight 클러스터에 대해 작업자 노드의 기본 개수(4)를 사용합니다. 32개 이상의 작업자 노드를 계획하는 경우 최소한 코어 8개와 14GB RAM을 가진 헤드 노드 크기를 선택해야 합니다.
>
> 노드 크기 및 관련된 비용에 대한 자세한 내용은 [HDInsight 가격 책정](https://azure.microsoft.com/pricing/details/hdinsight/)을 참조하세요.

## <a name="log-in-to-your-azure-subscription"></a>Azure 구독에 로그인합니다.

[Azure CLI 2.0 시작](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)의 단계에 따라 `az login` 명령을 사용하여 구독에 연결합니다.

## <a name="create-a-service-principal"></a>서비스 주체 만들기

> [!NOTE]
> 이러한 단계는 [Azure CLI를 사용하여 리소스에 액세스하기 위한 서비스 주체 만들기](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) 문서의 *암호를 사용하여 서비스 주체 만들기* 섹션의 요약된 버전입니다. 이러한 단계에서는 Azure REST API에 인증하는 데 사용되는 서비스 주체를 만듭니다.

1. 명령줄에서 다음 명령을 사용하여 Azure 구독을 나열합니다.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    목록에서 사용하려는 구독을 선택하고 **Subscription_ID** 및 __Tenant_ID__ 열을 확인합니다. 이 값을 저장합니다.

2. 다음 명령을 사용하여 Azure Active Directory에서 응용 프로그램을 만듭니다.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    `--display-name`, `--homepage` 및 `--identifier-uris`에 대한 값을 고유한 값으로 대체합니다. 새 Active Directory 항목에 대한 암호를 제공합니다.

   > [!NOTE]
   > `--home-page` 및 `--identifier-uris` 값은 인터넷에서 호스트되는 실제 웹 페이지를 참조할 필요가 없습니다. 이 값은 고유한 URI여야 합니다.

   이 명령에서 반환되는 값은 새 응용 프로그램에 대한 __App ID__입니다. 이 값을 저장합니다.

3. 다음 명령을 수행하여 **App ID**를 사용해 서비스 주체를 만듭니다.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     이 명령에서 반환되는 값은 __개체 ID__입니다. 이 값을 저장합니다.

4. **개체 ID** 값을 사용하여 **소유자** 역할을 서비스 주체에 할당합니다. 또한 이전에 받은 **구독 ID**를 사용합니다.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>인증 토큰 가져오기

다음 명령을 사용하여 인증 토큰을 검색합니다.

    curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
    -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    --data-urlencode "client_id=AppID" \
    --data-urlencode "grant_type=client_credentials" \
    --data-urlencode "client_secret=password" \
    --data-urlencode "resource=https://management.azure.com/"

    Replace **TenantID**, **AppID**, and **password** with the values obtained or used previously.

    If this request is successful, you receive a 200 series response and the response body contains a JSON document.

    The JSON document returned by this request contains an element named **access_token**. The value of **access_token** is used to authentication requests to the REST API.

   ```json
   {
       "token_type":"Bearer",
       "expires_in":"3599",
       "expires_on":"1463409994",
       "not_before":"1463406094",
       "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
   }
   ```

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

다음을 사용하여 리소스 그룹을 만듭니다.

* **SubscriptionID** 를 서비스 주체를 만들 때 받은 구독 ID로 바꿉니다.
* **AccessToken** 을 이전 단계에서 받은 액세스 토큰으로 바꿉니다.
* **DataCenterLocation** 을 리소스 그룹과 리소스를 만들려는 데이터 센터로 바꿉니다. 예를 들어 "미국 중남부"입니다.
* **ResourceGroupName** 을 이 그룹에 사용하려는 이름으로 바꿉니다.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

이 요청에 성공하면 200 시리즈 응답을 받게 되며 응답 본문에 그룹 정보가 담긴 JSON 문서가 포함되어 있습니다. `"provisioningState"` 요소는 `"Succeeded"`라는 값을 포함합니다.

## <a name="create-a-deployment"></a>배포 만들기

다음 명령을 사용하여 리소스 그룹에 템플릿을 배포합니다.

* **SubscriptionID** 및 **AccessToken**을 이전에 사용한 값으로 바꿉니다.
* **ResourceGroupName** 을 이전 섹션에서 만든 리소스 그룹 이름으로 바꿉니다.
* **DeploymentName** 을 이 배포에 사용하려는 이름으로 바꿉니다.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> 템플릿을 파일에 저장하는 경우 `-d "{ template and parameters}"` 대신 다음 명령을 사용할 수 있습니다.
>
> `--data-binary "@/path/to/file.json"`

이 요청에 성공하면 200 시리즈 응답을 받게 되며 응답 본문에 배포 작업 정보가 담긴 JSON 문서가 포함되어 있습니다.

> [!IMPORTANT]
> 이 시점에서는 배포가 제출되었지만 완료된 것은 아닙니다. 배포가 완료되려면 보통 몇 분(보통 15분 전후)이 소요됩니다.

## <a name="check-the-status-of-a-deployment"></a>배포 상태 확인

배포 상태를 확인하려면 다음 명령을 사용합니다.

* **SubscriptionID** 및 **AccessToken**을 이전에 사용한 값으로 바꿉니다.
* **ResourceGroupName** 을 이전 섹션에서 만든 리소스 그룹 이름으로 바꿉니다.

```bash
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

이 명령은 배포 작업 관련 정보가 담긴 JSON 문서를 반환합니다. `"provisioningState"` 요소는 배포의 상태를 포함합니다. `"Succeeded"` 값을 포함하면 배포가 성공적으로 완료됩니다.

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

