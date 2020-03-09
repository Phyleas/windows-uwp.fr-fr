---
Description: Les déclarations de produit permettent de s’assurer que votre application est affichée de manière appropriée dans le Microsoft Store et proposée à l’ensemble de clients approprié.
title: Déclarations de produit
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47011a22353f26361a392690d857bde1fc180c03
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853439"
---
# <a name="product-declarations"></a>Déclarations de produit

La section **déclarations de produit** de la page [Propriétés](enter-app-properties.md) du [processus d’envoi](app-submissions.md) permet de s’assurer que votre application est affichée de manière appropriée et proposée à l’ensemble de clients approprié, et les aide à comprendre comment ils peuvent utiliser votre application.

Les sections suivantes décrivent certaines des déclarations et ce que vous devez prendre en compte pour déterminer si chaque déclaration s’applique à votre application. Notez que deux de ces déclarations sont activées par défaut (comme décrit ci-dessous). En fonction de la catégorie de votre produit, vous pouvez également voir des déclarations supplémentaires. Veillez à passer en revue toutes les déclarations et à vous assurer qu’elles reflètent avec précision votre soumission.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Cette application permet aux utilisateurs d’effectuer des achats, mais n’utilise pas le système de commerce Microsoft Store.

Pour presque toutes les soumissions, vous devez désactiver cette case à cocher, car les applications qui offrent des opportunités d’achat d’éléments qui sont ou peuvent être consommées ou utilisées dans votre application doivent utiliser l’API d’achat Microsoft Store dans l’application pour créer et envoyer les modules complémentaires. Selon le [contrat de développement d’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), les applications qui ont été créées et soumises avant le 29 juin 2015 peuvent continuer à proposer des fonctionnalités d’achat dans l’application sans utiliser le moteur de commerce de Microsoft, tant que la fonctionnalité d’achat est conforme aux [stratégies de Microsoft Store](store-policies.md#108-financial-transactions). Si cela s’applique à votre application, vous devez activer cette case. Sinon, laissez-la désactivée.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Cette application a fait l'objet de tests pour voir si elle est conforme aux recommandations d'accessibilité.

En cochant cette case, vous rendez votre application détectable par les clients qui recherchent tout particulièrement des applications accessibles dans le Windows Store.

Cochez uniquement cette case si vous avez :

-   défini toutes les informations d'accessibilité relatives aux éléments de l'interface utilisateur, comme les noms accessibles ;
-   implémenté la navigation et l'utilisation à partir du clavier en tenant compte de l'ordre des onglets, de l'activation du clavier, de la navigation à l'aide des touches de direction et des raccourcis ;
-   vérifié la présence d'une expérience visuelle accessible respectant notamment un coefficient de contraste de texte de 4.5:1 et n'utilisant pas simplement la couleur pour transmettre des informations à l'utilisateur ;
-   utilisé des outils de test d'accessibilité, comme Inspect ou AccChecker, afin de vérifier votre application et de résoudre les erreurs de priorité élevée détectées par ces outils ;
-   vérifié les scénarios clés de votre application de bout en bout à l'aide de fonctionnalités et d'outils tels que le Narrateur, la Loupe, le clavier sur écran, le contraste élevé et la haute résolution.

Lorsque vous déclarez votre application comme étant accessible, vous certifiez qu'elle est accessible à tous les utilisateurs, y compris ceux souffrant de handicaps. Cela signifie, par exemple, que vous avez testé l'application avec le mode de contraste élevé et avec un lecteur d'écran. Vous avez également vérifié que l’interface utilisateur fonctionnait correctement avec un clavier, la Loupe et d’autres outils d’accessibilité.

Pour plus d’informations, voir [Accessibilité](../design/accessibility/accessibility.md), [Test de l'accessibilité](../design/accessibility/accessibility-testing.md) et [Accessibilité dans le Windows Store](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> ne pas répertorier votre application comme accessible, sauf si vous l’avez spécifiquement conçue et testée à cet effet. Si votre application est déclarée comme étant accessible, mais qu’elle ne prend pas réellement en charge l’accessibilité, vous allez probablement recevoir des commentaires négatifs de la part de la communauté.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Les clients peuvent installer cette application sur d'autres disques ou dispositifs de stockage amovible.

Cette case à cocher est activée par défaut, pour permettre aux clients d’installer votre application sur un support de stockage externe ou amovible, tel qu’une carte SD, ou sur un lecteur de volume non-système tel qu’un lecteur externe.

Si vous souhaitez empêcher l’installation de votre application sur d’autres lecteurs ou un stockage amovible et autoriser uniquement l’installation sur le disque dur interne sur leur appareil, décochez cette case. (Notez qu’il n’existe aucune option permettant de limiter l’installation afin qu’une application *ne* puisse être installée que sur un support de stockage amovible.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows peut intégrer les données de cette application dans les sauvegardes automatiques sur OneDrive.

Cette case est cochée par défaut pour permettre l'insertion des données de votre application quand un client choisit de paramétrer Windows pour des sauvegardes automatiques sur OneDrive.

Si vous voulez empêcher l’insertion des données de votre application dans les sauvegardes automatiques, décochez cette case.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Cette application envoie les données de Kinect aux services externes. 

Si votre application utilise des données de Kinect et l’envoie à un service externe, vous devez activer cette case.



 

 

 




