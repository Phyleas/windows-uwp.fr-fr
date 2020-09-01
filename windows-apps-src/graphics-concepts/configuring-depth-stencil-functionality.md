---
title: Configuration de la fonctionnalité de profondeur-gabarit
description: Cette section décrit les étapes de configuration de la mémoire tampon du stencil de profondeur et l’état du gabarit de profondeur pour l’étape de fusion de sortie.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Configuration de la fonctionnalité de profondeur-gabarit
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2d9c33e9625f36bbf183df46d5cd590f9e66d3c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162803"
---
# <a name="span-iddirect3dconceptsconfiguring_depth-stencil_functionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Configuration de la fonctionnalité de profondeur-gabarit


Cette section décrit les étapes de configuration de la mémoire tampon du stencil de profondeur et l’état du gabarit de profondeur pour l’étape de fusion de sortie.

Une fois que vous savez comment utiliser la mémoire tampon du stencil de profondeur et l’état du gabarit de profondeur correspondant, reportez-vous à [techniques de stencil avancées](#advanced-stencil-techniques).

## <a name="span-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Créer une profondeur-État du stencil


L’état de gabarit de profondeur indique à l’étape de fusion de sortie comment effectuer le [test de stencil de profondeur](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). Le test de stencil de profondeur détermine si un pixel donné doit être dessiné ou non.

## <a name="span-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Lier la profondeur-données du stencil à l’étape OM


Liez l’état de gabarit de profondeur.

Liez la ressource de stencil de profondeur à l’aide d’une vue.

Les cibles de rendu doivent toutes être du même type de ressource. Si l’anticrénelage est utilisé, toutes les cibles de rendu liées et les tampons de profondeur doivent avoir le même nombre d’échantillons.

Quand une mémoire tampon est utilisée comme cible de rendu, le test du stencil de profondeur et les cibles de rendu multiples ne sont pas pris en charge.

-   Jusqu’à 8 cibles de rendu peuvent être liées simultanément.
-   Toutes les cibles de rendu doivent avoir la même taille dans toutes les dimensions (largeur et hauteur, et profondeur pour la taille 3D ou de tableau pour les \* types tableau).
-   Chaque cible de rendu peut avoir un format de données différent.
-   Les masques d’écriture contrôlent les données qui sont écrites dans une cible de rendu. L’écriture de sortie masque le contrôle sur une cible par rendu, au niveau de chaque composant, quelles données sont écrites dans la ou les cibles de rendu.

## <a name="span-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Techniques avancées de stencil


La partie stencil de la mémoire tampon du stencil de profondeur peut être utilisée pour créer des effets de rendu tels que la composition, le décalque et le mode plan.

-   [Composition](#compositing)
-   [Décalque](#decaling)
-   [Plans et silhouettes](#outlines-and-silhouettes)
-   [Stencil recto-verso](#two-sided-stencil)
-   [Lecture de la mémoire tampon du stencil de profondeur comme une texture](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composition

Votre application peut utiliser la mémoire tampon de stencil pour les images 2D ou 3D composite sur une scène 3D. Un masque dans le tampon de stencil est utilisé pour occultait une zone de la surface cible de rendu. Des informations 2D stockées, telles que du texte ou des bitmaps, peuvent ensuite être écrites dans la zone bloqués. Votre application peut également restituer des primitives 3D supplémentaires dans la région masquée du stencil de la surface cible de rendu. Il peut même rendre une scène entière.

Les jeux composent souvent plusieurs scènes 3D ensemble. Par exemple, la conduite des jeux affiche généralement un miroir en mode arrière. Le miroir contient la vue de la scène 3D derrière le pilote. Il s’agit essentiellement d’une deuxième scène 3D composite avec la vue d’avance du pilote.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Décalque

Les applications Direct3D utilisent le décalqueing pour contrôler les pixels d’une image primitive particulière qui sont dessinés sur la surface cible de rendu. Les applications appliquent des dépassements aux images de primitives pour permettre l’affichage correct de polygones polygones.

Par exemple, lorsque vous appliquez des marques de pneu et des lignes jaunes à une chaussée, les marquages doivent apparaître directement en haut de la route. Toutefois, les valeurs z des marquages et de la route sont les mêmes. Par conséquent, il se peut que la mémoire tampon de profondeur ne produise pas une séparation nette entre les deux. Certains pixels de la primitive arrière peuvent être rendus au-dessus de la primitive avant et vice versa. L’image résultante semble en scintillement du cadre au frame. Cet effet est appelé z-combat ou flimmering.

Pour résoudre ce problème, utilisez un gabarit pour masquer la section de la primitive de retour où le décalcomanie s’affichera. Désactivez la mise en mémoire tampon z et affichez l’image de la primitive avant dans la zone masquée de la surface de rendu-cible.

Plusieurs fusions de texture peuvent être utilisées pour résoudre ce problème.

### <a name="span-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Plans et silhouettes

Vous pouvez utiliser la mémoire tampon de stencil pour obtenir des effets plus abstraits, tels que le mode plan et silhouetting.

Si votre application effectue deux passes de rendu, une pour générer le masque de stencil et la seconde pour appliquer le masque de stencil à l’image, mais avec les primitives légèrement plus petites sur la deuxième passe, l’image résultante contient uniquement le contour de la primitive. L’application peut ensuite remplir la zone masquée du gabarit de l’image avec une couleur unie, ce qui donne à la primitive un aspect en relief.

Si la taille et la forme du masque du gabarit sont identiques à celles de la primitive que vous affichez, l’image résultante contient un trou où la primitive doit être. Votre application peut alors remplir le trou avec du noir pour produire une silhouette de la primitive.

### <a name="span-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo-sided-stenciltwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span><span id="two-sided-stencil">Stencil recto-verso

Les volumes de clichés instantanés permettent de dessiner des ombres avec la mémoire tampon de stencil. L’application calcule les volumes de clichés instantanés convertis par la géométrie de la Boucher, en calculant les bords silhouettes et en les séparant de la lumière dans un ensemble de volumes 3D. Ces volumes sont ensuite restitués deux fois dans la mémoire tampon du stencil.

Le premier rendu dessine des polygones vers l’avant et incrémente les valeurs de mémoire tampon de stencil. Le deuxième rendu dessine les polygones de l’arrière-plan du volume de cliché instantané et décrémente les valeurs de la mémoire tampon du stencil.

Normalement, toutes les valeurs incrémentées et décrémentées s’annulent les unes des autres. Toutefois, la scène était déjà rendue avec une géométrie normale provoquant l’échec du test de la mémoire tampon z pour certains pixels, car le volume de cliché instantané est rendu. Les valeurs laissées dans la mémoire tampon du stencil correspondent aux pixels qui se trouvent dans l’ombre. Ces contenus de tampons de stencil restants sont utilisés comme masque, afin de mélanger en 3D un grand nombre de cœurs noirs englobant tous dans la scène. Avec la mémoire tampon de stencil jouant le rôle de masque, le résultat est d’assombrir les pixels qui se trouvent dans les ombres.

Cela signifie que la géométrie de l’ombre est dessinée deux fois par source de lumière, ce qui entraîne une pression sur le débit du vertex du GPU. La fonctionnalité de stencil à deux côtés a été conçue pour atténuer cette situation. Dans cette approche, il existe deux ensembles de l’état du gabarit (nommés ci-dessous), un jeu pour les triangles frontaux et l’autre pour les triangles de face arrière. De cette façon, une seule passe est dessinée par volume de cliché instantané, par lumière.

### <a name="span-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Lecture de la mémoire tampon du stencil de profondeur comme une texture

Une mémoire tampon de stencil de profondeur inactive peut être lue par un nuanceur comme une texture. Une application qui lit une mémoire tampon de stencil de profondeur comme une texture est rendue en deux passes, la première passe écrit dans la mémoire tampon de stencil de profondeur et la deuxième passe lit à partir de la mémoire tampon. Cela permet à un nuanceur de comparer les valeurs de la profondeur ou du gabarit précédemment écrites dans la mémoire tampon par rapport à la valeur du Notez pixel qui est affiché. Le résultat de la comparaison peut être utilisé pour créer des effets tels que le mappage de clichés instantanés ou les particules souples dans un système de particules.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Étape Output Merger (OM)](output-merger-stage--om-.md)

[Pipeline graphique](graphics-pipeline.md)

[Sortie-étape de fusion](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)