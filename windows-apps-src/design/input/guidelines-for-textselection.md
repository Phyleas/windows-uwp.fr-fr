---
Description: Cette rubrique décrit la nouvelle interface utilisateur Windows permettant de sélectionner et manipuler du texte, des images et des contrôles, et fournit des instructions d’expérience utilisateur qui doivent être prises en compte lors de l’utilisation de ces nouveaux mécanismes de sélection et de manipulation dans votre application Windows.
title: Sélection de texte et d’images
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: clavier, texte, entrées, interactions avec l’utilisateur
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 179eeffd014cb614fb5314826068d9690fc29807
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216972"
---
# <a name="selecting-text-and-images"></a>Sélection de texte et d’images


Cet article décrit la sélection et la manipulation de texte, d’images et de contrôles, et fournit des recommandations en matière d’expérience utilisateur à prendre en compte lors de l’utilisation de ces mécanismes dans vos applications.

> **API importantes**: [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Xaml.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez des glyphes de police lors de l’implémentation de votre propre interface utilisateur de barre de redimensionnement. Le symbole de sélection est une combinaison de deux polices Segoe UI disponibles à l’échelle du système. L’utilisation de ressources de police simplifie les problèmes de rendu pour différentes valeurs de ppp et fonctionne bien avec les divers plateaux d’échelle d’interface utilisateur. Lorsque vous implémentez vos propres barres de redimensionnement, elles doivent partager les caractéristiques d’interface utilisateur suivantes :

    -   forme circulaire,
    -   visible par rapport à tout arrière-plan,
    -   taille cohérente.
-   Fournissez une marge autour du contenu sélectionnable pour prendre en compte l’interface utilisateur de la barre de redimensionnement. Si votre application permet la sélection de texte dans une région qui n’effectue pas de panoramique/défilement, prévoyez une marge large d’une demi barre de redimensionnement sur les côtés gauche et droit de la zone de texte et une marge haute d’une barre de redimensionnement en haut et en bas de la zone de texte (comme illustré dans les images suivantes). Cela garantit que l’interface utilisateur complète de barre de redimensionnement soit exposée à l’utilisateur et réduit les interactions involontaires avec d’autres éléments d’interface utilisateur latéraux.

    ![marges de redimensionnement du texte sélectionné](images/textselection-gripper-margins.png)

-   Masquez l’interface utilisateur de redimensionnement au cours d’une interaction. Élimine l’occlusion par les symboles de sélection/redimensionnement au cours de l’interaction. Cela est utile lorsqu’une barre de redimensionnement n’est pas complètement masquée par le doigt ou dans le cas de plusieurs barres de redimensionnement de sélection de texte. Cela permet d’éliminer les artefacts visuels durant l’affichage de fenêtres enfants.

-   N’autorisez pas la sélection d’éléments d’interface utilisateur tels que des contrôles, des étiquettes, des images, des contenus propriétaires, etc. En règle générale, les applications Windows permettent cette sélection uniquement au sein de contrôles spécifiques. Les contrôles comme les boutons, les étiquettes et les logos ne peuvent pas être sélectionnés. Déterminez si la sélection représente un problème pour votre application et, le cas échéant, identifiez les zones de l’interface utilisateur dans lesquelles la sélection doit être interdite. 

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires


La sélection et la manipulation de texte représentent des difficultés particulières pour l’utilisateur en termes d’interactions tactiles. Les entrées effectuées par le biais de la souris, du stylo/stylet et du clavier sont des opérations extrêmement granulaires : un clic de souris ou un contact du stylo/stylet est généralement associé à un pixel unique, et une touche est appuyée ou non. Une entrée tactile n’est pas granulaire ; il est difficile d’associer la totalité de la surface de la pointe du doigt à un emplacement x-y spécifique sur l’écran pour placer avec précision un curseur de texte.

**Remarques et recommandations**

Utilisez les contrôles intégrés exposés par le biais des infrastructures de langage dans Windows pour générer des applications offrant une expérience d’interaction utilisateur de plateforme intégrale, y compris les comportements de sélection et de manipulation. Vous trouverez les fonctionnalités d’interaction des contrôles intégrés suffisantes pour la majorité des applications Windows.

Lorsque vous utilisez des contrôles de texte Windows standard, les éléments visuels et les comportements de sélection décrits dans cette rubrique ne peuvent pas être personnalisés.

**Sélection de texte**

Si votre application requiert une interface utilisateur personnalisée prenant en charge la sélection de texte, nous vous recommandons de suivre les comportements de sélection Windows décrits ici.

**Contenu modifiable et contenu non modifiable**


Avec l’entrée tactile, les interactions de sélection sont principalement effectuées par des gestes (par exemple, un appui pour placer un curseur d’insertion ou sélectionner un mot, et un glissement pour modifier une sélection). À l’instar des autres interactions tactiles de Windows, les interactions chronométrées sont limitées au geste d’appui prolongé pour afficher une interface utilisateur informative. Pour plus d’informations, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

Windows reconnaît deux états possibles pour les interactions de sélection, modifiable et non modifiable, et ajuste l’interface utilisateur, le retour et la fonctionnalité de sélection de manière appropriée.

**Contenu modifiable**

Un appui dans la moitié gauche d’un mot place le curseur immédiatement à gauche du mot, tandis qu’un appui dans la moitié droite le placement immédiatement à droite du mot.

L’image suivante montre comment placer un curseur d’insertion initial avec une barre de redimensionnement en appuyant à côté du début ou de la fin d’un mot.

![Appuyez (ou effectuez un appui prolongé) sur le côté gauche du mot pour placer un curseur et une barre de redimensionnement au début du mot. Appuyez (ou effectuez un appui prolongé) sur le côté droit du mot pour placer un curseur et une barre de redimensionnement à la fin du mot.](images/textselection-place-caret.png)

L’image suivante montre comment ajuster une sélection en faisant glisser la barre de redimensionnement.

![Faites glisser la barre de redimensionnement dans n’importe quelle direction pour ajuster la sélection (la première barre de redimensionnement reste ancrée et une deuxième barre de redimensionnement s’affiche). Faites glisser l’une des deux barres pour effectuer d’autres ajustements.](images/adjust-selection.png)

Les images suivantes montrent comment appeler le menu contextuel en appuyant dans la sélection ou sur une barre de redimensionnement (l’appui prolongé peut aussi être utilisé).

![Appuyez (ou effectuez un appui prolongé) dans la sélection ou sur un symbole de sélection/redimensionnement pour appeler le menu contextuel.](images/textselection-show-context.png)

**Remarque**    Ces interactions varient quelque peu dans le cas d’un mot mal orthographié. L’appui sur un mot qui est marqué comme incorrectement orthographié entraîne la mise en surbrillance de la totalité du mot et l’appel du menu contextuel d’orthographe suggérée.

 

**Contenu non modifiable**

L’image suivante montre comment sélectionner un mot en appuyant au sein de celui-ci (aucun espace n’est inclus dans la sélection initiale).

![Appuyez dans un mot pour le sélectionner (aucun espace n’est inclus dans la sélection initiale).](images/select-word.png)

Suivez les mêmes procédures que pour le texte modifiable pour ajuster la sélection et afficher le menu contextuel.

**Manipulation d’objet**

Dans la mesure du possible, utilisez les mêmes ressources de redimensionnement (ou similaire) que la sélection de texte lors de l’implémentation de la manipulation d’objets personnalisée dans une application Windows. Cela permet d’offrir une expérience d’interaction cohérente sur toute la plateforme.

Par exemple, il est possible également d’utiliser des barres de redimensionnement dans des applications de traitement d’image prenant en charge le redimensionnement et le rognage ou dans des applications de lecteur multimédia fournissant des barres de progression réglables, comme illustré dans les images suivantes.

![lecteur multimédia avec barre de progression](images/gripper-mediaplayer.png)

*Lecteur multimédia avec barre de progression réglable*

![image avec poignées de rognage](images/gripper-imagemanip.png)

*Éditeur d’images avec poignées de rognage*

## <a name="related-articles"></a>Articles connexes

### <a name="for-developers"></a>Pour les développeurs

- [Interactions utilisateur personnalisées](../layout/index.md)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple d’événements d’entrée utilisateur XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrée : exemple de test de positionnement tactile](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemple de défilement XAML, panoramique et zoom](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrée : exemple d’entrée manuscrite simplifiée](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrée : exemple de mouvements Windows 8](/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrée : manipulations et gestes, exemple](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Exemple d’entrée tactile DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
