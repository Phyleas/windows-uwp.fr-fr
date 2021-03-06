---
description: Récupérez ou créez le contenu web le plus actualisé et le plus populaire à l’aide de flux syndiqués générés conformément aux normes RSS et Atom via les fonctionnalités de l’espace de noms Windows.Web.Syndication.
title: Flux RSS/Atom
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1dad07260490f03ed75d1329487efdaeba47af0e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158203"
---
# <a name="rssatom-feeds"></a>Flux RSS/Atom


**API importantes**

-   [**Windows.Data.Xml.Dom**](/uwp/api/Windows.Data.Xml.Dom)
-   [**Windows.Web.AtomPub**](/uwp/api/Windows.Web.AtomPub)
-   [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication)

Récupérez ou créez le contenu web le plus actualisé et le plus populaire à l’aide de flux syndiqués générés conformément aux normes RSS et Atom via les fonctionnalités de l’espace de noms [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication).

## <a name="what-is-a-feed"></a>Qu’est-ce qu’un flux ?

Un flux Web est un document contenant un nombre indéfini d’entrées individuelles constituées de texte, de liens et d’images. Les mises à jours d’un flux prennent la forme de nouvelles entrées utilisées pour promouvoir le tout dernier contenu des éditeurs sur le Web. Les consommateurs de contenu peuvent utiliser une application de lecture de flux pour agréger et contrôler les flux d’autant d’auteurs de contenu qu’ils le souhaitent, ce qui leur donne un accès rapide et pratique au contenu le plus récent.

## <a name="which-feed-format-standards-are-supported"></a>Quelles normes de format de flux sont prises en charge ?

La plateforme Windows universelle (UWP) prend en charge la récupération des flux pour les normes de format RSS 0.91 à 2.0, et les normes Atom 0.3 à 1.0. Les classes de l’espace de noms [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication) peuvent définir les flux et éléments de flux qui peuvent représenter aussi bien des éléments RSS que des éléments Atom.

En outre, les formats Atom 1.0 et RSS 2.0 permettent tous les deux que les documents de flux contiennent des éléments ou attributs non définis dans les spécifications officielles. Avec le temps, ces éléments et attributs personnalisés sont devenus un moyen de définir des informations spécifiques à un domaine consommées par d’autres formats de données de services Web, comme GData et OData. Pour permettre la prise en charge de cette fonctionnalité ajoutée, la classe [**SyndicationNode**](/uwp/api/Windows.Web.Syndication.SyndicationNode) représente des éléments XML génériques. L’utilisation de **SyndicationNode** avec des classes de l’espace de noms [**Windows.Data.Xml.Dom**](/uwp/api/Windows.Data.Xml.Dom) permet aux applications d’accéder à leurs attributs, extensions et contenus éventuels.

Notez que pour la publication de contenu syndiqué, l’implémentation UWP du protocole Atom Publication ([**Windows.Web.AtomPub**](/uwp/api/Windows.Web.AtomPub)) prend uniquement en charge les opérations de contenu de flux conformément aux normes Atom et Atom Publication.

## <a name="using-syndicated-content-with-network-isolation"></a>Utilisation de contenu syndiqué avec l’isolement réseau

La fonctionnalité d’isolement réseau dans UWP permet à un développeur de contrôler et de limiter l’accès réseau par une application pour UWP. Toutes les applications n’ont pas besoin d’un accès au réseau. Néanmoins, pour celles qui en ont besoin, UWP propose différents niveaux d’accès qui peuvent être activés en sélectionnant les fonctionnalités appropriées.

L’isolement réseau permet à un développeur de définir pour chaque application la portée de l’accès réseau requis. Une application pour laquelle la portée appropriée ne serait pas définie ne peut pas accéder au type de réseau spécifié et au type spécifique de demande réseau (demandes sortantes à l’initiative du client ou à la fois demandes entrantes non sollicitées et demandes sortantes à l’initiative du client). La possibilité de définir et de mettre en œuvre l’isolement réseau garantit que si une application est compromise, elle ne pourra accéder qu’aux réseaux pour lesquels l’accès aura été expressément accordé à l’application. Cela réduit de façon significative la portée de l’impact sur d’autres applications et sur Windows.

L’isolement réseau affecte tous les éléments de classe des espaces de noms [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication) et [**Windows.Web.AtomPub**](/uwp/api/Windows.Web.AtomPub) qui essaient d’accéder au réseau. Windows applique activement l’isolement réseau. Un appel à un élément de classe de l’espace de noms **Windows.Web.Syndication** ou **Windows.Web.AtomPub** qui conduit à un accès réseau peut échouer en raison de l’isolement réseau si la fonctionnalité réseau appropriée n’a pas été activée.

Les fonctionnalités réseau d’une application sont configurées dans le manifeste de l’application à la création de cette dernière. Les fonctionnalités réseau sont généralement ajoutées à l’aide de Microsoft Visual Studio 2015 lors du développement de l’application. Elles peuvent également être définies manuellement dans le fichier manifeste de l’application à l’aide d’un éditeur de texte.

Pour plus d’informations sur l’isolement réseau et les fonctionnalités de réseau, voir la section « Fonctionnalités » dans la rubrique [Notions de base en matière de réseau](networking-basics.md).

## <a name="how-to-access-a-web-feed"></a>Comment accéder à un flux web

Cette section montre comment récupérer et afficher un flux web à l’aide des classes de l’espace de noms [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication) dans votre application pour UWP écrite en C# ou Javascript.

**Conditions préalables**

Pour vous assurer que votre application UWP est prête à être utilisée en réseau, vous devez définir les fonctionnalités réseau nécessaires dans le fichier **Package.appxmanifest** du projet. Si votre application doit se connecter en qualité de client à des services distants sur Internet, la fonctionnalité **internetClient** est nécessaire. Pour plus d’informations, voir la section « Fonctionnalités » dans la rubrique [Notions de base en matière de réseau](networking-basics.md).

**Récupération d’un contenu syndiqué dans un flux web**

Nous allons maintenant examiner du code qui illustre comment extraire un flux puis afficher chaque élément qu’il contient. Avant de pouvoir configurer et envoyer la requête, nous allons définir quelques variables que nous utiliserons durant l’opération et initialiser une instance de [**SyndicationClient**](/uwp/api/Windows.Web.Syndication.SyndicationClient), qui définit les méthodes et propriétés que nous utiliserons pour extraire et afficher le flux.

Le constructeur [**Uri**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) lève une exception si l’élément *uriString* passé au constructeur n’est pas un URI valide. Par conséquent, nous validons *uriString* à l’aide d’un bloc try/catch.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

Ensuite, nous configurerons la requête en définissant les informations d’identification de serveur (propriété [**ServerCredential**](/uwp/api/windows.web.syndication.syndicationclient.servercredential)), les informations d’identification de proxy (propriété [**ProxyCredential**](/uwp/api/windows.web.syndication.syndicationclient.proxycredential)) et les en-têtes HTTP (méthode [**SetRequestHeader**](/uwp/api/windows.web.syndication.syndicationclient.setrequestheader)) nécessaires. Une fois les paramètres de requête de base configurés, un objet [**Uri**](/uwp/api/windows.foundation.uri) valide est créé à l’aide d’une chaîne d’URI de flux fournie par l’application. L’objet **Uri** est ensuite transmis à la fonction [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) pour demander le flux.

En supposant que le contenu de flux souhaité a été renvoyé, l’exemple de code itère chaque élément de flux et appelle **displayCurrentItem** (que nous allons définir ensuite), pour afficher les éléments et leur contenu sous forme de liste dans l’interface utilisateur.

Vous devez écrire du code capable de gérer les exceptions au moment où vous appelez la plupart des méthodes réseau asynchrones. Votre gestionnaire d’exceptions peut récupérer des informations plus détaillées sur la cause de l’exception dans le but d’analyser l’échec et de prendre des décisions appropriées.

La méthode [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) lève une exception si aucune connexion n’a pu être établie avec le serveur HTTP ou si l’[**Uri**](/uwp/api/windows.foundation.uri) ne pointe pas vers un flux AtomPub ou RSS valide. L’exemple de code JavaScript utilise une fonction **onError** pour intercepter les exceptions, et affiche des informations plus détaillées sur l’exception si une erreur se produit.

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

Durant l’étape précédente, [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) a renvoyé le contenu de flux demandé et l’exemple de code a commencé à itérer les éléments de flux disponibles. Chacun de ces éléments est représenté à l’aide d’un objet [**SyndicationItem**](/uwp/api/Windows.Web.Syndication.SyndicationItem) qui inclut tout le contenu et les propriétés d’éléments offerts par la norme de syndication pertinente (RSS ou Atom). Dans l’exemple suivant, la fonction **displayCurrentItem** itère chaque élément et affiche son contenu dans différents éléments d’interface utilisateur.

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

Comme mentionné plus haut, le type de contenu représenté par un objet [**SyndicationItem**](/uwp/api/Windows.Web.Syndication.SyndicationItem) sera différent en fonction de la norme de flux (RSS ou Atom) utilisée pour publier le flux. Par exemple, un flux Atom est capable de fournir une liste de [**Contributors**](/uwp/api/windows.web.syndication.syndicationitem.contributors), ce qui n’est pas le cas d’un flux RSS. Toutefois, les éléments d’extension inclus dans un élément de flux qui ne sont pas pris en charge par l’une ou l’autre norme (par exemple, les éléments d’extension Dublin Core) sont accessibles à l’aide de la propriété [**SyndicationItem.ElementExtensions**](/uwp/api/windows.web.syndication.syndicationitem.elementextensions), puis affichés comme dans l’exemple de code suivant.

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```