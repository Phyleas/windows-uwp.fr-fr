---
title: Conseils pour l’amélioration de votre workflow de développement sur Windows 10
description: Conseils pour l’amélioration de votre workflow de développement sur Windows 10.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, développeur, conseils, performances, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 7d02e3b46d6938532bbc7024e8840b976b2715a6
ms.sourcegitcommit: 861c381a31e4a5fd75f94ca19952b2baaa2b72df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171155"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>Conseils pour l’amélioration des performances et des workflows de développement

Nous avons compilé quelques conseils qui contribueront à rendre votre workflow plus efficace et plus agréable. Vous avez d’autres conseils à partager ? Déposez une demande de tirage (pull request) en utilisant le bouton « Éditer » ci-dessus, ou soumettez un problème en utilisant le bouton « Commentaires » ci-dessous. Nous les ajouterons peut-être à la liste.

> [!NOTE]
> Si vous rencontrez des problèmes de performances liés au développement sur Windows 10, par exemple :
> - Outils de développement (compilateurs, éditeurs de liens, etc.) s’exécutant plus lentement que prévu sur Windows.
> - Plateformes d’exécution (comme Node, .NET ou Python) fonctionnant plus lentement sur Windows que sur d’autres plateformes.
> - Applications présentant des problèmes de performances liés aux E/S, au réseau ou à la création de processus. 
> 
> Faites-nous part de votre problème en le signalant dans le [dépôt de problèmes Windows Developer (WinDev)](https://github.com/microsoft/WinDev).

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>Utiliser des raccourcis pour ouvrir un projet dans VS Code ou l’Explorateur de fichiers Windows

Vous pouvez lancer VS Code à partir de votre ligne de commande dans le projet que vous avez ouvert à l’aide de la commande `code .`. Vous pouvez également ouvrir votre répertoire de projet à partir de la ligne de commande avec l’Explorateur de fichiers Windows en utilisant `explorer.exe .` à partir de Windows ou de votre distribution WSL. Vous devrez peut-être ajouter l’exécutable de VS Code à votre variable d’environnement PATH si cela ne fonctionne pas par défaut. Pour en savoir plus, consultez [Launching from the Command Line](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line).

![Capture d’écran de l’Explorateur de fichiers Windows](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>Utiliser le gestionnaire d’informations d’identification pour simplifier le processus d’authentification

Si vous utilisez Git pour la gestion de versions et la collaboration, vous pouvez simplifier le processus d’authentification en [configurant Git Credential Manager](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup) pour stocker vos jetons dans le Gestionnaire d’informations d’identification Windows. Nous vous recommandons également d’[ajouter un fichier .gitignore](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file) à votre projet.

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>Utiliser WSL pour tester votre pipeline de production avant le déploiement sur le cloud

Le Sous-système Windows pour Linux permet aux développeurs d’exécuter un environnement GNU/Linux (et notamment la plupart des utilitaires, applications et outils en ligne de commande) directement sur Windows, sans modification et tout en évitant la surcharge d’une machine virtuelle traditionnelle ou d’une configuration à double démarrage.

WSL cible une audience de développeur avec l’intention d’être utilisé dans le cadre d’une boucle de développement interne. Supposons que Sam crée un pipeline CI/CD (intégration continue et livraison continue) et qu’il souhaite le tester d’abord sur un ordinateur local (portable) avant de le déployer dans le cloud. Sam peut activer WSL (et WSL 2 pour améliorer la vitesse et les performances), puis utiliser une vraie instance Linux Ubuntu en local (sur l’ordinateur portable) avec les commandes et les outils Bash qu’ils préfèrent. Une fois le pipeline de développement vérifié localement, Sam peut alors envoyer (push) ce pipeline CI/CD vers le cloud (par ex. Azure) en le plaçant dans un conteneur Docker et en envoyant le conteneur à une instance cloud où il s’exécutera sur une machine virtuelle Ubuntu prête pour la production.

Pour connaître d’autres manières d’utiliser WSL, consultez l’[épisode Tabs vs Spaces sur WSL 2](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux).

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>Optimiser la vitesse de performance de WSL en évitant de passer d’un système de fichiers à un autre

Si vous utilisez à la fois Windows et le Sous-système Windows pour Linux, deux systèmes de fichiers sont installés : NTSF (Windows) et WSL (votre distribution Linux). Pour optimiser les performances, assurez-vous que les fichiers de votre projet sont stockés sur le même système que les outils que vous utilisez. Pour en savoir plus, consultez [Utiliser le système de fichiers approprié pour des performances plus rapides](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance).

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>Améliorer les vitesses de génération en ajoutant des exclusions Windows Defender

Vous pouvez améliorer la vitesse de génération en mettant à jour vos paramètres Windows Defender afin d’ajouter des exclusions pour les dossiers de projet ou types de fichier que vous considérez comme fiables et ainsi éviter les recherches de menaces de sécurité. Pour en savoir plus, consultez [Mettre à jour les paramètres Windows Defender pour améliorer les performances](../android/defender-settings.md).

![Capture d’écran de Windows Defender](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>Lancer toutes vos lignes de commande dans le Terminal Windows en même temps

* Vous pouvez lancer plusieurs outils en ligne de commande comme PowerShell, Ubuntu et Azure CLI dans une unique fenêtre à plusieurs volets en utilisant les [arguments de ligne de commande du Terminal Windows](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes). Après avoir installé le [Terminal Windows ](/windows/terminal/get-started), [WSL/Ubuntu](/windows/wsl/install-win10)et [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest), entrez cette commande dans PowerShell pour ouvrir une nouvelle fenêtre à plusieurs volets avec les trois outils :

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>Partager vos conseils

Avez-vous des conseils pour aider d’autres développeurs qui utilisent Windows à améliorer leur workflow ? [Envoyez une demande de tirage](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md) en ajoutant votre conseil à la page ou [signalez un problème](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**) si vous souhaitez ajouter un conseil sur un sujet particulier.

Avez-vous des problèmes liés aux performances que vous aimeriez que nous puissions résoudre ? Signalez-les dans le nouveau [dépôt des problèmes WinDev](https://github.com/microsoft/windev).

Merci aux développeurs. Nous sommes à votre écoute et cherchons à améliorer votre expérience !
