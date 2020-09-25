---
Description: Après avoir créé votre application en réservant un nom, vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une soumission.
title: Soumissions d’application
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: liste de vérification, Windows, UWP, envoi, envoi, jeu, application, envoi
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 99b4d7412727e5f195c32d3f3c21fe82b284e658
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219972"
---
# <a name="app-submissions"></a>Soumissions d’application


Après avoir [créé votre application en réservant un nom](create-your-app-by-reserving-a-name.md), vous pouvez commencer à vous occuper de sa publication. La première étape consiste à créer une **soumission**.

Vous pouvez démarrer votre soumission lorsque votre application est terminée et prête pour publication, ou commencer à entrer des informations avant même d’avoir écrit la moindre ligne de code. Les mises à jour que vous apportez à votre soumission sont enregistrées, ce qui vous permet de revenir et de travailler dessus à chaque fois que vous êtes prêt.

> [!NOTE]
> Vous devez disposer d’un [compte de développeur](https://developer.microsoft.com/store/register) actif dans l' [espace partenaires](https://partner.microsoft.com/dashboard) pour envoyer des applications au Microsoft Store.

Une fois votre application publiée, vous pouvez publier une version mise à jour en créant une autre soumission dans l’espace partenaires. Le fait de créer une soumission vous permet d'introduire et de publier tous les changements nécessaires, que vous chargiez d'autres packages ou que vous changiez juste des détails comme le prix ou la catégorie. Pour créer une soumission pour une application publiée, cliquez sur **mettre à jour** en regard de l’envoi le plus récent indiqué sur la page **vue d’ensemble** . Vous pouvez également [supprimer une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) si vous en avez besoin (puis la rendre à nouveau disponible ultérieurement, si vous le souhaitez).

> [!NOTE]
> Cette section de la documentation explique comment créer une soumission d’application dans l’espace partenaires. Vous pouvez également utiliser l' [API Microsoft Store soumission](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour automatiser les envois d’applications.

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Liste de vérification relative à la soumission d’une application

Voici la liste des informations que vous pouvez fournir quand vous soumettez votre application, avec des liens vers des informations complémentaires.

Les éléments que vous devez obligatoirement fournir ou spécifier sont signalés ci-dessous. Certains sont facultatifs ou ont des valeurs par défaut que vous pouvez modifier selon vos besoins. Vous n’êtes pas obligé de travailler sur ces sections dans l’ordre indiqué ici.

### <a name="pricing-and-availability-page"></a>Page Tarification et disponibilité
| Nom du champ                    | Notes                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Marchés**                   | Valeur par défaut : tous les marchés possibles  | [Définition du prix et sélection du marché](./define-market-selection.md)         |
| **Public ciblé**                | Par défaut : public public | [Public ciblé](choose-visibility-options.md#audience) |
| **Détectabilité**                | Par défaut : rendre cette application disponible et détectable dans le Windows Store | [Détectabilité](choose-visibility-options.md#discoverability) |
| **Planification**                  | Valeur par défaut : publication dès que possible        | [Configurer une planification précise de la publication](configure-precise-release-scheduling.md) |
| **Prix de base**                | Obligatoire                                    | [Définir et planifier les prix d’une application](set-and-schedule-app-pricing.md)              |
| **Essai gratuit**                | Par défaut : aucune version d'évaluation gratuite                      | [Essai gratuit](set-app-pricing-and-availability.md#free-trial)              |
| **Prix de vente**              | Facultatif                                    | [Commercialiser des applications et composants additionnels](put-apps-and-add-ons-on-sale.md)           |
| **Gestion des licences organisationnelles**    | Par défaut : autoriser l'acquisition en volume par des organisations | [Options de gestion des licences organisationnelles](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Page Propriétés

| Nom du champ                    | Notes                                       | Informations supplémentaires                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Catégorie et sous-catégorie**  | Obligatoire                                    | [Tableau des catégories et sous-catégories](category-and-subcategory-table.md)       |
| **URL de la politique de confidentialité**            | Requis pour de nombreuses applications. Consultez le [contrat de développeur d’applications](/legal/windows/agreements/app-developer-agreement) et les [stratégies de Microsoft Store](store-policies.md#105-personal-information) | [URL de la politique de confidentialité](enter-app-properties.md#privacy-policy-url)        |
| **Site web**                   | Facultatif                                    | [Site web](enter-app-properties.md#website)                   |
| **Coordonnées du support technique**      | Obligatoire si votre produit est disponible sur Xbox ; Sinon, facultatif (mais recommandé)                                   | [Coordonnées du support technique](enter-app-properties.md#support-contact-info)              |
| **Paramètres du jeu**             | Facultatif (applicable uniquement aux jeux)         | [Paramètres du jeu](enter-app-properties.md#game-settings) |
| **Mode d’affichage**             | Facultatif                   | [Mode d’affichage](enter-app-properties.md#display-mode) |
| **Déclarations de produit**          | Par défaut : les clients peuvent installer cette application sur un autre lecteur ou dispositif de stockage. Windows peut inclure les données de cette application dans les sauvegardes automatiques sur OneDrive | [Déclarations de produit](./product-declarations.md) |
| **Configuration exigée**      | Facultatif                                    | [Configuration exigée](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Page Classification par âge

| Nom du champ                    | Notes                                       | Informations supplémentaires                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classification par âge**               | Obligatoire                                    | [Classification par âge](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Page Packages

| Nom du champ                    | Notes                                  | Informations supplémentaires                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Contrôle du chargement des packages**    | Obligatoire (au moins un package)        | [Chargement des packages d’application](upload-app-packages.md) |
| **Disponibilité de la famille d’appareils** | Par défaut : basée sur les packages       | [Disponibilité de la famille d’appareils](device-family-availability.md) |
| **Lancement de package progressif**   | Facultatif (pour les mises à jour uniquement)            | [Lancement de package progressif](gradual-package-rollout.md) |
| **Mise à jour obligatoire**          | Facultatif (pour les mises à jour uniquement)            | [Mise à jour obligatoire](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Descriptions dans le Windows Store

Vous devez indiquer toutes les informations requises pour au moins l’une des langues prises en charge par votre application. Nous vous recommandons de fournir des [descriptions dans le Windows Store](create-app-store-listings.md) dans toutes les langues prises en charge de votre application, et vous pouvez également [fournir des descriptions dans le Windows Store dans d’autres langues](create-app-store-listings.md#store-listing-languages). Pour faciliter la gestion de plusieurs annonces pour le même produit, vous pouvez [Importer et exporter des listes de magasins](import-and-export-store-listings.md).

| Nom du champ                    | Notes                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Description**               | Obligatoire                                    | [Rédaction d’une description convaincante de l’application](write-a-great-app-description.md) |
| **Nouveautés de cette version**   | Facultatif                                 | [Notes de publication](create-app-store-listings.md#whats-new-in-this-version)       |
| **Fonctionnalités de l’application**              | Facultatif                                    | [Fonctionnalités du produit](create-app-store-listings.md#product-features)         |
| **Captures d’écran**               | Obligatoire (au moins une capture d’écran ; quatre ou plus recommandés)          | [Captures d’écran](app-screenshots-and-images.md#screenshots)          |
| **Stocker des logos**               | Recommandations requis pour certaines versions de système d’exploitation | [Stocker des logos](app-screenshots-and-images.md#store-logos)             |
| **Bandes-annonce**                  | Facultatif                                    | [Bandes-annonce](app-screenshots-and-images.md#trailers)                | 
| **Image Windows 10 et Xbox (16:9 super héros art)**     | Recommandé        | [Image Windows 10 et Xbox (16:9 super héros art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Images Xbox**     | Requis pour un affichage correct si vous publiez sur Xbox        | [Images Xbox](app-screenshots-and-images.md#xbox-images) |
| **Champs supplémentaires**  | Facultatif                                    | [Champs supplémentaires](create-app-store-listings.md#supplemental-fields) 
| **Termes de recherche**              | Facultatif                                    | [Termes de recherche](create-app-store-listings.md#search-terms)         |
| **Informations de copyright et de marque déposée** | Facultatif                                 | [Informations de copyright et de marque déposée](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termes du contrat de licence supplémentaires**  | Facultatif                                    | [Termes du contrat de licence supplémentaires](create-app-store-listings.md#additional-license-terms) |
| **Développé par**              | Facultatif                                    | [Développé par](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Page Options d’envoi

| Nom du champ                    | Notes                                       | Informations supplémentaires                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Options de conservation de la publication**     | Par défaut : publier cette soumission dès qu’elle transmet la certification (ou les dates que vous avez sélectionnées dans la section planification)      | [Options de conservation de la publication](manage-submission-options.md#publishing-hold-options)    
| **Notes pour la certification**     | Recommandé          | [Notes pour la certification](notes-for-certification.md)             |
| **Fonctionnalités restreintes**     | Obligatoire si votre produit déclare des [fonctionnalités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Fonctionnalités restreintes](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Pour plus d’informations sur la publication d’applications métier directement dans les entreprises, consultez distribuer des [applications métier aux entreprises](distribute-lob-apps-to-enterprises.md).
