---
description: Utilisez les exemples de code python de cette section pour en savoir plus sur l’envoi des options de jeu et des codes de fin à l’aide de l’API Microsoft Store soumission.
title: Exemple Python-soumission d’une application avec des options de jeu et des codes de fin
ms.date: 07/10/2017
ms.topic: article
keywords: API de soumission de Windows 10, UWP, Microsoft Store, exemples de code, options de jeu, codes de fin, listes avancées, Python
ms.localizationpriority: medium
ms.openlocfilehash: 26a2062ff1d8e0f03ff8507cab89bed942012143
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364072"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Exemple de code Python : soumission d’applications avec options de jeu et bandes-annonces

Cet article fournit des exemples de code Python qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* Obtenez un jeton d’accès Azure AD à utiliser avec l’API de soumission Microsoft Store.
* Créer une soumission d’applications
* Configurez la liste des données du magasin pour la soumission de l’application, y compris les options de liste avancées des [jeux](manage-app-submissions.md#gaming-options-object) et des [codes](manage-app-submissions.md#trailer-object) de fin.
* Téléchargez le fichier ZIP contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.
* Validez la soumission de l’application.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

Ce code appelle d’autres exemples de classes et de fonctions pour utiliser l’API de soumission Microsoft Store pour créer et valider une soumission d’application qui contient des options de jeu et un code de fin. Pour adapter ce code pour votre usage personnel :

* Assignez la `tenant` variable à l’ID de locataire de votre application, puis affectez les `client` `secret` variables et à l’ID client et la clé de votre application. Pour plus d’informations, consultez [comment associer une application Azure ad à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Affectez la `application_id` variable à l' [ID de magasin](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez créer une soumission.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py" range="1-74":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Obtention d’un jeton d’accès Azure AD et appel de l’API de soumission

L’exemple suivant définit les classes suivantes :

* La `DevCenterAccessTokenClient` classe définit une méthode d’assistance qui utilise le `tenantId` et les `clientId` `clientSecret` valeurs pour créer un Azure ad jeton d’accès à utiliser avec l’API de soumission Microsoft Store.
* La `DevCenterClient` classe définit des méthodes d’assistance qui appellent diverses méthodes dans l’API de soumission Microsoft Store et chargent le fichier zip contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py" range="1-126":::

<span id="token" />

## <a name="get-app-submission-listing-data"></a>Obtenir les données de la soumission de l’application

L’exemple suivant définit des fonctions d’assistance qui renvoient des données de liste au format JSON pour un nouvel exemple d’envoi d’application.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py" range="1-170":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
