---
description: Vous pouvez utiliser des extensions pour intégrer votre application de bureau empaquetée avec Windows 10 de manière prédéfinie.
title: Moderniser des applications de bureau existantes à l’aide de Desktop Bridge
ms.date: 09/11/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 91b5e975c40b7c9642cd452b3c67045c7be1127d
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668715"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>Intégrer votre application de bureau à Windows 10 et UWP

Si votre application de bureau comporte une [identité de package](modernize-packaged-apps.md), vous pouvez utiliser des extensions pour intégrer votre application à Windows 10 en utilisant des [extensions prédéfinies dans le manifeste du package](/uwp/schemas/appxpackage/uapmanifestschema/extensions).

Par exemple, utilisez une extension pour créer une exception de pare-feu, faites de votre application l’application par défaut pour un type de fichier, ou pointez des vignettes de l’écran de démarrage vers votre application. Pour utiliser une extension, il suffit d’ajouter un peu de XML au fichier manifeste du package de votre application. Aucun code n’est nécessaire.

Cet article décrit ces extensions et les tâches que vous pouvez effectuer en les utilisant.

> [!NOTE]
> Les fonctionnalités décrites dans cet article nécessitent que votre application de bureau comporte une [identité de package](modernize-packaged-apps.md), soit en [empaquetant votre application de bureau dans un package MSIX](/windows/msix/desktop/desktop-to-uwp-root), soit en [accordant l’identité de votre application à l’aide d’un package partiellement alloué](grant-identity-to-nonpackaged-apps.md).

## <a name="transition-users-to-your-app"></a>Migrer les utilisateurs vers votre application

Aidez les utilisateurs à migrer vers votre application empaquetée.

* [Pointer les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée](#point)
* [Ouvrir des fichiers à l’aide de votre application empaquetée au lieu de votre application de bureau](#make)
* [Associer votre application empaquetée à un ensemble de types de fichiers](#associate)
* [Ajouter des options aux menus contextuels de fichiers d'un certain type](#add)
* [Ouvrir certains types de fichiers directement à l’aide d’une URL](#open)

<a id="point"></a>

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Pointer les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée

Vos utilisateurs peuvent avoir épinglé votre application de bureau à la barre des tâches ou au menu Démarrer. Vous pouvez pointer ces raccourcis vers votre nouvelle application empaquetée.

#### <a name="xml-namespace"></a>Espace de noms XML

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nom | Description |
|-------|-------------|
|Catégorie |Toujours ``windows.desktopAppMigration``.
|AumID |L’ID de modèle utilisateur de l’application de votre application empaquetée. |
|ShortcutPath |Le chemin d’accès aux fichiers .lnk qui démarrent la version bureau de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make"></a>

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Ouvrir des fichiers à l’aide de votre application empaquetée au lieu de votre application de bureau

Vous pouvez vous assurer que les utilisateurs ouvrent votre nouvelle application empaquetée par défaut pour certains types de fichiers, au lieu d’ouvrir la version bureau de votre application.

Pour ce faire, vous devez spécifier l’[identificateur programmatique (ProgID)](/windows/desktop/shell/fa-progids) de chaque application à partir de laquelle vous souhaitez hériter des associations de fichiers.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Catégorie |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|MigrationProgId |L’[identificateur programmatique (ProgID)](/windows/desktop/shell/fa-progids) qui décrit l’application, le composant et la version de l’application de bureau à partir de laquelle vous souhaitez hériter des associations de fichiers.|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate"></a>

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Associer votre application empaquetée à un ensemble de types de fichiers

Vous pouvez associer votre application empaquetée à des extensions de types de fichier. Si un utilisateur clique avec le bouton droit sur un fichier, puis sélectionne l’option **Ouvrir avec**, votre application apparaît dans la liste de suggestions.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Catégorie |Toujours ``windows.fileTypeAssociation``.
|Nom | Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace.   |
|FileType |L’extension de fichier prise en charge par votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add"></a>

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Ajouter des options aux menus contextuels de fichiers d'un certain type

Dans la plupart des cas, les utilisateurs double-cliquent sur les fichiers pour les ouvrir. Si les utilisateurs cliquent sur un fichier avec le bouton droit, plusieurs options s’affichent.

Vous pouvez ajouter des options à ce menu. Ces options donnent aux utilisateurs d’autres moyens d’interagir avec votre fichier, par exemple l’impression, la modification ou l’aperçu du fichier.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Catégorie | Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|Verb |Le nom qui apparaît dans le menu contextuel Explorateur de fichiers. Cette chaîne est localisable en utilisant ```ms-resource```.|
|Id |L’ID unique du verbe. Si votre application est une application UWP, cet ID est transmis à votre application en tant qu’argument d’événement d’activation afin qu’elle puisse gérer la sélection de l’utilisateur de façon appropriée. Si votre application est une application empaquetée de confiance totale, elle reçoit des paramètres à la place (voir la puce suivante). |
|Paramètres |La liste des paramètres et des valeurs d’arguments associés au verbe. Si votre application est une application empaquetée de confiance totale, ces paramètres sont transmis à l’application en tant qu’arguments d’événement lorsque l’application est activée. Vous pouvez personnaliser le comportement de votre application en fonction de différents verbes d’activation. Si une variable peut contenir un chemin d’accès de fichier, placez la valeur du paramètre entre guillemets. Vous éviterez les problèmes qui se produisent dans les cas où le chemin d’accès contient des espaces. Si votre application est une application UWP, vous ne pouvez pas transmettre de paramètres. L’application reçoit l’ID à la place (voir la puce précédente).|
|Étendu |Indique que le verbe doit seulement apparaître, si l’utilisateur maintient enfoncée la touche **Maj** avant de cliquer avec le bouton droit sur le fichier pour afficher le menu contextuel. Cet attribut est facultatif et est défini par défaut sur la valeur **False** (par exemple, le verbe est toujours affiché) s’il n’est pas répertorié. Vous devez spécifier ce comportement individuellement pour chaque verbe (à l’exception de « Ouvrir », qui est toujours défini sur **False**).|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open"></a>

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Ouvrir certains types de fichiers directement à l’aide d’une URL

Vous pouvez vous assurer que les utilisateurs ouvrent votre nouvelle application empaquetée par défaut pour certains types de fichiers, au lieu d’ouvrir la version bureau de votre application.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Catégorie |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|UseUrl |Indique s’il faut ouvrir les fichiers directement à partir d’une cible URL. Si vous ne définissez pas cette valeur, lors de tentatives par votre application d’ouvrir un fichier à l’aide d’une URL, le système téléchargera d’abord le fichier localement. |
|Paramètres | Paramètres facultatifs. |
|FileType |Les extensions de fichier appropriées. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>Effectuer des tâches de configuration

* [Créer une exception de pare-feu pour votre application](#rules)
* [Placer vos fichiers DLL dans n'importe quel dossier du package](#load-paths)

<a id="rules"></a>

### <a name="create-firewall-exception-for-your-app"></a>Créer une exception de pare-feu pour votre application

Si votre application requiert une communication via un port, vous pouvez ajouter votre application à la liste des exceptions de pare-feu.

#### <a name="xml-namespace"></a>Espace de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Nom |Description |
|-------|-------------|
|Catégorie |Toujours ``windows.firewallRules``|
|Exécutable |Le nom du fichier exécutable que vous souhaitez ajouter à la liste des exceptions de pare-feu |
|Direction |Indique si la règle est une règle entrante ou sortante |
|IPProtocol |Le protocole de communication |
|LocalPortMin |Le numéro de port le moins élevé d’une plage de numéros de ports locaux. |
|LocalPortMax |Le numéro de port le plus élevé d’une plage de numéros de ports locaux. |
|RemotePortMax |Le numéro de port le moins élevé d’une plage de numéros de ports distants. |
|RemotePortMax |Le numéro de port le plus élevé d’une plage de numéros de ports distants. |
|Profil |Le type de réseau |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths"></a>

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Placer vos fichiers DLL dans n'importe quel dossier du package

Utilisez l’extension [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) pour déclarer jusqu’à cinq chemins de dossier dans le package d’application, relatifs au chemin racine du package d’application, à utiliser dans le chemin de recherche du chargeur pour les processus de l’application.

L’[ordre de recherche de DLL](/windows/win32/dlls/dynamic-link-library-search-order) pour les applications Windows comprend des packages dans le graphe de dépendances de package si les packages ont des droits d’exécution. Par défaut, cela comprend les packages principaux, facultatifs et de framework, même s’ils peuvent être remplacés par l’élément [uap6:AllowExecution](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution) dans le manifeste du package.

Par défaut, un package inclus dans l’ordre de recherche de DLL inclut son *chemin effectif*. Pour plus d’informations sur les chemins effectifs, consultez la propriété [EffectivePath](/uwp/api/windows.applicationmodel.package.effectivepath) (WinRT) et l’énumération [PackagePathType](/windows/win32/api/appmodel/ne-appmodel-packagepathtype) (Win32).

Si un package spécifie [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride), cette information est utilisée à la place du chemin effectif du package.

Chaque package ne peut contenir qu’une seule extension [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride). En d'autres termes, vous pouvez ajouter l'une d'elles à votre package principal, puis en ajouter une à chacun de vos [packages facultatifs et ensembles relatifs](/windows/msix/package/optional-packages).

#### <a name="xml-namespace"></a>espace de noms XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

Déclarez cette extension au niveau du package de votre manifeste d'application.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Nom | Description |
|-------|-------------|
|Category |Toujours ``windows.loaderSearchPathOverride``.
|FolderPath | Chemin du dossier qui contient vos fichiers DLL. Spécifiez un chemin relatif au dossier racine du package. Vous pouvez spécifier un maximum de cinq chemins dans une extension. Si vous souhaitez que le système recherche les fichiers dans le dossier racine du package, utilisez une chaîne vide pour l'un de ces chemins. N’incluez aucun chemin dupliqué, et vérifiez que vos chemins ne commencent ni ne se terminent par aucune barre oblique ou barre oblique inverse. <br><br> Le système ne recherchera pas les sous-dossiers. Veillez donc à répertorier explicitement chacun des dossiers contenant des fichiers DLL que le système doit charger.|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>Intégration avec l’Explorateur de fichiers

Aidez les utilisateurs à organiser vos fichiers et à interagir avec eux naturellement.

* [Définir le comportement de votre application lorsque les utilisateurs sélectionnent et ouvrent plusieurs fichiers en même temps](#define)
* [Afficher le contenu du fichier dans une image miniature dans l’Explorateur de fichiers](#show)
* [Afficher le contenu du fichier dans un volet de visualisation de l’Explorateur de fichiers](#preview)
* [Permettre aux utilisateurs de grouper des fichiers à l’aide de la colonne Type dans l’Explorateur de fichiers](#enable)
* [Mettre à disposition les propriétés du fichier pour la recherche, les index, les boîtes de dialogue des propriétés et le volet d’informations](#make-file-properties)
* [Spécifier un gestionnaire de menu contextuel pour un type de fichier](#context-menu)
* [Permettre l'affichage des fichiers de votre service cloud dans l'explorateur de fichiers](#cloud-files)

<a id="define"></a>

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Définir le comportement de votre application lorsque les utilisateurs sélectionnent et ouvrent plusieurs fichiers en même temps

Spécifier le comportement de votre application lorsqu’un utilisateur ouvre plusieurs fichiers simultanément.

#### <a name="xml-namespaces"></a>espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|MultiSelectModel |Voir ci-dessous |
|FileType |Les extensions de fichier appropriées. |

**MultiSelectModel**

Les applications de bureau empaquetées présentent les trois mêmes options que les applications de bureau standard.

* ``Player`` : Votre application est activée une seule fois. Tous les fichiers sélectionnés sont transmis à votre application en tant que paramètres d’argument.
* ``Single`` : votre application est activée une fois pour le premier fichier sélectionné. Les autres fichiers sont ignorés.
* ``Document`` : une nouvelle instance distincte de votre application est activée pour chaque fichier sélectionné.

 Vous pouvez définir des préférences spécifiques pour les différents types de fichiers et d’actions. Par exemple, il se peut que vous souhaitiez ouvrir les *documents* en mode *Document* et les *images* en mode *Player*.

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Si l’utilisateur ouvre jusqu’à 15 fichiers, le choix par défaut pour l’attribut **MultiSelectModel** est *Player*. Autrement, la valeur par défaut est *Document*. Les applications UWP sont toujours démarrées en mode *Player*.

<a id="show"></a>

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Afficher le contenu du fichier dans une image miniature dans l’Explorateur de fichiers

Permettez aux utilisateurs d’afficher une image miniature du contenu du fichier lorsque l’icône du fichier s’affiche en taille moyenne, grande ou très grande.

#### <a name="xml-namespace"></a>espace de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|FileType |Les extensions de fichier appropriées. |
|Clsid   |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview"></a>

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Afficher le contenu du fichier dans le volet de visualisation de l’Explorateur de fichiers

Permettez aux utilisateurs d’afficher un aperçu du contenu d’un fichier dans le volet de visualisation de l’Explorateur de fichiers.

#### <a name="xml-namespace"></a>espace de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|FileType |Les extensions de fichier appropriées. |
|Clsid   |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable"></a>

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permettre aux utilisateurs de grouper des fichiers à l’aide de la colonne Type dans l’Explorateur de fichiers

Vous pouvez associer une ou plusieurs valeurs prédéfinies pour vos types de fichiers avec le champ **Type**.

Dans l’Explorateur de fichiers, les utilisateurs peuvent regrouper ces fichiers à l’aide de ce champ. Les composants du système utilisent également ce champ à différentes fins, telles que l’indexation.

Pour plus d’informations sur le champ **Type** et les valeurs que vous pouvez utiliser pour ce champ, consultez [Utilisation des noms de type](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|FileType |Les extensions de fichier appropriées. |
|value |Une [valeur de type](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) valide |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="mediafiles">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties"></a>

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Mettre à disposition les propriétés du fichier pour la recherche, les index, les boîtes de dialogue des propriétés et le volet d’informations

#### <a name="xml-namespace"></a>espace de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Nom de l’association de types de fichiers. Vous pouvez utiliser ce nom pour organiser et regrouper des types de fichiers. Le nom doit contenir uniquement des minuscules, sans espace. |
|FileType |Les extensions de fichier appropriées. |
|Clsid  |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="context-menu"></a>

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Spécifier un gestionnaire de menu contextuel pour un type de fichier

Si votre application de bureau définit un [gestionnaire de menu contextuel](/windows/desktop/shell/context-menu-handlers), utilisez cette extension pour enregistrer le gestionnaire de menu.

#### <a name="xml-namespaces"></a>espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

Vous trouverez la référence complète du schéma ici : [com:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) et [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instructions

Pour enregistrer votre gestionnaire de menu contextuel, suivez ces instructions.

1. Dans votre application de bureau, implémentez un [gestionnaire de menu contextuel](/windows/desktop/shell/context-menu-handlers) en implémentant l'interface [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) ou [IExplorerCommandState](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate). Pour obtenir un exemple, consultez l'exemple de code [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb). Veillez à définir un GUID de classe pour chacun de vos objets d’implémentation. Par exemple, le code suivant définit un ID de classe pour une implémentation de [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. Dans le manifeste de votre package, spécifiez une extension d'application [com:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) qui enregistre un serveur de substitution COM avec l'ID de classe de votre implémentation de gestionnaire de menu contextuel.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. Dans le manifeste de votre package, spécifiez une extension d'application [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) qui enregistre votre implémentation de gestionnaire de menu contextuel.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files"></a>

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Permettre l'affichage des fichiers de votre service cloud dans l'explorateur de fichiers

Inscrivez les gestionnaires que vous souhaitez implémenter dans votre application. Vous pouvez également ajouter des options de menu local qui apparaîtront lorsque les utilisateurs cliquent avec le bouton droit sur vos fichiers cloud dans l'explorateur de fichier.

#### <a name="xml-namespace"></a>espace de noms XML

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.cloudfiles``.
|iconResource |L'icône représentant votre service fournisseur de fichier cloud. L'icône apparaît dans le volet Navigation de l'explorateur de fichier.  Les utilisateurs sélectionnent cette icône pour afficher les fichiers provenant de votre service cloud. |
|CustomStateHandler Clsid |L’ID de classe de l’application qui implémente l'élément CustomStateHandler. Le système utilise cet ID de classe pour demander les états et colonnes personnalisés des fichiers cloud. |
|ThumbnailProviderHandler Clsid |L’ID de classe de l’application qui implémente l'élément ThumbnailProviderHandler. Le système utilise cet ID de classe pour demander les images miniatures des fichiers cloud. |
|ExtendedPropertyHandler Clsid |L’ID de classe de l’application qui implémente l'élément ExtendedPropertyHandler.  Le système utilise cet ID de classe pour demander les propriétés étendues d'un fichier cloud. |
|Verbe |Le nom qui apparaît dans le menu local de l'explorateur de fichier pour les fichiers fournis par votre service cloud. |
|Id |L’ID unique du verbe. |

#### <a name="example"></a>Exemple

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start"></a>

## <a name="start-your-application-in-different-ways"></a>Démarrer votre application de différentes manières

* [Démarrer votre application à l’aide d’un protocole](#protocol)
* [Démarrer votre application à l’aide d’un alias](#alias)
* [Démarrer un fichier exécutable lorsque les utilisateurs se connectent à Windows](#executable)
* [Permettre aux utilisateurs de démarrer votre application lorsqu'ils connectent à un appareil à leur PC](#autoplay)
* [Redémarrer automatiquement après avoir reçu une mise à jour à partir du Microsoft Store](#updates)

<a id="protocol"></a>

### <a name="start-your-application-by-using-a-protocol"></a>Démarrer votre application à l’aide d’un protocole

Les associations de protocole permettent l’interopérabilité entre votre application empaquetée et les autres programmes et composants système. Lorsque votre application empaquetée est démarrée à l’aide d’un protocole, vous pouvez spécifier des paramètres spécifiques à transmettre à ses arguments d’événement d’activation afin qu’elle puisse se comporter en conséquence. Les paramètres sont pris en charge uniquement pour les applications empaquetées et de confiance totale. Les applications UWP ne peuvent pas utiliser de paramètres.

#### <a name="xml-namespace"></a>espace de noms XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.protocol``.
|Nom |Nom du protocole. |
|Paramètres |La liste des paramètres et des valeurs à transmettre en tant qu’arguments d’événement à votre application lorsqu’elle est activée. Si une variable peut contenir un chemin d’accès de fichier, placez la valeur du paramètre entre guillemets. Vous éviterez les problèmes qui se produisent dans les cas où le chemin d’accès contient des espaces. |

### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias"></a>

### <a name="start-your-application-by-using-an-alias"></a>Démarrer votre application à l’aide d’un alias

Les utilisateurs et autres processus peuvent utiliser un alias pour démarrer votre application sans avoir à spécifier le chemin d’accès complet à votre application. Vous pouvez spécifier ce nom d’alias.

#### <a name="xml-namespaces"></a>espaces de noms XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<uap3:Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.appExecutionAlias``.
|Exécutable |Le chemin d’accès relatif du fichier exécutable à démarrer lorsque l’alias est appelé. |
|Alias |Le nom court de votre application. Il doit toujours se terminer par l’extension « .exe ». Vous ne pouvez spécifier qu’un seul alias d’exécution d’application pour chaque application du package. Si plusieurs applications présentent le même alias, le système appellera la dernière inscrite. Prenez donc soin de choisir un alias peu susceptible d’être remplacé par d’autres applications.
|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable"></a>

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Démarrer un fichier exécutable lorsque les utilisateurs se connectent à Windows

Les tâches de démarrage permettent à votre application d’exécuter un fichier exécutable automatiquement dès qu’un utilisateur se connecte.

> [!NOTE]
> L’utilisateur doit démarrer votre application au moins une fois pour enregistrer cette tâche de démarrage.

Votre application peut déclarer plusieurs tâches de démarrage. Chaque tâche démarre indépendamment. Toutes les tâches de démarrage apparaissent dans le Gestionnaire des tâches sous l’onglet **Démarrage** avec le nom que vous spécifiez dans le manifeste de votre application et l’icône de votre application. Le Gestionnaire des tâches analyse automatiquement l’impact de vos tâches sur le démarrage.

Les utilisateurs peuvent désactiver manuellement la tâche de démarrage de votre application à l’aide du Gestionnaire des tâches. Si un utilisateur désactive une tâche, vous ne pouvez pas la réactiver par programme.

#### <a name="xml-namespace"></a>espace de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.startupTask``.|
|Exécutable |Le chemin d’accès relatif au fichier exécutable à démarrer. |
|TaskId |Un identificateur unique pour votre tâche. À l’aide de cet identificateur, votre application peut appeler les API de la classe [Windows.ApplicationModel.StartupTask](/uwp/api/Windows.ApplicationModel.StartupTask) pour activer ou désactiver une tâche de démarrage par programmation. |
|activé |Indique si la tâche est activée ou désactivée au démarrage. Les tâches activées seront exécutées la prochaine fois que l’utilisateur se connecte (sauf si celui-ci les désactive). |
|DisplayName |Le nom de la tâche qui s’affiche dans le Gestionnaire des tâches. Vous pouvez localiser cette chaîne à l’aide de ```ms-resource```. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay"></a>

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permettre aux utilisateurs de démarrer votre application lorsqu'ils connectent à un appareil à leur PC

La lecture automatique peut présenter votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC.

#### <a name="xml-namespace"></a>espace de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.autoPlayHandler``.
|ActionDisplayName |Une chaîne représentant l'action que les utilisateurs peuvent choisir avec un appareil qu'ils connectent à un PC (par exemple : « Importer les fichiers » ou « Lire la vidéo »). |
|ProviderDisplayName | Une chaîne qui représente votre application ou service (par exemple : « Lecteur vidéo Contoso »). |
|ContentEvent |Le nom d’un événement de contenu qui envoie une invite aux utilisateurs avec vos éléments ``ActionDisplayName`` et ``ProviderDisplayName``. Les événements de contenu se déclenchent lorsqu’un périphérique de volume, tel que la carte mémoire d’un appareil photo, une clé USB ou un DVD, est inséré dans le PC. Vous trouverez la liste complète de ces événements [ici](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verbe |Le paramètre Verbe identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre Verbe pour déterminer l’option sélectionnée par l’utilisateur pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété verb des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre Verbe, sauf la valeur open qui est réservée. |
|DropTargetHandler |L’ID de classe de l’application qui implémente l'interface [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget). Les fichiers du média amovibles sont transmis à la méthode [Drop](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) de votre implémentation [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget).  |
|Paramètres |Vous n'êtes pas obligé d'implémenter l'interface [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) pour tous les événements de contenu. Pour tous les événements de contenu, vous avez la possibilité de fournir des paramètres de ligne de commande au lieu d'implémenter l'interface [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget). Pour ces événements, la lecture automatique démarre votre application en utilisant ces paramètres de ligne de commande. Vous pouvez analyser ces paramètres dans le code d'initialisation de votre application afin de déterminer s'il a été démarré par la lecture automatique, puis fournir votre implémentation par défaut. |
|DeviceEvent |Le nom d’un événement d'appareil qui envoie une invite aux utilisateurs avec vos éléments ``ActionDisplayName`` et ``ProviderDisplayName``. Un événement d'appareil est déclenché lorsqu’un appareil est connecté au PC. Les événements d'appareil commencent par la chaîne ``WPD``. Ils sont répertoriés [ici](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |L’ID de classe de l’application qui implémente l'interface [IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |
|InitCmdLine |Le paramètre de chaîne que vous souhaitez transmettre dans la méthode [Initialiser](/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) de l'interface [IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |

### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates"></a>

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Redémarrer automatiquement après avoir reçu une mise à jour à partir du Microsoft Store

Si votre application est ouverte lorsque les utilisateurs installent une mise à jour, l’application se ferme.

Si vous souhaitez que cette application redémarre une fois la mise à jour terminée, appelez la fonction [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) dans chaque processus que vous souhaitez redémarrer.

Chaque fenêtre active dans votre application reçoit un message [WM_QUERYENDSESSION](/windows/desktop/Shutdown/wm-queryendsession). À ce stade, votre application peut rappeler la fonction [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) pour mettre à jour la ligne de commande, si nécessaire.

Lorsque chaque fenêtre active dans votre application reçoit le message [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession), votre application doit enregistrer les données et s'arrêter.

>[!NOTE]
> Votre fenêtre active reçoit également le message [WM_CLOSE](/windows/desktop/winmsg/wm-close) au cas où l’application ne gère pas le message [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession).

À ce stade, votre application dispose de 30 secondes pour fermer ses propres processus, sinon la plateforme les arrête de manière forcée.

Une fois la mise à jour terminée, votre application redémarre.

## <a name="work-with-other-applications"></a>Utiliser d’autres applications

Intégration avec d’autres applications, démarrer d’autres processus ou partager des informations.

* [Afficher votre application en tant que cible d’impression dans des applications qui prennent en charge l’impression](#printing)
* [Partager des polices avec d’autres applications Windows](#fonts)
* [Démarrer un processus Win32 à partir d’une application de plateforme Windows universelle (UWP)](#win32-process)

<a id="printing"></a>

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Afficher votre application en tant que cible d’impression dans des applications qui prennent en charge l’impression

Lorsque les utilisateurs souhaitent imprimer des données à partir d’une autre application, telle que le Bloc-notes Windows, vous pouvez afficher votre application comme cible d’impression dans la liste des cibles d’impression disponibles de l’application.

Vous devrez modifier votre application afin qu’elle reçoive les données d’impression au format XML Paper Specification (XPS).

#### <a name="xml-namespaces"></a>espaces de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.appPrinter``.
|DisplayName |Le nom que vous souhaitez voir apparaître dans la liste des cibles d’impression pour une application. |
|Paramètres |Tous les paramètres dont votre application a besoin pour gérer correctement la demande. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

Trouver un exemple qui utilise cette extension [ici](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)

<a id="fonts"></a>

### <a name="share-fonts-with-other-windows-applications"></a>Partager des polices avec d’autres applications Windows

Partagez vos polices personnalisées avec d’autres applications Windows.

> [!NOTE]
> Avant de pouvoir soumettre au Store une application qui utilise cette extension, vous devez obtenir l’approbation de l’équipe du Store. Pour cela, accédez à [https://aka.ms/storesupport](https://aka.ms/storesupport), cliquez sur **Nous contacter**, puis choisissez les options qui concernent la soumission d’applications au tableau de bord. Ce processus d’approbation permet de s’assurer qu’il n’y a pas de conflits entre les polices installées par votre application et celles installées avec le système d’exploitation. Si vous n’obtenez pas d’approbation, vous recevrez une erreur semblable à la suivante quand vous soumettrez votre application : « Erreur de validation de l’acceptation du package : Vous ne pouvez pas utiliser l’extension windows.sharedFonts avec ce compte. Contactez notre équipe du support technique si vous souhaitez demander des autorisations pour l’utiliser. »

#### <a name="xml-namespaces"></a>espaces de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.sharedFonts``.
|Fichier |Le fichier qui contient les polices que vous souhaitez partager. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process"></a>

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Démarrer un processus Win32 à partir d’une application de plateforme Windows universelle (UWP)

Démarrer un processus Win32 qui s’exécute en mode de confiance totale.

#### <a name="xml-namespaces"></a>espaces de noms XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fullTrustProcess``.
|GroupID |Une chaîne qui identifie un ensemble de paramètres que vous souhaitez transmettre au fichier exécutable. |
|Paramètres |Paramètres que vous souhaitez transmettre au fichier exécutable. |

#### <a name="example"></a>Exemple

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Cette extension peut être utile si vous souhaitez créer une interface utilisateur de plateforme Windows universelle qui s’exécute sur tous les appareils, mais que vous souhaitez que les composants de votre application Win32 poursuivent leur exécution en mode de confiance totale.

Il vous suffit de créer un package d’application Windows pour votre application Win32. Ensuite, ajoutez cette extension au fichier de package de votre application UWP. Cette extension indique que vous souhaitez démarrer un fichier exécutable dans le package d’application Windows.  Si vous souhaitez communiquer entre votre application UWP et votre application Win32, vous pouvez configurer un ou plusieurs [services d’application](/windows/uwp/launch-resume/app-services) pour ce faire. Pour en savoir plus sur ce scénario, reportez-vous [ici](/archive/blogs/appconsult/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app).

## <a name="next-steps"></a>Étapes suivantes

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
