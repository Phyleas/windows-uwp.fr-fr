---
description: Étendez les fonctionnalités de base de **Cortana** avec les commandes vocales qui lancent et exécutent une seule action dans une application Windows.
title: Interactions Cortana dans les applications Windows
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, canevas Cortana, conception Cortana, interface utilisateur, commandes vocales, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ca9f77d10f6e22d4e244b102cb8b85e1f75113fc
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057560"
---
# <a name="cortana-interactions-in-windows-apps"></a>Interactions Cortana dans les applications Windows

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).

Étendez les fonctionnalités de base de **Cortana** avec les commandes vocales qui lancent et exécutent une seule action dans une application Windows.

L’application cible peut être lancée au premier plan (l’application prend le focus et **Cortana** est fermée) ou activée en arrière-plan (**Cortana** conserve le focus, mais fournit les résultats de l’application), en fonction de la complexité de l’interaction. En règle générale, les commandes vocales qui requièrent un contexte ou une entrée utilisateur supplémentaires sont mieux gérées dans une application de premier plan, tandis que les commandes de base peuvent être gérées dans **Cortana** via une application en arrière-plan.

En intégrant les fonctionnalités de base de votre application et en fournissant un point d’entrée central permettant à l’utilisateur d’accomplir la plupart des tâches sans ouvrir votre application directement, **Cortana** devient une liaison entre votre application et l’utilisateur. Le fait de fournir ce raccourci vers les fonctionnalités d’application et de réduire le besoin de basculer entre les applications peut faire gagner beaucoup de temps et d’efforts à l’utilisateur.

> [!NOTE]
> Une commande vocale est un énoncé unique avec un but spécifique, défini dans un fichier de définition de commande vocale (VCD), dirigé vers une application installée via **Cortana**.
>
> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec un objectif unique.
>
> Les définitions de commandes vocales peuvent varier en complexité. Ils peuvent prendre en charge n’importe quel caractère à partir d’une seule et même énoncée à une collection d’énoncés de langage naturel plus flexibles, qui dénotent le même objectif.

## <a name="other-speech-and-conversation-components"></a>Autres composants de reconnaissance vocale et de conversation

### <a name="speech-voice-and-conversation-in-windows-10"></a>Discours, voix et conversation dans Windows 10

Pour plus d’informations sur la façon dont les différentes infrastructures de développement Windows assurent la reconnaissance vocale, la synthèse vocale et la prise en charge des conversations pour les développeurs qui créent des applications Windows, consultez [parole, voix et conversation dans Windows 10](/windows/apps/speech) .

### <a name="cortana-skills-kit"></a>Cortana Skills Kit (Kit de compétences Cortana)

Consultez le [Kit de compétences Cortana](/cortana/skills/) si vous souhaitez étendre Cortana en ajoutant vos propres compétences permettant aux utilisateurs d’interagir avec votre **service** via Cortana. [**Avis de désapprobation :** dans le cadre de notre objectif de transformer les expériences de productivité modernes en incorporant Cortana profondément dans Microsoft 365, nous mettons à niveau le kit de compétences Cortana pour les consommateurs (plateforme de développement) et toutes les compétences créées sur cette plateforme.]

## <a name="related-articles"></a>Articles connexes

- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
