---
Description: Vos mosaïques et toasts peuvent charger des chaînes et des images adaptées à la langue d’affichage, au facteur d’échelle d’affichage, au contraste élevé et à d’autres contextes d’exécution.
title: Prise en charge des notifications par vignette et toast pour la langue, la mise à l’échelle et le contraste élevé
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 88bcd5d6ce59d0561e76f46f6291f58ad03ddf3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156733"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Prise en charge des notifications par vignette et toast pour la langue, la mise à l’échelle et le contraste élevé

Vos mosaïques et toasts peuvent charger des chaînes et des images adaptées à la langue d’affichage, au [facteur d’échelle d’affichage](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), au contraste élevé et à d’autres contextes d’exécution. Pour plus d’informations sur l’utilisation de qualificateurs dans les noms de vos fichiers de ressources, consultez [adapter vos ressources à la langue, à la mise à l’échelle, ainsi qu’à d’autres qualificateurs](../../../app-resources/tailor-resources-lang-scale-contrast.md) et [icônes d’application et logos](../../style/app-icons-and-logos.md).

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Faire référence à une ressource de type chaîne à partir d’un modèle

Dans votre modèle de mosaïque ou Toast, vous pouvez faire référence à une ressource de type chaîne à l’aide du `ms-resource` schéma d’URI (Uniform Resource Identifier) suivi d’un simple identificateur de ressource de chaîne. Par exemple, si vous avez un fichier Resources. resx qui contient une entrée de ressource dont le nom est « adieu », vous avez une ressource de chaîne avec l’identificateur « adieu ». Pour plus d’informations sur les identificateurs de ressource de chaîne et les fichiers de ressources (. resw), consultez [localiser des chaînes dans votre interface utilisateur et le manifeste du package d’application](../../../app-resources/localize-strings-ui-manifest.md).

Voici comment une référence à l’identificateur de ressource de chaîne « adieu » s’affiche dans le corps du [texte](/uwp/schemas/tiles/tilesschema/element-text?branch=live) du contenu du modèle, à l’aide de `ms-resource` .

```xml
<text id="1">ms-resource:Farewell</text>
```

Si vous omettez le `ms-resource` schéma d’URI, le corps du texte est simplement un littéral de chaîne et *non* une référence à un identificateur.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Faire référence à une ressource d’image à partir d’un modèle

Dans votre modèle de mosaïque ou Toast, vous pouvez faire référence à une ressource d’image à l’aide du `ms-appx` schéma URI (Uniform Resource Identifier) suivi du nom de la ressource d’image. Il s’agit de la même façon que vous faites référence à une ressource d’image dans le balisage XAML (pour plus d’informations, consultez [référencer une image ou une autre ressource à partir du balisage et du code XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Par exemple, vous pouvez nommer des dossiers comme celui-ci.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

Dans ce cas, vous avez une ressource d’image unique et son nom (comme chemin d’accès absolu) est `/Assets/Images/welcome.png` . Voici comment vous utilisez ce nom dans votre modèle.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Notez dans cet exemple d’URI que le schéma (« `ms-appx` ») est suivi de « `://` », suivi d’un chemin d’accès absolu (un chemin d’accès absolu commence par « `/` »).

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hébergement et chargement d’images dans le Cloud

Les `ms-resource` `ms-appx` schémas d’URI et effectuent une correspondance de qualificateur automatique pour rechercher la ressource la plus appropriée pour le contexte actuel. Les schémas d’URI Web (par exemple,, `http` `https` et `ftp` ) n’effectuent pas de correspondance automatique de ce type.

Au lieu de cela, ajoutez à l’URI de votre image une chaîne de requête décrivant la valeur ou les valeurs du qualificateur demandé.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

Ensuite, dans l’app service qui fournit vos images, implémentez un gestionnaire HTTP qui inspecte et utilise la chaîne de requête pour déterminer l’image à retourner.

Vous devez également définir l’attribut [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) sur `true` dans la charge utile XML de la [mosaïque](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) ou de la notification [Toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live) . L’attribut **addImageQuery** apparaît dans les `visual` `binding` éléments, et `image` des schémas Tile et Toast. La définition explicite de **addImageQuery** sur un élément remplace toute valeur définie sur un ancêtre. Par exemple, une valeur **addImageQuery** de `true` dans un `image` élément se substitue à un **addImageQuery** de `false` dans son `binding` élément parent.

Il s’agit des chaînes de requête que vous pouvez utiliser.

| Qualificateur | Chaîne de requête | Exemple |
| --------- | ------------ | ------- |
| Scale | à l’échelle ms | ? MS-Scale = 400 |
| Langage | MS-lang | ? MS-lang = en-US |
| Comparez | MS-Contrast | ? MS-Contrast = élevé |

Pour obtenir une table de référence de toutes les valeurs de qualificateur possibles que vous pouvez utiliser dans vos chaînes de requête, consultez [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>API importantes

* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Rubriques connexes

* [Tailles d’écran et points d’arrêt pour la conception réactive](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Adaptez vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Indications pour les éléments](../../style/app-icons-and-logos.md)multimédias en mosaïque et en icône.
* [Globalisation et localisation](../../globalizing/globalizing-portal.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../../app-resources/localize-strings-ui-manifest.md)
* [Référencer une image ou une autre ressource à partir du balisage et du code XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Schéma de vignette](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Schéma de toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live)