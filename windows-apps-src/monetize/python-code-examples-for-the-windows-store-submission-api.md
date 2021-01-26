---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Utilisez les exemples de code python de cette section pour en savoir plus sur l’utilisation de l’API de soumission Microsoft Store.
title: Code Python pour soumettre des applications, des modules complémentaires et des vols
ms.date: 07/10/2017
ms.topic: article
keywords: API de soumission de Windows 10, UWP, Microsoft Store, exemples de code, Python
ms.localizationpriority: medium
ms.openlocfilehash: 3fd29164dd91199b1557496985577ea956893250
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811261"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Exemple de code Python : soumissions d’applications, d’extensions et de versions d’évaluation

Cet article fournit des exemples de code Python qui montrent comment utiliser l' [API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* [Obtenir un jeton d’accès Azure AD](#token)
* [Créer une extension](#create-add-on)
* [Crée une version d’évaluation du package](#create-package-flight)
* [Créer une soumission d’applications](#create-app-submission)
* [Créer une soumission d’extension](#create-add-on-submission)
* [Créer une soumission de version d’évaluation du package](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès Azure AD

L’exemple suivant montre comment [obtenir un jeton d’accès Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission Microsoft Store. Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API de soumission Microsoft Store avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un autre.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="1-20":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Créer une extension

L’exemple suivant montre comment [créer](create-an-add-on.md) et [supprimer](delete-an-add-on.md) un module complémentaire.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="26-52":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Crée une version d’évaluation du package

L’exemple suivant indique comment [créer](create-a-flight.md) et [supprimer](delete-a-flight.md) une version d’évaluation du package.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="58-87":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission d’application. Pour ce faire, le code crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis met à jour et valide l’envoi cloné dans l’espace partenaires. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie les détails de la nouvelle soumission et charge un nouveau package pour l’envoi vers le stockage d’objets BLOB Azure.
5. Ensuite, il [met à jour](update-an-app-submission.md) , puis [valide](commit-an-app-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="93-166":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission de module complémentaire. Pour ce faire, le code crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis met à jour et valide l’envoi cloné dans l’espace partenaires. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Après cela, elle crée [une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge une archive ZIP qui contient des icônes pour l’envoi vers le stockage d’objets BLOB Azure. Pour plus d’informations, consultez les instructions pertinentes sur le chargement d’une archive ZIP dans le stockage d’objets BLOB Azure dans [créer une soumission de module complémentaire](manage-add-on-submissions.md#create-an-add-on-submission).
5. Ensuite, il [met à jour](update-an-add-on-submission.md) , puis [valide](commit-an-add-on-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="172-245":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant montre comment utiliser plusieurs méthodes dans l’API Microsoft Store soumission pour créer une soumission de vol de packages. Pour ce faire, le code crée un nouvel envoi en tant que clone de la dernière soumission publiée, puis met à jour et valide l’envoi cloné dans l’espace partenaires. Cet exemple effectue les tâches suivantes, entre autres :

1. Pour commencer, il [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il charge un nouveau package pour l’envoi vers le stockage d’objets BLOB Azure. Pour plus d’informations, consultez les instructions pertinentes sur le chargement d’une archive ZIP dans le stockage d’objets BLOB Azure dans [créer une soumission de vol de package](manage-flight-submissions.md#create-a-package-flight-submission).
5. Ensuite, il [met à jour](update-a-flight-submission.md) , puis [valide](commit-a-flight-submission.md) la nouvelle soumission dans l’espace partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="251-325":::

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
