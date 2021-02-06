---
title: 'Prendre en charge des commandes vocales plus naturelles dans Cortana : conception et développement UWP Cortana'
description: Étendez Cortana avec des commandes vocales plus flexibles et plus naturelles qui permettent à un utilisateur d’indiquer le nom de votre application n’importe où dans la commande.
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 7716d4d623653c6b2d943135f2e2cf1ac9a40343
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99605994"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>Prendre en charge des commandes vocales en langage naturel dans Cortana

>[!WARNING]
> Cette fonctionnalité n’est plus prise en charge à partir de la mise à jour Windows 10 2020 (version 2004, nom de nom « 20H1 »).
>
> Consultez [Cortana dans Microsoft 365](/microsoft-365/admin/misc/cortana-integration) pour savoir comment Cortana transforme les expériences de productivité modernes.

Étendez **Cortana** avec des commandes vocales plus flexibles et plus naturelles qui permettent à un utilisateur d’indiquer le nom de votre application n’importe où dans la commande.

> [!NOTE]
> **API importantes**
>
> - [**Windows. ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**Éléments et attributs d’un fichier VCD v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

L’utilisation de commandes vocales pour étendre Cortana avec les fonctionnalités de votre application requiert que l’utilisateur spécifie l’application et la commande ou la fonction à exécuter. Pour ce faire, vous devez en effet annoncer le nom de l’application au début ou à la fin de la commande vocale. Par exemple, « Adventure Works, ajoutez un nouveau voyage à Las Vegas ».

Toutefois, le fait de spécifier le nom de l’application de cette façon peut paraître maladroit, Stilted ou n’a pas de sens. Dans de nombreux cas, la possibilité d’indiquer le nom de l’application ailleurs dans la commande est plus confortable et naturelle, et contribue à rendre l’interaction beaucoup plus intuitive et attrayante pour l’utilisateur. Notre exemple précédent, « Adventure Works, ajoute un nouveau voyage à Las Vegas ». peut être reformulée comme « ajouter un nouveau voyage Adventure Works à Las Vegas ». ou « à l’aide d’Adventure Works, ajoutez un nouveau voyage à Las Vegas ».

Vous pouvez configurer vos commandes vocales pour prendre en charge le nom de l’application en tant que :

- Préfixe-avant l’expression de commande
- Infixe-au sein de l’expression de commande
- Suffixe-après l’expression de commande

> [!TIP]
> **Conditions préalables**
>
> Cette rubrique s’appuie sur [l’activation d’une application en arrière-plan dans Cortana à l’aide de commandes vocales](cortana-launch-a-background-app-with-voice-commands.md). Nous continuons ici pour démontrer les fonctionnalités d’une application de planification et de gestion de voyage nommée **Adventure Works**.
>
> Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.
>
> - [Créer votre première application](/windows/uwp/get-started/your-first-app)
> - Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](/windows/uwp/xaml-platform/events-and-routed-events-overview).
>
> **Instructions relatives à l’expérience utilisateur**
>
> Pour plus d’informations sur l’intégration de votre application avec **Cortana** et les [interactions vocales](speech-interactions.md) , consultez [instructions de conception Cortana](cortana-design-guidelines.md) pour obtenir des conseils utiles sur la conception d’une application vocale utile et attrayante.

## <a name="specify-an-appname-element-in-the-vcd"></a>Spécifier un élément **appname** dans le VCD

L’élément **appname** est utilisé pour spécifier un nom convivial pour une application dans une commande vocale.

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>Spécifier où le nom de l’application peut être parlé dans la commande vocale

L’élément **ListenFor** a un attribut **RequireAppName** qui spécifie l’emplacement où le nom de l’application peut apparaître dans la commande vocale. Cet attribut prend en charge quatre valeurs.

1. **BeforePhrase**

   Par défaut.

   Indique que les utilisateurs doivent indiquer le nom de votre application avant l’expression de commande.

   Ici, Cortana écoute « Adventure Works quand est mon voyage à Las Vegas ».

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    Indique que les utilisateurs doivent indiquer le nom de votre application après l’expression de commande.

    Une liste d’expressions localisées de conjonctions prépositionnelles est fournie par le système. Cela comprend les expressions telles que, « using », « with » et « on ».

    Dans ce cas, Cortana écoute les commandes telles que « afficher mon prochain voyage à Las Vegas sur Adventure Works » et « montrer mon prochain voyage à Las Vegas avec Adventure Works ».

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    Indique que les utilisateurs doivent indiquer le nom de votre application avant ou après l’expression de commande.

    Pour la version du suffixe, une expression localisée répertoriant les conjonctions prépositionnelles est fournie par le système. Cela comprend les expressions telles que, « using », « with » et « on ».

    Dans ce cas, Cortana écoute les commandes telles que « Adventure Works, afficher mon prochain voyage à Las Vegas » ou « afficher mon prochain voyage à Las Vegas ».

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    Indique que les utilisateurs doivent indiquer le nom de votre application exactement à l’emplacement que vous spécifiez dans l’expression de commande. L’utilisateur n’a pas besoin d’indiquer le nom de l’application avant ou après l’expression.

    Vous devez explicitement référencer le nom de votre application à l’aide de la balise **{BuiltIn : AppName}** .

    Dans ce cas, Cortana écoute les commandes telles que « Adventure Works, afficher mon prochain voyage à Las Vegas » ou « montrer mon prochain voyage Adventure Works à Las Vegas ».

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>Cas particuliers

Lorsque vous déclarez un élément **ListenFor** où **RequireAppName** est « AfterPhrase » ou « ExplicitlySpecified », vous devez vous assurer que certaines conditions sont remplies :

1. **{BuiltIn : AppName}** doit apparaître une fois et une seule une fois quand **RequireAppName** est défini sur « ExplicitlySpecified ».

    Avec cette valeur, le système ne peut pas déduire où le nom de l’application peut apparaître dans la commande vocale. Vous devez spécifier explicitement l’emplacement.

2. Une commande vocale ne peut pas commencer par un élément **PhraseTopic** , qui est généralement utilisé pour la reconnaissance vocale de vocabulaire important. Au moins un mot doit être précédé.

    Cela permet de réduire le risque de lancement de votre application par **Cortana** si une commande contient le nom de votre application, ou une partie de celui-ci, n’importe où dans l’énoncé.

    Voici une déclaration non valide qui peut entraîner le lancement par **Cortana** de l’application **Adventure Works** si l’utilisateur dit « afficher les évaluations pour Kinect Adventure Works ».
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. Il doit y avoir au moins deux mots dans la chaîne **ListenFor** en plus du nom de votre application et des références aux éléments **PhraseTopic** .

    Comme pour le cas 2, vous devez vous assurer que vos commandes contiennent un contenu phonétique suffisant pour réduire les chances que votre application soit lancée de manière involontaire.

    Cela vous aide à configurer votre application pour un succès optimal afin que votre application ne soit pas lancée de manière incorrecte quand l’utilisateur l’indique, par exemple « Rechercher Kinect Adventure Works ».

    Voici des déclarations non valides susceptibles d’entraîner le lancement par **Cortana** de l’application **Adventure Works** si l’utilisateur dit « Hey Adventure Works » ou « Find Kinect Adventure Works ».

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>Remarques

La prise en charge d’une plus grande variation de la façon dont une commande vocale peut être intégrée à un utilisateur dans **Cortana** augmente également la convivialité générale de votre application.

Évitez d’avoir « Hey \[ app name \] » comme **appname**. Il est plus probable que les utilisateurs disent à « Hey Cortana » d’appeler Cortana par le biais de l’activation vocale, et le fait que « Hey \[ app name \] » dans l’énoncé n’est pas naturel. Par exemple, « Bonjour Cortana, montrez mon prochain voyage à Las Vegas sur Hey Adventure Works ».

Envisagez d’ajouter des variantes infix/suffixe à vos commandes vocales existantes. Comme nous l’avons vu ici, il n’est pas nécessaire d’ajouter un attribut supplémentaire à vos éléments **ListenFor** existants et de prendre en charge les variantes de suffixe. Il est beaucoup plus naturel de prononcer « Hey Cortana, montrer mon prochain voyage à Las Vegas sur Adventure Works » que « Hey Cortana, Adventure Works, j’ai le prochain voyage à Las Vegas ».

Envisagez d’utiliser le nom de votre application en tant que préfixe dans les cas où la commande vocale est en conflit avec les fonctionnalités **Cortana** existantes (appel, messagerie, etc.). Par exemple, « Adventure Works, \[ agent de voyage des messages à partir de \] Las Vegas course ».

## <a name="complete-example"></a>Exemple complet

Voici un fichier VCD qui illustre les différentes façons de fournir des commandes vocales en langage naturel supplémentaires.

> [!NOTE]
> Il est possible d’avoir plusieurs éléments **ListenFor** , chacun avec une valeur d’attribut **RequireAppName** différente.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <a name="related-articles"></a>Articles connexes

- [Interactions Cortana dans les applications Windows](cortana-interactions.md)
- [Instructions de conception de Cortana](cortana-design-guidelines.md)
- [Éléments et attributs d’un fichier VCD v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Exemple de commande vocale Cortana](https://go.microsoft.com/fwlink/p/?LinkID=619899)
