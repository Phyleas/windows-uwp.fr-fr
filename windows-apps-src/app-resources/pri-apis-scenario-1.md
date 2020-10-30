---
description: Dans ce scénario, nous allons créer une nouvelle application pour représenter notre système de génération personnalisé. Nous allons créer un indexeur de ressource et y ajouter des chaînes et d’autres types de ressources. Ensuite, nous allons générer et vider un fichier PRI.
title: Scénario 1 générer un fichier PRI à partir de ressources de type chaîne et de fichiers d’éléments multimédias
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 44f4b8297cc1a34a378af137f75babca64e4edf2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031672"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Scénario 1 : générer un fichier PRI à partir de ressources de type chaîne et de fichiers d’éléments multimédias
Dans ce scénario, nous allons utiliser les [API PRI (package Resource Indexing)](/windows/desktop/menurc/pri-indexing-reference) pour créer une nouvelle application représentant notre système de génération personnalisé. L’objectif de ce système de génération personnalisé, n’oubliez pas, est de créer des fichiers PRI pour une application UWP cible. Ainsi, dans le cadre de cette procédure pas à pas, nous allons créer des exemples de fichiers de ressources (contenant des chaînes et d’autres types de ressources) pour représenter les ressources de l’application UWP cible.

## <a name="new-project"></a>Nouveau projet
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet d' **application console Windows Visual C++** , puis nommez-le *CBSConsoleApp* (pour « build personnalisée application console système »).

Choisissez *x64* dans la liste déroulante **plateformes solution** .

## <a name="headers-static-library-and-dll"></a>En-têtes, bibliothèque statique et dll
Les API PRI sont déclarées dans le fichier d’en-tête MrmResourceIndexer. h (qui est installé dans `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\` ). Ouvrez le fichier `CBSConsoleApp.cpp` et incluez l’en-tête avec d’autres en-têtes dont vous aurez besoin.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Les API sont implémentées dans MrmSupport.dll, auxquelles vous accédez en établissant une liaison avec la bibliothèque statique MrmSupport. lib. Ouvrez les **Propriétés** de votre projet, cliquez sur entrée de l' **éditeur de liens**  >  **Input** , modifiez **AdditionalDependencies** et ajoutez `MrmSupport.lib` .

Générez la solution, puis copiez-la à `MrmSupport.dll` partir de `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` dans votre dossier de sortie de génération (probablement `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\` ).

Ajoutez la fonction d’assistance suivante à `CBSConsoleApp.cpp` , car nous en aurons besoin.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

Dans la `main()` fonction, ajoutez des appels pour initialiser et désinitialiser com.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Fichiers de ressources appartenant à l’application UWP cible
À présent, nous avons besoin de quelques exemples de fichiers de ressources (contenant des chaînes et d’autres types de ressources) pour représenter les ressources de l’application UWP cible. Ils peuvent bien entendu se trouver n’importe où dans votre système de fichiers. Toutefois, pour cette procédure pas à pas, il sera pratique de les placer dans le dossier de projet de CBSConsoleApp afin que tout se trouve au même endroit. Vous devez uniquement ajouter ces fichiers de ressources au système de fichiers. ne les ajoutez pas au projet CBSConsoleApp.

Dans le dossier qui contient `CBSConsoleApp.vcxproj` , ajoutez un nouveau sous-dossier nommé `UWPAppProjectRootFolder` . Dans ce nouveau sous-dossier, créez ces exemples de fichiers de ressources.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Ce fichier peut contenir n’importe quelle image PNG.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>Indexer les ressources et créer un fichier PRI
Dans la `main()` fonction, avant l’appel à Initialize com, déclarez des chaînes dont nous aurons besoin et créez également le dossier de sortie dans lequel nous allons générer notre fichier PRI.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Immédiatement après l’appel à Initialize COM, déclarez un handle d’indexeur de ressource, puis appelez [**MrmCreateResourceIndexer**](/windows/desktop/menurc/mrmcreateresourceindexer) pour créer un indexeur de ressource.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Voici une explication des arguments passés à **MrmCreateResourceIndexer** .

- Nom de la famille de packages de notre application UWP cible, qui sera utilisé comme nom de mappage de ressources lorsque nous générerons ultérieurement un fichier PRI à partir de cet indexeur de ressource.
- Racine du projet de notre application UWP cible. En d’autres termes, le chemin d’accès à nos fichiers de ressources. Nous le spécifions afin que nous puissions ensuite spécifier des chemins relatifs à cette racine dans les appels d’API suivants vers le même indexeur de ressource.
- Version de Windows que nous voulons cibler.
- Liste des qualificateurs de ressources par défaut.
- Pointeur vers notre handle d’indexeur de ressource pour que la fonction puisse le définir.

L’étape suivante consiste à ajouter nos ressources à l’indexeur de ressources que nous venons de créer. `resources.resw` est un fichier de ressources (. resw) qui contient les chaînes neutres pour notre application UWP cible. Faites défiler vers le haut (dans cette rubrique) si vous souhaitez afficher son contenu. `de-DE\resources.resw` contient nos chaînes allemandes et `en-US\resources.resw` nos chaînes en anglais. Pour ajouter les ressources de chaîne dans un fichier de ressources à un indexeur de ressource, vous devez appeler [**MrmIndexResourceContainerAutoQualifiers**](/windows/desktop/menurc/mrmindexresourcecontainerautoqualifiers). Troisièmement, nous appelons la fonction [**MrmIndexFile**](/windows/desktop/menurc/mrmindexfile) dans un fichier contenant une ressource d’image neutre pour l’indexeur de ressource.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

Dans l’appel à **MrmIndexFile** , la valeur L "MS-Resource:///Files/sample-image.png" est l’URI de ressource. Le premier segment de chemin d’accès est « Files » et c’est ce qui sera utilisé comme nom de sous-arborescence du mappage de ressources lorsque nous générerons ultérieurement un fichier PRI à partir de cet indexeur de ressource.

Après avoir informé l’indexeur de ressources sur nos fichiers de ressources, il est temps de générer un fichier PRI sur le disque en appelant la fonction [**MrmCreateResourceFile**](/windows/desktop/menurc/mrmcreateresourcefile) .

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

À ce stade, un fichier PRI nommé `resources.pri` a été créé à l’intérieur d’un dossier nommé `Generated PRIs` . Maintenant que nous avons terminé avec l’indexeur de ressource, nous appelons [**MrmDestroyIndexerAndMessages**](/windows/desktop/menurc/mrmdestroyindexerandmessages) pour détruire son handle et libérer toutes les ressources d’ordinateur qu’il a allouées.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Dans la mesure où un fichier PRI est binaire, il sera plus facile de voir ce que nous venons de générer si vous videz le fichier PRI binaire dans son équivalent XML. Ce n’est qu’un appel à [**MrmDumpPriFile**](/windows/desktop/menurc/mrmdumpprifile) .

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Voici une explication des arguments passés à **MrmDumpPriFile** .

- Chemin d’accès au fichier PRI à vider. Nous n’utilisons pas l’indexeur de ressource dans cet appel (nous l’avons simplement détruit). nous devons donc spécifier un chemin d’accès complet au fichier.
- Aucun fichier de schéma. Nous discuterons de ce qu’est un schéma plus loin dans cette rubrique.
- Uniquement les informations de base.
- Chemin d’accès d’un fichier XML à créer.

C’est ce que contient le fichier PRI, qui est vidé dans XML ici.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

Les informations commencent par une carte de ressources, nommée avec le nom de la famille de packages de notre application UWP cible. Le mappage des ressources comporte deux sous-arborescences de mappage de ressources : une pour les ressources de fichier que nous avons indexées et une autre pour nos ressources de type chaîne. Notez que le nom de la famille de packages a été inséré dans tous les URI de ressource.

La première ressource de chaîne *EnOnlyString* est EnOnlyString `en-US\resources.resw` et n’a qu’un seul candidat (qui correspond au qualificateur *language-en-US* ). Vient ensuite *LocalizedString1* à partir de `resources.resw` et `en-US\resources.resw` . Par conséquent, il a deux candidats : un *langage correspondant-en-US* et un candidat neutre de secours qui correspond à n’importe quel contexte. De même, *LocalizedString2* a deux candidats : *langue-de-de* et neutre. Enfin, *NeutralOnlyString* existe uniquement sous forme neutre. J’ai donné ce nom pour clarifier le fait qu’il n’est pas destiné à être localisé.

## <a name="summary"></a>Résumé
Dans ce scénario, nous avons montré comment utiliser les [API PRI (package Resource indexation)](/windows/desktop/menurc/pri-indexing-reference) pour créer un indexeur de ressource. Nous avons ajouté des ressources de type chaîne et des fichiers d’élément multimédia à l’indexeur de ressource. Ensuite, nous avons utilisé l’indexeur de ressource pour générer un fichier PRI binaire. Enfin, nous avons vidé le fichier PRI binaire au format XML afin de pouvoir confirmer qu’il contient les informations attendues.

## <a name="important-apis"></a>API importantes
* [Référence PRI (package Resource indexer)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Rubriques connexes
* [API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés](pri-apis-custom-build-systems.md)
