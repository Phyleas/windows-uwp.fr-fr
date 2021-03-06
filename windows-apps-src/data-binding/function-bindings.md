---
description: Découvrez comment utiliser les fonctions comme l’étape feuille du chemin de liaison de données dans l’extension de balisage xBind.
title: Fonctions dans x:Bind
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 4d677767f7eb73bf46784b3f256b511e54013548
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170043"
---
# <a name="functions-in-xbind"></a>Fonctions dans x:Bind

> [!NOTE]
> Pour obtenir des informations générales sur l’utilisation de la liaison des données dans votre application avec **{x:Bind}** (et pour une comparaison entre **{x:Bind}** et **{Binding}** ), consultez [Liaison des données en profondeur](data-binding-in-depth.md) et [Extension de balisage {x:Bind}](../xaml-platform/x-bind-markup-extension.md).

À compter de Windows 10, version 1607, **{x : Bind}** prend en charge l’utilisation d’une fonction comme niveau feuille du chemin de liaison. Cela présente les avantages suivants :

- Facilite la conversion de valeur
- Permet aux liaisons de dépendre de plus d’un paramètre

> [!NOTE]
> Pour utiliser des fonctions avec **{x:Bind}** , la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas utiliser des fonctions si votre application cible des versions antérieures de Windows 10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](../debug-test-perf/version-adaptive-code.md).

Dans l’exemple suivant, l’arrière-plan et le premier plan de l’élément sont liés à des fonctions de conversion reposant sur le paramètre de couleur :

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Chemin de la fonction

Le [chemin d’accès à la fonction](../xaml-platform/x-bind-markup-extension.md#property-path) est spécifié comme tout autre chemin d’accès aux propriétés et peut inclure des [points](../xaml-platform/x-bind-markup-extension.md#property-path-resolution) (.), des [indexeurs](../xaml-platform/x-bind-markup-extension.md#collections) ou des [casts](../xaml-platform/x-bind-markup-extension.md#casting) pour localiser la fonction.

Des fonctions statiques peuvent être spécifiées en utilisant la syntaxe XMLNamespace:ClassName.MethodName. Par exemple, utilisez la syntaxe ci-dessous pour la liaison à des fonctions statiques dans le code-behind.

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

Vous pouvez également utiliser des fonctions système directement dans le balisage pour accomplir des scénarios simples comme la mise en forme de la date, la mise en forme du texte, les concaténations de texte, etc. Par exemple :

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

Si le mode est OneWay/TwoWay, la détection de modification est réalisée sur le chemin de la fonction et la liaison est réévaluée si des modifications sont apportées à ces objets.

La fonction en cours de liaison doit :

- Être accessible par le code et les métadonnées (donc travail interne/privée dans C#), mais C++/CX aura besoin de méthodes qui soient des méthodes WinRT publiques.
- La surcharge repose sur le nombre d’arguments, pas sur le type ; la fonction essaiera de trouver une correspondance avec la première surcharge présentant ces nombreux arguments.
- Les types d’arguments doivent correspondre aux données transmises. Nous ne faisons pas de conversions restrictives.
- Le type de retour de la fonction doit correspondre au type de la propriété qui utilise la liaison.

Le moteur de liaison réagit aux notifications de changements de propriétés déclenchées avec le nom de la fonction et réévalue les liaisons si nécessaire. Par exemple :

```xaml
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```

```csharp
public class Person : INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> Vous pouvez utiliser des fonctions dans x :Bind pour obtenir les mêmes scénarios que ceux pris en charge par les éléments Converters et MultiBinding dans WPF.

## <a name="function-arguments"></a>Arguments de la fonction

Plusieurs arguments peuvent être spécifiés dans la fonction. Ils sont séparés par une virgule (,).

- Chemin de liaison. Même syntaxe que si vous établissiez une liaison directement à cet objet.
  - Si le mode est OneWay/TwoWay, la détection de modification sera réalisée et la liaison réévaluée lors des modifications de l’objet.
- Chaîne de constante entourée de guillemets (les guillemets sont nécessaires pour la désigner comme chaîne). L’accent circonflexe (^) peut être utilisé comme caractère d’échappement des guillemets dans les chaînes.
- Numéro de constante. Par exemple, 123.456
- Valeur booléenne. Sous la forme « x : True » ou « x : False »

### <a name="two-way-function-bindings"></a>Liaisons de fonctions bidirectionnelles

Dans un scénario de liaison bidirectionnelle, une deuxième fonction doit être spécifiée pour la direction inverse de la liaison. Pour ce faire, utilisez la propriété de liaison **BindBack**. Dans l’exemple ci-dessous, la fonction doit prendre un seul argument, qui est la valeur qui doit être transmises en retour au modèle.

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```

## <a name="see-also"></a>Voir aussi
* [Extension de balisage {x:Bind}](../xaml-platform/x-bind-markup-extension.md)