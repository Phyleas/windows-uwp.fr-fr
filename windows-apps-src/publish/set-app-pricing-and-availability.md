---
description: La page Tarification et disponibilité du processus de soumission d’application vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients.
title: Définir la tarification et la disponibilité d’une application
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, prix, disponible, détectable, version d’évaluation gratuite, versions d’évaluation, applications, date de publication
ms.localizationpriority: medium
ms.openlocfilehash: f7373ae49b867e9fb1b59f6d7fb18e32f4d7bf65
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029922"
---
# <a name="set-app-pricing-and-availability"></a>Définir la tarification et la disponibilité d’une application


La page **Tarification et disponibilité** du [processus de soumission d’application](app-submissions.md) vous permet de déterminer le prix de votre application, la mise à disposition ou non d’une version d’évaluation gratuite, ainsi que le mode, la date et l’emplacement d’accessibilité de votre application auprès des clients. Cet article décrit les options disponibles sur cette page et les éléments à prendre en compte pour la spécification de ces informations.


## <a name="markets"></a>Marchés

Le Microsoft Store atteint les clients dans plus de 240 pays et régions du monde entier. Par défaut, nous proposons votre application sur tous les marchés possibles. Si vous préférez, vous pouvez choisir les marchés spécifiques dans lesquels vous souhaitez proposer votre application. 

Pour plus d’informations, consultez [définir la sélection de marché](./define-market-selection.md).


## <a name="visibility"></a>Visibilité

La section **visibilité** vous permet de définir des restrictions sur la façon dont votre application peut être découverte et acquise, notamment si des personnes peuvent trouver votre application dans le Store ou consulter la liste de leur boutique.

Pour plus d’informations, voir [Choisir les options de visibilité](choose-visibility-options.md).


## <a name="schedule"></a>Planifier

Par défaut (sauf si vous avez sélectionné l’une des options **rendre cette application disponible mais non détectable dans le Store** dans la section [visibilité](choose-visibility-options.md#discoverability) ), votre application sera disponible pour les clients dès qu’elle passera la certification et terminez le processus de publication. Pour choisir d’autres dates, sélectionnez **afficher les options** pour développer cette section. 

Pour plus d’informations, consultez [configurer une planification de version précise](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Tarifs

Vous devez sélectionner un prix de base pour votre application (sauf si vous avez sélectionné l’option **arrêter l’acquisition** sous **rendre cette application disponible mais non détectable dans le Store** dans la section [visibilité](choose-visibility-options.md#discoverability) ), en choisissant soit **gratuit** , soit l’un des niveaux de prix disponibles. Vous pouvez également planifier des changements de prix pour indiquer la date et l’heure auxquelles le prix de votre application doit changer. En outre, vous avez la possibilité de personnaliser ces modifications pour des marchés spécifiques. Microsoft met régulièrement à jour les prix recommandés pour tenir compte des fluctuations de devise sur les différents marchés. Lorsqu’un prix recommandé change, la zone de tarification affiche un indicateur d’avertissement si les prix que vous avez sélectionnés ne sont pas alignés sur les nouvelles valeurs recommandées. Les prix de vos produits ne changent pas, vous contrôlez quand et si vous souhaitez mettre à jour ces prix. 

Pour plus d’informations, voir [Définir et planifier le prix de l’application](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Essai gratuit

De nombreux développeurs offrent aux utilisateurs la possibilité d’essayer gratuitement leur application à l’aide de la fonctionnalité d’essai fournie par le Windows Store. Par défaut, **aucune évaluation gratuite** n’est sélectionnée et il n’y aura pas d’essai pour votre application. Si vous souhaitez offrir une version d’évaluation, vous pouvez sélectionner une valeur dans la liste déroulante **version d’évaluation gratuite** .

Il existe deux types d’essai que vous pouvez choisir, et vous avez la possibilité de configurer la date et l’heure de début et de fin de l’évaluation de l’essai.

### <a name="time-limited"></a>Limitée dans le temps

Choisissez **limité au temps** pour permettre aux clients d’essayer gratuitement votre application pendant un certain nombre de jours : **1 jour** , **7 jours** , **15 jours** ou **30 jours** . Vous pouvez limiter les fonctionnalités en ajoutant du code pour [exclure ou limiter les fonctionnalités de la version d’évaluation](../monetize/in-app-purchases-and-trials.md), ou vous pouvez permettre aux clients d’accéder à toutes les fonctionnalités pendant cette période. 
> [!NOTE]
> Les versions d’évaluation limitées dans le temps ne sont pas présentées aux clients sur Windows 10 Build 10.0.10586 ou version antérieure, ou aux clients sur Windows Phone 8,1 et versions antérieures.

### <a name="unlimited"></a>Illimité

Choisissez **illimité** pour permettre aux clients d’accéder gratuitement à votre application. Si vous voulez les inciter à acheter la version complète ultérieurement, veillez à ajouter du code pour [exclure ou limiter des fonctionnalités de la version d’évaluation](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Dates de début et de fin

Par défaut, votre version d’évaluation sera disponible dès la publication de votre application et ne sera jamais proposée. Si vous le souhaitez, vous pouvez spécifier la date et l’heure auxquelles votre essai doit commencer à être proposé et le moment où il doit cesser d’être proposé. 

>[!NOTE]
> Ces dates s’appliquent uniquement aux clients sur Windows 10 (y compris la Xbox). Si votre application est disponible pour les clients sur des versions antérieures du système d’exploitation, la version d’évaluation sera proposée à ces clients tant que votre produit sera disponible. 

Pour définir les dates auxquelles la version d’évaluation doit être proposée aux clients sur Windows 10, définissez la liste déroulante **commence** à et/ou **se termine sur** **à, puis** choisissez la date et l’heure. Dans ce cas, vous pouvez choisir **UTC** afin que l’heure que vous sélectionnez soit heure UTC (Universal Coordinated Time), ou choisir **local** afin que ces heures soient utilisées dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plus d’un fuseau horaire, un seul fuseau horaire de ce marché sera utilisé. Pour le États-Unis, le fuseau horaire est utilisé.) Vous pouvez sélectionner **Personnaliser pour des marchés spécifiques** si vous souhaitez définir des dates différentes pour un ou plusieurs marchés.


## <a name="sale-pricing"></a>Prix de vente

Si vous souhaitez proposer votre application à un prix réduit pendant une durée limitée, vous pouvez créer et planifier une vente.

Pour plus d’informations, voir [Vendre des applications et des composants additionnels à prix réduit](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, votre application peut être proposée aux entreprises avec une formule d’achat en volume. Vous pouvez indiquer si et comment votre application peut être proposée dans cette section.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).


## <a name="publish-date"></a>Date de publication

Auparavant, la section **Date de publication** apparaissait sur cette page. Cette fonctionnalité est désormais disponible sur la page [options d’envoi](manage-submission-options.md) , dans la section **options de conservation de publication** . (Notez que pour contrôler le moment où votre application doit être publiée dans le Windows Store, nous vous recommandons d’utiliser la section [planification](configure-precise-release-scheduling.md) de la page **tarification et disponibilité** .)
