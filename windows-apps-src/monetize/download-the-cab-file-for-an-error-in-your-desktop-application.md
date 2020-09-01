---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour télécharger le fichier CAB pour une erreur dans votre application de bureau.
title: Télécharger le fichier CAB concernant une erreur dans votre application de bureau
ms.date: 03/06/2018
ms.topic: article
keywords: API Windows 10, UWP, Microsoft Store Analytics, Télécharger le fichier CAB, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162493"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Télécharger le fichier CAB concernant une erreur dans votre application de bureau

Utilisez cette méthode dans l’API Microsoft Store Analytics pour télécharger le fichier CAB associé à une erreur particulière pour une application de bureau que vous avez ajoutée au programme d' [application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Cette méthode peut uniquement Télécharger le fichier CAB pour une erreur d’application qui s’est produite au cours des 30 derniers jours. Les téléchargements de fichiers CAB sont également disponibles dans le [rapport d’intégrité](/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer le hachage d’ID du fichier CAB que vous souhaitez télécharger.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenir le hachage d’ID du fichier CAB que vous souhaitez télécharger. Pour obtenir cette valeur, utilisez la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre application et utilisez la valeur **cabIdHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | string | ID de produit de l’application de bureau pour laquelle vous souhaitez télécharger un fichier CAB. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un rapport d’analyse de l' [espace partenaires pour votre application de bureau](/windows/desktop/appxpkg/windows-desktop-application-program) (par exemple, le **rapport d’intégrité**) et récupérez l’ID de produit à partir de l’URL. |  Oui  |
| cabIdHash | string | Hachage d’ID unique du fichier CAB que vous souhaitez télécharger. Pour obtenir cette valeur, utilisez la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre application et utilisez la valeur **cabIdHash** dans le corps de la réponse de cette méthode. |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB à l’aide de cette méthode. Remplacez les paramètres *ApplicationID* et *cabIdHash* par les valeurs appropriées pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

Cette méthode retourne un code de réponse 302 (redirection) et l’en-tête d' **emplacement** dans la réponse est assigné à l’URI de signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cet URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur l’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de signalement d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Obtenir la trace de pile concernant une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)