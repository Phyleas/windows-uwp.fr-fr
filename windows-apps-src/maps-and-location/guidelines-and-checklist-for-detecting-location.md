---
description: Cette rubrique décrit les recommandations en matière de performance des applications qui nécessitent de géolocaliser un utilisateur.
title: Recommandations pour les applications avec la géolocalisation
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, emplacement, carte, géolocalisation
ms.localizationpriority: medium
ms.openlocfilehash: af9ea1a214bb3cb49dd65a77d1fde30e4bb3a064
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297742"
---
# <a name="guidelines-for-location-aware-apps"></a>Recommandations pour les applications avec la géolocalisation

**API importantes**

-   [**Géolocalisation**](/uwp/api/Windows.Devices.Geolocation)
-   [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

Cette rubrique décrit les recommandations en matière de performance des applications qui nécessitent de géolocaliser un utilisateur.

## <a name="recommendations"></a>Recommandations


-   Commencez à utiliser l’objet localisation seulement lorsque l’application requiert des données de localisation.

    Appelez l’élément [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) avant d’accéder à l’emplacement de l’utilisateur. À ce stade, votre application doit être au premier plan et l’élément **RequestAccessAsync** doit être appelé à partir du thread d’interface utilisateur. Jusqu’à ce que l’utilisateur l’y autorise, votre application ne peut pas accéder aux données d’emplacement.

-   Si la géolocalisation n’est pas cruciale pour votre application, n’y accédez que si l’utilisateur essaie d’effectuer une tâche qui la nécessite. Par exemple, si une application de réseau social comporte un bouton « Indiquer ma localisation », l’application ne doit pas accéder à la localisation avant que l’utilisateur ne clique sur le bouton. Il convient d’accéder immédiatement à la localisation si la fonction principale de votre application le nécessite.

-   La première utilisation de l’objet [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) doit être effectuée sur le thread principal d’interface utilisateur de l’application au premier plan, afin de déclencher l’affichage de l’invite de consentement à l’utilisateur. La première utilisation de l’objet **Geolocator** peut correspondre soit au premier appel à [**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync), soit à la première inscription d’un gestionnaire pour l’événement [**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged).

-   Précisez à l’utilisateur la manière dont les données de géolocalisation seront utilisées.
-   Fournissez une interface utilisateur pour permettre aux utilisateurs d’actualiser manuellement leur emplacement.
-   Affichez une barre ou un anneau de progression tout en attendant d’obtenir les données de géolocalisation. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   Affichez des messages d’erreur ou des boîtes de dialogue appropriés lorsque les services de localisation sont désactivés ou non disponibles.

    Si les paramètres d’emplacement n’autorisent pas votre application à accéder à l’emplacement de l’utilisateur, nous vous recommandons de fournir un lien pratique vers les **paramètres de confidentialité d’emplacement** dans l’application **Paramètres**. Par exemple, vous pouvez utiliser un contrôle de lien hypertexte ou appeler la méthode [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) pour lancer l’application **Paramètres** à partir du code à l’aide de l’URI `ms-settings:privacy-location`. Pour plus d’informations, voir [Lancer l’application Paramètres Windows](../launch-resume/launch-settings-app.md).

-   Supprimez les données de géolocalisation mises en cache et libérez l’objet [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) lorsque l’utilisateur désactive l’accès aux informations de géolocalisation.

    Libérez l’objet [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) si l’utilisateur désactive l’accès aux informations de géolocalisation via les Paramètres. L’application reçoit alors les résultats de ** \_ refus d’accès** pour tous les appels d’API d’emplacement. Si votre application enregistre ou met en cache les données de géolocalisation, effacez toutes les données mises à en cache lorsque l’utilisateur révoque l’accès aux informations de géolocalisation. Proposez un autre moyen de saisir manuellement un emplacement lorsque les informations de géolocalisation ne sont pas disponibles via les services de localisation.

-   Prévoyez une interface utilisateur permettant de réactiver les services de localisation. Par exemple, fournissez un bouton actualiser qui réinstancie l’objet [**géolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) et tente à nouveau d’obtenir des informations d’emplacement.

    Votre application doit proposer une interface utilisateur pour la réactivation des services de géolocalisation.

    -   Si l’utilisateur réactive l’accès à la géolocalisation après l’avoir désactivé, aucune notification n’est communiquée à l’application. La propriété [**status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) ne change pas et aucun événement [**statusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) ne se produit. Votre application doit créer un objet [**géolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) et appeler [**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) pour essayer d’obtenir des données d’emplacement mises à jour, ou s’abonner à nouveau aux événements [**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) . Si l’état indique ensuite que la géolocalisation a été réactivée, effacez tout élément d’interface utilisateur précédemment affiché par votre application pour avertir l’utilisateur que les services de localisation étaient désactivés et répondez de manière appropriée au nouvel état.
    -   Votre application doit également tenter à nouveau de se procurer des données de géolocalisation au moment de l’activation, lorsque l’utilisateur tente explicitement d’utiliser la fonctionnalité qui fait appel à des informations de géolocalisation ou à tout autre moment adapté au scénario.

**Performances**

-   Utilisez des demandes de localisation ponctuelles si votre application n’a pas besoin de recevoir des mises à jour de localisation. Par exemple, une application qui ajoute une balise de géolocalisation à une photo n’a pas besoin de recevoir d’événements de mise à jour géographique. Elle doit plutôt demander la géolocalisation via la méthode [**getGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync), comme décrit dans [Obtenir l’emplacement actuel](./get-location.md).

    Quand vous effectuez une demande de géolocalisation ponctuelle, vous devez définir les valeurs suivantes.

    -   Spécifiez la précision demandée par votre application en définissant la propriété [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) ou [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters). Pour obtenir des recommandations concernant l’utilisation de ces paramètres, voir plus bas.
    -   Définissez le paramètre d’âge maximal de [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) pour spécifier depuis combien de temps un emplacement peut avoir été obtenu pour être utile à votre application. Si votre application peut utiliser une position qui a été obtenue quelques secondes ou minutes auparavant, elle peut recevoir une position presque immédiatement et contribuer à prolonger l’autonomie de l’appareil.
    -   Définissez le paramètre timeout de [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync). Il s’agit du délai pendant lequel votre application peut attendre la réception d’une position ou d’une erreur. Vous devez établir un bon compromis entre réactivité et précision, en fonction des besoins de votre application.
-   Utilisez une session de géolocalisation continue quand des mises à jour fréquentes de la position sont nécessaires. Utilisez les événements [**positionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) et [**statusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) pour détecter un déplacement au-delà d’un seuil spécifique ou pour des mises à jour géographiques continues au fur et à mesure qu’elles se produisent.

    Lors d’une demande de mise à jour de géolocalisation, vous pouvez spécifier la précision demandée par votre application en définissant la propriété [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) ou [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters). Vous devez également définir la fréquence à laquelle les mises à jour de l’emplacement sont nécessaires, à l’aide de [**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold) ou de [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval).

    -   Spécifiez le seuil de déplacement. Certaines applications n’ont besoin de mises à jour géographiques que lorsque l’utilisateur effectue un long déplacement. Ainsi, une application qui fournit des actualités locales ou des mises à jour météorologiques peut ne pas avoir besoin de mises à jour géographiques, à moins que l’utilisateur ne change de lieu en se rendant dans une autre ville. Dans ce cas, vous réglez la distance minimale requise avant déclenchement d’un événement de mise à jour géographique en définissant la propriété [**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold). Cette propriété a pour effet de filtrer les événements [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged). Des événements de ce type ne se déclenchent que lorsque le changement de position dépasse le seuil de déplacement.

    -   Utilisez [**reportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) qui s’aligne sur l’expérience de votre application et qui réduit l’utilisation des ressources système. Par exemple, une application météo peut nécessiter une mise à jour des données toutes les 15 minutes seulement. En dehors des applications de navigation en temps réel, la plupart des applications n’exigent pas un flux constant de mises à jour géographiques extrêmement précises. Si votre application n’a pas besoin d’un flux de données ultra précis ou si elle nécessite des mises à jour de temps à autre, définissez la propriété **ReportInterval** pour indiquer la fréquence minimale des mises à jour géographiques dont a besoin votre application. La source de localisation peut ensuite économiser de l’énergie en calculant l’emplacement seulement en cas de besoin.

        S’agissant d’applications qui nécessitent des données en temps réel, il est nécessaire de définir la propriété [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval) avec la valeur 0 pour indiquer qu’aucun intervalle minimal n’est spécifié. L’intervalle de rapport par défaut est le plus court entre 1 seconde ou la fréquence que le matériel permet.

        Les appareils qui fournissent des données de géolocalisation peuvent suivre l’intervalle de rapport demandé par différentes applications et fournir des rapports de données au plus petit intervalle demandé. L’application qui a le plus besoin de précision reçoit ainsi les données dont elle a besoin. Par conséquent, il est possible que le service de géolocalisation génère des mises à jour à une fréquence plus élevée que celle demandée par votre application, si une autre application a demandé des mises à jour plus fréquentes.

        **Remarque**    Il n’est pas garanti que la source d’emplacement honorera la demande pour l’intervalle de rapport donné. Bien que certains services de géolocalisation ne tiennent pas compte de l’intervalle de rapport, vous avez quand même intérêt à le définir pour ceux qui le prennent en compte.

    -   Pour vous aider à économiser de l’énergie, définissez la propriété [**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) pour indiquer à la plateforme d’emplacement si votre application a besoin de données de haute précision. Si aucune application n’a besoin de données de grande précision, le système peut économiser de l’énergie en n’activant pas les services GPS.

        -   Définissez [**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) sur **HIGH** pour activer l’acquisition de données par GPS.
        -   Affectez à [**desiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy) la valeur **Default** et utilisez uniquement un modèle d’appel à application unique pour réduire la consommation d’énergie si votre application utilise l’emplacement uniquement à des fins de ciblage publicitaire.

        Si votre application a des besoins spécifiques en matière de précision, vous pouvez utiliser la propriété [**DesiredAccuracyInMeters**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracyinmeters) au lieu de [**DesiredAccuracy**](/uwp/api/windows.devices.geolocation.geolocator.desiredaccuracy). Ceci est particulièrement utile sur Windows Phone, où la position peut généralement être obtenue grâce aux satellites ou aux points d’accès cellulaires ou Wi-Fi. La sélection d’une valeur de précision plus spécifique aide le système à identifier les technologies adéquates à utiliser avec le moindre coût énergétique lors de la fourniture de la position.

        Par exemple :

        -   Si votre application obtient l’emplacement pour l’optimisation publicitaire, la météo, les actualités, et ainsi de suite, une précision de 5000 mètres suffit généralement.
        -   Si votre application affiche des transactions à proximité dans le voisinage, une précision de 300 mètres est généralement correcte pour fournir des résultats.
        -   Si l’utilisateur souhaite obtenir des recommandations pour choisir un restaurant dans les environs, une précision de 100 mètres suffit.
        -   Si l’utilisateur essaie de partager sa position, l’application doit demander une précision d’environ 10 mètres.
    -   Utilisez la propriété [**coordonnée. Accuracy**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) si votre application a des exigences spécifiques en matière de précision. Par exemple, les applications de navigation doivent utiliser la propriété **Geocoordinate.accuracy** pour déterminer si les données de géolocalisation disponibles répondent à leurs exigences.

-   Prenez en compte le retard lié à la prise en main. Lorsqu’une application demande des données de géolocalisation pour la première fois, le service de localisation peut accuser un léger retard (une à deux secondes) au moment de sa prise en main. Vous devez en tenir compte dans la conception de l’interface utilisateur de votre application. Par exemple, vous souhaiterez peut-être éviter de bloquer d’autres tâches en attente de la fin de l’appel à [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync).

-   Prenez en compte le comportement en arrière-plan. Si votre application n’a pas le focus, elle ne reçoit pas les événements de mise à jour de localisation tant qu’elle est interrompue en arrière-plan. Si votre application assure le suivi des mises à jour de localisation en les enregistrant dans un journal, tenez-en compte. Une fois que l’application récupère le focus, elle reçoit uniquement les nouveaux événements. Elle ne récupère aucune des mises à jour survenues pendant qu’elle était inactive.

-   Utilisez efficacement les capteurs bruts et de fusion. Il y a deux types de capteurs : *bruts* et *fusion*.

    -   Les capteurs bruts comprennent l’accéléromètre, le gyromètre et le magnétomètre.
    -   Les capteurs de fusion comprennent l’orientation, l’inclinomètre et la boussole. Les capteurs de fusion obtiennent les données d’un ensemble de capteurs bruts.

    Les API Windows Runtime peuvent accéder à tous ces capteurs à l’exception du magnétomètre. Les capteurs de fusion sont plus précis et stables que les capteurs bruts, mais ils consomment plus. Utilisez le capteur adapté à vos besoins. Pour plus d’informations, voir [Capteurs](../devices-sensors/sensors.md).

**Veille connectée**
- Lorsque l’ordinateur est en veille connectée, les objets [**géolocalisateurs**](/uwp/api/Windows.Devices.Geolocation.Geolocator) peuvent toujours être instanciés. Cependant, l’objet **Geolocator** ne trouvera aucun capteur à agréger et, par conséquent, les appels à [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) expireront au bout de 7 secondes, les détecteurs d’événements [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) ne seront jamais appelés et les détecteurs d’événements [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) seront appelés une fois avec l’état **NoData**.

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires


### <a name="detecting-changes-in-location-settings"></a>Détection des modifications dans les paramètres de géolocalisation

L’utilisateur peut désactiver la fonctionnalité de géolocalisation en définissant les **paramètres de confidentialité de l’emplacement** dans l’application **Paramètres**.

-   Pour détecter à quel moment l’utilisateur désactive ou réactive les services de localisation :
    -   Gérez l’événement [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged). La propriété [**Status**](/uwp/api/windows.devices.geolocation.statuschangedeventargs.status) de l’argument de l’événement **StatusChanged** a la valeur **Disabled** si l’utilisateur désactive les services de localisation.
    -   Vérifiez les codes d’erreur retournés par [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync). Si l’utilisateur a désactivé les services de localisation, les appels à **GetGeopositionAsync** échouent avec une erreur d' **accès \_ refusé** et la propriété [**LocationStatus**](/uwp/api/windows.devices.geolocation.geolocator.locationstatus) a la valeur **Disabled**.
-   Si vous disposez d’une application pour laquelle des données de géolocalisation sont primordiales, par exemple une application de cartographie, veillez à effectuer les tâches suivantes :
    -   Gérez l’événement [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) pour obtenir des mises à jour si l’emplacement de l’utilisateur change.
    -   Gérez l’événement [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) comme décrit précédemment, pour détecter les modifications apportées aux paramètres d’emplacement.

Notez que le service de localisation renvoie les données à mesure de leur disponibilité. Elle peut d’abord renvoyer une localisation avec un rayon d’erreur important, puis la mettre à jour à mesure que des informations plus précises se feront disponibles. Les applications qui affichent la localisation de l’utilisateur doivent normalement souhaiter la mettre à jour à mesure que des informations plus précises sont disponibles.

### <a name="graphical-representations-of-location"></a>Représentations graphiques de localisation

Faites en sorte que votre application utilise la [**géocoordination. précision**](/uwp/api/windows.devices.geolocation.geocoordinate.accuracy) pour indiquer clairement l’emplacement actuel de l’utilisateur sur la carte. Il existe trois principales bandes de précision : un rayon d’erreur d’environ 10 mètres, un rayon d’erreur d’environ 100 mètres et un rayon d’erreur de plus d’un kilomètre. En utilisant les informations de précision, vous pouvez vous assurer que votre application affiche l’emplacement de façon exacte dans le contexte des données disponibles. Pour plus d’informations générales sur l’utilisation du contrôle de carte, voir [Afficher des cartes avec des vues 2D, 3D et Streetside](./display-maps.md).

-   Pour une précision égale à peu près à 10 mètres (résolution GPS), l’emplacement peut être indiqué par un point ou une épingle sur la carte. Avec cette précision, les coordonnées de latitude et de longitude ainsi que l’adresse peuvent également être affichées.

    ![exemple de carte affichée avec une précision de GPS d’environ 10 mètres.](images/10metererrorradius.png)

-   Pour une précision entre 10 et 500 mètres (environ 100 mètres), la localisation est généralement reçue via une résolution Wi-Fi. La localisation obtenue à partir du signal cellulaire offre une précision d’environ 300 mètres. Dans ce cas, il est conseillé que votre application affiche un rayon d’erreur. Pour les applications qui affichent des directions où un point de centrage est requis, un tel point peut être affiché avec un rayon d’erreur qui l’entoure.

    ![exemple de carte affichée avec une précision Wi-Fi d’environ 100 mètres.](images/100metererrorradius.png)

-   Si la précision retournée est supérieure à 1 kilomètre, vous recevez probablement les informations de géolocalisation avec une résolution de niveau IP. Ce niveau de précision est souvent trop faible pour localiser avec exactitude un endroit spécifique sur une carte. Votre application doit zoomer au niveau ville sur la carte, ou sur la zone appropriée en fonction du rayon d’erreur (par exemple, le niveau région).

    ![exemple de carte affichée avec une précision Wi-Fi d’environ 1 kilomètre.](images/1000metererrorradius.png)

Lorsque la précision de localisation passe d’une bande de précision à une autre, offrez une transition appropriée entre les différentes représentations graphiques. Pour ce faire :

-   Rendez l’animation de transition fluide et maintenez la transition rapide et aisée.
-   Attendez quelques rapports consécutifs pour confirmer le changement de précision, pour mieux empêcher les zooms indésirables et trop fréquents.

### <a name="textual-representations-of-location"></a>Représentations textuelles de localisation

Certains types d’applications, par exemple une application météo ou d’informations locales, ont besoin de moyens de représenter la localisation de façon textuelle sur les différentes bandes de précision. Assurez-vous d’afficher l’emplacement clairement et uniquement jusqu’au niveau de précision fourni dans les données.

-   Pour une précision égale à peu près à 10 mètres (résolution GPS), les données de géolocalisation reçues sont assez précises et peuvent donc être communiquées au niveau du nom du voisinage. Le nom de la ville, le nom du département ou de la province et le nom du pays/de la région peuvent également être utilisés.
-   Pour une précision égale à peu près à 100 mètres (résolution Wi-Fi), les données de géolocalisation reçues sont moyennement précises et nous vous conseillons donc d’afficher les informations jusqu’au nom de la ville. Évitez d’utiliser le nom du voisinage.
-   Pour une précision supérieure à 1 kilomètre (résolution IP), affichez uniquement le département ou la province, ou le nom du pays/de la région.

### <a name="privacy-considerations"></a>Considérations relatives à la confidentialité

La géolocalisation d’un utilisateur correspond à des informations d’identification personnelle. Le site Web suivant fournit des recommandations concernant la protection de la vie privé des utilisateurs.

-   [Confidentialité Microsoft]( https://www.microsoft.com/privacy/dpd/default.aspx)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>Rubriques connexes

* [Configurer une limite géographique](./set-up-a-geofence.md)
* [Obtenir l’emplacement actuel](./get-location.md)
* [Affichage des cartes avec les vues 2D, 3D et Streetside](./display-maps.md)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [Exemple de géolocalisation UWP (géolocalisation)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
