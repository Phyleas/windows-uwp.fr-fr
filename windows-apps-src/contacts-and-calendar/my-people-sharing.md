---
title: Partage de Mes Contacts
description: Utilisez le partage de mes personnes pour permettre aux utilisateurs d’épingler des contacts à leur barre des tâches et de rester en contact facilement depuis n’importe où dans Windows.
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76d52fe3ed7e7fb74ae5338e589ab34751bedebe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173663"
---
# <a name="my-people-sharing"></a>Partage de Mes Contacts

La fonctionnalité mes contacts permet aux utilisateurs d’épingler des contacts à leur barre des tâches, ce qui leur permet de rester en contact aisé depuis n’importe où dans Windows, quelle que soit l’application à laquelle ils sont connectés. Désormais, les utilisateurs peuvent partager du contenu avec leurs contacts épinglés en faisant glisser les fichiers de l’Explorateur de fichiers vers leur code confidentiel mes contacts. Ils peuvent également partager des contacts dans le magasin de contacts Windows via la icône de partage standard. Poursuivez la lecture pour apprendre à activer votre application en tant que cible de partage de mes personnes.

![Panneau de partage de mes personnes](images/my-people-sharing.png)

## <a name="requirements"></a>Spécifications

+ Windows 10 et Microsoft Visual Studio 2019. Pour plus d’informations sur l’installation, consultez la page [obtenir une configuration avec Visual Studio](../get-started/get-set-up.md).
+ Connaissances de base de C# ou d’un langage de programmation orienté objet similaire. Pour commencer à utiliser C#, consultez [créer une application « Hello, World »](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Vue d’ensemble

Vous devez suivre trois étapes pour activer votre application en tant que cible de partage de mes personnes :

1. [Déclarez la prise en charge du contrat d’activation shareTarget dans le manifeste de votre application.](#declaring-support-for-the-share-contract)
2. [Annotez les contacts que les utilisateurs peuvent partager avec votre application.](#annotating-contacts)
3. Prendre en charge plusieurs instances de l’application en cours d’exécution en même temps.  Les utilisateurs doivent être en mesure d’interagir avec une version complète de votre application tout en l’utilisant pour les partager avec d’autres personnes. Ils peuvent l’utiliser dans plusieurs fenêtres de partage à la fois. Pour prendre cela en charge, votre application doit être en mesure d’exécuter plusieurs vues simultanément. Pour savoir comment procéder, consultez l’article [« afficher plusieurs vues pour une application »](../design/layout/show-multiple-views.md).

Une fois cette opération effectuée, votre application s’affiche en tant que cible de partage dans la fenêtre partage mes personnes, qui peut être lancée de deux façons :
1. Un contact est choisi via l’icône partager.
2. Le ou les fichiers sont déplacés et déplacés sur un contact épinglé à la barre des tâches.

## <a name="declaring-support-for-the-share-contract"></a>Déclaration de prise en charge du contrat de partage

Pour déclarer la prise en charge de votre application en tant que cible de partage, commencez par ouvrir votre application dans Visual Studio. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur **Package. appxmanifest** , puis sélectionnez **Ouvrir avec**. Dans le menu, sélectionnez **éditeur XML (texte)** , puis cliquez sur **OK**. Ensuite, apportez les modifications suivantes au manifeste :


**Avant**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Après**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Ce code ajoute la prise en charge de tous les fichiers et formats de données, mais vous pouvez choisir de spécifier les types de fichiers et les formats de données pris en charge (pour plus d’informations, consultez [la documentation relative à la classe ShareTarget](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) ).

## <a name="annotating-contacts"></a>Annoter des contacts

Pour autoriser la fenêtre de partage de mes personnes à afficher votre application en tant que cible de partage pour vos contacts, vous devez les écrire dans le magasin de contacts Windows. Pour savoir comment écrire des contacts, consultez l' [exemple intégration](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)de la carte de visite. 

Pour que votre application apparaisse en tant que cible de partage mes personnes lors du partage avec un contact, elle doit écrire une annotation sur ce contact. Les annotations sont des éléments de données de votre application qui sont associés à un contact. L’annotation doit contenir la classe activable correspondant à la vue souhaitée dans son membre **ProviderProperties** et déclarer la prise en charge de l’opération de **partage** .

Vous pouvez annoter des contacts à tout moment pendant l’exécution de votre application, mais vous devez généralement annoter les contacts dès qu’ils sont ajoutés au magasin de contacts Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

« AppId » est le nom de la famille de packages, suivi de «  ! » et l’ID de classe activable. Pour trouver le nom de la famille de packages, ouvrez **Package. appxmanifest** à l’aide de l’éditeur par défaut et recherchez dans l’onglet « Packaging ». Ici, « app » est la classe activable correspondant à la vue de la cible du partage.

## <a name="running-as-a-my-people-share-target"></a>Exécution en tant que cible de partage mes contacts

Enfin, pour exécuter l’application, remplacez la méthode [OnShareTargetActivated](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) dans la classe principale de votre application pour gérer l’activation de la cible de partage. La propriété [ShareTargetActivatedEventArgs. ShareOperation. contacts](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) contient le ou les contacts à partager, ou elle est vide s’il s’agit d’une opération de partage standard (et non d’un partage mes personnes).

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Voir aussi
+ [Ajout de la prise en charge de mes contacts](my-people-support.md)
+ [ShareTarget, classe](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Exemple d’intégration de carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)