---
Description: Lorsque vous avez terminé la création de la soumission de votre application et que vous cliquez sur envoyer au magasin, la soumission passe à l’étape de certification.
title: Processus de certification des applications
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, publication, prétraitement, certification, version, en attente, envoyer, publier, État, heure
ms.localizationpriority: medium
ms.openlocfilehash: d88d8deeb467f186f120fb8c1e579d5c9222aaf1
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852833"
---
# <a name="the-app-certification-process"></a>Processus de certification des applications

Une fois que vous avez terminé de créer la soumission de votre application et cliqué sur **Envoyer au Store**, la soumission passe à l’étape de certification. Ce processus s’effectue généralement en quelques heures, bien qu’il nécessite parfois trois journées ouvrables entières. Une fois que votre envoi est certifié, il peut s’écouler jusqu’à 24 heures avant que les clients puissent voir la liste de l’application pour une nouvelle soumission, ou pour une soumission mise à jour avec les modifications apportées aux packages. Si votre mise à jour ne modifie que la liste des détails, le processus de publication sera effectué en moins d’une heure.  Vous serez informé de la publication de votre envoi et de l’état de l’application dans le tableau de **bord.**

## <a name="preprocessing"></a>Prétraitement

Une fois que vous avez chargé avec succès les packages de l’application et soumis l’application pour certification, les packages sont mis en file d’attente à des fins de test. Un message s’affiche si des erreurs sont détectées pendant le pré-traitement. Pour plus d’informations sur les erreurs potentielles, voir [Résoudre les erreurs de soumission](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Plusieurs tests sont exécutés pendant cette phase :

-   **Tests de sécurité :** Ce premier test vérifie que les packages de votre application sont exempts de virus et de programmes malveillants. Si votre application échoue à ce test, vous devez vérifier votre système de développement en exécutant l’antivirus le plus récent, puis régénérer le package de votre application sur un système propre.
-   **Tests de conformité technique :** la conformité technique est testée à l’aide du Kit de certification des applications Windows. (Vous devez toujours prendre soin de [tester votre application à l’aide du Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) avant de la soumettre au Windows Store.)
-   **Conformité du contenu :** la durée de cette opération varie en fonction de la complexité de votre application, de la quantité de contenu visuel qu’elle comporte et du nombre d’applications que vous avez soumises récemment. Veillez à fournir toutes les informations que les testeurs doivent connaître dans la page [Remarques pour la certification](notes-for-certification.md).

Une fois le processus de certification terminé, vous recevez un rapport de certification indiquant si votre application a été ou non certifiée. Si votre application n’a pas été certifiée, le rapport indique le test auquel elle a échoué ou la [stratégie](store-policies.md) qui n’a pas été respectée. Après avoir corrigé le problème, vous pouvez créer une autre soumission pour votre application afin de recommencer le processus de certification.

## <a name="release"></a>Version finale

Lorsque votre application passe la certification, elle est prête à passer au processus de **publication** .

- Si vous avez indiqué que votre soumission doit être publiée dès que possible (option par défaut), le processus de publication commence immédiatement.
- S’il s’agit de la première fois que vous publiez l’application et que vous avez spécifié une **Date de publication** dans la section [planification](configure-precise-release-scheduling.md#release) , l’application devient disponible en fonction de vos sélections de **Date de publication** .
- Si vous avez utilisé les [options de conservation](manage-submission-options.md#publishing-hold-options) de la publication pour spécifier qu’elle ne doit pas être libérée avant une certaine date, nous attendons jusqu’à cette date pour commencer le processus de publication, sauf si vous sélectionnez Modifier la **Date**de publication.
- Si vous avez utilisé les [options de conservation](manage-submission-options.md#publishing-hold-options) de la publication pour spécifier que vous souhaitez publier l’envoi manuellement, nous ne démarrerons pas le processus de publication tant que vous n’avez pas sélectionné **publier maintenant** (ou sélectionné **modifier la date** de publication et choisi une date spécifique).


## <a name="publishing"></a>Publication

Les packages de votre application sont signés numériquement pour être protégés contre toute falsification après leur publication. Une fois que cette phase a commencé, vous ne pouvez plus annuler votre soumission ni en modifier la date de sortie.

Pour les nouvelles applications et les mises à jour qui incluent des modifications apportées aux packages de l’application, le processus de publication se termine dans les 24 heures. Pour les mises à jour qui modifient uniquement les options telles que les détails de la liste des magasins, mais ne modifient pas les packages de l’application, le processus de publication prendra moins d’une heure.

Pendant que votre application se trouve dans la phase de publication, le lien **afficher les détails** dans la colonne État de l’envoi de votre application vous permet de savoir quand vos nouveaux packages et les détails de la liste des magasins sont disponibles pour les clients sur chacune de vos versions de système d’exploitation prises en charge. Les étapes qui n’ont pas encore été réalisées s’affichent comme **En attente**. Votre application restera dans la phase de publication jusqu’à ce que le processus soit terminé, ce qui signifie que les nouveaux packages et/ou les détails de la liste sont disponibles pour tous les clients potentiels de votre application.

## <a name="in-the-store"></a>Dans le Windows Store 

Après avoir correctement effectué les étapes ci-dessus, l’état de la soumission passe de **Publication** à **Dans le Windows Store**. Les clients peuvent ainsi télécharger votre soumission à partir du Microsoft Store (sauf si vous avez choisi une autre option pour [Détectabilité](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nous effectuons également des vérifications ponctuelles sur les applications publiées pour identifier les problèmes potentiels et vérifier que votre application est conforme à toutes les [politiques du Microsoft Store](store-policies.md). Si nous détectons un problème, nous vous en informons et vous indiquons comment le résoudre, ou nous vous prévenons que votre application a été retirée du Windows Store.

 

 

 




