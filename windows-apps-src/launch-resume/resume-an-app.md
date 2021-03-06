---
title: Gérer la reprise d’une application
description: Apprenez à actualiser le contenu à l’écran lorsque le système reprend l’exécution de votre application.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: d7898dd727ffb4c9255b66725ea69d2005e8d650
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175143"
---
# <a name="handle-app-resume"></a>Gérer la reprise d’une application

**API importantes**

- [**Reprise**](/uwp/api/windows.ui.xaml.application.resuming)

Découvrez à quel endroit vous devez actualiser votre interface utilisateur lorsque le système reprend l’exécution de votre application. L’exemple présenté dans cette rubrique enregistre un gestionnaire d’événements pour l’événement [**Resuming**](/uwp/api/windows.ui.xaml.application.resuming).

## <a name="register-the-resuming-event-handler"></a>Enregistrer le gestionnaire d’événement de reprise

Enregistrez-vous pour gérer l’événement [**Resuming**](/uwp/api/windows.ui.xaml.application.resuming), qui indique que l’utilisateur revient vers votre application après s’en être éloigné.

```csharp
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```

```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>Actualiser le contenu affiché et récupérer les ressources

Le système suspend votre application quelques secondes après que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système restaure l’application là où elle s’était arrêtée. Pour l’utilisateur, c’est comme si l’application était exécutée en arrière-plan.

Lorsque votre application gère l’événement [**Resuming**](/uwp/api/windows.ui.xaml.application.resuming), votre application peut être suspendue pendant plusieurs heures ou jours. Elle doit alors actualiser le contenu ayant pu devenir obsolète pendant l’interruption de l’application, tel que les flux d’actualités ou l’emplacement de l’utilisateur.

C’est également le moment idéal pour restaurer toutes les ressources exclusives libérées lors de la suspension de votre application, telles que les descripteurs de fichiers, les appareils photo, les périphériques d’E/S, les périphériques externes et les ressources réseau.

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> Étant donné que l’événement de [**reprise**](/uwp/api/windows.ui.xaml.application.resuming) n’est pas déclenché à partir du thread d’interface utilisateur, un répartiteur doit être utilisé dans votre gestionnaire pour distribuer les appels à votre interface utilisateur.

## <a name="remarks"></a>Remarques

Une application jointe au débogueur Visual Studio ne sera pas suspendue. Toutefois, vous pouvez la suspendre à partir du débogueur et lui envoyer un événement **Resume** afin de déboguer votre code. Assurez-vous que la **barre d’outils Emplacement de débogage** est visible et cliquez sur la liste déroulante à côté de l’icône **Suspendre**. Ensuite, sélectionnez **Reprendre**.

Pour les applications Windows Phone Store, l’événement de [**reprise**](/uwp/api/windows.ui.xaml.application.resuming) est toujours suivi par [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched), même lorsque votre application est actuellement suspendue et que l’utilisateur relance votre application à partir d’une liste de vignettes ou d’applications principales. Les applications peuvent ignorer l’initialisation si un contenu est déjà défini sur la fenêtre active. Vous pouvez vérifier la propriété [**LaunchActivatedEventArgs.TileId**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.tileid) pour déterminer si l’application a été lancée à partir d’une vignette principale ou secondaire et, en fonction de l’information obtenue, décider si vous devez présenter une expérience de nouvelle exécution ou de reprise d’exécution de l’application.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la suspension d’une application](suspend-an-app.md)