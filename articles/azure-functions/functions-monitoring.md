---
title: Azure Functions controleren
description: Informatie over het gebruik van Azure Application Insights met Azure Functions voor het bewaken van een functie wordt uitgevoerd.
services: functions
author: ggailey777
manager: jeconnoc
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: glenga
ms.openlocfilehash: d4ff009c11b3a0f2ebe97e5c452f078eaa529fc3
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301720"
---
# <a name="monitor-azure-functions"></a>Azure Functions controleren

[Azure Functions](functions-overview.md) biedt ingebouwde integratie met [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) voor het bewaken van functies. In dit artikel laat zien hoe het configureren van functies voor het verzenden van het systeem gegenereerde logboekbestanden naar Application Insights.

![Application Insights Metrics Explorer](media/functions-monitoring/metrics-explorer.png)

Functions ook heeft [geïntegreerde bewaking die geen gebruik maakt van Application Insights](#monitoring-without-application-insights). We raden u Application Insights omdat deze optie biedt meer gegevens en betere manieren om de gegevens te analyseren.

## <a name="application-insights-pricing-and-limits"></a>Prijzen van Application Insights en limieten

U kunt gratis Application Insights-integratie met de functie-Apps uitproberen. Er is echter een dagelijkse limiet voor de hoeveelheid gegevens kan gratis worden verwerkt, en u deze limiet tijdens het testen mogelijk bereikt. Als uw dagelijkse limiet bijna is bereikt, biedt Azure portal en e-mailmeldingen.  Maar als u deze waarschuwingen mist en de limiet bereikt, nieuwe logboeken wordt niet weergegeven in Application Insights-query's. Dus rekening houden met de limiet om te voorkomen dat onnodig tijd voor het oplossen van problemen. Zie voor meer informatie, [beheren van prijzen en gegevensvolumes in Application Insights](../azure-monitor/app/pricing.md).

## <a name="enable-app-insights-integration"></a>App Insights-integratie inschakelen

Voor een functie-app om gegevens te verzenden naar Application Insights, moet deze weten de instrumentatiesleutel van een Application Insights-resource. De sleutel moet zich in een app-instelling met de naam **APPINSIGHTS_INSTRUMENTATIONKEY**.

U kunt instellen om deze verbinding in de [Azure-portal](https://portal.azure.com):

* [Automatisch voor een nieuwe functie-app](#new-function-app)
* [Handmatig verbinding maken met een App Insights-resource](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Nieuwe functie-app

1. Ga naar de functie-app **maken** pagina.

1. Stel de **Application Insights** overschakelen **op**.

1. Selecteer een **Application Insights-locatie**. Kies de regio die zich het dichtst bij de regio van uw functie-app in een [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) waar u uw gegevens worden opgeslagen.

   ![Application Insights inschakelen tijdens het maken van een functie-app](media/functions-monitoring/enable-ai-new-function-app.png)

1. Voer de vereiste gegevens in en selecteer **maken**.

De volgende stap is het [ingebouwde logboekregistratie uitschakelen](#disable-built-in-logging).

### <a name="manually-connect-an-app-insights-resource"></a>Handmatig verbinding maken met een App Insights-resource 

1. Maken van de Application Insights-resource. Toepassingstype ingesteld op **algemene**.

   ![Maken van een Application Insights-resource, typt u algemene](media/functions-monitoring/ai-general.png)

1. Kopieer de instrumentatiesleutel van de **Essentials** pagina van de Application Insights-resource. Beweeg de muisaanwijzer over het einde van de weergegeven waarde van de sleutel aan een **Klik om te kopiëren** knop.

   ![De Application Insights-instrumentatiesleutel kopiëren](media/functions-monitoring/copy-ai-key.png)

1. In de functie-app **toepassingsinstellingen** pagina [toevoegen van een app-instelling](functions-how-to-use-azure-function-app-settings.md#settings) door te klikken op **nieuwe instelling toevoegen**. Naam van de nieuwe instelling APPINSIGHTS_INSTRUMENTATIONKEY en plak de gekopieerde instrumentatiesleutel.

   ![Instrumentatiesleutel toevoegen aan app-instellingen](media/functions-monitoring/add-ai-key.png)

1. Klik op **Opslaan**.

## <a name="disable-built-in-logging"></a>Ingebouwde logboekregistratie uitschakelen

Wanneer u Application Insights hebt ingeschakeld, uitgeschakeld. de [ingebouwde logboekregistratie die gebruikmaakt van Azure storage](#logging-to-storage). De ingebouwde logboekregistratie is handig voor het testen met lichte workloads, maar is niet bedoeld voor gebruik in productieomgevingen hoge belasting. Voor het bewaken van de productie, wordt Application Insights aanbevolen. Als de ingebouwde logboekregistratie in productie wordt gebruikt, is het mogelijk dat de record logboekregistratie onvolledig vanwege een beperking in Azure Storage.

Als u wilt ingebouwde logboekregistratie uitschakelen, verwijdert de `AzureWebJobsDashboard` app-instelling. Zie voor meer informatie over het verwijderen van app-instellingen in de Azure-portal, de **toepassingsinstellingen** sectie van [over het beheren van een functie-app](functions-how-to-use-azure-function-app-settings.md#settings). Voordat u verwijdert de app-instelling, ervoor zorgen dat er geen bestaande functies in dezelfde functie-app voor Azure Storage-triggers of bindingen gebruiken.

## <a name="view-telemetry-in-monitor-tab"></a>Telemetrie weergeven in het tabblad Monitor

Nadat u hebt Application Insights-integratie ingesteld zoals wordt weergegeven in de vorige secties, kunt u bekijken telemetrische gegevens in de **Monitor** tabblad.

1. Selecteer in de pagina functie-app een functie die ten minste één keer is uitgevoerd nadat de Application Insights is geconfigureerd, en selecteer vervolgens de **Monitor** tabblad.

   ![Tabblad Monitor selecteren](media/functions-monitoring/monitor-tab.png)

1. Selecteer **vernieuwen** periodiek totdat de lijst van functieaanroepen wordt weergegeven.

   Het duurt tot vijf minuten voor de lijst worden weergegeven, vanwege de manier waarop de telemetriegegevens voor het batches van client voor verzending naar de server. (Deze vertraging niet van toepassing op de [Live Metrics Stream](../azure-monitor/app/live-stream.md). Deze service maakt verbinding met de Functions-host wanneer u de pagina laadt, zodat logboeken rechtstreeks naar de pagina worden gestreamd.)

   ![Lijst met aanroepen](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. De logboeken voor een bepaalde functie-aanroep, selecteer de **datum** kolomkoppeling voor deze aanroep.

   ![Aanroepdetails koppelen](media/functions-monitoring/invocation-details-link-ai.png)

   De uitvoer van de logboekregistratie voor deze aanroep wordt weergegeven in een nieuwe pagina.

   ![Aanroepdetails](media/functions-monitoring/invocation-details-ai.png)

Pagina's (aanroep lijst en details) een koppeling naar de Application Insights Analytics-query die de gegevens worden opgehaald:

![Uitvoeren in Application Insights](media/functions-monitoring/run-in-ai.png)

![Lijst met Application Insights Analytics aanroep](media/functions-monitoring/ai-analytics-invocation-list.png)

In deze query's, kunt u zien dat de aanroep-lijst beperkt tot de laatste 30 dagen, niet meer dan 20 rijen is (`where timestamp > ago(30d) | take 20`) en de lijst aanroep voor de afgelopen 30 dagen met geen limiet is.

Zie voor meer informatie, [telemetriegegevens op te vragen](#query-telemetry-data) verderop in dit artikel.

## <a name="view-telemetry-in-app-insights"></a>Telemetrie in de App Insights weergeven

Voor Application Insights openen vanuit een functie-app in Azure portal, selecteert u de **Application Insights** herstelkoppeling in de **functies geconfigureerd** sectie van de functie-app **overzicht** pagina.

![Application Insights-koppeling op de pagina overzicht](media/functions-monitoring/ai-link.png)

Zie voor meer informatie over het gebruik van Application Insights de [documentatie voor Application Insights](https://docs.microsoft.com/azure/application-insights/). In deze sectie ziet u enkele voorbeelden van hoe u gegevens weergeven in Application Insights. Als u al bekend met Application Insights bent, gaat u rechtstreeks naar [in de secties over het configureren en aanpassen van de telemetriegegevens](#configure-categories-and-log-levels).

In [Metrics Explorer](../azure-monitor/app/metrics-explorer.md), kunt u grafieken maken en waarschuwingen op basis van metrische gegevens zoals als aantal functieaanroepen, uitvoeringstijd en slagingspercentage.

![Metrics Explorer](media/functions-monitoring/metrics-explorer.png)

Op de [fouten](../azure-monitor/app/asp-net-exceptions.md) tabblad kunt u diagrammen maken en waarschuwingen op basis van functie fouten en server uitzonderingen. De **bewerkingsnaam** is de naam van de functie. Fouten in de afhankelijkheden worden niet weergegeven, tenzij u implementeren [aangepaste telemetrie](#custom-telemetry-in-c-functions) voor afhankelijkheden.

![Fouten](media/functions-monitoring/failures.png)

Op de [prestaties](../azure-monitor/app/performance-counters.md) tabblad kunt u prestatieproblemen analyseren.

![Prestaties](media/functions-monitoring/performance.png)

De **Servers** tabblad bevat Resourcegebruik en doorvoer per server. Deze gegevens is handig voor het opsporen van fouten in scenario's waarbij functies worden bogging uw onderliggende resources. Servers worden aangeduid als **rolinstanties in de Cloud**.

![Servers](media/functions-monitoring/servers.png)

De [Live Metrics Stream](../azure-monitor/app/live-stream.md) tabblad bevat metrische gegevens zoals deze is gemaakt in realtime.

![Livestream](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Telemetriegegevens op te vragen

[Application Insights Analytics](../azure-monitor/app/analytics.md) krijgt u toegang tot alle van de telemetriegegevens in de vorm van tabellen in een database. Analytics biedt een querytaal voor uitpakken, bewerken en de gegevens te visualiseren.

![Selecteer Analytics](media/functions-monitoring/select-analytics.png)

![Voorbeeld van Analytics](media/functions-monitoring/analytics-traces.png)

Hier volgt een voorbeeld waarin de distributie van aanvragen per worker in de afgelopen 30 minuten.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

De tabellen die beschikbaar zijn worden weergegeven in de **Schema** tabblad van het linkerdeelvenster. Hier vindt u gegevens die zijn gegenereerd door de functieaanroepen in de volgende tabellen:

* **traceringen** -logboeken die zijn gemaakt door de runtime en functiecode aan te geven.
* **aanvragen** : één voor elke functieaanroep.
* **uitzonderingen** - eventuele uitzonderingen die door de runtime.
* **customMetrics** -telling van geslaagde en mislukte aanroepen, slagingspercentage, duur.
* **customEvents** -gebeurtenissen bijgehouden door de runtime, bijvoorbeeld:  HTTP-aanvragen die een functie geactiveerd.
* **performanceCounters** -informatie over de prestaties van de servers die de functies worden uitgevoerd op.

De andere tabellen zijn voor beschikbaarheidstests en clientbrowser/telemetrie. U kunt aangepaste telemetrie om toe te voegen gegevens om ze te implementeren.

Binnen elke tabel die een deel van de Functions-specifieke gegevens is in een `customDimensions` veld.  Bijvoorbeeld, de volgende query haalt alle traceringen waarvoor logboek-niveau `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

De runtime biedt `customDimensions.LogLevel` en `customDimensions.Category`. U kunt opgeven dat aanvullende velden in de logboeken die u in uw functiecode aan te geven. Zie [gestructureerde logboekregistratie](#structured-logging) verderop in dit artikel.

## <a name="configure-categories-and-log-levels"></a>Configureren van de categorieën en meld u niveaus

U kunt Application Insights gebruiken zonder aangepaste configuratie, maar de standaard-configuratie kan leiden tot grote hoeveelheden gegevens. Als u een Azure van Visual Studio-abonnement, kunt u mogelijk uw datalimiet bereikt voor Application Insights. De rest van dit artikel wordt beschreven hoe configureren en aanpassen van de gegevens die uw functies naar Application Insights verzenden.

### <a name="categories"></a>Categorieën

De logger Azure Functions bevat een *categorie* voor elk logboek. De categorie geeft aan welk deel van de runtimecode of uw functiecode aan te geven het logboek geschreven. 

De Functions-runtime maakt logboeken die een categorie die begint met 'Host' hebben. Bijvoorbeeld de '-functie aan de slag,""functie uitgevoerd' en ' voltooid ' functielogboeken categorie 'Host.Executor' hebben. 

Als u Logboeken in uw functiecode aan te geven schrijven, is de categorie 'Functie'.

### <a name="log-levels"></a>Logboekniveaus

Het Azure functions-logboek bevat ook een *melden niveau* met elk logboek. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) een opsomming, en de code geheel getal geeft aan dat het relatieve belang:

|LogLevel    |Code|
|------------|---|
|Tracering       | 0 |
|Fouten opsporen       | 1 |
|Informatie | 2 |
|Waarschuwing     | 3 |
|Fout       | 4 |
|Kritiek    | 5 |
|Geen        | 6 |

Meld u niveau `None` in de volgende sectie wordt uitgelegd. 

### <a name="configure-logging-in-hostjson"></a>Logboekregistratie in host.json configureren

De *[host.json](functions-host-json.md)* bestand configureert hoeveel logboekregistratie een functie-app wordt verzonden naar Application Insights. Voor elke categorie, geeft u aan het minimale logboek-niveau te verzenden. Er zijn twee voorbeelden, een die gericht is op de [runtime van Functions versie 2.x](functions-versions.md#version-2x) (.NET Core) en één voor de versie 1.x-runtime.

### <a name="version-2x"></a>Versie 2.x

De runtime v2.x gebruikt de [.NET Core logboekregistratie filterhiërarchie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

In dit voorbeeld stelt u de volgende regels:

1. Voor logboeken met categorie `Host.Results` **` or `** `Function`, alleen verzenden `Error` niveau en van boven naar Application Insights. De logboeken voor `Warning` niveau en hieronder worden genegeerd.
2. Voor de logboeken met categorie `Host.Aggregator`, alle logboeken verzenden naar Application Insights. De `Trace` logboek-niveau is hetzelfde als wat u kunt sommige noemen `Verbose`, maar gebruik `Trace` in de [host.json](functions-host-json.md) bestand.
3. Voor alle andere logboeken verzenden alleen `Information` niveau en van boven naar Application Insights.

De categoriewaarde in [host.json](functions-host-json.md) bepaalt de logboekregistratie voor alle categorieën die met dezelfde waarde beginnen. Bijvoorbeeld, `Host` in [host.json](functions-host-json.md) besturingselementen voor logboekregistratie `Host.General`, `Host.Executor`, `Host.Results`, enzovoort.

Als [host.json](functions-host-json.md) omvat meerdere categorieën die met de dezelfde tekenreeks beginnen langer die eerst worden vergeleken. Stel bijvoorbeeld dat u wilt dat alles, van de runtime, behalve `Host.Aggregator` om aan te melden bij `Error` niveau, maar u wilt dat `Host.Aggregator` om aan te melden bij de `Information` niveau:

### <a name="version-2x"></a>Versie 2.x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Als u wilt onderdrukken alle logboeken voor een categorie, kunt u logboek-niveau `None`. Er zijn geen logboeken worden geschreven in die categorie en er is geen logboek-niveau erboven.

De volgende secties worden de belangrijkste categorieën van logboeken die de runtime wordt gemaakt. 

### <a name="category-hostresults"></a>Categorie Host.Results

Deze logboeken weergegeven als 'requests' in Application Insights. Ze geven aan slagen of mislukken van een functie.

![Grafiek van aanvragen](media/functions-monitoring/requests-chart.png)

Al deze logboeken worden geschreven op `Information` niveau, dus als u filteren op `Warning` of hoger, ziet u niet een van deze gegevens.

### <a name="category-hostaggregator"></a>Categorie Host.Aggregator

Deze logboeken bieden aantallen en gemiddelde van de functieaanroepen via een [configureerbare](#configure-the-aggregator) periode tijd wordt opgelost. De standaardperiode is 30 seconden of 1000 resultaten, afhankelijk van wat het eerste komt. 

De logboeken zijn beschikbaar in de **customMetrics** tabel in Application Insights. Voorbeelden zijn nummer wordt uitgevoerd, slagingspercentage en duur.

![customMetrics query](media/functions-monitoring/custom-metrics-query.png)

Al deze logboeken worden geschreven op `Information` niveau, dus als u filteren op `Warning` of hoger, ziet u niet een van deze gegevens.

### <a name="other-categories"></a>Andere categorieën

Alle logboeken voor categorieën dan degene die al zijn beschikbaar in de **traceringen** tabel in Application Insights.

![traceringen query](media/functions-monitoring/analytics-traces.png)

Alle logboeken met categorieën die met 'Host beginnen' worden geschreven door de Functions-runtime. De 'Functie gestart' en 'Functie voltooid' Logboeken hebt categorie 'Host.Executor'. Voor geslaagde uitvoeringen, deze logboeken zijn `Information` niveau; uitzonderingen zijn vastgelegd bij `Error` niveau. De runtime maakt ook `Warning` niveau zich aanmeldt, bijvoorbeeld: verzonden naar de wachtrij voor beheer van Onverwerkbare berichten in de wachtrij.

Logboeken geschreven door uw functiecode categorie 'Functie' hebben en mogelijk alle logboek-niveau.

## <a name="configure-the-aggregator"></a>De aggregator configureren

Als in de vorige sectie hebt genoteerd, combineert de runtime gegevens over de functie-uitvoeringen gedurende een bepaalde periode. De standaardperiode is 30 seconden of 1000 wordt uitgevoerd, afhankelijk van wat het eerste komt. U kunt deze instelling in configureren de [host.json](functions-host-json.md) bestand.  Hier volgt een voorbeeld:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Steekproeven configureren

Application Insights is een [steekproeven](../azure-monitor/app/sampling.md) functie die u beschermen kunt tegen het produceren van te veel telemetriegegevens op tijdstippen van de piekbelasting. Wanneer het aantal binnenkomende telemetriegegevens een opgegeven drempelwaarde overschrijdt, wordt de Application Insights willekeurig negeert een aantal inkomende objecten gestart. De standaardinstelling voor het maximum aantal items per seconde is 5. U kunt configureren van lijnen in [host.json](functions-host-json.md).  Hier volgt een voorbeeld:

### <a name="version-2x"></a>Versie 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 5
      }
    }
  }
}
```

### <a name="version-1x"></a>Versie 1.x 

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

> [!NOTE]
> [Sampling](../azure-monitor/app/sampling.md) is standaard ingeschakeld. Als u blijken te ontbreken gegevens, moet u mogelijk alleen de steekproeven-instellingen voor uw specifieke scenario bewaking aanpassen.

## <a name="write-logs-in-c-functions"></a>Schrijven Logboeken in C#-functies

U kunt Logboeken in uw functiecode aan te geven die worden weergegeven als traceringen in Application Insights schrijven.

### <a name="ilogger"></a>ILogger

Gebruik een [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parameter in uw functies in plaats van een `TraceWriter` parameter. Logboeken die zijn gemaakt met behulp van `TraceWriter` gaat u naar Application Insights, maar `ILogger` kunt u doen [gestructureerde logboekregistratie](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Met een `ILogger` -object, u kunt aanroepen `Log<level>` [uitbreidingsmethoden op ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) logboeken maken. Bijvoorbeeld, de volgende code schrijft `Information` logboeken met categorie 'Functie'.

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Gestructureerde logboekregistratie

De volgorde van de tijdelijke aanduidingen, niet de namen, bepaalt welke parameters worden gebruikt in het logboekbericht. Stel bijvoorbeeld dat u hebt de volgende code:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Als u de dezelfde tekenreeks behouden en de volgorde van de parameters, zijn de tekst van het resulterende waarden in op de verkeerde plaatsen.

Tijdelijke aanduidingen worden op deze manier afgehandeld, zodat u dit gestructureerde logboekregistratie doen kunt. Application Insights slaat de parameter naam / waarde-paren naast de bericht-tekenreeks. Het resultaat is dat de Berichtargumenten velden die u kunt een query worden op.

Als de aanroep van de methode logger er als in het vorige voorbeeld uitziet, kunt u bijvoorbeeld het veld query `customDimensions.prop__rowKey`. De `prop__` voorvoegsel wordt toegevoegd om ervoor te zorgen dat er geen conflicten tussen de velden de runtime wordt toegevoegd en uw functiecode aan te geven velden zijn toegevoegd.

U kunt ook een query op de tekenreeks voor het oorspronkelijke bericht door te verwijzen naar het veld `customDimensions.prop__{OriginalFormat}`.  

Hier volgt een voorbeeld-JSON-weergave van `customDimensions` gegevens:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="logging-custom-metrics"></a>Aangepaste metrische gegevens voor logboekregistratie  

In C#-script-functies, kunt u de `LogMetric` uitbreidingsmethode op `ILogger` te maken van aangepaste metrische gegevens in Application Insights. Hier volgt een voorbeeld-methodeaanroep:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Deze code is een alternatief voor aanroepen `TrackMetric` met behulp van [de Application Insights-API voor .NET](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>Schrijven Logboeken in JavaScript-functies

Gebruik in Node.js-functies, `context.log` om te schrijven Logboeken. Gestructureerde logboekregistratie is niet ingeschakeld.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Aangepaste metrische gegevens voor logboekregistratie  

Wanneer u gebruikmaakt van [versie 1.x](functions-versions.md#creating-1x-apps) van de Functions-runtime, Node.js-functies kunnen gebruiken de `context.log.metric` methode voor het maken van aangepaste metrische gegevens in Application Insights. Deze methode wordt momenteel niet ondersteund in versie 2.x. Hier volgt een voorbeeld-methodeaanroep:

```javascript
context.log.metric("TestMetric", 1234);
```

Deze code is een alternatief voor aanroepen `trackMetric` met behulp van [de Node.js-SDK voor Application Insights](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>Aangepaste telemetrie in C#-functies

U kunt de [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) NuGet-pakket aan aangepaste telemetrische gegevens verzenden naar Application Insights. De volgende C# voorbeeld wordt de [aangepaste telemetrie-API](../azure-monitor/app/api-custom-events-metrics.md). Het voorbeeld is voor een .NET-klassebibliotheek, maar de Application Insights-code is hetzelfde voor C#-script.

### <a name="version-2x"></a>Versie 2.x

De runtime versie 2.x maakt gebruik van nieuwe functies in Application Insights automatisch telemetrie te correleren met de huidige bewerking. Hoeft niet handmatig in te stellen de bewerking `Id`, `ParentId`, of `Name`.

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        // In Functions v2, TelemetryConfiguration.Active is initialized with the InstrumentationKey
        // from APPINSIGHTS_INSTRUMENTATIONKEY. Creating a default TelemetryClient like this will 
        // automatically use that key for all telemetry. It will also enable telemetry correlation
        // with the current operation.
        // If you require a custom TelemetryConfiguration, create it initially with
        // TelemetryConfiguration.CreateDefault() to include this automatic correlation.
        private static TelemetryClient telemetryClient = new TelemetryClient();

        [FunctionName("HttpTrigger2")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            telemetryClient.TrackMetric(metric);

            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Versie 1.x

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // This correlates all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Remove() niet aanroepen `TrackRequest` of `StartOperation<RequestTelemetry>`, omdat u dubbele aanvragen voor een functie-aanroep ziet.  Aanvragen automatisch worden bijgehouden in de Functions-runtime.

Stel `telemetryClient.Context.Operation.Id`. Dit is een algemene instelling en zullen onjuist correlatie wanneer er veel functies gelijktijdig worden uitgevoerd. In plaats daarvan, maak een nieuw exemplaar van de telemetrie (`DependencyTelemetry`, `EventTelemetry`) en wijzig de `Context` eigenschap. Geeft u in het exemplaar van de telemetrie naar de bijbehorende `Track` methode voor `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Dit zorgt ervoor dat de telemetrische gegevens de juiste correlatiedetails voor de huidige functie-aanroep is.

## <a name="custom-telemetry-in-javascript-functions"></a>Aangepaste telemetrie in JavaScript-functies

De [Application Insights Node.js SDK](https://www.npmjs.com/package/applicationinsights) is momenteel in de bètafase. Hier volgt een voorbeeld van code waarmee aangepaste telemetrie wordt verzonden naar Application Insights:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    context.done();
};
```

De `tagOverrides` parametersets `operation_Id` aan van de functie aanroep-ID. Deze instelling kunt u correlaties zichtbaar maken tussen alle van de automatisch gegenereerde en aangepaste telemetrie voor een bepaalde functie-aanroep.

## <a name="known-issues"></a>Bekende problemen

### <a name="dependencies"></a>Afhankelijkheden

Afhankelijkheden die de functie met andere services heeft automatisch niet weergegeven, maar kunt u aangepaste code om de afhankelijkheden weer te geven. De voorbeeldcode in de [C# aangepaste telemetrie sectie](#custom-telemetry-in-c-functions) laat zien hoe. De voorbeeldcode resulteert in een *toepassingsoverzicht* in Application Insights dat ziet er als volgt:

![Overzicht van de toepassing](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Problemen rapporteren

Om een probleem met Application Insights-integratie in Functions te melden of een suggestie of de aanvraag, [maken van een probleem in GitHub](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitoring-without-application-insights"></a>Bewaking zonder Application Insights

We raden Application Insights voor het controlefuncties omdat het biedt meer gegevens en betere manieren om de gegevens te analyseren. Maar als u liever het ingebouwde logboekregistratie-systeem die gebruikmaakt van Azure Storage, kunt u die gebruiken.

### <a name="logging-to-storage"></a>Logboekregistratie naar opslag

Ingebouwde logboekregistratie maakt gebruik van het opslagaccount dat is opgegeven door de verbindingsreeks in de `AzureWebJobsDashboard` app-instelling. Selecteer een functie in een functie-app-pagina en selecteer vervolgens de **Monitor** tabblad, en kies te houden in de klassieke weergave.

![Overschakelen naar de klassieke weergave](media/functions-monitoring/switch-to-classic-view.png)

 U een lijst met uitvoeringen van functie. Selecteer een functie wordt uitgevoerd om te controleren van de duur, de invoergegevens, de fouten en het bijbehorende logboekbestanden.

Als u Application Insights eerder hebt ingeschakeld, maar u wilt nu gaat u terug naar ingebouwde logboekregistratie uitschakelen Application Insights handmatig en selecteer vervolgens de **Monitor** tabblad. Als u wilt uitschakelen Application Insights-integratie, verwijdert u de instelling van de app APPINSIGHTS_INSTRUMENTATIONKEY.

Zelfs als de **Monitor** tabblad Application Insights-gegevens bevat, kunt u logboekgegevens zien in het bestandssysteem als u nog niet hebt [ingebouwde logboekregistratie uitgeschakeld](#disable-built-in-logging). In de Storage-resource, gaat u naar bestanden, selecteert u de file-service voor de functie en ga vervolgens naar `LogFiles > Application > Functions > Function > your_function` om te zien van het logboekbestand.

### <a name="real-time-monitoring"></a>Realtime-controle

U kunt streamen logboekbestanden op een opdrachtregel-sessie op een lokaal werkstation met de [Azure Command Line Interface (CLI)](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).  

Gebruik de volgende opdrachten op zich aanmelden, kiest u uw abonnement en de stream-logboekbestanden voor de Azure CLI:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Gebruik de volgende opdrachten naar uw Azure-account toevoegen, kiest u uw abonnement en de stream-logboekbestanden voor Azure PowerShell:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <function app name> -Tail
```

Zie voor meer informatie, [over het streamen van logboeken](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

### <a name="viewing-log-files-locally"></a>Logboekbestanden lokaal weergeven

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core-logboekregistratie](/aspnet/core/fundamentals/logging/)
