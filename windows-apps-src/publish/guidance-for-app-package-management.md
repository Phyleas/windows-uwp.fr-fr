---
Description: Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.
title: Aide sur la gestion des packages d’application
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9f5caa2610e19234cfd83119d570f858c540b401
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685133"
---
# <a name="guidance-for-app-package-management"></a>Aide sur la gestion des packages d’application

Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.

-   [Versions du système d’exploitation et distribution des packages](#os-versions-and-package-distribution)
-   [Ajout de packages pour Windows 10 à une application précédemment publiée](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Suppression d’une application du Windows Store](#removing-an-app-from-the-store)
-   [Suppression de packages pour une famille d’appareils précédemment prise en charge](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versions de système d'exploitation et distribution de package

Les différents systèmes d’exploitation peuvent exécuter différents types de packages. Si plusieurs de vos packages peuvent s’exécuter sur l’appareil d’un client, le Microsoft Store fournit la meilleure correspondance disponible.

En règle générale, les systèmes d’exploitation plus récents peuvent exécuter des packages ciblant des versions antérieures pour la même famille d’appareils. Les appareils Windows 10 peuvent exécuter toutes les versions de système d’exploitation prises en charge précédentes (par famille d’appareils). Les appareils Windows 10 Desktop peuvent exécuter des applications qui ont été conçues pour Windows 8.1 ou Windows 8 ; Les appareils Windows 10 mobile peuvent exécuter des applications qui ont été créées pour Windows Phone 8,1, Windows Phone 8 et même Windows Phone 7. x. Toutefois, les clients sur Windows 10 n’obtiendront ces packages que si l’application n’inclut pas les packages UWP ciblant la famille d’appareils concernée.

> [!IMPORTANT]
> Depuis le 31 octobre 2018, les nouveaux produits ne peuvent pas inclure des packages ciblant Windows 8. x/Windows Phone 8. x ou une version antérieure. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Suppression d'une application du Store

Parfois, il est possible que vous souhaitiez arrêter de fournir une application à vos clients, « annuler » sa publication. Pour ce faire, cliquez sur **Rendre votre application indisponible** sur la page **Vue d’ensemble de l’application**. Quelques heures après que vous avez confirmé vouloir la rendre indisponible, votre application disparaît du Store. Dès lors, aucun nouveau client ne peut plus y accéder (sauf s'il possède un [code promotionnel](generate-promotional-codes.md) et utilise un appareil Windows 10).

> [!IMPORTANT]
> Avec cette option, les paramètres de [visibilité](choose-visibility-options.md#discoverability) sélectionnés dans vos soumissions seront remplacés. 

Cette option a le même effet que si vous avez créé une soumission et choisi l’option **Rendre ce produit disponible mais non détectable dans le Store** avec l’option **Empêcher l'acquisition**. Toutefois, elle ne vous oblige pas à créer une nouvelle soumission.

Notez que les clients ayant déjà l’application pourront encore l’utiliser et la retélécharger (et même recevoir des mises à jour si vous envoyez de nouveaux packages ultérieurement).

Après l’indisponibilité de l’application, vous la verrez toujours dans l’espace partenaires. Si vous décidez de la remettre à disposition des clients, vous pouvez cliquer sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. L’application est mise à disposition des nouveaux clients (sauf paramétrage contraire dans votre dernière soumission) dans les heures suivant votre confirmation.

> [!NOTE]
> Si vous souhaitez que votre application reste disponible, mais voulez arrêter de la proposer aux nouveaux clients sur une version spécifique de système d’exploitation, vous pouvez créer une autre soumission et supprimer tous les packages associés à la version de système d’exploitation pour laquelle vous souhaitez empêcher toute nouvelle acquisition. Par exemple, si vous aviez précédemment des packages pour Windows Phone 8,1 et Windows 10, et que vous ne souhaitez pas continuer à proposer l’application aux nouveaux clients sur Windows Phone 8,1, supprimez tous vos packages Windows Phone 8,1 de la soumission. Une fois la mise à jour publiée, aucun nouveau client sur Windows Phone 8,1 ne pourra acquérir l’application si les clients qui l’ont déjà peut continuer à l’utiliser. Toutefois, l’application est toujours disponible pour les nouveaux clients sur Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Suppression de packages pour une famille d'appareils précédemment prise en charge

Si vous supprimez tous les packages pour une certaine [famille d’appareils](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) déjà prise en charge par votre application, vous êtes invité à confirmer qu’il s’agit de votre intention avant de pouvoir enregistrer vos modifications sur la page **packages** .

Lorsque vous publiez une soumission qui supprime tous les packages qui peuvent s’exécuter sur une famille d’appareils que votre application prenait déjà en charge, les nouveaux clients ne peuvent pas acquérir l’application sur cette famille d’appareils. Vous pouvez toujours publier une autre mise à jour pour proposer de nouveau des packages pour cette famille d'appareils.

Gardez à l'esprit que même si vous supprimez tous les packages prenant en charge une certaine famille d'appareils, tous les clients existants ayant déjà installé l'application sur ce type d'appareil pourra encore l'utiliser et obtenir les mises à jour que vous proposerez ultérieurement.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Ajout de packages pour Windows 10 à une application précédemment publiée

Si vous avez une application dans le Store qui inclut uniquement des packages pour Windows 8. x et/ou Windows Phone 8. x et que vous souhaitez mettre à jour votre application pour Windows 10, créez une nouvelle soumission et ajoutez vos packages UWP. msixupload ou. appxupload lors de l’étape [packages](upload-app-packages.md) . Une fois que votre application passe par le processus de certification, le package UWP est également disponible pour les nouvelles acquisitions par les clients sur Windows 10.

> [!NOTE]
> Une fois qu’un client sur Windows 10 reçoit votre package UWP, vous ne pouvez pas le faire passer à l’utilisation d’un package pour la version précédente du système d’exploitation. 

Notez que le numéro de version de vos packages Windows 10 doit être supérieur à ceux des packages Windows 8, Windows 8.1 et/ou Windows Phone 8,1 que vous avez utilisés. Pour plus d’informations, voir [Numérotation des versions de packages](package-version-numbering.md).

Pour plus d’informations sur la création de packages d’applications UWP pour le Store, voir [Création de packages d’applications](../packaging/index.md).
