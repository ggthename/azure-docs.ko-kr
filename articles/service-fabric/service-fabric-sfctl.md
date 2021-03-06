---
title: Azure Service Fabric CLI- sfctl | Microsoft Docs
description: Service Fabric CLI sfctl 명령을 설명합니다.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: b6e3c769307f037c9f31dbb3f1a86675145ec1f6
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495410"
---
# <a name="sfctl"></a>sfctl
Service Fabric 클러스터 및 엔터티 관리를 위한 명령. 이 버전은 Service Fabric 6.3 런타임과 호환됩니다.

명령은 동사 명사 패턴을 따릅니다. 자세한 내용은 하위 그룹을 참조하세요.

## <a name="subgroups"></a>하위 그룹
|하위 그룹|설명|
| --- | --- |
| [application](service-fabric-sfctl-application.md) | 응용 프로그램 및 응용 프로그램 유형을 성, 삭제 및 관리합니다. |
| [chaos](service-fabric-sfctl-chaos.md) | 비정상 상황 테스트 서비스를 시작, 중지 및 보고합니다. |
| [cluster](service-fabric-sfctl-cluster.md) | Service Fabric 클러스터를 선택, 관리 및 운영합니다. |
| [compose](service-fabric-sfctl-compose.md) | Docker Compose 응용 프로그램을 만들고, 삭제하고, 관리합니다. |
| [container](service-fabric-sfctl-container.md) | 클러스터 노드에서 컨테이너 관련 명령을 실행합니다. |
| [is](service-fabric-sfctl-is.md) | 쿼리하고 인프라 서비스에 명령을 보냅니다. |
| [node](service-fabric-sfctl-node.md) | 클러스터를 형성하는 노드를 관리합니다. |
| [partition](service-fabric-sfctl-partition.md) | 모든 서비스에 대한 파티션을 쿼리 및 관리합니다. |
| [property](service-fabric-sfctl-property.md) | Service Fabric 이름 아래에 있는 저장소 및 쿼리 속성입니다. |
| [replica](service-fabric-sfctl-replica.md) | 서비스 파티션에 속한 복제본을 관리합니다. |
| [rpm](service-fabric-sfctl-rpm.md) | 복구 관리자 서비스에 쿼리하고 명령을 보냅니다. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | 독립 실행형 Service Fabric 클러스터를 관리합니다. |
| [service](service-fabric-sfctl-service.md) | 서비스, 서비스 유형 및 서비스 패키지를 생성, 삭제 및 관리합니다. |
| [store](service-fabric-sfctl-store.md) | 클러스터 이미지 저장소에서 기본 파일 수준 작업을 수행합니다. |

## <a name="next-steps"></a>다음 단계
- Service Fabric CLI [설정](service-fabric-cli.md)
- [샘플 스크립트](/azure/service-fabric/scripts/sfctl-upgrade-application)를 사용하여 Microsoft Azure Service Fabric CLI를 사용하는 방법에 대해 알아봅니다.