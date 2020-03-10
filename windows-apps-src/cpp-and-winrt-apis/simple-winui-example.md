---
description: Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de WinUI dans un projet C++/WinRT.
title: Exemple de bibliothèque d’IU Windows C++/WinRT simple
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, Windows UI Library, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: aadf177bc4a44f67550dba1f6f706525b8460857
ms.sourcegitcommit: c9bab19599c0eb2906725fd86d0696468bb919fa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256172"
---
# <a name="a-simple-cwinrt-windows-ui-library-example"></a>Exemple de bibliothèque d’IU Windows C++/WinRT simple

Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de la [bibliothèque d’IU Windows (WinUI)](https://github.com/Microsoft/microsoft-ui-xaml) dans votre projet C++/WinRT. Par ailleurs, la bibliothèque d'IU Windows est elle-même écrite en C++/WinRT.

> [!NOTE]
> Le kit d’outils de la bibliothèque d’IU Windows (WinUI) est disponible sous forme de packages NuGet que vous pouvez ajouter à un projet existant ou nouveau à l’aide de Visual Studio, comme nous le verrons dans cette rubrique. Pour plus d’informations sur l’arrière-plan, la configuration et la prise en charge, consultez [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started).

## <a name="create-a-blank-app-hellowinuicppwinrt"></a>Créer une application vide (HelloWinUICppWinRT)

Dans Visual Studio, créez un projet à l’aide du modèle de projet **Blank App (C++/WinRT)** . Veillez à utiliser le modèle **(C++/WinRT)** , et non le modèle **(Windows universel)** .

Définissez le nom du nouveau projet avec *HelloWinUICppWinRT* et (pour que votre structure de dossiers corresponde à la procédure pas à pas) décochez **Placer la solution et le projet dans le même répertoire**.

## <a name="install-the-microsoftuixaml-nuget-package"></a>Installer le package NuGet Microsoft.UI.Xaml

Cliquez sur **Projet** \> **Gérer des packages NuGet...** \> **Parcourir**, tapez ou collez **Microsoft.UI.Xaml** dans la zone de recherche, sélectionnez l’élément dans les résultats de la recherche, puis cliquez sur **Installer** pour installer le package correspondant à votre projet (une invite Contrat de licence s’affichera également). Veillez à installer uniquement le package **Microsoft.UI.Xaml** et non **Microsoft.UI.Xaml.Core.Direct**.

## <a name="declare-winui-application-resources"></a>Déclarer des ressources d’application WinUI

Ouvrez `App.xaml` et collez la marque suivante entre les balises **Application** d’ouverture et de fermeture existantes.

```xaml
<Application.Resources>
    <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
</Application.Resources>
```

## <a name="add-a-winui-control-to-mainpage"></a>Ajouter un contrôle WinUI à MainPage

Ensuite, ouvrez `MainPage.xaml`. Dans la balise **Page** d’ouverture existante, il existe des déclarations d’espace de noms XML. Ajoutez la déclaration d’espace de noms XML `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`. Collez ensuite la marque suivante entre les balises **Page** d’ouverture et de fermeture existantes en remplaçant l’élément **StackPanel** existant.

```xaml
<muxc:NavigationView PaneTitle="Welcome">
    <TextBlock Text="Hello, World!" VerticalAlignment="Center" HorizontalAlignment="Center" Style="{StaticResource TitleTextBlockStyle}"/>
</muxc:NavigationView>
```

## <a name="edit-mainpagecpp-and-h-as-necessary"></a>Modifiez MainPage.cpp et .h selon vos besoins

Dans `MainPage.cpp`, supprimez le code à l’intérieur de votre implémentation de **MainPage::ClickHandler**, car le balisage XAML ne contient plus *myButton*.

Dans `MainPage.h`, modifiez vos fichiers inclus afin qu’ils ressemblent à ceux de la liste ci-dessous. Si vous utilisez WinUI à partir de plusieurs pages XAML, vous pouvez accéder à votre fichier d’en-tête précompilé (en général `pch.h`) et les y inclure.

```cppwinrt
#include "MainPage.g.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
```

Maintenant, créez le projet.

Lorsque vous ajoutez un package NuGet à un projet C++/WinRT (tel que le package **Microsoft.UI.Xaml**, que vous avez précédemment ajouté) et créer le projet, les outils génèrent un ensemble de fichiers d’en-tête de projection dans le dossier `\Generated Files\winrt` de votre projet. Si vous avez suivi la procédure pas à pas, vous disposez maintenant d’un dossier `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. Le changement que vous avez apporté à `MainPage.h` ci-dessus permet d’inclure ces fichiers d’en-tête de projection dans votre projet. Et c’est nécessaire pour que les références aux types dans le package NuGet soient résolues.

Vous pouvez à présent exécuter le projet.

![Capture d’écran de l’exemple de bibliothèque d’IU Windows C++/WinRT simple](images/winui.png)

## <a name="related-topics"></a>Rubriques connexes
* [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started)