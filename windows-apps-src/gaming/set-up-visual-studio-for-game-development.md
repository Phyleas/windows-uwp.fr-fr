---
title: Visual Studio Tools pour la programmation de jeux
description: Découvrez les outils pour la programmation de jeux DirectX disponibles dans Visual Studio, y compris l’éditeur d’images, l’éditeur de modèle et le concepteur de nuanceur.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, Visual Studio, outils, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 250450b2174ce249d1ec5afaf4c5188df9266f5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159263"
---
# <a name="visual-studio-tools-for-game-programming"></a>Visual Studio Tools pour la programmation de jeux



**Résumé**

-   [Créer un projet de jeu DirectX à partir d’un modèle](user-interface.md)
-   Visual Studio Tools pour la programmation de jeux DirectX


Si vous utilisez Visual Studio Ultimate pour développer des applications DirectX, il existe d’autres outils disponibles pour créer, modifier, prévisualiser et exporter des ressources d’image, de modèle et de nuanceur. Il existe également des outils que vous pouvez utiliser pour convertir des ressources au moment de la création et pour déboguer le code graphique DirectX.

Cette rubrique donne une vue d’ensemble de ces fonctionnalités.

## <a name="image-editor"></a>éditeur d’images


Utilisez l’Éditeur d’images pour exploiter les types de formats de texture et d’image enrichis que DirectX utilise. L’Éditeur d’images prend en charge les formats suivants :

-   .png
-   .jpg, .jpeg, .jpe, .jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif, .tiff
-   .tga

Créez des [fichiers de personnalisations de la build](#build-customizations-for-3d-assets) pour les convertir en fichiers .dds au moment de la création.

Pour plus d’informations, voir [Utilisation de textures et d’images](/visualstudio/designers/working-with-textures-and-images?view=vs-2015).

> **Remarque**    L’éditeur d’images n’est pas destiné à remplacer une application de modification d’image de fonctionnalité complète, mais convient à de nombreux scénarios d’affichage et de modification simples.

 

## <a name="model-editor"></a>Éditeur de modèle


Vous pouvez utiliser l’Éditeur de modèles pour créer des modèles en 3D de base ex nihilo ou pour afficher et modifier des modèles en 3D plus complexes à partir d’outils de modélisation en 3D complets. L’éditeur de modèle prend en charge plusieurs formats de modèle 3D qui sont utilisés dans le développement d’applications DirectX. Vous pouvez créer des [fichiers de personnalisations de la build](#build-customizations-for-3d-assets) pour les convertir en fichiers .cmo au moment de la création.

-   .fbx
-   .dae
-   .obj

Voici la capture d’écran d’un modèle dans l’éditeur auquel un éclairage est appliqué.

![Théière](images/modeleditor.png)

Pour plus d’informations, voir [Utilisation de modèles en 3D](/visualstudio/designers/working-with-3-d-models?view=vs-2015).

> **Remarque**    L’éditeur de modèle n’est pas destiné à remplacer une application de modification de modèle de fonctionnalités complète, mais convient à de nombreux scénarios d’affichage et de modification simples.

 

## <a name="shader-designer"></a>Concepteur Shader


Utilisez le Concepteur de nuanceurs pour créer des effets visuels personnalisés pour votre jeu ou application, même si vous ne connaissez pas la programmation HLSL.

Vous créez un nuanceur visuellement sous forme de graphique. Chaque nœud présente un aperçu du résultat jusqu’à la dernière opération. Voici un exemple qui applique un éclairage lambertien avec un aperçu de sphère.

![Graphique de nuanceur visuel](images/shaderdesigner.png)

Utilisez l’Éditeur de nuanceurs pour concevoir, modifier et enregistrer des nuanceurs au format .dgsl. Il permet également d’exporter les formats suivants.

-   .hlsl (code source)
-   .cso (bytecode)
-   .h (tableau de bytecode HLSL)

Créez des [fichiers de personnalisations de la build](#build-customizations-for-3d-assets) pour convertir ces formats en fichiers .cso au moment de la création.

Voici une partie du code HLSL exporté par l’Éditeur de nuanceurs. Il s’agit uniquement du code pour le nœud de l’éclairage lambertien.

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

Pour plus d’informations, voir [Utilisation de nuanceurs](/visualstudio/designers/working-with-shaders?view=vs-2015).

## <a name="build-customizations-for-3d-assets"></a>Personnalisations de la build pour les ressources 3D


Vous pouvez ajouter des personnalisations de la build à votre projet afin que Visual Studio convertisse les ressources en formats utilisables. Ensuite, vous pouvez charger les ressources dans votre application et les utiliser en créant et complétant des ressources DirectX comme dans n'importe quelle autre application DirectX.

Pour ajouter une personnalisation de build, cliquez avec le bouton droit sur le projet dans le **Explorateur de solutions** puis sélectionnez **personnalisations de la Build...**. Vous pouvez ajouter les types de personnalisations de build suivants à votre projet.

-   Le pipeline de contenu d’image prend les fichiers image en tant qu’entrée et génère des fichiers DirectDraw Surface (.dds).
-   Le pipeline de contenu de maillage prend les fichiers de maillage (notamment .fbx) et génère des fichiers de maillage .cmo.
-   Le pipeline de contenu de nuanceur prend les fichiers Visual Shader Graph (.dgsl) de l’Éditeur de nuanceurs Visual Studio et génère un fichier .cso (Compiled Shader Output).

Pour plus d’informations, voir [Utilisation de ressources 3D dans vos jeux et applications](/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015).

## <a name="debugging-directx-graphics"></a>Débogage de graphiques DirectX


Visual Studio fournit des outils de débogage propres aux graphiques. Utilisez ces outils pour déboguer les éléments suivants :

-   Le pipeline graphique.
-   La pile d’appels d’événements.
-   La table des objets.
-   L’état du périphérique.
-   Les bogues de nuanceur.
-   Les tampons constants et paramètres non initialisés ou incorrects.
-   La compatibilité de la version DirectX.
-   La prise en charge limitée de Direct2D.
-   Les exigences en matière de système d’exploitation et de SDK.

Pour plus d’informations, voir [Débogage de graphiques DirectX](/visualstudio/debugger/visual-studio-graphics-diagnostics?view=vs-2015).


 

 

 