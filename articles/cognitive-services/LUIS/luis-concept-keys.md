---
title: LUIS 키에 대한 이해
titleSuffix: Azure Cognitive Services
description: LUIS는 작성 및 엔드포인트라는 두 가지 키를 사용합니다. 작성 키는 LUIS 계정을 만들 때 자동으로 생성됩니다. LUIS 앱을 게시할 준비가 되면 엔드포인트 키를 만들고, LUIS 앱에 할당하고, 엔드포인트 쿼리에서 사용해야 합니다.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f7c1753e71025d3ce39b1b6e3fb7362f2df212f5
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637834"
---
# <a name="keys-in-luis"></a>LUIS의 키
LUIS는 두 가지 키인 [작성](#programmatic-key) 및 [엔드포인트](#endpoint-key)를 사용합니다. 작성 키는 LUIS 계정을 만들 때 자동으로 생성됩니다. LUIS 앱을 게시할 준비가 되면 [엔드포인트 키를 만들고](luis-how-to-azure-subscription.md#create-luis-endpoint-key), LUIS 앱에 [할당](luis-how-to-manage-keys.md#assign-endpoint-key)하고, [엔드포인트 쿼리에서 사용](#use-endpoint-key-in-query)해야 합니다. 

|키|목적|
|--|--|
|[작성 키](#programmatic-key)|작성, 게시, 협력자 관리, 버전 관리|
|[엔드포인트 키](#endpoint-key)| 쿼리|

게시 및 쿼리하려는 [지역](luis-reference-regions.md#publishing-regions)에서 LUIS 앱을 작성하는 것이 중요합니다.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>작성 키

시작 키라고도 하는 작성 키는 LUIS 계정을 만들 때 자동으로 생성되며 무료입니다. 각 작성 [지역](luis-reference-regions.md)에 모든 LUIS 앱에서 하나의 작성 키를 사용합니다. 작성 키는 LUIS 앱을 작성하거나 엔드포인트 쿼리를 테스트하기 위해 제공됩니다. 

작성 키를 찾으려면 [LUIS](luis-reference-regions.md#luis-website)에 로그인하고 오른쪽 위 탐색 모음에 있는 계정 이름을 클릭하여 **계정 설정**을 엽니다.

![작성 키](./media/luis-concept-keys/programatic-key.png)

**프로덕션 엔드포인트 쿼리**를 만들려면 Azure [LUIS 구독](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)을 만듭니다. 

> [!CAUTION]
> 몇몇 엔드포인트 호출이 [할당량](luis-boundaries.md#key-limits)으로 제공되므로 편의상 많은 샘플에서 작성 키를 사용합니다.  

## <a name="endpoint-key"></a>엔드포인트 키
 **프로덕션 엔드포인트 쿼리**가 필요한 경우, Azure Portal에서 [LUIS 키](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)를 만듭니다. 키를 만드는 데 사용되는 이름을 기억하세요. 키를 앱에 추가할 때 필요합니다.

LUIS 구독 프로세스가 완료되면 앱에 [키를 할당](luis-how-to-manage-keys.md#assign-endpoint-key)합니다. 

엔드포인트 키는 키를 만들 때 지정한 사용 플랜에 따라 엔드포인트 적중 할당량을 허용합니다. 가격 정보는 [Cognitive Services 가격](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h)을 참조하세요.

엔드포인트 키는 모든 LUIS 앱 또는 특정 LUIS 앱에 사용할 수 있습니다. 

LUIS 앱 작성에는 엔드포인트 키를 사용하지 마세요. 

## <a name="use-endpoint-key-in-query"></a>쿼리에 엔드포인트 키 사용
LUIS 엔드포인트는 두 가지 쿼리를 허용하고, 두 가지 쿼리는 모두 엔드포인트 키를 사용하지만, 서로 다른 위치에서 사용합니다.

|동사|예제 URL 및 키 위치|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>`subscription-key`의 쿼리 문자열 값<br><br>LUIS 엔드포인트 키 할당량 요금을 사용하려면 작성(시작) 키에서 새 엔드포인트 키로 `subscription-key`의 엔드포인트 쿼리 값을 변경합니다. 키를 만들고 할당하지만 ‘subscription-key’의 엔드포인트 쿼리 값을 변경하지 않는 경우에는 엔드포인트 키 할당량을 사용하지 않습니다.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> `Ocp-Apim-Subscription-Key`의 헤더 값<br><br>LUIS 엔드포인트 키 할당량 요금을 사용하려면 작성(시작) 키에서 새 엔드포인트 키로 `Ocp-Apim-Subscription-Key`의 엔드포인트 쿼리 값을 변경합니다. 키를 만들고 할당하지만 `Ocp-Apim-Subscription-Key`의 엔드포인트 쿼리 값을 변경하지 않는 경우에는 엔드포인트 키 할당량을 사용하지 않습니다.|

이전 URL에 사용되던 앱 ID `df67dcdb-c37d-46af-88e1-8b97951ca1c2`는 [대화형 데모](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/)에 사용되는 공용 IoT 앱입니다. 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Ocp-Apim-Subscription-Key의 API 사용법
LUIS API는 `Ocp-Apim-Subscription-Key` 헤더를 사용합니다. 헤더 이름은 사용 중인 키 및 API 집합에 따라 변경되지 않습니다. 헤더를 작성 API의 작성 키로 설정합니다. 엔드포인트를 사용하는 경우, 헤더를 엔드포인트 키로 설정합니다. 

작성 API의 엔드포인트 키를 전달할 수는 없습니다. 전달할 경우 다음 401 오류가 표시됩니다. 잘못된 끝점 키로 인해 액세스가 거부되었습니다. 

## <a name="key-limits"></a>키 제한
[키 제한](luis-boundaries.md#key-limits) 및 [Azure 지역](luis-reference-regions.md)을 참조하세요. 작성 키는 무료이며 작성에 사용됩니다. LUIS 끝점 키는 체험 계층을 포함하지만, 사용자가 만들고 **게시** 페이지에서 LUIS 앱과 연결되어야 합니다. 구독 키는 작성에 사용할 수 없고 엔드포인트 쿼리에만 사용할 수 있습니다.

게시 지역은 작성 지역과 다릅니다. 원하는 게시 지역에 해당하는 작성 지역에서 앱을 만들어야 합니다.

## <a name="key-limit-errors"></a>키 제한 오류
초당 할당량을 초과하는 경우, HTTP 429 오류가 표시됩니다. 월별 할당량을 초과하는 경우, HTTP 403 오류가 표시됩니다. LUIS [끝점](#endpoint-key) 키를 가져오고 [LUIS](luis-reference-regions.md#luis-website) 웹 사이트의 **게시** 페이지에서 앱에 키를 [할당](luis-how-to-manage-keys.md#assign-endpoint-key)하여 이러한 오류를 수정합니다.

## <a name="automating-assignment-of-the-endpoint-key"></a>엔드포인트 키 할당 자동화

엔드포인트 키를 LUIS 앱에 할당하기 위해 올바른 제작 및 게시 [지역](luis-reference-regions.md)에 대해 LUIS 웹 사이트를 사용해야 합니다. Azure Resource Manager 스크립트, Azure CLI, 프로그래밍 방식 SDK 또는 API를 사용하는 것과 같은 메커니즘에 관계 없이 이 작업을 수행하는 자동화된 메서드가 **없습니다**.

## <a name="next-steps"></a>다음 단계

* 작성 및 엔드포인트 키의 [개념](luis-how-to-manage-keys.md#assign-endpoint-key)을 알아봅니다.