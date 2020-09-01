---
title: Poursuivre l’activité utilisateur, même sur différents appareils
description: Cette rubrique explique comment aider les utilisateurs à reprendre ce qu’ils faisaient dans votre application, même sur plusieurs appareils.
keywords: activité de l’utilisateur, activités de l’utilisateur, chronologie, Cortana reprendre là où vous l’avez quittée, Cortana reprend là où je l’ai quitté, le projet Rome
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ebaca3b831ae30637a88d01319a89d139dde1cf8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162623"
---
# <a name="continue-user-activity-even-across-devices"></a>Poursuivre l’activité utilisateur, même sur différents appareils

Cette rubrique explique comment aider les utilisateurs à reprendre ce qu’ils faisaient dans votre application sur leur ordinateur et sur plusieurs appareils.

## <a name="user-activities-and-timeline"></a>Chronologie et activités de l’utilisateur

L’heure quotidienne est répartie sur plusieurs appareils. Nous pouvons utiliser notre téléphone sur le bus, sur un PC pendant la journée, puis sur un téléphone ou une tablette du soir. À partir de Windows 10 Build 1803 ou version ultérieure, la création d’une [activité utilisateur](/uwp/api/windows.applicationmodel.useractivities.useractivity) fait apparaître cette activité dans la chronologie Windows et dans la sélection de Cortana où j’ai quitté la fonctionnalité. La chronologie est une vue de tâche enrichie qui tire parti des activités de l’utilisateur pour afficher une vue chronologique de ce que vous avez travaillé. Il peut également inclure ce que vous travailliez sur plusieurs appareils.

![Image de la chronologie Windows](images/timeline.png)

De même, la liaison de votre téléphone à votre PC Windows vous permet de continuer ce que vous étiez déjà fait sur votre appareil iOS ou Android.

Imaginez un **UserActivity** comme un événement spécifique sur lequel l’utilisateur travaillait dans votre application. Par exemple, si vous utilisez un lecteur RSS, un **UserActivity** peut être le flux que vous lisez. Si vous jouez à un jeu, le **UserActivity** peut être le niveau que vous jouez. Si vous écoutez une application musicale, **UserActivity** peut être la sélection que vous écoutez. Si vous travaillez sur un document, le **UserActivity** peut être à l’endroit où vous l’avez quitté, et ainsi de suite.  En bref, un **UserActivity** représente une destination au sein de votre application, ce qui permet à l’utilisateur de reprendre ce qu’il faisait.

Lorsque vous faites appel à un **UserActivity** en appelant [UserActivity. CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), le système crée un enregistrement d’historique indiquant les heures de début et de fin de ce **UserActivity**. À mesure que vous vous renouvelez avec cette **UserActivity** au fil du temps, plusieurs enregistrements d’historique sont enregistrés pour celle-ci.

## <a name="add-user-activities-to-your-app"></a>Ajouter des activités utilisateur à votre application

Un [UserActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity) est l’unité de l’engagement utilisateur dans Windows. Il se compose de trois parties : un URI utilisé pour activer l’application à laquelle l’activité appartient, des visuels et des métadonnées qui décrivent l’activité.

1. Le [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) est utilisé pour reprendre l’application avec un contexte spécifique. En règle générale, ce lien prend la forme d’un gestionnaire de protocole pour un schéma (par exemple, « My-app://page2 ? action = Edit ») ou d’un AppUriHandler (par exemple, http://constoso.com/page2?action=edit) .
2. [VisualElements](/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) expose une classe qui permet à l’utilisateur d’identifier visuellement une activité avec un titre, une description ou des éléments de carte adaptative.
3. Enfin, le [contenu](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) est l’endroit où vous pouvez stocker les métadonnées de l’activité qui peuvent être utilisées pour regrouper et récupérer des activités dans un contexte spécifique. Souvent, cette opération prend la forme de [https://schema.org](https://schema.org) données.

Pour ajouter un **UserActivity** à votre application :

1. Générer des objets **UserActivity** lorsque le contexte de l’utilisateur change au sein de l’application (par exemple, la navigation entre les pages, le nouveau niveau de jeu, etc.)
2. Remplissez les objets **UserActivity** avec l’ensemble minimal de champs requis : [ActivityID](/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId), [ActivationUri](/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri)et [UserActivity. VisualElements. texteaffiché](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText).
3. Ajoutez un gestionnaire de modèles personnalisé à votre application afin qu’elle puisse être réactivée par un **UserActivity**.

Un **UserActivity** peut être intégré à une application avec seulement quelques lignes de code. Par exemple, imaginez ce code dans MainPage.xaml.cs, à l’intérieur de la classe MainPage (Remarque : suppose `using Windows.ApplicationModel.UserActivities;` ) :

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

La première ligne de la `GenerateActivityAsync()` méthode ci-dessus obtient le [UserActivityChannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)d’un utilisateur. Il s’agit du flux sur lequel les activités de cette application seront publiées. La ligne suivante interroge le canal d’une activité appelée `MainPage` .

* Votre application doit nommer les activités de sorte que le même ID soit généré chaque fois que l’utilisateur se trouve à un emplacement particulier dans l’application. Par exemple, si votre application est basée sur une page, utilisez un identificateur pour la page ; Si son document est basé sur, utilisez le nom du document (ou un hachage du nom).
* S’il existe une activité dans le flux avec le même ID, cette activité est retournée à partir du canal ayant la `UserActivity.State` valeur [published](/uwp/api/windows.applicationmodel.useractivities.useractivitystate). S’il n’existe aucune activité portant ce nom, et que l’activité New est retournée avec la `UserActivity.State` valeur **New**.
* Les activités sont limitées à votre application. Vous n’avez pas à vous soucier de l’ID d’activité en conflit avec les ID dans d’autres applications.

Après avoir obtenu ou créé l' **UserActivity**, spécifiez les deux autres champs obligatoires :  `UserActivity.VisualElements.DisplayText` et `UserActivity.ActivationUri` .

Ensuite, enregistrez les métadonnées **UserActivity** en appelant [SaveAsync](/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync), et enfin [CreateSession](/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession), qui retourne un [UserActivitySession](/uwp/api/windows.applicationmodel.useractivities.useractivitysession). Le **UserActivitySession** est l’objet que nous pouvons utiliser pour gérer le moment où l’utilisateur est réellement engagé avec le **UserActivity**. Par exemple, nous devons appeler `Dispose()` sur le **UserActivitySession** quand l’utilisateur quitte la page. Dans l’exemple ci-dessus, nous appelons également `Dispose()` on `_currentActivity` avant d’appeler `CreateSession()` . Cela est dû au fait que nous avons créé `_currentActivity` un champ de membre de notre page et que nous souhaitons arrêter une activité existante avant de démarrer la nouvelle (Remarque : `?` est l' [opérateur conditionnel null](/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-) qui teste la valeur null avant d’effectuer l’accès au membre).

Étant donné que, dans ce cas, `ActivationUri` est un schéma personnalisé, nous devons également inscrire le protocole dans le manifeste de l’application. Cette opération s’effectue dans le fichier XML Package. AppManifest, ou à l’aide du concepteur.

Pour apporter la modification à l’aide du concepteur, double-cliquez sur le fichier Package. AppManifest dans votre projet pour lancer le concepteur, sélectionnez l’onglet **déclarations** et ajoutez une définition de **protocole** . La seule propriété qui doit être remplie, pour l’instant, est **Name**. Il doit correspondre à l’URI que nous avons spécifiés ci-dessus, `my-app` .

À présent, nous devons écrire du code pour indiquer à l’application ce qu’elle doit faire lorsqu’elle a été activée par un protocole. Nous allons remplacer la `OnActivated` méthode dans App.Xaml.cs pour passer l’URI à la page principale, comme suit :

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

Ce que fait ce code détecte si l’application a été activée via un protocole. Si tel était le cas, il recherchera ce que l’application doit faire pour reprendre la tâche pour laquelle elle est activée. En tant qu’application simple, la seule activité que cette application reprend vous met sur la page secondaire lorsque l’application est lancée.

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>Utiliser des cartes adaptatives pour améliorer l’expérience de la chronologie

Les activités de l’utilisateur s’affichent dans Cortana et chronologie. Lorsque les activités s’affichent dans la chronologie, nous les affichons à l’aide de l’infrastructure de [carte adaptative](https://adaptivecards.io/) . Si vous ne fournissez pas de carte adaptative pour chaque activité, la chronologie créera automatiquement une carte d’activité simple en fonction du nom et de l’icône de votre application, du champ titre et du champ description facultative. Voici un exemple de charge utile de carte adaptative et la carte qu’elle produit.

![Carte adaptative](images/adaptivecard.png)]

Exemple de chaîne JSON de charge utile de carte adaptative :

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

Ajoutez la charge utile cartes adaptatives sous la forme d’une chaîne JSON au **UserActivity** comme suit :

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>Multiplateforme et intégration de service à service

Si votre application s’exécute sur plusieurs plateformes (par exemple, sur Android et iOS) ou conserve l’état utilisateur dans le Cloud, vous pouvez publier UserActivities via [Microsoft Graph](https://developer.microsoft.com/graph).
Une fois que votre application ou service est authentifié avec un compte Microsoft, il suffit d’appeler deux appels REST simples pour générer [des](/graph/api/resources/projectrome-historyitem) objets d' [activité](/graph/api/resources/projectrome-activity) et d’historique, en utilisant les mêmes données que celles décrites ci-dessus.

## <a name="summary"></a>Résumé

Vous pouvez utiliser l’API [UserActivity](/uwp/api/windows.applicationmodel.useractivities) pour faire apparaître votre application dans Timeline et Cortana.
* En savoir plus sur l' [API **UserActivity**](/uwp/api/windows.applicationmodel.useractivities)
* Consultez l' [exemple de code](https://github.com/Microsoft/project-rome).
* Découvrez [des cartes adaptatives plus sophistiquées](https://adaptivecards.io/).
* Publiez un **UserActivity** à partir d’iOS, d’Android ou de votre service web via [Microsoft Graph](https://developer.microsoft.com/graph).
* En savoir plus sur [le projet Rome sur GitHub](https://github.com/Microsoft/project-rome).

## <a name="key-apis"></a>API clés

* [Espace de noms UserActivities](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Rubriques connexes

* [Activités de l’utilisateur (documents de Rome du projet)](/windows/project-rome/user-activities/)
* [Cartes adaptatives](/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](https://adaptivecards.io/)
* [Gérer l’activation des URI](./handle-uri-activation.md)
* [En contact avec vos clients sur n’importe quelle plateforme à l’aide du Microsoft Graph, du flux d’activités et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)