---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir la trace de la pile pour une erreur dans votre application de bureau.
title: Obtenir la trace de pile concernant une erreur dans votre application de bureau
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, trace de la pile, erreur, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 6a960a455260c208b5a38139e24d00076c4fc45d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155583"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Obtenir la trace de pile concernant une erreur dans votre application de bureau

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir la trace de la pile pour une erreur dans une application de bureau que vous avez ajoutée au [programme d’application de bureau Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Cette méthode peut uniquement Télécharger la trace de la pile pour une erreur qui s’est produite au cours des 30 derniers jours. Les traces de pile sont également disponibles dans le [rapport d’intégrité](/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer le hachage d’ID du fichier cab associé à l’erreur pour laquelle vous souhaitez récupérer la trace de la pile.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenir le hachage d’ID du fichier CAB associé à l’erreur pour laquelle vous souhaitez récupérer la trace de la pile. Pour obtenir cette valeur, utilisez la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre application et utilisez la valeur **cabIdHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
 

### <a name="request-parameters"></a>Paramètres de la demande

| Paramètre        | Type   |  Description      |  Obligatoire  |
|---------------|--------|---------------|------|
| applicationId | string | ID de produit de l’application de bureau pour laquelle vous souhaitez obtenir une trace de la pile. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [rapport analytique pour votre application de bureau dans l’espace partenaires](/windows/desktop/appxpkg/windows-desktop-application-program) (par exemple, le **rapport d’intégrité**) et récupérez l’ID de produit à partir de l’URL. |  Oui  |
| cabIdHash | string | Hachage d’ID unique du fichier CAB associé à l’erreur pour laquelle vous souhaitez récupérer la trace de la pile. Pour obtenir cette valeur, utilisez la méthode [obtenir les détails d’une erreur dans votre application de bureau](get-details-for-an-error-in-your-desktop-application.md) pour récupérer les détails d’une erreur spécifique dans votre application et utilisez la valeur **cabIdHash** dans le corps de la réponse de cette méthode. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir la trace de pile avec cette méthode. Remplacez les paramètres *ApplicationID* et *cabIdHash* par les valeurs appropriées pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response


### <a name="response-body"></a>Response body

| Valeur      | Type    | Description                  |
|------------|---------|--------------------------------|
| Valeur      | tableau   | Tableau d’objets qui contiennent chacun une image de données de trace de pile. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs de la trace de pile](#stack-trace-values) ci-dessous. |
| @nextLink  | string  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10, mais que plus de 10 lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête.          |


### <a name="stack-trace-values"></a>Valeurs de la trace de pile

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur           | Type    | Description      |
|-----------------|---------|----------------|
| niveau            | string  |  Numéro de l’image représentant cet élément dans la pile d’appels.  |
| image   | string  |   Nom du fichier exécutable ou de l’image de bibliothèque qui contient la fonction appelée dans cette image de pile.           |
| function | string  |  Nom de la fonction appelée dans cette image de pile. Cette fonctionnalité n’est disponible que si votre application contient des symboles pour le fichier exécutable ou la bibliothèque.              |
| offset     | string  |  Décalage d’octet de l’instruction actuelle par rapport au début de la fonction.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport sur l’intégrité](../publish/health-report.md)
* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données de signalement d’erreurs pour votre application de bureau](get-desktop-application-error-reporting-data.md)
* [Obtenir les informations sur une erreur de votre application de bureau](get-details-for-an-error-in-your-desktop-application.md)
* [Télécharger le fichier CAB concernant une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)