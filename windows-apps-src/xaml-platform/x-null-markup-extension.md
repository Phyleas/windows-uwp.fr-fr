---
description: Découvrez comment vous pouvez utiliser l’extension de balisage x :Null dans le balisage XAML pour spécifier une valeur null pour une propriété.
title: Extension de balisage xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46256a5456583f9e434f76a2d06bc552c2cb0d3f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169033"
---
# <a name="xnull-markup-extension"></a>Extension de balisage {x:Null}


En XAML, un balisage spécifie une valeur **null** pour une propriété.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Remarques

**null** est le mot clé de référence Null pour C# et C++. Le mot clé Microsoft Visual Basic pour une référence Null est **Nothing**.

La valeur par défaut initiale peut varier selon les propriétés de dépendance, et n’est pas nécessairement **null**. De plus, de nombreuses propriétés de dépendance n’acceptent pas **null** en tant que valeur (via le balisage ou le code), en raison de leur implémentation interne. Le cas échéant, la définition d’une valeur d’attribut XAML avec **{x:Null}** peut engendrer une exception d’analyse.

Certains types Windows Runtime ont la valeur Nullable. Quand un type Nullable n’a pas déjà la valeur **null** comme valeur par défaut, vous pouvez utiliser **{x:Null}** pour définir la valeur **null** en XAML. Si vous utilisez des extensions de composant Visual C++ (C++/CX), les types Nullable sont représentés par [**Platform::IBox<T>**](/cpp/cppcx/platform-ibox-interface). Si vous utilisez des langages Microsoft .NET, les types Nullable sont représentés par [**Nullable<T>**](/dotnet/api/system.nullable-1).

## <a name="related-topics"></a>Rubriques connexes

* [**Nullable<T>**](/dotnet/api/system.nullable-1)
* [**IReference<T>**](/uwp/api/Windows.Foundation.IReference_T_)
 