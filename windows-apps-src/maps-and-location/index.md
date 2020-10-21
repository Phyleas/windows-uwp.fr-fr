---
title: Vue d’ensemble des cartes et des localisations
description: Cette section explique comment afficher des cartes, utiliser les services de carte, rechercher un lieu et configurer une limite géographique dans votre application. Cette section vous montre aussi comment lancer l’application Cartes Windows pour une carte, un itinéraire ou un ensemble de directions détaillées spécifique.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, carte, emplacement, services de carte
ms.localizationpriority: medium
ms.openlocfilehash: 61b36aa8299d98544c44039abb138f4422e0a164
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297662"
---
# <a name="maps-and-location-overview"></a>Vue d’ensemble des cartes et des localisations

Cette section explique comment afficher des cartes, utiliser les services de carte, rechercher un lieu et configurer une limite géographique dans votre application. Cette section vous montre aussi comment lancer l’application Cartes Windows pour une carte, un itinéraire ou un ensemble de directions détaillées spécifique.

[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services de carte nécessitent une clé d’authentification de cartes appelée [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Pour plus d’informations sur l’obtention et la définition d’une clé d’authentification de cartes, voir [Demander une clé d’authentification de cartes](authentication-key.md).

> [!TIP]
> Pour en savoir plus sur l’utilisation des cartes et de l’emplacement dans votre application, téléchargez les exemples suivants à partir du [dépôt Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) sur GitHub :
-   [Exemple de carte pour UWP (plateforme Windows universelle)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [Exemple de géolocalisation UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>Afficher des cartes


Affichez des cartes avec des vues 2D, 3D ou Streetside dans votre application en utilisant les API de l’espace de noms [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps). Vous pouvez marquer des points d’intérêt sur la carte avec des punaises, des images, des formes ou des éléments d’interface utilisateur XAML. Vous pouvez aussi superposer des images vignette ou remplacer entièrement les images de la carte.

| Rubrique | Description |
|-------|-------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services de carte de l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Afficher des cartes avec des vues 2D, 3D et Streetside](display-maps.md) | Affichez des cartes personnalisables dans votre application en utilisant la classe [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). Cette rubrique présente également des vues 3D aériennes et Streetside. |
| [Afficher des POI (centres d’intérêt) sur une carte](display-poi.md) | Ajoutez des points d’intérêt à une carte avec des punaises, des images, des formes et des éléments d’interface utilisateur XAML. |
| [Superposer des images sous forme de vignettes sur une carte](overlay-tiled-images.md) | Superposez des images vignette tierces ou personnalisées sur une carte à l’aide de sources vignette. Utilisez des sources vignette pour superposer des informations spécifiques (informations météorologiques, démographiques, sismiques...) ou pour remplacer entièrement la carte par défaut. |



## <a name="access-map-services"></a>Accéder aux services de carte

Ajoutez des itinéraires, des directions et des fonctionnalités de géocodage à votre application en utilisant les API de l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps).

| Rubrique | Description |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services de carte de l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Afficher des POI (centres d’intérêt) sur une carte](display-poi.md) | Ajoutez des points d’intérêt à une carte avec des punaises, des images, des formes et des éléments d’interface utilisateur XAML. |
| [Afficher des itinéraires et indications](routes-and-directions.md) | Demandez des itinéraires et des directions, puis affichez-les dans votre application. |
| [Effectuer un géocodage et un géocodage inverse](geocoding.md) | Convertissez des adresses en lieux géographiques (géocodage) et des lieux géographiques en adresses (géocodage inverse) en appelant les méthodes de la classe [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) de l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps). |
| [Rechercher et télécharger des packages de cartes pour une utilisation hors connexion](/uwp/api/windows.services.maps.offlinemaps)| Avant, votre application devait diriger les utilisateurs vers Paramètres pour télécharger des cartes hors connexion. Vous pouvez désormais utiliser les classes de l’espace de noms [Windows.Services.Maps.OfflineMaps](/uwp/api/windows.services.maps.offlinemaps) pour rechercher les packages téléchargés dans une zone donnée (en fonction de [Geopoint](/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](/uwp/api/windows.devices.geolocation.geoboundingbox), etc.). <br> Vous pouvez également vérifier et écouter l’état de téléchargement des packages de cartes, et démarrer un téléchargement sans demander à l’utilisateur de quitter votre application. <br> Vous trouverez des exemples montrant comment procéder dans le contenu de référence et dans l’[exemple de carte UWP (plateforme Windows universelle)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

## <a name="get-the-users-location"></a>Obtenir la localisation de l’utilisateur

Utilisez les API de l’espace de noms [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) pour obtenir la localisation actuelle de l’utilisateur et être notifié lorsqu’elle change dans votre application. Ces API sont aussi fréquemment utilisées dans les paramètres des API de cartes. Les API de l’espace de noms [**Windows.Devices.Geolocation.Geofencing**](/uwp/api/Windows.Devices.Geolocation.Geofencing) notifient votre application quand l’utilisateur entre ou sort d’une limite géographique (une zone géographique prédéfinie).

| Rubrique | Description |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Demander une clé d’authentification de cartes](authentication-key.md) | Pour pouvoir utiliser le [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) et les services de carte de l’espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps), votre application doit être authentifiée. Pour authentifier votre application, vous devez spécifier une clé d’authentification de cartes. Cet article décrit comment demander une clé d’authentification de cartes au [Centre de développement Bing Cartes](https://www.bingmapsportal.com/) et comment l’ajouter à votre application. |
| [Recommandations de conception pour les applications prenant en charge la géolocalisation](guidelines-and-checklist-for-detecting-location.md) | Recommandations de performance pour les applications qui demandent d’accéder à la localisation d’un utilisateur. |
| [Obtenir la localisation de l’utilisateur](get-location.md) | Obtenez l’accès à la localisation de l’utilisateur, puis récupérez-la. | 
| [Instructions pour l’utilisation du suivi des visites](guidelines-for-visits.md) | Découvrez comment utiliser la puissante fonctionnalité de suivi des visites pour rendre plus pratique le suivi de l’emplacement. |
| [Recommandations en matière de conception pour le géorepérage](guidelines-for-geofencing.md) | Recommandations de performances pour les applications qui utilisent la fonctionnalité de geofencing. |
| [Configurer une limite géographique](set-up-a-geofence.md) | Configurez une limite géographique dans votre application et découvrez comment gérer les notifications au premier plan et en arrière-plan. |

## <a name="launch-the-windows-maps-app"></a>Lancer l’application Cartes Windows

Votre application peut lancer l’application Cartes Windows, comme illustré ici, pour afficher des cartes et des directions détaillées spécifiques. Au lieu de fournir des fonctionnalités de carte directement dans votre application, envisagez d’utiliser l’application Cartes Windows. Pour plus d’informations, consultez [Lancer l’application Cartes Windows](../launch-resume/launch-maps-app.md).

![Exemple de l’application Cartes Windows.](images/mapnyc.png)

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Exemple de géolocalisation UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Obtenir l’emplacement actuel](get-location.md)
* [Recommandations de conception pour les applications prenant en charge la géolocalisation](guidelines-and-checklist-for-detecting-location.md)
* [Recommandations de conception pour les cartes](./display-maps.md)
* [Recommandations de conception pour les applications prenant en charge la confidentialité](../security/index.md)
* [Vidéos de la build 2015 : utilisation de cartes et de localisation sur un téléphone, une tablette et un PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
