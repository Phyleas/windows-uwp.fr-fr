---
title: Utilitaire PowerToys PowerRename pour Windows 10
description: Extension du shell Windows pour le changement de nom des fichiers en bloc
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 52c24b295e6d93a7a66fda25f89462ed0607bc19
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618558"
---
# <a name="powerrename-utility"></a>Utilitaire PowerRename

PowerRename est un outil de changement de nom en bloc qui vous permet d’effectuer les opérations suivantes :

- Modifiez les noms de fichiers d’un grand nombre de fichiers *(sans renommer tous les fichiers du même nom)*.
- Effectuer une recherche et un remplacement sur une section ciblée des noms de fichiers.
- Effectuez un changement de nom d’expression régulière sur plusieurs fichiers.
- Activez la case à cocher renommer les résultats attendus dans une fenêtre d’aperçu avant de finaliser un changement de nom en bloc.
- Annule une opération de changement de nom une fois qu’elle est terminée.

## <a name="demo"></a>Démonstration

Dans cette démonstration, toutes les instances du nom de fichier « Pampalona » sont remplacées par « Pamplona ». Étant donné que tous les fichiers sont nommés de manière unique, cela aurait pris beaucoup de temps pour s’exécuter manuellement un par un. PowerRename active un seul changement de nom en bloc. Notez que la commande « annuler le changement de nom » (Ctrl + Z) permet d’annuler la modification.

![Démonstration PowerRename](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>Menu PowerRename

Après avoir sélectionné des fichiers dans l’Explorateur de fichiers Windows, cliquez avec le bouton droit et sélectionnez *PowerRename* (qui s’affiche uniquement lorsqu’il est activé dans PowerToys), le menu PowerRename s’affiche. Le nombre d’éléments (fichiers) que vous avez sélectionnés s’affiche, ainsi que les valeurs de recherche et de remplacement, une liste d’options et une fenêtre d’aperçu affichant les résultats des valeurs de recherche et de remplacement que vous avez entrées.

![Capture d’écran du menu PowerRename](../images/powerrename-menu.png)

### <a name="search-for"></a>Rechercher

Entrez du texte ou une [expression régulière](https://wikipedia.org/wiki/Regular_expression) pour rechercher les fichiers de votre sélection qui contiennent les critères correspondant à votre entrée. Les éléments correspondants s’affichent dans la fenêtre d' *Aperçu* .

### <a name="replace-with"></a>Remplacer par

Entrez le texte pour remplacer la *recherche de* valeur entrée précédemment qui correspond à vos fichiers sélectionnés. Vous pouvez afficher le nom de fichier d’origine et le fichier renommé dans la fenêtre d' *Aperçu* .

### <a name="options---use-regular-expressions"></a>Options-utiliser des expressions régulières

Si elle est activée, la valeur de recherche est interprétée comme une [expression régulière](https://wikipedia.org/wiki/Regular_expression) (Regex). La valeur de remplacement peut également contenir des variables Regex (voir les exemples ci-dessous).  Si cette option n’est pas activée, la valeur de recherche est interprétée comme du texte brut à remplacer par le texte figurant dans le champ remplacer.

### <a name="options---case-sensitive"></a>Options-respect de la casse

Si cette option est activée, le texte spécifié dans le champ de recherche correspondra uniquement au texte des éléments si le texte est la même casse. La correspondance de casse est INSENSITIVE (ne reconnaissant pas de différence entre les lettres majuscules et minuscules) par défaut.

### <a name="options---match-all-occurrences"></a>Options-correspond à toutes les occurrences

Si cette option est activée, toutes les correspondances de texte dans le champ de recherche seront remplacées par le texte de remplacement.  Dans le cas contraire, seule la première instance de la recherche de texte dans le nom de fichier est remplacée (de gauche à droite).

Par exemple, étant donné le nom de `powertoys-powerrename.txt` fichier :

- Rechercher : `power`
- Renommer avec : `super`

La valeur du fichier renommé se traduirait par :

- Correspondre à toutes les occurrences (désactivées) : `supertoys-powerrename.txt`
- Correspondre à toutes les occurrences (activées) : `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>Options-exclure des fichiers

Les fichiers ne seront pas inclus dans l’opération. Seuls les dossiers seront inclus.

### <a name="options---exclude-folders"></a>Options-exclure des dossiers

Les dossiers ne seront pas inclus dans l’opération. Seuls les fichiers seront inclus.

### <a name="options---exclude-subfolder-items"></a>Options-exclure les éléments de sous-dossier

Les éléments dans les dossiers ne seront pas inclus dans l’opération. Par défaut, tous les sous-dossiers sont inclus.

### <a name="options---enumerate-items"></a>Options-énumérer les éléments

Ajoute un suffixe numérique aux noms de fichiers qui ont été modifiés dans l’opération. Par exemple : `foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>Options-nom de l’élément uniquement

Seule la partie nom de fichier (pas l’extension de fichier) est modifiée par l’opération. Par exemple : `txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>Options-extension d’élément uniquement

Seule la partie de l’extension de fichier (pas le nom de fichier) est modifiée par l’opération. Par exemple : `txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>Remplacer à l’aide de la date et de l’heure de création du fichier

Les attributs de date et d’heure de création d’un fichier peuvent être utilisés dans le texte *remplacer* par, en entrant un modèle de variable en fonction du tableau ci-dessous.

Modèle de variable |Explication
|:---|:---|
|$YYYY|Année représentée par un entier à quatre ou cinq chiffres, selon le calendrier utilisé.
|$YY|Année représentée uniquement par les deux derniers chiffres. Un zéro non significatif est ajouté pour les années à un seul chiffre.
|$Y|Année représentée uniquement par le dernier chiffre.
|$MMMM|Nom du mois
|$MMM|Nom abrégé du mois
|$MM|Month en chiffres avec des zéros non significatifs pour les mois à un seul chiffre.
|$M|Month en chiffres sans zéros non significatifs pour les mois à un seul chiffre.
|$DDDD|Nom du jour de la semaine
|$DDD|Nom abrégé du jour de la semaine
|$DD|Jour du mois en chiffres avec des zéros non significatifs pour les jours à un chiffre.
|$D|Jour du mois en chiffres sans zéros non significatifs pour les jours à un chiffre.
|$hh|Heures avec des zéros non significatifs pour les heures à un chiffre
|$h|Heures sans zéros non significatifs pour les heures à un chiffre
|$mm|Minutes avec des zéros non significatifs pour les minutes à un chiffre.
|$m|Minutes sans zéros non significatifs pour les minutes à un chiffre.
|$ss|Secondes avec des zéros non significatifs pour les secondes à un chiffre.
|$s|Secondes sans zéros non significatifs pour les secondes à un chiffre.
|$fff|Millisecondes représentées par trois chiffres complets.
|$ff|Les millisecondes sont représentées uniquement par les deux premiers chiffres.
|$f|Millisecondes représentées uniquement par le premier chiffre.

Par exemple, en fonction des noms de fichiers :

- `powertoys.png`, créé le 11/02/2020
- `powertoys-menu.png`, créé le 11/03/2020

Entrez les critères pour renommer les éléments :

- Rechercher : `powertoys`
- Renommer avec : `$MMM-$DD-$YY-powertoys`

La valeur du fichier renommé se traduirait par :

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>Expressions régulières

Pour la plupart des cas d’utilisation, une recherche et un remplacement simples suffisent. Dans certains cas, il peut arriver que des tâches de changement de nom complexes se présentent et nécessitent davantage de contrôle. Les [expressions régulières](https://wikipedia.org/wiki/Regular_expression) peuvent vous aider.

Les expressions régulières définissent un modèle de recherche pour le texte. Elles peuvent être utilisées pour rechercher, modifier et manipuler du texte. Le modèle défini par l’expression régulière peut correspondre une seule fois, plusieurs fois, ou pas du tout pour une chaîne donnée. PowerRename utilise la syntaxe [ECMAScript](https://wikipedia.org/wiki/ECMAScript) , qui est commune aux langages de programmation modernes.

Pour activer les expressions régulières, activez la case à cocher « utiliser des expressions régulières ».

**Remarque :** Vous souhaiterez probablement vérifier « Rechercher toutes les occurrences » lors de l’utilisation d’expressions régulières.

### <a name="examples-of-regular-expressions"></a>Exemples d’expressions régulières

#### <a name="simple-matching-examples"></a>Exemples de correspondances simples

| Rechercher     | Description                                           |
| -------------- | ------------- |
| ^              | Correspond au début du nom de fichier                   |
| $              | Mettre en correspondance la fin du nom de fichier                         |
| .*             | Correspond à tout le texte du nom                        |
| ^ foo           | Faire correspondre le texte commençant par « foo »                     |
| barre $           | Correspond au texte qui se termine par « bar »                       |
| ^ foo. \* barre $    | Faire correspondre le texte commençant par « foo » et se terminant par « bar » |
| .+? ( ? = bar)     | Faire correspondre tout jusqu’à « bar »                          |
| barre foo [\s\S] \* | Correspond à tout ce qui se trouve entre « foo » et « bar »              |

#### <a name="matching-and-variable-examples"></a>Exemples de correspondances et de variables

*Lorsque vous utilisez les variables, l’option « mettre en correspondance toutes les occurrences » doit être activée.*

| Rechercher | Remplacer par  | Description                                |
| ---------- | ------------- |--------------------------------------------|
| (.\*). format  | foo \_ $1.png   | Ajoute « foo \_ » au nom de fichier existant |
| (.\*). format  | $1 \_foo.png   | Ajoute « \_ foo » au nom de fichier existant  |
| (.\*)      | $1.txt        | Ajoute l’extension « . txt » au nom de fichier existant |
| (^ \w + \. $) \| (^ \w + $) | $2.txt | Ajoute l’extension « . txt » au nom de fichier existant uniquement si elle n’a pas d’extension |

### <a name="additional-resources-for-learning-regular-expressions"></a>Ressources supplémentaires pour l’apprentissage d’expressions régulières

Des exemples ou des tableaux de triche sont disponibles en ligne pour vous aider

[Didacticiel Regex : un aide-mémoire rapide par des exemples](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[Didacticiel sur les expressions régulières ECMAScript](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>Filtres de liste de fichiers

Les filtres peuvent être utilisés dans PowerRename pour limiter les résultats du changement de nom. Utilisez la fenêtre d' *Aperçu* pour vérifier les résultats attendus. Sélectionnez les en-têtes de colonne pour basculer entre les filtres.

- **Original**, la première colonne dans la fenêtre d' *Aperçu* est en cours de cycles entre :
  - Activé : le fichier sélectionné est renommé.
  - Désactivé : le fichier n’est pas sélectionné pour être renommé (même s’il correspond à la valeur entrée dans les critères de recherche).

- **Renommée**, la deuxième colonne dans les fenêtres d' *Aperçu* peut être activée/désactivée.
  - L’aperçu par défaut affiche tous les fichiers sélectionnés, avec uniquement les fichiers correspondant à la *recherche de* critères affichant la valeur renommer mise à jour.
  - La sélection de l’en-tête *renommé* activera l’Aperçu pour afficher uniquement les fichiers qui seront renommés. Les autres fichiers sélectionnés de votre sélection d’origine ne seront pas visibles.

![Démonstration du filtre PowerRename PowerToys](../images/powerrename-demo2.gif)
