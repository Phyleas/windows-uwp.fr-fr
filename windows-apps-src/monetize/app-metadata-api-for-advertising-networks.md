---
author: mcleanbyron
description: "Découvrez comment utiliser l’API REST de métadonnées d’application pour accéder à certaines métadonnées de types pour les applications. Cette API est destinée à être utilisée par les réseaux publicitaires pour récupérer des informations sur les applications dans le Windows Store, et ce, afin d’améliorer la façon dont ils vendent l’espace publicitaire aux annonceurs."
title: "API de métadonnées d’application pour les réseaux publicitaires"
translationtype: Human Translation
ms.sourcegitcommit: 97f7d8e5ebc2df1752a182d5765b6a8b28b539e5
ms.openlocfilehash: e2b0680e62014526f684e7c1f7fd7da83a14f5d3

---

# API de métadonnées d’application pour les réseaux publicitaires

Les réseaux publicitaires peuvent utiliser *l’API des métadonnées d’application* pour récupérer par programme des métadonnées sur les applications dans le Windows Store, notamment des détails tels que la description et la catégorie pour les descriptions dans le Windows Store de l’application, ainsi que des informations sur le fait que l’application cible ou non des enfants de moins de 13ans. L’accès à cette API est actuellement limité aux développeurs bénéficiant d’une autorisation sur l’API accordée par Microsoft.

Cet article fournit des instructions sur la manière de demander l’accès à l’API à l’aide du [portail API de métadonnées d’application](https://admetadata.portal.azure-api.net/), d’obtenir votre clé d’abonnement pour accéder à l’API et d’appeler l’API.

## Demander l’accès

Les réseaux publicitaires peuvent demander l’accès à l’API de métadonnées d’application en suivant ces instructions:

1. Accédez à la page [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) du portail API des métadonnées d’application.
2. Entrez les informations requises, puis cliquez sur le bouton **S’inscrire**.
3. Sur le même site, cliquez sur l’onglet **Produits**, puis cliquez sur **Détails de l’application pour la publicité**.
4. Sur la page suivante, cliquez sur le bouton **S’abonner**. Cela envoie votre demande d’accès à l’API de métadonnées d’application à Microsoft.

Une fois votre demande envoyée, vous recevez un message électronique dans un délai de 24heures environ qui vous indique si votre demande a été accordée ou refusée.

<span id="get-key" />
## Obtenir votre clé d’abonnement

Si vous êtes autorisé à accéder à l’API de métadonnées d’application, suivez ces instructions pour obtenir votre clé d’abonnement. Vous devez transmettre cette clé dans l’en-tête de demande des appels à l’API.

1. Accédez à la page [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) du portail API des métadonnées d’application, et connectez-vous avec votre e-mail et votre mot de passe.
2. Cliquez sur votre nom dans le coin supérieur droit du site, puis cliquez sur **Profil**.
3. Dans la section **Vos abonnements** de la page, cliquez sur **Afficher** en regard de **Clé primaire**. Il s’agit de votre clé d’abonnement. Copiez la clé afin de pouvoir l’utiliser ultérieurement lorsque vous appellerez l’API.

<span id="call-the-api" />
## Appeler l’API

Une fois que vous avez votre clé d’abonnement, vous êtes prêt à appeler l’API à l’aide de la syntaxe HTTP REST à partir du langage de programmation de votre choix. Pour plus d’informations sur la syntaxe de l’API, voir la section [Syntaxe de l’API](#syntax) ci-dessous. Pour obtenir des exemples de code en C#, JavaScript, Python et plusieurs autres langages, cliquez sur l’onglet **API** du portail API des métadonnées d’application, cliquez sur **Détails de l’application**, puis consultez la section **Exemples de code** au bas de la page.

Par ailleurs, vous pouvez appeler l’API à l’aide de l’interface utilisateur fournie par le portail API de métadonnées d’application:
  1. Dans le portail, cliquez sur l’onglet **API**, puis cliquez sur **Détails de l’application**.
  2. Sur la page suivante, entrez la valeur [app_id](#request-parameters) de l’application pour laquelle vous souhaitez récupérer des métadonnées dans le champ **app_id**, puis entrez votre clé d’abonnement dans le champ **Ocp_Apim_Subscription-Key**.
  3. Cliquez sur **Envoyer**. La réponse s’affiche en bas de la page.


<span id="syntax" />
## Syntaxe de l’API

Cette méthode présente la syntaxe de requête suivante.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | chaîne | Obligatoire. La clé de l’abonnement que vous avez [récupérée à partir du portail API de métadonnées d’application](#get-key).  |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------|
| app_id | chaîne | Obligatoire. L’ID de l’application pour laquelle vous souhaitez récupérer des métadonnées. Les valeurs possibles sont les suivantes:<br/><br/><ul><li>L’ID Windows Store pour l’application. Exemple d’ID Windows Store: 9NBLGGH29DM8.</li><li>L’ID de produit (parfois appelé *ID de l’application*) pour une application créée à l’origine pour Windows8.x ou Windows Phone8.x. L’ID de produit est un GUID.</li></ul> |

<span/>

### Exemple de requête

L’exemple suivant montre comment récupérer les métadonnées pour une application ayant l’ID Windows Store 9NBLGGH29DM8.

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### Corps de la réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode.

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8","isLive":true,
    "iconUrls":["//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"],
    "type":"App"
}
```

Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la table suivante.

| Valeur      | Type   | Description    |
|------------|--------|--------------------|
| storeId           | chaîne  | ID Windows Store de l’application. Exemple d’ID Windows Store: 9NBLGGH29DM8.     |  
| nom           | chaîne  | Nom de l’application.   |
| description           | chaîne  | Description dans le Windows Store pour l’application.  |
| phoneStoreGuid           | chaîne  | L’ID de produit (Windows Phone8.x) pour l’application. Il s’agit d’un GUID.  |
| windowsStoreGuid           | chaîne  | L’ID de produit (Windows8.x) pour l’application. Il s’agit d’un GUID. |
| storeCategory           | chaîne  | La catégorie pour l’application dans le Windows Store. Pour les valeurs prises en charge, voir [Tableau des catégories et sous-catégories](../publish/category-and-subcategory-table.md) pour les applications du Windows Store.  |
| iabCategory           | chaîne  | La catégorie de contenu pour l’application, telle que définie par l’Interactive Advertising Bureau (IAB). Par exemple, **Actualités** ou **Sports**. Pour obtenir la liste des catégories de contenu, voir la page [IAB Tech Lab Content Taxonomy](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy) (Taxonomie du contenu du laboratoire technique IAB) sur le site web IAB.   |
| iabCategoryId           | chaîne  | L’ID de la catégorie de contenu pour l’application. Par exemple, **IAB12** est l’ID de la catégorie Actualités, et **IAB17** est l’ID de la catégorie Sports. Pour obtenir une liste des ID de catégorie de contenu, voir la section 5.1 du document [OpenRTB API Specification](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf) (Spécification API OpenRTB). |
| coppa           | booléen  | True si l’application s’adresse à des enfants âgés de moins de 13ans et a donc des obligations en vertu du Children’s Online Privacy Protection Act (COPPA), sinon, false.  |
| downloadUrl           | chaîne  | Le lien vers la description de l’application dans le Windows Store. Ce lien est au format ```https://www.microsoft.com/store/apps/<Store ID>```.  |
| iconUrls           | chaîne  |  Le chemin d’accès relatif aux URL d’icônes associées à cette application. Pour récupérer les icônes, faites précéder les URL de *http* ou *https*.  |
| type           | chaîne  | Une des chaînes suivantes:**Application** ou **Jeu**.  |

<span/>



 

 



<!--HONumber=Nov16_HO1-->

