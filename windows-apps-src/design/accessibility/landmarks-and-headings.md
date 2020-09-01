---
description: Décrit les fonctionnalités des points de repère et des titres de l’accessibilité.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Repères et en-têtes
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c05f39c0497a2e2ef369abd04ed437f8387e60f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173993"
---
# <a name="landmarks-and-headings"></a>Repères et en-têtes

Une interface utilisateur est généralement organisée de manière visuellement efficace, ce qui permet à un utilisateur de voir rapidement ce qui les intéresse sans avoir à ralentir pour lire *tout* le contenu. Un utilisateur de lecteur d’écran doit avoir ce même capacité d’évolution. Les éléments géographiques et les en-têtes définissent des sections d’une interface utilisateur qui facilitent la navigation pour les utilisateurs de la technologie d’assistance (au niveau de). Le fait de marquer le contenu dans des points de vue et des en-têtes offre à l’utilisateur d’un lecteur d’écran la possibilité de parcourir du contenu de la même façon qu’un utilisateur à l’écran.

Les concepts des points d’accès [Aria](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), des [en-têtes Aria](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html)et des [titres html](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) ont été utilisés dans le contenu Web depuis des années pour permettre une navigation plus rapide par les utilisateurs de lecteurs d’écran. Les pages Web utilisent des points de vue et des titres pour rendre leur contenu plus utilisable en permettant à l’utilisateur d’accéder rapidement au gros bloc (repère) et au plus petit bloc (en-tête). Plus précisément, les lecteurs d’écran ont des commandes qui permettent aux utilisateurs de passer d’un point de vue à un autre et d’accéder entre les en-têtes (niveau de titre suivant/précédent ou spécifique). Il est important de prendre en compte les points de repère et les en-têtes dans vos applications pour les mêmes raisons.

Les points de vue permettent de regrouper le contenu en différentes catégories, telles que la recherche, la navigation, le contenu principal, etc. Une fois regroupés, l’utilisateur peut naviguer rapidement entre les groupes. Cette navigation rapide permet à l’utilisateur d’ignorer des quantités substantielles de contenu qui, auparavant, auraient dû être parcourues par élément pour une expérience médiocre.

Par exemple, lorsque vous utilisez un panneau d’onglets, considérez-le comme un repère de navigation. Lorsque vous utilisez une zone d’édition de la recherche, considérez cet repère de recherche et envisagez de définir votre contenu principal en tant que repère de contenu principal. Que ce soit dans un repère ou même en dehors d’un repère, envisagez de définir des sous-éléments comme en-têtes avec des niveaux de titre logiques.

Examinez la page **facilité d’accès** de l’application Paramètres Windows.

![Page facilité d’accès de l’application Paramètres Windows](images/EaseOfAccessSettings.png)  

Une zone de modification de recherche est incluse dans un repère de recherche. Les éléments de navigation à gauche sont enveloppés dans un repère de navigation et le contenu principal à droite est encapsulé dans un repère de contenu principal. Pour aller plus loin, dans le volet de navigation, il y a un titre de groupe principal appelé **facilité d’accès** , qui est un titre de niveau 1. Sous, il s’agit des sous **-options application**, **audition**, etc. Ils ont le niveau de titre 2. Le fait de définir les en-têtes se poursuit dans le contenu principal, en définissant à nouveau l’objet principal, l' **affichage**, comme niveau de titre 1 et les sous-groupes, par exemple **faire tout** le plus grand sous forme de titre niveau 2.

L’application paramètres est accessible sans les points de repère et les en-têtes, mais elle est plus utilisable. Un utilisateur de lecteur d’écran peut rapidement et facilement accéder au groupe (repère) dont il A besoin, puis accéder rapidement au sous-groupe (en-tête).

Utilisez [AutomationProperties. LandmarkTypeProperty](/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) pour configurer l’élément d’interface utilisateur en tant que [type d’élément géographique de](/windows/desktop/WinAuto/landmark-type-identifiers) votre choix. Cet élément d’interface utilisateur repère encapsule tous les autres éléments d’interface utilisateur qui ont un sens pour ce repère.

Utilisez [AutomationProperties. LocalizedLandmarkTypeProperty](/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) pour nommer spécifiquement le repère. Si vous sélectionnez un type de repère prédéfini, tel que principal ou de navigation, ces noms sont utilisés pour le nom du repère. Toutefois, si vous définissez le type de repère sur personnalisé, vous devez spécifiquement nommer le repère par le biais de cette propriété. Vous pouvez également utiliser cette propriété pour remplacer les noms par défaut des types d’éléments de rapport non personnalisés.

Utilisez [AutomationProperties. HeadingLevel](/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) pour définir l’élément d’interface utilisateur en tant que titre d’un niveau spécifique, de *niveau1* à *Level9*.

## <a name="examples"></a>Exemples

Pour obtenir de nombreux exemples de code qui montrent comment résoudre de nombreux problèmes courants d’accessibilité par programme dans les applications de bureau Windows, consultez [exemples de code pour la résolution des problèmes courants d’accessibilité par programmation dans les applications de bureau Windows](/accessibility-tools-docs/).

Ces exemples de code sont directement référencés par[ Microsoft Accessibility Insights pour Windows](https://github.com/microsoft/accessibility-insights-windows), ce qui peut aider à améliorer la plupart des problèmes d’accessibilité dans l’interface utilisateur.
