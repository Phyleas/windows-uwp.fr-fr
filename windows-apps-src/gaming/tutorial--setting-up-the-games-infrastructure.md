---
title: Configurer le projet de jeu
description: La première étape du développement de votre jeu consiste à configurer un projet dans Microsoft Visual Studio. Une fois que vous avez configuré un projet spécifiquement pour le développement de jeux, vous pouvez le réutiliser ultérieurement comme type de modèle.
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, jeux, configuration, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: c0c8d82752d9b73a3e3e7ffcec3ced1515564072
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409588"
---
# <a name="set-up-the-game-project"></a>Configurer le projet de jeu

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

La première étape du développement de votre jeu consiste à créer un projet dans Microsoft Visual Studio. Une fois que vous avez configuré un projet spécifiquement pour le développement de jeux, vous pouvez le réutiliser ultérieurement comme type de modèle.

## <a name="objectives"></a>Objectifs

* Créez un projet dans Visual Studio à l’aide d’un modèle de projet.
* Comprenez le point d’entrée et l’initialisation du jeu en examinant le fichier source de la classe **app** .
* Examinez la boucle du jeu.
* Passez en revue le fichier **Package. appxmanifest** du projet.

## <a name="create-a-new-project-in-visual-studio"></a>Création d'un projet dans Visual Studio

> [!NOTE]
> Pour plus d’informations sur la configuration du développement Visual Studio pour C++/WinRT&mdash;notamment l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet)&mdash;, consultez [Prise en charge de Visual Studio pour C++/WinRT](/windows/uwp/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Installez d’abord (ou mettez à jour) la dernière version de l’extension Visual Studio/WinRT C++ (VSIX). consultez la remarque ci-dessus. Ensuite, dans Visual Studio, créez un nouveau projet basé sur le modèle de projet **application de base (C++/WinRT)** . Ciblez la dernière version en disponibilité générale (autrement dit, pas la préversion) du SDK Windows.

## <a name="review-the-app-class-to-understand-iframeworkviewsource-and-iframeworkview"></a>Examiner la classe **app** pour comprendre **IFrameworkViewSource** et **IFrameworkView**

Dans votre projet d’application principal, ouvrez le fichier de code source `App.cpp` . Dans, il existe l’implémentation de la classe **app** , qui représente l’application et son cycle de vie. Dans ce cas, bien sûr, nous savons que l’application est un jeu. Toutefois, nous y reviendrons en tant qu' *application* afin de parler plus généralement de la façon dont une application plateforme Windows universelle (UWP) s’initialise.

### <a name="the-wwinmain-function"></a>Fonction wWinMain

La fonction **wWinMain** est le point d’entrée de l’application. Voici à quoi ressemble le **wWinMain** (à partir de `App.cpp` ).

```cppwinrt
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

Nous créons une instance de la classe **app** (il s’agit de la seule instance de l' **application** créée) et nous la transmettons à la méthode statique [**CoreApplication. Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run) . Notez que **CoreApplication. Run** attend une interface [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . La classe **app** doit donc implémenter cette interface.

Les deux sections suivantes de cette rubrique décrivent les interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) et [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Ces interfaces (ainsi que **CoreApplication. Run**) représentent un moyen pour votre application de fournir des fenêtres à un *fournisseur d’affichage*. Windows utilise ce fournisseur d’affichage pour connecter votre application au shell Windows afin que vous puissiez gérer les événements du cycle de vie des applications.

### <a name="the-iframeworkviewsource-interface"></a>Interface IFrameworkViewSource

La classe **app** implémente effectivement [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), comme vous pouvez le voir dans la liste ci-dessous.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    IFrameworkView CreateView()
    {
        return *this;
    }
    ...
}
```

Un objet qui implémente **IFrameworkViewSource** est un objet de *fabrique de fournisseurs d’affichage* . Le travail de cet objet consiste à fabriquer et à retourner un objet de *fournisseur d’affichage* .

**IFrameworkViewSource** a la méthode unique [**IFrameworkViewSource :: CreateView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource.createview). Windows appelle cette fonction sur l’objet que vous transmettez à **CoreApplication. Run**. Comme vous pouvez le voir ci-dessus, l’implémentation de l' **application :: CreateView** de cette méthode retourne `*this` . En d’autres termes, l’objet d' **application** se retourne lui-même. Étant donné que **IFrameworkViewSource :: CreateView** a un type de valeur de retour [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource), la classe **app** doit également implémenter *cette* interface. Et vous pouvez voir qu’il figure dans la liste ci-dessus.

### <a name="the-iframeworkview-interface"></a>Interface IFrameworkView

Un objet qui implémente [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) est un objet de *fournisseur d’affichage* . Et nous avons maintenant fourni des fenêtres avec ce fournisseur d’affichage. Il s’agit de l’objet d' **application** que nous avons créé dans **wWinMain**. La classe **app** sert donc à la fois à la *fabrique de fournisseur d’affichage* et au fournisseur d' *affichage*.

Désormais, Windows peut appeler les implémentations de la classe **app** des méthodes de **IFrameworkView**. Dans les implémentations de ces méthodes, votre application a la possibilité d’effectuer des tâches telles que l’initialisation, de commencer à charger les ressources dont elle a besoin, de connecter les gestionnaires d’événements appropriés et de recevoir les [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) que votre application utilisera pour afficher sa sortie.

Vos implémentations des méthodes de **IFrameworkView** sont appelées dans l’ordre indiqué ci-dessous.

- [**Initialiser**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)
- [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)
- [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)
- L’événement [**CoreApplicationView :: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) est déclenché. Par conséquent, si vous avez (éventuellement) inscrit pour gérer cet événement, votre gestionnaire **OnActivated** est appelé à ce moment-là.
- [**Exécuter**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)
- [**Annuler l’initialisation**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)

Et voici la structure de la classe **app** (en `App.cpp` ), qui illustre les signatures de ces méthodes.

```cppwinrt
struct App : winrt::implements<App, IFrameworkViewSource, IFrameworkView>
{
    ...
    void Initialize(Windows::ApplicationModel::CoreCoreApplicationView const& applicationView) { ... }
    void SetWindow(Windows::UI::Core::CoreWindow const& window) { ... }
    void Load(winrt::hstring const& entryPoint) { ... }
    void OnActivated(
        Windows::ApplicationModel::CoreCoreApplicationView const& applicationView,
        Windows::ApplicationModel::Activation::IActivatedEventArgs const& args) { ... }
    void Run() { ... }
    void Uninitialize() { ... }
    ...
}
```

Il s’agissait simplement d’une présentation de **IFrameworkView**. Nous allons bien plus de détails sur ces méthodes et comment les implémenter dans [la définition de l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md).

### <a name="tidy-up-the-project"></a>Nettoyer le projet

Le projet d’application principal que vous avez créé à partir du modèle de projet contient des fonctionnalités que nous devrions nettoyer à ce stade. Après cela, nous pouvons utiliser le projet pour recréer le jeu de la Galerie de tournage (**Simple3DGameDX**). Apportez les modifications suivantes à la classe **app** dans `App.cpp` .

- Supprimez ses membres de données.
- Supprimer **OnPointerPressed**, **OnPointerMoved**et **AddVisual**
- Supprimez le code de **SetWindow**.

Le projet est généré et exécuté, mais il affiche uniquement une couleur unie dans la zone cliente.

## <a name="the-game-loop"></a>Boucle de jeu

Pour obtenir une idée de ce à quoi ressemble une boucle de jeu, recherchez dans le code source l’exemple de jeu [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que vous avez téléchargé.

La classe **app** a un membre de données, nommé *m_main*, de type **GameMain**. Et ce membre est utilisé dans **App :: Run** comme ceci.

```cppwinrt
void Run()
{
    m_main->Run();
}
```

Vous pouvez trouver **GameMain :: Run** dans `GameMain.cpp` . C’est la principale boucle du jeu, et voici un plan très grossier qui présente les fonctionnalités les plus importantes.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
            Update();
            m_renderer->Render();
            m_deviceResources->Present();
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Et voici une brève description de ce que fait cette boucle de jeu principale.

Si la fenêtre de votre jeu n’est pas fermée, distribuez tous les événements, mettez à jour la minuterie, puis affichez et présentez les résultats du pipeline graphique. Il y a beaucoup d’autres choses à faire pour ces préoccupations. nous allons le faire dans les rubriques qui [définissent l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md), l' [infrastructure de rendu I : introduction au rendu](tutorial--assembling-the-rendering-pipeline.md)et l' [infrastructure de rendu II : le rendu de jeu](tutorial-game-rendering.md). Mais il s’agit de la structure de code de base d’un jeu DirectX UWP.

## <a name="review-and-update-the-packageappxmanifest-file"></a>Examiner et mettre à jour le fichier Package. appxmanifest

Le fichier **Package. appxmanifest** contient des métadonnées relatives à un projet UWP. Ces métadonnées sont utilisées pour l’empaquetage et le lancement de votre jeu, ainsi que pour l’envoi au Microsoft Store. Le fichier contient également des informations importantes que le système du joueur utilise pour fournir l’accès aux ressources système dont le jeu a besoin pour s’exécuter.

Lancez le **Concepteur de manifeste** en double-cliquant sur le fichier **Package. appxmanifest** dans **Explorateur de solutions**.

![capture d’écran de l’éditeur de manifeste package. Appx.](images/simple-dx-game-setup-app-manifest.png)

Pour plus d’informations sur le fichier **package.appxmanifest** et sur la création de packages, voir [Concepteur de manifeste](/previous-versions/br230259(v=vs.140)). Pour le moment, jetez un coup d’œil sur l’onglet **capacités** , puis examinez les options proposées.

![capture d’écran avec les fonctionnalités par défaut d’une application Direct3D.](images/simple-dx-game-setup-capabilities.png)

Si vous ne sélectionnez pas les fonctionnalités utilisées par votre jeu, telles que l’accès à **Internet** pour le tableau à score élevé global, vous ne pourrez pas accéder aux ressources et fonctionnalités correspondantes. Lorsque vous créez un nouveau jeu, veillez à sélectionner toutes les fonctionnalités requises par les API que votre jeu appelle.

Examinons à présent le reste des fichiers fournis avec le modèle d' **application DirectX 11 (Windows universel)** .

## <a name="review-other-important-libraries-and-source-code-files"></a>Examiner d’autres bibliothèques et fichiers de code source importants

Si vous envisagez de créer un modèle de projet de jeu pour vous-même, afin de pouvoir le réutiliser comme point de départ pour les projets futurs, vous souhaiterez copier `GameMain.h` et `GameMain.cpp` extraire le projet [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que vous avez téléchargé, puis l’ajouter à votre nouveau projet d’application principal. Examinez ces fichiers, Découvrez ce qu’ils font et supprimez tout ce qui est spécifique à **Simple3DGameDX**. Mettez également en commentaire tout ce qui dépend du code que vous n’avez pas encore copié. Le seul moyen d’un exemple, `GameMain.h` dépend de `GameRenderer.h` . Vous pouvez supprimer les marques de commentaire lorsque vous copiez plus de fichiers de **Simple3DGameDX**.

Voici un bref questionnaire sur certains des fichiers de **Simple3DGameDX** que vous trouverez utiles à inclure dans votre modèle, si vous en créez un. Dans tous les cas, ceux-ci sont tout aussi importants pour comprendre le fonctionnement de **Simple3DGameDX** .

|Fichier source|Dossier de fichiers|Description|
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|DeviceResources. h/. cpp|Services|Définit la classe **DeviceResources** , qui contrôle toutes les [ressources](tutorial--assembling-the-rendering-pipeline.md#resource)de l’appareil DirectX. Définit également l’interface **IDeviceNotify** , utilisée pour informer votre application que l’appareil de la carte graphique a été perdu ou recréé.|
|DirectXSample.h|Services|Implémente des fonctions d’assistance telles que **ConvertDipsToPixels**. **ConvertDipsToPixels** convertit une longueur en pixels indépendants du périphérique (DIP) en une longueur en pixels physiques.|
|GameTimer. h/. cpp|Services|Définit un minuteur haute résolution utile pour les applications de rendu interactives ou les jeux.|
|GameRenderer.h/.cpp|Rendu|Définit la classe **GameRenderer** , qui implémente un pipeline de rendu de base.|
|GameHud. h/. cpp|Rendu|Définit une classe pour restituer un affichage de tête haut (HUD) pour le jeu, à l’aide de Direct2D et DirectWrite.|
|VertexShader. HLSL et VertexShaderFlat. HLSL|Nuanceurs|Contient le code HLSL (High-Level Shader Language) pour les nuanceurs vertex de base.|
|PixelShader. HLSL et PixelShaderFlat. HLSL|Nuanceurs|Contient le code HLSL (High-Level Shader Language) pour les nuanceurs de pixels de base.|
|ConstantBuffers. hlsli|Nuanceurs|Contient des définitions de structure de données pour les mémoires tampons constantes et les structures de nuanceur utilisées pour passer des matrices MVP (Model-View-projection) et des données par vertex au nuanceur de sommets.|
|pch.h/.cpp|N/A|Contient les/WinRT C++ courants, Windows et DirectX.| 

### <a name="next-steps"></a>Étapes suivantes

À ce stade, nous avons montré comment créer un projet UWP pour un jeu DirectX, examinons quelques-uns des éléments qu’il contient et j’ai commencé à réfléchir à la façon de transformer ce projet en un type de modèle réutilisable pour les jeux. Nous avons également examiné certains des éléments importants de l’exemple de jeu **Simple3DGameDX** .

La section suivante [définit l’infrastructure d’application UWP du jeu](tutorial--building-the-games-uwp-app-framework.md). Nous examinerons plus en détail le fonctionnement de **Simple3DGameDX** .
