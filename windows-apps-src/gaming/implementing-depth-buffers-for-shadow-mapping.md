---
title: Procédure pas à pas--mémoires tampons de profondeur de Volume Shadow de Direct3D 11
description: Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, volumes cachés, mémoires tampons de profondeur, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e2b54f179d61a9479bdb921ef0a68b4c1772c20c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159383"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Procédure pas à pas : implémenter des volumes d’ombre à l’aide de tampons de profondeur dans Direct3D 11



Cette procédure pas à pas montre comment effectuer le rendu de volumes d’ombre (« shadow volumes ») avec des mappages de profondeur, en utilisant Direct3D 11 sur des appareils de tout niveau de fonctionnalité Direct3D.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Créer des ressources de périphérique pour un tampon de profondeur</a></p></td>
<td align="left"><p>Découvrez comment créer les ressources de périphérique Direct3D nécessaires pour prendre en charge le test de profondeur des volumes d’ombre.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Restituer le plan d’ombres dans le tampon</a></p></td>
<td align="left"><p>Générez le rendu du point de vue de la lumière pour créer un mappage de profondeur en deux dimensions qui représente le volume de l’ombre.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Générer le rendu de la scène avec un test de profondeur</a></p></td>
<td align="left"><p>Créez un effet d’ombre en ajoutant un test de profondeur à votre nuanceur de vertex (ou géométrie) et votre nuanceur de pixels.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Prendre en charge les mappages d’ombre sur une gamme de matériel</a></p></td>
<td align="left"><p>Générez le rendu des ombres en haute fidélité sur des appareils plus rapides et celui d’ombres plus rapides sur des appareils moins puissants.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Application du mappage d’ombres pour le portage Direct3D 9


Fonctionnalité de comparaison de profondeur Windows 8 ajoute pour les fonctionnalités de niveau 9 \_ 1 et 9 \_ 3. Vous pouvez désormais transférer du code de rendu contenant des volumes d’ombre sur DirectX 11. Le convertisseur Direct3D 11 peut être utilisé sur les appareils fonctionnant avec des niveaux de fonctionnalité 9 inférieurs. Cette procédure pas à pas montre comment une application ou un jeu Direct3D 11 peut implémenter des volumes d’ombres classiques avec le test de profondeur. Le code couvre le processus suivant :

1.  Création de ressources de périphérique Direct3D pour le mappage d’ombres.
2.  Ajout d’une passe de rendu pour créer le mappage de profondeur.
3.  Ajout du test de profondeur à la passe de rendu principale.
4.  Implémentation du code de nuanceur requis.
5.  Options d’accélération du rendu sur le matériel avec un niveau de fonctionnalité inférieur.

À la fin de cette procédure pas à pas, vous devez vous familiariser avec la procédure d’implémentation d’une technique de Volume Shadow compatible de base dans Direct3D 11 compatible avec les fonctionnalités de niveau 9 \_ 1 et ultérieures.

## <a name="prerequisites"></a>Prérequis


Vous devez [préparer votre environnement au développement de jeux de plateforme Windows universelle (UWP) DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore besoin de modèle, mais vous devez disposer de Microsoft Visual Studio 2015 pour générer l’exemple de code de cette procédure pas à pas.

## <a name="related-topics"></a>Rubriques connexes


**Direct3D**

* [Écriture de nuanceurs HLSL dans Direct3D 9](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Créer un projet DirectX 11 pour UWP](user-interface.md)

**Articles techniques sur le mappage d’ombres**

* [Techniques courantes pour améliorer les mappages de profondeur d’ombre](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [Mappages d’ombres en cascade (CSM)](/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 