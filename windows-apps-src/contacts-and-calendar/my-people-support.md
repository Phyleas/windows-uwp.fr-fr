---
title: Ajout de la prise en charge de Mes Contacts à une application
description: Explique comment ajouter le support technique de mon personnel à une application et comment épingler et détacher des contacts
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c0e174613357ad9e4e45d2776f3fbc618535b30
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339457"
---
# <a name="adding-my-people-support-to-an-application"></a>Ajout de la prise en charge de Mes Contacts à une application

> [!Note]
> À compter de la mise à jour Windows 10 mai 2019 (version 1903), les nouvelles installations de Windows 10 n’afficheront plus les « personnes dans la barre des tâches » par défaut. Les clients peuvent activer la fonctionnalité en cliquant avec le bouton droit sur la barre des tâches et en appuyant sur « Afficher les personnes dans la barre des tâches ». Les développeurs sont déconseillés d’ajouter un support technique à leurs applications et doivent visiter le [blog du développeur Windows](https://blogs.windows.com/windowsdeveloper/) pour plus d’informations sur l’optimisation des applications pour Windows 10.

La fonctionnalité mes contacts permet aux utilisateurs d’épingler des contacts à partir d’une application directement à leur barre des tâches, ce qui crée un nouvel objet contact avec lequel ils peuvent interagir de différentes manières. Cet article explique comment ajouter la prise en charge de cette fonctionnalité, en permettant aux utilisateurs d’épingler des contacts directement à partir de votre application. Lorsque les contacts sont épinglés, de nouveaux types d’interaction utilisateur deviennent disponibles, tels que le partage et les [notifications](my-people-notifications.md)de [mes personnes](my-people-sharing.md) .

![Conversation mes contacts](images/my-people-chat.png)

## <a name="requirements"></a>Configuration requise

+ Windows 10 et Microsoft Visual Studio 2019. Pour plus d’informations sur l’installation, consultez la page [obtenir une configuration avec Visual Studio](/windows/apps/get-started/get-set-up).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour commencer à utiliser C#, consultez [créer une application « Hello, World »](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Vue d’ensemble

Pour permettre à votre application d’utiliser la fonctionnalité mes contacts, vous devez effectuer trois opérations :

1. [Déclarez la prise en charge du contrat d’activation shareTarget dans le manifeste de votre application.](./my-people-sharing.md#declaring-support-for-the-share-contract)
2. [Annotez les contacts que les utilisateurs peuvent partager avec votre application.](./my-people-sharing.md#annotating-contacts)
3.  Prendre en charge plusieurs instances de votre application en cours d’exécution en même temps. Les utilisateurs doivent être en mesure d’interagir avec une version complète de votre application tout en l’utilisant dans un panneau de contact.  Ils peuvent même l’utiliser dans plusieurs panneaux de contact à la fois.  Pour prendre cela en charge, votre application doit être en mesure d’exécuter plusieurs vues simultanément. Pour savoir comment procéder, consultez l’article [« afficher plusieurs vues pour une application »](../design/layout/show-multiple-views.md).

Une fois cette opération effectuée, votre application s’affiche dans le panneau de contact pour les contacts annotés.

## <a name="declaring-support-for-the-contract"></a>Déclaration de prise en charge du contrat

Pour déclarer la prise en charge du contrat My People, ouvrez votre application dans Visual Studio. Dans le **Explorateur de solutions** , cliquez avec le bouton droit sur **Package. appxmanifest** , puis sélectionnez **Ouvrir avec**. Dans le menu, sélectionnez **éditeur XML (texte)** , puis cliquez sur **OK**. Apportez les modifications suivantes au manifeste :

**Avant**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**Après**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

Avec cet ajout, votre application peut maintenant être lancée par le biais de **Windows. ContactPanel** Contract, qui vous permet d’interagir avec les panneaux de contact.

## <a name="annotating-contacts"></a>Annoter des contacts

Pour autoriser l’affichage des contacts de votre application dans la barre des tâches via le volet mes personnes, vous devez les écrire dans le magasin de contacts Windows.  Pour savoir comment écrire des contacts, consultez l' [exemple de carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

Votre application doit également écrire une annotation sur chaque contact. Les annotations sont des éléments de données de votre application qui sont associés à un contact. L’annotation doit contenir la classe activable correspondant à la vue souhaitée dans son membre **ProviderProperties** et déclarer la prise en charge de l’opération **ContactProfile** .

Vous pouvez annoter des contacts à tout moment pendant l’exécution de votre application, mais vous devez généralement annoter les contacts dès qu’ils sont ajoutés au magasin de contacts Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

« AppId » est le nom de la famille de packages, suivi de «  ! » et l’ID de classe activable. Pour trouver le nom de la famille de packages, ouvrez **Package. appxmanifest** à l’aide de l’éditeur par défaut et recherchez dans l’onglet « Packaging ». Ici, « application » est la classe activable qui correspond à la vue de démarrage de l’application.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Autoriser les contacts à inviter de nouveaux utilisateurs potentiels

Par défaut, votre application s’affiche uniquement dans le panneau de contact pour les contacts que vous avez spécifiquement annotés.  Cela permet d’éviter toute confusion avec les contacts qui ne peuvent pas interagir via votre application.  Si vous souhaitez que votre application apparaisse pour les contacts que votre application ne connaît pas (pour inviter les utilisateurs à ajouter ce contact à leur compte, par exemple), vous pouvez ajouter ce qui suit à votre manifeste :

**Avant**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**Après**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

Avec cette modification, votre application s’affiche en tant qu’option disponible dans le panneau de contact pour tous les contacts que l’utilisateur a épinglés.  Lorsque votre application est activée à l’aide du contrat du panneau de contact, vous devez vérifier si le contact est bien celui que votre application connaît.  Si ce n’est pas le cas, vous devez afficher la nouvelle expérience utilisateur de votre application.

![Volet de contact Mes contacts](images/my-people.png)

## <a name="support-for-email-apps"></a>Prise en charge des applications de messagerie

Si vous écrivez une application de messagerie, vous n’avez pas besoin d’annoter chaque contact manuellement.  Si vous déclarez la prise en charge du volet contact et du protocole mailto :, votre application s’affiche automatiquement pour les utilisateurs ayant une adresse de messagerie.

## <a name="running-in-the-contact-panel"></a>Exécution dans le panneau de contact

Maintenant que votre application s’affiche dans le panneau de contact pour une partie ou la totalité des utilisateurs, vous devez gérer l’activation avec le contrat du panneau de contact.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

Lorsque votre application est activée avec ce contrat, elle reçoit un [objet ContactPanelActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Contient l’ID du contact avec lequel votre application tente d’interagir au lancement et un objet [ContactPanel](/uwp/api/windows.applicationmodel.contacts.contactpanel) . Vous devez conserver une référence à cet objet ContactPanel, ce qui vous permettra d’interagir avec le panneau.

L’objet ContactPanel a deux événements que votre application doit écouter :
+ L’événement **LaunchFullAppRequested** est envoyé lorsque l’utilisateur a appelé l’élément d’interface utilisateur qui demande que votre application complète soit lancée dans sa propre fenêtre.  Votre application est chargée de se lancer elle-même, en passant tout le contexte nécessaire.  Vous êtes libre de faire cela si vous le souhaitez (par exemple, via le lancement de protocole).
+ L' **événement de fermeture** est envoyé lorsque votre application est sur le lieu d’être fermée, ce qui vous permet d’enregistrer n’importe quel contexte.

L’objet ContactPanel vous permet également de définir la couleur d’arrière-plan de l’en-tête du panneau de contact (s’il n’est pas défini, le thème du système est utilisé par défaut) et de fermer par programmation le panneau de contact.

## <a name="supporting-notification-badging"></a>Prise en charge de la notification badge

Si vous souhaitez que les contacts épinglés à la barre des tâches soient badges lorsque de nouvelles notifications provenant de votre application sont liées à cette personne, vous devez inclure le paramètre **Hint-People** dans vos [notifications Toast](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) et les [notifications de mes personnes](./my-people-notifications.md)expressif.

![Badge des notifications de personnes](images/my-people-badging.png)

Pour Badger un contact, le nœud Toast de niveau supérieur doit inclure le paramètre Hint-People pour indiquer l’envoi ou le contact associé. Ce paramètre peut prendre l’une des valeurs suivantes :
+ **Adresse de messagerie** 
    + Par exemple, mailto:johndoe@mydomain.com
+ **Numéro de téléphone** 
    + Par exemple, Tél. : 888-888-8888
+ **Identifiant distant** 
    + Par exemple, remoteid : 1234

Voici un exemple d’identification d’une notification Toast liée à une personne spécifique :
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> Si votre application utilise les [API ContactStore](/uwp/api/windows.applicationmodel.contacts.contactstore) et utilise la propriété [StoredContact. RemoteId](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) pour lier des contacts stockés sur le PC à des contacts stockés à distance, il est essentiel que la valeur de la propriété RemoteId soit stable et unique. Cela signifie que l’ID distant doit identifier de manière cohérente un compte d’utilisateur unique et doit contenir une balise unique pour garantir qu’il n’est pas en conflit avec les ID distants d’autres contacts sur le PC, y compris les contacts appartenant à d’autres applications.
> Si les ID distants utilisés par votre application ne sont pas forcément stables et uniques, vous pouvez utiliser la classe RemoteIdHelper indiquée plus loin dans cette rubrique afin d’ajouter une balise unique à tous vos ID distants avant de les ajouter au système. Vous pouvez choisir de ne pas utiliser la propriété RemoteId au lieu de cela et vous créez une propriété étendue personnalisée dans laquelle stocker les ID distants de vos contacts.

## <a name="the-pinnedcontactmanager-class"></a>La classe PinnedContactManager

Le [PinnedContactManager](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) est utilisé pour gérer les contacts épinglés à la barre des tâches. Cette classe vous permet d’épingler et de détacher des contacts, de déterminer si un contact est épinglé et de déterminer si l’épinglage sur une surface particulière est pris en charge par le système sur lequel votre application est en cours d’exécution.

Vous pouvez récupérer l’objet PinnedContactManager à l’aide de la méthode **GetDefault** :

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Épinglage et désépinglage de contacts
Vous pouvez maintenant épingler et détacher des contacts à l’aide de l’PinnedContactManager que vous venez de créer. Les méthodes **RequestPinContactAsync** et **RequestUnpinContactAsync** fournissent à l’utilisateur des boîtes de dialogue de confirmation. elles doivent donc être appelées à partir de votre application Single-Threaded thread Apartment (ASTA ou UI).

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

Vous pouvez également épingler plusieurs contacts en même temps :

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Il n’existe actuellement aucune opération de traitement par lots pour désépingler les contacts.

**Remarque :** 

## <a name="see-also"></a>Voir aussi
+ [Partage de Mes Contacts](my-people-sharing.md)
+ [Mes contacts Notificatons](my-people-notifications.md)
+ [Vidéo Channel 9 sur l’ajout du support technique de mes personnes à une application](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Exemple d’intégration de My People](https://github.com/tonyPendolino/MyPeopleBuild2017)
+ [Exemple de carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Documentation de la classe PinnedContactManager](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Connecter votre application à des actions sur une carte de visite](./integrating-with-contacts.md)