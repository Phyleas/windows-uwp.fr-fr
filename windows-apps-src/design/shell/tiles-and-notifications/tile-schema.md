---
description: L’article suivant décrit toutes les propriétés et tous les éléments dans le contenu de la mosaïque.
title: Schéma de contenu de vignette
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: Windows 10, UWP, vignette, notification par vignette, contenu de la vignette, schéma, charge utile de vignette
ms.localizationpriority: medium
ms.openlocfilehash: 4d1953e6d745a41c3bdd85d5f4dd3c6c9df8b900
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034412"
---
# <a name="tile-content-schema"></a>Schéma de contenu de vignette

 

Les éléments suivants décrivent toutes les propriétés et tous les éléments contenus dans la mosaïque.

Si vous préférez utiliser du code XML brut au lieu de la [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consultez [le schéma XML](../tiles-and-notifications/adaptive-tiles-schema.md).

[TileContent](#tilecontent)
* [TileVisual](#tilevisual)
  * [TileBinding](#tilebinding)
    * [TileBindingContentAdaptive](#tilebindingcontentadaptive)
    * [TileBindingContentIconic](#tilebindingcontenticonic)
    * [TileBindingContentContact](#tilebindingcontentcontact)
    * [TileBindingContentPeople](#tilebindingcontentpeople)
    * [TileBindingContentPhotos](#tilebindingcontentphotos)


## <a name="tilecontent"></a>TileContent
TileContent est l’objet de niveau supérieur qui décrit le contenu d’une notification de vignette, y compris les éléments visuels.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Éléments visuels** | [ToastVisual](#tilevisual) | true | Décrit la partie visuelle de la notification de vignette. |


## <a name="tilevisual"></a>TileVisual
La partie visuelle des vignettes contient les spécifications visuelles pour toutes les tailles de mosaïques, et davantage de propriétés visuelles.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **TileSmall** | [TileBinding](#tilebinding) | false | Fournissez une petite liaison facultative pour spécifier le contenu pour la petite taille de mosaïque. |
| **TileMedium** | [TileBinding](#tilebinding) | false | Fournissez une liaison moyenne facultative pour spécifier le contenu pour la taille de la mosaïque moyenne. |
| **TileWide** | [TileBinding](#tilebinding) | false | Fournissez une liaison étendue facultative pour spécifier le contenu pour la taille de la mosaïque étendue. |
| **TileLarge** | [TileBinding](#tilebinding) | false | Fournissez une grande liaison facultative pour spécifier le contenu pour la grande taille de mosaïque. |
| **Personnalisation** | TileBranding | false | Le formulaire que la vignette doit utiliser pour afficher la personnalisation de l’application. Par défaut, hérite de la personnalisation de la vignette par défaut. |
| **DisplayName** | string | false | Chaîne facultative pour remplacer le nom complet de la vignette tout en affichant cette notification. |
| **Arguments** | string | false | Nouveauté de la mise à jour anniversaire : données définies par l’application qui sont retournées à votre application via la propriété TileActivatedInfo sur LaunchActivatedEventArgs lorsque l’utilisateur lance votre application à partir de la vignette dynamique. Cela vous permet de connaître les notifications de vignette que votre utilisateur a vues lorsqu’il a appuyé sur votre vignette dynamique. Sur les appareils sans la mise à jour anniversaire, cela sera tout simplement ignoré. |
| **LockDetailedStatus1** | string | false | Si vous spécifiez cela, vous devez également fournir une liaison TileWide. Il s’agit de la première ligne de texte qui s’affichera sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette en tant qu’application d’État détaillée. |
| **LockDetailedStatus2** | string | false | Si vous spécifiez cela, vous devez également fournir une liaison TileWide. Il s’agit de la deuxième ligne de texte qui s’affichera sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette en tant qu’application d’État détaillée. |
| **LockDetailedStatus3** | string | false | Si vous spécifiez cela, vous devez également fournir une liaison TileWide. Il s’agit de la troisième ligne de texte qui s’affichera sur l’écran de verrouillage si l’utilisateur a sélectionné votre vignette en tant qu’application d’État détaillée. |
| **BaseUri** | Uri | false | URL de base par défaut associée à des URL relatives dans des attributs de source d’image. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |
| **Langage**| string | false | Paramètres régionaux cibles de la charge utile du visuel lors de l’utilisation de ressources localisées, spécifiées sous la forme de balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans la liaison ou le texte. S’il n’est pas fourni, les paramètres régionaux système seront utilisés à la place. |


## <a name="tilebinding"></a>TileBinding
L’objet de liaison contient le contenu visuel pour une taille de vignette spécifique.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Contenu** | [ITileBindingContent](#itilebindingcontent) | false | Contenu visuel à afficher sur la vignette. L’une des [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#tilebindingcontenticonic), [TileBindingContentContact](#tilebindingcontentcontact), [TileBindingContentPeople](#tilebindingcontentpeople)ou [TileBindingContentPhotos](#tilebindingcontentphotos). |
| **Personnalisation** | TileBranding | false | Le formulaire que la vignette doit utiliser pour afficher la personnalisation de l’application. Par défaut, hérite de la personnalisation de la vignette par défaut. |
| **DisplayName** | string | false | Chaîne facultative pour remplacer le nom complet de la vignette pour cette taille de mosaïque. |
| **Arguments** | string | false | Nouveauté de la mise à jour anniversaire : données définies par l’application qui sont retournées à votre application via la propriété TileActivatedInfo sur LaunchActivatedEventArgs lorsque l’utilisateur lance votre application à partir de la vignette dynamique. Cela vous permet de connaître les notifications de vignette que votre utilisateur a vues lorsqu’il a appuyé sur votre vignette dynamique. Sur les appareils sans la mise à jour anniversaire, cela sera tout simplement ignoré. |
| **BaseUri** | Uri | false | URL de base par défaut associée à des URL relatives dans des attributs de source d’image. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |
| **Langage**| string | false | Paramètres régionaux cibles de la charge utile du visuel lors de l’utilisation de ressources localisées, spécifiées sous la forme de balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans la liaison ou le texte. S’il n’est pas fourni, les paramètres régionaux système seront utilisés à la place. |


## <a name="itilebindingcontent"></a>ITileBindingContent
Interface de marqueur pour le contenu de liaison de mosaïque. Celles-ci vous permettent de choisir ce que vous souhaitez spécifier pour les visuels de vignette, ou l’un des modèles spéciaux.

| Implémentations |
| --- |
| [TileBindingContentAdaptive](#tilebindingcontentadaptive) |
| [TileBindingContentIconic](#tilebindingcontenticonic) |
| [TileBindingContentContact](#tilebindingcontentcontact) |
| [TileBindingContentPeople](#tilebindingcontentpeople) |
| [TileBindingContentPhotos](#tilebindingcontentphotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Pris en charge sur toutes les tailles. Il s’agit de la méthode recommandée pour spécifier le contenu de votre mosaïque. Les modèles de mosaïque adaptative, nouveaux dans Windows 10, vous permettent de créer un large éventail de vignettes personnalisées via Adaptive.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Children** | IList<ITileBindingContentAdaptiveChild> | false | Éléments visuels inclus. Les objets [AdaptiveText](#adaptivetext), [AdaptiveImage](#adaptiveimage)et [AdaptiveGroup](#adaptivegroup) peuvent être ajoutés. Les enfants sont affichés de façon verticale. |
| **BackgroundImage** | [TileBackgroundImage](#tilebackgroundimage) | false | Image d’arrière-plan facultative qui est affichée derrière le contenu de la mosaïque, le fond perdu complet. |
| **PeekImage** | [TilePeekImage](#tilepeekimage) | false | Image d’aperçu facultative qui s’anime à partir du haut de la vignette. |
| **TextStacking** | [TileTextStacking](#tiletextstacking) | false | Contrôle l’empilement du texte (alignement vertical) du contenu des enfants dans son ensemble. |


## <a name="adaptivetext"></a>AdaptiveText
Élément de texte adaptatif.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Text** | string | false | Texte à afficher. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | Le style contrôle la taille de police, le poids et l’opacité du texte. |
| **HintWrap** | Boolean? | false | Affectez la valeur true pour activer l’habillage du texte. La valeur par défaut est false. |
| **HintMaxLines** | int? | false | Nombre maximal de lignes que l’élément de texte est autorisé à afficher. |
| **HintMinLines** | int? | false | Nombre minimal de lignes que l’élément de texte doit afficher. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alignement horizontal du texte. |
| **Langage** | string | false | Paramètres régionaux cibles de la charge XML, spécifiés en tant que balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, tels que ceux de la liaison ou de l’visuel. Si cette valeur est une chaîne littérale, cet attribut prend par défaut la langue de l’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut prend par défaut les paramètres régionaux choisis par Windows Runtime pour résoudre la chaîne. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Style de texte contrôle la taille de police, le poids et l’opacité. Une opacité subtile est de 60% opaque.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. Le style est déterminé par le convertisseur. |
| **Caption** | Inférieure à la taille de police des paragraphes. |
| **CaptionSubtle** | Identique à la légende, mais avec une opacité subtile. |
| **Corps** | Taille de police des paragraphes. |
| **BodySubtle** | Identique au corps, mais avec une opacité subtile. |
| **Base** | Taille de police des paragraphes, épaisseur gras. Fondamentalement la version en gras du corps. |
| **BaseSubtle** | Identique à la base, mais avec une opacité subtile. |
| **Sous-titre** | Taille de police de H4. |
| **SubtitleSubtle** | Identique au sous-titre, mais avec une opacité subtile. |
| **Titre** | Taille de police H3. |
| **TitleSubtle** | Identique au titre, mais avec une opacité subtile. |
| **TitleNumeral** | Identique au titre, mais avec la marge supérieure/inférieure supprimée. |
| **En-tête** | Taille de police H2. |
| **SubheaderSubtle** | Identique à l’en-tête, mais avec une opacité subtile. |
| **SubheaderNumeral** | Identique à l’en-tête, mais avec la marge supérieure/inférieure supprimée. |
| **En-tête** | Taille de police H1. |
| **HeaderSubtle** | Identique à l’en-tête, mais avec une opacité subtile. |
| **HeaderNumeral** | Identique à l’en-tête, mais avec la marge supérieure/inférieure supprimée. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Contrôle les alignements horizontaux du texte.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. L’alignement est déterminé automatiquement par le convertisseur. |
| **Automatique** | Alignement déterminé par la langue et la culture actuelles. |
| **Left** | Aligne horizontalement le texte à gauche. |
| **Gestionnaire** | Aligne horizontalement le texte au centre. |
| **Right** | Aligne horizontalement le texte à droite. |


## <a name="adaptiveimage"></a>AdaptiveImage
Image incluse.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http sont pris en charge. À compter de la mise à jour des créateurs de automne, les images Web peuvent atteindre jusqu’à 3 Mo sur des connexions normales et 1 Mo sur les connexions limitées. Sur les appareils qui n’exécutent pas encore la mise à jour des créateurs de automne, les images Web ne doivent pas dépasser 200 Ko. |
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Contrôlez le rognage souhaité de l’image. |
| **HintRemoveMargin** | Boolean? | false | Par défaut, les images à l’intérieur de groupes/sous-groupes ont une marge 8px autour d’elles. Vous pouvez supprimer cette marge en affectant à cette propriété la valeur true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alignement horizontal de l’image. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification de vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Spécifie le rognage souhaité de l’image.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. Comportement du rognage déterminé par le convertisseur. |
| **Aucun** | L’image n’est pas rognée. |
| **Circle** | L’image est rognée en une forme de cercle. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Spécifie l’alignement horizontal d’une image.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. Comportement d’alignement déterminé par le convertisseur. |
| **Stretch** | L’image est étirée pour remplir la largeur disponible (et éventuellement la hauteur disponible, en fonction de l’emplacement où l’image est placée). |
| **Left** | Alignez l’image à gauche, en affichant l’image à sa résolution native. |
| **Gestionnaire** | Alignez l’image dans le centre horizontalement, displayign l’image à sa résolution native. |
| **Right** | Alignez l’image à droite, en affichant l’image à sa résolution native. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Les groupes identifient de façon sémantique que le contenu du groupe doit être affiché dans son ensemble ou non affiché s’il ne l’est pas. Les groupes autorisent également la création de plusieurs colonnes.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Les sous-groupes sont affichés sous forme de colonnes verticales. Vous devez utiliser des sous-groupes pour fournir tout contenu à l’intérieur d’un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Les sous-groupes sont des colonnes verticales qui peuvent contenir du texte et des images.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) et [AdaptiveImage](#adaptiveimage) sont des enfants valides de sous-groupes. |
| **HintWeight** | int? | false | Contrôlez la largeur de cette colonne de sous-groupe en spécifiant le poids, par rapport aux autres sous-groupes. |
| **HintTextStacking** | [AdaptiveSubgroupTextStacking](#adaptivesubgrouptextstacking) | false | Contrôlez l’alignement vertical du contenu de ce sous-groupe. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Interface de marqueur pour les enfants de sous-groupe.

| Implémentations |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking spécifie l’alignement vertical du contenu.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. Le convertisseur sélectionne automatiquement l’alignement vertical par défaut. |
| **Top** | Alignement vertical en haut. |
| **Gestionnaire** | Alignement vertical au centre. |
| **Bas** | Alignement vertical en bas. |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Image d’arrière-plan affichée en plein-fond perdu sur la vignette.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http (s) sont pris en charge. La taille des images http doit être inférieure ou égale à 200 Ko. |
| **HintOverlay** | int? | false | Superposition noire sur l’image d’arrière-plan. Cette valeur contrôle l’opacité de la superposition noire, 0 sans superposition et 100 entièrement noir. La valeur par défaut est 20. |
| **HintCrop** | [TileBackgroundImageCrop](#tilebackgroundimagecrop) | false | Nouveauté de 1511 : Spécifiez comment vous souhaitez que l’image soit rognée. Dans les versions antérieures à 1511, cette opération sera ignorée et l’image d’arrière-plan sera affichée sans rognage. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification de vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Contrôle le rognage de l’image d’arrière-plan.

| Valeur | Signification |
|---|---|
| **Par défaut** | Le rognage utilise le comportement par défaut du convertisseur. |
| **Aucun** | L’image n’est pas rognée, carrée affichée. |
| **Circle** | L’image est rognée en cercle. |


## <a name="tilepeekimage"></a>TilePeekImage
Image d’aperçu qui s’anime à partir du haut de la vignette.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http (s) sont pris en charge. La taille des images http doit être inférieure ou égale à 200 Ko. |
| **HintOverlay** | int? | false | Nouveauté de 1511 : superposition noire sur l’image d’aperçu. Cette valeur contrôle l’opacité de la superposition noire, 0 sans superposition et 100 entièrement noir. La valeur par défaut est 20. Dans les versions précédentes, cette valeur sera ignorée et l’aperçu de l’image s’affichera avec 0 superposition. |
| **HintCrop** | [TilePeekImageCrop](#tilepeekimagecrop) | false | Nouveauté de 1511 : Spécifiez comment vous souhaitez que l’image soit rognée. Dans les versions antérieures à 1511, cette opération sera ignorée et l’aperçu de l’image s’affichera sans rognage. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification de vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Contrôle le rognage de l’image d’aperçu.

| Valeur | Signification |
|---|---|
| **Par défaut** | Le rognage utilise le comportement par défaut du convertisseur. |
| **Aucun** | L’image n’est pas rognée, carrée affichée. |
| **Circle** | L’image est rognée en cercle. |


### <a name="tiletextstacking"></a>TileTextStacking
La superposition de texte spécifie l’alignement vertical du contenu.

| Valeur | Signification |
|---|---|
| **Par défaut** | Valeur par défaut. Le convertisseur sélectionne automatiquement l’alignement vertical par défaut. |
| **Top** | Alignement vertical en haut. |
| **Gestionnaire** | Alignement vertical au centre. |
| **Bas** | Alignement vertical en bas. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Pris en charge sur les éditions petite et moyenne. Active un modèle de vignette sous forme, dans lequel une icône et un badge peuvent s’afficher à côté les uns des autres sur la vignette, dans le style de Windows Phone classique true. Le nombre en regard de l’icône est obtenu via une notification de badge distincte.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Icône** | [TileBasicImage](#tilebasicimage) | true | Au minimum, pour prendre en charge à la fois les vignettes de bureau et mobiles, petites et moyennes, fournissez une image de proportions carrées avec une résolution de 200x200, format PNG, avec transparence et aucune couleur autre que le blanc. Pour plus d’informations, consultez : [modèles de vignettes spéciaux](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Mobile uniquement. Pris en charge sur Small, medium et larges.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Image** | [TileBasicImage](#tilebasicimage) | true | Image à afficher. |
| **Text** | [TileBasicText](#tilebasictext) | false | Ligne de texte qui est affichée. Non affiché sur une petite vignette. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
Nouveauté de 1511 : prise en charge sur le niveau moyen, large et grand (bureau et mobile). Auparavant, cette dernière était mobile uniquement et moyenne et grande.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Images qui s’entourent de cercles. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
Anime un diaporama de photos. Pris en charge sur toutes les tailles.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Images** | IList<[TileBasicImage](#tilebasicimage)> | true | Vous pouvez fournir jusqu’à 12 images (Mobile affiche uniquement 9), qui sera utilisé pour le diaporama. Si vous ajoutez plus de 12, une exception est levée. |


### <a name="tilebasicimage"></a>TileBasicImage
Image utilisée sur différents modèles spéciaux.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http (s) sont pris en charge. La taille des images http doit être inférieure ou égale à 200 Ko. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification de vignette. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


### <a name="tilebasictext"></a>TileBasicText
Élément de texte de base utilisé sur différents modèles spéciaux.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Text** | string | false | Texte à afficher. |
| **Langage** | string | false | Paramètres régionaux cibles de la charge XML, spécifiés en tant que balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, tels que ceux de la liaison ou de l’visuel. Si cette valeur est une chaîne littérale, cet attribut prend par défaut la langue de l’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut prend par défaut les paramètres régionaux choisis par Windows Runtime pour résoudre la chaîne. |


## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Envoyer une notification par vignette locale](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Bibliothèque Notifications sur GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
