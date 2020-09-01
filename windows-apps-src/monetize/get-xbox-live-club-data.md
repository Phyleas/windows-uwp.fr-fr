---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer des données de Club Xbox Live.
title: Obtenir des données de club Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics, clubs
ms.localizationpriority: medium
ms.openlocfilehash: 155e8a48f9e9e207709af4e55bb8e052b4419867
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162363"
---
# <a name="get-xbox-live-club-data"></a>Obtenir des données de club Xbox Live

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les données du club pour votre [jeu Xbox Live](/gaming/xbox-live/index.md). Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

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
| applicationId | string | L' [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer les données du Club Xbox Live.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **communitymanagerclub**.  |  Oui  |
| startDate | Date | Date de début dans la plage de dates des données du Club à récupérer. La valeur par défaut est de 30 jours avant la date actuelle. |  Non  |
| endDate | Date | Date de fin dans la plage de dates des données du Club à récupérer. La valeur par défaut est la date actuelle. |  Non  |
| top | int | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | int | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10000 et skip=0 pour obtenir les 10000 premières lignes de données, top=10000 et skip=10000 pour obtenir les 10000 lignes suivantes, et ainsi de suite. |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention des données du club pour votre jeu Xbox Live. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

| Valeur      | Type   | Description                  |
|------------|--------|-------------------------------------------------------|
| Valeur      | tableau  | Tableau qui contient un objet [ProductData](#productdata) qui contient des données pour les clubs associées à votre jeu et un objet [XboxwideData](#xboxwidedata) qui contient les données du club pour tous les clients Xbox Live. Ces données sont incluses à des fins de comparaison avec les données de votre jeu.  |
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est retournée si le paramètre **supérieur** de la demande est défini sur 10000, mais qu’il y a plus de 10000 lignes de données pour la requête. |
| TotalCount | int    | Nombre total de lignes dans les résultats de la requête. |


### <a name="productdata"></a>ProductData

Cette ressource contient les données du Club de votre jeu.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| Date            |  string |   Date des données du Club.   |
|  applicationId               |    string     |  [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous avez récupéré les données du Club.   |
|  clubsWithTitleActivity               |    int     |  Le nombre de clubs qui interagit avec votre jeu.   |     
|  clubsExclusiveToGame               |   int      |  Le nombre de clubs qui exercent une intervention sociale exclusivement avec votre jeu.   |     
|  clubFacts               |   tableau      |   Contient un ou plusieurs objets [ClubFacts](#clubfacts) sur chacun des clubs qui interagit avec votre jeu.   |


### <a name="xboxwidedata"></a>XboxwideData

Cette ressource contient la moyenne des données du club pour tous les clients Xbox Live.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| Date            |  string |   Date des données du Club.   |
|  applicationId  |    string     |   Dans l’objet **XboxwideData** , cette chaîne est toujours la valeur **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   int     |  En moyenne, le nombre de clubs avec des clients qui sont interactifs avec un jeu Xbox Live.    |     
|  clubsExclusiveToGame               |   int      |  En moyenne, le nombre de clubs qui exercent une intervention sociale exclusivement avec un jeu Xbox Live.   |     
|  clubFacts               |   object      |  Contient un objet [ClubFacts](#clubfacts) . Cet objet n’a pas de sens dans le contexte de l’objet **XboxwideData** et a des valeurs par défaut.  |


### <a name="clubfacts"></a>ClubFacts

Dans l’objet **ProductData** , cet objet contient des données pour un club spécifique qui a une activité liée à votre jeu. Dans l’objet **XboxwideData** , cet objet n’a pas de signification et a des valeurs par défaut.

| Valeur           | Type    | Description        |
|-----------------|---------|--------------------|
|  name            |  string  |   Dans l’objet **ProductData** , il s’agit du nom du Club. Dans l’objet **XboxwideData** , il s’agit toujours de la valeur **XBOXWIDE**.           |
|  memberCount               |    int     | Dans l’objet **ProductData** , il s’agit du nombre de membres du Club, à l’exclusion des non-membres qui se rendent juste au Club. Dans l’objet **XboxwideData** , il s’agit toujours de 0.    |
|  titleSocialActionsCount               |    int     |  Dans l’objet **ProductData** , il s’agit du nombre d’actions sociales effectuées par les membres du Club et qui sont liées à votre jeu. Dans l’objet **XboxwideData** , il s’agit toujours de 0   |
|  isExclusiveToGame               |    Boolean     |  Dans l’objet **ProductData** , cette valeur indique si le club actuel est à l’usage social exclusivement avec votre jeu. Dans l’objet **XboxwideData** , il s’agit toujours de la valeur true.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données d’analyse Xbox Live](get-xbox-live-analytics.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Recevoir des données du Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)