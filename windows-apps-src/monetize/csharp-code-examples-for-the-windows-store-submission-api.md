---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Utilisez les exemples de code C# de cette section pour en savoir plus sur l’utilisation de l’API de soumission Microsoft Store.
title: Exemples C#-soumissions pour les applications, les modules complémentaires et les vols
ms.date: 08/03/2017
ms.topic: article
keywords: 'API de soumission Windows 10, UWP, Microsoft Store, exemples de code, C #'
ms.localizationpriority: medium
ms.openlocfilehash: ac16d6932a2f20e701d7446ac8c21c316cfe5d4a
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364122"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>\#Exemple C : soumissions pour les applications, les modules complémentaires et les vols

Cet article fournit des exemples de code C# qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* [Créer une soumission d’applications](#create-app-submission)
* [Créer une soumission d’extension](#create-add-on-submission)
* [Mettre à jour une soumission d’extension](#update-add-on-submission)
* [Créer une soumission de version d’évaluation du package](#create-flight-submission)

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour générer les exemples, créez une application console C# nommée **DeveloperApiCSharpSample** dans Visual Studio, copiez chaque exemple dans un fichier de code distinct dans le projet et générez le projet.

## <a name="prerequisites"></a>Prérequis

Ces exemples utilisent les bibliothèques suivantes :

* Microsoft.WindowsAzure.Storage.dll. Cette bibliothèque est disponible dans le [kit de développement logiciel Microsoft Azure SDK pour .NET](https://azure.microsoft.com/downloads/), ou vous pouvez l’obtenir en installant le [package NuGet WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* [Newtonsoft.Js](https://www.newtonsoft.com/json) Package NuGet à partir de Newtonsoft.

## <a name="main-program"></a>Programme principal

L’exemple suivant implémente un programme en ligne de commande qui appelle les autres exemples de méthode de cet article pour illustrer les différentes façons d’utiliser l’API de soumission Microsoft Store. Adaptez ce programme en fonction de vos besoins, comme suit :

* Affectez ```ApplicationId``` les ```InAppProductId``` Propriétés, et ```FlightId``` à l’ID de l’application, du module complémentaire et du vol de package que vous souhaitez gérer.
* Affectez les propriétés ```ClientId``` et ```ClientSecret``` à l’ID client et à la clé de votre application, et remplacez la chaîne *tenantid* dans l’URL ```TokenEndpoint``` par l’ID de locataire pour votre application. Pour plus d’informations, consultez [comment associer une application Azure ad à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/Program.cs" id="Main":::

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>Classe d’assistance ClientConfiguration

L’exemple d’application utilise la ```ClientConfiguration``` classe d’assistance pour passer Azure Active Directory données et les données d’application à chacun des exemples de méthodes qui utilisent l’API de soumission Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/ClientConfiguration.cs" id="ClientConfiguration":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API Microsoft Store soumission pour mettre à jour une soumission d’application. La ```RunAppSubmissionUpdateSample``` méthode de la classe crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis il met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode ```RunAppSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage Blob Azure.
5. Ensuite, il [met à jour](update-an-app-submission.md) , puis [valide](commit-an-app-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs" id="AppSubmissionUpdateSample":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API de soumission Microsoft Store pour créer une soumission de complément. Plus précisément, la méthode ```RunInAppProductSubmissionCreateSample``` fournie dans cette classe effectue les tâches suivantes :

1. Pour commencer, la méthode [crée une extension](create-an-add-on.md).
2. Ensuite, elle [crée une soumission pour la nouvelle extension](create-an-add-on-submission.md).
3. Elle charge une archive ZIP contenant des icônes associées à la soumission dans le stockage d’objets blob Azure.
4. Ensuite, il [valide la nouvelle soumission à l’espace partenaires](commit-an-add-on-submission.md).
5. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs" id="InAppProductSubmissionCreateSample":::

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Mettre à jour une soumission d’extension

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API Microsoft Store soumission pour mettre à jour une soumission de complément existante. La ```RunInAppProductSubmissionUpdateSample``` méthode de la classe crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis il met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode ```RunInAppProductSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Après cela, elle crée [une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
5. Ensuite, il [met à jour](update-an-add-on-submission.md) , puis [valide](commit-an-add-on-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs" id="InAppProductSubmissionUpdateSample":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API Microsoft Store soumission pour mettre à jour une soumission de vol de package. La ```RunFlightSubmissionUpdateSample``` méthode de la classe crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis il met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode ```RunFlightSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge un nouveau package associé à la soumission dans le stockage d’objets blob Azure.
5. Ensuite, il [met à jour](update-a-flight-submission.md) , puis [valide](commit-a-flight-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs" id="FlightSubmissionUpdateSample":::

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>Classe d’assistance IngestionClient

La classe ```IngestionClient``` fournit des méthodes d’assistance qui sont utilisées par d’autres méthodes dans l’exemple d’application pour effectuer les tâches suivantes :

* [Obtenez un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) qui peut être utilisé pour appeler des méthodes dans l’API de soumission Microsoft Store. Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API de soumission Microsoft Store avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
* Charger une archive ZIP contenant de nouvelles ressources pour une soumission d’application ou d’extension dans le stockage Blob Azure. Pour plus d’informations sur le chargement d’une archive ZIP dans le stockage Blob Azure pour les soumissions d’application et d’extension, consultez les instructions correspondantes dans [Créer une soumission d’application](manage-app-submissions.md#create-an-app-submission) et [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission).
* Traitez les requêtes HTTP pour l’API de soumission Microsoft Store.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/IngestionClient.cs" id="IngestionClient":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
