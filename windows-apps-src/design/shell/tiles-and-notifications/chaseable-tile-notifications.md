---
Description: Utilisez les notifications de vignettes localisables pour déterminer l’affichage de votre application sur sa vignette dynamique lorsque l’utilisateur clique dessus.
title: Notifications par vignette pouvant être suivies
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, vignettes chaseables, vignettes dynamiques, notifications de vignettes Chase
ms.localizationpriority: medium
ms.openlocfilehash: 951dc891fb34ae4be7551c08ff47eabc19ae9eb6
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100277"
---
# <a name="chaseable-tile-notifications"></a>Notifications par vignette pouvant être suivies

Les notifications de vignettes chaseables vous permettent de déterminer les notifications de vignettes affichées par la vignette dynamique de votre application lorsque l’utilisateur clique sur la vignette.  
Par exemple, une application d’actualités peut utiliser cette fonctionnalité pour déterminer le récit d’actualité affiché par la vignette dynamique quand l’utilisateur l’a lancée. Cela peut s’assurer que l’histoire est affichée de manière visible afin que l’utilisateur puisse la trouver. 

> [!IMPORTANT]
> **Nécessite une mise à jour anniversaire**: pour utiliser des notifications de vignettes chaseables avec des applications UWP C#, C++ ou vb, vous devez cibler le kit de développement logiciel (SDK) 14393 et exécuter la version 14393 ou une version ultérieure. Pour les applications UWP basées sur JavaScript, vous devez cibler le kit de développement logiciel (SDK) 17134 et exécuter la version 17134 ou une version ultérieure. 


> **API importantes**: [propriété LaunchActivatedEventArgs. TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [classe TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Fonctionnement

Pour activer les notifications de vignettes chaseables, utilisez la propriété **arguments** sur la charge utile de notification par vignette, similaire à la propriété Launch sur la charge utile de notification Toast, pour incorporer des informations sur le contenu dans la notification de vignette.

Lorsque votre application est lancée par le biais de la vignette dynamique, le système renvoie une liste d’arguments à partir des notifications de vignettes actuelles/récemment affichées.


## <a name="when-to-use-chaseable-tile-notifications"></a>Quand utiliser les notifications de vignettes chaseables

Les notifications de vignettes chaseables sont généralement utilisées lorsque vous utilisez la file d’attente de notification sur votre vignette dynamique (ce qui signifie que vous parcourez jusqu’à 5 notifications différentes). Ils sont également bénéfiques lorsque le contenu de votre vignette dynamique est potentiellement désynchronisé avec le contenu le plus récent de l’application. Par exemple, l’application News actualise sa vignette dynamique toutes les 30 minutes, mais lorsque l’application est lancée, elle charge les dernières actualités (ce qui peut ne pas inclure un élément qui se trouvait sur la vignette à partir du dernier intervalle d’interrogation). Dans ce cas, l’utilisateur risque de ne pas pouvoir trouver le récit qu’il a vu sur sa vignette dynamique. C’est là que les notifications de vignettes peuvent vous aider, en vous permettant de vous assurer que ce que l’utilisateur a vu sur sa vignette est facilement détectable.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Que faire avec les notifications de vignettes Chase

L’élément le plus important à noter est que dans la plupart des scénarios, **vous ne devez pas accéder directement à la notification spécifique** qui se trouvait sur la vignette lorsque l’utilisateur cliquait dessus. Votre vignette dynamique est utilisée comme point d’entrée de votre application. Il peut y avoir deux scénarios lorsqu’un utilisateur clique sur votre vignette dynamique : (1) il souhaitait lancer votre application normalement, ou (2) qu’elle souhaitait obtenir plus d’informations sur une notification spécifique qui était sur la vignette dynamique. Étant donné qu’il n’existe aucun moyen pour l’utilisateur d’indiquer explicitement le comportement qu’il veut, l’expérience idéale consiste à **lancer votre application normalement, tout en vous assurant que la notification que l’utilisateur a vue est facilement détectable**.

Par exemple, le fait de cliquer sur la vignette dynamique de l’application MSN News lance l’application normalement : elle affiche la page d’hébergement, ou l’article que l’utilisateur a déjà lu. Toutefois, dans la page d’hébergement, l’application s’assure que l’histoire de la vignette dynamique est facilement détectable. De cette façon, les deux scénarios sont pris en charge : le scénario dans lequel vous souhaitez simplement lancer/reprendre l’application, et le scénario dans lequel vous souhaitez afficher l’histoire spécifique.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Comment inclure la propriété arguments dans la charge utile de notification de vignette

Dans une charge utile de notification, la propriété arguments permet à votre application de fournir des données que vous pouvez utiliser pour identifier ultérieurement la notification. Par exemple, vos arguments peuvent inclure l’ID du récit, de sorte que, lorsqu’il est lancé, vous pouvez récupérer et afficher l’histoire. La propriété accepte une chaîne, qui peut être sérialisée comme vous le souhaitez (chaîne de requête, JSON, etc.), mais nous recommandons généralement le format de chaîne de requête, car il est très léger et encodé en XML.

La propriété peut être définie à la fois sur les éléments **TileVisual** et **TileBinding** , et s’affiche en cascade. Si vous souhaitez les mêmes arguments à chaque taille de mosaïque, définissez simplement les arguments sur le **TileVisual**. Si vous avez besoin d’arguments spécifiques pour des tailles de vignette spécifiques, vous pouvez définir les arguments sur des éléments **TileBinding** individuels.

Cet exemple crée une charge utile de notification qui utilise la propriété arguments pour que la notification puisse être identifiée ultérieurement. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Comment vérifier la propriété arguments lors du lancement de votre application

La plupart des applications ont un fichier App.xaml.cs qui contient une substitution pour la méthode [OnLaunched](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) . Comme son nom l’indique, votre application appelle cette méthode lorsqu’elle est lancée. Il accepte un seul argument, un objet [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) .

L’objet LaunchActivatedEventArgs a une propriété qui active les notifications Chase : la [propriété TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), qui fournit l’accès à un [objet TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Lorsque l’utilisateur lance votre application à partir de sa vignette (au lieu de la liste d’applications, de la recherche ou de tout autre point d’entrée), votre application initialise cette propriété.

L' [objet TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) contient une propriété appelée [RecentlyShownNotifications](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), qui contient la liste des notifications qui ont été affichées sur la vignette au cours des 15 dernières minutes. Le premier élément de la liste représente la notification actuellement sur la vignette, et les éléments suivants représentent les notifications que l’utilisateur a vues avant l’élément actuel. Si votre vignette a été effacée, cette liste est vide.

Chaque ShownTileNotification a une propriété arguments. La propriété arguments sera initialisée avec la chaîne d’arguments de votre charge utile de notification par vignette, ou null si votre charge utile n’incluait pas la chaîne d’arguments.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


### <a name="accessing-onlaunched-from-desktop-applications"></a>Accès à OnLaunched à partir d’applications de bureau

Les applications de bureau (comme WPF, etc.) à l’aide du [pont de bureau](https://developer.microsoft.com/windows/bridges/desktop)peuvent également utiliser des vignettes chaseuses ! La seule différence est l’accès aux arguments OnLaunched. Notez que vous devez [d’abord empaqueter votre application avec le pont Desktop](/windows/msix/desktop/source-code-overview).

> [!IMPORTANT]
> **Nécessite la mise à jour du 2018 octobre**: pour utiliser l' `AppInstance.GetActivatedEventArgs()` API, vous devez cibler le kit de développement logiciel (SDK) 17763 et exécuter Build 17763 ou une version ultérieure.

Pour les applications de bureau, pour accéder aux arguments de lancement, procédez comme suit...

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>Exemple de XML brut

Si vous utilisez des données XML brutes au lieu de la bibliothèque de notifications, voici le code XML.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Articles connexes

- [LaunchActivatedEventArgs. TileActivatedInfo, propriété](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo, classe](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)