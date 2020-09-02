---
title: Prise en main du développement d’applications UWP sur Xbox One
description: Découvrez comment configurer votre PC de développement et votre Xbox One console pour commencer avec le développement d’applications plateforme Windows universelle (UWP) sur Xbox One.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 243f8972f1ea5879ee77f036d97c05e29d446b12
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304701"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Prise en main du développement d’applications UWP sur Xbox One

Pour configurer correctement votre PC et votre console Xbox One pour le développement sur la plateforme Windows universelle (UWP), suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## <a name="before-you-start"></a>Avant de commencer

Avant de commencer, vous devez effectuer les opérations suivantes :
-   Configurez un PC avec la dernière version de Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Vérifier que vous disposez d’au moins 5 Go d’espace libre sur votre console Xbox One.

## <a name="setting-up-your-development-pc"></a>Configuration de votre PC de développement

1.  Installez Visual Studio 2015 Update 3, Visual Studio 2017 ou Visual Studio 2019.

    Si vous installez Visual Studio 2015 Update 3, veillez à choisir l’installation **personnalisée** , puis activez la case à cocher **outils de développement d’applications Windows universelles** . elle ne fait pas partie de l’installation par défaut. Si vous êtes développeur en C++, assurez-vous de choisir **Installation personnalisée** et de sélectionner **C++**.

    Si vous installez Visual Studio 2017 ou Visual Studio 2019, veillez à choisir la charge de travail **développement plateforme Windows universelle** . Si vous êtes un développeur C++, dans le volet **Résumé** à droite, sous **plateforme Windows universelle développement**, veillez à activer la case à cocher **Outils plateforme Windows universelle C++** . Il ne fait pas partie de l’installation par défaut.

    Pour plus d’informations, consultez [configurer votre environnement de développement UWP sur Xbox](development-environment-setup.md).

2.  Installez le dernier [Kit de développement logiciel (SDK) Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Activez le mode développeur pour votre PC de développement (**paramètres/mise à jour & sécurité/pour les développeurs/utiliser les fonctionnalités de développement/mode développeur**).

Maintenant que votre PC de développement est prêt, vous pouvez regarder cette vidéo ou poursuivre la lecture pour voir comment configurer votre Xbox pour le développement et créer et déployer une application UWP dessus.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configuration de votre console Xbox One

1.  Activez le mode développeur sur votre console Xbox One. Téléchargez l’application, récupérez le code d’activation, puis entrez-le dans la page **gérer les consoles Xbox One** de votre compte développeur d’applications de l’espace partenaires. Pour plus d’informations, consultez [activation de Xbox One en mode développeur](devkit-activation.md). 

2.  Ouvrez l’application d' **activation en mode dev** et sélectionnez **basculer et redémarrer**. Félicitations, vous disposez maintenant d’une console Xbox One en mode développeur !
  
  > [!NOTE]
  > Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
    
  > [!NOTE]
  > Avant de pouvoir déployer une application vers votre Xbox One en mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode développeur. 

## <a name="creating-your-first-project-in-visual-studio"></a>Création de votre premier projet dans Visual Studio

Pour plus d’informations, consultez [configurer votre environnement de développement UWP sur Xbox](development-environment-setup.md).

1.  **Pour C#**: créez un projet Windows universel et, dans la **Explorateur de solutions**, cliquez avec le bouton droit sur le projet et sélectionnez **Propriétés**. Sélectionnez l’onglet **Déboguer** , remplacez **appareil cible** par **ordinateur distant**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le champ **ordinateur distant** , puis sélectionnez **universel (protocole non chiffré)** dans la liste déroulante **mode d’authentification** .   

    Pour trouver l’adresse IP de votre console Xbox One, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.  **Pour les projets C++ et HTML/JavaScript**: vous suivez un chemin d’accès similaire aux projets C#, mais dans les propriétés du projet, accédez à l’onglet **débogage** , sélectionnez **ordinateur distant** dans le débogueur pour ouvrir la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le champ nom de l' **ordinateur** , puis sélectionnez **universel (protocole non chiffré)** **dans le**

3. Sélectionnez **x64** dans la liste déroulante à gauche du bouton de lecture vert dans la barre de menus supérieure.
   
4.  Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
5.  La première fois que vous effectuez cette opération, Visual Studio vous demande d’entrer un code confidentiel pour votre console Xbox One. Vous pouvez obtenir un code confidentiel en démarrant dev orig sur votre Xbox et en sélectionnant le bouton **afficher le code confidentiel de Visual Studio** .
  
6.  Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes ; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

7. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  

## <a name="thats-it"></a>C’est tout !

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Voir aussi  
- [Forum aux questions](frequently-asked-questions.md)  
- [Problèmes connus avec UWP dans le programme pour les développeurs Xbox](known-issues.md)
- [UWP sur Xbox One](index.md) 
