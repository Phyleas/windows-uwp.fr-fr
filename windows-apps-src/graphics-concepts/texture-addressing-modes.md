---
title: Modes d’adressage de texture
description: Votre application Direct3D peut attribuer des coordonnées de texture à tous les sommets d’une primitive.
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- Modes d’adressage de texture
- Mode d’adresse de texture Wrap
- Mode d’adresse de texture Mirror
- Mode d’adresse de texture Clamp
- Mode d’adresse de texture Border Color
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5e263876f414e5683ffc8a5645a12e5031b3d6fb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660184"
---
# <a name="texture-addressing-modes"></a>Modes d’adressage de texture


Votre application Direct3D peut attribuer des coordonnées de texture à tous les sommets d’une primitive. Les coordonnées de texture u et v que vous attribuez à un sommet sont généralement comprises entre 0,0 à 1,0 inclus. Si vous attribuez des coordonnées de texture en dehors de cette plage, vous pouvez créer des effets de texture spéciaux. .

Vous contrôlez ce que fait Direct3D avec les coordonnées de texture situés en dehors de la \[0.0, 1.0\] plage en définissant le mode d’adressage de texture. Par exemple, votre application peut définir le mode d’adressage de texture afin qu’une texture s’affiche sous forme de tuiles sur une primitive.

Direct3D permet aux applications d’effectuer un habillage de texture. Voir [Habillage de texture](texture-wrapping.md).

L’activation de texture encapsulant efficacement rend les coordonnées de texture en dehors de la \[0.0, 1.0\] plage non valide et le comportement de rastérisation ces coordonnées de texture impayée n’est pas défini dans ce cas. Lorsque l’habillage de texture est activé, les modes d’adressage de texture ne sont pas utilisés. Assurez-vous que votre application n’indique pas de coordonnées de texture inférieures à 0.0 ou supérieures à 1.0 lorsque habillage de texture est activé.

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>Résumé de la texture modes d’adressage


| Mode d’adressage de texture | Description                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Wrap                    | Répète la texture sur chaque jonction entière.                                                                                        |
| Mirror                  | Applique la texture en miroir à chaque limite entière.                                                                                        |
| Clamp                   | PINCES votre texture en coordonnées le \[0.0, 1.0\] plage ; Mode de pince s’applique la texture à une seule fois, puis se la couleur des pixels de contour. |
| Couleur de bordure            | Utilise une *couleur de bordure* arbitraire pour les coordonnées de texture en dehors de la plage 0.0 à 1.0 inclus.                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>Encapsuler le mode d’adresse de texture


Avec le mode d’adresse de texture Wrap, Direct3D répète la texture sur chaque jonction entière.

Par exemple, supposons que votre application crée une primitive carrée et spécifie des coordonnées de texture de (0.0, 0.0), (0.0, 3.0), (3.0, 3.0) et (3.0, 0.0). La définition du mode d’adressage de texture sur « Wrap » permet d’appliquer trois fois la texture dans les directions u et v, comme indiqué dans l’illustration suivante.

![Illustration d’une texture en mode « Wrap » dans la direction de u et la direction v](images/wrap.png)

Comparez-la avec le **mode d’adresse de texture Mirror** ci-dessous.

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>Mode d’adresse de texture de miroir


Avec le mode d’adresse de texture Mirror, Direct3D applique la texture en miroir à chaque limite entière.

Par exemple, supposons que votre application crée une primitive carrée et spécifie des coordonnées de texture de (0.0, 0.0), (0.0, 3.0), (3.0, 3.0) et (3.0, 0.0). La définition du mode d’adressage de texture sur « Miroir » permet d’appliquer trois fois la texture dans les directions u et v. Toutes les autres lignes et colonnes auxquelles la texture est appliquée sont une image en miroir de la ligne ou de la colonne précédente, comme illustré.

![Illustration des images mises en miroir dans une grille 3 x 3](images/mirror.png)

Comparez ceci avec le **mode d’adresse de texture Wrap** précédent.

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>Mode d’adresse de texture de serrage


Le mode d’adresse de texture Clamp provoque Direct3D à fixer vos coordonnées de texture à le \[0.0, 1.0\] plage ; Mode de pince s’applique la texture à une seule fois, puis se la couleur des pixels de contour.

Supposons que votre application crée une primitive carrée et assigne des coordonnées de texture de (0.0, 0.0), (0.0, 3.0), (3.0, 3.0), et (3.0, 0.0) aux sommets de la primitive. La définition du mode d’adressage de texture sur « Clamp » permet d’appliquer la texture une fois. Les couleurs des pixels en haut des colonnes et à la fin des lignes sont étendues vers le haut et la droite de la primitive respectivement.

L’illustration suivante indique une texture en mode « Clamp ».

![illustration d’une texture et d’une texture en mode « Clamp »](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>Mode d’adresse de texture de couleur de bordure


Avec le mode d’adresse de texture Border Color, Direct3D utilise une couleur arbitraire, appelée la *couleur de bordure*, pour les coordonnées de texture en dehors de la plage 0.0 et 1.0 inclus.

Dans l’illustration suivante, l’application spécifie que la texture est appliquée à la primitive à l’aide d’une bordure rouge.

![Illustration d’une texture et d’une texture avec une bordure rouge](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>Limitations de l’appareil


Même si le système permet généralement d’utiliser des coordonnées de texture en dehors de la plage 0.0 et 1.0 inclus, des limitations matérielles affectent souvent la plage de dépassement possible des coordonnées de texture. Un appareil de rendu indique la limite de la plage complète de coordonnées de texture autorisée par l’appareil, lorsque vous récupérez les fonctions de l’appareil.

Par exemple, si cette valeur est de 128, alors les coordonnées de texture d’entrée doivent être comprises dans la plage -128.0 à +128.0. Le passage de sommets avec des coordonnées de texture en dehors de cette plage n’est pas valide. La même restriction s’applique aux coordonnées de texture générées automatiquement et transformées.

Les limitations de texture répétées peuvent dépendre de la taille de la texture indexée par les coordonnées de texture. Dans ce cas, si la dimension de texture est 32 et que la plage de coordonnées de texture autorisée par l’appareil est de 512, la plage de coordonnées de texture valide réelle serait de 512 / 32 = 16, de sorte que les coordonnées de texture pour cet appareil doivent être comprises dans la plage -16.0 à +16.0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 




