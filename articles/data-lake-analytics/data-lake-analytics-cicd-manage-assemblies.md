---
title: Azure Data Lake에 대한 CI/CD 파이프라인에서 U-SQL 어셈블리를 관리하는 모범 사례
description: Azure DevOps를 사용하여 CI/CD 파이프라인에서 U-SQL C# 어셈블리를 관리하는 모범 사례를 알아봅니다.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: ''
ms.assetid: ''
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 10/30/2018
ms.openlocfilehash: 0f6f581d90b025dd538eb9b79da1a0addd4259f7
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50670290"
---
# <a name="best-practice-of-managing-u-sql-assemblies-in-cicd-pipeline"></a>CI/CD 파이프라인에서 U-SQL 어셈블리를 관리하는 모범 사례

이 문서에서는 새로 도입된 U-SQL 데이터베이스 프로젝트를 사용하여 U-SQL 어셈블리 소스 코드를 관리하는 방법에 대해 알아봅니다. 또한 Azure DevOps를 사용하여 어셈블리 등록을 위해 CI/CD(지속적인 통합 및 배포) 파이프라인을 설정하는 방법도 알아볼 수 있습니다.

## <a name="use-the-u-sql-database-project-to-manage-assembly-source-code"></a>U-SQL 데이터베이스 프로젝트를 사용하여 어셈블리 소스 코드 관리

[U-SQL 데이터베이스 프로젝트](data-lake-analytics-data-lake-tools-develop-usql-database.md)는 개발자가 U-SQL 데이터베이스를 빠르고 쉽게 개발, 관리 및 배포할 수 있도록 지원하는 Visual Studio의 프로젝트 형식입니다. 모든 U-SQL 데이터베이스 개체(자격 증명 제외)는 U-SQL 데이터베이스 프로젝트를 사용하여 관리할 수 있습니다. 

C# 어셈블리 소스 코드 및 어셈블리 등록 DDL U-SQL 스크립트를 관리하는 데 권장되는 방법은 다음과 같습니다.

* **U-SQL 데이터베이스 프로젝트**를 사용하여 어셈블리 등록 U-SQL 스크립트를 관리합니다.
* **클래스 라이브러리(U-SQL 응용 프로그램용)** 를 사용하여 사용자 정의 연산자, 함수 및 집계(UDO/UDF/UDAG)에 대한 C# 소스 코드 및 종속성을 관리합니다.
* U-SQL 데이터베이스 프로젝트를 사용하여 클래스 라이브러리 프로젝트를 참조합니다. 

U-SQL 데이터베이스 프로젝트는 클래스 라이브러리(U-SQL 응용 프로그램용) 프로젝트를 참조할 수 있습니다. U-SQL 데이터베이스에 등록된 어셈블리는 이 클래스 라이브러리(U-SQL 응용 프로그램용) 프로젝트에서 참조된 C# 소스 코드를 사용하여 만들 수 있습니다.

프로젝트를 만들고 참조를 추가하는 단계는 다음과 같습니다.
1. **파일 > 새로 만들기 > 프로젝트**를 통해 클래스 라이브러리(U-SQL 응용 프로그램용) 프로젝트를 만듭니다. 프로젝트는 **Azure Data Lake > U-SQL** 노드 아래에 있습니다.
   ![Data Lake Tools for Visual Studio - C# 클래스 라이브러리 프로젝트 만들기](./media/data-lake-analytics-cicd-manage-assemblies/create-c-sharp-class-library-project.png)
2. 클래스 라이브러리(U-SQL 응용 프로그램용) 프로젝트에 사용자 정의 C# 코드를 추가합니다. 
3. **파일 > 새로 만들기 > 프로젝트**를 통해 U-SQL 프로젝트를 만듭니다. 프로젝트는 **Azure Data Lake > U-SQL** 노드 아래에 있습니다.
   ![Data Lake Tools for Visual Studio--U-SQL 데이터베이스 프로젝트 만들기](media/data-lake-analytics-cicd-manage-assemblies/create-u-sql-database-project.png)
4. U-SQL 데이터베이스 프로젝트용 C# 클래스 라이브러리 프로젝트에 참조를 추가합니다.

    ![Data Lake Tools for Visual Studio--U-SQL 데이터베이스 프로젝트 참조 추가](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference.png) 

    ![Data Lake Tools for Visual Studio--U-SQL 데이터베이스 프로젝트 참조 추가](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-add-project-reference-wizard.png)
5. **프로젝트 > 새 항목 추가**를 마우스 오른쪽 단추로 클릭하여 어셈블리 스크립트를 U-SQL 데이터베이스 프로젝트에 만듭니다.
   ![Data Lake Tools for Visual Studio - 어셈블리 스크립트 추가](media/data-lake-analytics-cicd-manage-assemblies/add-assembly-script.png)
6. 어셈블리 스크립트를 열고, 어셈블리 디자인 보기의 **참조에서 어셈블리 만들기** 드롭다운 메뉴에서 참조된 어셈블리를 선택합니다.

    ![Data Lake Tools for Visual Studio--참조에서 어셈블리 만들기](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-create-assembly-from-reference.png)

7. **관리되는 종속성** 및 **추가 파일**(있는 경우)을 추가합니다. 추가 파일을 추가하는 경우 이 도구는 나중에 상대 경로를 사용하여 로컬 컴퓨터 및 빌드 컴퓨터 둘 다에서 어셈블리를 찾을 수 있습니다. 

편집기 창의 아래쪽에 있는 **@_DeployTempDirectory**는 도구를 빌드 출력 폴더로 가리키는 미리 정의된 변수입니다. 모든 어셈블리는 빌드 출력 폴더 아래에 어셈블리 이름의 하위 폴더가 있습니다. 모든 DLL 및 추가 파일이 해당 하위 폴더에 있습니다. 

## <a name="build-a-u-sql-database-project"></a>U-SQL 데이터베이스 프로젝트 빌드

U-SQL 데이터베이스 프로젝트에 대한 빌드 출력은 접미사 `.usqldbpack`으로 명명된 U-SQL 데이터베이스 배포 패키지입니다. `.usqldbpack` 패키지는 **DDL** 폴더에 단일 U-SQL 스크립트의 모든 DDL 문을 포함하고, **Temp** 폴더에 어셈블리에 대한 모든 빌드된 dll 및 추가 파일을 포함한 .zip 파일입니다.

## <a name="deploy-a-u-sql-database"></a>U-SQL 데이터베이스 배포

`.usqldbpack` 패키지는 Visual Studio 또는 배포 SDK를 사용하여 로컬 계정 또는 Azure Data Lake Analytics 계정에 배포할 수 있습니다. 

### <a name="deploy-a-u-sql-database-in-visual-studio"></a>Visual Studio에서 U-SQL 데이터베이스 배포

U-SQL 데이터베이스는 Visual Studio에서 U-SQL 데이터베이스 프로젝트 또는 `.usqldbpack` 패키지를 통해 배포할 수 있습니다.

#### <a name="deploy-through-a-u-sql-database-project"></a>U-SQL 데이터베이스 프로젝트를 통해 배포

1.  U-SQL 데이터베이스 프로젝트를 마우스 오른쪽 단추로 클릭한 후 **배포**를 선택합니다.
2.  **U-SQL 데이터베이스 배포 마법사**에서 데이터베이스를 배포하려는 **ADLA 계정**을 선택합니다. 로컬 계정과 ADLA 계정이 둘 다 지원됩니다.
3.  **데이터베이스 원본**은 자동으로 채워지며, 프로젝트의 빌드 출력 폴더에 있는 .usqldbpack 패키지를 가리킵니다.
4.  **데이터베이스 이름**에 이름을 입력하여 데이터베이스를 만듭니다. 같은 이름의 데이터베이스가 대상 Azure Data Lake Analytics 계정에 이미 있는 경우 데이터베이스 프로젝트에 정의된 모든 개체는 데이터베이스를 다시 만들지 않고도 생성됩니다.
5.  U-SQL 데이터베이스를 배포하려면 **제출**을 선택합니다. 모든 리소스(어셈블리 및 추가 파일)가 업로드되고 모든 DDL 문을 포함하는 U-SQL 작업이 제출됩니다.

    ![Data Lake Tools for Visual Studio--U-SQL 데이터베이스 프로젝트 배포](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project.png)

    ![Data Lake Tools for Visual Studio--U-SQL 데이터베이스 프로젝트 배포 마법사](./media/data-lake-analytics-cicd-manage-assemblies/data-lake-tools-deploy-usql-database-project-wizard.png)

### <a name="deploy-u-sql-database-in-azure-devops"></a>Azure DevOps에서 U-SQL 데이터베이스 배포

`PackageDeploymentTool.exe`는 U-SQL 데이터베이스를 배포하는 데 도움이 되는 프로그래밍 및 명령줄 인터페이스를 제공합니다. 이 SDK는 `build/runtime/PackageDeploymentTool.exe`에 있는 [U-SQL SDK Nuget 패키지](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)에 포함되어 있습니다.

Azure DevOps에서 명령줄 작업과 이 SDK를 사용하여 U-SQL 데이터베이스 새로 고침에 대한 자동화 파이프라인을 설정할 수 있습니다. [SDK 및 U-SQL 데이터베이스 배포에 대해 CI/CD 파이프라인을 설정하는 방법에 대해 자세히 알아봅니다](data-lake-analytics-cicd-overview.md#deploy-u-sql-database-through-azure-pipelines).

## <a name="next-steps"></a>다음 단계

* [Azure Data Lake Analytics에 대해 CI/CD 파이프라인을 설정하는 방법](data-lake-analytics-cicd-overview.md)
* [Azure Data Lake Analytics 코드를 테스트하는 방법](data-lake-analytics-cicd-test.md)
* [로컬 컴퓨터에서 U-SQL 스크립트 실행](data-lake-analytics-data-lake-tools-local-run.md)
