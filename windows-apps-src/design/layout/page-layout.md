---
title: Mise en page pour les applications UWP
description: Lors de la conception de votre application, la première chose à prendre en compte est la structure de la disposition. Cet article présente la structure commune des mises en page de base, y compris les éléments d’interface utilisateur dont vous aurez besoin et leur emplacement dans une page. Dans les applications UWP, chaque page comporte généralement des éléments de navigation, de commande et de contenu.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684547"
---
# <a name="page-layout"></a>Mise en page

Dans les applications UWP, chaque [**page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) comporte généralement des éléments de navigation, de commande et de contenu. 

Votre application peut comporter plusieurs pages : lorsqu’un utilisateur lance une application UWP, le code de l’application crée un [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) à placer dans la [**fenêtre**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)de l’application. Le frame peut alors [naviguer](../basics/navigate-between-two-pages.md) entre les instances de [**page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) de l’application. 

La plupart des pages suivent une structure de disposition commune et cet article décrit les éléments d’interface utilisateur dont vous aurez besoin, ainsi que leur emplacement sur une page. 

![structure de la page](images/page-components.svg)

## <a name="navigation"></a>Navigation
La disposition de votre application commence par le modèle de navigation que vous choisissez, qui définit la façon dont vos utilisateurs naviguent entre les pages de votre application. Pour cet article, nous allons aborder deux modèles de navigation courants : navigation de gauche et navigation en haut. Pour obtenir des conseils sur la sélection d’autres options de navigation, consultez [concepts de base de la conception de navigation pour les applications UWP](../basics/navigation-basics.md).

![modèles de navigation en haut et à gauche](images/top-left-nav.svg)

### <a name="left-nav"></a>Navigation de gauche
La navigation de gauche ou le modèle de [volet](../controls-and-patterns/navigationview.md) de navigation, est généralement réservée pour la navigation au niveau de l’application et existe au niveau le plus élevé de l’application, ce qui signifie qu’elle doit toujours être visible et disponible. Nous vous recommandons d’utiliser la navigation de gauche lorsqu’il y a plus de cinq éléments de navigation, ou plus de cinq pages dans votre application. Le modèle du volet de navigation contient généralement les éléments suivants :
- Éléments de navigation
- Point d’entrée dans les paramètres de l’application
- Point d’entrée dans les paramètres de compte

Le contrôle [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) implémente le modèle de navigation de gauche pour UWP.

Lorsqu’un élément de navigation est sélectionné, le frame doit accéder à la page de l’élément sélectionné.

![volet de navigation développé](images/navview-expanded.svg)

Le bouton de menu permet aux utilisateurs de développer et de réduire le volet de navigation. Lorsque la taille de l’écran est supérieure à 640 PX, le fait de cliquer sur le bouton de menu réduit le volet de navigation dans une barre.

![volet de navigation compact](images/navview-compact.svg)

Lorsque la taille de l’écran est inférieure à 640 PX, le volet de navigation est entièrement réduit.

![volet de navigation minimal](images/navview-minimal.svg)

### <a name="top-nav"></a>Navigation en haut

Le point de navigation en haut peut également servir de navigation de niveau supérieur. Tandis que la navigation de gauche est réductible, le point de début est toujours visible. Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) implémente le modèle de navigation et d’onglets supérieur pour UWP.

![navigation en haut](images/pivot-large.svg)

## <a name="command-bar"></a>Barre de commandes

Ensuite, vous souhaiterez peut-être fournir aux utilisateurs un accès facile aux tâches les plus courantes de votre application. Une [barre de commandes](../controls-and-patterns/app-bars.md) peut fournir l’accès aux commandes au niveau de l’application ou au niveau de la page, et elle peut être utilisée avec n’importe quel modèle de navigation.

![positionnement de la barre de commandes en haut ](images/app-bar-desktop.svg)

La barre de commandes peut être placée en haut ou en bas de la page, selon ce qui est le mieux adapté à votre application.

![positionnement de la barre de commandes en bas](images/app-bar-mobile.svg)

## <a name="content"></a>Content

Enfin, le contenu varie considérablement d’une application à l’autre, ce qui vous permet de présenter du contenu de plusieurs façons. Ici, nous décrivons des modèles de page courants que vous pouvez utiliser dans votre application. De nombreuses applications utilisent l’ensemble ou une partie de ces modèles de page pour afficher différents types de contenu. De même, n’hésitez pas à mélanger et à faire correspondre ces modèles pour optimiser votre application.

## <a name="landing"></a>Accueil

![page d’accueil](images/hero-screen.svg)

Les pages d’accueil, également connues sous le nom d’écrans bannière, apparaissent souvent au niveau supérieur d’une expérience d’application. La grande surface sert à exposer les applications, et plus particulièrement à mettre en avant le contenu que les utilisateurs peuvent vouloir parcourir et utiliser.

## <a name="collections"></a>Regroupements

![galerie](images/gridview.svg)

Les collections permettent aux utilisateurs de parcourir les groupes de contenu ou de données. L’[affichage Grille](../controls-and-patterns/item-templates-gridview.md) est idéal pour les photos ou les contenus centrés sur les médias ; le [mode Liste](../controls-and-patterns/item-templates-listview.md) est, quant à lui, plus particulièrement destiné aux données ou aux contenus riches en texte.

## <a name="masterdetail"></a>Maître/détail

![maître et détails](images/master-detail.svg)

Le modèle [maître/détails](../controls-and-patterns/master-details.md) se compose du mode Liste (maître) et d’une vue de contenu (détail). Ces deux volets sont figés et offrent un défilement vertical. Lorsqu’un élément de la vue liste est sélectionné, l’affichage du contenu est mis à jour en conséquence. 

## <a name="forms"></a>Formulaires
![formulaire](images/form.svg)

Un [formulaire](../controls-and-patterns/forms.md) est un groupe de contrôles qui collectent des données auprès de l’utilisateur et les envoient. La plupart des applications, voire toutes, utilisent un formulaire pour les pages de paramètres, les portails de connexion, les hubs de commentaires, la création de compte, etc. 

## <a name="sample-apps"></a>Exemples d’apps
Pour voir comment ces modèles peuvent être implémentés, consultez nos [exemples d’applications UWP](https://developer.microsoft.com/windows/samples):
- [Lecteur vidéo BuildCast](https://github.com/Microsoft/BuildCast)
- [Planificateur de déjeuners](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Clients commande la base de données](https://github.com/Microsoft/Windows-appsample-customers-orders-database)