---
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Découvrez les problèmes connus de la version actuelle du kit de développement logiciel (SDK) Microsoft Advertising.
title: Problèmes connus et dépannage pour les publicités dans les applications
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, problèmes connus, dépannage
ms.localizationpriority: medium
ms.openlocfilehash: c69c61cc1db0796edbaedb2f8e2970e1100c5774
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158533"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>Problèmes connus et dépannage pour les publicités dans les applications

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette rubrique répertorie les problèmes connus avec la version actuelle du kit de développement logiciel (SDK) Microsoft Advertising. Pour obtenir des conseils de dépannage supplémentaires, consultez les rubriques suivantes.

* [Guide de résolution des problèmes pour HTML et JavaScript](html-and-javascript-troubleshooting-guide.md)
* [Guide de résolution des problèmes pour XAML et C#](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>Interface AdControl inconnue en XAML

Le balisage XAML d’un contrôle [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) peut afficher incorrectement une ligne courbe bleue impliquant que l’interface est inconnue. Ce problème se produit uniquement lors d’un ciblage x86, et peut être ignoré.

## <a name="lasterror-from-previous-ad-request"></a>Élément lastError de la demande de publicité précédente

S’il reste un élément **lastError** de la demande de publicité précédente, l’événement peut être déclenché deux fois durant le prochain appel de publicité. Si la nouvelle demande de publicité est toujours effectuée et peut générer une publicité valide, ce comportement peut cependant prêter à confusion.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>Spots publicitaires et boutons de navigation sur les téléphones

Sur les téléphones (ou les émulateurs) qui ont des boutons de **retour**, de **démarrage**et de **recherche** au lieu de boutons matériels, le minuteur de compte à rebours et les boutons de clic pour les publicités interstitielles peuvent être masqués.

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>Les publicités récemment créées ne sont pas fournies à votre application

Si vous avez créé une publicité récemment (moins d’un jour), elle peut ne pas être disponible immédiatement. Si le contenu éditorial de la publicité a été approuvé, cette publicité est fournie à l’application une fois que le serveur de publicités l’a traitée. Elle est alors disponible en stock.

## <a name="no-ads-are-shown-in-your-app"></a>Aucune publicité n’est affichée dans votre application

Plusieurs raisons peuvent provoquer le non-affichage des publicités, notamment des erreurs réseau. Autres raisons possibles :

* Sélection d’une unité ad dans l’espace partenaires dont la taille est supérieure ou inférieure à la taille du **classe AdControl** dans le code de votre application.

* Les publicités ne s’affichent pas si vous utilisez une [valeur du mode test](set-up-ad-units-in-your-app.md#test-ad-units) pour votre ID d’unité publicitaire lors de l’exécution d’une application dynamique.

* Si vous avez créé un ID d’unité publicitaire dans la dernière demi-heure, la publicité risque de ne pas s’afficher tant que les serveurs n’ont pas propagé les nouvelles données dans le système. Les ID existants qui affichaient des publicités précédemment doivent en afficher immédiatement.

Si vous pouvez voir des publicités de test dans l’application, c’est que votre code fonctionne et qu’il peut afficher des publicités. Si vous rencontrez des problèmes, contactez le [support produit](https://developer.microsoft.com/windows/support). Sur cette page, choisissez **nous contacter**.

Vous pouvez également publier une question sur le [forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>Les publicités de test s’affichent dans votre application à la place des publicités dynamiques

Les publicités de test peuvent s’afficher même lorsque vous attendez des publicités dynamiques. Cela peut se produire dans les scénarios suivants :

* Notre plateforme de publicité ne peut pas vérifier ou trouver l’ID d’application Live utilisé dans le magasin. Dans ce cas, lorsqu’une unité publicitaire est créée par un utilisateur, son état peut démarrer à dynamique (non-test), mais passer à l’état de test dans les 6 heures qui suivent la première demande de publicité. Il revient à l’état dynamique en cas d’absence de demandes d’applications de test pendant 10 jours.

* Les applications chargées indépendamment ou les applications qui sont exécutées dans l’émulateur n’affichent pas de publicités dynamiques.

Quand une unité ad active traite des annonces de test, l’état de l’unité ad affiche **actif et** traite les publicités de test dans l’espace partenaires. Pour le moment, cela ne s’applique pas aux applications téléphoniques.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet

Lorsque vous utilisez le kit de développement logiciel (SDK) Microsoft Advertising, vous ne pouvez pas cibler de **processeur** dans votre projet. Si votre projet cible la plateforme **Toute UC**, un message d’avertissement peut s’afficher après que vous avez ajouté une référence semblable à ce qui suit.

![ReferenceError \- SolutionExplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Pour supprimer cet avertissement, mettez à jour votre projet pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Utilisez le **Gestionnaire de configurations** pour définir les cibles de plateforme pour déboguer et publier les configurations.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Lorsque vous créez vos packages d’application pour les soumettre au Windows Store (comme illustré dans les images suivantes), veillez à inclure les architectures que vous souhaitez cibler. Vous pouvez choisir d’ignorer x64 si vous prévoyez d’exécuter des builds x86 sur le système d’exploitation x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>Ordre de plan dans les applications JavaScript/HTML

Les applications HTML/JavaScript ne doivent pas placer d’éléments dans la plage MAX-10 réservée de l’ordre de plan. La seule exception est une superposition d’interruptions, par exemple une notification d’appel entrant pour une application Skype.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>Ne pas utiliser de bordures

La définition des propriétés associées aux bordures, héritées par la classe **AdControl** de sa classe parente entraîne le placement erroné de la publicité.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur les derniers problèmes connus et pour poser des questions relatives au kit de développement logiciel (SDK) Microsoft Advertising, visitez le [Forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).

 

 