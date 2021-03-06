---
title: Profileren van web-apps die worden uitgevoerd op een Azure-VM met behulp van Application Insights Profiler | Microsoft Docs
description: Web-apps voor het profiel op een Azure-VM met behulp van Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 01d57a10189f9281736e628a83465c96d282af70
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860140"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Profiel web-apps die worden uitgevoerd op een Azure-machine of een virtuele-machineschaalset instellen met behulp van Application Insights Profiler

U kunt ook Azure Application Insights Profiler implementeren voor deze services:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Profiler implementeren op een virtuele machine of een virtuele-machineschaalset
In dit artikel wordt beschreven hoe u Application Insights Profiler wordt uitgevoerd op uw virtuele Azure-machine (VM) of Azure virtuele-machineschaalset ophalen. Profiler is geïnstalleerd met de Azure Diagnostics-extensie voor VM's. De uitbreiding voor het uitvoeren van Profiler configureren en de Application Insights SDK inbouwen in uw toepassing.

1. Voeg de Application Insights-SDK op uw [ASP.NET-toepassing](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) of gewone [.NET-toepassing](windows-services.md?toc=/azure/azure-monitor/toc.json).  
  Als u profielen voor het aanvragen, moet u aanvraagtelemetrie verzenden naar Application Insights.

1. Azure Diagnostics-extensie installeren op uw virtuele machine. Zie voor sjabloonvoorbeelden in de volledige Resource Manager:  
    * [Virtuele machine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Virtuele-machineschaalset](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    Het belangrijkste deel is de ApplicationInsightsProfilerSink in de WadCfg. Toevoegen als u wilt dat Azure Diagnostics inschakelen van Profiler gegevens verzenden naar uw iKey, een andere sink in deze sectie.
    
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. De implementatiedefinitie gewijzigde omgeving implementeren.  

   De implementatie van een volledige sjabloon toepassen van de wijzigingen doorgaans omvat of een cloud service op basis van publiceren via PowerShell-cmdlets of Visual Studio.  

   De volgende PowerShell-opdrachten zijn een alternatieve benadering voor bestaande virtuele machines die alleen de Azure Diagnostics-extensie is van toepassing. De eerder genoemde ProfilerSink toevoegen aan de configuratie die wordt geretourneerd door de opdracht Get-AzureRmVMDiagnosticsExtension en klikt u vervolgens de bijgewerkte configuratie doorgeven aan de opdracht Set-AzureRmVMDiagnosticsExtension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Als de bedoelde toepassing wordt uitgevoerd via [IIS](https://www.microsoft.com/web/downloads/platform.aspx), schakel de `IIS Http Tracing` Windows-functie.

   a. Externe toegang tot de omgeving tot stand brengen en gebruik vervolgens de [Windows toevoegen functies]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) venster. Of Voer de volgende opdracht in PowerShell (als beheerder):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Als het tot stand brengen van externe toegang is een probleem, kunt u de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) de volgende opdracht uit te voeren:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Implementeer uw toepassing.

## <a name="can-profiler-run-on-on-premises-servers"></a>Kan de Profiler uitvoeren op on-premises servers?
Er zijn geen plannen voor de ondersteuning van Application Insights Profiler voor on-premises servers.

## <a name="next-steps"></a>Volgende stappen

- Genereren van verkeer naar uw toepassing (bijvoorbeeld: launch een [beschikbaarheidstest](monitor-web-app-availability.md)). Wacht 10 tot 15 minuten voor traceringen om te worden verzonden naar de Application Insights-exemplaar.
- Zie [Profiler-traceringen](profiler-overview.md?toc=/azure/azure-monitor/toc.json) in Azure portal.
- Zie voor hulp bij het oplossen van problemen van Profiler, [Profiler probleemoplossing](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
