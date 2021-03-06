---
title: 'Unity : Gestion de version de votre projet UWP'
description: Découvrez comment utiliser le contrôle de version avec un jeu Unity pour Xbox à l’aide de la plateforme Windows universelle (UWP).
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 67c4ec927d83ecba1257eb73fec451e3281333b2
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254164"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity : Gestion de version de votre projet UWP

Vous n’avez pas encore créé votre jeu Unity pour Xbox à l’aide de la plateforme Windows universelle (UWP) ?  Commencez par lire [Intégration de jeux Unity dans UWP sur Xbox](development-lanes-unity.md).

Plusieurs raisons peuvent motiver votre volonté d’ajouter des parties de votre répertoire UWP généré à la gestion de version, notamment l’ajout de dépendances (par exemple, le Kit SDK Xbox Live).  Nous allons utiliser ce scénario comme exemple dans le cadre de ce didacticiel, et nous espérons qu’il vous sera utile pour répondre aux besoins spécifiques de votre projet.

***Exclusion de responsabilité : nous allons utiliser Git comme solution de contrôle de version.  Si les vôtres diffèrent, les concepts doivent tout de même être traduits.** _

Pour actualiser votre mémoire, il s’agit de l’annuaire de notre jeu, _*_ScrapyardPhoenix_*_, qui ressemble actuellement à ce qui suit :

![Dossier de destination de la build](images/build-destination.png)

Et voici à quoi ressemble notre répertoire UWP :

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

Dans ce répertoire, nous ne sommes inquiets que d’un seul dossier, le dossier _*_ScrapyardPhoenix_*_ (insérer le nom de votre jeu ici).  Tous les autres éléments peuvent être ignorés dans notre gestion de version.

_*_Vous ne connaissez pas ce qu’est un fichier. gitignore ?  Consultez [gitignore](https://git-scm.com/docs/gitignore)._*_

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep... (this line will be modified and removed further down)
!/UWP/ScrapyardPhoenix/
```

Nous allons sélectionner quelques fichiers et dossiers différents dans le dossier **UWP/ScrapyardPhoenix** à ajouter à notre gestion de version.  Tout d’abord, examinons la chose dans son intégralité et en détail :

![Répertoire des builds UWP](images/uwp-build-directory.png)  

## <a name="folders"></a>Dossiers  

| Nom de dossier | Paramètre | Description |
|-------------|---------|-------------|
| `Assets` | **_Include_* _ | Contient des images Microsoft Store |
| `Data` | _*_Ignorer_*_ | Où Unity compile votre projet en (scènes, nuanceurs, scripts, Prefabs, etc.) |
| `Dependencies` | _*_Inclusion_*_ | Il s’agit d’un dossier que j’ai créé pour conserver toutes les dépendances UWP dans (par exemple, XboxLiveSDK.dll) |
| `Properties` | _*_Inclusion_*_ | Contient des paramètres plus avancés qui peuvent être modifiés par le développeur |
| `Unprocessed` | _*_Ignorer_*_ | Contient Unity `.dll` et des `.pdb` fichiers |

## <a name="files"></a>Fichiers  

| Nom de dossier | Paramètre | Description |
|-------------|---------|-------------|
| `App.cs` | _*_Inclure_*_ | Point d’entrée pour votre application UWP ; Cela peut être modifié et étendu avec d’autres fichiers sources. |
| `Package.appxmanifest` | _*_Inclusion_*_ | Fichier source du manifeste du package d’application pour votre package. msix ou. AppX |
| `project.json` | _*_Inclusion_*_ | Décrit les packages NuGet `_.csproj` dont dépend votre dépend |
| `ScrapyardPhoenix.csproj` | ***Include** _ | Décrit votre cible de génération UWP ; Si vous ajoutez des dépendances supplémentaires à votre projet UWP, ce `_.csproj` fichier contient ces informations. |
| `ScrapyardPhoenix.csproj.user` | ***Ignorer** _ | Ce fichier contient des informations sur l’utilisateur local |

## <a name="resulting-gitignore"></a>Fichier .gitignore obtenu

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep...
!/UWP/ScrapyardPhoenix/Assets/*
!/UWP/ScrapyardPhoenix/Dependencies/*
!/UWP/ScrapyardPhoenix/Properties/*
!/UWP/ScrapyardPhoenix/App.cs
!/UWP/ScrapyardPhoenix/Package.appxmanifest
!/UWP/ScrapyardPhoenix/project.json
!/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj
```

Et voilà, maintenant vos coéquipiers seront synchronisés avec le projet UWP que vous avez généré. Maintenant, n’hésitez pas à ajouter des ressources, sources et dépendances supplémentaires à votre projet UWP.

Quelques exemples supplémentaires de gestion de version du dossier UWP sont disponibles [ici](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## <a name="adding-dependencies-to-your-uwp-app"></a>Ajout de dépendances à votre application UWP

Ajoutez des dépendances à des DLL et des WINMD en les plaçant dans votre dossier **Unity Assets** sous un dossier **Plugins**, puis sélectionnez-les et définissez correctement les paramètres de leur plateforme cible dans l’inspecteur.

![Solution UWP](images/uwp-solution.PNG)

**_ScrapyardPhoenix (Windows universel)_** est le projet auquel vous ajoutez une référence, par exemple, le kit de développement logiciel (SDK) Xbox Live.

## <a name="see-also"></a>Voir aussi

- [Intégration de jeux existants dans Xbox](development-lanes-landing.md)
- [UWP sur Xbox One](index.md)
