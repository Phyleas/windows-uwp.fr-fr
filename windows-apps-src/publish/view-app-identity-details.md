---
Description: Affichez les détails relatifs à l’identité unique assignée à votre application par le Microsoft Store et recevez un lien vers la liste des boutiques de votre application.
title: Affichage des détails d’identité de l’application
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9232acbf83659c661e1b1f3c35a7fb7ad546e819
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157923"
---
# <a name="view-app-identity-details"></a>Affichage des détails d’identité de l’application


Vous pouvez afficher les détails relatifs à l’identité unique assignée à votre application par le Microsoft Store sur ses pages d' **identité d’application** . Vous pouvez également obtenir un lien vers la liste des boutiques de votre application sur cette page.

Pour consulter ces informations, accédez à l’une de vos applications, puis développez l’option **Gestion des applications** dans le menu de navigation gauche. Sélectionnez **identité** de l’application pour afficher ces détails.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valeurs à inclure dans le manifeste de votre package d’application

Les valeurs suivantes doivent être incluses dans le manifeste de votre package. Si vous [utilisez Microsoft Visual Studio pour créer vos packages](/windows/msix/package/packaging-uwp-apps)et que vous êtes connecté avec le même compte Microsoft que celui que vous avez associé à votre compte de développeur, ces détails sont inclus automatiquement. Si vous générez votre package manuellement, vous devez ajouter les éléments suivants :

-   **Package/Identité/Nom**
-   **Package/Identité/Serveur de publication**
-   **Package/propriétés/PublisherDisplayName**

Pour plus d'informations, voir [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans la [référence du schéma de manifeste de package](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Utilisés conjointement, ces éléments déclarent l'identité de votre application en établissant la « famille de packages » à laquelle appartiennent tous les packages de l'application. Les différents packages comporteront des détails supplémentaires, comme l'architecture et la version.


## <a name="additional-values-for-package-family"></a>Valeurs supplémentaires concernant la famille de packages

Les valeurs supplémentaires ci-après font référence à la famille de packages de votre application, mais ne figurent pas dans votre manifeste.

-   **Nom de la famille de packages (PFN)** : cette valeur est utilisée avec certaines API Windows.
-   **SID du package** : vous aurez besoin de cette valeur pour envoyer des notifications WNS à votre application. Pour plus d'informations, voir l'article [Vue d'ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Lien d’accès à la description de votre application

Le lien direct vers la page de votre application peut être partagé pour aider vos clients à trouver l’application dans le Windows Store. Ce lien est au format **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Quand un client clique sur ce lien, il ouvre la page de liste Web pour votre application. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la liste de votre application.

L’**ID Windows Store** de votre application figure également dans cette section. Cet ID Windows Store peut être utilisé pour [générer des badges Windows Store](https://developer.microsoft.com/store/badges) ou pour identifier votre application.

Le **lien de protocole du magasin** peut être utilisé pour établir une liaison directe avec votre application dans le magasin sans ouvrir de navigateur, par exemple quand vous effectuez une liaison à partir d’une application. Pour plus d’informations, consultez [lien vers votre application](link-to-your-app.md).



 

 