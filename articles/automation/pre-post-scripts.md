---
title: Azure에서 업데이트 관리 배포에 대한 사전 및 사후 스크립트 구성(미리 보기)
description: 이 문서에서는 업데이트 배포를 위해 사전 및 사후 스크립트를 구성하고 관리하는 방법에 대해 설명합니다.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d84596b586ea54dd4a64faf46b32226862d83198
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584624"
---
# <a name="manage-pre-and-post-scripts-preview"></a>사전 및 사후 스크립트 관리(미리 보기)

사전 및 사후 스크립트를 사용하면 업데이트 배포 이전(사전 작업)과 이후(사후 작업)에 Automation 계정에서 PowerShell Runbook을 실행할 수 있습니다. 사전 및 사후 스크립트는 로컬이 아닌 Azure 컨텍스트에서 실행됩니다. 사전 스크립트는 업데이트 배포가 시작할 때 실행됩니다. 사후 스크립트는 배포가 끝나고, 구성된 모든 다시 부팅이 진행된 후에 실행됩니다.

## <a name="runbook-requirements"></a>Runbook 요구 사항

Runbook을 사전 또는 사후 스크립트로 사용하려면 Runbook을 Automation 계정으로 가져와서 게시해야 합니다. 이 프로세스에 대한 자세한 내용은 [Runbook 게시](automation-creating-importing-runbook.md#publishing-a-runbook)를 참조하세요.

## <a name="using-a-prepost-script"></a>사전/사후 스크립트 사용

업데이트 배포에서 사전 및 사후 스크립트를 사용하려면 업데이트 배포를 만들어야 합니다. **사전 스크립트 + 사후 스크립트(미리 보기)** 를 선택합니다. 그러면 **사전 스크립트 + 사후 스크립트 선택** 페이지가 열립니다.  

![스크립트 선택](./media/pre-post-scripts/select-scripts.png)

사용하려는 스크립트를 선택합니다. 이 예에는 **UpdateManagement-TurnOnVms** Runbook이 사용되었습니다. Runbook을 선택하면 **스크립트 구성** 페이지가 열리고, 매개 변수 값을 제공하고, **사전 스크립트**를 선택합니다. 완료되면 **확인**을 클릭합니다.

![스크립트 구성](./media/pre-post-scripts/configure-script.png)

**UpdateManagement-TurnOffVms** 스크립트에 대해 이 프로세스를 반복합니다. 그러나 **스크립트 유형**을 선택할 때는 **사후 스크립트**를 선택합니다.

**선택한 항목** 섹션에 선택한 스크립트가 모두 표시되고 on은 사전 스크립트이고 다른 하나는 사후 스크립트입니다.

![선택한 항목](./media/pre-post-scripts/selected-items.png)

업데이트 배포 구성을 마칩니다.

업데이트 배포가 완료되면 **업데이트 배포**로 이동하여 결과를 볼 수 있습니다. 여기서 알 수 있듯이 사전 스크립트 및 사후 스크립트의 상태가 제공됩니다.

![업데이트 결과](./media/pre-post-scripts/update-results.png)

업데이트 배포 실행을 클릭하면 사전 및 사후 스크립트에 대한 추가 세부 정보가 제공됩니다. 실행 시 스크립트 원본에 대한 링크가 제공됩니다.

![배포 실행 결과](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>매개 변수 전달

사전 및 사후 스크립트가 구성되면 Runbook 예약과 같은 매개 변수를 전달할 수 있습니다. 매개 변수는 업데이트 배포를 만들 때 정의됩니다. 표준 Runbook 매개 변수 외에도 추가 매개 변수가 제공됩니다. 이 매개 변수는 **SoftwareUpdateConfigurationRunContext**입니다. 이 매개 변수는 JSON 문자열이며, 사전 또는 사후 스크립트에 매개 변수가 정의되면 업데이트 배포에서 자동으로 전달됩니다. 이 매개 변수에는 [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration)에서 반환되는 정보의 하위 집합인 업데이트 배포에 대한 정보가 포함되어 있습니다. 다음 표에서는 변수에 제공되는 속성을 보여 줍니다.

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext 속성

|자산  |설명  |
|---------|---------|
|SoftwareUpdateConfigurationName     | 소프트웨어 업데이트 구성의 이름        |
|SoftwareUpdateConfigurationRunId     | 실행에 대한 고유 ID        |
|SoftwareUpdateConfigurationSettings     | 소프트웨어 업데이트 구성과 관련된 속성의 컬렉션         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | 업데이트 배포를 대상으로 하는 운영 체제         |
|SoftwareUpdateConfigurationSettings.duration     | 업데이트 배포의 최대 기간은 ISO8601에 따라 `PT[n]H[n]M[n]S`로 실행됩니다. "유지 관리 기간"이라고도 합니다.          |
|SoftwareUpdateConfigurationSettings.Windows     | Windows 컴퓨터와 관련된 속성의 컬렉션         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | 업데이트 배포에서 제외되는 KB(기술 자료) 목록        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | 업데이트 배포에 대해 선택한 업데이트 분류        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | 업데이트 배포에 대한 다시 부팅 설정        |
|azureVirtualMachines     | 업데이트 배포의 Azure VM에 대한 resourceId 목록        |
|nonAzureComputerNames|업데이트 배포의 비 Azure 컴퓨터 FQDN 목록|

다음 예제는 **SoftwareUpdateConfigurationRunContext** 매개 변수에 전달된 JSON 문자열입니다.

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ], 
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

모든 속성이 포함된 전체 예제는 [소프트웨어 업데이트 구성 - 이름으로 가져오기](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)에서 찾을 수 있습니다.

> [!NOTE]
> [동적 그룹(미리 보기)](automation-update-management.md#using-dynamic-groups)을 사용하여 배포에 추가된 컴퓨터는 현재 **SoftwareUpdateConfigurationRunContext** 매개 변수의 일부가 아닙니다.

## <a name="samples"></a>샘플

사전 및 사후 스크립트에 대한 샘플은 [스크립트 센터 갤러리](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell)에서 찾을 수 있거나 Azure Portal을 통해 가져올 수 있습니다. 포털을 통해 가져오려면 Automation 계정의 **프로세스 자동화** 아래에서 **Runbook 갤러리**를 선택합니다. 필터에 대해 **업데이트 관리**를 사용합니다.

![갤러리 목록](./media/pre-post-scripts/runbook-gallery.png)

또는 다음 목록에서 표시한 대로 스크립트 이름으로 검색할 수 있습니다.

* 업데이트 관리 - VM 켜기
* 업데이트 관리 - VM 끄기
* 업데이트 관리 - 로컬에서 스크립트 실행
* 업데이트 관리 - 사전/사후 스크립트에 대한 템플릿
* 업데이트 관리 - 실행 명령을 사용하여 스크립트 실행

> [!IMPORTANT]
> 가져온 Runbook은 먼저 **게시**한 후에 사용해야 합니다. Automation 계정에서 Runbook을 찾으려면 **편집**을 선택하고 **게시**를 클릭합니다.

샘플은 모두 다음 예제에 정의된 기본 템플릿을 기반으로 합니다. 이 템플릿은 사전 및 사후 스크립트에 사용할 사용자 고유의 Runbook을 만드는 데 사용할 수 있습니다. Azure를 통해 인증하고 `SoftwareUpdateConfigurationRunContext` 매개 변수를 처리하는 데 필요한 논리가 포함되어 있습니다.

```powershell
<# 
.SYNOPSIS 
 Barebones script for Update Management Pre/Post 
 
.DESCRIPTION 
  This script is intended to be run as a part of Update Management Pre/Post scripts.  
  It requires a RunAs account. 
 
.PARAMETER SoftwareUpdateConfigurationRunContext 
  This is a system variable which is automatically passed in by Update Management during a deployment. 
#> 
 
param( 
    [string]$SoftwareUpdateConfigurationRunContext 
) 
#region BoilerplateAuthentication 
#This requires a RunAs account 
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection' 
 
Add-AzureRmAccount ` 
    -ServicePrincipal ` 
    -TenantId $ServicePrincipalConnection.TenantId ` 
    -ApplicationId $ServicePrincipalConnection.ApplicationId ` 
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint 
 
$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID 
#endregion BoilerplateAuthentication 
 
#If you wish to use the run context, it must be converted from JSON 
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext 
#Access the properties of the SoftwareUpdateConfigurationRunContext 
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines 
$runId = $context.SoftwareUpdateConfigurationRunId 
 
Write-Output $context 
 
#Example: How to create and write to a variable using the pre-script: 
<# 
#Create variable named after this run so it can be retrieved 
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false 
#Set value of variable  
Set-AutomationVariable –Name $runId -Value $vmIds 
#> 
 
#Example: How to retrieve information from a variable set during the pre-script 
<# 
$variable = Get-AutomationVariable -Name $runId 
#>      
```

## <a name="interacting-with-non-azure-machines"></a>비 Azure 머신과의 상호 작용

사전 및 사후 작업은 Azure 컨텍스트에서 실행되며 비 Azure 머신에는 액세스할 수 없습니다. 비 Azure 머신과 상호 작용하려면 다음 항목이 있어야 합니다.

* 실행 계정
* 머신에 설치된 Hybrid Runbook Worker
* 로컬에서 실행하려는 Runbook
* 부모 Runbook

비 Azure 머신과 상호 작용하기 위해 부모 Runbook이 Azure 컨텍스트에서 실행됩니다. 이 Runbook은 [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet을 사용하여 자식 Runbook을 호출합니다. `-RunOn` 매개 변수를 지정하고, 스크립트가 실행될 Hybrid Runbook Worker의 이름을 제공해야 합니다.

```powershell
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$resourceGroup = "AzureAutomationResourceGroup"
$aaName = "AzureAutomationAccountName"

$output = Start-AzureRmAutomationRunbook -Name "StartService" -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName -RunOn "hybridWorker"

$status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
while ($status.status -ne "Completed")
{ 
    Start-Sleep -Seconds 5
    $status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
}

$summary = Get-AzureRmAutomationJobOutput -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName

if ($summary.Type -eq "Error")
{
    Write-Error -Message $summary.Summary
}
```

## <a name="known-issues"></a>알려진 문제

* 사전 및 사후 스크립트를 사용하는 경우 개체 또는 배열을 매개 변수에 전달할 수 없습니다. Runbook이 실패합니다.

## <a name="next-steps"></a>다음 단계

Windows 가상 머신에 대한 업데이트를 관리하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Azure Windows VM에 대한 업데이트 및 패치 관리](automation-tutorial-update-management.md)
