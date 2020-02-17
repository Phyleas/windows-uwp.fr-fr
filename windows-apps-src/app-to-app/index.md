---
ms.assetid: E0728EB0-DFC3-4203-A367-8997B16E2328
description: Cette section explique comment partager des données entre des applications UWP, notamment comment utiliser le contrat de partage, copier et coller, et glisser-déplacer.
title: Communication entre les applications
ms.date: 02/12/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2de9d5ae04c526c7c35fda4a7aef713814b4fbc2
ms.sourcegitcommit: 366c96d16e9a330bd4fbd9e1e19983f890f72642
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77258335"
---
# <a name="app-to-app-communication"></a>Communication entre les applications

Cette section explique comment partager des données entre des applications UWP, notamment comment utiliser le contrat de partage, le copier-coller, le glisser-déplacer et les services d’application.

Le contrat de partage permet aux utilisateurs d’échanger rapidement des données entre des applications. Par exemple, un utilisateur peut partager une page web avec ses amis à l’aide d’une application de réseau social ou enregistrer un lien dans une application de prise de notes pour s’y référer plus tard. Utilisez un contrat de partage si votre application reçoit du contenu dans les scénarios qu’un utilisateur peut traiter rapidement tout en étant dans une autre application.

Une application peut prendre en charge la fonctionnalité de partage de deux manières. En premier lieu, il peut s’agir d’une [application source qui fournit le contenu que l’utilisateur veut partager](share-data.md). En second lieu, l’application peut être une [application cible que l’utilisateur sélectionne comme destination du contenu partagé](receive-data.md). Notez qu’une application peut à la fois être une application source ou une application cible. Si vous voulez que votre application partage du contenu en tant qu’application source, vous devez déterminer quels formats de données votre application peut fournir.

Outre le contrat de partage, les applications peuvent également intégrer des techniques classiques de transfert de données, comme le [glisser-déplacer](../design/input/drag-and-drop.md) ou le [copier-coller](copy-and-paste.md). Outre la communication entre les applications UWP, ces méthodes prennent également en charge le partage vers et depuis des applications de bureau.

Les applications UWP peuvent également créer des [services d’application](../launch-resume/how-to-create-and-consume-an-app-service.md) qui fournissent des fonctionnalités à d’autres applications UWP. Un service d’application s’exécute sous forme de tâche en arrière-plan dans l’application hôte et peut fournir son service à d’autres applications. Par exemple, un service d’application peut fournir un service de scanneur de code-barres que d’autres applications peuvent utiliser.

## <a name="in-this-section"></a>Dans cette section

| Rubrique | Description |
|-------|-------------|
| [Partager des données](share-data.md) | Cet article explique comment prendre en charge le contrat de partage dans une application UWP. Le contrat de partage constitue un moyen simple pour partager rapidement des données, telles que du texte, des liens, des photos et vidéos, entre les applications. Par exemple, un utilisateur peut partager une page web avec ses amis à l’aide d’une application de réseau social ou enregistrer un lien dans une application de prise de notes pour s’y référer plus tard. |
| [Recevoir des données](receive-data.md) | Cet article explique comment recevoir dans votre application UWP du contenu partagé à partir d’une autre application à l’aide du contrat de partage. Ce contrat de partage permet à votre application d’être présentée en tant qu’option quand l’utilisateur appelle l’option Partager. |
| [Copier et coller](copy-and-paste.md) | Cet article explique comment prendre en charge le copier-coller dans les applications UWP en utilisant le Presse-papiers. Le copier-coller est la méthode classique utiliser pour échanger des données entre les applications, ou dans une application, et presque chaque application peut prendre en charge les opérations du Presse-papiers dans une certaine mesure. |
| [Glisser-déplacer](../design/input/drag-and-drop.md) | Cet article explique comment ajouter le glisser-déplacer dans votre application UWP. Glisser-déplacer est une méthode naturelle et classique d’interaction avec le contenu comme les images et les fichiers. Une fois implémenté, le glisser-déplacer fonctionne parfaitement dans toutes les directions, notamment d’application à application, d’application à bureau et de bureau à application. |
| [Créer et consommer un service d’application](../launch-resume/how-to-create-and-consume-an-app-service.md) | Cet article explique comment créer un service d’application dans une application UWP qui fournit des services à d’autres applications UWP.  |

## <a name="see-also"></a>Voir également

- [Développer des applications UWP](https://docs.microsoft.com/windows/uwp/develop/)
