---
title: Microsoft의 기계 학습 제품 옵션 비교 - Azure | Microsoft Docs
description: Microsoft의 다양한 제품을 비교하여 기계 학습 모델을 빌드, 배포 및 관리합니다. 솔루션에 대해 선택할 제품을 결정합니다.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.openlocfilehash: ad6cb53bd69218d2af801cb56f882f5ffde3026c
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872555"
---
# <a name="what-are-the-machine-learning-product-options-from-microsoft"></a>Microsoft의 기계 학습 제품 옵션이란?

Microsoft에서는 다양한 제품 옵션을 제공하여 기계 학습 모델을 빌드, 배포 및 관리합니다. 이러한 제품을 비교하고 기계 학습 솔루션을 가장 효과적으로 개발해야 하는 항목을 선택합니다.

| 기계 학습 제품 | 정의 | 수행할 수 있는 작업 |
|-|-|-|
| ***클라우드*** | | |
| [Azure Machine Learning 서비스](#azure-machine-learning-services) | ML(Machine Learning)에 대해 관리되는 클라우드 서비스  | Python 및 CLI를 사용하여 Azure에서 모델을 학습, 배포 및 관리 |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | ML에 대한 끌어&ndash;서&ndash; 놓기 시각적 인터페이스 | 미리 구성된 알고리즘을 사용하여 모델을 빌드, 실험 및 배포 |
| [Azure Databricks](#azure-databricks) | Spark 기반 분석 플랫폼 | 모델 및 데이터 워크플로를 빌드 및 배포 |
| [Azure Cognitive Services](#azure-cognitive-services) | 미리 빌드된 AI 및 ML 모델을 사용한 Azure 서비스 | 앱에 지능형 기능을 쉽게 추가 |
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | 미리 설치된 데이터 과학 도구를 사용한 가상 머신 | 미리 구성된 환경에서 ML 솔루션을 개발 |
| ***온-프레미스***<br>*(온-프레미스 서버도 클라우드의 가상 머신에서 실행할 수 있습니다)* | | |
| [SQL Server Machine Learning 서비스](#sql-server-machine-learning-services) | SQL에 포함된 Analytics 엔진 | SQL Server 내에 모델을 빌드 및 배포 |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | 예측 분석에 대한 독립 실행형 엔터프라이즈 서버 | R 및 Python을 사용하여 모델을 빌드 및 배포 |
| ***개발자 도구*** | | |
| [ML.NET](#mlnet) | 오픈 소스, 플랫폼 간 ML SDK | .NET 응용 프로그램에 대한 ML 솔루션 개발 |
| [Windows ML](#windows-ml) | Windows 10 ML 플랫폼 | Windows 10 디바이스에서 학습된 모델을 평가 |

## <a name="azure-machine-learning-service"></a>Azure Machine Learning 서비스

[Azure Machine Learning 서비스](overview-what-is-azure-ml.md)는 대규모로 ML 모델을 학습, 배포 및 관리하는 데 사용되는 완벽한 관리형 클라우드 서비스입니다. 이 서비스는 오픈 소스 기술을 완벽히 지원하여 TensorFlow, PyTorch 및 scikit-learn 등의 수많은 오픈 소스 Python 패키지를 사용할 수 있습니다. [Azure 노트북](https://notebooks.azure.com/), [Jupyter 노트북](http://jupyter.org) 또는 [Visual Studio Code용 Azure Machine Learning](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/)와 같이 다양한 도구도 제공되어 데이터를 쉽게 검색하고 변환하게 한 다음, 모델을 학습 및 배포합니다. Azure Machine Learning 서비스에는 간편하고 효율적이고 정확한 모델 생성 및 튜닝을 자동화하는 기능이 포함됩니다.

클라우드 규모의 Python 및 CLI를 통해 ML 모델을 학습, 배포 및 관리하려면 Azure Machine Learning 서비스를 사용합니다.

>[!Note]
> Azure Machine Learning은 추가 비용 없이 사용해 볼 수 있습니다. 신용 카드 또는 Azure 구독이 필요하지 않습니다. 지금 시작해 보세요. https://azure.microsoft.com/free/

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](../studio/what-is-ml-studio.md)는 대화형 시각적 작업 영역을 제공하여 미리 빌드된 기계 학습 알고리즘을 통해 모델을 쉽고 빠르게 빌드, 테스트 및 배포하는 데 사용할 수 있습니다. Machine Learning Studio는 Excel과 같은 BI 도구 또는 사용자 지정 앱에서 쉽게 사용할 수 있는 웹 서비스로 모델을 게시합니다.
프로그래밍이 필요없습니다 - 대화형 캔버스에서 데이터 집합 및 분석 모듈을 연결하여 기계 학습 모델을 구성한 다음, 몇 번의 클릭으로 배포합니다.

코드가 필요없는 모델을 개발하고 배포하려는 경우 Machine Learning Studio를 사용하세요.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)는 Microsoft Azure Cloud Services 플랫폼에 대해 최적화된 Apache Spark 기반 분석 플랫폼입니다. Databricks는 Azure와 통합되어 데이터 과학자, 데이터 엔지니어, 비즈니스 분석가가 공동 작업할 수 있도록 하는 대화형 작업 영역, 간소화된 워크플로 및 원클릭 설정을 제공합니다.
웹 기반 노트북에서 Python, R, Scala 및 SQL 코드를 사용하여 데이터를 쿼리, 시각화 및 모델링합니다.

Apache Spark에서 기계 학습 솔루션을 빌드하는 데 공동 작업하려는 경우 Databricks를 사용합니다.

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

[Azure Cognitive Services](/azure/cognitive-services/welcome)는 자연스러운 통신 방법을 사용하는 앱을 빌드할 수 있는 API 집합입니다. 이러한 API를 사용하면 단 몇 줄의 코드로 사용자 앱이 사용자의 요구를 보고, 듣고, 말하고, 이해하며, 해석할 수 있습니다. 다음과 같은 앱에 지능형 기능을 쉽게 추가합니다. 

- 이모티콘 및 감정 검색
- 음성 인식
- LUIS(언어 해석)
- 지식 및 검색

Cognitive Services를 사용하여 디바이스 및 플랫폼에서 앱을 개발합니다. API는 끊임없이 개선되며, 설치하기가 매우 쉽습니다.

## <a name="azure-data-science-virtual-machine"></a>Azure 데이터 과학 가상 머신

[Data Science Virtual Machine](../data-science-virtual-machine/overview.md)은 데이터 과학을 수행하기 위해 특별히 빌드된 Microsoft Azure 클라우드에서 사용자 지정된 가상 머신 환경입니다. 여기에는 고급 분석을 위한 지능형 응용 프로그램 구축에 바로 뛰어들 수 있도록 다수의 유명한 데이터 과학 및 기타 도구가 미리 설치 및 구성되어 있습니다.

Data Science Virtual Machine은 Azure Machine Learning 서비스의 대상으로 지원됩니다.
또한 Windows와 Linux Ubuntu용 버전에서도 사용할 수 있습니다(Linux CentOS에서는 Azure Machine Learning 서비스를 지원하지 않습니다).
특정 버전 정보 및 포함된 항목 목록은 [Azure Data Science Virtual Machine 소개](../data-science-virtual-machine/overview.md)를 참조하세요.

단일 노드에서 작업을 실행하거나 호스트해야 하는 경우 데이터 과학 VM을 사용하세요. 또는 단일 컴퓨터에서 처리를 원격으로 강화해야 하는 경우에 사용합니다.

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning 서비스

[SQL Server Microsoft Machine Learning 서비스](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)는 SQL Server 데이터베이스의 관계형 데이터를 위해 R 및 Python에 통계 분석, 데이터 시각화 및 예측 분석을 추가합니다. Microsoft의 R 및 Python 라이브러리에는 SQL Server에서 대규모 및 병렬로 실행할 수 있는 고급 모델링 및 ML 알고리즘이 포함됩니다.

기본 제공 AI 및 SQL Server의 관계형 데이터에 대한 예측 분석이 필요한 경우 SQL Server Machine Learning Services를 사용합니다.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning 서버

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)는 R 및 Python 프로세스의 병렬 및 분산 워크로드를 호스트하고 관리하기 위한 엔터프라이즈 서버입니다. Microsoft Machine Learning Server는 Linux, Windows, Hadoop 및 Apache Spark에서 실행되며, [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/)에서도 사용할 수 있습니다. 이 서버는 [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) 및 [MicrosoftML 패키지](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)를 사용하여 빌드된 솔루션용 실행 엔진을 제공하며, 고성능 분석, 통계 분석, 기계 학습 및 대규모 데이터 집합을 지원하여 오픈 소스 R 및 Python을 확장합니다. 이 기능은 서버와 함께 설치되는 전용 패키지를 통해 제공됩니다. 개발을 위해 [Visual Studio용 R 도구](https://www.visualstudio.com/vs/rtvs/) 및 [Visual Studio용 Python 도구](https://www.visualstudio.com/vs/python/)와 같은 IDE를 사용할 수 있습니다.

서버에서 R 및 Python을 통해 구축된 모델을 빌드하고 운영하거나, Hadoop 또는 Spark 클러스터에서 대규모로 학습하는 R 및 Python을 배포해야 하는 경우 Microsoft Machine Learning Server를 사용합니다.

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/)는 무료, 오픈 소스, 플랫폼 간 기계 학습 프레임워크로서 사용자 지정 기계 학습 솔루션을 빌드해서 .NET 응용 프로그램에 통합할 수 있습니다.

.NET 응용 프로그램에 기계 학습 솔루션을 통합하려는 경우 ML.NET를 사용합니다.

## <a name="windows-ml"></a>Windows ML

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/)은 응용 프로그램에서 학습된 기계 학습 모델을 사용하면서 Windows 10 장치에서 로컬로 학습된 모델을 평가할 수 있습니다.

Windows 응용 프로그램 내에서 학습된 기계 학습 모델을 사용하려는 경우 Windows ML을 사용합니다.

## <a name="next-steps"></a>다음 단계

- Microsoft의 사용 가능한 모든 AI(인공지능) 개발 제품을 알아보려면 [Microsoft AI 플랫폼](https://www.microsoft.com/ai)을 참조
- AI 솔루션을 개발하는 방법에 대한 학습은 [Microsoft AI School](https://aischool.microsoft.com/learning-paths)을 참조
