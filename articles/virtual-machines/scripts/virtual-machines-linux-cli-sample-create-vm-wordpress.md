---
title: "Azure CLI 스크립트 샘플 - WordPress를 사용하여 Linux VM 만들기 | Microsoft Docs"
description: "Azure CLI 스크립트 샘플 - WordPress를 사용하여 Linux VM 만들기"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: e5362b32a0f93433c77c27a6fc116960c45404f9
ms.openlocfilehash: 8e4068428b814265140b76905bc7f90d9e88f94b
ms.lasthandoff: 03/01/2017

---

# <a name="create-a-vm-with-wordpress"></a>WordPress를 사용하여 VM 만들기

이 스크립트는 가상 컴퓨터를 만들고 Azure Virtual Machine 사용자 지정 스크립트 확장을 사용하여 WordPress를 설치합니다. 스크립트를 실행하면 `http://<public IP of VM>/wordpress`에서 WordPress 구성 사이트에 액세스할 수 있습니다. 

이 샘플은 Bash 셸에서 작동합니다. Windows 클라이언트에서 Azure CLI 스크립트 실행과 관련된 옵션은 [Windows에서 Azure CLI 실행](../virtual-machines-windows-cli-options.md)을 참조하세요.

이 스크립트를 실행하기 전에 `az login` 명령을 사용하여 Azure와의 연결이 생성되었는지 확인합니다.

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli[기본](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "VM 빠른 생성")]

## <a name="clean-up-deployment"></a>배포 정리 

스크립트 샘플을 실행한 후에는 다음 명령을 사용하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 컴퓨터 및 모든 관련된 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 참고 사항 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | 가상 컴퓨터를 만들고 네트워크 카드, 가상 네트워크, 서브넷 및 NSG에 연결합니다. 또한 이 명령은 사용할 가상 컴퓨터 이미지와 관리 자격 증명을 지정합니다.  |
| [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port) | 인바운드 트래픽을 허용하도록 네트워크 보안 그룹 규칙을 만듭니다. 이 샘플에서 HTTP 트래픽에 대해 포트 80이 열립니다. |
| [az vm extension set](https://docs.microsoft.com/cli/azure/vm#create) | 가상 컴퓨터에 WordPress를 설치하는 스크립트를 호출하는 사용자 지정 스크립트 확장을 추가합니다. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure/overview)를 참조하세요.

추가 가상 컴퓨터 CLI 스크립트 샘플은 [Azure Linux VM 설명서](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)에서 확인할 수 있습니다.

