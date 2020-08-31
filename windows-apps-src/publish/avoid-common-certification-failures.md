---
Description: Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application.
title: Éviter les échecs de certification courants
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07c814fc48e47b2bdc8980ac72732783d7ea9139
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158023"
---
# <a name="avoid-common-certification-failures"></a>Éviter les échecs de certification courants


Consultez cette liste pour éviter les problèmes qui empêchent souvent les applications d’être certifiées, ou qui peuvent être identifiés au cours d’une vérification ponctuelle après la publication de l’application.

> [!NOTE]
> Veillez à passer en revue les [stratégies de Microsoft Store](store-policies.md) pour vous assurer que votre application répond à toutes les conditions requises.

-   Ne soumettez votre application que lorsqu’elle est terminée. Nous vous invitons à utiliser la description de votre application pour signaler les fonctionnalités à venir. Toutefois, assurez-vous que votre application ne contient pas de sections incomplètes, de liens vers des pages web en construction, ni toute autre chose qui donnerait au client l'impression que votre application est incomplète.

-   [Procédez au test de votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre.

-   Testez votre application dans différentes configurations pour vérifier qu'elle est aussi stable que possible.

-   Assurez-vous que votre application ne se bloque pas sans connectivité réseau. Même si une connexion est requise pour utiliser réellement votre application, cette dernière doit fonctionner correctement en l’absence de connexion.

-   [Fournissez toutes les informations nécessaires](notes-for-certification.md) à l’utilisation de votre application, telles que le nom d’utilisateur et le mot de passe d’un compte de test, si votre application exige que les utilisateurs se connectent à un service ou les étapes requises pour accéder aux fonctionnalités masquées ou verrouillées.

-   Inclure une [URL de politique de confidentialité](enter-app-properties.md#privacy-policy-url) si votre application en requiert une ; par exemple, si votre application accède à n’importe quel type d’informations personnelles ou si elle est requise par la Loi. Pour déterminer si votre application requiert une stratégie de confidentialité, passez en revue le [contrat de développeur d’applications](/legal/windows/agreements/app-developer-agreement) et les stratégies de [Microsoft Store](store-policies.md).

-   Vérifiez que la description de votre application indique clairement ce que fait votre application. Si vous avez besoin d'aide, consultez nos recommandations en matière de [rédaction d'une description d'application attractive](write-a-great-app-description.md).

-   Fournissez des réponses complètes et précises à toutes les questions de la section évaluations de l' [âge](age-ratings.md) .

-   Ne [déclarez pas votre application comme accessible](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines) à moins de l’avoir spécifiquement conçue et testée pour des scénarios d’accessibilité.

-   Si votre application utilise les API de commerce à partir de l’espace de noms [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store), prenez soin de tester l’application et de vérifier qu’elle gère les exceptions par défaut. En outre, assurez-vous que votre application utilise la classe [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) et non la classe [**CurrentAppSimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) , qui est uniquement à des fins de test. (Notez que si votre application cible Windows 10, version 1607 ou ultérieure, nous vous recommandons d’utiliser les membres de l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) au lieu de l’espace de noms Windows. ApplicationModel. Store.)


 

 