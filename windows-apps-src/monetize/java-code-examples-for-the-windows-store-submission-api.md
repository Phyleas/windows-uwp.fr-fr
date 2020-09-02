---
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Utilisez les exemples de code Java de cette section pour en savoir plus sur l’utilisation de l’API de soumission Microsoft Store.
title: Exemples Java-soumissions pour les applications, les modules complémentaires et les vols
ms.date: 07/10/2017
ms.topic: article
keywords: API de soumission de Windows 10, UWP, Microsoft Store, exemples de code, Java
ms.localizationpriority: medium
ms.openlocfilehash: d10390dbb5364ff4f05de211167551d91dfab858
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363932"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Exemple de code Java : soumissions d’applications, d’extensions et de versions d’évaluation

Cet article fournit des exemples de code Java qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* [Obtenir un jeton d’accès Azure AD](#token)
* [Créer une extension](#create-add-on)
* [Crée une version d’évaluation du package](#create-package-flight)
* [Créer une soumission d’applications](#create-app-submission)
* [Créer une soumission d’extension](#create-add-on-submission)
* [Créer une soumission de version d’évaluation du package](#create-flight-submission)

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour obtenir le listing du code complet, voir la section du [listing du code](java-code-examples-for-the-windows-store-submission-api.md#code-listing) à la fin de cet article.

## <a name="prerequisites"></a>Prérequis

Ces exemples utilisent les bibliothèques suivantes :

* [Journalisation Apache](https://commons.apache.org/proper/commons-logging/)  -commons-logging-1,2 (1.2. jar).
* [Apache HttpComponents Core 4.4.5 et Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar et httpclient-4.5.2.jar).
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) et [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar et javax.json-1.0.4.jar).

## <a name="main-program-and-imports"></a>Programme principal et importations

L’exemple suivant montre les instructions d’importation utilisées par tous les exemples de code et implémente un programme de ligne de commande qui appelle les autres exemples de méthode.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/MainExample.java" range="1-64":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

L’exemple suivant montre comment [obtenir un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission Microsoft Store. Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API de soumission Microsoft Store avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="65-95":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Créer une extension

L’exemple suivant montre comment [créer](create-an-add-on.md) et [supprimer](delete-an-add-on.md) un module complémentaire.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="310-345":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crée une version d’évaluation du package

L’exemple suivant indique comment [créer](create-a-flight.md) et [supprimer](delete-a-flight.md) une version d’évaluation du package.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="185-221":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission d’application. Pour ce faire, la `SubmitNewApplicationSubmission` méthode crée une soumission en tant que clone de la dernière soumission publiée, puis elle met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode `SubmitNewApplicationSubmission` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage Blob Azure.
5. Ensuite, il [met à jour](update-an-app-submission.md) , puis [valide](commit-an-app-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="97-183":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission de module complémentaire. Pour ce faire, la `SubmitNewInAppProductSubmission` méthode crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode `SubmitNewInAppProductSubmission` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Après cela, elle crée [une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge une archive ZIP contenant des icônes associées à la soumission dans le stockage d’objets blob Azure.
5. Ensuite, il [met à jour](update-an-add-on-submission.md) , puis [valide](commit-an-add-on-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="347-431":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission de vol de packages. Pour ce faire, la `SubmitNewFlightSubmission` méthode crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis met à jour et valide l’envoi cloné dans l’espace partenaires. Plus précisément, la méthode `SubmitNewFlightSubmission` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge un nouveau package associé à la soumission dans le stockage d’objets blob Azure.
5. Ensuite, il [met à jour](update-a-flight-submission.md) puis [valide](commit-a-flight-submission.md) la nouvelle soumission à PartnerCenter.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="223-308":::

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Méthodes d’utilitaire pour charger des fichiers de soumission et gérer les réponses aux demandes

Les méthodes d’utilitaire suivantes illustrent les tâches suivantes :

* Chargement d’une archive ZIP contenant de nouvelles ressources pour une soumission d’applications ou d’extension dans le stockage d’objets blob Azure. Pour plus d’informations sur le chargement d’une archive ZIP dans le stockage d’objets blob Azure pour les soumissions d’applications et d’extension, consultez les instructions correspondantes dans [Créer une soumission d’applications](manage-app-submissions.md#create-an-app-submission), [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission) et [Créer une soumission de version d’évaluation du package](manage-flight-submissions.md#create-a-package-flight-submission).
* Gestion des réponses aux demandes.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="433-490":::

<span id="code-listing" />

## <a name="complete-code-listing"></a>Listing du code complet

Le listing du code suivant contient tous les exemples précédents organisés dans un seul fichier source.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="1-491":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
