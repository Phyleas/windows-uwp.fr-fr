---
description: Apprenez à utiliser l’attribut x :DefaultBindMode dans le balisage XAML pour spécifier un mode par défaut pour x :Bind autre que OneTime.
title: attribut xDefaultBindMode
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 817b89dc8f3ab7952cdbfc53489eb1afb12024f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166873"
---
# <a name="xdefaultbindmode-attribute"></a>Attribut x:DefaultBindMode

Dans le balisage XAML, spécifie un mode par défaut pour x :Bind.

**x :DefaultBindMode** est disponible à partir de Windows 10, version 1607 (mise à jour anniversaire), version SDK 14393.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Remarques

[x :bind](x-bind-markup-extension.md) a le mode par défaut **OneTime**. Cela a été choisi pour des raisons de performances, car l’utilisation de **OneWay** entraîne la génération d’un code supplémentaire pour se déformer et gérer la détection des modifications. Vous pouvez utiliser **x :DefaultBindMode** pour modifier le mode par défaut de x :bind pour un segment spécifique de l’arborescence de balises. Le mode spécifié s’applique à toutes les expressions x :Bind sur cet élément et ses enfants, qui ne spécifient pas explicitement un mode dans le cadre de la liaison.

## <a name="related-topics"></a>Rubriques connexes

* [extension de balisage x :Bind](x-bind-markup-extension.md)
