---
title: Azure Data Lake Analytics beheren met Azure .NET SDK
description: In dit artikel wordt beschreven hoe u de Azure .net SDK gebruikt voor het schrijven van apps die de Data Lake Analytics-taken, gegevensbronnen, en gebruikers beheren.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/18/2017
ms.openlocfilehash: e1e4628a1c777ecc54fdf9a26b334a10b7ab8cb3
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55810576"
---
# <a name="manage-azure-data-lake-analytics-a-net-app"></a>Azure Data Lake Analytics een .NET-app beheren
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dit artikel wordt beschreven hoe u voor het beheren van Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken met behulp van een app die is geschreven met behulp van de Azure .NET SDK. 

## <a name="prerequisites"></a>Vereisten

* **Visual Studio 2015, Visual Studio 2013 update 4 of Visual Studio 2012 met Visual C++ geïnstalleerd**.
* **Microsoft Azure SDK voor .NET versie 2.5 of hoger**.  U kunt dit installeren met het [Webplatforminstallatieprogramma](https://www.microsoft.com/web/downloads/platform.aspx).
* **Vereiste NuGet-pakketten**

### <a name="install-nuget-packages"></a>NuGet-pakketten installeren

|Pakket|Versie|
|-------|-------|
|[Microsoft.Rest.ClientRuntime.Azure.Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| 2.3.1|
|[Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|3.0.0|
|[Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|2.2.0|
|[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|1.6.0-Preview|
|[Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|3.4.0-Preview|

U kunt deze pakketten via de opdrachtregel NuGet installeren met de volgende opdrachten:

```
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a>Algemene variabelen

``` csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a>Authentication

U hebt meerdere opties voor aanmelden bij Azure Data Lake Analytics. Het volgende fragment toont een voorbeeld van verificatie met interactieve gebruikersverificatie met een pop-upvenster.

``` csharp
using System;
using System.IO;
using System.Threading;
using System.Security.Cryptography.X509Certificates;

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Analytics;
using Microsoft.Azure.Management.DataLake.Analytics.Models;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Azure.Graph.RBAC;

public static Program
{
   public static string TENANT = "microsoft.onmicrosoft.com";
   public static string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
   public static System.Uri ARM_TOKEN_AUDIENCE = new System.Uri( @"https://management.core.windows.net/");
   public static System.Uri ADL_TOKEN_AUDIENCE = new System.Uri( @"https://datalake.azure.net/" );
   public static System.Uri GRAPH_TOKEN_AUDIENCE = new System.Uri( @"https://graph.windows.net/" );

   static void Main(string[] args)
   {
      string MY_DOCUMENTS= System.Environment.GetFolderPath( System.Environment.SpecialFolder.MyDocuments);
      string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");

      var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
      var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var graphCreds = GetCreds_User_Popup(TENANT, GRAPH_TOKEN_AUDIENCE, CLIENTID, tokenCache);
   }
}
```

De broncode voor **GetCreds_User_Popup** en de code voor andere opties voor verificatie worden behandeld in de [Data Lake Analytics .NET verificatie-opties](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options)


## <a name="create-the-client-management-objects"></a>De client-management-objecten maken

``` csharp
var resourceManagementClient = new ResourceManagementClient(armCreds) { SubscriptionId = subid };

var adlaAccountClient = new DataLakeAnalyticsAccountManagementClient(armCreds);
adlaAccountClient.SubscriptionId = subid;

var adlsAccountClient = new DataLakeStoreAccountManagementClient(armCreds);
adlsAccountClient.SubscriptionId = subid;

var adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(adlCreds);
var adlaJobClient = new DataLakeAnalyticsJobManagementClient(adlCreds);

var adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

var  graphClient = new GraphRbacManagementClient(graphCreds);
graphClient.TenantID = domain;

```

## <a name="manage-accounts"></a>Accounts beheren

### <a name="create-an-azure-resource-group"></a>Een Azure-resourcegroep maken

Als u dit nog niet hebt gemaakt, moet u een Azure-resourcegroep te maken van de onderdelen van uw Data Lake Analytics hebben. U moet uw referenties voor verificatie, abonnements-ID en een locatie. De volgende code laat zien hoe u een resourcegroep wilt maken:

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```
Zie voor meer informatie, Azure-resourcegroepen en Data Lake Analytics.

### <a name="create-a-data-lake-store-account"></a>Een Data Lake Store-account maken

ADLA-account vereist ooit een ADLS-account. Als u niet wilt gebruiken al hebt, kunt u maken met de volgende code:

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

De volgende code maakt een ADLS-account

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a>Lijst met Data Lake Store-accounts

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a>Lijst met Data Lake Analytics-accounts

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a>Controleren of een account bestaat

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a>Informatie ophalen over een account

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a>Een account verwijderen

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-the-default-data-lake-store-account"></a>De standaard Data Lake Store-account ophalen

Elk Data Lake Analytics-account vereist een Data Lake Store-standaardaccount. Gebruik deze code om te bepalen van de Store-standaardaccount van een Analytics-account.

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a>Gegevensbronnen beheren

Data Lake Analytics ondersteunt momenteel de volgende gegevensbronnen:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage-account](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a>Koppelen aan een Azure Storage-account

U kunt koppelingen naar Azure Storage-accounts maken.

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a>Lijst met Azure Storage-gegevensbronnen

``` csharp
var stg_accounts = adlaAccountClient.StorageAccounts.ListByAccount(rg, adla);

if (stg_accounts != null)
{
  foreach (var stg_account in stg_accounts)
  {
      Console.WriteLine($"Storage account: {0}", stg_account.Name);
  }
}
```

### <a name="list-data-lake-store-data-sources"></a>Lijst met Data Lake Store-gegevensbronnen

``` csharp
var adls_accounts = adlsClient.Account.List();

if (adls_accounts != null)
{
  foreach (var adls_accnt in adls_accounts)
  {
      Console.WriteLine($"ADLS account: {0}", adls_accnt.Name);
  }
}
```

### <a name="upload-and-download-folders-and-files"></a>Uploaden en downloaden van bestanden en mappen
U kunt het Data Lake Store-client management bestandssysteemobject gebruiken om te uploaden en downloaden van afzonderlijke bestanden of mappen van Azure naar uw lokale computer met de volgende methoden:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

De eerste parameter voor deze methoden is de naam van de Data Lake Store-Account, gevolgd door de parameters voor het bronpad en het doelpad.

Het volgende voorbeeld ziet het downloaden van een map in de Data Lake Store.

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a>Maak een bestand in een Data Lake Store-account

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();
      
      memstream.Position = 0;

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a>Controleer of de paden van Azure Storage-account
De volgende code controleert als een Azure Storage-account (storageAccntName) bestaat in een Data Lake Analytics-account (analyticsAccountName) en als een container (containerName) in de Azure Storage-account bestaat.

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a>Catalogus en taken beheren
Het object DataLakeAnalyticsCatalogManagementClient biedt methoden voor het beheren van de SQL-database die is opgegeven voor elk Azure Data Lake Analytics-account. De DataLakeAnalyticsJobManagementClient biedt methoden voor het indienen en beheren van taken worden uitgevoerd op de database met U-SQL-scripts.

### <a name="list-databases-and-schemas"></a>Lijst met databases en schema 's
Tussen de verschillende dingen die u kunt een lijst, zijn de meest voorkomende databases en hun schema. De volgende code verkrijgt van een verzameling van databases, en vervolgens het schema voor elke database worden opgesomd.

``` csharp
var databases = adlaCatalogClient.Catalog.ListDatabases(adla);
foreach (var db in databases)
{
  Console.WriteLine($"Database: {db.Name}");
  Console.WriteLine(" - Schemas:");
  var schemas = adlaCatalogClient.Catalog.ListSchemas(adla, db.Name);
  foreach (var schm in schemas)
  {
      Console.WriteLine($"\t{schm.Name}");
  }
}
```

### <a name="list-table-columns"></a>Lijst met kolommen
De volgende code toont hoe u toegang tot de database met een Data Lake Analytics-catalogus management-client om de kolommen in een bepaalde tabel weer te geven.

``` csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  Console.WriteLine($"\t{utc.Name}");
}
```

### <a name="submit-a-u-sql-job"></a>Een U-SQL-taak verzenden
De volgende code toont hoe u een Data Lake Analytics-taak management-client gebruiken om een taak te verzenden.
``` csharp
string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
string scriptTxt = string.Empty;
using (StreamReader sr = new StreamReader(scriptStrm))
{
    scriptTxt = sr.ReadToEnd();
}

var jobName = "SR_Wikipedia";
var jobId = Guid.NewGuid();
var properties = new USqlJobProperties(scriptTxt);
var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
var jobInfo = adlaJobClient.Job.Create(adla, jobId, parameters);
Console.WriteLine($"Job {jobName} submitted.");
```

### <a name="list-failed-jobs"></a>Lijst met mislukte taken
De volgende code bevat informatie over taken die niet zijn geslaagd.

``` csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a>Lijst met pijplijnen
De volgende code bevat informatie over elke pijplijn van taken die worden verzonden naar het account.

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a>Lijst met herhalingen
De volgende code bevat informatie over elke herhaling van de taken die worden verzonden naar het account.

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a>Algemene scenario's voor graph

### <a name="look-up-user-in-the-aad-directory"></a>Opzoeken van gebruiker in de AAD-directory

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a>De object-id van een gebruiker in de AAD-directory ophalen

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a>Compute-beleid beheren
Het object DataLakeAnalyticsAccountManagementClient biedt methoden voor het beheren van de compute-beleidsregels voor een Data Lake Analytics-account.

### <a name="list-compute-policies"></a>Lijst met compute-beleid
De volgende code haalt een lijst met compute-beleidsregels voor een Data Lake Analytics-account.

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a>Maak een nieuw beleid voor compute
De volgende code maakt een nieuw beleid voor compute voor een Data Lake Analytics-account, het maximumaantal AU's beschikbaar stellen voor de opgegeven gebruiker tot 50 en de prioriteit van de minimale job tot 250 tekens.

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
