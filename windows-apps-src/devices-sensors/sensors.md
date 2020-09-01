---
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: Capteurs
description: Les capteurs permettent à votre application de connaître la relation entre un appareil et le monde physique qui l’entoure. Ils peuvent indiquer à votre application la direction, l’orientation et le mouvement de l’appareil.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8c76893dd56c2c3d207444b5c6331c0cd4445ed1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159653"
---
# <a name="sensors"></a>Capteurs



Les capteurs permettent à votre application de connaître la relation entre un appareil et le monde physique qui l’entoure. Ils peuvent indiquer à votre application la direction, l’orientation et le mouvement de l’appareil. Ces capteurs peuvent rendre votre jeu, votre utilitaire ou votre application de réalité augmentée plus utile et interactive en fournissant une forme unique d’entrée, par exemple l’utilisation du mouvement de l’appareil pour organiser les personnages à l’écran ou pour simuler la présence dans un cockpit avec l’appareil pour volant.

En règle générale, vous devez décider dès le début si votre application dépendra exclusivement des capteurs ou si ceux-ci offriront simplement un mécanisme supplémentaire de contrôle. Par exemple, un jeu de course automobile utilisant l’appareil comme volant virtuel pourrait également être contrôlé par l’intermédiaire d’une interface graphique utilisateur à l’écran. De cette façon, l’application fonctionne quels que soient les capteurs disponibles sur le système. En revanche, un jeu de labyrinthe à bille avec inclinaison pourrait être codé pour ne fonctionner que sur les systèmes équipés des capteurs adéquats. Vous devez faire le choix stratégique quant à la dépendance totale envers des capteurs. Notez qu’un modèle de contrôle avec la souris ou les fonctionnalités tactiles procure davantage de contrôle, en contrepartie d’une immersion moindre.

| Rubrique                                                       | Description  |
|-------------------------------------------------------------|--------------|
| [Étalonner les capteurs](calibrate-sensors.md)                   | Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux. L’énumération [<strong>MagnetometerAccuracy</strong>](/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) peut vous aider à déterminer la marche à suivre lorsque votre appareil est en cours d’étalonnage. |
| [Orientation de capteur](sensor-orientation.md)                 | Les données du capteur provenant des classes [<strong>OrientationSensor</strong>](/uwp/api/Windows.Devices.Sensors.OrientationSensor) sont définies par leurs axes de référence. Ces axes sont définis par l’orientation paysage de l’appareil et pivotent avec celui-ci à mesure que l’utilisateur le fait tourner. |
| [Utiliser l’accéléromètre](use-the-accelerometer.md)           | Découvrez comment utiliser l’accéléromètre pour répondre aux mouvements de l’utilisateur. |
| [Utiliser le compas](use-the-compass.md)                       | Découvrez comment utiliser la boussole pour déterminer l’orientation actuelle. |
| [Utiliser le gyromètre](use-the-gyrometer.md)                   | Découvrez comment utiliser le gyromètre pour détecter les changements de mouvements de l’utilisateur. | 
| [Utiliser l’inclinomètre](use-the-inclinometer.md)             | Découvrez comment utiliser l’inclinomètre pour déterminer le tangage, le roulis et le lacet. |
| [Utiliser le capteur de luminosité](use-the-light-sensor.md)             | Découvrez comment utiliser le capteur de luminosité ambiante pour détecter les changements de luminosité. |
| [Utiliser le capteur d’orientation](use-the-orientation-sensor.md) | Découvrez comment utiliser les capteurs d’orientation pour déterminer l’orientation de l’appareil.|

## <a name="sensor-batching"></a>Traitement par lot du capteur

Certains capteurs prennent en charge le concept de traitement par lot. Cela varie selon le capteur individuel disponible. Lorsqu’un capteur implémente le traitement par lot, il recueille plusieurs points de données sur un intervalle de temps spécifié et transfère toutes ces données en même temps. Cela diffère du comportement normal, où un capteur communique ses résultats dès qu’il effectue une lecture. Le diagramme suivant montre comment les données sont collectées et ensuite remises, tout d’abord avec la remise normale, puis avec la remise par lot.

![Collecte par lot du capteur](images/batchsample.png)

Le principal avantage du traitement par lot du capteur est de prolonger l’autonomie de la batterie. L’envoi différé des données permet de réduire la consommation d’énergie du processeur et d’éliminer la nécessité d’un traitement immédiat des données. Les composants du système peuvent rester en veille jusqu’à ce qu’ils soient requis, ce qui génère d’importantes économies d’énergie.

Vous pouvez modifier la fréquence à laquelle le capteur envoie les lots en ajustant la latence. Par exemple, le capteur de l' [**accéléromètre**](/uwp/api/Windows.Devices.Sensors.Accelerometer) a la propriété [**ReportLatency**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency) . Lorsque cette propriété est définie pour une application, le capteur envoie les données après le laps de temps spécifié. Vous pouvez contrôler la quantité de données accumulée sur une période de latence donnée en définissant la propriété [**ReportInterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval).

La définition de la latence s’accompagne néanmoins de quelques inconvénients. Le premier est que chaque capteur présente une valeur [**MaxBatchSize**](/uwp/api/windows.devices.sensors.accelerometer.maxbatchsize) qui varie selon ses caractéristiques. Cette valeur correspond au nombre d’événements que le capteur peut mettre en cache avant d’être obligé de les envoyer. Si vous multipliez **MaxBatchSize** par [**ReportInterval**](/uwp/api/windows.devices.sensors.accelerometer.reportinterval), vous obtenez la valeur [**ReportLatency**](/uwp/api/windows.devices.sensors.accelerometer.reportlatency) maximale. Si vous spécifiez une valeur supérieure à celle-ci, la latence maximale est utilisée afin d’empêcher la perte de données. En outre, différentes applications peuvent chacune définir une latence spécifique. Pour répondre aux besoins de toutes les applications, la période de latence la plus courte est utilisée. Pour ces raisons, la latence que vous définissez dans votre application peut ne pas correspondre à la latence observée.

Si un capteur utilise la création de rapports par lots, l’appel de [**GetCurrentReading**](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) efface le lot de données actuel et démarre une nouvelle période de latence.

## <a name="accelerometer"></a>Accéléromètre

Le capteur [**Accelerometer**](/uwp/api/Windows.Devices.Sensors.Accelerometer) mesure les valeurs de force G le long des axes X, Y et Z de l’appareil. Il convient parfaitement aux applications simples basées sur des mouvements. Notez que les « valeurs de force G » incluent l’accélération due à la gravité. Si l’appareil a **FaceUp** comme [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) sur une table, l’accéléromètre indique -1 G sur l’axe Z. Ainsi, les accéléromètres ne mesurent pas nécessairement simplement l’accélération par rapport aux coordonnées, le taux de changement de vélocité. Durant l’utilisation d’un accéléromètre, assurez-vous d’effectuer la distinction entre le vecteur gravitationnel de la gravité et le vecteur d’accélération linéaire du mouvement. Notez que le vecteur gravitationnel doit avoir une valeur normalisée de 1 pour un appareil immobile.

Les schémas suivants illustrent ce qui suit :

-   V1 = Vecteur 1 = Force due à la gravité
-   V2 = Vecteur 2 = Axe -Z du châssis de l’appareil (pointe vers la direction opposée à l’arrière de l’écran)
-   Θi = Angle d’inclinaison = angle entre l’axe –Z du châssis de l’appareil et le vecteur de gravité

![Accéléromètre](images/accelerometer1.png)![Mesure d’accéléromètre](images/accelerometer2.png)

Parmi les applications qui peuvent utiliser l’accéléromètre, citons les jeux dans lesquels une bille à l’écran glisse dans la direction dans laquelle vous inclinez l’appareil (vecteur gravitationnel). Ce type de fonctionnalité reflète étroitement celui de l’[**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) et pourrait être obtenu avec ce capteur, à l’aide d’une combinaison de tangage et de roulis. Le recours au vecteur de gravité de l’accéléromètre simplifie quelque peu ce processus en fournissant un vecteur manipulé facilement de manière mathématique pour l’inclinaison de l’appareil. Autre exemple : une application qui émet un bruit de fouet quand l’utilisateur déplace l’appareil d’un coup sec (vecteur d’accélération linéaire).

Pour obtenir un exemple d’implémentation, consultez l' [exemple accéléromètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer).

## <a name="activity-sensor"></a>Capteur d’activité

Le capteur [**Activity**](/uwp/api/Windows.Devices.Sensors.ActivitySensor) détermine l’état actuel de l’appareil connecté au capteur. Ce capteur est fréquemment utilisé dans des applications de santé et forme pour suivre l’activité de course ou de marche d’un utilisateur porteur d’un appareil. Consultez [**ActivityType**](/uwp/api/Windows.Devices.Sensors.ActivityType) pour obtenir la liste des activités possibles pouvant être détectées par cette API de capteur.

Pour obtenir un exemple d’implémentation, consultez l' [exemple de capteur d’activité](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor).

## <a name="altimeter"></a>Altimètre

Le capteur [**Altimeter**](/uwp/api/Windows.Devices.Sensors.Altimeter) retourne une valeur qui indique l’altitude du capteur. Cela vous permet de suivre les changements d’altitude en termes de mètres par rapport au niveau de la mer. Un exemple d’application pouvant utiliser cela est une application qui mesure les changements d’altitude durant une course pour calculer les calories brûlées. Dans ce cas, les données de ce capteur pourraient être combinées avec celles du capteur [**Activity**](/uwp/api/Windows.Devices.Sensors.ActivitySensor) pour fournir des informations de suivi plus précises.

Pour obtenir un exemple d’implémentation, consultez l' [exemple Altimeter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter).

## <a name="barometer"></a>Baromètre

Le capteur [**Barometer**](/uwp/api/Windows.Devices.Sensors.Barometer) permet à une application d’obtenir les lectures barométriques. Une application météo pourrait utiliser ces informations pour indiquer la pression atmosphérique actuelle. Cela permettrait de fournir des informations plus détaillées et de prévoir d’éventuels changements des conditions météo.

Pour obtenir un exemple d’implémentation, consultez l' [exemple baromètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer).

## <a name="compass"></a>Boussole

Le capteur [**Compass**](/uwp/api/Windows.Devices.Sensors.Compass) retourne un titre 2D en ce qui concerne le nord magnétique en fonction du plan horizontal de la terre. Le capteur de boussole ne doit pas être utilisé pour déterminer l’orientation spécifique de l’appareil ou pour représenter quoi que ce soit dans un espace en trois dimensions. Certaines caractéristiques géographiques pouvant provoquer une inclinaison naturelle du cap, certains systèmes prennent en charge à la fois [**HeadingMagneticNorth**](/uwp/api/windows.devices.sensors.compassreading.headingmagneticnorth) et [**HeadingTrueNorth**](/uwp/api/windows.devices.sensors.compassreading.headingtruenorth). Réfléchissez à celle qui convient le mieux à votre application, mais souvenez-vous que les systèmes n’indiquent pas tous la valeur du Nord magnétique. Le gyromètre et le magnétomètre (un appareil mesurant la magnitude de la force magnétique) combinent leurs données pour générer le cap de la boussole, ce qui a comme effet net de stabiliser les données (la force du champ magnétique est très instable en raison des composants électriques du système).

![Lectures de la boussole par rapport au Pôle Nord magnétique](images/compass.png)

Les applications qui doivent afficher une rose des vents ou parcourir une carte utilisent généralement le capteur de boussole.

Pour obtenir un exemple d’implémentation, consultez l' [exemple Compass](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

## <a name="gyrometer"></a>Gyromètre

Le capteur [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) mesure les vélocités angulaires le long des axes X, Y et Z. Ces valeurs sont très utiles dans les applications simples basées sur des mouvements, qui ne se soucient pas de l’orientation de l’appareil mais de sa rotation à différentes vitesses. Les gyromètres peuvent être perturbés en cas de bruit dans les données ou de décalage constant le long d’un ou plusieurs axes. Interrogez l’accéléromètre pour vérifier si l’appareil se déplace, afin de déterminer si le gyromètre est perturbé par un décalage, puis compensez le cas échéant dans votre application.

![Gyromètre avec tangage, roulis et lacet](images/gyrometer.png)

Comme exemple d’application pouvant utiliser le gyromètre, citons un jeu qui fait tourner une roulette de casino en fonction d’un bref mouvement de l’appareil.

Pour obtenir un exemple d’implémentation, consultez l' [exemple gyromètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer).

## <a name="inclinometer"></a>Inclinometer

Le capteur [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) spécifie les valeurs de lacet, de tangage et de roulement d’un appareil, et fonctionne mieux avec les applications qui s’intéressent à la façon dont l’appareil est situé dans l’espace. Les valeurs de tangage et de roulis sont dérivées en prenant le vecteur de gravité de l’accéléromètre et en intégrant les données du gyromètre. Le lacet est établi à partir des données du magnétomètre et du gyromètre (de manière semblable au cap de la boussole). Les inclinomètres offrent des données d’orientation avancées dans un format facilement compréhensible et utilisable. Vous pouvez les utiliser quand vous souhaitez connaître l’orientation de l’appareil mais que vous n’avez pas besoin de manipuler les données des capteurs.

![Inclinomètre avec données de tangage, roulis et lacet](images/inclinometer.png)

Les applications qui changent leur vue pour s’adapter à l’orientation de l’appareil peuvent utiliser le capteur inclinomètre. Une application qui affiche un avion dont la position correspond au tangage, au roulis et au lacet de l’appareil utiliserait également les mesures de l’inclinomètre.

Pour obtenir un exemple d’implémentation, consultez l’exemple inclinometer [https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer) .

## <a name="light-sensor"></a>Capteur de lumière

Le capteur de [**lumière**](/uwp/api/Windows.Devices.Sensors.LightSensor) est en charge de déterminer la lumière ambiante entourant le capteur. Cela permet à une application de déterminer les changements de luminosité à proximité de l’appareil. Par exemple, un utilisateur disposant d’un appareil de type tablette peut passer d’un espace couvert à un espace découvert pendant une journée ensoleillée. Une application intelligente pourrait utiliser cette valeur pour modifier le contraste entre l’arrière-plan et la police affichée. Le contenu resterait ainsi lisible dans les conditions plus lumineuses de l’extérieur.

Pour obtenir un exemple d’implémentation, consultez l' [exemple capteur Light](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor).

## <a name="orientation-sensor"></a>Capteur d’orientation

L’orientation de l’appareil est exprimée par l’intermédiaire du quaternion et d’une matrice de rotation. Le [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) offre un degré élevé de précision pour déterminer la façon dont l’appareil se trouve dans l’espace par rapport au titre absolu. Les données **OrientationSensor** sont dérivées de l’accéléromètre, du gyromètre et du magnétomètre. Comme tels, les capteurs inclinomètre et boussole peuvent être dérivés des valeurs de quaternion. Les quaternions et les matrices de rotation se prêtent bien à la manipulation mathématique avancée et sont souvent utilisés dans la programmation graphique. Les applications ayant recours à une manipulation complexe doivent utiliser de préférence le capteur d’orientation, car de nombreuses transformations sont basées sur les quaternions et les matrices de rotation.

![Données de capteur d’orientation](images/orientation-sensor.png)

Le capteur d’orientation est souvent utilisé dans les applications de réalité augmentée avancées qui peignent une superposition sur votre environnement en fonction de la direction dans laquelle pointe l’arrière de l’appareil.

Pour obtenir un exemple d’implémentation, consultez l' [exemple de capteur d’orientation](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor).

## <a name="pedometer"></a>Podomètre

Le capteur [**Pedometer**](/uwp/api/Windows.Devices.Sensors.Pedometer) effectue le suivi du nombre d’étapes effectuées par l’utilisateur qui effectue l’appareil connecté. Le capteur est configuré pour suivre le nombre de pas pendant une période donnée. Plusieurs applications de forme et santé suivent ainsi le nombre de pas effectués pour aider l’utilisateur à définir et à atteindre divers objectifs. Ces informations peuvent ensuite être collectées et stockées pour afficher la progression au fil du temps.

Pour obtenir un exemple de implémentation, consultez l' [exemple Pedometer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer).

## <a name="proximity-sensor"></a>Capteur de proximité

Le capteur [**Proximity**](/uwp/api/Windows.Devices.Sensors.ProximitySensor) peut indiquer s’il détecte ou non certains objets. En plus de déterminer si un objet est ou non à portée de l’appareil, le capteur de proximité peut également déterminer la distance de l’objet détecté. Un exemple d’utilisation pourrait être une application qui doit sortir du mode veille quand un utilisateur entre dans une plage spécifiée. L’appareil pourrait rester en état de veille avec faible consommation d’énergie jusqu’à ce que le capteur de proximité détecte un objet, avec pour effet d’activer l’appareil.

Pour obtenir un exemple d’implémentation, consultez l' [exemple capteur de proximité](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor).

## <a name="simple-orientation"></a>Orientation simple

Le [**SimpleOrientationSensor**](/uwp/api/windows.devices.sensors.simpleorientationsensor) détecte l’orientation actuelle du quadrant du périphérique spécifié ou bien il est reporté ou est face à. Il possède six états [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) possibles (**NotRotated**, **Rotated90**, **Rotated180**, **Rotated270**, **FaceUp**, **FaceDown**).

Une application de lecture qui change son affichage selon que l’appareil est tenu parallèlement ou perpendiculairement au sol utiliserait les valeurs de SimpleOrientationSensor pour déterminer comment l’appareil est tenu.

Pour obtenir un exemple d’implémentation, consultez l' [exemple de capteur d’orientation simple](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor).