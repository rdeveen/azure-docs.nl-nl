---
title: Python gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Python gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/28/2019
ms.openlocfilehash: b611eb02203c872e3497b5b7c12acddd9eab14c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188385"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Snelstartgids: Python gebruiken om een query uit te voeren voor een Azure SQL-database

 In deze Quick Start wordt gedemonstreerd hoe u [Python](https://python.org) gebruikt om verbinding te maken met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om een query uit te voeren voor de gegevens. Bekijk onze [verwijzings](https://docs.microsoft.com/python/api/overview/azure/sql)-documentatie, de [pyodbc GitHub-opslagplaats](https://github.com/mkleehammer/pyodbc/wiki/) en een [pyodbc-voorbeeld](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) voor meer informatie voor SDK.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Python en verwante software voor uw besturingssysteem:
  
  - **MacOS**: Installeer Homebrew en Python, installeer het ODBC-stuurprogramma en SQLCMD, en installeer vervolgens het Python-stuurprogramma voor SQL Server. Zie stappen 1.2, 1.3 en 2.1 in [Python-apps maken met behulp van SQL Server in macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/). Zie [Het Microsoft ODBC-stuurprogramma installeren in Linux en macOS](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server) voor meer informatie.
    
  - **Ubuntu**: Installeer Python en andere vereiste pakketten met `sudo apt-get install python python-pip gcc g++ build-essential`. Download en installeer het ODBC-stuurprogramma, SQLCMD en het Python-stuurprogramma voor SQL Server. Zie voor instructies [Een ontwikkelomgeving configureren voor pyodbc Python-ontwikkeling](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).
    
  - **Windows**: Installeer Python, het ODBC-stuurprogramma en SQLCMD, en het Python-stuurprogramma voor SQL Server. Zie [Een ontwikkelomgeving configureren voor pyodbc Python-ontwikkeling](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows) voor instructies.

## <a name="get-sql-server-connection-information"></a>Verbindingsgegevens voor SQL Server ophalen

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="create-code-to-query-your-sql-database"></a>Code maken om query's uit te voeren op uw SQL-database 

1. Maak in een teksteditor een nieuw bestand met de naam *sqltest.py*.  
   
1. Voeg de volgende code toe. Gebruik uw eigen waarden voor \<server >, \<database >, \<gebruikersnaam > en \<wachtwoord >.
   
   >[!IMPORTANT]
   >Voor de code in dit voorbeeld worden de voorbeeldgegevens gebruikt van AdventureWorksLT, die u als bron kunt kiezen bij het maken van uw database. Als in uw database andere gegevens staan, kunt u tabellen uit uw eigen database gebruiken in de SELECT-query. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>De code uitvoeren

1. Voer de volgende opdracht uit op een opdrachtprompt:

   ```cmd
   python sqltest.py
   ```

1. Controleer of de bovenste twintig rijen Categorie/Product worden geretourneerd en sluit vervolgens het opdrachtvenster.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft Python-stuurprogramma's voor SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python-ontwikkelaarscentrum](https://azure.microsoft.com/develop/python/?v=17.23h)

