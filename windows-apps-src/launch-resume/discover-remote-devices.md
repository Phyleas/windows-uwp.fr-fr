---
title: Détecter des appareils distants
description: Découvrez comment découvrir des appareils distants à partir de votre application à l’aide de Project Rome.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome
ms.localizationpriority: medium
ms.openlocfilehash: 01c13a30c8869643badc69c546b0a5212308956f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155923"
---
# <a name="discover-remote-devices"></a>Détecter des appareils distants
Votre application peut utiliser le réseau sans fil, la connexion Bluetooth et le Cloud pour détecter les appareils Windows qui sont connectés avec le même compte Microsoft que l’appareil qui découvre. Aucun logiciel particulier n’est nécessaire sur les appareils distants pour qu’ils soient détectables.

> [!NOTE]
> Ce guide suppose que vous avez déjà accès à la fonctionnalité Systèmes distants, conformément à la procédure décrite dans [Lancer une application sur un appareil distant](launch-a-remote-app.md).

## <a name="filter-the-set-of-discoverable-devices"></a>Filtrer l’ensemble des appareils détectables
Vous pouvez affiner l’ensemble des appareils détectables à l’aide d’un [**RemoteSystemWatcher**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) avec des filtres. Les filtres peuvent détecter le type de détection (réseau proximal ou réseau local et connexion Cloud), le type d’appareil (ordinateur de bureau, appareil mobile, Xbox, concentrateur et holographique) et l’état de disponibilité (l’état de disponibilité d’un appareil pour utiliser les fonctionnalités du système distant).

Les objets de filtre doivent être construits avant ou pendant l’initialisation de l’objet **RemoteSystemWatcher** , car ils sont passés en tant que paramètre dans son constructeur. Le code suivant crée un filtre de chaque type disponible, puis les ajoute à la liste.

> [!NOTE]
> Le code de ces exemples exige que vous disposiez `using Windows.System.RemoteSystems` d’une instruction dans votre fichier.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> La valeur de filtre « proximal » ne garantit pas le degré de proximité physique. Pour les scénarios qui nécessitent une proximité physique fiable, utilisez la valeur [**RemoteSystemDiscoveryType. SpatiallyProximal**](/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) dans votre filtre. Actuellement, ce filtre autorise uniquement les appareils qui sont détectés par Bluetooth. À mesure que de nouveaux mécanismes et protocoles de découverte garantissant une proximité physique sont pris en charge, ils sont également inclus ici.  
Il existe également une propriété dans la classe [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) qui indique si un appareil découvert est en fait au sein de la proximité physique : [**RemoteSystem. IsAvailableBySpatialProximity**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Si vous envisagez de découvrir des appareils sur un réseau local (déterminé par votre sélection de filtre de type de détection), votre réseau doit utiliser un profil « privé » ou « de domaine ». Votre appareil ne découvre pas les autres périphériques sur un réseau « public ».

Une fois créée, la liste des objets [**IRemoteSystemFilter**](/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter) peut être transmise au constructeur d’un **RemoteSystemWatcher**.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Quand la méthode [**Start**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.start) de l’observateur est appelée, elle déclenche l’événement [**RemoteSystemAdded**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded) uniquement si un appareil détecté répond à tous les critères suivants :
* L’appareil est détectable par une connexion proximale.
* Il s’agit d’un ordinateur de bureau ou d’un téléphone.
* Il est considéré comme disponible.

Ensuite, la procédure de traitement des événements, de récupération des objets [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) et de connexion aux appareils distants est exactement le même que dans [Lancer une application sur un appareil distant](launch-a-remote-app.md). En résumé, les objets **RemoteSystem** sont stockés en tant que propriétés d’objets [**RemoteSystemAddedEventArgs**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) , qui sont transmis avec chaque événement **RemoteSystemAdded** .

## <a name="discover-devices-by-address-input"></a>Détecter des appareils par entrée d’adresse
Certains appareils ne peuvent pas être associés à un utilisateur ou détectés par un balayage, mais ils restent accessibles si l’application détectrice utilise une adresse directe. La classe [**HostName**](/uwp/api/windows.networking.hostname) permet de représenter l’adresse d’un appareil distant. Cela est souvent stocké sous la forme d’une adresse IP, mais plusieurs autres formats sont autorisés (pour plus d’informations, consultez le [**constructeur du nom d’hôte**](/uwp/api/windows.networking.hostname.-ctor) ).

Un objet **RemoteSystem** est récupéré si un objet **HostName** valide est fourni. Si l’adresse n’est pas valide, une référence d’objet `null` est renvoyée.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Interrogation d’une fonctionnalité sur un système distant

Bien qu’elles soient séparées du filtrage de la découverte, l’interrogation des fonctionnalités des appareils peut être une partie importante du processus de découverte. À l’aide de la méthode [**RemoteSystem. GetCapabilitySupportedAsync**](/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) , vous pouvez interroger des systèmes distants détectés pour la prise en charge de certaines fonctionnalités telles que la connectivité de session à distance ou le partage d’entités spatiales. Consultez la classe [**KnownRemoteSystemCapabilities**](/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) pour obtenir la liste des fonctionnalités interrogeables.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Découverte entre utilisateurs

Les développeurs peuvent spécifier la découverte de _tous_ les appareils à proximité du périphérique client, et pas seulement les appareils inscrits auprès du même utilisateur. Elle est implémentée par le biais d’une **IRemoteSystemFilter**spéciale, [**RemoteSystemAuthorizationKindFilter**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter). Elle est implémentée comme les autres types de filtres :

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Une valeur [**RemoteSystemAuthorizationKind**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) de **anonyme** permet la découverte de tous les appareils proximal, y compris ceux des utilisateurs non approuvés.
* La valeur **SameUser** filtre la détection uniquement sur les appareils inscrits auprès du même utilisateur que l’appareil client. Il s'agit du comportement par défaut.

### <a name="checking-the-cross-user-sharing-settings"></a>Vérification des paramètres de partage entre utilisateurs

Outre le filtre ci-dessus spécifié dans votre application de découverte, l’appareil client lui-même doit également être configuré pour autoriser les expériences partagées des appareils connectés avec d’autres utilisateurs. Il s’agit d’un paramètre système qui peut être interrogé à l’aide d’une méthode statique dans la classe **RemoteSystem** :

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Pour modifier ce paramètre, l’utilisateur doit ouvrir l’application **paramètres** . Dans le **System**menu Shared  >  **Experiences**Shared from  >  **Devices** , il existe une zone de liste déroulante dans laquelle l’utilisateur peut spécifier les appareils avec lesquels il peut partager le système.

![page des paramètres des expériences partagées](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Rubriques connexes
* [Applications et appareils connectés (projet Rome)](connected-apps-and-devices.md)
* [Lancer une application sur un appareil distant](launch-a-remote-app.md)
* [Référence sur l’API Systèmes distants](/uwp/api/Windows.System.RemoteSystems)
* [Exemple de systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)