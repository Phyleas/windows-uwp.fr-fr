---
Description: Découvrez comment stocker et récupérer des données d’application locales, itinérantes et temporaires.
title: Stocker et récupérer des paramètres et d’autres données d’application
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690368"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>Stocker et récupérer des paramètres et d’autres données d’application

Les *données d’application* sont des données mutables qui sont créées et gérées par une application spécifique. Il comprend l’état d’exécution, les paramètres d’application, les préférences de l’utilisateur, le contenu de référence (par exemple, les définitions de dictionnaire dans une application de dictionnaire) et d’autres paramètres. Les données d’application diffèrent des données *utilisateur*, des données que l’utilisateur crée et gère lors de l’utilisation d’une application. Les données utilisateur incluent les documents ou les fichiers multimédias, les transcriptions de messagerie ou de communication, ou les enregistrements de base de données contenant le contenu créé par l’utilisateur. Les données utilisateur peuvent être utiles ou significatives pour plusieurs applications. Il s’agit souvent de données que l’utilisateur souhaite manipuler ou transmettre en tant qu’entité indépendante de l’application elle-même, par exemple un document.

**Remarque importante à propos des données d’application :** La durée de vie des données de l’application est liée à la durée de vie de l’application. Si l’application est supprimée, toutes les données de l’application seront perdues en conséquence. N’utilisez pas de données d’application pour stocker des données utilisateur ou tout ce que les utilisateurs peuvent percevoir comme précieux et irremplaçables. Nous vous recommandons d’utiliser les bibliothèques de l’utilisateur et Microsoft OneDrive pour stocker ce genre d’informations. Les données d’application sont idéales pour le stockage des préférences, des paramètres et des favoris de l’utilisateur spécifiques à l’application.

## <a name="types-of-app-data"></a>Types de données d’application

Il existe deux types de données d’application : les paramètres et les fichiers.

### <a name="settings"></a>Paramètres

Utilisez les paramètres pour stocker les préférences utilisateur et les informations d’état de l’application. L’API de données d’application vous permet de créer et de récupérer facilement des paramètres (nous vous présenterons des exemples plus loin dans cet article).

Voici les types de données que vous pouvez utiliser pour les paramètres de l’application :

- **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **double**
- **Booléen**
- **Char16**, **chaîne**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime), [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - Pour C#/.net, utilisez : [**System. DateTimeOffset**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0), [**System. TimeSpan**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)
- **GUID**, [**point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**taille**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size), [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue): ensemble de paramètres d’application associés qui doivent être sérialisés et désérialisés atomiquement. Utilisez des paramètres composites pour gérer facilement les mises à jour atomiques des paramètres interdépendants. Le système garantit l’intégrité des paramètres composites lors de l’accès simultané et de l’itinérance. Les paramètres composites sont optimisés pour de petites quantités de données, et les performances peuvent être médiocres si vous les utilisez pour des jeux de données volumineux.

### <a name="files"></a>Fichiers

Utilisez des fichiers pour stocker des données binaires ou pour activer vos propres types sérialisés personnalisés.

## <a name="storing-app-data-in-the-app-data-stores"></a>Stockage des données d’application dans les magasins de données d’application


Quand une application est installée, le système lui fournit ses propres banques de données par utilisateur pour les paramètres et les fichiers. Vous n’avez pas besoin de savoir où et comment ces données existent, car le système est responsable de la gestion du stockage physique, en veillant à ce que les données restent isolées des autres applications et des autres utilisateurs. Le système conserve également le contenu de ces banques de données lorsque l’utilisateur installe une mise à jour de votre application et supprime le contenu de ces banques de données de façon complète et propre lorsque votre application est désinstallée.

Dans son magasin de données d’application, chaque application possède des répertoires racine définis par le système : un pour les fichiers locaux, un pour les fichiers itinérants et un autre pour les fichiers temporaires. Votre application peut ajouter de nouveaux fichiers et de nouveaux conteneurs à chacun de ces répertoires racine.

## <a name="local-app-data"></a>Données de l’application locale


Les données d’application locales doivent être utilisées pour toutes les informations qui doivent être conservées entre les sessions d’application et ne sont pas appropriées pour les données d’application itinérantes. Les données qui ne s’appliquent pas aux autres appareils doivent également être stockées ici. Il n’existe aucune restriction de taille générale sur les données locales stockées. Utilisez le magasin de données de l’application locale pour les données qui n’ont pas de sens pour l’itinérance et pour les jeux de données volumineux.

### <a name="retrieve-the-local-app-data-store"></a>Récupérer le magasin de données de l’application locale

Avant de pouvoir lire ou écrire les données de l’application locale, vous devez récupérer le magasin de données de l’application locale. Pour récupérer le magasin de données de l’application locale, utilisez la propriété [**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) pour obtenir les paramètres locaux de l’application en tant qu’objet [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) . Utilisez la propriété [**ApplicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) pour obtenir les fichiers dans un objet [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) . Utilisez la propriété [**ApplicationData. LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder) pour récupérer le dossier dans le magasin de données de l’application locale, où vous pouvez enregistrer des fichiers qui ne sont pas inclus dans la sauvegarde et la restauration.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>Créer et récupérer un paramètre local simple

Pour créer ou écrire un paramètre, utilisez la propriété [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) pour accéder aux paramètres dans le conteneur `localSettings` que nous avons obtenu à l’étape précédente. Cet exemple crée un paramètre nommé `exampleSetting`.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Pour récupérer le paramètre, vous utilisez la même propriété [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) que celle utilisée pour créer le paramètre. Cet exemple montre comment récupérer le paramètre que nous venons de créer.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>Créer et récupérer une valeur composite locale

Pour créer ou écrire une valeur composite, créez un objet [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) . Cet exemple crée un paramètre composite nommé `exampleCompositeSetting` et l’ajoute au conteneur `localSettings`.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

Cet exemple montre comment récupérer la valeur composite que nous venons de créer.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>Créer et lire un fichier local

Pour créer et mettre à jour un fichier dans le magasin de données de l’application locale, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) et [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Cet exemple crée un fichier nommé `dataFile.txt` dans le conteneur `localFolder` et écrit la date et l’heure actuelles dans le fichier. La valeur **ReplaceExisting** de l’énumération [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indique de remplacer le fichier s’il existe déjà.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données de l’application locale, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)et [**Windows. Storage. FileIO. méthode readtextasync** ](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Cet exemple ouvre le fichier `dataFile.txt` créé à l’étape précédente et lit la date à partir du fichier. Pour plus d’informations sur le chargement des ressources de fichiers à partir de différents emplacements, consultez [Comment charger des ressources de fichier](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>Données itinérantes


Si vous utilisez des données itinérantes dans votre application, vos utilisateurs peuvent facilement synchroniser les données d’application de votre application sur plusieurs appareils. Si un utilisateur installe votre application sur plusieurs appareils, le système d’exploitation conserve les données de l’application synchronisées, ce qui réduit la quantité de travail d’installation que l’utilisateur doit effectuer pour votre application sur son deuxième appareil. L’itinérance permet également à vos utilisateurs de continuer une tâche, telle que la composition d’une liste, juste là où elle s’est arrêtée sur un autre appareil. Le système d’exploitation réplique les données itinérantes dans le Cloud lorsqu’il est mis à jour et synchronise les données avec les autres appareils sur lesquels l’application est installée.

Le système d’exploitation limite la taille des données d’application que chaque application peut déplacer. Consultez [**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota). Si l’application atteint cette limite, aucune des données d’application de l’application n’est répliquée dans le Cloud tant que les données de l’application itinérante totale de l’application ne sont pas de nouveau inférieures à la limite. Pour cette raison, il est recommandé d’utiliser des données itinérantes uniquement pour les préférences de l’utilisateur, les liens et les petits fichiers de données.

Les données d’itinérance pour une application sont disponibles dans le Cloud tant qu’elles sont accessibles par l’utilisateur à partir d’un appareil dans l’intervalle de temps requis. Si l’utilisateur n’exécute pas une application plus longtemps que cet intervalle de temps, ses données itinérantes sont supprimées du Cloud. Si un utilisateur désinstalle une application, ses données d’itinérance ne sont pas automatiquement supprimées du Cloud, elles sont conservées. Si l’utilisateur réinstalle l’application dans l’intervalle de temps, les données itinérantes sont synchronisées à partir du Cloud.

### <a name="roaming-data-dos-and-donts"></a>Les données d’itinérance n’ont pas

- Utilisez l’itinérance pour les préférences et personnalisations de l’utilisateur, les liens et les petits fichiers de données. Par exemple, utilisez l’itinérance pour préserver la préférence de couleur d’arrière-plan d’un utilisateur sur tous les appareils.
- Utilisez l’itinérance pour permettre aux utilisateurs de continuer une tâche sur plusieurs appareils. Par exemple, les données d’application itinérantes comme le contenu d’un brouillon de message électronique ou la page affichée le plus récemment dans une application de lecteur.
- Gérez l’événement [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) en mettant à jour les données d’application. Cet événement se produit lorsque les données de l’application ont juste terminé la synchronisation à partir du Cloud.
- Des références itinérantes au contenu plutôt qu’à des données brutes. Par exemple, itinérance d’une URL plutôt que du contenu d’un article en ligne.
- Pour les paramètres importants à temps critique, utilisez le paramètre *HighPriority* associé à [**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings).
- Ne pas rendre itinérantes les données d’application spécifiques à un appareil. Certaines informations sont pertinentes uniquement en local, par exemple un nom de chemin d’accès à une ressource de fichier local. Si vous décidez d’utiliser des informations locales itinérantes, assurez-vous que l’application peut récupérer si les informations ne sont pas valides sur l’appareil secondaire.
- Ne pas rendre itinérants de grands ensembles de données d’application. Il y a une limite à la quantité de données d’application qu’une application peut déplacer. Utilisez la propriété [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) pour atteindre ce nombre maximal. Si une application atteint cette limite, aucune donnée ne peut être itinérante tant que la taille du magasin de données de l’application n’est plus supérieure à la limite. Lorsque vous concevez votre application, réfléchissez à la manière de placer une liaison sur des données plus volumineuses afin de ne pas dépasser la limite. Par exemple, si l’enregistrement d’un état de jeu requiert 10 Ko, l’application peut uniquement autoriser l’utilisateur à stocker jusqu’à 10 jeux.
- N’utilisez pas l’itinérance pour les données qui s’appuient sur la synchronisation instantanée. Windows ne garantit pas une synchronisation instantanée. l’itinérance peut être considérablement retardée si un utilisateur est hors connexion ou sur un réseau à latence élevée. Assurez-vous que votre interface utilisateur ne dépend pas de la synchronisation instantanée.
- N’utilisez pas l’itinérance pour les données fréquemment modifiées. Par exemple, si votre application effectue le suivi des informations fréquemment modifiées, telles que la position d’une chanson par seconde, ne les stockez pas comme données d’application itinérante. Au lieu de cela, choisissez une représentation moins fréquente qui offre toujours une bonne expérience utilisateur, comme la chanson actuellement jouée.

### <a name="roaming-pre-requisites"></a>Conditions préalables pour l’itinérance

Tout utilisateur peut tirer parti des données de l’application itinérante s’il utilise un compte Microsoft pour se connecter à son appareil. Toutefois, les utilisateurs et les administrateurs de stratégie de groupe peuvent désactiver l’itinérance des données d’application sur un appareil à tout moment. Si un utilisateur choisit de ne pas utiliser un compte Microsoft ou désactive les fonctionnalités de données itinérantes, il peut toujours utiliser votre application, mais les données d’application sont locales sur chaque appareil.

Les données stockées dans [**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault) sont uniquement transition si un utilisateur a effectué un « approuvé ». Si un appareil n’est pas approuvé, les données sécurisées dans ce coffre ne sont pas itinérantes.

### <a name="conflict-resolution"></a>Résolution des conflits

L’itinérance des données d’application n’est pas destinée à une utilisation simultanée sur plusieurs appareils à la fois. Si un conflit se produit pendant la synchronisation parce qu’une unité de données particulière a été modifiée sur deux appareils, le système privilégie toujours la valeur qui a été écrite en dernier. Cela garantit que l’application utilise les informations les plus récentes. Si l’unité de données est un paramètre composite, la résolution des conflits se produira toujours au niveau de l’unité de paramètre, ce qui signifie que le composite avec la dernière modification sera synchronisé.

### <a name="when-to-write-data"></a>Quand écrire des données

Selon la durée de vie attendue du paramètre, les données doivent être écrites à des moments différents. Les données d’application changeant rarement ou lentement doivent être écrites immédiatement. Toutefois, les données d’application qui changent fréquemment doivent être écrites régulièrement à intervalles réguliers (par exemple, une fois toutes les 5 minutes), ainsi que lorsque l’application est interrompue. Par exemple, une application de musique peut écrire les paramètres « chanson actuelle » à chaque fois qu’une nouvelle chanson commence, mais la position réelle dans la chanson ne doit être écrite qu’à la suspension.

### <a name="excessive-usage-protection"></a>Protection excessive de l’utilisation

Le système a plusieurs mécanismes de protection en place afin d’éviter une utilisation inappropriée des ressources. Si les données d’application ne se transforment pas comme prévu, il est probable que l’appareil a été temporairement restreint. L’attente d’un certain temps permet généralement de résoudre automatiquement cette situation et aucune action n’est nécessaire.

### <a name="versioning"></a>Contrôle de version

Les données d’application peuvent utiliser le contrôle de version pour effectuer une mise à niveau d’une structure de données vers une autre. Le numéro de version est différent de la version de l’application et peut être défini sur. Bien que cela ne soit pas appliqué, il est fortement recommandé d’utiliser des numéros de version croissants, car les complications indésirables (y compris la perte de données) peuvent se produire si vous essayez de passer à un numéro de version de données inférieur qui représente des données plus récentes.

Les données d’application sont itinérantes uniquement entre les applications installées avec le même numéro de version. Par exemple, les appareils sur la version 2 migreront les données entre les autres et les appareils de la version 3 feront la même chose, mais aucun itinérance ne se produira entre un appareil exécutant la version 2 et un appareil exécutant la version 3. Si vous installez une nouvelle application qui utilisait différents numéros de version sur d’autres appareils, l’application nouvellement installée synchronisera les données d’application associées au numéro de version le plus élevé.

### <a name="testing-and-tools"></a>Test et outils

Les développeurs peuvent verrouiller leur appareil afin de déclencher la synchronisation des données d’application itinérantes. S’il semble que les données de l’application n’effectuent pas de transition dans un intervalle de temps donné, vérifiez les éléments suivants et assurez-vous que :

- Vos données d’itinérance ne dépassent pas la taille maximale (pour plus d’informations, consultez [**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) ).
- Vos fichiers sont fermés et libérés correctement.
- Au moins deux appareils exécutent la même version de l’application.


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>S’inscrire pour recevoir une notification lors de la modification des données itinérantes

Pour utiliser les données d’application itinérante, vous devez vous inscrire pour les modifications de données itinérantes et récupérer les conteneurs de données itinérantes afin de pouvoir lire et écrire des paramètres.

1.  Inscrivez-vous pour recevoir une notification lors de la modification des données itinérantes.

    L’événement [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged) vous avertit lorsque des données itinérantes sont modifiées. Cet exemple définit `DataChangeHandler` comme gestionnaire pour les modifications de données itinérantes.

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Obtenir les conteneurs pour les paramètres et les fichiers de l’application.

    Utilisez la propriété [**ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) pour accéder aux paramètres et à la propriété [**ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) pour récupérer les fichiers.

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>Créer et récupérer des paramètres d’itinérance

Utilisez la propriété [**ApplicationDataContainer. Values**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values) pour accéder aux paramètres dans le conteneur `roamingSettings` que nous avons obtenu dans la section précédente. Cet exemple crée un paramètre simple nommé `exampleSetting` et une valeur composite nommée `composite`.

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

Cet exemple récupère les paramètres que nous venons de créer.

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>Créer et récupérer des fichiers itinérants

Pour créer et mettre à jour un fichier dans le magasin de données d’application d’itinérance, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) et [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Cet exemple crée un fichier nommé `dataFile.txt` dans le conteneur `roamingFolder` et écrit la date et l’heure actuelles dans le fichier. La valeur **ReplaceExisting** de l’énumération [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indique de remplacer le fichier s’il existe déjà.

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données d’application d’itinérance, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync), et [ **Windows. Storage. FileIO. méthode readtextasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Cet exemple ouvre le fichier `dataFile.txt` créé dans la section précédente et lit la date à partir du fichier. Pour plus d’informations sur le chargement des ressources de fichiers à partir de différents emplacements, consultez [Comment charger des ressources de fichier](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>Données d’application temporaires


Le magasin de données d’application temporaire fonctionne comme un cache. Ses fichiers ne sont pas itinérants et peuvent être supprimés à tout moment. La tâche de maintenance système peut supprimer automatiquement les données stockées à cet emplacement à tout moment. L’utilisateur peut également effacer des fichiers du magasin de données temporaire à l’aide du nettoyage de disque. Les données temporaires de l’application peuvent être utilisées pour stocker des informations temporaires pendant une session d’application. Il n’y a aucune garantie que ces données sont conservées au-delà de la fin de la session de l’application, car le système peut récupérer l’espace utilisé si nécessaire. L’emplacement est disponible via la propriété [**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) .

### <a name="retrieve-the-temporary-data-container"></a>Récupérer le conteneur de données temporaire

Utilisez la propriété [**ApplicationData. temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) pour récupérer les fichiers. Les étapes suivantes utilisent la variable `temporaryFolder` de cette étape.

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>Créer et lire des fichiers temporaires

Pour créer et mettre à jour un fichier dans le magasin de données d’application temporaire, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) et [**Windows. Storage. FileIO. WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync). Cet exemple crée un fichier nommé `dataFile.txt` dans le conteneur `temporaryFolder` et écrit la date et l’heure actuelles dans le fichier. La valeur **ReplaceExisting** de l’énumération [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption) indique de remplacer le fichier s’il existe déjà.


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

Pour ouvrir et lire un fichier dans le magasin de données d’application temporaire, utilisez les API de fichier, telles que [**Windows. Storage. StorageFolder. GetFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync), [**Windows. Storage. StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync), et [ **Windows. Storage. FileIO. méthode readtextasync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). Cet exemple ouvre le fichier `dataFile.txt` créé à l’étape précédente et lit la date à partir du fichier. Pour plus d’informations sur le chargement des ressources de fichiers à partir de différents emplacements, consultez [Comment charger des ressources de fichier](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10)).

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>Organiser les données d’application avec des conteneurs


Pour vous aider à organiser vos fichiers et paramètres de données d’application, vous créez des conteneurs (représentés par les objets [**ApplicationDataContainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer) ) au lieu de travailler directement avec des répertoires. Vous pouvez ajouter des conteneurs aux magasins de données d’application locaux, itinérants et temporaires. Les conteneurs peuvent être imbriqués jusqu’à 32 niveaux de profondeur.

Pour créer un conteneur de paramètres, appelez la méthode [**ApplicationDataContainer. CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer) . Cet exemple crée un conteneur de paramètres local nommé `exampleContainer` et ajoute un paramètre nommé `exampleSetting`. La valeur **Always** de l’énumération [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition) indique que le conteneur est créé s’il n’existe pas déjà.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>Supprimer les paramètres et les conteneurs de l’application


Pour supprimer un paramètre simple dont votre application n’a plus besoin, utilisez la méthode [**ApplicationDataContainerSettings. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove) . Cet exemple deletesthe `exampleSetting` paramètre local que nous avons créé précédemment.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Pour supprimer un paramètre composite, utilisez la méthode [**ApplicationDataCompositeValue. Remove**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove) . Cet exemple supprime le paramètre local `exampleCompositeSetting` composite que nous avons créé dans un exemple précédent.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Pour supprimer un conteneur, appelez la méthode [**ApplicationDataContainer. DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer) . Cet exemple supprime le conteneur de paramètres de `exampleContainer` local que nous avons créé précédemment.

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>Contrôle de version des données de votre application


Vous pouvez éventuellement préverser les données de l’application pour votre application. Cela vous permettrait de créer une version future de votre application qui modifiera le format de ses données d’application sans causer de problèmes de compatibilité avec la version précédente de votre application. L’application vérifie la version des données de l’application dans le magasin de données, et si la version est inférieure à la version attendue par l’application, l’application doit mettre à jour les données de l’application au nouveau format et mettre à jour la version. Pour plus d’informations, consultez la propriété[**application. version**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version) et la méthode [**ApplicationData. SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) .

## <a name="related-articles"></a>Articles connexes

* [**Windows. Storage. ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**Windows. Storage. ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**Windows. Storage. ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**Windows. Storage. ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**Windows. Storage. ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
