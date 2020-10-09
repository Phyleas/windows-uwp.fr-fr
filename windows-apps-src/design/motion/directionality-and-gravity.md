---
title: Orientation et gravité-animation dans les applications Windows
description: En savoir plus sur l’utilisation de la direction du mouvement, la direction de navigation et la gravité dans des scènes animées en affichant des exemples.
label: Directionality and gravity
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 87047e20d4513c9120c79bb329c008dad104a352
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860083"
---
# <a name="directionality-and-gravity"></a>Direction et gravité

Les signaux directionnels permettent de consolider le modèle mental du trajet qu’un utilisateur effectue sur les expériences. Il est important que la direction d’un mouvement prenne en charge la continuité de l’espace et l’intégrité des objets dans l’espace.

Le mouvement directionnel est soumis à des forces comme la gravité. L’application de forces au mouvement renforce le sentiment naturel du mouvement.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/category/Motion">ouvrir l’application et voir Mouvement en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>Direction du mouvement

:::row:::
    :::column:::
La direction du mouvement correspond au mouvement physique. À l’instar de la nature, les objets peuvent se déplacer dans n’importe quel axe des pays-X, Y, Z. C’est ainsi que nous pensons au déplacement d’objets à l’écran.
Lorsque vous déplacez des objets, évitez les collisions innaturelles. Gardez à l’esprit que les objets proviennent de et que vous accédez à, et que vous prenez toujours en charge des constructions de niveau supérieur qui peuvent être utilisées dans la scène, telles que la direction de défilement ou la hiérarchie de disposition.
    :::column-end:::
    :::column:::
        ![Brève vidéo avec un cercle, puis l’ajout d’un axe X, un axe Y et un axe Z.](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Direction de navigation

La direction de la navigation entre les scènes dans votre application est conceptuelle. Les utilisateurs naviguent vers l’avant et vers l’arrière. Les scènes se déplacent et sortent de la vue. Ces concepts s’associent au déplacement physique pour guider l’utilisateur.

Lorsque la navigation entraîne le déplacement d’un objet de la scène précédente à la nouvelle scène, l’objet effectue un déplacement de A à B simple sur l’écran. Pour vous assurer que le mouvement semble plus physique, l’accélération standard est ajoutée, ainsi que le sentiment de gravité.

Pour la navigation vers l’arrière, le déplacement est inversé (B-to-A). Lorsque l’utilisateur navigue vers l’arrière, il est attendu de revenir à l’état précédent dès que possible. La synchronisation est plus rapide, plus directe et utilise l’accélération de décélération.

Ici, ces principes sont appliqués lorsque l’élément sélectionné reste à l’écran pendant la navigation vers l’avant et l’arrière.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)

Lorsque la navigation entraîne le remplacement des éléments à l’écran, il est important de montrer où se termine la scène, ainsi que l’origine de la nouvelle scène.

Cela a plusieurs avantages :

- Il consolidifie le modèle mental de l’utilisateur de l’espace.
- La durée de la scène de sortie fournit plus de temps pour préparer le contenu à animer pour la scène entrante.
- Il améliore les performances perçues de l’application.

Il existe 4 directions de navigation discrètes à prendre en compte.

:::row:::
    :::column:::
**Avant** Célébrez le contenu entrant dans la scène d’une manière qui n’entre pas en conflit avec le contenu sortant. Le contenu ralentit dans la scène.
    :::column-end:::
    :::column:::
        ![avancer dans le sens](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Transfert** Le contenu s’arrête rapidement. Accélération de l’écran des objets.
    :::column-end:::
    :::column:::
        ![transfert de la direction](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
Vers **l’arrière** Identique à Forward-in, mais inversé.
    :::column-end:::
    :::column:::
        ![Brève vidéo présentant un cercle entrant à partir de la droite du cadre et s’arrêtant au milieu du cadre.](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Sortie** vers l’arrière Identique au transfert, mais inversé.
    :::column-end:::
    :::column:::
        ![direction vers l’arrière](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Gravité

La gravité rend vos expériences plus naturelles. Les objets qui se déplacent sur l’axe des Z et qui ne sont pas ancrés à la scène par une offre à l’écran sont susceptibles d’être affectés par la gravité. À mesure qu’un objet s’interrompt sans la scène et qu’il atteint la vélocité d’échappement, la gravité descend sur l’objet et crée une courbe plus naturelle de la trajectoire de l’objet lors de son déplacement.

La gravité se manifeste généralement lorsqu’un objet doit passer d’une scène à une autre. Pour cette raison, l’animation connectée utilise le concept de gravité.

Ici, un élément dans la ligne supérieure de la grille est affecté par gravité, ce qui entraîne un léger déplacement à mesure qu’il quitte sa place et se déplace vers l’avant.

![Brève vidéo présentant un élément Rectangle qui laisse la ligne supérieure d’une grille, qui se dépose légèrement, puis effectue un zoom avant de la fenêtre.](images/continuity-photos.gif)

## <a name="related-articles"></a>Articles connexes

- [Vue d’ensemble du mouvement](index.md)
- [Minutage et accélération](timing-and-easing.md)
