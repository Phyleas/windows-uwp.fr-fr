---
description: Cet article décrit les meilleures pratiques pour créer et afficher des paramètres d’application.
title: Recommandations en matière de paramètres de l’application
ms.assetid: 2D765E90-3FA0-42F5-A5CB-BEDC14C3F60A
label: Guidelines
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b3f3388951dc2e3d854fcb97269e5bfb0449a6e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032402"
---
# <a name="guidelines-for-app-settings"></a>Recommandations en matière de paramètres de l’application

Les paramètres de l’application sont les parties de votre application Windows que l’utilisateur peut personnaliser, via une page de paramètres d’application. Par exemple, une application de lecteur de flux d’actualités peut permettre à l’utilisateur de spécifier les sources d’informations à afficher ou le nombre de colonnes à présenter à l’écran, tandis qu’une application météo peut offrir à l’utilisateur la possibilité de choisir entre les unités de mesure Celsius et Fahrenheit par défaut. Cet article décrit les meilleures pratiques et propose des recommandations pour créer et afficher des paramètres d’application.

## <a name="when-to-provide-a-settings-page"></a>À quel moment proposer une page de paramètres ?

Voici des exemples d’options d’application qui figurent dans une page de paramètres d’application :

- Les options de configuration qui affectent le comportement de l’application et qui sont rarement modifiées, comme le choix de l’unité Celsius ou Fahrenheit comme unité de température par défaut dans une application météo, le changement de paramètres de compte dans une application de messagerie, les paramètres des notifications ou les options d’accessibilité.
- Les options qui dépendent des préférences de l’utilisateur, comme la musique, les effets sonores ou les thèmes de couleur.
- Les informations d’application rarement consultées, telles que la déclaration de confidentialité, l’aide, la version de l’application ou les informations de copyright.

Les commandes qui font partie du flux de travail classique de l’application (par exemple, la modification de la taille du pinceau dans une application artistique) ne doivent pas figurer dans une page de paramètres. Pour plus d’informations sur la disposition des commandes, voir [Informations de base sur la conception des commandes](../basics/commanding-basics.md).

## <a name="general-recommendations"></a>Recommandations générales

- Faites en sorte que les pages de paramètres restent simples et utilisez des contrôles binaires (activé/désactivé). Un [bouton bascule](../controls-and-patterns/toggles.md) constitue généralement le contrôle le mieux adapté pour un paramètre binaire.
- Pour permettre aux utilisateurs de choisir un élément dans un ensemble de 5 options au maximum ayant un lien entre elles, mais mutuellement exclusives, utilisez des [cases d’option](../controls-and-patterns/radio-button.md).
- Créez un point d’entrée pour tous les paramètres d’application de votre page de paramètres d’application.
- Veillez à ce que vos paramètres restent simples. Définissez les valeurs par défaut actives et limitez au minimum le nombre de paramètres.
- Si un utilisateur modifie un paramètre, l’application doit immédiatement refléter cette modification.
- N’incluez pas de commandes qui font partie intégrante du flux de travail courant de l’application.

## <a name="entry-point"></a>Point d’entrée

La manière dont les utilisateurs accèdent à votre page de paramètres d’application doit reposer sur la disposition de votre application.

**Volet de navigation**

Pour une disposition de volet de navigation, les paramètres d’application doivent correspondre au dernier élément de la liste de choix de navigation et être épinglés dans la partie inférieure :

![point d’entrée des paramètres d’application pour un volet de navigation](images/appsettings-nav-settings.png)

**Barre de l’application**

Si vous utilisez une [barre de l’application](../controls-and-patterns/app-bars.md) ou une barre d’outils, placez le point d’entrée des paramètres en tant que l’un des derniers éléments du sous-menu « Plus ». Si vous avez besoin d’améliorer la détectabilité du point d’entrée des paramètres pour votre application, placez le point d’entrée directement dans la barre de l’application plutôt que dans le sous-menu.

![point d’entrée des paramètres d’application pour une barre d’application](../controls-and-patterns/images/appbar_rs2_overflow_icons.png)

**Hub**

Si vous utilisez une disposition de type concentrateur, le point d’entrée des paramètres d’application doit être placé dans le sous-menu « Plus » d’une barre d’application.

**Onglets/sélecteurs de vue**

Dans le cas d’une disposition d’onglets ou de sélecteurs de vue, nous vous déconseillons de placer le point d’entrée des paramètres d’application parmi l’un des premiers éléments de la navigation. Placez plutôt le point d’entrée des paramètres d’application dans le sous-menu « Plus » d’une barre d’application.

**Maître/détails**

Plutôt que d’enfouir profondément le point d’entrée des paramètres d’application dans un volet maître/détails, faites-en le dernier élément épinglé du niveau supérieur du volet maître.

## <a name="layout"></a>Disposition


La fenêtre des paramètres d’application doit s’ouvrir en mode plein écran et remplir toute la fenêtre. Si votre menu de paramètres d’application ne comporte pas plus de quatre groupes de niveau supérieur, ces groupes doivent s’afficher en cascade en descendant d’une colonne.

![disposition d’une page de paramètres d’application sur un appareil de bureau](images/appsettings-layout-navpane-desktop.png)


## <a name="color-mode-settings"></a>Paramètres du « Mode couleur »


Si votre application permet aux utilisateurs de choisir le mode couleur de l’application, présentez ces options à l’aide de [cases d’option](../controls-and-patterns/radio-button.md) ou d’une [zone de liste modifiable](../controls-and-patterns/combo-box.md) avec l’en-tête « Choisir un mode d’application ». Les options doivent être les suivantes :
- Léger
- Foncé
- Windows default

Nous vous recommandons également d’ajouter un lien hypertexte à la page Couleurs de l’application Paramètres Windows, qui permet aux utilisateurs d’accéder au mode d’application par défaut actif et de le modifier. Utilisez la chaîne « Paramètres couleur Windows » pour le texte du lien hypertexte et `ms-settings:colors` pour l’URI.

![Section « Choisir un mode »](images/appsettings_mode.png)

<!--
<div class="microsoft-internal-note">
Detailed redlines showing preferred text strings for the "Choose a mode" section are available on [UNI](https://uni/DesignDepot.FrontEnd/#/ProductNav/2543/0/dv/?t=Windows%7CControls%7CColorMode&f=RS2).
</div>
-->

## <a name="about-section-and-feedback-button"></a>Section « À propos de » et bouton de retour d’expérience


Nous vous recommandons de placer la section « À propos de » dans votre application sous forme de page dédiée ou dans sa propre section. Si vous voulez inclure un bouton « Envoyer un retour d’expérience », placez-le au bas de la page « À propos de cette application ».

Sous un sous-titre « Informations légales », placez les « Conditions d’utilisation » et la « Déclaration de confidentialité » (sous forme de [boutons de lien hypertexte](../controls-and-patterns/hyperlinks.md) avec habillage de texte), ainsi que des informations légales supplémentaires, comme les droits d’auteur.

![section « à propos de cette application » avec bouton « envoyer des commentaires »](images/appsettings-about.png)


## <a name="recommended-page-content"></a>Contenu de page recommandé


Une fois que vous avez répertorié les éléments à inclure dans votre page de paramètres d’application, tenez compte des recommandations suivantes :

- Regroupez les paramètres similaires ou connexes sous une même étiquette de paramètres.
- Faites en sorte que le nombre total de paramètres ne dépasse pas quatre ou cinq.
- Affichez les mêmes paramètres, quel que soit le contexte de l’application. Si certains paramètres ne sont pas pertinents dans un contexte spécifique, désactivez-les dans le menu volant des paramètres d’application.
- Utilisez des étiquettes descriptives composées d’un seul mot pour les paramètres. Par exemple, nommez l’entrée « Comptes » plutôt que « Paramètres de compte » pour les paramètres liés aux comptes. Si vous ne voulez qu’une seule option pour vos paramètres et que ces derniers ne se prêtent pas à une étiquette descriptive, utilisez « Options » ou « Par défaut ».
- Si un paramètre est directement lié au web plutôt qu’à un menu volant, informez-en l’utilisateur à l’aide d’un indice visuel, par exemple « Aide (en ligne) » ou « Forums web », doté d’un style de [lien hypertexte](../controls-and-patterns/hyperlinks.md). Envisagez de regrouper plusieurs liens vers le web dans un menu volant constitué d’un seul paramètre. Par exemple, un paramètre « À propos de » peut ouvrir un menu volant proposant des liens vers vos conditions d’utilisation, votre déclaration de confidentialité et le support technique de votre application.
- Combinez les paramètres peu utilisés dans une même entrée afin que chaque paramètre plus courant puisse disposer de sa propre entrée. Placez le contenu ou les liens qui contiennent uniquement des informations dans un paramètre « À propos de ».
- Ne dupliquez pas la fonctionnalité du volet « Autorisations ». Windows fournit ce volet par défaut et ne vous permet pas de le modifier.

- Ajouter le contenu des paramètres aux menus volants des paramètres
- Présentez le contenu de haut en bas dans une seule colonne que l’utilisateur pourra faire défiler, si nécessaire. Limitez le défilement à un maximum de deux fois la hauteur de l’écran.
- Utilisez les contrôles suivants pour les paramètres d’application :

    - [Boutons bascule](../controls-and-patterns/toggles.md) : permettent aux utilisateurs de définir des valeurs Activé ou Désactivé.
    - [Cases d’option](../controls-and-patterns/radio-button.md) : permettent à l’utilisateur de choisir un élément dans un ensemble maximal de 5 options ayant un lien entre elles, mais mutuellement exclusives.
    - [Zone de texte](../controls-and-patterns/text-block.md) : permet aux utilisateurs d’entrer du texte. Utilisez le type de zone de texte qui correspond au type de texte que vous obtenez de l’utilisateur, tel qu’un message électronique ou un mot de passe.
    - [Liens hypertexte](../controls-and-patterns/hyperlinks.md) : dirigent l’utilisateur vers une autre page de l’application ou vers un site web externe. Quand un utilisateur clique sur un lien hypertexte, le menu volant des paramètres se ferme.
    - [Boutons](../controls-and-patterns/buttons.md) : permettent aux utilisateurs de lancer une action immédiate sans quitter l’actuel menu volant des paramètres.
- Ajoutez un message descriptif si l’un des contrôles est désactivé. Placez ce message au-dessus du contrôle désactivé.
- Une fois l’animation du menu volant Paramètres et de l’en-tête terminée, animez le contenu et les contrôles dans un bloc unique. Animez le contenu à l’aide des animations [**enterPage**](/previous-versions/windows/apps/br212672(v=win.10)) ou [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition) avec un décalage gauche de 100 px.
- Utilisez des en-têtes de section, des paragraphes et des étiquettes pour vous aider à organiser et clarifier le contenu, si nécessaire.
- Si vous avez besoin de répéter des paramètres, utilisez un niveau supplémentaire d’interface utilisateur ou un modèle développer/réduire, mais évitez les hiérarchies de plus de deux niveaux. Par exemple, une application météo qui fournit des paramètres par ville peut répertorier les villes et permettre à l’utilisateur d’appuyer sur la ville pour ouvrir un nouveau menu volant ou développer les options de paramètres.
- Si le chargement des contrôles ou du contenu Web prend du temps, utilisez le contrôle de progression indéterminée pour indiquer aux utilisateurs que les informations sont en cours de chargement. Pour plus d’informations, consultez [Recommandations en matière de contrôles de progression](../controls-and-patterns/progress-controls.md).
- N’utilisez pas de boutons pour la navigation ou pour valider des modifications. Utilisez des liens hypertexte pour accéder à d’autres pages et, au lieu d’utiliser un bouton pour valider les modifications, enregistrez automatiquement les modifications apportées aux paramètres d’application quand un utilisateur ferme le menu volant des paramètres.



## <a name="related-articles"></a>Articles connexes

* [Informations de base sur la conception des commandes](../basics/commanding-basics.md)
* [Recommandations en matière de contrôles de progression](../controls-and-patterns/progress-controls.md)
* [Stocker et récupérer des données d’application](./store-and-retrieve-app-data.md)
* [**EntranceThemeTransition**](/uwp/api/Windows.UI.Xaml.Media.Animation.EntranceThemeTransition)
