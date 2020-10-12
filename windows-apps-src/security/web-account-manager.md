---
title: Gestionnaire de comptes web
description: Cet article explique comment utiliser AccountsSettingsPane pour connecter votre application plateforme Windows universelle (UWP) à des fournisseurs d’identité externes, tels que Microsoft ou Facebook, à l’aide des API du gestionnaire de comptes Web Windows 10.
ms.date: 12/06/2017
ms.topic: article
keywords: windows 10, uwp, sécurité
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 0a67c88eb7eb70308e6dcbbd096289c0617793b1
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933070"
---
# <a name="web-account-manager"></a>Gestionnaire de comptes web

Cet article explique comment utiliser **[AccountsSettingsPane](/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** pour connecter votre application plateforme Windows universelle (UWP) à des fournisseurs d’identité externes, tels que Microsoft ou Facebook, à l’aide des API du gestionnaire de comptes Web Windows 10. Vous allez apprendre à demander à l’utilisateur l’autorisation d’utiliser son compte Microsoft, d’obtenir un jeton d’accès et de l’utiliser pour effectuer des opérations de base (par exemple, obtenir des données de profil ou télécharger des fichiers sur leur compte OneDrive). Pour obtenir l’autorisation et l’accès utilisateur, les étapes sont similaires quel que soit le fournisseur d’identité, à condition qu’il prenne en charge le Gestionnaire de compte web.

> [!NOTE]
> Pour obtenir un exemple de code complet, consultez l' [exemple WebAccountManagement sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement).

## <a name="get-set-up"></a>Se préparer

Pour commencer, créez une nouvelle application vierge dans Visual Studio. 

Ensuite, pour mettre en place la connexion à des fournisseurs d’identité, vous devez associer votre application au Windows Store. Pour ce faire, cliquez avec le bouton droit sur votre projet, choisissez **stocker**  >  **associer l’application au Windows Store**, puis suivez les instructions de l’Assistant. 

Enfin, créez une interface utilisateur très simple constituée d’un bouton XAML et de deux zones de texte.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

Reliez un gestionnaire d’événements à votre bouton dans le code-behind :

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Pour finir, ajoutez les espaces de noms suivants afin de ne pas avoir à vous soucier ultérieurement des problèmes de référence : 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>Afficher le volet Paramètres des comptes

Le système fournit une interface utilisateur intégrée pour la gestion des fournisseurs d’identité et des comptes Web appelée **AccountsSettingsPane**. Vous pouvez l’afficher comme suit :

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Si vous exécutez votre application et cliquez sur le bouton « Se connecter », elle affiche une fenêtre vide. 

![Capture d’écran de la fenêtre choisir un compte sans compte.](images/tb-1.png)

Le volet est vide, car le système propose uniquement un interpréteur de commandes de l’interface utilisateur. Il revient au développeur de programmer le remplissage du volet avec les fournisseurs d’identité. 

> [!TIP]
> Si vous le souhaitez, vous pouvez utiliser **[ShowAddAccountAsync](/uwp/api/windows.ui.applicationsettings.accountssettingspane.showaddaccountasync)** au lieu de **[Show](/uwp/api/windows.ui.applicationsettings.accountssettingspane.show#Windows_UI_ApplicationSettings_AccountsSettingsPane_Show)**, qui renverra un **[IAsyncAction](/uwp/api/Windows.Foundation.IAsyncAction)**, pour interroger l’état de l’opération. 

## <a name="register-for-accountcommandsrequested"></a>S’inscrire à AccountCommandsRequested

Pour ajouter des commandes au volet, il faut tout d’abord s’inscrire au gestionnaire d’événements AccountCommandsRequested. Cela indique au système d’exécuter la logique de génération quand l’utilisateur demande à afficher le volet (par exemple, clique sur notre bouton XAML). 

Dans votre code-behind, remplacez les événements OnNavigatedTo et OnNavigatedFrom et ajoutez-y le code suivant : 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Les utilisateurs n’interagissent pas très souvent avec les comptes, ce qui signifie qu’en procédant ainsi pour l’inscription et la désinscription de votre gestionnaire d’événements, vous évitez les fuites de mémoire. De cette façon, votre volet personnalisé est uniquement en mémoire lorsqu’il y a de grandes chances qu’un utilisateur le demande (parce qu’il est sur une page « Paramètres » ou « Connexion », par exemple). 

## <a name="build-the-account-settings-pane"></a>Conception du volet Paramètres du compte

La méthode BuildPaneAsync est appelée chaque fois que le **AccountsSettingsPane** est affiché. C’est ici que nous allons enregistrer le code nécessaire à la personnalisation des commandes affichées dans le volet. 

Commencez par obtenir un report. Cela indique au système de retarder l’émission de la **AccountsSettingsPane** jusqu’à ce que nous ayons fini de la créer.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Ensuite, obtenez un fournisseur à l’aide de la méthode WebAuthenticationCoreManager.FindAccountProviderAsync. L’URL du fournisseur varie en fonction du fournisseur et figure dans la documentation correspondante. Pour les comptes et Azure Active Directory Microsoft, il s’agit de « https \: //login.Microsoft.com ». 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Notez que nous passons également la chaîne « consumers » au paramètre facultatif *authority*. En effet, Microsoft fournit deux types d’authentification : les comptes Microsoft (MSA) pour les « consommateurs » et Azure Active Directory (AAD) pour les « entreprises ». Le paramètre « consumers » indique que nous voulons l’option MSA. Si vous développez une application d’entreprise, utilisez la chaîne « organizations » à la place.

Enfin, ajoutez le fournisseur au **AccountsSettingsPane** en créant un **[WebAccountProviderCommand](/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** de la façon suivante : 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

La méthode GetMsaToken que nous avons transmise à notre nouveau **WebAccountProviderCommand** n’existe pas encore (nous allons la générer à l’étape suivante). n’hésitez donc pas à l’ajouter maintenant en tant que méthode vide.

Exécutez le code ci-dessus pour que votre volet ressemble à ceci : 

![Capture d’écran de la fenêtre choisir un compte avec des comptes listés.](images/tb-2.png)

### <a name="request-a-token"></a>Demander un jeton

Une fois que l’option de compte Microsoft s’affiche dans **AccountsSettingsPane**, nous devons gérer ce qui se produit lorsque l’utilisateur le sélectionne. Nous avons enregistré notre méthode GetMsaToken afin qu’elle se déclenche quand l’utilisateur choisit d’ouvrir une session avec son compte Microsoft. C’est donc ainsi que nous allons obtenir le jeton. 

Pour obtenir un jeton, utilisez la méthode RequestTokenAsync comme suit : 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Dans cet exemple, nous transmettons la chaîne « WL. Basic » au paramètre _scope_ . L’étendue représente le type d’informations concernant un utilisateur spécifique que vous demandez au service fournisseur. Certaines étendues fournissent un accès uniquement aux informations de base d’un utilisateur, telles que le nom et l’adresse de messagerie, tandis que d’autres étendues peuvent accorder l’accès à des informations sensibles telles que les photos de l’utilisateur ou la boîte de réception de l’e-mail. En règle générale, votre application doit utiliser l’étendue la moins permissive nécessaire pour atteindre sa fonction. Les fournisseurs de services fournissent de la documentation sur les étendues nécessaires pour obtenir des jetons à utiliser avec leurs services. 

* Pour Microsoft 365 et les étendues Outlook.com, consultez [utiliser l’API REST d’Outlook (version 2,0)](/previous-versions/office/office-365-api/api/version-2.0/use-outlook-rest-api). 
* Pour les étendues OneDrive, consultez [authentification et connexion onedrive](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes). 

> [!TIP]
> Éventuellement, si votre application utilise un indicateur de connexion (pour renseigner le champ utilisateur avec une adresse de messagerie par défaut) ou une autre propriété spéciale relative à l’expérience de connexion, indiquez-la dans la propriété **[WebTokenRequest. valeur appproperties](/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** . Le système ignore alors la propriété lors de la mise en cache du compte Web, ce qui empêche les incompatibilités de compte dans le cache.

Si vous développez une application d’entreprise, vous souhaiterez probablement vous connecter à une instance Azure Active Directory (AAD) et utiliser l’API Microsoft Graph plutôt que les services MSA classiques. Dans ce cas, utilisez le code suivant : 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Le reste de cet article décrit la suite du scénario MSA, mais le code pour AAD est très similaire. Pour plus d’informations sur AAD/Microsoft Graph, notamment un exemple complet sur GitHub, voir la [documentation Microsoft Graph](https://developer.microsoft.com/graph).

## <a name="use-the-token"></a>Utilisation du jeton

La méthode RequestTokenAsync renvoie un objet WebTokenRequestResult, qui contient les résultats de votre demande. Si votre demande a abouti, elle contiendra un jeton.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> Si vous recevez une erreur lors de la demande d’un jeton, vérifiez que vous avez associé votre application au Windows Store comme décrit à l’étape 1. Votre application ne pourra pas obtenir de jeton si vous avez ignoré cette étape. 

Une fois le jeton en votre possession, vous pouvez l’utiliser pour appeler les API de votre fournisseur. Dans le code ci-dessous, nous appelons l' [API Microsoft Live](/office/) de l’utilisateur pour obtenir des informations de base sur l’utilisateur et l’afficher dans l’interface utilisateur. Notez cependant que dans la plupart des cas, il est recommandé de stocker le jeton une fois obtenu, puis de l’utiliser dans une méthode distincte.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

La méthode utilisée pour appeler les différentes API REST varie d’un fournisseur à l’autre ; voir la documentation sur les API du fournisseur pour plus d’informations sur l’utilisation de votre jeton. 

## <a name="store-the-account-for-future-use"></a>Stocker le compte pour une utilisation ultérieure

Les jetons sont utiles pour obtenir immédiatement des informations relatives à un utilisateur, mais leur durée de validité est généralement très variable : les jetons MSA, par exemple, ne sont valides que pendant quelques heures. Heureusement, vous n’avez pas besoin d’afficher à nouveau les **AccountsSettingsPane** chaque fois qu’un jeton expire. Lorsqu’un utilisateur a autorisé une fois votre application, vous pouvez stocker les informations de compte de l’utilisateur pour une utilisation future. 

Pour ce faire, utilisez la classe **[webaccount](/uwp/api/windows.security.credentials.webaccount)** . Un **compte webaccount** est retourné par la méthode que vous avez utilisée pour demander le jeton :

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Une fois que vous disposez d’une instance **webaccount** , vous pouvez facilement la stocker. Dans l’exemple suivant, nous utilisons LocalSettings. Pour plus d’informations sur l’utilisation de LocalSettings et d’autres méthodes pour stocker des données utilisateur, consultez [stocker et récupérer des données et des paramètres d’application](../design/app-settings/store-and-retrieve-app-data.md).

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Ensuite, nous pouvons utiliser une méthode asynchrone telle que la suivante pour tenter d’obtenir un jeton en arrière-plan avec le **compte webaccount**stocké.

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Placez la méthode ci-dessus juste avant le code qui génère le **AccountsSettingsPane**. Si le jeton est obtenu en arrière-plan, il n’est pas nécessaire d’afficher le volet. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

Dans la mesure où il est très simple d’obtenir un jeton silencieusement, nous vous recommandons d’utiliser ce processus pour actualiser votre jeton entre les sessions, plutôt que de mettre en cache un jeton existant (étant donné que ce jeton peut expirer à tout moment).

> [!NOTE]
> L’exemple ci-dessus couvre uniquement les cas de réussite et d’échec de base. Votre application doit également prendre en compte des scénarios plus inhabituels, comme un utilisateur qui révoque l’autorisation de votre application ou qui supprimer son compte de Windows, par exemple, et les gérer de manière fluide.  

## <a name="remove-a-stored-account"></a>Supprimer un compte stocké

Si vous conservez un compte Web, vous souhaiterez peut-être accorder à vos utilisateurs la possibilité de dissocier leur compte de votre application. De cette façon, ils peuvent « se déconnecter » de l’application : leurs informations de compte ne seront plus chargées automatiquement lors du lancement. Pour ce faire, supprimez d’abord les informations sur les comptes et les fournisseurs enregistrés dans le stockage. Appelez ensuite **[SignOutAsync](/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** pour effacer le cache et invalider les jetons existants que votre application peut avoir. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Ajouter des fournisseurs qui ne prennent pas en charge WebAccountManager

Si vous souhaitez intégrer l’authentification d’un service à votre application, mais que ce service ne prend pas en charge gestionnaire-Google + ou Twitter, par exemple, vous pouvez toujours ajouter manuellement ce fournisseur au **AccountsSettingsPane**. Pour ce faire, créez un nouvel objet WebAccountProvider et fournissez vos propres icône de nom et. png, puis ajoutez-le à la liste WebAccountProviderCommands. Voici le code stub : 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> Cela ajoute uniquement une icône à **AccountsSettingsPane** et exécute la méthode que vous spécifiez lorsque l’utilisateur clique sur l’icône (GetTwitterTokenAsync, dans le cas présent). Vous devez fournir le code qui gère l’authentification réelle. Pour plus d’informations, consultez [service d’authentification Web](web-authentication-broker.md), qui fournit des méthodes d’assistance pour l’authentification à l’aide des services REST. 

## <a name="add-a-custom-header"></a>Ajouter un en-tête personnalisé

Vous pouvez personnaliser le volet Paramètres du compte à l’aide de la propriété HeaderText, comme ceci : 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Capture d’écran de la fenêtre choisir un compte sans compte listé et un message indiquant que mon application Isard fonctionne mieux si vous êtes connecté.](images/tb-3.png)

Ne développez pas trop le texte de l’en-tête ; il doit rester bref et concis. Si votre processus de connexion est compliqué et que vous avez besoin d’afficher plus d’informations, renvoyez l’utilisateur à une page distincte à l’aide d’un lien personnalisé. 

## <a name="add-custom-links"></a>Ajouter des liens personnalisés

Vous pouvez ajouter des commandes personnalisées à la classe AccountsSettingsPane. Elles apparaissent sous forme de liens sous vos WebAccountProviders pris en charge. Les commandes personnalisées sont parfaites pour les tâches simples sur les comptes d’utilisateurs, telles que l’affichage d’une politique de confidentialité ou l’ouverture d’une page de support pour les utilisateurs rencontrant des difficultés. 

Voici un exemple : 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Capture d’écran de la fenêtre choisir un compte sans compte listé et un lien vers une politique de confidentialité.](images/tb-4.png)

En théorie, vous pouvez utiliser les commandes de paramètres pour tout. Toutefois, nous vous recommandons d’en limiter l’utilisation aux scénarios intuitifs liés aux comptes, tels que ceux décrits ci-dessus. 

## <a name="see-also"></a>Voir aussi

[Espace de noms Windows.Security.Authentication.Web.Core](/uwp/api/windows.security.authentication.web.core)

[Espace de noms Windows.Security.Credentials](/uwp/api/windows.security.credentials)

[AccountsSettingsPane, classe](/uwp/api/windows.ui.applicationsettings.accountssettingspane)

[Service Broker d’authentification web](web-authentication-broker.md)

[Exemple de gestion de compte Web](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)

[Application du planificateur du déjeuner](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)