---
Description: Votre application peut charger des fichiers de ressources d’image contenant des images adaptées pour le facteur d’échelle de l’affichage, le thème, le contraste élevé et d’autres contextes d’exécution.
title: Charger des images et des ressources adaptées pour la mise à l’échelle, le thème, le contraste élevé et autres
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 0a1d6639a901385c3a33fb0ed670b9b7e4cf683e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157603"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Charger des images et des ressources adaptées pour la mise à l’échelle, le thème, le contraste élevé et autres
Votre application peut charger des fichiers de ressources d’image (ou d’autres fichiers de ressources) adaptés pour [afficher le facteur d’échelle](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), le thème, le contraste élevé et d’autres contextes d’exécution. Ces images peuvent être référencées à partir du code impératif ou du balisage XAML, par exemple en tant que propriété **source** d’une **image**. Ils peuvent également apparaître dans le fichier source du manifeste de votre package d’application (le `Package.appxmanifest` fichier) &mdash; , par exemple, sous la forme de l’icône de l’application sous l’onglet ressources visuelles du concepteur de manifeste Visual Studio &mdash; ou sur vos vignettes et toasts. En utilisant des qualificateurs dans les noms de fichiers de vos images et en les chargeant éventuellement de manière dynamique à l’aide d’un [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), vous pouvez charger le fichier image le plus approprié qui correspond le mieux aux paramètres d’exécution de l’utilisateur pour l’échelle d’affichage, le thème, le contraste élevé, la langue et d’autres contextes.

Une ressource d’image est contenue dans un fichier de ressources d’image. Vous pouvez également considérer l’image comme un élément multimédia et le fichier qui la contient comme un fichier de ressources. vous pouvez trouver ces types de fichiers de ressources dans le dossier \Assets de votre projet. Pour plus d’informations sur l’utilisation de qualificateurs dans les noms de vos fichiers de ressources d’image, consultez [adapter vos ressources aux qualificateurs de langage, de mise à l’échelle et autres](tailor-resources-lang-scale-contrast.md).

Certains qualificateurs courants pour les images sont l' [échelle](tailor-resources-lang-scale-contrast.md#scale), le [thème](tailor-resources-lang-scale-contrast.md#theme), le [contraste](tailor-resources-lang-scale-contrast.md#contrast)et la [cible](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Qualifier une ressource d’image à des fins de mise à l’échelle, de thème et de contraste
La valeur par défaut du `scale` qualificateur est `scale-100` . Par conséquent, ces deux variantes sont équivalentes (elles fournissent toutes deux une image à l’échelle 100 ou le facteur de mise à l’échelle 1).

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


Vous pouvez utiliser des qualificateurs dans des noms de dossiers au lieu de noms de fichiers. Il s’agit d’une meilleure stratégie si vous avez plusieurs fichiers d’éléments multimédias par qualificateur. À des fins d’illustration, ces deux variantes sont équivalentes aux deux valeurs ci-dessus.

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

Vous trouverez ci-dessous un exemple de la façon dont vous pouvez fournir des variantes d’une ressource d’image &mdash; nommée `/Assets/Images/logo.png` &mdash; pour différents paramètres de l’échelle d’affichage, du thème et du contraste élevé. Cet exemple utilise le nom de dossier.

<blockquote>
<pre>
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
</pre>
</blockquote>

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Référencer une image ou une autre ressource à partir du balisage et du code XAML
Le nom &mdash; ou l’identificateur &mdash; d’une ressource d’image est son chemin d’accès et son nom de fichier avec tous les qualificateurs supprimés. Si vous nommez des dossiers et/ou des fichiers comme dans l’un des exemples de la section précédente, vous avez une ressource d’image unique et son nom (comme chemin d’accès absolu) est `/Assets/Images/logo.png` . Voici comment vous utilisez ce nom dans le balisage XAML.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Notez que vous utilisez le `ms-appx` modèle d’URI, car vous faites référence à un fichier qui provient du package de votre application. Consultez [schémas d’URI](uri-schemes.md). Et voici comment faire référence à la même ressource d’image dans le code impératif.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Vous pouvez utiliser `ms-appx` pour charger des fichiers arbitraires à partir de votre package d’application.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

Le `ms-appx-web` schéma accède aux mêmes fichiers que `ms-appx` , mais dans le compartiment Web.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Pour chacun des scénarios indiqués dans ces exemples, utilisez la surcharge de [constructeur d’URI](/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) qui déduit le [UriKind](/dotnet/api/system.urikind). Spécifiez un URI absolu valide, y compris le schéma et l’autorité, ou laissez l’autorité par défaut au package de l’application comme dans l’exemple ci-dessus.

Notez que dans ces exemples d’URI, le schéma (« `ms-appx` » ou « » `ms-appx-web` ) est suivi de « `://` », suivi d’un chemin d’accès absolu. Dans un chemin d’accès absolu, le «» de début `/` provoque l’interprétation du chemin d’accès à partir de la racine du package.

> [!NOTE]
> Les `ms-resource` schémas d’URI (pour les [ressources de type chaîne](localize-strings-ui-manifest.md)) et `ms-appx(-web)` (pour les images et autres éléments multimédias) effectuent une correspondance de qualificateur automatique pour trouver la ressource la plus appropriée pour le contexte actuel. Le `ms-appdata` schéma d’URI (utilisé pour charger les données d’application) n’effectue pas de correspondance automatique de ce type, mais vous pouvez répondre au contenu de [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) et charger explicitement les ressources appropriées à partir des données de l’application à l’aide de leur nom de fichier physique complet dans l’URI. Pour plus d’informations sur les données d’application, consultez [stocker et récupérer des paramètres et d’autres données d’application](../design/app-settings/store-and-retrieve-app-data.md). Les schémas d’URI Web (par exemple,, `http` `https` et `ftp` ) n’effectuent pas de correspondance automatique. Pour plus d’informations sur ce que vous devez faire dans ce cas, consultez [hébergement et chargement d’images dans le Cloud](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Les chemins d’accès absolus sont un bon choix si vos fichiers image restent là où ils se trouvent dans la structure du projet. Si vous souhaitez être en mesure de déplacer un fichier image, mais que vous êtes prudent qu’il reste dans le même emplacement par rapport à son fichier de balisage XAML de référence, au lieu d’un chemin d’accès absolu, vous pouvez utiliser un chemin d’accès relatif au fichier de balisage contenant. Dans ce cas, vous n’avez pas besoin d’utiliser un schéma d’URI. Vous bénéficierez toujours de la correspondance de qualificateur automatique dans ce cas, mais uniquement parce que vous utilisez le chemin d’accès relatif dans le balisage XAML.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Consultez également [prise en charge des vignettes et des toasts pour la langue, l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Qualifier une ressource image pour la cible
Vous pouvez utiliser les `scale` `targetsize` qualificateurs et sur différentes variantes de la même ressource image, mais vous ne pouvez pas les utiliser sur une seule variante d’une ressource. En outre, vous devez définir au moins une variante sans `TargetSize` qualificateur. Cette variante doit soit définir une valeur pour `scale` , soit lui laisser la valeur par défaut `scale-100` . Par conséquent, ces deux variantes de la `/Assets/Square44x44Logo.png` ressource sont valides.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Et ces deux variantes sont valides. 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Mais ce variant n’est pas valide.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Faire référence à un fichier image à partir de votre manifeste de package d’application
Si vous nommez des dossiers et/ou des fichiers comme dans l’un des deux exemples valides de la section précédente, vous avez une seule ressource d’image d’icône d’application et son nom (comme chemin d’accès relatif) est `Assets\Square44x44Logo.png` . Dans le manifeste de votre package d’application, il vous suffit de faire référence à la ressource par son nom. Il n’est pas nécessaire d’utiliser un schéma d’URI.

![Ajouter une ressource, anglais](images/app-icon.png)

C’est tout ce que vous devez faire et le système d’exploitation effectuera la correspondance de qualificateur automatique pour trouver la ressource la plus appropriée pour le contexte actuel. Pour obtenir la liste de tous les éléments du manifeste de package d’application que vous pouvez localiser ou auxquels vous pouvez accéder de cette manière, consultez [éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Qualifier une ressource d’image pour LayoutDirection
Consultez [mise en miroir d’images](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Charger une image pour une langue ou un autre contexte spécifique
Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../design/globalizing/globalizing-portal.md).

Le [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) par défaut (obtenu à partir de [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contient une valeur de qualificateur pour chaque nom de qualificateur, représentant le contexte d’exécution par défaut (en d’autres termes, les paramètres de l’utilisateur et de l’ordinateur actuels). Les fichiers image sont mis en correspondance &mdash; en fonction des qualificateurs de leurs noms &mdash; par rapport aux valeurs de qualificateur dans ce contexte d’exécution.

Toutefois, il peut arriver que vous souhaitiez que votre application remplace les paramètres système et soit explicite à propos de la langue, de l’échelle ou d’une autre valeur de qualificateur à utiliser lors de la recherche d’une image correspondante à charger. Par exemple, vous souhaiterez peut-être contrôler exactement quand et quelles images à contraste élevé sont chargées.

Pour ce faire, vous pouvez créer un nouveau **ResourceContext** (au lieu d’utiliser celui par défaut), en remplaçant ses valeurs, puis en utilisant cet objet de contexte dans vos recherches d’images.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

Pour le même effet à un niveau global, vous *pouvez* remplacer les valeurs de qualificateur dans le **ResourceContext**par défaut. Au lieu de cela, nous vous conseillons d’appeler [**ResourceContext. SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Vous définissez des valeurs une fois avec un appel à **SetGlobalQualifierValue** , puis ces valeurs sont en vigueur sur le **ResourceContext** par défaut chaque fois que vous l’utilisez pour les recherches. Par défaut, la classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) utilise le **ResourceContext**par défaut.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Mise à jour des images en réponse aux événements de modification de valeur de qualificateur
Votre application en cours d’exécution peut répondre aux modifications apportées aux paramètres système qui affectent les valeurs de qualificateur dans le contexte de ressource par défaut. L’un de ces paramètres système appelle l’événement [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) sur [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En réponse à cet événement, vous pouvez recharger vos images à l’aide du **ResourceContext**par défaut, que [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) utilise par défaut.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>API importantes
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext. SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Rubriques connexes
* [Adaptez vos ressources à la langue, à la mise à l’échelle et à d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](localize-strings-ui-manifest.md)
* [Stocker et récupérer des paramètres et autres données d’application](../design/app-settings/store-and-retrieve-app-data.md)
* [Prise en charge des vignettes et des toasts pour la langue, la mise à l’échelle et le contraste élevé](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
* [Éléments de manifeste localisables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Mise en miroir des images](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalisation et localisation](../design/globalizing/globalizing-portal.md)