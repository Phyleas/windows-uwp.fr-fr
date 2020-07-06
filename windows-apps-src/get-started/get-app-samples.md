---
title: Obtenir des exemples d’application Windows
description: Découvrez comment télécharger des exemples de code Windows à partir de GitHub.
ms.date: 06/30/2020
ms.topic: article
keywords: windows 10, exemple de code, exemples de code
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 285388c4c1b4791e1ca271a853476416b6d13c13
ms.sourcegitcommit: 179f8098d10e338ad34fa84934f1654ec58161cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85757616"
---
# <a name="get-windows-app-samples"></a>Obtenir des exemples d’application Windows

De nombreux exemples de code Windows officiels sont disponibles dans différents dépôts GitHub, notamment des [exemples d’applications Plateforme Windows universelle (UWP)](https://github.com/microsoft/Windows-universal-samples), des [exemples Windows classiques](https://github.com/microsoft/Windows-classic-samples) ainsi qu’une collection d’[exemples de documentation pour les développeurs Windows](#windows-developer-documentation-samples). Ces exemples contiennent des démonstrations de la plupart des fonctionnalités Windows et de leurs modèles d’utilisation d’API.

:::image type="content" source="images/github-windows-samples-page.png" alt-text="Dépôt GitHub d’exemples universels Windows":::

Pour faciliter la recherche d’exemples spécifiques, vous pouvez explorer et effectuer des recherches dans une collection d’exemples de code classés par catégories pour différents outils et technologies de développement Microsoft via le [navigateur d’exemples](https://docs.microsoft.com/samples/browse/).

:::image type="content" source="images/samples-browser-windows.png" alt-text="Navigateur d’exemples Microsoft":::

### <a name="windows-developer-documentation-samples"></a>Exemples de documentation pour les développeurs Windows

Voici une liste d’exemples de mini-applications créées spécifiquement en appui de la documentation pour les développeurs Windows. Sauf indication contraire, les exemples suivants sont tous des applications Plateforme Windows universelle (UWP) qui ont été mises à jour pour utiliser les derniers contrôles [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4).

- [RSS Reader](https://github.com/Microsoft/Windows-appsample-rssreader) : permet de récupérer les flux RSS et de consulter des articles
- [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes) : permet d’explorer différentes modalités d’entrée et différents scénarios de sensibilisation des utilisateurs
- [Customer Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) : fonctionnalités utiles aux développeurs d’entreprise, comme l’authentification AAD (Azure Active Directory), les contrôles d’interface utilisateur (notamment une grille de données), l’intégration de bases de données SQL Azure et Sqlite, Entity Framework ainsi que les services d’API cloud
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) : permet de planifier les déjeuners avec vos amis et collègues
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook) : Windows Ink (barre d’outils Windows Ink incluse) et fonctionnalités du contrôleur radial (pour les appareils rotatifs comme Surface Dial)
- [Network Helper (Quiz Game)](https://github.com/Microsoft/Windows-appsample-networkhelper) : découverte réseau et communication
- [HUE Lights Controller](https://github.com/Microsoft/Windows-appsample-huelightcontroller) : domotique intelligente avec Cortana et Bluetooth basse consommation (Bluetooth LE)
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze) : jeu 3D de base utilisant DirectX
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab) : permet d’afficher et de modifier les fichiers image

## <a name="download-the-code"></a>Télécharger le code

Pour télécharger les exemples, accédez à l’un des dépôts Microsoft comme celui des [exemples d’applications Plateforme Windows universelle (UWP)](https://github.com/microsoft/Windows-universal-samples). Sélectionnez **Clone or download**, puis **Download ZIP**.

![Téléchargement des exemples](images/SamplesDownloadButton.png)

Le fichier .zip contient toujours les exemples les plus récents. Vous n’avez pas besoin d’un compte GitHub pour télécharger le fichier. Lors de la publication d’une mise à jour du kit SDK, ou si vous souhaitez obtenir des changements et ajouts récents, il vous suffit de télécharger le dernier fichier zip.

> [!NOTE]
> Pour ouvrir, générer et exécuter des exemples Windows, vous devez disposer de Visual Studio et du SDK Windows. Vous pouvez vous procurer une copie gratuite de [Visual Studio Community](https://www.microsoft.com/?ref=go).  
>
> Pour que les exemples fonctionnent correctement, veillez à décompresser l’intégralité de l’archive, et pas simplement les exemples individuels. Pour limiter la duplication, de nombreux exemples dépendent de fichiers communs contenus dans le dossier *SharedContent* et utilisent des fichiers liés, notamment des exemples de fichiers modèles et de ressources d’image.

## <a name="open-the-samples"></a>Ouvrir les exemples

Après avoir téléchargé le fichier .zip, ouvrez les exemples dans Visual Studio.

1. Avant de décompresser l’archive, cliquez avec le bouton droit sur le fichier, puis sélectionnez **Propriétés** > **Débloquer** > **Appliquer**. Décompressez ensuite l’archive dans un dossier local sur votre ordinateur.

    ![Archive décompressée](images/SamplesUnzip1.png)

2. Chaque dossier du dossier Samples contient un exemple de fonctionnalité Windows.

    ![Dossiers d’exemples](images/SamplesUnzip2.png)

3. Sélectionnez un exemple. Les langages pris en charge sont indiqués par des sous-dossiers propres aux langages.

    ![Dossiers de langages](images/SamplesUnzip3.png)

4. Sélectionnez le dossier correspondant à la langage que vous souhaitez utiliser. Dans le contenu du dossier, vous verrez un fichier de solution Visual Studio (.sln) que vous pouvez ouvrir dans Visual Studio.

    ![Solution VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Formuler des commentaires, poser des questions et signaler des problèmes

En cas de problème ou question, utilisez l’onglet **Problèmes** dans le dépôt pour signaler un nouveau problème.

![Image de commentaires](images/GitHubUWPSamplesFeedback.png)
