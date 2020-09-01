---
description: Utilisez cette méthode dans l’API Microsoft Store Analytics pour récupérer des données Xbox Live Analytics.
title: Obtenir des données d’analyse Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, services Store, API Microsoft Store Analytics, Xbox Live Analytics
ms.localizationpriority: medium
ms.openlocfilehash: 5649a81e1c6e869e9e010841cb31432350bb33d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172883"
---
# <a name="get-xbox-live-analytics-data"></a>Obtenir des données d’analyse Xbox Live

Utilisez cette méthode dans l’API Microsoft Store Analytics pour obtenir les 30 derniers jours de données d’analyse générale pour les clients qui lisent votre [jeu Xbox Live](/gaming/xbox-live/index.md), y compris l’utilisation des accessoires d’appareils, le type de connexion Internet, la distribution joueur, les statistiques de jeu, ainsi que les données des amis et des abonnés. Ces informations sont également disponibles dans le [rapport Xbox Analytics](../publish/xbox-analytics-report.md) dans l’espace partenaires.

> [!IMPORTANT]
> Cette méthode prend uniquement en charge les jeux pour Xbox ou les jeux qui utilisent les services Xbox Live. Ces jeux doivent passer par le [processus d’approbation du concept](../gaming/concept-approval.md), qui comprend les jeux publiés par les [partenaires Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) et les jeux soumis par le biais du [ ID@Xbox programme](/gaming/xbox-live/developer-program-overview.md#id). Actuellement, cette méthode ne prend pas en charge les jeux publiés par le biais du [programme Xbox Live Creators](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

Des données analytiques supplémentaires pour les jeux Xbox Live sont disponibles par le biais des méthodes suivantes :
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Obtenir les données du hub de jeux Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtenir les données d’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)

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
| applicationId | string | [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous souhaitez récupérer les données de Xbox Live Analytics générales.  |  Oui  |
| metricType | string | Chaîne qui spécifie le type de données Xbox Live Analytics à récupérer. Pour cette méthode, spécifiez la valeur **ProductValues**.  |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre une demande d’obtention de données d’analyse générales pour les clients qui lisent votre jeu Xbox Live. Remplacez la valeur *ApplicationID* par l’ID du magasin de votre jeu.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

Cette méthode retourne un tableau de *valeurs* qui contient les objets suivants.

| Object      | Description                  |
|-------------|---------------------------------------------------|
| ProductData   |   Contient un objet [DeviceProperties](#deviceproperties) et un objet [UserProperties](#userproperties) qui contiennent les 30 derniers jours de données d’analyse des appareils et des utilisateurs pour votre jeu.    |  
| XboxwideData   |  Contient un objet [DeviceProperties](#deviceproperties) et un objet [UserProperties](#userproperties) qui contiennent les 30 derniers jours de données d’analyse d’appareil et d’utilisateur moyennes pour tous les clients Xbox Live, sous forme de pourcentages. Ces données sont incluses à des fins de comparaison avec les données de votre jeu.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

Cette ressource contient des données d’utilisation de l’appareil pour votre jeu ou des données d’utilisation de périphérique moyennes pour tous les clients Xbox Live au cours des 30 derniers jours.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  applicationId               |    string     |  [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous avez récupéré les données d’analyse.   |
|  connectionTypeDistribution               |    tableau     |   Contient des objets qui indiquent le nombre de clients qui utilisent une connexion Internet câblée par rapport à une connexion Internet sans fil sur Xbox. Chaque objet a deux champs de chaîne : <ul><li>**contype**: spécifie le type de connexion.</li><li>**deviceCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui utilisent le type de connexion. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui utilisent le type de connexion.</li></ul>   |     
|  deviceCount               |   string      |  Dans l’objet **ProductData** , ce champ spécifie le nombre d’appareils clients sur lesquels votre jeu a été lu au cours des 30 derniers jours. Dans l’objet **XboxwideData** , ce champ a toujours la valeur 1, ce qui indique un pourcentage de départ de 100% pour les données de tous les clients Xbox Live.   |     
|  eliteControllerPresentDeviceCount               |   string      |  Dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui utilisent le contrôleur sans fil Xbox Elite. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui utilisent le contrôleur sans fil Xbox Elite.  |     
|  externalDrivePresentDeviceCount               |   string      |  Dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui utilisent un disque dur externe sur Xbox. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui utilisent un disque dur externe sur Xbox.  |


### <a name="userproperties"></a>UserProperties

Cette ressource contient des données utilisateur pour votre jeu ou des données utilisateur moyennes pour tous les clients Xbox Live au cours des 30 derniers jours.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  applicationId               |    string     |   [ID de stockage](in-app-purchases-and-trials.md#store-ids) du jeu pour lequel vous avez récupéré les données d’analyse.  |
|  userCount               |    string     |   Dans l’objet **ProductData** , ce champ spécifie le nombre de clients qui ont joué votre jeu au cours des 30 derniers jours. Dans l’objet **XboxwideData** , ce champ a toujours la valeur 1, ce qui indique un pourcentage de départ de 100% pour les données de tous les clients Xbox Live.   |     
|  dvrUsageCounts               |   tableau      |  Contient des objets qui indiquent combien de clients ont utilisé Game DVR pour enregistrer et afficher le jeu. Chaque objet a deux champs de chaîne : <ul><li>**dvrName**: spécifie la fonctionnalité DVR du jeu utilisée. Les valeurs possibles sont **gameClipUploads**, **gameClipViews**, **screenshotUploads**et **screenshotViews**.</li><li>**userCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui ont utilisé la fonctionnalité de jeux DVR spécifiée. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui ont utilisé la fonctionnalité de jeux DVR.</li></ul>   |     
|  followerCountPercentiles               |   tableau      |  Contient des objets qui fournissent des détails sur le nombre d’abonnés pour les clients. Chaque objet a deux champs de chaîne : <ul><li>**pourcentage**: actuellement, cette valeur est toujours 50, ce qui indique que les données du suiveur sont fournies comme une valeur médiane.</li><li>**valeur**: dans l’objet **ProductData** , ce champ spécifie le nombre médian d’abonnés pour les clients de votre jeu. Dans l’objet **XboxwideData** , ce champ spécifie le nombre médian d’abonnés pour tous les clients Xbox Live.</li></ul>  |   
|  friendCountPercentiles               |   tableau      |  Contient des objets qui fournissent des détails sur le nombre d’amis pour les clients. Chaque objet a deux champs de chaîne : <ul><li>**pourcentage**: actuellement, cette valeur est toujours 50, ce qui indique que les données des amis sont fournies en tant que valeur médiane.</li><li>**valeur**: dans l’objet **ProductData** , ce champ spécifie le nombre médian d’amis pour les clients de votre jeu. Dans l’objet **XboxwideData** , ce champ spécifie le nombre médian d’amis pour tous les clients Xbox Live.</li></ul>  |     
|  gamerScoreRangeDistribution               |   tableau      |  Contient des objets qui fournissent des détails sur la distribution joueur pour les clients. Chaque objet a deux champs de chaîne : <ul><li>**scoreRange**: plage joueur pour laquelle le champ suivant fournit des données d’utilisation. Par exemple, **10 000 Ko**.</li><li>**userCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui ont un joueur dans la plage spécifiée pour tous les jeux qu’ils ont lus. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui ont un joueur dans la plage spécifiée pour tous les jeux qu’ils ont joués.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   tableau      |  Contient des objets qui fournissent des détails sur la distribution joueur pour votre jeu. Chaque objet a deux champs de chaîne : <ul><li>**scoreRange**: plage joueur pour laquelle le champ suivant fournit des données d’utilisation. Par exemple, **100-200**.</li><li>**userCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui ont un joueur dans la plage spécifiée pour votre jeu. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui ont un joueur dans la plage spécifiée pour votre jeu.</li></ul>   |
|  socialUsageCounts               |   tableau      |  Contient des objets qui fournissent des détails sur l’utilisation sociale pour les clients. Chaque objet a deux champs de chaîne : <ul><li>**scName**: type d’utilisation sociale. Par exemple, **gameInvites** et **textMessages**.</li><li>**userCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui ont participé au type d’utilisation sociale spécifié. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui ont participé au type d’utilisation sociale spécifié.</li></ul>   |
|  streamingUsageCounts               |   tableau      |  Contient des objets qui fournissent des détails sur l’utilisation de la diffusion en continu pour les clients. Chaque objet a deux champs de chaîne : <ul><li>**stName**: type de plateforme de streaming. Par exemple, **youtubeUsage**, **twitchUsage**et **mixerUsage**.</li><li>**userCount**: dans l’objet **ProductData** , ce champ spécifie le nombre de clients de votre jeu qui ont utilisé la plateforme de diffusion en continu spécifiée. Dans l’objet **XboxwideData** , ce champ spécifie le pourcentage de tous les clients Xbox Live qui ont utilisé la plateforme de diffusion en continu spécifiée.</li></ul>  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Accéder aux données d’analyse à l’aide des services Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données de réussite Xbox Live](get-xbox-live-achievements-data.md)
* [Obtenir des données d’intégrité Xbox Live](get-xbox-live-health-data.md)
* [Recevoir des données du Hub de jeu Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtenir des données de club Xbox Live](get-xbox-live-club-data.md)
* [Obtenir des données multijoueur Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtenir les données d’utilisation simultanée Xbox Live](get-xbox-live-concurrent-usage-data.md)