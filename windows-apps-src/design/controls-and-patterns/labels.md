---
Description: Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
title: Étiquettes
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 68b062dc4bd70c81b1b8b57808fad8e9c7498d75
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172613"
---
# <a name="labels"></a>Étiquettes

 

Une étiquette correspond au nom ou au titre d’un contrôle ou d’un groupe de contrôles associés.

> **API importantes** : propriété Header, [classe TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

En XAML, de nombreux contrôles disposent d’une propriété Header intégrée que vous utilisez pour afficher l’étiquette. Pour les contrôles qui n’ont pas de propriété Header, ou pour étiqueter des groupes de contrôles, vous pouvez utiliser un élément [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

![capture d’écran du contrôle d’étiquette standard](images/label-standard.png)

## <a name="recommendations"></a>Recommandations


-   Utilisez une étiquette pour indiquer à l’utilisateur ce qu’il doit entrer dans un contrôle adjacent. Vous pouvez également étiqueter un groupe de contrôles associés ou afficher un texte d’instructions à côté de ce type de groupe.
-   Si vous étiquetez des contrôles, écrivez l’étiquette sous forme de nom ou de groupe nominal, et non sous la forme d’une phrase ou d’un texte d’instructions. Évitez d’utiliser le signe deux-points ou tout autre signe de ponctuation.
-   Lorsque vous insérez un texte d’instructions dans une étiquette, vous pouvez utiliser davantage de caractères, ainsi que des signes de ponctuation.


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code
* [Exemple d’éléments de base d’une interface utilisateur XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>Rubriques connexes
* [Contrôles de texte](text-controls.md)
* [TextBox.Header, propriété](/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header, propriété](/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header, propriété](/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header, propriété](/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header, propriété](/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header, propriété](/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header, propriété](/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header, propriété](/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock, classe](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 