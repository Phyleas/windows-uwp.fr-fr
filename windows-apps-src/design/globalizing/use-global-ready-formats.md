---
description: Concevez votre application pour qu’elle soit prête à l’international en mettant en forme de manière appropriée les dates, les heures, les nombres, les numéros de téléphone et les devises. Vous pourrez ensuite adapter votre application à des cultures, régions et langues supplémentaires sur le marché mondial.
title: Globaliser vos formats de date/heure/chiffres
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 8c3bacbfbbe944cddfe014fcd34038ca9a56c36e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034342"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globaliser vos formats de date/heure/chiffres

Concevez votre application pour qu’elle soit prête à l’international en mettant en forme de manière appropriée les dates, les heures, les nombres, les numéros de téléphone et les devises. Vous pourrez ensuite adapter votre application à des cultures, régions et langues supplémentaires sur le marché mondial.

## <a name="introduction"></a>Introduction

Lors de la création de votre application, si vous pensez plus largement qu’une seule langue et une seule culture, vous aurez moins de problèmes inattendus lorsque votre application se développera sur de nouveaux marchés. Par exemple, les dates, les heures, les nombres, les calendriers, les devises, les numéros de téléphone, les unités de mesure et les formats du papier sont des éléments susceptibles de s’afficher différemment selon la culture ou la langue.

Les différentes régions et cultures utilisent des formats de date et d’heure différents. Celles-ci incluent des conventions pour l’ordre des jours et des mois de la date, pour la séparation des heures et des minutes dans le temps, et même pour la ponctuation utilisée comme séparateur. En outre, les dates peuvent être affichées dans différents formats longs (« mercredi 28 mars, 2012 ») ou des formats courts (« 3/28/12 »), qui varient d’une culture à l’autre. Et, bien sûr, les noms et les abréviations des jours de la semaine et les mois de l’année diffèrent entre les langues.

Vous pouvez afficher un aperçu des formats utilisés pour les différentes langues. Accédez à **paramètres**  >  **heure &** langue  >  **& langue** , puis cliquez sur **date, heure, & paramètres régionaux**  >  **modifier les formats de date, d’heure ou de nombre** . Sous l’onglet **formats** , sélectionnez une langue dans la liste déroulante **format** et affichez un aperçu des formats des **exemples** .

Cette rubrique utilise les termes « liste des langues du profil utilisateur », « liste des langues du manifeste de l’application » et « liste des langues du runtime de l’application ». Pour plus d’informations sur ce que signifient ces termes et sur la manière d’accéder à leurs valeurs, consultez [comprendre les langages de profil utilisateur et les langages du manifeste d’application](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Mettre en forme les dates et les heures de la liste des langues du runtime de l’application

Si vous devez autoriser les utilisateurs à choisir une date, ou à sélectionner une heure, utilisez les [contrôles calendrier, date et heure](../controls-and-patterns/date-and-time.md)standard. Ils utilisent automatiquement le meilleur format de date et heure pour la liste des langues du runtime de l’application.

Si vous devez afficher des dates ou des heures vous-même, vous pouvez utiliser la classe [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) . Par défaut, **DateTimeFormatter** utilise automatiquement le format de date et d’heure le plus approprié pour la liste des langues du runtime de l’application. Ainsi, le code ci-dessous met en forme une valeur **DateTime** donnée dans la meilleure méthode pour cette liste. En guise d’exemple, supposons que la liste des langages de manifeste de votre application comprend l’anglais (États-Unis), qui est également votre valeur par défaut et l’allemand (Allemagne). Si la date actuelle est le 6 2017 novembre et que la liste des langues du profil utilisateur contient en premier allemand (Allemagne), le formateur fournit « 06.11.2017 ». Si la liste des langues du profil utilisateur contient en anglais (États-Unis) en premier (ou si elle ne contient ni l’anglais, ni l’allemand), le formateur donne « 11/6/2017 » (car « en-US » correspond à, ou est utilisé comme valeur par défaut).

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

Vous pouvez tester le code ci-dessus sur votre propre ordinateur comme celui-ci.

- Assurez-vous que votre projet contient des fichiers de ressources qualifiés pour « en-US » et « de-DE » (consultez [adapter vos ressources pour connaître la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Modifiez votre liste de langues de profil utilisateur dans **paramètres**  >  **heure & langue langue**  >  **&**  >  **langues** . Ajoutez allemand (Allemagne), définissez-le comme valeur par défaut, puis réexécutez le code.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Mettre en forme les dates et les heures de la liste des langues du profil utilisateur

N’oubliez pas que, par défaut, **DateTimeFormatter** correspond à la liste des langues du runtime de l’application. De cette façon, si vous affichez des chaînes telles que « date date &lt; &gt; », la langue correspondra au format de date.

Si, pour une raison quelconque, vous souhaitez mettre en forme des dates et/ou des heures uniquement en fonction de la liste des langues du profil utilisateur, vous pouvez le faire à l’aide d’un code similaire à celui de l’exemple ci-dessous. Toutefois, si vous le faites, sachez que l’utilisateur peut choisir une langue pour laquelle votre application n’a pas de chaînes traduites. Par exemple, si votre application n’est pas localisée en allemand (Allemagne), mais que l’utilisateur choisit que comme langue par défaut, cela peut entraîner l’affichage de chaînes inhabituelles, telles que « la date est 06.11.2017 ».

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Mettre correctement en forme les nombres et les devises

La mise en forme des nombres varie en fonction des cultures. Les différences de mise en forme peuvent concerner le nombre de décimales affichées, le caractère servant de séparateur décimal et le symbole monétaire. Utilisez les classes de l’espace de noms [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) pour afficher les nombres décimaux, les pourcentages, les valeurs de permouture et les devises. La plupart du temps, vous souhaiterez que ces classes de formateur utilisent le meilleur format pour le profil utilisateur. Toutefois, vous pouvez utiliser les formateurs pour afficher une devise pour toute région ou format.

Cet exemple montre comment afficher les devises à la fois par profil utilisateur et pour un système monétaire donné spécifique.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

Vous pouvez tester le code ci-dessus sur votre propre PC en modifiant le pays ou la région dans **paramètres**  >  **heure & langue**  >  **&**  >  **pays ou région** de langue. Choisissez un pays ou une région (peut-être Islande) et réexécutez le code.

## <a name="use-a-culturally-appropriate-calendar"></a>Utiliser un calendrier adapté à la culture

Le calendrier peut être différent selon les régions et les langues. Le calendrier grégorien n’est pas le calendrier utilisé par défaut dans toutes les régions. Les utilisateurs dans certaines régions peuvent choisir d’autres calendriers, tels que le calendrier d’ère japonaise ou les calendriers lunaires arabes. Les dates et heures du calendrier sont également sensibles aux différents fuseaux horaires et à l’heure d’été.

Pour vous assurer que le format de calendrier par défaut est utilisé, vous pouvez utiliser les [contrôles de calendrier, de date et d’heure](../controls-and-patterns/date-and-time.md)standard. Pour les scénarios plus complexes, lorsque l’utilisation directe d’opérations sur des dates de calendrier peut être nécessaire, **Windows. Globalization** fournit une classe [**Calendar**](/uwp/api/windows.globalization.calendar?branch=live) qui donne une représentation de calendrier appropriée pour la culture, la région et le type de calendrier donnés.

## <a name="format-phone-numbers-appropriately"></a>Mettre correctement en forme les numéros de téléphone

Les numéros de téléphone affichent une mise en forme différente selon les régions. Le nombre de chiffres, la façon dont les chiffres sont regroupés et l’importance de certaines parties du numéro de téléphone varient d’un pays à l’autre. À partir de Windows 10, version 1607, vous pouvez utiliser les classes de l’espace de noms [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) pour mettre en forme les numéros de téléphone de manière appropriée pour la région actuelle.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analyse une chaîne de chiffres et vous permet de déterminer si les chiffres sont un numéro de téléphone valide dans la région active ; compare l’égalité de deux nombres ; et pour extraire les différentes parties fonctionnelles du numéro de téléphone, telles que le code de pays ou le code de zone géographique.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) met en forme une chaîne de chiffres ou un **PhoneNumberInfo** pour l’affichage, même lorsque la chaîne de chiffres représente un numéro de téléphone partiel. Vous pouvez utiliser cette mise en forme de nombres partiels pour mettre en forme un nombre lorsqu’un utilisateur entre le nombre.

L’exemple ci-dessous montre comment utiliser **PhoneNumberFormatter** pour formater un numéro de téléphone lors de son entrée. Chaque fois que du texte change dans une **zone** de texte nommée phoneNumberInputTextBox, le contenu de la zone de texte est mis en forme à l’aide de la région par défaut actuelle et affiché dans un **TextBlock** nommé phoneNumberOutputTextBlock. À des fins de démonstration, la chaîne est également mise en forme à l’aide de la région de la Nouvelle-Zélande et affichée dans un TextBlock nommé phoneNumberOutputTextBlockNZ.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

Vous pouvez tester le code ci-dessus sur votre propre PC en modifiant le pays ou la région dans **paramètres**  >  **heure & langue**  >  **&**  >  **pays ou région** de langue. Choisissez un pays ou une région (par exemple, Nouvelle-Zélande pour confirmer que les formats correspondent), puis réexécutez le code. Pour les données de test, vous pouvez effectuer une recherche sur le Web pour le numéro de téléphone d’une entreprise en Nouvelle-Zélande.

## <a name="the-users-language-and-cultural-preferences"></a>Les préférences de langue et de culture de l’utilisateur

Pour les scénarios où vous souhaitez fournir des fonctionnalités différentes basées uniquement sur la langue, la région ou les préférences culturelles de l’utilisateur, Windows vous donne la possibilité d’accéder à ces préférences, par le biais de [**Windows.SysTEM. UserProfile. GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). Si nécessaire, utilisez la classe **GlobalizationPreferences** pour obtenir les préférences actuelles de l’utilisateur (emplacement géographique, langues par défaut, devises par défaut, etc.). Toutefois, n’oubliez pas que si les chaînes/images de votre application ne sont pas localisées pour la langue par défaut de l’utilisateur, les dates et heures et les autres données formatées pour cette langue par défaut ne correspondent pas aux chaînes que vous affichez.

## <a name="important-apis"></a>API importantes

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendrier](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles de calendrier, de date et d’heure](../controls-and-patterns/date-and-time.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)
* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Exemples

* [Exemple de détails et d’opérations mathématiques dans un calendrier](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Calendar%20details%20and%20math%20sample%20(Windows%208))
* [Exemple de mise en forme des dates et heures](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
* [Exemple de préférences de globalisation](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
* [Exemple de mise en forme et d’analyse des nombres](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Number%20formatting%20and%20parsing%20sample%20(Windows%208))
