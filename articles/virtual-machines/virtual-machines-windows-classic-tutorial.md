---
title: "Azure Portal에서 VM 만들기 | Microsoft Docs"
description: "Azure 포털에서 Windows 가상 컴퓨터만들기"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 15e6eb0fcd3baf34c2144aa3ce13a8aff59f9a5a
ms.openlocfilehash: 502c420b927ab835e585e848f153328d9f9565ee
ms.lasthandoff: 03/01/2017


---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Azure 포털에서 Windows를 실행하는 가상 컴퓨터 만들기
> [!div class="op_single_selector"]
> * [Azure 클래식 포털](virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [PowerShell: 클래식 배포](virtual-machines-windows-classic-create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
>
>

<br>

> [!IMPORTANT]
> Azure에는 리소스를 만들고 작업하기 위한 [리소스 관리자 및 클래식](../azure-resource-manager/resource-manager-deployment-model.md)라는 두 가지 배포 모델이 있습니다. 이 문서에서는 클래식 배포 모델 사용에 대해 설명합니다. 새로운 배포는 대부분 리소스 관리자 모델을 사용하는 것이 좋습니다. **Azure Portal**을 통해 [Resource Manager 배포 모델을 사용하여 이러한 단계를 수행하는 방법](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 알아봅니다.

이 자습서에서는 Azure Portal에서 Windows를 실행하는 Azure VM(가상 컴퓨터)을 만드는 방법을 보여 줍니다. 한 예로, Windows Server 이미지를 사용할 것이지만, 해당 아미지는 Azure가 제공하는 여러 이미지 중 하나일 뿐입니다. 참고: 이미지 선택은 구독에 따라 달라집니다. 예를 들어 Windows 데스크톱 이미지는 MSDN 구독자가 사용할 수 있습니다.

이 섹션에서는 Azure Portal에서 **대시보드**를 사용하여 가상 컴퓨터를 선택하고 만드는 방법을 보여 줍니다.

[사용자 고유의 이미지](virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)를 사용하여 VM을 만들 수도 있습니다. 이 방법 및 다른 방법에 대한 자세한 내용은 [Windows 가상 컴퓨터를 만드는 다양한 방법](virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 참조하세요.

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <a id="createvirtualmachine"> </a>가상 컴퓨터 만들기
[!INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>다음 단계
* Azure Portal에서 [Resource Manager 배포 모델을 사용하여 VM을 만드는 방법](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)을 알아봅니다.
* 가상 컴퓨터에 로그온합니다. 지침은 [Windows Server를 실행하는 가상 컴퓨터에 로그온](virtual-machines-windows-classic-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)을 참조하세요.
* 데이터를 저장할 디스크를 연결합니다. 빈 디스크와 데이터가 포함된 디스크를 모두 연결할 수 있습니다. 지침은 [클래식 배포 모델을 사용하여 만든 Windows 가상 컴퓨터에 데이터 디스크 연결](virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)을 참조하세요.

