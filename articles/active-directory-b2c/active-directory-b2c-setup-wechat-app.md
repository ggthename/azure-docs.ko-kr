---
title: Azure Active Directory B2C를 사용하여 WeChat 계정으로 등록 설정 및 로그인 | Microsoft Docs
description: 고객에게 Azure Active Directory B2C를 사용하여 응용 프로그램에서 WeChat 계정으로 등록 및 로그인을 제공합니다.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 953868f90c4f761b1d02c314e0e1a4e04b8404d9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842614"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C를 사용하여 WeChat 계정으로 등록 설정 및 로그인

> [!NOTE]
> 이 기능은 미리 보기 상태입니다.
> 

## <a name="create-a-wechat-application"></a>WeChat 응용 프로그램 만들기

Azure AD(Azure Active Directory) B2C에서 WeChat 계정을 ID 공급자로 사용하려면 테넌트에 해당 계정을 나타내는 응용 프로그램을 만들어야 합니다. WeChat 계정이 없는 경우 [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html)에서 정보를 얻을 수 있습니다.

### <a name="register-a-wechat-application"></a>WeChat 응용 프로그램 등록

1. WeChat 자격 증명을 사용하여 [https://open.weixin.qq.com/](https://open.weixin.qq.com/)에 로그인합니다.
2. **管理中心**(관리 센터)를 선택합니다.
3. 새 응용 프로그램을 등록하기 위한 단계를 따릅니다.
4. **授权回调域**(콜백 URL)에 `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`를 입력합니다. 예를 들어 테넌트 이름이 contoso인 경우 URL을 `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`로 설정합니다.
5. **앱 ID** 및 **앱 키**를 복사합니다. 테넌트에 ID 공급자를 추가하려면 이러한 항목이 필요합니다.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>테넌트에서 WeChat을 ID 공급자로 구성

1. Azure AD B2C 테넌트의 전역 관리자로 [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. Azure AD B2C 테넌트를 포함하는 디렉터리를 사용하려면 위쪽 메뉴에서 **디렉터리 및 구독 필터**를 클릭하고 테넌트가 포함된 디렉터리를 선택합니다.
3. Azure Portal의 왼쪽 상단 모서리에서 **모든 서비스**를 선택하고 **Azure AD B2C**를 검색하여 선택합니다.
4. **ID 공급자**를 선택한 다음, **추가**를 선택합니다.
5. **이름**을 제공합니다. 예를 들어 *WeChat*을 입력합니다.
6. **ID 공급자 형식**을 클릭하고 **WeChat(미리 보기)** 을 선택한 다음, **확인**을 클릭합니다.
7. **이 ID 공급자 설정**을 선택하고 이전에 기록한 앱 ID를 **클라이언트 ID**로 입력한 후, 기록한 앱 키를 이전에 만든 WeChat 응용 프로그램의 **클라이언트 암호**로 입력합니다.
8. **확인**을 클릭한 다음 **만들기**를 클릭하여 WeChat 구성을 저장합니다.

