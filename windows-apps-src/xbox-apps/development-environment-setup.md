---
title: Configurer votre plateforme UWP sur l’environnement de développement Xbox
description: Apprenez à configurer et à tester un environnement de développement UWP sur Xbox, composé d’un ordinateur de développement connecté à une console Xbox One via un réseau local.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 408568e38bd9147564e7ebece17466e22364f3cc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164183"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Configurer votre plateforme UWP sur l’environnement de développement Xbox

La plateforme Windows universelle (UWP) sur l’environnement de développement Xbox se compose d’un ordinateur dédié au développement connecté à une console Xbox One via un réseau local.
Le PC de développement nécessite Visual Studio 2015 Update 3, Visual Studio 2017 ou Visual Studio 2019.
Le PC de développement requiert également Windows 10, la version 14393 du kit de développement logiciel (SDK) Windows 10 ou une version ultérieure, ainsi qu’un large éventail d’outils de prise en charge.

Cet article couvre les étapes relatives à la configuration et au test de votre environnement de développement.

## <a name="visual-studio-setup"></a>Installation Visual Studio

1. Installez Visual Studio 2015 Update 3, Visual Studio 2017 ou Visual Studio 2019. Pour en savoir plus et pour l’installation, voir [Téléchargements et outils pour Windows 10](https://dev.windows.com/downloads). Nous vous recommandons d’utiliser la dernière version de Visual Studio afin de pouvoir recevoir les dernières mises à jour pour les développeurs et la sécurité.


2. Si vous installez Visual Studio 2017 ou Visual Studio 2019, veillez à choisir la charge de travail **développement plateforme Windows universelle** . Si vous êtes un développeur C++, veillez également à activer la case à cocher **outils de plateforme Windows universelle c++** dans le volet **Résumé** à droite, sous **plateforme Windows universelle développement**. Il ne fait pas partie de l’installation par défaut.

    ![Installer Visual Studio 2019](images/development-environment-setup-1.png)

    Si vous installez Visual Studio 2015 Update 3, vérifiez que la case à cocher **outils de développement d’applications Windows universelles** est activée.

    ![Installer Visual Studio 2015 Update 2 ou version ultérieure.](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Installation du Kit de développement logiciel (SDK) Windows 10

Installez la dernière version du Kit SDK Windows 10. Il s’agit de votre installation de Visual Studio, mais si vous souhaitez la télécharger séparément, consultez le [Kit de développement logiciel (SDK) Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).


## <a name="enabling-developer-mode"></a>Activation du mode développeur

Avant de pouvoir déployer des applications à partir de votre PC de développement, vous devez activer le mode développeur. Dans l’application **paramètres** , accédez à **mettre à jour & sécurité**  /  **pour les développeurs**, puis sous **utiliser les fonctionnalités de développement**, sélectionnez **mode développeur**.

## <a name="setting-up-your-xbox-one"></a>Configuration de votre Xbox One

Avant de pouvoir déployer une application sur votre Xbox One, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode développeur. 

## <a name="create-your-first-app"></a>Créer votre première application

1. Assurez-vous que l’ordinateur de développement se trouve sur le même réseau local que la console Xbox One cible. En règle générale, cela signifie qu’ils doivent utiliser le même routeur et être sur le même sous-réseau. Une connexion réseau câblée est recommandée.

2. Assurez-vous que votre console Xbox One se trouve en Mode développeur.  Pour plus d’informations, consultez [activation de Xbox One en mode développeur](devkit-activation.md).

3. Déterminez le langage de programmation que vous voulez utiliser pour votre application UWP.

4. Sur votre PC de développement, dans Visual Studio, sélectionnez **nouveau/projet**.

5. Dans la fenêtre **nouveau projet** , sélectionnez **application Windows universelle/vide (Windows universel)**.

### <a name="starting-a-c-project"></a>Démarrage d’un projet c#

  ![Boîte de dialogue Nouveau projet](images/development-environment-setup-2.png)

1. Dans la boîte de dialogue **nouveau projet Windows universel** , sélectionnez Build 14393 ou version ultérieure dans la liste déroulante **version minimale** . Sélectionnez le dernier Kit de développement logiciel (SDK) dans la liste déroulante **version cible** . Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

2. Configurez votre environnement de développement pour le débogage à distance :

    a. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.

    b. Sous l’onglet **Déboguer** , remplacez **plateforme** par **x64**. (x86 n’est plus une plateforme prise en charge sur Xbox.)

    c. Sous **options de démarrage**, remplacez l' **appareil cible** par **ordinateur distant**.

    d. Dans **Ordinateur distant**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

    e. Dans la liste déroulante **Mode d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

    ![Pages de propriétés BlankApp C#](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Démarrage d’un projet C++

  ![Projet C++](images/development-environment-setup-3.png)

1. Dans la boîte de dialogue **nouveau projet Windows universel** , sélectionnez Build 14393 ou version ultérieure dans la liste déroulante **version minimale** . Sélectionnez le dernier Kit de développement logiciel (SDK) dans la liste déroulante **version cible** . Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

2. Configurez votre environnement de développement pour le débogage à distance :

   a. Dans le **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.

   b. Sur l’onglet **Débogage** et définissez **Débogueur à lancer** sur **Ordinateur distant**.

   c. Dans **Nom de l’ordinateur**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

   d. Dans la liste déroulante **Type d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

   e. Dans la liste déroulante **Platform** , sélectionnez **x64**.

    ![Pages de propriétés BlankApp C++](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>ÉPINGLer votre appareil avec Visual Studio

1. Enregistrez vos paramètres et assurez-vous que votre console Xbox One est en Mode développeur.

2. Une fois votre projet ouvert dans Visual Studio, appuyez sur F5.

3. S’il s’agit de votre premier déploiement, vous obtiendrez une boîte de dialogue issue de Visual Studio vous demandant d’épingler par paire votre appareil.

    a. Pour obtenir un code confidentiel, ouvrez **Dev Home** à partir de l’écran d’accueil sur votre console Xbox One.

    b. Sous l’onglet dossier de **démarrage** , sous **actions rapides**, sélectionnez **afficher le code PIN de Visual Studio**.
  
    ![Boîte de dialogue Jumeler avec Visual Studio](images/development-environment-setup-5.png)

    c. Entrez votre code confidentiel dans la boîte de dialogue **Jumeler avec Visual Studio**. Le code confidentiel suivant est juste un exemple. Le vôtre sera différent.

    ![Boîte de dialogue du code confidentiel Jumeler avec Visual Studio](images/devhome_pin.png)

    d. Des erreurs de déploiement, le cas échéant, s’afficheront dans la fenêtre **Sortie**.

Félicitations, vous avez correctement créé et déployé votre première application UWP sur Xbox !

## <a name="see-also"></a>Voir aussi
- [Activation du Mode développeur Xbox One](devkit-activation.md)  
- [Téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads)  
- [Programme Windows Insider](https://insider.windows.com/)  
- [Présentation des outils Xbox One](introduction-to-xbox-tools.md) 
- [UWP sur Xbox One](index.md)