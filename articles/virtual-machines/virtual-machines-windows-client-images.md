---
title: "Azure에서 Windows 클라이언트 이미지 사용 | Microsoft Docs"
description: "Visual Studio 구독 혜택을 사용하여 개발/테스트 시나리오용으로 Azure에서 Windows 7/8/10을 배포하는 방법"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 91c3880a-cede-44f1-ae25-f8f9f5b6eaa4
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/28/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 568bd06d1dfd0e253b960dcf2fb5409a390da91b
ms.openlocfilehash: 2116abf974974c285d94d673b6a614360edbc593


---
# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>개발/테스트 시나리오용으로 Azure에서 Windows 클라이언트 사용
적절한 Visual Studio(이전의 MSDN) 구독이 있으면 Azure에서 개발/테스트 시나리오에 Windows 7, Windows 8 또는 Windows 10을 사용할 수 있습니다. 이 문서에서는 Azure에서 Windows 클라이언트를 실행하고 Azure 갤러리 이미지를 사용하기 위한 적격성 요구 사항에 대해 대략적으로 설명합니다.

## <a name="subscription-eligibility"></a>구독 적격성
활성 Visual Studio 구독자, 즉 Visual Studio 구독 라이선스를 받은 사용자는 개발 및 테스트용으로 Windows 클라이언트를 사용할 수 있습니다. Windows 클라이언트는 모든 유형의 Azure 구독에서 실행 중인 Azure 가상 컴퓨터와 사용자의 자체 하드웨어에서 사용 가능합니다. Windows 클라이언트는 일반적인 프로덕션용으로 배포하거나 Azure에서 사용할 수 없으며, 활성 Visual Studio 구독자가 아닌 사용자는 사용할 수 없습니다.

편의상 Azure 갤러리의 [적격 개발/테스트 제품](#eligible-offers)내에서 특정 Windows 10 이미지를 제공하고 있습니다. Visual Studio 구독자(사용 중인 제품 유형은 관계없음)는 64비트 Windows 7, Windows 8 또는 Windows 10 이미지를 [적절하게 준비하고 작성](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)한 다음 [Azure에 업로드](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)할 수도 있습니다. 이렇게 업로드하는 이미지 역시 활성 Visual Studio 구독자가 개발/테스트용으로만 사용할 수 있습니다.

## <a name="eligible-offers"></a>적격 제품
아래 테이블에는 Azure 갤러리를 통해 Windows 10을 배포할 수 있는 제품 ID가 자세히 나와 있습니다. Windows 10 이미지는 다음 제품에 대해서만 표시됩니다. 다른 제품 유형에서 Windows 클라이언트를 실행해야 하는 Visual Studio 구독자는 64비트 Windows 7, Windows 8 또는 Windows 10 이미지를 [적절하게 준비하고 작성](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)한 다음 [Azure에 업로드](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)해야 합니다.

| 제품 이름 | 제품 번호 | 사용 가능한 클라이언트 이미지 |
|:--- |:---:|:---:|
| [종량제 개발/테스트](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise(MPN) 구독자](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional 구독자](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional 구독자](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium with MSDN(혜택)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise 구독자](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise(BizSpark) 구독자](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Enterprise 개발/테스트](https://azure.microsoft.com/ofers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Azure 구독 확인
제품 ID를 모르는 경우 Azure 포털 또는 계정 포털을 통해 확인할 수 있습니다.

구독 제품 ID는 Azure 포털 내의 '구독' 블레이드에 표시됩니다.

![Azure 포털의 제품 ID 세부 정보](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Azure 계정 포털의 ['구독' 탭](http://account.windowsazure.com/Subscriptions) 에서도 제품 ID를 확인할 수 있습니다.

![Azure 계정 포털의 제품 ID 세부 정보](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 

## <a name="next-steps"></a>다음 단계
이제 [PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), [Resource Manager 템플릿](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 또는 [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)를 사용하여 VM을 배포할 수 있습니다.




<!--HONumber=Nov16_HO5-->


