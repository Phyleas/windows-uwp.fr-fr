---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit de certification des applications Windows
description: Pour maximiser les chances de publication sur le Microsoft Store ou de certification Windows de votre application, validez-la et testez-la sur votre ordinateur avant de l’envoyer pour certification. Cette rubrique explique comment installer et exécuter le Kit de certification des applications Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, certification des applications
ms.localizationpriority: medium
ms.openlocfilehash: be02f9b049a1beb1866d21c97f11fe3efeb815f3
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339347"
---
# <a name="windows-app-certification-kit"></a>Kit de certification des applications Windows

Pour faire [certifier votre application par Windows](/windows/win32/win_cert/windows-certification-portal) et préparer sa [publication sur le Microsoft Store](../publish/app-submissions.md), vous devez d’abord la valider et la tester localement. Cette rubrique explique comment installer et exécuter le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) pour veiller à ce que votre application soit aussi sûre qu'efficace.

## <a name="prerequisites"></a>Prérequis

Conditions préalables pour tester une application Windows universelle :

- Vous devez installer et exécuter Windows 10.
- Vous devez installer le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/downloads/app-certification-kit/), qui est inclus dans le kit de développement logiciel (SDK) Windows pour Windows 10.
- Vous devez [activer votre appareil pour le développement](/windows/apps/get-started/enable-your-device-for-development).
- Vous devez déployer l’application Windows que vous voulez tester sur votre ordinateur.

> [!NOTE]
> **Mises à niveau en place :** L’installation d'une version plus récente du [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) remplace toute version précédente installée.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validez votre application Windows de manière interactive à l’aide du Kit de certification des applications Windows

1. À partir du menu **Démarrer**, recherchez **Applications**, **Kits Windows**, puis cliquez sur **Kit de certification des applications Windows**.

2. Dans le Kit de certification des applications Windows, sélectionnez la catégorie de validation que vous souhaitez effectuer. Par exemple : si vous validez une application Windows, sélectionnez **Valider une application Windows**.

    Vous pouvez accéder directement à l’application que vous testez ou choisir l’application dans une liste depuis l’interface utilisateur. Quand le Kit de certification des applications Windows est exécuté pour la première fois, l’interface utilisateur répertorie toutes les applications Windows que vous avez installées sur votre ordinateur. Pour toutes les exécutions ultérieures, l’interface utilisateur affiche les applications Windows les plus récentes que vous avez validées. Si l’application que vous voulez tester n’est pas répertoriée, vous pouvez cliquer sur **Mon application n’est pas répertoriée** pour obtenir une liste exhaustive de toutes les applications installées sur votre système.

3. Une fois que vous avez entré ou sélectionné l’application que vous voulez tester, cliquez sur **Suivant**.

4. À partir de l’écran suivant, vous verrez le flux de travail de test correspondant au type d’application que vous testez. Les tests grisés dans la liste ne sont pas applicables à votre environnement. Par exemple, si vous testez une application Windows 10 sous Windows 7, seuls les tests statiques s’appliqueront au flux de travail. Notez que le Microsoft Store peut appliquer tous les tests à partir de ce flux de travail. Sélectionnez les tests que vous souhaitez exécuter et cliquez sur **Suivant**.

    Le Kit de certification des applications Windows commence la validation de l’application.

5. À l’invite après le test, entrez le chemin d’accès du dossier où vous voulez enregistrer le rapport de test.

    Le Kit de certification des applications Windows crée un fichier HTML, ainsi qu’un rapport XML et l’enregistre dans ce dossier.

6. Ouvrez le fichier de rapport et examinez les résultats du test.

> [!NOTE]
> Si vous utilisez Visual Studio, vous pouvez exécuter le Kit de certification des applications Windows lorsque vous créez votre package d’application. Pour plus d’informations, voir [Création de packages d’application UWP](/windows/msix/package/packaging-uwp-apps).

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validez votre application Windows à l’aide du Kit de certification des applications Windows à partir d’une ligne de commande

> [!IMPORTANT]
> Le Kit de certification des applications Windows doit être exécuté dans le contexte d’une session utilisateur active.

1. Dans la fenêtre de commande, accédez au répertoire contenant le Kit de certification des applications Windows.

    **Remarque** Le chemin par défaut est C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\.

2. Entrez les commandes suivantes dans cet ordre pour tester une application qui est déjà installée sur votre ordinateur de test :

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Vous pouvez également utiliser les commandes suivantes si l’application n’est pas installée. Le Kit de certification des applications Windows ouvre le package et applique le flux de travail de test approprié :

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. Une fois le test terminé, ouvrez le fichier de rapport nommé `[report file name]` et examinez les résultats du test.

**Remarque** Le Kit de certification des applications Windows peut être exécuté à partir d’un service, mais le service doit initialiser le processus du Kit dans une session utilisateur active et ne peut pas être exécuté au sein de Session0.

**Remarque** Pour en savoir plus sur la ligne de commande du Kit de certification des applications Windows, entrez la commande `appcert.exe /?`.

## <a name="testing-with-a-low-power-computer"></a>Test avec un ordinateur à faible consommation d’énergie

Les seuils du test de performances du Kit de certification des applications Windows sont basés sur les performances d’un ordinateur à faible consommation d’énergie.

Les caractéristiques de l’ordinateur sur lequel le test est exécuté peuvent influencer les résultats du test. Pour déterminer si les performances de votre application répondent aux [Politiques du Microsoft Store](/legal/windows/agreements/store-policies), nous vous recommandons de tester votre application sur un ordinateur à faible consommation d’énergie, tel qu’un ordinateur équipé d’un processeur Intel Atom, d’une résolution d’écran de 1366 x 768 (ou supérieure) et d’un disque dur rotatif (par opposition à un disque SSD).

À mesure que les ordinateurs à faible consommation d’énergie évoluent, leurs caractéristiques de performances peuvent elles aussi varier. Reportez-vous aux [Politiques du Microsoft Store](/legal/windows/agreements/store-policies) les plus récentes et testez votre application avec la dernière version du Kit de certification des applications Windows pour vous assurer que votre application est conforme aux dernières spécifications en matière de performances.

## <a name="related-topics"></a>Rubriques connexes

- [Tests du Kit de certification des applications Windows](windows-app-certification-kit-tests.md)
- [Politiques du Microsoft Store](/legal/windows/agreements/store-policies)