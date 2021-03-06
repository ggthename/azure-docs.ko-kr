---
title: Java 및 Maven을 사용하여 Azure에서 첫 번째 함수 만들기 | Microsoft Docs
description: Java 및 Maven을 사용하여 Azure에 간단한 HTTP 트리거 함수를 만들어 게시합니다.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure 함수, 함수, 이벤트 처리, 계산, 서버를 사용하지 않는 아키텍처
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 7483ac4521b0b997111dcc5705ba8c28a8443299
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116405"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Java 및 Maven을 사용하여 Azure에서 첫 번째 함수 만들기(미리 보기)

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

이 빠른 시작에서는 Maven을 사용하여 [서버 없는](https://azure.microsoft.com/solutions/serverless/) 함수 프로젝트를 만들어 로컬로 테스트하고 Azure Functions에 배포하는 과정을 안내합니다. 마친 후에는 Azure에서 HTTP 트리거 함수 앱이 실행됩니다.

![cURL로 명령줄에서 Hello World 함수 액세스](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>필수 조건
Java 통해 함수 앱을 개발하려면 다음을 설치해야 합니다.

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/) 버전 8
-  [Apache Maven](https://maven.apache.org) 버전 3.0 이상
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> 이 퀵 스타트를 완료하려면 JAVA_HOME 환경 변수를 JDK 설치 위치로 설정해야 합니다.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions 핵심 도구 설치

Azure Functions Core Tools는 터미널 또는 명령 프롬프트에서 Azure Functions의 작성, 실행 및 디버그를 위한 로컬 개발 환경을 제공합니다. 

계속하기 전에 로컬 컴퓨터에 [Core Tools의 버전 2](functions-run-local.md#v2)를 설치합니다.

## <a name="generate-a-new-functions-project"></a>새 Functions 프로젝트 생성

빈 폴더에서 다음 명령을 실행하여 [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html)으로부터 Functions 프로젝트를 생성합니다.

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows(CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven이 프로젝트 생성을 완료하기 위해 필요한 값을 요청합니다. _groupId_, _artifactId_ 및 _version_ 값은 [Maven 명명 규칙](https://maven.apache.org/guides/mini/guide-naming-conventions.html) 참고를 참조하세요. _appName_ 값은 Azure 전체에서 고유해야 하므로 기본적으로 Maven이 이전에 입력한 _artifactId_를 기준으로 앱 이름을 생성합니다. _packageName_ 값은 생성된 함수 코드에 대한 Java 패키지를 결정합니다.

`appRegion` 값은 배포된 함수 앱을 실행할 [Azure 지역](https://azure.microsoft.com/global-infrastructure/regions/)을 지정합니다. Azure CLI에서 `az account list-locations` 명령을 통해 지역 이름 값 목록을 가져올 수 있습니다. `resourceGroup` 값은 함수 앱을 만들 Azure 리소스 그룹을 지정합니다.

아래의 `com.fabrikam.functions` 및 `fabrikam-functions` 식별자는 예제로 사용되며 이 빠른 시작의 이후 단계를 좀 더 쉽게 읽는 데 도움이 됩니다. 이 단계에서는 Maven에 각자 고유의 값을 제공하시기 바랍니다.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven은 이름이 _artifactId_인 새 폴더에 프로젝트 파일을 만드는데, 이 예제에서는 `fabrikam-functions`입니다. 이 프로젝트에서 생성된 즉시 실행 가능 코드는 "Hello" 문자열 이후의 요청 본문을 에코하는 간단한 [HTTP 트리거](/azure/azure-functions/functions-bindings-http-webhook) 함수입니다.

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}
```

## <a name="run-the-function-locally"></a>로컬에서 함수 실행

디렉터리를 새로 만든 프로젝트 폴더로 변경하고 Maven으로 함수를 실행합니다.

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Java 9에서 `javax.xml.bind.JAXBException` 예외가 발생하면 [GitHub](https://github.com/jOOQ/jOOQ/issues/6477)에서 해결 방법을 참조하세요.

함수가 시스템에서 로컬로 실행되고 HTTP 요청에 응답할 준비가 완료되면 이 출력이 보입니다.

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

새 터미널 창에서 curl을 사용하여 명령줄에서 함수를 트리거합니다.

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
```

터미널에서 `Ctrl-C`를 사용하여 함수 코드를 중지합니다.

## <a name="deploy-the-function-to-azure"></a>Azure에 함수 배포

Azure Functions에 대한 배포 프로세스는 Azure CLI의 계정 자격 증명을 사용합니다. 계속하려면 [Azure CLI에 로그인](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)합니다.

```azurecli
az login
```

`azure-functions:deploy` Maven 대상을 사용하여 새 함수 앱에 코드를 배포합니다.

```
mvn azure-functions:deploy
```

배포가 완료되면 Azure 함수 앱에 액세스하는 데 사용할 수 있는 URL이 표시됩니다.

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

`cURL`을 사용하여 Azure에서 실행 중인 함수 앱을 테스트합니다. 아래 예제에서는 이전 단계에서 만든 사용자 고유의 함수 앱에 배포한 URL과 일치하도록 URL을 변경해야 합니다.

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
```

## <a name="make-changes-and-redeploy"></a>변경 및 재배포

생성된 프로젝트에서 `src/main.../Function.java` 원본 파일을 편집하여 함수 앱에서 반환된 텍스트를 변경합니다. 이 줄 변경:

```java
return request.createResponse(200, "Hello, " + name);
```

다음으로:

```java
return request.createResponse(200, "Hi, " + name);
```

변경 내용을 저장하고 이전처럼 터미널에서 `azure-functions:deploy`를 실행하여 다시 배포합니다. 함수 앱이 업데이트되고 이 요청은 다음을 갖습니다.

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

업데이트된 출력:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>다음 단계

간단한 HTTP 트리거가 있는 Java 함수 앱을 만들어 Azure Functions에 배포했습니다.

- Java 함수 개발에 대한 자세한 내용은 [Java 함수 개발자 가이드](functions-reference-java.md)를 참조합니다.
- `azure-functions:add` Maven 대상을 사용하여 프로젝트에 다른 트리거가 있는 다른 함수를 추가합니다.
- [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) 및 [Eclipse](functions-create-maven-eclipse.md)를 사용하여 로컬로 함수를 작성하고 디버그합니다. 
- Visual Studio Code를 사용하여 Azure에 배포된 함수를 디버그합니다. 자세한 지침은 Visual Studio Code [서버 없는 Java 응용 프로그램](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) 설명서를 참조하세요.
