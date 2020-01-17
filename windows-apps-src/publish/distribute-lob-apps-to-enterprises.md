---
Description: Vous pouvez publier des applications métiers directement à l’attention des entreprises pour une acquisition en volume par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation, sans mettre vos applications à la disposition générale dans le Windows Store.
title: Distribuer des applications métier aux entreprises
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp, cœur de métier, métier, applications d’entreprise, store pour entreprises, store pour éducation, entreprise
ms.localizationpriority: medium
ms.openlocfilehash: faf750ece274776a147dff9e825f32534eb13014
ms.sourcegitcommit: 7a8aea567b26283c71420e0d305d78f675e1fba7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125671"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuer des applications métier aux entreprises

Vous avez plusieurs options pour distribuer des applications métier aux utilisateurs de votre organisation à l’aide de [packages MSIX](https://docs.microsoft.com/windows/msix/) sans rendre les applications largement disponibles pour le public. Vous pouvez utiliser les outils de gestion des appareils, configurer un déploiement basé sur un programme d’installation d’application, chargement les applications directement, ou publier les applications sur le Microsoft Store pour les entreprises ou Microsoft Store pour l’éducation.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Configuration Manager de point de terminaison Microsoft et Microsoft Intune

Si votre organisation utilise le point de terminaison Microsoft Configuration Manager ou Microsoft Intune pour gérer les appareils, vous pouvez déployer des applications métier à l’aide de ces outils. Pour plus d’informations, voir ces articles :

* [Présentation de la gestion d’applications dans Configuration Manager](https://docs.microsoft.com/configmgr/apps/understand/introduction-to-application-management)
* [Vue d’ensemble du cycle de vie des applications dans Microsoft Intune](https://docs.microsoft.com/intune/apps/app-lifecycle)

## <a name="app-installer"></a>Programme d’installation d’application

Le programme d’installation de l’application permet d’installer les applications Windows 10 en double-cliquant directement sur un package d’application MSIX ou en double-cliquant sur un fichier. appinstaller qui installe le package d’application à partir d’un serveur Web. Cela signifie que les utilisateurs n’ont pas besoin d’utiliser PowerShell ou d’autres outils de développement pour installer des applications métier. Le programme d’installation d’application peut également installer des packages d’application qui incluent des packages facultatifs et des jeux associés.

Vous pouvez télécharger Programme d’installation d’application pour une utilisation hors connexion au sein de l’entreprise à partir du [portail web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) de Microsoft Store pour Entreprises. Pour plus d’informations sur le programme d’installation d’application, consultez [installer des applications Windows 10 avec le programme d’installation d’application](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Chargement indépendant

Une autre option pour distribuer des applications métier directement aux utilisateurs de votre organisation est chargement. Cette option est similaire au déploiement basé sur l’installation de l’application, car elle permet aux utilisateurs d’installer directement des packages d’application MSIX. À compter de Windows 10 version 2004, chargement est activé par défaut et les utilisateurs peuvent installer des applications en double-cliquant sur les packages d’application MSIX signés. Sur Windows 10 version 1909 et les versions antérieures, chargement requiert une configuration supplémentaire et l’utilisation d’un script PowerShell. Pour plus d’informations, consultez l’article [Chargement indépendant d’applications métier dans Windows 10](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store pour les entreprises ou les Microsoft Store pour l’éducation

Vous pouvez publier des applications métiers directement à l’attention des entreprises pour une acquisition en volume par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation, sans mettre vos applications à la disposition générale dans le Windows Store. Quand vous utilisez cette option, les applications sont signées par le Windows Store et doivent être conformes aux stratégies standard du magasin.

> [!NOTE]
> Pour l’instant, seules les applications gratuites peuvent être distribuées de façon exclusive aux entreprises par le biais de Microsoft Store pour Entreprises ou de Microsoft Store pour Éducation. Si vous soumettez une application payante en tant qu’application métier, elle ne sera pas disponible pour l’entreprise. 

> [!IMPORTANT]
> Vous ne pouvez pas utiliser l'[API de soumission au Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) pour publier des applications métier directement à destination des entreprises. Toutes les soumissions pour les applications métier doivent être publiées via l’espace partenaires.

### <a name="set-up-the-enterprise-association"></a>Configurer l’association d’entreprise

Lorsque vous envisagez de publier des applications métier exclusivement à l’attention d’une entreprise, la première étape consiste à établir l’association entre votre compte et le magasin privé de l’entreprise.

> [!IMPORTANT]
> Ce processus d’association doit être lancé par l’entreprise et doit utiliser l’adresse de messagerie associée au compte Microsoft qui a été utilisé pour créer le compte de développeur. Pour plus d’informations, voir [Utilisation des applications métier](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps).

Lorsqu'une entreprise vous invite à publier des applications destinées à son utilisation exclusive, vous recevez un e-mail contenant un lien pour confirmer l'association. Vous pouvez également confirmer ces associations en accédant à la section **Associations d’entreprise** de vos **Paramètres de compte** (pour autant que vous êtes connecté avec le compte Microsoft qui a été utilisé pour ouvrir le compte de développeur).

Pour confirmer l'association, cliquez sur **Accepter**. Votre compte pourra désormais publier des applications destinées à une utilisation exclusive par l'entreprise.

### <a name="submit-lob-apps"></a>Soumettre des applications métiers

Lorsque vous êtes prêt à publier une application destinée à une utilisation exclusive par une entreprise, vous suivez un processus similaire au processus de soumission d'application. L’application est soumise au même [processus de certification](the-app-certification-process.md) et doit être conforme à l’ensemble des [stratégies du Microsoft Store](store-policies.md). Seuls quelques aspects du processus sont différents.

#### <a name="visibility"></a>Visibilité

Une fois que vous avez configuré une association d’entreprise, chaque fois que vous soumettez une application, une zone de liste déroulante s’affiche dans la section **Visibilité** de la page **Tarification et disponibilité** de la soumission. La sélection par défaut est **Vente au détail**. Pour mettre l’application à la disposition exclusive d’une entreprise, vous devez choisir **Distribution d’applications métier**.

Une fois l’option **Distribution d’applications métier** sélectionnée, les options **Visibilité** habituelles sont remplacées par une liste des entreprises pour lesquelles vous pouvez publier des applications exclusives. Personne en dehors des entreprises que vous sélectionnez ne sera en mesure de visualiser ou télécharger l’application.

Vous devez sélectionner au moins une entreprise pour publier une application en tant qu’application cœur de métier.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Gestion des licences organisationnelles

Par défaut, la case **Proposer mon application aux organisations via le service de gestion de licences en volume (en ligne) du Store** est cochée lorsque vous soumettez une application. Lorsque vous publiez des applications métiers, cette case doit rester cochée afin que l’entreprise puisse acquérir votre application en volume. Personne ne pourra accéder à l’application, hormis les entreprises que vous avez sélectionnées dans la section **Distribution et visibilité**.

Si vous souhaitez mettre l’application à disposition de l’entreprise à l’aide de licences en mode hors connexion, vous pouvez également cocher la case **Permettre aux entreprises d’acheter des licences en mode hors connexion**.

Pour plus d’informations, voir [Options de gestion des licences organisationnelles](organizational-licensing.md).

#### <a name="age-ratings"></a>Classification par âge

Pour les applications métier, la [classification par âge](age-ratings.md) du processus de soumission fonctionne comme pour les applications commerciales. Toutefois, une option supplémentaire vous permet d’indiquer manuellement la classification par âge de votre application dans le Windows Store au lieu de remplir le questionnaire ou d’importer un identificateur de classification IARC existant. Cette évaluation manuelle n’est utilisable qu’avec la distribution métier ; par conséquent, si vous redéfinissez le paramètre **Visibilité** de l’application sur **Vente au détail**, vous devrez répondre au questionnaire d’évaluation de l’âge avant de pouvoir publier la soumission.

### <a name="enterprise-deployment-of-lob-apps"></a>Déploiement d’applications métier dans les entreprises

Lorsque vous cliquez sur **Envoyer au Store**, le processus de certification de l'application s'exécute. À l’issue de ce processus, un administrateur de l’entreprise doit ajouter l’application à son magasin privé dans le portail Microsoft Store pour Entreprises ou Microsoft Store pour Éducation. L’entreprise peut alors déployer l’application à l’attention de ses utilisateurs.

> [!NOTE]
> Pour obtenir votre application métier, l’organisation doit se trouver dans un [marché pris en charge](https://docs.microsoft.com/windows/whats-new/windows-store-for-business-overview#supported-markets), et vous ne devez pas avoir [exclu ce marché](define-pricing-and-market-selection.md) lorsque vous avez soumis l’application. 

Pour plus d’informations, consultez les articles [Utilisation des applications cœur de métier](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps) et [Distribuer des applications à l’aide de votre magasin privé](https://docs.microsoft.com/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Mettre à jour des applications métiers

Pour publier les mises à jour d’une application que vous avez déjà publiée en tant qu’application métier, il vous suffit de créer une autre soumission. Vous pouvez charger de nouveaux packages ou apporter des modifications, puis cliquer sur **Envoyer au Store** pour mettre à disposition la version mise à jour. Veillez à ce que les sélections d’entreprises dans **Visibilité** restent les mêmes, sauf si vous souhaitez leur apporter des modifications, par exemple en sélectionnant une autre entreprise pouvant acquérir l’application ou en supprimant l’une des entreprises auxquelles vous l’avez déjà distribuée.

Si vous souhaitez ne plus offrir une application que vous avez déjà publiée en tant qu’application métier et que vous souhaitez empêcher toute nouvelle acquisition, vous devez créer une soumission. En premier lieu, vous devez modifier votre sélection sous **Visibilité** en choisissant **Vente au détail** au lieu de **Distribution d’applications métier**. Puis, dans la section [Détectabilité](choose-visibility-options.md#discoverability), choisissez **Rendre ce produit disponible mais non détectable dans le Store** avec l’option **Empêcher l’acquisition**.

Une fois le processus de certification appliqué à la soumission, l’application n’est plus disponible pour de nouvelles acquisitions (les personnes qui en disposent déjà pourront cependant continuer à l’utiliser).

> [!NOTE]
> Si vous redéfinissez une application sur **Vente au détail**, vous devrez répondre au [questionnaire d’évaluation de l’âge](age-ratings.md) si vous ne l’avez pas encore fait, même si l’application n’est pas disponible pour de nouvelles acquisitions.