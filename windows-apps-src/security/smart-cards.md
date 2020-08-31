---
title: Cartes à puce
description: Cette rubrique explique comment les applications de plateforme Windows universelle (UWP) peuvent utiliser des cartes à puce pour connecter des utilisateurs à des services réseau sécurisés, notamment comment accéder aux lecteurs de carte à puce physiques, créer des cartes à puce virtuelles, communiquer avec des cartes à puce, authentifier des utilisateurs, réinitialiser des PIN d’utilisateur et supprimer ou déconnecter des cartes à puce.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 5c792cde951b2bf01585256f51f67a545d96510d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170833"
---
# <a name="smart-cards"></a>Cartes à puce




Cette rubrique explique comment les applications de plateforme Windows universelle (UWP) peuvent utiliser des cartes à puce pour connecter des utilisateurs à des services réseau sécurisés, notamment comment accéder aux lecteurs de carte à puce physiques, créer des cartes à puce virtuelles, communiquer avec des cartes à puce, authentifier des utilisateurs, réinitialiser des PIN d’utilisateur et supprimer ou déconnecter des cartes à puce. 

## <a name="configure-the-app-manifest"></a>Configurer le manifeste de l’application


Pour que votre application puisse authentifier des utilisateurs à l’aide de cartes à puce ou de cartes à puce virtuelles, vous devez configurer la fonction **Certificats utilisateur partagés** dans le fichier Package.appxmanifest du projet.

## <a name="access-connected-card-readers-and-smart-cards"></a>Accéder à des lecteurs de cartes et à des cartes à puce connectés


Vous pouvez rechercher les lecteurs et les cartes à puce attachées en passant l’ID d’appareil (spécifié dans [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)) à la méthode [**SmartCardReader. FromIdAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync) . Pour accéder aux cartes à puce actuellement attachées à l’appareil de lecture retourné, appelez [**SmartCardReader. FindAllCardsAsync**](/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync).

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

Vous devez également configurer votre application de manière à surveiller les événements [**CardAdded**](/uwp/api/windows.devices.smartcards.smartcardreader.cardadded) en implémentant une méthode pour déterminer le comportement de l’application lors de l’insertion d’une carte.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Vous pouvez ensuite passer chaque objet [**SmartCard**](/uwp/api/Windows.Devices.SmartCards.SmartCard) retourné à [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) pour accéder aux méthodes permettant à votre application d’accéder à sa configuration et de la personnaliser.

## <a name="create-a-virtual-smart-card"></a>Créer une carte à puce virtuelle


Pour créer une carte à puce virtuelle à l’aide de [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning), votre application doit d’abord fournir un nom convivial, une clé d’administration et un [**SmartCardPinPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy). Le nom convivial est généralement fourni à l’application, mais votre application doit fournir une clé d’administration et générer une instance du **SmartCardPinPolicy** actuel avant de passer les trois valeurs à [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

1.  Créer une instance d’un [**SmartCardPinPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)
2.  Générez la valeur de clé d’administration en appelant [**CryptographicBuffer. GenerateRandom**](/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) sur la valeur de clé d’administration fournie par le service ou l’outil de gestion.
3.  Passez ces valeurs avec la chaîne *FriendlyNameText* à [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

Une fois que [**RequestVirtualSmartCardCreationAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) a retourné l’objet [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associé, la carte à puce virtuelle est approvisionnée et prête à être utilisée.

>[!NOTE]
>Pour créer une carte à puce virtuelle à l’aide d’une application UWP, l’utilisateur qui exécute l’application doit être membre du groupe administrateurs. Si l’utilisateur n’est pas membre du groupe administrateurs, la création de la carte à puce virtuelle échoue.

## <a name="handle-authentication-challenges"></a>Gérer les demandes d’authentification


Pour authentifier des utilisateurs au moyen de cartes à puce ou de cartes à puce virtuelles, votre application doit être en mesure de répondre aux demandes entre les données des clés d’administration stockées sur la carte et les données des clés d’administration gérées par le serveur d’authentification ou l’outil de gestion.

L’exemple de code suivant montre comment prendre en charge l’authentification par carte à puce pour des services ou la modification des détails d’une carte à puce physique ou virtuelle. Si les données générées à l’aide de la clé d’administration sur la carte (« challenge ») sont identiques aux données de la clé d’administration fournies par le serveur ou l’outil de gestion (« adminkey »), l’authentification réussit.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

Ce code est référencé dans le reste de cette rubrique pour illustrer comment remplir une action d’authentification et comment appliquer des modifications aux informations d’une carte à puce et d’une carte à puce virtuelle.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>Vérifier la réponse d’authentification par carte à puce ou carte à puce virtuelle


Maintenant que nous avons défini la logique pour les demandes d’authentification, nous pouvons soit communiquer avec le lecteur pour accéder à la carte à puce, soit accéder à une carte à puce virtuelle à des fins d’authentification.

1.  Pour commencer la demande, appelez [**GetChallengeContextAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) à partir de l’objet [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associé à la carte à puce. Cette opération génère une instance de [**SmartCardChallengeContext**](/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext), qui contient la valeur du [**Challenge**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge) de la carte.

2.  Passez ensuite la valeur de demande de la carte et la clé d’administration fournie par le service ou l’outil de gestion à **ChallengeResponseAlgorithm** (défini dans l’exemple précédent).

3.  [**VerifyResponseAsync**](/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync) retourne **true** si l’authentification réussit.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>Modifier ou réinitialiser un code PIN d’utilisateur


Pour modifier le code PIN associé à une carte à puce :

1.  Accédez à la carte et générez l’objet [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) associé.
2.  Appelez [**RequestPinChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) pour afficher une interface utilisateur pour effectuer cette opération.
3.  Si le code PIN a été correctement modifié, l’appel retourne **true**.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

Pour demander une réinitialisation du code PIN :

1.  Appelez [**RequestPinResetAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) pour initier l’opération. Cet appel comprend une méthode [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) qui représente la carte à puce et la demande de réinitialisation du code confidentiel.
2.  [**SmartCardPinResetHandler**](/uwp/api/windows.devices.smartcards.smartcardpinresethandler) fournit des informations utilisées par **ChallengeResponseAlgorithm**, encapsulé dans un appel [**SmartCardPinResetDeferral**](/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral), pour comparer la valeur de demande de la carte et la clé d’administration fournie par le service ou l’outil de gestion pour authentifier la demande.

3.  Si la demande réussit, l’appel à [**RequestPinResetAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) est terminé, et la valeur **true** est retournée si le code PIN est correctement réinitialisé.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>Supprimer une carte à puce ou une carte à puce virtuelle


Lorsqu’une carte à puce physique est supprimée, un événement [**CardRemoved**](/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved) se déclenche lorsque la carte est supprimée.

Associez le déclenchement de cet événement sur le lecteur de carte à la méthode qui définit le comportement de votre application suite au retrait de la carte ou du lecteur comme gestionnaire d’événements. Vous pouvez simplement fournir à l’utilisateur une notification indiquant que la carte a été retirée.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

La suppression d’une carte à puce virtuelle est gérée par programme en extrayant d’abord la carte, puis en appelant [**RequestVirtualSmartCardDeletionAsync**](/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) à partir de l’objet retourné [**SmartCardProvisioning**](/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) .

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```