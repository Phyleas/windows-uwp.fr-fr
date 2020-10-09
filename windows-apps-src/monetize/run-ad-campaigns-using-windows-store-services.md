---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Utilisez l’API Microsoft Store promotions pour gérer par programme les campagnes publicitaires promotionnelles pour les applications qui sont inscrites auprès de votre compte espace partenaires de votre organisation ou de votre organisation.
title: Exécuter des campagnes Active Directory à l’aide des services Store
ms.date: 06/04/2018
ms.topic: article
keywords: API de promotion Windows 10, UWP, Microsoft Store, campagnes ad
ms.localizationpriority: medium
ms.openlocfilehash: 2be721137e6c09913eafd2c58bab07f1ae6f2728
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878512"
---
# <a name="run-ad-campaigns-using-store-services"></a>Exécuter des campagnes Active Directory à l’aide des services Store

Utilisez l' *API Microsoft Store promotions* pour gérer par programme les campagnes publicitaires promotionnelles pour les applications qui sont inscrites auprès de votre compte espace partenaires de votre organisation ou de votre organisation. Cette API vous permet de créer, mettre à jour et surveiller vos campagnes et d’autres ressources associées, telles que le ciblage et les éléments créatifs. Cette API est particulièrement utile pour les développeurs qui créent de grands volumes de campagnes et qui souhaitent le faire sans l’aide de l’espace partenaires. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API Microsoft Store promotions, [obtenez un jeton d’accès Azure ad](#obtain-an-azure-ad-access-token). Une fois que vous avez obtenu un jeton, vous avez 60 minutes pour utiliser ce jeton dans les appels à l’API Microsoft Store promotions avant l’expiration du jeton. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API Microsoft Store promotions](#call-the-windows-store-promotions-api).

Vous pouvez également créer et gérer des campagnes Active Directory à l’aide de l’espace partenaires. vous pouvez également accéder aux campagnes de publicité que vous créez par programme par le biais de l’API Microsoft Store promotions dans l’espace partenaires. Pour plus d’informations sur la gestion des campagnes Active Directory dans l’espace partenaires, consultez [créer une campagne publicitaire pour votre application](./index.md).

> [!NOTE]
> Tout développeur disposant d’un compte espace partenaires peut utiliser l’API Microsoft Store promotions pour gérer les campagnes Active Directory pour ses applications. Les agences multimédias peuvent également demander l’accès à cette API pour exécuter des campagnes AD pour le compte de leurs annonceurs. Si vous êtes une Agence multimédia qui souhaite en savoir plus sur cette API ou en demander l’accès, envoyez votre demande à storepromotionsapi@microsoft.com .

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Étape 1 : remplir les conditions préalables à l’utilisation de l’API Microsoft Store promotions

Avant de commencer à écrire du code pour appeler l’API de promotions Microsoft Store, assurez-vous que vous avez rempli les conditions préalables suivantes.

* Avant de pouvoir créer et démarrer correctement une campagne publicitaire à l’aide de cette API, vous devez [d’abord créer une campagne publicitaire payante à l’aide de la page **campagnes publicitaires** dans l’espace partenaires](./index.md). vous devez ajouter au moins un instrument de paiement sur cette page. Après cela, vous serez en mesure de créer avec succès des lignes de livraison facturables pour les campagnes Active Directory à l’aide de cette API. Les lignes de livraison des campagnes de publicité que vous créez à l’aide de cette API facturent automatiquement l’instrument de paiement par défaut choisi dans la page **campagnes publicitaires** de l’espace partenaires.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et de l’autorisation [Administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) sur l’annuaire. Si vous utilisez déjà Microsoft 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouveau Azure ad dans l’espace partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD représente l’application ou le service à partir duquel vous souhaitez appeler l’API Microsoft Store promotions. Il vous faut l’ID tenant, l’ID client et la clé pour obtenir un jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Vous ne devez effectuer cette tâche qu’une seule fois. Une fois que vous les avez, vous pouvez réutiliser l’ID tenant, l’ID client et la clé chaque fois que vous devez créer un jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte espace partenaires et récupérer les valeurs requises :

1.  Dans l’Espace partenaires, [associez le compte Espace partenaires de votre organisation à l’annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, dans la page **utilisateurs** de la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour gérer les campagnes de promotion de votre compte espace partenaires. Veillez à attribuer à cette application le rôle **Gestionnaire**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une application Azure AD dans l’Espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder à ses paramètres, puis copiez les valeurs **ID tenant** et **ID client**.

4. Cliquez sur **Ajouter une nouvelle clé**. Sur l’écran suivant, copiez la valeur **Clé**. Vous ne pourrez plus accéder à cette information une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes de l’API Microsoft Store promotions, vous devez d’abord obtenir un jeton d’accès Azure AD que vous transmettez à l’en-tête **authorization** de chaque méthode de l’API. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la *valeur \_ ID de locataire* dans l’URI de publication et les paramètres * \_ ID client* et clé * \_ secrète client* , spécifiez l’ID de locataire, l’ID client et la clé de votre application que vous avez récupéré dans l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens).

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Étape 3 : appeler l’API Microsoft Store promotions

Une fois que vous disposez d’un jeton d’accès Azure AD, vous êtes prêt à appeler l’API Microsoft Store promotions. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

Dans le contexte de l’API de promotion Microsoft Store, une campagne publicitaire se compose d’un objet *campagne* qui contient des informations de haut niveau sur la campagne, et d’objets supplémentaires qui représentent les *lignes de livraison*, les *profils de ciblage*et les *éléments créatifs* pour la campagne publicitaire. L’API comprend différents jeux de méthodes qui sont regroupés en fonction de ces types d’objets. Pour créer une campagne, vous appelez généralement une méthode de publication différente pour chacun de ces objets. L’API fournit également des méthodes d’extraction que vous pouvez utiliser pour récupérer des objets et des méthodes PUT que vous pouvez utiliser pour modifier les objets de la campagne, de la ligne de livraison et du ciblage.

Pour plus d’informations sur ces objets et leurs méthodes associées, consultez le tableau suivant.


| Object       | Description   |
|---------------|-----------------|
| Campagnes |  Cet objet représente la campagne Active Directory et se trouve en haut de la hiérarchie du modèle objet pour les campagnes Active Directory. Cet objet identifie le type de campagne que vous exécutez (payant, maison ou communauté), l’objectif de campagne, les lignes de livraison pour la campagne et d’autres détails. Chaque campagne peut être associée à une seule application.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, consultez [gérer des campagnes ad](manage-ad-campaigns.md).<br/><br/>**Note** &nbsp; Remarque &nbsp; Après avoir créé une campagne publicitaire, vous pouvez récupérer les données de performances de la campagne à l’aide de la méthode obtenir les données de performance de la [campagne Active Directory](get-ad-campaign-performance-data.md) dans l' [API Microsoft Store Analytics](access-analytics-data-using-windows-store-services.md).  |
| Lignes de livraison | Chaque campagne possède une ou plusieurs lignes de livraison qui sont utilisées pour acheter un inventaire et livrer vos annonces. Pour chaque ligne de livraison, vous pouvez définir le ciblage, définir le prix de votre offre et déterminer le montant que vous souhaitez dépenser en définissant un budget et en établissant un lien vers les éléments créatifs que vous souhaitez utiliser.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, consultez [gérer les lignes de livraison pour les campagnes de publicité](manage-delivery-lines-for-ad-campaigns.md). |
| Ciblage des profils | Chaque ligne de livraison a un profil de ciblage qui spécifie les utilisateurs, les zones géographiques et les types d’inventaire que vous souhaitez cibler. Les profils de ciblage peuvent être créés en tant que modèle et partagés entre les lignes de livraison.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, consultez [gérer les profils de ciblage pour les campagnes Active Directory](manage-targeting-profiles-for-ad-campaigns.md). |
| Éléments créatifs | Chaque ligne de livraison comporte une ou plusieurs éléments créatifs qui représentent les publicités présentées aux clients dans le cadre de la campagne. Un élément créatif peut être associé à une ou plusieurs lignes de livraison, même dans des campagnes ad, à condition qu’il représente toujours la même application.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, consultez [gérer les éléments créatifs pour les campagnes ad](manage-creatives-for-ad-campaigns.md). |


Le diagramme suivant illustre la relation entre les campagnes, les lignes de livraison, les profils de ciblage et les éléments créatifs.

![Hiérarchie des campagnes Active Directory](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès Azure AD et appeler l’API Microsoft Store promotions à partir d’une application console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple nécessite que le [package JSON.net](https://www.newtonsoft.com/json) de Newtonsoft désérialise les données JSON retournées par l’API de promotions Microsoft Store.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Promotions/cs/Program.cs" id="PromotionsApiExample":::

## <a name="related-topics"></a>Rubriques connexes

* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les lignes de livraison pour les campagnes de publicité](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les éléments créatifs pour les campagnes ad](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md)


 