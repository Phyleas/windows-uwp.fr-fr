---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Autorisations d’accès aux fichiers
description: Les applications peuvent accéder à certains emplacements du système de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers, ou en déclarant des fonctionnalités.
ms.date: 09/10/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.custom: contperfq1
ms.openlocfilehash: 6bb0e44fa5ea31d73feebdfb9ced9951cc2bc937
ms.sourcegitcommit: 2050175043c8c175be3312f17769b4af241b3275
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90027648"
---
# <a name="file-access-permissions"></a>Autorisations d’accès aux fichiers

Les applications de plateforme Windows universelle (UWP) peuvent accéder à certains emplacements de systèmes de fichiers par défaut. Les applications peuvent également accéder à des emplacements supplémentaires par le biais du sélecteur de fichiers, ou en déclarant des fonctionnalités.

## <a name="locations-that-all-apps-can-access"></a>Emplacements accessibles à toutes les applications

Lorsque vous créez une application, vous pouvez accéder par défaut aux emplacements suivants du système de fichiers :

### <a name="application-install-directory"></a>Répertoire d’installation de l’application
Dossier dans lequel votre application est installée sur le système de l’utilisateur.

Il existe deux manières principales d’accéder à des fichiers et des dossiers dans le répertoire d’installation de votre application :

1. Vous pouvez récupérer une classe [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) qui représente le répertoire d’installation de votre application, comme suit :

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    Vous pouvez accéder aux fichiers et dossiers du répertoire à l’aide des méthodes [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder). Dans l’exemple, cette classe **StorageFolder** est stockée dans la variable `installDirectory`. Pour plus d’informations sur l’utilisation de votre package d’application et du répertoire d’installation, consultez l’[exemple d’informations de package d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) sur GitHub.

2. Vous pouvez récupérer un fichier directement à partir du répertoire d’installation de votre application en utilisant un URI d’application, comme suit :

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    Une fois la méthode [**GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) exécutée, elle renvoie une classe [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) qui représente le fichier `file.txt` dans le répertoire d’installation de l’application (`file` dans cet exemple).
    
    Le préfixe « ms-appx:/// » de l’URI fait référence au répertoire d’installation de l’application. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment utiliser des URI pour référencer du contenu](/previous-versions/windows/apps/hh781215(v=win.10)).

En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans le répertoire d’installation de votre application à l’aide de certaines [API Win32 et COM pour les applications de plateforme Windows universelle (UWP)](/uwp/win32-and-com/win32-and-com-for-uwp-apps) et de certaines [fonctions de la bibliothèque C/C++ standard à partir de Microsoft Visual Studio](/cpp/cpp/c-cpp-language-and-standard-libraries).

Le répertoire d’installation de l’application est un emplacement en lecture seule. Vous ne pouvez pas accéder à ce répertoire d’installation par le biais du sélecteur de fichiers.

### <a name="application-data-locations"></a>Emplacements des données d’application
Les dossiers dans lesquels votre application peut stocker des données. Ces dossiers (local, roaming et temporary) sont créés lors de l’installation de votre application.

Il existe deux manières principales d’accéder à des fichiers et des dossiers dans les emplacements des données de votre application :

1.  Utilisez les propriétés [**ApplicationData**](/uwp/api/Windows.Storage.ApplicationData) pour récupérer un dossier de données de l’application.

    Par exemple, vous pouvez utiliser [**ApplicationData**](/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) pour récupérer une classe [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) qui représente le dossier local de votre application, comme suit :
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    Si vous souhaitez accéder au dossier roaming ou temporary de votre application, utilisez la propriété [**RoamingFolder**](/uwp/api/windows.storage.applicationdata.roamingfolder) ou [**TemporaryFolder**](/uwp/api/windows.storage.applicationdata.temporaryfolder) à la place.
    
    Une fois que vous avez récupéré une classe [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) qui représente un emplacement des données de l’application, vous pouvez accéder aux fichiers et dossiers situés dans cet emplacement en utilisant les méthodes **StorageFolder**. Dans cet exemple, ces objets **StorageFolder** sont stockés dans la variable `localFolder`. Pour plus d’informations sur l’utilisation des emplacements de données de l’application, consultez la page [Classe ApplicationData](/uwp/api/windows.storage.applicationdata), puis téléchargez l’[exemple de données d’application](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) à partir de GitHub.

2. Par exemple, vous pouvez récupérer un fichier directement à partir du dossier local de votre application en utilisant un URI d’application, comme suit :
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    Une fois la méthode [**GetFileFromApplicationUriAsync**](/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) exécutée, elle renvoie une classe [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) qui représente le fichier `file.txt` dans le dossier local de l’application (`file` dans cet exemple).
    
    Le préfixe « ms-appdata:///local/ » de l’URI fait référence au dossier local de l’application. Pour accéder à des fichiers dans les dossiers roaming ou temporary, utilisez « ms-appdata:///roaming/ » ou « ms-appdata:///temporary/ » à la place. Pour en savoir plus sur l’utilisation des URI d’application, voir [Comment charger des ressources de fichiers](/previous-versions/windows/apps/hh781229(v=win.10)).

En outre, contrairement à d’autres emplacements, vous pouvez également accéder à des fichiers dans les emplacements de données de votre application à l’aide de certaines [API Win32 et COM pour les applications UWP](/uwp/win32-and-com/win32-and-com-for-uwp-apps) et de certaines fonctions de la bibliothèque C/C++ standard à partir de Microsoft Visual Studio.

Vous ne pouvez pas accéder aux dossiers local, roaming ni temporary par le biais du sélecteur de fichiers.

### <a name="removable-devices"></a>Périphériques amovibles

De plus, votre application peut accéder par défaut à certains fichiers sur les périphériques connectés. Cela est une option si votre application utilise l’[extension de lecture automatique](/previous-versions/windows/apps/hh464906(v=win.10)) pour se lancer automatiquement lorsque l’utilisateur connecte un appareil, tel qu’un appareil photo ou une clé USB, à son système. Les fichiers auxquels votre application peut accéder sont limités aux types de fichiers spécifiques qui sont spécifiés par le biais de déclarations d’associations de types de fichiers dans le manifeste de votre application.

Bien entendu, vous pouvez également accéder à des fichiers et dossiers sur un périphérique amovible en appelant le sélecteur de fichiers (en utilisant [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) et [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)) et en laissant l’utilisateur sélectionner les fichiers et dossiers auxquels votre application pourra accéder. Découvrez comment utiliser le sélecteur de fichiers dans [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md).

> [!NOTE]
> Pour en savoir plus sur l’accès à une carte mémoire Secure Digital ou à d’autres appareils amovibles, consultez l’article [Accéder à la carte SD](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Emplacements accessibles aux applications UWP

### <a name="users-downloads-folder"></a>Dossier Téléchargements de l’utilisateur

Dossier dans lequel les fichiers téléchargés sont enregistrés par défaut.

Par défaut, votre application peut seulement accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur que votre application a créé. Toutefois, vous pouvez accéder aux fichiers et dossiers situés dans le dossier Téléchargements de l’utilisateur en appelant un sélecteur de fichiers ([**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) ou [**FolderPicker**](/uwp/api/Windows.Storage.Pickers.FolderPicker)) afin que les utilisateurs puissent parcourir et sélectionner les fichiers ou les dossiers auxquels votre application pourra accéder.

- Vous pouvez créer un fichier dans le dossier Téléchargements de l’utilisateur comme suit :

    ```csharp
    using Windows.Storage;
    StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
        function(newFile) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](/uwp/api/windows.storage.downloadsfolder.createfileasync) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un fichier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) qui représente le fichier qui a été créé. Ce fichier est appelé `newFile` dans cet exemple.

- Vous pouvez créer un sous-dossier dans le dossier Téléchargements de l’utilisateur comme suit :

    ```csharp
    using Windows.Storage;
    StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
    ```
    
    ```javascript
    Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
        function(newFolder) {
            // Process folder
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](/uwp/api/windows.storage.downloadsfolder.createfolderasync) est surchargé afin que vous puissiez spécifier ce que le système doit faire si le dossier Téléchargements contient déjà un sous-dossier du même nom. Une fois ces méthodes exécutées, elles renvoient une classe [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) qui représente le sous-dossier qui a été créé. Ce fichier est appelé `newFolder` dans cet exemple.

## <a name="accessing-additional-locations"></a>Accès à des emplacements supplémentaires

Outre les emplacements par défaut, une application peut accéder à des fichiers et dossiers supplémentaires en [déclarant des fonctionnalités dans le manifeste de l’application](../packaging/app-capability-declarations.md), ou en [appelant un sélecteur de fichiers](quickstart-using-file-and-folder-pickers.md) pour permettre à l’utilisateur de sélectionner les fichiers et les dossiers auxquels l’application peut accéder.

Les applications qui déclarent l’extension [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) disposent des autorisations de systèmes de fichiers à partir du répertoire d’où elles sont lancées dans la fenêtre de console et aux niveaux inférieurs.

### <a name="retaining-access-to-files-and-folders"></a>Conserver l’accès aux fichiers et aux dossiers

Lorsque votre application récupère un fichier ou un dossier à l’aide d’un sélecteur, d’une activation de fichier, d’une opération de glisser-déposer, etc., elle a uniquement accès à ce fichier ou à ce dossier pendant l’opération de récupération. Si vous souhaitez accéder automatiquement à ce fichier ou à ce dossier la prochaine fois, vous pouvez l’ajouter à la [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) pour permettre à votre application d’y accéder. Vous pouvez également utiliser la [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) pour gérer facilement la liste des fichiers récemment utilisés.

### <a name="capabilities-for-accessing-other-locations"></a>Fonctionnalités d’accès aux autres emplacements

Le tableau qui suit dresse une liste d’emplacements supplémentaires auxquels vous pouvez accéder en déclarant une ou plusieurs fonctionnalités, et en utilisant l’API [**Windows.Storage**](/uwp/api/Windows.Storage) associée.

| Emplacement | Fonctionnalité | API Windows.Storage |
|----------|------------|---------------------|
| Tous les fichiers auxquels l’utilisateur a accès. Par exemple : documents, images, photos, téléchargements, bureau, OneDrive, etc. | **broadFileSystemAccess**<br><br>Il s’agit d’une fonctionnalité restreinte. L’accès est configurable dans **Paramètres** > **Confidentialité** > **Système de fichiers**. Étant donné que les utilisateurs peuvent accorder ou refuser leur autorisation à tout moment dans **Paramètres**, vous devez vous assurer que votre application résiste à ces modifications. Si vous constatez que votre application ne dispose d’aucun accès, vous pouvez inviter l’utilisateur à modifier le paramètre en fournissant un lien vers l’article [Accès au système de fichiers Windows 10 et confidentialité](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy). L’utilisateur doit fermer l’application, activer/désactiver le paramètre et redémarrer l’application. S’il active/désactive le paramètre pendant l’exécution de l’application, la plateforme interrompt votre application de sorte que vous puissiez enregistrer l’état, puis quitter l’application afin d’appliquer le nouveau paramètre. Dans la mise à jour d’avril 2018, la valeur par défaut pour l’autorisation est Activé. Dans la mise à jour d’octobre 2018, la valeur par défaut est Désactivé.<br /><br />Si vous soumettez une application au Store qui déclare cette fonctionnalité, vous devez fournir des descriptions supplémentaires de la raison pour laquelle votre application a besoin de cette fonctionnalité, et comment elle a l’intention de l’utiliser.<br/><br/>Cette fonctionnalité concerne les API dans l’espace de noms [**Windows.Storage**](/uwp/api/Windows.Storage). Consultez la section **Exemple** à la fin de cet article pour découvrir comment activer cette fonctionnalité dans votre application.<br/><br/>**Remarque :** Cette fonctionnalité n’est pas prise en charge sur Xbox. | n/a |
| Documents | **documentsLibrary**<br><br>Remarque : vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. <br><br>Utilisez cette fonctionnalité si votre application :<br>- facilite l’accès hors connexion interplateforme à du contenu OneDrive spécifique à l’aide d’URL ou d’ID de ressource OneDrive valides ;<br>- Enregistre automatiquement les fichiers ouverts sur le OneDrive de l’utilisateur en mode hors connexion | [KnownFolders.DocumentsLibrary](/uwp/api/windows.storage.knownfolders.documentslibrary) |
| Musique     | **musicLibrary** <br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| Images  | **picturesLibrary**<br> Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| Vidéos    | **videosLibrary**<br>Voir également [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| Périphériques amovibles  | **removableStorage**  <br><br>Remarque : Vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. <br><br>Voir également [Accéder à la carte SD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](/uwp/api/windows.storage.knownfolders.removabledevices) |  
| Bibliothèques du groupe résidentiel  | Au moins une des fonctionnalités suivantes est requise. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](/uwp/api/windows.storage.knownfolders.homegroup) |      
| Appareils de serveur multimédia (DLNA) | Au moins une des fonctionnalités suivantes est requise. <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| Dossiers UNC (Universal Naming Convention) | Une combinaison des fonctionnalités suivantes est requise. <br><br>Fonctionnalité des réseaux domestiques et d’entreprise : <br>- **privateNetworkClientServer** <br><br>Et au moins une fonctionnalité de réseaux Internet et publics : <br>- **internetClient** <br>- **internetClientServer** <br><br>Et, le cas échéant, la fonctionnalité des informations d’identification de domaine :<br>- **enterpriseAuthentication** <br><br>**Remarque :** Vous devez ajouter des associations de types de fichiers dans le manifeste de l’application pour déclarer les types de fichiers spécifiques auxquels votre application pourra accéder dans cet emplacement. | Récupérez un dossier en utilisant : <br>[StorageFolder.GetFolderFromPathAsync](/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>Récupérez un fichier en utilisant : <br>[StorageFile.GetFileFromPathAsync](/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>Exemple

Cet exemple ajoute la fonctionnalité restreinte **broadFileSystemAccess**. Outre la spécification de la fonctionnalité, l’espace de noms `rescap` doit être ajouté, et il est également ajouté à `IgnorableNamespaces`.

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Pour obtenir la liste complète des fonctionnalités de l’application, consultez l’article [Déclarations des fonctionnalités d’application](../packaging/app-capability-declarations.md).
