---
description: La rubrique précédente (Comment le système de gestion des ressources met en correspondance et sélectionne les ressources) aborde la correspondance entre les qualificateurs de manière générale. Cette rubrique se concentre plus particulièrement sur la correspondance entre les balises de langue.
title: Comment le système de gestion des ressources met en correspondance les balises de langue
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 3c5b2d595585cabdfb9f2983f2ad87f05b7fa5a4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031882"
---
# <a name="how-the-resource-management-system-matches-language-tags"></a>Comment le système de gestion des ressources met en correspondance les balises de langue

La rubrique précédente ([Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)) aborde la correspondance entre les qualificateurs de manière générale. Cette rubrique se concentre plus particulièrement sur la correspondance entre les balises de langue.

## <a name="introduction"></a>Introduction

Les ressources avec des qualificateurs de balise de langue sont comparées et évaluées en fonction de la liste des langues du runtime de l’application. Pour obtenir les définitions des différentes listes de langues, consultez comprendre les langages de [profil utilisateur et les langages du manifeste d’application](../design/globalizing/manage-language-and-region.md). La mise en correspondance pour la première langue d’une liste se produit avant la mise en correspondance de la deuxième langue d’une liste, même pour d’autres variantes régionales. Par exemple, une ressource de en-GB est choisie sur une ressource fr-CA si la langue d’exécution de l’application est en-US. Uniquement s’il n’y a pas de ressources pour une forme de en est une ressource pour fr-CA choisie (Notez que la langue par défaut de l’application n’a pas pu être définie sur une forme de en l’occurrence).

Le mécanisme de notation utilise les données incluses dans le registre de sous-balise [BCP-47](https://tools.ietf.org/html/bcp47) et d’autres sources de données. Il permet un dégradé de notation avec différentes qualités de correspondance et, lorsque plusieurs candidats sont disponibles, il sélectionne le candidat avec le score de correspondance le plus approprié.

Ainsi, vous pouvez baliser le contenu de la langue en termes génériques, mais vous pouvez toujours spécifier un contenu spécifique si nécessaire. Par exemple, votre application peut avoir de nombreuses chaînes en anglais qui sont communes à la États-Unis, au Royaume-Uni et à d’autres régions. Le marquage de ces chaînes en tant que « en » (anglais) économise de l’espace et la surcharge de localisation. Lorsque des différences doivent être faites, par exemple dans une chaîne contenant le mot « couleur/couleur », les États-Unis et les versions britanniques peuvent être balisées séparément à l’aide des sous-balises de langue et de région, comme « en-US » et « en-GB », respectivement.

## <a name="language-tags"></a>Balises de langue

Les langues sont identifiées à l’aide de balises de langue BCP-47 correctement formées et normalisées. Les composants sous-balise sont définis dans le registre de sous-balise BCP-47. La structure normale d’une balise de langage BCP-47 se compose d’un ou plusieurs des éléments de sous-balise suivants.

- Sous-balise de langue (obligatoire).
- Sous-balise de script (qui peut être déduite en utilisant la valeur par défaut spécifiée dans le registre de sous-balise).
- Sous-zone de région (facultatif).
- Sous-balise de variante (facultatif).

Des éléments de sous-balise supplémentaires peuvent être présents, mais ils auront un effet négligeable sur la correspondance de langue. Il n’existe aucune plage de langue définie à l’aide de la carte générique (« *»), par exemple, « en-* ».

## <a name="matching-two-languages"></a>Correspondance de deux langues

Chaque fois que Windows compare deux langages, il est généralement effectué dans le contexte d’un processus plus important. Il peut être dans le contexte de l’évaluation de plusieurs langues, par exemple lorsque Windows génère la liste de langues de l’application (voir comprendre les langages [de profil utilisateur et les langages du manifeste d’application](../design/globalizing/manage-language-and-region.md)). Pour ce faire, Windows met en correspondance plusieurs langues des préférences de l’utilisateur avec les langues spécifiées dans le manifeste de l’application. La comparaison peut également être dans le contexte de l’évaluation de la langue, ainsi que d’autres qualificateurs pour une ressource particulière. C’est le cas, par exemple, lorsque Windows résout une ressource de fichier particulière en un contexte de ressource particulier. avec l’emplacement d’hébergement de l’utilisateur ou la mise à l’échelle actuelle ou la résolution de l’appareil comme d’autres facteurs (en dehors du langage) qui sont pris en compte dans la sélection des ressources.

Lorsque deux balises de langue sont comparées, la comparaison se voit attribuer un score en fonction de la proximité de la correspondance.

| Correspond | Score | Exemple |
| ----- | ----- | ------- |
| Concordance exacte | Le plus élevé | en-au : en-au |
| Correspondance variant (langue, script, région, variante) |  | en-au-variant1 : en-au-variant1-t-ja |
| Correspondance de la région (langue, script, région) |  | en-au : en-au-variant1 |
| Correspondance partielle (langage, script) |  |  |
| -Correspondance de la zone de macro |  | en-au : en-053 |
| -Correspondance neutre pour la région |  | en-au : en |
| -Correspondance de l’affinité orthographique (prise en charge limitée) |  | en-au : en-Go |
| -Correspondance de la région préférée |  | en-au : en-US |
| -N’importe quelle région correspond |  | en-au : en-CA |
| Langue non déterminée (toutes les correspondances de langage) |  | en-au : und |
| Aucune correspondance (incompatibilité de script ou balise de langue principale incompatible) | Minimale | en-au : fr-FR |

### <a name="exact-match"></a>Concordance exacte

Les balises sont exactement égales (tous les éléments de sous-balise correspondent). Une comparaison peut être promue vers ce type de correspondance à partir d’une correspondance de variante ou de région. Par exemple, en-US correspond à en-US.

### <a name="variant-match"></a>Correspondance variant

Les balises correspondent aux sous-balises Language, script, Region et variant, mais elles diffèrent dans un autre domaine.

### <a name="region-match"></a>Correspondance de la région

Les balises correspondent aux sous-balises de langue, de script et de région, mais elles diffèrent dans un autre cas. Par exemple, de-DE-1996 correspond à-DE, et en-US-x-pirate correspond en-US.

### <a name="partial-matches"></a>Correspondances partielles

Les balises correspondent aux sous-balises Language et script, mais elles diffèrent dans la région ou une autre sous-balise. Par exemple, en-US correspond à, ou en-US correspond à en- \* .

#### <a name="macro-region-match"></a>Correspondance de la zone de macro

Les balises correspondent aux sous-balises Language et script. les deux balises ont des sous-balises Region, une qui désigne une région de macro englobant l’autre région. Les sous-balises de la région de macro sont toujours numériques et sont dérivées des codes de pays et de zone des statistiques des États-Unis. Pour plus d’informations sur l’englobement des relations, consultez [composition de régions géographiques (continent), sous-régions géographiques et regroupements économiques et autres sélectionnés](https://unstats.un.org/unsd/methods/m49/m49regin.htm).

**Remarque** Les codes d’absence pour « regroupements économiques » ou « autres regroupements » ne sont pas pris en charge dans BCP-47.
 
**Remarque** Une balise avec la sous-balise « 001 » de la zone macro est considérée comme équivalente à une étiquette indépendante de la région. Par exemple, « es-001 » et « es » sont traités comme synonymes.

#### <a name="region-neutral-match"></a>Correspondance neutre pour la région

Les balises correspondent aux sous-balises Language et script, et une seule balise a une balise Region. Une correspondance parente est préférable à d’autres correspondances partielles.

#### <a name="orthographic-affinity-match"></a>Correspondance d’affinité orthographique

Les balises correspondent aux sous-balises Language et script, et les sous-balises Region ont une affinité orthographique. L’affinité s’appuie sur les données conservées dans Windows qui définissent des régions d’affinité spécifiques à une langue, par exemple « en-IE » et « en-GB ».

#### <a name="preferred-region-match"></a>Correspondance de la région préférée

Les balises correspondent aux sous-balises Language et script, et l’une des sous-balises Region est la sous-balise Region par défaut de la langue. Par exemple, « fr-FR » est la région par défaut de la sous-balise « fr ». Par conséquent, fr-FR est une meilleure correspondance pour fr-is than is fr-CA. Cela s’appuie sur les données conservées dans Windows qui définissent une région par défaut pour chaque langue dans laquelle Windows est localisé.

#### <a name="sibling-match"></a>Correspondance sœur

Les balises correspondent aux sous-balises Language et script, et les deux possèdent des sous-balises Region, mais aucune autre relation n’est définie entre elles. Dans le cas de plusieurs correspondances sœurs, le dernier frère énuméré est le gagnant, en l’absence d’une correspondance plus élevée.

### <a name="undetermined-language"></a>Langue indéterminée

Une ressource peut être marquée comme « und » pour indiquer qu’elle correspond à n’importe quelle langue. Cette balise peut également être utilisée avec une balise de script pour filtrer les correspondances en fonction du script. Par exemple, « und-LATN » correspond à toute balise de langue qui utilise le script latin. Vous trouverez plus de détails à la suite.

### <a name="script-mismatch"></a>Incompatibilité de script

Lorsque les balises correspondent uniquement à la balise de langue primaire, mais pas au script, la paire est considérée comme ne correspondant pas et est notée sous le niveau d’une correspondance valide.

### <a name="no-match"></a>Pas de correspondance

Les sous-balises de langue primaire ne correspondent pas. elles sont évaluées sous le niveau d’une correspondance valide. Par exemple, zh-Hant ne correspond pas à zh-Hans.

## <a name="examples"></a>Exemples

La langue de l’utilisateur « zh-Hans-CN » (chinois simplifié (Chine)) correspond aux ressources suivantes dans l’ordre de priorité indiqué. Un X n’indique aucune correspondance.

![Correspondance pour le chinois simplifié (Chine)](images/language_matching_1.png)

1. Correspondance exacte ; 2. & 3. Correspondance de la région ; 4. Correspondance parente ; 5,5. Correspondance sœur.

Quand une sous-balise de langue a une valeur Suppress-Script définie dans le registre de sous-balise BCP-47, la correspondance correspondante se produit, en tenant compte de la valeur du code de script supprimé. Par exemple, en-Latn-US correspond à en-US. Dans cet exemple suivant, la langue de l’utilisateur est « en-au » (anglais (Australie)).

![Correspondance pour l’anglais (Australie)](images/language_matching_2.png)

1. Correspondance exacte ; 2. Correspondance de la zone de macro ; 1,3. Correspondance neutre pour la région ; 4. Correspondance de l’affinité orthographique ; 5,5. Correspondance de la région préférée ; 6,3. Correspondance sœur.

## <a name="matching-a-language-to-a-language-list"></a>Correspondance d’une langue avec une liste de langues

Parfois, la correspondance a lieu dans le cadre d’un processus plus vaste de correspondance d’une seule langue avec une liste de langues. Par exemple, il peut y avoir une correspondance entre une ressource basée sur une langue et une liste de langues pour une application. Le score de la correspondance est pondéré par la position de la première langue correspondante dans la liste. Plus la langue est basse dans la liste, plus le score est bas.

Lorsque la liste de langues contient au moins deux variantes régionales ayant les mêmes balises de langue et de script, les comparaisons de la première balise de langue sont uniquement évaluées pour les correspondances exacte, variant et région. Les correspondances partielles de score sont reportées à la dernière variante régionale. Cela permet aux utilisateurs de contrôler précisément le comportement de correspondance pour leur liste de langues. Le comportement de correspondance peut inclure l’autorisation d’une correspondance exacte pour un élément secondaire dans la liste pour être privilégiée par rapport à une correspondance partielle pour le premier élément de la liste, s’il existe un troisième élément qui correspond à la langue et au script du premier. Voici un exemple.

- Liste de langues (dans l’ordre) : « PT-PT » (portugais (Portugal)), « en-US » (anglais (États-Unis)), « PT-BR » (portugais (Brésil)).
- Ressources : « en-US », « PT-BR ».
- Ressource avec le score supérieur : « en-US ».
- Description : la comparaison commence par « PT-PT », mais ne trouve pas de correspondance exacte. En raison de la présence de « PT-BR » dans la liste des langues de l’utilisateur, la correspondance partielle est reportée à la comparaison avec « PT-BR ». La comparaison de langage suivante est « en-US », qui a une correspondance exacte. La ressource gagnante est donc « en-US ».

OR

- Liste de langues (dans l’ordre) : « es-MX » (espagnol (Mexique)), « es-HO » (espagnol (Honduras)).
- Ressources : « en-ES », « es-HO ».
- Ressource avec le score supérieur : « es-HO ».

## <a name="undetermined-language-und"></a>Langue indéterminée (« und »)

La balise de langue « und » peut être utilisée pour spécifier une ressource qui correspond à n’importe quel langage en l’absence d’une meilleure correspondance. Il peut être considéré comme similaire à la plage de langue BCP-47 « *» ou «* - &lt; script &gt; ». Voici un exemple.

- Liste de langues : « en-US », « zh-Hans-CN ».
- Ressources : « zh-Hans-CN », « und ».
- Ressource avec le score supérieur : « und ».
- Description : la comparaison commence par « en-US », mais ne trouve pas de correspondance basée sur « fr » (partielle ou meilleure). Étant donné qu’une ressource est marquée avec « und », l’algorithme de correspondance l’utilise.

La balise « und » permet à plusieurs langages de partager une seule ressource et permet de traiter des langues individuelles comme des exceptions. Par exemple,

- Liste de langues : « zh-Hans-CN », « en-US ».
- Ressources : « zh-Hans-CN », « und ».
- Ressource avec le score supérieur : « zh-Hans-CN ».
- Description : la comparaison trouve une correspondance exacte pour le premier élément et ne vérifie donc pas la présence de la ressource intitulée « und ».

Vous pouvez utiliser « und » avec une balise de script pour filtrer les ressources par script. Par exemple,

- Liste de langues : « ru ».
- Ressources : « und-LATN », « und-Cyrl », « und-arabe ».
- Ressource avec le score supérieur : « und-Cyrl ».
- Description : la comparaison ne trouve pas de correspondance pour « ru » (partielle ou supérieure), et correspond donc à la balise de langue « und ». La valeur de suppression de script « Cyrl » associée à la balise de langue « ru » correspond à la ressource « und-Cyrl ».

## <a name="orthographic-regional-affinity"></a>Affinité régionale orthographique

Lorsque deux balises de langue avec des différences de sous-zone de région sont mises en correspondance, certaines paires de régions peuvent avoir une affinité plus élevée par rapport à d’autres. Les seuls groupes avec affinités pris en charge sont pour l’anglais (« en »). Les sous-étiquettes de région « PH » (Philippines) et « LR » (Liberia) ont une affinité orthographique avec la sous-balise de région « US ». Toutes les autres sous-balises Region sont regroupées avec la sous-balise Region « GB » (Royaume-Uni). Par conséquent, quand les ressources « en-US » et « en-GB » sont disponibles, une liste de langues « fr-HK » (anglais (Hong-Kong SAR)) obtiendra un score plus élevé avec les ressources « en-GB » plutôt qu’avec les ressources « en-US ».

## <a name="handling-languages-with-many-regional-variants"></a>Gestion de langues avec de nombreuses variantes régionales

Certains langages ont des communautés de haut-parleurs dans différentes régions qui utilisent différentes variétés de langues de langue, &mdash; telles que l’anglais, le français et l’espagnol, qui sont parmi celles les plus souvent prises en charge dans les applications multilingues. Les différences régionales peuvent inclure des différences dans orthography (par exemple, « couleur » par rapport à « couleur ») ou des différences de dialecte telles que le vocabulaire (par exemple, « camion » par rapport à « camion »).

Ces langages avec des variantes régionales significatives présentent certains défis lors de la création d’une application mondialisable : « combien de variantes régionales différentes doivent être prises en charge ? ». « Quels sont les éléments ? » « Quelle est la méthode la plus économique pour gérer ces ressources de variantes régionales pour mon application ? » Cela dépasse le cadre de cette rubrique pour répondre à toutes ces questions. Toutefois, les mécanismes de correspondance de langue dans Windows offrent des fonctionnalités qui peuvent vous aider à gérer les variantes régionales.

Les applications ne prennent souvent en charge qu’une seule variété de langue donnée. Supposons qu’une application possède des ressources pour une seule variété d’anglais, qui sont censées être utilisées par les intervenants anglais, quelle que soit la région à partir de laquelle ils appartiennent. Dans ce cas, la balise « en » sans sous-balise de région reflète cette attente. Toutefois, les applications peuvent avoir historiquement utilisé une balise telle que « en-US » qui comprend une sous-zone de région. Dans ce cas, cela fonctionne également : l’application n’utilise qu’une seule variété d’anglais, et Windows gère la correspondance d’une ressource marquée pour une variante régionale avec une préférence de langue utilisateur pour une variante régionale différente de manière appropriée.

Toutefois, si plusieurs variétés régionales vont être prises en charge, une différence telle que « en » et « en-US » peut avoir un impact significatif sur l’expérience utilisateur et il est important de prendre en compte les sous-balises de région à utiliser.

Supposons que vous souhaitiez fournir des localisations françaises distinctes pour le français, telles qu’elles sont utilisées dans le Canada et dans le français européen. Pour le français canadien, vous pouvez utiliser « fr-CA ». Pour les intervenants de l’Europe, la localisation utilise le français (France) et, par conséquent, « fr-FR » peut être utilisé pour cela. Mais que se passe-il si un utilisateur donné est de Belgique, avec la préférence de langue « fr-to »; qu’est-ce qu’il obtiendra ? La région « être » est différente de « FR » et « CA », ce qui suggère une correspondance « n’importe quelle région » pour les deux. Toutefois, la France devient la région préférée pour le français. par conséquent, la valeur « fr-FR » sera considérée comme la meilleure correspondance dans ce cas.

Supposons que vous avez d’abord localisé votre application pour une seule variété de français, à l’aide de chaînes françaises (France), mais que vous les ayez qualifiées comme « fr », puis que vous souhaitez ajouter la prise en charge du français canadien. Seules certaines ressources doivent être retraduites pour le français canadien. Vous pouvez continuer à utiliser toutes les ressources d’origine, en les conservant comme « fr », et simplement ajouter le petit ensemble de nouvelles ressources à l’aide de « fr-CA ». Si la langue de l’utilisateur est « fr-CA », la ressource « fr-CA » aura un score de correspondance supérieur à celui de la ressource « fr ». Toutefois, si la langue de l’utilisateur est pour toute autre variété de français, la ressource neutre pour la région « fr » est une meilleure correspondance que la ressource « fr-CA ».

Autre exemple : Supposons que vous souhaitiez fournir des localisations espagnoles distinctes pour les intervenants entre l’Espagne et les orateurs de Amérique latine. Supposons que les traductions pour Amérique latine ont été fournies par un fournisseur au Mexique. Devez-vous utiliser « es-ES » (Espagne) et « es-MX » (Mexique) pour deux ensembles de ressources ? Si vous l’avez fait, cela pourrait créer des problèmes pour les orateurs d’autres régions d’Amérique latine comme l’Argentine ou la Colombie, car ils obtenaient les ressources « es-ES ». Dans ce cas, il existe une meilleure alternative : vous pouvez utiliser une sous-balise de zone de macro, « es-419 » pour indiquer que vous souhaitez utiliser les ressources pour les orateurs de n’importe quelle partie de Amérique latine ou des Caraïbes.

Les balises de langue neutres pour les régions et les sous-balises de région de macro peuvent être très efficaces si vous souhaitez prendre en charge plusieurs variétés régionales. Pour réduire le nombre de ressources distinctes dont vous avez besoin, vous pouvez qualifier une ressource donnée d’une façon qui reflète la couverture la plus large pour laquelle elle s’applique. Complétez ensuite une ressource largement applicable avec une variante plus spécifique, si nécessaire. Un élément multimédia avec un qualificateur de langage indépendant de la région sera utilisé pour les utilisateurs de toute variété régionale, sauf s’il existe une autre ressource avec un qualificateur plus régional qui s’applique à cet utilisateur. Par exemple, une ressource « en » correspondra à un utilisateur en anglais australien, mais une ressource avec « en-053 » (anglais comme étant utilisé en Australie ou en Nouvelle-Zélande) sera une meilleure correspondance pour cet utilisateur, tandis qu’une ressource avec « en-au » sera la meilleure correspondance possible.

L’anglais nécessite une attention particulière. Si une application ajoute la localisation pour deux variétés anglaises, celles-ci seront probablement pour l’anglais des États-Unis et pour le Royaume-Uni, ou « international », anglais. Comme indiqué ci-dessus, certaines régions situées en dehors des États-Unis suivent États-Unis les conventions d’orthographe, et la correspondance de langue Windows prend cela en considération. Dans ce scénario, il n’est pas recommandé d’utiliser la balise indépendante de la région « en » pour l’une des variantes. Utilisez plutôt « FR-GB » et « en-US ». (Si une ressource donnée ne requiert pas de variantes distinctes, toutefois, « en » peut être utilisé.) Si « en-GB » ou « en-US » est remplacé par « en », cela interfère avec l’affinité régionale de l’orthographique fournie par Windows. Si une troisième localisation en anglais est ajoutée, utilisez une sous-balise spécifique ou de région de macro pour les variantes supplémentaires, si nécessaire (par exemple, « en-CA », « en-au » ou « en-053 »), mais continuez à utiliser « en-GB » et « en-US ».

## <a name="related-topics"></a>Rubriques connexes

* [Comment le système de gestion des ressources met en correspondance et sélectionne les ressources](how-rms-matches-and-chooses-resources.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](../design/globalizing/manage-language-and-region.md)
* [Composition de régions géographiques (continent), de sous-régions géographiques et de regroupements économiques et autres sélectionnés](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
