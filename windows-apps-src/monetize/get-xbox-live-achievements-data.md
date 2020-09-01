---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer des données de résultats Xbox Live.
title: Obtenir des données de réussite Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics, réalisations
ms.localizationpriority: medium
ms.openlocfilehash: 7d4c031d967c48c65f0f9be7b386c11c12b7366e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162373"
---
# <a name="get-xbox-live-achievements-data"></a>Obtenir des données de réussite Xbox Live

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir le nombre de clients qui ont déverrouillé chaque réalisation de votre [jeu Xbox Live](/gaming/xbox-live/index.md) au cours de la journée la plus récente pour laquelle les données de résultats sont disponibles, les 30 jours précédents à ce jour et la durée de vie totale de votre jeu jusqu’à ce jour. Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox ou les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation du concept](../gaming/concept-approval.md), qui comprend les jeux publiés par les [partenaires Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis par le biais du [ ID@Xbox programme](/gaming/xbox-live/developer-program-overview.md#id). Actuellement, cette méthode ne prend pas en charge les jeux publiés par le biais du [programme Xbox Live Creators](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) pour l’API Microsoft Store Analytics.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de demande       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande


| Paramètre        | Type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | string | L' [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer les données des résultats Xbox Live.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **réalisations**.  |  Oui  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données de primes pour les clients qui ont déverrouillé les 10 premières réalisations pour votre jeu Xbox Live. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau d’objets qui contiennent des données pour chaque réalisation dans votre jeu. Pour plus d’informations sur les données de chaque objet, consultez le tableau suivant.                                                                                                                      |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 100, mais qu’il y a plus de 100 lignes de données pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête.  |


Les éléments du tableau *Value* comportent les valeurs suivantes :

| Valeur               | Type   | Description                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | ID de stockage du jeu pour lequel vous récupérez des données de résultats.     |
| reportDateTime     | string |  Date des données de résultats.    |
| achievementId          | nombre |  ID de la réalisation. |
| achievementName           | string | Nom de la réalisation.  |
| joueur           | nombre |  Récompense joueur pour la réussite.  |
| dailyUnlocks           | nombre |  Nombre de clients qui ont déverrouillé la prime le jour spécifié par *reportDateTime*.  |
| monthlyUnlocks              | nombre |  Nombre de clients qui ont déverrouillé la réalisation dans les 30 jours précédant la journée spécifiée par *reportDateTime*.   |
| totalUnlocks | nombre |  Le nombre de clients qui ont déverrouillé la réalisation pendant la durée de vie de votre jeu, jusqu’à la date spécifiée par *reportDateTime*.   |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir les données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)