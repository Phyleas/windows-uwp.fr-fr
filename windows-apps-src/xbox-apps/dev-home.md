---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Page d’hébergement pour développeurs sur la console (dev orig)
description: Découvrez comment l’expérience des outils de développement sur le Xbox One console Development Kit contribue à la productivité des développeurs.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 40100adb1bd9337d933b8ebd155847bde71e341a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172813"
---
# <a name="developer-home-on-the-console-dev-home"></a>Page d’hébergement pour développeurs sur la console (dev orig)
   
  
Dev orig est une expérience d’outils sur Xbox One Development Kit conçue pour aider les développeurs à la productivité. Dev orig offre des fonctionnalités permettant de gérer et de configurer votre kit de développement, de gérer les utilisateurs, de lancer des titres installés et d’effectuer des captures et des traces. Dans les prochaines versions, nous continuerons à développer la fonctionnalité pour activer des fonctionnalités supplémentaires en fonction de vos commentaires, et également à activer l’extensibilité et l’ajout de vos propres outils.   
   
  
Nous sommes très intéressés par vos commentaires sur la page de développement et les scénarios qui vous intéressent le plus. Veuillez fournir vos commentaires à l’aide des méthodes décrites sous **Envoyer des commentaires** dans le menu principal de l’application ou par le biais de votre responsable de compte de développeur (Dam).   
   
  
Pour lancer la page d’hébergement dev à partir de la récupération de novembre 2015 ou ultérieure :  
 
   1. Ouvrez le repère en vous déplaçant à gauche ou en double-cliquant sur le bouton de la flèche.  
   1. Descendre à **paramètres** (icône d’engrenage)   
   1. Sélectionner **tous les paramètres**  
   1. Dans la page **développeur** par défaut, sélectionnez page d' **hébergement des développeurs** (l’icône de démarrage)   

 ![](images/dev_home_icons.png)   
  
Sur les récupérations antérieures, sélectionnez la vignette dev Accueil sur le côté droit de l’écran d’accueil dans **contenu proposé** ou affichez la liste des applications dans Xbox One Manager et lancez **dev Accueil**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface utilisateur  
   
  
L’en-tête de l’interface utilisateur de l’hébergement dev contient les informations importantes suivantes sur la console de développement :   
 
   *  **Adresse IP de la console :** Adresse IP actuelle de la console.   
   *  **Nom de la console :** Nom d’hôte actuel de la console.  
   *  **Bac à sable :** Nom du bac à sable (sandbox) dans lequel se trouve la console.  
   *  **Version du système d’exploitation :** Version de récupération actuelle qui s’exécute sur la console.
   *  Heure système actuelle.   

   
  
Le reste de l’interface utilisateur de la page d’hébergement dev est divisé en pages suivantes. Pour plus d’informations sur les outils de ces pages, consultez leurs rubriques individuelles.   
 
   *  [Page d'accueil](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Paramètres](devhome-settings.md)  
   *  [Capture de média](devhome-capture.md)  
   *  [Mise en réseau](devhome-networking.md)  
   *  [Niveau de performance](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
En appuyant sur le bouton de **menu** de votre contrôleur, vous pouvez accéder au menu principal qui permet de configurer l’espace de travail d’application, la possibilité de gérer les informations d’identification pour accéder aux emplacements réseau et les informations sur l’utilisation de commentaires sur l’application.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>EXPÉRIENCE utilisateur du mode d’alignement  
   
  
Plusieurs outils existants et à venir dans dev orig, tels que la mise en réseau et la version multijoueur, sont conçus pour être utilisés sur le côté pendant que vous exécutez votre titre, afin que vous puissiez accéder facilement aux outils pendant que vous effectuez des tests.   
   
  
Pour accéder au mode alignement, mettez en surbrillance le titre de l’outil approprié, appuyez sur le bouton **Afficher** sur votre contrôleur, puis sélectionnez **Aligner** dans le menu contextuel :  
 ![](images/dev_home_4.png)   
  
L’outil Accueil du développeur s’ancrera à droite de l’écran. Vous pouvez changer de contexte en appuyant comme d’habitude deux fois sur le bouton Nexus.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personnalisation de l’outil Accueil du développeur  
   
  
L’outil Accueil du développeur a été conçu pour être modulable et personnalisable. Vous pouvez configurer l’application pour l’adapter à votre flux de travail, puis l’enregistrer en tant qu’espace de travail. Cet espace de travail peut être exporté et importé, ce qui vous permet de copier la disposition vers d’autres consoles en fonction des besoins. Ces options se trouvent dans le menu principal sous **espace de travail**. Le fichier exporté se trouve sur le lecteur de travail du système dans le `Dev Home\Workspaces` répertoire.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Outils de redimensionnement et de réorganisation  
   
  
Pour modifier la taille ou la position d’un outil, utilisez le bouton de menu contextuel (bouton afficher sur votre contrôleur), tandis que le titre a le focus. Dans le menu contextuel, sélectionnez **déplacer** ou **Redimensionner**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Modification de la couleur de thème et de l’image d’arrière-plan  
   
  
Dans le menu principal, vous pouvez sélectionner **espace de travail** , puis **modifier la couleur du thème**. Sélectionnez une nouvelle couleur et sélectionnez **Enregistrer** pour mettre à jour la couleur de thème utilisée pour la mise en surbrillance du focus.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Définition de l’application par défaut pour un package  
   
  
Si un package contient plusieurs applications, la page d’hébergement dev vous permet de définir l’application par défaut à lancer. Mettez en surbrillance le package dans le lanceur et appuyez sur le bouton **A** pour ouvrir la liste des applications disponibles. Mettez en surbrillance celui que vous souhaitez définir comme valeur par défaut et appuyez sur le bouton **Afficher** , puis choisissez **définir comme valeur par défaut** dans le menu contextuel.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Utilisation de dev orig pour inscrire et lancer des titres à partir d’un partage réseau  
   
  
À partir du lanceur, en bas de la liste applications et jeux installés, vous pouvez sélectionner l’option **inscrire un jeu sur un partage réseau** pour exécuter une version libre de fichier d’un titre à distance.   
 ![](images/dev_home_8.png)   
  
Vous pouvez ensuite entrer le chemin d’accès réseau vers le fichier appxmanifest.xml pour le titre que vous souhaitez inscrire. La page d’hébergement dev tente d’enregistrer le titre à l’aide des informations d’identification existantes pour ce partage et, si nécessaire, vous invite à entrer de nouvelles informations d’identification réseau. Si vous avez besoin d’accéder à des partages supplémentaires (par exemple, pour accéder à des ressources liées symboliquement sur un serveur distinct), vous devez les ajouter à l’aide de l’option ci-dessous.   
   
  
Vous pouvez gérer ces informations d’identification stockées (et en ajouter d’autres) sur la console via l’option **gérer les informations d’identification réseau** du menu principal.   
 ![](images/dev_home_9.png)   
  
Vous pouvez afficher les informations d’identification actuellement sur la console, modifier les informations d’identification en sélectionnant le chemin d’accès et en cliquant sur **un** bouton et en supprimant les informations d’identification en sélectionnant le lien supprimer et en cliquant sur **un** bouton.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Contenu de cette section  
  
[Page d’hébergement (dev orig)](devhome-home.md)  


&nbsp;&nbsp;Fournit un accès rapide aux tâches exécutées régulièrement sur une console de développement. 
  
  
[Page Xbox Live (page d’hébergement dev)](devhome-live.md)  


&nbsp;&nbsp;Capture les informations multijoueur et affiche l’état actuel du service Xbox Live. 
  
  
[Page Paramètres (page d’hébergement dev)](devhome-settings.md)  


&nbsp;&nbsp;Fournit l’accès à différents paramètres pour la console de développement. 
  
  
[Page capture de média (début de la dev)](devhome-capture.md)  


&nbsp;&nbsp;La page **capture multimédia** de la page de capture d’écran de démarrage capture la vidéo du titre en cours d’exécution sur la console. 
  
  
[Page mise en réseau (page d’hébergement dev)](devhome-networking.md)  


&nbsp;&nbsp;Simule différentes conditions de mise en réseau à des fins de dépannage. 
  
  
[Page performances (dev orig)](devhome-performance.md)  


&nbsp;&nbsp;Simule diverses conditions d’utilisation du processeur et de l’activité de disque à des fins de dépannage. 
 