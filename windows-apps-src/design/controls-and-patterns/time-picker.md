---
Description: Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier.
title: Sélecteur d’heure
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9f9fe89d14da884d7cce25771bc67ecf3fec6abc
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218772"
---
# <a name="time-picker"></a>Sélecteur d’heure
 

Le sélecteur d’heure offre aux utilisateurs une méthode standard de sélection d’une heure à l’aide d’une entrée tactile, de la souris ou du clavier.

![Exemple de sélecteur d’heure](images/time-picker-closed.png)

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | La bibliothèque d’interface utilisateur Windows version 2.2 ou ultérieure inclut pour ce contrôle un nouveau modèle qui utilise des angles arrondis. Pour plus d’informations, consultez [Rayons des angles](../style/rounded-corner.md). WinUI est un package NuGet qui contient de nouveaux contrôles et des fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/). |

> **API de plateforme** : [classe TimePicker](/uwp/api/Windows.UI.Xaml.Controls.TimePicker), [propriété Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?
Utilisez un sélecteur d’heure pour permettre à un utilisateur de sélectionner une valeur d’heure unique.

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TimePicker">ouvrir l’application et voir le contrôle TimePicker en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Le point d’entrée affiche l’heure choisie, et lorsque l’utilisateur sélectionne ce point d’entrée, la surface du sélecteur s’agrandit à la verticale à partir du milieu pour que l’utilisateur effectue une sélection. Le sélecteur d’heure se superpose aux autres éléments de l’interface utilisateur ; il ne les ferme pas.

![Exemple de développement du sélecteur d’heure](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Créer un sélecteur d’heure

Cet exemple indique comment créer un sélecteur d’heure simple avec un en-tête.

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

Le sélecteur d’heure qui en résulte se présente comme suit :

![Exemple de sélecteur d’heure](images/time-picker-closed.png)

> [!NOTE]
> Pour obtenir des informations importantes sur les valeurs de date et d’heure, voir [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article *Contrôles de date et d’heure*.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Affichage du calendrier](calendar-view.md)
- [Sélecteur de dates](date-picker.md)
