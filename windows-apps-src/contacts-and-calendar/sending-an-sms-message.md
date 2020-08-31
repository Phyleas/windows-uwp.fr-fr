---
description: Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.
title: Envoyer un message SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: contacts, SMS, envoi
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ceaeffdb0d8b3207a95e981f16590fe8e9e12e46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154653"
---
# <a name="send-an-sms-message"></a>Envoyer un message SMS

Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi.

Pour appeler ce code, déclarez les fonctionnalités **chat**, **smsSend**et **chatSystem** dans votre manifeste de package. Il s’agit de [fonctionnalités restreintes](../packaging/app-capability-declarations.md#special-and-restricted-capabilities) , mais vous pouvez les utiliser dans votre application. Vous avez besoin d’une approbation uniquement si vous envisagez de publier votre application dans le Windows Store. Consultez [types de comptes, emplacements et frais](../publish/account-types-locations-and-fees.md).

## <a name="launch-the-compose-sms-dialog"></a>Lancer la boîte de dialogue de rédaction d’un message SMS

Créez un nouvel objet [**ChatMessage**](/uwp/api/windows.applicationmodel.chat.chatmessage) et définissez les données que vous souhaitez préremplir dans la boîte de dialogue composer un message électronique. Appelez [**ShowComposeSmsMessageAsync**](/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) pour afficher la boîte de dialogue.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

Vous pouvez utiliser le code suivant pour déterminer si l’appareil qui exécute votre application peut envoyer des messages SMS.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Résumé et étapes suivantes

Cette rubrique vous a montré comment lancer la boîte de dialogue de rédaction d’un message SMS. Pour plus d’informations sur la sélection de contacts en tant que destinataires d’un message SMS, voir la section [Sélectionner des contacts](selecting-contacts.md). Téléchargez les [Exemples d’applications Windows universelles](https://github.com/Microsoft/Windows-universal-samples) dans GitHub pour voir d’autres exemples illustrant comment envoyer et recevoir des messages SMS à l’aide d’une tâche en arrière-plan.

## <a name="related-topics"></a>Rubriques connexes

* [Sélectionner des contacts](selecting-contacts.md)