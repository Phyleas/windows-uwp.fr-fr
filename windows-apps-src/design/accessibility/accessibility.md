---
Description: Présente les concepts d’accessibilité liés aux applications Windows.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: Accessibilité
label: Accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e410094f738860e71dadb960fccbdbc59306050
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234139"
---
# <a name="accessibility"></a>Accessibilité  

L’accessibilité concerne la création d’expériences qui rendent votre application Windows utilisable par des personnes qui utilisent la technologie dans un large éventail d’environnements et qui approchent votre interface utilisateur avec un éventail de besoins et d’expériences. Pour certaines situations, les exigences en matière d’accessibilité sont imposées par la loi. Il est toutefois préférable de gérer les aspects liés à l’accessibilité quelles que soient les exigences juridiques, afin que votre application ait l’audience la plus étendue possible.

> Il y a également une déclaration de Microsoft Store concernant l’accessibilité de votre application !

| Article | Description |
|---------|-------------|
| [Vue d’ensemble de l’accessibilité](accessibility-overview.md) | Cet article est une vue d’ensemble des concepts et technologies liés aux scénarios d’accessibilité des applications Windows. |
| [Conception de logiciels inclusifs](designing-inclusive-software.md) | En savoir plus sur l’évolution de la conception inclusive avec les applications Windows pour Windows 10.  Concevez et développez un logiciel inclusif en tenant compte de l’accessibilité. |
| [Développement d’applications Windows inclusives](developing-inclusive-windows-apps.md) | Cet article est une feuille de route pour le développement d’applications Windows accessibles. |
| [Test de l’accessibilité](accessibility-testing.md) | Procédures de test à suivre pour vous assurer que votre application Windows est accessible. |
| [Accessibilité dans le Windows Store](accessibility-in-the-store.md) | Décrit la configuration requise pour déclarer votre application Windows comme accessible dans le Microsoft Store. |
| [Liste de vérification de l’accessibilité](accessibility-checklist.md) | Fournit une liste de vérification pour vous aider à vous assurer que votre application Windows est accessible. |
| [Présenter des informations d’accessibilité élémentaires](basic-accessibility-information.md) | Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations de base nécessaires aux technologies d’assistance. |
| [Accessibilité du clavier](keyboard-accessibility.md) | Si votre application ne fournit pas un bon accès par le clavier, les non-voyants ou les utilisateurs ayant des problèmes de mobilité peuvent rencontrer des difficultés à utiliser votre application ou risquent de ne pas pouvoir l’utiliser du tout. |
| [Lecteurs d’écran et boutons système matériel](system-button-narration.md) | Les lecteurs d’écran, tels que le [narrateur](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator), doivent être en mesure de reconnaître et de gérer les événements de bouton du système matériel et de communiquer leur État aux utilisateurs. Dans certains cas, le lecteur d’écran peut avoir besoin de gérer des événements de bouton en mode exclusif et de ne pas les laisser se propager à d’autres gestionnaires. |
| [Repères et en-têtes](landmarks-and-headings.md) | Les repères et les en-têtes définissent les sections d’une interface utilisateur qui optimisent la navigation pour les utilisateurs de technologies d’assistance telles que les lecteurs d’écran. |
| [Thèmes à contraste élevé](high-contrast-themes.md) | Décrit les étapes nécessaires pour garantir que votre application Windows est utilisable quand un thème à contraste élevé est actif. |
| [Exigences de texte accessible](accessible-text-requirements.md) | Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire. Cette rubrique présente également les rôles d’automatisation d’interface utilisateur de Microsoft que les éléments de texte d’une application Windows peuvent avoir, et les meilleures pratiques pour le texte dans les graphiques. |
| [Pratiques d’accessibilité à éviter](practices-to-avoid.md) | Répertorie les pratiques à éviter si vous souhaitez créer une application Windows accessible. |
| [Homologues d’automation personnalisés](custom-automation-peers.md) | Décrit le concept des homologues d’automatisation pour UI Automation, et la manière dont vous pouvez fournir une prise en charge de l’automatisation pour votre propre classe d’interface utilisateur personnalisée. |
| [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md) | Répertorie les modèles de contrôle Microsoft UI Automation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter. |

## <a name="related-topics"></a>Rubriques connexes  
* [**Windows. UI. Xaml. Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) 
* [Prise en main du narrateur](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
