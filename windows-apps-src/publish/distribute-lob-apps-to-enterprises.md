---
Description: Vous pouvez publier des applications métier directement dans des entreprises pour une acquisition en volume via le Microsoft Store pour l’entreprise ou des Microsoft Store pour l’éducation, sans mettre les applications à disposition générale dans le Store.
title: Distribuer des applications métier aux entreprises
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: Windows 10, UWP, LOB, métier, applications d’entreprise, Store pour entreprises, Store pour l’éducation, entreprise
ms.localizationpriority: medium
ms.openlocfilehash: 9fccf3cab82724f12789131b8450795201e0fba6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161943"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuer des applications métier aux entreprises

Vous avez plusieurs options pour distribuer des applications métier aux utilisateurs de votre organisation à l’aide de [packages MSIX](/windows/msix/) sans rendre les applications largement disponibles pour le public. Vous pouvez utiliser les outils de gestion des appareils, configurer un déploiement basé sur un programme d’installation d’application, chargement les applications directement, ou publier les applications sur le Microsoft Store pour les entreprises ou Microsoft Store pour l’éducation.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Configuration Manager de point de terminaison Microsoft et Microsoft Intune

Si votre organisation utilise le point de terminaison Microsoft Configuration Manager ou Microsoft Intune pour gérer les appareils, vous pouvez déployer des applications métier à l’aide de ces outils. Pour plus d’informations, voir les articles suivants :

* [Présentation de la gestion d’applications dans Configuration Manager](/configmgr/apps/understand/introduction-to-application-management)
* [Vue d’ensemble du cycle de vie des applications dans Microsoft Intune](/intune/apps/app-lifecycle)

## <a name="app-installer"></a>Programme d’installation d’application

Le programme d’installation de l’application permet d’installer les applications Windows 10 en double-cliquant directement sur un package d’application MSIX ou en double-cliquant sur un fichier. appinstaller qui installe le package d’application à partir d’un serveur Web. Cela signifie que les utilisateurs n’ont pas besoin d’utiliser PowerShell ou d’autres outils de développement pour installer des applications métier. Le programme d’installation d’application peut également installer des packages d’application qui incluent des packages facultatifs et des jeux associés.

Vous pouvez télécharger Programme d’installation d’application pour une utilisation hors connexion au sein de l’entreprise à partir du [portail web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) de Microsoft Store pour Entreprises. Pour plus d’informations sur le programme d’installation d’application, consultez [installer des applications Windows 10 avec le programme d’installation d’application](/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Chargement indépendant

Une autre option pour distribuer des applications métier directement aux utilisateurs de votre organisation est chargement. Cette option est similaire au déploiement basé sur l’installation de l’application, car elle permet aux utilisateurs d’installer directement des packages d’application MSIX. À compter de Windows 10 version 2004, chargement est activé par défaut et les utilisateurs peuvent installer des applications en double-cliquant sur les packages d’application MSIX signés. Sur Windows 10 version 1909 et les versions antérieures, chargement requiert une configuration supplémentaire et l’utilisation d’un script PowerShell. Pour plus d'informations, voir [Charger la version test d'applications métier dans Windows 10](/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store pour les entreprises ou les Microsoft Store pour l’éducation

Vous pouvez publier des applications métier directement dans des entreprises pour une acquisition en volume via des Microsoft Store pour l’entreprise ou des Microsoft Store pour l’éducation, sans mettre les applications à disposition générale dans le Store. Quand vous utilisez cette option, les applications sont signées par le Windows Store et doivent être conformes aux stratégies standard du magasin.

> [!NOTE]
> Pour le moment, seules les applications gratuites peuvent être distribuées exclusivement aux entreprises par le biais de Microsoft Store for Business ou Microsoft Store for Education. Si vous soumettez une application payante comme LOB, elle ne sera pas disponible pour l’entreprise. 

> [!IMPORTANT]
> Vous ne pouvez pas utiliser l' [API Microsoft Store soumission](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour publier des applications métier directement dans les entreprises. Toutes les soumissions pour les applications métier doivent être publiées via l’espace partenaires.

### <a name="set-up-the-enterprise-association"></a>Configurer l’Association d’entreprise

Lorsque vous envisagez de publier des applications métier exclusivement à l’attention d’une entreprise, la première étape consiste à établir l’association entre votre compte et le magasin privé de l’entreprise.

> [!IMPORTANT]
> Ce processus d’association doit être initié par l’entreprise et doit utiliser l’adresse de messagerie associée au compte Microsoft utilisé pour créer le compte de développeur. Pour plus d’informations, voir [Utilisation des applications métier](/microsoft-store/working-with-line-of-business-apps).

Lorsqu'une entreprise vous invite à publier des applications destinées à son utilisation exclusive, vous recevez un e-mail contenant un lien pour confirmer l'association. Vous pouvez également confirmer ces associations en accédant à la section **associations d’entreprise** de vos **paramètres de compte** (à condition que vous soyez connecté avec le compte Microsoft utilisé pour ouvrir le compte de développeur).

Pour confirmer l'association, cliquez sur **Accepter**. Votre compte pourra désormais publier des applications destinées à une utilisation exclusive par l'entreprise.

### <a name="submit-lob-apps"></a>Envoyer des applications métier

Lorsque vous êtes prêt à publier une application destinée à une utilisation exclusive par une entreprise, vous suivez un processus similaire au processus de soumission d'application. L’application passe par le même [processus de certification](the-app-certification-process.md)et doit se conformer à toutes les [stratégies de Microsoft Store](store-policies.md). Seuls quelques aspects du processus sont différents.

#### <a name="visibility"></a>Visibilité

Une fois que vous avez configuré une association d’entreprise, chaque fois que vous soumettez une application, une zone de liste déroulante s’affiche dans la section **visibilité** de la page **tarification et disponibilité** de la soumission. La sélection par défaut est **Vente au détail**. Pour mettre l’application à la disposition exclusive d’une entreprise, vous devez choisir **Distribution d’applications métier**.

Une fois la **distribution métier** sélectionnée, les options de **visibilité** habituelles sont remplacées par la liste des entreprises sur lesquelles vous pouvez publier des applications exclusives. Personne en dehors de la ou des entreprises que vous sélectionnez ne pourra afficher ou télécharger l’application.

Vous devez sélectionner au moins une entreprise afin de publier une application en tant qu’application métier.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, la case **Proposer mon application aux organisations via le service de gestion de licences en volume (en ligne) du Store** est cochée lorsque vous soumettez une application. Lors de la publication d’applications métier, cette case doit rester activée pour que l’entreprise puisse acquérir votre application en volume. Cela ne rend pas l’application disponible pour quiconque en dehors de l’entreprise que vous avez sélectionnée dans la section **distribution et visibilité** .

Si vous souhaitez mettre l’application à disposition de l’entreprise à l’aide de licences en mode hors connexion, vous pouvez également cocher la case **Permettre aux entreprises d’acheter des licences en mode hors connexion**.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

#### <a name="age-ratings"></a>Classification par âge

Pour les applications métier, la [classification par âge](age-ratings.md) du processus de soumission fonctionne comme pour les applications commerciales. Toutefois, une option supplémentaire vous permet d’indiquer manuellement la classification par âge de votre application dans le Windows Store au lieu de remplir le questionnaire ou d’importer un identificateur de classification IARC existant. Cette évaluation manuelle ne peut être utilisée qu’avec la distribution LOB. par conséquent, si vous modifiez le paramètre de **visibilité** de l’application en **distribution de vente au détail**, vous devez prendre le questionnaire de classement chronologique pour pouvoir publier l’envoi.

### <a name="enterprise-deployment-of-lob-apps"></a>Déploiement d’applications métier dans les entreprises

Lorsque vous cliquez sur **Envoyer au Store**, le processus de certification de l'application s'exécute. Une fois qu’elle est prête, un administrateur de l’entreprise doit l’ajouter à son magasin privé dans le Microsoft Store for Business ou Microsoft Store for Education Portal. L'entreprise peut alors déployer l'application à l'attention de ses utilisateurs.

> [!NOTE]
> Pour obtenir votre application métier, l’organisation doit se trouver sur un [marché pris en charge](/windows/whats-new/windows-store-for-business-overview#supported-markets)et vous ne devez pas avoir [exclu ce marché](./define-market-selection.md) lors de la soumission de votre application. 

Pour plus d'informations, voir [Utilisation des applications métier](/microsoft-store/working-with-line-of-business-apps) et [Distribuer des applications à l'aide de votre magasin privé](/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Mettre à jour les applications métier

Pour publier les mises à jour d'une application que vous avez déjà publiée en tant qu'application métier, il vous suffit de créer une soumission. Vous pouvez transférer de nouveaux packages ou apporter des modifications, puis cliquer sur **Envoyer au Store** pour mettre à disposition la version mise à jour. Veillez à conserver les sélections de l’entreprise **de la même** façon, sauf si vous souhaitez apporter des modifications, par exemple en sélectionnant une entreprise supplémentaire pour acquérir l’application, ou en supprimant l’une des entreprises pour lesquelles vous l’avez précédemment distribuée.

Si vous souhaitez ne plus offrir une application que vous avez déjà publiée en tant qu’application métier et que vous souhaitez empêcher toute nouvelle acquisition, vous devez créer une soumission. Tout d’abord, vous devez modifier votre sélection de **visibilité** de la **distribution métier** à la **distribution de détail**. Ensuite, dans la section [détectabilité](choose-visibility-options.md#discoverability) , choisissez **rendre ce produit disponible mais non détectable dans le magasin** avec l’option arrêter l' **acquisition** .

Une fois le processus de certification appliqué à la soumission, l’application n’est plus disponible pour de nouvelles acquisitions (les personnes qui en disposent déjà pourront cependant continuer à l’utiliser).

> [!NOTE]
> Lorsque vous modifiez une application pour une **distribution de vente au détail**, vous devez compléter le questionnaire sur les évaluations de l' [âge](age-ratings.md) si vous ne l’avez pas déjà fait, même si l’application n’est pas disponible pour de nouvelles acquisitions.