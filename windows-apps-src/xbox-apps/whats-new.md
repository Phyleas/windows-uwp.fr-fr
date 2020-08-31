---
title: Nouveautés de la plateforme UWP sur Xbox One
description: Consultez les nouvelles fonctionnalités, les mises à jour des fonctionnalités existantes et les correctifs de bogues pour les développeurs dans la dernière mise à jour de UWP sur Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 937d13c8764cda2f0a4592f6ca3a78d3b35a9529
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168953"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Nouveautés de la dernière mise à jour de la plateforme UWP sur Xbox One pour les développeurs

La dernière mise à jour de plateforme Windows universelle (UWP) sur Xbox 1 contient les nouvelles fonctionnalités suivantes, les mises à jour des fonctionnalités existantes et les correctifs de bogues.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>les applications et les jeux x86 ne sont plus pris en charge sur Xbox  
Xbox ne prend plus en charge le développement d’applications x86 ou les envois d’application x86 au magasin.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Les applications peuvent désormais prendre en charge la navigation vers l’application précédente 
UWP sur Xbox une application peut désormais prendre en charge la navigation vers l’application précédente. Pour ce faire, abonnez-vous à l’événement [**Windows.UI.Core.SystemNavigationManager. Rerequested**](/uwp/api/Windows.UI.Core.SystemNavigationManager) et affectez à la propriété **Handled** la **valeur false** dans votre gestionnaire d’événements.

> [!NOTE]
> Pour des raisons de compatibilité, cette fonctionnalité est disponible uniquement pour les applications qui sont créées avec la version la plus récente de UWP sur Xbox One. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>La page d’hébergement dev est désormais l’expérience d’utilisation par défaut sur les consoles de développement
Les consoles de développement lancent désormais la page d’hébergement dev comme expérience d’utilisation par défaut. Cela vous permet de travailler sans avoir à cliquer dans l’écran d’accueil de la vente au détail. La page d’accueil dev comprend désormais une action rapide pour lancer l’écran d’accueil de la vente au détail. En outre, un nouveau paramètre vous permet de faire de la vente au détail l’expérience par défaut. 

## <a name="new-dev-home-user-interface"></a>Nouvelle interface utilisateur de développement
L’interface utilisateur de dev Developer comprend désormais les améliorations de productivité suivantes :
 - Les données importantes, telles que l’adresse IP et la version de récupération, s’affichent désormais en haut de l’écran pour la visibilité. 
 - La page d’hébergement dev dispose désormais d’une interface utilisateur avec onglets qui regroupe les outils en jeux logiques, ce qui permet de naviguer rapidement.
 - Les boutons d’action rapide sur le premier onglet de la page d’hébergement dev permettent un accès rapide aux actions les plus couramment utilisées. 

## <a name="wdp-for-xbox-enhancements"></a>Améliorations de WDP pour Xbox
Le portail d’appareils Windows (WDP) comprend désormais une prise en charge supplémentaire des paramètres de la console. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Vous pouvez maintenant basculer le type de votre titre UWP entre « application » et « Game »
Le passage du type de votre titre UWP entre « application » et « Game » vous permet de tester des scénarios de jeux sans les publier dans le Windows Store. Dans Developer orig, sélectionnez l’application dans le volet **jeux & Apps** , appuyez sur le bouton afficher du contrôleur, sélectionnez Détails de l' **application** , puis modifiez le type en « application » ou « jeu ».

## <a name="see-also"></a>Voir aussi
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)
 - Vous pouvez désormais effectuer une capture d’écran de la console. Pour plus d’informations sur la réalisation d’une capture d’écran, voir la rubrique de référence [/ext/screenshot](wdp-media-capture-api.md).
 - L’outil peut déployer une build de fichier isolé de votre application. Pour plus d’informations sur les builds de fichier isolé, consultez la rubrique de référence [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Il est possible d’accéder aux fichiers de développement sur votre console via l’Explorateur de fichiers sur votre ordinateur de développement. Pour plus d’informations sur l’accès aux fichiers via l’Explorateur de fichiers, voir la rubrique de référence [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Voir aussi
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)