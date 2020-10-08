---
title: WinUI 3 Preview 2 (juillet 2020)
description: Vue d’ensemble de la version WinUI 3 Preview 2.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 11c7ff587c7c237c19ad627587f082be84e68bf8
ms.sourcegitcommit: 337f31b3fe3ff434dbc2c232fb84c3b22ebd4be8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804573"
---
# <a name="windows-ui-library-3-preview-2-july-2020"></a>Bibliothèque d’interface utilisateur Windows 3 Preview 2 (juillet 2020)

La bibliothèque d’interface utilisateur Windows (WinUI) 3 est un framework d’expérience utilisateur natif pour les applications de bureau Windows et UWP.

**WinUI 3 Preview 2** est une version de qualité et de stabilité axée sur la résolution des bogues et des problèmes connus de la préversion 1.

**Consultez [Limitations et problèmes connus de Preview 2](#preview-2-limitations-and-known-issues)** .

> [!Important]
> Cette préversion de WinUI 3 est destinée à une évaluation anticipée et à la collecte de commentaires de la communauté des développeurs. Il ne doit **PAS** être utilisé pour les applications de production.
>
> Nous continuerons à fournir des préversions de WinUI 3 tout au long de 2020 et au début de 2021, après quoi la première version officielle sera rendue disponible.
>
> Utilisez le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml) pour fournir des commentaires et journaliser des suggestions et des problèmes.

## <a name="install-winui-3-preview-2"></a>Installer WinUI 3 Preview 2

WinUI 3 Preview 2 inclut des modèles de projet Visual Studio qui vous permettre de démarrer la génération d’applications avec une interface utilisateur basée sur WinUI, et un package NuGet qui contient les bibliothèques WinUI. Pour installer WinUI 3 Preview 2, effectuez les étapes suivantes.

> [!NOTE]
> Vous pouvez également cloner et générer la version WinUI 3 Preview 2 de l’application [XAML Controls Gallery](#xaml-controls-gallery-winui-3-preview-2-branch).

1. Vérifiez que votre ordinateur de développement dispose de Windows 10 version 1803 (build 17134) ou d’une version plus récente.

2. Installer [Visual Studio 2019, version 16.7.2](https://visualstudio.microsoft.com/vs/)

    Vous devez inclure les charges de travail suivantes lors de l’installation de Visual Studio :
    - Développement .NET Desktop
    - Développement pour la plateforme Windows universelle

    Pour générer des applications C++, vous devez également inclure les charges de travail suivantes :
    - Développement Desktop en C++
    - Le composant facultatif *Outils de plateforme Windows universelle C++ (v142)* pour la charge de travail Plateforme Windows universelle (voir « Détails de l’installation » sous la section « Développement pour plateforme Windows universelle », dans le volet de droite)

    Une fois que vous avez téléchargé Visual Studio, veillez à activer les préversions .NET dans le programme : 
    - Accédez à outils > Options > Fonctionnalités en préversion > sélectionnez « Utiliser les préversions du kit SDK .NET Core (nécessite un redémarrage) ». 

3. Si vous voulez créer des projets WinUI de bureau pour les applications C#/.NET 5 et C++/Win32, vous devez également installer les versions x64 et x86 de .NET 5 Preview 5. **Notez que .NET 5 Preview 5 est actuellement la seule préversion prise en charge de .NET 5 pour WinUI 3**:

    - [Programme d’installation x64 pour .NET 5 Preview 5](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x64-installer)
    - [Programme d’installation x86 pour .NET 5 Preview 5](https://dotnet.microsoft.com/download/dotnet/thank-you/sdk-5.0.100-preview.5-windows-x86-installer)

4. Téléchargez et installez le [package VSIX WinUI 3 Preview 2](https://aka.ms/winui3/previewdownload). Ce package VSIX ajoute les modèles de projet WinUI 3 et le package NuGet contenant les bibliothèques WinUI 3 à Visual Studio 2019.

    Pour obtenir des instructions sur la façon d’ajouter le package VSIX à Visual Studio, consultez [Recherche et utilisation des extensions Visual Studio](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).


## <a name="create-winui-projects"></a>Créer des projets WinUI

Après l’installation du package VSIX WinUI 3 Preview 2, vous êtes prêt à créer un projet à l’aide de l’un des modèles de projet WinUI dans Visual Studio. Pour accéder aux modèles de projet WinUI dans la boîte de dialogue **Créer un projet**, filtrez le langage sur **C++** ou **C#** , la plateforme sur **Windows** et le type de projet sur **WinUI**. Vous pouvez également rechercher *WinUI*, puis sélectionner l’un des modèles C# ou C++ disponibles.

![Modèles de projet WinUI](images/winui-projects-csharp.png)

Pour plus d’informations sur la façon de commencer avec les modèles de projet WinUI, consultez les articles suivants :

- [Bien démarrer avec WinUI 3 pour les applications de bureau](get-started-winui3-for-desktop.md)
- [Bien démarrer avec WinUI 3 pour les applications UWP](get-started-winui3-for-uwp.md)

En plus des [limitations et les problèmes connus](#preview-2-limitations-and-known-issues), la génération d’une application à l’aide des projets WinUI est semblable à la génération d’une application UWP avec XAML et WinUI 2.x. Par conséquent, la majeure partie des instructions et de la documentation relatives aux applications UWP et les espaces de noms WinRT **Windows.UI** figurant dans le SDK Windows sont applicables.

Si vous avez créé un projet à l’aide de WinUI 3 Preview 1, vous pouvez mettre à niveau votre projet pour utiliser Preview 2. Consultez les instructions détaillées sur [notre dépôt GitHub](https://aka.ms/winui3/upgrade-instructions).

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
| Composant Windows Runtime (WinUI dans UWP) | C# et C++ | Crée un [composant Windows Runtime](/windows/uwp/winrt-components/) écrit en C# ou en C++/WinRT qui peut être consommé par n’importe quelle application UWP avec une interface utilisateur basée sur WinUI, quel que soit le langage de programmation dans lequel l’application est écrite. |

### <a name="item-templates-for-winui-3"></a>Modèles d’élément pour WinUI 3

Les modèles d’élément suivants sont disponibles pour être utilisés dans un projet WinUI. Pour accéder à ces modèles de projet WinUI, cliquez avec le bouton droit sur le nœud du projet dans l’**Explorateur de solutions**, sélectionnez **Ajouter** -> **Nouvel élément**, puis cliquez sur **WinUI** dans la boîte de dialogue **Ajouter un nouvel élément**.

![Modèles d’élément WinUI](images/winui-items-csharp.png)

| Modèle | Language | Description |
|----------|----------|-------------|
| Page vierge (WinUI) | C# et C++ | Ajoute un fichier XAML et un fichier de code qui définit une nouvelle page dérivée de la classe **Microsoft.UI.Xaml.Controls.Page** de la bibliothèque WinUI. |
| Fenêtre vide (WinUI dans les applications de bureau) | C# et C++ | Ajoute un fichier XAML et un fichier de code qui définit une nouvelle fenêtre dérivée de la classe **Microsoft.UI.Xaml.Window** de la bibliothèque WinUI. |
| Contrôle personnalisé (WinUI) | C# et C++ | Ajoute un fichier de code pour créer un contrôle basé sur un modèle avec un style par défaut. Le contrôle basé sur un modèle dérive de la classe **Microsoft.UI.Xaml.Controls.Control** de la bibliothèque WinUI.<p></p>Pour obtenir une procédure pas à pas qui montre comment utiliser ce modèle d’élément, consultez [Contrôles XAML basés sur un modèle pour les applications UWP et WinUI 3 avec C++/WinRT](xaml-templated-controls-cppwinrt-winui3.md). Pour plus d’informations sur les contrôles basés sur des modèles, consultez [Contrôles XAML personnalisés](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |
| Dictionnaire de ressources (WinUI) | C# et C++ | Ajouter une collection à clé et vide de ressources XAML. Pour plus d’informations, consultez [Informations de référence sur les ressources ResourceDictionary et XAML](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| Fichier de ressources (WinUI) | C# et C++ | Ajoute un fichier dans lequel stocker les ressources de type chaîne et les ressources conditionnelles de votre application. Vous pouvez utiliser cet élément pour faciliter la localisation de votre application. Pour plus d’informations, consultez [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](/windows/uwp/app-resources/localize-strings-ui-manifest). |
| Contrôle utilisateur (WinUI) | C# et C++ | Ajoute un fichier XAML et un fichier de code pour la création d’un contrôle utilisateur dérivé de la classe **Microsoft.UI.Xaml.Controls.UserControl** de la bibliothèque WinUI. En général, un contrôle utilisateur encapsule des contrôles existants associés et fournit sa propre logique.<p></p>Pour plus d’informations sur les contrôles utilisateur, consultez [Contrôles XAML personnalisés](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls). |

## <a name="bug-fixes-and-other-improvements-in-winui-3-preview-2"></a>Résolutions de bogues et autres améliorations dans WinUI 3 Preview 2

Il s’agit d’une liste complète des corrections de bogues et d’autres mises à jour pour Preview 2. Pour obtenir la liste des corrections de bogues les plus critiques résolus dans cette version, consultez notre [annonce de version](https://aka.ms/winui3/preview2-announcement).

> [!NOTE]
> WinUI 3 Preview 2 utilise la version 2.4.2 de la bibliothèque WinUI 2. 

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) et [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged) fonctionnent désormais comme prévu dans les applications de bureau C#
  - Cela a supprimé certains autres problèmes qui concernaient les contrôles de collection qui n’étaient pas mis à jour dans l’interface utilisateur alors qu’ils l’étaient dans le back-end.
  - *Merci à @hshristov d’avoir soumis un [problème similaire](https://github.com/microsoft/microsoft-ui-xaml/issues/2490) sur GitHub !*
- Preview 2 est désormais compatible avec [.NET 5 Preview 5](/dotnet/api/?view=net-5.0&preserve-view=true) pour les applications de bureau
- WinUI 3 a désormais un équivalent avec [WinUI 2.4](../winui2/release-notes/winui-2.4.md), qui inclut de nouveaux contrôles et de nouvelles fonctionnalités comme un [NavigationView hiérarchique](../winui2/release-notes/winui-2.4.md#hierarchical-navigation) et [ProgressRing](../winui2/release-notes/winui-2.4.md#progressring).
- Plantage résolu : utilisation de [TabView](/windows/uwp/design/controls-and-patterns/tab-view) avec une interaction tactile
- [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) dans l’[exemple Galerie de contrôles XAML](#xaml-controls-gallery-winui-3-preview-2-branch) utilise désormais le mode Left au lieu du mode LeftCompact
- Plantage résolu : saisie trop rapide dans la validation d’entrée et [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box)
  - *Merci à @paulovilla d’avoir soumis [ce problème](https://github.com/microsoft/microsoft-ui-xaml/issues/2563) sur GitHub !*
- Plantage résolu : interaction avec l’interface utilisateur XAML pendant que le menu [TextBox](/windows/uwp/design/controls-and-patterns/text-box) est actif
- Le texte du titre [Exemple Galerie de contrôles XAML](#xaml-controls-gallery-winui-3-preview-2-branch) n’est plus illisible après avoir navigué vers plusieurs pages
- L’utilisation des fonctionnalités tactiles avec [WebView2](/microsoft-edge/webview2/) n’entraîne plus un léger décalage de position
- Les classes dans WinUIEdit.dll ont été déplacées de l’espace de noms Windows.UI.Text vers l’espace de noms Microsoft.UI.Text
- Plantage résolu : sélection d’un élément dans [TreeView](/windows/uwp/design/controls-and-patterns/tree-view) en mode de sélection multiple (dans Windows 10 version 1803)
- Les membres Point, Rect et Size sont maintenant de type double dans la projection C# des API pour les applications de bureau.
  - *Merci à @dotMorten d’avoir soumis [ce problème](https://github.com/microsoft/microsoft-ui-xaml/issues/2474) sur GitHub !*
- Plantage résolu : utilisation de [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) avec un fichier .rtf
- Le bouton de fermeture de [TabView](/windows/uwp/design/controls-and-patterns/tab-view) n’a plus d’info-bulle vide
- Le contrôle [Image](/windows/uwp/design/controls-and-patterns/images-imagebrushes) restitue désormais correctement les fichiers SVG
  - *Merci à @mqudsi d’avoir soumis [ce problème](https://github.com/microsoft/microsoft-ui-xaml/issues/2565) sur GitHub !*
- Plantage résolu : utilisation/navigation vers l’élément Page
- L’utilisation des fonctionnalités tactiles pour sélectionner des éléments dans un [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) désélectionne désormais tous les autres éléments (en mode de sélection unique)
- Plantage résolu : l’exception LayoutSliderException due à la valeur définie sur le contrôle [Slider](/windows/uwp/design/controls-and-patterns/slider) dimensionné de façon spécifique ne se produit plus 
  - *Merci à @hig-dev d’avoir soumis [ce problème](https://github.com/microsoft/microsoft-ui-xaml/issues/477) sur GitHub !*
- Plantage résolu : l’utilisation de [ColorPicker](/windows/uwp/design/controls-and-patterns/color-picker) provoquait un plantage au moment de l’arrêt
- Plantage résolu : l’utilisation de [Pivot](/windows/uwp/design/controls-and-patterns/pivot) provoquait un plantage au moment de l’arrêt
- Plantage résolu : plantage de [NavigationView](/windows/uwp/design/controls-and-patterns/navigationview) provoqué par des ressources manquantes dans Windows 10 version 1803
- Plantage résolu : Focus sur l’éditeur personnalisé [RichEditBox](/windows/uwp/design/controls-and-patterns/rich-edit-box) 
- Plantage résolu : [SemanticZoom](/windows/uwp/design/controls-and-patterns/semantic-zoom) 
- la liaison fonctionne désormais comme prévu dans le balisage avec Mode=OneWay qui est implicite
  - *Merci à @tomasfabian d’avoir soumis [ce problème](https://github.com/microsoft/microsoft-ui-xaml/issues/2630) sur GitHub !*
- Animation corrigée : Nouveautés de l’[exemple Galerie de contrôles XAML](#xaml-controls-gallery-winui-3-preview-2-branch)

## <a name="new-features-and-capabilities-introduced-in-winui-3-preview-1"></a>Nouvelles fonctionnalités et fonctions introduites dans WinUI 3 Preview 1

Les fonctionnalités et fonctions suivantes ont été introduites dans WinUI 3 Preview 1 et continuent à être prises en charge dans WinUI 3 Preview 2.

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

## <a name="preview-2-limitations-and-known-issues"></a>Limitations et problèmes connus de Preview 2

La version Preview 2 est, comme son nom l’indique, seulement une préversion. Les scénarios concernant les applications de bureau Win32 sont particulièrement nouveaux. Attendez-vous à des bogues, des limitations et d’autres problèmes.

Voici quelques-uns des problèmes connus avec WinUI 3 Preview 2. Si vous détectez un problème qui ne figure pas dans la liste ci-dessous, faites-le nous savoir en contribuant à un problème existant ou en consignant un nouveau problème dans le dépôt [GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Prise en charge de plateforme et de système d’exploitation

WinUI 3 Preview 2 est compatible avec les PC qui exécutent la mise à jour d’avril 2018 de Windows 10 (version 1803 - build 17134) et les versions ultérieures.

### <a name="developer-tools"></a>Outils de développement

- Seules les applications C# et C++/WinRT sont prises en charge pour le moment
- Les applications de bureau prennent en charge .NET 5 et C# 8, et doivent être empaquetées
- Les applications UWP prennent en charge .NET Native et C# 7.3
- IntelliSense est incomplet
- Aucun concepteur visuel
- Aucun rechargement à chaud
- Aucune arborescence visuelle dynamique
- Le développement avec VS Code n’est pas encore pris en charge
- Les nouvelles applications C++/CX ne sont pas prises en charge. Toutefois, vos applications existantes continueront à fonctionner (veuillez passer à C++/WinRT dès que possible)
- Le contenu WinUI 3 ne peut être que dans une fenêtre par processus ou un ApplicationView par application
- Le déploiement d’appareils de bureau sans package n’est pas pris en charge
- Aucune prise en charge d’ARM64
- Contrôles personnalisés C# dans les applications UWP : `Themes/Generic.xaml` n’est pas généré automatiquement. Vous pouvez contourner ce problème en créant manuellement un dossier Thèmes dans votre classe et en plaçant un fichier XAML, appelé `Generic.xaml`, à l’intérieur de celui-ci.
- Après avoir ajouté un contrôle personnalisé WinUI à votre projet, il se peut que l’en-tête « CustomControl.h » soit manquant dans vos fichiers. Vous pouvez contourner ce problème en ajoutant manuellement cet en-tête dans votre fichier `pch.h`.
- L’ajout de DataGrid, d’autres contrôles du Kit de ressources Communauté Windows et de contrôles de bibliothèque tiers peut entraîner l’échec de votre build. Pour contourner ce problème, ajoutez ce dictionnaire fusionné à votre fichier `App.xaml` :
  ```xaml
  <ResourceDictionary Source="ms-appx:///<library_name>/Themes/Generic.xaml"/>
  ```

### <a name="missing-platform-features"></a>Fonctionnalités de plateforme manquantes

- Support Xbox
- Support HoloLens
- Fenêtres contextuelles
- Prise en charge des entrées manuscrites
- Acrylique en arrière-plan
- MediaElement et MediaPlayerElement
- RenderTargetBitmap
- MapControl
- SwapChainPanel ne prend pas en charge la transparence
- Global Reveal utilise le comportement de secours, un pinceau plein
- XAML Islands n’est pas pris en charge dans cette version
- Les bibliothèques d’écosystèmes tiers ne fonctionneront pas complètement
- Les éditeurs IME ne fonctionnent pas

### <a name="known-issues"></a>Problèmes connus


- Applications UWP C# :

  Le framework WinUI 3 est un ensemble de composants WinRT, et bien que WinRT présente des types et des objets similaires à ceux qui se trouvent dans .NET, ils ne sont pas intrinsèquement compatibles.  Les projections C#/WinRT gèrent l’interopérabilité entre .NET et WinRT dans .NET 5, ce qui vous permet d’utiliser librement les interfaces .NET dans votre application .NET 5 dès aujourd’hui. 
  
  Toutefois, C#/WinRT n’est pas en mesure de gérer l’interopérabilité dans les applications .NET natives, donc les API WinUI 3 sont projetées directement dans les applications UWP. Du coup, vous ne pouvez plus utiliser ces mêmes interfaces .NET. **Une fois que les applications UWP n’utilisent plus .NET Native, cette limitation n’existe plus**.

  Par exemple, l’API `INotifyPropertyChanged` est projetée dans l’espace de noms `System.ComponentModel` pour WinUI3 dans les applications de bureau, mais elle apparaît dans l’espace de noms `Microsoft.UI.Xaml.Data` pour WinUI3 dans les applications UWP (et toutes les applications C++). 
  
  Ce problème s'applique à :
    - `INotifyPropertyChanged` (et types associés)
    - `INotifyCollectionChanged`
    - `ICommand`

> [!Note] 
> Avec `INotifyPropertyChanged` et `INotifyCollectionChanged` qui ne fonctionnent pas comme prévu, la classe `ObservableCollection<T>` est également affectée. Pour un exemple d’implémentation de votre propre version de `ObservableCollection<T>`, consultez [cet exemple](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs). 


## <a name="xaml-controls-gallery-winui-3-preview-2-branch"></a>XAML Controls Gallery (Branche WinUI 3 Preview 2)

Consultez la [branche WinUI 3 Preview 2 de l’application XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) pour obtenir un exemple d’application qui inclut l’ensemble des contrôles et fonctionnalités WinUI 3 Preview 2.

![Application XAML Controls Gallery WinUI 3 Preview 2](images/WinUI3XamlControlsGalleryP2.png)<br/>
*Exemple de l’application XAML Controls Gallery WinUI 3 Preview 2*

Pour télécharger l’exemple, clonez la branche **winui3preview** à l’aide de la commande suivante :

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

Après le clonage, veillez à basculer vers la branche **winui3preview** dans votre environnement Git local :

```
git checkout winui3preview
```
