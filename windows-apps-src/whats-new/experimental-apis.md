---
title: API expérimentales
description: Découvrez comment les API expérimentales sont fournies en externe à des fins d’évaluation dans le cadre des SDK Windows Insider, afin que les développeurs puissent les essayer.
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, expérimentales, api
ms.localizationpriority: medium
ms.openlocfilehash: 5a4813e7b4ae1e3dd16017066758aa8a35d0570a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170803"
---
# <a name="experimental-apis"></a>API expérimentales

Les API expérimentales en sont à un stade précoce de conception et sont susceptibles d’être modifiées, car les propriétaires intègrent des commentaires et ajoutent du support pour des scénarios supplémentaires.

Ces API sont évaluées en externe à l’aide de [Kits de développement logiciel (SDK) Windows Insider](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) afin que les développeurs puissent les essayer et fournir des commentaires avant de les intégrer à la plateforme officielle. Lors de leur évaluation, il n’existe aucune promesse de stabilité ou d’engagement.

## <a name="consuming-experimental-apis"></a>Utilisation d’API expérimentales
IntelliSense vous indique si une API est expérimentale. Lorsque vous utilisez une API expérimentale, vous recevez également un avertissement du compilateur tel que « ... sert uniquement à des fins d’évaluation et est susceptible d’être modifiée ou supprimée dans de futures mises à jour ».

Ces avertissements contribuent à vous protéger contre la création de dépendances sur des API expérimentales dans le cadre de projets de production. Pour les projets expérimentaux, vous pouvez ignorer ou désactiver ces avertissements.

Par défaut, ces API sont désactivées au moment de l’exécution et les appeler entraîne une exception d’exécution. Il s’agit d’une autre protection contribuant à empêcher les dépendances involontaires et une large distribution d’applications qui utilisent des API expérimentales.

Afin d’activer ces API pour expérimentation, utilisez le module de fonctionnalités du [Portail d’appareil Windows (WDP)](../debug-test-perf/device-portal.md) sur l’appareil cible pour activer la fonctionnalité correspondant à l’API que vous souhaitez appeler.

La documentation dédiée à une API expérimentale spécifique est à la discrétion de l’équipe qui en est propriétaire.

## <a name="providing-feedback"></a>Formulation de commentaires

Si vous avez essayé une API expérimentale et souhaitez formuler des commentaires, utilisez la catégorie **Plateforme des développeurs** dans le [Hub de commentaires Windows](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub).