---
description: Une valeur scalaire doit être encapsulée dans un objet de classe de référence avant d’être transmise à une fonction qui attend **IInspectable**. Ce processus d’encapsulation est appelé *boxing* de la valeur.
title: Conversions boxing et unboxing de valeurs scalaires vers IInspectable avec C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, conversion boxing, scalaire, valeur
ms.localizationpriority: medium
ms.openlocfilehash: 3c1a64b97b40608e877f18b764ae92835d2bc4a7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154373"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Conversions boxing et unboxing de valeurs scalaires vers IInspectable avec C++/WinRT
 
L’[**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) est l’interface racine de chaque classe runtime dans Windows Runtime (WinRT). Il s’agit d’un concept analogue à [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) se trouvant à la racine de chaque interface et classe COM et de **System.Object** se trouvant à la racine de chaque classe [Common Type System](/dotnet/standard/base-types/common-type-system).

En d’autres termes, une fonction qui attend **IInspectable** peut se voir transmettre une instance de n’importe quelle classe runtime. Mais vous ne pouvez pas transmettre directement une valeur scalaire, comme une valeur numérique ou une valeur de texte, vers une telle fonction. Au lieu de cela, une valeur scalaire doit être encapsulée dans un objet de classe de référence. Ce processus d’encapsulation est appelé *boxing* de la valeur.

> [!IMPORTANT]
> Vous pouvez convertir (boxing et unboxing) tout type qu’il est possible de passer à une API Windows Runtime. En somme, il s’agit d’un type Windows Runtime. Les valeurs numériques et textuelles (chaînes) correspondent aux exemples ci-dessus. Un autre exemple est un `struct` que vous définissez dans IDL. Si vous essayez de faire une conversion boxing d’un `struct` C++ standard (qui n’est pas défini dans IDL), le compilateur vous rappelle que cette conversion est possible uniquement pour un type Windows Runtime. Une classe runtime est un type Windows Runtime, mais vous pouvez bien sûr passer des classes runtime aux API Windows Runtime sans les convertir.

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) fournit la fonction [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value), qui prend une valeur scalaire et retourne la valeur convertie dans un **IInspectable**. Pour annuler la conversion d’un **IInspectable** et revenir à une valeur scalaire, il existe les fonctions [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) et [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Exemples de conversion boxing d’une valeur
La fonction accesseur [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) renvoie un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), qui est une valeur scalaire. Nous pouvons convertir cette valeur **hstring** et la transmettre à une fonction qui attend **IInspectable** comme suit.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Pour définir la propriété de contenu d’un élément [**Button**](/uwp/api/windows.ui.xaml.controls.button) XAML, appelez la fonction mutateur [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Pour définir la propriété de contenu sur une valeur de chaîne, vous pouvez utiliser ce code.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Tout d’abord, le constructeur de conversion [**hstring**](/uwp/cpp-ref-for-winrt/hstring) convertit la chaîne littérale en un **hstring**. Puis la surcharge de **winrt::box_value** qui prend un **hstring** est appelée.

## <a name="examples-of-unboxing-an-iinspectable"></a>Exemples de conversion unboxing d’un IInspectable
Dans vos propres fonctions qui attendent **IInspectable**, vous pouvez utiliser [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) pour effectuer une conversion unboxing, et vous pouvez utiliser [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) pour effectuer une conversion unboxing avec une valeur par défaut.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Déterminer le type d’une valeur convertie
Si vous recevez une valeur convertie et que vous n'êtes pas certain du type qu’elle contient (vous devez connaître son type pour lui appliquer une conversion unboxing), vous pouvez interroger la valeur convertie pour rechercher son interface [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue), puis appeler **Type** sur elle. Voici un exemple de code :

`WINRT_ASSERT` est une définition de macro qui s’étend à [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Modèle de fonction winrt::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Modèle de fonction winrt::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [Modèle de fonction winrt::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)