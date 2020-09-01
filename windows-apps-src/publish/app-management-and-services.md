---
Description: Gérez et affichez les détails relatifs à chacune de vos applications dans l’espace partenaires, et configurez les services tels que les tests et cartes A/B.
title: Gestion des applications et services
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e34a4d87ae3df23f8641a09d1558b8c8606667fa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164283"
---
# <a name="app-management-and-services"></a>Gestion des applications et services

Vous pouvez gérer et afficher les détails relatifs à chacune de vos applications dans l' [espace partenaires](https://partner.microsoft.com/dashboard), et configurer des services tels que les notifications, les tests A/B et les mappages.

Lorsque vous utilisez une application dans l’espace partenaires, vous verrez des sections dans le menu de navigation gauche pour la gestion des **services** et des **applications**. Développez ces sections pour accéder aux fonctionnalités décrites ci-après.

## <a name="services"></a>Services

La section **Services** vous permet de gérer les différents services pour vos applications.

## <a name="xbox-live"></a>Xbox Live

Si vous publiez un jeu, vous pouvez activer le [programme de créateurs Xbox Live](https://www.xbox.com/developers/creators-program) sur cette page. Cela vous permet de commencer à configurer et à tester les fonctionnalités de Xbox Live, puis de publier votre jeu de programme de créateurs Xbox Live.

Pour plus d’informations, consultez [la page prise en main du programme des créateurs Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) et [création d’un nouveau titre de programme de créateurs Xbox Live et publication dans l’environnement de test](/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Expérimentation

Utilisez la page **Expérimentation** pour créer et exécuter des expériences pour vos applications de plateforme Windows universelle (UWP) avec des tests A/B. Un test A/B vous permet d’évaluer l’efficacité des modifications apportées aux fonctionnalités de votre application (ou leurs variantes) sur certains clients avant d’implémenter les modifications pour le reste de vos clients.

Pour plus d’informations, voir [Exécuter des expériences d’application avec des tests A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Maps

Pour utiliser les services de carte dans les applications ciblant Windows 10 ou Windows 8. x, visitez le [Centre de développement Bing Maps](https://www.bingmapsportal.com/). Pour plus d’informations sur la façon de demander une clé d’authentification Maps à partir du centre de développement Bing Maps et de l’ajouter à votre application, consultez [demander une clé d’authentification par mappages](../maps-and-location/authentication-key.md) pour plus d’informations. 

Utilisez la page **mappages** uniquement pour les applications précédemment publiées pour Windows Phone 8,1 et versions antérieures. Pour utiliser les services de mappage dans ces applications, vous devez demander un ID d’application de service cartographique et un jeton à inclure dans le code de votre application. Lorsque vous cliquez sur **obtenir un jeton**, nous générons un ID d’application de service cartographique (**ApplicationID**) et un jeton d’authentification de service de mappage (**AuthenticationToken**) pour votre application. Veillez à ajouter ces valeurs à votre code avant d’empaqueter et d’envoyer votre application. Pour plus d’informations, voir [Comment ajouter un contrôle de carte à une page (Windows Phone 8.1)](/previous-versions/windows/apps/jj207033(v=vs.105)).

## <a name="product-collections-and-purchases"></a>Collections et achats de produits

Pour utiliser l’API de collection Microsoft Store et l’API d’achat Microsoft Store pour accéder aux informations de propriété pour les applications et les modules complémentaires, vous devez entrer les ID client Azure AD associés ici. Notez que la prise en compte de ces modifications peut prendre jusqu’à 16 heures.

Pour plus d’informations, consultez [gérer les habilitations de produit à partir d’un service](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentement de l’administrateur

Si votre produit s’intègre à Azure AD et appelle des API qui demandent des [autorisations d’application ou des autorisations déléguées](/graph/permissions-reference) qui requièrent le consentement de l’administrateur, entrez votre ID de client Azure ad ici. Cela permet aux administrateurs qui acquièrent l’application pour leur organisation de donner leur consentement pour que votre produit agisse pour le compte de tous les utilisateurs du locataire.

Pour plus d’informations, consultez [Demande de consentement d’un client entier](/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>Gestion des applications

La section **gestion des applications** vous permet d’afficher les détails des packages et des identités, ainsi que de gérer les noms de votre application.

## <a name="app-identity"></a>Identité des applications

Cette page propose des informations détaillées sur l’identité unique de votre application dans le Windows Store, notamment la ou les URL à associer à la description de l’application.

Pour plus d’informations, voir [Visualiser les détails d’identité d’application](view-app-identity-details.md).

## <a name="manage-app-names"></a>Gestion des noms d’application

C’est là que vous visualisez tous les noms que vous avez réservés pour votre application. Vous pouvez réserver d’autres noms ici ou supprimer des noms que vous n’utilisez plus.

Pour plus d’informations, voir [Gestion des noms d’application](manage-app-names.md).

## <a name="current-packages"></a>Packages actuels

Cette page propose des informations détaillées sur tous vos packages publiés.

> [!NOTE]
> Vous ne verrez aucune information ici tant que votre application n’a pas été publiée.

Nous affichons le nom, la version et l’architecture de chaque package. Cliquez sur **Détails** pour accéder à des informations complémentaires comme la langue prise en charge, les fonctionnalités de l’application et les tailles de fichier. Les informations affichées pour chaque package peuvent varier en fonction du système d’exploitation ciblé et d’autres facteurs. 

Les développeurs ayant des autorisations OEM peuvent également [générer des packages de préinstallation](generate-preinstall-packages-for-oems.md) depuis la page **Packages actuels**.

## <a name="wnsmpns"></a>WNS/MPNS

La section **WNS/MPNs** fournit des options pour vous aider à créer et à envoyer des notifications aux clients de votre application. 

> [!TIP]
> Pour les applications UWP, nous vous suggérons d’utiliser la fonctionnalité **notifications** dans l’espace partenaires. Cette fonctionnalité vous permet d’envoyer des notifications à tous les clients de votre application, ou à un sous-ensemble ciblé de vos clients Windows 10 qui répondent aux critères que vous avez définis dans un [segment de clientèle](create-customer-segments.md). Pour plus d’informations, consultez [Envoyer des notifications aux clients de votre application](send-push-notifications-to-your-apps-customers.md).

En fonction du type de package de votre application et de ses exigences spécifiques, vous pouvez également utiliser l’une des options suivantes : 

-   **Windows Push Notification Services (WNS)** vous permet d’envoyer un toast, une vignette, un badge et des mises à jour brutes depuis votre propre service cloud. Pour plus d'informations, voir l'article [Vue d'ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Microsoft Azure Mobile Apps** vous permet d’envoyer des notifications Push, d’authentifier et de gérer les utilisateurs des applications et de stocker les données des applications dans le cloud. Pour plus d’informations, voir la [documentation sur les applications mobiles](/azure/app-service-mobile/).

-   Le **service de notifications push Microsoft (MPNS)** peut être utilisé avec des packages. xap précédemment publiés pour Windows Phone. Vous pouvez envoyer un nombre limité de notifications non authentifiées sans intervenir sur la configuration, bien que nous vous recommandons d’utiliser des notifications authentifiées pour éviter les seuils de limitation. Si vous utilisez MPNS, vous devez télécharger un certificat dans le champ fourni sur la page **WNS/MPNs** . Pour plus d’informations, voir [Configuration d’un service web authentifié pour envoyer des notifications Push pour Windows Phone 8](/previous-versions/windows/apps/ff941099(v=vs.105)).
 

 