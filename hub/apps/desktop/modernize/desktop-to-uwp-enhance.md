---
Description: Améliorez votre application de bureau pour les utilisateurs de Windows 10 avec les API de la plateforme Windows universelle (UWP).
title: Utiliser des API UWP dans des applications de bureau
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 78d9760c5ef21b29d09babaace0f4379b6a51209
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75302603"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Appeler des API UWP dans des applications de bureau

Vous pouvez utiliser des API de la plateforme Windows universelle (UWP) pour ajouter à vos applications de bureau des expériences modernes qui s’activent pour utilisateurs de Windows 10.

Commencez par configurer votre projet avec les références requises. Appelez ensuite les API UWP à partir de votre code pour ajouter des expériences Windows 10 à votre application de bureau. Vous pouvez générer séparément pour les utilisateurs de Windows 10 ou distribuer les mêmes fichiers binaires à tous les utilisateurs, quelle que soit la version de Windows utilisée.

Certaines API UWP sont prises en charge uniquement dans des applications de bureau qui ont l’[identité de package](modernize-packaged-apps.md). Pour plus d’informations, consultez [API UWP disponibles](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurer votre projet

Pour utiliser des API UWP, vous devez apporter quelques modifications à votre projet.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modifier un projet .NET pour utiliser des API d’exécution Windows

Il existe deux options pour les projets .NET :

* Si votre application cible Windows 10 version 1803 ou ultérieure, vous pouvez installer un package NuGet qui fournit toutes les références nécessaires.
* Vous pouvez également ajouter les références manuellement.

#### <a name="to-use-the-nuget-option"></a>Pour utiliser l’option NuGet

1. Assurez-vous que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils -> Gestionnaire de package NuGet-> Paramètres du Gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour **Format de gestion de package par défaut**.

2. Votre projet étant ouvert dans Visual Studio, cliquez dessus avec le bouton droit dans l’**Explorateur de solutions**, puis sélectionnez **Gérer les packages NuGet**.

3. Dans la fenêtre **Gestionnaire de packages NuGet**, sélectionnez l’onglet **Parcourir**, puis recherchez `Microsoft.Windows.SDK.Contracts`.

4. Une fois le package de `Microsoft.Windows.SDK.Contracts` trouvé, dans le volet droit de la fenêtre **Gestionnaire de packages NuGet**, sélectionnez la **Version** du package à installer en fonction de la version de Windows 10 que vous souhaitez cibler :

    * **10.0.18362.xxxx** : choisissez cette version pour Windows 10, version 1903.
    * **10.0.17763.xxxx** : choisissez cette version pour Windows 10, version 1809.
    * **10.0.17134.xxxx**: choisissez cette version pour Windows 10, version 1803.

5. Cliquez sur **Installer**.

#### <a name="to-add-the-required-references-manually"></a>Pour ajouter les références requises manuellement

1. Ouvrez la boîte de dialogue **Gestionnaire de références**, choisissez le bouton **Parcourir**, puis sélectionnez **Tous les fichiers**.

    ![boîte de dialogue ajouter une référence](images/desktop-to-uwp/browse-references.png)

2. Ajoutez une référence à ces fichiers.

    |File|Emplacement|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. Dans la fenêtre **Propriétés**, définissez le champ **Copie locale** de chaque fichier *.winmd* sur **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modifier un C++ projet Win32 pour utiliser des API d’exécution Windows

Utilisez [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) pour utiliser des d’exécution Windows. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Pour configurer votre projet pour C++/WinRT :

* Pour de nouveaux projets, vous pouvez installer l’[Extension Visual Studio (VSIX) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un des modèles de projet C++/WinRT inclus dans cette extension.
* Pour des projets existants, vous pouvez installer le package NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans le projet.

Pour plus d’informations sur ces options, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows 10

Vous êtes maintenant prêt à ajouter des expériences modernes qui s’activent quand des utilisateurs exécutent votre application sur Windows 10. Utilisez ce flux de conception.

:white_check_mark: **commencez pas choisir les expériences que vous voulez ajouter**

Le est vaste. Par exemple, vous pouvez simplifier votre flux de bons de commande à l’aide d’[API de monétisation](/windows/uwp/monetize) ou [attirer l’attention sur votre application](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) lorsque vous avez quelque chose d’intéressant à partager, par exemple, une nouvelle image publiée par un autre utilisateur.

![Toast](images/desktop-to-uwp/toast.png)

Même si les utilisateurs ignorent ou ferment votre message, ils peuvent le revoir dans le centre de notifications et cliquer dessus pour ouvrir votre application. Cela rend votre application plus attractive et présente l’avantage supplémentaire de la faire paraître profondément intégrée avec le système d’exploitation. Nous vous présentons le code de cette expérience un peu plus loin dans cet article.

Pour d’autres idées, consultez la [Documentation UWP](/windows/uwp/get-started/).

:white_check_mark: **Décidez entre améliorer ou étendre**

Vous nous entendrez souvent utiliser les termes *améliorer* et *étendre*. Nous allons donc prendre quelques instants pour expliquer ce qu’ils signifient exactement.

Nous utilisons le terme *améliorer* pour décrire les API d’exécution Windows que vous pouvez appeler directement à partir de votre application de bureau (que vous ayez ou non choisi d’empaqueter celle-ci dans un package MSIX). Une fois que vous avez choisi une expérience Windows 10, identifiez les API dont vous avez besoin pour la créer, puis vérifiez si elles figurent dans [cette liste](desktop-to-uwp-supported-api.md). Il s’agit d’une liste d’API que vous pouvez appeler directement à partir de votre application de bureau. Si votre API ne figure pas dans cette liste, c’est parce que la fonctionnalité qui y est associée ne peut s’exécuter qu’à l’intérieur d’un processus UWP. Souvent, il s’agit d’API qui affichent du code XAML UWP, comme un contrôle de carte UWP ou une invite de sécurité Windows Hello.

> [!NOTE]
> Si les API qui affichent du code XAML UWP ne peuvent généralement pas être appelées directement à partir du bureau, vous pouvez peut-être adopter d’autres approches. Si vous souhaitez héberger des contrôles XAML UWP ou d’autres expériences visuelles personnalisées, vous pouvez utiliser des [îlots XAML](xaml-islands.md) (à partir de Windows 10, version 1903) et la [couche visuelle](visual-layer-in-desktop-apps.md) (à partir de Windows 10, version 1803). Ces fonctionnalités sont utilisables dans des applications de bureau empaquetées ou non.

Si vous avez choisi d’empaqueter votre application de bureau dans un package MSIX, une autre option consiste à *étendre* l’application en ajoutant un projet UWP à votre solution. Le projet de bureau est toujours le point d’entrée de votre application, mais le projet UWP vous donne accès à toutes les API qui n’apparaissent pas dans cette [liste](desktop-to-uwp-supported-api.md). L’application de bureau peut communiquer avec le processus UWP en utilisant un service d’application, et nous offrons de nombreux conseils sur la façon de configurer cette fonctionnalité. Si vous souhaitez ajouter une expérience qui requiert un projet UWP, voir [Étendre à l’aide de composants UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Contrats API de référence**

Si vous pouvez appeler l’API directement à partir de votre application de bureau, ouvrez un navigateur et recherchez la rubrique de référence de cette API.
Sous le résumé de l’API, vous trouverez une table décrivant le contrat API pour cette API. Voici un exemple d’une telle table :

![Table de contrat API](images/desktop-to-uwp/contract-table.png)

Si vous avez une application de bureau .NET, ajoutez une référence à ce contrat API et définissez la propriété **Copie locale** de ce fichier sur la valeur **False**. Si vous avez un projet C++, ajoutez à vos **Autres répertoires Include** un chemin d’accès au dossier qui contient ce contrat.

:white_check_mark: **Appeler les API pour ajouter votre expérience**

Voici le code qui vous permettrait d’afficher la fenêtre de notification que nous avons examinée plus haut. Ces API s’affichent dans cette [liste](desktop-to-uwp-supported-api.md) afin que vous puissiez ajouter ce code à votre application de bureau et l’exécuter immédiatement.

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

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Prise en charge des bases d’installation Windows XP, Windows Vista et Windows 7/8

Vous pouvez moderniser votre application pour Windows 10 sans avoir à créer une nouvelle branche ni à gérer des bases de code distinctes.

Si vous souhaitez créer des fichiers binaires distincts pour les utilisateurs de Windows 10, utilisez la compilation conditionnelle. Si vous préférez créer un ensemble de fichiers binaires que vous déployez sur tous les utilisateurs de Windows, utilisez des vérifications à l’exécution.

Jetons un coup d’œil à chaque option.

### <a name="conditional-compilation"></a>Compilation conditionnelle

Vous pouvez conserver une seule base de code et compiler un ensemble de fichiers binaires uniquement pour les utilisateurs de Windows 10.

D’abord, ajoutez une nouvelle configuration de build à votre projet.

![Configuration de build](images/desktop-to-uwp/build-config.png)

Pour cette configuration de build, créez une constante pour identifier le code qui appelle des API d’exécution Windows.  

Pour les projets .NET, la constante s’appelle **Constante de compilation conditionnelle**.

![préprocesseur](images/desktop-to-uwp/compilation-constants.png)

Pour les projets C++, la constante s’appelle **Définition du préprocesseur**.

![préprocesseur](images/desktop-to-uwp/pre-processor.png)

Ajoutez cette constante devant un bloc de code UWP.

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

Vous pouvez compiler un ensemble de fichiers binaires pour l’ensemble de vos utilisateurs Windows, quelle que soit la version de Windows exécutée. Votre application appelle des API d’exécution Windows uniquement si l’utilisateur l’exécute en tant qu’application empaquetée sur Windows 10.

Le moyen le plus simple d’ajouter à votre code des vérifications à l’exécution consiste à installer le package NuGet [Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/), puis à utiliser la méthode ``IsRunningAsUWP()`` pour désactiver tout le code qui appelle les API d’exécution Windows. Pour plus de détails, consultez ce billet de blog : [Pont du bureau : identifier le contexte de l’application](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Exemples connexes

* [Exemple Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d’API Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemples de passerelle d’applications de bureau vers UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Trouvez des réponses à vos questions

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
