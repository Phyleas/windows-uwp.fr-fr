---
description: Utilisez l’API Windows. Globalization. DateTimeFormatting avec des modèles et des modèles personnalisés pour afficher les dates et les heures exactement au format de votre choix.
title: Utiliser des modèles de format des dates et heures
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: dbabbcaccd88b187a03c83909bcb38d5f64b30bb
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034312"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Utiliser des modèles de format des dates et heures

Utilisez les classes de l’espace de noms [**Windows. Globalization. DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) avec des modèles et modèles personnalisés pour afficher les dates et les heures exactement au format de votre choix.

## <a name="introduction"></a>Introduction

La classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) fournit différentes façons de mettre en forme les dates et les heures de manière appropriée pour les langues et les régions dans le monde entier. Vous pouvez utiliser les formats standard pour l’année, le mois, le jour, etc. Vous pouvez aussi passer un modèle de format à l’argument *formatTemplate* du constructeur **DateTimeFormatter** , tel que « LongDate » ou « Month Day ».

Toutefois, lorsque vous voulez encore plus de contrôle sur l’ordre et le format des composants de l’objet [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) que vous souhaitez afficher, vous pouvez passer un modèle de format à l’argument *formatTemplate* du constructeur. Un modèle de format utilise une syntaxe spéciale, qui vous permet d’obtenir des composants individuels d’un objet **DateTime** &mdash; uniquement le nom du mois, ou simplement la valeur de l’année, par exemple pour les &mdash; afficher au format personnalisé de votre choix. En outre, il est possible de localiser le modèle pour l’adapter à d’autres langues et régions.

**Remarque**  Il s’agit uniquement d’une vue d’ensemble des modèles de format. Pour consulter une description plus complète des modèles et types de formats, voir la section « Remarques » de la classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live).

## <a name="the-difference-between-format-templates-and-format-patterns"></a>La différence entre les modèles de format et les modèles de format

Un modèle de format est une chaîne de format agnostique à la culture. Ainsi, si vous construisez un **DateTimeFormatter** à l’aide d’un modèle de format, le formateur affiche vos composants de format dans le bon ordre pour la langue actuelle. À l’inverse, un modèle de format est spécifique à la culture. Si vous construisez un **DateTimeFormatter** à l’aide d’un modèle de format, le formateur utilise le modèle exactement comme indiqué. Par conséquent, un modèle n’est pas necesssarily valide entre les cultures.

Nous allons illustrer cette distinction avec un exemple. Nous passons un modèle de format simple (pas un modèle) au constructeur **DateTimeFormatter** . Il s’agit du modèle de format « jour du mois ».

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Ce code crée un formateur basé sur la langue et la région du contexte actuel. L’ordre des composants dans un modèle de format n’a pas d’importance. le formateur les affiche dans le bon ordre pour la langue actuelle. Par conséquent, il affiche « janvier 1 » pour l’anglais (États-Unis), « 1 janvier » pour le français (France) et « 1 月 1 日 » pour le japonais.

En revanche, un modèle de format est spécifique à la culture. Nous allons accéder au modèle de format pour notre modèle de format.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Cela donne différents résultats en fonction de la langue et de la région du Runtime. Des régions différentes peuvent utiliser des composants différents, dans des ordres différents, avec ou sans caractères supplémentaires et espacement.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Dans l’exemple ci-dessus, nous avons entré une chaîne de format dépendante de la culture, et nous obtenons une chaîne de format propre à la culture (qui était une fonction du langage et de la région qui s’est produite lors de l’appel de `dateFormatter.Patterns` ). Par conséquent, si vous construisez un **DateTimeFormatter** à partir d’un modèle de format spécifique à la culture, il sera uniquement valide pour des langues/régions spécifiques.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Le formateur ci-dessus retourne des valeurs spécifiques à la culture pour les composants individuels à l’intérieur des crochets {} . Toutefois, l’ordre des composants dans un modèle de format est indifférent. Vous recevez précisément ce que vous demandez, qui peut ou non être culturellement approprié. Ce formateur est valide pour l’anglais (États-Unis), mais pas pour le français (France) ni pour le japonais.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

En outre, un modèle qui est correct aujourd’hui peut ne pas être correct dans le futur. Les pays ou régions peuvent modifier leurs systèmes de calendrier, ce qui modifie un modèle de format. Windows met à jour la sortie des formateurs en fonction des modèles de format pour s’adapter à ces modifications. Par conséquent, vous ne devez utiliser la syntaxe de modèle que dans une ou plusieurs de ces conditions.

-   si vous n’êtes pas dépendant d’une sortie particulière pour un format ;
-   si vous n’avez pas besoin que le format respecte une norme propre à une culture ;
-   si vous voulez précisément que le modèle soit invariable dans toutes les cultures ;
-   Vous avez l’intention de localiser la chaîne de modèle de format réelle elle-même.

Voici un résumé de la distinction entre les modèles de format et les modèles de format.

**Mettre en forme les modèles, par exemple « jour du mois »**

-   Représentation abstraite d’un format [DateTime](/uwp/api/windows.foundation.datetime?branch=live) qui comprend des valeurs pour le mois, le jour, etc., dans n’importe quel ordre.
-   Garantie de retourner un format standard valide pour toutes les valeurs langue-région prises en charge par Windows.
-   Garantie de produire une chaîne formatée adaptée à la langue-région donnée.
-   Toutes les combinaisons de composants ne sont pas valides. Par exemple, « jour de DayOfWeek » n’est pas valide.

**Modèles de format, tels que « {month. complet} {Day. Integer} »**

-   Chaîne triée explicitement qui exprime le nom complet du mois, suivi d’un espace, suivi de l’entier du jour, dans cet ordre, ou du modèle de format spécifique que vous spécifiez.
-   Peut ne pas correspondre à un format standard valable pour toute paire langue-région.
-   Pas de garantie d’être adapté d’un point de vue culturel.
-   N’importe quelle combinaison de composants peut être spécifiée dans n’importe quel ordre.

## <a name="examples"></a>Exemples

Supposons que vous souhaitiez afficher le mois et le jour courants avec l’heure courante dans un format spécifique. Par exemple, vous souhaitez que les utilisateurs américains voient une chaîne de ce type :

``` syntax
June 25 | 1:38 PM
```

La partie date correspond au modèle de format « jour du mois » et la partie heure correspond au modèle de format « heure ». Ainsi, vous pouvez construire des formateurs pour les modèles de format de date et d’heure appropriés, puis concaténer leur sortie à l’aide d’une chaîne de format localisable.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` est un identificateur de ressource qui fait référence à une ressource localisable dans un fichier de ressources (. resw). Pour une langue par défaut de l’anglais (États-Unis), cette valeur est définie sur « {0} | {1} » avec un commentaire indiquant que « {0} » est la date et « {1} » est l’heure. De cette façon, les traducteurs peuvent ajuster les éléments de mise en forme en fonction des besoins. Par exemple, ils peuvent modifier l’ordre des éléments s’il semble plus naturel dans une langue ou une région que l’heure précède la date. Ils peuvent également remplacer le caractère « | » par tout autre caractère de séparation.

Une autre façon d’implémenter cet exemple consiste à interroger les deux formateurs pour leurs modèles de format, à les concaténer ensemble, puis à construire un troisième formateur à partir du modèle de format résultant.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>API importantes

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de mise en forme des dates et heures](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
