---
title: Créer une application Android simple avec Xamarin. Android
description: Comment commencer à écrire des applications Android avec Xamarin. Android
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Android, didacticiel, XAML
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255204"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Prise en main du développement pour Android à l’aide de Xamarin. Android

Ce guide vous aidera à vous familiariser avec Xamarin. Android sur Windows pour créer une application multiplateforme qui fonctionnera sur les appareils Android.

Dans cet article, vous allez créer une application Android simple à l’aide de Xamarin. Android et de Visual Studio 2019.

## <a name="requirements"></a>Spécifications

Pour utiliser ce didacticiel, vous avez besoin des éléments suivants :

- Windows 10
- [Visual Studio 2019 : Community, Professional ou Enterprise](https://visualstudio.microsoft.com/downloads/) (voir la remarque)
- La charge de travail « développement mobile avec .NET » pour Visual Studio 2019

> [!NOTE]
> Ce guide fonctionne avec Visual Studio 2017 ou 2019. Si vous utilisez Visual Studio 2017, certaines instructions peuvent être incorrectes en raison des différences de l’interface utilisateur entre les deux versions de Visual Studio.

Vous devez également avoir un téléphone Android ou un émulateur configuré dans lequel exécuter votre application. Consultez [configuration d’un émulateur Android](emulator.md).

## <a name="create-a-new-xamarinandroid-project"></a>Créer un projet Xamarin.Android

Démarrez Visual Studio. Sélectionnez fichier > nouveau > projet pour créer un nouveau projet.

Dans la boîte de dialogue Nouveau projet, sélectionnez le modèle **application Android (Xamarin)** , puis cliquez sur **suivant**.

Nommez le projet **TimeChangerAndroid** , puis cliquez sur **créer**.

Dans la boîte de dialogue nouvelle application multiplateforme, sélectionnez **application vide**. Dans la **version minimale d’Android**, sélectionnez **Android 5,0 (lollipop)**. Cliquez sur **OK**.

Xamarin crée une solution avec un projet unique nommé **TimeChangerAndroid**.

## <a name="create-a-ui-with-xaml"></a>Créer une interface utilisateur avec XAML

Dans le répertoire **Resources\layout** de votre projet, ouvrez **activity_main. xml**. Le code XML de ce fichier définit le premier écran qu’un utilisateur verra lors de l’ouverture de TimeChanger.

L’interface utilisateur de TimeChanger est simple. Il affiche l’heure actuelle et contient des boutons permettant d’ajuster l’heure par incréments d’une heure. Elle utilise un vertical `LinearLayout` pour aligner le temps au-dessus des boutons et `LinearLayout` un horizontal pour réorganiser les boutons côte à côte. Le contenu est centré dans l’écran en affectant à l’attribut **Android : Gravity** la `LinearLayout`valeur **Center** dans la verticale.

Remplacez le contenu de **activity_main. xml** par le code suivant.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

À ce stade, vous pouvez exécuter **TimeChangerAndroid** et afficher l’interface utilisateur que vous avez créée. Dans la section suivante, vous allez ajouter des fonctionnalités à votre interface utilisateur pour afficher l’heure actuelle et activer les boutons pour effectuer une action.

## <a name="add-logic-code-with-c"></a>Ajouter du code logique avec C #

Ouvrez **MainActivity.cs**. Ce fichier contient la logique de code-behind qui ajoutera des fonctionnalités à l’interface utilisateur.

### <a name="set-the-current-time"></a>Définir l’heure actuelle

Tout d’abord, vous pouvez obtenir `TextView` une référence au qui affichera l’heure. Utilisez **FindViewById** pour rechercher dans tous les éléments d’interface utilisateur avec l' **ID Android : ID** (qui a été `"@+id/timeDisplay"` défini à dans le code XML de l’étape précédente). Il s’agit `TextView` du qui affichera l’heure actuelle.

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Les contrôles d’interface utilisateur doivent être mis à jour sur le thread d’interface utilisateur. Les modifications apportées à partir d’un autre thread peuvent ne pas mettre correctement à jour le contrôle tel qu’il s’affiche à l’écran. Étant donné qu’il n’y a aucune garantie, ce code s’exécutera toujours sur le thread d’interface utilisateur, utilisez la méthode **RunOnUiThread** pour vous assurer que les mises à jour s’affichent correctement. Voici la méthode complète `UpdateTimeLabel` .

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>Mettre à jour l’heure actuelle une fois toutes les secondes

À ce stade, l’heure actuelle sera exacte pour, au plus une seconde après le lancement de TimeChangerAndroid. L’étiquette doit être mise à jour régulièrement pour conserver l’heure exacte. Un objet **minuteur** appellera périodiquement une méthode de rappel qui met à jour l’étiquette avec l’heure actuelle.

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>Créer les gestionnaires d’événements de clic sur le bouton

Tout ce qu’il faut faire est d’incrémenter ou de décrémenter la propriété HourOffset et d’appeler UpdateTimeLabel.

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>Relier les boutons monter et descendre à leurs gestionnaires d’événements correspondants

Pour associer les boutons à leurs gestionnaires d’événements correspondants, utilisez d’abord FindViewById pour rechercher les boutons par leur ID. Une fois que vous avez une référence à l’objet bouton, vous pouvez ajouter un gestionnaire d' `Click` événements à son événement.

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>Fichier MainActivity.cs terminé

Lorsque vous avez terminé, MainActivity.cs doit ressembler à ceci :

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>Exécutez l'application.

Pour exécuter l’application, appuyez sur **F5** ou cliquez sur déboguer > démarrer le débogage. Selon la façon dont votre [débogueur est configuré](emulator.md), votre application est lancée sur un appareil ou dans un émulateur.

## <a name="related-links"></a>Liens connexes

- [Testez sur un appareil ou un émulateur Android](emulator.md).
- [Créer un exemple d’application Android à l’aide de Xamarin. Forms](xamarin-forms.md)
