---
description: Cet article explique comment utiliser la classe LowLightFusion pour traiter des bitmaps.
title: Traiter les bitmaps avec l’API de fusion faible légère
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, fusion faible légère, bitmaps, traitement d’image
ms.localizationpriority: medium
ms.openlocfilehash: 4e82eb780efe83125a09417f349f84ee9451c1f0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363832"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Traiter des images bitmap avec l’API LowLightFusion

Les images de faible luminosité sont difficiles à capturer avec une qualité d’image correcte, en particulier sur les appareils mobiles avec une ouverture fixe et une taille de capteur. Pour compenser le faible éclairage, les appareils peuvent augmenter le temps d’exposition ou le gain de capteur, ce qui peut entraîner un flou directionnel et un bruit accru dans les images. 

La [classe LowLightFusion](/uwp/api/windows.media.core.lowlightfusion) améliore la qualité des images de faible luminosité en échantillonnant des informations de pixel à partir de plusieurs frames dans une proximité temporelle étroite, c’est-à-dire des images de rafales courtes, afin de réduire le bruit et le flou du mouvement. Il s’agit d’une fonctionnalité utile à ajouter à une application de modification de photos, par exemple.

Cette fonctionnalité est également mise à disposition par le biais de la [classe AdvancedPhotoCapture](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture), qui applique l’algorithme de fusion faible légère à une séquence d’images directement après la capture des images, si nécessaire. Pour découvrir comment implémenter cette fonctionnalité, consultez capture de [photos faible-clair](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture) .

## <a name="prepare-the-images-for-processing"></a>Préparer les images à traiter

Dans cet exemple, nous allons montrer comment utiliser la [classe LowLightFusion](/uwp/api/windows.media.core.lowlightfusion), ainsi que le [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour permettre à un utilisateur de sélectionner plusieurs images sur lesquelles effectuer une fusion faible.

Tout d’abord, nous devrons déterminer le nombre d’images acceptées par l’algorithme et créer une liste pour contenir ces frames.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetMaxLLFFrames":::

Une fois que nous avons déterminé le nombre de trames acceptées par l’algorithme de fusion faible légère, nous pouvons utiliser le [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour permettre à l’utilisateur de choisir les images à utiliser dans l’algorithme.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetFrames":::

Maintenant que nous avons sélectionné le nombre correct de frames, nous devons décoder les images en [SoftwareBitmaps](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) et vérifier que le format de SoftwareBitmaps est correct pour LowLightFusion.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetDecodeFrames":::


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Fusible des bitmaps en une seule bitmap

Maintenant que nous avons un nombre correct de frames dans un format acceptable, nous pouvons utiliser la méthode **[FuseAsync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** pour appliquer l’algorithme de fusion faible légère. Notre résultat est l’image traitée, avec une clarté améliorée, sous la forme d’un SoftwareBitmap. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetFuseFrames":::

Enfin, nous allons nettoyer les SoftwareBitmap résultants en encodant et en les enregistrant dans une image « normale » conviviale, semblable aux images d’entrée que nous avons démarrées avec.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetEncodeFrame":::


## <a name="before-and-after"></a>Avant et après

Voici un exemple d’image d’entrée et l’image de sortie obtenue après l’application de l’algorithme de fusion faible légère.

> [!div class="mx-tableFixed"] 
| Frame d’entrée | Sortie de fusion légère faible | 
|-------------|-------------------------|
| ![Trame d’entrée pour l’algorithme de fusion faible légère](./images/LLF-Input.png) | ![Trame de résultat de l’algorithme de fusion faible légère](./images/LLF-Output.png) |

Vous pouvez voir à partir du frame d’entrée que l’éclairage et la clarté des ombres entourant la bannière ont été améliorés.

## <a name="related-topics"></a>Rubriques connexes 
[LowLightFusion, classe](/uwp/api/windows.media.core.lowlightfusion)  
[LowLightFusionResult, classe](/uwp/api/windows.media.core.lowlightfusionresult)
