---
Description: Après avoir créé votre application en réservant un nom, vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une soumission.
title: Soumissions d’application
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: liste de vérification, windows, uwp, soumission, soumettre, jeu, application
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260023"
---
# <a name="app-submissions"></a>Soumissions d’application


Après avoir [créé votre application en réservant un nom](create-your-app-by-reserving-a-name.md), vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une **soumission**.

Vous pouvez démarrer votre soumission lorsque votre application est terminée et prête pour publication, ou commencer à entrer des informations avant même d’avoir écrit la moindre ligne de code. Les mises à jour que vous apportez à votre soumission sont enregistrées, ce qui vous permet de revenir et de travailler dessus à chaque fois que vous êtes prêt.

> [!NOTE]
> Vous devez disposer d’un [compte de développeur](https://developer.microsoft.com/store/register) actif dans l' [espace partenaires](https://partner.microsoft.com/dashboard) pour envoyer des applications au Microsoft Store.

Une fois votre application publiée, vous pouvez publier une version mise à jour en créant une autre soumission dans l’espace partenaires. Le fait de créer une soumission vous permet d'introduire et de publier tous les changements nécessaires, que vous chargiez d'autres packages ou que vous changiez juste des détails comme le prix ou la catégorie. Pour créer une soumission pour une application publiée, cliquez sur **mettre à jour** en regard de l’envoi le plus récent indiqué sur la page **vue d’ensemble** . Vous pouvez également [supprimer une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) si vous en avez besoin (puis la rendre à nouveau disponible ultérieurement, si vous le souhaitez).

> [!NOTE]
> Cette section de la documentation explique comment créer une soumission d’application dans l’espace partenaires. Sinon, vous pouvez utiliser [l’API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser la soumission d’applications.

> [!IMPORTANT]
> Depuis le 31 octobre 2018, les nouveaux produits ne peuvent pas inclure des packages ciblant Windows 8. x/Windows Phone 8. x ou une version antérieure. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Liste de vérification relative à la soumission d’une application

Voici la liste des informations que vous pouvez fournir quand vous soumettez votre application, avec des liens vers des informations complémentaires.

Les éléments que vous devez obligatoirement fournir ou spécifier sont signalés ci-dessous. Certains sont facultatifs ou ont des valeurs par défaut que vous pouvez modifier selon vos besoins. Vous n’avez pas besoin de travailler sur ces sections dans l’ordre indiqué ici.

### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité
| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Secteurs**                   | Par défaut : tous les marchés possibles  | [Définir la tarification et la sélection du marché](define-pricing-and-market-selection.md)         |
| **Audience**                | Par défaut : public non privé | [Audience](choose-visibility-options.md#audience) |
| **Découverte**                | Par défaut : rendre cette application accessible et détectable dans le Store | [Découverte](choose-visibility-options.md#discoverability) |
| **Programmateur**                  | Par défaut : publication dès que possible        | [Configurer une planification de publication précise](configure-precise-release-scheduling.md) |
| **Prix de base**                | Obligatoire                                    | [Définir et planifier la tarification des applications](set-and-schedule-app-pricing.md)              |
| **Évaluation gratuite**                | Par défaut : aucune version d'évaluation gratuite                      | [Évaluation gratuite](set-app-pricing-and-availability.md#free-trial)              |
| **Tarif réduit**              | Facultatif                                    | [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md)           |
| **Licences organisationnelles**    | Par défaut : autoriser l'acquisition en volume par des organisations | [Options de licence d’organisation](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Catégorie et sous-catégorie**  | Obligatoire                                    | [Table de catégorie et de sous-catégorie](category-and-subcategory-table.md)       |
| **URL de la politique de confidentialité**            | Obligatoire pour plusieurs applications. Voir le [contrat du développeur d'applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [stratégies du Microsoft Store](store-policies.md#105-personal-information) | [URL de la politique de confidentialité](enter-app-properties.md#privacy-policy-url)        |
| **Hameçonnage**                   | Facultatif                                    | [Hameçonnage](enter-app-properties.md#website)                   |
| **Informations de contact du support technique**      | Obligatoire si votre produit est disponible sur Xbox ; dans le cas contraire facultatif (mais recommandé)                                   | [Informations de contact du support technique](enter-app-properties.md#support-contact-info)              |
| **Paramètres du jeu**             | Facultatif (applicable uniquement aux jeux)         | [Paramètres du jeu](enter-app-properties.md#game-settings) |
| **Mode d’affichage**             | Facultatif                   | [Mode d’affichage](enter-app-properties.md#display-mode) |
| **Déclarations de produit**          | Par défaut : les clients peuvent installer cette application sur un autre lecteur ou dispositif de stockage. Windows peut inclure les données de cette application dans les sauvegardes automatiques sur OneDrive | [Déclarations de produit](app-declarations.md) |
| **Configuration système requise**      | Facultatif                                    | [Configuration système requise](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Page Classification par âge

| Nom du champ                    | Remarques                                       | Informations supplémentaires                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Évaluations de l’âge**               | Obligatoire                                    | [Évaluations de l’âge](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Page Packages

| Nom du champ                    | Remarques                                  | Informations supplémentaires                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Contrôle de téléchargement de package**    | Obligatoire (au moins un package)        | [Télécharger des packages d’application](upload-app-packages.md) |
| **Disponibilité de la famille d’appareils** | Par défaut : basée sur les packages       | [Disponibilité de la famille d’appareils](device-family-availability.md) |
| **Déploiement graduel de packages**   | Facultatif (pour les mises à jour uniquement)            | [Déploiement graduel de packages](gradual-package-rollout.md) |
| **Mise à jour obligatoire**          | Facultatif (pour les mises à jour uniquement)            | [Mise à jour obligatoire](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descriptions dans le Windows Store

Vous devez indiquer toutes les informations requises pour au moins l’une des langues prises en charge par votre application. Nous vous recommandons de fournir des [descriptions dans le Windows Store](create-app-store-listings.md) dans toutes les langues prises en charge de votre application, et vous pouvez également [fournir des descriptions dans le Windows Store dans d’autres langues](create-app-store-listings.md#store-listing-languages). Pour faciliter la gestion de plusieurs descriptions pour le même produit, vous pouvez [importer et exporter des descriptions du Store](import-and-export-store-listings.md).

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Obligatoire                                    | [Écrire une description d’application intéressante](write-a-great-app-description.md) |
| **Nouveautés de cette version**   | Facultatif                                 | [Notes de publication](create-app-store-listings.md#whats-new-in-this-version)       |
| **Fonctionnalités de l’application**              | Facultatif                                    | [Fonctionnalités du produit](create-app-store-listings.md#product-features)         |
| **Captures d’écran**               | Obligatoire (au moins une capture d’écran, quatre ou plus recommandées)          | [Captures d’écran](app-screenshots-and-images.md#screenshots)          |
| **Stocker les logos**               | Recommandé ; obligatoire pour certaines versions du système d’exploitation | [Stocker les logos](app-screenshots-and-images.md#store-logos)             |
| **Codes**                  | Facultatif                                    | [Codes](app-screenshots-and-images.md#trailers)                | 
| **Image Windows 10 et Xbox (16:9 super héros art)**     | Recommandé        | [Image Windows 10 et Xbox (16:9 super héros art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Images Xbox**     | Requis pour un affichage correct si vous publiez sur Xbox        | [Images Xbox](app-screenshots-and-images.md#xbox-images) |
| **Champs supplémentaires**  | Facultatif                                    | [Champs supplémentaires](create-app-store-listings.md#supplemental-fields) 
| **Termes de recherche**              | Facultatif                                    | [Termes de recherche](create-app-store-listings.md#search-terms)         |
| **Informations de copyright et de marque** | Facultatif                                 | [Informations de copyright et de marque](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termes du contrat de licence supplémentaires**  | Facultatif                                    | [Termes du contrat de licence supplémentaires](create-app-store-listings.md#additional-license-terms) |
| **Développé par**              | Facultatif                                    | [Développé par](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Page des options de soumission

| Nom du champ                    | Remarques                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Options de conservation de la publication**     | Par défaut : publier cette soumission dès qu’elle a obtenu la certification (ou selon les dates que vous avez sélectionnées dans la section Planification)      | [Options de conservation de la publication](manage-submission-options.md#publishing-hold-options)    
| **Notes pour la certification**     | Recommandé          | [Notes pour la certification](notes-for-certification.md)             |
| **Fonctionnalités restreintes**     | Obligatoire si votre produit déclare des [fonctionnalités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Fonctionnalités restreintes](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Pour plus d’informations sur la publication d’applications métier directement à l’attention des entreprises, voir [Distribuer des applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).
