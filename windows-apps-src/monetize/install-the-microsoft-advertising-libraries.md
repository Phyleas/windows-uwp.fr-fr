---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Découvrez comment installer le kit de développement logiciel (SDK) Microsoft Advertising pour afficher des publicités dans des applications plateforme Windows universelle (UWP) pour Windows 10.
title: Installer le SDK Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, installation, kit de développement logiciel (SDK), bibliothèque de publication
ms.localizationpriority: medium
ms.openlocfilehash: a7ec56281c5f1d441d3808fa91491d0d290018f3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155493"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Installer le SDK Microsoft Advertising

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Pour afficher les publicités dans vos applications UWP pour Windows 10, installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Ce kit de développement logiciel (SDK) est une extension de Visual Studio 2015 et des versions ultérieures.

> [!NOTE]
> Si vous développez une application UWP JavaScript/HTML et que vous avez installé le kit de développement logiciel (SDK) Windows 10 version 10.0.14393 (mise à jour anniversaire) ou une version ultérieure, vous devez également installer la bibliothèque [WinJS](https://github.com/winjs/winjs) . Cette bibliothèque a été utilisée dans les versions précédentes du kit de développement logiciel (SDK) Windows 10, mais à partir de la version 10.0.14393 du kit de développement logiciel (SDK) Windows 10 (mise à jour anniversaire), cette bibliothèque doit être installée séparément.

<span id="install-msi" />

## <a name="install-via-msi"></a>Installer via MSI

Pour installer le kit de développement logiciel (SDK) Microsoft Advertising via le programme d’installation MSI :

1.  Fermez toutes les instances de Visual Studio.

2. Si vous aviez précédemment installé une version antérieure du Kit de développement Microsoft Advertising, du kit Microsoft Universal Ad Client, de l’extension Ad Mediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant. Si vous le souhaitez, ouvrez une fenêtre d' **invite de commandes** et exécutez ces commandes pour nettoyer toutes les anciennes versions du SDK de publication qui ont pu être installées avec Visual Studio, mais qui peuvent ne pas apparaître dans la liste des programmes installés sur votre ordinateur :
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Téléchargez et installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). L’installation peut prendre quelques minutes. Attendez la fin du processus.

4.  Démarrez Visual Studio.

5.  Si vous disposez d’un projet qui fait référence à des bibliothèques publicitaires à partir de n’importe quelle version antérieure du kit de développement logiciel (SDK) Microsoft Advertising, du kit de développement logiciel (SDK) client universel ou du kit de développement logiciel (SDK) Microsoft Store engagement et monétisation, nous vous recommandons d’ouvrir votre projet dans Visual Studio et de régénérer votre **projet, puis** **Explorateur de solutions**de cliquer sur **régénérer**

  Dans le cas contraire, si vous utilisez le kit de développement logiciel (SDK) Microsoft Advertising pour la première fois dans votre projet, vous êtes maintenant prêt à [Ajouter une référence au kit de développement logiciel (SDK) Microsoft Advertising](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Installer via NuGet

Pour installer le kit de développement logiciel (SDK) Microsoft Advertising dans un projet UWP spécifique via NuGet :

1.  Fermez toutes les instances de Visual Studio.

2.  Si vous aviez précédemment installé une version antérieure du Kit de développement Microsoft Advertising, du kit Microsoft Universal Ad Client, de l’extension Ad Mediator ou du SDK d’engagement et de monétisation de la Boutique Microsoft, désinstallez ces versions maintenant. Si vous le souhaitez, ouvrez une fenêtre d' **invite de commandes** et exécutez ces commandes pour nettoyer toutes les anciennes versions du SDK de publication qui ont pu être installées avec Visual Studio, mais qui peuvent ne pas apparaître dans la liste des programmes installés sur votre ordinateur :
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Démarrez Visual Studio et ouvrez le projet dans lequel vous souhaitez utiliser le kit de développement logiciel (SDK) Microsoft Advertising.
    > [!NOTE]
    > Si votre projet contient déjà des références de bibliothèque d’une installation MSI antérieure du kit de développement logiciel (SDK), supprimez ces références de votre projet. Des icônes s’afficheront en regard de ces références, car les bibliothèques auxquelles elles sont associées ont été supprimées au cours des étapes précédentes.

4. Dans Visual Studio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

5. Dans la zone de recherche, tapez **Microsoft. Advertising. Xaml** (pour un projet XAML) ou **Microsoft.Advertising.JS** (pour un projet JavaScript/html) et installez le package correspondant. Une fois l’installation du package terminée, enregistrez votre solution.
    > [!NOTE]
    > Si la fenêtre **sortie** signale une erreur *Install-Package* qui indique que le chemin d’accès spécifié est trop long, vous devrez peut-être configurer NuGet pour extraire les packages vers un autre emplacement avec un chemin d’accès plus petit que l’emplacement par défaut. Pour ce faire, ajoutez la valeur `repositoryPath` à un fichier nuget.config sur votre ordinateur, puis affectez-la à un chemin court de dossier, dans lequel extraire les packages. Pour plus d’informations, consultez [cet article](/nuget/consume-packages/configuring-nuget-behavior) de la documentation NuGet. Sinon, vous pouvez essayer de déplacer votre projet Visual Studio vers un dossier différent présentant un chemin plus court.

6. Fermez votre solution, puis rouvrez-la.

7.  Si votre projet fait déjà référence à des bibliothèques d’une version antérieure du kit de développement logiciel (SDK) Microsoft Advertising qui a été installée par le biais de NuGet et que vous avez mis à jour votre projet vers une version plus récente du kit de développement logiciel (SDK), nous vous recommandons **de nettoyer et**de **régénérer votre**projet (dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de votre projet

  Dans le cas contraire, si vous utilisez le kit de développement logiciel (SDK) pour la première fois dans votre projet, vous êtes maintenant prêt à [Ajouter une référence au kit de développement logiciel (SDK) Microsoft Advertising](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Ajouter une référence au kit de développement logiciel (SDK) Microsoft Advertising

Après avoir installé le kit de développement logiciel (SDK) Microsoft Advertising, suivez ces instructions pour référencer le kit de développement logiciel (SDK) dans votre projet afin de pouvoir utiliser les API de publicité.

1. Ouvrez votre projet dans Visual Studio.
    > [!NOTE]
    > Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **un processeur**, vous ne serez pas en mesure d’ajouter une référence au kit de développement logiciel (SDK) Microsoft Advertising dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

2. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence**.

3. Dans le **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis activez la case à cocher en regard de **Microsoft Advertising Kit de développement logiciel (SDK) pour XAML** (pour les applications XAML) ou **Microsoft Advertising Kit de développement logiciel (SDK** ) pour JavaScript (pour les applications générées en JavaScript et html).

4.  Dans **Gestionnaire de références**, cliquez sur OK.

Pour obtenir des procédures pas à pas qui montrent comment prendre en main les API de publication, consultez les articles suivants :

* [Spots publicitaires](interstitial-ads.md)
* [Publicités natives](native-ads.md)
* [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md)
* [Classe AdControl en HTML 5 et JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Compréhension des packages d’infrastructure dans le kit de développement logiciel (SDK) Microsoft Advertising

La bibliothèque Microsoft.Advertising.dll dans le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (pour les applications UWP) est configurée en tant que *package d’infrastructure*. Cette bibliothèque contient les API publicitaires des espaces de noms [Microsoft.Advertising](/uwp/api/microsoft.advertising) et [Microsoft.Advertising.WinRT.UI](/uwp/api/microsoft.advertising.winrt.ui).

Étant donné que cette bibliothèque est un package d’infrastructure, cela signifie qu’après l’installation par un utilisateur d’une version de votre application qui utilise cette bibliothèque, cette bibliothèque est automatiquement mise à jour sur son appareil via Windows Update chaque fois que nous publions une nouvelle version de la bibliothèque avec des correctifs et des améliorations des performances. Cela permet de s’assurer que vos clients disposent toujours de la dernière version disponible de la bibliothèque installée sur leurs appareils.

Si vous publiez une nouvelle version du kit de développement logiciel (SDK) qui introduit de nouvelles API ou fonctionnalités dans cette bibliothèque, vous devrez installer la dernière version du kit de développement logiciel (SDK) pour utiliser ces fonctionnalités. Dans ce scénario, vous devrez également publier votre application mise à jour dans le Windows Store.