---
description: L’espace partenaires vous offre plusieurs options pour permettre aux testeurs de tester votre application avant de la proposer au public.
title: Tests bêta et distribution ciblée
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, test bêta, distribution limitée, bêta, bêtas, test, testeurs
ms.localizationpriority: medium
ms.openlocfilehash: 0d383e5725d7a26283675740d7646fc26fdb537d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031202"
---
# <a name="beta-testing-and-targeted-distribution"></a>Tests bêta et distribution ciblée

Quelle que soit le soin avec lequel vous testez votre application, rien de vaut un test dans des conditions réelles, avec d’autres utilisateurs. Les testeurs peuvent épingler des problèmes qui vous auraient échappés, comme des fautes d’orthographe, un flux d’application peu clair ou des erreurs pouvant entraîner un blocage de l’application. Vous aurez ensuite la possibilité de résoudre ces problèmes avant de publier la soumission au public, ce qui entraînera un produit final plus soigné. 

L’espace partenaires vous offre plusieurs options pour permettre aux testeurs de tester votre application avant de la proposer au public.

Quelle que soit la méthode choisie, voici quelques points à prendre en compte lorsque vous testez votre application en version bêta.

- Vous ne pouvez pas révoquer l’accès à l’application une fois qu’un testeur l’a téléchargée. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles.
- Vous devez déterminer le mode de collecte de leurs commentaires. Envisagez de fournir un lien qui permet aux testeurs d’envoyer facilement des commentaires par courrier électronique (ou via le [Hub de commentaires](../monetize/launch-feedback-hub-from-your-app.md), si la confidentialité n’est pas un problème). 
- Vous pouvez consulter les [rapports d’analyse](analytics.md) de votre application, notamment les rapports d’utilisation et d’intégrité, ainsi que les évaluations ou les révisions laissées par vos testeurs.
- Lorsque vous distribuez l’application aux testeurs, vous pouvez y inclure des modules complémentaires. Étant donné que vous ne souhaitez probablement pas les payer pour un module complémentaire, vous pouvez [générer des codes promotionnels](generate-promotional-codes.md) et les distribuer à vos testeurs pour leur permettre d’obtenir gratuitement le module complémentaire, ou vous pouvez définir le prix du module complémentaire pour qu’il soit **gratuit** pendant le test (avant de mettre l’application à la disposition d’autres clients, créez une nouvelle soumission pour le module complémentaire pour modifier son prix). Notez que chaque module complémentaire ne peut être acheté qu’une seule fois par compte Microsoft, de sorte que le même testeur ne pourra pas tester plus d’une fois le processus d’acquisition du module complémentaire. 
- Vous pouvez fournir à vos testeurs une version mise à jour de votre application à tout moment en créant une nouvelle soumission avec de nouveaux packages. Les testeurs obtiendront la mise à jour après avoir effectué le processus de certification, tout comme ils ont obtenu le package d’origine, mais personne d’autre ne pourra l’obtenir (sauf si vous apportez des modifications supplémentaires, telles que le déplacement d’une application d’un **public privé** vers un public **public** ou la modification de l’appartenance aux groupes qui peuvent l’obtenir).

## <a name="private-audience"></a>Audience privée

Si vous souhaitez autoriser les testeurs à utiliser votre application avant qu’elle soit disponible pour d’autres personnes, et vous assurer que personne d’autre ne peut voir sa liste, utilisez l’option **audience privée** sous [visibilité](choose-visibility-options.md) (dans la page **tarification et disponibilité** de votre envoi). Il s’agit de la seule méthode qui vous permet de distribuer votre application à des testeurs tout en empêchant tout autre utilisateur de voir la liste des magasins de l’application, même s’ils ont pu taper son lien direct. 

L’option **audience privée** ne peut être utilisée que si vous n’avez pas encore publié votre application sur un public public. Vous pouvez utiliser cette option avec les applications ciblant n’importe quelle version du système d’exploitation, mais vos testeurs doivent exécuter Windows 10, version 1607 ou ultérieure (y compris Xbox One) et doivent être connectés avec le compte Microsoft associé à l’adresse de messagerie que vous fournissez.

Pour plus d’informations, consultez [audience privée](choose-visibility-options.md#audience).


## <a name="package-flights"></a>Versions d’évaluation de package

Si vous avez déjà publié votre application, vous pouvez créer des vols de packages pour distribuer un ensemble différent de packages aux personnes que vous spécifiez. Vous pouvez même créer plusieurs vols de packages pour la même application à utiliser avec différents groupes de personnes. C’est un excellent moyen de tester différents packages simultanément, et vous pouvez extraire des packages d’un vol vers votre envoi non volé si vous décidez que les packages sont prêts à être distribués à tout le monde.

Les vols de packages peuvent être utilisés avec des applications ciblant n’importe quelle version du système d’exploitation, mais vos testeurs peuvent uniquement recevoir l’application s’ils exécutent Windows. Desktop Build 10586 ou version ultérieure ; Windows. mobile Build 10586,63 ou version ultérieure ; ou Xbox One.

Pour en savoir plus, consultez [Versions d’évaluation de package](package-flights.md).


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Masquage de l’application dans Windows Store et utilisation des codes promotionnels

Cette option offre un autre moyen de limiter la distribution d’une application à un certain groupe de testeurs, tout en empêchant quiconque de découvrir votre application dans le Store (ou de l’acquérir sans code promotionnel). Toutefois, contrairement à l’option public public, il est possible pour quiconque de voir la liste de votre application s’ils ont le lien direct. Si la confidentialité est essentielle pour votre envoi, nous vous recommandons d’effectuer une publication sur un public privé à la place.

Le masquage de l’application et l’utilisation de codes promotionnels peuvent être utilisés avec des applications ciblant n’importe quelle version du système d’exploitation, mais vos testeurs peuvent uniquement récupérer l’application s’ils exécutent Windows 10.

Pour utiliser cette option :

- Dans la section **visibilité** de la page **tarification et disponibilité** , sous [détectabilité](choose-visibility-options.md#discoverability), sélectionnez **rendre ce produit disponible mais non détectable dans le Windows Store** . Choisissez l’option **arrêter l’acquisition : tout client disposant d’un lien direct peut voir la liste des magasins du produit, mais il ne peut le télécharger que s’il en a déjà détenu le produit ou s’il a un code promotionnel et utilise un appareil Windows 10** . 
- Une fois que l’application a réussi la certification, [Générez des codes promotionnels](generate-promotional-codes.md) pour l’application et distribuez-les à vos testeurs. Vous pouvez générer des codes qui autorisent jusqu’à 1600 remboursements pour une même application sur une période de six mois. Ces codes fournissent aux testeurs un lien direct vers la description de l’application et leur permettent de télécharger celle-ci gratuitement, même si vous avez défini un prix lors de la création de votre soumission.
- Lorsque vous êtes prêt à mettre votre application à la disposition du public, créez une nouvelle soumission et modifiez l’option de **visibilité** pour **rendre ce produit disponible et détectable dans le magasin** (avec toutes les autres modifications souhaitées).


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>Distribution ciblée avec un lien vers la liste de votre application

Contrairement aux options décrites ci-dessus, cette option est valable pour les clients Windows Phone 8,1 et Windows 10 (mais pas sur Windows 8. x). Aucun client ne pourra trouver l’application en effectuant une recherche ou en parcourant le Store, mais toute personne disposant d’un lien direct vers la liste de son Boutique peut la télécharger sur un appareil exécutant Windows Phone 8,1 ou version antérieure, ou sur Windows 10. Gardez à l’esprit que pour que vos testeurs téléchargent l’application gratuitement, vous devez définir son prix sur **gratuit** .

Pour utiliser cette option :
- Dans la section **visibilité** de la page **tarification et disponibilité** , sous [détectabilité](choose-visibility-options.md#discoverability), sélectionnez **rendre ce produit disponible mais non détectable dans le Windows Store** . Choisissez l’option **lien direct uniquement : tout client disposant d’un lien direct vers le Listing du produit peut le télécharger, sauf sur Windows 8. x.**
- Une fois votre produit publié, distribuez le lien ( **URL** sur la [page identité](view-app-identity-details.md)de l’application) à vos testeurs afin qu’ils puissent l’essayer.
- Lorsque vous êtes prêt à mettre votre application à la disposition du public, créez une nouvelle soumission et modifiez l’option de **visibilité** pour **rendre ce produit disponible et détectable dans le magasin** (avec toutes les autres modifications souhaitées).

> [!IMPORTANT]
> Depuis le 31 octobre 2018, les nouveaux produits ne peuvent pas inclure des packages ciblant Windows Phone 8. x ou une version antérieure. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>Distribution ciblée pour Windows Phone clients avec des adresses e-mail spécifiées

> [!IMPORTANT]
> Cette option n’est pas disponible pour les nouvelles soumissions. Si vous avez déjà sélectionné cette option pour une application ciblant Windows Phone 8,1 ou version antérieure, vous pourrez continuer à l’utiliser pour cette application. Vous pouvez apporter des modifications à la liste des testeurs (jusqu’à 10 000) en créant une nouvelle soumission. 

Avec cette option, les personnes avec les adresses de messagerie que vous avez spécifiées peuvent télécharger votre application (sur un appareil exécutant Windows Phone 8,1 ou version antérieure uniquement) en utilisant le lien direct vers sa liste. Aucun autre client ne pourra télécharger l’application, même s’il possède le lien, et qu’il ne pourra pas trouver l’application dans le magasin en effectuant une recherche ou en navigant. Pour que les testeurs puissent télécharger l’application, vous devez leur attribuer un lien ( **URL** sur la [page identité](view-app-identity-details.md)de l’application) et ils doivent être connectés avec un compte Microsoft associé à une adresse de messagerie que vous avez fournie. Vous pouvez également rendre l’application disponible pour les testeurs sur des appareils Windows 10 en [générant des codes promotionnels](generate-promotional-codes.md). toute personne disposant de l’un des codes promotionnels de votre application peut la télécharger sur un appareil Windows 10, même si vous n’avez pas entré son adresse de messagerie ici.
