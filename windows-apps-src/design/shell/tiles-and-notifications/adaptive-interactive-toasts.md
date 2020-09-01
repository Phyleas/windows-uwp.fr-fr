---
Description: Les notifications toast adaptatives et interactives contextuelles et flexibles (plus de contenu, des images incluses/une interaction utilisateur facultatives).
title: Contenu des toasts
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, notifications Toast, toasts interactifs, toasts adaptatifs, contenu Toast, charge utile Toast
ms.localizationpriority: medium
ms.openlocfilehash: 97dd16d712dca3de69a98c608b7c8947ebbddfea
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173363"
---
# <a name="toast-content"></a>Contenu des toasts

Les notifications Toast adaptatives et interactives vous permettent de créer des notifications flexibles avec du texte, des images et des boutons/entrées.

> **API importantes** : [Package NuGet UWP Community Toolkit Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Pour afficher les modèles hérités de Windows 8.1 et Windows Phone 8,1, consultez le [catalogue de modèles Toast hérité](/previous-versions/windows/apps/hh761494(v=win.10)).


## <a name="getting-started"></a>Mise en route

**Installez la bibliothèque Notifications.** Si vous préférez utiliser C# plutôt que XML pour générer les notifications, installez le package NuGet [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (recherchez « notifications uwp ». Les exemples de code C# indiqués dans cet article utilisent la version 1.0.0 du package NuGet.

**Installez Notifications Visualizer.** Cette application Windows gratuite vous aide à concevoir des notifications de Toast interactives en fournissant un aperçu visuel instantané de votre toast quand vous le modifiez, à l’instar de l’éditeur XAML/mode Design de Visual Studio. Consultez le [visualiseur de notifications](notifications-visualizer.md) pour plus d’informations ou [Téléchargez le visualiseur de notifications à partir du Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envoi d’une notification Toast

Pour savoir comment envoyer une notification, consultez [Envoyer un toast local](send-local-toast.md). Cette documentation traite uniquement de la création du contenu Toast.


## <a name="toast-notification-structure"></a>Structure de notification toast

Les notifications Toast sont une combinaison de certaines propriétés de données telles que la balise/le groupe (qui vous permettent d’identifier la notification) et le *contenu du Toast*.

Les principaux composants du contenu Toast sont...
* **Launch**: définit les arguments qui seront retransmis à votre application quand l’utilisateur clique sur votre toast, ce qui vous permet d’établir un lien vers le contenu correct affiché par le Toast. Pour plus d’informations, consultez [Envoyer un toast local](send-local-toast.md).
* **visuel**: partie visuelle du Toast, y compris la liaison générique qui contient le texte et les images.
* **actions**: partie interactive du Toast, y compris les entrées et les actions.
* **audio**: contrôle le son joué lorsque le toast est présenté à l’utilisateur.

Le contenu Toast est défini dans du code XML brut, mais vous pouvez utiliser notre [bibliothèque NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) pour obtenir un modèle objet C# (ou C++) afin de construire le contenu Toast. Cet article documente tout ce qui se passe dans le contenu du Toast.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

Voici une représentation visuelle du contenu du Toast :

![structure de notification toast](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Élément visuel

Chaque toast doit spécifier un visuel, où vous devez fournir une liaison Toast générique, qui peut contenir du texte, des images et bien plus encore. Ces éléments sont affichés sur différents appareils Windows, y compris les ordinateurs de bureau, les téléphones, les tablettes et les Xbox.

Pour tous les attributs pris en charge dans la section visuelle et ses éléments enfants, [consultez la documentation du schéma](toast-schema.md#toastvisual).

L’identité de votre application sur la notification Toast est transporte via l’icône de votre application. Toutefois, si vous utilisez le remplacement du logo de l’application, nous affichons le nom de votre application sous vos lignes de texte.

| Identité de l’application pour un toast normal | Identité de l’application avec appLogoOverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Éléments de texte

Chaque toast doit avoir au moins un élément de texte et peut contenir deux éléments de texte supplémentaires, tous de type [**AdaptiveText**](toast-schema.md#adaptivetext).

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Depuis la mise à jour anniversaire de Windows 10, vous pouvez contrôler le nombre de lignes de texte affichées à l’aide de la propriété **HintMaxLines** sur le texte. La valeur par défaut (et maximum) est de 2 lignes de texte pour le titre, et jusqu’à 4 lignes (combinées) pour les deux éléments de description supplémentaires (le deuxième et le troisième **AdaptiveText**).

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>Remplacement du logo de l’application

Par défaut, votre toast affiche le logo de votre application. Toutefois, vous pouvez remplacer ce logo par votre propre image [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) . Par exemple, s’il s’agit d’une notification d’une personne, nous vous recommandons de remplacer le logo de l’application par une image de cette personne.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Vous pouvez utiliser la propriété **HintCrop** pour modifier le rognage de l’image. Par exemple, **Circle** produit une image entourée d’un cercle. Dans le cas contraire, l’image est carré. Les dimensions de l’image sont 48 pixels à 100% de mise à l’échelle.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>Image de héros

**Nouveauté de la mise à jour anniversaire**: les toasts peuvent afficher une image de héros, qui est un [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) présenté de manière visible dans la bannière Toast et dans le centre de maintenance. Les dimensions de l’image sont 364x180 pixels à 100% de mise à l’échelle.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Image Inline

Vous pouvez fournir une image Inline pleine largeur qui s’affiche lorsque vous développez le Toast.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>Restrictions de taille d’image

Les images que vous utilisez dans votre notification Toast peuvent être source à partir de...

 - http://
 - ms-appx:///
 - ms-appdata:///

Pour les images Web distantes http et HTTPS, il existe des limites sur la taille de fichier de chaque image individuelle. Dans la mise à jour des créateurs de automne (16299), nous avons augmenté la limite à 3 Mo sur les connexions normales et 1 Mo sur les connexions limitées. Avant cela, les images étaient toujours limitées à 200 Ko.

| Connexion normale | Connexion limitée | Avant la mise à jour des créateurs de automne |
| - | - | - |
| 3 Mo | 1 Mo | 200 Ko |

Si une image dépasse la taille du fichier ou ne peut pas être téléchargée, ou expire, l’image est supprimée et le reste de la notification s’affiche.


## <a name="attribution-text"></a>Texte de l’attribution

**Nouveauté de la mise à jour anniversaire**: Si vous devez référencer la source de votre contenu, vous pouvez utiliser le texte de l’attribution. Ce texte est toujours affiché au bas de votre notification, avec l’identité de votre application ou l’horodateur de la notification.

Dans les versions antérieures de Windows qui ne prennent pas en charge le texte de l’attribution, le texte sera simplement affiché sous la forme d’un autre élément de texte (en supposant que vous n’avez pas encore le maximum de trois éléments de texte).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>Horodateur personnalisé

**Nouveautés de Creators Update**: vous pouvez désormais remplacer l’horodateur fourni par le système par votre propre horodateur qui représente avec précision le moment où le message/les informations/le contenu a été généré. Cet horodateur est visible dans le centre de maintenance.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Pour en savoir plus sur l’utilisation d’un horodateur personnalisé, consultez [horodatages personnalisés sur les toasts](custom-timestamps-on-toasts.md).

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="progress-bar"></a>Barre de progression

**Nouveautés de Creators Update**: vous pouvez fournir une barre de progression sur votre notification toast pour informer l’utilisateur de la progression des opérations telles que les téléchargements.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Pour en savoir plus sur l’utilisation d’une barre de progression, consultez [barre de progression Toast](toast-progress-bar.md).


## <a name="headers"></a>headers

**Nouveautés de Creators Update**: vous pouvez regrouper les notifications sous les en-têtes dans le centre de maintenance. Par exemple, vous pouvez regrouper des messages d’une conversation de groupe sous un en-tête, ou des notifications de groupe d’un thème commun sous un en-tête, ou plus.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Pour en savoir plus sur l’utilisation des en-têtes, consultez [en-têtes Toast](toast-headers.md).


## <a name="adaptive-content"></a>Contenu adaptatif

**Nouveauté de la mise à jour anniversaire**: en plus du contenu indiqué ci-dessus, vous pouvez également afficher du contenu adaptatif supplémentaire qui est visible lorsque le toast est développé.

Ce contenu supplémentaire est spécifié à l’aide de Adaptive, qui vous permet d’en savoir plus sur en lisant la documentation sur les [vignettes adaptatives](create-adaptive-tiles.md).

Notez que tout contenu adaptatif doit être contenu dans un [**AdaptiveGroup**](./toast-schema.md#adaptivegroup). Dans le cas contraire, il ne sera pas rendu à l’aide de Adaptive.


### <a name="columns-and-text-elements"></a>Colonnes et éléments de texte

Voici un exemple d’utilisation de colonnes et de certains éléments de texte adaptatif avancés. Étant donné que les éléments de texte se trouvent dans un **AdaptiveGroup**, ils prennent en charge toutes les propriétés de style adaptatif riches.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="buttons"></a>Boutons

Les boutons rendent votre toast interactif, ce qui permet à l’utilisateur d’effectuer des actions rapides sur votre notification Toast sans interrompre son flux de travail actuel. Par exemple, les utilisateurs peuvent répondre à un message directement à partir d’un toast ou supprimer un e-mail sans même ouvrir l’application de messagerie. Les boutons s’affichent dans la partie développée de votre notification.

Pour en savoir plus sur l’implémentation de boutons de bout en bout, consultez [Envoyer un toast local](send-local-toast.md).

Les boutons peuvent effectuer les actions suivantes...

-   Activation de l’application au premier plan, avec un argument qui peut être utilisé pour accéder à une page ou à un contexte spécifique.
-   Activation de la tâche en arrière-plan de l’application, pour une réponse rapide ou un scénario similaire.
-   activation d’une autre application par le biais d’un lancement par protocole ;
-   Exécution d’une action système, telle que la répétition ou la disparition de la notification.

> [!NOTE]
> Vous pouvez avoir jusqu’à 5 boutons (y compris les éléments de menu contextuel que nous aborderons plus tard).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>Boutons avec des icônes

Vous pouvez ajouter des icônes à vos boutons. Ces icônes sont des images de 16 pixels transparentes en blanc à 100% de mise à l’échelle, et elles ne doivent pas être incluses dans l’image elle-même. Si vous choisissez de fournir des icônes sur une notification Toast, vous devez fournir des icônes pour tous vos boutons dans la notification, car elle transforme le style de vos boutons en boutons d’icône.

> [!NOTE]
> Pour l’accessibilité, veillez à inclure une version blanche de l’icône (une icône noire pour les arrière-plans blancs), de sorte que lorsque l’utilisateur active contraste élevé mode blanc, l’icône est visible. En savoir plus sur la [page d’accessibilité Toast](tile-toast-language-scale-contrast.md).

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>Boutons avec activation de mise à jour en attente

**Nouveautés de la mise à jour des créateurs de automne**: sur les boutons d’activation en arrière-plan, vous pouvez utiliser un comportement d’activation après activation de **PendingUpdate** pour créer des interactions à plusieurs étapes dans vos notifications Toast. Quand l’utilisateur clique sur le bouton, votre tâche en arrière-plan est activée et le toast est placé dans un État « mise à jour en attente », où il reste à l’écran jusqu’à ce que votre tâche en arrière-plan remplace le Toast par un nouveau Toast.

Pour savoir comment implémenter cela, consultez [mise à jour en attente de Toast](toast-pending-update.md).

![Toast avec mise à jour en attente](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Actions du menu contextuel

**Nouveauté de la mise à jour anniversaire**: vous pouvez ajouter des actions de menu contextuel supplémentaires au menu contextuel existant qui apparaît lorsque l’utilisateur clique avec le bouton droit sur votre toast dans le centre de maintenance. Notez que ce menu s’affiche uniquement quand l’utilisateur clique avec le bouton droit dans le centre de maintenance. Il n’apparaît pas quand vous cliquez avec le bouton droit sur une bannière de message Toast.

> [!NOTE]
> Sur les appareils plus anciens, ces actions de menu contextuel supplémentaires s’affichent simplement comme des boutons normaux sur votre toast.

Les actions de menu contextuel supplémentaires que vous ajoutez (telles que « changer d’emplacement ») s’affichent au-dessus des deux entrées système par défaut.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> Des éléments de menu contextuel supplémentaires contribuent à la limite totale de 5 boutons sur un toast.

L’activation d’éléments de menu contextuel supplémentaires est gérée de la même façon que les boutons Toast.


## <a name="inputs"></a>Entrées

Les entrées sont spécifiées dans la région actions de la région Toast du Toast, ce qui signifie qu’elles sont visibles uniquement lorsque le toast est développé.


### <a name="quick-reply-text-box"></a>Zone de texte de réponse rapide

Pour activer une zone de texte de réponse rapide (par exemple, dans une application de messagerie), ajoutez une entrée de texte et un bouton, puis référencez l’ID du champ d’entrée de texte afin que le bouton s’affiche à côté du champ d’entrée. L’icône du bouton doit être une image de 32x32 pixels sans remplissage, des pixels blancs définis sur transparent et une échelle de 100%.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entrées avec barre de boutons

Vous pouvez également avoir une (ou plusieurs) entrées avec des boutons normaux affichés sous les entrées.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrée de sélection

Outre les zones de texte, vous pouvez également utiliser un menu de sélection.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


### <a name="snoozedismiss"></a>Répéter/ignorer

À l’aide d’un menu de sélection et de deux boutons, nous pouvons créer une notification de rappel qui utilise les actions répéter et ignorer du système. Veillez à définir le scénario sur rappel pour que la notification se comporte comme un rappel.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Nous allons lier le bouton répéter à l’entrée du menu de sélection à l’aide de la propriété **SelectionBoxId** du bouton Toast.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

Pour utiliser les actions système répéter et ignorer :

-   Spécifiez un **ToastButtonSnooze** ou un **ToastButtonDismiss**
-   Spécifiez éventuellement une chaîne de contenu personnalisée :
    -   Si vous ne fournissez pas de chaîne, nous utiliserons automatiquement les chaînes localisées pour « répéter » et « ignorer ».
-   Spécifiez éventuellement le **SelectionBoxId**:
    -   Si vous ne voulez pas que l’utilisateur sélectionne un intervalle de répétition, mais souhaitez simplement que votre notification se répète une seule fois pendant un intervalle de temps défini par le système (et cohérent dans l’ensemble du système d’exploitation), ne construisez aucun élément &lt;input&gt;.
    -   Si vous voulez fournir des sélections d’intervalle de répétition :
        -   Spécifier **SelectionBoxId** dans l’action de répétition
        -   Correspond à l’ID de l’entrée avec le **SelectionBoxId** de l’action de répétition
        -   Spécifiez la valeur de **ToastSelectionBoxItem**en tant que nonNegativeInteger qui représente l’intervalle de répétition en minutes.



## <a name="audio"></a>Audio

L’audio personnalisé a toujours été pris en charge par mobile et est pris en charge dans la version de bureau 1511 (Build 10586) ou plus récente. L’audio personnalisé peut être référencé via les chemins d’accès suivants :

-   ms-appx:///
-   ms-appdata:///

Vous pouvez également choisir parmi la [liste des MS-winsoundevents](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements), qui ont toujours été prises en charge sur les deux plateformes.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

Consultez la [page schéma audio](/uwp/schemas/tiles/toastschema/element-audio) pour plus d’informations sur l’audio dans les notifications Toast. Pour savoir comment envoyer un toast à l’aide de l’audio personnalisé, consultez l’article [audio personnalisé sur les toasts](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmes, rappels et appels entrants

Pour créer des alarmes, des rappels et des notifications d’appels entrants, il vous suffit d’utiliser une notification Toast normale avec une valeur de scénario qui lui est assignée. Le scénario adusts quelques comportements pour créer une expérience utilisateur cohérente et unifiée.

> [!IMPORTANT]
> Lorsque vous utilisez le rappel ou l’alarme, vous devez fournir au moins un bouton sur votre notification Toast. Dans le cas contraire, le Toast sera traité comme un toast normal.

* **Rappel**: la notification reste à l’écran jusqu’à ce que l’utilisateur la ignore ou effectue une action. Sur Windows Mobile, le Toast s’affichera également en préversion. Un rappel sera émis.
* **Alarme**: en plus des comportements de rappel, les alarmes effectuent en outre une boucle audio avec un son d’alarme par défaut.
* **IncomingCall**: les notifications d’appel entrantes sont affichées en mode plein écran sur les périphériques Windows Mobile. Dans le cas contraire, ils ont les mêmes comportements que les alarmes, sauf qu’ils utilisent l’audio de sonnerie et que leurs boutons ont un style différent.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="localization-and-accessibility"></a>Localisation et accessibilité

Vos mosaïques et toasts peuvent charger des chaînes et des images adaptées à la langue d’affichage, au facteur d’échelle d’affichage, au contraste élevé et à d’autres contextes d’exécution. Pour plus d’informations, consultez [prise en charge des notifications par vignette et toast pour la langue, la mise à l’échelle et le contraste élevé](tile-toast-language-scale-contrast.md).


## <a name="handling-activation"></a>Gestion de l’activation
Pour savoir comment gérer les activations de Toast (l’utilisateur qui clique sur votre toast ou sur les boutons du Toast), consultez [Envoyer un toast local](send-local-toast.md).
 
## <a name="related-topics"></a>Rubriques connexes

* [Envoyer un toast local et gérer l’activation](send-local-toast.md)
* [Bibliothèque de notifications sur GitHub (qui fait partie du kit de la communauté UWP)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Prise en charge des notifications par vignette et toast pour la langue, la mise à l’échelle et le contraste élevé](tile-toast-language-scale-contrast.md)