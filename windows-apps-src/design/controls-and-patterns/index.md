---
description: Bénéficiez de conseils de conception et d’instructions de développement pour ajouter des contrôles et des modèles à votre application Windows. Vous trouverez plus de 45 contrôles puissants utilisables avec votre application.
title: Contrôles et modèles Windows - Développement d’applications Windows
keywords: contrôles uwp, interface utilisateur, contrôles d’application, contrôles windows
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: f7a06c75b37b62436c4ef0ebcb8ccae2a6153b85
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829429"
---
# <a name="controls-for-windows-apps"></a>Contrôles pour les applications Windows

![Contrôles](../images/controls-2x.png)

Dans le développement d’applications Windows, un <i>contrôle</i> est un élément d’interface utilisateur qui affiche du contenu ou permet une interaction. Les contrôles constituent les blocs de construction de l’interface utilisateur. Un <i>modèle</i> est en quelque sorte une recette permettant d’associer plusieurs contrôles pour créer un élément nouveau.

Nous vous proposons plus de 45 contrôles, des simples boutons aux contrôles de données puissants, tels que l’affichage Grille.  Ces contrôles font partie du système Fluent Design et peuvent vous aider à créer une interface utilisateur scalable et audacieuse qui s’adapte à tous les appareils et toutes les tailles d’écran.

Les articles de cette section donnent des recommandations en matière de conception et des instructions de programmation pour l’ajout de contrôles et de modèles à votre application Windows.

## <a name="intro"></a>Introduction

Instructions générales et exemples de code d’ajout et de stylisation de contrôles en XAML et C#.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">Ajouter des contrôles et gérer les événements</a></b> <br/>
L’ajout de contrôles à votre application se fait en trois étapes : l’ajout de contrôle à l’interface utilisateur de votre application, la définition de propriétés sur le contrôle et l’ajout de code aux gestionnaires d’événements du contrôle pour que ce dernier soit opérationnel.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">Application de styles aux contrôles</a></b> <br/>
Vous pouvez personnaliser l’apparence de vos applications de nombreuses manières à l’aide de l’infrastructure XAML. Les styles permettent de définir les propriétés des contrôles et de réutiliser ces paramètres pour uniformiser l’apparence de plusieurs contrôles.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Obtenir la bibliothèque d’interface utilisateur Windows

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Certains contrôles sont uniquement disponibles dans la bibliothèque d’interface utilisateur Windows (WinUI), package NuGet qui contient les nouveaux contrôles et fonctionnalités d’interface utilisateur. Pour l’obtenir, consultez [Vue d’ensemble et instructions d’installation de la bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).<br/>À compter de WinUI 2.2, de nombreux contrôles utilisent des angles arrondis comme style par défaut. Pour plus d’informations, consultez [Rayons des angles](../style/rounded-corner.md).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

## <a name="alphabetical-index"></a>Index alphabétique

Informations détaillées en matière de contrôles et de modèles spécifiques. (Pour une liste triée par fonction, voir [Index des contrôles par fonction](controls-by-function.md).)

:::row:::
    :::column:::

- Lecteur visuel animé (voir [Lottie](/windows/communitytoolkit/animations/lottie)) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Zone de suggestion automatique](auto-suggest-box.md)
- [Button](buttons.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage du calendrier](calendar-view.md)
- [Case à cocher](checkbox.md)
- [Sélecteur de couleurs](color-picker.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Zone de liste modifiable](combo-box.md)
- [Barre de commandes](app-bars.md)
- [Menu volant de barre de commandes](command-bar-flyout.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Carte de visite](contact-card.md)
- [Boîte de dialogue de contenu](dialogs-and-flyouts/dialogs.md)
- [Lien de contenu](content-links.md)
- [Menu contextuel](menus.md)
- [Sélecteur de dates](date-picker.md)
- [Boîtes de dialogue et menus volants](dialogs-and-flyouts/index.md)
- [Bouton de liste déroulante](buttons.md#create-a-drop-down-button) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Vue symétrique](flipview.md)
- [Flyout](dialogs-and-flyouts/flyouts.md)
- [Formulaires](forms.md) (modèle)
- [Vue Grille](listview-and-gridview.md)
- [Lien hypertexte](hyperlinks.md)
- [Bouton Lien hypertexte](hyperlinks.md#create-a-hyperlinkbutton)
- [Images et pinceaux image](images-imagebrushes.md)
- [Contrôles pour l’entrée manuscrite](inking-controls.md)
- [Vue Liste](listview-and-gridview.md)
- [Contrôle de carte](../../maps-and-location/display-maps.md)
- [Maître/détails](master-details.md) (modèle)
- [Lecture de contenu multimédia](media-playback.md)
- [Barre de menus](menus.md#create-a-menu-bar) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Menu volant](menus.md)
- [Affichage de navigation](navigationview.md) :::image type="icon" source="images/winui-logo-16x16.png":::

    :::column-end:::
    :::column:::

- [Zone de nombre](number-box.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Vue parallaxe](..\motion\parallax.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Zone de mot de passe](password-box.md)
- [Photo de la personne](person-picture.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Tableau croisé dynamique](pivot.md)
- [Barre de progression](progress-controls.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Anneau de progression](progress-controls.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Case d’option](radio-button.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Contrôle d’évaluation](rating.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Bouton de répétition](buttons.md#create-a-repeat-button)
- [Zone d’édition riche](rich-edit-box.md)
- [Bloc de texte riche](rich-text-block.md)
- [Visionneuse à défilement](scroll-controls.md)
- [Recherche](search.md) (modèle)
- [Zoom sémantique](semantic-zoom.md)
- [Formes](shapes.md)
- [Curseur](slider.md)
- [Bouton Fractionner](buttons.md#create-a-split-button) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Mode Fractionné](split-view.md)
- [Contrôle de balayage](swipe.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Affichage Onglet](tab-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Conseil éducatif](dialogs-and-flyouts/teaching-tip.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Bloc de texte](text-block.md)
- [Zone de texte](text-box.md)
- [Sélecteur d’heure](time-picker.md)
- [Commutateur bascule](toggles.md)
- [Bouton bascule](buttons.md)
- [Bouton bascule partagé](buttons.md#create-a-toggle-split-button)
- [Info-bulles](tooltips.md)
- [Arborescence](tree-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Vue à deux volets](two-pane-view.md) :::image type="icon" source="images/winui-logo-16x16.png":::
- [Affichage Web](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>Galerie de contrôles XAML

Obtenez l’application _Galerie de contrôles XAML_ à partir du Microsoft Store pour voir ces contrôles et le système Fluent Design en action. L’application est un compagnon interactif de ce site web. Une fois installée, vous pouvez utiliser des liens dans des pages de contrôle individuelles pour lancer l’application et voir le contrôle en action.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>Contrôles supplémentaires

Les contrôles supplémentaires pour le développement Windows sont disponibles auprès de sociétés comme <a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> et <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a>. Ces contrôles fournissent une prise en charge supplémentaire pour les développeurs de l’entreprise et .NET, en optimisant les commandes système standard à l’aide de contrôles et de services personnalisés.