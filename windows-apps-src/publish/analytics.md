---
Description: Bénéficiez d’une analytique détaillée pour vos applications Windows, dans l’espace partenaires ou via d’autres méthodes.
title: Analyser le niveau de performance de l’application
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, analytique, rapports, tableau de bord, applications, données, métriques
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259038"
---
# <a name="analyze-app-performance"></a>Analyser le niveau de performance de l’application

Vous pouvez afficher des analytiques détaillées pour vos applications dans l' [espace partenaires](https://partner.microsoft.com/dashboard). Les statistiques et les graphiques vous permettent de savoir où en sont vos applications : combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous pouvez également obtenir des métriques sur l’intégrité de l’application, l’utilisation des publicités, etc.

Vous pouvez afficher les rapports analytiques directement dans l’espace partenaires ou [Télécharger les rapports dont vous avez besoin](download-analytic-reports.md) pour analyser vos données hors connexion. Nous vous proposons également plusieurs moyens d' [accéder à vos données d’analyse en dehors de l’espace partenaires](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Visualiser les principales analyses de toutes vos applications

Pour visualiser les principales analyses relatives à vos applications les plus téléchargées, développez **Analyser**, puis sélectionnez **Vue d’ensemble**. Par défaut, la page Vue d’ensemble affiche des informations sur les cinq applications qui ont été acquises le plus souvent au cours de leur durée de vie. Pour choisir d’autres applications publiées à afficher, sélectionnez **Filtres**.

## <a name="view-individual-reports-for-each-app"></a>Visualiser des rapports individuels pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants :

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur les acquisitions d’extensions](add-on-acquisitions-report.md)
-   [Rapport d’utilisation](usage-report.md)
-   [Rapport d’intégrité](health-report.md)
-   [Rapport d’évaluation](ratings-report.md)
-   [Rapport Avis](reviews-report.md)
-   [Rapport Commentaires](feedback-report.md)
-   [Rapport Xbox Analytics](xbox-analytics-report.md)
-   [Rapport Insights](insights-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de campagne publicitaire](promote-your-app-report.md)


> [!NOTE]
> Les données figurant dans ces rapports dépendant des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Accéder aux données d’analyse en dehors de l’espace partenaires

Outre l’affichage des rapports dans l’espace partenaires, vous pouvez accéder à l’analyse d’application d’une autre manière.

### <a name="microsoft-store-analytics-api"></a>API d'analyse du Microsoft Store

Utilisez [l’API d’analyse du Store](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Pack de contenu du Centre de développement Windows pour Power BI

Utilisez le [Pack de contenu du centre de développement Windows pour Power bi](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) pour explorer et surveiller vos données d’analyse de l’espace partenaires dans Power bi. Power BI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser Power BI pour accéder à vos données d’analyse.

* [S’inscrire à Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [En savoir plus sur l’utilisation de Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le Pack de contenu du centre de développement Windows pour Power BI pour vous connecter à vos données d’analyse](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour vous connecter au Pack de contenu du centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification d’un répertoire Azure AD associé à votre compte espace partenaires. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans Power BI ne sont pas actualisées automatiquement et vous devez vous connecter à Power BI pour actualiser vos données. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](https://account.azure.com/organization). Pour plus d’informations sur la configuration de l’Association, consultez [associer des Azure Active Directory à votre compte espace partenaires](associate-azure-ad-with-dev-center.md).
