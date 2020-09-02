---
description: Utilisez les exemples de code C# de cette section pour en savoir plus sur l’envoi des options de jeu et des codes de fin à l’aide de l’API Microsoft Store soumission.
title: Exemple C#-soumission d’une application avec des options de jeu et des codes de fin
ms.date: 07/10/2017
ms.topic: article
keywords: 'API de soumission Windows 10, UWP, Microsoft Store, exemples de code, options de jeu, codes de fin, listes avancées, C #'
ms.localizationpriority: medium
ms.openlocfilehash: 16d51a12c6d703be3a76c1c90d5ab6a2e9442603
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363192"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>\#Exemple C : soumission d’une application avec des options de jeu et des codes de fin

Cet article fournit des exemples de code C# qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* Obtenez un jeton d’accès Azure AD à utiliser avec l’API de soumission Microsoft Store.
* Créer une soumission d’applications
* Configurez la liste des données du magasin pour la soumission de l’application, y compris les options de liste avancées des [jeux](manage-app-submissions.md#gaming-options-object) et des [codes](manage-app-submissions.md#trailer-object) de fin.
* Téléchargez le fichier ZIP contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.
* Validez la soumission de l’application.

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour générer les exemples, créez une application console C# nommée **DevCenterApiSample** dans Visual Studio, copiez chaque exemple dans un fichier de code distinct dans le projet, puis générez le projet.

## <a name="prerequisites"></a>Prérequis

Ces exemples présentent les exigences suivantes :

* Ajoutez une référence à l’assembly System. Web dans votre projet.
* Installez le [Newtonsoft.Jssur](https://www.newtonsoft.com/json) le package NuGet à partir de Newtonsoft dans votre projet.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

La ```CreateAndSubmitSubmissionExample``` classe définit une ```Execute``` méthode publique qui appelle d’autres exemples de méthodes pour utiliser l’API de soumission Microsoft Store pour créer et valider une soumission d’application qui contient des options de jeu et un code de fin. Pour adapter ce code pour votre usage personnel :

* Assignez la ```tenantId``` variable à l’ID de locataire de votre application, puis affectez les ```clientId``` ```clientSecret``` variables et à l’ID client et la clé de votre application. Pour plus d’informations, consultez [comment associer une application Azure ad à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Affectez la ```applicationId``` variable à l' [ID de magasin](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous souhaitez créer une soumission.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs" id="CreateAndSubmitSubmissionExample":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

La ```DevCenterAccessTokenClient``` classe définit une méthode d’assistance qui utilise le ```tenantId``` et les ```clientId``` ```clientSecret``` valeurs pour créer un Azure ad jeton d’accès à utiliser avec l’API de soumission Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs" id="DevCenterAccessTokenClient":::

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Méthodes d’assistance pour appeler l’API de soumission et charger les fichiers de soumission

La ```DevCenterClient``` classe définit des méthodes d’assistance qui appellent diverses méthodes dans l’API de soumission Microsoft Store et chargent le fichier zip contenant les packages, les images de liste et les fichiers de code de fin pour la soumission de l’application.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs" id="DevCenterClient":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
