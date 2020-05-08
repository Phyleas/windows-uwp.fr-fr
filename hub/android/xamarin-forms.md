---
title: Créer une application Android simple avec Xamarin. Forms
description: Comment commencer à écrire des applications Android avec Xamarin. Forms
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Forms, XAML, didacticiel
ms.date: 04/28/2020
ms.openlocfilehash: a1426bfef9863227c1ac110bc295536786695df7
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255193"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Prise en main du développement pour Android à l’aide de Xamarin. Forms

Ce guide vous aidera à vous familiariser avec Xamarin. Forms sur Windows pour créer une application multiplateforme qui fonctionnera sur les appareils Android.

Dans cet article, vous allez créer une application Android simple à l’aide de Xamarin. Forms et de Visual Studio 2019.

## <a name="requirements"></a>Spécifications

Pour utiliser ce didacticiel, vous avez besoin des éléments suivants :

- Windows 10
- [Visual Studio 2019 : Community, Professional ou Enterprise](https://visualstudio.microsoft.com/downloads/) (voir la remarque)
- La charge de travail « développement mobile avec .NET » pour Visual Studio 2019

> [!NOTE]
> Ce guide fonctionne avec Visual Studio 2017 ou 2019. Si vous utilisez Visual Studio 2017, certaines instructions peuvent être incorrectes en raison des différences de l’interface utilisateur entre les deux versions de Visual Studio.

Vous devez également avoir un téléphone Android ou un émulateur configuré dans lequel exécuter votre application. Consultez [test sur un appareil ou un émulateur Android](emulator.md).

## <a name="create-a-new-xamarinforms-project"></a>Créer un projet Xamarin. Forms

Démarrez Visual Studio. Cliquez sur fichier > nouveau > projet pour créer un nouveau projet.

Dans la boîte de dialogue Nouveau projet, sélectionnez le modèle **application mobile (Xamarin. Forms)** , puis cliquez sur **suivant**.

Nommez le projet **TimeChangerForms** , puis cliquez sur **créer**.

Dans la boîte de dialogue nouvelle application multiplateforme, sélectionnez **vide**. Dans la section plateforme, cochez **Android** et désactivez toutes les autres zones. Cliquez sur **OK**.

Xamarin crée une nouvelle solution avec deux projets : **TimeChangerForms** et **TimeChangerForms. Android.**

## <a name="create-a-ui-with-xaml"></a>Créer une interface utilisateur avec XAML

Développez le projet **TimeChangerForms** et ouvrez **MainPage. Xaml**. Le code XAML de ce fichier définit le premier écran qu’un utilisateur verra lors de l’ouverture de TimeChanger.

L’interface utilisateur de TimeChanger est simple. Il affiche l’heure actuelle et contient des boutons permettant d’ajuster l’heure par incréments d’une heure. Elle utilise un StackLayout vertical pour aligner le temps au-dessus des boutons et un StackLayout horizontal pour réorganiser les boutons côte à côte. Le contenu est centré sur l’écran en définissant le **HorizontalOptions** et le **VerticalOptions** de l’StackLayout vertical sur **« CenterAndExpand »**.

Remplacez le contenu de MainPage. XAML par le code suivant.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

À ce stade, l’interface utilisateur est terminée. TimeChangerForms, toutefois, n’est pas généré, car les méthodes **UpButton_Clicked** et **DownButton_Clicked** sont référencées dans le XAML, mais ne sont pas définies n’importe où. Même si l’application a été exécutée, l’heure actuelle ne s’affiche pas. Dans la section suivante, vous allez corriger ces erreurs et ajouter des fonctionnalités à votre interface utilisateur.

## <a name="add-logic-code-with-c"></a>Ajouter du code logique avec C #

Dans le Explorateur de solutions, cliquez avec le bouton droit sur MainPage. xaml, puis cliquez sur **afficher le code**. Ce fichier contient le code-behind qui ajoutera des fonctionnalités à l’interface utilisateur.

### <a name="set-the-current-time"></a>Définir l’heure actuelle

Le code de ce fichier peut référencer des contrôles déclarés dans le XAML à l’aide de la valeur de l’attribut **x :Name** du contrôle. Dans ce cas, l’étiquette qui affiche l’heure actuelle est `time`appelée.

Les contrôles d’interface utilisateur doivent être mis à jour sur le thread principal. Les modifications apportées à partir d’un autre thread peuvent ne pas mettre correctement à jour le contrôle tel qu’il s’affiche à l’écran. Étant donné qu’il n’y a aucune garantie, ce code s’exécutera toujours sur le thread principal, utilisez la méthode **BeginInvokeOnMainThread** pour vous assurer que les mises à jour s’affichent correctement. Voici la méthode UpdateTimeLabel complète.

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>Mettre à jour l’heure actuelle une fois toutes les secondes

À ce stade, l’heure actuelle sera exacte pour, au plus une seconde après le lancement de TimeChangerForms. L’étiquette doit être mise à jour régulièrement pour conserver l’heure exacte. Un objet **minuteur** appellera périodiquement une méthode de rappel qui met à jour l’étiquette avec l’heure actuelle.

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>Ajouter HourOffset

Les boutons monter et descendre permettent d’ajuster l’heure par incréments d’une heure. Ajoutez une propriété **HourOffset** pour suivre l’ajustement en cours.

```csharp
public int HourOffset { get; private set; }
```

À présent, mettez à jour la méthode UpdateTimeLabel pour connaître la propriété HourOffset.

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>Ajouter des gestionnaires d’événements de clic sur le bouton

Tout ce qu’il faut faire est d’incrémenter ou de décrémenter la propriété HourOffset et d’appeler UpdateTimeLabel.

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

Lorsque vous avez terminé, MainPage.xaml.cs doit ressembler à ceci :

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>Exécuter l’application

Pour exécuter l’application, appuyez sur **F5** ou cliquez sur déboguer > démarrer le débogage. Selon la façon dont votre [débogueur est configuré](emulator.md), votre application est lancée sur un appareil ou dans un émulateur.

## <a name="related-links"></a>Liens connexes

- [Testez sur un appareil ou un émulateur Android](emulator.md).

- [Créer un exemple d’application Android à l’aide de Xamarin. Android](xamarin-android.md)
