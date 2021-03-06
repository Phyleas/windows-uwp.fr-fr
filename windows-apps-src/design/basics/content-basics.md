---
description: Vue d’ensemble des éléments d’interface utilisateur et modèles de page courants pour afficher le contenu dans votre application Windows.
title: Notions de base de la conception de contenu pour les applications Windows
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3933910d77249476e76c4d87dfd96af66bd418a8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031573"
---
# <a name="content-design-basics-for-windows-apps"></a>Notions de base de la conception de contenu pour les applications Windows

Une application a principalement pour objet d’offrir un accès à un contenu. Dans la mesure où les applications répondent à des buts différents, leur contenu se présente sous de nombreuses formes : dans une application de retouche photo, la photo est le contenu ; dans une application de voyage, les cartes et les informations sur les destinations sont le contenu, et ainsi de suite. 

Cet article fournit une vue d’ensemble de la présentation de contenu dans votre application. Nous y décrivons les éléments d’interface utilisateur et les modèles de page courants que vous pouvez utiliser pour afficher le contenu, quelle qu’en soit la forme.

## <a name="common-page-patterns"></a>Modèles de page courants

De nombreuses applications utilisent l’ensemble ou une partie de ces modèles de page pour afficher différents types de contenu. De même, n’hésitez pas à combiner ces modèles pour optimiser le contenu de votre application.

### <a name="landing"></a>Accueil

![page d’accueil](images/content-basics/hero-screen.png)

Les pages d’accueil, également connues sous le nom d’écrans bannière, apparaissent souvent au niveau supérieur d’une expérience d’application. La grande surface sert à exposer les applications, et plus particulièrement à mettre en avant le contenu que les utilisateurs peuvent vouloir parcourir et utiliser.

### <a name="collections"></a>Regroupements

![galerie](images/content-basics/gridview.png)

Les collections permettent aux utilisateurs de parcourir les groupes de contenu ou de données. L’[affichage Grille](../controls-and-patterns/item-templates-gridview.md) est idéal pour les photos ou les contenus centrés sur les médias ; le [mode Liste](../controls-and-patterns/item-templates-listview.md) est, quant à lui, plus particulièrement destiné aux données ou aux contenus riches en texte.


### <a name="masterdetail"></a>Maître/détail

![maître et détails](images/content-basics/master-detail.png)

Le modèle [maître/détails](../controls-and-patterns/master-details.md) se compose du mode Liste (maître) et d’une vue de contenu (détail). Ces deux volets sont figés et offrent un défilement vertical. Il existe une relation claire entre l’élément de liste et la vue de contenu : lorsque l’élément de la vue maître est sélectionné, la vue détaillée est mise à jour en conséquence. Outre la navigation dans la vue détaillée, il est possible d’ajouter et de supprimer des éléments de la vue maître.

### <a name="details"></a>Détails

![plusieurs vues](images/multi-view.png)

Pensez à créer une page dédiée à l’affichage du contenu pour que les utilisateurs puissent voir la page exempte de distractions lorsqu’ils trouvent le contenu qu’ils recherchent. Dans la mesure du possible, [créez une option d’affichage plein écran](../layout/show-multiple-views.md) qui ajuste le contenu à l’écran et masque tous les autres éléments de l’interface utilisateur. 

Pour tenir compte des changements de taille de l’écran, pensez également à créer une [conception réactive](design-and-ui-intro.md) qui masque/affiche les éléments de l’interface utilisateur en fonction des besoins.

### <a name="forms"></a>Formulaires
![formulaire](images/content-basics/forms.png)

Un [formulaire](../controls-and-patterns/forms.md) est un groupe de contrôles qui collectent des données auprès de l’utilisateur et les envoient. La plupart des applications, voire toutes, utilisent un formulaire pour les pages de paramètres, les portails de connexion, les hubs de commentaires, la création de compte, etc. 

## <a name="common-content-elements"></a>Éléments de contenu couramment utilisés

Pour créer ces modèles de page, vous devez utiliser une combinaison d’éléments de contenu. Voici quelques éléments d’interface utilisateur couramment utilisés pour afficher du contenu. (Pour en obtenir la liste complète, consultez [Contrôles et modèles](../controls-and-patterns/index.md).

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Catégorie</th>
<th align="left">Éléments</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Audio et vidéo<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Contrôles de transport et de lecture de média</a></td>
<td align="left">Lit du contenu audio et vidéo.</td>
</tr>
<tr class="even">
<td align="left">Visionneuses d’image<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Vue symétrique</a>, <a href="../controls-and-patterns/images-imagebrushes.md">image</a></td>
<td align="left">Affiche des images. La vue symétrique permet d’afficher les images d’une collection, par exemple les photos d’un album ou les éléments d’une page de détails sur le produit, image par image.</td>
</tr>
<tr class="odd">
<td align="left">Regroupements <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Mode Liste et affichage Grille</a></td>
<td align="left">Présente les éléments dans une liste interactive ou une grille. Utilisez ces éléments pour permettre aux utilisateurs de sélectionner un film parmi une liste de nouveautés ou de gérer un inventaire.</td>
</tr>
<tr class="even">
<td align="left">Texte et saisie de texte <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Bloc de texte</a>, <a href="../controls-and-patterns/text-box.md">zone de texte</a>, <a href="../controls-and-patterns/rich-edit-box.md">zone d’édition enrichie</a></p>
</td>
<td align="left">Affiche le texte. Certains éléments permettent à l’utilisateur de modifier du texte. Pour plus d’informations, consultez <a href="../controls-and-patterns/text-controls.md">Contrôles de texte</a>.
<p>Pour savoir comment afficher du texte, consultez <a href="../style/typography.md">Typographie</a>.</p>
</td>
</tr>
<tr class="odd">
<td align="left">Cartes<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Affiche une carte symbolique ou photoréaliste de la Terre.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Restitue le contenu HTML.</td>
</tr>
</tbody>
</table>
</div>
