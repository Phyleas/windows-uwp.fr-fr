---
description: En plus de créer une campagne publicitaire pour votre application qui s’exécutera dans des applications Windows, vous pouvez promouvoir votre application à l’aide d’autres canaux.
title: Créer une campagne personnalisée de promotion d’applications
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, personnalisé, application, promotion, campagne
ms.localizationpriority: medium
ms.openlocfilehash: c04a9705797e69543e9aec8f4e6205c815f76800
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878432"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Créer une campagne personnalisée de promotion d’applications

En plus de créer une [campagne publicitaire pour votre application](../monetize/index.md) qui s’exécutera dans des applications Windows, vous pouvez également promouvoir votre application à l’aide d’autres canaux. Par exemple, vous pouvez promouvoir votre application en faisant appel à un fournisseur de commercialisation d’applications tiers, ou en publiant des liens vers votre application sur des médias sociaux. Ces activités sont appelées *campagnes personnalisées*.

Si vous exécutez des campagnes personnalisées pour votre application, vous pouvez suivre les performances relatives de chaque campagne en créant une URL différente pour chaque campagne personnalisée, où chaque URL contient un *ID de campagne*différent. Quand un client exécutant Windows 10 clique sur une URL qui contient un ID de campagne, Microsoft associe le clic à la campagne personnalisée correspondante et met ces données à votre disposition dans l' [espace partenaires](https://partner.microsoft.com/dashboard).

> [!IMPORTANT]
> Ces données sont suivies uniquement pour les clients sur Windows 10. Les clients utilisant d’autres systèmes d’exploitation peuvent suivre le lien vers la description de votre application, mais les données sur les activités de ces clients ne seront pas incluses.

Il existe deux principaux types de données associés aux campagnes personnalisées : les *affichages de pages* pour la liste des magasins de votre application et les *conversions*. Une conversion est une acquisition d’application qui résulte d’un client qui consulte la page de liste de magasins de votre application à partir d’une URL qui contient un ID de campagne personnalisé. Pour plus d’informations sur les conversions, consultez [comprendre comment les acquisitions d’application sont qualifiées de conversions](#understanding-how-acquisitions-qualify-as-conversions) dans cette rubrique.

Pour récupérer les données de performances de campagne personnalisée de votre application, procédez comme suit :

* Vous pouvez afficher des données sur les affichages de pages et les conversions de votre application ou du module complémentaire à partir des **vues de pages d’application et des conversions par ID de campagne** et les graphiques de **conversions de campagnes totales** dans le [rapport acquisitions](acquisitions-report.md).
* Si votre application est une application plateforme Windows universelle (UWP), vous pouvez utiliser les API du SDK Windows pour récupérer par programmation l’ID de campagne personnalisé qui a abouti à une conversion.

## <a name="example-custom-campaign-scenario"></a>Exemple de scénario de campagne personnalisée

Imaginez un développeur de jeu venant d’achever la création d’un jeu et désireux de promouvoir celui-ci auprès des joueurs de ses jeux existants. Elle publie l’annonce de la nouvelle version du jeu sur sa page Facebook, y compris un lien vers la liste des boutiques du jeu. La plupart de ses joueurs suivent également sur Twitter, donc elle présente également une annonce avec le lien vers la liste des boutiques du jeu.

Pour assurer le suivi de la réussite de chacun de ces canaux de promotion, le développeur crée deux variantes de l’URL à la liste de la boutique du jeu :

* L’URL sur laquelle elle sera publiée sur sa page Facebook comprend l’ID de la campagne personnalisée `my-facebook-campaign`

* L’URL à laquelle elle sera publiée sur Twitter comprend l’ID de la campagne personnalisée `my-twitter-campaign`

À mesure que les abonnés Facebook et Twitter cliquent sur les URL, Microsoft effectue le suivi de chaque clic et l’associe à la campagne personnalisée correspondante. Les acquisitions éligibles du jeu et achats de modules complémentaires sont associés à la campagne personnalisée et déclarés comme autant de conversions.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Comprendre comment les acquisitions sont qualifiées de conversions

Une *conversion* de campagne personnalisée est une acquisition qui résulte d’un client cliquant sur une URL qui est promue via une campagne personnalisée. Il existe différents scénarios pour être éligibles en tant que conversion pour les **vues de page d’application et les conversions par ID de campagne** et les graphiques de **conversions de campagnes totales** dans le [rapport acquisitions](acquisitions-report.md) et pour être éligibles comme conversion pour [la récupération par programmation de l’ID de campagne](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Conversions éligibles dans le rapport acquisitions

Les scénarios suivants sont considérés comme une conversion pour les **vues de page d’application et les conversions par ID de campagne** et graphiques de **conversions de campagnes totales** dans le [rapport acquisitions](acquisitions-report.md):

* Un client *avec ou sans un compte Microsoft reconnu* clique sur une URL d’application qui contient un ID de campagne personnalisé et est redirigé vers la liste des boutiques pour l’application. Ensuite, ce même client acquiert l’application dans les 24 heures qui suivent le premier clic sur l’URL du Microsoft Store avec l’ID de la campagne personnalisée.

* Si le client acquiert l’application sur un autre appareil que celui sur lequel il a cliqué sur l’URL avec l’ID de la campagne personnalisée, la conversion est comptabilisée uniquement si le client est connecté avec le même compte Microsoft que lorsqu’il clique sur l’URL.

> [!NOTE]
> Pour les acquisitions d’applications qui sont comptabilisées comme des conversions pour une campagne personnalisée, les achats de modules complémentaires de cette application sont également comptabilisés comme des conversions pour la même campagne personnalisée.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Conversions éligibles lors de la récupération par programmation de l’ID de campagne

Pour être éligible en tant que conversion lors de la récupération par programme de l’ID de campagne associé à l’application, les conditions suivantes doivent être réunies :

* Sur un appareil exécutant **Windows 10, version 1607 ou ultérieure**: un client (qu’il soit connecté à un compte Microsoft reconnu ou non) clique sur une URL qui contient un ID de campagne personnalisé et est redirigé vers la page de liste des boutiques pour l’application. Le client acquiert l’application tout en affichant la liste du magasin suite à un clic sur l’URL.

* Sur un appareil exécutant **Windows 10, version 1511 ou antérieure**: un client (qui doit être connecté avec un compte Microsoft reconnu) clique sur une URL qui contient un ID de campagne personnalisé et est redirigé vers la page de liste des boutiques pour l’application. Le client acquiert l’application tout en affichant la liste du magasin suite à un clic sur l’URL. Sur ces versions de Windows 10, l’utilisateur doit être connecté avec un compte Microsoft reconnu afin que l’acquisition puisse être considérée comme une conversion lors de l’extraction par programmation de l’ID de campagne.

> [!NOTE]
> Si le client quitte la page de liste de la boutique, mais retourne à la page avec 24 heures (sur le même appareil ou sur un autre appareil lorsqu’il est connecté avec le même compte Microsoft) et acquiert l’application, cette opération **est** considérée comme une conversion dans les **vues de page d’application et les conversions par ID de campagne** et les graphiques [Acquisitions report](acquisitions-report.md)de **conversions de campagnes totales** dans Toutefois, cette **opération n’est pas** considérée comme une conversion si vous récupérez par programmation l’ID de campagne.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Incorporer un ID de campagne personnalisé à l’URL de la page de Microsoft Store de votre application

Pour créer une URL de page Microsoft Store pour votre application avec un ID de campagne personnalisé :

1.  Créez une chaîne d’ID pour la campagne personnalisée. Cette chaîne peut contenir jusqu’à 100 caractères, même s’il est recommandé de définir un ID de campagne court facilement identifiable.

   > [!NOTE]
   > La chaîne d’ID de la campagne peut être visible par d’autres développeurs lorsqu’ils consultent le [rapport acquisitions](acquisitions-report.md) pour leurs applications. Cela peut se produire lorsqu’un client clique sur votre ID de campagne personnalisé pour entrer dans le magasin et achète une autre application de développeur au sein de la même session, attribuant ainsi cette conversion à votre ID de campagne. Ce développeur verra le nombre de conversions de leur propre application résultant d’un clic initial sur votre ID de campagne, y compris le nom de l’ID de campagne, mais ils ne verront pas de données sur le nombre d’utilisateurs qui ont acheté vos propres applications (ou les applications de tous les autres développeurs) après avoir cliqué sur votre ID de campagne.

2.  Obtenir le lien pour le Listing de votre application dans le format HTML ou de protocole.

    * Utilisez l’URL HTML si vous souhaitez que les clients accèdent à la liste de la boutique web de votre application dans un navigateur sur n’importe quel système d’exploitation. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la liste de votre application. Cette URL a le format **`https://www.microsoft.com/store/apps/*your app ID*`** . Par exemple, l’URL HTML pour Skype est `https://www.microsoft.com/store/apps/9wzdncrfj364` . Vous pouvez trouver cette URL sur la page identité de votre [application](view-app-identity-details.md#link-to-your-apps-listing) .

    * Utilisez le format de protocole si vous promouvez votre application à partir d’autres applications Windows qui s’exécutent sur un appareil ou un ordinateur sur lequel l’application UWP est installée, ou lorsque vous savez que vos clients se trouvent sur un appareil qui prend en charge le Microsoft Store. Ce lien accède directement à la liste des boutiques de votre application sans ouvrir de navigateur. Cette URL a le format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`** . Par exemple, l’URL de protocole pour Skype est `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Ajouter la chaîne suivante à la fin de l’URL de votre application :

    * Pour une URL au format HTML, ajoutez **`?cid=*my custom campaign ID*`** . Par exemple, si Skype introduit un ID de campagne avec la ** \_ campagne personnalisée**, la nouvelle URL, y compris l’ID de campagne, serait la suivante : `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign` .

    * Pour une URL de format de protocole, ajoutez **`&cid=*my custom campaign ID*`** . Par exemple, si Skype introduit un ID de campagne avec la ** \_ campagne personnalisée**, la nouvelle URL de protocole, y compris l’ID de campagne, serait la suivante : `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign` .

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Récupérer par programme l’ID de campagne personnalisée pour une application

Si votre application est une application UWP, vous pouvez récupérer par programme l’ID de campagne personnalisé associé à l’acquisition d’une application à l’aide des API de la SDK Windows. Ces API rendent possible de nombreux scénarios d’analyse et de monétisation. Par exemple, vous pouvez déterminer si l’utilisateur actuel a acquis votre application après l’avoir découverte via votre campagne Facebook, puis personnaliser l’expérience de l’application en conséquence. En guise d’alternative, si vous utilisez un fournisseur de commercialisation d’applications tiers, vous pouvez lui renvoyer des données.

Ces API ne retournent une chaîne d’ID de campagne que si le client a cliqué sur votre URL avec l’ID de campagne incorporé, affiché la page Microsoft Store de votre application, puis acquiert votre application sans quitter la page de liste de la boutique. Si l’utilisateur quitte la page, puis retourne et acquiert l’application ultérieurement, cette opération n’est pas [considérée comme une conversion lors de](#conversions) l’utilisation de ces API.

Vous pouvez utiliser différentes API en fonction de la version de Windows 10 que votre application cible :

* Windows 10, version 1607 ou ultérieure : utilisez la classe [**StoreContext**](/uwp/api/windows.services.store.storecontext) dans l’espace de noms **Windows. services. Store** . Lorsque vous utilisez cette API, vous pouvez récupérer des ID de campagne personnalisés pour toutes les [acquisitions qualifiées](#conversions), que l’utilisateur soit connecté ou non avec un compte Microsoft reconnu.

* Windows 10, version 1511 ou antérieure : utilisez la classe [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) dans l’espace de noms **Windows. ApplicationModel. Store** . Lorsque vous utilisez cette API, vous pouvez uniquement récupérer des ID de campagne personnalisés pour les [acquisitions qualifiées](#conversions) où l’utilisateur est connecté avec un compte Microsoft reconnu.

> [!NOTE]
> Bien que l’espace de noms **Windows. ApplicationModel. Store** soit disponible dans toutes les versions de Windows 10, nous vous recommandons d’utiliser les API de l’espace de noms **Windows. services. Store** si votre application cible windows 10, version 1607 ou ultérieure. Pour plus d’informations sur les différences entre ces espaces de noms, consultez [achats et évaluations dans l’application](../monetize/in-app-purchases-and-trials.md#choose-namespace). L’exemple de code suivant montre comment structurer votre code pour utiliser les deux API dans le même projet.

### <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment récupérer l’ID de campagne personnalisé. Cet exemple utilise les deux ensembles d’API dans les espaces de noms **Windows. services. Store** et **Windows. ApplicationModel. Store** à l’aide du [code adaptatif](../debug-test-perf/version-adaptive-code.md)de la version. En suivant ce processus, votre code peut s’exécuter sur n’importe quelle version de Windows 10. Pour utiliser ce code, la version du système d’exploitation cible de votre projet doit être l' **édition anniversaire Windows 10 (10,0 ; Build 14394)** ou version ultérieure, même si la version minimale du système d’exploitation peut être une version antérieure.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Ce code effectue les actions suivantes :

1. Tout d’abord, il vérifie si la classe [**StoreContext**](/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows. services. Store** est disponible sur l’appareil actuel (cela signifie que l’appareil exécute Windows 10, version 1607 ou ultérieure). Dans ce cas, le code continue d’utiliser cette classe.

2. Ensuite, il tente d’obtenir l’ID de campagne personnalisé pour le cas où l’utilisateur actuel a une compte Microsoft reconnue. Pour ce faire, le code obtient un objet [**StoreSku**](/uwp/api/Windows.Services.Store.StoreSku) qui représente la référence SKU de l’application actuelle, puis accède à la propriété [**CampaignId**](/uwp/api/windows.services.store.storecollectiondata.CampaignId) pour récupérer l’ID de campagne, s’il en existe un.
3. Le code essaie ensuite de récupérer l’ID de campagne pour le cas où l’utilisateur actuel n’a pas de compte Microsoft reconnu. Dans ce cas, l’ID de la campagne est incorporé dans la licence de l’application. Le code récupère la licence à l’aide de la méthode [**GetAppLicenseAsync**](/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) , puis analyse le contenu JSON de la licence pour obtenir la valeur d’une clé nommée *customPolicyField1*. Cette valeur contient l’ID de la campagne.

4. Si la classe [**StoreContext**](/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows. services. Store** n’est pas disponible, le code revient ensuite à utiliser la méthode [**GetAppPurchaseCampaignIdAsync**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) dans l’espace de noms **Windows. ApplicationModel. Store** pour récupérer l’ID de campagne personnalisé (cet espace de noms est disponible dans toutes les versions de Windows 10, y compris la version 1511 et les versions antérieures). Notez que lorsque vous utilisez cette méthode, vous pouvez uniquement récupérer des ID de campagne personnalisés pour les [acquisitions qualifiées](#conversions) pour lesquelles l’utilisateur a une compte Microsoft reconnue.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Spécifier l’ID de campagne dans le fichier proxy pour l’espace de noms Windows. ApplicationModel. Store

L’espace de noms **Windows. ApplicationModel. Store** comprend [**CurrentAppSimulator**](/uwp/api/windows.applicationmodel.store.currentappsimulator), une classe spéciale qui simule les opérations de stockage pour le test de votre code avant de soumettre votre application au Windows Store. Cette classe récupère les données d' [un fichier local nommé Windows.StoreProxy.xml fichier](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). L’exemple de code précédent montre comment inclure à la fois **CurrentApp** et **CurrentAppSimulator** dans du code de débogage et de non-débogage dans votre projet. Pour tester ce code dans un environnement de débogage, ajoutez un élément **AppPurchaseCampaignId** au fichier WindowsStoreProxy.xml sur votre ordinateur de développement, comme indiqué dans l’exemple suivant. Quand vous exécutez l’application, la méthode [**GetAppPurchaseCampaignIdAsync**](/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) retourne toujours cette valeur.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. En revanche, vous devez publier une application dans le Windows Store et télécharger cette application sur votre appareil de développement pour utiliser sa licence à des fins de test. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Tester votre campagne personnalisée

Avant de promouvoir une URL de campagne personnalisée, nous vous recommandons de tester celle-ci en procédant comme suit :

1.  Connectez-vous à un compte Microsoft sur l’appareil que vous utilisez pour le test.

2.  Cliquez sur l’URL de votre campagne personnalisée. Assurez-vous que vous accédez à la page de votre application, puis fermez l’application UWP ou la page du navigateur.

3.  Cliquez plusieurs fois sur l’URL, en fermant l’application UWP ou la page de navigateur après chaque visite de la page de votre application. Lors de l' **une** des visites de la page de votre application, demandez à votre application de générer une conversion. Comptez le nombre total de fois où vous avez cliqué sur l’URL.

4. Vérifiez que les vues de page et les conversions attendues apparaissent dans les graphiques **affichages de la page d’application et conversions par ID de campagne** et conversions de **campagnes totales** dans le [rapport acquisitions](acquisitions-report.md), puis testez le code de votre application pour confirmer s’il peut récupérer correctement l’ID de la campagne à l’aide des API décrites ci-dessus.