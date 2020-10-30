---
description: Résolvez les événements et les erreurs de l’application Examen à l’aide de l’observateur d’événements.
title: Résolvez à l’aide de l’observateur d’événements.
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
keywords: windows 10, uwp, éducation
ms.localizationpriority: medium
ms.openlocfilehash: cd30d54f1bff5fd43fbeb6e286e327fed9f8a585
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031482"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>Résolvez à l’aide de l’observateur d’événements.

Vous pouvez utiliser l’observateur d’événements pour afficher les événements et les erreurs de l’application Examen. L’application Examen consigne les événements lorsqu’une demande de verrouillage a été reçue, qu’une inscription d’appareil a été menée à bien, que des politiques de verrouillage ont été appliquées, et plus encore.

Pour activer l’affichage des événements dans l’observateur d’événements :
1. Ouvrez la `Event Viewer`
2. Accédez à `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`.
3. Cliquez avec le bouton droit `Operational` et sélectionnez `Enable Log`

Pour enregistrer les journaux d’événement :
1. Cliquez avec le bouton droit sur `Operational`
2. Cliquez sur `Save All Events As…`
