---
title: Configureren en beheren van Azure Notebook-projecten
description: Over het beheren van de metagegevens van het project, project-bestanden, van het project-omgeving en installatiestappen via de gebruikersinterface van Azure-notitieblokken en de directe terminal toegang.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 35dd6ff1-a14a-4a2e-b173-6d8467de3e89
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: kraigb
ms.openlocfilehash: 54b211584b170d6e2ee0bcaa6c80bcaed376814f
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904366"
---
# <a name="manage-and-configure-projects"></a>Projecten beheren en configureren

Een project in notitieblokken van Azure is in feite een configuratie van de onderliggende virtuele Linux-machine waarin Jupyter-notebooks uitgevoerd, samen met een map en de beschrijvende metagegevens. Het projectdashboard in notitieblokken van Azure kunt u bestanden beheren en configureren van het project kenmerken:

- De rekenlaag waarop het project wordt uitgevoerd, dit kan de gratis laag of een virtuele machine van Azure.
- Project-metagegevens, waaronder een naam, beschrijving, een id die wordt gebruikt bij het delen van het project, en of het project is openbaar of privé.
- De notebook van het project, gegevens en andere bestanden, die u, zoals een ander bestandssysteem beheert.
- De omgeving van een project, die u via opstartscripts of rechtstreeks via de terminal beheert.
- Logboeken, die u via de terminal openen.

> [!Note]
> De beheer- en configuratie van functies die hier worden beschreven zijn alleen beschikbaar voor de eigenaar van het project die het project in eerste instantie worden gemaakt. U kunt echter het project klonen in uw eigen account, in welk geval u de eigenaar en het project naar wens kunt configureren.

Azure-notitieblokken wordt de onderliggende virtuele machine gestart wanneer u een laptop of een ander bestand uitvoeren. De server automatisch bestanden worden opgeslagen en worden afgesloten na 60 minuten van inactiviteit. U kunt ook de server stoppen op elk gewenst moment met de **afsluiten** opdracht (sneltoets: h).

## <a name="compute-tier"></a>Rekenlaag

De **uitvoeren** vervolgkeuzelijst in het projectdashboard is waar u de compute-laag waarop het project wordt uitgevoerd. Standaard projecten uitvoeren op de **gratis Compute** laag, wat beperkt is tot 4 GB geheugen en 1 GB aan gegevens om misbruik te voorkomen:

![COMPUTE-laag vervolgkeuzelijst in het projectdashboard](media/project-compute-tier-list.png)

Met behulp van een andere virtuele machine die u in een Azure-abonnement hebt ingericht, kunt u deze beperkingen negeren. U moet installeren en uitvoeren van JupyterHub op die virtuele machine. De Data Science Virtual Machine-installatiekopieën (elk besturingssysteem) zijn goede keuze omdat ze JupyterHub standaard bevatten.

Zodra u een naar behoren geconfigureerd Azure-machine hebt, selecteert u de **Direct Compute** optie in de vervolgkeuzelijst die wordt u gevraagd een naam (om weer te geven in de lijst), de IP-adres en poort (meestal 8000, de standaardpoort waarnaar van de virtuele machine JupyterHub luistert), en de referenties van de virtuele machine:

![Prompt voor het verzamelen van informatie over server voor de directe Compute option](media/project-compute-tier-direct.png)

Als de volgende voorwaarden voldaan wordt, wordt ook de vervolgkeuzelijst weergegeven [Data Science Virtual Machine (DSVM)](/azure/machine-learning/data-science-virtual-machine) exemplaren. (Als een van deze voorwaarden niet worden voldaan, u kunt nog steeds verbinding met de DSVM met behulp van de directe Compute option en de waarden hebt verkregen via de Azure portal in te voeren.)

- U bent aangemeld bij Azure-laptops met een account dat gebruikmaakt van Azure Active Directory (AAD), zoals een bedrijfsaccount.
- Uw account is verbonden met een Azure-abonnement.
- Hebt u een of meer virtuele machines in dat aan het abonnement, met ten minste leestoegang hebben, die gebruikmaken van de virtuele Machine voor Datatechnologie voor Linux (Ubuntu)-installatiekopie.)

![Data Science Virtual Machine-exemplaren in de vervolgkeuzelijst in het projectdashboard](media/project-compute-tier-dsvm.png)

Wanneer u een exemplaar van de DSVM selecteert, Azure-notitieblokken wordt u mogelijk gevraagd om de specifieke machine-referenties gebruikt bij het maken van de virtuele machine.

Voor het maken van een nieuw exemplaar van de DSVM, volgt u de instructies op [maken van een Ubuntu Data Science VM](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Gebruik de **Data Science Virtual Machine voor Linux (Ubuntu)** image als u wilt dat de DSVM wordt weergegeven in de vervolgkeuzelijst in notitieblokken van Azure.  Als voor andere redenen die u wilt gebruiken van de installatiekopie van het Windows- of CentOS, kunt u de **Direct Compute** handmatig verbinding maken met de DSVM.

## <a name="edit-project-metadata"></a>Project metagegevens bewerken

Selecteer in het projectdashboard **projectinstellingen**en selecteer vervolgens de **informatie** tabblad, dat de metagegevens van het project bevat, zoals beschreven in de volgende tabel. U kunt de metagegevens van het project op elk gewenst moment wijzigen.

| Instelling | Description |
| --- | --- |
| Projectnaam | Een beschrijvende naam voor uw project die gebruikmaakt van Azure-notitieblokken weer te geven. Bijvoorbeeld: 'Hello World in Python'. |
| Project-id | Een aangepaste id die deel van de URL uitmaken die u gebruiken voor het delen van een project (het formulier is `https://notebooks.azure.com/<user_id>/projects/<project_id>`). Deze ID kunt alleen letters, cijfers en afbreekstreepjes bevatten en is beperkt tot 30 tekens bevatten. Als u niet zeker weet wat u wilt gebruiken, is een algemene overeenkomst met een kleine versie van de naam van uw project waar de spaties zijn ingeschakeld in afbreekstreepjes bevatten, bijvoorbeeld 'Mijn Project Name' in "Mijn-project-name" verandert. |
| Openbare project | Als is ingesteld, kan iedereen met de koppeling voor toegang tot het project. Wanneer u een privé-project maakt, moet u deze optie uitschakelen. |
| Klonen verbergen | Als is ingesteld, andere gebruikers kunnen een lijst met klonen die zijn gemaakt voor dit project niet zien. Klonen verbergen is handig voor projecten die worden gedeeld met veel mensen die geen deel uitmaken van dezelfde organisatie, zoals bij het gebruik van een notitieblok voor het geven van een klasse. |

> [!Important]
>
> Wijzigen van de project-ID ongeldig maken een koppeling maakt naar het project dat u hebt mogelijk eerder hebt gedeeld.

## <a name="manage-project-files"></a>Project-bestanden beheren

Het projectdashboard ziet u de inhoud van het project tot mappensysteem. U kunt verschillende opdrachten gebruiken voor het beheren van deze bestanden.

### <a name="create-new-files-and-folders"></a>Nieuwe bestanden en mappen maken

De **+ nieuw** opdracht (sneltoets: n) maakt u nieuwe bestanden of mappen. Wanneer u de opdracht, selecteert u eerst het type item te maken:

| Itemtype | Description | Opdrachtgedrag |
| --- | --- | --- |
| **Notebook** | Een Jupyter-notebook | Geeft een pop-upvenster waarin u de bestandsnaam en de taal van het notitieblok opgeeft. |
| **Map** | Een submap | Hiermee maakt u een bewerkingsveld met het in de lijst met bestanden van het project waarin u de naam van de map. |
| **Leeg bestand** | Een bestand waarin u alle inhoud, zoals tekst, gegevens, enzovoort kunt opslaan. | Maakt een bewerkingsveld in de lijst met bestanden van het project waarin u de bestandsnaam invoeren. |
| **Markdown** | Een Markdown-bestand. | Maakt een bewerkingsveld in de lijst met bestanden van het project waarin u de bestandsnaam invoeren. |

### <a name="upload-files"></a>Bestanden uploaden

De **uploaden** opdracht biedt twee opties voor het importeren van gegevens vanuit andere locaties: **Via URL** en **van Computer**. Zie voor meer informatie, [werken met gegevensbestanden in Azure Notebook projecten](work-with-project-data-files.md).

### <a name="select-file-specific-commands"></a>Selecteer bestand-specifieke opdrachten

Elk item in de lijst met bestanden van het project bevat opdrachten via een snelmenu met de rechtermuisknop op:

![Opdrachten in het contextmenu van een bestand](media/project-file-commands.png)

| Opdracht | Sneltoets | Bewerking |
| --- | --- | --- |
| Voer | r (of klik op) | Voert een laptop-bestand. Andere bestandstypen zijn geopend voor weergave.  |
| Koppeling kopiëren | Y | Een koppeling naar het bestand naar het Klembord gekopieerd. |
| Uitvoeren in Jupyter-Lab | "J" | Voert een notitieblok in Jjupyterlab, dit is een meer op ontwikkelaars gerichte interface dan Jupyter normaal biedt. |
| Preview | p | Hiermee opent u een HTML-Preview-versie van het bestand. voor laptops is de Preview-versie een alleen-lezen weergave van de notebook. Zie voor meer informatie de [Preview](#preview) sectie. |
| Bestand bewerken | Ik | Hiermee opent het bestand voor het bewerken van. |
| Downloaden | d | Downloadt een zip-bestand dat het bestand of de inhoud van een map bevat. |
| Naam wijzigen | a | Wordt u gevraagd om een nieuwe naam voor het bestand of map. |
| Verwijderen | x | Wordt om bevestiging wordt gevraagd, en vervolgens wordt het bestand permanent verwijderd uit het project. Verwijderingen kunnen niet ongedaan worden gemaakt. |
| Verplaatsen | m | Een bestand is verplaatst naar een andere map in hetzelfde project. |

#### <a name="preview"></a>Preview

Een voorbeeld van een bestand of een laptop is een alleen-lezen weergave van de inhoud. uitvoeren van de notebook cellen is uitgeschakeld. Een Preview-versie wordt voor iedereen die een koppeling naar het bestand of de notebook heeft, maar is niet aangemeld bij Azure-notitieblokken weergegeven. Nadat u bent aangemeld, een gebruiker de notebook bij hun account kunt klonen of ze kunnen de notebook downloaden naar de lokale computer.

De voorbeeldpagina ondersteunt verschillende opdrachten in de werkbalk met sneltoetsen:

| Opdracht | Sneltoets | Bewerking |
| --- | --- | --- |
| Delen | s | Geeft het delen pop-upvenster van waaruit u kunt een koppeling verkrijgen, delen op sociale media, HTML-code verkrijgen voor het insluiten van en een e-mail verzenden. |
| Klonen | c  | Kloon de notebook voor uw account. |
| Voer | R | De notebook uitgevoerd als u mag doen. |
| Downloaden | d | Een kopie van de notebook downloadt. |

## <a name="configure-the-project-environment"></a>De project-omgeving configureren

Er zijn drie manieren om te configureren van de omgeving van de onderliggende virtuele machine waarin uw laptops worden uitgevoerd:

- Een eenmalige Initialisatiescript voor lidddatabase van opnemen
- Instellingen van de omgeving van het project gebruiken om op te geven van de installatiestappen uit
- Toegang tot de virtuele machine via een terminal.

Alle vormen van projectconfiguratie worden toegepast wanneer de virtuele machine wordt gestart, en dus is van invloed op alle laptops binnen het project.

### <a name="one-time-initialization-script"></a>Eenmalige Initialisatiescript voor lidddatabase van

De eerste keer Azure Notebooks een server voor het project wordt gemaakt, wordt er gezocht naar een bestand in het project met de naam *aznbsetup.sh*. Als dit bestand aanwezig is, wordt het door de Azure-notitieblokken uitgevoerd. De uitvoer van het script wordt opgeslagen in de projectmap als *. aznbsetup.log*.

### <a name="environment-setup-steps"></a>Instellingsstappen omgeving

Instellingen van de omgeving van het project kunt u afzonderlijke stappen die u configureert u de omgeving maken.

Selecteer in het projectdashboard **projectinstellingen**en selecteer vervolgens de **omgeving** tabblad waarin toevoegen, verwijderen en setup stappen voor het project wijzigen:

![Project instellingen pop-upvenster met tabblad omgeving geselecteerd](media/project-settings-environment-steps.png)

Als u wilt een stap toevoegen, selecteert u eerst **+ toevoegen**, selecteert u een staptype in de **bewerking** vervolgkeuzelijst:

![Bewerking de verkeersselector voor op een nieuwe omgeving setup stap](media/project-settings-environment-details.png)

De informatie die u vervolgens projecteren, is afhankelijk van het type bewerking dat u hebt gekozen:

- **Requirements.txt**: Selecteer in de tweede vervolgkeuzelijst met een *requirements.txt* bestand dat zich al in het project. Selecteer vervolgens een Python-versie van de derde vervolgkeuzelijst die wordt weergegeven. Met behulp van een *requirements.txt* -bestand, Azure-notitieblokken uitvoeringen `pip install -r` met de *requirements.txt* bestand bij het starten van een notebook-server. U hoeft niet te pakketten uit binnen de notebook zelf expliciet te installeren.

- **Shell-script**: Selecteer in de tweede vervolgkeuzelijst met een bash-shell-script in het project (meestal een bestand met de *.sh* extensie) die alle opdrachten die u uitvoeren wilt voor het initialiseren van de omgeving bevat.

- **Environment.yml**: Selecteer in de tweede vervolgkeuzelijst met een *environments.yml* -bestand voor Python-projecten met behulp van een conda-omgeving.

Wanneer u de stappen toegevoegd hebt, selecteert u **opslaan**.

### <a name="use-the-terminal"></a>Gebruik de terminal

In het projectdashboard de **Terminal** opdracht opent u een Linux-terminal die u direct toegang tot de server biedt. In de terminal kunt u downloaden van gegevens, bewerken of beheren van bestanden, processen controleren en zelfs gebruik hulpprogramma's zoals vi en nano.

> [!Note]
> Hebt u opstartscripts in de omgeving van uw project, is het mogelijk dat weergegeven een bericht weergegeven dat aangeeft dat de installatie nog steeds bezig de terminal te openen.

U kunt een standaard Linux-opdrachten in de terminal op te geven. U kunt ook `ls` in de basismap om te zien van de verschillende omgevingen die bestaan op de virtuele machine, zoals *anaconda2_501*, *anaconda3_420*, *anaconda3_501*, *IfSharp*, en *R*, samen met een *project* map waarin het project:

![Project-terminal in Azure-notitieblokken](media/project-terminal.png)

Als u wilt invloed hebben op een specifieke omgeving, wijzig de mappen eerst naar die map omgeving.

Voor de Python-omgevingen, kunt u vinden `pip` en `conda` in *bin* map van elke omgeving. U kunt ook ingebouwde aliassen gebruiken voor de omgevingen:

```bash
# Anaconda 2 5.3.0/Python 2.7: python27
python27 -m pip install <package>

# Anaconda 3 4.2.0/Python 3.5: python35
python35 -m pip install <package>

# Anaconda 3 5.3.0/Python 3.6: python36
python36 -m pip install <package>
```

Wijzigingen in de server gelden alleen voor de huidige sessie, met uitzondering van bestanden en mappen die u maakt in de *project* map zelf. Bijvoorbeeld, het bewerken van een bestand in de projectmap gehandhaafd blijft tussen sessies, maar pakketten met `pip install` niet.

> [!Note]
> Als u `python` of `python3`, aanroepen van het system-installatie geïnstalleerde versies van Python, die niet worden gebruikt voor notebooks. U hebt geen machtigingen voor bewerkingen, zoals `pip install` , dus zorg ervoor dat de versie-specifieke-aliassen gebruiken.

## <a name="access-notebook-logs"></a>Logboeken van de notebook openen

Als u problemen ondervindt bij het uitvoeren van een laptop, de uitvoer van Jupyter wordt opgeslagen in een map met de naam *. nb.log*. U hebt toegang tot deze logboeken via de **Terminal** opdracht of het projectdashboard.

Doorgaans wanneer u Jupyter lokaal uitvoert u mogelijk hebt gestart vanuit een terminal-venster. In het terminalvenster ziet u uitvoer zoals de status van de kernel.

Als u zich aanmeldt, gebruik de volgende opdracht in de terminal:

```bash
cat .nb.log
```

U kunt ook de opdracht gebruiken vanaf een lege codecel in een Python-notebook:

```bash
!cat .nb.log
```

## <a name="next-steps"></a>Volgende stappen

- [Procedure: Werken met gegevens projectbestanden](work-with-project-data-files.md)
- [Toegang tot cloud-gegevens in een notitieblok](access-data-resources-jupyter-notebooks.md)
