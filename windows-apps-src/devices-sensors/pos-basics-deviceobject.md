---
title: Objets d’appareil PointOfService
description: Découvrez comment créer un objet d’appareil PointOfService et en savoir plus sur le cycle de vie des objets d’appareil dans le modèle d’application plateforme Windows universelle (UWP).
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, point de service, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4c9a5008756831eed9819a3b323d167dcc4b2744
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043401"
---
# <a name="pointofservice-device-objects"></a>Objets d’appareil PointOfService

## <a name="creating-a-device-object"></a>Création d’un objet appareil

Une fois que vous avez identifié l’appareil PointOfService que vous souhaitez utiliser, à partir d’une nouvelle énumération ou d’un DeviceID stocké, vous appelez simplement [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) avec l'[**ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) de périphérique que vous avez choisi par programmation ou l’utilisateur a choisi de créer un nouvel objet d’appareil de point de service.

Cet exemple tente de créer un nouvel objet BarcodeScanner avec FromIdAsync à l’aide d’un DeviceID. En cas d’échec lors de la création de l’objet, un message de débogage est écrit.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

Une fois que vous avez un objet d’appareil, vous pouvez accéder aux méthodes, propriétés et événements de l’appareil.  

## <a name="device-object-lifecycle"></a>Cycle de vie des objets périphériques

Avant Windows 8, les applications avaient un cycle de vie simple. Les applications Win32 et .NET sont en cours d’exécution ou ne sont pas en cours d’exécution, et les périphériques PointOfService étaient généralement revendiqués pour le cycle de vie complet des applications. Lorsqu’un utilisateur les réduit ou les ferme, elles continuent de s’exécuter. Cela ne posait aucun problème jusqu’à ce que les appareils mobiles et la gestion de l’alimentation prennent une importance croissante.

Windows 8 a introduit un nouveau modèle d’application avec des applications UWP. Globalement, un état suspendu a été ajouté. Une application UWP est suspendue peu après que l’utilisateur l’a réduite ou bascule vers une autre application. Cela signifie que les threads de l’application sont arrêtés, que l’application est conservée en mémoire, sauf si le système d’exploitation doit libérer des ressources, et tous les objets d’appareil représentant des périphériques PointOfService sont fermés automatiquement pour permettre à d’autres applications d’accéder aux périphériques. Lorsque l’utilisateur revient à l’application, il peut être restauré rapidement à un État en cours d’exécution et restaurer des connexions de périphériques PointOfService, à condition qu’elles soient toujours disponibles à la reprise.

Vous pouvez détecter quand un objet est fermé pour une raison quelconque avec un \<DeviceObject\> . Le gestionnaire d’événements fermés note l’ID de l’appareil pour réétablir la connexion à l’avenir.   Vous pouvez également gérer cela sur une notification d’interruption de l’application pour enregistrer les ID de l’appareil afin de rétablir les connexions de l’appareil lors de la notification de reprise de l’application.  Assurez-vous de ne pas doubler sur les gestionnaires d’événements et d’effectuer des actions en double pour l’objet appareil sur les deux \<DeviceObject\> . Fermé et interruption de l’application.

> [!TIP]
> Pour plus d’informations sur le cycle de vie des applications Windows 10 plateforme Windows universelle (UWP), consultez les rubriques suivantes :
> - [Cycle de vie des applications Windows 10 plateforme Windows universelle (UWP)](../launch-resume/app-lifecycle.md)
> - [Gérer la suspension de l’application](../launch-resume/suspend-an-app.md)
> - [Gérer la reprise d’une application](../launch-resume/resume-an-app.md)
