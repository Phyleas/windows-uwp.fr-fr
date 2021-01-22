---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Si vous disposez d’un catalogue d’applications et de modules complémentaires, vous pouvez utiliser l’API de collection Microsoft Store et l’API d’achat Microsoft Store pour accéder aux informations de propriété de ces produits à partir de vos services.
title: Gérer les droits sur les produits à partir d’un service
ms.date: 01/21/2021
ms.topic: article
keywords: API de collection Windows 10, UWP, Microsoft Store, Microsoft Store achat d’API, afficher les produits, autoriser les produits
ms.localizationpriority: medium
ms.openlocfilehash: 7674a9b966510d914850e1fc8b2c8ca531f64a20
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668736"
---
# <a name="manage-product-entitlements-from-a-service"></a>Gérer les droits sur les produits à partir d’un service

Si vous disposez d’un catalogue d’applications et de modules complémentaires, vous pouvez utiliser l' *API de collection Microsoft Store* et l' *api d’achat Microsoft Store* pour accéder aux informations de droits de ces produits à partir de vos services. Un *droit représente le droit d’un client* d’utiliser une application ou un module complémentaire publié via l’Microsoft Store.

Ces API sont constituées des méthodes REST, qui sont conçues pour être utilisées par les développeurs dont les catalogues d’extensions sont pris en charge par les services multiplateformes. Ces API vous permettent d’effectuer les actions suivantes :

-   API de collection Microsoft Store : [requête pour les produits appartenant à un utilisateur](query-for-products.md) et [rapportant un produit consommable comme étant rempli](report-consumable-products-as-fulfilled.md).
-   API d’achat de Microsoft Store : [accorder un produit gratuit à un utilisateur](grant-free-products.md), [obtenir des abonnements pour un utilisateur](get-subscriptions-for-a-user.md)et [modifier l’état de facturation d’un abonnement pour un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> L’API de collection Microsoft Store et l’API d’achat utilisent l’authentification Azure Active Directory (Azure AD) pour accéder aux informations de possession du client. Pour utiliser ces API, vous (ou votre organisation) devez disposer d’un répertoire Azure AD et vous devez disposer d’une autorisation d' [administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour l’annuaire. Si vous utilisez déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD.

## <a name="overview"></a>Vue d’ensemble

Les étapes suivantes décrivent le processus de bout en bout pour l’utilisation de l’API de collection Microsoft Store et de l’API d’achat :

1.  [Configurez une application dans Azure ad](#step-1).
2.  [Associez votre ID d’application Azure ad à votre application dans l’espace partenaires](#step-2).
3.  Dans votre service, [créez Azure ad des jetons d’accès](#step-3) qui représentent votre identité d’éditeur.
4.  Dans votre application Windows cliente, [créez une clé d’ID de Microsoft Store](#step-4) qui représente l’identité de l’utilisateur actuel, puis repassez cette clé à votre service.

Ce processus de bout en bout implique deux composants logiciels qui effectuent différentes tâches :

* **Votre service**. Il s’agit d’une application qui s’exécute en toute sécurité dans le contexte de votre environnement d’entreprise et qui peut être implémentée à l’aide de n’importe quelle plateforme de développement de votre choix. Votre service est responsable de la création des jetons d’accès Azure AD nécessaires pour le scénario et de l’appel des URI REST pour l’API de collection Microsoft Store et l’API d’achat.
* **Votre application Windows cliente**. Il s’agit de l’application pour laquelle vous souhaitez accéder aux informations sur les droits des clients et les gérer (y compris les modules complémentaires de l’application). Cette application est chargée de créer les clés d’ID de Microsoft Store dont vous avez besoin pour appeler l’API de collection Microsoft Store et acheter l’API à partir de votre service.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Étape 1 : configurer une application dans Azure AD

Avant de pouvoir utiliser l’API de collection Microsoft Store ou l’API d’achat, vous devez créer une application Web Azure AD, récupérer l’ID de locataire et l’ID d’application de l’application, et générer une clé. L’application Web Azure AD représente le service à partir duquel vous souhaitez appeler l’API de collection Microsoft Store ou l’API d’achat. Vous avez besoin de l’ID de locataire, de l’ID d’application et de la clé pour générer Azure AD des jetons d’accès dont vous avez besoin pour appeler l’API.

1.  Si vous ne l’avez pas déjà fait, suivez les instructions de la procédure d' [intégration d’applications avec Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) pour inscrire une application **Web/API** avec Azure ad.
    > [!NOTE]
    > Lorsque vous inscrivez votre application, vous devez choisir application **Web/API** comme type d’application afin de pouvoir récupérer une clé (également appelée clé *secrète client*) pour votre application. Pour appeler l’API de collection Microsoft Store ou l’API d’achat, vous devez fournir une clé secrète client lorsque vous demandez un jeton d’accès à partir d’Azure AD dans une étape ultérieure.

2.  Dans le [portail de gestion Azure](https://portal.azure.com/), accédez à **Azure Active Directory**. Sélectionnez votre annuaire, cliquez sur **inscriptions d’applications** dans le volet de navigation gauche, puis sélectionnez votre application.
3.  Vous accédez à la page d’inscription principale de l’application. Dans cette page, copiez la valeur ID de l' **application** pour l’utiliser ultérieurement.
4.  Créez une clé dont vous aurez besoin plus tard (il s’agit d’une clé *secrète client*). Dans le volet gauche, cliquez sur **paramètres** , puis sur **clés**. Dans cette page, suivez les étapes pour [créer une clé](/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copiez cette clé pour une utilisation ultérieure.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Étape 2 : associer votre ID d’application Azure AD à votre application cliente dans l’espace partenaires

Avant de pouvoir utiliser l’API de collection Microsoft Store ou l’API achat pour configurer la propriété et les achats de votre application ou du module complémentaire, vous devez associer votre ID d’application Azure AD à l’application (ou à l’application qui contient le module complémentaire) dans l’espace partenaires.

> [!NOTE]
> Vous ne devez effectuer cette tâche qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID d’application et la clé secrète client, vous pouvez réutiliser ces valeurs chaque fois que vous avez besoin de créer un nouveau Azure AD jeton d’accès.

1.  Connectez-vous à l' [espace partenaires](https://partner.microsoft.com/dashboard) , puis sélectionnez votre application.
2.  Accédez à la page **services** &gt; **et regroupements de produits** , puis entrez votre ID d’application Azure ad dans l’un des champs **ID client** disponibles.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Étape 3 : créer des jetons d’accès Azure AD

Avant de pouvoir récupérer une clé d’ID de Microsoft Store ou d’appeler l’API de collection Microsoft Store ou l’API d’achat, votre service doit créer plusieurs jetons d’accès Azure AD qui représentent votre identité d’éditeur. Chaque jeton sera utilisé avec une API différente. La durée de vie de chacun des jetons est de 60 minutes, et vous pouvez les actualiser une fois qu’ils sont arrivés à expiration.

> [!IMPORTANT]
> Créez Azure AD jetons d’accès uniquement dans le contexte de votre service, et non dans votre application. Votre clé secrète client risque d’être compromise si elle est envoyée à votre application.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Comprendre les différents jetons et URI d’audience

Selon les méthodes que vous souhaitez appeler dans le Microsoft Store API de collection ou l’API d’achat, vous devez créer deux ou trois jetons différents. Chaque jeton d’accès est associé à un URI d’audience différent.

  * Dans tous les cas, vous devez créer un jeton avec l' `https://onestore.microsoft.com` URI d’audience. Dans une étape ultérieure, vous allez passer ce jeton à l’en-tête d' **autorisation** des méthodes dans l’API de collection Microsoft Store ou l’API d’achat.
      > [!IMPORTANT]
      > Utilisez l' `https://onestore.microsoft.com` audience uniquement avec les jetons d’accès stockés en toute sécurité dans votre service. L’exposition des jetons d’accès avec cette audience en dehors de votre service peut rendre votre service vulnérable aux attaques par relecture.

  * Si vous souhaitez appeler une méthode dans l’API de collection Microsoft Store pour [Rechercher des produits détenus par un utilisateur](query-for-products.md) ou [signaler un produit consommable comme étant rempli](report-consumable-products-as-fulfilled.md), vous devez également créer un jeton avec l' `https://onestore.microsoft.com/b2b/keys/create/collections` URI d’audience. Dans une étape ultérieure, vous allez passer ce jeton à une méthode cliente dans le SDK Windows pour demander une clé d’ID de Microsoft Store que vous pouvez utiliser avec l’API de collection Microsoft Store.

  * Si vous souhaitez appeler une méthode dans l’API Microsoft Store acheter pour [accorder un produit gratuit à un utilisateur](grant-free-products.md), [obtenir des abonnements](get-subscriptions-for-a-user.md)à un utilisateur ou [modifier l’état de facturation d’un abonnement pour un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md), vous devez également créer un jeton avec l' `https://onestore.microsoft.com/b2b/keys/create/purchase` URI d’audience. Dans une étape ultérieure, vous allez passer ce jeton à une méthode cliente dans le SDK Windows pour demander une clé d’ID de Microsoft Store que vous pouvez utiliser avec l’API d’achat Microsoft Store.

<span />

### <a name="create-the-tokens"></a>Créer les jetons

Pour créer les jetons d’accès, utilisez l’API OAuth 2,0 dans votre service en suivant les instructions des [appels de service à service à l’aide des informations d’identification du client](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) pour envoyer une requête HTTP Après au ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` point de terminaison. Voici un exemple de requête.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Pour chaque jeton, spécifiez les données de paramètre suivantes :

* Pour les *paramètres \_ ID client* et *\_ clé secrète client* , spécifiez l’ID d’application et la clé secrète client pour votre application que vous avez récupérée à partir de la [portail de gestion Azure](https://portal.azure.com/). Ces deux paramètres sont requis pour créer un jeton d’accès avec le niveau d’authentification requis par l’API de collection Microsoft Store ou l’API d’achat.

* Pour le paramètre de *ressource* , spécifiez l’un des URI d’audience répertoriés dans la [section précédente](#access-tokens), en fonction du type de jeton d’accès que vous créez.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens). Pour plus d’informations sur la structure d’un jeton d’accès, voir [Jeton et types de réclamations pris en charge](/azure/active-directory/develop/id-tokens).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Étape 4 : créer une clé d’ID de Microsoft Store

Avant de pouvoir appeler une méthode dans l’API de collection Microsoft Store ou l’API d’achat, votre application doit créer une clé d’ID de Microsoft Store et l’envoyer à votre service. Cette clé est un JSON Web Token (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété du produit. Pour plus d’informations sur les revendications dans cette clé, consultez [revendications dans une clé d’ID de Microsoft Store](#claims-in-a-microsoft-store-id-key).

Actuellement, la seule façon de créer une clé d’ID de Microsoft Store consiste à appeler une API plateforme Windows universelle (UWP) à partir du code client dans votre application. La clé générée représente l’identité de l’utilisateur actuellement connecté au Microsoft Store sur l’appareil.

> [!NOTE]
> Chaque clé d’ID de Microsoft Store est valide pour 90 jours. Après l’expiration d’une clé, vous pouvez [la renouveler](renew-a-windows-store-id-key.md). Nous vous recommandons de renouveler vos clés d’ID de Microsoft Store plutôt que d’en créer de nouvelles.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Pour créer une clé d’ID de Microsoft Store pour l’API de collection Microsoft Store

Procédez comme suit pour créer une clé d’ID de Microsoft Store que vous pouvez utiliser avec l’API de regroupement Microsoft Store pour [Rechercher des produits détenus par un utilisateur](query-for-products.md) ou [signaler un produit consommable comme étant rempli](report-consumable-products-as-fulfilled.md).

1.  Transmettez le jeton d’accès Azure AD qui a la valeur `https://onestore.microsoft.com/b2b/keys/create/collections` d’URI d’audience de votre service à votre application cliente. Il s’agit de l’un des jetons que vous avez créés [précédemment à l’étape 3](#step-3).

2.  Dans votre code d’application, appelez l’une des méthodes suivantes pour récupérer une clé d’ID de Microsoft Store :

  * Si votre application utilise la classe [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour gérer les achats dans l’application, utilisez la méthode [StoreContext. GetCustomerCollectionsIdAsync](/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) .

  * Si votre application utilise la classe [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) pour gérer les achats dans l’application, utilisez la méthode [CurrentApp. GetCustomerCollectionsIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) .

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Si vous conservez des ID d’utilisateur anonymes dans le contexte des services que vous gérez en tant que serveur de publication de l’application actuelle, vous pouvez également transmettre un ID utilisateur au paramètre *publisherUserId* pour associer l’utilisateur actuel à la nouvelle clé d’id de Microsoft Store (l’ID utilisateur sera incorporé dans la clé). Dans le cas contraire, si vous n’avez pas besoin d’associer un ID utilisateur à la clé d’ID de Microsoft Store, vous pouvez transmettre n’importe quelle valeur de chaîne au paramètre *publisherUserId* .

3.  Une fois que votre application a créé une clé d’ID de Microsoft Store, transmettez la clé à votre service.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Pour créer une clé d’ID de Microsoft Store pour l’API d’achat Microsoft Store

Suivez ces étapes pour créer une clé d’ID de Microsoft Store que vous pouvez utiliser avec l’API d’achat Microsoft Store pour [accorder un produit gratuit à un utilisateur](grant-free-products.md), [obtenir des abonnements à un utilisateur](get-subscriptions-for-a-user.md)ou [modifier l’état de facturation d’un abonnement pour un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Transmettez le jeton d’accès Azure AD qui a la valeur `https://onestore.microsoft.com/b2b/keys/create/purchase` d’URI d’audience de votre service à votre application cliente. Il s’agit de l’un des jetons que vous avez créés [précédemment à l’étape 3](#step-3).

2.  Dans votre code d’application, appelez l’une des méthodes suivantes pour récupérer une clé d’ID de Microsoft Store :

  * Si votre application utilise la classe [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) dans l’espace de noms [Windows. services. Store](/uwp/api/windows.services.store) pour gérer les achats dans l’application, utilisez la méthode [StoreContext. GetCustomerPurchaseIdAsync](/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) .

  * Si votre application utilise la classe [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans l’espace de noms [Windows. ApplicationModel. Store](/uwp/api/windows.applicationmodel.store) pour gérer les achats dans l’application, utilisez la méthode [CurrentApp. GetCustomerPurchaseIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) .

    Transmettez votre jeton d’accès Azure AD au paramètre *serviceTicket* de la méthode. Si vous conservez des ID d’utilisateur anonymes dans le contexte des services que vous gérez en tant que serveur de publication de l’application actuelle, vous pouvez également transmettre un ID utilisateur au paramètre *publisherUserId* pour associer l’utilisateur actuel à la nouvelle clé d’id de Microsoft Store (l’ID utilisateur sera incorporé dans la clé). Dans le cas contraire, si vous n’avez pas besoin d’associer un ID utilisateur à la clé d’ID de Microsoft Store, vous pouvez transmettre n’importe quelle valeur de chaîne au paramètre *publisherUserId* .

3.  Une fois que votre application a créé une clé d’ID de Microsoft Store, transmettez la clé à votre service.

### <a name="diagram"></a>Diagramme

Le diagramme suivant illustre le processus de création d’une clé d’ID de Microsoft Store.

  ![Créer une clé d’ID du Windows Store](images/b2b-1.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Revendications dans une clé d’ID de Microsoft Store

Une clé d’ID de Microsoft Store est un JSON Web Token (JWT) qui représente l’identité de l’utilisateur dont vous souhaitez accéder aux informations de propriété du produit. Lorsqu’elle est décodée à l’aide de base64, une clé d’ID de Microsoft Store contient les revendications suivantes.

* `iat`: &nbsp; &nbsp; &nbsp; Identifie l’heure à laquelle la clé a été émise. Cette revendication permet de déterminer l’âge du jeton. Cette valeur est exprimée en temps époque.
* `iss`: &nbsp; &nbsp; &nbsp; Identifie l’émetteur. La valeur est la même que celle de la `aud` revendication.
* `aud`: &nbsp; &nbsp; &nbsp; Identifie l’audience. Doit être définie sur l’une des valeurs suivantes : `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`: &nbsp; &nbsp; &nbsp; Identifie l’heure d’expiration à laquelle la clé ne sera plus acceptée pour traiter quoi que ce soit, sauf pour le renouvellement des clés. La valeur de cette revendication est exprimée en temps époque.
* `nbf`: &nbsp; &nbsp; &nbsp; Identifie l’heure à laquelle le jeton sera accepté pour le traitement. La valeur de cette revendication est exprimée en temps époque.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`: &nbsp; &nbsp; &nbsp; ID client qui identifie le développeur.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`: &nbsp; &nbsp; &nbsp; Une charge utile opaque (chiffrée et encodée en base64) qui contient des informations destinées uniquement à une utilisation par les services Microsoft Store.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`: &nbsp; &nbsp; &nbsp; ID d’utilisateur qui identifie l’utilisateur actuel dans le contexte de vos services. Il s’agit de la même valeur que celle que vous transmettez dans le paramètre facultatif *publisherUserId* de la [méthode que vous utilisez pour créer la clé](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`: &nbsp; &nbsp; &nbsp; URI que vous pouvez utiliser pour renouveler la clé.

Voici un exemple d’un en-tête de clé ID Microsoft Store décodé.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Voici un exemple de jeu de revendications de clé d’ID Microsoft Store décodé.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Obtenir les abonnements d’un utilisateur](get-subscriptions-for-a-user.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renouveler une clé d’ID du Microsoft Store](renew-a-windows-store-id-key.md)
* [Intégration d’applications dans Azure Active Directory](/azure/active-directory/develop/quickstart-register-app)
* [Connaître le manifeste d’application Azure Active Directory]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Types de jeton et de revendication pris en charge](/azure/active-directory/develop/id-tokens)
