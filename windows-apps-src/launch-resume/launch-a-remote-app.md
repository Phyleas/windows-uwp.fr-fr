---
title: Lancer une application sur un appareil distant
description: Découvrez comment lancer une application UWP ou une application de bureau Windows à distance sur un autre appareil, pour permettre à un utilisateur de démarrer une tâche sur un appareil et de le terminer sur un autre.
ms.date: 02/12/2018
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 4163106c5439ec8881c1b5042f63fb7abf4fd668
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362502"
---
# <a name="launch-an-app-on-a-remote-device"></a>Lancer une application sur un appareil distant

Cet article explique comment lancer une application Windows sur un appareil distant.

Depuis Windows 10 version 1607, une application UWP peut lancer une application UWP ou une application de bureau Windows à distance sur un autre appareil qui exécute également Windows 10 version 1607 ou ultérieure, pourvu que les deux appareils soient signés avec le même compte Microsoft (MSA). C’est le cas d’utilisation le plus simple du projet Rome.

La fonctionnalité de lancement à distance active les expériences utilisateur orientées tâche ; un utilisateur peut démarrer une tâche sur un appareil et le terminer sur un autre. Par exemple, si l’utilisateur est à l’écoute de musique sur son téléphone dans sa voiture, il peut ensuite lire les fonctionnalités sur son Xbox lorsqu’il arrive chez lui. Le lancement à distance permet aux applications de transmettre des données contextuelles à l’application distante en cours de lancement, afin de reprendre l’emplacement où la tâche a été interrompue.

## <a name="preliminary-setup"></a>Installation préliminaire

### <a name="add-the-remotesystem-capability"></a>Ajouter la fonctionnalité remoteSystem

Pour que votre application puisse lancer une application sur un appareil distant, vous devez ajouter la fonctionnalité `remoteSystem` dans le manifeste de votre package d’application. Vous pouvez utiliser le concepteur de manifeste de package pour l’ajouter en sélectionnant **système distant** sous l’onglet **fonctionnalités** , ou vous pouvez ajouter manuellement la ligne suivante au fichier _Package. appxmanifest_ de votre projet.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Activer le partage entre appareils

En outre, l’appareil client doit être configuré pour autoriser le partage entre appareils. Ce paramètre, qui est accessible dans **paramètres**: **System**  >  **expériences partagées du système partagé**  >  **entre les appareils**, est activé par défaut. 

![page des paramètres des expériences partagées](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Trouver un appareil distant

Vous devez tout d’abord trouver l’appareil auquel vous souhaitez vous connecter. La section [Détecter les périphériques distants](discover-remote-devices.md) explique en détail comment faire. Nous allons utiliser une approche simple, qui permet un filtrage par type d’appareil ou de connectivité. Nous allons créer un observateur qui recherche des appareils distants et écrire des gestionnaires d’événement qui surviennent lorsque des appareils sont détectés ou supprimés. Ce qui nous fournira un ensemble d’appareils distants.

Le code de ces exemples exige que vous disposiez `using Windows.System.RemoteSystems` d’une instruction dans vos fichiers de classe.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetBuildDeviceList":::

La première chose à faire avant d’effectuer lancement à distance est d’appeler `RemoteSystem.RequestAccessAsync()`. Vérifiez la valeur de retour pour vous assurer que votre application est autorisée à accéder à des appareils distants. Cette vérification peut échouer si vous n’avez ajouté la fonctionnalité `remoteSystem` à votre application.

Les gestionnaires d’événements de l’Observateur du système sont appelés lorsqu’un appareil auquel nous pouvons nous connecter est détecté ou n’est plus disponible. Nous allons utiliser ces gestionnaires d’événements pour conserver une liste mise à jour d’appareils auxquels nous pouvons nous connecter.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetEventHandlers":::


Nous assurons le suivi des appareils par ID de système distant, à l’aide d’un **dictionnaire**. Un objet **ObservableCollection** est utilisé pour stocker la liste des appareils que nous pouvons énumérer. Un objet **ObservableCollection** facilite aussi la liaison de la liste des appareils à l’interface utilisateur, mais nous nous en dispenserons dans cet exemple.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetMembers":::

Ajoutez un appel à `BuildDeviceList()` dans le code de démarrage de l’application avant d’essayer de lancer une application à distance.

## <a name="launch-an-app-on-a-remote-device"></a>Lancer une application sur un appareil distant

Lancez une application à distance en passant l’appareil auquel vous souhaitez vous connecter à l’API [**RemoteLauncher. LaunchUriAsync**](/uwp/api/windows.system.remotelauncher.launchuriasync) . Il existe trois surcharges pour cette méthode. La plus simple, reprise dans cet exemple, spécifie l’URI qui active l’application sur l’appareil distant. Dans cet exemple, l’URI ouvre l’application Cartes sur l’ordinateur distant avec une vue 3D sur l’Aiguille de l’espace de Seattle.

Les autres surcharges de **RemoteLauncher.LaunchUriAsync** vous permettent de spécifier des options telles que l’URI du site web à afficher si aucune application capable de gérer l’URI ne peut être lancée sur l’appareil distant, et une liste facultative de noms de famille de package pouvant servir à lancer l’URI sur l’appareil distant. Vous pouvez également fournir des données sous la forme de paires clé/valeur. Vous pouvez transmettre des données à l’application que vous activez pour fournir un contexte à l’application distante, comme le nom de la chanson à lire et le lieu de lecture en cours lorsque vous basculez la lecture d’un appareil à un autre.

Concrètement, vous pouvez fournir l’interface utilisateur pour sélectionner l’appareil que vous souhaitez cibler. Mais pour simplifier cet exemple, nous allons utiliser le premier appareil distant sur la liste.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetRemoteUriLaunch":::

L’objet [**RemoteLaunchUriStatus**](/uwp/api/windows.system.remotelaunchuristatus) renvoyé par **RemoteLauncher.LaunchUriAsync()** permet de savoir si le lancement à distance a abouti ou non (et dans ce cas, la raison de l’échec).

## <a name="related-topics"></a>Rubriques connexes

[Référence sur l’API Systèmes distants](/uwp/api/Windows.System.RemoteSystems)  
[Vue d’ensemble des applications et appareils connectés (Rome du projet)](connected-apps-and-devices.md)  
[Détecter des appareils distants](discover-remote-devices.md)  
L’[exemple Systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) montre comment détecter un système distant, lancer une application sur un système distant et utiliser des services d’application pour échanger des messages avec des applications qui s’exécutent sur deux systèmes.
