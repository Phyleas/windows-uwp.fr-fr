---
title: Obtenir et comprendre les données de bande magnétique
description: Découvrez comment obtenir et interpréter les données à partir d’un lecteur de bande magnétique à l’aide d’API de point de service (UWP) plateforme Windows universelle (UWP).
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos, lecteur de bande magnétique
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238274"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Obtenir et comprendre les données de bande magnétique

Une fois que vous avez configuré votre lecteur de bande magnétique dans votre application à l’aide de la procédure décrite dans prise en main [du point de service](pos-basics.md), vous êtes prêt à commencer à obtenir des données à partir de celui-ci.

## <a name="subscribe-to-datareceived-events"></a>S’abonner aux événements DataReceived

Chaque fois que le lecteur reconnaît une carte balayée, il lève l’un des trois événements suivants :

* [Événement AamvaCardDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): se produit lorsqu’une carte véhicule à moteur est glissée.
* [Événement BankCardDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): se produit lorsqu’une carte bancaire est glissée.
* [Événement VendorSpecificDataReceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): se produit lorsqu’une carte spécifique au fournisseur est glissée.

Votre application doit uniquement s’abonner aux événements pris en charge par le lecteur de bande magnétique. Vous pouvez voir quels types de cartes sont pris en charge avec [MagneticStripeReader. SupportedCardTypes](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes).

Le code suivant illustre l’abonnement aux trois * événements**DataReceived** :

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

Le gestionnaire d’événements reçoit le [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) et un objet *args* , dont le type varie en fonction de l’événement :

* Événement **AamvaCardDataReceived** : [classe MagneticStripeReaderAamvaCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* Événement **BankCardDataReceived** : [classe MagneticStripeReaderBankCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* Événement **VendorSpecificDataReceived** : [classe MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Obtenir les données

Pour les événements **AamvaCardDataReceived** et **BankCardDataReceived** , vous pouvez obtenir des données directement à partir de l’objet *args* . L’exemple suivant montre comment obtenir quelques propriétés et les assigner à des variables membres :

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

Toutefois, certaines données, y compris toutes les données de l’événement **VendorSpecificDataReceived** , doivent être récupérées via l’objet de **rapport** , qui est une propriété du paramètre *args* . Il s’agit du type [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Vous pouvez utiliser la propriété [CardType](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) pour déterminer le type de carte qui a fait l’objet d’un balayage, puis l’utiliser pour indiquer comment vous interprétez les données de [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)et [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Les données de chacune des pistes sont représentées en tant qu’objets [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) . À partir de cette classe, vous pouvez accéder aux types de données suivants :

* [Data](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): données brutes ou décodées.
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): données discrétionnaires. 
* [EncryptedData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): données chiffrées.

L’extrait de code suivant obtient le rapport et les données de suivi, puis vérifie le type de carte :

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Voir aussi

* [Lecteur de bande magnétique](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader, classe](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs, classe](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs, classe](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs, classe](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)