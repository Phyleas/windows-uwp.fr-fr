---
description: Le lecteur multimédia dispose de contrôles de transport XAML personnalisables permettant de gérer du contenu audio et vidéo.
title: Créer des contrôles de transport de média personnalisés
ms.assetid: 6643A108-A6EB-42BC-B800-22EABD7B731B
label: Create custom media transport controls
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3329ea23c831b1bd91955013f0bdedcbbd08aa34
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032932"
---
# <a name="create-custom-transport-controls"></a>Créer des contrôles de transport personnalisés



MediaPlayerElement dispose de contrôles de transport XAML personnalisables permettant de gérer du contenu audio et vidéo dans une application Windows. Ici, nous démontrons comment personnaliser le modèle MediaTransportControls. Nous allons expliquer comment utiliser le menu de dépassement, ajouter un bouton personnalisé et modifier le curseur.

> **API importantes**  : [MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement), [MediaPlayerElement.AreTransportControlsEnabled](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.aretransportcontrolsenabled), [MediaTransportControls](/uwp/api/Windows.Media.SystemMediaTransportControls)

Avant de démarrer, prenez le temps de vous familiariser avec les classes MediaPlayerElement et MediaTransportControls. Pour plus d’informations, voir le Guide du contrôle MediaPlayerElement.

> [!TIP]
> Les exemples de cette rubrique sont basés sur l’[Exemple de contrôles de transport de média](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls). Vous pouvez télécharger l’exemple pour afficher et exécuter le code validé.

> [!NOTE]
> **MediaPlayerElement** est uniquement disponible dans Windows 10, version 1607 ou ultérieure. Si vous développez une application pour une version antérieure de Windows 10, vous devez utiliser [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) à la place. Tous les exemples de cette page fonctionnent avec **MediaElement** également.

## <a name="when-should-you-customize-the-template"></a>Quand faut-il personnaliser le modèle ?

**MediaPlayerElement** intègre des contrôles de transport compatibles sans modification avec la plupart des applications de lecture audio et vidéo. Ils sont fournis par la classe [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) et comprennent des boutons qui permettent de lire, d’arrêter, de naviguer dans les médias, de régler le volume, de passer en plein écran, de diffuser sur un second appareil, d’activer des sous-titres, de basculer entre les pistes audio et de paramétrer la vitesse de lecture. MediaTransportControls a des propriétés qui vous permettent de contrôler si chaque bouton est affiché et activé. Vous pouvez également définir la propriété [**IsCompact**](/uwp/api/windows.ui.xaml.controls.mediatransportcontrols.iscompact) pour spécifier si les contrôles sont affichés sur une ou deux lignes.

Toutefois, il peut arriver que vous deviez personnaliser davantage l’apparence du contrôle ou changer son comportement. En voici quelques exemples :
- changement des icônes, du comportement du curseur et des couleurs ;
- déplacement des boutons de commande les moins utilisés vers un menu de dépassement ;
- modification de l’ordre dans lequel les commandes sont déplacées lorsque le contrôle est redimensionné ;
- fourniture d’un bouton de commande qui n’est pas présent dans l’ensemble par défaut.

> [!NOTE]
> Les boutons visibles à l’écran disparaissent des contrôles de transport intégrés dans un ordre prédéfini s’il n’y a pas assez de place dans l’écran. Pour changer cet ordre ou placer des commandes qui ne tiennent pas dans un menu de dépassement, vous devez personnaliser les contrôles.

Vous pouvez personnaliser l’apparence du contrôle en modifiant le modèle par défaut. Pour modifier le comportement du contrôle ou ajouter de nouvelles commandes, vous pouvez créer un contrôle personnalisé qui est dérivé de MediaTransportControls.

> [!TIP]
> Les modèles de contrôle personnalisables sont une fonctionnalité puissante de la plateforme XAML, mais leur utilisation entraîne des conséquences que vous devez prendre en compte. Quand vous personnalisez un modèle, il devient une portion statique de votre application. Par conséquent, il ne reçoit aucune des mises à jour de plateforme qui sont apportées au modèle par Microsoft. Si les mises à jour de modèle sont effectuées par Microsoft, vous devez recueillir le nouveau modèle et le modifier à nouveau afin de profiter des avantages du modèle actualisé.

## <a name="template-structure"></a>Structure du modèle

Le [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) fait partie du style par défaut. Vous pouvez copier ce style par défaut dans votre projet pour le modifier. Le ControlTemplate est divisé en sections similaires aux autres modèles de contrôle XAML.
- La première section du modèle contient les définitions [**Style**](/uwp/api/Windows.UI.Xaml.Style) des différents composants de MediaTransportControls.
- La deuxième section définit les différents états visuels utilisés par l’élément MediaTransportControls.
- La troisième section contient l’élément [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) qui rassemble ces différents éléments MediaTransportControls et définit la manière dont les composants sont disposés.

> [!NOTE]
> Pour plus d’informations sur la modification des modèles, consultez [Control templates](./control-templates.md). Utilisez un éditeur de texte ou des éditeurs similaires de votre IDE pour ouvrir les fichiers XAML dans \(*Program Files* )\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\\( *SDK version* )\Generic. Le style et le modèle par défaut de chaque contrôle sont définis dans le fichier **generic.xaml**. Pour rechercher le modèle MediaTransportControls dans generic.xaml, recherchez « MediaTransportControls ».

Dans les sections suivantes, vous allez apprendre à personnaliser plusieurs des éléments principaux des contrôles de transport :
- [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider) : permet à l’utilisateur de parcourir ses fichiers multimédias et d’afficher la progression
- [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) : contient l’ensemble des boutons.
Pour plus d’informations, consultez la section d’anatomie de la rubrique de référence sur MediaTransportControls.

## <a name="customize-the-transport-controls"></a>Personnaliser les contrôles de transport

Si vous souhaitez simplement modifier l’apparence de l’élément MediaTransportControls, vous pouvez créer une copie du style et du modèle de contrôle par défaut, et la modifier. Toutefois, si vous souhaitez également enrichir ou modifier la fonctionnalité du contrôle, vous devez créer une nouvelle classe dérivée de MediaTransportControls.

### <a name="re-template-the-control"></a>Remodéliser le contrôle

**Pour personnaliser le modèle et le style par défaut de MediaTransportControls**
1. Copiez le style par défaut de Styles et modèles MediaTransportControls dans un ResourceDictionary de votre projet.
2. Donnez au Style une valeur x:Key pour l’identifier, comme ceci.

```xaml
<Style TargetType="MediaTransportControls" x:Key="myTransportControlsStyle">
    <!-- Style content ... -->
</Style>
```

3. Ajoutez un MediaPlayerElement avec MediaTransportControls à votre interface utilisateur.
4. Définissez la propriété Style de l’élément MediaTransportControls sur votre ressource Style personnalisée, comme illustré ici.

```xaml
<MediaPlayerElement AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls Style="{StaticResource myTransportControlsStyle}"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Pour en savoir plus sur la modification des styles et des modèles, voir [Contrôles de style](./xaml-styles.md) et [Modèles de contrôle](./control-templates.md).

### <a name="create-a-derived-control"></a>Créer un contrôle dérivé

Pour ajouter ou modifier les fonctionnalités des contrôles de transport, vous devez créer une nouvelle classe dérivée de MediaTransportControls. Une classe dérivée appelée `CustomMediaTransportControls` est illustrée dans [l’Exemple de contrôles de transport de média](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls) et dans les autres exemples sur cette page.

**Pour créer une classe dérivée de MediaTransportControls**
1. Ajoutez un nouveau fichier de classe à votre projet.
    - Dans Visual Studio, sélectionnez Projet &gt; Ajouter une classe. La boîte de dialogue Ajouter un nouvel élément s’ouvre.
    - Dans la boîte de dialogue Ajouter un nouvel élément, entrez un nom pour le fichier de classe, puis cliquez sur Ajouter. (Dans l’Exemple de contrôles de transport de média, la nouvelle est nommée `CustomMediaTransportControls`.)
2. Modifiez le code de classe à dériver de la classe MediaTransportControls.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
}
```

3. Copiez le style par défaut pour [**MediaTransportControls**](/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) dans un [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) de votre projet. Il s’agit du style et du modèle que vous modifiez.
(Dans l’Exemple de contrôles de transport média, un nouveau dossier nommé « Thèmes » est créé, et un fichier ResourceDictionary nommé generic.xaml lui est ajouté.)
4. Remplacez la propriété [**TargetType**](/uwp/api/windows.ui.xaml.style.targettype) du style par le nouveau type de contrôle personnalisé. (Dans l’exemple, la propriété TargetType est remplacée par `local:CustomMediaTransportControls`.)

```xaml
xmlns:local="using:CustomMediaTransportControls">
...
<Style TargetType="local:CustomMediaTransportControls">
```

5. Définissez la propriété [**DefaultStyleKey**](/uwp/api/windows.ui.xaml.controls.control.defaultstylekey) de votre classe personnalisée. Cela indique à votre classe personnalisée d’utiliser une classe Style avec une propriété TargetType de `local:CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }
}
```

6. Ajoutez un [**MediaPlayerElement**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement) à votre balisage XAML, puis ajoutez-y les contrôles de transport personnalisés. Notez que les API permettant de masquer, d’afficher, de désactiver et d’activer les boutons par défaut fonctionnent toujours avec un modèle personnalisé.

```xaml
<MediaPlayerElement Name="MediaPlayerElement1" AreTransportControlsEnabled="True" Source="video.mp4">
    <MediaPlayerElement.TransportControls>
        <local:CustomMediaTransportControls x:Name="customMTC"
                                            IsFastForwardButtonVisible="True"
                                            IsFastForwardEnabled="True"
                                            IsFastRewindButtonVisible="True"
                                            IsFastRewindEnabled="True"
                                            IsPlaybackRateButtonVisible="True"
                                            IsPlaybackRateEnabled="True"
                                            IsCompact="False">
        </local:CustomMediaTransportControls>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```

Vous pouvez maintenant modifier le style et le modèle de contrôle pour mettre à jour l’apparence de votre contrôle personnalisé, et le code de contrôle pour mettre à jour son comportement.

### <a name="working-with-the-overflow-menu"></a>Utilisation du menu de dépassement

Vous pouvez déplacer des boutons de commande MediaTransportControls vers un menu de dépassement, afin que les commandes les moins utilisées soient masquées jusqu’à ce que l’utilisateur en ait besoin.

Dans le modèle MediaTransportControls, les boutons de commande sont contenus dans un élément [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar). La barre de commandes prend en charge le concept de commandes principales et secondaires. Les commandes principales sont les boutons qui apparaissent dans le contrôle par défaut et sont toujours visibles (sauf si vous désactivez ou masquez le bouton, ou qu’il n’y a pas suffisamment de place). Les commandes secondaires sont affichées dans un menu de dépassement qui apparaît quand un utilisateur clique sur le bouton points de suspension (...). Pour plus d’informations, voir l’article [Barres d’application et barres de commande](app-bars.md).

Pour déplacer un élément des commandes principales de la barre de commandes vers le menu de dépassement, vous devez modifier le modèle de contrôle XAML.

**Pour déplacer une commande vers le menu de dépassement :**
1. Dans le modèle de contrôle, recherchez l’élément CommandBar nommé `MediaControlsCommandBar`.
2. Ajoutez une section [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) au code XAML pour la classe CommandBar. Placez-la après la balise fermante de la propriété [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands).

```xaml
<CommandBar x:Name="MediaControlsCommandBar" ... >  
  <CommandBar.PrimaryCommands>
...
    <AppBarButton x:Name='PlaybackRateButton'
                    Style='{StaticResource AppBarButtonStyle}'
                    MediaTransportControlsHelper.DropoutOrder='4'
                    Visibility='Collapsed'>
      <AppBarButton.Icon>
        <FontIcon Glyph="&#xEC57;"/>
      </AppBarButton.Icon>
    </AppBarButton>
...
  </CommandBar.PrimaryCommands>
<!-- Add secondary commands (overflow menu) here -->
  <CommandBar.SecondaryCommands>
    ...
  </CommandBar.SecondaryCommands>
</CommandBar>
```

3. Pour ajouter des commandes au menu, coupez et collez le code XAML des objets [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) souhaités de PrimaryCommands dans SecondaryCommands. Dans cet exemple, nous déplaçons le contrôle `PlaybackRateButton` vers le menu de dépassement.

4. Ajoutez une étiquette au bouton et supprimez les informations de style, comme illustré ici.
Étant donné que le menu de dépassement se compose de boutons de texte, vous devez ajouter une étiquette de texte au bouton et supprimer le style qui définit la hauteur et la largeur du bouton. Sinon, il ne s’affiche pas correctement dans le menu de dépassement.

```xaml
<CommandBar.SecondaryCommands>
    <AppBarButton x:Name='PlaybackRateButton'
                  Label='Playback Rate'>
    </AppBarButton>
</CommandBar.SecondaryCommands>
```

> [!IMPORTANT]
> Vous devez tout de même afficher le bouton et l’activer pour pouvoir l’utiliser dans le menu de dépassement. Dans cet exemple, l’élément PlaybackRateButton n’est pas visible dans le menu de dépassement, sauf si la propriété IsPlaybackRateButtonVisible est true. Il n’est pas activé, sauf si la propriété IsPlaybackRateEnabled est true. La définition de ces propriétés est illustrée dans la section précédente.

### <a name="adding-a-custom-button"></a>Ajout d’un bouton personnalisé

Il se peut que vous souhaitiez personnaliser la classe MediaTransportControls pour pouvoir ajouter une commande personnalisée au contrôle. Que vous l’ajoutiez en tant que commande principale ou secondaire, la procédure de création du bouton de commande et de modification de son comportement est la même. Dans [l’Exemple de contrôles de transport de média](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlCustomMediaTransportControls), un bouton « rating » est ajouté aux commandes principales.

**Pour ajouter un bouton de commande personnalisé**
1. Créez un objet AppBarButton et ajoutez-le à la classe CommandBar dans le modèle de contrôle.

```xaml
<AppBarButton x:Name="LikeButton"
              Icon="Like"
              Style="{StaticResource AppBarButtonStyle}"
              MediaTransportControlsHelper.DropoutOrder="3"
              VerticalAlignment="Center" />
```

Vous devez l’ajouter à la CommandBar dans l’emplacement approprié. (Pour plus d’informations, voir la section Utilisation du menu de dépassement.) Sa position dans l’interface utilisateur est déterminée par l’emplacement du bouton dans le balisage. Par exemple, si vous souhaitez que ce bouton s’affiche en tant que dernier élément dans les commandes principales, ajoutez-le à la fin de la liste de commandes principales.

Vous pouvez également personnaliser l’icône du bouton. Pour plus d'informations, consultez la référence <a href="/uwp/api/Windows.UI.Xaml.Controls.AppBarButton"><b>AppBarButton</b></a>.
    

2. Dans la méthode [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), obtenez le bouton à partir du modèle et enregistrez un gestionnaire pour son événement [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Ce code va dans la classe `CustomMediaTransportControls`.

```csharp
public sealed class CustomMediaTransportControls :  MediaTransportControls
{
    // ...

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    //...
}
```

3. Ajoutez du code au gestionnaire d’événements Click pour effectuer l’action qui se produit lorsque le bouton est cliqué.
Voici le code complet de la classe.

```csharp
public sealed class CustomMediaTransportControls : MediaTransportControls
{
    public event EventHandler< EventArgs> Liked;

    public CustomMediaTransportControls()
    {
        this.DefaultStyleKey = typeof(CustomMediaTransportControls);
    }

    protected override void OnApplyTemplate()
    {
        // Find the custom button and create an event handler for its Click event.
        var likeButton = GetTemplateChild("LikeButton") as Button;
        likeButton.Click += LikeButton_Click;
        base.OnApplyTemplate();
    }

    private void LikeButton_Click(object sender, RoutedEventArgs e)
    {
        // Raise an event on the custom control when 'like' is clicked.
        var handler = Liked;
        if (handler != null)
        {
            handler(this, EventArgs.Empty);
        }
    }
}
```

**Contrôles de transport de média personnalisés avec un bouton « J’aime » ajouté**
![Contrôle de transport de média personnalisé avec un bouton J’aime supplémentaire](images/controls/mtc_double_custom_inprod.png)

### <a name="modifying-the-slider"></a>Modification du curseur

Le contrôle seek de la classe MediaTransportControls est fourni par un élément [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider). Vous pouvez notamment le personnaliser en changeant la granularité du comportement de recherche.

Le curseur de recherche par défaut est divisé en 100 portions, de sorte que le comportement de recherche est limité à ces nombreuses sections. Vous pouvez modifier la granularité du curseur de recherche en obtenant la classe Slider à partir de l’arborescence visuelle XAML dans votre gestionnaire d’événements [**MediaOpened**](/uwp/api/windows.media.playback.mediaplayer.mediaopened) sur [**MediaPlayerElement.MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement). Cet exemple montre comment utiliser [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) pour obtenir une référence à la classe Slider, puis changer la fréquence d’étape par défaut du curseur de 1 % en 0,1 % (1 000 étapes) si le média est plus long que 120 minutes. Le MediaPlayerElement est nommé `MediaPlayerElement1`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
  MediaPlayerElement1.MediaPlayer.MediaOpened += MediaPlayerElement_MediaPlayer_MediaOpened;
  base.OnNavigatedTo(e);
}

private void MediaPlayerElement_MediaPlayer_MediaOpened(object sender, RoutedEventArgs e)
{
  FrameworkElement transportControlsTemplateRoot = (FrameworkElement)VisualTreeHelper.GetChild(MediaPlayerElement1.TransportControls, 0);
  Slider sliderControl = (Slider)transportControlsTemplateRoot.FindName("ProgressSlider");
  if (sliderControl != null && MediaPlayerElement1.NaturalDuration.TimeSpan.TotalMinutes > 120)
  {
    // Default is 1%. Change to 0.1% for more granular seeking.
    sliderControl.StepFrequency = 0.1;
  }
}
```
## <a name="related-articles"></a>Articles connexes

- [Lecture de contenu multimédia](media-playback.md)
