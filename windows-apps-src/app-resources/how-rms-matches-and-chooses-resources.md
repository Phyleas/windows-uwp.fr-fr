---
description: Quand une ressource est requise, plusieurs candidats sont susceptibles de correspondre, avec plus ou moins d’exactitude, au contexte de ressource actuel. Le système de gestion des ressources analyse tous les candidats et identifie le meilleur à retourner. Cette rubrique décrit ce processus en détail et donne des exemples.
title: Comment le système de gestion des ressources met en correspondance et sélectionne les ressources
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: d430aae696b0f021e2412a73f137ea6db826937b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031852"
---
# <a name="how-the-resource-management-system-matches-and-chooses-resources"></a>Comment le système de gestion des ressources met en correspondance et sélectionne les ressources
Quand une ressource est requise, plusieurs candidats sont susceptibles de correspondre, avec plus ou moins d’exactitude, au contexte de ressource actuel. Le système de gestion des ressources analyse tous les candidats et identifie le meilleur à retourner. Pour ce faire, tous les qualificateurs doivent être pris en compte pour classer tous les candidats.

Dans ce processus de classement, les différents qualificateurs ont des priorités différentes : la langue a le plus grand impact sur le classement global, puis sur le contraste, puis sur la mise à l’échelle, etc. Pour chaque qualificateur, les qualificateurs candidats sont comparés à la valeur du qualificateur de contexte pour déterminer une qualité de correspondance. La façon dont la comparaison est effectuée dépend du qualificateur.

Pour plus d’informations sur la façon dont la correspondance des balises de langue est effectuée, consultez [Comment le système de gestion des ressources correspond aux balises de langue](how-rms-matches-lang-tags.md).

Pour certains qualificateurs, tels que la mise à l’échelle et le contraste, il y a toujours un degré de correspondance minimal. Par exemple, un candidat qualifié pour « Scale-100 » correspond à un contexte de « Scale-400 » à un certain degré, bien qu’il ne s’agit pas d’un candidat qualifié pour « Scale-200 » ou (pour une correspondance parfaite) « Scale-400 ».

Toutefois, pour d’autres qualificateurs, tels que la langue ou la région d’hébergement, il est possible d’avoir une comparaison sans correspondance (ainsi que des degrés de correspondance). Par exemple, un candidat qualifié pour la langue « en-US » est une correspondance partielle pour un contexte de « en-GB », mais un candidat qualifié comme « fr » n’est pas une correspondance. De même, un candidat qualifié pour la région d’hébergement « 155 » (Europe de l’Ouest) correspond à un contexte pour un utilisateur dont le paramètre de région d’hébergement est « FR », mais un candidat qualifié comme « US » ne correspond pas du tout.

Lorsqu’un candidat est évalué, s’il y a une comparaison sans correspondance pour un qualificateur, ce candidat obtient un classement global sans correspondance et n’est pas sélectionné. Ainsi, les qualificateurs de priorité supérieure peuvent avoir le poids le plus élevé dans la sélection de la meilleure correspondance, mais même un qualificateur de faible priorité peut éliminer un candidat en raison d’une non-correspondance.

Un candidat est neutre par rapport à un qualificateur s’il n’est pas marqué pour ce qualificateur. Pour tout qualificateur, un candidat neutre est toujours une correspondance pour la valeur du qualificateur de contexte, mais uniquement avec une qualité de correspondance inférieure à celle d’un candidat qui a été marqué pour ce qualificateur et qui a un degré de correspondance (exact ou partiel). Par exemple, si nous avons des candidats qualifiés pour « en-US », « en », « fr » et un candidat indépendant de la langue, alors pour un contexte avec une valeur de qualificateur de langue de « en-GB », les candidats seront classés dans l’ordre suivant : « en », « en-US », neutre et « fr ». Dans ce cas, « fr » ne correspond pas du tout, tandis que les autres candidats correspondent à un certain degré.

Le processus de classement global commence par l’évaluation des candidats par rapport au qualificateur de priorité la plus élevée, qui est la langue. Les non-correspondances sont éliminées. Les candidats restants sont classés par rapport à leur qualité de correspondance pour la langue. S’il existe des liens, le qualificateur suivant, le plus haut niveau de priorité, est pris en compte, en utilisant la qualité de correspondance pour le contraste pour différencier les candidats liés. Après le contraste, le qualificateur Scale est utilisé pour distinguer les liens restants, et ainsi de suite jusqu’à autant de qualificateurs que nécessaire pour parvenir à un classement bien ordonné.

Si tous les candidats sont supprimés en raison des qualificateurs qui ne correspondent pas au contexte, le chargeur de ressources passe par une deuxième passe en recherchant un candidat par défaut à afficher. Les candidats par défaut sont déterminés lors de la création du fichier PRI et sont nécessaires pour s’assurer qu’il y a toujours un candidat à sélectionner pour un contexte d’exécution. Si un candidat a des qualificateurs qui ne correspondent pas et ne sont pas une valeur par défaut, ce candidat de ressource est levé de façon permanente.

Pour tous les candidats aux ressources en cours d’examen, le chargeur de ressources examine la valeur du qualificateur de contexte de priorité la plus élevée et choisit celle qui a la meilleure correspondance ou le meilleur score par défaut. Toute correspondance réelle est considérée comme meilleure qu’un score par défaut.

En cas de lien, la valeur du qualificateur de contexte suivant la priorité la plus élevée est inspectée et le processus se poursuit, jusqu’à ce qu’une meilleure correspondance soit trouvée.

## <a name="example-of-choosing-a-resource-candidate"></a>Exemple de choix d’un candidat à la ressource
Examinez ces fichiers.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
fr/images/contrast-high/logo.scale-400.jpg
fr/images/contrast-high/logo.scale-100.jpg
de/images/logo.jpg
```

Et supposons qu’il s’agit des paramètres du contexte actuel.

```console
Application language: en-US; fr-FR;
Scale: 400
Contrast: Standard
```

Le système de gestion des ressources élimine trois des fichiers, car le contraste élevé et la langue allemande ne correspondent pas au contexte défini par les paramètres. Cela laisse ces candidats.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

Pour les candidats restants, le système de gestion des ressources utilise le qualificateur de contexte de priorité la plus élevée, qui est la langue. Les ressources en anglais sont plus proches que celles du français, car l’anglais est répertorié avant le français dans les paramètres.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
```

Ensuite, le système de gestion des ressources utilise le qualificateur de contexte de priorité la plus élevée, à l’échelle. Il s’agit donc de la ressource retournée.

```console
en/images/logo.scale-400.jpg
```

Vous pouvez utiliser la méthode avancée [**NamedResource. ResolveAll**](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live) pour récupérer tous les candidats dans l’ordre où ils correspondent aux paramètres de contexte. Pour l’exemple que nous venons d’examiner, **ResolveAll** retourne des candidats dans cet ordre.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/logo.scale-100.jpg
```

## <a name="example-of-producing-a-fallback-choice"></a>Exemple de production d’un choix de secours
Examinez ces fichiers.

```console
en/images/logo.scale-400.jpg
en/images/logo.scale-200.jpg
en/images/logo.scale-100.jpg  
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Et supposons qu’il s’agit des paramètres du contexte actuel.

```console
User language: de-DE;
Scale: 400
Contrast: High
```

Tous les fichiers sont éliminés, car ils ne correspondent pas au contexte. Nous devons donc entrer une passe par défaut, où la valeur par défaut (consultez [compiler manuellement les ressources avec MakePri.exe](compile-resources-manually-with-makepri.md)) pendant la création du fichier PRI était This.

```console
Language: fr-FR;
Scale: 400
Contrast: Standard
```

Cela laisse toutes les ressources qui correspondent à l’utilisateur actuel ou à la valeur par défaut.

```console
fr/images/contrast-standard/logo.scale-400.jpg
fr/images/contrast-standard/logo.scale-100.jpg
de/images/contrast-standard/logo.jpg
```

Le système de gestion des ressources utilise le qualificateur de contexte de priorité la plus élevée, langue, pour retourner la ressource nommée avec le score le plus élevé.

```console
de/images/contrast-standard/logo.jpg
```

## <a name="important-apis"></a>API importantes
* [NamedResource. ResolveAll](/uwp/api/windows.applicationmodel.resources.core.namedresource.resolveall?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Compiler des ressources manuellement avec MakePri.exe](compile-resources-manually-with-makepri.md)
