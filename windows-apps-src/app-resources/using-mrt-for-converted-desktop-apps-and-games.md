---
title: Utilisation du MRT pour les applications de bureau et les jeux convertis
description: En incluant votre application ou jeu .NET ou Win32 dans un package .msix ou .appx, vous pouvez exploiter le système de gestion des ressources pour charger des ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, MRT, PRI. ressources, jeux, Centennial, convertisseur d’applications de bureau, MUI, assembly satellite
ms.localizationpriority: medium
ms.openlocfilehash: b86cbcfcc5a6c6284b993dcad1325b108b1ab353
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636489"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Utiliser le système de gestion des ressources Windows 10 dans une application ou un jeu hérité

Les applications et les jeux .NET et Win32 sont souvent localisés dans différentes langues pour étendre leur marché complet adressable. Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../design/globalizing/globalizing-portal.md). En incluant votre application ou jeu .NET ou Win32 dans un package .msix ou .appx, vous pouvez exploiter le système de gestion des ressources pour charger des ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques.

Il existe de nombreuses façons de localiser une application Win32 traditionnelle, mais Windows 8 a introduit un [nouveau système de gestion des ressources](/previous-versions/windows/apps/jj552947(v=win.10)) qui fonctionne dans des langages de programmation, entre différents types d’applications, et fournit des fonctionnalités au-delà de la localisation simple. Ce système sera appelé « MRT » dans cette rubrique. Historiquement, qui conpensée pour « technologie de ressources moderne », mais le terme « moderne » a été abandonné. Le gestionnaire de ressources peut également être appelé MRM (moderne Gestionnaire des ressources) ou PRI (index de ressources de package).

Combiné au déploiement basé sur MSIX ou. AppX (par exemple, à partir de l’Microsoft Store), MRT peut automatiquement fournir les ressources les plus applicables pour un utilisateur/appareil donné, ce qui réduit le téléchargement et l’installation de la taille de votre application. Cette réduction de taille peut être importante pour les applications avec une grande quantité de contenu localisé, peut-être dans l’ordre de plusieurs *gigaoctets* pour les jeux AAA. Les avantages supplémentaires du MRT incluent les listes localisées dans le shell Windows et la logique de secours automatique Microsoft Store lorsque la langue par défaut d’un utilisateur ne correspond pas à vos ressources disponibles.

Ce document décrit l’architecture de haut niveau du fichier MRT et fournit un guide de Portage pour vous aider à déplacer des applications Win32 héritées vers MRT avec des modifications de code minimes. Une fois le déploiement de MRT effectué, les avantages supplémentaires (tels que la possibilité de segmenter les ressources par facteur d’échelle ou thème de système) sont mis à la disposition du développeur. Notez que la localisation basée sur MRT fonctionne pour les applications UWP et Win32 traitées par le pont de bureau (également appelé « Centennial »).

Dans de nombreux cas, vous pouvez continuer à utiliser vos formats de localisation et votre code source existants tout en intégrant avec MRT pour la résolution des ressources au moment de l’exécution et en minimisant la taille des téléchargements. il ne s’agit pas d’une approche « tout ou rien ». Le tableau suivant récapitule le travail et le coût estimé/avantage de chaque phase. Ce tableau n’inclut pas les tâches qui ne sont pas liées à la localisation, telles que l’application d’icônes haute résolution ou à contraste élevé. Pour plus d’informations sur la façon de fournir plusieurs ressources pour les vignettes, les icônes, etc., consultez [adapter vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Travail</th>
<th>Avantage</th>
<th>Coût estimé</th>
</tr>
<tr>
<td>Localiser le manifeste du package</td>
<td>Le minimum de travail requis pour que votre contenu localisé apparaisse dans le shell Windows et dans le Microsoft Store</td>
<td>Petite</td>
</tr>
<tr>
<td>Utiliser le MRT pour identifier et localiser des ressources</td>
<td>Conditions préalables à la réduction des tailles de téléchargement et d’installation ; langue de secours automatique</td>
<td>Medium</td>
</tr>
<tr>
<td>Créer des packs de ressources</td>
<td>Dernière étape pour réduire les tailles de téléchargement et d’installation</td>
<td>Petite</td>
</tr>
<tr>
<td>Migrer vers des API et des formats de ressources MRT</td>
<td>Tailles de fichier beaucoup plus petites (en fonction de la technologie des ressources existante)</td>
<td>Grande</td>
</tr>
</table>

## <a name="introduction"></a>Introduction

La plupart des applications non triviales contiennent des éléments d’interface utilisateur appelés *ressources* qui sont dissociés du code de l’application (avec des *valeurs codées en dur* créées dans le code source lui-même). Il existe plusieurs raisons de préférer des ressources à des valeurs codées en dur (facilité de modification par des non-développeurs, par exemple), mais l’une des principales raisons est de permettre à l’application de choisir différentes représentations de la même ressource logique au moment de l’exécution. Par exemple, le texte à afficher sur un bouton (ou l’image à afficher dans une icône) peut différer selon la ou les langues que l’utilisateur comprend, les caractéristiques du périphérique d’affichage ou si des technologies d’assistance sont activées sur l’utilisateur.

Ainsi, l’objectif principal de toute technologie de gestion des ressources est de traduire, lors de l’exécution, une demande de *nom de ressource* logique ou symbolique (telle que `SAVE_BUTTON_LABEL` ) en la meilleure *valeur* réelle possible (par exemple, « enregistrer ») à partir d’un ensemble de *candidats* possibles (par exemple, « enregistrer », « Speichern » ou « 저장 »). Le MRT fournit une fonction de ce type et permet aux applications d’identifier les candidats aux ressources à l’aide d’un large éventail d’attributs, appelés *qualificateurs*, tels que la langue de l’utilisateur, le facteur d’échelle d’affichage, le thème sélectionné par l’utilisateur et d’autres facteurs environnementaux. Le programme MRT prend même en charge les qualificateurs personnalisés pour les applications qui en ont besoin (par exemple, une application peut fournir des ressources graphiques différentes aux utilisateurs ayant ouvert une session avec un compte ou des utilisateurs invités, sans ajouter explicitement ce contrôle dans chaque partie de leur application). MRT fonctionne avec les ressources de type chaîne et les ressources basées sur des fichiers, où les ressources basées sur les fichiers sont implémentées en tant que références aux données externes (les fichiers eux-mêmes).

### <a name="example"></a> Exemple

Voici un exemple simple d’une application avec des étiquettes de texte sur deux boutons ( `openButton` et `saveButton` ) et un fichier PNG utilisé pour un logo ( `logoImage` ). Les étiquettes de texte sont localisées en anglais et en allemand, et le logo est optimisé pour les affichages de bureau normaux (facteur d’échelle de 100%) et les téléphones haute résolution (facteur d’échelle de 300%). Notez que ce diagramme présente une vue conceptuelle de haut niveau du modèle. elle ne correspond pas exactement à l’implémentation.

:::image type="content" source="images\conceptual-resource-model.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque.":::

Dans le graphique, le code de l’application fait référence aux trois noms de ressource logique. Lors de l’exécution, la `GetResource` Pseudo-fonction utilise MRT pour rechercher les noms de ressources dans la table de ressources (appelée fichier PRI) et trouver le candidat le plus approprié en fonction des conditions ambiantes (la langue de l’utilisateur et le facteur d’échelle de l’affichage). Dans le cas des étiquettes, les chaînes sont utilisées directement. Dans le cas de l’image du logo, les chaînes sont interprétées comme des noms de fichiers et les fichiers sont lus sur le disque. 

Si l’utilisateur parle une langue autre que l’anglais ou l’allemand, ou a un facteur d’échelle d’affichage autre que 100% ou 300%, MRT choisit le candidat correspondant « le plus proche » en fonction d’un ensemble de règles de secours (voir [système de gestion des ressources](/previous-versions/windows/apps/jj552947(v=win.10)) pour plus d’informations sur l’arrière-plan).

Notez que le MRT prend en charge des ressources adaptées à plusieurs qualificateurs. par exemple, si l’image du logo contenait du texte incorporé qui devait également être localisé, le logo aurait quatre candidats : fr/Scale-100, dé/Scale-100, in/Scale-300 et DE/Scale-300.

### <a name="sections-in-this-document"></a>Sections de ce document

Les sections suivantes décrivent les tâches de haut niveau requises pour intégrer MRT à votre application.

#### <a name="phase-0-build-an-application-package"></a>Phase 0 : créer un package d’application

Cette section explique comment faire en sorte que votre application de bureau existante crée un package d’application. Aucune fonctionnalité MRT n’est utilisée à ce niveau.

#### <a name="phase-1-localize-the-application-manifest"></a>Phase 1 : localisation du manifeste d’application

Cette section explique comment localiser le manifeste de votre application (afin qu’il apparaisse correctement dans le shell Windows) tout en utilisant le format de ressource et l’API hérités pour empaqueter et localiser des ressources. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2 : utiliser le MRT pour identifier et localiser des ressources

Cette section explique comment modifier le code de votre application (et éventuellement la disposition des ressources) pour localiser des ressources à l’aide de MRT, tout en utilisant vos API et formats de ressources existants pour charger et consommer les ressources. 

#### <a name="phase-3-build-resource-packs"></a>Phase 3 : créer des packs de ressources

Cette section décrit les modifications finales nécessaires à la séparation de vos ressources dans des *packs de ressources*distincts, réduisant ainsi la taille du téléchargement (et de l’installation) de votre application.

### <a name="not-covered-in-this-document"></a>Non abordé dans ce document

Une fois les phases 0-3 ci-dessus terminées, vous disposerez d’une application « bundle » qui peut être soumise à la Microsoft Store et qui réduira la taille de téléchargement et d’installation des utilisateurs en omettant les ressources dont ils n’ont pas besoin (par exemple, les langues qu’ils ne parlent pas). D’autres améliorations de la taille et de la fonctionnalité de l’application peuvent être effectuées en effectuant une dernière étape.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Phase 4 : migrer vers des API et des formats de ressource MRT

Cette phase n’entre pas dans le cadre de ce document ; Il implique le déplacement de vos ressources (en particulier des chaînes) à partir de formats hérités tels que des dll MUI ou des assemblys de ressources .NET dans des fichiers PRI. Cela peut entraîner des économies d’espace supplémentaires pour le téléchargement & les tailles d’installation. Il permet également l’utilisation d’autres fonctionnalités MRT, telles que la réduction du téléchargement et de l’installation des fichiers image en fonction du facteur d’échelle, des paramètres d’accessibilité, etc.

## <a name="phase-0-build-an-application-package"></a>Phase 0 : créer un package d’application

Avant d’apporter des modifications aux ressources de votre application, vous devez d’abord remplacer vos packages et technologies d’installation actuels par la technologie standard de déploiement et d’empaquetage UWP. Pour ce faire, deux possibilités s'offrent à vous :

* Si vous disposez d’une application de bureau de grande taille avec un programme d’installation complexe ou que vous utilisez un grand nombre de points d’extensibilité de système d’exploitation, vous pouvez utiliser l’outil Desktop App Converter pour générer les informations de mise en page et de manifeste du fichier UWP à partir de votre programme d’installation d’application existant (par exemple, un MSI).
* Si vous disposez d’une application de bureau plus petite avec relativement peu de fichiers ou un simple programme d’installation et aucun hook d’extensibilité, vous pouvez créer manuellement la disposition des fichiers et les informations du manifeste.
* Si vous procédez à une reconstruction à partir de la source et que vous souhaitez mettre à jour votre application pour qu’elle soit une application UWP pure, vous pouvez créer un projet dans Visual Studio et vous fier à l’IDE pour effectuer une grande partie du travail.

Si vous souhaitez utiliser le convertisseur d’application de [Bureau](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw), consultez [empaqueter une application de bureau à l’aide du convertisseur](/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) d’application de bureau pour plus d’informations sur le processus de conversion. Vous trouverez un ensemble complet d’exemples de convertisseurs Desktop sur [le pont de bureau pour les exemples UWP GitHub référentiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Si vous souhaitez créer manuellement le package, vous devez créer une structure de répertoires qui comprend tous les fichiers de votre application (fichiers exécutables et contenu, mais pas le code source) et un fichier manifeste de package (. appxmanifest). Vous trouverez un exemple dans [l’exemple Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), mais un fichier manifeste de package de base qui exécute l’exécutable de bureau nommé `ContosoDemo.exe` est le suivant, où le <span style="background-color: yellow">texte mis en surbrillance</span> serait remplacé par vos propres valeurs.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

Pour plus d’informations sur le fichier manifeste du package et la disposition des packages, consultez [manifeste du package d’application](/uwp/schemas/appxpackage/appx-package-manifest).

Enfin, si vous utilisez Visual Studio pour créer un nouveau projet et migrer votre code existant dans, consultez [créer une application « Hello, World »](../get-started/create-a-hello-world-app-xaml-universal.md). Vous pouvez inclure votre code existant dans le nouveau projet, mais vous devrez probablement apporter des modifications de code significatives (en particulier dans l’interface utilisateur) afin de l’exécuter en tant qu’application UWP pure. Ces modifications n’entrent pas dans le cadre de ce document.

## <a name="phase-1-localize-the-manifest"></a>Phase 1 : localisation du manifeste

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Étape 1,1 : mettre à jour les chaînes & les ressources dans le manifeste

En phase 0, vous avez créé un fichier manifeste du package de base (. appxmanifest) pour votre application (en fonction des valeurs fournies au convertisseur, extraites de l’identité MSI ou entrées manuellement dans le manifeste), mais il ne contient pas d’informations localisées et ne prend pas en charge des fonctionnalités supplémentaires telles que les ressources de la mosaïque de démarrage haute résolution, etc.

Pour vous assurer que le nom et la description de votre application sont correctement localisés, vous devez définir certaines ressources dans un ensemble de fichiers de ressources et mettre à jour le manifeste du package pour y faire référence.

#### <a name="creating-a-default-resource-file"></a>Création d’un fichier de ressources par défaut

La première étape consiste à créer un fichier de ressources par défaut dans votre langue par défaut (par exemple, l’anglais des États-Unis). Vous pouvez effectuer cette opération manuellement à l’aide d’un éditeur de texte ou à l’aide du concepteur de ressources dans Visual Studio.

Si vous souhaitez créer les ressources manuellement :

1. Créez un fichier XML nommé `resources.resw` et placez-le dans un `Strings\en-us` sous-dossier de votre projet. Utilisez le code BCP-47 approprié si votre langue par défaut n’est pas l’anglais (États-Unis).
2. Dans le fichier XML, ajoutez le contenu suivant, où le <span style="background-color: yellow">texte mis en surbrillance</span> est remplacé par le texte approprié pour votre application, dans la langue par défaut.

> [!NOTE]
> Il existe des restrictions sur les longueurs de certaines de ces chaînes. Pour plus d’informations, consultez [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Si vous souhaitez utiliser le concepteur dans Visual Studio :

1. Créez le `Strings\en-us` dossier (ou un autre langage approprié) dans votre projet et ajoutez un **nouvel élément** au dossier racine de votre projet, en utilisant le nom par défaut `resources.resw` . Veillez à choisir le **fichier de ressources (. resw)** et non le **dictionnaire de ressources** . un dictionnaire de ressources est un fichier utilisé par les applications XAML.
2. À l’aide du concepteur, entrez les chaînes suivantes (utilisez le même, `Names` mais remplacez le `Values` par le texte approprié pour votre application) :

:::image type="content" source="images\editing-resources-resw.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque." :::

> [!NOTE]
> Si vous démarrez avec le concepteur Visual Studio, vous pouvez toujours modifier directement le code XML en appuyant sur `F7` . Toutefois, si vous démarrez avec un fichier XML minimal, *le concepteur ne reconnaît pas le fichier* , car il manque beaucoup de métadonnées supplémentaires ; vous pouvez résoudre ce problème en copiant les informations XSD réutilisables à partir d’un fichier généré par le concepteur dans votre fichier XML modifié manuellement.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Mettre à jour le manifeste pour référencer les ressources

Une fois que les valeurs sont définies dans le `.resw` fichier, l’étape suivante consiste à mettre à jour le manifeste pour référencer les chaînes de ressources. Là encore, vous pouvez modifier un fichier XML directement ou vous fier au concepteur de manifeste de Visual Studio.

Si vous modifiez directement le code XML, ouvrez le `AppxManifest.xml` fichier et apportez les modifications suivantes aux <span style="background-color: lightgreen">valeurs mises en surbrillance</span> : utilisez ce texte *exact* , et non le texte propre à votre application. Il n’est pas nécessaire d’utiliser ces noms de ressources exacts &mdash; &mdash; , mais ce que vous choisissez doit correspondre exactement à ce qui se trouve dans le `.resw` fichier. Ces noms doivent correspondre à ceux `Names` que vous avez créés dans le `.resw` fichier, avec pour préfixe le `ms-resource:` schéma et l' `Resources/` espace de noms. 

> [!NOTE]
> De nombreux éléments du manifeste ont été omis de cet extrait de code. ne supprimez rien.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Si vous utilisez le concepteur de manifeste Visual Studio, ouvrez le fichier. appxmanifest et modifiez les valeurs des <span style="background-color: lightgreen">valeurs en surbrillance</span> dans l’onglet **application* et l’onglet *Packaging* :

:::image type="content" source="images\editing-application-info.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque." :::

:::image type="content" source="images\editing-packaging-info.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque." :::

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Étape 1,2 : générer un fichier PRI, créer un package MSIX et vérifier qu’il fonctionne

Vous devez maintenant être en mesure de générer le `.pri` fichier et de déployer l’application pour vérifier que les informations correctes (dans votre langue par défaut) s’affichent dans le menu Démarrer.

Si vous générez dans Visual Studio, appuyez simplement sur `Ctrl+Shift+B` pour générer le projet, puis cliquez avec le bouton droit sur le projet et choisissez `Deploy` dans le menu contextuel.

Si vous effectuez la génération manuellement, procédez comme suit pour créer un fichier de configuration pour `MakePRI` l’outil et pour générer le `.pri` fichier lui-même (vous trouverez plus d’informations dans la section [packages d’applications manuelles](/windows/msix/package/manual-packaging-root)) :

1. Ouvrez une invite de commandes développeur à partir du dossier **Visual studio 2017** ou **Visual Studio 2019** dans le menu Démarrer.
2. Basculez vers le répertoire racine du projet (celui qui contient le fichier. appxmanifest et le dossier **Strings** ).
3. Tapez la commande suivante, en remplaçant « contoso_demo.xml » par un nom adapté à votre projet, et « en-US » par la langue par défaut de votre application (ou conservez-le, le cas échéant). Notez que le fichier XML est créé dans le répertoire parent (et**non** dans le répertoire du projet), car il ne fait pas partie de l’application (vous pouvez choisir n’importe quel autre répertoire, mais veillez à le remplacer dans les commandes ultérieures).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Vous pouvez taper `makepri createconfig /?` pour voir ce que fait chaque paramètre, mais en Résumé :
      * `/cf` définit le nom de fichier de configuration (la sortie de cette commande)
      * `/dq` définit les qualificateurs par défaut, dans ce cas le langage `en-US`
      * `/pv` définit la version de la plateforme, dans ce cas Windows 10
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

4. Maintenant, vous disposez d’un fichier de configuration, exécutez `MakePRI` à nouveau pour rechercher des ressources sur le disque et les empaqueter dans un fichier PRI. Remplacez « contoso_demop.xml » par le nom de fichier XML que vous avez utilisé à l’étape précédente et veillez à spécifier le répertoire parent à la fois pour l’entrée et la sortie : 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Vous pouvez taper `makepri new /?` pour voir ce que fait chaque paramètre, mais en bref :
      * `/pr` définit la racine du projet (dans ce cas, le répertoire actif)
      * `/cf` définit le nom du fichier de configuration, créé à l’étape précédente.
      * `/of` définit le fichier de sortie 
      * `/mf` crée un fichier de mappage (afin que nous puissions exclure des fichiers du package dans une étape ultérieure)
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

5. Vous disposez maintenant d’un `.pri` fichier avec les ressources de langue par défaut (par exemple, en-US). Pour vérifier qu’elle fonctionnait correctement, vous pouvez exécuter la commande suivante :

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Vous pouvez taper `makepri dump /?` pour voir ce que fait chaque paramètre, mais en bref :
      * `/if` définit le nom du fichier d’entrée 
      * `/of` définit le nom du fichier de sortie ( `.xml` est ajouté automatiquement)
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

6. Enfin, vous pouvez ouvrir `..\resources.xml` un éditeur de texte et vérifier qu’il répertorie vos `<NamedResource>` valeurs (comme `ApplicationDescription` et `PublisherDisplayName` ) ainsi que les `<Candidate>` valeurs de la langue par défaut que vous avez choisie (il y aura un autre contenu au début du fichier ; ignorez-le pour le moment).

Vous pouvez ouvrir le fichier `..\resources.map.txt` de mappage pour vérifier qu’il contient les fichiers nécessaires à votre projet (y compris le fichier PRI, qui ne fait pas partie du répertoire du projet). Important, le fichier de mappage n’inclura *pas* de référence à votre `resources.resw` fichier, car le contenu de ce fichier a déjà été incorporé dans le fichier PRI. Toutefois, elle contient d’autres ressources, telles que les noms de fichiers de vos images.

#### <a name="building-and-signing-the-package"></a>Génération et signature du package 

Maintenant que le fichier PRI est créé, vous pouvez générer et signer le package :

1. Pour créer le package d’application, exécutez la commande suivante en remplaçant `contoso_demo.appx` par le nom du fichier. msix/. AppX que vous souhaitez créer et en veillant à choisir un autre répertoire pour le fichier (cet exemple utilise le répertoire parent ; il peut se trouver n’importe où, mais **ne doit pas** être le répertoire du projet).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Vous pouvez taper `makeappx pack /?` pour voir ce que fait chaque paramètre, mais en bref :
      * `/m` définit le fichier manifeste à utiliser
      * `/f` définit le fichier de mappage à utiliser (créé à l’étape précédente) 
      * `/p` définit le nom du package de sortie
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

2. Une fois le package créé, il doit être signé. Le moyen le plus simple d’obtenir un certificat de signature consiste à créer un projet Windows universel vide dans Visual Studio et à copier le `.pfx` fichier qu’il crée, mais vous pouvez en créer un manuellement à l’aide des `MakeCert` utilitaires et, `Pvk2Pfx` comme décrit dans [comment créer un certificat de signature de package d’application](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > Si vous créez manuellement un certificat de signature, assurez-vous que vous placez les fichiers dans un répertoire différent de celui de votre projet source ou de votre source de package. sinon, il peut être inclus dans le package, y compris la clé privée !

3. Pour signer le package, utilisez la commande suivante. Notez que le `Publisher` spécifié dans l' `Identity` élément de `AppxManifest.xml` doit correspondre au `Subject` du certificat (il ne s’agit **pas** de l' `<PublisherDisplayName>` élément, qui est le nom complet localisé à afficher aux utilisateurs). Comme d’habitude, remplacez les `contoso_demo...` noms de fichiers par les noms appropriés pour votre projet et (**très important**) Assurez-vous que le `.pfx` fichier ne se trouve pas dans le répertoire actif (sinon, il aurait été créé dans le cadre de votre package, y compris la clé de signature privée !) :

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Vous pouvez taper `signtool sign /?` pour voir ce que fait chaque paramètre, mais en bref :
      * `/fd` définit l’algorithme de synthèse de fichiers (SHA256 est la valeur par défaut pour. AppX)
      * `/a` sélectionne automatiquement le meilleur certificat
      * `/f` Spécifie le fichier d’entrée qui contient le certificat de signature

Enfin, vous pouvez maintenant double-cliquer sur le `.appx` fichier pour l’installer, ou si vous préférez la ligne de commande, vous pouvez ouvrir une invite PowerShell, accéder au répertoire contenant le package et taper ce qui suit ( `contoso_demo.appx` en remplaçant par le nom de votre package) :

```CMD
add-appxpackage contoso_demo.appx
```

Si vous recevez des erreurs sur le certificat qui n’est pas approuvé, assurez-vous qu’il est ajouté au magasin de l’ordinateur (et**non** au magasin de l’utilisateur). Pour ajouter le certificat au magasin de l’ordinateur, vous pouvez utiliser la ligne de commande ou l’Explorateur Windows.

Pour utiliser la ligne de commande :

1. Exécutez une invite de commandes Visual Studio 2017 ou Visual Studio 2019 en tant qu’administrateur.
2. Basculez vers le répertoire qui contient le `.cer` fichier (n’oubliez pas de vérifier qu’il se trouve en dehors de vos répertoires sources ou de projets !)
3. Tapez la commande suivante, `contoso_demo.cer` en remplaçant par votre nom de fichier :
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Vous pouvez exécuter `certutil -addstore /?` pour voir ce que fait chaque paramètre, mais en bref :
      * `-addstore` Ajoute un certificat à un magasin de certificats
      * `TrustedPeople` indique le magasin dans lequel le certificat est placé

Pour utiliser l’Explorateur Windows :

1. Accédez au dossier qui contient le `.pfx` fichier.
2. Double-cliquez sur le `.pfx` fichier pour que l' **Assistant importation certicicate** apparaisse.
3. Choisissez `Local Machine` et cliquez sur `Next`
4. Acceptez l’invite d’élévation de l’administrateur du contrôle de compte d’utilisateur, si elle apparaît, puis cliquez sur `Next`
5. Entrez le mot de passe de la clé privée, le cas échéant, puis cliquez sur `Next`
6. Sélectionnez `Place all certificates in the following store`
7. Cliquez sur `Browse` , puis sélectionnez le `Trusted People` dossier (et**non** « éditeurs approuvés »)
8. Cliquez sur `Next` , puis `Finish`

Après avoir ajouté le certificat au `Trusted People` magasin, réessayez d’installer le package.

Vous devez maintenant voir votre application apparaître dans la liste « toutes les applications » du menu Démarrer, avec les informations correctes à partir du `.resw`  /  `.pri` fichier. Si vous voyez une chaîne vide ou une chaîne `ms-resource:...` , cela signifie que le problème est survenu. Vérifiez vos modifications et assurez-vous qu’elles sont correctes. Si vous cliquez avec le bouton droit sur votre application dans le menu Démarrer, vous pouvez l’épingler sous forme de vignette et vérifier que les informations correctes s’affichent également.

### <a name="step-13-add-more-supported-languages"></a>Étape 1,3 : ajouter d’autres langues prises en charge

Une fois les modifications apportées au manifeste du package et le `resources.resw` fichier initial créé, il est facile d’ajouter des langues supplémentaires.

#### <a name="create-additional-localized-resources"></a>Créer des ressources localisées supplémentaires

Tout d’abord, créez les valeurs de ressource localisées supplémentaires. 

Dans le `Strings` dossier, créez des dossiers supplémentaires pour chaque langue que vous prenez en charge à l’aide du code BCP-47 approprié (par exemple, `Strings\de-DE` ). Dans chacun de ces dossiers, créez un `resources.resw` fichier (à l’aide d’un éditeur XML ou du concepteur Visual Studio) qui comprend les valeurs de ressource traduites. Il est supposé que vous avez déjà les chaînes localisées disponibles quelque part et que vous devez simplement les copier dans le `.resw` fichier ; ce document ne couvre pas l’étape de traduction proprement dite. 

Par exemple, le `Strings\de-DE\resources.resw` fichier peut se présenter comme ceci, avec le <span style="background-color: yellow">texte en surbrillance</span> `en-US` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

Les étapes suivantes supposent que vous avez ajouté des ressources pour `de-DE` et `fr-FR` , mais le même modèle peut être suivi pour n’importe quel langage.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Mettre à jour le manifeste du package pour répertorier les langues prises en charge

Le manifeste du package doit être mis à jour pour répertorier les langues prises en charge par l’application. Le convertisseur d’application de bureau ajoute la langue par défaut, mais les autres doivent être ajoutés de manière explicite. Si vous modifiez le `AppxManifest.xml` fichier directement, mettez à jour le `Resources` nœud comme suit, en ajoutant autant d’éléments que vous le souhaitez, et en remplaçant les <span style="background-color: yellow">langues appropriées que vous prenez en charge</span> et en vous assurant que la première entrée de la liste est la langue par défaut (de secours). Dans cet exemple, la valeur par défaut est anglais (États-Unis) avec prise en charge supplémentaire pour l’allemand (Allemagne) et le français (France) :

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Si vous utilisez Visual Studio, vous ne devez rien faire. Si vous examinez `Package.appxmanifest` , vous devriez voir la valeur <span style="background-color: yellow">x-Generate</span> spéciale, qui oblige le processus de génération à insérer les langues qu’il trouve dans votre projet (en fonction des dossiers nommés avec les codes BCP-47). Notez qu’il ne s’agit pas d’une valeur valide pour un manifeste de package réel. elle fonctionne uniquement pour les projets Visual Studio :

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Regénération avec les valeurs localisées

À présent, vous pouvez créer et déployer votre application, à nouveau. Si vous modifiez vos préférences linguistiques dans Windows, vous devriez voir que les valeurs nouvellement localisées apparaissent dans le menu Démarrer (les instructions relatives à la modification de votre langue sont indiquées ci-dessous).

Pour Visual Studio, vous pouvez à nouveau utiliser `Ctrl+Shift+B` pour générer et cliquer avec le bouton droit sur le projet pour `Deploy` .

Si vous générez manuellement le projet, suivez les mêmes étapes que ci-dessus, mais ajoutez les autres langues, séparées par des traits de soulignement, à la liste des qualificateurs par défaut ( `/dq` ) lors de la création du fichier de configuration. Par exemple, pour prendre en charge les ressources en anglais, en allemand et en français ajoutées à l’étape précédente :

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Cela créera un fichier PRI qui contient tous les languagesthat spécifiés que vous pouvez facilement utiliser pour le test. Si la taille totale de vos ressources est faible ou si vous ne prenez en charge qu’un petit nombre de langues, cela peut être acceptable pour votre application d’expédition ; C’est uniquement si vous souhaitez bénéficier des avantages de la réduction de la taille d’installation/téléchargement de vos ressources, dont vous avez besoin pour effectuer les tâches supplémentaires de création de modules linguistiques distincts.

#### <a name="test-with-the-localized-values"></a>Tester avec les valeurs localisées

Pour tester les nouvelles modifications localisées, il vous suffit d’ajouter une nouvelle langue d’interface utilisateur préférée à Windows. Il n’est pas nécessaire de télécharger les modules linguistiques, de redémarrer le système ou d’afficher l’intégralité de l’interface utilisateur de Windows dans une langue étrangère. 

1. Exécuter l' `Settings` application ( `Windows + I` )
2. Accédez à `Time & language`
3. Accédez à `Region & language`
4. Cliquez sur `Add a language`
5. Tapez (ou sélectionnez) la langue de votre choix (par exemple `Deutsch` ou `German` )
 * S’il existe des sous-langues, choisissez celle de votre choix (par exemple, `Deutsch / Deutschland` )
6. Sélectionnez la nouvelle langue dans la liste langue
7. Cliquez sur `Set as default`

Maintenant, ouvrez le menu Démarrer et recherchez votre application, et vous devez voir les valeurs localisées pour la langue sélectionnée (d’autres applications peuvent également apparaître localisées). Si vous ne voyez pas le nom localisé immédiatement, patientez quelques minutes jusqu’à ce que le cache du menu Démarrer soit actualisé. Pour revenir à votre langue native, il vous suffit d’en faire la langue par défaut dans la liste des langues. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Étape 1,4 : localisation d’autres parties du manifeste du package (facultatif)

D’autres sections du manifeste de package peuvent être localisées. Par exemple, si votre application gère les extensions de fichier, elle doit alors avoir une `windows.fileTypeAssociation` extension dans le manifeste, en utilisant le <span style="background-color: lightgreen">texte vert mis en surbrillance</span> comme indiqué (car il fait référence aux ressources) et en remplaçant le <span style="background-color: yellow">texte jaune mis en surbrillance</span> par des informations spécifiques à votre application :

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Vous pouvez également ajouter ces informations à l’aide du concepteur de manifeste de Visual Studio, à l’aide `Declarations` de l’onglet, en tenant compte des <span style="background-color: lightgreen">valeurs en surbrillance</span>:

:::image type="content" source="images\editing-declarations-info.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque." :::

Ajoutez à présent les noms de ressources correspondants à chacun de vos `.resw` fichiers, en remplaçant le <span style="background-color: yellow">texte mis en surbrillance</span> par le texte approprié pour votre application (n’oubliez pas d’effectuer cette opération pour *chaque langue prise en charge !*) :

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Elle s’affiche alors dans des parties de l’interpréteur de commandes Windows, telles que l’Explorateur de fichiers :

:::image type="content" source="images\file-type-tool-tip.png" alt-text="Capture d’écran d’une étiquette de code source, d’une étiquette de table de recherche et d’un nom de fichier sur le disque.":::

Générez et testez le package comme précédemment, en exerçant tous les nouveaux scénarios qui doivent afficher les nouvelles chaînes d’interface utilisateur.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2 : utiliser le MRT pour identifier et localiser des ressources

La section précédente a montré comment utiliser MRT pour localiser le fichier manifeste de votre application afin que le shell Windows puisse afficher correctement le nom de l’application et d’autres métadonnées. Aucune modification de code n’a été requise pour ce. elle nécessitait simplement l’utilisation de `.resw` fichiers et d’outils supplémentaires. Cette section explique comment utiliser le MRT pour localiser des ressources dans vos formats de ressources existants et utiliser votre code de gestion des ressources existant avec des modifications minimes.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Hypothèses relatives à la disposition de fichier existante & le code d’application

Étant donné qu’il existe de nombreuses façons de localiser des applications de bureau Win32, ce document va simplifier les hypothèses de la structure de l’application existante que vous devrez mapper à votre environnement spécifique. Vous devrez peut-être apporter des modifications à votre structure de base de code ou de ressource existante pour répondre aux exigences du fichier MRT, et celles-ci sont largement hors de portée pour ce document.

#### <a name="resource-file-layout"></a>Disposition du fichier de ressources

Cet article suppose que vos ressources localisées ont toutes les mêmes noms de fichiers (par exemple, `contoso_demo.exe.mui` ou `contoso_strings.dll` ou `contoso.strings.xml` ), mais qu’elles sont placées dans des dossiers différents avec des noms BCP-47 ( `en-US` , `de-DE` , etc.). Peu importe le nombre de fichiers de ressources dont vous disposez, leurs noms, leurs formats de fichier/API associées, etc. La seule chose à faire est que chaque ressource *logique* a le même nom de fichier (mais placée dans un répertoire *physique* différent). 

En guise d’exemple de compteur, si votre application utilise une structure de fichier plat avec un `Resources` répertoire unique contenant les fichiers `english_strings.dll` et `french_strings.dll` , elle ne correspond pas bien à MRT. Une meilleure structure serait un `Resources` répertoire contenant des sous-répertoires et des fichiers `en\strings.dll` et `fr\strings.dll` . Il est également possible d’utiliser le même nom de fichier de base, mais avec des qualificateurs intégrés, tels que `strings.lang-en.dll` et `strings.lang-fr.dll` , mais l’utilisation de répertoires avec les codes de langue est conceptuellement plus simple. c’est donc ce que nous allons concentrer.

>[!NOTE]
> Il est toujours possible d’utiliser le programme MRT et les avantages de l’empaquetage même si vous ne pouvez pas suivre cette Convention d’affectation de noms de fichiers. Cela nécessite simplement plus de travail.

Par exemple, l’application peut avoir un ensemble de commandes d’interface utilisateur personnalisées (utilisées pour les étiquettes de bouton, etc.) dans un fichier texte simple nommé <span style="background-color: yellow">ui.txt</span>, disposé sous un dossier <span style="background-color: yellow">UICommands</span> :

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>Code de chargement des ressources

Cet article suppose que, à un moment donné dans votre code, vous souhaitez localiser le fichier qui contient une ressource localisée, le charger, puis l’utiliser. Les API utilisées pour charger les ressources, les API utilisées pour extraire les ressources, etc., ne sont pas importantes. En pseudo-code, il existe trois étapes :

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT nécessite uniquement la modification des deux premières étapes de ce processus : la façon dont vous déterminez les meilleures ressources candidates et comment les localiser. Vous n’avez pas besoin de modifier la façon dont vous chargez ou utilisez les ressources (bien qu’elle fournisse les fonctionnalités nécessaires si vous souhaitez en tirer parti).

Par exemple, l’application peut utiliser l’API Win32 `GetUserPreferredUILanguages` , la fonction CRT `sprintf` et l’API Win32 `CreateFile` pour remplacer les trois fonctions pseudocode ci-dessus, puis analyser manuellement le fichier texte à la recherche de `name=value` paires. (Les détails ne sont pas importants. il s’agit simplement de montrer que le MRT n’a aucun impact sur les techniques utilisées pour gérer les ressources une fois qu’elles ont été localisées).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Étape 2,1 : modifications du code pour utiliser MRT pour localiser des fichiers

Le basculement de votre code pour utiliser le MRT pour localiser des ressources n’est pas difficile. Il requiert l’utilisation de quelques types WinRT et quelques lignes de code. Les principaux types que vous allez utiliser sont les suivants :

* [ResourceContext](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), qui encapsule l’ensemble actuellement actif de valeurs de qualificateur (langue, facteur d’échelle, etc.)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (la version WinRT, et non la version .net), qui permet d’accéder à toutes les ressources à partir du fichier PRI
* [ResourceMap](/uwp/api/windows.applicationmodel.resources.core.resourcemap), qui représente un sous-ensemble spécifique des ressources dans le fichier PRI (dans cet exemple, les ressources basées sur des fichiers et les ressources de type chaîne)
* [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), qui représente une ressource logique et tous ses candidats possibles
* [ResourceCandidate](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), qui représente une ressource candidate concrète unique 

En pseudo-code, la façon dont vous résolvez un nom de fichier de ressources donné (comme `UICommands\ui.txt` dans l’exemple ci-dessus) est la suivante :

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

Notez, en particulier, que le code ne demande **pas** de dossier de langage spécifique, `UICommands\en-US\ui.txt` même si c’est la façon dont les fichiers existent sur le disque. Au lieu de cela, il demande le nom de fichier *logique* `UICommands\ui.txt` et s’appuie sur MRT pour trouver le fichier sur disque approprié dans l’un des répertoires de langue.

À partir de là, l’exemple d’application peut continuer à utiliser `CreateFile` pour charger `absoluteFileName` et analyser les `name=value` paires comme auparavant ; aucune de cette logique n’a besoin d’être modifiée dans l’application. Si vous écrivez en C# ou C++/CX, le code réel n’est pas beaucoup plus complexe que cela (et en fait, la plupart des variables intermédiaires peuvent être élidé). pour plus d’informations, consultez la section sur le **chargement des ressources .net**ci-dessous. Les applications C++/WRL-based seront plus complexes en raison des API COM de bas niveau utilisées pour activer et appeler les API WinRT, mais les étapes fondamentales que vous prenez sont les mêmes : consultez la section sur le **chargement des ressources MUI Win32**, ci-dessous.

#### <a name="loading-net-resources"></a>Chargement des ressources .NET

Étant donné que .NET dispose d’un mécanisme intégré pour localiser et charger des ressources (appelés « assemblys satellites »), il n’existe pas de code explicite à remplacer comme dans l’exemple synthétique ci-dessus-dans .NET, vous avez simplement besoin de vos dll de ressource dans les répertoires appropriés et ils sont automatiquement localisés pour vous. Quand une application est empaquetée en tant que MSIX ou. AppX à l’aide de packs de ressources, la structure de répertoires est quelque peu différente, plutôt que de faire en sorte que les répertoires de ressources soient des sous-répertoires du répertoire principal de l’application. 

Par exemple, Imaginez une application .NET avec la mise en page suivante, où tous les fichiers se trouvent dans le `MainApp` dossier :

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

Après la conversion en. AppX, la disposition ressemblera à ce qui suit, en supposant que `en-US` était la langue par défaut et que l’utilisateur a à la fois l’allemand et le français répertoriés dans leur liste de langues :

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

Étant donné que les ressources localisées n’existent plus dans les sous-répertoires situés sous l’emplacement d’installation de l’exécutable principal, la résolution des ressources .NET intégrées échoue. Heureusement, .NET dispose d’un mécanisme bien défini pour gérer les tentatives de chargement d’assembly ayant échoué : l' `AssemblyResolve` événement. Une application .NET utilisant MRT doit s’inscrire à cet événement et fournir l’assembly manquant pour le sous-système de ressources .NET. 

Voici un exemple concis de l’utilisation des API WinRT pour localiser les assemblys satellites utilisés par .NET : le code présenté est compressé intentionnellement pour afficher une implémentation minimale, bien que vous puissiez le voir correspondre étroitement au pseudo-code ci-dessus, avec le passé `ResolveEventArgs` fournissant le nom de l’assembly que nous devons localiser. Une version exécutable de ce code (avec des commentaires détaillés et la gestion des erreurs) est disponible dans le fichier `PriResourceRsolver.cs` de l’exemple de programme de [résolution d' **assembly .net** sur GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo).

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Compte tenu de la classe ci-dessus, vous devez ajouter les éléments suivants dans le code de démarrage de votre application (avant de pouvoir charger les ressources localisées) :

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

Le Runtime .NET déclenche l' `AssemblyResolve` événement lorsqu’il ne peut pas trouver les dll de ressource, auquel cas le gestionnaire d’événements fourni localise le fichier souhaité via MRT et retourne l’assembly.

> [!NOTE]
> Si votre application a déjà un `AssemblyResolve` Gestionnaire à d’autres fins, vous devrez intégrer le code de résolution des ressources avec votre code existant.

#### <a name="loading-win32-mui-resources"></a>Chargement des ressources MUI Win32

Le chargement des ressources MUI Win32 est fondamentalement le même que le chargement d’assemblys satellites .NET, mais à l’aide de code C++/CX ou C++/WRL à la place. L’utilisation de C++/CX permet un code plus simple qui correspond étroitement au code C# ci-dessus, mais il utilise des extensions de langage C++, des commutateurs de compilateur et un surplus d’exécution supplémentaire que vous pouvez souhaiter éviter. Si tel est le cas, l’utilisation de C++/WRL fournit une solution bien plus faible au détriment d’un code plus détaillé. Néanmoins, si vous êtes familiarisé avec la programmation ATL (ou COM en général), WRL devrait vous paraître familier. 

L’exemple de fonction suivant montre comment utiliser C++/WRL pour charger une DLL de ressource spécifique et retourner un `HINSTANCE` qui peut être utilisé pour charger des ressources supplémentaires à l’aide des API de ressource Win32 habituelles. Notez que contrairement à l’exemple C# qui initialise explicitement le `ResourceContext` avec la langue demandée par le Runtime .net, ce code s’appuie sur le langage actuel de l’utilisateur.

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>Phase 3 : génération de packs de ressources

Maintenant que vous avez un « pack Fat » qui contient toutes les ressources, il existe deux chemins d’accès à la création de packages principaux et de packages de ressources distincts afin de réduire les tailles de téléchargement et d’installation :

* Prenez un pack Fat existant et exécutez-le par le biais de [l’outil Bundle Generator](https://www.microsoft.com/store/apps/9nblggh43pmq) pour créer automatiquement des packs de ressources. Il s’agit de l’approche recommandée si vous disposez d’un système de génération qui produit déjà un pack FAT et que vous souhaitez le poster-traiter pour générer les packs de ressources.
* Générez directement les packages de ressources individuels et créez-les dans un bundle. Il s’agit de l’approche recommandée si vous avez davantage de contrôle sur votre système de génération et que vous pouvez générer les packages directement.

### <a name="step-31-creating-the-bundle"></a>Étape 3,1 : création du bundle

#### <a name="using-the-bundle-generator-tool"></a>Utilisation de l’outil Bundle Generator

Pour pouvoir utiliser l’outil Bundle Generator, le fichier de configuration PRI créé pour le package doit être mis à jour manuellement pour supprimer la `<packaging>` section.

Si vous utilisez Visual Studio, reportez-vous à la rubrique pour vous [assurer que les ressources sont installées sur un appareil, qu’un appareil en ait besoin ou non](/previous-versions/dn482043(v=vs.140)) pour obtenir des informations sur la façon de générer toutes les langues dans le package principal en créant les fichiers `priconfig.packaging.xml` et `priconfig.default.xml` .

Si vous modifiez manuellement des fichiers, procédez comme suit : 

1. Créez le fichier de configuration de la même manière qu’avant, en remplaçant le chemin d’accès correct, le nom de fichier et les langues :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Ouvrez manuellement le `.xml` fichier créé et supprimez la section entière (tout en conservant les `&lt;packaging&rt;` autres éléments intacts) :

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Générez le `.pri` fichier et le `.appx` package comme précédemment, en utilisant le fichier de configuration mis à jour et les noms de répertoire et de fichier appropriés (voir ci-dessus pour plus d’informations sur ces commandes) :

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Une fois le package créé, utilisez la commande suivante pour créer le bundle, à l’aide des noms de répertoire et de fichier appropriés :

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Vous pouvez maintenant passer à l’étape finale, signature (voir ci-dessous).

#### <a name="manually-creating-resource-packages"></a>Création manuelle de packages de ressources

La création manuelle de packages de ressources requiert l’exécution d’un ensemble de commandes légèrement différent pour générer des `.pri` `.appx` fichiers distincts. ceux-ci sont tous similaires aux commandes utilisées ci-dessus pour créer des packages FAT. une explication minime est donc donnée. Remarque : toutes les commandes supposent que le répertoire actif est le répertoire contenant le `AppXManifest.xml` fichier, mais que tous les fichiers sont placés dans le répertoire parent (vous pouvez utiliser un autre répertoire, si nécessaire, mais vous ne devez pas polluer le répertoire du projet avec l’un de ces fichiers). Comme toujours, remplacez les noms de fichiers « contoso » par vos propres noms de fichiers.

1. Utilisez la commande suivante pour créer un fichier de configuration qui nomme **uniquement** la langue par défaut comme qualificateur par défaut, dans le cas présent, `en-US` :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Créez un fichier par défaut `.pri` et un `.map.txt` fichier pour le package principal, ainsi qu’un ensemble supplémentaire de fichiers pour chaque langue trouvée dans votre projet, à l’aide de la commande suivante :

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Utilisez la commande suivante pour créer le package principal (qui contient le code exécutable et les ressources de langue par défaut). Comme toujours, modifiez le nom comme vous le souhaitez, même si vous devez placer le package dans un répertoire distinct pour faciliter la création du bundle plus tard (cet exemple utilise le `..\bundle` Répertoire) :

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Une fois le package principal créé, utilisez la commande suivante une fois pour chaque langue supplémentaire (par exemple, répétez cette commande pour chaque fichier de mappage de langue généré à l’étape précédente). Là encore, la sortie doit se trouver dans un répertoire distinct (le même que celui du package principal). Notez que la langue est spécifiée à la **fois** dans l' `/f` option et l' `/p` option, et l’utilisation du nouvel `/r` argument (qui indique qu’un package de ressources est souhaité) :

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Combinez tous les packages du répertoire de regroupement dans un `.appxbundle` fichier unique. La nouvelle `/d` option spécifie le répertoire à utiliser pour tous les fichiers du bundle (c’est pourquoi les `.appx` fichiers sont placés dans un répertoire distinct à l’étape précédente) :

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

La dernière étape de la création du package consiste à signer.

### <a name="step-32-signing-the-bundle"></a>Étape 3,2 : signature du bundle

Une fois que vous avez créé le `.appxbundle` fichier (à l’aide de l’outil de génération d’un bundle ou manuellement), vous disposez d’un seul fichier qui contient le package principal et tous les packages de ressources. La dernière étape consiste à signer le fichier afin que Windows l’installe :

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Cela génère un fichier signé `.appxbundle` qui contient le package principal et tous les packages de ressources spécifiques à la langue. Vous pouvez double-cliquer sur un fichier de package pour installer l’application, ainsi que les langues appropriées en fonction des préférences linguistiques de l’utilisateur.

## <a name="related-topics"></a>Rubriques connexes

* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)