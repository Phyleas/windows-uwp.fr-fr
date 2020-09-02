---
title: Communiquer avec un service d’application distant
description: Échangez des messages avec un service d’application exécuté sur un appareil distant à l’aide du projet Rome.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, appareils connectés, systèmes distants, Rome, projet Rome, tâche en arrière-plan, app service
ms.localizationpriority: medium
ms.openlocfilehash: 779205a47b85cf9f9a0aec9db910b97995dc2cd8
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363912"
---
# <a name="communicate-with-a-remote-app-service"></a>Communiquer avec un service d’application distant

En plus de lancer une application sur un appareil distant avec un URI, vous pouvez exécuter des *services d’application* et communiquer avec eux sur des appareils distants. Un appareil Windows peut servir d’appareil client ou d’appareil hôte. Ce qui vous donne un nombre quasiment illimité de modes d’interaction avec les appareils connectés, sans avoir besoin d’amener une application au premier plan.

## <a name="set-up-the-app-service-on-the-host-device"></a>Configurer le service d’application sur l’appareil hôte
Pour exécuter un service d’application sur un appareil distant, un fournisseur de ce service d’application doit être installé sur cet appareil. Ce guide utilise la version CSharp de l' [exemple de service d’application de nombres aléatoires](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices), disponible sur le [référentiel d’exemples Windows universel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Pour obtenir des instructions sur la rédaction du code de votre service d’application, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md).

Que vous utilisiez un service d’application prêt à l’emploi ou créé par vos soins, vous devez lui apporter quelques modifications pour le rendre compatible avec les systèmes distants. Dans Visual Studio, accédez au projet du fournisseur App service (appelé « AppServicesProvider » dans l’exemple) et sélectionnez son fichier _Package. appxmanifest_ . Cliquez sur le bouton droit et sélectionnez **Afficher le code** pour afficher le contenu du fichier. Créez un élément **Extensions** à l’intérieur de l’élément d' **application** principal (ou recherchez-le s’il existe déjà). Créez ensuite une **extension** pour définir le projet comme app service et référencez son projet parent.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

En regard de l’élément **AppService** , ajoutez l’attribut **SupportsRemoteSystems** :

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Pour pouvoir utiliser des éléments de cet espace de noms **uap3** , vous devez ajouter la définition d’espace de noms en haut du fichier manifeste, si ce n’est déjà fait.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Ensuite, générez votre projet de fournisseur App service et déployez-le sur le ou les appareils hôtes.

## <a name="target-the-app-service-from-the-client-device"></a>Cibler le service d’application à partir de l’appareil client
L’appareil à partir duquel le service d’application distant doit être appelé a besoin de l’application avec la fonctionnalité Systèmes distants. Vous pouvez l’ajouter dans l’application qui fournit le service d’application sur l’appareil hôte (auquel cas la même application doit être installée sur les deux appareils) ou dans une autre application.

Les instructions **using** suivantes sont nécessaires pour que le code de cette section s’exécute tel quel :

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetUsings":::


Vous devez d’abord instancier un objet [**AppServiceConnection**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection) , de la même façon que vous appelez un app service localement. Ce processus est décrit plus en détail dans [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Dans cet exemple, le service d’application à cibler est le service Générateur de nombres aléatoires.

> [!NOTE]
> On suppose qu’un objet [RemoteSystem](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) a déjà été acquis par un moyen quelconque dans le code qui appelle la méthode suivante. Pour obtenir des instructions sur ce type de configuration, consultez [Lancer une application sur un appareil distant](launch-a-remote-app.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetAppService":::

Ensuite, un objet [**RemoteSystemConnectionRequest**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) est créé pour l’appareil distant concerné. Il est utilisé pour ouvrir la connexion **AppServiceConnection** à cet appareil. Notez que dans l’exemple ci-dessous, le traitement et le signalement des erreurs sont grandement simplifiés par souci de concision.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetRemoteConnection":::

À ce stade, vous devez avoir une connexion ouverte à un service d’application sur un ordinateur distant.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Échanger des messages avec le service sur la connexion à distance

Maintenant, vous pouvez échanger des messages avec le service sous la forme d’objets [**ValueSet**](/uwp/api/windows.foundation.collections.valueset). Pour plus d’informations, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Le service Générateur de nombres aléatoires prend deux entiers avec les clés `"minvalue"` et `"maxvalue"` comme entrées, sélectionne de manière aléatoire un entier entre ces deux valeurs et le renvoie au processus appelant avec la clé `"Result"`.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

Maintenant que vous êtes connecté à un service d’application sur un appareil hôte ciblé, exécutez une opération sur cet appareil et recevez les données sur votre appareil client.

## <a name="related-topics"></a>Rubriques connexes

[Vue d’ensemble des applications et appareils connectés (Rome du projet)](connected-apps-and-devices.md)  
[Lancer une application sur un appareil distant](launch-a-remote-app.md)  
[Créer et consommer un service d’application](how-to-create-and-consume-an-app-service.md)  
[Référence sur l’API Systèmes distants](/uwp/api/Windows.System.RemoteSystems)  
[Exemple de systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
