---
description: Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de WinUI dans un projet C++/WinRT.
title: Exemple de bibliothèque d’IU Windows C++/WinRT simple
ms.date: 07/12/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, Windows UI Library, WinUI
ms.localizationpriority: medium
ms.openlocfilehash: 8242055e3c448e2720226859f2ea10e1ae54794f
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437133"
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

## <a name="edit-pchh-as-necessary"></a>Modifier pch.h, si nécessaire

Lorsque vous ajoutez un package NuGet à un projet C++/WinRT (tel que le package **Microsoft.UI.Xaml**, que vous avez précédemment ajouté) et créer le projet, les outils génèrent un ensemble de fichiers d’en-tête de projection dans le dossier `\Generated Files\winrt` de votre projet. Si vous avez suivi la procédure pas à pas, vous disposez maintenant d’un dossier `\HelloWinUICppWinRT\HelloWinUICppWinRT\Generated Files\winrt`. Pour placer ces fichiers d’en-têtes dans votre projet et permettre la résolution des références vers ces nouveaux types, vous pouvez les inclure dans votre fichier d’en-têtes précompilé (généralement `pch.h`).

Vous devez inclure uniquement les en-têtes qui correspondent aux types que vous utilisez. Mais voici un exemple qui contient tous les fichiers d’en-têtes générés pour le package **Microsoft.UI.Xaml**.

```cppwinrt
// pch.h
...
#include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
#include "winrt/Microsoft.UI.Xaml.Controls.h"
#include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
#include "winrt/Microsoft.UI.Xaml.Media.h"
#include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
...
```

## <a name="edit-mainpagecpp"></a>Modifier MainPage.cpp

Dans `MainPage.cpp`, supprimez le code à l’intérieur de votre implémentation de **MainPage::ClickHandler**, car le balisage XAML ne contient plus *myButton*.

Vous pouvez à présent lancer le processus de génération et exécuter le projet.

![Capture d’écran de l’exemple de bibliothèque d’IU Windows C++/WinRT simple](images/winui.png)

## <a name="related-topics"></a>Rubriques connexes
* [Prise en main de la bibliothèque d’IU Windows](/uwp/toolkits/winui/getting-started)