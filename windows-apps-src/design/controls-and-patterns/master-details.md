---
Description: Le modèle Maître/Détails affiche une liste principale et les détails de l’élément actuellement sélectionné. Ce modèle est souvent utilisé pour les listes/carnets d’adresse de messagerie et de contacts.
title: Maître/détails
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b414a1177bbfad670e3b623babce068a299a713e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172593"
---
# <a name="masterdetails-pattern"></a>Modèle Maître/détails

 

Le modèle Maître/détails a un volet principal (généralement avec un [affichage de liste](lists.md)) et un volet des détails pour le contenu. Quand un élément de la liste principale est sélectionné, le volet des détails est mis à jour. Ce modèle est souvent utilisé pour la messagerie et les carnets d’adresses.

> **API importantes** : [classe ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView), [classe SplitView](/uwp/api/windows.ui.xaml.controls.splitview)

![Exemple de modèle Maître/détails](images/HIGSecOne_MasterDetail.png)

> [!TIP]
> Si vous souhaitez utiliser un contrôle XAML qui implémente ce modèle pour vous, nous vous recommandons d’utiliser le [contrôle XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) à partir du Kit de ressources Communauté Windows.

## <a name="is-this-the-right-pattern"></a>Est-ce le modèle approprié ?

Le modèle Maître/détails fonctionne bien si vous voulez :

-   Créer une application de messagerie, un carnet d’adresses ou une application basée sur une disposition liste/détails.
-   Rechercher et hiérarchiser une grande collection de contenu.
-   Permettre d’ajouter et de supprimer rapidement des éléments dans une liste tout en basculant entre les contextes.

## <a name="choose-the-right-style"></a>Choisir le style approprié

Quand vous implémentez le modèle Maître/détails, nous vous recommandons d’utiliser le style empilé ou le style côte à côte, en fonction de l’espace disponible sur l’écran.

| Largeur de fenêtre disponible | Style recommandé |
|------------------------|-------------------|
| 320 epx-640 epx        | Empilé           |
| 641 epx ou plus large       | Côte-à-côte      |

 
## <a name="stacked-style"></a>Style empilé

Le style empilé ne permet de visualiser qu’un seul volet à la fois : le volet principal ou le volet des détails.

![Détail du volet principal en mode Empilé](images/patterns-md-stacked.png)

L’utilisateur commence au niveau du volet principal et descend dans le volet des détails en sélectionnant un élément dans la liste principale. Pour l’utilisateur, les vues Maître et Détails apparaissent dans deux pages distinctes.

### <a name="create-a-stacked-masterdetails-pattern"></a>Créer un modèle Maître/détails empilé

Une des façons de créer le modèle Maître/détails empilé est d’utiliser des pages distinctes pour le volet principal et pour le volet des détails. Placez l'affichage Maître sur une page et le volet Détails dans une autre page.

![Parties du modèle Maître/détails de style empilé](images/patterns-md-stacked-parts.png)

Pour la page d'affichage Maître, un contrôle d’[affichage Liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte. 

Pour la page d’affichage Détails, utilisez l’[élément de contenu](../layout/layout-panels.md) le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition **en grille** pour organiser les éléments dans un formulaire.

Pour plus d’informations sur la navigation entre les pages, consultez [Historique de navigation et navigation vers l’arrière pour les applications Windows](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Style côte à côte

Dans le style côte à côte, le volet principal et le volet des détails sont visibles en même temps.

![Modèle Maître/détails](images/patterns-masterdetail-400x227.png)

La liste du volet principal a un visuel de sélection pour indiquer l’élément sélectionné. La sélection d’un nouvel élément dans la liste principale entraîne la mise à jour du volet des détails.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Créer un modèle Maître/détails côte à côte

L’une des façons de créer un modèle Maître/Détails côte à côte consiste à utiliser le contrôle [mode Fractionné](split-view.md). Placez l'affichage Maître dans le volet du mode Fractionné et l’affichage Détails dans le contenu du mode Fractionné.

![parties du mode Fractionné Maître/Détail](images/patterns_md_splitview_parts.png)

Pour le volet principal, un contrôle d’[affichage de liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte.

Pour le contenu des détails, utilisez l’[élément de contenu](../layout/layout-panels.md) le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition **en grille** pour organiser les éléments dans un formulaire.

## <a name="adaptive-layout"></a>Disposition adaptative

Pour implémenter un modèle Maître/Détails pour n’importe quelle taille d’écran, créez une interface utilisateur réactive avec une [disposition adaptative](../layout/layouts-with-xaml.md).

![disposition adaptative Maître/Détails](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Créer un modèle Maître/Détails adaptatif
Pour créer une disposition adaptative, définissez différents [**VisualStates**](/uwp/api/windows.ui.xaml.visualstate) pour votre interface utilisateur et déclarez des points d’arrêt pour les différents états avec des [**AdaptiveTriggers**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

Les exemples suivants implémentent le modèle Maître/Détails avec des dispositions adaptatives et illustrent la liaison de données avec des ressources statiques, de base de données et en ligne : 
- [Exemple de Maître/Détails](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Exemples de Maître/Détails et de Sélection](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Exemple de Maître/Détails Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [Exemple de lecteur RSS](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> Si vous souhaitez utiliser un contrôle XAML qui implémente ce modèle pour vous, nous vous recommandons d’utiliser le [contrôle XAML MasterDetailsView](/windows/communitytoolkit/controls/masterdetailsview) à partir du Kit de ressources Communauté Windows.

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [Recherche](search.md)
- [Barre de l’application et barre de commandes](app-bars.md)
- [Classe ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Classe SplitView](/uwp/api/windows.ui.xaml.controls.splitview)