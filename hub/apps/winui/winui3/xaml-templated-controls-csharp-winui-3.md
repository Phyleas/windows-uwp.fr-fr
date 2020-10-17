---
description: Cet article vous guide dans la création d’un contrôle XAML basé sur un modèle pour WinUI 3 avec C#.
title: Contrôles XAML basés sur un modèle pour les applications WinUI 3 avec C#
ms.date: 09/11/2020
ms.topic: article
keywords: windows 10, uwp, contrôle personnalisé, contrôle basé sur un modèle, winui
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 618bfc5a9937d29c546dc9420a1cba1c25fcc0ea
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061697"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>Contrôles XAML basés sur un modèle pour les applications WinUI 3 avec C#

Cet article vous guide dans la création d’un contrôle XAML basé sur un modèle pour WinUI 3 avec C#. Les contrôles basés sur un modèle héritent de **Microsoft.UI.Xaml.Controls.Control**, et ont une structure visuelle et un comportement visuel qui peuvent être personnalisés à l’aide de modèles de contrôle XAML.

Avant de suivre les étapes décrites dans cet article, vous devez vous assurer que votre environnement de développement est configuré pour créer des applications WinUI 3. Pour obtenir des informations sur la configuration, consultez [Bien démarrer avec WinUI 3 pour les applications de bureau](./get-started-winui3-for-desktop.md).

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Dans la boîte de dialogue **Créer un projet**, sélectionnez le modèle de projet **Application vide (WinUI dans UWP)** , en veillant à sélectionner la version de langage C#. Définissez le nom du projet sur « BgLabelControlApp » afin que les noms de fichiers correspondent au code dans les exemples ci-dessous. Définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**. Cette procédure pas à pas fonctionne également pour les applications de bureau créées avec le modèle de projet **Application vide, empaquetée (WinUI dans les applications de bureau)** , veillez simplement à effectuer toutes les étapes du projet **BgLabelControlApp (bureau)** .

![Modèle de projet d’application vide](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>Ajouter un contrôle basé sur un modèle à votre application

Pour ajouter un contrôle basé sur un modèle, cliquez sur le menu **Projet** dans la barre d’outils ou cliquez avec le bouton droit sur votre projet dans l’**Explorateur de solutions** et sélectionnez **Ajouter un nouvel élément**. Sous **Visual C#->WinUI**, sélectionnez le modèle **Contrôle personnalisé (WinUI)** . Nommez le nouveau contrôle « BgLabelControl », puis cliquez sur *Ajouter*. Cette opération ajoute deux nouveaux fichiers à votre projet. `BgLabelControl.cs` contient le code-behind pour le contrôle. 

## <a name="update-the-code-behind-file"></a>Mettre à jour le fichier code-behind

Dans le fichier code-behind BgLabelControl.xaml.cs, vous voyez que le constructeur définit la propriété **DefaultStyleKey** pour notre contrôle. Cette clé identifie le modèle par défaut qui sera utilisé si le consommateur du contrôle ne spécifie pas explicitement un modèle. La valeur de la clé est le *type* du contrôle. Vous verrez comment cette clé est utilisée ultérieurement, quand nous implémenterons notre fichier de modèle générique.

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

Le contrôle basé sur un modèle aura une étiquette de texte définissable programmatiquement dans le code, en XAML, ou via une liaison de données. Pour que le système tienne à jour le texte de l’étiquette du contrôle, le contrôle doit être implémenté en tant que [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty). Pour ce faire, nous déclarons d’abord une propriété de type chaîne, que nous appelons **Label**. Au lieu d’utiliser une variable de stockage, nous définissons et obtenons la valeur de la propriété de dépendance en appelant [GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) et [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue). Ces méthodes sont fournies par [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject), dont **Microsoft.UI.Xaml.Controls.Control** hérite.

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
Ensuite, nous déclarons la propriété de dépendance et nous l’inscrivons auprès du système en appelant [DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register). Cette méthode spécifie le nom et le type de la propriété **Label**, le type du propriétaire de la propriété, la classe **BgLabelControl** et la valeur par défaut de la propriété.

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string)));
```

Ces deux étapes suffisent pour implémenter une propriété de dépendance, mais dans cet exemple, nous allons ajouter un gestionnaire facultatif pour l’événement **OnLabelChanged**. Cet événement est déclenché par le système dès que la valeur de la propriété est mise à jour. Dans ce cas, nous vérifions si le nouveau texte de l’étiquette est une chaîne vide ou non, et nous mettons à jour une variable de classe en conséquence.

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    {
        BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
        String s = e.NewValue as String; //null checks omitted
        if (s == String.Empty)
        {
            labelControl.HasLabelValue = false;
        }
        else
        {
            labelControl.HasLabelValue = true;
        }
    }
}
```
Pour plus d’informations sur le fonctionnement des propriétés de dépendance, consultez [Vue d’ensemble des propriétés de dépendance](/windows/uwp/xaml-platform/dependency-properties-overview).

## <a name="define-the-default-style-for-bglabelcontrol"></a>Définir le style par défaut pour BgLabelControl
Un contrôle basé sur un modèle doit fournir un modèle de style par défaut qui est utilisé si le consommateur du contrôle ne définit pas explicitement un style. Dans cette étape, nous allons créer un fichier de modèle générique pour le contrôle.

Vérifiez que l’option **Afficher tous les fichiers** est activée dans l’**Explorateur de solutions**. Sous le nœud de votre projet, créez un dossier et nommez-le « Themes ». Sous `Themes`, ajoutez un nouvel élément de type **Visual C# > WinUI > Dictionnaire de ressources (WinUI)** , puis nommez-le « Generic.xaml ». Les noms de dossier et de fichier doivent être ainsi pour que le framework XAML recherche le style par défaut d’un contrôle basé sur un modèle. Supprimez le contenu par défaut de Generic.xaml, puis collez le balisage ci-dessous.



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

Dans cet exemple, vous voyez que l’attribut **TargetType** de l’élément **Style** est défini sur le type **BgLabelControl** dans l’espace de noms **BgLabelControlApp**. Ce type est la même valeur que celle que nous avons spécifiée plus haut pour la propriété **DefaultStyleKey** dans le constructeur du contrôle qui l’identifie comme style par défaut du contrôle.

La propriété **Text** de **TextBlock** dans le modèle du contrôle est liée à la propriété de dépendance **Label** de notre contrôle. La propriété est liée à l’aide de l’extension de balisage [TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension). Cet exemple lie également l’arrière-plan **Grid** à la propriété de dépendance **Background** qui est héritée de la classe **Control**.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de BgLabelControl à la page d’interface utilisateur principale

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après l’élément **Button** (dans **StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Générez et exécutez l’application, et vous verrez le contrôle basé sur un modèle, avec la couleur d’arrière-plan et l’étiquette que nous avons spécifiés.

![Résultat du contrôle basé sur un modèle](images/winui-templated-control-result.png)


