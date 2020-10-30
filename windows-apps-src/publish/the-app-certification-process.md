---
description: Lorsque vous avez terminé la création de la soumission de votre application et que vous cliquez sur envoyer au magasin, la soumission passe à l’étape de certification.
title: Processus de certification des applications
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, publication, prétraitement, certification, version, en attente, envoyer, publier, État, heure
ms.localizationpriority: medium
ms.openlocfilehash: 414a182074f03256c3c66492eab21e574e1a960c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034982"
---
# <a name="the-app-certification-process"></a>Processus de certification des applications

Lorsque vous avez terminé la création de la soumission de votre application et que vous cliquez sur **Envoyer au magasin** , la soumission passe à l’étape de certification. Ce processus s’effectue généralement en quelques heures, bien qu’il nécessite parfois trois journées ouvrables entières. Une fois que votre envoi est certifié, il peut s’écouler jusqu’à 24 heures avant que les clients puissent voir la liste de l’application pour une nouvelle soumission, ou pour une soumission mise à jour avec les modifications apportées aux packages. Si votre mise à jour ne modifie que la liste des détails, le processus de publication sera effectué en moins d’une heure.  Vous serez informé de la publication de votre envoi et de l’état de l’application dans le tableau de **bord.**

## <a name="preprocessing"></a>Prétraitement

Une fois que vous avez chargé avec succès les packages de l’application et soumis l’application pour certification, les packages sont mis en file d’attente à des fins de test. Un message s’affiche si des erreurs sont détectées pendant le pré-traitement. Pour plus d’informations sur les erreurs potentielles, voir [Résoudre les erreurs de soumission](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Plusieurs tests sont exécutés pendant cette phase :

-   **Tests de sécurité :** Ce premier test vérifie que les packages de votre application sont exempts de virus et de programmes malveillants. Si votre application échoue à ce test, vous devez vérifier votre système de développement en exécutant l’antivirus le plus récent, puis régénérer le package de votre application sur un système propre.
-   **Tests de conformité technique :** la conformité technique est testée à l’aide du Kit de certification des applications Windows. (Vous devez toujours prendre soin de [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre au Windows Store.)
-   **Conformité du contenu :** la durée de cette opération varie en fonction de la complexité de votre application, de la quantité de contenu visuel qu’elle comporte et du nombre d’applications que vous avez soumises récemment. Veillez à fournir toutes les informations que les testeurs doivent connaître dans la page [Remarques pour la certification](notes-for-certification.md).

Une fois le processus de certification terminé, vous recevez un rapport de certification indiquant si votre application a été ou non certifiée. Si votre application n’a pas été certifiée, le rapport indique le test auquel elle a échoué ou la [stratégie](store-policies.md) qui n’a pas été respectée. Après avoir corrigé le problème, vous pouvez créer une autre soumission pour votre application afin de recommencer le processus de certification.

## <a name="release"></a>Libérer

Lorsque votre application passe la certification, elle est prête à passer au processus de **publication** .

- Si vous avez indiqué que votre soumission doit être publiée dès que possible (option par défaut), le processus de publication commence immédiatement.
- S’il s’agit de la première fois que vous publiez l’application et que vous avez spécifié une **Date de publication** dans la section [planification](configure-precise-release-scheduling.md#release) , l’application devient disponible en fonction de vos sélections de **Date de publication** .
- Si vous avez utilisé les [options de conservation](manage-submission-options.md#publishing-hold-options) de la publication pour spécifier qu’elle ne doit pas être libérée avant une certaine date, nous attendons jusqu’à cette date pour commencer le processus de publication, sauf si vous sélectionnez Modifier la **Date** de publication.
- Si vous avez utilisé les [options de conservation](manage-submission-options.md#publishing-hold-options) de la publication pour spécifier que vous souhaitez publier l’envoi manuellement, nous ne démarrerons pas le processus de publication tant que vous n’avez pas sélectionné **publier maintenant** (ou sélectionné **modifier la date** de publication et choisi une date spécifique).


## <a name="publishing"></a>Publication

Les packages de votre application sont signés numériquement pour être protégés contre toute falsification après leur publication. Une fois que cette phase a commencé, vous ne pouvez plus annuler votre soumission ni en modifier la date de sortie.

Pour les nouvelles applications et les mises à jour qui incluent des modifications apportées aux packages de l’application, le processus de publication se termine dans les 24 heures. Pour les mises à jour qui modifient uniquement les options telles que les détails de la liste des magasins, mais ne modifient pas les packages de l’application, le processus de publication prendra moins d’une heure.

Pendant que votre application se trouve dans la phase de publication, le lien **afficher les détails** dans la colonne État de l’envoi de votre application vous permet de savoir quand vos nouveaux packages et les détails de la liste des magasins sont disponibles pour les clients sur chacune de vos versions de système d’exploitation prises en charge. Les étapes qui n’ont pas encore été effectuées s’affichent **en attente** . Votre application restera dans la phase de publication jusqu’à ce que le processus soit terminé, ce qui signifie que les nouveaux packages et/ou les détails de la liste sont disponibles pour tous les clients potentiels de votre application.

## <a name="in-the-store"></a>Dans le Windows Store 

Après avoir correctement effectué les étapes ci-dessus, l’état de la soumission passe de **Publication** à **Dans le Windows Store** . Votre envoi sera ensuite disponible dans le Microsoft Store que les clients pourront télécharger (sauf si vous avez choisi une autre option de [découverte](choose-visibility-options.md#discoverability) ). 

> [!NOTE]
> Nous effectuons également des contrôles ponctuels des applications une fois qu’elles ont été publiées afin que nous puissions identifier les problèmes potentiels et vous assurer que votre application est conforme à toutes les [stratégies de Microsoft Store](store-policies.md). Si nous détectons un problème, nous vous en informons et vous indiquons comment le résoudre, ou nous vous prévenons que votre application a été retirée du Windows Store.

 

 

 




