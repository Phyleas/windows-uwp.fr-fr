---
title: Modifier dynamiquement les listes d’expressions VCD de Cortana-conception et développement UWP de Cortana
description: Accédez à la liste des expressions prises en charge (éléments PhraseList) et mettez-les à jour dans un fichier de définition de commande vocale (VCD) au moment de l’exécution à l’aide du résultat de la reconnaissance vocale.
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 9f9e0aeb1cf23eb64df3104cf1f90e2b30d083ba
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057766"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>Modifier dynamiquement les listes d’expressions de VCD VCD

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).

Accédez à la liste des expressions prises en charge (éléments **PhraseList** ) et mettez-les à jour dans un fichier de définition de commande vocale (VCD) au moment de l’exécution à l’aide du résultat de la reconnaissance vocale.

> [!NOTE]
> Une commande vocale est un énoncé unique avec un but spécifique, défini dans un fichier de définition de commande vocale (VCD), dirigé vers une application installée via **Cortana**.
>
> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec un objectif unique.
>
> Les définitions de commandes vocales peuvent varier en complexité. Ils peuvent prendre en charge n’importe quel caractère à partir d’une seule et même énoncée à une collection d’énoncés de langage naturel plus flexibles, qui dénotent le même objectif.

La modification dynamique d’une liste d’expressions au moment de l’exécution est utile si la commande vocale est spécifique à une tâche impliquant un type de données d’application défini par l’utilisateur ou transitoire.

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Par exemple, imaginons que vous avez une application de voyage dans laquelle les utilisateurs peuvent entrer des destinations, et que vous souhaitez que les utilisateurs puissent démarrer l’application en disant le nom de l’application, suivi de « afficher le voyage à la &lt; destination &gt; ». Dans l’élément **ListenFor** lui-même, vous devez spécifier un élément tel que : `<ListenFor> Show trip to {destination}  </ListenFor>` , où « destination » est la valeur de l’attribut **label** pour **PhraseList**.

La mise à jour de la liste d’expressions au moment de l’exécution élimine la nécessité de créer un élément **ListenFor** distinct pour chaque destination possible. Au lieu de cela, vous pouvez remplir dynamiquement les **PhraseList** avec des destinations spécifiées par l’utilisateur lors de l’entrée dans leurs itinéraires.

Pour plus d’informations sur **PhraseList** et d’autres éléments VCD, consultez les informations de référence sur les [**éléments et les attributs VCD v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) .

> [!TIP]
> **Conditions préalables**
>
> Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.
>
> - [Créer votre première application](/windows/uwp/get-started/your-first-app)
> - Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Instructions relatives à l’expérience utilisateur**
>
> Pour plus d’informations sur l’intégration de votre application avec **Cortana** et les [interactions vocales](speech-interactions.md) , consultez [instructions de conception Cortana](cortana-design-guidelines.md) pour obtenir des conseils utiles sur la conception d’une application vocale utile et attrayante.

## <a name="identify-the-command-and-update-the-phrase-list"></a>Identifier la commande et mettre à jour la liste d’expressions

Voici un exemple de fichier VCD qui définit une **commande** « showTripToDestination » et un **PhraseList** qui définit trois options de destination dans notre application **Adventure Works** Travel. Lorsque l’utilisateur enregistre et supprime des destinations dans l’application, l’application met à jour les options dans le **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

Pour mettre à jour un élément **PhraseList** dans le fichier VCD, récupérez l’élément **commandSet** qui contient la liste d’expressions. Utilisez l’attribut **Name** de cet élément **commandSet** (le **nom** doit être unique dans le fichier VCD) comme clé pour accéder à la propriété [**VoiceCommandManager. InstalledCommandSets**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager) et obtenir la référence [**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) .

Une fois que vous avez identifié le jeu de commandes, récupérez une référence à la liste d’expressions que vous souhaitez modifier et appelez la méthode [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet) ; Utilisez l’attribut **label** de l’élément **PhraseList** et un tableau de chaînes en tant que nouveau contenu de la liste d’expressions.

> [!NOTE]
> Si vous modifiez une liste d’expressions, toute la liste d’expressions est remplacée. Si vous souhaitez insérer de nouveaux éléments dans une liste d’expressions, vous devez spécifier à la fois les éléments existants et les nouveaux éléments dans l’appel à [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet).

Dans cet exemple, nous mettons à jour le **PhraseList** illustré dans l’exemple précédent avec une destination supplémentaire de Phoenix.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>Notes

L’utilisation d’un **PhraseList** pour contraindre la reconnaissance est appropriée pour un ou plusieurs mots relativement petits. Lorsque l’ensemble de mots est trop grand (des centaines de mots, par exemple) ou ne doit pas être limité, utilisez l’élément **PhraseTopic** et un élément **Subject** pour affiner la pertinence des résultats de reconnaissance vocale afin d’améliorer l’évolutivité.

Dans notre exemple, nous avons un **PhraseTopic** avec un **scénario** de « recherche », mieux affiné par un **objet** « City \\ State ».

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Activer une application au premier plan avec les commandes vocales via Cortana](cortana-launch-a-foreground-app-with-voice-commands.md)
- [Activer une application en arrière-plan dans Cortana à l’aide de commandes vocales](cortana-launch-a-background-app-with-voice-commands.md)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
