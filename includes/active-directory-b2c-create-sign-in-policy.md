---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 19e7c919345c0f56b274737840f8150f7d710501
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132777"
---
응용 프로그램에서 로그인만 사용하려면 **로그인** 정책을 사용합니다. 이 정책은 로그인하는 동안 고객이 경험하게 될 환경, 그리고 로그인 성공 시 응용 프로그램에서 수신하게 될 토큰의 콘텐츠를 설명합니다.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
**로그인 정책**을 클릭합니다.

블레이드의 위쪽에서 **+추가**를 클릭합니다.

**이름**은 응용 프로그램에서 사용하는 로그인 정책 이름을 결정합니다. 예를 들어 **SiIn**을 입력합니다.

**ID 공급자**를 클릭하고 **로컬 계정 로그인**을 선택합니다. 또한 필요에 따라 이미 구성되어 있는 소셜 ID 공급자를 선택할 수 있습니다. **확인**을 클릭합니다.

**응용 프로그램 클레임**을 클릭합니다. 여기서 성공적인 로그인 환경 이후에 응용 프로그램으로 다시 전송된 토큰에서 반환하려는 클레임을 선택합니다. 예를 들어 **표시 이름**, **ID 공급자**, **우편 번호** 및 **사용자의 개체 ID**를 선택합니다. **확인**을 클릭합니다.

**만들기**를 클릭합니다. 방금 만든 정책은 **로그인 정책** 블레이드에서 **B2C_1_SiIn**(**B2C\_1\_** 조각이 자동으로 추가됨)으로 표시됩니다.

**B2C_1_SiIn**을 클릭하여 해당 정책을 엽니다.

**응용 프로그램** 드롭다운에서 **Contoso B2C 앱**을 선택하고, **회신 URL/리디렉션 URI** 드롭다운에서 `https://localhost:44321/`을 선택합니다.

**지금 실행**을 클릭합니다. 새 브라우저 탭이 열리고 응용 프로그램에 로그인한 사용자 환경을 실행할 수 있습니다.

> [!NOTE]
> 정책 만들기 및 업데이트가 적용되려면 최대 1분이 걸립니다.
>