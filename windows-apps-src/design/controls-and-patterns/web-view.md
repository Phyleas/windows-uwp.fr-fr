---
Description: Un contrôle d’affichage web incorpore dans votre application une vue qui affiche le contenu web à l’aide du moteur de rendu Microsoft Edge. Des liens hypertexte peuvent également apparaître et fonctionner dans un contrôle d’affichage web.
title: Affichage web
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 43d0471b6e7ebc36df4f80a1b214b0721ae25570
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081409"
---
# <a name="web-view"></a>Affichage web

Un contrôle d’affichage web incorpore dans votre application une vue qui affiche le contenu web à l’aide du moteur de rendu Microsoft Edge. Des liens hypertexte peuvent également apparaître et fonctionner dans un contrôle d’affichage web.

> **API importantes** : [Classe WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un contrôle d’affichage web pour afficher du contenu HTML à mise en forme enrichie provenant d’un serveur web distant, de code généré de manière dynamique ou de fichiers de contenu dans votre package d’application. Le contenu enrichi peut aussi contenir du code de script, et communiquer entre le script et le code de votre application.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/WebView">ouvrir l’application et voir l’objet WebView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-web-view"></a>Créer un affichage web

**Modifier l’apparence d’une vue web**

[WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) n’étant pas une sous-classe de [Control](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), il n’a pas de modèle de contrôle. Il est cependant possible de définir différentes propriétés pour contrôler certains aspects visuels de l’affichage web.
- Pour limiter la zone d’affichage, définissez les propriétés [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) et [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height). 
- Pour translater, mettre à l’échelle, incliner et faire pivoter un affichage web, utilisez la propriété [RenderTransform](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform).
- Pour contrôler l’opacité de l’affichage web, définissez la propriété [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity).
- Pour spécifier une couleur à utiliser en arrière-plan de la page web quand le contenu HTML ne spécifie pas de couleur, définissez la propriété [DefaultBackgroundColor](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor). 

**Obtenir le titre de la page web**

Vous pouvez obtenir le titre du document HTML actuellement affiché dans l’affichage web en utilisant la propriété [DocumentTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.documenttitle). 

**Événements d’entrée et ordre de tabulation**

Bien que WebView ne soit pas une sous-classe de Control, il reçoit le focus d’entrée du clavier et fait partie intégrante de la séquence de tabulation. Il fournit une méthode [Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.focus), et des événements [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) et [LostFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus), mais il n’a pas de propriétés en rapport avec la tabulation. Sa position dans la séquence de tabulation est identique à sa position dans l’ordre du document XAML. La séquence de tabulation inclut tous les éléments du contenu de l’affichage web qui peuvent recevoir le focus d’entrée. 

Comme indiqué dans le tableau Événements de la page consacrée à la classe [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView), l’affichage web ne prend pas en charge la plupart des événements d’entrée utilisateur hérités de [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), comme [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown), [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) et [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed). À la place, vous pouvez faire appel à [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) avec la fonction JavaScript **eval** pour utiliser les gestionnaires d’événements HTML, et aussi pour utiliser **window.external.notify** à partir du gestionnaire d’événements HTML afin de notifier l’application à l’aide de [WebView.ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify).

### <a name="navigating-to-content"></a>Accès au contenu

La vue web fournit plusieurs API pour la navigation de base : [GoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goback), [GoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goforward), [Stop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.stop), [Refresh](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.refresh), [CanGoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoback) et [CanGoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoforward). Ces API vous permettent d’ajouter des fonctionnalités de navigation web standard à votre application. 

Pour définir le contenu initial de l’affichage web, définissez la propriété [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.source) en XAML. L’analyseur XAML convertit automatiquement la chaîne en [Uri](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

La propriété Source peut être définie dans le code, mais il est généralement préférable d’utiliser une des méthodes **Navigate** pour charger du contenu dans le code. 

Pour charger du contenu web, utilisez la méthode [Navigate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigate) avec un **Uri** qui fait appel au schéma HTTP ou HTTPS. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Pour accéder à un URI avec une requête POST et des en-têtes HTTP, utilisez la méthode [NavigateWithHttpRequestMessage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage). Cette méthode prend en charge seulement [HttpMethod.Post](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.post) et [HttpMethod.Get](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.get) pour la valeur de la propriété [HttpRequestMessage.Method](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage.method). 

Pour charger du contenu non compressé et non chiffré à partir des magasins de données [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) ou [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) de votre application, utilisez la méthode **Navigate** avec un **Uri** qui fait appel au [schéma ms-appdata](/windows/uwp/app-resources/uri-schemes). La prise en charge de l’affichage web pour ce schéma nécessite de placer votre contenu dans un sous-dossier sous le dossier local ou temporaire. Ceci permet l’accès à des URI comme ms-appdata:///local/*dossier*/*fichier*.html et ms-appdata:///temp/*dossier*/*fichier*.html. (Pour charger des fichiers compressés ou chiffrés, consultez [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri).) 

Chacun de ces sous-dossiers de premier niveau est isolé du contenu des autres sous-dossiers de premier niveau. Par exemple, vous pouvez accéder à ms-appdata:///temp/dossier1/fichier.html, mais ce fichier ne peut pas contenir de lien vers ms-appdata:///temp/dossier2/fichier.html. Vous pouvez cependant ajouter des liens vers du contenu HTML dans le package d’application en utilisant le **schéma ms-appx-web**, et vers du contenu web en utilisant des schémas d’URI **http** et **https**.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Pour charger du contenu à partir de votre package d’application, utilisez la méthode **Navigate** avec un **Uri** qui fait appel au [schéma ms-appx-web](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)). 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Vous pouvez charger du contenu local via un résolveur personnalisé en utilisant la méthode [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri). Ceci permet de mettre en œuvre des scénarios avancés comme le téléchargement et la mise en cache de contenu web en vue d’une utilisation hors ligne, ou l’extraction du contenu d’un fichier compressé.

### <a name="responding-to-navigation-events"></a>Réponse aux événements de navigation

Le contrôle d’affichage web fournit plusieurs événements que vous pouvez utiliser pour répondre aux états de la navigation et du chargement de contenu. Les événements se produisent dans l’ordre suivant pour le contenu de la vue web racine : [NavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationstarting), [ContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.contentloading), [DOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded), [NavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** : se produit avant que l’affichage web accède à du nouveau contenu. Vous pouvez annuler la navigation dans un gestionnaire pour cet événement en définissant la propriété WebViewNavigationStartingEventArgs.Cancel sur true. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** : se produit quand l’affichage web a commencé à charger du nouveau contenu. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** : se produit quand l’affichage web a fini d’analyser le contenu HTML actuel. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** : se produit quand l’affichage web a fini de charger le contenu actuel ou en cas d’échec de la navigation. Pour déterminer si la navigation a échoué, vérifiez les propriétés [IsSuccess](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess) et [WebErrorStatus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus) de la classe [WebViewNavigationCompletedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs). 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Des événements similaires se produisent dans le même ordre pour chaque **iframe** du contenu de l’affichage web : 
- [FrameNavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting) : se produit avant qu’une trame de l’affichage web accède à du nouveau contenu. 
- [FrameContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framecontentloading) : se produit quand une trame de l’affichage web a commencé à charger du nouveau contenu. 
- [FrameDOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded) : se produit quand une trame de l’affichage web a fini d’analyser son contenu HTML actuel. 
- [FrameNavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted) : se produit quand une trame de l’affichage web a fini de charger son contenu. 

### <a name="responding-to-potential-problems"></a>Réponse aux problèmes potentiels

Vous pouvez répondre aux problèmes potentiels liés au contenu, comme les scripts de longue durée, le contenu que l’affichage web ne peut pas charger et les avertissements relatifs à du contenu dangereux. 

Votre application peut sembler ne pas répondre pendant l’exécution de scripts. L’événement [LongRunningScriptDetected](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected) se produit régulièrement pendant que l’affichage web exécute du JavaScript et donne la possibilité d’interrompre le script. Pour déterminer depuis combien de temps le script est exécuté, vérifiez la propriété [ExecutionTime](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime) de la classe [WebViewLongRunningScriptDetectedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs). Pour interrompre le script, définissez la propriété [StopPageScriptExecution](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution) des arguments d’événement sur **true**. Le script interrompu ne sera pas réexécuté, sauf s’il est rechargé au cours d’une navigation ultérieure dans l’affichage web. 

Le contrôle d’affichage web ne peut pas héberger des types de fichier arbitraires. Quand il y a une tentative de charger du contenu que l’affichage web ne peut pas héberger, l’événement [UnviewableContentIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified) se produit. Vous pouvez gérer cet événement et avertir l’utilisateur, ou utiliser la classe [Launcher](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) pour rediriger le fichier vers un navigateur externe ou une autre application.

De même, l’événement [UnsupportedUriSchemeIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified) se produit quand un schéma d’URI non pris en charge, comme fbconnect:// ou mailto://, est appelé dans le contenu web. Vous pouvez gérer cet événement de façon à personnaliser le comportement au lieu d’autoriser le lanceur système par défaut à lancer l’URI.

L’événement [UnsafeContentWarningDisplayingevent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) se produit quand l’affichage web montre une page d’avertissement pour du contenu qui a été signalé comme dangereux par le filtre SmartScreen. Si l’utilisateur choisit de poursuivre la navigation, l’avertissement ne s’affichera plus et l’événement ne se déclenchera pas la prochaine fois qu’il accèdera à la page.

### <a name="handling-special-cases-for-web-view-content"></a>Gestion des cas particuliers pour le contenu de l’affichage web

Vous pouvez utiliser la propriété [ContainsFullScreenElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement) et l’événement [ContainsFullScreenElementChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged) pour détecter et activer les expériences en plein écran dans le contenu web, comme la lecture vidéo en plein écran, et y répondre. Par exemple, vous pouvez utiliser l’événement ContainsFullScreenElementChanged pour redimensionner l’affichage web afin qu’il occupe la totalité de l’affichage de votre application, ou comme dans l’exemple ci-après, afin de passer une application du mode fenêtre au mode plein écran quand une expérience web plein écran est souhaitée.

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Vous pouvez utiliser l’événement [NewWindowRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested) pour gérer les cas où le contenu web hébergé demande l’affichage d’une nouvelle fenêtre, comme une fenêtre contextuelle. Vous pouvez utiliser un autre contrôle WebView pour afficher le contenu de la fenêtre demandée.

Utilisez l’événement [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) pour activer les fonctionnalités web qui nécessitent des fonctionnalités spéciales. Ces dernières incluent actuellement la géolocalisation, le stockage IndexedDB, et la prise en charge du son et de la vidéo de l’utilisateur (par exemple d’un microphone ou d’une webcam). Si votre application accède à l’emplacement ou au média de l’utilisateur, vous devez déclarer cette fonctionnalité dans le manifeste de l’application. Par exemple, une application qui utilise la géolocalisation doit inclure au minimum les déclarations de fonctionnalités suivantes dans le fichier Package.appxmanifest :

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

En plus de la gestion par l’application de l’événement [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested), l’utilisateur devra approuver des boîtes de dialogue système standard pour les applications faisant appel à des fonctionnalités de géolocalisation ou multimédias afin d’activer ces dernières.

Voici un exemple de la façon dont une application activerait la géolocalisation dans une carte de Bing :

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Si votre application nécessite une entrée utilisateur ou d’autres opérations asynchrones pour répondre à une demande d’autorisation, utilisez la méthode [Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) de [WebViewPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) afin de créer un élément [WebViewDeferredPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest) qui pourra être traité à un moment ultérieur. Consultez [WebViewPermissionRequest.Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer). 

Si les utilisateurs doivent se déconnecter de façon sécurisée d’un site web hébergé dans un affichage web, ou dans d’autres cas où la sécurité est importante, appelez la méthode statique [ClearTemporaryWebDataAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) pour effacer de la session d’affichage web tout le contenu mis en cache localement. Ceci permet d’empêcher les utilisateurs malveillants d’accéder à des données sensibles. 

### <a name="interacting-with-web-view-content"></a>Interaction avec le contenu de l’affichage web

Vous pouvez interagir avec le contenu de l’affichage web en utilisant la méthode [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) pour appeler ou injecter un script dans le contenu de l’affichage web, et en utilisant l’événement [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) pour récupérer des informations à partir du contenu de l’affichage web.

Pour appeler un script JavaScript à l’intérieur du contenu de l’affichage web, utilisez la méthode [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync). Le script appelé peut retourner seulement des valeurs de chaîne. 

Par exemple, si le contenu d’un affichage web nommé `webView1` contient une fonction nommée `setDate` qui nécessite 3 paramètres, vous pouvez l’appeler comme ceci. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Vous pouvez utiliser **InvokeScriptAsync** avec la fonction JavaScript **eval** pour injecter du contenu dans la page web.

Ici, le texte d’une zone de texte XAML (`nameTextBox.Text`) est écrit dans un élément div d’une page HTML hébergée dans `webView1`. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Les scripts dans le contenu de l’affichage web peuvent utiliser **window.external.notify** avec un paramètre de chaîne pour renvoyer des informations à votre application. Pour recevoir ces messages, vous devez gérer l’événement [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify). 

Pour permettre à une page web externe de déclencher l’événement **ScriptNotify** lors de l’appel de window.external.notify, vous devez inclure l’URI de la page dans la section **ApplicationContentUriRules** du manifeste de l’application. (Vous pouvez effectuer cette opération dans Microsoft Visual Studio, sous l’onglet URI de contenu du concepteur Package.appxmanifest.) Les URI dans cette liste doivent utiliser le protocole HTTPS et peuvent contenir des caractères génériques de sous-domaine (par exemple, `https://*.microsoft.com`), mais ils ne peuvent pas contenir de caractères génériques de domaine (par exemple, `https://*.com` et `https://*.*`). La configuration requise pour le manifeste ne s’applique pas au contenu qui provient du package d’application, qui utilise un URI ms-local-stream:// ou qui est chargé à l’aide de [NavigateToString](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetostring). 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Accès au composant Windows Runtime dans un affichage web

Vous pouvez utiliser la méthode [AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) pour injecter une instance d’une classe native à partir d’un composant Windows Runtime dans le contexte JavaScript de l’affichage web. Ceci permet un accès complet aux méthodes natives, aux propriétés et aux événements de cet objet dans le contenu JavaScript de cet affichage web. La classe doit être décorée avec l’attribut [AllowForWeb](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute). 

Par exemple, ce code injecte une instance de `MyClass` importée à partir d’un composant Windows Runtime dans un affichage web.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Pour plus d’informations, consultez [WebView.AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject). 

Par ailleurs, le contenu JavaScript approuvé d’un affichage web peut être autorisé à accéder directement aux API Windows Runtime. Ceci fournit des fonctionnalités natives puissantes pour les applications web hébergées dans un affichage web. Pour activer cette fonctionnalité, l’URI du contenu approuvé doit être autorisé dans la section ApplicationContentUriRules de l’application au sein du fichier Package.appxmanifest, avec WindowsRuntimeAccess spécifiquement défini sur « all ». 

Cet exemple montre une section du manifeste de l’application. Ici, un URI local est autorisé à accéder au composant Windows Runtime. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>Options pour l’hébergement de contenu web

Vous pouvez utiliser la propriété [WebView.Settings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.settings) (de type [WebViewSettings](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings)) pour activer ou désactiver JavaScript et IndexedDB. Par exemple, si vous utilisez un affichage web pour afficher du contenu strictement statique, il peut être judicieux de désactiver JavaScript pour optimiser les performances.

### <a name="capturing-web-view-content"></a>Capture du contenu de l’affichage web

Pour activer le partage du contenu de l’affichage web avec d’autres applications, utilisez la méthode [CaptureSelectedContentToDataPackageAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync), qui retourne le contenu sélectionné sous forme d’élément [DataPackage](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Cette méthode étant asynchrone, vous devez utiliser un report pour empêcher le retour du gestionnaire d’événements [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) avant la fin de l’appel asynchrone. 

Pour obtenir une image d’aperçu du contenu actuel de l’affichage web, utilisez la méthode [CapturePreviewToStreamAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync). Cette méthode crée une image du contenu actuel et l’écrit dans le flux spécifié. 

### <a name="threading-behavior"></a>Comportement de threading

Par défaut, le contenu de l’affichage web est hébergé sur le thread d’interface utilisateur sur les postes de travail et en dehors de celui-ci sur tous les autres appareils. Vous pouvez utiliser la propriété statique [WebView.DefaultExecutionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode) pour obtenir le comportement de threading par défaut du client actuel. Si nécessaire, vous pouvez utiliser le constructeur [WebView(WebViewExecutionMode)](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.-ctor#Windows_UI_Xaml_Controls_WebView__ctor_Windows_UI_Xaml_Controls_WebViewExecutionMode_) pour remplacer ce comportement. 

> **Remarque**&nbsp;&nbsp;Des problèmes de performances peuvent se présenter quand du contenu est hébergé sur le thread d’interface utilisateur sur des appareils mobiles. Ainsi, veillez à effectuer des tests sur tous les appareils cibles en cas de changement de la propriété DefaultExecutionMode.

Un affichage web qui héberge le contenu en dehors du thread d’interface utilisateur n’est pas compatible avec les contrôles parents qui nécessitent la propagation de mouvements entre le contrôle d’affichage web et le parent, comme [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView), [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) et les autres contrôles associés. Ces contrôles ne sont pas en mesure de recevoir des mouvements effectués dans l’affichage web hors thread. En outre, l’impression de contenu web hors thread n’est pas directement prise en charge : vous devez imprimer un élément avec le remplissage [WebViewBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) à la place.

## <a name="recommendations"></a>Recommandations


-   Vérifiez que le site web chargé a une mise en forme adaptée à l’appareil, et qu’il utilise des couleurs, une typographie et une navigation cohérentes avec le reste de votre application.
-   Les champs d’entrée doivent avoir une taille appropriée. Il se peut que les utilisateurs ne réalisent pas qu’ils peuvent faire un zoom avant pour entrer du texte.
-   Si un affichage web n’a pas la même apparence que le reste de votre application, utilisez d’autres contrôles ou adoptez une autre approche pour accomplir les tâches en question. Si l’apparence de votre affichage web correspond au reste de votre application, l’expérience sera beaucoup plus fluide et transparente aux yeux des utilisateurs.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-topics"></a>Rubriques connexes

- [Classe WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)
