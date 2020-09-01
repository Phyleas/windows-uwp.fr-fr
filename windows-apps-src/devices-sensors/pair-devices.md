---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Jumeler des appareils
description: Pour pouvoir être utilisés, certains appareils doivent être jumelés. L’espace de noms Windows.Devices.Enumeration prend en charge trois méthodes différentes de jumelage des appareils.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: d3fff5a8868cfe29c944336a33c8b6554b74ebf4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175443"
---
# <a name="pair-devices"></a>Jumeler des appareils



**API importantes**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

Pour pouvoir être utilisés, certains appareils doivent être jumelés. L’espace de noms [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) prend en charge trois méthodes différentes de jumelage des appareils.

-   Jumelage automatique
-   Jumelage de base
-   Jumelage personnalisé

**Conseil**    Certains appareils n’ont pas besoin d’être couplés pour pouvoir être utilisés. Ce sujet est abordé dans la section sur le jumelage automatique.

 

## <a name="automatic-pairing"></a>Jumelage automatique


Parfois, vous souhaitez utiliser un appareil dans votre application, sans pour autant chercher à le jumeler. Vous souhaitez simplement être en mesure d’utiliser la fonctionnalité associée à l’appareil. Par exemple, si votre application est dédiée à la capture d’images d’une webcam, ce n’est pas nécessairement l’appareil qui vous intéresse, mais plutôt la capture d’image. Si des API dédiées à l’appareil qui vous intéresse sont disponibles, vous êtes soumis au jumelage automatique.

Le cas échéant, il vous suffit d’utiliser les API associées à l’appareil afin de transmettre les appels nécessaires, et de vous appuyer sur le système pour l’exécution des jumelages nécessaires. Pour utiliser la fonctionnalité de certains appareils, il n’est pas forcément nécessaire de procéder un jumelage. Si aucun jumelage n’est nécessaire, les API d’appareil exécutent en arrière-plan l’action de jumelage. Ainsi, vous n’êtes pas tenu d’intégrer cette fonctionnalité dans votre application. Votre application ne recevra aucune information relative au jumelage des appareils, ce qui ne vous empêchera pas d’accéder à ces derniers et d’utiliser leur fonctionnalité.

## <a name="basic-pairing"></a>Jumelage de base


Lors d’un jumelage de base, votre application utilise les API [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) pour tenter de jumeler l’appareil. Dans ce scénario, vous laissez Windows exécuter et gérer le processus de jumelage. Si une interaction utilisateur est nécessaire, elle est gérée par Windows. Vous devez utiliser le jumelage de base si vous devez effectuer un jumelage avec un appareil, et qu’il n’existe pas d’API d’appareil dédiée aux tentatives de jumelage automatique. Vous souhaitez seulement utiliser l’appareil et devez d’abord effectuer un jumelage avec ce dernier.

Pour tenter de procéder à un jumelage de base, vous devez d’abord obtenir l’objet [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) associé à l’appareil qui vous intéresse. Une fois que vous recevez cet objet, vous interagissez avec la propriété [**DeviceInformation.Pairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing), qui est un objet [**DeviceInformationPairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing). Pour procéder à un jumelage, il vous suffit d’appeler [**DeviceInformationPairing.PairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.pairasync). Il vous faudra **await** le résultat, ceci pour octroyer à votre application le temps nécessaire à la tentative de jumelage. Le résultat de l’action de jumelage est alors renvoyé. Si aucune erreur n’est identifiée, l’appareil est jumelé.

Si vous avez recours au jumelage de base, vous disposez également d’un accès à des informations supplémentaires sur l’état de l’appareil. Par exemple, vous connaissez l’état de jumelage ([**IsPaired**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) et savez si l’appareil peut être jumelé ([**CanPair**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Ces deux données sont des propriétés de l’objet [**DeviceInformationPairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing). Si vous utilisez le jumelage automatique, vous n’avez peut-être pas accès à ces informations, sauf si vous obtenez les objets [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) appropriés.

## <a name="custom-pairing"></a>Jumelage personnalisé


Grâce au jumelage personnalisé, votre application peut prendre part au processus de jumelage. Dès lors, votre application peut spécifier les éléments [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) qui sont pris en charge pour le processus de jumelage. Vous êtes également chargé de créer votre propre interface utilisateur pour interagir avec l’utilisateur au besoin. Utilisez le jumelage personnalisé lorsque vous souhaitez que votre application possède un peu plus d’influence sur l’exécution du processus de jumelage ou pour afficher votre propre interface utilisateur de jumelage.

Pour implémenter le jumelage personnalisé, vous devez obtenir l’objet [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) associé à l’appareil qui vous intéresse, tout comme avec le jumelage de base. Toutefois, la propriété spécifique qui vous intéresse est [**DeviceInformation. jumelage. Custom**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.custom). Vous obtiendrez un objet [**DeviceInformationCustomPairing**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing) . Toutes les méthodes [**DeviceInformationCustomPairing. PairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairasync) vous obligent à inclure un paramètre [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) . Il indique les actions que l’utilisateur devra effectuer pour tenter de jumeler l’appareil. Consultez la page de référence **DevicePairingKinds** pour en savoir plus sur les différents types d’action que l’utilisateur devra effectuer. Tout comme avec le jumelage de base, il vous faudra **await** le résultat afin d’octroyer à votre application le temps nécessaire à la procédure de jumelage. Le résultat de l’action de jumelage est alors renvoyé. Si aucune erreur n’est identifiée, l’appareil est jumelé.

Pour prendre en charge le jumelage personnalisé, il vous faudra créer un gestionnaire pour l’événement [**PairingRequested**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested). Ce gestionnaire doit veiller à tenir compte de tous les autres [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) qui peuvent être utilisés dans un scénario de jumelage personnalisé. L’action appropriée à entreprendre dépend de l’élément **DevicePairingKinds** fourni avec les arguments de l’événement.

Il est important de garder à l’esprit que le jumelage personnalisé est toujours une opération de niveau système. Pour cette raison, lorsque vous utilisez un ordinateur de bureau ou un appareil Windows Phone, une boîte de dialogue système apparaît juste avant l’opération de jumelage. En effet, ces deux plateformes valorisent une expérience utilisateur qui nécessite le consentement de l’utilisateur. Dans la mesure où cette boîte de dialogue est automatiquement générée, vous n’aurez pas besoin de créer votre propre boîte de dialogue lorsque vous sélectionnez un élément [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) de **ConfirmOnly** avec ces plateformes. Pour les autres **DevicePairingKinds**, vous devrez exécuter certaines opérations spécifiques de gestion, en fonction de la valeur **DevicePairingKinds** spécifique. Pour savoir comment gérer le jumelage personnalisé associé à différentes valeurs **DevicePairingKinds**, voir les exemples.

À compter de Windows 10, version 1903, un nouveau **DevicePairingKinds** est pris en charge, **ProvidePasswordCredential**. Cette valeur signifie que l’application doit demander un nom d’utilisateur et un mot de passe auprès de l’utilisateur afin de s’authentifier auprès de l’appareil couplé. Pour gérer ce cas, appelez la méthode [**AcceptWithPasswordCredential**](/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_) des arguments d’événement du gestionnaire d’événements **PairingRequested** pour accepter le jumelage. Transmettez un objet [**PasswordCredential**](/uwp/api/windows.security.credentials.passwordcredential) qui encapsule le nom d’utilisateur et le mot de passe en tant que paramètre. Notez que le nom d’utilisateur et le mot de passe de l’appareil distant sont différents de ceux de l’utilisateur connecté localement et qu’ils ne le sont pas.

## <a name="unpairing"></a>Annulation du jumelage


L’opération d’annulation s’applique uniquement aux scénarios de jumelage de base ou personnalisé décrits plus haut. Si vous utilisez le jumelage automatique, les informations d’état de jumelage de l’appareil ne sont pas transmises à l’application ; aucune annulation de jumelage n’est nécessaire. Les processus d’annulation des jumelages de base et personnalisé sont identiques. Pour quelle raison ? Il n’est pas nécessaire de fournir d’informations supplémentaires ou d’interagir dans le processus d’annulation de jumelage.

Si vous souhaitez annuler le jumelage d’un appareil, vous devez commencer par obtenir l’objet [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) qui lui est associé. Vous devez ensuite récupérer la propriété [**DeviceInformation. appairage**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) et appeler [**DeviceInformationPairing. UnpairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.unpairasync). Comme c’était le cas avec le jumelage, il vous faudra **await** le résultat. Le résultat de l’action d’annulation de jumelage est renvoyé. Si aucune erreur n’est identifiée, le jumelage de l’appareil est annulé.

## <a name="sample"></a>Exemple


Pour télécharger un exemple illustrant comment utiliser les API [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration), cliquez [ici](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

 

 