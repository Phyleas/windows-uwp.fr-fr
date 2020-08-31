---
Description: Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.
title: Aide sur la gestion des packages d’application
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5ecd8cc96196c31615eac032183956de3bee9e4b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171053"
---
# <a name="guidance-for-app-package-management"></a>Aide sur la gestion des packages d’application

Découvrez comment les packages de votre application sont mis à la disposition de vos clients, et comment gérer des scénarios de package spécifiques.

-   [Versions de système d’exploitation et distribution de package](#os-versions-and-package-distribution)
-   [Ajout de packages pour Windows 10 à une application publiée précédemment](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Suppression d'une application du Windows Store](#removing-an-app-from-the-store)
-   [Suppression de packages pour une famille d'appareils précédemment prise en charge](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versions de système d’exploitation et distribution de package

Les différents systèmes d’exploitation peuvent exécuter différents types de packages. Si plusieurs de vos packages peuvent être exécutés sur l’appareil d’un client, le Microsoft Store fournira la meilleure correspondance disponible.

En règle générale, les systèmes d’exploitation plus récents peuvent exécuter des packages ciblant des versions antérieures pour la même famille d’appareils. Les appareils Windows 10 peuvent exécuter toutes les versions de système d’exploitation prises en charge précédentes (par famille d’appareils). Les appareils de bureau Windows 10 peuvent exécuter des applications conçues pour Windows 8.1 ou Windows 8. Les appareils mobiles Windows 10 peuvent exécuter des applications conçues pour Windows Phone 8.1, Windows Phone 8, voire Windows Phone 7.x. Toutefois, les clients sur Windows 10 n’obtiendront ces packages que si l’application n’inclut pas les packages UWP ciblant la famille d’appareils concernée.

> [!IMPORTANT]
> Vous ne pouvez plus charger de nouveaux packages XAP générés à l’aide du ou des kits de développement logiciel (SDK) Windows Phone 8. x. Les applications qui sont déjà dans Store avec des packages XAP continuent de fonctionner sur les appareils Windows 10 mobile. Pour plus d’informations, consultez ce billet de [blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Suppression d'une application du Windows Store

Parfois, vous souhaiterez peut-être arrêter d’offrir une application aux clients, en « annulant la publication ». Pour ce faire, cliquez sur **rendre l’application indisponible** dans la page **vue d’ensemble** de l’application. Une fois que vous avez confirmé que vous souhaitez rendre l’application indisponible, dans quelques heures, elle n’est plus visible dans le Store, et aucun nouveau client ne pourra l’obtenir (à moins qu’elle n’ait un [code promotionnel](generate-promotional-codes.md) et qu’elle utilise un appareil Windows 10).

> [!IMPORTANT]
> Cette option remplace tous les paramètres de [visibilité](choose-visibility-options.md#discoverability) que vous avez sélectionnés dans vos envois. 

Cette option a le même effet que si vous avez créé une soumission et choisi **rendre ce produit disponible mais non détectable dans le magasin** avec l’option **arrêter l’acquisition** . Toutefois, il n’est pas nécessaire de créer une nouvelle soumission.

Notez que tous les clients qui disposent déjà de l’application pourront toujours l’utiliser et la télécharger à nouveau (et peuvent même obtenir des mises à jour si vous soumettez de nouveaux packages ultérieurement).

Après l’indisponibilité de l’application, vous la verrez toujours dans l’espace partenaires. Si vous décidez de la remettre à disposition des clients, vous pouvez cliquer sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. L’application est mise à disposition des nouveaux clients (sauf paramétrage contraire dans votre dernière soumission) dans les heures suivant votre confirmation.

> [!NOTE]
> Si vous souhaitez que votre application reste disponible, mais que vous ne souhaitiez pas la proposer aux nouveaux clients sur une version particulière du système d’exploitation, vous pouvez créer une soumission et supprimer tous les packages pour la version du système d’exploitation sur laquelle vous souhaitez empêcher les nouvelles acquisitions. Par exemple, si vous aviez précédemment des packages pour Windows Phone 8,1 et Windows 10, et que vous ne souhaitez pas continuer à proposer l’application aux nouveaux clients sur Windows Phone 8,1, supprimez tous vos packages Windows Phone 8,1 de la soumission. Une fois la mise à jour publiée, aucun nouveau client sur Windows Phone 8,1 ne pourra acquérir l’application si les clients qui l’ont déjà peut continuer à l’utiliser. Toutefois, l’application est toujours disponible pour les nouveaux clients sur Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Suppression de packages pour une famille d'appareils précédemment prise en charge

Si vous supprimez tous les packages pour une certaine [famille d’appareils](/uwp/extension-sdks/device-families-overview) déjà prise en charge par votre application, vous êtes invité à confirmer qu’il s’agit de votre intention avant de pouvoir enregistrer vos modifications sur la page **packages** .

Lorsque vous publiez une soumission qui supprime tous les packages qui peuvent s’exécuter sur une famille d’appareils que votre application prenait déjà en charge, les nouveaux clients ne peuvent pas acquérir l’application sur cette famille d’appareils. Vous pouvez toujours publier une autre mise à jour pour proposer de nouveau des packages pour cette famille d'appareils.

Gardez à l'esprit que même si vous supprimez tous les packages prenant en charge une certaine famille d'appareils, tous les clients existants ayant déjà installé l'application sur ce type d'appareil pourra encore l'utiliser et obtenir les mises à jour que vous proposerez ultérieurement.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Ajout de packages pour Windows 10 à une application publiée précédemment

Si vous avez une application dans le Store qui inclut uniquement des packages pour Windows 8. x et/ou Windows Phone 8. x et que vous souhaitez mettre à jour votre application pour Windows 10, créez une nouvelle soumission et ajoutez vos packages UWP. msixupload ou. appxupload lors de l’étape [packages](upload-app-packages.md) . Une fois que votre application passe par le processus de certification, le package UWP est également disponible pour les nouvelles acquisitions par les clients sur Windows 10.

> [!NOTE]
> Une fois qu’un client sur Windows 10 reçoit votre package UWP, vous ne pouvez pas le faire passer à l’utilisation d’un package pour la version précédente du système d’exploitation. 

Notez que le numéro de version de vos packages Windows 10 doit être supérieur à ceux des packages Windows 8, Windows 8.1 et/ou Windows Phone 8,1 que vous avez utilisés. Pour plus d’informations, consultez la page [numérotation des versions de package](package-version-numbering.md).

Pour plus d’informations sur l’empaquetage d’applications UWP pour le Windows Store, consultez [empaquetage d’applications](../packaging/index.md).