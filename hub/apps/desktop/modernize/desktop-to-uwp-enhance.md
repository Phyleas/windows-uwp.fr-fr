---
description: Améliorez votre application de bureau pour les utilisateurs de Windows 10 avec les API Windows Runtime.
title: Appeler des API Windows Runtime dans les applications de bureau
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2b0d6bb305490e05c2670f0e0a326601c51a8373
ms.sourcegitcommit: 609441402c17d92e7bfac83a6056909bb235223c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2020
ms.locfileid: "90837814"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>Appeler des API Windows Runtime dans les applications de bureau

Vous pouvez utiliser des API de la plateforme Windows universelle (UWP) pour ajouter à vos applications de bureau des expériences modernes qui s’activent pour utilisateurs de Windows 10.

Commencez par configurer votre projet avec les références requises. Appelez ensuite les API Windows Runtime à partir de votre code pour ajouter des expériences Windows 10 à votre application de bureau. Vous pouvez générer séparément pour les utilisateurs de Windows 10 ou distribuer les mêmes fichiers binaires à tous les utilisateurs, quelle que soit la version de Windows utilisée.

Certaines API Windows Runtime sont prises en charge uniquement dans des applications de bureau qui ont l’[identité de package](modernize-packaged-apps.md). Pour plus d’informations, consultez [API Windows Runtime disponibles](desktop-to-uwp-supported-api.md).

## <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modifier un projet .NET pour utiliser des API d’exécution Windows

Il existe plusieurs options pour les projets .NET :

* À partir de .NET 5 Preview 8, vous pouvez ajouter un moniker de framework cible à votre fichier de projet pour accéder aux API WinRT. Cette option est prise en charge dans les projets qui ciblent Windows 10 version 1809 ou ultérieure.
* Pour les versions précédente antérieures de .NET, vous pouvez installer le package NuGet `Microsoft.Windows.SDK.Contracts` pour ajouter toutes les références nécessaires à votre projet. Cette option est prise en charge dans les projets qui ciblent Windows 10 version 1803 ou ultérieure.
* Si votre projet cible à la fois .NET 5 Preview 8 (ou ultérieures) et les versions antérieures de .NET, vous pouvez configurer le fichier de projet de sorte qu’il utilise les deux options.

### <a name="net-5-preview-8-and-later-use-the-target-framework-moniker-option"></a>Versions .NET 5 Preview 8 et ultérieures : utiliser l’option moniker de framework cible 

Cette option est uniquement prise en charge dans les projets qui utilisent .NET 5 Preview 8 (ou une version antérieure) et ciblent Windows 10 version 1809 ou une version de système d’exploitation ultérieure. Pour plus d’informations sur ce scénario, consultez [ce billet de blog](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5/).

1. Votre projet étant ouvert dans Visual Studio, cliquez dessus avec le bouton droit dans l’**Explorateur de solutions**, puis sélectionnez **Modifier le fichier de projet**. Votre fichier de projet doit se présenter comme suit.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. Remplacez la valeur de l’élément **TargetFramework** par l’une des chaînes suivantes :

    * **net5.0-windows10.0.17763.0** : utilisez cette valeur si votre application cible Windows 10 version 1809.
    * **net5.0-windows10.0.18362.0** : utilisez cette valeur si votre application cible Windows 10 version 1903.
    * **net5.0-windows10.0.19041.0** : utilisez cette valeur si votre application cible Windows 10 version 2004.

    Par exemple, l’élément suivant concerne un projet qui cible Windows 10 version 2004.

    ```csharp
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    ```

3. Enregistrez vos modifications et fermez le fichier de projet.

### <a name="earlier-versions-of-net-install-the-microsoftwindowssdkcontracts-nuget-package"></a>Versions antérieures de .NET : installer le package NuGet Microsoft.Windows.SDK.Contracts

Utilisez cette option si votre application utilise .NET Core 3.x, .NET 5 Preview 7 (ou version antérieure) ou le .NET Framework. Cette option est prise en charge dans les projets qui ciblent Windows 10 version 1803 ou une version de système d’exploitation ultérieure.

1. Assurez-vous que les [références de package](/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils -> Gestionnaire de package NuGet-> Paramètres du Gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour **Format de gestion de package par défaut**.

2. Votre projet étant ouvert dans Visual Studio, cliquez dessus avec le bouton droit dans l’**Explorateur de solutions**, puis sélectionnez **Gérer les packages NuGet**.

3. Dans la fenêtre **Gestionnaire de packages NuGet**, sélectionnez l’onglet **Parcourir**, puis recherchez `Microsoft.Windows.SDK.Contracts`.

4. Une fois le package de `Microsoft.Windows.SDK.Contracts` trouvé, dans le volet droit de la fenêtre **Gestionnaire de packages NuGet**, sélectionnez la **Version** du package à installer en fonction de la version de Windows 10 que vous souhaitez cibler :

    * **10.0.19041.xxxx** : choisissez cette version pour Windows 10 version 2004.
    * **10.0.18362.xxxx** : choisissez cette version pour Windows 10, version 1903.
    * **10.0.17763.xxxx** : choisissez cette version pour Windows 10, version 1809.
    * **10.0.17134.xxxx**: choisissez cette version pour Windows 10, version 1803.

5. Cliquez sur **Installer**.

### <a name="configure-projects-that-multi-target-different-versions-of-net"></a>Configurer des projets qui ciblent plusieurs versions différentes de .NET

Si votre projet cible à la fois .NET 5 Preview 8 (ou une version ultérieure) et des versions antérieures (y compris .NET Core 3.x et le .NET Framework), vous pouvez configurer le fichier de projet de façon à utiliser le moniker de framework cible pour extraire automatiquement les références d’API WinRT pour .NET 5 Preview 8 (ou versions ultérieures) et utiliser le package NuGet `Microsoft.Windows.SDK.Contracts` pour les versions antérieures.

1. Votre projet étant ouvert dans Visual Studio, cliquez dessus avec le bouton droit dans l’**Explorateur de solutions**, puis sélectionnez **Modifier le fichier de projet**. L’exemple suivant montre un fichier de projet pour une application qui utilise .NET Core 3.1.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. Remplacez l’élément **TargetFramework** du fichier par un élément **TargetFrameworks** (au pluriel). Dans cet élément, spécifiez les monikers de framework cibles pour toutes les versions de .NET que vous ciblez en les séparant par des points virgules. 

    * Pour .NET 5 Preview 8 ou version ultérieure, utilisez l’un des monikers de framework cibles suivants :
        * **net5.0-windows10.0.17763.0** : utilisez cette valeur si votre application cible Windows 10 version 1809.
        * **net5.0-windows10.0.18362.0** : utilisez cette valeur si votre application cible Windows 10 version 1903.
        * **net5.0-windows10.0.19041.0** : utilisez cette valeur si votre application cible Windows 10 version 2004.
    * Pour .NET Core 3.x, utilisez **netcoreapp3.0** ou **netcoreapp3.1**.
    * Pour le .NET Framework, utilisez **net46**.

    L’exemple suivant montre comment cibler à la fois .NET Core 3.1 et .NET 5 Preview 8 (pour Windows 10 version 2004).

    ```csharp
    <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
    ```

3. Après l’élément **PropertyGroup**, ajoutez l’élément **PackageReference**, qui inclut une instruction conditionnelle qui installe le package NuGet `Microsoft.Windows.SDK.Contracts` pour toutes les versions de .NET Core 3.x ou le .NET Framework ciblé par votre application. L’élément **PackageReference** doit être un enfant de l’élément **ItemGroup**. L’exemple suivant montre comment procéder pour .NET Core 3.1.

    ```csharp
    <ItemGroup>
      <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                        Include="Microsoft.Windows.SDK.Contracts"
                        Version="10.0.19041.0" />
    </ItemGroup>
    ```

    Lorsque vous avez terminé, votre fichier de projet doit se présenter comme suit.

    ```csharp
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
        <UseWPF>true</UseWPF>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                         Include="Microsoft.Windows.SDK.Contracts"
                         Version="10.0.19041.0" />
      </ItemGroup>
    </Project>
    ```

4. Enregistrez vos modifications et fermez le fichier de projet.

## <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modifier un C++ projet Win32 pour utiliser des API d’exécution Windows

Utilisez [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) pour utiliser des d’exécution Windows. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Pour configurer votre projet pour C++/WinRT :

* Pour de nouveaux projets, vous pouvez installer l’[Extension Visual Studio (VSIX) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un des modèles de projet C++/WinRT inclus dans cette extension.
* Pour des projets existants, vous pouvez installer le package NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans le projet.

Pour plus d’informations sur ces options, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Ajouter des expériences Windows 10

Vous êtes maintenant prêt à ajouter des expériences modernes qui s’activent quand des utilisateurs exécutent votre application sur Windows 10. Utilisez ce flux de conception.

:white_check_mark: **commencez pas choisir les expériences que vous voulez ajouter**

Le est vaste. Par exemple, vous pouvez simplifier votre flux de bons de commande à l’aide d’[API de monétisation](/windows/uwp/monetize) ou [attirer l’attention sur votre application](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) lorsque vous avez quelque chose d’intéressant à partager, par exemple, une nouvelle image publiée par un autre utilisateur.

![Notification toast](images/desktop-to-uwp/toast.png)

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

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
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

Pour en savoir plus sur les notifications, voir [Notifications toast adaptatives et interactives](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

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

![Constante de compilation conditionnelle](images/desktop-to-uwp/compilation-constants.png)

Pour les projets C++, la constante s’appelle **Définition du préprocesseur**.

![Constante de définition de préprocesseur](images/desktop-to-uwp/pre-processor.png)

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

Le moyen le plus simple d’ajouter à votre code des vérifications à l’exécution consiste à installer le package NuGet [Desktop Bridge Helpers](https://www.nuget.org/packages/DesktopBridge.Helpers/), puis à utiliser la méthode ``IsRunningAsUWP()`` pour désactiver tout le code qui appelle les API d’exécution Windows. Pour plus de détails, consultez ce billet de blog : [Pont du bureau : identifier le contexte de l’application](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context).

## <a name="related-samples"></a>Exemples connexes

* [Exemple Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Vignette secondaire](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemple d’API Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Application WinForms qui implémente un UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemples de passerelle d’applications de bureau vers UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Trouvez des réponses à vos questions

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).