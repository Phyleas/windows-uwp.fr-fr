---
title: Utilitaire d’exécution PowerToys pour Windows 10
description: Un lancement rapide pour les utilisateurs avec pouvoir qui contient des fonctionnalités supplémentaires sans sacrifier les performances.
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 126c38cd98f0d8ff1102c7f53f14cb95ec7e38c5
ms.sourcegitcommit: 382ae62f9d9bf980399a3f654e40ef4f85eae328
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99534398"
---
# <a name="powertoys-run-utility"></a>Utilitaire d’exécution PowerToys

L’exécution d’PowerToys est un lancement rapide pour les utilisateurs avec pouvoir qui contient des fonctionnalités supplémentaires sans sacrifier les performances. Il s’agit d’un utilitaire open source et modulaire qui peut recevoir des plug-ins supplémentaires.

Pour utiliser PowerToys Run, sélectionnez <kbd></kbd> + <kbd>espace</kbd> Alt et commencez à taper !

*Si ce raccourci n’est pas ce que vous aimez, ne vous inquiétez pas, il est entièrement configurable dans les paramètres.*

![Démonstration d’exécution des applications avec PowerToys](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>Configuration requise

- Windows 10 version 1903 ou ultérieure
- Après l’installation, les PowerToys doivent être activés et en cours d’exécution en arrière-plan pour que cet utilitaire fonctionne

## <a name="features"></a>Fonctionnalités

Les fonctionnalités d’exécution PowerToys sont les suivantes :

- Rechercher des applications, des dossiers ou des fichiers

- Rechercher les processus en cours d’exécution (précédemment appelés [WindowWalker](https://github.com/betsegaw/windowwalker/))

- Boutons cliquables avec raccourcis clavier (par exemple, *ouvrir en tant qu’administrateur* ou *ouvrir le dossier conteneur*)

- Appeler le plug-in Shell à l’aide `>`  de (par exemple, `> Shell:startup` ouvrira le dossier de démarrage de Windows)

- Effectuer un calcul simple à l’aide de la calculatrice

## <a name="settings"></a>Paramètres

Les options d’exécution suivantes sont disponibles dans le menu des paramètres PowerToys.

  | **Paramètres** |**Action** |
  | --- | --- |
  | Ouvrir PowerToys Run | Définir le raccourci clavier pour ouvrir/masquer l’exécution des PowerToys |
  | Ignorer les raccourcis en mode plein écran |  En mode plein écran (F11), l’exécution ne sera pas engagée avec le raccourci |
  | Nombre maximal de résultats |  Nombre maximal de résultats affichés sans défilement |
  | Effacer la requête précédente au lancement | Lors du lancement, les recherches précédentes ne seront pas mises en surbrillance |
  | Désactiver l’avertissement de détection de lecteur | L’avertissement, si tous vos lecteurs ne sont pas indexés, n’est plus visible |

## <a name="keyboard-shortcuts"></a>Raccourcis clavier

  | **Raccourcis** | **Action** |
  | --- | --- |
  | Alt + espace | Ouvrir ou masquer l’exécution des PowerToys |
  | Échap | Masquer l’exécution des PowerToys |
  | Ctrl+Shift+Enter | (Applicable uniquement aux applications) Ouvrir l’application sélectionnée en tant qu’administrateur |
  | Ctrl+Maj+E | (Applicable uniquement aux applications et aux fichiers) Ouvrir le dossier contenant dans l’Explorateur de fichiers |
  | Ctrl+C | (Applicable uniquement aux dossiers et aux fichiers) Copier l’emplacement du chemin |
  | Onglet | Parcourir les résultats de la recherche et les boutons de menu contextuel |

## <a name="action-key"></a>Clé d’action

Celles-ci forcent l’exécution des PowerToys dans des plug-ins ciblés uniquement.

  | **Clé d’action** | **Action** |
  | --- | --- |
  | `=` | Calculatrice uniquement. Exemple `=2+2` |
  | `?` | Recherche de fichiers uniquement. Exemple `?road` de recherche `roadmap.txt` |
  | `.` | Recherche d’applications installée uniquement. Exemple `.code` pour récupérer Visual Studio code |
  | `//` | URL uniquement. Exemple `//docs.microsoft.com` pour que votre navigateur par défaut accède à https://docs.microsoft.com |
  | `<` | Processus en cours d’exécution uniquement. Exemple `<outlook` de recherche de tous les processus qui contiennent Outlook |
  | `>` | Commande shell uniquement. Exemple `>ping localhost` de requête ping |
  | `:` | Clés de Registre uniquement. Exemple `:hkcu` de recherche de la clé de registre HKEY_CURRENT_USER |
  | `!` | Services Windows uniquement. Exemple `!alu` de recherche du service de passerelle de la couche application à démarrer ou à arrêter |

## <a name="system-commands"></a>Commandes système

Avec PowerToys v 0.31 et on, vous pouvez désormais exécuter des actions au niveau du système.

  | **Clé d’action**   |   **Action** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | Arrête l’ordinateur |
  | `Restart` | Redémarre l’ordinateur |
  | `Sign Out` | Signe l’extraction de l’utilisateur actuel |
  | `Lock` | Verrouille l’ordinateur |
  | `Sleep` | Met l’ordinateur en veille |
  | `Hibernate` | Met l’ordinateur en veille prolongée |
  | `Empty Recycle Bin` | Vide la corbeille |

## <a name="indexer-settings"></a>Paramètres de l’indexeur

Si les paramètres de l’indexeur ne sont pas configurés pour couvrir tous les lecteurs, vous recevrez l’avertissement suivant :

![Avertissement d’exécution de l’indexeur PowerToys](../images/pt-run-warning.png)

Vous pouvez désactiver l’avertissement dans les paramètres PowerToys ou sélectionner l’avertissement pour développer les lecteurs en cours d’indexation. Une fois l’avertissement sélectionné, les options de la « recherche de fenêtres » des paramètres Windows 10 s’ouvrent.

![Paramètres d’indexation](../images/pt-run-indexing.png)

Dans ce menu « recherche de fenêtres », vous pouvez :

- Sélectionnez le mode « étendu » pour activer l’indexation sur tous les lecteurs de votre ordinateur Windows 10.
- Spécifiez les chemins d’accès des dossiers à exclure.
- Sélectionnez les paramètres de l’indexeur de recherche avancée (en bas des options de menu) pour définir les paramètres d’index avancés, ajouter ou supprimer des emplacements de recherche, indexer les fichiers chiffrés, etc.

![Paramètres d’indexation avancés](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>Problèmes connus

Pour obtenir la liste de tous les problèmes connus et les suggestions, consultez le [produit PowerToys référentiel Problems on GitHub](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher).

## <a name="attribution"></a>Attribution

- [Wox](https://github.com/Wox-launcher/Wox/)

- [Explorateur de fenêtres de la version bêta de Tadele](https://github.com/betsegaw/windowwalker)
