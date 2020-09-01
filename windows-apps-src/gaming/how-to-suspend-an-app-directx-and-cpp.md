---
title: Comment suspendre une application (DirectX et C++)
description: Cette rubrique montre comment enregistrer des données importantes relatives à l’état du système et aux applications lorsque le système interrompt l’exécution de votre application DirectX de plateforme UWP.
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, suspendre, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 28c93228f149b25ba129b632d23f9798712ee80d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163113"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>Comment suspendre une application (DirectX et C++)



Cette rubrique montre comment enregistrer des données importantes relatives à l’état du système et aux applications lorsque le système interrompt l’exécution de votre application DirectX de plateforme UWP.

## <a name="register-the-suspending-event-handler"></a>Enregistrer le gestionnaire d’événements de suspension


Tout d’abord, inscrivez-vous pour gérer l’événement [**CoreApplication :: suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending) , qui est déclenché lorsque votre application est déplacée vers un état suspendu par une action de l’utilisateur ou du système.

Ajoutez le code suivant à votre implémentation de la méthode [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) de votre fournisseur d’affichage :

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>Enregistrer les données d’application avant la suspension


Lorsque votre application traite l’événement [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending), elle a la possibilité d’enregistrer ses données d’application importantes dans la fonction du gestionnaire. L’application doit utiliser l’API de stockage [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) pour enregistrer des données d’application simples de façon synchrone. Si vous développez un jeu, enregistrez les informations critiques relatives à l’état du jeu. N’oubliez pas de mettre en suspens le traitement audio !

Maintenant, mettez en œuvre le rappel. Enregistrez les données d’application dans cette méthode.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

Ce rappel doit s’effectuer dans les cinq secondes. Durant ce rappel, vous devez demander un report en appelant [**SuspendingOperation::GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral), qui lance le compte à rebours. Quand votre application a terminé l’enregistrement, appelez [**SuspendingDeferral::Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete) pour signaler au système que votre application est maintenant prête à être suspendue. Si vous ne demandez pas de report ou si votre application met plus de 5 secondes pour enregistrer les données, elle est suspendue automatiquement.

Ce rappel se produit sous la forme d’un message d’événement traité par [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) pour le [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)de l’application. Ce rappel n’est pas effectué si vous n’appelez pas [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) à partir de la boucle principale de votre application (mise en œuvre dans la méthode [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) de votre fournisseur d’affichage).

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="call-trim"></a>Appeler Trim()


À partir de Windows 8.1, toutes les applications DirectX UWP doivent appeler [**IDXGIDevice3 :: Trim**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) lors de la suspension. Cet appel donne instruction au pilote graphique de libérer toutes les mémoires tampons temporaires allouées pour l’application, ce qui limite les chances de voir l’application se terminer pour cause de récupération des ressources de mémoire pendant qu’elle est à l’état de suspension. Il s’agit d’une condition de certification pour Windows 8.1.

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>Libérer les ressources exclusives et les descripteurs de fichiers


Lorsque votre application traite l’événement [**CoreApplication::Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending), elle a également la possibilité de libérer les ressources exclusives et les descripteurs de fichiers. En libérant explicitement les ressources exclusives et les descripteurs de fichiers, vous permettez aux autres applications d’y accéder pendant que votre application ne les utilise pas. Lorsque l’application est activée après un arrêt, elle doit ouvrir ses ressources exclusives et descripteurs de fichiers.

## <a name="remarks"></a>Remarques


Le système suspend votre application chaque fois que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan.

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il met fin à votre application. Lorsque l’utilisateur rebascule vers une application suspendue qui a été arrêtée, le système envoie un événement [**Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) et doit restaurer ses données d’application dans son gestionnaire de l’événement **CoreApplicationView::Activated**.

Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers au moment où elle est mise en suspens pour ensuite les restaurer lorsque l’application est activée après avoir été arrêtée.

## <a name="related-topics"></a>Rubriques connexes

* [Comment relancer une application (DirectX et C++)](how-to-resume-an-app-directx-and-cpp.md)
* [Comment activer une application (DirectX et C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 