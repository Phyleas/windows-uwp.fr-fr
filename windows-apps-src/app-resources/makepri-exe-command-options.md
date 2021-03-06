---
description: MakePri.exe a le jeu de commandes createconfig, dump, New, ResourcePack et Versioned. Cette rubrique décrit en détail leur utilisation.
title: Options de ligne de commande de MakePri.exe
template: detail.hbs
ms.date: 04/10/2018
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 7443efbb227bf3f9ea64db58902ebeb67b02f676
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031732"
---
# <a name="makepriexe-command-line-options"></a>Options de ligne de commande de MakePri.exe

[MakePri.exe](compile-resources-manually-with-makepri.md) a le jeu de commandes `createconfig` ,,, `dump` `new` `resourcepack` et `versioned` . Cette rubrique décrit en détail les options de ligne de commande pour leur utilisation.

> [!NOTE]
> MakePri.exe est installé lorsque vous activez l’option **SDK Windows pour les applications gérées UWP** lors de l’installation du kit de développement logiciel (SDK) Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (et dans les dossiers nommés pour les autres architectures). Par exemple : `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="getting-help-from-the-command-line"></a>Obtenir de l’aide à partir de la ligne de commande

Vous pouvez exécuter `MakePri.exe help` ou `MakePri.exe /?` pour afficher les commandes que vous pouvez utiliser avec MakePri.exe. Vous pouvez également émettre `MakePri.exe <command> /?` pour voir des détails sur une commande et, dans de rares cas, même `MakePri.exe <command> <option>` pour voir des détails sur une option.

## <a name="makepri-commands"></a>Commandes MakePri

```console
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Commande Createconfig

La `createconfig` commande crée un nouveau fichier de configuration PRI initialisé définissant les valeurs par défaut du qualificateur que vous spécifiez. Exécutez `MakePri.exe createconfig /?` pour obtenir de l’aide détaillée sur cette commande.

```console
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Dump, commande

La `dump` commande génère un fichier XML vidé contenant une liste de toutes les ressources dans un fichier PRI spécifié. Exécutez `MakePri.exe dump /?` pour obtenir de l’aide détaillée sur cette commande.

> [!NOTE]
> Un pack de ressources sans schéma est un Pack qui a été créé avec le commutateur *omitSchemaFromResourcePacks* dans le fichier de configuration PRI. Pour vider un pack de ressources sans schéma, utilisez le commutateur `/es <main_package_PRI_file>` . Si vous ne spécifiez pas le fichier principal, vous verrez le message *d’erreur « le fichier Resources. pri dans le package a été endommagé ; le chiffrement a échoué (erreur PRI222:0xdef0000f-une erreur non spécifiée s’est produite)* ».

```console
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>Nouvelle commande

La `new` commande crée un nouveau fichier pri en indexant les fichiers de votre projet comme indiqué par votre fichier de configuration. Exécutez `MakePri.exe new /?` pour obtenir de l’aide détaillée sur cette commande.

```console
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the .appx package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>Commande ResourcePack

La `resourcepack` commande crée un nouveau fichier pri en indexant les fichiers de votre projet comme indiqué par votre fichier de configuration. Un fichier PRI de Pack de ressources contient uniquement des variantes de ressources supplémentaires déjà spécifiées dans un fichier PRI existant. Exécutez `MakePri.exe resourcepack /?` pour obtenir de l’aide détaillée sur cette commande.

```console
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Commande avec version

La `versioned` commande crée un fichier PRI avec version en indexant les fichiers de votre projet comme indiqué par votre fichier de configuration. Exécutez `MakePri.exe versioned /?` pour obtenir de l’aide détaillée sur cette commande.

```console
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll (ex)

Vous utilisez l’option de DLL d’extension (/ex) avec `createconfig` , `dump` , `new` , `resourcepack` et `versioned` pour spécifier l’emplacement de la dll d’extension de l’environnement du système de gestion des ressources.

## <a name="logging47metadata-file"></a>Journalisation&#47;fichier de métadonnées

MakePri peut inclure des informations spécifiques à un pack de ressources dans le fichier de métadonnées de l’indexeur. Voici un exemple de fichier journal pour `resources.pri` avec les fichiers PRI de ressource `german.pri` et `highresolution.pri` .

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>&#47;option IndexFile (IF)

Vous utilisez l’option de fichier d’index (/IF) avec `dump` , `resourcepack` et `versioned` pour spécifier un fichier PRI d’entrée.

Pour `resourcepack` et `versioned` , au lieu de fournir un fichier PRI comme paramètre d’entrée pour/IndexFile (si), vous pouvez fournir à la place un fichier de schéma.

```console
/IndexFile(if) <FILEPATH>
```

**FilePath** est un jeton qui spécifie l’emplacement du fichier PRI d’entrée ou du fichier de schéma PRI.

## <a name="47indexoptionsio-option"></a>Option &#47;IndexOptions (e/s)

Vous utilisez l’option d’index (/IO) avec `new` , `resourcepack` et `versioned` pour spécifier des options qui fournissent un contrôle détaillé sur le comportement des indexeurs de ressources. Les options d’index sont désactivées par défaut.

```console
/IndexOptions(io) <OPTIONS>
```

**Options** est une liste séparée par des virgules constituée des options suivantes.

- +/-HiddenFiles (HF). Fichiers et dossiers masqués d’index (+) ou ignorés (-).
- +/-LinkedFiles (LF). Index (+) ou ignorer les fichiers et dossiers liés.

## <a name="47mappingfilemf-option"></a>Option &#47;MappingFile (MF)

Vous utilisez l’option de fichier de mappage (/MF) avec `new` , `resourcepack` et `versioned` pour générer un fichier de mappage. [MakeAppx.exe](/windows/msix/package/create-app-package-with-makeappx-tool) utilise le fichier de mappage pour générer des packages d’application.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** est un jeton qui spécifie le format du fichier de mappage. Le seul format pris en charge valide est `appx` .

```console
/mf appx
```

Il s’agit d’un exemple de contenu d’un fichier de mappage principal.

```console
"ResourceDimensions"                   "language-de-de"
```

Il s’agit d’un exemple de contenu d’un fichier de mappage de Pack de ressources.

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Résumé de la sortie

Si des packs de ressources sont créés, le résumé de la sortie de MakePRI.exe est plus détaillé. Voici un exemple.

```console
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>&#47;option de remplacement (o)

Si l’option de surécriture (/o) n’est pas fournie et que le ou les fichiers de sortie spécifiés existent déjà, MakePri.exe nécessite une confirmation avant le remplacement.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;option OutputFile (of)

Vous utilisez l’option de fichier de sortie (/of) avec `dump` ,, `new` `resourcepack` et `versioned` pour spécifier l’emplacement de sortie et le nom du fichier PRI à générer. Si MakePri.exe génère plus d’un fichier PRI de ressource, il les place dans le dossier parent du fichier cible. Par exemple, si vous spécifiez, `/of MyParentFolder\TargetFile.pri` MakePri.exe génère `TargetFile.language-en.pri` et à `TargetFile.scale-100.pri` côté de `TargetFile.pri` `ParentFolder` .

Voici un exemple de condition d’erreur et le message d’erreur correspondant.

| État d’erreur | Message d’erreur |
| --------------- | ------------- |
| Le nom du fichier de sortie est le même que l’un des noms des packs de ressources dans la configuration. | Configuration non valide : le nom du Pack de ressources <resource pack name> ne peut pas être le même que le fichier de sortie <> OutputFileName. pri. |

## <a name="reversemaprm-option"></a>/ReverseMap (RM) (option)

Vous utilisez l’option de mappage inverse (/RM) avec `new` , `resourcepack` et `versioned` pour générer une section de mappage inverse dans le fichier PRI, qui peut être utilisé pour le débogage.

## <a name="47schemafilesf-option"></a>Option &#47;fichierschéma (DF)

Vous utilisez l’option de fichier de schéma (/SF) avec `new` , `resourcepack` et `versioned` pour écrire un fichier de schéma à l’emplacement spécifié.

Pour `resourcepack` et `versioned` , au lieu de fournir un fichier PRI comme paramètre d’entrée pour/IndexFile (si), vous pouvez fournir à la place un fichier de schéma.

```console
/SchemaFile(sf) <FILEPATH>
```

**FilePath** est un jeton qui spécifie l’emplacement d’écriture du fichier de schéma.

Il s’agit d’un exemple de fichier de schéma.

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor (VMA) est déconseillé

L’option version principale (/VMA) (pour la `new` commande) est déconseillée et l’utilisation de ce message génère un message d’avertissement.

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Pour fournir le numéro de version principale, utilisez l' [resources@majorVersion](makepri-exe-configuration.md) attribut dans votre fichier de configuration.

## <a name="related-topics"></a>Rubriques connexes

* [MakePri.exe](compile-resources-manually-with-makepri.md)
