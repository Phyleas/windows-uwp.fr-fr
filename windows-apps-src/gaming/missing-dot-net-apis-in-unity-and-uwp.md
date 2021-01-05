---
title: API .NET manquantes dans Unity et UWP
description: Découvrez les API .NET manquantes lors de la création de jeux UWP dans Unity, ainsi que des solutions de contournement pour les problèmes courants.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: Windows 10, UWP, jeux, .net, Unity
ms.localizationpriority: medium
ms.openlocfilehash: b687f3ec09a99ae6ccb81e5c205eb454e0af0e04
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860116"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>API .NET manquantes dans Unity et UWP

Lors de la création d’un jeu UWP à l’aide de .NET, vous pouvez constater que certaines API que vous pouvez utiliser dans l’éditeur Unity ou pour un jeu PC autonome ne sont pas présentes pour UWP. Cela est dû au fait que .NET pour les applications UWP comprend un sous-ensemble des types fournis dans le .NET Framework complet pour chaque espace de noms.

En outre, certains moteurs de jeu utilisent des versions différentes de .NET qui ne sont pas entièrement compatibles avec .NET pour UWP, telles que les mono de Unity. Ainsi, lorsque vous écrivez votre jeu, tout peut fonctionner correctement dans l’éditeur, mais lorsque vous vous rendez à la génération pour UWP, vous pouvez obtenir des erreurs telles que : **le type ou l’espace de noms’Formatters’n’existe pas dans l’espace de noms’System. Runtime. Serialization' (une référence d’assembly est-elle manquante ?)**

Heureusement, Unity fournit certaines de ces API manquantes comme méthodes d’extension et types de remplacement, qui sont décrits dans [plateforme Windows universelle : types .net manquants sur le serveur principal de script .net](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). Toutefois, si la fonctionnalité dont vous avez besoin n’est pas ici, la [vue d’ensemble de .net pour les applications Windows 8. x](/previous-versions/windows/apps/br230302(v=vs.140)) explique comment vous pouvez convertir votre code pour utiliser WinRT ou .net pour les API Windows Runtime. (Il aborde Windows 8, mais s’applique également aux applications UWP Windows 10.)

## <a name="net-standard"></a>.NET Standard

Pour comprendre pourquoi certaines API peuvent ne pas fonctionner, il est important de comprendre les différentes versions de .NET et la façon dont UWP met en œuvre .NET. Le [.NET standard](/dotnet/standard/net-standard) est une spécification formelle des API .net qui est censée être multiplateforme et unifier les différentes versions de .net. Chaque implémentation de .NET prend en charge une certaine version du .NET Standard. Vous pouvez consulter un tableau des normes et des implémentations au niveau de la [prise en charge de l’implémentation .net](/dotnet/standard/net-standard#net-implementation-support).

Chaque version du kit de développement logiciel (SDK) UWP est conforme à un autre niveau de .NET Standard. Par exemple, le kit de développement logiciel (SDK) 16299 (la mise à jour des créateurs de automne) prend en charge .NET Standard 2,0.

Si vous souhaitez savoir si une certaine API .NET est prise en charge dans la version UWP que vous ciblez, vous pouvez vérifier la référence de l' [api .NET standard](/dotnet/api/index?view=netstandard-2.0&preserve-view=true) et sélectionner la version du .NET standard qui est prise en charge par cette version de UWP.

## <a name="scripting-backend-configuration"></a>Configuration du serveur principal de script

La première chose à faire si vous rencontrez des problèmes de génération pour UWP consiste à vérifier les paramètres du **lecteur** (**fichiers > paramètres de Build**, sélectionnez **plateforme Windows universelle**, puis **paramètres du lecteur**). Sous **autres paramètres > la configuration**, les trois premières listes déroulantes (**Scripting Runtime**, **serveur principal de script** et **niveau de compatibilité d’API**) sont tous des paramètres importants à prendre en compte.

La **version du runtime de script** est celle utilisée par le backend de script Unity, qui vous permet d’acquérir la version équivalente (à peu près) de la prise en charge de .NET Framework que vous choisissez. Toutefois, gardez à l’esprit que toutes les API de cette version du .NET Framework ne seront pas prises en charge, mais uniquement celles de la version de .NET Standard que votre UWP cible.

Souvent, avec les nouvelles versions de .NET, d’autres API sont ajoutées à .NET Standard qui peuvent vous permettre d’utiliser le même code sur des plateformes autonomes et UWP. Par exemple, la [System.Runtime.Serialization.Jssur](/dotnet/api/system.runtime.serialization.json) l’espace de noms a été introduite dans .NET standard 2,0. Si vous définissez la **version du runtime de script** sur **.net 3,5 équivalent** (qui cible une version antérieure du .NET standard), vous obtiendrez une erreur lors de la tentative d’utilisation de l’API. Passez à l' **équivalent .net 4,6** (qui prend en charge .NET standard 2,0), et l’API fonctionnera.

Le **serveur principal de script** peut être **.net** ou **IL2CPP**. Pour cette rubrique, nous supposons que vous avez choisi **.net**, car c’est là qu’interviennent les problèmes abordés ici. Pour plus d’informations, consultez les serveurs principaux de [script](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) .

Enfin, vous devez définir le **niveau de compatibilité** de l’API sur la version de .net sur laquelle vous souhaitez que votre jeu s’exécute. Cela doit correspondre à la **version du runtime de script**.

En général, pour la **version du runtime de script** et le niveau de compatibilité de l' **API**, vous devez sélectionner la dernière version disponible afin de bénéficier d’une plus grande compatibilité avec le .NET Framework, et ainsi vous permettre d’utiliser davantage d’API .net.

![Configuration : script de la version du Runtime ; Serveur principal de script ; Niveau de compatibilité de l’API](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Compilation dépendante de la plateforme

Si vous créez votre jeu Unity pour plusieurs plateformes, y compris UWP, vous souhaiterez utiliser la compilation dépendante de la plateforme pour vous assurer que le code destiné à UWP est uniquement exécuté quand le jeu est créé en tant que UWP. De cette façon, vous pouvez utiliser la .NET Framework complète pour les plateformes bureau autonome et autres plateformes, et les API WinRT pour UWP, sans obtenir d’erreurs de génération.

Utilisez les directives suivantes pour compiler uniquement le code lors de l’exécution en tant qu’application UWP :

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` est conçu uniquement pour vérifier si vous compilez le code C# par rapport au serveur principal de script .NET. Si vous utilisez un autre serveur principal de script, tel que IL2CPP, utilisez à la [`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) place.

Pour obtenir la liste complète des directives de compilation dépendant de la plateforme, consultez [compilation dépendant](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)de la plateforme.

## <a name="common-issues-and-workarounds"></a>Problèmes courants et solutions de contournement

Les scénarios suivants décrivent les problèmes courants qui peuvent survenir lorsque les API .NET sont absentes du sous-ensemble UWP et comment les contourner.

### <a name="data-serialization-using-binaryformatter"></a>Sérialisation de données à l’aide de BinaryFormatter

Il est courant pour les jeux de sérialiser les données d’enregistrement afin que les joueurs ne puissent pas facilement les manipuler. Toutefois, [BinaryFormatter](/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), qui sérialise un objet en binaire, n’est pas disponible dans les versions antérieures du .NET standard (avant 2,0). Envisagez plutôt d’utiliser [XmlSerializer](/dotnet/api/system.xml.serialization.xmlserializer) ou [DataContractJsonSerializer](/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) .

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>opérations d'E/S

Certains types de l’espace de noms [System.IO](/dotnet/api/system.io) , tels que [FileStream](/dotnet/api/system.io.filestream), ne sont pas disponibles dans les versions antérieures du .NET standard. Toutefois, Unity fournit les types de [répertoire](/dotnet/api/system.io.directory), de [fichier](/dotnet/api/system.io.file)et **FileStream** , ce qui vous permet de les utiliser dans votre jeu.

Vous pouvez également utiliser les API [Windows. Storage](/uwp/api/Windows.Storage) , qui sont uniquement disponibles pour les applications UWP. Toutefois, ces API limitent l’accès de l’application à son stockage spécifique et ne lui offrent pas un accès gratuit à l’ensemble du système de fichiers. Pour plus d’informations [, consultez fichiers, dossiers et bibliothèques](../files/index.md) .

Il est important de noter que la méthode [Close](/dotnet/api/system.io.stream.close) est uniquement disponible dans .NET standard 2,0 et versions ultérieures (même si Unity fournit une méthode d’extension). Utilisez à la place [dispose](/dotnet/api/system.io.stream.dispose) .

### <a name="threading"></a>Thread

Certains types dans les espaces de noms [System. Threading](/dotnet/api/system.threading) , tels que [ThreadPool](/dotnet/api/system.threading.threadpool), ne sont pas disponibles dans les versions antérieures du .NET standard. Dans ce cas, vous pouvez utiliser le [Windows.SysTEM. ](/uwp/api/windows.system.threading) Espace de noms de thread à la place.

Voici comment vous pouvez gérer les threads dans un jeu Unity, en utilisant la compilation qui dépend de la plateforme pour préparer les plateformes UWP et non UWP :

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>Sécurité

Une partie de la **System. Security.** _ les espaces de noms, tels que [System. Security. Cryptography. X509Certificates](/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0&preserve-view=true), ne sont pas disponibles lorsque vous créez un jeu Unity pour UWP. Dans ce cas, utilisez _*Windows. Security.* *_ Les API, qui couvrent la plupart des fonctionnalités.

L’exemple suivant obtient simplement les certificats d’un magasin de certificats portant le nom donné :

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

Pour plus d’informations sur l’utilisation des API de sécurité WinRT, consultez [sécurité](../security/index.md) .

### <a name="networking"></a>Mise en réseau

Certains des espaces de noms _*System &period; net.* *_ , tels que [System .net. mail](/dotnet/api/system.net.mail?view=netstandard-2.0&preserve-view=true), ne sont pas non plus disponibles lors de la création d’un jeu Unity pour UWP. Pour la plupart de ces API, utilisez le _*Windows. Networking* *_ et le _*Windows. Web correspondants.* *_ API WinRT pour bénéficier d’une fonctionnalité similaire. Pour plus d’informations, consultez [mise en réseau et services Web](../networking/index.md) .

Dans le cas de _ * System .net. mail * *, utilisez l’espace de noms [Windows. ApplicationModel. email](/uwp/api/windows.applicationmodel.email) . Pour plus d’informations, consultez [Envoyer un message électronique](../contacts-and-calendar/sending-email.md) .

## <a name="see-also"></a>Voir aussi

* [Plateforme Windows universelle : types .NET absents sur le serveur principal de script .NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [Vue d’ensemble de .NET pour les applications UWP](/previous-versions/windows/apps/br230302(v=vs.140))
* [Guides de Portage UWP Unity](https://unity3d.com/partners/microsoft/porting-guides)
