---
Description: Le sélecteur de dates de calendrier est un contrôle déroulant optimisé pour la sélection d’une seule date dans un affichage calendrier, dans lequel les informations contextuelles comme le jour de la semaine ou l’exhaustivité du calendrier sont importantes.
title: Sélecteur de dates du calendrier
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e5ddaf909119830bd8c75c698396c08a7a98427
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173533"
---
# <a name="calendar-date-picker"></a>Sélecteur de dates du calendrier

Le sélecteur de dates de calendrier est un contrôle déroulant optimisé pour la sélection d’une seule date dans un affichage calendrier, dans lequel les informations contextuelles comme le jour de la semaine ou l’exhaustivité du calendrier sont importantes. Vous pouvez modifier le calendrier pour fournir du contexte supplémentaire ou pour limiter les dates disponibles.

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | La bibliothèque d’interface utilisateur Windows version 2.2 ou ultérieure inclut pour ce contrôle un nouveau modèle qui utilise des angles arrondis. Pour plus d’informations, consultez [Rayons des angles](../style/rounded-corner.md). WinUI est un package NuGet qui contient de nouveaux contrôles et des fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

> **API de plateforme** : [classe CalendarDatePicker](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [propriété Date](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date), [événement DateChanged](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un **sélecteur de dates du calendrier** pour permettre à un utilisateur de choisir une date unique à partir d’un affichage Calendrier contextuel. Utilisez-le pour des éléments tels que le choix d’une date de rendez-vous ou de départ.

Pour permettre à un utilisateur de choisir une date connue, par exemple, une date de naissance, où le contexte du calendrier n’est pas important, pensez à utiliser un [sélecteur de dates](date-picker.md).

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/CalendarDatePicker">ouvrir l’application et voir l’objet CalendarDatePicker en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le point d’entrée affiche le texte de l’espace réservé si aucune date n’a été définie. Sinon, il affiche la date choisie. Lorsque l’utilisateur sélectionne le point d’entrée, un affichage de calendrier est développé pour que l’utilisateur sélectionne une date. Cet affichage de calendrier se superpose aux autres éléments de l’interface utilisateur ; il ne les ferme pas.

![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>Créer un sélecteur de dates

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

Le sélecteur de dates du calendrier qui en résulte se présente comme suit :

![Exemple de sélecteur de dates du calendrier](images/calendar-date-picker-closed.png)

Le sélecteur de dates du calendrier possède un [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) interne pour la sélection de dates. Un sous-ensemble des propriétés CalendarView, comme [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted) et [FirstDayOfWeek](/uwp/api/windows.ui.xaml.controls.calendardatepicker.firstdayofweek), existe sur CalendarDatePicker et est transmis au CalendarView interne pour vous permettre de le modifier. 

Toutefois, vous ne pouvez pas modifier le [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) du CalendarView interne pour autoriser la sélection multiple. Si vous avez besoin de permettre à un utilisateur de sélectionner plusieurs dates ou que vous avez besoin que le calendrier soit toujours visible, envisagez d’utiliser un affichage Calendrier plutôt qu’un sélecteur de dates du calendrier. Voir l’article [Affichage Calendrier](calendar-view.md) pour plus d’informations sur la manière dont vous pouvez modifier l’affichage du calendrier.

### <a name="selecting-dates"></a>Sélection de dates

Utilisez la propriété [Date](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date) pour obtenir ou définir la date sélectionnée. Par défaut, la propriété Date est **null**. Lorsqu’un utilisateur sélectionne une date dans l’affichage Calendrier, cette propriété est mise à jour. Un utilisateur peut effacer la date en cliquant sur la date sélectionnée dans l’affichage Calendrier pour la désélectionner. 

Vous pouvez définir la date dans votre code comme suit.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Lorsque vous définissez la date dans le code, la valeur est limitée par les propriétés [MinDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.mindate) et [MaxDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.maxdate).
- Si la valeur **Date** est inférieure à **MinDate**, la valeur est définie sur **MinDate**.
- Si la valeur **Date** est supérieure à **MaxDate**, la valeur est définie sur **MaxDate**.

Vous pouvez gérer l’événement [DateChanged](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged) pour être averti lorsque la valeur Date a changé.

> [!NOTE]
> Pour obtenir des informations importantes sur les valeurs de date, consultez [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article Contrôles de date et d’heure.

### <a name="setting-a-header-and-placeholder-text"></a>Définition d’un texte d’en-tête et d’espace réservé

Vous pouvez ajouter un contrôle [Header](/uwp/api/windows.ui.xaml.controls.calendardatepicker.header) (ou étiquette) et un contrôle [PlaceholderText](/uwp/api/windows.ui.xaml.controls.calendardatepicker.placeholdertext) (ou filigrane) au sélecteur de date de calendrier pour indiquer à l’utilisateur son rôle. Si vous voulez personnaliser l’aspect de l’en-tête, vous pouvez définir la propriété [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.headertemplate) au lieu de la propriété Header.

Le texte d’espace réservé par défaut est « sélectionner une date ». Vous pouvez le supprimer en définissant la propriété PlaceholderText sur une chaîne vide, ou vous pouvez fournir un texte personnalisé comme illustré ici.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Affichage du calendrier](calendar-view.md)
- [Sélecteur de dates](date-picker.md)
- [Sélecteur d’heure](time-picker.md)