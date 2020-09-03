---
description: Cette rubrique explique le concept général des qualificateurs, comment les utiliser et l’objectif de chacun des noms de qualificateurs.
title: Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 59f0b636384ba133e985f0704e2033c1acc5f15e
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89412013"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs

Cette rubrique explique le concept général des qualificateurs de ressource, leur utilisation et le rôle de chacun des noms de qualificateur. Consultez [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) pour obtenir une table de référence de toutes les valeurs de qualificateur possibles.

Votre application peut charger des ressources et des ressources qui sont adaptées aux contextes d’exécution, tels que la langue d’affichage, le contraste élevé, le [facteur d’échelle d’affichage](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)et bien d’autres. Pour ce faire, vous devez nommer les dossiers ou les fichiers de vos ressources pour qu’ils correspondent aux noms de qualificateurs et aux valeurs de qualificateur qui correspondent à ces contextes. Par exemple, vous souhaiterez peut-être que votre application charge un ensemble différent de ressources d’image en mode de contraste élevé.

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Nom du qualificateur, valeur du qualificateur et qualificateur

Un nom de qualificateur est une clé qui est mappée à un ensemble de valeurs de qualificateur. Voici le nom du qualificateur et les valeurs du qualificateur pour le contraste.

| Context | Nom du qualificateur | Valeurs de qualificateur |
| :--------------- | :--------------- | :--------------- |
| Paramètre de contraste élevé | élevé | standard, haut, noir, blanc |

Vous associez un nom de qualificateur à une valeur de qualificateur pour former un qualificateur. `<qualifier name>-<qualifier value>` format d’un qualificateur. `contrast-standard` est un exemple de qualificateur.

Ainsi, pour un contraste élevé, l’ensemble de qualificateurs est `contrast-standard` ,, `contrast-high` `contrast-black` et `contrast-white` . Les noms de qualificateurs et les valeurs de qualificateur ne respectent pas la casse. Par exemple, `contrast-standard` et `Contrast-Standard` sont le même qualificateur.

## <a name="use-qualifiers-in-folder-names"></a>Utiliser des qualificateurs dans les noms de dossiers

Voici un exemple d’utilisation de qualificateurs pour nommer des dossiers qui contiennent des fichiers d’éléments multimédias. Utilisez des qualificateurs dans les noms de dossiers si vous avez plusieurs fichiers d’éléments multimédias par qualificateur. De cette façon, vous définissez le qualificateur une seule fois au niveau du dossier, et le qualificateur s’applique à tout ce qui se trouve dans le dossier.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Si vous nommez vos dossiers comme dans l’exemple ci-dessus, votre application utilise le paramètre de contraste élevé pour charger les fichiers de ressources à partir du dossier nommé pour le qualificateur approprié. Ainsi, si le paramètre est contraste élevé noir, les fichiers de ressources dans le `\Assets\Images\contrast-black` dossier sont chargés. Si le paramètre est défini sur aucun (c’est-à-dire que l’ordinateur n’est pas en mode de contraste élevé), les fichiers de ressources dans le `\Assets\Images\contrast-standard` dossier sont chargés.

## <a name="use-qualifiers-in-file-names"></a>Utiliser des qualificateurs dans les noms de fichiers

Au lieu de créer et de nommer des dossiers, vous pouvez utiliser un qualificateur pour nommer les fichiers de ressources eux-mêmes. Vous préférerez peut-être le faire si vous n’avez qu’un seul fichier de ressources par qualificateur. Voici un exemple.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

Le fichier dont le nom contient le qualificateur le plus approprié pour le paramètre est celui qui est chargé. Cette logique de correspondance fonctionne de la même façon pour les noms de fichiers que pour les noms de dossiers.

## <a name="reference-a-string-or-image-resource-by-name"></a>Référencer une ressource de type chaîne ou image par nom

Consultez [faire référence à un identificateur de ressource de chaîne à partir du balisage XAML](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml), [faire référence à un identificateur de ressource de chaîne à partir du code](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)et [référencer une image ou une autre ressource à partir du balisage et du code XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Correspondances du qualificateur réel et neutre
Vous n’avez pas besoin de fournir un fichier de ressources pour *chaque* valeur de qualificateur. Par exemple, si vous avez besoin d’une seule ressource visuelle pour le contraste élevé et l’autre pour le contraste standard, vous pouvez nommer ces ressources comme suit.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

Le premier nom de fichier contient le `contrast-high` qualificateur. Ce qualificateur est une correspondance *réelle* pour tout paramètre de contraste élevé lorsque le contraste élevé est *activé*. En d’autres termes, il s’agit d’une correspondance proche qui est préférable. Une correspondance *réelle* ne peut se produire que si le qualificateur contient une valeur *réelle* , comme c’est le cas pour ce. Dans ce cas, `high` est une valeur *réelle* pour `contrast` .

Le fichier nommé n' `logo.png` a pas de qualificateur de contraste. L’absence d’un qualificateur est une valeur *neutre* . Si aucune correspondance préférée n’est trouvée, la valeur neutre sert de correspondance de secours. Dans cet exemple, si le contraste élevé est *désactivé*, il n’y a pas de correspondance réelle. La correspondance *neutre* est la meilleure correspondance qui peut être trouvée, et donc la ressource `logo.png` est chargée.

Si vous deviez modifier le nom de `logo.png` en `logo.contrast-standard.png` , le nom de fichier contiendrait une valeur de qualificateur réelle. Avec un contraste élevé, il y aurait une correspondance réelle avec `logo.contrast-standard.png` , et c’est le fichier de ressources qui serait chargé. Ainsi, les mêmes fichiers sont chargés, dans les mêmes conditions, mais en raison de différentes correspondances.

Si vous n’avez besoin que d’un ensemble de ressources pour un contraste élevé et un pour un contraste standard, vous pouvez utiliser des noms de dossiers au lieu de noms de fichiers. Dans ce cas, si vous omettez le nom du dossier, vous obtenez entièrement la correspondance neutre.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Pour plus d’informations sur le fonctionnement de la correspondance des qualificateurs, consultez [système de gestion des ressources](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Qualificateurs multiples

Vous pouvez combiner des qualificateurs dans des noms de dossiers et de fichiers. Par exemple, vous souhaiterez peut-être que votre application charge des ressources d’image lorsque le mode de contraste élevé est activé *et* que le facteur d’échelle d’affichage est 400. Pour ce faire, vous pouvez utiliser des dossiers imbriqués.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Pour `logo.png` et les autres fichiers à charger, les paramètres doivent correspondre à la *fois aux deux* qualificateurs.

Une autre option consiste à combiner plusieurs qualificateurs dans un nom de dossier.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

Dans un nom de dossier, vous combinez plusieurs qualificateurs séparés par un trait de soulignement. `<qualifier1>[_<qualifier2>...]` est le format.

Vous pouvez combiner plusieurs qualificateurs dans un nom de fichier au même format.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

Selon les outils et le flux de travail que vous utilisez pour la création d’éléments multimédias, ou sur ce que vous trouvez le plus facile à lire et/ou gérer, vous pouvez choisir une stratégie d’attribution de noms unique pour tous les qualificateurs, ou vous pouvez les combiner pour différents qualificateurs.

## <a name="alternateform"></a>AlternateForm

Le `alternateform` qualificateur est utilisé pour fournir une autre forme de ressource à des fins spéciales. Cela est généralement utilisé uniquement par les développeurs d’applications japonaises pour fournir une chaîne Furigana pour laquelle la valeur `msft-phonetic` est réservée (consultez la section « prise en charge de Furigana pour les chaînes japonaises qui peuvent être triées » dans [How to prepare for localization](/previous-versions/windows/apps/hh967762(v=win.10))).

Votre système cible ou votre application doit fournir une valeur par rapport à laquelle les `alternateform` qualificateurs sont mis en correspondance. N’utilisez pas le `msft-` préfixe pour vos propres `alternateform` valeurs de qualificateur personnalisées.

## <a name="configuration"></a>Configuration

Il est peu probable que vous ayez besoin du `configuration` nom du qualificateur. Il peut être utilisé pour spécifier des ressources qui s’appliquent uniquement à un environnement de création donné, tel que des ressources test uniquement.

Le `configuration` qualificateur est utilisé pour charger une ressource qui correspond le mieux à la valeur de la `MS_CONFIGURATION_ATTRIBUTE_VALUE` variable d’environnement. Par conséquent, vous pouvez définir la variable sur la valeur de chaîne qui a été affectée aux ressources pertinentes, par exemple `designer` ou `test` .

## <a name="contrast"></a>Comparez

Le `contrast` qualificateur est utilisé pour fournir les ressources qui correspondent le mieux aux paramètres de contraste élevé.

## <a name="custom"></a>Custom

Votre application peut définir une valeur pour le `custom` qualificateur, puis les ressources chargées qui correspondent le mieux à cette valeur. Par exemple, vous pouvez charger des ressources en fonction de la licence de votre application. Lorsque votre application démarre, elle vérifie sa licence et l’utilise comme valeur pour le `custom` qualificateur en appelant [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue), comme indiqué dans l’exemple de code.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

Dans ce scénario, vous donnez des noms aux ressources qui incluent les qualificateurs `custom-premium` , `custom-standard` et `custom-trial` .

## <a name="devicefamily"></a>DeviceFamily

Il est peu probable que vous ayez besoin du `devicefamily` nom du qualificateur. Vous pouvez et devez éviter de l’utiliser chaque fois que cela est possible, car il existe des techniques que vous pouvez utiliser à la place, qui sont bien plus pratiques et fiables. Ces techniques sont décrites dans [détection de la plateforme sur laquelle votre application s’exécute](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) et du [code adaptatif](../debug-test-perf/version-adaptive-code.md)de la version.

Toutefois, en dernier recours, il est possible d’utiliser des qualificateurs devicefamily pour nommer des dossiers qui contiennent vos vues XAML (une vue XAML est un fichier XAML qui contient la disposition et les contrôles de l’interface utilisateur).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

Ou vous pouvez nommer les fichiers.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

Dans les deux cas, chaque copie de `MainPage.[<qualifier>].xaml` partage un commun `MainPage.xaml.cs` , qui reste inchangé dans votre projet en termes de nom, d’emplacement et de contenu.

Vous pouvez également utiliser un qualificateur devicefamily pour nommer un fichier de ressources ( `.resw` ) ou un dossier. Par exemple, lorsque votre application s’exécute sur la famille d’appareils mobiles, l’élément d’interface utilisateur `<TextBlock x:Uid="DeviceFriendlyName"/>` utilisera les ressources de texte et de premier plan définies dans votre `Resources.devicefamily-mobile.resw` fichier si elle contient

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Pour plus d’informations sur l’utilisation d’un fichier de ressources, consultez [Localize Your UI Strings](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

Il est peu probable que vous ayez besoin du `dxfeaturelevel` nom du qualificateur. Il a été conçu pour être utilisé avec les ressources de jeu Direct3D, afin de provoquer le chargement de ressources de niveau inférieur pour qu’elles correspondent à une configuration matérielle de niveau inférieur particulière. Mais la prévalence de cette configuration matérielle est maintenant tellement faible que nous vous recommandons de ne pas utiliser ce qualificateur.

## <a name="homeregion"></a>HomeRegion

Le `homeregion` qualificateur correspond au paramètre de l’utilisateur pour le pays ou la région. Il représente l’emplacement d’hébergement de l’utilisateur. Les valeurs incluent toute [balise de région BCP-47](https://tools.ietf.org/html/bcp47)valide. Autrement dit, n’importe quel code de région **iso 3166-1 alpha-2** à deux lettres, ainsi que le jeu de codes géographiques à trois chiffres **ISO 3166-1** pour les régions composées (voir la section [de la Division statistique des Nations Unies M49 composition des codes de région](https://unstats.un.org/unsd/methods/m49/m49regin.htm)). Les codes « sélectionnés économiques et autres regroupements » ne sont pas valides.

## <a name="language"></a>Langage

Un `language` qualificateur correspond au paramètre de langue d’affichage. Les valeurs incluent toute [balise de langue BCP-47](https://tools.ietf.org/html/bcp47)valide. Pour obtenir la liste des langues, consultez le registre de la sous- [balise IANA Language](https://www.iana.org/assignments/language-subtag-registry).

Si vous souhaitez que votre application prenne en charge des langues d’affichage différentes, et que vous avez des littéraux de chaîne dans votre code ou dans votre balisage XAML, déplacez ces chaînes hors du code/balise et dans un fichier de ressources ( `.resw` ). Vous pouvez ensuite effectuer une copie traduite de ce fichier de ressources pour chaque langue prise en charge par votre application.

En général, vous utilisez un `language` qualificateur pour nommer les dossiers qui contiennent vos fichiers de ressources ( `.resw` ).

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Vous pouvez omettre la `language-` partie d’un `language` qualificateur (autrement dit, le nom du qualificateur). Vous ne pouvez pas effectuer cette opération avec les autres types de qualificateurs ; et vous ne pouvez le faire que dans un nom de dossier.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

Au lieu de nommer des dossiers, vous pouvez utiliser des `language` qualificateurs pour nommer les fichiers de ressources eux-mêmes.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Pour plus d’informations sur la localisation de votre application à l’aide de ressources de type chaîne, consultez [Localize Your UI](localize-strings-ui-manifest.md) Strings et guide pratique pour référencer une ressource de type chaîne dans votre application.

## <a name="layoutdirection"></a>LayoutDirection

Un `layoutdirection` qualificateur correspond à la direction de la disposition du paramètre de langue d’affichage. Par exemple, il peut être nécessaire de mettre en miroir une image pour une langue de droite à gauche, telle que l’arabe ou l’hébreu. Les panneaux de disposition et les images de votre interface utilisateur répondent au sens de la disposition de manière appropriée si vous définissez leur propriété [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consultez [ajuster la disposition et les polices et prenez en charge RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Toutefois, le `layoutdirection` qualificateur est pour les cas où le simple basculement n’est pas approprié et il vous permet de répondre à la direction de l’ordre de lecture et de l’alignement de texte spécifiques de manière plus générale.

## <a name="scale"></a>Scale

Windows sélectionne automatiquement un facteur d’échelle pour chaque affichage en fonction de son PPP (points par pouce) et de la distance d’affichage de l’appareil. Voir [pixels effectifs et facteur d’échelle](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Vous devez créer vos images à plusieurs tailles recommandées (au moins 100, 200 et 400) pour que Windows puisse choisir la taille parfaite ou utiliser la taille la plus proche et la mettre à l’échelle. Pour que Windows puisse identifier le fichier physique qui contient la taille correcte de l’image pour le facteur d’échelle d’affichage, vous utilisez un `scale` qualificateur. L’échelle d’une ressource correspond à la valeur de [DisplayInformation. ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale)ou de la ressource suivante à l’échelle la plus grande.

Voici un exemple de définition du qualificateur au niveau du dossier.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Et cet exemple le définit au niveau du fichier.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Pour plus d’informations sur la qualification d’une ressource pour `scale` et `targetsize` , consultez [qualifier une ressource d’image pour la cible](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>Taille

Le `targetsize` qualificateur est principalement utilisé pour spécifier les icônes d' [Association de type de fichier](/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type) ou les icônes de [protocole](/windows/desktop/search/-search-3x-wds-ph-ui-extensions) à afficher dans l’Explorateur de fichiers. La valeur du qualificateur représente la longueur latérale d’une image carrée en pixels bruts (physiques). La ressource dont la valeur correspond au paramètre d’affichage de l’Explorateur de fichiers est chargée ; ou la ressource avec la valeur suivante la plus grande en l’absence de correspondance exacte.

Vous pouvez définir des ressources qui représentent plusieurs tailles de `targetsize` valeur de qualificateur pour l’icône d’application ( `/Assets/Square44x44Logo.png` ) sous l’onglet ressources visuelles du concepteur de manifeste de package d’application.

Pour plus d’informations sur la qualification d’une ressource pour `scale` et `targetsize` , consultez [qualifier une ressource d’image pour la cible](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Thème

Le `theme` qualificateur est utilisé pour fournir les ressources qui correspondent le mieux au paramètre de mode d’application par défaut ou la substitution de votre application à l’aide de [application. RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme).


## <a name="shell-light-theme-and-unplated-resources"></a>Thème Light Shell et ressources implates
La *mise à jour de Windows 10 mai 2019* a introduit un nouveau thème « Light » pour le shell Windows. Par conséquent, certaines ressources d’application précédemment affichées sur un arrière-plan sombre s’affichent sur un arrière-plan clair. Pour les applications auxquelles les applications qui ont fourni altform des ressources non plaquées pour la barre des tâches et les sélecteurs de fenêtres (Alt + Tab, vue des tâches, etc.), vous devez vérifier qu’ils ont un contraste acceptable sur un arrière-plan clair.

### <a name="providing-light-theme-specific-assets"></a>Fourniture de ressources spécifiques à un thème clair
Les applications qui souhaitent fournir une ressource personnalisée pour le thème Light Shell peuvent utiliser un nouveau qualificateur de ressource de formulaire de substitution : `altform-lightunplated` . Ce qualificateur reflète le qualificateur altform-non-plaqué existant. 

### <a name="downlevel-considerations"></a>Considérations de niveau inférieur
Les applications ne doivent pas utiliser le `theme-light` qualificateur avec le `altform-unplated` qualificateur. Cela entraîne un comportement imprévisible sur RS5 et les versions antérieures de Windows en raison de la façon dont les ressources sont chargées pour la barre des tâches. Dans les versions antérieures de Windows, la version de Theme-Light peut être utilisée de manière incorrecte. Le `altform-lightunplated` qualificateur évite ce problème. 

### <a name="compatibility-behavior"></a>Comportement de compatibilité
À des fins de compatibilité descendante, Windows comprend une logique permettant de détecter une icône monochromatique et de vérifier si elle contraste avec l’arrière-plan prévu. Si l’icône ne répond pas aux exigences de contraste, Windows recherche une version blanche du contraste de la ressource. Si ce n’est pas possible, Windows revient à l’utilisation de la version plaquée de la ressource.



## <a name="important-apis"></a>API importantes

* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>Rubriques connexes

* [Pixels effectifs et facteur d’échelle](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Système de gestion des ressources](resource-management-system.md)
* [Comment préparer la localisation](/previous-versions/windows/apps/hh967762(v=win.10))
* [Détection de la plateforme d’exécution de votre application](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Programmation avec les SDK d’extension](/uwp/extension-sdks/device-families-overview)
* [Localiser vos chaînes d’interface utilisateur](localize-strings-ui-manifest.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [Composition statistique des Nations Unies M49 composition des codes de région](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
* [Registre de sous-balise IANA Language](https://www.iana.org/assignments/language-subtag-registry)
* [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)