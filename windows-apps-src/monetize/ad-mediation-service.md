---
description: Le service de médiation publicitaire de Microsoft vous permet d'optimiser vos revenus publicitaires et vos capacités de promotion d'applications en affichant des publicités issues de plusieurs réseaux publicitaires.
title: Service de médiation publicitaire de Microsoft
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, pub, publicité, médiation publicitaire
ms.localizationpriority: medium
ms.openlocfilehash: 13cdba15e73028e0bf0560dc373e6f06bd0e86d5
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507193"
---
# <a name="microsoft-ad-mediation-service"></a>Service de médiation publicitaire de Microsoft

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Lorsque vous utilisez le [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) pour [afficher des publicités dans vos applications](display-ads-in-your-app.md), vous pouvez éventuellement utiliser le service de médiation publicitaire de Microsoft pour optimiser vos revenus publicitaires. Cet article présente une vue d'ensemble du service de médiation publicitaire et de ses objectifs.

Le service de médiation publicitaire fait partie de la [plateforme de monétisation des publicités de Microsoft](https://developer.microsoft.com/windows/ad-monetization-platform). Cette plateforme comprend les éléments suivants.

![addreferences](images/ad-mediation-service.png)

Le service de médiation publicitaire vise à optimiser les recettes publicitaires de vos applications en s’appuyant sur ces fonctionnalités.

## <a name="diversity-of-demand-by-market-and-format"></a>Diversité de la demande par marché et par format

Le service de médiation publicitaire intègre un grand nombre de réseaux publicitaires et les différents formats de publicité (bannière, spot sous forme de bannière, spot vidéo et natif). Le service de médiation publicitaire collabore avec chaque réseau publicitaire pour s'assurer qu’ils sont en mesure de diffuser les publicités en offrant le plus fort potentiel d'engagement des utilisateurs finaux. Nous continuerons à ajouter différents partenaires publicitaires pour développer la variété et la qualité de la demande.

## <a name="manage-complexity-of-ad-network-relationships"></a>Gérer la complexité des relations avec le réseau publicitaire  

Le service de médiation publicitaire intègre un grand nombre de réseaux publicitaires pour vous éviter ce travail. Une fois que vous avez utilisé le kit de développement logiciel (SDK) Microsoft Advertising pour afficher les publicités dans votre application, vous pouvez modifier vos paramètres ad Mediation [dans l’espace partenaires](../publish/in-app-ads.md#mediation-settings) pour afficher les publicités de plusieurs réseaux Active Directory. Vous pouvez ainsi recevoir des publicités de nouveaux réseaux publicitaires sans avoir besoin de modifier votre code.

Nous gérons la relation de bout en bout avec les réseaux publicitaires pour votre compte. Nous prenons en charge toutes les opérations, qu'il s'agisse de l’intégration de réseau publicitaire, de la diffusion des annonces, de la création de rapports ou des paiements, sans effort supplémentaire de votre part.

## <a name="machine-learning-based-yield-optimization-algorithms"></a>Algorithmes d’optimisation du rendement basés sur le Machine Learning

Le service de médiation publicitaire a pour mission de générer le plus fort rendement pour les développeurs. Pour cela, il utilise des algorithmes d’optimisation du rendement basés sur le Machine Learning. Pour chaque appel de publicité, les algorithmes déterminent le meilleur ordre dans lequel appeler les réseaux publicitaires *en cascade* afin d'optimiser vos revenus. Pour ce faire, vous devez tenir compte de nombreux facteurs, notamment les suivants :

* Utilisateur faisant la demande.
* Application concernée par la demande.
* Performances passées du réseau publicitaire.
* Format de publicité et marché d'où provient la demande.
* Heure de l'appel de publicité.
* Catégorie de contenu de l’application.
* Segment de l’utilisateur.
* Emplacement de l’utilisateur.

Les nouveaux réseaux publicitaires sont automatiquement inclus et leurs performances sont évaluées par le biais d’un budget d'apprentissage. Dans un bref délai, ils trouvent leur place dans la cascade. Cela rend les réseaux publicitaires plus compétitifs et permet au développeur de tirer le meilleur parti de la monétisation par le biais des applications.

Nous vous recommandons vivement d'utiliser nos [paramètres de médiation recommandés](../publish/in-app-ads.md#mediation-settings) pour optimiser les revenus issus des publicités dans vos applications. Cela permet à nos algorithmes d'obtenir le meilleur rendement pour votre application. Toutefois, vous avez également la possibilité de choisir vos propres paramètres de médiation dans l’espace partenaires pour avoir davantage de contrôle sur les réseaux Active Directory qui servent les publicités et l’ordre dans lequel elles sont utilisées.

## <a name="rich-data-and-signals"></a>Données et signaux enrichis

Le service de médiation publicitaire collabore avec différents réseaux publicitaires pour améliorer le ciblage des utilisateurs afin d'afficher des publicités plus pertinentes pour chaque utilisateur. Pour ce faire, nous envoyons des signaux plus riches concernant l’utilisateur et l’application au réseau publicitaire, tout en tenant compte des exigences de confidentialité.

## <a name="related-topics"></a>Rubriques connexes

* [SDK Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Paramètres de médiation](../publish/in-app-ads.md#mediation-settings)
* [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
