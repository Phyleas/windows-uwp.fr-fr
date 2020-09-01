---
title: Porter la boucle de jeu
description: Montre comment implémenter une fenêtre pour un jeu de plateforme Windows universelle (UWP) et comment récupérer la boucle de jeu, notamment comment créer une interface IFrameworkView pour contrôler une classe CoreWindow en plein écran.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, Portage, boucle de jeu, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e62b7e6576ff1b39cbeba2c201f929952abd4105
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159093"
---
# <a name="port-the-game-loop"></a>Porter la boucle de jeu



**Résumé**

-   [Partie 1 : initialiser Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Partie 2 : convertir l’infrastructure de rendu](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Partie 3 : porter la boucle de jeu


Montre comment implémenter une fenêtre pour un jeu de plateforme Windows universelle (UWP) et comment récupérer la boucle de jeu, notamment comment créer une interface [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) pour contrôler une classe [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) en plein écran. Partie 3 de la procédure pas à pas [Porter une application Direct3D 9 simple vers DirectX 11 et UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Créer une fenêtre


Pour configurer une fenêtre de bureau avec une fenêtre d’affichage Direct3D 9, nous devions implémenter l’infrastructure de fenêtrage traditionnelle des applications de bureau. Nous devions créer un HWND, définir la taille de la fenêtre, fournir un rappel de traitement de fenêtre, le rendre visible, etc.

L’environnement UWP possède un système beaucoup plus simple. Au lieu de configurer une fenêtre traditionnelle, un jeu de Microsoft Store à l’aide de DirectX implémente [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Cette interface existe pour les applications et les jeux DirectX qui s’exécutent directement dans un [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) à l’intérieur du conteneur d’application.

> **Remarque**    Windows fournit des pointeurs managés à des ressources telles que l’objet d’application source et le [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow). Consultez [**handle to Object, opérateur (^)**](/cpp/extensions/handle-to-object-operator-hat-cpp-component-extensions).

 

Votre « principale » classe doit hériter de l’interface [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) et implémenter les cinq méthodes **IFrameworkView** : [**Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [**Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load), [**Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) et [**Uninitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). En plus de créer l’interface **IFrameworkView**, qui correspond (essentiellement) à l’emplacement auquel votre jeu va résider, vous devez implémenter une classe de fabrique qui crée une instance de votre interface **IFrameworkView**. Votre jeu possède quand même un exécutable avec une méthode appelée **main()**, mais celle-ci peut uniquement utiliser la fabrique pour créer l’instance **IFrameworkView**.

Fonction main

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Fabrique IFrameworkView

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Porter la boucle de jeu


Examinons la boucle de jeu de notre implémentation Direct3D 9. Ce code existe dans la fonction main de l’application. Chaque itération de cette boucle traite un message de fenêtre ou génère le rendu d’une image.

Boucle de jeu dans un jeu sur ordinateur Direct3D 9

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

La boucle de jeu est similaire, mais plus facile, dans la version UWP de notre jeu :

La boucle de jeu va dans la méthode [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) (plutôt que **main()**) car notre jeu fonctionne au sein de la classe [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView).

Au lieu d’implémenter un Framework de gestion des messages et d’appeler [**PeekMessage**](/windows/desktop/api/winuser/nf-winuser-peekmessagea), nous pouvons appeler la méthode [**ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) intégrée à la [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)de la fenêtre de l’application. La boucle de jeu n’a pas besoin de se ramifier et de gérer les messages : il suffit d’appeler la méthode **ProcessEvents** et de continuer.

Boucle de jeu dans le jeu de Microsoft Store Direct3D 11

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Nous avons à présent une application UWP qui configure la même infrastructure graphique de base, et génère le rendu du même cube haut en couleur que celui de notre exemple DirectX 9.

## <a name="where-do-i-go-from-here"></a>Étapes suivantes


Marquez d’un signet le [Forum Aux Questions (FAQ) sur le portage DirectX 11](directx-porting-faq.md).

Les modèles UWP DirectX incluent une infrastructure de périphérique Direct3D robuste prête à l’emploi avec votre jeu UWP. Pour obtenir des conseils sur la sélection du modèle approprié, voir [Créer un projet de jeu DirectX à partir d’un modèle](user-interface.md).

Consultez les Articles détaillés suivants sur le développement de jeux Microsoft Store Game :

-   [Procédure pas à pas : jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Audio pour les jeux](working-with-audio-in-your-directx-game.md)
-   [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 