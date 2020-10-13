---
Description: Découvrez comment utiliser des en-têtes pour regrouper visuellement vos notifications Toast dans le centre de maintenance.
title: En-têtes Toast
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, toast, en-tête, en-têtes Toast, notification, toasts de groupe, centre de maintenance
ms.localizationpriority: medium
ms.openlocfilehash: 95cd6083cf4430f25b1514a7e163d04892097903
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984475"
---
# <a name="toast-headers"></a>En-têtes Toast

Vous pouvez regrouper visuellement un ensemble de notifications associées dans le centre de maintenance en utilisant un en-tête de Toast dans vos notifications.

> [!IMPORTANT]
> **Nécessite la mise à jour des créateurs de bureau et 1.4.0 de la bibliothèque de notifications**: vous devez exécuter desktop Build 15063 ou une version ultérieure pour afficher les en-têtes Toast. Vous devez utiliser la version 1.4.0 ou supérieure de la [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de la boîte à outils de la communauté UWP pour construire l’en-tête dans le contenu de votre toast. Les en-têtes sont pris en charge uniquement sur le bureau.

Comme indiqué ci-dessous, cette conversation de groupe est unifiée dans un en-tête unique, « camping !! ». Chaque message de la conversation est une notification de Toast distincte partageant le même en-tête Toast.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Vous pouvez également choisir de regrouper visuellement vos notifications par catégorie, comme les rappels de vol, le suivi des packages et bien plus encore.

## <a name="add-a-header-to-a-toast"></a>Ajouter un en-tête à un toast

Voici comment ajouter un en-tête à une notification Toast.

> [!NOTE]
> Les en-têtes sont pris en charge uniquement sur le bureau. Les appareils qui ne prennent pas en charge les en-têtes ignorent simplement l’en-tête.

#### <a name="builder-syntax"></a>[Syntaxe du générateur](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddHeader("6289", "Camping!!", "action=openConversation&id=6289")
    .AddText("Anyone have a sleeping bag I can borrow?");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

---

En résumé...

1. Ajoutez l' **en-tête** à votre **ToastContent**
2. Assigner les propriétés d' **ID**, de **titre**et d' **arguments** requis
3. Envoyer votre notification ([en savoir plus](send-local-toast.md))
4. Sur une autre notification, utilisez le même **ID** d’en-tête pour les unifier sous l’en-tête. L' **ID** est la seule propriété utilisée pour déterminer si les notifications doivent être regroupées, ce qui signifie que le **titre** et les **arguments** peuvent être différents. Le **titre** et les **arguments** de la notification la plus récente au sein d’un groupe sont utilisés. Si cette notification est supprimée, le **titre** et les **arguments** reviennent à la notification suivante la plus récente.


## <a name="handle-activation-from-a-header"></a>Gérer l’activation à partir d’un en-tête

Les en-têtes sont cliquables par les utilisateurs, afin que l’utilisateur puisse cliquer sur l’en-tête pour en savoir plus à partir de votre application.

Par conséquent, les applications peuvent fournir des **arguments** sur l’en-tête, de la même façon que les arguments Launch sur le Toast lui-même.

L’activation est gérée de la même façon que l' [activation ordinaire de Toast](send-local-toast.md#step-4-handling-activation), ce qui signifie que vous pouvez récupérer ces arguments dans la méthode **OnActivated** de la `App.xaml.cs` même façon que lorsque l’utilisateur clique sur le corps de votre toast ou sur un bouton de votre toast.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Informations supplémentaires

L’en-tête sépare visuellement les notifications et les regroupe. Il ne change aucune autre logistique quant au nombre maximal de notifications qu’une application peut avoir (20) et au comportement premier sorti de la liste des notifications.

L’ordre des notifications dans les en-têtes est le suivant... Pour une application donnée, la notification la plus récente de l’application (et le groupe d’en-têtes entier dans le cas d’un en-tête) s’affiche en premier.

L' **ID** peut être n’importe quelle chaîne de votre choix. Il n’existe aucune restriction de longueur ou de caractère sur les propriétés dans **ToastHeader**. La seule contrainte est que votre contenu XML Toast entier ne peut pas être supérieur à 5 Ko.

La création d’en-têtes ne modifie pas le nombre de notifications affichées dans le centre de maintenance avant que le bouton « voir » s’affiche (ce nombre est 3 par défaut et peut être configuré par l’utilisateur pour chaque application dans les paramètres système des notifications).

Le fait de cliquer sur l’en-tête, comme un clic sur le titre de l’application, n’efface pas les notifications appartenant à cet en-tête (votre application doit utiliser les API toast pour effacer les notifications appropriées).


## <a name="related-topics"></a>Rubriques connexes

- [Envoyer un toast local et gérer l’activation](send-local-toast.md)
- [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
