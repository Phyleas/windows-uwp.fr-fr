---
Description: Les listes affichent et activent l’interaction avec du contenu basé sur des collections.
title: Collections et listes
ms.assetid: C73125E8-3768-46A5-B078-FDDF42AB1077
label: Collections and Lists
template: detail.hbs
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 75bc81f4295fb76f5a7cc61b3cadd1496f57dc4c
ms.sourcegitcommit: 1b06c27e7fa4726fd950cbeaf05206c0a070e3c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80893479"
---
# <a name="collections-and-lists"></a>Collections et listes

Les collections et les listes font référence à la représentation de plusieurs éléments de données associés qui apparaissent ensemble. Les collections peuvent être représentées de plusieurs façons, par différents contrôles de collection (également appelés affichages Collection). Les contrôles de collection affichent et permettent des interactions avec le contenu basé sur des collections (liste de contacts, liste de dates, collection d’images, etc.).

> **API importantes** : [Classe ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [classe GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView), [classe FlipView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flipview), [classe TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview), [classe ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2)

Les contrôles traités dans cet article sont les suivants :

- Affichages Liste, principalement utilisés pour afficher des collections de contenus riches en texte
- Affichages Grille, principalement utilisés pour afficher des collections de contenus riches en images
- Affichages symétriques, principalement utilisés pour afficher des collections de contenus comportant beaucoup d’images qui nécessitent la mise au point d’exactement un élément à la fois
- Arborescences, principalement utilisées pour afficher des collections de contenus riches en texte dans une hiérarchie spécifique
- ItemsRepeater, composant personnalisable permettant de créer des contrôles de collection personnalisés

Des recommandations en matière de conception, des fonctionnalités et des exemples sont fournis ci-dessous pour chaque contrôle.

Chacun de ces contrôles (à l’exception d’ItemsRepeater) intègre des fonctionnalités de style et d’interaction. Toutefois, pour personnaliser davantage l’apparence visuelle de votre affichage de collection et des éléments qu’il contient, un [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) est utilisé. Vous trouverez des informations détaillées sur les modèles de données et sur la personnalisation de l’apparence d’un affichage de collection dans la page [Modèles et conteneurs d’éléments](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/item-containers-templates).

Chacun de ces contrôles (à l’exception d’ItemsRepeater) a également un comportement intégré permettant la sélection d’un ou de plusieurs éléments. Pour en savoir plus, consultez [Vue d’ensemble des modes de sélection](selection-modes.md).

Un des scénarios non traités dans cet article est l’affichage des collections dans un tableau ou sur plusieurs colonnes. Si vous souhaitez afficher une collection dans ce format, envisagez d’utiliser le [contrôle DataGrid](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) du [Kit de ressources Communauté Windows](https://docs.microsoft.com/windows/communitytoolkit/). 

> **Windows 10 Fall Creators Update - Modification de comportement** Par défaut, au lieu d’effectuer la sélection, un stylet actif fait défiler ou parcourt une liste dans les applications UWP (comme l’interaction tactile, le pavé tactile et le stylet passif).
> Si votre application repose sur le comportement précédent, vous pouvez remplacer le défilement du stylet et rétablir le comportement précédent. Pour plus d’informations, consultez la rubrique d’informations de référence sur les API pour la [classe Scroll Viewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, observez les contrôles <a href="xamlcontrolsgallery:/item/ListView">ListView</a>, <a href="xamlcontrolsgallery:/item/GridView">GridView</a>, <a href="xamlcontrolsgallery:/item/FlipView">FlipView</a>, <a href="xamlcontrolsgallery:/item/TreeView">TreeView</a> et <a href="xamlcontrolsgalley:/item/ItemsRepeater">ItemsRepeater</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="list-views"></a>Affichages de liste

Les affichages de liste représentent des éléments contenant beaucoup de texte, en général dans une disposition empilée verticalement à une seule colonne. Ils vous permettent de classer des éléments et d’affecter des en-têtes de groupe, de glisser-déposer des éléments, de traiter du contenu et de réorganiser des éléments.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un affichage Liste pour :

- Afficher une collection constituée principalement d’éléments textuels, où tous les éléments doivent avoir le même comportement visuel et d’interaction.
- Représenter une collection de contenus unique ou catégorisée.
- Prendre en charge un grand nombre de cas d’usage, notamment les suivants (les plus courants) :
    - Créer une liste de messages ou un journal de messages.
    - Créer une liste de contacts.
    - Créer le volet principal dans le [modèle Maître/Détails](master-details.md). Un modèle Maître/Détails est souvent utilisé dans les applications de messagerie, dans lesquelles un volet (maître) contient une liste d’éléments sélectionnables, tandis que l’autre (détails) affiche une vue détaillée de l’élément sélectionné.
    

### <a name="examples"></a>Exemples

Voici un affichage de liste simple qui montre une liste de contacts et regroupe les éléments de données par ordre alphabétique. Les en-têtes de groupe (chaque lettre de l’alphabet dans cet exemple) peuvent également être personnalisés pour rester « rémanents ». Dans ce cas, ils apparaissent toujours en haut du ListView pendant le défilement.

![Un affichage Liste avec des données regroupées](images/listview-grouped-example-resized-final.png)

Il s’agit d’un ListView qui a été inversé pour afficher un journal des messages, les plus récents apparaissant en bas. Avec un ListView inversé, les éléments apparaissent au bas de l’écran avec une animation intégrée.

![Affichage de liste inversée](images/listview-inverted-2.png)

### <a name="related-articles"></a>Articles connexes
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
<td align="left"><p><a href="listview-and-gridview.md">Mode Liste et mode Grille</a></p></td>
<td align="left"><p>Découvrez les principaux éléments de l’utilisation d’un affichage Liste ou Grille dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Modèles et conteneurs d’éléments</a></p></td>
<td align="left"><p>Les éléments que vous affichez dans un affichage de liste ou de grille peuvent jouer un rôle majeur dans l’apparence générale de votre application. Pour rendre votre application plus attrayante, personnalisez l’apparence des éléments de votre collection en modifiant les modèles de contrôle et les modèles de données.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-listview.md">Modèles d’éléments pour le mode Liste</a></p></td>
<td align="left"><p>Utilisez ces exemples de modèles d’éléments pour un ListView afin d’obtenir l’apparence des types d’applications courants.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="inverted-lists.md">Listes inversées</a></p></td>
<td align="left"><p>Les listes inversées placent les nouveaux éléments ajoutés en bas, comme dans une application de chat. Suivez les recommandations de cet article pour utiliser une liste inversée dans votre application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pull-to-refresh.md">Balayer pour actualiser</a></p></td>
<td align="left"><p>Le mécanisme Balayer pour actualiser permet à l’utilisateur de dérouler une liste de données à l’aide de la fonction tactile pour récupérer plus de données. Lisez cet article pour implémenter le modèle Balayer pour actualiser dans votre affichage de liste.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface utilisateur imbriquée</a></p></td>
<td align="left"><p>L’interface utilisateur imbriquée est une interface utilisateur qui expose des contrôles exploitables inclus dans un conteneur, lequel peut également être actionné par l’utilisateur. Par exemple, si un élément de l’affichage Liste contient un bouton, l’utilisateur peut sélectionner l’élément de liste ou appuyer sur le bouton imbriqué à l’intérieur. Suivez ces bonnes pratiques pour fournir la meilleure expérience d’interface utilisateur imbriquée à vos utilisateurs.</p></td>
</tr>
</tbody>
</table>

## <a name="grid-views"></a>Affichages Grille

Les affichages Grille conviennent pour l’organisation et l’exploration des collections de contenus à base d’images. Une disposition de liste Grille défile verticalement et s’étend horizontalement. Les éléments se trouvent dans une disposition enveloppée, car ils s’affichent dans un ordre de lecture de gauche à droite, puis de haut en bas.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un affichage de grille pour :

- Afficher une collection de contenus dans laquelle le point focal de chaque élément est une image et où chaque élément doit avoir le même comportement visuel et d’interaction.
- Afficher des bibliothèques de contenu.
- Mettre en forme les deux affichages de contenu associés à un [zoom sémantique](semantic-zoom.md).
- Prendre en charge un grand nombre de cas d’usage, notamment les suivants (les plus courants) :
    - Interface utilisateur de type vitrine (par exemple, exploration d’applications, de chansons, de produits)
    - Bibliothèques de photos interactives

### <a name="examples"></a>Exemples

Cet exemple illustre une disposition d’affichage Grille standard, dans ce cas pour des applications de navigation. Les métadonnées pour les éléments d’un affichage Grille sont généralement limitées à quelques lignes de texte et à une évaluation.

![Exemple de disposition d’affichage Grille](images/controls_gridview_example02.png)

Un affichage Grille est une solution idéale pour une bibliothèque de contenu, souvent utilisée pour présenter du contenu multimédia tel que des images et vidéos. Dans une bibliothèque de contenu, l’utilisateur s’attend à pouvoir appuyer sur un élément pour appeler une action.

![Exemple de bibliothèque de contenu](images/gridview-simple-example-final.png)

### <a name="related-articles"></a>Articles connexes
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
<td align="left"><p><a href="listview-and-gridview.md">Mode Liste et mode Grille</a></p></td>
<td align="left"><p>Découvrez les principaux éléments de l’utilisation d’un affichage Liste ou Grille dans votre application.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="item-containers-templates.md">Modèles et conteneurs d’éléments</a></p></td>
<td align="left"><p>Les éléments que vous affichez dans un affichage de liste ou de grille peuvent jouer un rôle majeur dans l’apparence générale de votre application. Pour rendre votre application plus attrayante, personnalisez l’apparence des éléments de votre collection en modifiant les modèles de contrôle et les modèles de données.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="item-templates-gridview.md">Modèles d’éléments pour le mode Grille</a></p></td>
<td align="left"><p>Utilisez ces exemples de modèles d’éléments pour un GridView afin d’obtenir l’apparence des types d’applications courants.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="nested-ui.md">Interface utilisateur imbriquée</a></p></td>
<td align="left"><p>L’interface utilisateur imbriquée est une interface utilisateur qui expose des contrôles exploitables inclus dans un conteneur, lequel peut également être actionné par l’utilisateur. Par exemple, si un élément de l’affichage de grille contient un bouton, l’utilisateur peut sélectionner l’élément de grille ou appuyer sur le bouton imbriqué à l’intérieur. Suivez ces bonnes pratiques pour fournir la meilleure expérience d’interface utilisateur imbriquée à vos utilisateurs.</p></td>
</tr>
</tbody>
</table>

## <a name="flip-views"></a>Affichages symétriques

Les affichages symétriques sont adaptés à la consultation de collections de contenus basés sur des images, en particulier quand l’expérience souhaitée est d’afficher une seule image à la fois. Un affichage symétrique permet à l’utilisateur de déplacer ou de « retourner » les éléments de la collection (verticalement ou horizontalement), chaque élément apparaissant l’un après l’autre après l’interaction de l’utilisateur.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un affichage symétrique pour :

- Afficher une collection de petite à moyenne taille (moins de 25 éléments), composée d’images avec peu ou pas de métadonnées.
- Afficher des éléments un par un et autoriser l’utilisateur final à parcourir les éléments à son rythme.
- Prendre en charge un grand nombre de cas d’usage, notamment les suivants (les plus courants) :
    - Galeries de photos
    - Galeries ou vitrines de produits

### <a name="examples"></a>Exemples

Les deux exemples suivants illustrent un FlipView qui se retourne des éléments horizontalement et verticalement, respectivement.

![Affichage symétrique horizontal](images/controls_flipview_horizonal.jpg)

![Affichage symétrique vertical](images/controls_flipview_vertical.jpg)

### <a name="related-articles"></a>Articles connexes
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
<td align="left"><p><a href="flipview.md">Vue symétrique</a></p></td>
<td align="left"><p>Découvrez les bases de l’utilisation d’un affichage symétrique dans votre application, ainsi que la façon de personnaliser l’apparence de vos éléments dans cet affichage.</p></td>
</tr>
</tbody>
</table>

## <a name="tree-views"></a>Arborescences

Les arborescences sont adaptées à l’affichage de collections textuelles associées à une hiérarchie importante qui doit être présentée. Les éléments d’arborescence sont réductibles/développables, s’affichent dans une hiérarchie visuelle, peuvent être complétés par des icônes et peuvent être déplacés par glisser-déposer entre arborescences. Les arborescences autorisent l’imbrication de niveau N.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez une arborescence pour :

- Afficher une collection d’éléments imbriqués dont le contexte et la signification dépendent d’une hiérarchie ou d’une chaîne d’organisation spécifique.
- Prendre en charge un grand nombre de cas d’usage, notamment les suivants (les plus courants) :
    - Explorateur de fichiers
    - Organigramme d’entreprise

### <a name="examples"></a>Exemples

Voici un exemple d’arborescence qui représente un explorateur de fichiers et qui affiche de nombreux éléments imbriqués complétés par des icônes.

![Arborescence avec des icônes](images/treeview-icons.png)

### <a name="related-articles"></a>Articles connexes
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
<td align="left"><p><a href="tree-view.md">Arborescence</a></p></td>
<td align="left"><p>Découvrez les bases de l’utilisation d’une arborescence dans votre application, ainsi que la façon de personnaliser l’apparence et l’interaction de vos éléments dans une arborescence.</p></td>
</tr>
</tbody>
</table>

## <a name="itemsrepeater"></a>ItemsRepeater

ItemsRepeater est différent du reste des contrôles de collection mentionnés dans cette page en ce sens qu’il ne fournit pas d’interaction ni de style prêt à l’emploi. Il est simplement placé dans une page sans définir de propriétés. ItemsRepeater est plutôt un composant que vous pouvez utiliser en tant que développeur pour créer votre propre contrôle de collection personnalisé, en particulier un contrôle que vous ne pouvez pas obtenir avec les autres contrôles de cet article. ItemsRepeater est un panneau piloté par les données aux performances élevées que vous pouvez adapter à vos besoins exacts.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un ItemsRepeater si :

- Vous avez à l’esprit une interface et une expérience utilisateur spécifiques que vous ne pouvez pas créer à l’aide de contrôles de collection existants.
- Vous disposez d’une source de données existante pour vos éléments (par exemple, des données extraites d’Internet, d’une base de données ou d’une collection préexistante dans votre code-behind).

### <a name="examples"></a>Exemples

Les trois exemples suivants sont tous des contrôles ItemsRepeater liés à la même source de données (une collection de nombres). La collection de nombres est représentée de trois façons, chaque ItemsRepeaters ci-dessous utilisant un [Layout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layout) et un [ItemTemplate](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate?view=winui-2.2) personnalisé différent.

![ItemsRepeater avec barres horizontales](images/itemsrepeater-1.png)
![ItemsRepeater avec barres verticales](images/itemsrepeater-2.png)
![ItemsRepeater avec représentation circulaire](images/itemsrepeater-3.png)

### <a name="related-articles"></a>Articles connexes
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
<td align="left"><p><a href="items-repeater.md">ItemsRepeater</a></p></td>
<td align="left"><p>Apprenez les bases de l’utilisation d’un ItemsRepeater dans votre application, et découvrez comment implémenter tous les composants d’interaction et visuels nécessaires pour votre affichage de collection.</p></td>
</tr>
</tbody>
</table>


## <a name="globalization-and-localization-checklist"></a>Liste de contrôle de globalisation et de localisation

<table>
<tr>
<th>Renvoi à la ligne</th><td>Autorisez deux lignes pour l’étiquette de liste.</td>
</tr>
<tr>
<th>Extension horizontale</th><td>Vérifiez que les champs prennent en charge l’extension de texte et qu’ils peuvent défiler.</td>
</tr>
<tr>
<th>Espacement vertical</th><td>Utilisez les caractères non latins pour l’espacement vertical afin d’assurer un affichage correct des scripts non latins.</td>
</tr>
</table>

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

**Conception et recommandations en matière d’expérience utilisateur**
- [Maître/détails](master-details.md)
- [Volet de navigation](navigationview.md)
- [Zoom sémantique](semantic-zoom.md)
- [Glisser-déplacer](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
- [Images miniatures](../../files/thumbnails.md)

**Informations de référence sur les API**
- [ListView, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [ComboBox, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox)
- [ListBox, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
