---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtenir les propriétés du fichier
description: Obtenez les propriétés &\#8212;de niveau supérieur, de base et étendues&\# d'un fichier représenté par un objet StorageFile.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1812908319bed81ab776e8e66753f4f881a8728f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156543"
---
# <a name="get-file-properties"></a>Obtenir les propriétés du fichier

**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtenez les propriétés (de niveau supérieur, de base et étendues) d’un fichier représenté par un objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Pour un exemple complet, consultez l'[Exemple d'accès aux fichiers](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Autorisations d'accès à l'emplacement**

    Par exemple, le code de ces exemples nécessite la fonctionnalité **picturesLibrary**, mais votre emplacement peut avoir besoin d’une autre fonctionnalité, voire d’aucune. Pour en savoir plus, voir [Autorisations d’accès aux fichiers](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtention des propriétés de niveau supérieur d’un fichier

De nombreuses propriétés de fichier de haut niveau sont accessibles en tant que membres de la classe [**StorageFile**](/uwp/api/Windows.Storage.StorageFile). Ces propriétés incluent les attributs des fichiers, le type de contenu, la date de création, le nom d’affichage, le type de fichier, etc.

> [!NOTE]
> N'oubliez pas de déclarer la fonctionnalité **picturesLibrary**.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, en accédant à quelques-unes des propriétés de niveau supérieur de chaque fichier.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>Obtention des propriétés de base d’un fichier

De nombreuses propriétés de fichier de base sont obtenues en appelant d’abord la méthode [**StorageFile.GetBasicPropertiesAsync**](/uwp/api/windows.storage.storagefile.getbasicpropertiesasync). Cette méthode retourne un objet [**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties) qui définit les propriétés de taille de l’élément (fichier ou dossier), ainsi que la date de sa dernière modification.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, en accédant à quelques-unes des propriétés de base de chacun d’eux.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>Obtention des propriétés étendues d’un fichier

Outre les propriétés de fichier de haut niveau et de base, il existe de nombreuses propriétés associées au contenu du fichier. Ces propriétés étendues sont accessibles en appelant la méthode [**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Un objet [**BasicProperties**](/uwp/api/Windows.Storage.FileProperties.BasicProperties) est obtenu en appelant la propriété [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties).) Alors que les propriétés de fichier de niveau supérieur et de base sont accessibles en tant que propriétés d’une classe ([**StorageFile**](/uwp/api/Windows.Storage.StorageFile) et **BasicProperties**, respectivement), les propriétés étendues sont obtenues en passant une collection [IEnumerable](/dotnet/api/system.collections.ienumerable) d’objets [String](/dotnet/api/system.string) qui représentent les noms des propriétés à récupérer à la méthode **BasicProperties.RetrievePropertiesAsync**. Cette méthode retourne ensuite une collection [IDictionary](/dotnet/api/system.collections.idictionary). Chaque propriété étendue est ensuite récupérée à partir de la collection par nom ou par index.

Cet exemple énumère tous les fichiers de la bibliothèque d’images, spécifie les noms des propriétés souhaitées (**DataAccessed** et **FileOwner**) dans un objet [List](/dotnet/api/system.collections.generic.list-1), passe cet objet [List](/dotnet/api/system.collections.generic.list-1) à [**BasicProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) pour récupérer ces propriétés, puis extrait celles-ci par nom de l’objet [IDictionary](/dotnet/api/system.collections.idictionary) retourné.

Pour obtenir la liste complète des propriétés étendues d'un fichier consultez [Propriétés principales de Windows](/windows/desktop/properties/core-bumper).

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 