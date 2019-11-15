---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Le kit SDK Windows, le kit SDK Microsoft Advertising, le kit Microsoft Store Services SDK et le Microsoft Store fournissent de nombreuses fonctionnalités qui vous permettent de générer davantage de revenus avec vos applications et de conquérir des clients en engageant les utilisateurs.
title: Monétisation, engagement et services du Store
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, uwp, monétiser, engager, promouvoir, services du Store
ms.localizationpriority: medium
ms.openlocfilehash: 460179f7f57e17f78fdb3fd3bd289e761a8a7b4f
ms.sourcegitcommit: 2dba9b4e81151d14ca90d36341274a3b59926197
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057447"
---
# <a name="monetization-engagement-and-store-services"></a>Monétisation, engagement et services du Store

Le kit SDK Windows, le kit SDK Microsoft Advertising, le kit Microsoft Store Services SDK et le Microsoft Store fournissent des fonctionnalités qui vous permettent de générer davantage de revenus avec vos applications et de conquérir des clients en engageant les utilisateurs. Les rubriques de cette section indiquent comment générer ces fonctionnalités dans votre application.

Pour plus d’informations sur les frais facturés par le Microsoft Store et sur la façon dont vous recevez l’argent généré par votre application, consultez [Rémunération](../publish/getting-paid-apps.md).

## <a name="choose-a-pricing-model"></a>Choisir un modèle de tarification

:::row:::
    :::column:::
        ![Facturer un tarif pour votre application](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**Facturer un tarif pour votre application**

Vous pouvez facturer à l'avance un prix pour votre application. Nous prenons en charge une large gamme de niveaux de prix, avec notamment une option pour définir des prix variables selon les marchés. Vous pouvez même planifier des soldes pour réduire le prix de votre application pendant une période limitée.

[Définir le prix d’une application](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Essais gratuits](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**Essais gratuits**

Vous pouvez proposer une version d'évaluation gratuite de votre application pour que davantage de clients l'essaient. Pour inciter vos clients à acheter la version complète, vous pouvez limiter les fonctionnalités de la version d'évaluation (en n'y incluant par exemple que le premier niveau d'un jeu), afficher des publicités ou spécifier une durée d'évaluation limitée.

[Offrir un essai](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Achats dans l'application](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**Achats dans l'application**

Que votre application soit payante ou gratuite, vous pouvez utiliser les achats dans l'application pour vous assurer un flux de revenus continu. Utilisez les achats dans l’application pour permettre aux clients de passer d’une version gratuite de votre application à une version payante, ou proposer à la vente dans votre application des extensions durables ou consommables.

[Utiliser les achats dans l’application](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>Monétiser votre application en insérant des annonces publicitaires

:::row:::
    :::column:::
        ![À chaque contexte sa publicité](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**À chaque contexte sa publicité**

Nous prenons en charge un large éventail d'expériences publicitaires adaptées à la plupart des besoins, notamment des bannières publicitaires, des spots publicitaires (bannières et vidéos), des vidéos publicitaires linéaires, des publicités jouables et des publicités natives. Notre plateforme est conforme aux normes OpenRTB, VAST 2.x, MRAID 2, et VPAID 3 et est compatible avec MOAT et IAS.

[Explorer les options publicitaires](../publish/create-an-ad-campaign-for-your-app.md)
[Installer le SDK publicitaire](https://aka.ms/ads-sdk-uwp)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Service de médiation publicitaire](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**Service de médiation publicitaire**

Optimisez les revenus publicitaires dans vos applications grâce au service de médiation publicitaire de Microsoft qui permet de diffuser des publicités à partir de plusieurs réseaux publicitaires populaires. Vous pouvez configurer vos paramètres de médiation dans le tableau de bord de l’Espace partenaires, sans toucher une ligne de code. Si vous nous laissez configurer la médiation pour votre compte, nos algorithmes d’apprentissage vous aideront à optimiser vos revenus publicitaires sur tous les marchés pris en charge par votre application.

[Utiliser le service publicitaire](https://aka.ms/admediationblog)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Analyses](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**Analyses**

Les rapports d’analyse détaillés permettent de surveiller les performances de vos publicités dans les applications et vous fournissent les informations dont vous avez besoin pour optimiser vos revenus publicitaires. Nous proposons également une API RESTful utilisable pour obtenir ces données par programmation.

[Examiner les performances](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>Autres opportunités de monétisation

![Autres opportunités de monétisation](images/monetize-other-opportunities.png)

Vous êtes à la recherche d'autres moyens d'augmenter votre monétisation ? Envisagez ces options.

 Rubrique                | Description                 |
|--------------------|-----------------------------|
| [Programme d’affiliation Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=617665) | Gagnez des commissions en ajoutant des liens vers les produits Microsoft depuis votre application, votre blog, votre page web ou d’autres supports de communication. Ces liens peuvent renvoyer vers des applications, des jeux, de la musique, des films, du matériel, des accessoires et d’autres produits vendus sur le Microsoft Store.
| [Expérimentation A/B](https://go.microsoft.com/fwlink/p/?LinkId=722784) | Effectuez des tests A/B sur vos applications afin de mesurer l’efficacité des modifications apportées aux fonctionnalités pour certains clients, avant d’effectuer la modification globale pour le reste des clients.
| [Engager les clients avec le kit Microsoft Store Services SDK](microsoft-store-services-sdk.md) | Le kit Microsoft Store Services SDK contient des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à susciter l’engagement chez vos clients. Ces fonctionnalités incluent les notifications ciblées, les tests A/B et le lancement du hub de commentaires depuis votre application.
| [Démarrer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md) | Ajoutez du code dans vos applications UWP pour diriger vos clients Windows 10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Gérez ensuite ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans l’Espace partenaires. Cette fonctionnalité nécessite Microsoft Store Services SDK. 
| [Configurer votre application pour recevoir des notifications Push de l’Espace partenaires](configure-your-app-to-receive-dev-center-notifications.md) | Inscrivez un canal de notification pour votre application UWP pour qu’elle puisse recevoir les [notifications Push de l’Espace partenaires](../publish/send-push-notifications-to-your-apps-customers.md), et effectuez le suivi du taux de lancement d’applications résultant des notifications Push. Cette fonctionnalité nécessite Microsoft Store Services SDK.
| [Journaliser des événements personnalisés pour l’Espace partenaires](log-custom-events-for-dev-center.md) | Journalisez des événements personnalisés depuis votre application UWP, et passez en revue les événements dans le [Rapport d’utilisation](../publish/usage-report.md) de l’Espace partenaires. Cette fonctionnalité nécessite Microsoft Store Services SDK.
| [Demander des évaluations et des avis](request-ratings-and-reviews.md) | Incitez vos clients à évaluer ou à donner leur avis sur votre application en affichant par programmation une IU destinée aux évaluations et aux avis.
| [Services du Microsoft Store](using-windows-store-services.md) | Découvrez comment utiliser les API RESTful pour automatiser les soumissions effectuées vers le Windows Store et d’autres tâches associées au Windows Store, et accéder aux données d’analyse.
| [Ajouter des fonctionnalités de version de démonstration commerciale (RDX) à votre application](retail-demo-experience.md) | Incluez un mode de version de démonstration commerciale dans votre application Windows pour que les clients qui essaient des ordinateurs et des appareils sur le point de vente puissent y accéder.

## <a name="monetization-analytics"></a>Analyse de la monétisation

![Analyse de la monétisation](images/monetize-analytics.png)

Surveillez les performances de votre application dans le Microsoft Store grâce à ces rapports.

- [Résumé du paiement](../publish/payout-summary.md)
- [Rapport sur les acquisitions](../publish/acquisitions-report.md)
- [Rapport sur les acquisitions d’extensions](../publish/add-on-acquisitions-report.md)
- [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md)
- [Extraire des données d’analyse à l’aide de notre API REST](access-analytics-data-using-windows-store-services.md)
- [Créer des segments de clients](../publish/create-customer-segments.md)
- [Rapport Commentaires](../publish/feedback-report.md)
- [Rapport d’utilisation](../publish/usage-report.md)