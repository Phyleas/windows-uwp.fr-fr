---
title: Développez des applications éducatives.
description: Cette section décrit les ressources des applications Windows universelles à votre disposition pour l’écriture d’applications éducatives pour la plateforme Windows 10.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, éducation
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: 9a32efc4be4d14bf703dfd597d7f4abd53cccd1d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161313"
---
# <a name="develop-universal-windows-apps-for-education"></a>Développement d’applications Universal Windows pour l’éducation
![capture d’écran d’application Examen](images/take-a-test-screen-small.png)

Les ressources suivantes vous aideront à écrire une application Windows universelle pour l’éducation.

### <a name="accessibility"></a>Accessibilité
Les applications éducatives doivent être accessibles. Pour plus d’informations, voir [Développement d’applications pour l’accessibilité](https://developer.microsoft.com/windows/accessible-apps).


### <a name="secure-assessments"></a>Évaluations sécurisées
Une évaluation ou un test d’application suppose généralement de créer un environnement *verrouillé* afin d’empêcher les étudiants d’utiliser d’autres ordinateurs ou ressources Internet au cours d’un test. Cette fonctionnalité est disponible via [l’API Examen](take-a-test-api.md). Pour obtenir un exemple d’environnement de test dans lequel l’accès en ligne est verrouillé pour les tests stratégiques, consultez l’application web [Examen](/education/windows/take-tests-in-windows-10) dans le Centre informatique de Windows.

### <a name="user-input"></a>Entrée utilisateur
Les entrées de l’utilisateur constituent une partie essentielle des applications éducatives ; les commandes de l’interface utilisateur doivent être réactives et intuitives afin de ne pas distraire les utilisateurs. Pour obtenir une vue d’ensemble des options d’entrée disponibles dans une application Windows universelle, consultez la page [Notions fondamentales sur les interactions](../design/input/input-primer.md) ainsi que les rubriques de la section Conception et interface utilisateur. Les exemples d’application suivants présentent également les méthodes de gestion de base de l’interface utilisateur dans la plateforme Windows universelle.
- [L’exemple d’entrée de base](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput) vous montre comment traiter les interactions dans les applications Windows universelles.
- [L’exemple du mode d’interaction des utilisateurs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode) vous montre comment détecter le mode d’interaction des utilisateurs, et comment y répondre.
- [L’exemple des visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) vous montre comment tirer parti des nouveaux visuels de focus dessinés du système, ou comment créer vos propres visuels de focus si les éléments dessinés du système ne satisfont pas vos besoins.

La plateforme Windows Ink permet d’optimiser les applications éducatives en les ajustant avec un mode d’entrée que les étudiants ont l’habitude d’utiliser. Consultez la page [Interactions avec le stylet et WindowsInk dans les applications UWP](../design/input/pen-and-stylus-interactions.md) et les rubriques qu’elle contient pour accéder à un guide complet qui vous permettra d’implémenter Windows Ink dans votre application. Les exemples d’application suivants montrent comment fonctionne cette API.
- [L’exemple d’entrée manuscrite](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink) démontre comment utiliser la fonctionnalité d’entrée manuscrite (comme la capture de données d’entrée manuscrite et l’interprétation des traits d’encre) dans les applications Windows universelles utilisant JavaScript.
- [L’exemple simple d’entrée manuscrite](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) démontre comment utiliser la fonctionnalité d’entrée manuscrite (comme la capture de données d’entrée manuscrite de l’utilisateur et l’exécution de la reconnaissance des entrées manuscrites sur les traits d’encre) dans les applications Windows universelles utilisant C#.
- [L’exemple d’entrée manuscrite complexe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) démontre comment utiliser la fonctionnalité avancée InkPresenter afin d’entrelacer l’entrée manuscrite avec d’autres objets, sélectionner les traits d’encre, copier/coller et traiter les événements. Le système repose sur la plateforme Windows universelle dans C++ et peut s’exécuter sur les SKU des appareils mobiles et de bureau Windows 10.


### <a name="microsoft-store"></a>Microsoft Store
En règle générale, les applications éducatives sont distribuées à une organisation spécifique dans certaines circonstances. Pour plus d’informations, consultez la page [Distribuer des applications métier aux entreprises](../publish/distribute-lob-apps-to-enterprises.md).

## <a name="related-topics"></a>Rubriques connexes
- [Windows 10 pour l’éducation](/education/windows/index) sur le Centre informatique de Windows