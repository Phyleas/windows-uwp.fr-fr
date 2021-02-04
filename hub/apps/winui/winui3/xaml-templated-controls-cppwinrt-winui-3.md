---
description: Cet article vous guide dans la création d’un contrôle XAML basé sur un modèle pour WinUI 3 avec C++/WinRT.
title: Contrôles XAML basés sur un modèle pour les applications WinUI 3 avec C++/WinRT
ms.date: 07/09/2020
ms.topic: article
keywords: windows 10, uwp, contrôle personnalisé, contrôle basé sur un modèle, winui, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: d319374791eeb0a02b0291c66f25f55e31bcbc4b
ms.sourcegitcommit: 6759309a3fbb6ede498c95c04c05f57a074ab070
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99069166"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>Contrôles XAML basés sur un modèle pour les applications WinUI 3 avec C++/WinRT

Cet article vous guide dans la création d’un contrôle XAML basé sur un modèle pour WinUI 3 avec C++/WinRT. Les contrôles basés sur un modèle héritent de **Microsoft.UI.Xaml.Controls.Control**, et ont une structure visuelle et un comportement visuel qui peuvent être personnalisés à l’aide de modèles de contrôle XAML. Cet article décrit le même scénario que l’article [Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl) mais il a été adapté pour utiliser WinUI 3.

Avant de suivre les étapes décrites dans cet article, vous devez vous assurer que votre environnement de développement est configuré pour créer des applications WinUI 3. Pour obtenir des informations sur la configuration, consultez [Bien démarrer avec WinUI 3 pour les applications de bureau](./get-started-winui3-for-desktop.md). Vous devez aussi télécharger et installer la dernière version de l’[extension Visual Studio (VSIX) C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) à partir de [Visual Studio Marketplace](https://marketplace.visualstudio.com).

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Dans la boîte de dialogue `Create a new project`, sélectionnez le modèle de projet **Application vide (WinUI dans UWP)** , en veillant à sélectionner la version de langage C++. Définissez le nom du projet sur « BgLabelControlApp » afin que les noms de fichiers correspondent au code dans les exemples ci-dessous. Définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**. Cette procédure pas à pas fonctionne également pour les applications de bureau créées avec le modèle de projet **Application vide, empaquetée (WinUI dans les applications de bureau)** , veillez simplement à effectuer toutes les étapes du projet **BgLabelControlApp (bureau)** .

![Modèle de projet d’application vide](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>Ajouter un contrôle basé sur un modèle à votre application

Pour ajouter un contrôle basé sur un modèle, cliquez sur le menu **Projet** dans la barre d’outils ou cliquez avec le bouton droit sur votre projet dans l’**Explorateur de solutions** et sélectionnez **Ajouter un nouvel élément**. Sous **Visual C++->WinUI**, sélectionnez le modèle **Contrôle personnalisé (WinUI)** . Nommez le nouveau contrôle « BgLabelControl », puis cliquez sur *Ajouter*. Cette opération ajoute trois nouveaux fichiers à votre projet. `BgLabelControl.h` est l’en-tête contenant les déclarations de contrôle et `BgLabelControl.cpp` contient l’implémentation C++/WinRT du contrôle. `BgLabelControl.idl` est le fichier de définition d’interface qui permet au contrôle d’être instancié en tant que classe de runtime.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implémenter la classe de contrôle personnalisé BgLabelControl

Dans les étapes suivantes, vous allez mettre à jour le code dans les fichiers `BgLabelControl.idl`, `BgLabelControl.h` et `BgLabelControl.cpp` dans le répertoire du projet afin d’implémenter la classe de runtime. 


La classe du contrôle basé sur un modèle sera instanciée à partir du balisage XAML ; il s’agira donc d’une classe de runtime. Quand vous générez le projet terminé, le compilateur MIDL (MIDL.exe) utilise le fichier `BgLabelControl.idl` pour générer le fichier de métadonnées Windows Runtime (.winmd) de votre contrôle, qui sera référencé par les consommateurs de votre composant. Pour plus d’informations sur la création de classes de runtime, consultez [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Le contrôle basé sur un modèle que nous créons expose une propriété unique qui est une chaîne utilisée comme étiquette du contrôle. Remplacez le contenu de `BgLabelControl.idl` par le code suivant.

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

La liste ci-dessus illustre le modèle qui vous suivez lorsque vous déclarez une propriété de dépendance (DP). Il existe deux éléments pour chaque DP. Tout d’abord, vous déclarez une propriété statique en lecture seule de type DependencyProperty. Elle porte le nom de votre propriété de dépendance (DP) plus Property. Vous allez utiliser cette propriété statique dans votre implémentation. Ensuite, vous déclarez une propriété en lecture-écriture instance avec le type et le nom de votre DP. Si vous souhaitez créer une propriété jointe (plutôt qu’une DP), consultez les exemples de code présentés dans [Propriétés jointes personnalisées](/windows/uwp/xaml-platform/custom-attached-properties).

Notez que les classes XAML référencées dans le code ci-dessus se trouvent dans les espaces de noms Microsoft.UI.Xaml. C’est ce qui les distingue en tant que contrôles WinUI, par opposition aux contrôles XAML UWP qui sont définis dans les espaces de noms Windows.UI.XAML.

Remplacez le contenu du fichier BgLabelControl.h par le code suivant.

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl>
    {
        BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

        winrt::hstring Label()
        {
            return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
        }

        void Label(winrt::hstring const& value)
        {
            SetValue(m_labelProperty, winrt::box_value(value));
        }

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

Le code affiché ci-dessus implémente les propriétés **Label** et **LabelProperty**, ajoute un gestionnaire d’événements statiques nommé **OnLabelChanged** pour traiter les changements apportés à la valeur de la propriété de dépendance, puis ajoute un membre privé pour stocker le champ de sauvegarde pour **LabelProperty**. De nouveau, notez que les classes XAML référencées dans le fichier d’en-tête se trouvent dans les espaces de noms Microsoft.UI.Xaml qui appartiennent au framework WinUI 3 plutôt que dans les espaces de noms Windows.UI.Xaml utilisés par le framework d’interface utilisateur UWP.


Remplacez ensuite le contenu du fichier BgLabelControl.cpp par le code suivant.

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"
#endif

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```
Cette procédure pas à pas n’utilise pas le rappel **OnLabelChanged**, mais elle est fournie pour que vous puissiez voir comment inscrire une propriété de dépendance avec un rappel de modification de propriété. L’implémentation de **OnLabelChanged** montre également comment obtenir un type projeté dérivé à partir d’un type projeté de base. (Dans le cas présent, le type projeté de base est DependencyObject). Et cela montre comment obtenir un pointeur vers le type qui implémente le type projeté. Cette deuxième opération n’est possible que dans le projet qui implémente le type projeté (autrement dit, le projet qui implémente la classe de runtime).

La fonction [xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) est fournie par l’espace de noms Windows.UI.Xaml.Interop qui n’est pas inclus par défaut dans le modèle de projet WinUI 3. Ajoutez une ligne au fichier d’en-tête précompilé pour votre projet, `pch.h`, pour inclure le fichier d’en-tête associé à cet espace de noms.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>Définir le style par défaut pour BgLabelControl

Dans son constructeur, **BgLabelControl** définit une clé de style par défaut pour lui-même. Un contrôle basé sur un modèle doit avoir un style par défaut (contenant un modèle de contrôle par défaut) qu’il peut utiliser pour effectuer lui-même son rendu, au cas où le consommateur du contrôle ne définit pas un style et/ou un modèle. Dans cette section, nous allons ajouter un fichier de balisage pour le projet contenant notre style par défaut.

Vérifiez que l’option **Afficher tous les fichiers** est activée dans l’**Explorateur de solutions**. Sous le nœud de votre projet, créez un dossier (pas un filtre, mais un dossier) et nommez-le « Themes ». Sous `Themes`, ajoutez un nouvel élément de type **Visual C++ > WinUI > Dictionnaire de ressources (WinUI)** , puis nommez-le « Generic.xaml ». Les noms de dossier et de fichier doivent être ainsi pour que le framework XAML recherche le style par défaut d’un contrôle basé sur un modèle. Supprimez le contenu par défaut de Generic.xaml, puis collez le balisage ci-dessous.

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

Dans ce cas, la seule propriété que définit le style par défaut est le modèle de contrôle. Le modèle se compose d’un carré (dont l’arrière-plan est lié à la propriété **Background** que toutes les instances du type **Control** XAML ont) et un élément de texte (dont le texte est lié à la propriété de dépendance **BgLabelControl::Label**).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de BgLabelControl à la page d’interface utilisateur principale

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après l’élément **Button** (dans **StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

De plus, ajoutez la directive include suivante à `MainPage.h` afin que le type **MainPage** (une combinaison de la compilation du balisage XAML et de code impératif) tienne compte du type de contrôle basé sur un modèle **BgLabelControl**. Si vous souhaitez utiliser **BgLabelControl** à partir d’une autre page XAML, ajoutez cette même directive include pour le fichier d’en-tête de cette page. Ou bien, placez simplement une seule directive include dans votre fichier d’en-tête précompilé.

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

Lancez à présent le processus de génération et exécutez le projet. Vous verrez que le modèle de contrôle par défaut se lie au pinceau d’arrière-plan et à l’étiquette, de l’instance **BgLabelControl** du balisage.

![Résultat du contrôle basé sur un modèle](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implémentation de fonctions substituables, comme **MeasureOverride** et **OnApplyTemplate**

Vous dérivez un contrôle basé sur un modèle de la classe de runtime **Control**, qui elle-même dérive des classes de runtime de base. Et il existe des méthodes substituables de **Control**, de **FrameworkElement** et de **UIElement** que vous pouvez remplacer dans votre classe dérivée. Voici un exemple de code illustrant comment procéder.

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

Les fonctions *substituables* se présentent différemment dans des projections de langage différentes. En C#, par exemple, les fonctions substituables apparaissent généralement sous forme de fonctions virtuelles protégées. En C++/WinRT, elles ne sont ni protégées ni virtuelles, mais vous pouvez quand même les remplacer et fournir votre propre implémentation, comme indiqué ci-dessus.



## <a name="generating-the-control-source-files-without-using-a-template"></a>Génération des fichiers sources du contrôle sans utiliser de modèle.

Cette section montre comment générer les fichiers sources nécessaires à la création de votre contrôle personnalisé sans utiliser le modèle d’élément **Contrôle personnalisé**. 

Tout d’abord, ajoutez un nouvel élément Fichier Midl (.idl) au projet. Dans le menu **Projet**, sélectionnez **Ajouter un nouvel élément...** , puis tapez « MIDL » dans la zone de recherche pour rechercher l’élément de fichier .idl. Nommez le nouveau fichier `BgLabelControl.idl` afin que le nom soit cohérent avec les étapes décrites dans cet article. Supprimez le contenu par défaut de `BgLabelControl.idl` et collez-le dans la déclaration de classe de runtime que nous avons vue dans les étapes précédentes.


Après avoir enregistré le nouveau fichier .idl, l’étape suivante consiste à générer le fichier de métadonnées Windows Runtime (.winmd) et les stubs pour les fichiers d’implémentation .cpp et .h que vous allez utiliser pour implémenter le contrôle basé sur un modèle. Générez ces fichiers en créant la solution, ce qui entraîne la compilation, par le compilateur MIDL (midl.exe), du fichier .idl que vous avez créé. Notez que la génération de la solution ne s’effectue pas correctement et que Visual Studio affiche des erreurs de build dans la fenêtre Sortie, mais les fichiers nécessaires sont générés.

Copiez les fichiers stub BgLabelControl.h et BgLabelControl.cpp de \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\ dans le dossier du projet. Dans l’**Explorateur de solutions**, vérifiez que l’option Afficher tous les fichiers est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

Le compilateur place une ligne static_assert en haut des fichiers BgLabelControl.h et BgLabelControl.cpp pour empêcher que les fichiers générés ne soient compilés. Lors de l’implémentation de votre contrôle, vous devez supprimer ces lignes des fichiers que vous avez placés dans le répertoire de votre projet. Pour cette procédure pas à pas, vous pouvez simplement remplacer tout le contenu des fichiers par le code fourni ci-dessus.
