---
description: Vous pouvez définir la date et l’heure précises auxquelles votre application doit être disponible dans le magasin, ce qui vous offre une plus grande flexibilité et la possibilité de personnaliser des dates pour différents marchés.
title: Configurer une planification précise de la publication
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, planification, date de publication, dates, lancement
ms.localizationpriority: medium
ms.openlocfilehash: bf9fe34eb36cab57677ba8ef22397c9c345ecb40
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035172"
---
# <a name="configure-precise-release-scheduling"></a>Configurer une planification précise de la publication

La section **planification** de la page [tarification et disponibilité](set-app-pricing-and-availability.md) vous permet de définir la date et l’heure précises auxquelles votre application doit être disponible dans le Store, ce qui vous offre une plus grande flexibilité et la possibilité de personnaliser des dates pour différents marchés.

> [!NOTE]
> Bien que cette rubrique fasse référence aux applications, la planification des versions pour les envois de compléments utilise le même processus.

Vous pouvez également choisir de définir une date à laquelle le produit ne doit plus être disponible dans le magasin. Notez que cela signifie que le produit ne peut plus être trouvé dans le magasin par le biais d’une recherche ou d’une navigation, mais que les clients disposant d’un lien direct peuvent voir la liste des magasins du produit. Ils peuvent uniquement le télécharger s’ils possèdent déjà le produit ou s’ils disposent d’un [code promotionnel](generate-promotional-codes.md) et utilisent un appareil Windows 10.

Par défaut (sauf si vous avez sélectionné l’une des options **rendre cette application disponible mais non détectable dans le Store** dans la section [visibilité](choose-visibility-options.md#discoverability) ), votre application sera disponible pour les clients dès qu’elle passera la certification et terminez le processus de publication. Pour choisir d’autres dates, sélectionnez **afficher les options** pour développer cette section.

Notez que vous ne pourrez pas configurer de dates dans la section **planification** si vous avez sélectionné l’une des options **rendre cette application disponible mais non détectable dans le Store** dans la section [visibilité](choose-visibility-options.md#discoverability) , car votre application n’est pas disponible pour les clients, il n’y a donc aucune date de publication à configurer.

> [!IMPORTANT]
> Les dates que vous spécifiez dans la section planification s’appliquent uniquement aux clients sur Windows 10.
>
>Si votre application précédemment publiée prend en charge des versions de système d’exploitation antérieures, toute date d' **acquisition d’arrêt** que vous sélectionnez ne s’appliquera pas à ces clients. ils pourront toujours acquérir l’application (sauf si vous soumettez une mise à jour avec une nouvelle sélection dans la section [visibilité](choose-visibility-options.md#discoverability) ou si vous sélectionnez **rendre l’application indisponible** dans la page **vue d’ensemble** de l’application).

## <a name="base-schedule"></a>Planification de base

Les sélections que vous effectuez pour la planification de base s’appliquent à tous les marchés dans lesquels votre application est disponible, sauf si vous ajoutez ultérieurement des dates pour des marchés spécifiques (ou des groupes de marché) en sélectionnant [Personnaliser pour des marchés spécifiques](#customize-the-schedule-for-specific-markets).

Deux options s’affichent ici : **mise en sortie** et arrêt de l' **acquisition** .

## <a name="release"></a>Libérer

Dans la liste déroulante **version** , vous pouvez définir le moment où vous souhaitez que votre application soit disponible dans le Windows Store. Cela signifie que l’application est détectable dans le magasin par le biais d’une recherche ou d’une navigation, et que les clients peuvent consulter la liste de leur boutique et acquérir l’application.

>[!NOTE]
> Une fois que votre application a été publiée et qu’elle est disponible dans le Store, vous ne pourrez plus sélectionner une date de **publication** (puisque l’application aura déjà été publiée).

Voici les options que vous pouvez configurer pour le calendrier de **publication** d’un produit :
- dès **que possible** : le produit sera publié dès qu’il sera certifié et publié. Il s'agit de l'option par défaut.
- **à** : le produit est publié à la date et à l’heure que vous sélectionnez. Vous avez également deux options :
   - **UTC** : l’heure que vous sélectionnez sera l’heure UTC (Universal Coordinated Time), afin que l’application se libère en même temps.
   - **Local** : l’heure que vous sélectionnez sera utilisée dans chaque fuseau horaire associé à un marché. (Notez que pour les marchés qui incluent plus d’un fuseau horaire, un seul fuseau horaire de ce marché sera utilisé. Pour le États-Unis, le fuseau horaire est utilisé. Une liste complète des fuseaux horaires s’affiche plus loin dans cette page.)
- **non planifié** : l’application ne sera pas disponible dans le Store. Si vous choisissez cette option, vous pouvez rendre l’application disponible dans le Store plus tard en créant une nouvelle soumission et en choisissant l’une des autres options.

## <a name="stop-acquisition"></a>Arrêter l’acquisition

Dans la liste déroulante **arrêter l’acquisition** , vous pouvez définir une date et une heure pour ne plus autoriser les nouveaux clients à l’acquérir à partir du Store ou à découvrir sa liste. Cela peut être utile si vous souhaitez contrôler précisément le moment où une application ne sera plus proposée aux nouveaux clients, par exemple lorsque vous coordonnerez la disponibilité entre plusieurs de vos applications.

Par défaut, l’option **arrêter l’acquisition** est définie sur jamais. Pour modifier cette valeur, sélectionnez **at** dans la liste déroulante et spécifiez une date et une heure, comme décrit ci-dessus. À la date et à l’heure que vous sélectionnez, les clients ne seront plus en mesure d’acquérir l’application.

Il est important de comprendre que cette option a le même impact que la sélection de **rendre cette application détectable, mais non disponible** dans la section [visibilité](choose-visibility-options.md#discoverability) et que vous choisissez **arrêter l’acquisition : tout client disposant d’un lien direct peut voir le Listing Store du produit, mais il ne peut le télécharger que s’il en a déjà détenu le produit ou s’il utilise un appareil Windows 10.** Pour arrêter complètement l’offre d’une application aux nouveaux clients, cliquez sur **rendre l’application indisponible** dans la page vue d’ensemble de l’application. Pour en savoir plus, consultez l’article [Suppression d’une application du Windows Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si vous sélectionnez une date pour **arrêter l’acquisition** et décidez ultérieurement que vous souhaitez rendre l’application à nouveau disponible, vous pouvez créer une nouvelle soumission et modifier **arrêter l’acquisition** en **n'** importe quel type. L’application sera à nouveau disponible après la publication de votre soumission mise à jour.

## <a name="customize-the-schedule-for-specific-markets"></a>Personnaliser la planification pour des marchés spécifiques

Par défaut, les options que vous sélectionnez ci-dessus s’appliquent à tous les marchés dans lesquels votre application est proposée. Pour personnaliser le prix de marchés spécifiques, cliquez sur **Personnaliser pour des marchés spécifiques** . La fenêtre contextuelle de **sélection du marché** s’affiche, répertoriant tous les marchés dans lesquels vous avez choisi de rendre votre application disponible. Si vous avez exclu un marché dans la section [Markets](./define-market-selection.md) , ces marchés ne seront pas affichés.

Pour ajouter une planification pour un marché, sélectionnez-la, puis cliquez sur **Enregistrer** . Vous voyez alors les mêmes options de **mise en sortie** et d' **arrêt** que celles décrites ci-dessus, mais les sélections que vous effectuez ne s’appliquent qu’à ce marché.

Pour ajouter une planification qui s’appliquera à plusieurs marchés, vous allez créer un *groupe de marché* . Pour ce faire, sélectionnez les marchés que vous souhaitez inclure, puis entrez un nom pour le groupe. (Ce nom est destiné à votre référence uniquement et n’est pas visible pour les clients.) Par exemple, si vous souhaitez créer un groupe de marché pour Amérique du Nord, vous pouvez sélectionner **Canada** , **Mexique** et **États-Unis** , puis le nommer **Amérique du Nord** ou un autre nom que vous choisissez. Lorsque vous avez terminé de créer votre groupe de marché, cliquez sur **Enregistrer** . Vous voyez alors les mêmes options de **mise en sortie** et d' **arrêt** que celles décrites ci-dessus, mais les sélections que vous effectuez ne s’appliquent qu’à ce groupe de marché.

Pour ajouter une planification personnalisée pour un marché supplémentaire, ou un groupe de marché supplémentaire, cliquez simplement sur **Personnaliser pour des marchés spécifiques** et répétez ces étapes. Pour modifier les marchés inclus dans un groupe de marché, sélectionnez son nom. Pour supprimer le calendrier personnalisé pour un groupe de marché (ou un marché individuel), cliquez sur **supprimer** .

> [!NOTE]
> Un marché ne peut pas appartenir à plus d’un des groupes de marché que vous utilisez dans la section **planification** .

## <a name="global-time-zones"></a>Fuseaux horaires globaux

Vous trouverez ci-dessous un tableau qui indique les fuseaux horaires spécifiques utilisés sur chaque marché. par conséquent, lorsque votre envoi utilise l’heure locale (par exemple, une version à 9 heures locales), vous pouvez déterminer l’heure à laquelle il sera publié sur chaque marché, en particulier sur les marchés qui ont plusieurs fuseaux horaires, tels que le Canada.

| Marché | Time Zone (Fuseau horaire) |
|--------|-----------|
| Afghanistan  |  (UTC+04:30) Kaboul |
| Albanie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Algérie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Samoa américaines  |  (UTC+13:00) Samoa |
| Andorre  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Angola  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Anguilla  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Antarctique  |  (UTC+12:00) Auckland, Wellington |
| Antigua-et-Barbuda  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Argentine  |  (UTC-03:00) Buenos Aires |
| Arménie  |  (UTC+04:00) Abu Dhabi, Mascate |
| Aruba  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Australie  |  (UTC+10:00) Canberra, Melbourne, Sydney |
| Autriche  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Azerbaïdjan  |  (UTC+04:00) Bakou |
| Bahamas (Les)  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Bahreïn  |  (UTC+04:00) Abu Dhabi, Mascate |
| Bangladesh  |  (UTC+06:00) Dacca |
| Barbade  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Bélarus  |  (UTC+03:00) Minsk |
| Belgique  |  (UTC+01:00) Bruxelles, Copenhague, Madrid, Paris |
| Belize  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Bénin  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Bermudes  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Bhoutan  |  (UTC+06:00) Dacca |
| République bolivarienne du Venezuela  |  (UTC-04:00) Caracas |
| Bolivie  |  (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Bonaire, Saint-Eustache et Saba  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Bosnie-Herzégovine  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Botswana  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Bouvet (Île)  |  (UTC+00:00) Monrovia, Reykjavik |
| Brésil  |  (UTC-03:00) Brasilia |
| Territoire britannique de l’Océan Indien  |  (UTC+06:00) Dacca |
| Vierges britanniques (îles)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Brunéi Darussalam  |  (UTC+08:00) Irkoutsk |
| Bulgarie  |  (UTC+02:00) Chisinau |
| Burkina Faso  |  (UTC+00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC+02:00) Harare, Pretoria |
| CÃ ́TE Côte-d’Ivoire  |  (UTC+00:00) Monrovia, Reykjavik |
| Cambodge  |  (UTC+07:00) Bangkok, Hanoï, Jakarta |
| Cameroun  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Canada  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Cap-Vert  |  (UTC-01:00) Îles du Cabo Verde |
| Cayman (îles)  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| République centrafricaine  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Tchad  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Chili  |  (UTC-04:00) Santiago |
| Chine  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Christmas (Île)  |  (UTC+07:00) Krasnoïarsk |
| Cocos-Keeling (îles)  |  (UTC+06:30) Yangon (Rangoon) |
| Colombie  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Comores (Les)  |  (UTC+03:00) Nairobi |
| Congo  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Congo (RDC)  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Cook (Îles)  |  (UTC-10:00) Hawaï |
| Costa Rica  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Croatie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| CuraÃ § AO  |  (UTC-04:00) Cuiaba |
| Chypre  |  (UTC+02:00) Chisinau |
| Tchéquie  |  (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Danemark  |  (UTC+01:00) Bruxelles, Copenhague, Madrid, Paris |
| Djibouti  |  (UTC+03:00) Nairobi |
| Dominique  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| République dominicaine  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Équateur  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Égypte  |  (UTC+02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Guinée équatoriale  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Érythrée  |  (UTC+03:00) Nairobi |
| Estonie  |  (UTC+02:00) Chisinau |
| Éthiopie  |  (UTC+03:00) Nairobi |
| Malouines (îles)  |  (UTC-04:00) Santiago |
| Féroé (îles)  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Fidji (îles)  |  (UTC+12:00) Fidji |
| Finlande  |  (UTC+02:00) Helsinki, Kiev, Riga, Sofia, Tallinn, Vilnius |
| France  |  (UTC+01:00) Bruxelles, Copenhague, Madrid, Paris |
| Guyane  |  (UTC-03:00) Cayenne, Fortaleza |
| Polynésie française  |  (UTC-10:00) Hawaï |
| Terres australes et antarctiques françaises  |  (UTC+05:00) Achgabat, Tachkent |
| Gabon  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Gambie  |  (UTC+00:00) Monrovia, Reykjavik |
| Géorgie  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Allemagne  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Ghana  |  (UTC+00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Grèce  |  (UTC+02:00) Athènes, Bucarest |
| Groenland  |  (UTC+00:00) Monrovia, Reykjavik |
| Grenade  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Guadeloupe  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Guam  |  (UTC+10:00) Guam, Port Moresby |
| Guatemala  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Guernesey  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinée  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinée-Bissau  |  (UTC+00:00) Monrovia, Reykjavik |
| Guyane  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Haïti  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Heard et McDonald (îles)  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Saint-Siège (État de la Cité du Vatican)  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Honduras  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Hong Kong (R.A.S.)  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Hongrie  |  (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Islande  |  (UTC+00:00) Monrovia, Reykjavik |
| Inde  |  (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Indonésie  |  (UTC+07:00) Bangkok, Hanoï, Jakarta |
| Irak  |  (UTC+04:00) Abu Dhabi, Mascate |
| Irlande  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Israël  |  (UTC+02:00) Jérusalem |
| Italie  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Jamaïque  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Japon  |  (UTC+09:00) Osaka, Sapporo, Tokyo |
| Jersey  |  (UTC+00:00) Monrovia, Reykjavik |
| Jordanie  |  (UTC+02:00) Chisinau |
| Kazakhstan  |  (UTC+05:00) Achgabat, Tachkent |
| Kenya  |  (UTC+03:00) Nairobi |
| Kiribati  |  (UTC+14:00) Île de Kiritimati |
| Corée du Sud  |  (UTC+09:00) Séoul |
| Koweït  |  (UTC+04:00) Abu Dhabi, Mascate |
| Kirghizistan  |  (UTC+06:00) Astana |
| Laos  |  (UTC+07:00) Bangkok, Hanoï, Jakarta |
| Lettonie  |  (UTC+02:00) Chisinau |
| Liban  |  (UTC+02:00) Chisinau |
| Lesotho  |  (UTC+02:00) Harare, Pretoria |
| Libéria  |  (UTC+00:00) Monrovia, Reykjavik |
| Libye  |  (UTC+02:00) Chisinau |
| Liechtenstein  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Lituanie  |  (UTC+02:00) Chisinau |
| Luxembourg  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Macao (R.A.S.)  |  (UTC+08:00) Beijing, Chongqing, Hong Kong, Urumqi |
| Madagascar  |  (UTC+03:00) Nairobi |
| Malawi  |  (UTC+02:00) Harare, Pretoria |
| Malaisie  |  (UTC+08:00) Kuala Lumpur, Singapour |
| Maldives  |  (UTC+05:00) Achgabat, Tachkent |
| Mali  |  (UTC+00:00) Monrovia, Reykjavik |
| Malte  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Man, île de  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Marshall (Îles)  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Ancien |
| Martinique  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Mauritanie  |  (UTC+00:00) Monrovia, Reykjavik |
| Maurice  |  (UTC+04:00) Port Louis |
| Mayotte  |  (UTC+03:00) Nairobi |
| Mexique  |  (UTC-06:00) Guadalajara, Mexico City, Monterrey |
| Micronésie  |  (UTC+10:00) Guam, Port Moresby |
| Moldova  |  (UTC+02:00) Chisinau |
| Monaco  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Mongolie  |  (UTC+07:00) Krasnoïarsk |
| Monténégro  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Montserrat  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Maroc  |  (UTC+01:00) Casablanca |
| Mozambique  |  (UTC+02:00) Harare, Pretoria |
| Myanmar  |  (UTC+06:30) Yangon (Rangoon) |
| Namibie  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Nauru  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Ancien |
| Népal  |  (UTC+05:45) Katmandou |
| Pays-Bas  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Nouvelle Calédonie  |  (UTC+11:00) Îles Salomon, Nouvelle-Calédonie |
| Nouvelle-Zélande  |  (UTC+12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Heure du Centre (États-Unis et Canada) |
| Niger  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Nigéria  |  (UTC+01:00) Afrique Centrale de l’Ouest |
| Niue  |  (UTC+13:00) Samoa |
| Norfolk (Île)  |  (UTC+11:00) Îles Salomon, Nouvelle-Calédonie |
| Macédoine du Nord  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Mariannes du Nord (Îles)  |  (UTC+10:00) Guam, Port Moresby |
| Norvège  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Oman  |  (UTC+04:00) Abu Dhabi, Mascate |
| Pakistan  |  (UTC+05:00) Islamabad, Karachi |
| Palau  |  (UTC+09:00) Osaka, Sapporo, Tokyo |
| Autorité palestinienne  |  (UTC+02:00) Chisinau |
| Panama  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Papouasie-Nouvelle-Guinée  |  (UTC+10:00) Vladivostok |
| Paraguay  |  (UTC-04:00) Asuncion |
| Pérou  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Philippines  |  (UTC+08:00) Kuala Lumpur, Singapour |
| Pitcairn (îles)  |  (UTC-08:00) Heure du Pacifique (États-Unis et Canada) |
| Pologne  |  (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Portugal  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Qatar  |  (UTC+04:00) Abu Dhabi, Mascate |
| La réunion  |  (UTC+04:00) Port Louis |
| Roumanie  |  (UTC+02:00) Chisinau |
| ROW  |  (UTC-07:00) Heure des Rocheuses (États-Unis et Canada) |
| Russie  |  (UTC+03:00) Moscou, Saint-Pétersbourg, Volgograd |
| Rwanda  |  (UTC+02:00) Harare, Pretoria |
| SÃ £ o TomÃ © et PrÃncipe  |  (UTC+00:00) Monrovia, Reykjavik |
| Saint BarthÃ © lemy  |  (UTC+04:00) Erevan |
| Sainte-Hélène, Ascension et Tristan da Cunha  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| Saint-Kitts-et-Nevis  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Sainte-Lucie  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Saint-Martin (partie française)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Saint Pierre et Miquelon  |  (UTC-02:00) Centre-Atlantique - Ancien |
| Saint-Vincent-et-les Grenadines  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Samoa  |  (UTC+13:00) Samoa |
| Saint-Marin  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Arabie saoudite  |  (UTC+03:00) Koweït, Riyad |
| Sénégal  |  (UTC+00:00) Monrovia, Reykjavik |
| Serbie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Seychelles  |  (UTC+04:00) Abu Dhabi, Mascate |
| Sierra Leone  |  (UTC+00:00) Monrovia, Reykjavik |
| Singapour  |  (UTC+08:00) Kuala Lumpur, Singapour |
| Sint-Maarten (partie néerlandaise)  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Slovaquie  |  (UTC+01:00) Belgrade, Bratislava, Budapest, Ljubljana, Prague |
| Slovénie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Salomon (Îles)  |  (UTC+11:00) Îles Salomon, Nouvelle-Calédonie |
| Somalie  |  (UTC+03:00) Nairobi |
| Afrique du Sud  |  (UTC+02:00) Harare, Pretoria |
| Géorgie du Sud et Sandwich du Sud (Îles)  |  (UTC-02:00) Centre-Atlantique - Ancien |
| Espagne  |  (UTC+01:00) Bruxelles, Copenhague, Madrid, Paris |
| Sri Lanka  |  (UTC+05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Suriname  |  (UTC-03:00) Cayenne, Fortaleza |
| Svalbard et Jan Mayen  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Swaziland  |  (UTC+02:00) Harare, Pretoria |
| Suède  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Suisse  |  (UTC+01:00) Amsterdam, Berlin, Berne, Rome, Stockholm, Vienne |
| Taïwan  |  (UTC+08:00) Taipei |
| Tadjikistan  |  (UTC+05:00) Achgabat, Tachkent |
| Tanzanie  |  (UTC+03:00) Nairobi |
| Thaïlande  |  (UTC+07:00) Bangkok, Hanoï, Jakarta |
| Timor-Leste  |  (UTC+09:00) Séoul |
| Togo  |  (UTC+00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC+13:00) Nuku’alofa |
| Tonga  |  (UTC+13:00) Nuku’alofa |
| Trinité-et-Tobago  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Tunisie  |  (UTC+01:00) Sarajevo, Skopje, Varsovie, Zagreb |
| Turquie  |  (UTC+03:00) Istanbul |
| Turkménistan  |  (UTC+05:00) Achgabat, Tachkent |
| Turks et Caicos (îles)  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Tuvalu  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Ancien |
| États-Unis Îles mineures éloignées des États-Unis  |  (UTC+13:00) Samoa |
| États-Unis Îles Vierges  |  (UTC-04:00) Heure de l’Atlantique (Canada) |
| Ouganda  |  (UTC+03:00) Nairobi |
| Ukraine  |  (UTC+02:00) Chisinau |
| Émirats arabes unis  |  (UTC+04:00) Abu Dhabi, Mascate |
| Royaume-Uni  |  (UTC+00:00) Dublin, Édimbourg, Lisbonne, Londres |
| États-Unis  |  (UTC-05:00) Heure de l’Est (États-Unis et Canada) |
| Uruguay  |  (UTC-03:00) Brasilia |
| Ouzbékistan  |  (UTC+05:00) Achgabat, Tachkent |
| Vanuatu  |  (UTC+11:00) Îles Salomon, Nouvelle-Calédonie |
| Vietnam  |  (UTC+07:00) Bangkok, Hanoï, Jakarta |
| Wallis et Futuna  |  (UTC+12:00) Petropavlovsk-Kamchatsky - Ancien |
| Yémen  |  (UTC+04:00) Abu Dhabi, Mascate |
| Zambie  |  (UTC+02:00) Harare, Pretoria |
| Zimbabwe  |  (UTC+02:00) Harare, Pretoria |
