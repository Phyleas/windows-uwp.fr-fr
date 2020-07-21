---
Description: Concevez et développez votre application de façon à ce qu’elle fonctionne correctement sur les systèmes avec des configurations de langue et de culture différentes.
Search.Refinement.TopicID: 180
title: Directives relatives à la globalisation
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: d71cf2289654860b47aef18c117ac9d6d36fab0a
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493244"
---
# <a name="guidelines-for-globalization"></a>Directives relatives à la globalisation

Concevez et développez votre application de façon à ce qu’elle fonctionne correctement sur les systèmes avec des configurations de langue et de culture différentes. Utiliser des API de [**globalisation**](/uwp/api/Windows.Globalization?branch=live) pour mettre en forme les données ; et évitez les hypothèses dans votre code sur la langue, la région, la classification des caractères, le système d’écriture, le format de date/heure, les nombres, les devises, les pondérations et les règles de tri.

| Recommandation | Description |
| ------------- | ----------- |
| Prenez la culture en compte lors de la manipulation et de la comparaison de chaînes. | Par exemple, ne modifiez pas la casse des chaînes avant de les comparer. Consultez [recommandations pour l’utilisation de chaînes](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Lors du classement (tri) des chaînes et d’autres données, ne partez pas du principe qu’elle est toujours effectuée par ordre alphabétique. | Pour les langues qui n’utilisent pas de script latin, le classement est basé sur des facteurs tels que la prononciation ou le nombre de traits de stylet. Même les langues qui utilisent le script latin n’utilisent pas toujours le tri alphabétique. Par exemple, dans certaines cultures, un annuaire n’est pas trié par ordre alphabétique. Windows peut gérer le tri pour vous, mais si vous créez votre propre algorithme de tri, veillez à prendre en compte les méthodes de tri utilisées dans vos marchés cibles. |
| Mettre en forme les nombres, les dates, les heures, les adresses et les numéros de téléphone de manière appropriée. | Ces formats varient selon les cultures, les régions, les langues et les marchés. Si vous affichez ces données, utilisez les API de [**globalisation**](/uwp/api/Windows.Globalization?branch=live) pour obtenir le format approprié pour un public particulier. Consultez [globaliser vos formats date/heure/nombre](use-global-ready-formats.md). L’ordre d’affichage de la famille et des noms donnés, ainsi que le format des adresses, peuvent également différer. Utilisez les affichages de date, d’heure et de nombre standard. Utilisez les contrôles de sélecteur de date et d’heure standard. Utilisez les informations d’adresse standard. |
| Prend en charge les unités de mesure et de devise internationales. | Des unités et des échelles différentes sont utilisées dans différents pays, bien que les systèmes métrique et impérial soient les plus populaires. Veillez à prendre en charge la mesure système appropriée si vous traitez des mesures telles que la longueur, la température et la zone. Utilisez la propriété [**GeographicRegion. CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) pour obtenir le jeu de devises en cours d’utilisation dans une région. |
| Utilisez Unicode pour le codage des caractères. | Par défaut, Microsoft Visual Studio utilise l’encodage de caractères Unicode pour tous les documents. Si vous utilisez un autre éditeur, veillez à enregistrer les fichiers sources dans les codages de caractères Unicode appropriés. Toutes les API Windows Runtime retournent des chaînes codées selon le codage de caractères UTF-16. |
| Prenez en charge les formats de papier internationaux. | Les formats de papier les plus courants diffèrent d’un pays à l’autre. par conséquent, si vous incluez des fonctionnalités qui dépendent du format du papier, telles que l’impression, assurez-vous de prendre en charge et de tester les tailles internationales communes. |
| Enregistrez la langue du clavier ou de l’IME. | Lorsque votre application demande à l’utilisateur d’entrer du texte, enregistrez la balise de langue pour la disposition de clavier ou l’éditeur de méthode d’entrée (IME) actuellement activé. Cela garantit que, lorsque l’entrée est affichée ultérieurement, elle sera présentée à l’utilisateur dans le format approprié. Utilisez la propriété [**Language. CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) pour récupérer la langue d’entrée actuelle. |
| N’utilisez pas de langue pour supposer la région d’un utilisateur. et n’utilisez pas de région pour supposer la langue d’un utilisateur. | La langue et la région sont des concepts distincts. Un utilisateur peut parler une variante régionale particulière d’une langue, comme en-GB pour l’anglais comme parlé au Royaume-Uni, mais l’utilisateur peut être dans un pays ou une région totalement différent. Déterminez si votre application nécessite des connaissances sur la langue de l’utilisateur (pour le texte de l’interface utilisateur, par exemple) ou sur la région (pour la gestion des licences, par exemple). Pour plus d’informations, consultez [comprendre les langages de profil utilisateur et les langages du manifeste d’application](manage-language-and-region.md). |
| Les règles de comparaison des balises de langue ne sont pas triviales. | Les [balises de langage BCP-47](https://tools.ietf.org/html/bcp47) sont complexes. Il existe un certain nombre de problèmes lors de la comparaison de balises de langue, dont notamment des problèmes de correspondance entre les informations de script, les balises héritées et les différentes variantes régionales. Le système de gestion des ressources de Windows prend en charge la correspondance pour vous. Vous pouvez spécifier un ensemble de ressources dans des langues quelconques et le système choisit celle qui est appropriée pour l’utilisateur et l’application. Consultez [les ressources d’application et le système de gestion des ressources](../../app-resources/index.md) , ainsi que la [façon dont le système de gestion des ressources correspond aux balises de langue](../../app-resources/how-rms-matches-lang-tags.md). |
| Concevez votre interface utilisateur pour prendre en charge différentes longueurs de texte et tailles de police pour les étiquettes et les contrôles de saisie de texte. | Les chaînes traduites dans des langages différents peuvent varier considérablement en longueur. vous aurez donc besoin de vos contrôles d’interface utilisateur pour les dimensionner de façon dynamique sur leur contenu. Les caractères courants dans d’autres langages incluent des marques au-dessus ou en dessous de ce qui est généralement utilisé en anglais (comme Å ou Ņ). Utilisez les tailles de police standard et les hauteurs de ligne pour fournir un espace vertical adéquat. Sachez que les polices pour d’autres langues peuvent nécessiter des tailles de police minimales plus élevées pour rester lisibles. Consultez les classes de l’espace de noms [Windows. Globalization. fonts](/uwp/api/windows.globalization.fonts?branch=live) . |
| Prend en charge la mise en miroir de l’ordre de lecture. | L’alignement du texte et l’ordre de lecture peuvent être de gauche à droite (comme en anglais, par exemple) ou de droite à gauche (RTL) (comme en arabe ou en Hébreu, par exemple). Si vous localisez votre produit dans des langues qui utilisent un autre ordre de lecture que le vôtre, assurez-vous que la disposition de vos éléments d’interface utilisateur prend en charge la mise en miroir. Même les éléments tels que les boutons précédent, les effets de transition de l’interface utilisateur et les images devront peut-être être mis en miroir. Pour plus d’informations, consultez [ajuster la disposition et les polices et prendre en charge RTL](adjust-layout-and-fonts--and-support-rtl.md). |
| Affichez correctement le texte et les polices. | Les paramètres optimaux de police, taille de police et direction du texte varient entre les différents marchés. Pour plus d’informations, consultez [**ajuster la disposition et les polices, et prendre en charge**](adjust-layout-and-fonts--and-support-rtl.md) les polices RTL et [international](loc-international-fonts.md). |

## <a name="important-apis"></a>API importantes
 
* [Globalisation](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language. CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations relatives à l'utilisation de chaînes](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md)
* [Comprendre les langues de profil utilisateur et les langues du manifeste de l’application](manage-language-and-region.md)
* [Balises de langue BCP-47](https://tools.ietf.org/html/bcp47)
* [Ressources d’application et système de gestion des ressources](../../app-resources/index.md)
* [Comment le système de gestion des ressources met en correspondance les balises de langue](../../app-resources/how-rms-matches-lang-tags.md)
* [Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](adjust-layout-and-fonts--and-support-rtl.md)
* [Polices internationales](loc-international-fonts.md)
* [Rendre votre application localisable](prepare-your-app-for-localization.md)

## <a name="samples"></a>Exemples

* [Exemple de préférences de globalisation](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
