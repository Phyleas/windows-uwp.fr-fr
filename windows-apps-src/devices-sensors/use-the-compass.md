---
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: Utiliser le compas
description: Découvrez comment utiliser l’API Compass de plateforme Windows universelle (UWP) pour déterminer l’en-tête actuel dans une application de navigation.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ddd5ddddb31cf93977cb0d5bd9c16e916c4126b
ms.sourcegitcommit: e81227399ba0f286e74e4977d757237829440a2e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96310197"
---
# <a name="use-the-compass"></a>Utiliser le compas


**API importantes**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**Boussole**](/uwp/api/Windows.Devices.Sensors.Compass)

**Exemple**

-   Pour une implémentation plus complète, consultez l' [exemple Compass](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

Découvrez comment utiliser la boussole pour déterminer l’orientation actuelle.

Une application peut récupérer l’orientation actuelle par rapport au nord magnétique ou géographique. Les applications de navigation utilisent la boussole pour déterminer la direction à laquelle un appareil fait face, puis pour orienter la carte de façon appropriée.

## <a name="prerequisites"></a>Prérequis

Vous devez maîtriser le langage XAML (Extensible Application Markup Language), Microsoft Visual C# et les événements.

L’appareil ou émulateur que vous utilisez doit prendre en charge une boussole.

## <a name="create-a-simple-compass-app"></a>Créer une application de boussole simple

Cette section se divise en deux sous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application simple de boussole. La sous-section suivante décrit l’application que vous venez de créer.

### <a name="instructions"></a>Instructions

-   Créez un projet en choisissant une **Application vide (Windows universel)** dans les modèles de projet **Visual C#**.

-   Ouvrez le fichier MainPage.xaml.cs de votre projet et remplacez le code existant par ce qui suit.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **CompassCS**, vous devez remplacer `namespace App1` par `namespace CompassCS`.

-   Ouvrez le fichier MainPage.xaml et remplacez le contenu d’origine par le code XML suivant.

```xml
    <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **CompassCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="CompassCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:CompassCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer**  >  **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier les valeurs de la boussole en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Arrêtez l’application en retournant dans Visual Studio et en appuyant sur Maj + F5 ou sélectionnez **Déboguer**  >  **arrêter le débogage** pour arrêter l’application.

### <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée de la boussole dans votre application.

L’application établit une connexion avec la boussole par défaut dans la méthode **MainPage**.

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

L’application établit l’intervalle de rapport dans la méthode **MainPage**. Le code suivant récupère l’intervalle minimal pris en charge par l’appareil et le compare à un intervalle demandé de 16 millisecondes (ce qui représente une fréquence de rafraîchissement de 60 Hz). Si l’intervalle pris en charge minimum est supérieur à l’intervalle demandé, le code définit la valeur sur l’intervalle minimum. Sinon, il définit la valeur sur l’intervalle demandé.

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

Les nouvelles données de la boussole sont capturées dans la méthode **ReadingChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

Ces nouvelles valeurs sont écrites dans les TextBlocks identifiés dans le code XAML du projet.

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 
