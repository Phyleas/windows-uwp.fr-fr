---
description: Pour exécuter une expérience dans votre app. de plateforme Windows universelle (UWP) avec des tests A/B, vous devez code l’expérience dans votre application.
title: Coder votre application à des fins d’expérimentation
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, kit de développement logiciel (SDK) Microsoft Store services, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: a5229be4d0ea2ce98ec10530458fe29af10fa7f0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033602"
---
# <a name="code-your-app-for-experimentation"></a>Coder votre application à des fins d’expérimentation

Une fois que vous avez [créé un projet et défini des variables distantes dans l’espace partenaires](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), vous êtes prêt à mettre à jour le code de votre application plateforme Windows universelle (UWP) pour effectuer les opérations suivantes :
* Recevoir des valeurs de variables distantes à partir de l’espace partenaires.
* utiliser des variables distantes pour configurer des expériences d’application pour vos utilisateurs ;
* Consignez les événements dans l’espace partenaires qui indiquent le moment où les utilisateurs ont consulté votre expérience et effectué une action souhaitée (également appelée *conversion* ).

Pour ajouter ce comportement à votre application, vous allez utiliser les API fournies par le Microsoft Store Services SDK.

Les sections suivantes décrivent le processus général d’obtention de variations pour votre expérience et de journalisation des événements dans l’espace partenaires. Une fois que vous avez codé votre application pour l’expérimentation, vous pouvez [définir une expérience dans l’espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md). Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Certaines des API d’expérimentation dans le kit de développement logiciel (SDK) Microsoft Store services utilisent le [modèle asynchrone](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) pour récupérer des données de l’espace partenaires. Cela signifie qu’une partie de l’exécution de ces méthodes peut avoir lieu après l’appel des méthodes, afin que l’interface utilisateur de votre application puisse rester réactive pendant que les opérations se terminent. Le modèle asynchrone exige que votre application utilise le mot-clé **async** et l’opérateur **await** pour appeler les API, comme illustré par les exemples de code dans cet article. Par convention, les méthodes asynchrones se terminent par **Async** .

## <a name="configure-your-project"></a>Configurer votre projet

Pour commencer, installez le Kit de développement logiciel Microsoft Store Services SDK sur votre ordinateur de développement et ajoutez les références nécessaires à votre projet.

1. [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Ouvrez votre projet dans Visual Studio.
3. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références** , puis sélectionnez **Ajouter une référence** .
3. Dans le **Gestionnaire de références** , développez **Windows universel** , puis cliquez sur **Extensions** .
4. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK** .

> [!NOTE]
> Les exemples de code de cet article partent du principe que votre fichier de code **utilise** des instructions pour les espaces de noms **System. Threading. Tasks** et **Microsoft. services. Store. engagement** .

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obtenir des données de variante et consigner l’événement d’affichage pour votre expérience

Dans le projet, recherchez le code de la fonctionnalité que vous souhaitez modifier dans votre expérience. Ajoutez du code qui récupère les données pour une variation, utilisez ces données pour modifier le comportement de la fonctionnalité que vous testez, puis consignez l’événement d’affichage de votre expérience dans le service de test A/B dans l’espace partenaires.

Le code spécifique requis dépendra de votre application, mais l’exemple suivant illustre le processus de base. Pour obtenir un exemple de code complet, consultez [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="ExperimentCodeSample":::

Les étapes suivantes décrivent les éléments importants de ce processus en détail.

1. Déclarez un objet [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) qui représente l’attribution de variante actuelle et un objet [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) que vous allez utiliser pour enregistrer les événements d’affichage et de conversion dans l’espace partenaires.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet1":::

2. Déclarez une variable de chaîne affectée à l’[ID de projet](run-app-experiments-with-a-b-testing.md#terms) de l’expérience que vous souhaitez récupérer.
    > [!NOTE]
    > Vous obtenez un ID de projet lorsque vous [créez un projet dans l’espace partenaires](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). L’ID de projet présenté ci-dessous n’est fourni qu’à titre d’exemple.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet2":::

3. Obtenez l’affectation de variante mise en cache actuelle de votre expérience en appelant la méthode statique [GetCachedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) et transmettez l’ID de projet de votre expérience à la méthode. Cette méthode renvoie un objet [StoreServicesExperimentVariationResult](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) qui donne accès à l’affectation de variante via la propriété [ExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet3":::

4. Vérifiez la propriété [IsStale](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) pour déterminer si l’affectation de variante mise en cache doit être actualisée avec une affectation de variante distante à partir du serveur. Si tel n’est pas le cas, appelez la méthode statique [GetRefreshedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) pour rechercher une affectation de variante mise à jour sur le serveur et actualiser la variante mise en cache locale.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet4":::

5. Utilisez les méthodes [GetBoolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32) ou [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) de l’objet [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) pour obtenir les valeurs de l’affectation de variante. Dans chaque méthode, le premier paramètre est le nom de la variation que vous souhaitez récupérer (il s’agit du même nom que celui d’une variante que vous entrez dans l’espace partenaires). Le deuxième paramètre est la valeur par défaut que la méthode doit retourner s’il n’est pas en mesure de récupérer la valeur spécifiée dans l’espace partenaires (par exemple, s’il n’y a aucune connectivité réseau) et qu’une version mise en cache de la variante n’est pas disponible.

    L’exemple suivant utilise [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) pour obtenir une variable nommée *buttonText* et spécifie la valeur par défaut **Grey Button** à cette variable.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet5":::

6. Dans votre code, utilisez les valeurs de variable pour modifier le comportement de la fonctionnalité que vous testez. Par exemple, le code suivant affecte la valeur *buttonText* au contenu d’un bouton dans votre application. Cet exemple suppose que vous avez déjà défini ce bouton ailleurs dans votre projet.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet6":::

7. Enfin, consignez l' [événement d’affichage](run-app-experiments-with-a-b-testing.md#terms) de votre expérience dans le service de test A/B de l’espace partenaires. Initialisez le champ ```logger``` sur un objet [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) et appelez la méthode [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Transmettez l’objet [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) qui représente l’attribution de variation actuelle (cet objet fournit le contexte relatif à l’événement à l’espace partenaires) et le nom de l’événement d’affichage de votre expérience. Celui-ci doit correspondre au nom de l’événement d’affichage que vous entrez pour votre expérience dans l’espace partenaires. Votre code doit consigner l’événement d’affichage lorsque l’utilisateur commence à visualiser une variante faisant partie intégrante de votre expérience.

    L’exemple suivant montre comment consigner un événement d’affichage nommé **userViewedButton** . Dans cet exemple, l’objectif est d’inciter l’utilisateur à cliquer sur un bouton dans l’application, afin de consigner l’événement d’affichage une fois que l’application a récupéré les données de variante (en l’occurrence, le texte du bouton) et lui a attribué le contenu du bouton.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet7":::

## <a name="log-conversion-events-to-partner-center"></a>Consigner les événements de conversion dans l’espace partenaires

Ensuite, ajoutez le code qui journalise les [événements de conversion](run-app-experiments-with-a-b-testing.md#terms) au service de test A/B dans l’espace partenaires. Votre code doit consigner un événement de conversion quand l’utilisateur atteint un objectif pour votre expérience. Le code spécifique dont vous avez besoin dépend de votre application, mais voici les étapes générales. Pour obtenir un exemple de code complet, consultez [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Dans le code qui s’exécute quand l’utilisateur atteint un objectif pour l’un des objectifs de l’expérience, appelez de nouveau la méthode [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) et transmettez l’objet [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) et le nom d’un événement de conversion pour votre expérience. Celui-ci doit correspondre à l’un des noms d’événements de conversion que vous entrez pour votre expérience dans l’espace partenaires.

    L’exemple suivant consigne un événement de conversion nommé **userClickedButton** à partir du gestionnaire d’événements **Click** pour un bouton. Dans cet exemple, l’objectif de l’expérience est d’obtenir de l’utilisateur qu’il clique sur le bouton.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet8":::

## <a name="next-steps"></a>Étapes suivantes

Dès lors que vous avez codé l’expérience dans votre application, vous êtes prêt pour les étapes suivantes :
1. [Définissez votre expérience dans l’espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md). Créez une expérience qui définisse les événements d’affichage, les événements de conversion et les variantes uniques pour votre test A/B.
2. [Exécutez et gérez votre expérience dans l’espace partenaires](manage-your-experiment.md).


## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans l’espace partenaires](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Définir votre expérience dans l’Espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans l’Espace partenaires](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
