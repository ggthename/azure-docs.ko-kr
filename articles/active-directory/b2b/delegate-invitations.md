---
title: Azure Active Directory B2B 공동 작업에 대한 초대 위임 | Microsoft Docs
description: Azure Active Directory B2B 공동 작업 사용자 속성은 구성 가능합니다.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: fd00fb8da3cf36518f9e28e827e59fd7ff45d687
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622213"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B 공동 작업에 대한 초대 위임

Azure AD(Azure Active Directory) B2B 공동 작업을 사용하면 전역 관리자가 아니더라도 초대할 보낼 수 있습니다. 그 대신 정책을 사용하여 초대를 보낼 수 있는 역할을 가진 사용자에게 초대를 위임할 수 있습니다. 게스트 사용자 초대를 위임하는 중요한 새 방식은 게스트 초대자 역할을 사용하는 것입니다.

## <a name="guest-inviter-role"></a>게스트 초대자 역할
사용자에게 초대를 전송할 수 있는 게스트 초대자 역할에 할당할 수 있습니다. 전역 관리자 역할의 구성원이 아니더라도 초대를 보낼 수 있습니다. 기본적으로 전역 관리자가 일반 사용자에게 초대를 사용하지 않도록 설정한 경우가 아니면 일반 사용자도 초대 API를 호출할 수 있습니다. Azure Portal 또는 PowerShell을 사용하여 API를 호출할 수도 있습니다.

다음은 PowerShell을 사용하여 게스트 초대자 역할에 사용자를 추가하는 방법을 보여주는 예입니다.

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>초대할 수 있는 사용자 제어

Azure Active Directory > 사용자 설정 > 외부 공동 작업 설정 관리로 이동합니다.

![externalusers](https://user-images.githubusercontent.com/13383753/45905128-2c47f680-bda4-11e8-955d-6219c67935e0.PNG)

Azure AD B2B 공동 작업을 사용하면 테넌트 관리자가 다음 초대 정책을 설정할 수 있습니다.

- 초대 해제
- 관리자 및 게스트 초대자 역할의 사용자만 초대 가능
- 관리자, 게스트 초대자 역할 및 구성원은 초대 가능
- 게스트를 포함한 모든 사용자가 초대를 수행할 수 있습니다.

기본적으로 테넌트는 #4로 설정됩니다. (게스트를 포함한 모든 사용자가 B2B 사용자를 초대할 수 있습니다.)

## <a name="next-steps"></a>다음 단계

Azure AD B2B 공동 작업에 대한 다음 문서를 살펴보세요.

- [Azure AD B2B 공동 작업이란?](what-is-b2b.md)
- [초대 없이 B2B 공동 작업 게스트 사용자 추가](add-user-without-invite.md)
- [역할에 B2B 공동 작업 사용자 추가](add-guest-to-role.md)


