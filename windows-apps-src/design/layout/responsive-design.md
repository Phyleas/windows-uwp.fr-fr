---
Description: Découvrez les techniques de conception réactive pour adapter votre application à des appareils spécifiques
title: Techniques de conception réactive
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68867415"
---
# <a name="responsive-design-techniques"></a>Techniques de conception réactive

Les applications UWP utilisent les pixels effectifs pour garantir la lisibilité et la convivialité de votre interface utilisateur sur tous les appareils Windows. Dès lors, pourquoi souhaiteriez-vous personnaliser l’interface utilisateur de votre application pour une famille d’appareils spécifique ?

- **Pour tirer le meilleur parti de l’espace et limiter la navigation**

    Si vous concevez une application pour qu’elle ait une apparence appropriée sur un appareil doté d’un petit écran, par exemple une tablette, l’application pourra être utilisée sur un PC équipé d’un écran beaucoup plus grand, mais de l’espace sera probablement inutilisé. Vous pouvez personnaliser l’application pour afficher davantage de contenu lorsque la taille de l’écran est supérieure à une certaine valeur. Par exemple, une application d’achat peut afficher une catégorie de marchandise à la fois sur une tablette, mais afficher simultanément plusieurs catégories et produits sur un PC ou un ordinateur portable.

    En plaçant plus de contenu à l’écran, vous réduisez les étapes de navigation que l’utilisateur doit effectuer.

- **Pour tirer parti des fonctionnalités des appareils**

    Certains appareils sont plus susceptibles d’être dotés de fonctionnalités particulières. Par exemple, les ordinateurs portables sont susceptibles d’être équipés d’un capteur d’emplacement et d’un appareil photo, contrairement à un téléviseur. Votre application peut détecter les fonctionnalités qui sont disponibles et les activer.

- **Pour optimiser les entrées**

    La bibliothèque de contrôles universels fonctionne avec tous les types d’entrée (tactile, stylet, clavier, souris), mais vous pouvez toujours optimiser certains types d’entrée en réorganisant vos éléments d’interface utilisateur. Par exemple, si vous placez des éléments de navigation en bas de l’écran, ils seront plus facilement accessibles aux utilisateurs de téléphone, mais la plupart des utilisateurs de PC s’attendent à voir des éléments de navigation en haut de l’écran.

Lorsque vous optimisez l’interface utilisateur de votre application pour des largeurs d’écran spécifiques, nous disons que vous créez une conception réactive. Voici six techniques de conception réactive que vous pouvez utiliser pour personnaliser l’interface utilisateur de votre application.

>[!TIP]
> De nombreux contrôles UWP implémentent automatiquement ces comportements réactifs. Pour créer une interface utilisateur réactive, nous vous recommandons d'utiliser les [contrôles UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Repositionner

Vous pouvez modifier l’emplacement et la position des éléments de l’interface utilisateur pour tirer le meilleur parti de la taille de la fenêtre. Dans cet exemple, la plus petite fenêtre empile les éléments verticalement. Lorsque l’application se traduit par une plus grande fenêtre, les éléments peuvent tirer parti d'une plus importante largeur.

![Repositionner](images/rsp-design/rspd-reposition2.gif)

Dans cet exemple de conception pour une application de photos, l’application repositionne son contenu sur des écrans plus grands.

## <a name="resize"></a>Redimensionner

Vous pouvez optimiser la taille de la fenêtre en ajustant les marges et la taille des éléments de l’interface utilisateur. Par exemple, cela peut enrichir l’expérience de lecture sur un écran plus grand en augmentant simplement le cadre de contenu.

![Redimensionnement des éléments de conception](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Ajuster dynamiquement

En modifiant le flux des éléments de l’interface utilisateur en fonction de l’appareil et de l’orientation, votre application peut offrir un affichage de contenu optimal. Par exemple, lors du passage à un écran plus grand, il peut être judicieux d'ajouter des colonnes, d'utiliser des conteneurs plus grands ou de  générer des éléments de liste d’une manière différente.

Cet exemple montre comment une colonne unique de contenu à défilement vertical sur un plus petit écran peut être réorganisée sur un écran plus grand pour afficher deux colonnes de texte.

![Ajustement dynamique des éléments de conception](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Afficher/Masquer

Vous pouvez afficher ou masquer des éléments d’interface utilisateur en fonction de l’espace disponible à l’écran, ou lorsque l’appareil prend en charge des fonctionnalités supplémentaires, des situations spécifiques ou des orientations d’écran favorites.

![Masquage des éléments de conception](images/rsp-design/rspd-revealhide.gif)

Par exemple, les contrôles du lecteur multimédia réduisent l'ensemble de boutons sur les plus petits écrans et le développent sur les plus grands écrans. Le lecteur multimédia dans une plus grande fenêtre, par exemple, peut gérer beaucoup plus de fonctionnalités à l’écran que dans une plus petite fenêtre.

Une partie de la technique de révélation ou de masquage comprend le choix de l’affichage des métadonnées. Avec de plus petites fenêtres, il est préférable d’afficher une quantité minimale de métadonnées. Dans de plus grandes fenêtres, une quantité importante de métadonnées peut être exposée. Voici quelques exemples où il apparaît intéressant d'afficher ou de masquer des métadonnées :

- Dans une application de messagerie, vous pouvez afficher l’avatar de l’utilisateur.
- Dans une application de musique, vous pouvez afficher davantage d’informations sur un album ou un artiste.
- Dans une application vidéo, vous pouvez afficher plus d’informations sur un film ou une émission, en présentant par exemple des détails sur la distribution et l’équipe technique.
- Dans n’importe quelle application, vous pouvez décomposer les colonnes et révéler plus de détails.
- Dans n’importe quelle application, vous pouvez disposer un élément à l’horizontal alors qu’il était empilé verticalement. Lorsque vous passez d’un téléphone ou d’une phablette à un appareil plus grand, les éléments de liste empilés peuvent changer pour faire apparaître des lignes d’éléments de liste et des colonnes de métadonnées.

## <a name="replace"></a>Remplacer

Cette technique vous permet de changer d’interface utilisateur pour des points d'arrêt spécifiques. Dans cet exemple, le volet de navigation et son interface utilisateur compacte et temporaire fonctionnent bien pour un plus petit écran, mais des onglets peuvent être plus appropriés pour un plus grand écran.

![Remplacement des éléments de conception](images/rsp-design/rspd-replace.gif)

Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) prend en charge cette technique réactive et permet aux utilisateurs de définir la position du volet en haut ou à gauche.

## <a name="re-architect"></a>Remodéliser

Vous pouvez réduire ou répliquer l’architecture de votre application pour mieux cibler des appareils spécifiques. Dans cet exemple, le développement de la fenêtre affiche l’intégralité du modèle maître/détails.

![exemple de réorganisation d’une interface utilisateur](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Rubriques connexes

- [Tailles d’écran et points d’arrêt](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Dispositions réactives avec XAML](layouts-with-xaml.md)
- [Contrôles et modèles UWP](../controls-and-patterns/index.md)
