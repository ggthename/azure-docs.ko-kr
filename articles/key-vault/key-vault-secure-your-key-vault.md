---
title: Azure Key Vault 보안 | Microsoft Docs
description: Azure Key Vault, 키 및 비밀을 관리하기 위해 키 자격 증명 모음의 액세스 권한을 관리합니다. 키 자격 증명 모음의 인증 및 권한 부여 모델과 키 자격 증명 모음을 보호하는 방법에 대해 설명합니다.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 4a1de3c011f1f8cfa1ea9246efad4ebb7f9e3a76
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364526"
---
# <a name="secure-your-key-vault"></a>키 자격 증명 모음 보안
Azure Key Vault는 암호화 키와 비밀(예: 인증서, 연결 문자열, 암호)를 보호하는 클라우드 서비스입니다. 이 데이터는 민감하고 업무상 중요하기 때문에 키 자격 증명 모음에 대한 액세스는 보호해야 하며 권한이 부여된 응용 프로그램과 사용자만 액세스할 수 있도록 해야 합니다. 

이 문서에서는 키 자격 증명 모음 액세스 모델에 대한 개요를 제공합니다. 여기서는 인증 및 권한 부여에 대해 설명하고 키 자격 증명 모음 액세스의 보안을 유지하는 방법을 설명합니다.

## <a name="overview"></a>개요
키 자격 증명 모음에 대한 액세스는 별도의 두 인터페이스, 즉 관리 평면 및 데이터 평면을 통해 제어됩니다. 두 평면에서 적절한 인증 및 권한 부여를 갖추고 있어야 호출자(사용자 또는 응용 프로그램)에서 키 자격 증명 모음에 액세스할 수 있습니다. 인증은 호출자의 ID를 확인하는 반면, 권한 부여는 호출자에서 수행할 수 있는 작업을 결정합니다.

인증을 위해 데이터 평면과 관리 평면 모두 Azure Active Directory를 사용합니다. 권한 부여를 위해서는 관리 평면에서 RBAC(역할 기반 액세스 제어)를 사용하는 반면 데이터 평면에서는 키 자격 증명 모음 액세스 정책을 사용합니다.

여기서 다루는 항목에 대한 간략한 개요는 다음과 같습니다.

[Azure Active Directory를 통한 인증](#authentication-using-azure-active-directory) - 관리 평면과 데이터 평면을 통해 키 자격 증명 모음에 액세스하기 위해 호출자에서 Azure Active Directory를 통해 인증하는 방법을 설명합니다. 

[관리 평면 및 데이터 평면](#management-plane-and-data-plane) - 관리 평면과 데이터 평면은 키 자격 증명 모음에 액세스하는 데 사용되는 두 액세스 평면입니다. 각 액세스 평면마다 특정 작업을 지원합니다. 여기서는 각 평면에서 사용하는 액세스 엔드포인트, 지원되는 작업 및 액세스 제어 방법을 설명합니다. 

[관리 평면 액세스 제어](#management-plane-access-control) - 관리 평면 작업에 액세스하기 위해 역할 기반 액세스 제어를 사용하는 방법에 대해 설명합니다.

[데이터 평면 액세스 제어](#data-plane-access-control) - 데이터 평면 액세스를 제어하기 위해 키 자격 증명 모음 액세스 정책을 사용하는 방법에 대해 설명합니다.

[예제](#example) - 별도의 세 팀(보안 팀, 개발자/운영자 및 감사자)을 특정 작업을 수행하여 Azure에서 응용 프로그램을 개발, 관리 및 모니터링할 수 있도록 키 자격 증명 모음에 대한 액세스 제어를 설정하는 방법에 대해 설명합니다.

## <a name="authentication-using-azure-active-directory"></a>Azure Active Directory를 통한 인증
Azure 구독에 키 자격 증명 모음을 만들 때 해당 구독의 Azure Active Directory 테넌트에 자동으로 연결됩니다. 모든 호출자(사용자 및 응용 프로그램)가 해당 테넌트에 등록되어 있어야 하고 해당 키 자격 증명 모음에 액세스하기 위해 인증을 받아야 합니다. 이 요구 사항은 관리 평면과 데이터 평면 액세스에 모두 적용됩니다. 두 경우 모두 응용 프로그램에서 다음과 같은 두 가지 방법으로 키 자격 증명 모음에 액세스할 수 있습니다.

* **사용자+앱 액세스** - 로그인한 사용자를 대신하여 키 자격 증명 모음에 액세스하는 응용 프로그램에서 사용합니다. Azure PowerShell과 Azure Portal이 이러한 액세스 유형의 예입니다. 사용자에게 액세스 권한을 부여하는 방법에는 다음 두 가지가 있습니다. 
- 모든 응용 프로그램에서 키 자격 증명 모음에 액세스할 수 있도록 사용자에게 액세스 권한을 부여합니다.
- 특정 응용 프로그램을 사용하는 경우에만 사용자에게 키 자격 증명 모음 액세스 권한을 부여합니다(복합 ID라고도 함).

* **앱 전용 액세스** - 디먼 서비스 또는 백그라운드 작업 등을 실행하는 응용 프로그램에서 사용합니다. 키 자격 증명 모음에 대한 액세스 권한이 응용 프로그램의 ID에 부여됩니다.

두 유형의 응용 프로그램 모두에서 [지원되는 인증 방법](../active-directory/develop/authentication-scenarios.md) 중 하나를 사용하여 Azure Active Directory를 통해 인증하고 토큰을 가져옵니다. 응용 프로그램 유형에 따라 사용되는 인증 방법이 다릅니다. 그런 다음 응용 프로그램에서 이 토큰을 사용하고 REST API 요청을 키 자격 증명 모음에 보냅니다. 관리 평면 요청은 Azure Resource Manager 엔드포인트를 통해 라우팅됩니다. 데이터 평면에 액세스하는 경우 응용 프로그램에서 키 자격 증명 모음에 직접 통신합니다. 자세한 내용은 [전체 인증 흐름](../active-directory/develop/v1-protocols-oauth-code.md)을 참조하십시오. 

응용 프로그램에서 토큰을 요청하는 리소스 이름은 응용 프로그램에서 관리 평면과 데이터 평면 중 어느 것에 액세스하는지에 따라 다릅니다. 따라서 리소스 이름은 Azure 환경에 따라 뒷부분의 표에서 설명하는 관리 평면 또는 데이터 평면 엔드포인트 중 하나입니다.

관리 평면과 데이터 평면 모두에 대한 인증을 위해 하나의 메커니즘이 있으면 다음과 같은 이점이 있습니다.

* 조직에서는 조직의 모든 키 자격 증명 모음에 대한 액세스를 중앙에서 제어할 수 있습니다.
* 사용자가 떠나는 경우 곧바로 조직의 모든 키 자격 증명 모음에 대한 액세스 권한을 잃게 됩니다.
* 조직은 Azure Active Directory에 있는 옵션(예: 추가 보안을 위해 다단계 인증 사용)을 통해 인증을 사용자 지정할 수 있습니다.

## <a name="management-plane-and-data-plane"></a>관리 평면 및 데이터 평면
Azure Key Vault는 Azure Resource Manager 배포 모델을 통해 사용할 수 있는 Azure 서비스입니다. 키 자격 증명 모음을 만들면 키, 비밀 및 인증서와 같은 중요한 개체를 저장하기 위한 가상 컨테이너가 생깁니다. 관리 평면 및 데이터 평면 인터페이스를 사용하여 키 자격 증명 모음에서 특정 작업이 수행됩니다. 관리 평면은 키 자격 증명 모음 자체를 관리하는 데 사용됩니다. 여기에는 특성 관리 및 데이터 평면 액세스 정책 설정과 같은 작업이 포함됩니다. 데이터 평면 인터페이스는 키 자격 증명 모음에 저장된 키, 비밀 및 인증서를 추가, 삭제, 수정 및 사용하는 데 사용됩니다.

관리 평면 및 데이터 평면 인터페이스는 아래 표시된 다른 엔드포인트를 통해 액세스됩니다. 두 번째 열에서는 다양한 Azure 환경에서 이러한 엔드포인트에 대한 DNS 이름을 설명합니다. 세 번째 열에서는 각 액세스 평면에서 수행할 수 있는 작업을 설명합니다. 각 액세스 평면에는 자체 액세스 제어 메커니즘도 있습니다. 관리 평면 액세스 제어는 Azure Resource Manager RBAC(역할 기반 액세스 제어)를 사용하여 설정됩니다. 데이터 평면 액세스 제어는 키 자격 증명 모음 액세스 정책을 사용하여 설정됩니다.

| 액세스 평면 | 액세스 엔드포인트 | 작업 | 액세스 제어 메커니즘 |
| --- | --- | --- | --- |
| 관리 평면 |**전역:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure 미국 정부:**<br> management.usgovcloudapi.net:443<br><br> **Azure 독일:**<br> management.microsoftazure.de:443 |키 자격 증명 모음 만들기/읽기/업데이트/삭제 <br> 키 자격 증명 모음에 대한 액세스 정책 설정<br>키 자격 증명 모음에 대한 태그 설정 |Azure Resource Manager 역할 기반 Access Control(RBAC) |
| 데이터 평면 |**전역:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure 미국 정부:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure 독일:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |키: 암호 해독, 암호화, UnwrapKey, WrapKey, 확인, 로그인, 권한 가져오기, 목록 표시, 업데이트, 만들기, 가져오기, 삭제, Backup, 복원<br><br> 암호: 권한 가져오기, 목록 표시, 설정, 삭제 |키 자격 증명 모음 액세스 정책 |

관리 평면과 데이터 평면 액세스 제어는 독립적으로 작동합니다. 예를 들어, 응용 프로그램에 키 자격 증명 모음의 키를 사용하기 위한 액세스 권한을 부여하려면 데이터 평면 액세스 권한만 부여하면 됩니다. 액세스 권한은 키 자격 증명 모음 액세스 정책을 통해 부여됩니다. 반대로, 자격 증명 모음 속성 및 태그를 읽어야 하지만 데이터(키, 비밀 또는 인증서)에 액세스할 수 없는 사용자는 제어 평면 액세스 권한만 있으면 됩니다. RBAC을 사용하여 사용자에게 ‘읽기’ 액세스 권한을 지정함으로써 액세스 권한이 부여됩니다.

## <a name="management-plane-access-control"></a>관리 평면 액세스 제어
관리 평면은 키 자격 증명 모음 자체에 영향을 주는 다음과 같은 작업으로 구성됩니다.

- 키 자격 증명 모음 만들기 또는 삭제
- 구독의 자격 증명 모음 목록 가져오기
- 키 자격 증명 모음 속성(예: SKU, 태그 등) 검색
- 키 및 비밀에 대한 사용자 및 응용 프로그램 액세스를 제어하는 키 자격 증명 모음 액세스 정책 설정

관리 평면 액세스 제어는 RBAC를 사용합니다. 이전 섹션의 표에서 관리 평면을 통해 수행할 수 있는 키 자격 증명 모음 작업의 전체 목록을 참조하세요. 

### <a name="role-based-access-control-rbac"></a>RBAC(역할 기반 Access Control)
각 Azure 구독에는 Azure Active Directory가 있습니다. 이 디렉터리의 사용자, 그룹 및 응용 프로그램에 Resource Manager 배포 모델을 사용하는 Azure 구독의 리소스를 관리할 수 있는 액세스 권한을 부여할 수 있습니다. 이러한 유형의 액세스 제어를 RBAC(역할 기반 Access Control)라고 합니다. 이 액세스를 관리하기 위해 [Azure 포털](https://portal.azure.com/), [Azure CLI 도구](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) 또는 [Azure Resource Manager REST API](https://msdn.microsoft.com/library/azure/dn906885.aspx)를 사용할 수 있습니다.

리소스 그룹에 키 자격 증명 모음을 만들고 Azure Active Directory를 사용하여 관리 평면에 대한 액세스를 제어합니다. 예를 들어, 리소스 그룹의 키 자격 증명 모음을 관리할 수 있는 권한을 사용자 또는 그룹에게 부여할 수 있습니다.

적절한 RBAC 역할을 할당하여 특정 범위에 속한 사용자, 그룹 및 응용 프로그램에 액세스 권한을 부여할 수 있습니다. 예를 들어, 키 자격 증명 모음을 관리하기 위해 사용자에게 액세스 권한을 부여하려면 특정 범위에 속한 해당 사용자에게 미리 정의된 ‘Key Vault 참가자’ 역할을 할당합니다. 이 경우 범위는 구독, 리소스 그룹 또는 특정 키 자격 증명 모음입니다. 구독 수준에서 할당된 역할은 해당 구독 내 모든 리소스 그룹 및 리소스에 적용됩니다. 리소스 그룹 수준에서 할당된 역할은 해당 리소스 그룹의 모든 리소스에 적용됩니다. 특정 리소스에 할당된 역할이 해당 리소스에 적용됩니다. 미리 정의된 여러 역할([RBAC: 기본 제공 역할](../role-based-access-control/built-in-roles.md) 참조)이 있습니다. 미리 정의된 역할이 요구에 맞지 않는 경우 고유한 역할을 정의할 수 있습니다.

> [!IMPORTANT]
> 사용자에게 키 자격 증명 모음 관리 평면에 대한 참가자 권한(RBAC)이 있는 경우 이 사용자는 데이터 평면에 대한 액세스를 제어하는 키 자격 증명 모음 액세스 정책을 설정하여 자신에게 데이터 평면 액세스 권한을 부여할 수 있습니다. 따라서 권한이 있는 사람만 자신의 키 자격 증명 모음, 키, 암호 및 인증서를 액세스하고 관리할 수 있도록 해당 키 자격 증명 모음에 대해 '참가자' 액세스 권한을 갖는 사용자를 엄격하게 제어하는 것이 좋습니다.
> 
> 

## <a name="data-plane-access-control"></a>데이터 평면 액세스 제어
키 자격 증명 모음 데이터 평면 작업은 키, 비밀 및 인증서와 같은 저장된 개체에 적용됩니다. 키 작업에는 키 만들기, 가져오기, 업데이트, 나열, 백업 및 복원이 포함됩니다. 암호화 작업에는 서명, 확인, 암호화, 암호 해독, 래핑, 래핑 해제, 태그 및 기타 키 특성 설정이 포함됩니다. 마찬가지로 비밀 관련 작업에는 가져오기, 설정, 나열, 삭제가 포함됩니다.

데이터 평면 액세스 권한은 키 자격 증명 모음에 대한 액세스 정책을 설정함으로써 부여됩니다. 사용자, 그룹 또는 응용 프로그램은 키 자격 증명 모음의 관리 평면에 대해 참가자 권한(RBAC)이 있어야 해당 키 자격 증명 모음에 대한 액세스 정책을 설정할 수 있습니다. 키 자격 증명 모음의 키 또는 암호에 대해 특정 작업을 수행하기 위해 사용자, 그룹 또는 응용 프로그램에 액세스 권한을 부여할 수 있습니다. 각 키 자격 증명 모음은 최대 1024개 액세스 정책 항목을 지원합니다. Azure Active Directory 보안 그룹을 만들고 해당 그룹에 사용자를 추가하여 여러 사용자에게 키 자격 증명 모음의 데이터 평면에 대한 액세스 권한을 부여하십시오.

### <a name="key-vault-access-policies"></a>키 자격 증명 모음 액세스 정책
키 자격 증명 모음 액세스 정책은 키, 비밀 및 인증서에 대한 권한을 별도로 부여합니다. 예를 들어, 사용자에게 키에 대한 액세스 권한만 부여하고 비밀에 대해서는 권한을 부여하지 않을 수 있습니다. 그러나 키, 비밀 또는 인증서에 대한 액세스 권한은 자격 증명 모음 수준에서 지정됩니다. 키 자격 증명 모음 액세스 정책은 특정 키/비밀/인증서와 같은 세분화된 개체 수준 권한을 지원하지 않습니다. [Azure 포털](https://portal.azure.com/), [Azure CLI 도구](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) 또는 [키 자격 증명 모음 관리 REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx)를 사용하면 자격 증명 모음에 대한 액세스 정책을 설정할 수 있습니다.

> [!IMPORTANT]
> 키 자격 증명 모음 액세스 정책은 자격 증명 모음 수준에 적용됩니다. 예를 들어 사용자에게 키를 만들고 삭제하는 권한이 부여되면 해당 사용자는 키 자격 증명 모음의 모든 키에 대해 이러한 작업을 수행할 수 있습니다.

액세스 정책 외에도 데이터 평면 액세스는 보안의 추가 레이어에 대한 [방화벽 및 가상 네트워크 규칙](key-vault-network-security.md)을 구성하여 [Azure Key Vault에 대한 Virtual Network 서비스 엔드포인트](key-vault-overview-vnet-service-endpoints.md)를 사용하여 제한될 수도 있습니다.

## <a name="example"></a>예
SSL에는 인증서를, 데이터 저장에는 Azure Storage를, 로그인 작업에는 RSA 2048비트 키를 사용하는 응용 프로그램을 개발하고, 이 응용 프로그램을 Azure Virtual Machine(또는 가상 머신 확장 집합)에서 실행한다고 가정합니다. 이 경우 키 자격 증명 모음을 사용하여 모든 응용 프로그램 비밀을 저장하고, 응용 프로그램에서 Azure Active Directory에서 인증을 받는 데 사용하는 부트스트랩 인증서를 저장할 수 있습니다.

다음은 키 자격 증명 모음에 저장된 키 및 비밀 유형에 대한 요약입니다.

* **SSL 인증서** - SSL에 사용
* **저장소 키** - 저장소 계정에 액세스하는 데 사용
* **RSA 2048비트 키** - 서명 작업에 사용
* **부트스트랩 인증서** - Azure Active Directory에서 인증을 받는 데 사용 액세스 권한이 부여되면 저장소 키를 가져오고 서명을 위해 RSA 키를 사용할 수 있습니다.

이제 이 응용 프로그램을 관리, 배포 및 감사하는 사람들을 만나 보겠습니다. 이 예제에서는 세 가지 역할을 사용합니다.

* **보안 팀** - 일반적으로 ‘CSO(최고 보안 책임자) 사무실’ IT 직원 또는 동등 직급의 사람입니다. 이 팀은 비밀의 적절한 보호를 담당합니다. 예를 들어, SSL 인증서, 서명에 사용되는 RSA 키, 연결 문자열 및 저장소 계정 키가 여기에 해당합니다.
* **개발자/운영자** - 이 응용 프로그램을 개발한 다음, Azure에 배포하는 직원입니다. 일반적으로 보안 팀에 속하지 않으므로 SSL 인증서 및 RSA 키와 같은 중요한 데이터에 액세스할 수 없습니다. 배포하는 응용 프로그램에서만 해당 개체에 액세스할 수 있어야 합니다.
* **감사자** - 대개 개발자 및 일반 IT 직원과 격리된 다양한 사람입니다. 인증서, 키 및 비밀의 사용 및 유지 관리를 검토하고 보안 표준을 준수하도록 지시할 책임이 있습니다. 

여기서는 이 응용 프로그램 범위를 벗어나지만 관련이 있는 추가 역할에 대해 설명합니다. 해당 역할은 구독(또는 리소스 그룹) 관리자입니다. 구독 관리자는 보안 팀의 초기 액세스 권한을 설정합니다. 구독 관리자는 이 응용 프로그램에 필요한 리소스를 포함하는 리소스 그룹을 사용하여 보안 팀에 액세스 권한을 부여합니다.

이제 이 응용 프로그램의 상황에서 각 역할이 수행하는 작업을 살펴 보겠습니다.

* **보안 팀**
  * 키 자격 증명 모음 만들기
  * 키 자격 증명 모음 로깅 사용 설정
  * 키/암호 추가
  * 재해 복구를 위한 키 백업 만들기
  * 키 자격 증명 모음 액세스 정책을 설정하여 사용자와 응용 프로그램에서 특정 작업을 수행할 수 있는 권한 부여
  * 주기적 키/암호 롤백
* **개발자/운영자**
  * 보안 팀에서 부트스트랩 및 SSL 인증서(지문), 저장소 키(비밀 URI) 및 서명 키(키 URI)에 대한 참조 가져오기
  * 프로그래밍 방식으로 키와 암호에 액세스하는 응용 프로그램 개발 및 배포
* **감사자**
  * 사용 현황 로그를 검토하여 적절한 키/암호 사용 및 데이터 보안 표준 준수 확인

이제 할당된 작업을 수행하기 위해 각 역할 및 응용 프로그램에 필요한 액세스 권한을 확인해 보겠습니다. 

| 사용자 역할 | 관리 평면 사용 권한 | 데이터 평면 사용 권한 |
| --- | --- | --- |
| 보안 팀 |키 자격 증명 모음 참가자 |키: 백업, 만들기, 삭제, 권한 가져오기, 가져오기, 목록 표시, 복원 <br> 암호: 모두 |
| 개발자/운영자 |배포하는 VM에서 키 자격 증명 모음의 암호를 가져올 수 있게 하는 키 자격 증명 모음 배포 권한 |없음 |
| 감사자 |없음 |키: 목록 표시<br>암호: 목록 표시 |
| 응용 프로그램 |없음 |키: 로그인<br>암호: 권한 가져오기 |

> [!NOTE]
> 감사자에게는 키와 암호에 대한 목록 사용 권한이 필요하므로 태그, 활성화 및 만료 날짜와 같이 로그에서 내보내지 않은 키와 암호의 특성을 검사할 수 있습니다.
> 
> 

세 가지 역할은 키 자격 증명 모음 권한 외에, 다른 리소스에 대한 액세스 권한이 필요합니다. 예를 들어 VM(또는 Web Apps 등)을 배포할 수 있으려면 개발자/운영자에게도 해당 리소스 형식에 대한 '참가자' 액세스 권한이 필요합니다. 감사자에게는 키 자격 증명 모음 로그를 저장할 저장소 계정에 대한 ‘읽기’ 액세스 권한이 필요합니다.

이 문서는 키 자격 증명 모음 액세스 보안을 유지하는 데 주안점을 두므로, 이 주제와 관련된 개념만 다룹니다. 인증서 배포, 프로그래밍 방식의 키 및 비밀 액세스와 관련된 세부 사항과 기타 내용은 다른 문서에서 다룹니다. 예:

- 키 자격 증명 모음에 저장된 인증서를 VM에 저장된 배포하는 작업은 [blog post: Deploy Certificates to VMs from customer-managed Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)(블로그 게시물: 고객 관리 Key Vault에서 VM으로 인증서 배포)에 설명되어 있습니다.
- [Azure Key Vault 클라이언트 샘플 다운로드](https://www.microsoft.com/download/details.aspx?id=45343)에서는 부트스트랩 인증서를 사용하여 키 자격 증명 모음에 액세스하기 위해 Azure AD에서 인증을 받는 방법을 설명합니다.

대부분의 액세스 권한은 Azure Portal을 사용하여 부여할 수 있습니다. 세분화된 사용 권한을 부여하려는 경우 원하는 결과를 얻기 위해 Azure PowerShell 또는 CLI를 사용해야 할 수 있습니다. 

여기서 보여 주는 PowerShell 조각은 다음과 같이 가정합니다.

* Azure Active Directory 관리자가 세 가지 역할, 즉 Contoso Security Team, Contoso App Devops, Contoso App Auditors를 보여 주는 보안 그룹을 만들었습니다. 또한 관리자는 자신이 속한 그룹에 사용자를 추가했습니다.
* **ContosoAppRG**는 모든 리소스가 있는 리소스 그룹입니다. 로그는 **contosologstorage**에 저장됩니다. 
* **ContosoKeyVault** 키 자격 증명 모음 및 **contosologstorage** 키 자격 증명 모음 로그에 사용되는 저장소 계정은 동일한 Azure 위치에 있어야 합니다.

먼저 구독 관리자가 보안 팀에 '키 자격 증명 모음 참가자' 및 '사용자 액세스 관리자' 역할을 할당합니다. 이러한 역할을 사용하여 보안 팀에서 다른 리소스에 대한 액세스를 관리하고 ContosoAppRG 리소스 그룹의 키 자격 증명 모음을 관리할 수 있습니다.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

다음 스크립트는 보안 팀이 키 자격 증명 모음을 만들고, 로깅 및 액세스 권한을 설정하는 방법을 보여 줍니다. Key Vault 액세스 정책 권한에 대한 자세한 내용은 [Azure Key Vault 키, 비밀 및 인증서 정보](about-keys-secrets-and-certificates.md)를 참조하세요.

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

정의된 사용자 지정 역할은 ContosoAppRG 리소스 그룹을 만든 구독에만 할당할 수 있습니다. 동일한 사용자 지정 역할이 다른 구독의 다른 프로젝트에 사용되는 경우 해당 범위에 더 많은 구독이 추가될 수 있습니다.

개발자/운영자의 사용자 지정 역할로 “배포/작업” 권한을 할당하면 범위가 리소스 그룹으로 지정됩니다. 이를 통해 리소스 그룹 ‘ContosoAppRG’에 만든 VM만 비밀(SSL 인증서 및 부트스트랩 인증서)에 액세스하도록 할 수 있습니다. 개발자/운영자 팀 멤버가 다른 리소스 그룹에 만든 VM은 비밀 URI가 있더라도 이러한 비밀에 액세스할 수 없습니다.

이 예제에서는 간단한 시나리오를 보여 줍니다. 실생활 시나리오에서는 더 복잡할 수 있으며 필요에 따라 키 자격 증명 모음에 대한 권한을 조정해야 할 수도 있습니다. 이 예제에서는 개발자/운영자가 응용 프로그램에서 참조해야 하는 키 및 비밀 참조(URI 및 지문)를 보안 팀에서 제공한다고 가정합니다. 개발자/운영자에게는 데이터 평면 액세스가 필요하지 않습니다. 이 예제에서는 키 자격 증명 모음 보호에 중점을 둡니다. 마찬가지로 [VM](https://azure.microsoft.com/services/virtual-machines/security/), [저장소 계정](../storage/common/storage-security-guide.md) 및 기타 Azure 리소스 보안도 고려해야 합니다.

> [!NOTE]
> 참고: 이 예제에서는 프로덕션 환경에서 키 자격 증명 모음 액세스를 잠글 수도 있습니다. 개발자는 개발한 응용 프로그램에 속한 자격 증명 모음, VM 및 저장소 계정을 관리할 수 있는 모든 권한을 갖춘 구독 또는 리소스 그룹을 가지고 있어야 합니다.

[Key Vault 방화벽 및 가상 네트워크를 구성](key-vault-network-security.md)하여 키 자격 증명 모음에 대한 액세스를 더 보호하는 것이 좋습니다.

## <a name="resources"></a>리소스
* [Azure Active Directory 역할 기반 Access Control](../role-based-access-control/role-assignments-portal.md)
  
  이 문서에서는 Azure Active Directory 역할 기반 Access Control 및 작동 방식에 대해 설명합니다.
* [RBAC: 기본 제공 역할](../role-based-access-control/built-in-roles.md)
  
  이 문서에서는 RBAC에서 사용할 수 있는 모든 기본 제공 역할에 대해 자세히 설명합니다.
* [리소스 관리자 배포 및 클래식 배포 이해](../azure-resource-manager/resource-manager-deployment-model.md)
  
  이 문서에서는 리소스 관리자 배포 및 기존 배포 모델에 대해 설명하고 리소스 관리자를 사용할 때와 리소스 그룹을 사용할 때의 이점에 대해서도 설명합니다.
* [Azure PowerShell을 사용하여 역할 기반 Access Control 관리](../role-based-access-control/role-assignments-powershell.md)
  
  이 문서에서는 Azure PowerShell을 사용하여 역할 기반 액세스 제어를 관리하는 방법에 대해 설명합니다.
* [REST API를 사용하여 역할 기반 Access Control 관리](../role-based-access-control/role-assignments-rest.md)
  
  이 문서에서는 REST API를 사용하여 RBAC를 관리하는 방법을 보여 줍니다.
* [Ignite를 사용한 Microsoft Azure에 대한 역할 기반 Access Control](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  이 2015 Microsoft Ignite 컨퍼런스 동영상은 Azure의 액세스 관리 및 보고 기능에 대해 설명합니다. 또한 Azure Active Directory를 사용하여 Azure 구독에 대한 액세스를 보호하는 모범 사례도 알아봅니다.
* [OAuth 2.0 및 Azure Active Directory를 사용하여 웹 응용 프로그램에 대한 액세스 권한 부여](../active-directory/develop/v1-protocols-oauth-code.md)
  
  이 문서에서는 Azure Active Directory로 인증하기 위한 OAuth 2.0 흐름 전체에 대해 설명합니다.
* [키 자격 증명 모음 관리 REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  이 문서는 키 자격 증명 모음 액세스 정책 설정을 포함하여 프로그래밍 방식으로 키 자격 증명 모음을 관리하는 REST API에 대한 참조입니다.
* [ 키 자격 증명 모음 REST API](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  키 자격 증명 모음 REST API 참조 문서에 대한 링크입니다.
* [키 액세스 제어](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  암호 액세스 제어 참조 문서에 대한 링크입니다.
* [암호 액세스 제어](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  키 액세스 제어 참조 문서에 대한 링크입니다.
* PowerShell을 사용하여 키 자격 증명 모음 액세스 정책 [설정](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) 및 [제거](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy)
  
  키 자격 증명 모음 액세스 정책을 관리하는 PowerShell cmdlet 참조 문서에 대한 링크입니다.

## <a name="next-steps"></a>다음 단계
[키 자격 증명 모음 방화벽 및 가상 네트워크 구성](key-vault-network-security.md)

관리자를 위한 시작 자습서에 대해서는 [Azure 키 자격 증명 모음 시작](key-vault-get-started.md)을 참조하십시오.

키 자격 증명 모음의 사용 현황 로깅에 대한 자세한 내용은 [Azure 키 자격 증명 모음 로깅](key-vault-logging.md)을 참조하십시오.

Azure 키 자격 증명 모음에서 키 및 암호를 사용하는 방법에 대한 자세한 내용은 [키 및 암호 정보](https://msdn.microsoft.com/library/azure/dn903623.aspx)를 참조하십시오.

키 자격 증명 모음에 대한 질문이 있으면 [Azure Key Vault 포럼](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)을 방문하십시오.

