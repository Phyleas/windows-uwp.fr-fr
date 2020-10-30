---
description: Bénéficiez d’une analytique détaillée pour vos applications Windows, dans l’espace partenaires ou via d’autres méthodes.
title: Analyser le niveau de performance de l’application
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, analytique, rapports, tableau de bord, applications, données, métriques
ms.localizationpriority: medium
ms.openlocfilehash: 08ee9c5ddf729ac7a9026c36bc9fbddad0af5dc5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032982"
---
# <a name="analyze-app-performance"></a>Analyser le niveau de performance de l’application

Vous pouvez afficher des analytiques détaillées pour vos applications dans l' [espace partenaires](https://partner.microsoft.com/dashboard). Les statistiques et les graphiques vous permettent de savoir où en sont vos applications : combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous pouvez également trouver des métriques sur l’intégrité de l’application, l’utilisation de l’annuaire et bien plus encore.

Vous pouvez afficher les rapports analytiques directement dans l’espace partenaires ou [Télécharger les rapports dont vous avez besoin](download-analytic-reports.md) pour analyser vos données hors connexion. Nous vous proposons également plusieurs moyens d' [accéder à vos données d’analyse en dehors de l’espace partenaires](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Afficher l’analytique des clés pour toutes vos applications

Pour afficher l’analytique clé de vos applications les plus téléchargées, développez **analyse** et sélectionnez **vue d’ensemble** . Par défaut, la page vue d’ensemble affiche des informations sur les cinq applications qui ont le plus de durées d’acquisition. Pour choisir différentes applications publiées à afficher, sélectionnez **filtres** .

## <a name="view-individual-reports-for-each-app"></a>Afficher des rapports individuels pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants :

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur les acquisitions des extensions](add-on-acquisitions-report.md)
-   [Rapport d’utilisation](usage-report.md)
-   [Rapport sur l’intégrité](health-report.md)
-   [Rapport sur les évaluations](ratings-report.md)
-   [Rapport sur les révisions](reviews-report.md)
-   [Rapport sur les commentaires](feedback-report.md)
-   [Rapport d’analytique Xbox](xbox-analytics-report.md)
-   [Rapport d’insights](insights-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de campagne de publicité](/windows/uwp/publish/ad-campaign-report)


> [!NOTE]
> Les données figurant dans ces rapports dépendant des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Accéder aux données d’analyse en dehors de l’espace partenaires

Outre l’affichage des rapports dans l’espace partenaires, vous pouvez accéder à l’analyse d’application d’une autre manière.

### <a name="microsoft-store-analytics-api"></a>API Microsoft Store Analytics

Utilisez l' [API Windows Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Pack de contenu du Centre de développement Windows pour Power BI

Utilisez le [Pack de contenu du centre de développement Windows pour Power bi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) pour explorer et surveiller vos données d’analyse de l’espace partenaires dans Power bi. Power BI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser Power BI pour accéder à vos données d’analyse.

* [S’inscrire à Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Découvrir comment utiliser Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le pack de contenu du Centre de développement Windows pour Power BI pour se connecter à vos données d’analyse](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour vous connecter au Pack de contenu du centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification d’un répertoire Azure AD associé à votre compte espace partenaires. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans Power BI ne sont pas actualisées automatiquement et vous devez vous connecter à Power BI pour actualiser vos données. Si votre organisation utilise déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous avez déjà Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](https://account.azure.com/organization). Pour plus d’informations sur la configuration de l’Association, consultez [associer des Azure Active Directory à votre compte espace partenaires](./associate-azure-ad-with-partner-center.md).
