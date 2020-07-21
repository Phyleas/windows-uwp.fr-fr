---
Description: Cette rubrique définit les termes liste des langues du profil utilisateur, liste des langues du manifeste de l’application et liste des langues du runtime de l’application. Nous utiliserons ces termes dans cette rubrique et d’autres sujets dans ce domaine de fonctionnalités. il est donc important de savoir ce qu’ils signifient.
title: Comprendre les langues de profil utilisateur et les langues du manifeste de l’application
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation
ms.localizationpriority: medium
ms.openlocfilehash: 9998436b106acce6a9223140e66d2633c2210a54
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493354"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Comprendre les langues de profil utilisateur et les langues du manifeste de l’application
Un utilisateur Windows peut utiliser des **paramètres**  >  **&**  >  **région de langue & langue** pour configurer une liste triée de langues d’affichage préférées, ou simplement une langue d’affichage par défaut unique. Une langue peut avoir une variante régionale. Par exemple, vous pouvez sélectionner la langue espagnole telle qu’elle est parlée en Espagne, espagnole parlée au Mexique, espagnol comme parlé dans le États-Unis, entre autres.

En outre, dans **paramètres**de l'  >  **heure &** langue  >  **& langue**, mais distincte de la langue, l’utilisateur peut spécifier son emplacement (appelé région) dans le monde. Notez que le paramètre de la langue d’affichage (et de la variante régionale) n’est pas un déterminant du paramètre de la région, et vice versa. Par exemple, un utilisateur peut actuellement habiter en France, mais choisir une langue d’affichage de Español (México) par défaut.

Pour les applications Windows, un langage est représenté sous la forme d’une [balise de langue BCP-47](https://tools.ietf.org/html/bcp47). Par exemple, la balise de langue BCP-47 « en-US » correspond à l’anglais (États-Unis) dans **paramètres**. Les API Windows Runtime appropriées acceptent et retournent des représentations sous forme de chaîne de balises de langue BCP-47.

Consultez également le registre de la sous- [balise IANA Language](https://www.iana.org/assignments/language-subtag-registry).

Les trois sections suivantes définissent les termes « liste des langues du profil utilisateur », « liste des langues du manifeste de l’application » et « liste des langues du runtime de l’application ». Nous utiliserons ces termes dans cette rubrique et d’autres sujets dans ce domaine de fonctionnalités. il est donc important de savoir ce qu’ils signifient.

## <a name="user-profile-language-list"></a>Liste des langues du profil utilisateur
La liste langue du profil utilisateur est le nom de la liste configurée par l’utilisateur dans **paramètres**  >  **heure & langue**langue  >  **&**  >  **langues**. Dans le code, vous pouvez utiliser la propriété [**GlobalizationPreferences. Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) pour accéder à la liste des langues du profil utilisateur sous la forme d’une liste en lecture seule de chaînes, où chaque chaîne est une [balise de langue BCP-47](https://tools.ietf.org/html/bcp47) unique telle que « en-US » ou « ja-JP ».

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Liste des langues du manifeste de l’application
La liste de langages du manifeste de l’application est la liste des langues pour lesquelles votre application déclare (ou déclare) la prise en charge. Cette liste croît au fur et à mesure que vous progressez dans le cycle de vie du développement jusqu’à la localisation.

La liste est déterminée au moment de la compilation, mais vous disposez de deux options pour contrôler exactement comment cela se produit. L’une des options consiste à laisser Visual Studio déterminer la liste des fichiers de votre projet. Pour ce faire, définissez d’abord la **langue par défaut** de votre application sous l’onglet **application** dans le fichier source du manifeste de votre package d’application ( `Package.appxmanifest` ). Ensuite, confirmez que le même fichier contient cette configuration (c’est le cas par défaut).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Chaque fois que Visual Studio génère votre fichier de manifeste de package d’application généré ( `AppxManifest.xml` ), il développe cet `Resource` élément unique dans le fichier source en une Union de tous les qualificateurs de langage qu’il trouve dans votre projet (consultez [adapter vos ressources pour connaître la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)). Par exemple, si vous avez commencé à localiser et que vous avez des ressources de type chaîne, image et/ou fichier dont le nom de dossier ou de fichier est « en-US », « ja-JP » et « fr-FR », votre `AppxManifest.xml` fichier généré contient les éléments suivants (la première entrée de la liste est la langue par défaut que vous définissez).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

L’autre option consiste à remplacer cet élément « x-Generate » `<Resource>` dans le fichier source du manifeste du package d’application ( `Package.appxmanifest` ) par la liste développée des `<Resource>` éléments (en veillant à répertorier d’abord la langue par défaut). Cette option implique davantage de travail de maintenance, mais il peut s’agir d’une option appropriée si vous utilisez un système de génération personnalisé.

Pour commencer, votre liste de langages de manifeste de l’application ne contient qu’une seule langue. C’est peut-être en-US. Toutefois &mdash; , lorsque vous configurez manuellement votre manifeste, ou lorsque vous ajoutez des ressources traduites à votre projet, &mdash; cette liste va croître.

Lorsque votre application se trouve dans le Microsoft Store, les langues de la liste langue du manifeste de l’application sont celles qui sont affichées aux clients. Pour obtenir la liste des balises de langue BCP-47 spécifiquement prises en charge par le Microsoft Store, consultez [langues prises en charge](../../publish/supported-languages.md).

Dans le code, vous pouvez utiliser la propriété [**ApplicationLanguages. ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) pour accéder à la liste des langages du manifeste de l’application sous la forme d’une liste en lecture seule de chaînes, où chaque chaîne est une balise de langue BCP-47 unique.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Liste des langues du runtime de l’application
La troisième liste de langues d’intérêt est l’intersection entre les deux listes que nous venons de décrire. Lors de l’exécution, la liste des langues pour lesquelles votre application a déclaré la prise en charge (liste de langages du manifeste de l’application) est comparée à la liste des langues pour lesquelles l’utilisateur a déclaré une préférence (la liste de langues du profil utilisateur). La liste de langues du runtime de l’application est définie sur cette intersection (si l’intersection n’est pas vide) ou uniquement sur la langue par défaut de l’application (si l’intersection est vide).

Plus précisément, la liste des langues du runtime de l’application est constituée de ces éléments.

1.  **(Facultatif) remplacement de la langue principale**. Le [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) est un paramètre de remplacement simple pour les applications qui offrent à l’utilisateur leur propre choix de langage indépendant, ou les applications qui ont une bonne raison de remplacer les choix de langue par défaut. Pour en savoir plus, voir [Exemple de ressources d’application et de localisation](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208)).
2.  **Langues de l’utilisateur prises en charge par l’application**. Il s’agit de la liste des langues du profil utilisateur filtrée par la liste des langues du manifeste de l’application. Le filtrage des langues de l’utilisateur par celles prises en charge par l’application préserve la cohérence entre les Kits de développement logiciel (SDK), les bibliothèques de classes, les packages d’infrastructure dépendants et l’application.
3.  **Si 1 et 2 sont vides, il s’agit de la langue par défaut ou de la première langue prise en charge par l’application**. Si la liste de langues du profil utilisateur ne contient aucune langue prise en charge par l’application, la langue d’exécution de l’application est la première langue prise en charge par l’application.

Dans le code, vous pouvez utiliser la propriété [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) pour accéder à la liste des langues du runtime de l’application sous la forme d’une chaîne contenant une liste délimitée par des points-virgules de balises de langue BCP-47.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

Vous pouvez également y accéder en tant que liste en lecture seule de chaînes, chacune contenant une balise de langue BCP-47 unique. Pour ce faire, vous pouvez utiliser la propriété [**ResourceContext. Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) ou [**ApplicationLanguages. Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) .

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

La liste des langues du runtime d’application détermine les ressources que Windows charge pour votre application, ainsi que la ou les langues utilisées pour mettre en forme les dates, les heures, les nombres et d’autres composants. Consultez [globaliser vos formats date/heure/nombre](use-global-ready-formats.md).

**Remarque** Si la langue du profil utilisateur et le langage du manifeste de l’application sont des variantes régionales les unes des autres, la variante régionale de l’utilisateur est utilisée comme langue du runtime de l’application. Par exemple, si l’utilisateur préfère en-GB et que l’application prend en charge en-US, alors la langue du runtime de l’application est en-GB. Cela garantit que les dates, les heures et les nombres sont mis en forme plus étroitement aux attentes de l’utilisateur (en Go), mais que les ressources localisées sont toujours chargées (en raison de la mise en correspondance de la langue) dans la langue prise en charge par l’application (en-US).

## <a name="qualify-resource-files-with-their-language"></a>Qualifier des fichiers de ressources avec leur langue
Nommez vos fichiers de ressources, ou leurs dossiers, avec des qualificateurs de ressources de langue. Pour en savoir plus sur les qualificateurs de ressources, consultez [adapter vos ressources pour la langue, la mise à l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md). Un fichier de ressources peut être une image (ou une autre ressource), ou il peut s’agir d’un fichier de conteneur de ressources, tel qu’un fichier *. resw* qui contient des chaînes de texte.

**Remarque** Même les ressources dans la langue par défaut de votre application doivent spécifier le qualificateur de langue. Par exemple, si la langue par défaut de votre application est l’anglais (États-Unis), qualifiez vos ressources en tant que `\Assets\Images\en-US\logo.png` .

- Windows effectue une correspondance complexe, y compris des variantes régionales comme en-US et en-GB. Incluez la sous-balise Region, le cas échéant. Découvrez [Comment le système de gestion des ressources correspond aux balises de langue](../../app-resources/how-rms-matches-lang-tags.md).
- Spécifiez une sous-balise de script de langue dans le qualificateur lorsqu’il n’existe aucune valeur de suppression de script définie pour la langue. Par exemple, au lieu de zh-CN ou zh-TW, utilisez zh-Hant, zh-Hant-TW ou zh-Hans (pour plus de détails, consultez le registre de la sous- [balise de langue IANA](https://www.iana.org/assignments/language-subtag-registry)).
- Pour les langues qui ont un dialecte standard unique, il n’est pas nécessaire d’inclure le qualificateur de région. Par exemple, utilisez ja au lieu de ja-JP.
- Certains outils et d’autres composants, tels que les traducteurs d’ordinateurs, peuvent trouver des balises de langue spécifiques, telles que des informations de dialecte régionale, utiles pour comprendre les données.

### <a name="not-all-resources-need-to-be-localized"></a>Toutes les ressources n’ont pas besoin d’être localisées

La localisation n’est peut-être pas nécessaire pour toutes les ressources.

- Au minimum, assurez-vous que toutes les ressources existent dans la langue par défaut.
- Un sous-ensemble de certaines ressources peut suffire pour une langue étroitement liée (localisation partielle). Par exemple, vous pouvez ne pas localiser toute l’interface utilisateur de votre application dans le catalan si votre application possède un ensemble complet de ressources en espagnol. Pour les utilisateurs qui parlent catalan et espagnol, les ressources qui ne sont pas disponibles en catalan apparaissent en espagnol.
- Certaines ressources peuvent nécessiter des exceptions pour des langues spécifiques, tandis que la plupart des autres ressources mappent à une ressource commune. Dans ce cas, marquez la ressource destinée à être utilisée pour toutes les langues avec la balise de langue non déterminée « und ». Windows interprète la balise de langue « und » comme un caractère générique (semblable à « \* ») dans la mesure où elle correspond à la langue d’application supérieure après toute autre correspondance spécifique. Par exemple, si certaines ressources sont différentes pour le finnois, mais que le reste des ressources est le même pour toutes les langues, la ressource finlandaise doit être marquée avec la balise de langue finnoise, et le reste doit être marqué avec « und ».
- Pour les ressources basées sur un script de langue, telles qu’une police ou une hauteur de texte, utilisez la balise de langue indéterminée avec un script spécifié : 'und- &lt; script &gt; '. Par exemple, pour les polices latines, utilisez `und-Latn\\fonts.css` et pour les polices cyrilliques, utilisez `und-Cryl\\fonts.css` .

## <a name="set-the-http-accept-language-request-header"></a>Définir l’en-tête de demande HTTP Accept-Language
Déterminez si les services Web que vous appelez ont la même étendue de localisation que celle de votre application. Les requêtes HTTP effectuées à partir d’applications Windows dans des requêtes Web typiques, et XMLHttpRequest (XHR), utilisent l’en-tête de requête Accept-Language HTTP standard. Par défaut, l’en-tête HTTP est défini sur la liste langue du profil utilisateur. Chaque langue de la liste est développée pour inclure les éléments neutres de la langue et un poids (q). Par exemple, la liste de langue d’un utilisateur fr-FR et en-US produit un en-tête de demande HTTP Accept-Language de fr-FR, FR, en-US, fr ("fr-FR, fr ; q = 0,8, en-US ; q = 0.5, en ; q = 0,3"). Toutefois, si votre application météo (par exemple) affiche une interface utilisateur en français (France), mais que la langue supérieure de l’utilisateur dans sa liste de préférences est l’allemand, vous devez demander explicitement le français (France) au service afin de rester cohérent au sein de votre application.

## <a name="apis-in-the-windowsglobalization-namespace"></a>API dans l’espace de noms Windows. Globalization
En règle générale, les API de l’espace de noms [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) utilisent la liste des langues du runtime d’application pour déterminer la langue. Si aucune des langues n’a un format correspondant, les paramètres régionaux de l’utilisateur sont utilisés. Il s’agit des paramètres régionaux utilisés pour l’horloge système. Les paramètres régionaux utilisateur sont disponibles à partir de **paramètres**  >  **heure & langue**  >  **& langue**  >  **supplémentaire date, heure & région paramètres régionaux**  >  **: modifier les formats de date, d’heure ou de nombre**. Les API **Windows. Globalization** ont également des remplacements pour spécifier une liste de langues à utiliser, au lieu de la liste de langues du runtime de l’application.

À l’aide de la classe [**Language**](/uwp/api/windows.globalization.language?branch=live) , vous pouvez examiner les détails d’une langue particulière, tels que le script du langage, le nom complet et le nom natif.

## <a name="use-geographic-region-when-appropriate"></a>Utiliser la région géographique le cas échéant
Dans **paramètres**  >  **heure & région de langue**  >  **&**  >  **pays ou région**de langue, l’utilisateur peut spécifier son emplacement dans le monde. Vous pouvez utiliser ces paramètres à la place de la langue pour choisir le contenu à afficher à l’utilisateur. Par exemple, une application de News peut afficher par défaut le contenu de cette région.

Dans le code, vous pouvez accéder à ce paramètre à l’aide de la propriété [**GlobalizationPreferences. HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) .

À l’aide de la classe [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) , vous pouvez examiner les détails d’une région particulière, tels que son nom d’affichage, son nom natif et les devises en cours d’utilisation.

## <a name="examples"></a>Exemples
Le tableau suivant contient des exemples de ce que l’utilisateur peut voir dans l’interface utilisateur de votre application sous différents paramètres de langue et de région.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Liste des langues du manifeste de l’application</th>
<th align="left">Liste des langues du profil utilisateur</th>
<th align="left">Langue principale de substitution de l’application (en option)</th>
<th align="left">Liste des langues du runtime de l’application</th>
<th align="left">Ce que l’utilisateur voit dans l’application</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Anglais (GB) (par défaut) ; allemand (Allemagne)</td>
<td align="left">Anglais (GB)</td>
<td align="left">aucun</td>
<td align="left">Anglais (GB)</td>
<td align="left">Interface utilisateur : anglais (GB)<br>Dates/Heures/Nombres : anglais (GB)</td>
</tr>
<tr>
<td align="left">Allemand (Allemagne) (par défaut) ; français (France) ; italien (Italie)</td>
<td align="left">Français (Autriche)</td>
<td align="left">aucun</td>
<td align="left">Français (Autriche)</td>
<td align="left">Interface utilisateur : français (France) (langue de base pour français (Autriche))<br>Dates/Heures/Nombres : français (Autriche)</td>
</tr>
<tr>
<td align="left">Anglais (US) (par défaut) ; français (France) ; anglais (GB)</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">aucun</td>
<td align="left">Anglais (Canada) ; français (Canada)</td>
<td align="left">Interface utilisateur : Anglais (US) (langue de base pour anglais (Canada))<br>Dates/Heures/Nombres : anglais (Canada)</td>
</tr>
<tr>
<td align="left">Espagnol (Espagne) (par défaut) ; espagnol (Mexique) ; espagnol (Amérique latine) ; portugais (Brésil)</td>
<td align="left">Anglais (US)</td>
<td align="left">aucun</td>
<td align="left">Espagnol (Espagne)</td>
<td align="left">Interface utilisateur : espagnol (Espagne) (utilise le paramètre par défaut car aucune langue de base n’est disponible pour l’anglais)<br>Dates/Heures/Nombres : espagnol (Espagne)</td>
</tr>
<tr>
<td align="left">Catalan (par défaut) ; espagnol (Espagne) ; français (France)</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">aucun</td>
<td align="left">Catalan ; français (France)</td>
<td align="left">Interface utilisateur : principalement en catalan et un peu en français (France) car toutes les chaînes ne sont pas en catalan<br>Dates/Heures/Nombres : catalan</td>
</tr>
<tr>
<td align="left">Anglais (GB) (par défaut) ; français (France) ; allemand (Allemagne)</td>
<td align="left">Allemand (Allemagne) ; anglais (GB)</td>
<td align="left">Anglais (GB) (choisi par l’utilisateur dans l’interface utilisateur de l’application)</td>
<td align="left">Anglais (GB) ; allemand (Allemagne)</td>
<td align="left">Interface utilisateur : anglais (GB) (langue de substitution)<br>Dates/Heures/Nombres : anglais (GB)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> Pour obtenir la liste des codes pays/région standard utilisés par Microsoft, consultez la [liste pays/région officiels](/windows/uwp/publish/supported-languages).

## <a name="important-apis"></a>API importantes
* [GlobalizationPreferences. langues](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext. langues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages. langues](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Langue](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Balise de langue BCP-47](https://tools.ietf.org/html/bcp47)
* [Registre de sous-balise IANA Language](https://www.iana.org/assignments/language-subtag-registry)
* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Langues prises en charge](../../publish/supported-languages.md)
* [Globaliser vos formats de date/heure/chiffres](use-global-ready-formats.md)
* [Comment le système de gestion des ressources met en correspondance les balises de langue](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Exemples
* [Exemple de ressources d’application et de localisation](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
