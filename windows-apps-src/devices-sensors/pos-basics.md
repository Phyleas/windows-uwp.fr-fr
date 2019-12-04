---
title: Notions de base sur le point de service
description: Cet article comporte des informations sur la prise en main des API UWP PointOfService.
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 4749b66cc5ce2593aead75d65993f70106da7c8b
ms.sourcegitcommit: 2d709ddcc31f52d2a4ace1134aea45057d99a615
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782580"
---
# <a name="point-of-service-basics"></a>Notions de base sur le point de service

Cette section contient des rubriques qui sont communes à toutes les catégories d’appareils de point de service.

|Sujet |Description |
|------|------------|
| [Déclaration de capacité](pos-basics-capability.md)      | Découvrez comment ajouter la fonctionnalité **pointOfService** à votre manifeste d’application.  Cette fonctionnalité est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.  |
| [Énumération des appareils](pos-basics-enumerating.md)        | Découvrez comment définir un sélecteur d’appareils qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service.  |
| [Création d’un objet appareil](pos-basics-deviceobject.md)  | Découvrez comment créer un objet appareil PointOfService qui vous permet d’accéder aux propriétés en lecture seule du périphérique et de revendiquer le périphérique pour une utilisation exclusive. |
| [Revendication et activer](pos-basics-claim.md)  | Découvrez Comment réserver un périphérique PointOfService pour une utilisation exclusive et l’activer pour les opérations d’e/s.  |
| [Partager des périphériques avec d’autres utilisateurs](pos-basics-sharing.md) | Découvrez comment partager des périphériques réseau ou Bluetooth connectés avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur des périphériques partagés plutôt que sur des périphériques dédiés connectés à chaque ordinateur.
| [PointOfService de bout en bout](pos-get-started.md)  | Il s’agit d’un exemple de bout en bout d’interaction avec les périphériques PointOfService à l’aide des exemples ci-dessus. |
|

## <a name="see-also"></a>Articles associés

| Sujet   | Description |
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
