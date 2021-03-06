---
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: Utiliser le capteur d’orientation
description: Découvrez comment utiliser les capteurs d’orientation pour déterminer l’orientation de l’appareil.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0217567fd2b78542b745a02fbdfa3bd816d9a2b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159453"
---
# <a name="use-the-orientation-sensor"></a>Utiliser le capteur d’orientation


**API importantes**

-   [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
-   [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor)
-   [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation)

**Exemples**

-   [Exemple de capteur d’orientation](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [Exemple de capteur d’orientation simple](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

Découvrez comment utiliser les capteurs d’orientation pour déterminer l’orientation de l’appareil.

Il existe deux types différents d’API de capteur d’orientation inclus dans l’espace de noms [**Windows. Devices. Sensors**](/uwp/api/Windows.Devices.Sensors) : [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) et [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation). Même si tous deux sont des capteurs d’orientation, ils sont utilisés à des fins très différentes. Toutefois, étant donné que les deux sont des capteurs d’orientation, ils sont traités tous les deux dans cet article.

L’API [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) est utilisée pour les applications 3D, deux obtiennent un Quaternion et une matrice de rotation. Un Quaternion peut être plus facilement interprété comme une rotation des points \[ x, y, z \] sur un axe arbitraire (avec une matrice de rotation qui représente les rotations autour de trois axes). Les quaternions s’appuient sur des mathématiques quelque peu exotiques en ce sens qu’elles impliquent les propriétés géométriques des nombres complexes et les propriétés mathématiques des nombres imaginaires. Toutefois, leur utilisation est simple, et les infrastructures telles que DirectX les prennent en charge. Une application en 3D peut utiliser le capteur d’orientation pour régler la perspective de l’utilisateur. Ce capteur associe les entrées de l’accéléromètre, du gyromètre et de la boussole.

L’API [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) permet de déterminer l’orientation de l’appareil en termes de définition comme portrait vers le haut, portrait vers le bas, paysage à gauche, paysage à droite. Il peut également détecter si un appareil est face vers le haut ou face vers le bas. Au lieu de renvoyer des propriétés telles que « Portrait vers le haut » ou « Paysage à gauche », ce capteur renvoie une valeur de rotation : « Sans rotation », « Rotation90DegrésSensAntiHoraire », etc. Le tableau suivant établit une correspondance entre les propriétés d’orientation courantes et les lectures du capteur.

| Orientation     | Lecture du capteur correspondante      |
|-----------------|-----------------------------------|
| Portrait vers le haut     | SansRotation                        |
| Paysage à gauche  | Rotation90DegrésSensAntiHoraire  |
| Portrait vers le bas   | Rotation180DegrésSensAntiHoraire |
| Paysage à droite | Rotation270DegrésSensAntiHoraire |

## <a name="prerequisites"></a>Prérequis

Vous devez maîtriser le langage XAML (Extensible Application Markup Language), Microsoft Visual C# et les événements.

L’appareil ou émulateur que vous utilisez doit prendre en charge un capteur d’orientation.

## <a name="create-an-orientationsensor-app"></a>Créer une application de capteur d’orientation

Cette section se divise en deux sous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application d’orientation. La sous-section suivante décrit l’application que vous venez de créer.

###  <a name="instructions"></a>Instructions

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private OrientationSensor _sensor;

            private async void ReadingChanged(object sender, OrientationSensorReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    OrientationSensorReading reading = e.Reading;

                    // Quaternion values
                    txtQuaternionX.Text = String.Format("{0,8:0.00000}", reading.Quaternion.X);
                    txtQuaternionY.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Y);
                    txtQuaternionZ.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Z);
                    txtQuaternionW.Text = String.Format("{0,8:0.00000}", reading.Quaternion.W);

                    // Rotation Matrix values
                    txtM11.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M11);
                    txtM12.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M12);
                    txtM13.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M13);
                    txtM21.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M21);
                    txtM22.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M22);
                    txtM23.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M23);
                    txtM31.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M31);
                    txtM32.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M32);
                    txtM33.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M33);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _sensor = OrientationSensor.GetDefault();

                // Establish the report interval for all scenarios
                uint minReportInterval = _sensor.MinimumReportInterval;
                uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                _sensor.ReportInterval = reportInterval;

                // Establish event handler
                _sensor.ReadingChanged += new TypedEventHandler<OrientationSensor, OrientationSensorReadingChangedEventArgs>(ReadingChanged);
            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **OrientationSensorCS**, vous devez remplacer `namespace App1` par `namespace OrientationSensorCS`.

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

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,4,0,0" TextWrapping="Wrap" Text="M11:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,36,0,0" TextWrapping="Wrap" Text="M12:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,72,0,0" TextWrapping="Wrap" Text="M13:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="31" Margin="4,118,0,0" TextWrapping="Wrap" Text="M21:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,160,0,0" TextWrapping="Wrap" Text="M22:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,201,0,0" TextWrapping="Wrap" Text="M23:" VerticalAlignment="Top" Width="35"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,234,0,0" TextWrapping="Wrap" Text="M31:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,274,0,0" TextWrapping="Wrap" Text="M32:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="4,322,0,0" TextWrapping="Wrap" Text="M33:" VerticalAlignment="Top" Width="39"/>
            <TextBlock x:Name="txtM11" HorizontalAlignment="Left" Height="19" Margin="43,4,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM12" HorizontalAlignment="Left" Height="23" Margin="43,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM13" HorizontalAlignment="Left" Height="15" Margin="43,72,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM21" HorizontalAlignment="Left" Height="20" Margin="43,114,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM22" HorizontalAlignment="Left" Height="19" Margin="43,156,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM23" HorizontalAlignment="Left" Height="16" Margin="43,197,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM31" HorizontalAlignment="Left" Height="17" Margin="43,230,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM32" HorizontalAlignment="Left" Height="19" Margin="43,270,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM33" HorizontalAlignment="Left" Height="21" Margin="43,322,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,8,0,0" TextWrapping="Wrap" Text="Quaternion X:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="194,36,0,0" TextWrapping="Wrap" Text="Quaternion Y:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,72,0,0" TextWrapping="Wrap" Text="Quaternion Z:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionX" HorizontalAlignment="Left" Height="15" Margin="279,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="104"/>
            <TextBlock x:Name="txtQuaternionY" HorizontalAlignment="Left" Height="12" Margin="275,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="108"/>
            <TextBlock x:Name="txtQuaternionZ" HorizontalAlignment="Left" Height="19" Margin="275,68,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="89"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="194,96,0,0" TextWrapping="Wrap" Text="Quaternion W:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionW" HorizontalAlignment="Left" Height="12" Margin="279,96,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="72"/>

        </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **OrientationSensorCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="OrientationSensorCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:OrientationSensorCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer**  >  **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier l’orientation en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Arrêtez l’application en retournant dans Visual Studio et en appuyant sur Maj + F5 ou sélectionnez **Déboguer**  >  **arrêter le débogage** pour arrêter l’application.

###  <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée du capteur d’orientation dans votre application.

L’application établit une connexion avec le capteur d’orientation par défaut dans la méthode **MainPage**.

```csharp
_sensor = OrientationSensor.GetDefault();
```

L’application établit l’intervalle de rapport dans la méthode **MainPage**. Le code suivant récupère l’intervalle minimal pris en charge par l’appareil et le compare à un intervalle demandé de 16 millisecondes (ce qui représente une fréquence de rafraîchissement de 60 Hz). Si l’intervalle pris en charge minimum est supérieur à l’intervalle demandé, le code définit la valeur sur l’intervalle minimum. Sinon, il définit la valeur sur l’intervalle demandé.

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

Les nouvelles données du capteur sont capturées dans la méthode **ReadingChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

Ces nouvelles valeurs sont écrites dans les TextBlocks identifiés dans le code XAML du projet.

## <a name="create-a-simpleorientation-app"></a>Créer une application d’orientation simple

Cette section se divise en deux sous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application d’orientation simple. La sous-section suivante décrit l’application que vous venez de créer.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private SimpleOrientationSensor _simpleorientation;

            // This event handler writes the current sensor reading to
            // a text block on the app' s main page.

            private async void OrientationChanged(object sender, SimpleOrientationSensorOrientationChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    SimpleOrientation orientation = e.Orientation;
                    switch (orientation)
                    {
                        case SimpleOrientation.NotRotated:
                            txtOrientation.Text = "Not Rotated";
                            break;
                        case SimpleOrientation.Rotated90DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 90 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated180DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 180 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated270DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 270 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Faceup:
                            txtOrientation.Text = "Faceup";
                            break;
                        case SimpleOrientation.Facedown:
                            txtOrientation.Text = "Facedown";
                            break;
                        default:
                            txtOrientation.Text = "Unknown orientation";
                            break;
                    }
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _simpleorientation = SimpleOrientationSensor.GetDefault();

                // Assign an event handler for the sensor orientation-changed event
                if (_simpleorientation != null)
                {
                    _simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor, SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
                }
            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **SimpleOrientationCS**, vous devez remplacer `namespace App1` par `namespace SimpleOrientationCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **SimpleOrientationCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="SimpleOrientationCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:SimpleOrientationCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer**  >  **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier l’orientation en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Arrêtez l’application en retournant dans Visual Studio et en appuyant sur Maj + F5 ou sélectionnez **Déboguer**  >  **arrêter le débogage** pour arrêter l’application.

### <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée du capteur d’orientation simple dans votre application.

L’application établit une connexion avec le capteur par défaut dans la méthode **MainPage**.

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

Les nouvelles données du capteur sont capturées dans la méthode **OrientationChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

Ces nouvelles valeurs sont écrites dans un TextBlock identifié dans le code XAML du projet.

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```