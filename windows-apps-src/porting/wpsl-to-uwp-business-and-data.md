---
description: Derrière votre interface utilisateur se trouvent les couches métier et les couches de données.
title: Portage Windows Phone couches métier et de données Silverlight vers UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25d8bba5e1b26613185017642d63128cc2b1f7f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259091"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Portage Windows Phone couches métier et de données Silverlight vers UWP


Rubrique précédente : [Portage pour le modèle d’E/S, d’appareil et d’application](wpsl-to-uwp-input-and-sensors.md).

Derrière votre interface utilisateur se trouvent les couches métier et les couches de données. Le code de ces couches appelle le système d’exploitation et les API .NET Framework (par exemple, pour le traitement en arrière-plan, l’emplacement, l’appareil photo, le système de fichiers, le réseau et l’accès à d’autres données). La grande majorité de ces éléments sont [disponibles pour une application de plateforme Windows universelle (UWP)](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)). Vous pouvez donc espérer être en mesure de porter la plus grande partie de ce code sans avoir à le modifier.

## <a name="asynchronous-methods"></a>Méthodes asynchrones

L’une des priorités de la plateforme Windows universelle (UWP) consiste à vous permettre de générer des applications qui réagissent efficacement et de manière cohérente. Les animations sont toujours fluides, et les interactions tactiles (telles que le balayage et les mouvements panoramiques) sont instantanées, sans retard, ce qui donne le sentiment que l’interface utilisateur ne fait qu’un avec votre doigt. Pour ce faire, toute API UWP qui ne peut pas garantir qu’elle s’exécutera en 50 millisecondes maximum est rendue asynchrone ; son nom comprend le suffixe suivant : **Async**. Votre thread d’interface utilisateur réapparaît immédiatement suite à l’appel d’une méthode **Async** ; le travail est alors effectué sur un autre thread. L’utilisation d’une méthode **Async** a été facilitée via l’optimisation de la syntaxe, au moyen de l’opérateur C# **await**, des objets de promesse JavaScript et des continuations C++. Pour en savoir plus, voir [Programmation asynchrone](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).

## <a name="background-processing"></a>Traitement en arrière-plan

Une application Windows Phone Silverlight peut utiliser un objet **ScheduledTaskAgent** géré pour effectuer une tâche alors que l’application n’est pas au premier plan. Une application UWP utilise la classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) pour créer et enregistrer une tâche en arrière-plan, de la même manière. Vous définissez une classe qui implémente le travail effectué par votre tâche en arrière-plan. Le système exécute régulièrement votre tâche en arrière-plan, en appelant la méthode [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de votre classe pour exécuter le travail. Dans une application UWP, n’oubliez pas de définir la déclaration **Tâches en arrière-plan** dans le manifeste du package d’application. Pour plus d’informations, voir [Définir des tâches en arrière-plan pour les besoins de votre application](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Pour transférer des fichiers de données volumineux en arrière-plan, une application Windows Phone Silverlight utilise la classe **BackgroundTransferService** . Pour effectuer cette opération, une application UWP utilise des API de l’espace de noms [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer). Les fonctions utilisent un modèle semblable pour lancer des transferts, mais la nouvelle API présente des performances et fonctionnalités optimisées. Pour en savoir plus, voir [Transfert de données en arrière-plan](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

Une application Windows Phone Silverlight utilise les classes managées de l’espace de noms **Microsoft. Phone. BackgroundAudio** pour lire l’audio pendant que l’application n’est pas au premier plan. UWP utilise le modèle d’application Windows Phone Store. Voir [Contenu audio en arrière-plan](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) et l’exemple [Contenu audio en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio).

## <a name="cloud-services-networking-and-databases"></a>Services cloud, mise en réseau et bases de données

L’hébergement de services de données et d’application dans le cloud est possible par le biais de la plateforme Azure. Voir [Prise en main de Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/). Pour les solutions qui nécessitent à la fois des données en ligne et des données hors connexion, voir [Utilisation de la synchronisation des données hors connexion dans Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

UWP offre une prise en charge partielle de la classe **System.Net.HttpWebRequest**, mais ne prend pas en charge la classe **System.Net.WebClient**. L’alternative prospective recommandée est la classe [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)) si vous avez besoin que votre code soit portable vers d’autres plateformes prenant en charge .NET). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) pour représenter une requête HTTP.

À l’heure actuelle, les applications UWP ne prennent pas en charge les scénarios qui impliquent une utilisation intensive des données, notamment les scénarios métier. Toutefois, vous pouvez utiliser SQLite pour gérer les services de base de données transactionnelle en local. Pour plus d’informations, voir [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Transmettez les URI absolus, et non relatifs, aux types Windows Runtime. Voir [Transmission d’un URI au Windows Runtime](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Lanceurs et sélecteurs

Avec les lanceurs et les sélecteurs (qui se trouvent dans l’espace de noms **Microsoft. Phone. Tasks** ), une application Windows Phone Silverlight peut interagir avec le système d’exploitation pour effectuer des opérations courantes, telles que la composition d’un message électronique, le choix d’une photo ou le partage de certains types de données avec une autre application. Recherchez **Microsoft. Phone. Tasks** dans la rubrique [Windows Phone les mappages de classe et d’espace de noms Silverlight vers Windows 10](wpsl-to-uwp-namespace-and-class-mappings.md) pour trouver le type UWP équivalent. Ces éléments vont des mécanismes du même ordre (lanceurs et sélecteurs) à l’implémentation d’un contrat de partage de données entre applications.

Un Windows Phone application Silverlight peut être placé dans un état dormant ou même désactivé quand vous utilisez, par exemple, la tâche de sélection de photos. Une application UWP reste active et en cours d’exécution lors de l’utilisation de la classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monétisation (mode d’évaluation et achats dans l’application)

Une application Windows Phone Silverlight peut utiliser la classe UWP [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) pour la plupart de ses fonctionnalités de mode d’évaluation et d’achat dans l’application, de sorte que le code n’a pas besoin d’être porté. Toutefois, une application Windows Phone Silverlight appelle **MarketplaceDetailTask. Show** pour proposer l’application à acheter :

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Portez le code suivant pour appeler la méthode UWP [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) :

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Si vous disposez de code qui simule les fonctionnalités d’achat dans l’application et d’achat d’application à des fins de test, vous pouvez porter ce code afin qu’il utilise plutôt la classe [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator).

## <a name="notifications-for-tile-or-toast-updates"></a>Notifications relatives aux mises à jour de vignette ou toast

Les notifications sont une extension du modèle de notification push pour les applications Windows Phone Silverlight. Lorsque vous recevez une notification provenant des services de notifications Push Windows (WNS), vous pouvez faire apparaître les informations sur l’interface utilisateur, avec une mise à jour de vignette ou avec un toast. Pour le portage du côté de l’interface utilisateur dédié à vos fonctionnalités de notification, voir [Vignettes et toasts](w8x-to-uwp-porting-xaml-and-ui.md).

Pour plus d’informations sur l’utilisation des notifications dans une application UWP, voir [Envoi de notifications toast](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Pour obtenir des informations et des didacticiels sur l’utilisation des vignettes, des toasts, des badges, des bannières et des notifications dans une application Windows Runtime utilisant C++, C# ou Visual Basic, voir [Utilisation de vignettes, de badges et de notifications toast](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Stockage (accès aux fichiers)

Windows Phone code Silverlight qui stocke les paramètres de l’application en tant que paires clé-valeur dans le stockage isolé est facilement porté. Voici un exemple avant et après, la première Windows Phone version de Silverlight :

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

Et son équivalent UWP :

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Bien qu’un sous-ensemble de l’espace de noms **Windows. Storage** soit disponible pour eux, de nombreux Windows Phone les applications Silverlight effectuent des e/s de fichier avec la classe **IsolatedStorageFile** , car elles sont prises en charge plus longtemps. En supposant que **IsolatedStorageFile** est utilisé, voici un exemple avant et après l’écriture et la lecture d’un fichier, la première Windows Phone version de Silverlight :

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

Et son équivalent pour UWP :

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Une application Windows Phone Silverlight dispose d’un accès en lecture seule à la carte SD facultative. Une application UWP présente quant à elle un accès en lecture et en écriture à la carte mémoire Secure Digital. Pour plus d’informations, voir [Accéder à la carte SD](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Pour plus d’informations sur l’accès aux fichiers photo, musique et vidéo dans une application UWP, voir [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Pour plus d’informations, voir [Fichiers, dossiers et bibliothèques](https://docs.microsoft.com/windows/uwp/files/index).

Rubrique suivante : [Portage pour différents facteurs de forme et expériences utilisateur](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Rubriques connexes

* [Mappages d’espaces de noms et de classes](wpsl-to-uwp-namespace-and-class-mappings.md)
 

