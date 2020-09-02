---
description: Apprenez à utiliser des publicités natives, un format ad basé sur les composants où chaque composant de la publicité est remis à votre application en tant qu’élément individuel.
title: Publicités natives
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, contrôle AD, Native ad
ms.localizationpriority: medium
ms.openlocfilehash: 417560c9099937324b39a8cdfafb7d62ec7e64e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364082"
---
# <a name="native-ads"></a>Publicités natives

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Une publicité native est un format de publicité basée sur un composant où chaque élément de l’annonce publicitaire (par exemple, titre, image, description et texte de l’appel à l’action) est fourni à votre application sous forme d’élément individuel. Vous pouvez intégrer ces éléments à votre application à l’aide de vos propres polices, couleurs, animations et autres composants de l’interface utilisateur pour assembler une expérience utilisateur discrète qui correspond à l’apparence de votre application tout en obtenant un rendement élevé des publicités.

Pour les annonceurs, les publicités natives fournissent des placements très performants, car l’expérience Active Directory est étroitement intégrée à l’application et les utilisateurs ont tendance à interagir davantage avec ces types de publicités.

> [!NOTE]
> Les publicités natives sont actuellement prises en charge uniquement pour les applications UWP basées sur XAML pour Windows 10. La prise en charge des applications UWP écrites en HTML et JavaScript est prévue pour une version ultérieure du kit de développement logiciel (SDK) Microsoft Advertising.

## <a name="prerequisites"></a>Prérequis

* Installez le [Kit de développement logiciel (SDK) Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) avec visual studio 2015 ou une version ultérieure de Visual Studio. Pour obtenir des instructions d’installation, consultez [cet article](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-native-ad-into-your-app"></a>Intégrer un ad natif dans votre application

Suivez ces instructions pour intégrer une Active Directory native dans votre application et confirmer que votre implémentation native d’AD affiche une publicité de test.

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.
    > [!NOTE]
    > Si vous utilisez un projet existant, ouvrez le fichier Package. appxmanifest dans votre projet et assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée. Votre application a besoin de cette fonctionnalité pour recevoir des publicités de test et des publicités en direct.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **un processeur**, vous ne serez pas en mesure d’ajouter une référence au kit de développement logiciel (SDK) Microsoft Advertising dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence au kit de développement logiciel (SDK) Microsoft Advertising dans votre projet :

    1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur **références**, puis sélectionnez **Ajouter une référence...**
    2.  Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour XAML** (version 10.0).
    3.  Dans **Gestionnaire de références**, cliquez sur OK.

4. Dans le fichier de code approprié de votre application (par exemple, dans MainPage.xaml.cs ou dans un fichier de code pour une autre page), ajoutez les références d’espace de noms suivantes.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Namespaces":::

5.  À un emplacement approprié dans votre application (par exemple, dans ```MainPage``` ou une autre page), déclarez un objet [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) et plusieurs champs de chaîne qui représentent l’ID d’application et l’ID d’unité Active Directory pour votre ad natif. L’exemple de code suivant affecte les `myAppId` `myAdUnitId` champs et aux [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour les publicités natives.
    > [!NOTE]
    > Chaque **NativeAdsManagerV2** a une *unité publicitaire* correspondante qui est utilisée par nos services pour servir les publicités au contrôle AD natif, et chaque unité Active Directory se compose d’un *ID d’unité Active Directory* et d’un *ID d’application*. Dans ces étapes, vous attribuez des valeurs d’ID d’unité ad et d’ID d’application de test à votre contrôle. Ces valeurs de test ne peuvent être utilisées que dans une version test de votre application. Avant de publier votre application dans le Windows Store, vous devez [remplacer ces valeurs de test par des valeurs dynamiques](#release) de l’espace partenaires.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="Variables":::

6.  Dans le code qui s’exécute au démarrage (par exemple, dans le constructeur de la page), instanciez l’objet **NativeAdsManagerV2** et connectez les gestionnaires d’événements pour les événements **AdReady** et **ErrorOccurred** de l’objet.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ConfigureNativeAd":::

7.  Lorsque vous êtes prêt à afficher une publicité native, appelez la méthode **RequestAd** pour extraire une publicité.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="RequestAd":::

8.  Quand une Active Directory native est prête pour votre application, votre gestionnaire d’événements [AdReady](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) est appelé, et un objet [NativeAdV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) qui représente la valeur Active Directory native est passé au paramètre *e* . Utilisez les propriétés **NativeAdV2** pour obtenir chaque élément de la publicité native et afficher ces éléments sur votre page. Veillez à appeler également la méthode **RegisterAdContainer** pour inscrire l’élément d’interface utilisateur qui joue le rôle de conteneur pour la publicité native. Cela est nécessaire pour suivre correctement les impressions et les clics sur les annonces.
    > [!NOTE]
    > Certains éléments de la publicité Native sont requis et doivent toujours être affichés dans votre application. Pour plus d’informations, consultez nos [instructions relatives aux publicités natives](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

    Par exemple, supposons que votre application contienne un ```MainPage``` (ou une autre page) avec l' **StackPanel**suivant. Ce **StackPanel** contient une série de contrôles qui affichent différents éléments d’une Active Directory native, y compris le titre, la description, les images, *sponsorisée par* du texte et un bouton qui affiche l' *appel au texte de l’action* .

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    L’exemple de code suivant montre un gestionnaire d’événements **AdReady** qui affiche chaque élément de la publicité native dans les contrôles de l’élément **StackPanel** , puis appelle la méthode **RegisterAdContainer** pour inscrire le **StackPanel**. Ce code part du principe qu’il est exécuté à partir du fichier code-behind de la page qui contient l' **StackPanel**.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="AdReady":::

9.  Définissez un gestionnaire d’événements pour l’événement **ErrorOccurred** afin de gérer les erreurs liées à la publicité native. L’exemple suivant écrit les informations d’erreur dans la fenêtre **sortie** de Visual Studio pendant le test.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs" id="ErrorOccurred":::

10.  Compilez et exécutez l’application pour la voir avec une publicité de test.

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Publier votre application avec des publicités en direct

Une fois que vous avez confirmé que votre implémentation Active Directory Native affiche une publicité de test, suivez ces instructions pour configurer votre application de manière à afficher des publicités réelles et à envoyer votre application mise à jour au Store.

1.  Assurez-vous que votre implémentation native d’AD respecte nos [instructions pour les publicités natives](ui-and-user-experience-guidelines.md#guidelines-for-native-ads).

2.  Dans l’espace partenaires, accédez à la page [annonces dans l’application](../publish/in-app-ads.md) et [créez une unité ad](set-up-ad-units-in-your-app.md#live-ad-units). Pour le type d’unité ad, spécifiez **Native**. Prenez note de l’ID d’unité publicitaire et de l’ID de l’application.
    > [!NOTE]
    > Les valeurs d’ID d’application pour les unités AD de test et les unités ad UWP activent des formats différents. Les valeurs d’ID d’application de test sont des GUID. Lorsque vous créez une unité ad UWP en temps réel dans l’espace partenaires, la valeur de l’ID d’application pour l’unité ad correspond toujours à l’ID de magasin de votre application (un exemple de valeur d’ID de magasin ressemble à 9NBLGGH4R315).

3. Vous pouvez éventuellement activer la médiation AD pour la publicité Native en configurant les paramètres dans la section [paramètres de médiation](../publish/in-app-ads.md#mediation) de la page [annonces dans l’application](../publish/in-app-ads.md) . La médiation ad vous permet d’optimiser votre chiffre d’affaires et vos fonctionnalités de promotion d’applications en affichant des publicités à partir de plusieurs réseaux Active Directory.

4.  Dans votre code, remplacez les valeurs de l’unité ad du test (autrement dit, les paramètres *ApplicationID* et *AdUnitId* du constructeur [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) ) par les valeurs dynamiques que vous avez générées dans l’espace partenaires.

5.  [Soumettez votre application](../publish/app-submissions.md) au magasin à l’aide de l’espace partenaires.

6.  Passez en revue les [rapports de performances publicitaires](../publish/advertising-performance-report.md) dans l’espace partenaires.

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>Gérer les unités AD pour plusieurs publicités natives dans votre application

Vous pouvez utiliser plusieurs emplacements ad natifs dans une même application. Dans ce scénario, nous vous recommandons d’affecter une unité ad différente à chaque placement de publicité natif. L’utilisation de différentes unités AD pour Native Active Directory vous permet de [configurer séparément les paramètres de médiation](../publish/in-app-ads.md#mediation) et d’obtenir des [données de rapport](../publish/advertising-performance-report.md) discrètes pour chaque contrôle. Cela permet également à nos services de mieux optimiser les publicités que nous avons utilisées dans votre application.

> [!IMPORTANT]
> Vous pouvez utiliser chaque unité ad dans une seule application. Si vous utilisez une unité ad dans plusieurs applications, les publicités ne sont pas prises en charge pour cette unité Active Directory.

## <a name="related-topics"></a>Rubriques connexes

* [Instructions pour les publicités natives](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [Publicités dans l’application](../publish/in-app-ads.md)
* [Configurer des unités AD pour votre application](set-up-ad-units-in-your-app.md)
