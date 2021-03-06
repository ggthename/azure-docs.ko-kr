---
title: '자습서: BorrowBox와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 BorrowBox 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dd8e4178-9a63-492a-bd48-782e94e404af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2018
ms.author: jeedes
ms.openlocfilehash: a8ed2f04bf3004907cdd6e33bfb30260233fb101
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157159"
---
# <a name="tutorial-azure-active-directory-integration-with-borrowbox"></a>자습서: BorrowBox와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Active Directory)와 BorrowBox를 통합하는 방법에 대해 알아봅니다.

BorrowBox를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- BorrowBox 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 BorrowBox에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

BorrowBox와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Single Sign-on이 설정된 BorrowBox 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 BorrowBox 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-borrowbox-from-the-gallery"></a>갤러리에서 BorrowBox 추가
Azure AD에 대한 BorrowBox 통합을 구성하려면 갤러리의 BorrowBox를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 BorrowBox를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![이미지](./common/selectazuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![이미지](./common/a_select_app.png)
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![이미지](./common/a_new_app.png)

4. 검색 상자에 **BorrowBox**를 입력하고 결과 패널에서 **BorrowBox**를 선택한 후 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

     ![이미지](./media/borrowbox-tutorial/tutorial_borrowbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 테스트 사용자 "Britta Simon"을 기준으로 하여 BorrowBox에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 BorrowBox 사용자를 확인할 수 있어야 합니다. 즉, Azure AD 사용자와 BorrowBox의 관련 사용자 간에 연결 관계가 설정되어 있어야 합니다.

BorrowBox에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[BorrowBox 테스트 사용자 만들기](#create-a-borrowbox-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 BorrowBox에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 BorrowBox 응용 프로그램에서 Single Sign-On을 구성합니다.

**BorrowBox에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. [Azure Portal](https://portal.azure.com/)의 **BorrowBox** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![이미지](./common/B1_B2_Select_SSO.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![이미지](./common/b1_b2_saml_sso.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 단추를 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![이미지](./common/b1-domains_and_urlsedit.png)

4. 앱이 Azure와 이미 사전 통합되었으므로 사용자는 **기본 SAML 구성** 섹션에서 아무 단계도 수행할 필요가 없습니다.

    ![이미지](./media/borrowbox-tutorial/tutorial_borrowbox_url.png)

    a. **추가 URL 설정**을 클릭합니다.

    b. **로그인 URL** 텍스트 상자에서 `https://fe.bolindadigital.com/wldcs_bol_fo/b2i/mainPage.html?b2bSite=<ID>` 패턴을 사용하여 URL을 입력합니다.

    ![이미지](./media/borrowbox-tutorial/tutorial_borrowbox_url1.png)

    > [!NOTE]
    > 로그온 URL 값은 실제 값이 아닙니다. 이 값을 실제 로그온 URL로 업데이트합니다. 값을 얻으려면 [BorrowBox 클라이언트 지원 팀](mailto:borrowbox@bolinda.com)에 문의하세요.

5. BorrowBox 응용 프로그램에는 특정 형식의 SAML 어설션이 필요합니다. 이 응용 프로그램에 대해 다음 클레임을 구성합니다. 응용 프로그램 통합 페이지의 **사용자 특성 및 클레임** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 단추를 클릭하여 **사용자 특성 및 클레임** 대화 상자를 엽니다.

    ![이미지](./media/borrowbox-tutorial/i4-attribute.png)

6. **사용자 특성 및 클레임** 대화 상자의 **사용자 클레임** 섹션에서 위의 이미지에 표시된 것과 같이 SAML 토큰 특성을 구성하고 다음 단계를 수행합니다.
    
    a. **편집 아이콘**을 클릭하여 **사용자 클레임 관리** 대화 상자를 엽니다.

    ![이미지](./media/borrowbox-tutorial/i2-attribute.png)

    ![이미지](./media/borrowbox-tutorial/i3-attribute.png)

    b. **원본 특성** 목록에서 **user.mail**을 선택합니다.

    다. **저장**을 클릭합니다. 

7. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 적절한 인증서를 다운로드한 다음 컴퓨터에 저장합니다.

    ![이미지](./media/borrowbox-tutorial/tutorial_borrowbox_certificate.png) 

8. **BorrowBox** 쪽에서 Single Sign-On을 구성하려면 다운로드한 인증서/메타데이터를 Azure Portal에서 [BorrowBox 지원 팀](mailto:borrowbox@bolinda.com)에 보내야 합니다. 이렇게 설정하면 SAML SSO 연결이 양쪽에서 제대로 설정됩니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    ![이미지](./common/d_users_and_groups.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![이미지](./common/d_adduser.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![이미지](./common/d_userproperties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 **brittasimon@yourcompanydomain.extension**을 입력합니다.  
    예를 들어 BrittaSimon@contoso.com

    다. **속성**을 선택하고 **암호 표시** 확인란을 선택한 다음, 암호 상자에 표시된 값을 적어 둡니다.

    d. **만들기**를 선택합니다.
 
### <a name="create-a-borrowbox-test-user"></a>BorrowBox 테스트 사용자 만들기

이 섹션에서는 BorrowBox에서 Britta Simon 사용자를 만듭니다. BorrowBox는 JIT(Just-In-Time) 프로비전(기본적으로 사용됨)을 지원합니다. 이 섹션에 작업 항목이 없습니다. 사용자가 없으면 BorrowBox 액세스를 시도할 때 새 사용자가 생성됩니다.
>[!Note]
>사용자를 수동으로 만들어야 하는 경우  [BorrowBox 지원 팀](mailto:borrowbox@bolinda.com)에 문의하세요.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 BorrowBox 액세스 권한을 부여하여 Britta Simon이 Azure Single Sign-On을 사용할 수 있도록 설정합니다.

1. Azure Portal에서 **엔터프라이즈 응용 프로그램**을 선택한 다음, **모든 응용 프로그램**을 선택합니다.

    ![이미지](./common/d_all_applications.png)

2. 응용 프로그램 목록에서 **BorrowBox**를 선택합니다.

    ![이미지](./media/borrowbox-tutorial/tutorial_borrowbox_app.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    ![이미지](./common/d_leftpaneusers.png)

4. **추가** 단추를 선택하고 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![이미지](./common/d_assign_user.png)

4. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

5. **할당 추가** 대화 상자에서 **할당** 단추를 선택합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 BorrowBox 타일을 클릭하면 BorrowBox 응용 프로그램에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)
