---
title: Distribution d’un composant Windows Runtime managé
description: Vous pouvez distribuer votre composant Windows Runtime par copie de fichiers.
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690383"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>Distribution d’un composant Windows Runtime managé

Vous pouvez distribuer votre composant Windows Runtime par copie de fichiers. Toutefois, si votre composant comprend de nombreux fichiers, l’installation peut être fastidieuse pour vos utilisateurs. En outre, les erreurs de mise en place de fichiers ou d’échec de définition de références peuvent provoquer des problèmes. Vous pouvez empaqueter un composant complexe comme un kit de développement logiciel (SDK) d’extension Visual Studio, pour faciliter son installation et son utilisation. Les utilisateurs n’ont besoin de définir qu’une seule référence pour l’ensemble du package. Ils peuvent facilement localiser et installer votre composant à l’aide de la boîte de dialogue **extensions et mises à jour** , comme décrit dans [recherche et utilisation des extensions Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015).

## <a name="planning-a-distributable-windows-runtime-component"></a>Planification d’un composant de Windows Runtime distribuable

Choisissez des noms uniques pour les fichiers binaires, tels que les fichiers. winmd. Nous vous recommandons d’utiliser le format suivant pour garantir l’unicité :

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

Vos fichiers binaires seront installés dans des packages d’application, éventuellement avec des fichiers binaires d’autres développeurs. Consultez « SDK d’extension » dans Guide pratique [pour créer un kit de développement logiciel](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)(SDK).

Pour décider comment distribuer votre composant, réfléchissez à son degré de complexité. Un SDK d’extension ou un gestionnaire de package similaire est recommandé dans les cas suivants :

-   Votre composant est constitué de plusieurs fichiers.
-   Vous fournissez des versions de votre composant pour plusieurs plateformes (x86 et ARM, par exemple).
-   Vous fournissez à la fois les versions Debug et Release de votre composant.
-   Votre composant contient des fichiers et des assemblys qui sont utilisés uniquement au moment du Design.

Un kit de développement logiciel (SDK) d’extension est particulièrement utile si plusieurs des éléments ci-dessus sont vrais.

> **Notez**  pour les composants complexes, le système de gestion des packages NuGet offre une alternative open source aux kits de développement logiciel (SDK) d’extension. Comme les kits de développement logiciel (SDK) d’extension, NuGet vous permet de créer des packages qui simplifient l’installation de composants complexes. Pour une comparaison des packages NuGet et des kits de développement logiciel (SDK) d’extension Visual Studio, consultez [Ajout de références à l’aide de NuGet et d’un SDK d’extension](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015).

## <a name="distribution-by-file-copy"></a>Distribution par copie de fichiers

Si votre composant se compose d’un fichier. winmd unique ou d’un fichier. winmd et d’un fichier d’index de ressources (. PRI), vous pouvez simplement mettre le fichier. winmd à la disposition des utilisateurs à copier. Les utilisateurs peuvent placer le fichier à l’emplacement de leur choix dans un projet, utiliser la boîte de dialogue **Ajouter un élément existant** pour ajouter le fichier. winmd au projet, puis utiliser la boîte de dialogue Gestionnaire de références pour créer une référence. Si vous incluez un fichier. pri ou un fichier. xml, demandez aux utilisateurs de placer ces fichiers avec le fichier. winmd.

> **Notez**  Visual Studio produit toujours un fichier. pri lorsque vous générez votre composant Windows Runtime, même si votre projet n’inclut aucune ressource. Si vous disposez d’une application de test pour votre composant, vous pouvez déterminer si le fichier. pri est utilisé en examinant le contenu du package d’application dans le dossier bin\\Debug\\AppX. Si le fichier. pri de votre composant n’apparaît pas, vous n’avez pas besoin de le distribuer. Vous pouvez également utiliser l’outil [MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10)) pour vider le fichier de ressources à partir de votre projet de composant Windows Runtime. Par exemple, dans la fenêtre d’invite de commandes Visual Studio, tapez : makepri dump/if MyComponent. pri/of MyComponent. pri. XML vous pouvez en savoir plus sur les fichiers. pri dans le [système de gestion des ressources (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="distribution-by-extension-sdk"></a>Distribution par extension SDK

Un composant complexe comprend généralement des ressources Windows, mais consultez la remarque sur la détection des fichiers. pri vides dans la section précédente.

**Pour créer un kit de développement logiciel (SDK) d’extension**

1.  Vérifiez que vous avez installé le kit de développement logiciel (SDK) Visual Studio. Vous pouvez télécharger le kit de développement logiciel (SDK) Visual Studio à partir de la page de [téléchargements Visual Studio](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs) .
2.  Créez un projet à l’aide du modèle de projet VSIX. Vous pouvez trouver le modèle sous Visual C# ou Visual Basic, dans la catégorie extensibilité. Ce modèle est installé dans le cadre du kit de développement logiciel (SDK) Visual Studio. ([Procédure pas à pas : création C# d’un kit de développement logiciel à l’aide de ou Visual Basic](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015) ou [procédure pas à pas : création d’un kit de développement logiciel à l’aide C++ ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)de, illustre l’utilisation de ce modèle dans un scénario très )
3.  Déterminez la structure de dossiers pour votre kit de développement logiciel (SDK). La structure de dossiers commence au niveau racine de votre projet VSIX, avec les dossiers **References**, **Redist**et **designtime** .

    -   **References** est l’emplacement des fichiers binaires que vos utilisateurs peuvent programmer. Le kit de développement logiciel (SDK) d’extension crée des références à ces fichiers dans les projets Visual Studio de vos utilisateurs.
    -   **Redist** est l’emplacement des autres fichiers qui doivent être distribués avec vos fichiers binaires dans les applications créées à l’aide de votre composant.
    -   **Designtime** est l’emplacement des fichiers qui sont utilisés uniquement lorsque les développeurs créent des applications qui utilisent votre composant.

    Dans chacun de ces dossiers, vous pouvez créer des dossiers de configuration. Les noms autorisés sont Debug, Retail et CommonConfiguration. Le dossier CommonConfiguration est destiné aux fichiers qui sont identiques, qu’ils soient utilisés par les versions commerciales ou de débogage. Si vous distribuez uniquement des versions commerciales de votre composant, vous pouvez tout placer dans CommonConfiguration et omettre les deux autres dossiers.

    Dans chaque dossier de configuration, vous pouvez fournir des dossiers d’architecture pour les fichiers spécifiques à la plateforme. Si vous utilisez les mêmes fichiers pour toutes les plateformes, vous pouvez fournir un seul dossier nommé Neutral. Vous pouvez trouver des informations sur la structure des dossiers, notamment d’autres noms de dossiers d’architecture, dans [procédure : créer un kit de développement logiciel (SDK](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)). (Cet article aborde les kits de développement logiciel de plateforme et les Kits SDK d’extension. Il peut s’avérer utile de réduire la section sur les kits de développement logiciel (SDK) de plateforme, afin d’éviter toute confusion. )

4.  Créez un fichier manifeste du SDK. Le manifeste spécifie les informations de nom et de version, les architectures prises en charge par votre kit de développement logiciel (SDK), les versions .NET et d’autres informations sur la façon dont Visual Studio utilise votre SDK. Vous trouverez plus d’informations et un exemple dans [procédure : créer un kit de développement logiciel (SDK](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)).
5.  Créez et distribuez le kit de développement logiciel (SDK) d’extension. Pour obtenir des informations détaillées, notamment la localisation et la signature du package VSIX, consultez [déploiement VSIX](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015).

## <a name="related-topics"></a>Rubriques connexes

* [Création d’un kit de développement logiciel](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [système de gestion des packages NuGet](https://github.com/NuGet/Home)
* [Système de gestion des ressources (Windows)](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [Recherche et utilisation des extensions Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Options de commande MakePRI. exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
