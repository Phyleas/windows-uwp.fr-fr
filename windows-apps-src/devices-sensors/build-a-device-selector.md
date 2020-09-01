---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Créer un sélecteur d’appareil
description: La création d’un sélecteur d’appareil permet de limiter les appareils que vous parcourez lors de l’énumération de ceux-ci.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 057b6d3087ae08cab6704dd83ebf1bf93b933a21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159693"
---
# <a name="build-a-device-selector"></a>Créer un sélecteur d’appareil



**API importantes**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

La création d’un sélecteur d’appareil permet de limiter les appareils que vous parcourez lors de l’énumération de ceux-ci. Cela vous permet d’obtenir uniquement des résultats pertinents et d’améliorer les performances du système. Dans la plupart des cas, vous obtenez un sélecteur d’appareils à partir d’une pile d’appareils. Par exemple, vous pouvez utiliser [**GetDeviceSelector**](/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) pour les appareils détectés via sur USB. Ces sélecteurs d’appareils retournent une chaîne AQS (syntaxe de recherche avancée). Si vous ne connaissez pas bien le format AQS, voir [Utilisation de la syntaxe de recherche avancée par programmation](/windows/desktop/search/-search-3x-advancedquerysyntax).

## <a name="building-the-filter-string"></a>Création de la chaîne de filtre

Il existe quelques cas où vous devez énumérer des appareils alors qu’aucun sélecteur d’appareils fourni n’est pas disponible pour votre scénario. Un sélecteur d’appareils est une chaîne de filtre AQS qui contient les informations suivantes. Avant de créer une chaîne de filtre, vous devez connaître certains éléments clés d’information sur les appareils que vous souhaitez énumérer.

-   Les éléments [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) des appareils qui vous intéressent. Pour plus d’informations sur l’incidence de **DeviceInformationKind** sur la façon d’énumérer les appareils, voir [Énumérer les appareils](enumerate-devices.md) ;
-   la procédure de génération d’une chaîne de filtre AQS, expliquée dans cette rubrique ;
-   les propriétés qui vous intéressent ; Les propriétés disponibles dépendent des éléments [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind). Pour plus d’informations, voir [Propriétés d’informations d’appareil](device-information-properties.md).
-   Les protocoles que vous interrogez. Cela est nécessaire uniquement si vous recherchez des appareils sur un réseau sans fil ou filaire. Pour plus d’informations sur cette procédure, voir [Énumérer des appareils sur un réseau](enumerate-devices-over-a-network.md).

Lorsque vous utilisez les API [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration) , vous combinez souvent le sélecteur d’appareil avec le type d’appareil qui vous intéresse. La liste des types d’appareils disponibles est définie par l’énumération [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind). Cette combinaison de facteurs vous permet de limiter les appareils disponibles à ceux qui vous intéressent. Si vous ne spécifiez pas le **DeviceInformationKind** ou si la méthode que vous utilisez ne fournit pas de paramètre **DeviceInformationKind**, le type par défaut est **DeviceInterface**.

Les API [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration) utilisent la syntaxe AQS canonique, mais tous les opérateurs ne sont pas pris en charge. Pour obtenir la liste des propriétés disponibles lors de la création de la chaîne de filtre, voir [Propriétés d’informations sur l’appareil](device-information-properties.md).

**Attention**    Les propriétés personnalisées définies à l’aide du `{GUID} PID` format ne peuvent pas être utilisées lors de la construction de votre chaîne de filtre AQS. Cela vient du fait que le type de propriété est dérivé du nom de propriété bien connu.

 

Le tableau suivant répertorie les opérateurs AQS et les types de paramètres qu’ils prennent en charge.

| Opérateur                       | Types pris en charge                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP \_ égal**                 | Chaîne, booléen, GUID, UInt16, UInt32                                       |
| **COP \_ NOTEQUAL**              | Chaîne, booléen, GUID, UInt16, UInt32                                       |
| **COP \_ LESSTHAN**              | UInt16, UInt32                                                              |
| **COP \_ GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP \_ LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP \_ GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **la \_ valeur COP \_ contient**       | Chaîne, tableau de chaînes, tableau de booléens, tableau de GUID, tableau d’UInt16, tableau d’UInt32 |
| **\_valeur COP \_ NOTCONTAINS**    | Chaîne, tableau de chaînes, tableau de booléens, tableau de GUID, tableau d’UInt16, tableau d’UInt32 |
| **\_valeur COP \_ STARTSWITH**     | Chaîne                                                                      |
| **COP \_ valeur \_ ENDSWITH**       | Chaîne                                                                      |
| **COP \_ DOSWILDCARDS**          | Non prise en charge                                                               |
| **COP- \_ mot \_ égal**           | Non prise en charge                                                               |
| **COP \_ Word \_ STARTSWITH**      | Non prise en charge                                                               |
| **COP \_ propre à l’application \_** | Non prise en charge                                                               |


> **Conseil**    Vous pouvez spécifier **null** pour **COP \_ EQUAL** ou **COP \_ NOTEQUAL**. Cela se traduit par une propriété sans valeur ou par le fait que la valeur n’existe pas. Dans AQS, vous spécifiez **null** à l’aide de crochets vides \[ \] .

> **Important**    Lorsque vous utilisez **la \_ valeur COP \_ Contains** et **COP \_ value \_ NOTCONTAINS** Operators, ils se comportent différemment avec les chaînes et les tableaux de chaînes. Dans le cas d’une chaîne, le système effectue une recherche sans respect de la casse pour voir si l’appareil contient la chaîne indiquée comme sous-chaîne. Dans le cas d’un tableau de chaînes, aucune recherche n’est effectuée dans les sous-chaînes. Avec le tableau de chaînes, une recherche est effectuée pour voir s’il contient la chaîne spécifiée complète. Il n’est pas possible d’effectuer une recherche dans un tableau de chaînes pour voir si les éléments qui le composent contiennent une sous-chaîne.

Si vous ne pouvez pas créer de chaîne de filtre AQS unique qui parcourt vos résultats de manière appropriée, vous pouvez filtrer les résultats après les avoir reçus. Toutefois, si vous choisissez de procéder ainsi, nous recommandons de limiter les résultats issus de la chaîne de filtre AQS initiale autant que possible lorsque vous la fournissez aux API [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration). Cela permet d’améliorer les performances de votre application.

## <a name="aqs-string-examples"></a>Exemples de chaînes AQS

Les exemples suivants montrent comment la syntaxe AQS permet de limiter les appareils que vous souhaitez énumérer. Toutes ces chaînes de filtre sont associées à un [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) pour créer un filtre complet. Si aucun type d’appareil n’est spécifié, n’oubliez pas que le type par défaut est **DeviceInterface**.

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**DeviceInterface**, il énumère tous les objets qui contiennent la classe d’interface de capture audio et qui sont activés. **=** traduit en **COP \_ égal**à.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**Device**, il énumère tous les objets qui ont au moins un id de matériel GenCdRom. **~~** Convertit la ** \_ valeur COP \_ Contains**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**DeviceContainer**, il énumère tous les objets qui ont un nom de modèle contenant la sous-chaîne Microsoft. **~~** Convertit la ** \_ valeur COP \_ Contains**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**DeviceInterface**, il énumère tous les objets qui ont un nom commençant par la sous-chaîne Microsoft. **~&lt;** convertit en **COP \_ STARTSWITH**.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**Device**, il énumère tous les objets qui ont une propriété **System.Devices.IpAddress** définie. **&lt;&gt;\[\]** traduit en **COP \_ NOTEQUALS** combiné avec une valeur **null** .

``` syntax
System.Devices.IpAddress:<>[]
```

Lorsque ce filtre est associé à un type de [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)**Device**, il énumère tous les objets qui n’ont pas de propriété **System.Devices.IpAddress** définie. **=\[\]** traduit en **COP \_ égal** à combiné avec une valeur **null** .

``` syntax
System.Devices.IpAddress:=[]
```

 

 