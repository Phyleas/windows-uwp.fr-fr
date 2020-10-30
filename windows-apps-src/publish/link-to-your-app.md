---
description: Vous pouvez aider les clients à découvrir votre application en établissant un lien vers la liste de votre application dans la Microsoft Store.
title: Créer un lien vers votre application
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, liaison, protocole du Windows Store, liaison à une application, lien vers l’application
ms.localizationpriority: medium
ms.openlocfilehash: 916f82714feb65e3d9d4db48703c831b8128021c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033912"
---
# <a name="link-to-your-app"></a>Créer un lien vers votre application


Vous pouvez aider les clients à découvrir votre application en établissant un lien vers la liste de votre application dans la Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtention du lien vers la description de votre application dans le Store

Pour obtenir l’URL de la liste des magasins de votre application, accédez à la page [identité](view-app-identity-details.md) de l’application de l’application dans la section **gestion des applications** . L’URL est au format **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Quand un client clique sur ce lien, il ouvre la page de liste Web pour votre application. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la liste de votre application.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Liaison au Listing Store de votre application avec le badge Microsoft Store

Vous pouvez lier directement à la liste de votre application avec un badge personnalisé pour permettre aux clients de savoir que votre application se trouve dans la Microsoft Store.

Pour créer votre badge, accédez à la page [badges Microsoft Store](https://developer.microsoft.com/store/badges) . Vous devez disposer de l' **ID de stockage** de 12 caractères de votre application pour générer le badge et le lien. Vous pouvez trouver l’ID du **magasin** de votre application dans la page [identité](view-app-identity-details.md) de l’application de la section **gestion des applications** .

> [!NOTE]
> Pour obtenir des informations et des exigences relatives à l’utilisation du badge Microsoft Store, consultez les [instructions marketing d’application](app-marketing-guidelines.md) .


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Liaison directe à votre application dans le Microsoft Store

Vous pouvez créer un lien qui lance le Microsoft Store et accède directement à la page de liste de votre application sans ouvrir de navigateur à l’aide du schéma **MS-Windows-Store :** URI.

Ces liens sont utiles si vous savez que vos utilisateurs se trouvent sur un appareil Windows et que vous souhaitez qu’ils arrivent directement sur la page de la liste du Windows Store. Par exemple, vous souhaiterez peut-être utiliser ce lien après avoir vérifié les chaînes de l’agent utilisateur dans un navigateur pour confirmer que le système d’exploitation de l’utilisateur prend en charge le magasin ou que vous communiquez déjà via une application UWP.

Pour utiliser ce schéma d’URI pour établir une liaison directe avec la liste de la boutique de votre application, ajoutez l’ID du magasin de votre application à ce lien :

`ms-windows-store://pdp/?ProductId=`

Pour plus d’informations sur l’utilisation du protocole Microsoft Store, consultez [lancer l’application Microsoft](../launch-resume/launch-store-app.md).

 

 




