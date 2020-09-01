---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: Cette étude de cas, qui repose sur les informations fournies dans Bookstore1, commence par une application 8.1 universelle qui affiche des données groupées dans un contrôle SemanticZoom.
title: 'Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore2'
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b2681eb1fd14ed7e86c00054c5f5c7ba3efb97c6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167613"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Étude de cas de portage d’application Windows Runtime 8.x vers UWP : Bookstore2


Cette étude de cas, qui repose sur les informations fournies dans [Bookstore1](w8x-to-uwp-case-study-bookstore1.md), commence par une application 8.1 universelle qui affiche des données groupées dans un contrôle [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom). Dans le modèle de vue, chaque instance de l' **auteur** de la classe représente le groupe de livres écrits par cet auteur, et dans le **SemanticZoom**, nous pouvons soit afficher la liste des livres regroupés par auteur, soit effectuer un zoom arrière pour afficher une liste de raccourcis des auteurs. Grâce à cette liste, vous pouvez vous déplacer beaucoup plus rapidement que si vous faisiez défiler la liste des ouvrages. Nous suivons la procédure de portage de l’application vers une application de plateforme Windows universelle (UWP) Windows 10.

**Remarque**    Lors de l’ouverture \_ de Bookstore2Universal 10 dans Visual Studio, si vous voyez le message « mise à jour requise de Visual Studio », suivez les étapes décrites dans [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Téléchargements

[Téléchargez l' \_ application Bookstore2 81 Universal 8,1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81).

[Téléchargez l' \_ application Bookstore2Universal 10 Windows 10](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

## <a name="the-universal-81-app"></a>Application 8.1 universelle

Voici \_ à quoi ressemble Bookstore2 81 : l’application que nous allons porter. Il s’agit d’un défilement horizontal (défilement vertical sur Windows Phone) [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) présentant des livres regroupés par auteur. Vous pouvez effectuer un zoom arrière vers la liste de raccourcis et, à partir de cette dernière, revenir à n’importe quel groupe. Cette application comporte deux parties principales : le modèle d’affichage, qui fournit la source de données groupées, et l’interface utilisateur, qui est liée à ce modèle d’affichage. Comme nous allons le voir, ces deux parties sont facilement portées depuis la technologie WinRT 8.1 vers Windows 10.

![Bookstore2 \- 81 sur Windows, vue zoomée](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Bookstore2 \_ 81 sur Windows, vue zoomée
 

![Bookstore2 \- 81 sur Windows, vue zoomée](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Bookstore2 \_ 81 sur Windows, vue zoomée

![Bookstore2 \- 81 sur Windows Phone, vue zoomée](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2 \_ 81 sur Windows Phone, vue zoomée

![Bookstore2 \- 81 sur Windows Phone, vue zoomée](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2 \_ 81 sur Windows Phone, vue zoomée

##  <a name="porting-to-a-windows10-project"></a>Portage d’une application vers un projet Windows 10

La \_ solution Bookstore2 81 est un projet d’application universelle 8,1. Le \_ projet Bookstore2 81. Windows génère le package d’application pour Windows 8.1, et le \_ projet Bookstore2 81. windowsphone génère le package d’application pour Windows Phone 8,1. Bookstore2 \_ 81. Shared est le projet qui contient le code source, les fichiers de balisage et d’autres ressources et ressources, qui sont utilisées par les deux autres projets.

Comme dans l’étude de cas précédente, l’option que nous allons sélectionner (parmi celles qui sont décrites à la section [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md)) consiste à porter le contenu du projet partagé vers une application Windows 10 qui cible la famille d’appareils universelle.

Commencez par créer un projet Application vide (universelle Windows). Nommez-le Bookstore2Universal \_ 10. Il s’agit des fichiers à copier à partir de Bookstore2 \_ 81 vers Bookstore2Universal \_ 10.

**Dans le projet partagé**

-   Copiez le dossier contenant les fichiers PNG de l’image de couverture du livre (le dossier est \\ ressources \\ CoverImages). Une fois le dossier copié, dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit de la souris sur le dossier que vous avez copié et sélectionnez **Inclure dans le projet**. Cette commande correspond à ce que nous avons voulu dire par « insertion » de fichiers ou de dossiers dans un projet. Chaque fois que vous copiez un fichier ou un dossier, pour chaque copie, sélectionnez **Actualiser** dans **Explorateur de solutions**, puis incluez le fichier ou dossier dans le projet. Cette opération n’est pas nécessaire pour les fichiers dont vous modifiez la destination.
-   Copiez le dossier contenant le fichier source du modèle d’affichage (le dossier est \\ ViewModel).
-   Copiez le fichier MainPage.xaml et remplacez le fichier dans la destination.

**Dans le projet Windows**

-   Copiez le fichier BookstoreStyles.xaml. Nous l’utiliserons comme point de départ, car l’ensemble des clés de ressources figurant dans ce fichier sera résolu dans une application Windows 10, alors que certaines clés de ressources du fichier WindowsPhone correspondant ne le seront pas.
-   Copiez les fichiers SeZoUC.xaml et SeZoUC.xaml.cs. Nous allons commencer par la version de cet affichage pour Windows, qui est appropriée pour les fenêtres étendues. Ensuite, nous adapterons ce contenu à des fenêtres plus petites et, de ce fait, à des appareils plus petits.

Modifiez le code source et les fichiers de balisage que vous venez de copier, puis remplacez toutes les références à l' \_ espace de noms Bookstore2 81 par Bookstore2Universal \_ 10. Une méthode rapide consiste à utiliser la fonctionnalité **Remplacer dans les fichiers**. Aucune modification du code n’est nécessaire dans le modèle d’affichage, ni dans tout autre code impératif. Mais, juste pour faciliter la visibilité de la version de l’application en cours d’exécution, remplacez la valeur retournée par la propriété **Bookstore2Universal \_ 10. BookstoreViewModel. AppName** « Bookstore2 \_ 81 » par « Bookstore2Universal \_ 10 ».

Vous pouvez maintenant générer l’application et l’exécuter. Voici à quoi ressemble notre nouvelle application UWP, sans aucun effort de portage vers Windows 10 pour l’instant.

![Application Windows 10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

L’application Windows 10 avec les modifications de code source initiales s’exécutant sur un appareil de bureau, vue zoomée

![Application Windows 10 avec le code source initial modifié, exécutée sur un appareil de bureau (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

L’application Windows 10 avec les modifications de code source initiales s’exécutant sur un appareil de bureau, vue zoomée

Le modèle d’affichage et les vues avec zoom avant et arrière fonctionnent ensemble correctement. Cependant, il existe certains problèmes, qui les rendent difficiles à voir. Exemple de problème : l’élément [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) ne défile pas. Cela est dû au fait que, dans Windows 10, le style par défaut d’un contrôle [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) entraîne sa mise en forme verticale (et les règles de conception de Windows 10 recommandent de l’utiliser de cette façon dans les applications nouvelles et dans les applications portées). Toutefois, les paramètres de défilement horizontal dans le modèle de panneau éléments personnalisés que nous avons copiés à partir du \_ projet Bookstore2 81 (qui a été conçu pour l’application 8,1) sont en conflit avec les paramètres de défilement vertical dans le style par défaut de Windows 10 qui est appliqué suite à un portage vers une application Windows 10. Deuxième problème : l’application ne peut pas encore adapter son interface utilisateur à l’appareil de l’utilisateur, afin de lui offrir la meilleure expérience possible, quelle que soit la taille des fenêtres et de l’appareil. Enfin, les styles et pinceaux corrects ne sont pas encore utilisés, ce qui entraîne l’invisibilité d’une partie du texte (cela inclut les en-têtes de groupe sur lesquels vous pouvez cliquer pour effectuer un zoom arrière). Ainsi, dans les trois sections suivantes ([Modifications de conception des éléments SemanticZoom et GridView](#semanticzoom-and-gridview-design-changes), [L’interface utilisateur adaptative](#adaptive-ui), et [Stylisation universelle](#universal-styling)) nous allons remédier à ces problèmes.

## <a name="semanticzoom-and-gridview-design-changes"></a>Modifications de conception des éléments SemanticZoom et GridView

Les modifications apportées à la conception du contrôle [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dans Windows 10 sont décrites à la section [Modifications SemanticZoom](w8x-to-uwp-porting-xaml-and-ui.md). Nous n’avons aucune tâche à effectuer dans cette section suite à ces modifications.

Les modifications apportées à l’élément [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) sont décrites dans la section [Modifications GridView/ListView](w8x-to-uwp-porting-xaml-and-ui.md). Nous devons effectuer quelques réglages mineurs pour nous adapter à ces modifications, comme décrit ci-dessous.

-   Dans l’élément `ZoomedInItemsPanelTemplate` du fichier SeZoUC.xaml, définissez les paramètres `Orientation="Horizontal"` et `GroupPadding="0,0,0,20"`.
-   Toujours dans ce fichier, supprimez l’élément `ZoomedOutItemsPanelTemplate`, puis supprimez l’attribut `ItemsPanel` de la vue avec zoom arrière.

C’est tout !

## <a name="adaptive-ui"></a>Interface utilisateur adaptative

Une fois cette modification effectuée, la disposition de l’interface utilisateur proposée par le fichier SeZoUC.xaml est très utile lorsque l’application s’exécute dans une fenêtre large (opération uniquement possible sur un appareil doté d’un grand écran). Par contre, lorsque la fenêtre d’application est étroite (comme sur un appareil de petite taille, voire sur certains appareils plus grands), l’interface utilisateur proposée par l’application du Windows Phone Store est sans aucun doute la plus appropriée.

Nous pouvons utiliser la fonction Gestionnaire d’état visuel adaptative pour y parvenir. Nous allons définir des propriétés sur les éléments visuels afin que, par défaut, l’interface utilisateur présente une disposition étroite à l’aide des plus petits modèles que nous utilisions dans l’application du Windows Phone Store. Ensuite, nous détecterons les cas où la fenêtre de l’application est d’une largeur égale ou supérieure à une taille spécifique (mesurée en [pixels effectifs](w8x-to-uwp-porting-xaml-and-ui.md)), et nous modifierons alors les propriétés des éléments visuels comme il convient afin d’obtenir une disposition plus grande et plus large. Nous affecterons à ces changements de propriété un état visuel ; nous utiliserons un déclencheur adaptatif pour effectuer un suivi en continu et déterminer si l’état visuel doit ou non être appliqué, selon la largeur de la fenêtre en pixels effectifs. Dans ce cas précis, nous baserons le déclenchement sur la largeur de la fenêtre, mais il est également possible de choisir la hauteur de la fenêtre comme déclencheur.

Une largeur minimale de 548 epx est appropriée pour ce cas d’utilisation, car cette valeur correspond à la taille de l’appareil le plus petit auquel nous voulons appliquer la disposition d’écran large. En général, les téléphones présentent une largeur inférieure à 548 epx. Ainsi, sur ce type d’appareil de petite taille, nous conserverons la disposition étroite par défaut. Sur un PC, par défaut, la fenêtre s’ouvre sur une largeur suffisante pour déclencher le passage à l’état large. À partir de là, vous serez en mesure de faire glisser la fenêtre afin qu’elle soit suffisamment étroite pour afficher deux colonnes pour les éléments d’une taille de 250 x 250. Si vous utilisez une largeur inférieure, le déclencheur sera désactivé, l’état visuel large sera supprimé, et la disposition étroite par défaut continuera d’être appliquée.

Ainsi, quelles sont les propriétés que nous devons définir—et modifier—pour bénéficier de ces deux dispositions ? Il existe deux solutions, qui font chacune appel à une méthode différente.

1.  Nous pouvons placer deux contrôles [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dans notre balisage. Il peut s’agir d’une copie de la balise que nous utilisons dans le Windows Runtime application 8. x (à l’aide de contrôles [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) à l’intérieur de celle-ci) et réduite par défaut. Quant à l’autre, il correspond à une copie du balisage que nous avons utilisé dans l’application du Windows Phone Store (en utilisant des contrôles [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) à l’intérieur de cette copie), visible par défaut. L’état visuel fait alors basculer les propriétés de visibilité des deux contrôles **SemanticZoom**. L’exécution de cette technique nécessite peu d’efforts, mais ses performances sont généralement moyennes. Ainsi, si vous l’utilisez, vous devez profiler votre application et vous assurer qu’elle répond toujours à vos objectifs en termes de performances.
2.  Nous pouvons utiliser un seul élément [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) contenant des contrôles [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView). Pour obtenir nos deux dispositions, dans l’état visuel large, nous modifions les propriétés des contrôles **ListView**, y compris des modèles qui leur sont appliqués, pour entraîner leur disposition de la même manière que celle d’un élément [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView). Cette solution offre des performances sans doute meilleures, mais se révèle également la plus difficile à appliquer, à cause du nombre de petites différences entre les styles et modèles des éléments **GridView** et **ListView**, et entre leurs divers types d’éléments. Cette solution est étroitement associée au mode de conception des styles et modèles par défaut à ce moment précis ; nous obtenons une solution fragile et sensible aux éventuelles modifications futures des valeurs par défaut.

Dans cette étude de cas, nous allons choisir la première solution. Cependant, vous pouvez choisir la deuxième méthode, si elle vous convient mieux. Voici les étapes à suivre pour implémenter cette première solution.

-   Sur le [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dans le balisage de votre nouveau projet, définissez `x:Name="wideSeZo"` et `Visibility="Collapsed"` .
-   Revenez au projet Bookstore2 \_ 81. windowsphone et ouvrez SeZoUC. Xaml. Copiez le balisage de l’élément [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) de ce fichier et collez-le juste après l’élément `wideSeZo` dans votre nouveau projet. Définissez `x:Name="narrowSeZo"` sur l’élément que vous venez de coller.
-   Toutefois, l’élément `narrowSeZo` nécessite quelques styles que nous n’avons pas encore copiés. Là encore, dans Bookstore2 \_ 81. windowsphone, copiez les deux styles ( `AuthorGroupHeaderContainerStyle` et `ZoomedOutAuthorItemContainerStyle` ) en dehors de SeZoUC. xaml et collez-les dans BookstoreStyles. xaml dans votre nouveau projet.
-   Vous avez maintenant deux éléments [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dans votre nouveau SeZoUC. Xaml. Incluez ces deux éléments dans un élément **Grid**.
-   Dans le fichier BookstoreStyles.xaml de votre nouveau projet, ajoutez le mot `Wide` à ces trois clés de ressources (et à leurs références dans le fichier SeZoUC.xaml, mais uniquement aux références à l’intérieur de l’élément `wideSeZo`) : `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate`, et `BookTemplate`.
-   Dans le \_ projet Bookstore2 81. windowsphone, ouvrez BookstoreStyles. Xaml. À partir de ce fichier, copiez ces trois mêmes ressources (mentionnées ci-dessus), et les deux convertisseurs d’élément de liste de saut, et la déclaration de préfixe d’espace de noms Windows \_ interface \_ XAML \_ contrôles \_ primitives, puis collez-les dans BookstoreStyles. xaml dans votre nouveau projet.
-   Enfin, dans le fichier SeZoUC.xaml de votre nouveau projet, ajoutez le balisage de gestionnaire d’état visuel approprié pour l’élément **Grid** que vous avez ajouté ci-dessus.

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>Stylisation universelle

Nous allons désormais résoudre certains problèmes de style, y compris celui que nous avons introduit ci-dessus lors de l’opération de copie à partir de l’ancien projet.

-   Dans le fichier MainPage.xaml, remplacez l’arrière-plan de l’élément `LayoutRoot` par `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Dans le fichier BookstoreStyles.xaml, définissez la valeur de la ressource `TitlePanelMargin` sur `0` (ou sur toute valeur qui vous convienne).
-   Dans le fichier SeZoUC.xaml, définissez la marge de l’élément `wideSeZo` sur `0` (ou sur toute valeur qui vous convienne).
-   Dans le fichier BookstoreStyles.xaml, supprimez l’attribut Margin de l’élément `AuthorGroupHeaderTemplateWide`.
-   Supprimez l’attribut FontFamily des éléments `AuthorGroupHeaderTemplate` et `ZoomedOutAuthorTemplate`.
-   Bookstore2 \_ 81 a utilisé `BookTemplateTitleTextBlockStyle` les `BookTemplateAuthorTextBlockStyle` clés de `PageTitleTextBlockStyle` ressource, et comme une indirection, de sorte qu’une clé unique avait des implémentations différentes dans les deux applications. Nous n’avons plus besoin de cette indirection ; nous pouvons référencer directement les styles du système. Par conséquent, dans toute l’application, remplacez ces références par `TitleTextBlockStyle`, `CaptionTextBlockStyle` et `HeaderTextBlockStyle`, respectivement. Vous pouvez également utiliser la fonction **Remplacer dans les fichiers** de Visual Studio pour effectuer cette opération rapidement et de manière précise. Vous pouvez ensuite supprimer ces trois ressources inutilisées.
-   Dans `AuthorGroupHeaderTemplate`, remplacez l’élément `PhoneAccentBrush` par `SystemControlBackgroundAccentBrush` et définissez `Foreground="White"` sur l’élément **TextBlock** afin qu’il apparaisse correctement lors de l’exécution sur la famille d’appareils mobiles.
-   Dans l’élément `BookTemplateWide`, copiez l’attribut Foreground du deuxième élément **TextBlock** vers le premier.
-   Dans l’élément `ZoomedOutAuthorTemplateWide`, remplacez la référence à `SubheaderTextBlockStyle` (un peu trop importante, désormais) par une référence à `SubtitleTextBlockStyle`.
-   La vue avec zoom arrière (liste de raccourcis) ne se superpose plus à la vue avec zoom avant dans la nouvelle plateforme. De ce fait, nous pouvons supprimer l’attribut `Background` de la vue avec zoom arrière de l’élément `narrowSeZo`.
-   Pour que tous les styles et modèles figurent dans un seul fichier, retirez l’élément `ZoomedInItemsPanelTemplate` du fichier SeZoUC.xaml et placez-le dans le fichier BookstoreStyles.xaml.

Une fois cette séquence d’opérations de stylisation effectuée, l’application ressemble à ceci.

![Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom avant et deux tailles de fenêtres)](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom avant et deux tailles de fenêtres)

![Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom arrière et deux tailles de fenêtres)](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom arrière et deux tailles de fenêtres)

![Application Windows 10 portée, exécutée sur un appareil mobile (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile, vue zoomée

![Application Windows 10 portée, exécutée sur un appareil mobile (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile, vue zoomée

## <a name="conclusion"></a>Conclusion

Cette étude de cas reposait sur une interface utilisateur plus ambitieuse que celle de l’étude précédente. Comme pour l’étude de cas précédente, ce modèle d’affichage particulier n’a requis aucun effort. Nous avons surtout dû nous concentrer sur la refactorisation de l’interface utilisateur. Certaines modifications inévitables ont été le fruit de la combinaison de deux projets en un seul, qui devait toujours prendre en charge de nombreux facteurs de forme (en fait, nettement plus qu’auparavant). Certaines de ces modifications étaient liées aux changements apportés à la plateforme.

Dans l’étude de cas suivante, [QuizGame](w8x-to-uwp-case-study-quizgame.md), nous examinons l’accès aux données groupées et leur affichage.