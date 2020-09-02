---
description: Utilisez les exemples de code Java de cette section pour en savoir plus sur l’envoi des options de jeu et des codes de fin à l’aide de l’API Microsoft Store soumission.
title: Exemple Java-soumission d’une application avec des options de jeu et des codes de fin
ms.date: 07/10/2017
ms.topic: article
keywords: API de soumission de Windows 10, UWP, Microsoft Store, exemples de code, options de jeu, codes de fin, listes avancées, Java
ms.localizationpriority: medium
ms.openlocfilehash: 38351c06fd680a16eaf54b0f2f8c84460a275028
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363072"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Exemple de code Java : soumission d’applications avec options de jeu et bandes-annonces

Cet article fournit des exemples de code Java qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* Obtenez un jeton d’accès Azure AD à utiliser avec l’API de soumission Microsoft Store.
* Créer une soumission d’applications
* Configurez la liste des données du magasin pour la soumission de l’application, y compris les options de liste avancées des [jeux](manage-app-submissions.md#gaming-options-object) et des [codes](manage-app-submissions.md#trailer-object) de fin.
* Téléchargez le fichier ZIP contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.
* Validez la soumission de l’application.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

La `CreateAndSubmitSubmissionExample` classe implémente un `main` programme qui appelle d’autres exemples de méthodes pour utiliser l’API de soumission Microsoft Store pour créer et valider une soumission d’application qui contient des options de jeu et un code de fin. Pour adapter ce code pour votre usage personnel :

* Assignez la `tenantId` variable à l’ID de locataire de votre application, puis affectez les `clientId` `clientSecret` variables et à l’ID client et la clé de votre application. Pour plus d’informations, consultez [comment associer une application Azure ad à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Affectez la `applicationId` variable à l' [ID de magasin](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez créer une soumission.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java" range="1-313":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

La `DevCenterAccessTokenClient` classe définit une méthode d’assistance qui utilise le `tenantId` et les `clientId` `clientSecret` valeurs pour créer un Azure ad jeton d’accès à utiliser avec l’API de soumission Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java" range="1-69":::


<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Méthodes d’assistance pour appeler l’API de soumission et charger les fichiers de soumission

La `DevCenterClient` classe définit des méthodes d’assistance qui appellent diverses méthodes dans l’API de soumission Microsoft Store et chargent le fichier zip contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.

> [!div class="tabbedCodeSnippets"]
:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java" range="1-224":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
