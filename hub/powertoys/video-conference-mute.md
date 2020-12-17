---
title: Utilitaire de désactivation de la vidéoconférence PowerToys pour Windows 10
description: Utilitaire qui permet aux utilisateurs de désactiver rapidement le microphone (audio) et d’éteindre l’appareil photo (vidéo) pendant une conférence téléphonique avec une seule frappe de touche, quelle que soit l’application qui a le focus sur l’ordinateur.
ms.date: 12/07/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9a7ec901ffc31e565adb94a1a043fd149064c12
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612047"
---
# <a name="video-conference-mute-preview"></a>Vidéoconférence muette (version préliminaire)

> [!IMPORTANT]
> Il s’agit d’une fonctionnalité en version préliminaire qui est incluse uniquement dans la préversion de PowerToys. L’exécution de cette version préliminaire requiert Windows 10 version 1903 (Build 18362) ou version ultérieure.

Éteignez rapidement votre microphone (audio) et éteignez votre appareil photo (vidéo) pendant une conférence téléphonique avec une seule frappe, quelle que soit l’application qui a le focus sur votre ordinateur.

## <a name="usage"></a>Usage

Les paramètres par défaut pour utiliser la vidéoconférence muette sont les suivants :

- <kbd>⊞ Win</kbd> + <kbd>N</kbd> pour basculer l’audio et la vidéo en même temps
- <kbd>⊞ Win</kbd> + <kbd>MAJ</kbd> + Pour activer/désactiver <kbd>le</kbd> microphone
- <kbd>⊞ Win</kbd> + <kbd>MAJ</kbd> + <kbd>O</kbd> pour basculer la vidéo

![Capture d’écran des notifications muet audio et vidéo](../images/pt-video-audio-mute-notification.png)

Quand vous utilisez les touches de raccourci du microphone et/ou de l’appareil photo, vous voyez s’afficher une petite fenêtre de barre d’outils qui vous indique si le microphone et l’appareil photo sont activés, désactivés ou non utilisés. Vous pouvez définir la position de cette barre d’outils dans l’onglet de la vidéoconférence muette des paramètres PowerToys.

> [!NOTE]
> N’oubliez pas que la [version préliminaire/expérimentale de PowerToys](https://github.com/microsoft/PowerToys/releases/) doit être installée et en cours d’exécution, avec la fonctionnalité de mise en sourdine des vidéoconférences activée dans les paramètres PowerToys pour pouvoir utiliser cet utilitaire.

## <a name="settings"></a>Paramètres

L’onglet vidéoconférence de la vidéoconférence dans les paramètres PowerToys offre les options suivantes :

- **Raccourcis :** Modifiez la touche de raccourci utilisée pour désactiver votre microphone, votre appareil photo ou les deux.
- **Microphone sélectionné :** Sélectionnez le microphone sur votre ordinateur qui sera ciblé par cet utilitaire.
- **Appareil photo sélectionné :** Sélectionnez l’appareil photo sur votre ordinateur qui sera ciblé par cet utilitaire.
- **Image de superposition d’appareil photo :** Sélectionnez une image à utiliser comme espace réservé lorsque votre appareil photo est désactivé. (Par défaut, un écran noir s’affiche lorsque l’appareil photo est désactivé avec cet utilitaire).
- **Barre d’outils :** Détermine l’emplacement d’affichage du *microphone, de l’appareil photo dans* la barre d’outils quand l’option bascule (coin supérieur droit par défaut).
- **Afficher la barre d’outils sur :** Indiquez si vous préférez que la barre d’outils soit affichée sur le moniteur principal uniquement (valeur par défaut) ou sur toutes les analyses.
- **Masquer la barre d’outils quand l’appareil photo et le microphone sont** activés : une case à cocher est disponible pour activer ou désactiver cette option.

![Options de sourdine pour la vidéoconférence dans les paramètres PowerToys](../images/pt-video-conference-mute-settings.png)

## <a name="how-does-this-work-under-the-hood"></a>Comment cela fonctionne-t-il dans le capot

L’application interagit avec le son et la vidéo de différentes manières.

Si une caméra cesse de fonctionner, l’application qui l’utilise ne peut pas être récupérée tant que l’API n’a pas fait l’expérience d’une réinitialisation complète. Pour activer ou désactiver l’appareil photo de confidentialité globale lors de l’utilisation de l’appareil photo dans une application, en général, il se bloque et ne peut pas être récupéré.

Donc, comment les PowerToys gèrent-ils cela afin que vous puissiez conserver la diffusion en continu ?

- **Audio :** PowerToys utilise l’API muet de microphone global dans Windows.  Les applications doivent être récupérées lorsque cette option est activée ou désactivée.
- **Vidéo :** PowerToys dispose d’un pilote virtuel pour l’appareil photo. La vidéo est routée via le pilote et redirigée vers l’application. Le fait de sélectionner la touche de raccourci de la vidéoconférence vidéo arrête la vidéo de la diffusion en continu, mais l’application pense toujours qu’elle reçoit de la vidéo, la vidéo est simplement remplacée par le noir ou l’espace réservé d’image que vous avez enregistré dans les paramètres.

### <a name="debug-the-camera-driver"></a>Déboguer le pilote de l’appareil photo

Pour déboguer le pilote de l’appareil photo, recherchez dans ce dossier sur votre ordinateur :

```markdown
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\PowerToysVideoConference.log
```

Vous pouvez également créer un vide `PowerToysVideoConferenceVerbose.flag` dans le même répertoire pour activer le mode de journalisation détaillé dans le pilote.

## <a name="known-issues"></a>Problèmes connus

Pour afficher tous les problèmes connus actuellement ouverts sur l’utilitaire Video Conference muet, consultez [#6246 de suivi PowerToys sur GitHub](https://github.com/microsoft/PowerToys/issues/6246). L’équipe de développement PowerToys et la communauté de contributeurs travaillent activement à la résolution de ces problèmes et planifient la conservation de l’utilitaire en version préliminaire jusqu’à ce que les problèmes essentiels soient résolus.

![Capture d’écran de la conférence vidéo PowerToys avec 5 participants et paramètres de périphérique affichés](../images/pt-video-conference.png)
