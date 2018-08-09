---
author: normesta
Description: Enhance your desktop app for Windows 10 users by using Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Améliorer votre application de bureau pour Windows10
ms.author: normesta
ms.date: 08/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aafe2d09fc27a2693ccf2c4c9d8f189aa0164a3c
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976507"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Améliorer votre application de bureau pour Windows10

Vous pouvez utiliser les API UWP pour ajouter des expériences modernes qui se déclenchent pour les utilisateurs de Windows10.

D'abord, configurez votre projet. Ensuite, ajoutez des expériences Windows10. Vous pouvez créer séparément pour les utilisateurs de Windows10 ou distribuer exactement les mêmes fichiers binaires à tous les utilisateurs, quelle que soit la version de Windows utilisée.

## <a name="first-set-up-your-project"></a>D'abord, configurez votre projet

Vous devrez apporter quelques modifications à votre projet pour utiliser les API UWP.

### <a name="modify-a-net-project-to-use-uwp-apis"></a>Modifier un projet .NET pour utiliser les API UWP

Ouvrez la boîte de dialogue **Gestionnaire de références**, choisissez le bouton **Parcourir**, puis sélectionnez **Tous les fichiers**.

![boîte de dialogue ajouter une référence](images/desktop-to-uwp/browse-references.png)

Ensuite, ajoutez une référence à ces fichiers.

|Fichier|Emplacement|
|--|--|
|System.Runtime.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*version du sdk*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*version du sdk*>\Windows.Foundation.FoundationContract\<*version*>|

Dans la fenêtre **Propriétés**, réglez le champ **Copie locale** de chaque fichier *.winmd* sur **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>Modifier un projet C++ pour utiliser les API UWP

Ouvrez les pages de propriétés de votre projet.

Dans les paramètres **Général** du groupe de paramètres **C/C++**, définissez le champ **Consommer l'extension Windows Runtime** sur la valeur **Oui (/ZW)**.

   ![Consommer l'extension Windows Runtime](images/desktop-to-uwp/consume-runtime-extensions.png)

Ouvrez la boîte de dialogue **Répertoires #using supplémentaires**, puis ajoutez ces répertoires.

* %VSInstallDir%\Common7\IDE\VC\vcpackages
* C:\Program Files (x86)\Windows Kits\10\UnionMetadata
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*dernière version*>
* C:\Program Files (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*dernière version*>

Ouvrez la boîte de dialogue **Autres répertoires Include**, puis ajoutez ce répertoire: C:\Program Files (x86)\Windows Kits\10\Include\<*dernière version*>\um

![Autres répertoires Include](images/desktop-to-uwp/additional-include.png)

Dans les paramètres **Génération de code** du groupe de paramètres **C/C++**, définissez le paramètre **Activation de la régénération minimale** sur la valeur **Non (/GM-)**.

![Activation de la régénération minimale](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows10

Vous êtes maintenant prêt à ajouter des expériences modernes qui se déclenchent lorsque les utilisateurs exécutent votre application sur Windows10. Utilisez ce flux de conception.

:white_check_mark: **D’abord, choisissez les expériences que vous voulez ajouter**

Vous avez le choix parmi une grande variété. Par exemple, vous pouvez simplifier votre flux de bons de commande à l’aide des API de monétisation ou attirer l'attention vers votre application lorsque vous avez quelque chose d’intéressant à partager, par exemple, une nouvelle image publiée par un autre utilisateur.

![Toast](images/desktop-to-uwp/toast.png)

Même si les utilisateurs ignorent ou ferment votre message, ils peuvent le revoir dans le centre de notifications et cliquer sur le message pour ouvrir votre application. Cela rend votre application plus attractive et présente l’avantage supplémentaire de faire paraître votre application profondément intégrée au système d’exploitation. Nous vous présenterons le code de cette expérience un peu plus tard.

Visitez notre [centre de développement](https://developer.microsoft.com/windows) pour trouver de l'inspiration.

:white_check_mark: **Décidez entre améliorer ou étendre**

Vous nous entendrez souvent utiliser les termes «améliorer» et «étendre» donc nous allons prendre quelques instants pour expliquer ce que signifie chacun de ces termes exactement.

Nous utilisons le terme «améliorer» pour décrire les API UWP que vous pouvez appeler directement à partir de votre application de bureau. Une fois que vous avez choisi une expérience Windows10, identifiez les API dont vous avez besoin pour la créer, puis vérifiez si elles figurent dans cette [liste](desktop-to-uwp-supported-api.md). Il s’agit d’une liste des API que vous pouvez appeler directement à partir de votre application de bureau. Si votre API n’apparaît pas dans cette liste, c'est parce que la fonctionnalité associée à cette API ne peut s’exécuter qu'au sein d'un processus UWP. Souvent, il s'agit d'API qui présentent des interfaces utilisateur modernes, comme un contrôle de carte UWP ou une confirmation de sécurité Windows Hello.

Cela dit, si vous souhaitez inclure ces expériences dans votre application, il suffit d'«étendre» l’application en ajoutant un projet UWP à votre solution. Le projet de bureau est toujours le point d’entrée de votre application, mais le projet UWP vous donne accès à toutes les API qui n’apparaissent pas dans cette [liste](desktop-to-uwp-supported-api.md). L’application de bureau peut communiquer avec le processus UWP en utilisant un service d’application et nous offrons de nombreux conseils sur la façon de configurer cette fonctionnalité. Si vous souhaitez ajouter une expérience qui requiert un projet UWP, voir [Étendre avec UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Référencer des contrats API**

Si vous pouvez appeler l’API directement à partir de votre application de bureau, ouvrez un navigateur et recherchez la rubrique de référence de cette API.
Sous le résumé de l’API, vous trouverez une table qui décrit le contrat API pour cette API. Voici un exemple de cette table:

![Table de contrat API](images/desktop-to-uwp/contract-table.png)

Si vous avez une application de bureau .NET, ajoutez une référence à ce contrat API et définissez la propriété **Copie locale** de ce fichier sur la valeur **False**. Si vous avez un projet C++, ajoutez à vos **Autres répertoires Include** un chemin d’accès au dossier qui contient ce contrat.

:white_check_mark: **Appeler les API pour ajouter votre expérience**

Voici le code qui vous permettrait d'afficher la fenêtre de notification que nous avons examinée plus haut. Ces API s’affichent dans cette [liste](desktop-to-uwp-supported-api.md) afin que vous puissiez ajouter ce code à votre application de bureau et l’exécuter immédiatement.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```
Pour en savoir plus sur les notifications, voir [Notifications toast adaptatives et interactives](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Prise en charge des bases d'installation WindowsXP, WindowsVista et Windows7/8

Vous pouvez moderniser votre application pour Windows10sans avoir à créer une nouvelle branche ni à gérer des bases de code distinctes.

Si vous souhaitez créer des fichiers binaires distincts pour les utilisateurs de Windows10, utilisez la compilation conditionnelle. Si vous préférez créer un ensemble de fichiers binaires que vous déployez sur tous les utilisateurs de Windows, utilisez des vérifications à l’exécution.

Jetons un coup d’œil à chaque option.

### <a name="conditional-compilation"></a>Compilation conditionnelle

Vous pouvez conserver une seule base de code et compiler un ensemble de fichiers binaires uniquement pour les utilisateurs de Windows10.

D'abord, ajoutez une nouvelle configuration de build à votre projet.

![Configuration de build](images/desktop-to-uwp/build-config.png)

Pour cette configuration de build, créez une constante pour identifier le code qui appelle des API UWP.  

Pour les projets .NET, la constante s'appelle **Constante de compilation conditionnelle**.

![préprocesseur](images/desktop-to-uwp/compilation-constants.png)

Pour les projets C++, la constante s'appelle **Définition du préprocesseur**.

![préprocesseur](images/desktop-to-uwp/pre-processor.png)

Ajoutez cette constante avant un bloc de code UWP.

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

Le compilateur génère ce code uniquement si cette constante est définie dans votre configuration de build active.

### <a name="runtime-checks"></a>Vérifications à l’exécution

Vous pouvez compiler un ensemble de fichiers binaires pour l’ensemble de vos utilisateurs Windows, quelle que soit la version de Windows exécutée. Votre application appelle des API UWP uniquement si l’utilisateur exécute votre application en tant qu'application empaquetée sur Windows10.

Le moyen le plus simple pour ajouter à votre code des vérifications à l’exécution consiste à installer ce package Nuget: [Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/), puis à utiliser la méthode ``IsRunningAsUWP()`` pour désactiver tout le code UWP. consultez ce billet de blog pour plus d’informations: [Pont du bureau: identifier le contexte de l’application](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vidéo associée

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Exemples connexes

* [Exemple Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d'API Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemples de passerelle d’applications de bureau vers UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).