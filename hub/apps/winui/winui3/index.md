---
title: WinUI 3 Preview 3 (novembre 2020)
description: Vue d’ensemble de la version WinUI 3 Preview 3.
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: 69855aea647b608d9253e4f71b6d7d38917def61
ms.sourcegitcommit: a4ca8ba143862411cd1104515cfeb98f1bcdb780
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857420"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Bibliothèque d’interface utilisateur Windows 3 Preview 3 (novembre 2020)

La bibliothèque d’interface utilisateur Windows (WinUI) 3 est un framework d’expérience utilisateur natif pour les applications de bureau Windows et UWP.

**WinUI 3 Preview 3** offre des fonctionnalités nouvelles et améliorées ainsi que des correctifs de bogues importants.

**Consultez [Limitations et problèmes connus de la version Preview 3](#preview-3-limitations-and-known-issues)** .

> [!Important]
> Cette préversion de WinUI 3 est destinée à une évaluation anticipée et à la collecte de commentaires de la communauté des développeurs. Il ne doit **PAS** être utilisé pour les applications de production.
>
> Nous continuerons de délivrer des préversions de WinUI 3 en 2021, qui seront suivies de la première version officielle.
>
> Utilisez le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml) pour fournir des commentaires et journaliser des suggestions et des problèmes.

## <a name="install-winui-3-preview-3"></a>Installer WinUI 3 Preview 3

WinUI 3 Preview 3 inclut des modèles de projet Visual Studio qui vous permettent de générer des applications avec une interface utilisateur basée sur WinUI, et un package NuGet qui contient les bibliothèques WinUI. Pour installer WinUI 3 Preview 3, effectuez ces étapes.

> [!NOTE]
> Vous pouvez également cloner et générer la version WinUI 3 Preview 3 de [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-3-branch).

1. Vérifiez que votre ordinateur de développement dispose de Windows 10 version 1803 (build 17134) ou d’une version plus récente.

2. Installez [Visual Studio 2019, version 16.9 Preview](https://visualstudio.microsoft.com/vs/preview/)

    Vous devez inclure les charges de travail suivantes lors de l’installation de Visual Studio :
    - Développement .NET Desktop (installe également .NET 5)
    - Développement pour la plateforme Windows universelle

    Pour générer des applications C++, vous devez également inclure les charges de travail suivantes :
    - Développement Desktop en C++
    - Le composant facultatif *Outils de plateforme Windows universelle C++ (v142)* pour la charge de travail Plateforme Windows universelle (voir « Détails de l’installation » sous la section « Développement pour plateforme Windows universelle », dans le volet de droite)
3. Assurez-vous que votre système a une source de package NuGet activée pour **nuget.org**. Pour plus d’informations, consultez [Configurations NuGet courantes](/nuget/consume-packages/configuring-nuget-behavior).

4. Téléchargez et installez le [package VSIX WinUI 3 Preview 3](https://aka.ms/winui3/preview3-download). Ceci ajoute à la fois les modèles de projet WinUI 3 et le package NuGet contenant les bibliothèques WinUI 3 à Visual Studio 2019.

    Pour obtenir des instructions sur la façon d’ajouter le package VSIX à Visual Studio, consultez [Recherche et utilisation des extensions Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).

#### <a name="webview2"></a>Vue web 2

Si vous utilisez le contrôle WebView2 dans votre application, installez la **version Canal Dev du navigateur Microsoft Edge** à partir de [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download). Veillez à désinstaller toutes les instances existantes de Microsoft Edge Beta, de Microsoft Edge Dev et de Microsoft Edge WebView2 Runtime.

#### <a name="windows-community-toolkit"></a>Windows Community Toolkit

Si vous utilisez le kit de ressources Communauté Windows, [téléchargez la dernière version](https://aka.ms/wct-winui3).

## <a name="create-winui-projects"></a>Créer des projets WinUI

Après l’installation du package VSIX WinUI 3 Preview 3, vous êtes prêt à créer un projet en utilisant un des modèles de projet WinUI dans Visual Studio. Pour accéder aux modèles de projet WinUI dans la boîte de dialogue **Créer un projet**, filtrez le langage sur **C++** ou **C#** , la plateforme sur **Windows** et le type de projet sur **WinUI**. Vous pouvez également rechercher *WinUI*, puis sélectionner l’un des modèles C# ou C++ disponibles.

![Modèles de projet WinUI](images/winui-projects-csharp.png)

Pour plus d’informations sur la façon de commencer avec les modèles de projet WinUI, consultez les articles suivants :

- [Bien démarrer avec WinUI 3 pour les applications de bureau](get-started-winui3-for-desktop.md)
- [Bien démarrer avec WinUI 3 pour les applications UWP](get-started-winui3-for-uwp.md)

En plus des [limitations et les problèmes connus](#preview-3-limitations-and-known-issues), la génération d’une application à l’aide des projets WinUI est semblable à la génération d’une application UWP avec XAML et WinUI 2.x. Par conséquent, la majeure partie de la [documentation d’aide](/windows/uwp/design/) pour les applications UWP et les espaces de noms WinRT **Windows.UI** figurant dans le SDK Windows est applicable.

Avec cette version, nous avons également ajouté une [documentation de référence de l’API WinUI 3](/windows/winui/api/) pour toutes les API WinRT portées sur WinUI 3.

Si vous avez créé un projet en utilisant WinUI 3 Preview 2, vous pouvez mettre à niveau votre projet pour qu’il utilise Preview 3. Pour obtenir des instructions détaillées, consultez le [dépôt GitHub WinUI](https://aka.ms/winui3/upgrade-instructions).

### <a name="project-templates-for-winui-3"></a>Modèles de projet pour WinUI 3

Vous pouvez utiliser ces modèles de projet WinUI pour créer des applications.

| Modèle | Language | Description |
|----------|----------|-------------|
| Blank App, Packaged (WinUI in Desktop) | C# et C++ | Crée une application .NET 5 de bureau (C#) ou Win32 native (C++) avec une interface utilisateur basée sur WinUI. Le projet généré comprend une fenêtre de base qui dérive de la classe **Microsoft.UI.Xaml.Window** de la bibliothèque WinUI que vous pouvez utiliser pour commencer à générer votre interface utilisateur. Pour plus d’informations sur ce type de projet, consultez [Bien démarrer avec WinUI 3 pour les applications de bureau](get-started-winui3-for-desktop.md).<p></p>La solution inclut également un [projet de création de packages d’applications Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) qui est configuré pour générer l’application dans un [package MSIX](/windows/msix/overview). Vous bénéficiez ainsi d’une expérience de déploiement moderne, de la possibilité de l’intégrer aux fonctionnalités de Windows 10 par le biais d’extensions de package, et bien plus encore.  |
| Application vide (WinUI dans UWP)  | C# et C++ | Crée une application UWP avec une interface utilisateur basée sur WinUI. Le projet généré comprend une page de base qui dérive de la classe **Microsoft.UI.Xaml.Controls.Page** de la bibliothèque WinUI que vous pouvez utiliser pour commencer à générer votre interface utilisateur. Pour plus d’informations sur ce type de projet, consultez [Bien démarrer avec WinUI 3 pour les applications UWP](get-started-winui3-for-uwp.md). |

Vous pouvez utiliser ces modèles de projet WinUI pour générer des composants qui peuvent être chargés et utilisés par une application basée sur WinUI.

| Modèle | Language | Description |
|----------|----------|-------------|
| Class Library (WinUI in Desktop) | C# uniquement | Crée une bibliothèque de classes managées (DLL) .NET 5 en C# qui peut être utilisée par d’autres applications de bureau .NET 5 avec une interface utilisateur basée sur WinUI.  |
| Bibliothèque de classes (WinUI dans UWP)  | C# uniquement | Crée une bibliothèque de classes managées (DLL) en C# qui peut être utilisée par d’autres applications UWP avec une interface utilisateur basée sur WinUI. |
| Composant Windows Runtime (WinUI) | C++ | Crée un [composant Windows Runtime](/windows/uwp/winrt-components/) écrit en C++/WinRT qui peut être consommé par n’importe quelle application UWP ou de bureau avec une interface utilisateur basée sur WinUI, quel que soit le langage de programmation dans lequel l’application est écrite. |
| Composant Windows Runtime (UWP) | C# | Crée un [composant Windows Runtime](/windows/uwp/winrt-components/) écrit en C# qui peut être consommé par n’importe quelle application UWP avec une interface utilisateur basée sur WinUI, quel que soit le langage de programmation dans lequel l’application est écrite. |

### <a name="item-templates-for-winui-3"></a>Modèles d’élément pour WinUI 3

Les modèles d’élément suivants sont disponibles pour être utilisés dans un projet WinUI. Pour accéder à ces modèles d’élément WinUI, cliquez avec le bouton droit sur le nœud du projet dans l’**Explorateur de solutions**, sélectionnez **Ajouter** -> **Nouvel élément**, puis cliquez sur **WinUI** dans la boîte de dialogue **Ajouter un nouvel élément**.

![Modèles d’élément WinUI](images/winui-items-csharp.png)

| Modèle | Language | Description |
|----------|----------|-------------|
| Page vierge (WinUI) | C# et C++ | Ajoute un fichier XAML et un fichier de code qui définit une nouvelle page dérivée de la classe **Microsoft.UI.Xaml.Controls.Page** de la bibliothèque WinUI. |
| Fenêtre vide (WinUI dans les applications de bureau) | C# et C++ | Ajoute un fichier XAML et un fichier de code qui définit une nouvelle fenêtre dérivée de la classe **Microsoft.UI.Xaml.Window** de la bibliothèque WinUI. |
| Contrôle personnalisé (WinUI) | C# et C++ | Ajoute un fichier de code pour créer un contrôle basé sur un modèle avec un style par défaut. Le contrôle basé sur un modèle est dérivé de la classe **Microsoft.UI.Xaml.Controls.Control** de la bibliothèque WinUI.<p></p>Pour obtenir une procédure pas à pas qui montre comment utiliser ce modèle d’élément, consultez [Contrôles XAML basés sur un modèle pour les applications UWP et WinUI 3 avec C++/WinRT](xaml-templated-controls-cppwinrt-winui-3.md) et [Contrôles XAML basés sur un modèle pour les applications WinUI 3 avec C#](xaml-templated-controls-csharp-winui-3.md). Pour plus d’informations sur les contrôles basés sur des modèles, consultez [Contrôles XAML personnalisés](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Dictionnaire de ressources (WinUI) | C# et C++ | Ajouter une collection à clé et vide de ressources XAML. Pour plus d’informations, consultez [Informations de référence sur les ressources ResourceDictionary et XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Fichier de ressources (WinUI) | C# et C++ | Ajoute un fichier dans lequel stocker les ressources de type chaîne et les ressources conditionnelles de votre application. Vous pouvez utiliser cet élément pour faciliter la localisation de votre application. Pour plus d’informations, consultez [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Contrôle utilisateur (WinUI) | C# et C++ | Ajoute un fichier XAML et un fichier de code pour la création d’un contrôle utilisateur dérivé de la classe **Microsoft.UI.Xaml.Controls.UserControl** de la bibliothèque WinUI. En général, un contrôle utilisateur encapsule des contrôles existants associés et fournit sa propre logique.<p></p>Pour plus d’informations sur les contrôles utilisateur, consultez [Contrôles XAML personnalisés](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

### <a name="visual-studio-support"></a>Support Visual Studio

Pour tirer parti des dernières fonctionnalités des outils ajoutées à WinUI 3 Preview 3, comme le rechargement à chaud, l’arborescence d’éléments visuels dynamique et l’Explorateur de propriétés dynamique, vous devez utiliser la dernière préversion de Visual Studio avec la dernière préversion de WinUI 3. Le tableau ci-dessous montre la compatibilité des versions futures avec WinUI 3 Preview 3 :

| Version de Visual Studio  | WinUI 3 Preview 3  |
|---|---|
| 16.8 RTM  | Non   |
| Préversions 16.9  | Oui  | 
| 16.9 RTM  | Non   |
| Préversions 16.10  | Oui   |


## <a name="new-features-and-capabilities-in-preview-3"></a>Nouvelles fonctionnalités dans Preview 3

- Prise en charge d’ARM64
- Glisser-déplacer à l’intérieur et à l’extérieur des applications
- RenderTargetBitmap (actuellement seulement le contenu XAML - pas le contenu SwapChainPanel)
- Améliorations apportées à notre expérience des outils/du développement :
  - Arborescence d’éléments visuels dynamique, rechargement à chaud, Explorateur de propriétés dynamique et outils similaires
  - IntelliSense fonctionne maintenant pour WinUI 3 
- Prise en charge de MRT Core
  - Ceci rend les applications plus rapides et plus légères au démarrage, et permet une recherche plus rapide des ressources.
- Prise en charge des curseurs personnalisés
- Entrée hors thread
- Améliorations des performances depuis la version Preview 2
- Fenêtres multiples dans les applications de bureau - prise en charge améliorée depuis la version Preview 2


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>Nouvelles fonctionnalités introduites dans les préversions passées de WinUI 3

Les fonctionnalités suivantes ont été introduites dans WinUI 3 Preview 1 & 2 et continuent d’être prises en charge dans WinUI 3 Preview 3.

- Possibilité de créer des applications de bureau avec WinUI, y compris [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) pour les applications Win32
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [Mises à jour de TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- Mises à jour de thèmes sombres
- Améliorations et mises à jour de [WebView2](/microsoft-edge/hosting/webview2)
  - Prise en charge des résolutions élevées
  - Prise en charge du redimensionnement et du déplacement de fenêtre
  - Mise à jour pour cibler une version plus récente de Microsoft Edge
  - Vous n’avez plus besoin de référencer un package NuGet propre à WebView2
- SwapChainPanel
- Améliorations requises pour la migration open source

Pour plus d’informations sur les avantages de WinUI 3 et sur la feuille de route de WinUI, consultez la [feuille de route de la bibliothèque d’interface utilisateur Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) sur GitHub.

### <a name="provide-feedback-and-suggestions"></a>Fournir des commentaires et des suggestions

Nous serions heureux de recevoir vos commentaires dans le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="whats-coming-next"></a>Quelles sont les étapes suivantes ?

Consultez notre [feuille de route détaillée des fonctionnalités](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap) pour voir quand des fonctionnalités spécifiques seront intégrées à WinUI 3. 

## <a name="preview-3-limitations-and-known-issues"></a>Limitations et problèmes connus de Preview 3

La version Preview 3 n’est qu’une préversion. En particulier, les scénarios concernant les applications de bureau sont nouveaux. Attendez-vous à des bogues, des limitations et d’autres problèmes.

Voici quelques-uns des problèmes connus avec WinUI 3 Preview 3. Si vous trouvez un problème qui ne figure pas dans la liste ci-dessous, faites-le nous savoir en contribuant à un problème existant ou en consignant un nouveau problème via le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Prise en charge de plateforme et de système d’exploitation

WinUI 3 Preview 3 est compatible avec les PC qui exécutent la mise à jour d’avril 2018 de Windows 10 (version 1803 - build 17134) et les versions ultérieures.

### <a name="developer-tools"></a>Outils de développeur

- Seules les applications C# et C++/WinRT sont prises en charge
- Les applications de bureau prennent en charge .NET 5 et C# 9, et elles doivent être empaquetées dans une application MSIX
- Les applications UWP prennent en charge .NET Native et C# 7.3
- Les outils de développement et IntelliSense peuvent ne pas fonctionner correctement dans Visual Studio version 16.8.
- Aucune prise en charge du Concepteur XAML
- Les nouvelles applications C++/CX ne sont pas prises en charge. Toutefois, vos applications existantes continueront à fonctionner (veuillez passer à C++/WinRT dès que possible)
- La prise en charge de plusieurs fenêtres dans les applications de bureau est en cours de développement, mais n’est pas encore terminée et stable.
  - Signalez un bogue sur notre dépôt si vous rencontrez de nouveaux problèmes ou des régressions avec le comportement des fenêtres multiples.
- Le déploiement d’appareils de bureau sans package n’est pas pris en charge
- Lors de l’exécution d’une application de bureau avec F5, vérifiez que vous exécutez le projet d’empaquetage. Si vous utilisez F5 sur le projet d’application, vous allez exécuter une application non empaquetée, que WinUI 3 ne prend pas encore en charge.

### <a name="missing-platform-features"></a>Fonctionnalités de plateforme manquantes

- Support Xbox
- Support HoloLens
- Fenêtres contextuelles
  - Plus spécifiquement, la propriété `ShouldConstrainToRootBounds` agit toujours comme si elle était définie sur `true`, quelle que soit la valeur de la propriété.
- Prise en charge des entrées manuscrites
- Acrylique
- MediaElement et MediaPlayerElement
- MapControl
- RenderTargetBitmap pour SwapChainPanel et contenu non-XAML
- SwapChainPanel ne prend pas en charge la transparence
- Global Reveal utilise le comportement de secours, un pinceau plein
- XAML Islands n’est pas pris en charge dans cette version
- Les bibliothèques d’écosystèmes tiers ne fonctionneront pas complètement
- Les éditeurs IME ne fonctionnent pas

### <a name="known-issues"></a>Problèmes connus

- Alt+F4 ne ferme pas les fenêtres des applications de bureau.

-   Si vous utilisez une WebView2 dans votre application, mais qu’elle n’est pas rendue ou chargée, il se peut que vous exécutiez une version incompatible du navigateur Edge. [Téléchargez la version Dev Channel de Microsoft Edge](https://www.microsoftedgeinsider.com/en-us/download) et veillez à désinstaller toutes les instances existantes de Microsoft Edge Beta, de Microsoft Edge Dev et de Microsoft Edge WebView2 Runtime.

- Les fonctions de marshaling ne doivent pas être utilisées dans les applications WinUI .NET 5, car elles n’interagissent pas correctement avec C#/WinRT. Pour plus d’informations, consultez [cette page](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md).

- Si vous subissez un plantage en définissant une propriété d’URI, par exemple en utilisant un `{Binding}` dans le balisage XAML, vous pouvez contourner ce problème en utilisant `{x:Bind}` ou une préversion de C#/WinRT. Pour ce faire, ajoutez les lignes suivantes à votre fichier .csproj :

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- Pour les applications UWP C# :

  L’environnement WinUI 3 est un ensemble de composants WinRT qui peuvent être utilisés à partir de C++ (avec C++/WinRT) ou C# . Pour C#, il existe deux versions de .NET, en fonction du modèle d’application : lors de l’utilisation de WinUI 3 dans une application UWP, vous utilisez .NET Native ; lors de l’utilisation dans une application de bureau, vous utilisez .NET 5 (et C#/WinRT).

  Lors de l’utilisation de C# pour une application WinUI 3 dans UWP, il y a quelques différences dans les espaces de noms de l’API par rapport à C# dans une application de bureau WinUI 3 ou une application WinUI 2 C# : certains types se trouvent dans un espace de noms `Microsoft` au lieu d’un espace de noms `System`. Par exemple, l’interface `INotifyPropertyChanged` n’est pas dans l’espace de noms `System.ComponentModel`, mais se trouve dans l’espace de noms `Microsoft.UI.Xaml.Data`. 

  Cela s’applique aux :
    - `INotifyPropertyChanged` (et types associés)
    - `INotifyCollectionChanged`
    - `ICommand`

  Les versions de l’espace de noms `System` existent toujours, mais ne peuvent pas être utilisées avec WinUI 3. Cela signifie que `ObservableCollection` ne fonctionne pas en l’état dans les applications UWP C# WinUI 3. Pour une solution de contournement, consultez l’[exemple CollectionsInterop](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs) dans l’[exemple XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview).

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML Controls Gallery (Branche WinUI 3 Preview 3)

Consultez la [branche WinUI 3 Preview 3 de la galerie de contrôles XAML](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) pour obtenir un exemple d’application qui inclut l’ensemble des contrôles et des fonctionnalités de WinUI 3 Preview 3.

![Application XAML Controls Gallery WinUI 3 Preview 3](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
*Exemple de l’application XAML Controls Gallery WinUI 3 Preview 3*

Pour télécharger l’exemple, clonez la branche **winui3preview** à l’aide de la commande suivante :

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Après le clonage, veillez à basculer vers la branche **winui3preview** dans votre environnement Git local : 

```
git checkout winui3preview
```
