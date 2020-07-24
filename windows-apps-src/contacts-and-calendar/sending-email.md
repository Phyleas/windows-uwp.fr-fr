---
description: Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
title: Envoi de courrier électronique
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: contacts, e-mail, envoi
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7839a26afca81913e50296ac5ed9bb9210edbf2
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997896"
---
# <a name="send-email"></a>Envoi de courrier électronique

Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.

**Dans cet article**

-   [Lancer la boîte de dialogue de rédaction d’un message électronique](#launch-the-compose-email-dialog)
-   [Résumé et étapes suivantes](#summary-and-next-steps)
-   [Rubriques connexes](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Lancer la boîte de dialogue de rédaction d’un message électronique

Créez un objet [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) et définissez les données à préremplir dans la boîte de dialogue de rédaction d’un message électronique. Appelez [**ShowComposeNewEmailAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) pour afficher la boîte de dialogue.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> Les pièces jointes que vous ajoutez à un message électronique à l’aide de la classe [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) s’affichent uniquement dans l’application de messagerie. Si les utilisateurs ont un autre programme de messagerie configuré comme programme de messagerie par défaut, la fenêtre composer s’affiche sans la pièce jointe. Il s'agit d'un problème connu.

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message électronique. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message électronique, voir [Sélectionner des contacts](selecting-contacts.md). Voir [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) pour sélectionner un fichier à utiliser en pièce jointe d’un message électronique.

## <a name="related-topics"></a>Rubriques connexes

* [Sélection de contacts](selecting-contacts.md)
 