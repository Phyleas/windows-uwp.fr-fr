---
Description: Identifiez les périphériques d’entrée connectés à un périphérique d’application Windows et identifiez leurs fonctionnalités et attributs.
title: Identifier des appareils d’entrée
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: périphérique, numériseur, entrées, interactions
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 792b2f71408928de0278dd0c623f13923a2165a8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970024"
---
# <a name="identify-input-devices"></a>Identifier des appareils d’entrée


Identifiez les périphériques d’entrée connectés à un périphérique d’application Windows et identifiez leurs fonctionnalités et attributs.

> **API importantes**: [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows. UI. Xaml. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>Récupérer les propriétés de la souris


L’espace de noms [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) contient la classe [**MouseCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseCapabilities) utilisée pour récupérer les propriétés exposées par une ou plusieurs souris connectées. Créez simplement un objet **MouseCapabilities** et obtenez les propriétés qui vous intéressent.

**Notez**  que les valeurs retournées par les propriétés présentées ici sont basées sur toutes les souris détectées : les propriétés booléennes retournent une valeur différente de zéro si au moins une souris prend en charge une fonctionnalité spécifique, et les propriétés numériques retournent la valeur maximale exposée par une souris.

 

Le code suivant utilise une série d’éléments [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) pour afficher les propriétés et les valeurs individuelles de la souris.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>Récupérer les propriétés du clavier


L’espace de noms [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) contient la classe [**KeyboardCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.KeyboardCapabilities), utilisée pour savoir si un clavier est connecté. Créez simplement un objet **KeyboardCapabilities** et obtenez la propriété [**KeyboardPresent**](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent).

Le code suivant utilise un élément [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) pour afficher la propriété et la valeur de clavier.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>Récupérer les propriétés de l’interaction tactile


L’espace de noms [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) contient la classe [**TouchCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.TouchCapabilities) utilisée pour récupérer si des numériseurs tactiles sont connectés. Créez simplement un objet **TouchCapabilities** et obtenez les propriétés qui vous intéressent.

**Notez**  que les valeurs retournées par les propriétés décrites ici sont basées sur tous les numériseurs tactiles détectés : les propriétés booléennes retournent une valeur différente de zéro si au moins un digitaliseur prend en charge une fonctionnalité spécifique, et les propriétés numériques retournent la valeur maximale exposée par un digitaliseur.

 

Le code suivant utilise une série d’éléments [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) permettant d’afficher les propriétés et valeurs de l’interaction tactile.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>Récupérer les propriétés du pointeur


L’espace de noms [**Windows. Devices. Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) contient la classe [**PointerDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice) utilisée pour récupérer si des appareils détectés prennent en charge l’entrée de pointeur (tactile, pavé tactile, souris ou stylet). Créez simplement un objet **PointerDevice** et obtenez les propriétés qui vous intéressent.

**Notez**  que les valeurs retournées par les propriétés décrites ici sont basées sur tous les périphériques de pointage détectés : les propriétés booléennes retournent une valeur différente de zéro si au moins un périphérique prend en charge une fonctionnalité spécifique, et les propriétés numériques retournent la valeur maximale exposée par un périphérique de pointeur.

Le code suivant utilise un tableau permettant d’afficher les propriétés et valeurs de chaque périphérique de pointage.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>Articles connexes

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Exemple d’entrée à faible latence](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Exemple de mode d’interaction utilisateur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple de fonctionnalités de périphériques](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
