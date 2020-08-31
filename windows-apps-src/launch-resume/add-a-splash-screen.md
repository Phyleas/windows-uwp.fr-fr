---
title: Ajouter un écran de démarrage
description: Définissez l’image de l’écran de démarrage de votre application et la couleur d’arrière-plan à l’aide de Microsoft Visual Studio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06f9d412976ab9ccd5af5c8108bd5632792e4112
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167913"
---
# <a name="add-a-splash-screen"></a>Ajouter un écran de démarrage

Définissez l’image de l’écran de démarrage de votre application et la couleur d’arrière-plan à l’aide de Microsoft Visual Studio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Définir l’image de l’écran de démarrage et la couleur d’arrière-plan dans Visual Studio

Quand vous utilisez un modèle Visual Studio pour créer votre application, une image par défaut est ajoutée à votre projet et définie comme image de l’écran de démarrage. Par défaut, la couleur d’arrière-plan de votre écran de démarrage est gris clair. Pour modifier l’image ou la couleur par défaut de l’écran de démarrage de votre application, procédez comme suit :

1. Ouvrez votre projet d’application plateforme Windows universelle (UWP) existant dans Visual Studio.
2. Dans l’**Explorateur de solutions**, ouvrez le fichier Package.appxmanifest. Vous pouvez également ouvrir ce fichier à partir de la barre de menus en choisissant **Projet** &gt; **Windows Store** &gt; **Modifier le manifeste d’application**.
3. Ouvrez l’onglet **ressources visuelles** et sélectionnez **écran de démarrage** dans le volet **toutes les ressources visuelles** à gauche de la fenêtre « package. appxmanifest ». Si vous modifiez l’écran de démarrage pour la première fois, le chemin « ressources \\SplashScreen.png » s’affiche dans le champ de l' **écran de démarrage** .

    La capture d’écran suivante montre la fenêtre « package. appxmanifest » dans Visual Studio. Selon le type de projet, l’ensemble des ressources visuelles peut différer légèrement.

    ![capture d’écran de la fenêtre « package. appxmanifest » dans Visual Studio 2019](images/appmanifest.png)

    Si vous ouvrez « package. appxmanifest » dans un éditeur de texte, l' [**élément SplashScreen**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) apparaît en tant qu’enfant de l' [**élément VisualElements**](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements). Le balisage de l’écran de démarrage par défaut dans le fichier manifeste ressemble à ceci dans un éditeur de texte :

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Pour sélectionner une nouvelle image d’écran de démarrage pour une application UWP, appuyez sur le bouton comportant des points de suspension, qui s’affiche à côté de l’étiquette **1240 x 600 px** sous **Composants mis à l’échelle**. Choisissez l’image de 1240 x 600 pixels (.png, .jpg ou .jpeg) que vous voulez utiliser comme image d’écran de démarrage.

    **Important**    L’image de l’écran de démarrage que vous choisissez doit être 620 x 300 pixels à l’aide d’un facteur de mise à l’échelle 1x. Lorsque vous concevez votre écran de démarrage, notez également qu’il est inférieur à l’écran et centré. Il ne remplit pas l’écran comme l’écran de démarrage d’une application du Windows Phone Store.

5. Pour sélectionner une nouvelle image d’écran de démarrage pour une application du Windows Phone Store, appuyez sur le bouton comportant des points de suspension, qui s’affiche à côté de l’étiquette **1152 x 1920 px** sous **Composants mis à l’échelle**. Choisissez l’image de 1 152 x 1 920 pixels (.png, .jpg ou .jpeg) que vous voulez utiliser comme image d’écran de démarrage.

    **Important**    L’image de l’écran de démarrage que vous choisissez doit être de 1152 x 1920 pixels, ce qui correspond à la taille correcte d’un facteur de mise à l’échelle de 2,4 x. Si c’est la seule ressource que vous fournissez, elle sera réduite pour les facteurs d’échelle de 1,4x et 1x.

6. Dans le champ **Couleur d’arrière-plan** de la section **Écran de démarrage**, définissez la couleur d’arrière-plan affichée avec l’image d’écran de démarrage. Vous pouvez entrer le nom d’une couleur ou « \# » et la valeur hexadécimale d’une couleur. Pour obtenir la liste des noms des couleurs disponibles, consultez l' [**élément SplashScreen**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen). La définition d’une couleur d’arrière-plan d’écran de démarrage est facultative. Si vous ne spécifiez pas de couleur pour une application UWP, la couleur d’arrière-plan de l’écran de démarrage est définie par défaut sur un gris clair (valeur hexadécimale \# 464646). Il s’agit de la même couleur que la couleur d’arrière-plan par défaut de la **vignette** (voir le champ **Couleur d’arrière-plan** de la section **Mosaïque et logos** de l’onglet **Ressources visuelles**). Si vous ne spécifiez pas de couleur pour un Windows Phone ou si vous la définissez sur « transparent », la couleur d’arrière-plan de l’écran de démarrage sera transparente.

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes

Si votre application met un certain temps à se charger, ajoutez un écran de démarrage étendu. Pour obtenir des instructions pas à pas, consultez [créer un écran de démarrage personnalisé](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Rubriques connexes

* [Créer un écran de démarrage personnalisé](create-a-customized-splash-screen.md)
* [Référence du schéma de manifeste de package : élément SplashScreen](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Classe Windows.ApplicationModel.Activation.SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)