---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 0b5250f1e01aece4f73d83dc7327f193a58f53cf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161493"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>Partie 2 : Ajouter un contrôle InkCanvas UWP à l’aide d'îles XAML

Il s’agit de la deuxième partie d’un tutoriel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso Expenses. Pour obtenir une vue d’ensemble du tutoriel, des conditions préalables et des instructions pour le téléchargement de l’exemple d’application, consultez [Tutoriel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article part du principe que vous avez terminé la [partie 1](modernize-wpf-tutorial-1.md).

Dans le scénario fictif de ce tutoriel, l’équipe de développement de Contoso souhaite ajouter la prise en charge des signatures numériques à l’application Contoso Expenses. Le contrôle UWP **InkCanvas** est une option intéressante pour ce scénario, car il prend en charge les fonctionnalités d’encre numérique et celles basées sur l’intelligence artificielle, telles que la possibilité de reconnaître du texte et des formes. Pour ce faire, vous allez utiliser le contrôle [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP inclus dans un wrapper fourni avec le kit de ressources de la communauté Windows. Ce contrôle inclut l’interface et la fonctionnalité **InkCanvas** UWP dans un wrapper à utiliser dans une application WPF. Pour plus d’informations sur les contrôles UWP inclus dans un wrapper, consultez [Héberger des contrôles XAML UWP dans les applications de bureau (XAML Islands)](xaml-islands.md).

## <a name="configure-the-project-to-use-xaml-islands"></a>Configurer le projet pour utiliser XAML Islands

Avant de pouvoir ajouter un contrôle **InkCanvas** à l’application Contoso Expenses, vous devez d’abord configurer le projet pour prendre en charge XAML Islands UWP.

1. Dans Visual Studio 2019, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** dans l’**Explorateur de solutions**, puis choisissez **Gérer les packages NuGet**.

    ![Menu Gérer les packages NuGet dans Visual Studio](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**. Recherchez le package `Microsoft.Toolkit.Wpf.UI.Controls` et installez la version 6.0.0 ou une version ultérieure.

    > [!NOTE]
    > Ce package contient toute l’infrastructure nécessaire pour l’hébergement de XAML Islands UWP dans une application WPF, notamment le contrôle **InkCanvas** UWP inclus dans un wrapper. Un package similaire nommé `Microsoft.Toolkit.Forms.UI.Controls` est disponible pour les applications Windows Forms.

3. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** dans l’**Explorateur de solutions** et choisissez**Ajouter > Nouvel élément**.

4. Sélectionnez le **Fichier manifeste de l’application**, nommez-le **app.manifest**, puis cliquez sur **Ajouter**. Pour plus d’informations sur les manifestes d’application, consultez [cet article](/windows/desktop/SbsCs/application-manifests).

5. Dans le fichier manifeste, supprimez les marques de commentaire de l’élément `<supportedOS>` suivant pour Windows 10.

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. Dans le fichier manifeste, localisez l’élément `<application>` commenté suivant.

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. Supprimez cette section et remplacez-la par le code XML suivant. Cela permet de configurer l’application pour qu’elle prenne en charge DPI et de mieux gérer différents facteurs de mise à l’échelle pris en charge par Windows 10.

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. Enregistrez et fermez le fichier `app.manifest`.

9. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoExpenses.Core** et choisissez **Propriétés**.

10. Dans la section **Ressources** de l’onglet **Application**, vérifiez que la liste déroulante **Manifeste** est définie sur **app.manifest**.

    ![Manifeste de l’application .NET Core](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. Enregistrez les modifications apportées aux propriétés du projet.

## <a name="add-an-inkcanvas-control-to-the-app"></a>Ajouter un contrôle InkCanvas à l’application

Votre projet étant configuré pour utiliser des XAML Islands UWP, vous pouvez maintenant ajouter un contrôle [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP inclus dans un wrapper à l’application.

1. Dans **Explorateur de solutions**, développez le dossier **Affichages** du projet **ContosoExpenses.Core**, puis double-cliquez sur le fichier **ExpenseDetail.xaml**.

2. Dans l’élément **Window** au début du fichier XAML, ajoutez l’attribut suivant. Ce dernier référence l’espace de noms XAML pour le contrôle [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) UWP inclus dans un wrapper.

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Une fois cet attribut ajouté, l’élément **Window** doit ressembler à ce qui suit.

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. Dans le fichier **ExpenseDetail.xaml**, localisez la balise `</Grid>` de fermeture qui précède immédiatement le commentaire `<!-- Chart -->`. Ajoutez le code XAML suivant juste avant la balise de fermeture `</Grid>`. Ce code XAML ajoute un contrôle **InkCanvas** (préfixé par le mot clé **kit d’outil** que vous avez défini précédemment en tant qu’espace de noms) et un simple **TextBlock** qui agit comme un en-tête pour le contrôle.

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. Enregistrez le fichier **ExpenseDetail.xaml**.

6. Appuyez sur la touche F5 pour créer et exécuter l’application dans le débogueur.

7. Choisissez un employé dans la liste, puis choisissez l’une des dépenses disponibles. Notez que la page des détails des dépenses comporte de l’espace pour le contrôle **InkCanvas**.

    ![Stylet d’entrée manuscrits uniquement](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    Si vous avez un appareil qui prend en charge un stylet numérique, comme un Surface, et que vous suivez ce tutoriel sur un ordinateur physique, continuez et essayez de l’utiliser. L’encre numérique s’affiche à l’écran. Toutefois, si vous n’avez pas d’appareil avec stylet et que vous essayez de signer avec la souris, rien ne se passe. En effet, le contrôle **InkCanvas** est activé uniquement pour les stylets numériques par défaut. Cependant, nous pouvons changer ce comportement.

8. Fermez l’application et double-cliquez sur le fichier **ExpenseDetail.xaml.cs** sous le dossier **Affichages** du projet **ContosoExpenses.Core**.

9. Ajoutez la déclaration d’espace de noms suivante au début du fichier :

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. Recherchez le constructeur `ExpenseDetail()`.

11. Ajoutez la ligne de code suivante après la méthode `InitializeComponent()` et enregistrez le fichier de code.

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Vous pouvez utiliser l’objet **InkPresenter** pour personnaliser l’expérience d’entrée manuscrite par défaut. Ce code utilise la propriété **InputDeviceTypes** pour permettre l’entrée avec la souris comme avec le stylet.

12. Appuyez sur F5 pour regénérer et exécuter l’application dans le débogueur. Choisissez un employé dans la liste, puis choisissez l’une des dépenses disponibles.

13. Essayez maintenant de dessiner quelque chose dans l’espace de signature avec la souris. Cette fois, l’encre s’affiche à l’écran.

    ![Signature](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>Étapes suivantes

À ce stade du tutoriel, vous avez ajouté un contrôle **InkCanvas** UWP à l’application Contoso Expenses. Vous êtes maintenant prêt à passer à la [Partie 3 : Ajouter un contrôle CalendarView UWP à avec XAML Islands](modernize-wpf-tutorial-3.md).