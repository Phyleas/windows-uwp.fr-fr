---
Description: Vous pouvez définir la date et l’heure précises auxquelles votre application doit devenir disponible dans le Windows Store, ce qui vous donne une plus grande flexibilité et la possibilité de personnaliser les dates pour différents marchés.
title: Configurer une planification précise de la publication
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, planification, date de publication, dates, lancement
ms.localizationpriority: medium
ms.openlocfilehash: eebd98d8e1ce39ef8d9876ab4749bcc76012f9fa
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210385"
---
# <a name="configure-precise-release-scheduling"></a>Configurer une planification précise de la publication

La section **Planification** de la page [Tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de définir la date et l’heure précises auxquelles votre application doit devenir disponible dans le Windows Store, ce qui vous offre un surcroît de flexibilité et la possibilité de personnaliser les dates pour différents marchés.

> [!NOTE]
> Bien que cet article fasse référence aux applications, la planification de la publication des soumissions d’extensions applique le même processus.

En outre, vous pouvez choisir de définir une date à laquelle le produit ne sera plus disponible dans le Store. Notez qu'ainsi le produit n'est plus accessible dans le Store via une recherche ou une navigation, mais que les clients disposant d'un lien direct peuvent voir la description du produit dans le Store. Ils peuvent le télécharger uniquement s'ils possèdent déjà le produit ou s’ils disposent d'un [code promotionnel](generate-promotional-codes.md) et utilisent un appareil Windows 10.

Par défaut (sauf si vous avez sélectionné l'une des options **Rendre votre application disponible, mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability)), votre application sera mise à disposition des clients dès qu'elle aura obtenu la certification et terminé le processus de publication. Pour choisir d’autres dates, sélectionnez **Afficher les options** pour développer cette section.

Notez que vous ne serez pas en mesure de configurer des dates dans la section **Planification** si vous avez sélectionné l'une des options **Rendre votre application disponible, mais non détectable dans le Store** dans la section [Visibilité](choose-visibility-options.md#discoverability). En effet, puisque votre application n'est pas publiée, il n’existe aucune date de publication à configurer.

> [!IMPORTANT]
> Les dates que vous spécifiez dans la section Planification s’appliquent uniquement aux clients sous Windows 10.
>
>Si votre application précédemment publiée prend en charge des versions de système d’exploitation antérieures, toute date d' **acquisition d’arrêt** que vous sélectionnez ne s’appliquera pas à ces clients. ils pourront toujours acquérir l’application (sauf si vous soumettez une mise à jour avec une nouvelle sélection dans la section [visibilité](choose-visibility-options.md#discoverability) ou si vous sélectionnez **rendre l’application indisponible** dans la page **vue d’ensemble** de l’application).

## <a name="base-schedule"></a>Planification de base

Vos sélections pour la Planification de base s’appliquent à tous les marchés dans lesquels votre application est disponible, sauf si vous ajoutez ensuite des dates pour des marchés spécifiques (ou des groupes de marché) en sélectionnant [Personnaliser pour des marchés spécifiques](#customize-the-schedule-for-specific-markets).

Vous disposez de deux options : **Publication** et **Empêcher l'acquisition**. 

## <a name="release"></a>Version finale

Dans le menu déroulant **Publication**, vous pouvez définir le moment où vous souhaitez que votre application soit disponible dans le Store. Cela signifie que l’application est détectable dans le Store via une recherche ou une navigation et que les clients peuvent afficher sa description dans le Store et acquérir l’application.

>[!NOTE]
> Une fois votre application publiée et disponible dans le Store, vous ne serez plus en mesure de sélectionner une date de **Publication** (dans la mesure où l’application est déjà publiée).

Voici les options que vous pouvez configurer pour la planification de **Publication** d'un produit :
- **dès que possible** : le produit sera publié dès qu’il sera certifié et publié. Il s'agit de l'option par défaut.
- **à** : le produit sera publié à la date et à l’heure de votre choix. Vous disposez de deux options supplémentaires :
   - **UTC** : l'heure sélectionnée correspondra à l'heure universelle coordonnée (UTC), afin que l’application soit publiée partout en même temps.
   - **Local** : l'heure sélectionnée sera utilisée dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plusieurs fuseaux horaires, seul un fuseau horaire sur ce marché sera utilisé. Pour le États-Unis, le fuseau horaire est utilisé. Une liste complète des fuseaux horaires s’affiche plus loin dans cette page.)
- **non planifiée** : l’application ne sera pas disponible dans le Store. Si vous choisissez cette option, vous pouvez rendre l’application disponible dans le Windows Store ultérieurement en créant une nouvelle soumission et en choisissant l’une des autres options.

## <a name="stop-acquisition"></a>Empêcher l'acquisition

Dans la liste déroulante **Empêcher l'acquisition**, vous pouvez définir une date et une heure auxquelles vous souhaitez empêcher les nouveaux clients d'acquérir votre application à partir du Store ou de la détecter dans ses descriptions. Cela peut être utile si vous voulez contrôler précisément le moment où une application ne sera plus proposée aux nouveaux clients, par exemple, lorsque vous coordonnez la disponibilité de plusieurs de vos applications.

Par défaut, l'option **Empêcher l'acquisition** est configurée sur jamais. Pour modifier ce paramètre, sélectionnez **à** dans le menu déroulant et spécifiez une date et une heure, comme décrit ci-dessus. À la date et à l'heure que vous sélectionnez, les clients ne seront plus en mesure d’acquérir l’application.

Il est important de comprendre que cette option a le même impact que la sélection de **rendre cette application détectable, mais non disponible** dans la section [visibilité](choose-visibility-options.md#discoverability) et que vous choisissez **arrêter l’acquisition : tout client disposant d’un lien direct peut voir le Listing Store du produit, mais il ne peut le télécharger que s’il en a déjà détenu le produit ou s’il utilise un appareil Windows 10.** Pour arrêter d’offrir une application aux nouveaux clients, cliquez sur **Rendre votre application indisponible** sur la page Vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si vous sélectionnez une date pour **Empêcher l'acquisition** et décidez ultérieurement que vous souhaitez la rendre disponible à nouveau, vous pouvez créer une nouvelle soumission et reconfigurer **Empêcher l'acquisition** sur **Jamais**. L’application sera à nouveau disponible après la publication de votre soumission mise à jour.

## <a name="customize-the-schedule-for-specific-markets"></a>Personnaliser la planification pour des marchés spécifiques 

Par défaut, les options que vous sélectionnez ci-dessus s'appliquent à tous les marchés dans lesquels votre application est proposée. Pour personnaliser le prix pour des marchés spécifiques, cliquez sur **Personnaliser pour des marchés spécifiques**. La fenêtre contextuelle **Sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. Si vous avez exclu des marchés dans la section [Marchés](define-pricing-and-market-selection.md), ces marchés ne s'afficheront pas. 

Pour ajouter une planification pour un marché, sélectionnez-le et cliquez sur **Enregistrer**. Vous disposerez des options **Publication** et **Empêcher l'acquisition** déjà décrites ci-dessus, mais les sélections que vous ferez s’appliqueront uniquement à ce marché.

Pour ajouter une planification à appliquer à plusieurs marchés, vous devez créer un *Groupe de marchés*. Pour ce faire, sélectionnez les marchés que vous souhaitez inclure, puis entrez un nom pour le groupe. (Ce nom sert uniquement de référence et est invisible pour les clients). Par exemple, si vous souhaitez créer un groupe de marchés pour l’Amérique du Nord, vous pouvez sélectionner **Canada**, **Mexique** et **États-Unis**et l'appeler **Amérique du Nord** ou lui donner un autre nom de votre choix. Lorsque vous avez fini de créer votre groupe de marchés, cliquez sur **Enregistrer**. Vous disposerez des options **Publication** et **Empêcher l'acquisition** déjà décrites ci-dessus, mais les sélections que vous ferez s’appliqueront uniquement à ce groupe de marchés.

Pour ajouter une planification personnalisée pour un marché ou un groupe de marchés supplémentaire, cliquez simplement à nouveau sur **Personnaliser pour des marchés spécifiques** et répétez ces étapes. Pour modifier les marchés inclus dans un groupe de marchés, sélectionnez son nom. Pour supprimer la planification personnalisée pour un groupe de marchés (ou un marché individuel), cliquez sur **Supprimer**.

> [!NOTE]
> Un marché ne peut appartenir qu’à un seul des groupes de marchés utilisés dans la section **Planification**. 

## <a name="global-time-zones"></a>Fuseaux horaires globaux

Vous trouverez ci-dessous un tableau qui indique les fuseaux horaires spécifiques utilisés sur chaque marché. par conséquent, lorsque votre envoi utilise l’heure locale (par exemple, une version à 9 heures locales), vous pouvez déterminer l’heure à laquelle il sera publié sur chaque marché, en particulier sur les marchés ayant plus d’une heure z un, comme le Canada.

| Marché | Fuseau horaire |
|--------|-----------|
| Afghanistan  |  (UTC + 04:30) Kaboul |
| Albanie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Algérie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Samoa américaines  |  (UTC + 13:00) Samoa |
| Andorre  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Angola  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Anguilla  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Antarctique  |  (UTC + 12:00) Auckland, Wellington |
| Antigua-et-Barbuda  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Argentine  |  (UTC-03:00) Ville de Buenos |
| Arménie  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Aruba  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Australie  |  (UTC + 10:00) Canberra, Melbourne, Sydney |
| Autriche  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Azerbaïdjan  |  (UTC + 04:00) Bakou |
| Bahamas, Les  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Bahreïn  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Bangladesh  |  (UTC + 06:00) Dacca |
| Barbade (La)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Biélorussie  |  (UTC + 03:00) Minsk |
| Belgique  |  (UTC + 01:00) Bruxelles, Copenhague, Madrid, Paris |
| Belize  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Bénin  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Bermudes  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Bhoutan  |  (UTC + 06:00) Dacca |
| République République bolivarienne du Venezuela  |  (UTC-04:00) Caracas |
| Bolivie  |  (UTC-04:00) Georgetown, la Paz, Manaus, San Juan |
| Bonaire, Saint-Eustache et Saba  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Bosnie-Herzégovine  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Botswana  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Bouvet (Île)  |  (UTC + 00:00) Monrovia, Reykjavik |
| Brésil  |  (UTC-03:00) Brasilia |
| Territoires britanniques de l’océan Indien  |  (UTC + 06:00) Dacca |
| Îles Vierges britanniques  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Brunéi Darussalam  |  (UTC + 08:00) Irkoutsk |
| Bulgarie  |  (UTC + 02:00) Chisinau |
| Burkina-Faso  |  (UTC + 00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC + 02:00) Harare, Pretoria |
| CÃ ́TE Côte-d’Ivoire  |  (UTC + 00:00) Monrovia, Reykjavik |
| Cambodge  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Cameroun  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Canada  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Cabo Verde  |  (UTC-01:00) Cabo Verde est. |
| Caïmans (îles)  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| République centrafricaine  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Tchad  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Chili  |  (UTC-04:00) Horaire |
| Chine  |  (UTC + 08:00) Pékin, Chongqing, Hong Kong, Urumqi |
| Christmas (île)  |  (UTC + 07:00) Krasnoïarsk |
| Cocos-Keeling (îles)  |  (UTC + 06:30) Rangoon (Rangoon) |
| Colombie  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Comores (Les)  |  (UTC + 03:00) Nairobi |
| Congo  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Congo (RDC)  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Cook (îles)  |  (UTC-10:00) Hawaï |
| Costa Rica  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Croatie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| CuraÃ § AO  |  (UTC-04:00) Cuiabá |
| Chypre  |  (UTC + 02:00) Chisinau |
| République tchèque  |  (UTC + 01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Danemark  |  (UTC + 01:00) Bruxelles, Copenhague, Madrid, Paris |
| Djibouti  |  (UTC + 03:00) Nairobi |
| Dominique  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| République dominicaine  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Équateur  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Égypte  |  (UTC + 02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Guinée équatoriale  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Érythrée  |  (UTC + 03:00) Nairobi |
| Estonie  |  (UTC + 02:00) Chisinau |
| Éthiopie  |  (UTC + 03:00) Nairobi |
| Malouines (îles)  |  (UTC-04:00) Horaire |
| Féroé (îles)  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Fidji  |  (UTC + 12:00) Îles |
| Finlande  |  (UTC + 02:00) Helsinki, Kiev, Riga, Sofia, Tallinn, Vilnius |
| France  |  (UTC + 01:00) Bruxelles, Copenhague, Madrid, Paris |
| Guyane française  |  (UTC-03:00) Cayenne, Fortaleza |
| Polynésie française  |  (UTC-10:00) Hawaï |
| Terres australes et antarctiques françaises  |  (UTC + 05:00) Achgabat, Tachkent |
| Gabon  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Gambie  |  (UTC + 00:00) Monrovia, Reykjavik |
| Géorgie  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Germany  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Ghana  |  (UTC + 00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Grèce  |  (UTC + 02:00) Athènes, Bucarest |
| Groenland  |  (UTC + 00:00) Monrovia, Reykjavik |
| Grenade  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Guadeloupe  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Guam  |  (UTC + 10:00) Guam, Port Moresby |
| Guatemala  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Guernesey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinée  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinée-Bissau  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guyana  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Haïti  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Heard et McDonald (Îles)  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Saint-Siège (État de la Cité du Vatican)  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Honduras  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Hong Kong (R.A.S.)  |  (UTC + 08:00) Pékin, Chongqing, Hong Kong, Urumqi |
| Hongrie  |  (UTC + 01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Islande  |  (UTC + 00:00) Monrovia, Reykjavik |
| Inde  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Indonésie  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Irak  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Irlande  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Israël  |  (UTC + 02:00) Jérusalem |
| Italie  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Jamaïque  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Japan  |  (UTC + 09:00) Osaka, Sapporo, Tokyo |
| Jersey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Jordanie  |  (UTC + 02:00) Chisinau |
| Kazakhstan  |  (UTC + 05:00) Achgabat, Tachkent |
| Kenya  |  (UTC + 03:00) Nairobi |
| Kiribati  |  (UTC + 14:00) Kiritimati (île) |
| Corée  |  (UTC + 09:00) Séoul |
| Koweït  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Kirghizistan  |  (UTC + 06:00) Astana |
| Laos  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Lettonie  |  (UTC + 02:00) Chisinau |
| Liban  |  (UTC + 02:00) Chisinau |
| Lesotho  |  (UTC + 02:00) Harare, Pretoria |
| Liberia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Libye  |  (UTC + 02:00) Chisinau |
| Liechtenstein  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Lituanie  |  (UTC + 02:00) Chisinau |
| Luxembourg  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Macao R.A.S.  |  (UTC + 08:00) Pékin, Chongqing, Hong Kong, Urumqi |
| Macédoine (ex-République yougoslave de Macédoine)  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Madagascar  |  (UTC + 03:00) Nairobi |
| Malawi  |  (UTC + 02:00) Harare, Pretoria |
| Malaisie  |  (UTC + 08:00) Kuala Lumpur, Singapour |
| Maldives  |  (UTC + 05:00) Achgabat, Tachkent |
| Mali  |  (UTC + 00:00) Monrovia, Reykjavik |
| Malte  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Man, île de  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Marshall (îles)  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-ancien |
| Martinique  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Mauritanie  |  (UTC + 00:00) Monrovia, Reykjavik |
| Maurice  |  (UTC + 04:00) Port Louis |
| Mayotte  |  (UTC + 03:00) Nairobi |
| Mexique  |  (UTC-06:00) Guadalajara, Mexico, Monterrey |
| Micronésie  |  (UTC + 10:00) Guam, Port Moresby |
| République de Moldova  |  (UTC + 02:00) Chisinau |
| Monaco  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Mongolie  |  (UTC + 07:00) Krasnoïarsk |
| Monténégro  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Montserrat  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Maroc  |  (UTC + 01:00) Casablanca |
| Mozambique  |  (UTC + 02:00) Harare, Pretoria |
| Myanmar  |  (UTC + 06:30) Rangoon (Rangoon) |
| Namibie  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Nauru  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-ancien |
| Népal  |  (UTC + 05:45) Katmandou |
| Pays-Bas  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Nouvelle-Calédonie  |  (UTC + 11:00) Îles Salomon, Nouvelle-Calédonie |
| Nouvelle-Zélande  |  (UTC + 12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Heure centrale (États-Unis & Canada) |
| Niger  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Nigeria  |  (UTC + 01:00) Afrique centrale de l’ouest |
| Niue  |  (UTC + 13:00) Samoa |
| Norfolk (île)  |  (UTC + 11:00) Îles Salomon, Nouvelle-Calédonie |
| Mariannes du Nord (îles)  |  (UTC + 10:00) Guam, Port Moresby |
| Norvège  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Oman  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Pakistan  |  (UTC + 05:00) Islamabad, Karachi |
| Palau  |  (UTC + 09:00) Osaka, Sapporo, Tokyo |
| Autorité palestinienne  |  (UTC + 02:00) Chisinau |
| Panama  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Papouasie-Nouvelle-Guinée  |  (UTC + 10:00) Vladivostok |
| Paraguay  |  (UTC-04:00) Asunción |
| Pérou  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Philippines  |  (UTC + 08:00) Kuala Lumpur, Singapour |
| Pitcairn (îles)  |  (UTC-08:00) Heure du Pacifique (États-Unis & Canada) |
| Pologne  |  (UTC + 01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Portugal  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Qatar  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Réunion (île)  |  (UTC + 04:00) Port Louis |
| Roumanie  |  (UTC + 02:00) Chisinau |
| ROW  |  (UTC-07:00) Montagnes Rocheuses (États-Unis & Canada) |
| Russie  |  (UTC + 03:00) Moscou, Saint-Pétersbourg |
| Rwanda  |  (UTC + 02:00) Harare, Pretoria |
| SÃ £ o TomÃ © et PrÃncipe  |  (UTC + 00:00) Monrovia, Reykjavik |
| Saint BarthÃ © lemy  |  (UTC + 04:00) Erevan |
| Sainte-Hélène, Ascension et Tristan da Cunha  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Saint Kitts et Nevis  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Sainte-Lucie  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Saint-Martin (partie française)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Saint-Pierre-et-Miquelon  |  (UTC-02:00) Mid-Atlantique-ancien |
| Saint-Vincent-et-les-Grenadines  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Samoa  |  (UTC + 13:00) Samoa |
| Saint-Marin  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Arabie Saoudite  |  (UTC + 03:00) Koweït, Riyad |
| Sénégal  |  (UTC + 00:00) Monrovia, Reykjavik |
| Serbie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Seychelles  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Sierra Leone  |  (UTC + 00:00) Monrovia, Reykjavik |
| Singapour  |  (UTC + 08:00) Kuala Lumpur, Singapour |
| Sint-Maarten (partie néerlandaise)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Slovaquie  |  (UTC + 01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Slovénie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Salomon (îles)  |  (UTC + 11:00) Îles Salomon, Nouvelle-Calédonie |
| Somalie  |  (UTC + 03:00) Nairobi |
| Afrique du Sud  |  (UTC + 02:00) Harare, Pretoria |
| Géorgie du Sud-et-les îles Sandwich du Sud  |  (UTC-02:00) Mid-Atlantique-ancien |
| Espagne  |  (UTC + 01:00) Bruxelles, Copenhague, Madrid, Paris |
| Sri Lanka  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Surinam  |  (UTC-03:00) Cayenne, Fortaleza |
| Svalbard et Jan Mayen  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Swaziland  |  (UTC + 02:00) Harare, Pretoria |
| Suède  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Suisse  |  (UTC + 01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Taïwan  |  (UTC + 08:00) Taipei |
| Tadjikistan  |  (UTC + 05:00) Achgabat, Tachkent |
| Tanzanie  |  (UTC + 03:00) Nairobi |
| Thaïlande  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Timor-Leste  |  (UTC + 09:00) Séoul |
| Togo  |  (UTC + 00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC + 13:00) Nuku’alofa |
| Tonga  |  (UTC + 13:00) Nuku’alofa |
| Trinité-et-Tobago  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Tunisie  |  (UTC + 01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Turquie  |  (UTC + 03:00) Istanbul |
| Turkménistan  |  (UTC + 05:00) Achgabat, Tachkent |
| Turks et Caïcos (îles)  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Tuvalu  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-ancien |
| Îles mineures éloignées des États-Unis  |  (UTC + 13:00) Samoa |
| Îles Vierges (É.-U.)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Ouganda  |  (UTC + 03:00) Nairobi |
| Ukraine  |  (UTC + 02:00) Chisinau |
| Émirats arabes unis  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Royaume-Uni  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| États-Unis  |  (UTC-05:00) Heure de la côte est (États-Unis & Canada) |
| Uruguay  |  (UTC-03:00) Brasilia |
| Ouzbékistan  |  (UTC + 05:00) Achgabat, Tachkent |
| Vanuatu  |  (UTC + 11:00) Îles Salomon, Nouvelle-Calédonie |
| Vietnam  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Wallis-et-Futuna  |  (UTC + 12:00) Petropavlovsk-Kamchatsky-ancien |
| Sahara occidentale (litige)  |  (UTC + 00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Yémen  |  (UTC + 04:00) Abu Dhabi, Mascate |
| Zambie  |  (UTC + 02:00) Harare, Pretoria |
| Zimbabwe  |  (UTC + 02:00) Harare, Pretoria |
