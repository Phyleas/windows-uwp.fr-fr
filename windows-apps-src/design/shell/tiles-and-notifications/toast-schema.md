---
Description: L’article suivant décrit toutes les propriétés et tous les éléments contenus dans Toast content.
title: Schéma du contenu de notification toast
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6399cb3aa6c22e188ed84941c3209632511d90e4
ms.sourcegitcommit: 8b01b9ab7293dad1259da32d1459fdd454796e12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92020169"
---
# <a name="toast-content-schema"></a>Schéma du contenu de notification toast

 

Les éléments suivants décrivent toutes les propriétés et tous les éléments du contenu Toast.

Si vous préférez utiliser du code XML brut au lieu de la [bibliothèque de notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/), consultez [le schéma XML](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/schema-root).

[ToastContent](#toastcontent)
* [ToastVisual](#toastvisual)
  * [ToastBindingGeneric](#toastbindinggeneric)
    * [IToastBindingGenericChild](#itoastbindinggenericchild)
    * [ToastGenericAppLogo](#toastgenericapplogo)
    * [ToastGenericHeroImage](#toastgenericheroimage)
    * [ToastGenericAttributionText](#toastgenericattributiontext)
* [IToastActions](#itoastactions)
* [ToastAudio](#toastaudio)
* [ToastHeader](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent est l’objet de niveau supérieur qui décrit le contenu d’une notification, y compris les éléments visuels, les actions et l’audio.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Lancer**| string | false | Chaîne transmise à l’application lorsqu’elle est activée par le Toast. L’application définit le format et le contenu de cette chaîne pour son propre usage. Lorsque l’utilisateur appuie ou clique sur le toast pour lancer l’application associée, la chaîne de lancement fournit le contexte à l’application qui lui permet de montrer à l’utilisateur une vue pertinente pour le contenu du Toast, plutôt que de le lancer par défaut. |
| **Élément visuel** | [ToastVisual](#toastvisual) | true | Décrit la partie visuelle de la notification Toast. |
| **Actions** | [IToastActions](#itoastactions) | false | Créez éventuellement des actions personnalisées avec des boutons et des entrées. |
| **Audio** | [ToastAudio](#toastaudio) | false | Décrit la partie audio de la notification Toast. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Spécifie le type d’activation qui sera utilisé lorsque l’utilisateur cliquera sur le corps de ce Toast. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveautés de Creators Update : options supplémentaires relatives à l’activation de la notification Toast. |
| **Scénario** | [ToastScenario](#toastscenario) | false | Déclare le scénario pour lequel votre toast est utilisé, comme une alarme ou un rappel. |
| **DisplayTimestamp** | DateTimeOffset? | false | Nouveautés de Creators Update : remplacez l’horodateur par défaut par un horodateur personnalisé qui représente le moment où le contenu de la notification a été remis, et non l’heure à laquelle la notification a été reçue par la plateforme Windows. |
| **En-tête** | [ToastHeader](#toastheader) | false | Nouveautés de Creators Update : ajoutez un en-tête personnalisé à votre notification pour regrouper plusieurs notifications dans le centre de maintenance. |


### <a name="toastscenario"></a>ToastScenario
Spécifie le scénario représenté par le Toast.

| Valeur | Signification |
|---|---|
| **Par défaut** | Comportement normal du Toast. |
| **Rappel** | Notification de rappel. Elle sera affichée au préalable et rester sur l’écran de l’utilisateur jusqu’à ce qu’elle soit fermée. |
| **Alarme** | Une notification d’alarme. Elle sera affichée au préalable et rester sur l’écran de l’utilisateur jusqu’à ce qu’elle soit fermée. L’audio est bouclé par défaut et utilise l’audio-alarme. |
| **IncomingCall** | Notification d’appel entrant. Elle sera affichée au préalable dans un format d’appel spécial et rester sur l’écran de l’utilisateur jusqu’à ce qu’elle soit fermée. L’audio est en boucle par défaut et utilise l’audio de sonnerie. |


## <a name="toastvisual"></a>ToastVisual
La partie visuelle des toasts contient les liaisons, qui contiennent du texte, des images, du contenu adaptatif, et bien plus encore.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **BindingGeneric** | [ToastBindingGeneric](#toastbindinggeneric) | true | Liaison Toast générique, qui peut être rendue sur tous les appareils. Cette liaison est obligatoire et ne peut pas être null. |
| **BaseUri** | Uri | false | URL de base par défaut associée à des URL relatives dans des attributs de source d’image. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |
| **Langage**| string | false | Paramètres régionaux cibles de la charge utile du visuel lors de l’utilisation de ressources localisées, spécifiées sous la forme de balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans la liaison ou le texte. S’il n’est pas fourni, les paramètres régionaux système seront utilisés à la place. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
La liaison générique est la liaison par défaut pour les toasts, et est l’endroit où vous spécifiez le texte, les images, le contenu adaptatif, et bien plus encore.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | Contenu du corps du Toast, qui peut inclure du texte, des images et des groupes (ajouté dans la mise à jour anniversaire). Les éléments de texte doivent précéder tous les autres éléments, et seuls 3 éléments de texte sont pris en charge. Si un élément de texte est placé après un autre élément, il est placé en haut ou supprimé. Enfin, certaines propriétés de texte telles que HintStyle ne sont pas prises en charge sur les éléments de texte enfants racine et ne fonctionnent qu’à l’intérieur d’un AdaptiveSubgroup. Si vous utilisez AdaptiveGroup sur des appareils sans la mise à jour anniversaire, le contenu du groupe sera simplement supprimé. |
| **AppLogoOverride** | [ToastGenericAppLogo](#toastgenericapplogo) | false | Un logo facultatif pour remplacer le logo de l’application. |
| **HeroImage** | [ToastGenericHeroImage](#toastgenericheroimage) | false | Une image « héros » proposée en option et affichée sur le Toast et dans le centre de maintenance. |
| **Attribution** | [ToastGenericAttributionText](#toastgenericattributiontext) | false | Texte d’attribution facultatif qui sera affiché en bas de la notification Toast. |
| **BaseUri** | Uri | false | URL de base par défaut associée à des URL relatives dans des attributs de source d’image. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |
| **Langage**| string | false | Paramètres régionaux cibles de la charge utile du visuel lors de l’utilisation de ressources localisées, spécifiées sous la forme de balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Ces paramètres régionaux sont remplacés par les paramètres régionaux spécifiés dans la liaison ou le texte. S’il n’est pas fourni, les paramètres régionaux système seront utilisés à la place. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Interface de marqueur pour les éléments enfants Toast qui incluent du texte, des images, des groupes, etc.

| Implémentations |
| --- |
| [AdaptiveText](#adaptivetext) |
| [AdaptiveImage](#adaptiveimage) |
| [AdaptiveGroup](#adaptivegroup) |
| [AdaptiveProgressBar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Élément de texte adaptatif. S’il est placé dans le niveau supérieur ToastBindingGeneric. Children, seul HintMaxLines sera appliqué. Mais s’il est placé en tant qu’enfant d’un groupe ou d’un sous-groupe, le style de texte intégral est pris en charge.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Text** | chaîne ou [BindableString](#bindablestring) | false | Texte à afficher. La prise en charge de la liaison de données a été ajoutée dans Creators Update, mais fonctionne uniquement pour les éléments de texte de niveau supérieur. |
| **HintStyle** | [AdaptiveTextStyle](#adaptivetextstyle) | false | Le style contrôle la taille de police, le poids et l’opacité du texte. Fonctionne uniquement pour les éléments de texte à l’intérieur d’un groupe ou d’un sous-groupe. |
| **HintWrap** | Boolean? | false | Affectez la valeur true pour activer l’habillage du texte. Les éléments de texte de niveau supérieur ignorent cette propriété et encapsulent toujours (vous pouvez utiliser HintMaxLines = 1 pour désactiver l’encapsulation pour les éléments de texte de niveau supérieur). Les éléments de texte dans les groupes/sous-groupes ont par défaut la valeur false pour l’encapsulation. |
| **HintMaxLines** | int? | false | Nombre maximal de lignes que l’élément de texte est autorisé à afficher. |
| **HintMinLines** | int? | false | Nombre minimal de lignes que l’élément de texte doit afficher. Fonctionne uniquement pour les éléments de texte à l’intérieur d’un groupe ou d’un sous-groupe. |
| **HintAlign** | [AdaptiveTextAlign](#adaptivetextalign) | false | Alignement horizontal du texte. Fonctionne uniquement pour les éléments de texte à l’intérieur d’un groupe ou d’un sous-groupe. |
| **Langage** | string | false | Paramètres régionaux cibles de la charge XML, spécifiés en tant que balises de langue BCP-47 telles que « en-US » ou « fr-FR ». Les paramètres régionaux spécifiés ici remplacent tous les autres paramètres régionaux spécifiés, tels que ceux de la liaison ou de l’visuel. Si cette valeur est une chaîne littérale, cet attribut prend par défaut la langue de l’interface utilisateur de l’utilisateur. Si cette valeur est une référence de chaîne, cet attribut prend par défaut les paramètres régionaux choisis par Windows Runtime pour résoudre la chaîne. |


### <a name="bindablestring"></a>BindableString
Valeur de liaison pour les chaînes.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **BindingName** | string | true | Obtient ou définit le nom qui correspond à la valeur de vos données de liaison. |


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
| **HintCrop** | [AdaptiveImageCrop](#adaptiveimagecrop) | false | Nouveauté de la mise à jour anniversaire : contrôlez le rognage souhaité de l’image. |
| **HintRemoveMargin** | Boolean? | false | Par défaut, les images à l’intérieur de groupes/sous-groupes ont une marge 8px autour d’elles. Vous pouvez supprimer cette marge en affectant à cette propriété la valeur true. |
| **HintAlign** | [AdaptiveImageAlign](#adaptiveimagealign) | false | Alignement horizontal de l’image. Fonctionne uniquement pour les images à l’intérieur d’un groupe ou d’un sous-groupe. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


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
Nouveauté de la mise à jour anniversaire : les groupes identifient sémantiquement que le contenu du groupe doit être affiché dans son ensemble ou non affiché s’il ne l’est pas. Les groupes autorisent également la création de plusieurs colonnes.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Les sous-groupes sont affichés sous forme de colonnes verticales. Vous devez utiliser des sous-groupes pour fournir tout contenu à l’intérieur d’un AdaptiveGroup. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Nouveauté de la mise à jour anniversaire : les sous-groupes sont des colonnes verticales qui peuvent contenir du texte et des images.

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


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Nouveauté de Creators Update : barre de progression. Pris en charge uniquement sur les toasts sur le bureau, version 15063 ou ultérieure.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Titre** | chaîne ou [BindableString](#bindablestring) | false | Obtient ou définit une chaîne de titre facultative. Prend en charge la liaison de données. |
| **Valeur** | double ou [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) ou [BindableProgressBarValue](#bindableprogressbarvalue) | false | Obtient ou définit la valeur de la barre de progression. Prend en charge la liaison de données. La valeur par défaut est 0. |
| **ValueStringOverride** | chaîne ou [BindableString](#bindablestring) | false | Obtient ou définit une chaîne facultative à afficher à la place de la chaîne de pourcentage par défaut. Si cette valeur n’est pas fournie, un nom similaire à « 70% » s’affiche. |
| **État** | chaîne ou [BindableString](#bindablestring) | true | Obtient ou définit une chaîne d’État (obligatoire), qui s’affiche sous la barre de progression sur la gauche. Cette chaîne doit refléter l’état de l’opération, par exemple « téléchargement... » ou « installation... » |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Classe qui représente la valeur de la barre de progression.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Valeur** | double | false | Obtient ou définit la valeur (0,0-1,0) représentant le pourcentage effectué. |
| **IsIndeterminate** | bool | false | Obtient ou définit une valeur indiquant si la barre de progression est indéterminée. Si la valeur est true, la **valeur** sera ignorée. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Valeur de barre de progression pouvant être liée.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **BindingName** | string | true | Obtient ou définit le nom qui correspond à la valeur de vos données de liaison. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Logo à afficher à la place du logo de l’application.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http sont pris en charge. La taille des images http doit être inférieure ou égale à 200 Ko. |
| **HintCrop** | [ToastGenericAppLogoCrop](#toastgenericapplogocrop) | false | Spécifiez comment vous souhaitez que l’image soit rognée. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Contrôle le rognage de l’image du logo de l’application.

| Valeur | Signification |
|---|---|
| **Par défaut** | Le rognage utilise le comportement par défaut du convertisseur. |
| **Aucun** | L’image n’est pas rognée, carrée affichée. |
| **Circle** | L’image est rognée en cercle. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Une image « héros » proposée qui est affichée sur le Toast et dans le centre de maintenance.

| Propriété | Type | Obligatoire |Description |
|---|---|---|---|
| **Source** | string | true | URL de l’image. MS-AppX, MS-AppData et http sont pris en charge. La taille des images http doit être inférieure ou égale à 200 Ko. |
| **AlternateText** | string | false | Texte de remplacement décrivant l’image, utilisé à des fins d’accessibilité. |
| **AddImageQuery** | Boolean? | false | Affectez la valeur « true » pour autoriser Windows à ajouter une chaîne de requête à l’URL de l’image fournie dans la notification Toast. Utilisez cet attribut si votre serveur héberge des images et peut gérer des chaînes de requête, soit en extrayant une variante d’image basée sur les chaînes de requête, soit en ignorant la chaîne de requête et en retournant l’image telle qu’elle est spécifiée sans la chaîne de requête. Cette chaîne de requête spécifie la mise à l’échelle, le paramètre de contraste et la langue. par exemple, la valeur « www.website.com/images/hello.png » indiquée dans la notification devient « www.website.com/images/hello.png ? MS-Scale = 100&MS-Contrast = standard&MS-lang = en-US » |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Texte d’attribution affiché en bas de la notification Toast.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Text** | string | true | Texte à afficher. |
| **Langage** | string | false | Paramètres régionaux cibles de la charge utile du visuel lors de l’utilisation de ressources localisées, spécifiées sous la forme de balises de langue BCP-47 telles que « en-US » ou « fr-FR ». S’il n’est pas fourni, les paramètres régionaux système seront utilisés à la place. |


## <a name="itoastactions"></a>IToastActions
Interface de marqueur pour les actions/entrées Toast.

| Implémentations |
| --- |
| [ToastActionsCustom](#toastactionscustom) |
| [ToastActionsSnoozeAndDismiss](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implémente [IToastActions](#itoastactions)*

Créez vos propres actions et entrées personnalisées à l’aide de contrôles tels que des boutons, des zones de texte et des entrées de sélection.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Entrées** | IList<[IToastInput](#itoastinput)> | false | Entrées comme des zones de texte et des entrées de sélection. Seules 5 entrées sont autorisées. |
| **Boutons** | IList<[IToastButton](#itoastbutton)> | false | Les boutons s’affichent après toutes les entrées (ou adjacentes à une entrée si le bouton est utilisé comme un bouton de réponse rapide). Seuls les boutons jusqu’à 5 sont autorisés (ou moins si vous avez également des éléments de menu contextuel). |
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Nouveauté de la mise à jour anniversaire : éléments de menu contextuel personnalisés, fournissant des actions supplémentaires si l’utilisateur clique avec le bouton droit sur la notification. Vous pouvez *combiner*jusqu’à 5 boutons et éléments de menu contextuel. |


## <a name="itoastinput"></a>IToastInput
Interface de marqueur pour les entrées Toast.

| Implémentations |
| --- |
| [ToastTextBox](#toasttextbox) |
| [ToastSelectionBox](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implémente [IToastInput](#itoastinput)*

Contrôle de zone de texte dans lequel l’utilisateur peut taper du texte.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Id** | string | true | L’ID est obligatoire et est utilisé pour mapper le texte entré par l’utilisateur dans une paire clé-valeur d’ID/valeur que votre application consomme plus tard. |
| **Titre** | string | false | Texte de titre à afficher au-dessus de la zone de texte. |
| **PlaceholderContent** | string | false | Texte d’espace réservé à afficher dans la zone de texte lorsque l’utilisateur n’a pas encore saisi de texte. |
| **DefaultInput** | string | false | Texte initial à placer dans la zone de texte. Laissez cette valeur null pour une zone de texte vide. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implémente [IToastInput](#itoastinput)*

Contrôle de zone de sélection, qui permet aux utilisateurs de choisir dans une liste déroulante d’options.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Id** | string | true | L’ID est obligatoire. Si l’utilisateur a sélectionné cet élément, cet ID sera renvoyé au code de votre application, représentant la sélection choisie. |
| **Contenu** | string | true | Le contenu est obligatoire et est une chaîne qui est affichée sur l’élément de sélection. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Élément de zone de sélection (élément que l’utilisateur peut sélectionner dans la liste déroulante).

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Id** | string | true | L’ID est obligatoire et est utilisé pour mapper le texte entré par l’utilisateur dans une paire clé-valeur d’ID/valeur que votre application consomme plus tard. |
| **Titre** | string | false | Texte du titre à afficher au-dessus de la zone de sélection. |
| **DefaultSelectionBoxItemId** | string | false | Ce paramètre contrôle l’élément sélectionné par défaut, et fait référence à la propriété ID de l' [ToastSelectionBoxItem](#toastselectionboxitem). Si vous ne fournissez pas ce paramètre, la sélection par défaut est vide (l’utilisateur ne voit rien). |
| **Éléments** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Éléments de sélection à partir desquels l’utilisateur peut les sélectionner dans ce SelectionBox. Seulement 5 éléments peuvent être ajoutés. |


## <a name="itoastbutton"></a>IToastButton
Interface de marqueur pour les boutons Toast.

| Implémentations |
| --- |
| [ToastButton](#toastbutton) |
| [ToastButtonSnooze](#toastbuttonsnooze) |
| [ToastButtonDismiss](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implémente [IToastButton](#itoastbutton)*

Bouton sur lequel l’utilisateur peut cliquer.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Contenu** | string | true | Obligatoire. Texte à afficher sur le bouton. |
| **Arguments** | string | true | Obligatoire. Chaîne d’arguments définie par l’application, que l’application recevra plus tard si l’utilisateur clique sur ce bouton. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Contrôle le type d’activation que ce bouton utilisera lorsque vous cliquez dessus. La valeur par défaut est premier plan. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveautés de Creators Update : Obtient ou définit des options supplémentaires relatives à l’activation du bouton Toast. |


### <a name="toastactivationtype"></a>ToastActivationType
Détermine le type d’activation qui sera utilisé lorsque l’utilisateur interagit avec une action spécifique.

| Valeur | Signification |
|---|---|
| **Premier plan** | Valeur par défaut. Votre application de premier plan est lancée. |
| **Contexte** | La tâche en arrière-plan correspondante (en supposant que vous configurez tout) est déclenchée et vous pouvez exécuter du code en arrière-plan (comme l’envoi du message de réponse rapide de l’utilisateur) sans interrompre l’utilisateur. |
| **Protocole** | Lancez une autre application à l’aide de l’activation du protocole. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Nouveautés de Creators Update : options supplémentaires relatives à l’activation.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **AfterActivationBehavior** | [ToastAfterActivationBehavior](#toastafteractivationbehavior) | false | Nouveautés de la mise à jour des créateurs de automne : Obtient ou définit le comportement que le Toast doit utiliser lorsque l’utilisateur appelle cette action. Cela fonctionne uniquement sur le bureau, pour [ToastButton](#toastbutton) et [ToastContextMenuItem](#toastcontextmenuitem). |
| **ProtocolActivationTargetApplicationPfn** | string | false | Si vous utilisez *ToastActivationType. Protocol*, vous pouvez éventuellement spécifier le PFN cible, de sorte que si plusieurs applications sont inscrites pour gérer le même URI de protocole, l’application souhaitée sera toujours lancée. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Spécifie le comportement que le Toast doit utiliser lorsque l’utilisateur effectue une action sur le Toast.

| Valeur | Signification |
|---|---|
| **Par défaut** | Comportement par défaut Le Toast sera rejeté lorsque l’utilisateur entreprendra une action sur le Toast. |
| **PendingUpdate** | Une fois que l’utilisateur a cliqué sur un bouton de votre toast, la notification restera présente dans un état visuel « mise à jour en attente ». Vous devez immédiatement mettre à jour votre toast à partir d’une tâche en arrière-plan afin que l’utilisateur ne voit pas l’état visuel « mise à jour en attente » pendant trop longtemps. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implémente [IToastButton](#itoastbutton)*

Bouton répéter géré par le système qui gère automatiquement la répétition de la notification.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **CustomContent** | string | false | Texte personnalisé facultatif affiché sur le bouton qui remplace le texte « répéter » localisé par défaut. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implémente [IToastButton](#itoastbutton)*

Bouton d’annulation géré par le système qui ignore la notification quand l’utilisateur clique dessus.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **CustomContent** | string | false | Texte personnalisé facultatif affiché sur le bouton qui remplace le texte « faire disparaître » localisé par défaut. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
* Implémente [IToastActions](#itoastactions)

Construit automatiquement une zone de sélection pour les intervalles de répétition et les boutons répéter/ignorer, tous les paramètres localisés automatiquement et la logique de répétition est géré automatiquement par le système.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **ContextMenuItems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Nouveauté de la mise à jour anniversaire : éléments de menu contextuel personnalisés, fournissant des actions supplémentaires si l’utilisateur clique avec le bouton droit sur la notification. Vous pouvez avoir jusqu’à 5 éléments. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Entrée d’élément de menu contextuel.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Contenu** | string | true | Obligatoire. Texte à afficher. |
| **Arguments** | string | true | Obligatoire. Chaîne d’arguments définie par l’application, que l’application peut récupérer ultérieurement une fois qu’elle est activée quand l’utilisateur clique sur l’élément de menu. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Contrôle le type d’activation que cet élément de menu utilisera quand vous cliquez dessus. La valeur par défaut est premier plan. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Nouveautés de Creators Update : options supplémentaires relatives à l’activation de l’élément de menu contextuel Toast. |


## <a name="toastaudio"></a>ToastAudio
Spécifiez l’audio à lire lors de la réception de la notification Toast.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Sources** | URI | false | Fichier multimédia à lire à la place du son par défaut. Seuls MS-AppX et MS-AppData sont pris en charge. |
| **Circuit** | boolean | false | Affectez la valeur true si le son doit être répété tant que le toast est affiché. false pour lire une seule fois (valeur par défaut). |
| **Sans assistance** | boolean | false | True pour désactiver le son ; false pour permettre la lecture du son de notification Toast (par défaut). |


## <a name="toastheader"></a>ToastHeader
Nouveautés de Creators Update : un en-tête personnalisé qui regroupe plusieurs notifications dans le centre de maintenance.

| Propriété | Type | Obligatoire | Description |
|---|---|---|---|
| **Id** | string | true | Identificateur créé par le développeur qui identifie de façon unique cet en-tête. Si deux notifications ont le même ID d’en-tête, elles s’affichent sous le même en-tête dans le centre de maintenance. |
| **Titre** | string | true | Titre de l’en-tête. |
| **Arguments**| string | true | Obtient ou définit une chaîne d’arguments définie par le développeur qui est retournée à l’application lorsque l’utilisateur clique sur cet en-tête. Ne peut pas être null. |
| **ActivationType** | [ToastActivationType](#toastactivationtype) | false | Obtient ou définit le type d’activation que cet en-tête utilisera quand vous cliquez dessus. La valeur par défaut est premier plan. Notez que seul le premier plan et le protocole sont pris en charge. |
| **ActivationOptions** | [ToastActivationOptions](#toastactivationoptions) | false | Obtient ou définit des options supplémentaires relatives à l’activation de l’en-tête Toast. |


## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : Envoyer une notification toast et gérer l’activation](/archive/blogs/tiles_and_toasts/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10)
* [Bibliothèque Notifications sur GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
