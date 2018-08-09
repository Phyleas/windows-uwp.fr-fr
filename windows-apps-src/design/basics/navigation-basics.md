---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Informations de base relatives à la navigation pour les applicationsUWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0980f24394a075596b60e4a8005b303857b91304
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843069"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Informations de base relatives à la conception de la navigation pour les applications UWP

![En-tête de base de navigation](images/nav/navigation-basics-header.jpg)

Si l’on considère une application comme une collection de pages, le terme *navigation* décrit le fait de se déplacer d’une page à l’autre et au sein d’une même page. La navigation est le point de départ de l’expérience utilisateur, car elle permet aux utilisateurs de rechercher le contenu et les fonctionnalités qui les intéressent. C’est un élément très important, qui peut être difficile à réaliser correctement.

Nous avons un nombre considérable de choix à faire pour la navigation. Par exemple:

:::row::: :::column::: ![exemple de navigation1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

Il n’existe aucune conception de navigation unique qui fonctionne pour toutes les applications, mais il existe un ensemble de principes et de recommandations, que vous pouvez suivre pour mieux déterminer la conception appropriée à votre application.

## <a name="principles-of-good-navigation"></a>Principes d’une navigation réussie

Commençons par les principes de base d’une navigation réussie:

- **Cohérence:** répondre aux attentes des utilisateurs.
- **Simplicité:** ne pas faire plus que nécessaire.
- **Clarté:** fournir des chemins d’accès clair et des options.

### <a name="consistency"></a>Cohérence

La navigation doit être cohérente avec les attentes des utilisateurs. Utilisez des [contrôles standard](#use-the-right-controls) auxquels les utilisateurs sont familiers et respectez les conventions standard pour les icônes, les emplacements et le style pour rendre la navigation prévisible et intuitive pour les utilisateurs.

![image des composants d'une page](images/nav/page-components.svg)

> Les utilisateurs s’attendent à trouver certains éléments d’interface utilisateur dans des emplacements standard.

### <a name="simplicity"></a>Simplicité

Un recours moins large aux éléments de navigation simplifie la prise de décisions pour les utilisateurs. Le fait de fournir un accès simple à des destinations importantes et de masquer les éléments moins importants permet aux utilisateurs d’accéder plus facilement et rapidement à la page de leur choix.

:::row::: :::column::: ![exemple faire](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Constantly provide many navigation options to overwhelm the user.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Clarté

Des chemins d’accès clairs favorisent une navigation logique pour les utilisateurs. Le fait de rendre les options de navigation évidentes et de clarifier les relations entre les pages de navigation doit empêcher les utilisateurs de se perdre.

![exemple faire](images/nav/clarity-image.svg)

> Les destinations sont clairement nommées pour que les utilisateurs sachent où ils sont.

## <a name="general-recommendations"></a>Recommandations générales

Maintenant nous allons utiliser nos principes de conception (cohérence, simplicité et clarté) pour élaborer des règles générales.

1. Réfléchissez à vos utilisateurs. Définissez le parcours qu’ils sont susceptibles de suivre dans votre application et cherchez à savoir pourquoi les utilisateurs accèdent à une page et où ils souhaitent aller ensuite. 

2. Évitez les hiérarchies de navigation profondes. Au-delà de trois niveaux de navigation, vous risquez d’égarer votre utilisateur dans une hiérarchie profonde qu’il aura des difficultés à quitter.

3. Évitez l’effet «bâton sauteur». L’effet du bâton sauteur se produit lorsqu’il existe du contenu associé, mais que la navigation vers celui-ci oblige l’utilisateur à remonter d’un niveau puis à descendre à nouveau.

## <a name="use-the-right-structure"></a>Utiliser la structure appropriée

Maintenant que vous êtes familiarisé avec les principes de navigation générale, comment devez-vous structurer votre application? Il existe deux structures générales: plate et hiérarchique.

:::row::: :::column::: ![Pages organisées dans une structure plate](images/nav/flat-lateral-structure.svg) :::column-end::: :::column span="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from on page to another in any order. 

        We recommend using a flat structure when:
        <ul>
        <li>The pages can be viewed in any order.</li>
        <li>The pages are clearly distinct from each other and don't have an obvious parent/child relationship.</li>
        <li>There are fewer than 8 pages in the group.<br/>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)</li>
        </ul>

    :::column-end:::
:::row-end:::

:::row::: :::column::: ![Pages organisées dans une hiérarchie](images/nav/hierarchical-structure.svg) :::column-end::: :::column span="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page. 

        We recommend a hiearchical structure when:
        <ul>
        <li>Pages should be traversed in a specific order.</li>
        <li>There is a clear parent-child relationship between pages.</li>
        <li>There are more than 7 pages in the group.</li>
        </ul>
    :::column-end:::
:::row-end:::

:::row::: :::column::: ![Application à structure hybride](images/nav/combining-structures.svg) :::column-end::: :::column span="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree. 
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Utilisez les contrôles appropriés

Une fois que vous avez choisi votre structure de page, vous devez déterminer comment les utilisateurs navigueront à travers ces pages. La plateformeUWP fournit une variété de contrôles de navigation pour garantir une expérience de navigation cohérente et fiable dans votre application. 

Nous vous recommandons de sélectionner un contrôle de navigation en fonction du nombre d’éléments de navigation dans votre application. Si vous disposez de cinq éléments de navigation ou moins, utilisez la navigation de niveau supérieur, comme les [onglets et les sélecteurs de vue](../controls-and-patterns/tabs-pivot.md). Si vous avez plus de sixéléments de navigation, utilisez la navigation de gauche, notamment l’[affichage de navigation](../controls-and-patterns/navigationview.md) ou [la vue maître/détails](../controls-and-patterns/master-details.md).

:::row::: :::column::: ![Image de cadre](images/nav/thumbnail-frame.svg) :::column-end::: :::column span="2"::: <a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame"><b>Cadre</b></a>

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row::: :::column::: ![image d'onglets et de sélecteur de vue](images/nav/thumbnail-tabs-pivot.svg) :::column-end::: :::column span="2"::: <a href="../controls-and-patterns/tabs-pivot.md"><b>Onglets et sélecteur de vue</b></a><br>

        Displays a horizontal list of links to pages at the same level. Use when:
        <ul>
        <li>There are 2-5 pages. (You can use tabs/pivots when there are more than 5 pages, but it might be difficult to fit all the tabs/pivots on the screen.)</li>
        <li>You expect users to switch between pages frequently.</li>
        </ul>
:::row-end:::

:::row::: :::column::: ![image d'onglets et de sélecteur de vue](images/nav/thumbnail-navview.svg) :::column-end::: :::column span="2"::: <a href="../controls-and-patterns/navigationview.md"><b>Affichage de navigation</b></a><br>

        Displays a vertical list of links to top-level pages. Use when:
        <ul>
        <li>The pages exist at the top level.</li>
        <li>There are many navigational items (more than 5).</li>
        <li>You don't expect users to switch between pages frequently.</li>
        </ul>
:::row-end:::

:::row::: :::column::: ![image de la vue maître/détails](images/nav/thumbnail-master-detail.svg) :::column-end::: :::column span="2"::: <a href="../controls-and-patterns/master-details.md"><b>Maître/détails</b></a><br>

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        <ul>
        <li>You expect users to switch between child items frequently.</li>
        <li>You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.</li>
        </ul>

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::row-end:::

:::row::: :::column::: ![Image de liens hypertexte et boutons](images/nav/thumbnail-hyperlinks-buttons.svg) :::column-end::: :::column span="2"::: <a href="../controls-and-patterns/hyperlinks.md"><b>Liens hypertexte</b></a> et <a href="../controls-and-patterns/buttons.md"><b>Boutons</b></a><br>

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Étape suivante: Ajouter du code de navigation à votre application

L’article suivant [Implémenter la navigation de base](navigate-between-two-pages.md), indique le code nécessaire pour utiliser un contrôle de cadre afin d’activer la navigation de base entre deux pages de votre application. 