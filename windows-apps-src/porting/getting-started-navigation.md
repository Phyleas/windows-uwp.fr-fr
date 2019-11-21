---
title: 'Prise en main : Navigation'
description: 'Prise en main : Navigation'
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcbc8f6737c2b7450e42ed01a752087d6e9034c1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259150"
---
# <a name="getting-started-navigation"></a>Prise en main : Navigation


## <a name="adding-navigation"></a>Ajout de la navigation

iOS fournit la classe **UINavigationController** pour faciliter la navigation : vous pouvez pousser et faire apparaître des affichages pour créer une hiérarchie de **UIViewControllers** qui définissent votre application.

En revanche, une application Windows 10 contenant plusieurs vues prend plus d’une approche de site Web pour la navigation. Vous pouvez imaginer vos utilisateurs passant de page en page en cliquant sur des contrôles tandis qu’ils explorent l’application. Pour plus d’informations, voir [Informations de base relatives à la conception de la navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics).

L’une des façons de gérer cette navigation dans une application Windows 10 consiste à utiliser la classe [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) . La procédure pas à pas suivante vous montre comment tester et mettre en pratique cette opération.

Sans quitter la solution commencée précédemment, ouvrez le fichier **MainPage.xaml**, puis ajoutez un bouton dans l’affichage **Conception**. Remplacez la valeur « Button » de la propriété **Content** du bouton par « Go To Page ». Créez ensuite un gestionnaire pour l’événement **Click** du bouton, comme illustré dans la figure ci-dessous. Si vous ne vous rappelez pas comment procéder, consultez la procédure pas à pas décrite dans la section précédente (conseil : double-cliquez sur le bouton dans le mode **Création**).

![ajout d’un bouton et de son événement click dans visual studio](images/ios-to-uwp/vs-go-to-page.png)

Ajoutons une nouvelle page. Dans l’affichage **Solution**, appuyez sur le menu **Projet**, puis sur **Ajouter un nouvel élément**. Appuyez sur **Page vierge** comme le montre la figure ci-dessous, puis appuyez sur **Ajouter**.

![ajout d’une nouvelle page dans visual studio](images/ios-to-uwp/vs-add-new-page.png)

Ajoutons ensuite un bouton au fichier BlankPage.xaml. Utilisons le contrôle AppBarButton et attribuons-lui une image de flèche Précédent : dans la vue **XAML**, ajoutez ` <AppBarButton Icon="Back"/>` entre les éléments `<Grid> </Grid>`.

À présent, nous allons ajouter un gestionnaire d’événements au bouton : double-cliquez sur le contrôle en mode **Design** , Microsoft Visual Studio ajouter le texte « AppBarButton\_cliquez » dans la zone de **clic** , comme illustré dans la figure suivante, puis ajoute et affiche le gestionnaire d’événements correspondant dans le fichier BlankPage.Xaml.cs.

![ajout d’un bouton précédent et de son événement click dans visual studio](images/ios-to-uwp/vs-add-back-button.png)

De retour dans la vue **XAML** du fichier BlankPage.xaml, le code XAML (Extensible Application Markup Language) de l’élément `<AppBarButton>` doit maintenant ressembler à ceci :

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

Revenez au fichier BlankPage.xaml.cs et ajoutez ce code pour accéder à la page précédente dès que l’utilisateur appuie sur le bouton.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

Pour finir, ouvrez le fichier MainPage.xaml.cs et ajoutez ce code. Le fichier BlankPage s’ouvre après que l’utilisateur a appuyé sur le bouton.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

Exécutez le programme à présent. Appuyez sur le bouton « Go To Page » (Atteindre la page) pour accéder à l’autre page, puis appuyez sur le bouton doté de la flèche Précédent pour revenir à la page précédente.

La navigation entre les pages est gérée par la classe [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame). Comme la classe **UINavigationController** dans iOS utilise les méthodes **pushViewController** et **popViewController** , la classe **Frame** des applications UWP fournit des méthodes [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) et [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) . La classe **Frame** possède également une méthode appelée [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), qui comblera vos attentes.

Cette procédure pas à pas crée une nouvelle instance de BlankPage chaque fois que vous y accédez (l’instance précédente sera automatiquement libérée, ou *publiée*). Si vous ne souhaitez pas qu’une nouvelle instance soit créée à chaque fois, ajoutez le code suivant au constructeur de la classe BlankPage dans le fichier BlankPage.xaml.cs. Le comportement [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) est ainsi activé.

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

Vous pouvez également vous procurer ou définir la propriétéCacheSize[**de la classe**Frame](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) pour gérer le nombre de pages de l’historique de navigation qu’il est possible de mettre en cache.

Pour plus d’informations sur la navigation, voir [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) et [Exemple d’animations de personnages XAML](https://code.msdn.microsoft.com/windowsapps/Personality-Animations-3f857919).

**Remarque**  pour plus d’informations sur la navigation dans les applications UWP en JavaScript et HTML, consultez [démarrage rapide : utilisation de la navigation sur une seule page](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10)).
 
### <a name="next-step"></a>Étape suivante

[Mise en route : animation](getting-started-animation.md)

