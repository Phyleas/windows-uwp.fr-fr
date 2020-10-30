---
description: Le Microsoft Store Services SDK contient des bibliothèques et des outils qui vous permettent de doter vos applications de fonctionnalités conçues pour vous aider à générer plus de revenus et conquérir des clients.
title: Impliquer les clients avec Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.date: 08/21/2017
ms.topic: article
keywords: Kit de développement logiciel (SDK) Windows 10, UWP, Microsoft Store services
ms.localizationpriority: medium
ms.openlocfilehash: 8356367b47242f7bda01da753cc8599aff9edd79
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030532"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Impliquer les clients avec Microsoft Store Services SDK

Le kit de développement logiciel (SDK) Microsoft Store Services fournit des fonctionnalités qui vous aident à prendre en main des clients dans vos applications de plateforme Windows universelle (UWP), telles que l’envoi de notifications ciblées à vos applications et l’exécution d’expériences/B dans vos applications. Ce kit de développement logiciel (SDK) est une extension pour Visual Studio 2015 et d’autres versions de Visual Studio.

> [!NOTE]
> Pour afficher les publicités dans vos applications UWP, utilisez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) au lieu du kit de développement logiciel (sdk) Microsoft Store services. Les bibliothèques de publication ont été déplacées du kit de développement logiciel (SDK) Microsoft Store services vers le kit de développement logiciel Microsoft Advertising. Pour plus d’informations, voir [Afficher des publicités dans votre application](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Scénarios pris en charge par le kit de développement logiciel (SDK) Microsoft Store services

Le kit de développement logiciel (SDK) Microsoft Store Services prend actuellement en charge les scénarios suivants pour les applications UWP. Pour obtenir une documentation de référence sur les API, consultez informations de référence sur l' [API SDK Microsoft Store services](/uwp/api/overview/engagement).

|  Scénario  |  Description   |
|------------|----------------|
|  [Exécuter des expériences dans votre application UWP avec des tests A/B](run-app-experiments-with-a-b-testing.md)    |  Exécutez des tests A/B dans vos applications de plateforme Windows universelle (UWP) pour évaluer l’efficacité de fonctionnalités spécifiques auprès de certains clients avant de les mettre à la disposition de tous. Une fois que vous avez défini une expérience dans l’espace partenaires, utilisez la classe [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) pour obtenir des variantes de votre expérience dans votre application, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis utilisez la méthode [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) pour envoyer des événements d’affichage et de conversion à l’espace partenaires. Enfin, utilisez l’espace partenaires pour afficher les résultats et gérer l’expérience.  |
|  [Lancer le Hub de commentaires à partir de votre application UWP](launch-feedback-hub-from-your-app.md)    |  Utilisez la classe [StoreServicesFeedbackLauncher](/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) dans votre application UWP pour diriger vos clients Windows 10 vers le Hub de commentaires, qui leur permettra de soumettre leurs problèmes, suggestions et votes pour. Gérez ensuite ces commentaires dans le [Rapport sur les commentaires](../publish/feedback-report.md) affiché dans l’Espace partenaires. |
|  [Configurer votre application UWP pour recevoir des notifications push de l’espace partenaires](configure-your-app-to-receive-dev-center-notifications.md)    |  Utilisez la classe [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) dans votre application UWP pour inscrire votre application afin de recevoir des notifications push ciblées que vous envoyez à vos clients à l’aide de l’espace partenaires.  |
|   [Consigner les événements personnalisés dans votre application UWP pour le rapport d’utilisation dans l’espace partenaires](log-custom-events-for-dev-center.md)   |  Utilisez la classe [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) dans votre application UWP pour consigner les événements personnalisés associés à votre application dans l’espace partenaires. Ensuite, examinez le nombre total d’occurrences de vos événements personnalisés dans la section **événements personnalisés** du [rapport d’utilisation](../publish/usage-report.md) dans l’espace partenaires.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Prérequis

Le Microsoft Store Services SDK nécessite les éléments suivants :

* Visual Studio 2015 ou une version ultérieure.
* Visual Studio Tools pour applications Windows universelles installé avec votre version de Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Installer le Kit de développement logiciel (SDK)

Il existe deux options pour installer le kit de développement logiciel (SDK) Microsoft Store services sur votre ordinateur de développement :

* **Programme d’installation MSI** &nbsp; &nbsp; Vous pouvez installer le kit de développement logiciel (SDK) via le programme d’installation MSI disponible [ici](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK).
* **Package NuGet** &nbsp; &nbsp; Vous pouvez installer le kit de développement logiciel (SDK) en tant que package NuGet.

Microsoft publie régulièrement de nouvelles versions du Microsoft Store Services SDK avec des améliorations des performances et de nouvelles fonctionnalités. Si vous possédez des projets existants qui utilisent le SDK et que vous souhaitez utiliser la dernière version, téléchargez et installez la dernière version du SDK sur votre ordinateur de développement.

<span id="install-msi" />

### <a name="install-via-msi"></a>Installer via MSI

Pour installer le Microsoft Store Services SDK via le programme d’installation MSI :

1.  Fermez toutes les instances de Visual Studio.

2. Si vous avez déjà installé le kit de développement logiciel (SDK) Microsoft Store engagement et monétisation, le kit de développement logiciel (SDK) client universel ou l’extension ad médiateur, désinstallez ces kits d’installation. Si vous le souhaitez, ouvrez une fenêtre d' **invite de commandes** et exécutez ces commandes pour nettoyer toutes les anciennes versions du SDK qui ont pu être installées avec Visual Studio, mais qui peuvent ne pas apparaître dans la liste des programmes installés sur votre ordinateur :
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Téléchargez et installez le [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Démarrez Visual Studio.

5.  Si vous disposez d’un projet qui fait référence à des bibliothèques à partir de n’importe quelle version antérieure du kit de développement logiciel (SDK) Microsoft Store services, Microsoft Advertising SDK, le kit de développement logiciel (SDK) client universel ou Microsoft Store Kit de développement logiciel (SDK), nous vous recommandons d’ouvrir votre projet dans Visual Studio et de nettoyer et de régénérer votre projet (dans **Explorateur de solutions** , de cliquer avec le bouton droit sur le nœud de votre projet et **de sélectionner** **nettoyer** , puis de cliquer à nouveau avec le bouton droit sur le nœud de votre projet et de

  Dans le cas contraire, si vous utilisez le kit de développement logiciel (SDK) pour la première fois dans votre projet, vous êtes maintenant prêt à [Ajouter la référence d’assembly à votre projet](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Installer via NuGet

Pour installer les bibliothèques du kit de développement logiciel (SDK) Microsoft Store services via NuGet :

1.  Fermez toutes les instances de Visual Studio.

2. Si vous avez déjà installé le kit de développement logiciel (SDK) Microsoft Store engagement et monétisation, le kit de développement logiciel (SDK) client universel ou l’extension ad médiateur, désinstallez ces kits d’installation. Si vous le souhaitez, ouvrez une fenêtre d' **invite de commandes** et exécutez ces commandes pour nettoyer toutes les anciennes versions du SDK qui ont pu être installées avec Visual Studio, mais qui peuvent ne pas apparaître dans la liste des programmes installés sur votre ordinateur :
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Démarrez Visual Studio et ouvrez le projet dans lequel vous souhaitez utiliser le kit de développement logiciel (SDK) Microsoft Store services.
    > [!NOTE]
    > Si votre projet contient déjà des références de bibliothèque d’une installation MSI antérieure du kit de développement logiciel (SDK), supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans Visual Studio, cliquez sur **Projet** et sur **Gérer les packages NuGet** .

5. Dans la zone de recherche, tapez **Microsoft. services. Store. engagement** et installez le package Microsoft. services. Store. engagement. Une fois l’installation du package terminée, enregistrez votre solution.
    > [!NOTE]
    > Si la fenêtre **sortie** signale une erreur *Install-Package* qui indique que le chemin d’accès spécifié est trop long, vous devrez peut-être configurer NuGet pour extraire les packages vers un autre emplacement avec un chemin d’accès plus petit que l’emplacement par défaut. Pour ce faire, ajoutez la valeur `repositoryPath` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](/nuget/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projet Visual Studio vers un dossier différent présentant un chemin plus court. Le problème peut également être dû à un chemin d’accès aux packages globaux trop long. Dans ce cas, ajoutez la `globalPackagesFolder` valeur dans votre fichier nuget.config.

6. Fermez la solution Visual Studio qui contient votre projet, puis rouvrez la solution.

7.  Si votre projet référence déjà des bibliothèques d’une version antérieure du Microsoft Store Services SDK installée via NuGet et que vous avez mis à jour votre projet vers une version plus récente du SDK, nous vous recommandons de nettoyer et de régénérer votre projet (dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le nœud de votre projet, puis sélectionnez **Nettoyer** , cliquez de nouveau avec le bouton droit sur le nœud de projet, puis sélectionnez **Régénérer** ).

  Dans le cas contraire, si vous utilisez le kit de développement logiciel (SDK) pour la première fois dans votre projet, vous êtes maintenant prêt à [Ajouter la référence d’assembly à votre projet](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Ajouter la référence d’assembly à votre projet

Après avoir installé le kit de développement logiciel (SDK) Microsoft Store services via le programme d’installation MSI ou NuGet, suivez ces instructions pour référencer l’assembly du kit de développement logiciel (SDK) dans votre projet UWP.

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si votre projet est une application JavaScript qui cible **n’importe quel processeur** , mettez à jour votre projet pour utiliser une sortie de génération spécifique à l’architecture (par exemple, **x86** ).

2. Dans **l’Explorateur de solutions** , cliquez avec le bouton droit sur **Références** , puis cliquez sur **Ajouter une référence** .

3. Dans **le gestionnaire de références** , développez **Windows universel** , cliquez sur **Extensions** , puis activez la case à cocher en regard de **Microsoft engagement Framework** . Cela vous permet d’utiliser les API de l’espace de noms [Microsoft. services. Store. engagement](/uwp/api/microsoft.services.store.engagement) .

3. Cliquez sur **OK** .

> [!NOTE]
> Si vous avez installé les bibliothèques du kit de développement logiciel (SDK) via NuGet, votre projet contiendra une référence **Microsoft. services. Store. engagement** . La référence **Microsoft. services. Store. engagement** représente le package NuGet (plutôt que les bibliothèques qu’elle contient) et vous pouvez l’ignorer.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Présentation des packages d’infrastructure dans le SDK

La bibliothèque Microsoft.Services.Store.Engagement.dll dans le kit de développement logiciel (SDK) Microsoft Store services est configurée en tant que *package d’infrastructure* . Cette bibliothèque contient les API de l’espace de noms [Microsoft.Services.Store.Engagement](/uwp/api/microsoft.services.store.engagement).

Étant donné que cette bibliothèque est un package d’infrastructure, cela signifie qu’après l’installation par un utilisateur d’une version de votre application qui utilise cette bibliothèque, cette bibliothèque est automatiquement mise à jour sur son appareil via Windows Update chaque fois que nous publions une nouvelle version de la bibliothèque avec des correctifs et des améliorations des performances. Cela permet de s’assurer que vos clients disposent toujours de la dernière version disponible de la bibliothèque installée sur leurs appareils.

Si vous publiez une nouvelle version du kit de développement logiciel (SDK) qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du kit de développement logiciel (SDK) pour utiliser ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le Windows Store.

## <a name="related-topics"></a>Rubriques connexes

* [Informations de référence sur les API du Microsoft Store Services SDK](/uwp/api/overview/engagement)
* [Exécuter des expériences avec des tests A/B](run-app-experiments-with-a-b-testing.md)
* [Lancer le Hub de commentaires à partir de votre application](launch-feedback-hub-from-your-app.md)
* [Configurer votre application pour recevoir des notifications Push de l’Espace partenaires](configure-your-app-to-receive-dev-center-notifications.md)
* [Journaliser des événements personnalisés pour l’Espace partenaires](log-custom-events-for-dev-center.md)
