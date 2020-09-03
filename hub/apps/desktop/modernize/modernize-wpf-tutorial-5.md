---
description: Ce tutoriel montre comment ajouter des interfaces utilisateur XAML UWP, créer des packages MSIX et incorporer d’autres composants modernes dans votre application WPF.
title: Créer un package et déployer avec MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands, îles xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 18b89caa0de947d2b95b46c3deb11378912b6012
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161423"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Partie 5 : Créer un package et déployer avec MSIX

Il s’agit de la dernière partie d’un tutoriel qui montre comment moderniser un exemple d’application de bureau WPF nommée Contoso Expenses. Pour obtenir une vue d’ensemble du tutoriel, des conditions préalables et des instructions pour le téléchargement de l’exemple d’application, consultez [Tutoriel : Moderniser une application WPF](modernize-wpf-tutorial.md). Cet article part du principe que vous avez terminé la [partie 4](modernize-wpf-tutorial-4.md).

Dans [partie 4](modernize-wpf-tutorial-4.md) vous avez appris que certaines API WinRT, dont l’API des notifications, requièrent une identité du package avant de pouvoir être utilisées dans une application. Vous pouvez obtenir l’identité du package en empaquetant l’application Contoso Expenses à l’aide de [MSIX](/windows/msix), le format d’empaquetage introduit dans Windows 10 pour empaqueter et déployer des applications Windows. MSIX offre aux développeurs et aux professionnels de l’informatique les avantages suivants :

- Utilisation du réseau et espace de stockage optimisés.
- Désinstallation propre complète, grâce à un conteneur léger dans lequel l’application est exécutée. Pas de clés de Registre et fichiers temporaires conservés sur le système.
- Découple les mises à jour du système d’exploitation des mises à jour et personnalisations de l’application.
- Simplifie le processus d’installation, de mise à jour et de désinstallation.

Cette partie du tutoriel explique comment empaqueter l’application Contoso Expenses dans un package MSIX.

## <a name="package-the-application"></a>Empaqueter l’application

Visual Studio 2019 offre un moyen simple d’empaqueter une application de bureau à l’aide d’un Projet de création de package d’application Windows. 

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur la solution **ContosoExpenses**, puis choisissez **Ajouter > Nouveau projet**.

    ![Ajouter un nouveau projet](images/wpf-modernize-tutorial/AddNewProject.png)

3. Dans la boîte de dialogue **Ajouter un nouveau projet**, recherchez `packaging`, choisissez le modèle **Projet de création de package d’application Windows** dans la catégorie C#, puis cliquez sur **Suivant**.

    ![Projet de création de package d’application Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nommez le nouveau projet `ContosoExpenses.Package`, puis cliquez sur **Créer**.

5. Sélectionnez **Windows 10, version 1903 (10.0; Build 18362)** pour la **Version cible** et la **Version minimale**, puis cliquez sur **OK**.

    Le projet **ContosoExpenses. Package** est ajouté à la solution **ContosoExpenses**. Ce projet comprend un [manifeste de package](/uwp/schemas/appxpackage/uapmanifestschema/schema-root), décrivant l’application, ainsi que des ressources par défaut qui sont utilisées pour des éléments tels que l’icône dans le menu programmes et la vignette dans l’écran d’accueil. Toutefois, contrairement à un projet UWP, le projet de création de package ne contient pas de code. Son objectif est d’empaqueter une application de bureau existante.

6. Dans le projet **ContosoExpenses.Package**, cliquez avec le bouton droit sur le nœud **Applications**, puis choisissez **Ajouter une référence**. Ce nœud spécifie les applications de votre solution qui seront incluses dans le package.

6. Dans la liste des projets, sélectionnez **ContosoExpenses.Core**, puis cliquez sur **OK**.

7. Développez le nœud **Applications** et vérifiez que le projet **ContosoExpense.Core** est référencé et affiché en caractères gras. Cela signifie qu’il sera utilisé comme point de départ pour le package.

8. Cliquez avec le bouton droit sur le projet **ContosoExpenses.Package**, puis choisissez **Définir comme projet de démarrage**.

9. Appuyez sur **F5** pour démarrer l’application empaquetée dans le débogueur.

À ce stade, vous pouvez remarquer des modifications indiquant que l’application s’exécute maintenant en tant qu’application empaquetée :

- L’icône affichée dans la barre des tâches ou dans le menu Démarrer est désormais la ressource par défaut incluse dans chaque **Projet de création de package d’applications Windows**.
- En cliquant avec le bouton droit sur l’application **ContosoExpense.Package** affichée dans le menu Démarrer, vous pouvez constater la présence d’options généralement réservées aux applications téléchargées à partir du Microsoft Store, telles que **Paramètres de l’application**, **Évaluer et donner un avis** et **Partager**.

    ![ContosoExpenses dans le menu Démarrer](images/wpf-modernize-tutorial/StartMenu.png)

- Si vous souhaitez désinstaller l’application, cliquez avec le bouton droit sur **ContosoExpense.Package** dans le menu Démarrer, puis choisissez **Désinstaller**. L’application est immédiatement supprimée, sans rien laisser sur le système.

## <a name="test-the-notification"></a>Tester la notification

À présent que vous avez empaqueté l’application Contoso Expenses avec MSIX, vous pouvez tester le scénario de notification qui ne fonctionnait pas à la fin de la [partie 4](modernize-wpf-tutorial-4.md).

1. Dans l’application Contoso Expenses, choisissez un employé dans la liste, puis cliquez sur le bouton **Ajouter une nouvelle dépense**.
2. Complétez tous les champs du formulaire, puis appuyez sur **Enregistrer**.
3. Vérifiez que vous voyez une notification du système d’exploitation.

![Notification toast](images/wpf-modernize-tutorial/ToastNotification.png)