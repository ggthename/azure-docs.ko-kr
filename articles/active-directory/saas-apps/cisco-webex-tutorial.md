---
title: '자습서: Cisco Webex와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory 및 Cisco Webex 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 73e20afdcacec76482f8ebf01bf2cef2105912a6
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005525"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>자습서: Cisco Webex와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Cisco Webex를 통합하는 방법에 대해 알아봅니다.

Cisco Webex를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Cisco Webex에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Cisco Webex에 자동으로 로그인되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Cisco Webex와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Cisco Webex Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 따릅니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Cisco Webex 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="add-cisco-webex-from-the-gallery"></a>갤러리에서 Cisco Webex 추가
Cisco Webex의 Azure AD 통합을 구성하려면 갤러리의 Cisco Webex를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리의 Cisco Webex를 추가하려면 다음 단계를 수행합니다.**

1. [Azure Portal](https://portal.azure.com)의 왼쪽 창에서 **Azure Active Directory** 아이콘을 선택합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![엔터프라이즈 응용 프로그램 블레이드][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 선택합니다.

    ![새 응용 프로그램 단추][3]

4. 검색 상자에 **Cisco Webex**를 입력합니다. 

5. 결과 패널에서 **Cisco Webex**를 선택합니다. 그런 다음, **추가** 단추를 선택하여 응용 프로그램을 추가합니다.

    ![결과 목록의 Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 “Britta Simon”이라는 테스트 사용자를 기반으로 Cisco Webex에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Cisco Webex 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Cisco Webex의 관련 사용자 간에 연결이 형성되어야 합니다.

Cisco Webex에서 Azure AD의 **사용자 이름**과 동일한 **Username** 값을 제공합니다. 이렇게 해서 두 사용자 간의 연결 관계를 형성했습니다. 

Cisco Webex에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. [Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on) - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. [Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user) - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. [Cisco Webex 테스트 사용자 만들기](#create-a-cisco-webex-test-user) - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Cisco Webex에 만듭니다.
4. [Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user) - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. [Single Sign-On 테스트](#test-single-sign-on) - 구성이 작동하는지 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Cisco Webex 응용 프로그램에서 Single Sign-On을 구성합니다.

**Cisco Webex에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Cisco Webex** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크][4]

2. Single Sign-On을 사용하려면 **Single Sign-On** 대화 상자의 **모드** 드롭다운 목록에서 **SAML 기반 로그온**을 선택합니다.
 
    ![Single Sign-On 대화 상자](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. 다른 웹 브라우저 창에서 Cisco Webex 회사 사이트에 관리자로 로그인합니다.

4. 메뉴의 왼쪽에서 **설정**을 클릭합니다.

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. 설정 페이지의 **인증** 섹션에서 아래로 스크롤하여 **수정**을 클릭합니다.

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. **타사 ID 공급자 통합 (고급)** 을 선택하고 다음 화면으로 이동합니다.

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. **디렉터리 메타데이터 내보내기** 페이지에서 **메타데이터 파일 다운로드**를 클릭하여 메타데이터 파일을 다운로드합니다.

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. Azure Portal의 **Cisco Webex 도메인 및 URL** 섹션에서 다운로드한 **서비스 공급자 메타데이터 파일**을 업로드하고 다음 단계를 수행하여 응용 프로그램을 구성합니다.

    a. **메타데이터 파일 업로드**를 클릭합니다.

    ![Cisco Webex 도메인 및 URL Single Sign-On 정보](./media/cisco-webex-tutorial/tutorial_ciscowebex_upload.png)

    b. **폴더 로고**를 클릭하여 메타데이터 파일을 선택하고 **업로드**를 클릭합니다.

    ![Cisco Webex 도메인 및 URL Single Sign-On 정보](./media/cisco-webex-tutorial/tutorial_ciscowebex_uploadconfig.png)

    다. **서비스 공급자 메타데이터 파일**을 성공적으로 업로드한 후에는 **식별자** 및 **응답 URL** 값이 **Cisco Webex 도메인 및 URL** 섹션 텍스트 상자에 다음과 같이 자동으로 입력됩니다.

    ![Cisco Webex 도메인 및 URL Single Sign-On 정보](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    d. **로그온 URL** 텍스트 상자에 다음과 같은 패턴을 사용하여 URL을 입력합니다. `https://<SUBDOMAIN>.webex.com/`
     
    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 이러한 값을 실제 로그온 URL로 업데이트합니다. 이러한 값을 얻으려면 [Cisco Webex 클라이언트 지원 팀](https://www.webex.co.in/support/support-overview.html)에 문의하세요.

9. Cisco Webex 응용 프로그램은 특정 특성을 포함하는 SAML 어설션이 필요합니다. 이 응용 프로그램에 대한 다음 특성을 구성합니다. 응용 프로그램 통합 페이지의 **사용자 특성** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. 다음 스크린샷은 이에 대한 예제를 보여 줍니다.
    
    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_ciscowebex_07.png) 

10. **Single sign-on** 대화 상자의 **사용자 특성** 섹션에서 위의 이미지에 표시된 것과 같이 SAML 토큰 특성을 구성하고 다음 단계를 수행합니다.
    
    |  특성 이름  | 특성 값 |
    | --------------- | -------------------- |    
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   uid    | user.mail |

    a. **특성 추가**를 클릭하여 **특성 추가** 대화 상자를 엽니다.

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_attribute_04.png)

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_attribute_05.png)
    
    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.
    
    다. **값** 목록에서 해당 행에 대해 표시된 특성을 입력합니다.
    
    d. **Ok**를 클릭합니다.

11. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 선택한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

12. **저장**을 선택합니다.

    ![Single Sign-On 저장 단추 구성](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
13. Cisco Webex 회사 사이트 관리자 페이지에서 Azure AD 메타데이터 파일을 찾아 업로드하려면 파일 브라우저 옵션을 사용합니다. 그런 다음, **메타데이터에서 인증 기관이 서명한 인증서 필요(더 안전)** 를 선택하고 다음 화면으로 이동합니다. 

    ![Configure Single Sign-On](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

14. **SSO 연결 테스트**를 선택하고 새 브라우저 탭이 열리면 로그인하여 Azure AD로 인증합니다.

15. **Cisco Cloud Collaboration Management** 브라우저 탭으로 돌아갑니다. 테스트에 성공하면 **이 테스트에 성공했습니다. Single Sign-On 사용 옵션**을 사용하고 **저장**을 클릭합니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 선택합니다.

    ![Azure Active Directory 단추](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 선택합니다.

    ![추가 단추](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    다. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 선택합니다.
 
### <a name="create-a-cisco-webex-test-user"></a>Cisco Webex 테스트 사용자 만들기

이 섹션에서는 Cisco Webex에서 Britta Simon이라는 사용자를 만드는 것이 목적입니다. Cisco Webex는 기본적으로 사용하도록 설정된 Just-In-Time 프로비전 및 자동 사용자 프로비전을 지원합니다. 자동 사용자 프로비전 구성 방법에 대한 자세한 내용은 [여기](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)에서 제공합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 사용자 Britta Simon에게 Cisco Webex에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 Cisco Webex에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 엽니다. 다음으로 디렉터리 보기로 이동한 다음, **엔터프라이즈 응용 프로그램**으로 이동합니다.  

2. **모든 응용 프로그램**을 선택합니다.

    ![사용자 할당][201] 

3. 응용 프로그램 목록에서 **Cisco Webex**를 선택합니다.

    ![응용 프로그램 목록의 Cisco Webex 링크](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 선택합니다. **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 **사용자 목록**에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 선택합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Cisco Webex 타일을 선택하면 Cisco Webex 응용 프로그램에 자동으로 로그인됩니다.

액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS 앱을 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

