---
title: 포함 파일
description: 포함 파일
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: fb62468f2746072cea02185dbda022a1a5830fcf
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410452"
---
지금까지 응용 프로그램에서 작업하는 유일한 개발자인 것처럼 응용 프로그램 코드를 실행했습니다. 이 섹션에서는 Azure Dev Spaces에서 팀 개발을 간소화하는 방법에 대해 알아봅니다.
* 필요에 따라 공유 개발 공간 또는 고유한 개발 공간에서 작업하여 개발자 팀이 동일한 환경에서 작업하도록 설정합니다.
* 각 개발자가 다른 개발자를 방해하지 않고 격리된 상태에서 자신의 코드를 반복할 수 있도록 지원합니다.
* 모의 개체를 만들거나 종속성을 시뮬레이션할 필요 없이 코드를 커밋하기 전에 종단 간 코드를 테스트합니다.

### <a name="challenges-with-developing-microservices"></a>마이크로 서비스 개발 문제
현재 샘플 응용 프로그램은 그다지 복잡하지 않습니다. 하지만 실제 개발 환경에서는 더 많은 서비스를 추가하고 개발 팀이 성장함에 따라 문제가 곧 발생합니다.

수십 개의 다른 서비스와 상호 작용하는 서비스를 사용하는 자신을 상상해 보세요.

- 개발을 위해 모든 작업을 로컬로 실행하는 것은 현실적이지 않습니다. 개발 컴퓨터에는 전체 앱을 실행할 수 있을 만큼 충분한 리소스가 없을 수 있습니다. 또는 앱에 공용으로 연결할 수 있는 엔드포인트가 있을 수 있습니다(예: 앱에서 SaaS 앱의 웹후크에 응답하는 경우).

- 사용하는 서비스만 실행하려고 할 수는 있지만, 이 경우 종속성, 즉 종속성의 종속성까지 완전히 종료해야 합니다. 또는 작업을 수행하지 않았기 때문에 종속성을 작성하고 실행하는 방법을 쉽게 알지 못하는 것이 문제입니다.
- 일부 개발자는 서비스 종속성 대부분을 시뮬레이션하거나 모델링하는 데 의존합니다. 이렇게 하면 도움이 되는 경우도 있지만, 그러한 모의 개체를 관리하는 것은 곧 자체의 개발 노력에 착수해야 할 수도 있습니다. 또한 이로 인해 개발 공간을 프로덕션과 매우 다르게 보이도록 하고, 감지하기 어려운 버그의 영향이 미칠 수 있습니다.
- 따라서 모든 종류의 종단 간 테스트를 수행하는 것이 어려워집니다. 통합 테스트는 현실적으로 커밋 후에만 수행할 수 있습니다. 이는 나중에 개발 주기에서 문제가 발생한다는 것을 의미합니다.

![](../articles/dev-spaces/media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>공유 개발 공간에서 작업
Azure Dev Spaces를 사용하면 Azure에서 *공유* 개발 공간을 설정할 수 있습니다. 각 개발자는 응용 프로그램의 일부에만 집중할 수 있으며, 시나리오에 종속된 다른 모든 서비스와 클라우드 리소스가 이미 포함되어 있는 개발 공간에서 *사전 커밋 코드*를 반복적으로 개발할 수 있습니다. 종속성은 항상 최신이며, 개발자는 프로덕션을 미러하는 방식으로 작업합니다.

### <a name="work-in-your-own-space"></a>개발자 고유의 공간에서 작업
서비스에 대한 코드를 개발하고 이 코드를 체크 인할 준비가 되기 전에 코드 상태가 좋지 않은 경우가 많습니다. 아직도 코드를 반복적으로 만들고, 테스트하고, 솔루션에서 실험합니다. Azure Dev Spaces는 **공간**이라는 개념을 제공합니다. 이 공간을 사용하면 팀 구성원을 방해할 염려 없이 격리된 상태에서 작업할 수 있습니다.

> [!Note]
> 계속 진행하기 전에 두 서비스에 대한 모든 VS Code 창을 닫고 각 서비스의 루트 폴더에서 `azds up -d`를 실행하세요. 이는 미리 보기 제한 사항입니다.

서비스가 현재 실행 중인 곳을 자세히 살펴보겠습니다. `azds list-up` 명령을 실행하면 다음과 유사한 결과가 표시됩니다.

```
Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      default   Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      default   Pod      3m ago   Running
webfrontend                   default   Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  default   Pod      1m ago   Running
```

DevSpace 열은 두 서비스 모두 `default`라는 공간에서 실행되고 있음을 보여줍니다. 공용 URL을 열고 웹앱으로 이동하는 모든 사용자는 이전에 작성한 코드 경로를 호출하며, 이 코드는 두 서비스 모두를 통해 실행됩니다. 이제 `mywebapi` 개발을 계속하려는 경우를 가정해 보겠습니다. 개발 환경을 사용하는 다른 개발자에게 영향을 주지 않으면서 코드를 변경하고 이를 테스트하려면 어떻게 해야 할까요? 이렇게 하려면 자신만의 고유한 공간을 설정합니다.

### <a name="create-a-dev-space"></a>개발 환경 만들기
`default` 이외의 공간에서 고유한 버전의 `mywebapi`를 실행하려면 다음 명령을 사용하여 고유한 공간을 만들면 됩니다.

``` 
azds space select --name scott
```

메시지가 나타나면 **부모 개발 환경**으로 `default`을 선택합니다. 즉, 새 공간 `default/scott`는 `default` 공간에서 파생됩니다. 이것이 어떻게 도움이 되는지 테스트로 간략히 표시됩니다. 

위 예제에서 제가 작업 중인 공간을 동료가 식별할 수 있도록 새 공간에 제 이름을 사용했지만 여러분은 'sprint4' 또는 'demo'와 같이 유연한 의미를 담아 원하는 대로 지칭할 수 있습니다.

`azds space list` 명령을 실행하면 개발 환경의 모든 공간 목록이 표시됩니다. 현재 선택된 공간 옆에 별표(*)가 나타납니다. 여러분의 경우에는 'default/scott'라는 공간이 만들어질 때 자동으로 선택되었습니다. 언제든지 `azds space select` 명령을 사용하여 다른 공간을 선택할 수 있습니다.
