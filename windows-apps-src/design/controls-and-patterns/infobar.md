---
description: Un contrôle InfoBar (barre d’informations) est une notification insérée pour les messages essentiels à l’échelle de l’application.
title: InfoBar
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, winui, uwp
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: f790e4ed1d16ac42c95f9a835a3b9cc7f3598190
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104540"
---
# <a name="infobar"></a>InfoBar
Le contrôle InfoBar sert à afficher aux utilisateurs des messages d’état à l’échelle de l’application qui sont très visibles, mais non intrusifs. Il existe des niveaux de gravité intégrés qui permettent d’indiquer facilement le type de message affiché, ainsi qu’une option pour inclure votre propre appel à un bouton d’action ou de lien hypertexte. Étant donné que le contrôle InfoBar est inséré avec d’autres contenus d’interface utilisateur, cette option est là pour que le contrôle soit toujours visible ou pour que l’utilisateur le fasse disparaître. 

**Obtenir la bibliothèque d’interface utilisateur Windows**

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Le contrôle **InfoBar** nécessite la bibliothèque d’interface utilisateur Windows, package NuGet qui contient les nouveaux contrôles et les nouvelles fonctionnalités d’interface utilisateur destinés aux applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la bibliothèque d’interface utilisateur Windows :** [Classe InfoBar](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> Tout au long de ce document, nous utilisons l’alias **muxc** en XAML pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous l’avons ajouté à notre élément [Page](/uwp/api/windows.ui.xaml.controls.page) : `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Dans le code-behind, nous utilisons également l’alias **muxc** en C# pour représenter les API de la bibliothèque d’interface utilisateur Windows que nous avons incluses dans notre projet. Nous avons ajouté cette instruction **using** en haut du fichier : `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?
Utilisez un contrôle InfoBar quand un utilisateur doit être informé, accuser réception ou entreprendre une action en cas de changement d’état d’une application. Par défaut, la notification reste dans la zone de contenu jusqu’à ce qu’elle soit fermée par l’utilisateur, mais elle n’interrompt pas nécessairement le workflow de l’utilisateur.

Un contrôle InfoBar occupe de l’espace dans votre disposition et se comporte comme n’importe quel autre élément enfant. Il ne recouvre pas un autre contenu et ne flotte pas non plus au-dessus.

N’utilisez pas de contrôle InfoBar pour confirmer une action de l’utilisateur qui ne change pas l’état de l’application ou y répondre directement, pour les alertes urgentes ou pour les messages non essentiels.

### <a name="remarks"></a>Notes
Utilisez un contrôle InfoBar qui est fermé par l’utilisateur ou quand l’état est résolu pour les scénarios qui ont un impact **direct** sur la perception de l’application ou sur l’expérience d’application.


Voici quelques exemples :
- Connectivité Internet perdue
- Erreur lors de l’enregistrement d’un document quand il est déclenché automatiquement, sans lien avec une action spécifique de l’utilisateur
- Aucun microphone branché lors de la tentative d’enregistrement
- Impossible de se connecter à votre téléphone
- L’abonnement à l’application a expiré


Utilisez un contrôle InfoBar qui est fermé par l’utilisateur pour les scénarios qui ont un impact **indirect** sur la perception de l’application ou sur l’expérience d’application

Voici quelques exemples :
- L’enregistrement d’un appel a commencé
- Mise à jour appliquée avec un lien vers les « Notes de publication »
- Les conditions d’utilisation ont été mises à jour et nécessitent un accusé de réception
- Une sauvegarde de l’ensemble de l’application s’est terminée correctement, de manière asynchrone
- L’abonnement à l’application est sur le point d’expirer

### <a name="when-should-a-different-control-be-used"></a>Quand un autre contrôle doit-il être utilisé ?

Dans certains scénarios, il peut être plus approprié d’utiliser un contrôle [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout) ou [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip).

- Dans les scénarios où une notification persistante n’est pas nécessaire, par exemple pour l’affichage d’informations dans le contexte d’un élément d’interface utilisateur spécifique, il est préférable d’utiliser un [Flyout](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts) (menu volant). 
- Pour les scénarios où l’application confirme une action de l’utilisateur, en indiquant les informations que l’utilisateur **_doit_* _ lire, utilisez un [ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs) (boîte de dialogue de contenu).
  - De plus, si la gravité d’un changement d’état de l’application nécessite que toutes les autres possibilités d’interaction de l’utilisateur avec l’application soient bloquées, utilisez un ContentDialog.
- Dans les scénarios où une notification est un moment d’apprentissage temporaire, il est préférable d’utiliser un [TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip) (conseil d’apprentissage).


Pour plus d’informations sur le choix du contrôle de notification approprié, consultez l’article [Boîtes de dialogue et menus volants](/uwp/design/controls-and-patterns/dialogs-and-flyouts/).


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/InfoBar">ouvrir l’application et voir le contrôle InfoBar à l’œuvre</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>Créer un contrôle InfoBar
Le code XAML ci-dessous décrit un contrôle InfoBar inséré ayant le style par défaut pour une notification d’informations. Une barre d’informations peut être placée comme n’importe quel autre élément et suit le comportement de la disposition de base.
Par exemple, dans un StackPanel (panneau d’empilement) vertical, le contrôle InfoBar se développe horizontalement pour occuper la largeur disponible. 


Par défaut, le contrôle InfoBar n’est pas visible. Affectez à la propriété IsOpen la valeur true dans le code XAML ou le code-behind pour afficher le contrôle InfoBar.


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![Exemple de contrôle InfoBar dans l’état par défaut avec un bouton de fermeture et un message](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>Utilisation de niveaux de gravité prédéfinis
Le type de la barre d’informations peut être défini à l’aide de la propriété Severity pour définir automatiquement une couleur d’état, une icône et des paramètres de technologie d’assistance cohérents en fonction de l’importance de la notification. Si aucune gravité n’est définie, le style par défaut des informations est appliqué.


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![Exemple de contrôle InfoBar dans l’état d’avertissement avec un bouton de fermeture et un message](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>Fermeture programmatique dans un contrôle InfoBar
Un contrôle InfoBar peut être fermé par l’utilisateur à l’aide du bouton de fermeture ou programmatiquement. Si la notification doit être dans la vue jusqu’à ce que l’état soit résolu et que vous voulez supprimer la possibilité pour l’utilisateur de fermer la barre d’informations, vous pouvez définir la propriété IsClosable sur false.


La valeur par défaut de cette propriété est true, auquel cas le bouton de fermeture est présent et prend la forme d’un « X ».


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![Exemple de contrôle InfoBar dans un état d’erreur sans bouton de fermeture](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>Personnalisation : Couleur d’arrière-plan et icône
En dehors des niveaux de gravité prédéfinis, les propriétés Background et IconSource peuvent être définies pour personnaliser l’icône et la couleur d’arrière-plan. Le contrôle InfoBar maintient les paramètres de technologie d’assistance de la gravité définis, ou utilise le paramètre par défaut si aucune valeur n’a été définie.


Il est possible de définir une couleur d’arrière-plan personnalisée par le biais de la propriété Background standard, ce qui remplace la couleur définie par la propriété Severity.
Gardez à l’esprit l’accessibilité et la lisibilité du contenu lors de la définition de votre propre couleur.


Une icône personnalisée peut être définie par le biais de la propriété IconSource. Par défaut, une icône est visible (dans la mesure où le contrôle n’est pas réduit).
Cette icône peut être supprimée en affectant à la propriété IsIconVisible la valeur false. Pour les icônes personnalisées, la taille d’icône recommandée est de 20 px.


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![Exemple de contrôle InfoBar dans l’état par défaut avec une couleur d’arrière-plan personnalisée, une icône personnalisée et un bouton de fermeture](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>Ajouter un bouton d’action

Un bouton d’action supplémentaire peut être ajouté en définissant votre propre bouton qui hérite de [ButtonBase](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) et en lui affectant une valeur dans la propriété ActionButton. Le style personnalisé est appliqué aux boutons d’action de type [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) et [HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) pour la cohérence et l’accessibilité. En plus de la propriété ActionButton, des boutons d’action supplémentaires qui s’affichent sous le message peuvent être ajoutés par le biais du contenu personnalisé.


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![Exemple de contrôle InfoBar dans l’état d’erreur avec un message d’une seule ligne et un bouton d’action](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![Exemple de contrôle InfoBar avec un message qui étend sur plusieurs lignes et un lien hypertexte](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>Wrapping de contenu
Si le contenu de l’InfoBar, à l’exclusion du contenu personnalisé, ne peut pas tenir sur une seule ligne horizontale, les lignes sont disposées verticalement. Les contrôles Title, Message et ActionButton, le cas échéant, s’affichent sur des lignes distinctes.


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![Exemple de contrôle InfoBar dans l’état de réussite avec un message et un titre multilignes](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>Contenu personnalisé
Du contenu XAML peut être ajouté à un contrôle InfoBar à l’aide de la propriété Content. Il apparaîtra sur une ligne spécifique sous le reste du contenu du contrôle. Le contrôle InfoBar sera développé pour s’ajuster au contenu défini.


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <muxc:ProgressBar IsIndeterminate="True" Margin="0,0,0,6" MaxWidth="200"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![Exemple de contrôle InfoBar dans son état par défaut avec une barre de progression indéterminée](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>Création d’un style léger

Vous pouvez modifier le style et le ControlTemplate par défaut pour donner au contrôle une apparence unique. Pour obtenir une liste des ressources de thème disponibles, consultez la section [Styles et modèles Control](/windows/winui/api/microsoft.ui.xaml.controls.infobar#control-style-and-template) dans la documentation de l’API InfoBar.
Pour plus d’informations, consultez la section [Style léger](./xaml-styles.md#lightweight-styling) de l’article [Application de styles aux contrôles](./xaml-styles.md). 

Par exemple, le code suivant définit la couleur d’arrière-plan de toutes les barres d’informations (InfoBar) à bleu :

```xaml
<Page.Resources>
    <x:SolidColorBrush x:Key="InfoBarInformationalSeverityBackgroundBrush" Color="LightBlue"></x:SolidColorBrush>
</Page.Resources>
```
### <a name="canceling-close"></a>Annulation de la fermeture
L’événement Closing peut être utilisé pour annuler et/ou retarder la fermeture d’un contrôle InfoBar. Vous pouvez l’utiliser pour garder le contrôle InfoBar ouvert ou laisser un peu de temps pour qu’une action personnalisée se produise. Quand la fermeture d’un contrôle InfoBar est annulée, la valeur d’IsOpen redevient true. Vous pouvez également annuler une fermeture par programmation.


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>Recommandations

### <a name="enter-and-exit-usability"></a>Facilité d’utilisation de l’entrée et de la sortie
#### <a name="flashing-content"></a>Contenu clignotant
Le contrôle InfoBar ne doit pas apparaître et disparaître rapidement afin d’éviter un clignotement à l’écran. Évitez les visuels clignotants pour les personnes souffrant de photosensibilité et afin d’améliorer la facilité d’utilisation de votre application.


Pour les contrôles InfoBar qui apparaissent et disparaissent automatiquement par le biais d’une condition d’état de l’application, nous vous recommandons d’inclure une logique dans votre application afin d’empêcher l’apparition ou la disparition rapide d’un contenu ou son affichage plusieurs fois dans une ligne. Toutefois, ce contrôle doit en général être utilisé pour les messages d’état à longue durée de vie.


#### <a name="updating-the-infobar"></a>Mise à jour du contrôle InfoBar
Une fois le contrôle ouvert, tout changement apporté aux différentes propriétés, comme la mise à jour du message ou de la propriété Severity, ne déclenchent pas d’événement de notification. Si vous souhaitez informer les utilisateurs qui utilisent des lecteurs d’écran du contenu mis à jour du contrôle InfoBar, nous vous recommandons de fermer puis de rouvrir le contrôle pour déclencher l’événement.


#### <a name="inline-messages-offsetting-content"></a>Messages insérés décalant du contenu
Pour les contrôles InfoBar qui sont insérés avec d’autres contenus d’interface utilisateur, gardez à l’esprit que le reste de la page réagira à l’ajout de l’élément.


Les contrôles InfoBar dont la hauteur est substantielle peuvent considérablement modifier la disposition des autres éléments sur la page. Si le contrôle InfoBar apparaît ou disparaît rapidement, en particulier de manière répétée, l’utilisateur peut être troublé par le changement de l’état visuel.


### <a name="color-and-icon"></a>Couleur et icône
Quand vous personnalisez la couleur et l’icône en dehors des niveaux de gravité prédéfinis, gardez à l’esprit les attentes des utilisateurs en ce qui concerne les connotations du jeu d’icônes et de couleurs standard.


De plus, les couleurs de gravité prédéfinies ont déjà été conçues pour les changements de thème, le mode de contraste élevé, l’accessibilité à la confusion des couleurs et le contraste avec les couleurs de premier plan. Nous vous recommandons d’utiliser ces couleurs quand cela est possible et d’inclure une logique personnalisée dans votre application afin de l’adapter aux différents états de couleur et aux différentes fonctionnalités d’accessibilité.


Consultez les recommandations relatives à l’expérience utilisateur pour les [icônes standard](/windows/win32/uxguide/vis-std-icons) et la [couleur](/windows/win32/uxguide/vis-color) afin de garantir que votre message est communiqué clairement et qu’il est accessible aux utilisateurs.


#### <a name="severity"></a>Gravité
 Évitez de définir la propriété Severity (Gravité) pour une notification qui ne correspond pas aux informations communiquées dans le titre (Title), le message ou le contenu personnalisé.
 

 Les informations qui l’accompagnent doivent viser à communiquer les éléments suivants pour utiliser cette gravité.
 - Erreur : une erreur ou un problème qui s’est produit.
 - Avertissement : condition susceptible de provoquer un problème à l’avenir.
 - Réussite : une tâche de longue durée et/ou en arrière-plan s’est terminée.
 - Default : informations générales qui nécessitent l’attention de l’utilisateur.

Les icônes et les couleurs ne doivent pas être les seuls composants de l’interface utilisateur représentant le sens de votre notification. Le texte du titre et/ou du message de la notification doivent être inclus dans les informations d’affichage.


### <a name="message"></a>Message 

Le texte de votre notification ne sera pas de longueur constante dans toutes les langues. Pour les propriétés Title et Message, cela peut avoir une influence sur le fait que votre notification s’étendra, ou non, sur une deuxième ligne. Nous vous recommandons d’éviter le positionnement en fonction de la longueur du message ou d’autres éléments de l’interface utilisateur définis dans une langue spécifique.

La notification suit le comportement de mise en miroir standard quand elle est localisée dans/à partir de langues de droite à gauche (DàG) ou de gauche à droite (GàD). L’icône est mise en miroir uniquement en cas de direction.

Pour plus d’informations sur la localisation de texte dans votre notification, consultez les recommandations fournies pour [Ajuster la disposition et les polices, et prendre en charge l’écriture DàG](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl).

## <a name="related-articles"></a>Articles connexes

_ [Boîtes de dialogue et menus volants](./dialogs-and-flyouts/index.md)