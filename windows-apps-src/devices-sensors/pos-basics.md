---
title: Notions de base sur le point de service
description: Cet article contient des informations sur la prise en main des API PointOfService Windows Runtime.
ms.date: 12/3/2019
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: cb8acf5f7dce8e81d72850a4dbd097083b240864
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730043"
---
# <a name="point-of-service-basics"></a>Notions de base sur le point de service

Cette section contient des rubriques qui sont communes à toutes les catégories d’appareils de point de service.

|Rubrique |Description |
|------|------------|
| [Déclaration des fonctionnalités](pos-basics-capability.md)      | Découvrez comment ajouter la fonctionnalité **pointOfService** à votre manifeste d’application.  Cette fonctionnalité est requise pour l’utilisation de l’espace de noms Windows. Devices. PointOfService.  |
| [Énumération des appareils](pos-basics-enumerating.md)        | Découvrez comment définir un sélecteur d’appareil qui est utilisé pour interroger les appareils disponibles pour le système et utiliser ce sélecteur pour énumérer les appareils de point de service.  |
| [Création d’un objet appareil](pos-basics-deviceobject.md)  | Découvrez comment créer un objet d’appareil PointOfService qui vous donne accès aux propriétés en lecture seule du périphérique et réclamer l’utilisation exclusive du périphérique. |
| [Revendication et activer](pos-basics-claim.md)  | Découvrez Comment réserver un périphérique PointOfService pour une utilisation exclusive et l’activer pour les opérations d’e/s.  |
| [Partage de périphériques avec d’autres personnes](pos-basics-sharing.md) | Découvrez comment partager des périphériques réseau ou Bluetooth connectés avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur des périphériques partagés plutôt que sur des périphériques dédiés connectés à chaque ordinateur.
| [PointOfService de bout en bout](pos-get-started.md)  | Il s’agit d’un exemple de bout en bout d’interaction avec les périphériques PointOfService à l’aide des exemples ci-dessus. |
|

## <a name="see-also"></a>Voir aussi

| Rubrique   | Description |
|:--------|:------------|
| [Distribution d’applications](../publish/distribute-lob-apps-to-enterprises.md) | Découvrez les options de distribution de votre application aux clients d’entreprise. |
| [Cycle de vie des applications](../launch-resume/app-lifecycle.md) | Découvrez le cycle de vie d’une application UWP et ce qui se passe lorsque Windows lance, interrompt et reprend votre application. |
| [Ressources d’application](../app-resources/index.md) | Apprenez à créer, à empaqueter et à consommer les ressources de chaîne, d’image et de fichier de votre application. |
| [Liaison de données](../data-binding/index.md) | Découvrez comment utiliser la liaison de données pour afficher des données dans l’interface utilisateur de votre application. |
| [Énumération des appareils](enumerate-devices.md) | Apprenez à utiliser des techniques d’énumération avancées pour rechercher vos périphériques.|
| [Applications adaptatives de version](../debug-test-perf/version-adaptive-apps.md) | Apprenez à concevoir votre application pour qu’elle s’exécute sur plusieurs versions de Windows 10.|
|


## <a name="sample-code"></a>Exemple de code
+ [Exemple de scanneur de codes-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de tiroir-caisse]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
