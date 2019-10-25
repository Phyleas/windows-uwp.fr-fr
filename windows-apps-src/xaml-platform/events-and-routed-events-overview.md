---
description: Nous décrivons le concept de programmation des événements dans une application Windows Runtime, C#lors de l’utilisation C++ de, d'C++Visual Basic ou d’extensions de composants visuels (/CX) en tant que langage de programmation et XAML pour votre définition d’interface utilisateur.
title: Vue d’ensemble des événements et des événements routés
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690398"
---
# <a name="events-and-routed-events-overview"></a>Vue d’ensemble des événements et des événements routés

**API importantes**
- [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Nous décrivons le concept de programmation des événements dans une application Windows Runtime, C#lors de l’utilisation C++ de, d'C++Visual Basic ou d’extensions de composants visuels (/CX) en tant que langage de programmation et XAML pour votre définition d’interface utilisateur. Vous pouvez assigner des gestionnaires pour les événements dans le cadre des déclarations pour les éléments d’interface utilisateur en XAML, ou vous pouvez ajouter les gestionnaires dans le code. Windows Runtime prend en charge les *événements routés*: certains événements d’entrée et événements de données peuvent être gérés par des objets au-delà de l’objet qui a déclenché l’événement. Les événements routés sont utiles lorsque vous définissez des modèles de contrôle, ou utilisez des pages ou des conteneurs de disposition.

## <a name="events-as-a-programming-concept"></a>Événements en tant que concept de programmation

En général, les concepts d’événement lors de la programmation d’une application Windows Runtime sont similaires au modèle d’événement dans les langages de programmation les plus populaires. Si vous savez comment utiliser des Microsoft .NET ou C++ des événements déjà, vous avez un début de tête. Mais vous n’avez pas besoin de connaître les concepts du modèle d’événement pour effectuer certaines tâches de base, telles que l’attachement de gestionnaires.

Quand vous utilisez C#, Visual Basic ou C++/CX comme langage de programmation, l’interface utilisateur est définie dans le balisage (XAML). Dans la syntaxe de balisage XAML, certains des principes de connexion des événements entre les éléments de balisage et les entités de code d’exécution sont similaires à d’autres technologies Web, telles que ASP.NET ou HTML5.

**Notez**  le code qui fournit la logique d’exécution pour une interface utilisateur définie en XAML est souvent appelé *code-behind* ou fichier code-behind. Dans les vues de solution Microsoft Visual Studio, cette relation est représentée graphiquement, avec le fichier code-behind qui est un fichier dépendant et imbriqué par rapport à la page XAML à laquelle il fait référence.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Bouton. Click : Introduction aux événements et XAML

L’une des tâches de programmation les plus courantes pour une Windows Runtime application consiste à capturer l’entrée d’utilisateur dans l’interface utilisateur. Par exemple, votre interface utilisateur peut avoir un bouton sur lequel l’utilisateur doit cliquer pour envoyer des informations ou modifier l’État.

Vous définissez l’interface utilisateur de votre application Windows Runtime en générant du code XAML. Ce code XAML est généralement la sortie d’une aire de conception dans Visual Studio. Vous pouvez également écrire le code XAML dans un éditeur de texte brut ou un éditeur XAML tiers. Lors de la génération de ce XAML, vous pouvez connecter des gestionnaires d’événements pour des éléments d’interface utilisateur en même temps que vous définissez tous les autres attributs XAML qui établissent des valeurs de propriété de cet élément d’interface utilisateur.

Pour connecter les événements en XAML, vous spécifiez le nom de forme de chaîne de la méthode de gestionnaire que vous avez déjà définie ou que vous définirez ultérieurement dans votre code-behind. Par exemple, ce code XAML définit un objet [**bouton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) avec d’autres propriétés ([attribut x :Name](x-name-attribute.md), [**contenu**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) assignées en tant qu’attributs, et relie un gestionnaire pour l’événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) du bouton en référençant une méthode nommée `ShowUpdatesButton_Click`:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Conseil**l'  *événement câblage* est un terme de programmation. Il fait référence au processus ou au code dans lequel vous indiquez que les occurrences d’un événement doivent appeler une méthode de gestionnaire nommée. Dans la plupart des modèles de code procédural, la connexion d’événements est un code « AddHandler » implicite ou explicite qui nomme à la fois l’événement et la méthode, et implique généralement une instance d’objet cible. En XAML, le « AddHandler » est implicite et la connexion des événements consiste entièrement à nommer l’événement comme nom d’attribut d’un élément objet et à nommer le gestionnaire comme valeur de cet attribut.

Vous écrivez le gestionnaire réel dans le langage de programmation que vous utilisez pour l’ensemble du code et du code-behind de votre application. Avec l’attribut `Click="ShowUpdatesButton_Click"`, vous avez créé un contrat qui, lorsque le code XAML est compilé et analysé, l’étape de compilation du balisage XAML dans l’action de génération de votre IDE et l’analyse XAML éventuelle lors du chargement de l’application peuvent trouver une méthode nommée `ShowUpdatesButton_Click` dans le cadre du co de l’application. annulation. `ShowUpdatesButton_Click` doit être une méthode qui implémente une signature de méthode compatible (basée sur un délégué) pour n’importe quel gestionnaire de l’événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) . Par exemple, ce code définit le gestionnaire de `ShowUpdatesButton_Click`.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

Dans cet exemple, la méthode `ShowUpdatesButton_Click` est basée sur le délégué [**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler) . Vous savez qu’il s’agit du délégué à utiliser, car vous verrez ce délégué nommé dans la syntaxe de la méthode [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) .

**Conseil**  Visual Studio offre un moyen pratique de nommer le gestionnaire d’événements et de définir la méthode de gestionnaire lors de la modification du code XAML. Quand vous fournissez le nom d’attribut de l’événement dans l’éditeur de texte XAML, attendez un moment jusqu’à ce qu’une liste Microsoft IntelliSense s’affiche. Si vous cliquez sur **&lt;&gt;de gestionnaire d’événements** dans la liste, Microsoft Visual Studio suggérera un nom de méthode basé sur le **x :Name** de l’élément (ou le nom de type), le nom de l’événement et un suffixe numérique. Vous pouvez ensuite cliquer avec le bouton droit sur le nom du gestionnaire d’événements sélectionné, puis cliquer sur **naviguer vers le gestionnaire d’événements**. Cela permet d’accéder directement à la définition du gestionnaire d’événements qui vient d’être inséré, comme indiqué dans la vue de l’éditeur de code de votre fichier code-behind pour la page XAML. Le gestionnaire d’événements a déjà la signature correcte, y compris le paramètre *sender* et la classe de données d’événement que l’événement utilise. En outre, si une méthode de gestionnaire avec la signature correcte existe déjà dans votre code-behind, le nom de cette méthode apparaît dans la liste déroulante de saisie semi-automatique, ainsi que l’option **&lt;nouveau gestionnaire d’événements&gt;** . Vous pouvez également appuyer sur la touche Tab en tant que raccourci au lieu de cliquer sur les éléments de la liste IntelliSense.

## <a name="defining-an-event-handler"></a>Définition d’un gestionnaire d’événements

Pour les objets qui sont des éléments d’interface utilisateur et déclarés en XAML, le code de gestionnaire d’événements est défini dans la classe partielle qui sert de code-behind pour une page XAML. Les gestionnaires d’événements sont des méthodes que vous écrivez dans le cadre de la classe partielle associée à votre XAML. Ces gestionnaires d’événements sont basés sur les délégués utilisés par un événement particulier. Vos méthodes de gestionnaire d’événements peuvent être publiques ou privées. L’accès privé fonctionne parce que le gestionnaire et l’instance créés par le XAML sont finalement joints par la génération de code. En général, nous vous recommandons de rendre les méthodes de gestionnaire d’événements privées dans la classe.

**Notez**  gestionnaires d’événements pour C++ ne pas être définis dans des classes partielles, ils sont déclarés dans l’en-tête en tant que membre de classe privée. Les actions de génération d' C++ un projet s’occupent de la génération de code qui prend en charge le système de type C++XAML et le modèle de code-behind pour.

### <a name="the-sender-parameter-and-event-data"></a>Paramètres et données d’événement de l' *expéditeur*

Le gestionnaire que vous écrivez pour l’événement peut accéder à deux valeurs qui sont disponibles comme entrée pour chaque cas où votre gestionnaire est appelé. La première valeur de ce type est *sender*, qui est une référence à l’objet auquel le gestionnaire est attaché. Le paramètre *sender* est typé en tant que type d' **objet** de base. Une technique courante consiste à convertir l' *expéditeur* en un type plus précis. Cette technique est utile si vous envisagez de vérifier ou de modifier l’état de l’objet d' *expéditeur* lui-même. En fonction de la conception de votre propre application, vous connaissez généralement un type qui peut effectuer un cast de l' *expéditeur* en toute sécurité, en fonction de l’emplacement auquel le gestionnaire est attaché ou d’autres caractéristiques de conception.

La deuxième valeur est Data Events, qui apparaît généralement dans les définitions de syntaxe comme paramètre *e* . Vous pouvez découvrir les propriétés des données d’événement qui sont disponibles en examinant le paramètre *e* du délégué affecté à l’événement spécifique que vous gérez, puis en utilisant IntelliSense ou l’Explorateur d’objets dans Visual Studio. Vous pouvez utiliser la documentation de référence Windows Runtime.

Pour certains événements, les valeurs de propriété spécifiques aux données d’événement sont aussi importantes que la connaissance de l’événement. Cela est particulièrement vrai pour les événements d’entrée. Pour les événements de pointeur, la position du pointeur lorsque l’événement s’est produit peut être importante. Pour les événements de clavier, toutes les activations de touches possibles déclenchent un événement [**Keyverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) . Pour déterminer la clé sur laquelle un utilisateur a appuyé, vous devez accéder au [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) qui est disponible pour le gestionnaire d’événements. Pour plus d’informations sur la gestion des événements d’entrée, consultez [interactions du clavier](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) et gérer l' [entrée du pointeur](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input). Les événements d’entrée et les scénarios d’entrée ont souvent des considérations supplémentaires qui ne sont pas abordées dans cette rubrique, telles que la capture de pointeur pour les événements de pointeur, et les touches de modification et les codes de clé de plateforme pour les événements de clavier.

### <a name="event-handlers-that-use-the-async-pattern"></a>Gestionnaires d’événements qui utilisent le modèle **asynchrone**

Dans certains cas, vous souhaiterez utiliser des API qui utilisent un modèle **asynchrone** dans un gestionnaire d’événements. Par exemple, vous pouvez utiliser un [**bouton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) dans un [**appbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) pour afficher un sélecteur de fichier et interagir avec lui. Toutefois, la plupart des API de sélecteur de fichiers sont asynchrones. Elles doivent être appelées dans une portée/awaitable **asynchrone**, et le compilateur applique cela. Par conséquent, vous pouvez ajouter le mot clé **Async** à votre gestionnaire d’événements afin que le gestionnaire soit maintenant **Async** **void**. Désormais, votre gestionnaire d’événements est autorisé à effectuer des appels/awaitable **asynchrones**.

Pour obtenir un exemple de gestion des événements d’interaction avec l’utilisateur à l’aide du modèle **Async** , consultez [accès aux fichiers et sélecteurs](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10)) (qui fait partie de la série[créer votre première Windows Runtime application à l’aide C# de ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10)) ). Voir aussi [appeler des API asynchrones en C].

## <a name="adding-event-handlers-in-code"></a>Ajout de gestionnaires d’événements dans le code

XAML n’est pas la seule manière d’assigner un gestionnaire d’événements à un objet. Pour ajouter des gestionnaires d’événements à un objet donné dans le code, y compris aux objets qui ne sont pas utilisables en XAML, vous pouvez utiliser la syntaxe spécifique au langage pour ajouter des gestionnaires d’événements.

Dans C#, la syntaxe consiste à utiliser l’opérateur`+=`. Vous inscrivez le gestionnaire en référençant le nom de la méthode de gestionnaire d’événements sur le côté droit de l’opérateur.

Si vous utilisez du code pour ajouter des gestionnaires d’événements aux objets qui apparaissent dans l’interface utilisateur d’exécution, il est courant d’ajouter ces gestionnaires en réponse à un événement ou un rappel de durée de vie d’objet, tel que [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) ou [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), afin que les gestionnaires d’événements sur le l’objet approprié est prêt pour les événements initiés par l’utilisateur au moment de l’exécution. Cet exemple montre un plan XAML de la structure de la page, puis C# fournit la syntaxe du langage pour l’ajout d’un gestionnaire d’événements à un objet.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Notez**  une syntaxe plus détaillée existe. Dans 2005, C# ajoutait une fonctionnalité appelée inférence de délégué, qui permet à un compilateur de déduire la nouvelle instance de délégué et d’activer la syntaxe précédente et plus simple. La syntaxe détaillée est fonctionnellement identique à l’exemple précédent, mais crée explicitement une instance de délégué avant de l’inscrire, ce qui ne tire pas parti de l’inférence de délégué. Cette syntaxe explicite est moins courante, mais vous pouvez toujours la voir dans certains exemples de code.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Il existe deux possibilités pour Visual Basic syntaxe. L’une consiste à effectuer C# une liaison parallèle de la syntaxe et à joindre des gestionnaires directement aux instances. Cela nécessite le mot clé **AddHandler** et également l’opérateur **AddressOf** qui déréférencent le nom de la méthode de gestionnaire.

L’autre option pour Visual Basic syntaxe consiste à utiliser le mot clé **Handles** sur les gestionnaires d’événements. Cette technique est appropriée pour les cas où les gestionnaires sont censés exister sur les objets au moment du chargement et persistent tout au long de la durée de vie de l’objet. L’utilisation de **Handles** sur un objet défini en XAML requiert que vous fournissiez un **nom** / **x :Name**. Ce nom devient le qualificateur d’instance requis pour la partie *instance. Event* de la syntaxe **Handles** . Dans ce cas, vous n’avez pas besoin d’un gestionnaire d’événements basé sur la durée de vie des objets pour initialiser l’attachement des autres gestionnaires d’événements. les connexions de **Handles** sont créées lorsque vous compilez votre page XAML.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Notez**  Visual Studio et son aire de conception XAML favorisent généralement la technique de gestion d’instance au lieu du mot clé **Handles** . Cela est dû au fait que l’établissement de la connexion du gestionnaire d’événements en XAML fait partie du flux de travail concepteur-développeur standard, et que la technique des mots clés **Handles** est incompatible avec la connexion des gestionnaires d’événements en XAML.

Dans C++/CX, vous utilisez également la syntaxe **+=** , mais il existe des différences par rapport C# au format de base :

- Aucune inférence de délégué n’existe. vous devez donc utiliser **ref New** pour l’instance de délégué.
- Le constructeur délégué a deux paramètres et requiert que l’objet cible soit le premier paramètre. En général, vous spécifiez **ce**.
- Le constructeur délégué exige que l’adresse de la méthode soit le second paramètre, de sorte que l’opérateur de référence **&** précède le nom de la méthode.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Suppression de gestionnaires d’événements dans le code

Il n’est généralement pas nécessaire de supprimer les gestionnaires d’événements dans le code, même si vous les avez ajoutés dans le code. Le comportement de la durée de vie des objets pour la plupart des objets Windows Runtime tels que les pages et les contrôles détruira les objets lorsqu’ils seront déconnectés de la [**fenêtre**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) principale et de son arborescence d’éléments visuels, et que toutes les références de délégué sont également détruites. .NET effectue cette fonction via garbage collection et Windows Runtime C++avec/CX utilise des références faibles par défaut.

Dans certains cas rares, il est préférable de supprimer les gestionnaires d’événements de manière explicite. Ces outils sont les suivants :

- Gestionnaires que vous avez ajoutés pour les événements statiques, qui ne peuvent pas obtenir de garbage collection de façon conventionnelle. Les événements des classes [**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) et [**Clipboard**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) sont des exemples d’événements statiques dans l’API Windows Runtime.
- Code de test dans lequel vous souhaitez que le minutage de la suppression du gestionnaire soit immédiat, ou du code dans lequel vous allez échanger des gestionnaires d’événements anciens/nouveaux pour un événement au moment de l’exécution.
- Implémentation d’un accesseur **Remove** personnalisé.
- Événements statiques personnalisés.
- Gestionnaires de navigation entre les pages.

[**FrameworkElement. uncharged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded) ou [**page. NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) sont des déclencheurs d’événements possibles qui ont des positions appropriées dans la gestion d’État et la durée de vie des objets, ce qui vous permet de les utiliser pour supprimer des gestionnaires pour d’autres événements.

Par exemple, vous pouvez supprimer un gestionnaire d’événements nommé **textBlock1\_PointerEntered** de l’objet cible **textBlock1** à l’aide de ce code.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Vous pouvez également supprimer des gestionnaires pour les cas où l’événement a été ajouté par le biais d’un attribut XAML, ce qui signifie que le gestionnaire a été ajouté dans le code généré. Cela est plus facile si vous avez fourni une valeur de **nom** pour l’élément dans lequel le gestionnaire a été attaché, car cela fournit une référence d’objet pour le code ultérieurement ; Toutefois, vous pouvez également parcourir l’arborescence des objets afin de trouver la référence d’objet nécessaire dans les cas où l’objet n’a pas de **nom**.

Si vous avez besoin de supprimer un gestionnaire d' C++événements dans/CX, vous avez besoin d’un jeton d’inscription, que vous devez avoir reçu de la valeur de retour de l’inscription du gestionnaire d’événements`+=`. Cela est dû au fait que la valeur que vous utilisez pour le côté droit de la `-=` C++la désinscription dans la syntaxe/CX est le jeton, et non le nom de la méthode. Pour C++/CX, vous ne pouvez pas supprimer les gestionnaires qui ont été ajoutés en tant qu' C++attribut XAML, car le code généré par/CX n’enregistre pas de jeton.

## <a name="routed-events"></a>Événements routés

Le Windows Runtime avec C#, Microsoft Visual Basic ou C++/CX prend en charge le concept d’événement routé pour un ensemble d’événements qui sont présents sur la plupart des éléments d’interface utilisateur. Ces événements sont destinés aux scénarios d’entrée et d’interaction utilisateur, et ils sont implémentés sur la classe de base [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) . Voici une liste d’événements d’entrée qui sont des événements routés :

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Déplacez**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**Vient**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Détenteur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**Événementiel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Drainées**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Un événement routé est un événement qui est potentiellement passé (*routé*) d’un objet enfant à chacun de ses objets parents successifs dans une arborescence d’objets. La structure XAML de votre interface utilisateur est proche de cette arborescence, avec la racine de cette arborescence qui est l’élément racine en XAML. L’arborescence d’objets réelle peut varier quelque peu de l’imbrication des éléments XAML, car l’arborescence d’objets n’inclut pas les fonctionnalités de langage XAML telles que les balises d’éléments de propriété. Vous pouvez concevoir l’événement routé comme une *propagation* à partir de n’importe quel élément enfant d’élément objet XAML qui déclenche l’événement, vers l’élément objet parent qui le contient. L’événement et ses données d’événement peuvent être gérés sur plusieurs objets le long de l’itinéraire d’événement. Si aucun élément n’a de gestionnaires, l’itinéraire continue de se poursuivre jusqu’à ce que l’élément racine soit atteint.

Si vous connaissez des technologies Web telles que DHTML (Dynamic HTML) ou HTML5, vous connaissez peut-être déjà le concept d’événement de *propagation* .

Lorsqu’un événement routé est propagé via son itinéraire d’événement, tous les gestionnaires d’événements attachés accèdent tous à une instance partagée de données d’événement. Par conséquent, si une des données d’événement est accessible en écriture par un gestionnaire, toutes les modifications apportées aux données d’événement sont transmises au gestionnaire suivant et ne peuvent plus représenter les données d’événement d’origine de l’événement. Lorsqu’un événement a un comportement d’événement routé, la documentation de référence inclut des remarques ou d’autres notations relatives au comportement routé.

### <a name="the-originalsource-property-of-routedeventargs"></a>Propriété **OriginalSource** de **RoutedEventArgs**

Lorsqu’un événement propage un itinéraire d’événement, l' *expéditeur* n’est plus le même objet que l’objet de déclenchement d’événements. À la place, *sender* est l’objet dans lequel le gestionnaire appelé est attaché.

Dans certains cas, l' *expéditeur* n’est pas intéressant et vous intéresse à la place des informations telles que les objets enfants possibles du pointeur lorsqu’un événement pointeur se déclenche, ou l’objet d’une interface utilisateur plus grande qui a le focus lorsqu’un utilisateur a appuyé sur une touche du clavier. Dans ce cas, vous pouvez utiliser la valeur de la propriété [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) . À tous les points de l’itinéraire, **OriginalSource** signale l’objet d’origine qui a déclenché l’événement, au lieu de l’objet où le gestionnaire est attaché. Toutefois, pour les événements d’entrée [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) , cet objet d’origine est souvent un objet qui n’est pas immédiatement visible dans le XAML de définition d’interface utilisateur au niveau de la page. Au lieu de cela, cet objet source d’origine peut être une partie modèle d’un contrôle. Par exemple, si l’utilisateur place le pointeur sur le bord le plus élevé d’un [**bouton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), pour la plupart des événements de pointeur, **OriginalSource** est une partie de modèle de [**bordure**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) dans le [**modèle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template), et non pas le **bouton** lui-même.

**Conseil**  la propagation des événements d’entrée est particulièrement utile si vous créez un contrôle basé sur un modèle. Tout contrôle ayant un modèle peut avoir un nouveau modèle appliqué par son consommateur. Le consommateur qui tente de recréer un modèle de travail peut supprimer intentionnellement une gestion des événements déclarée dans le modèle par défaut. Vous pouvez toujours fournir la gestion des événements au niveau du contrôle en attachant des gestionnaires dans le cadre de la substitution [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) dans la définition de classe. Vous pouvez ensuite intercepter les événements d’entrée qui se propagent jusqu’à la racine du contrôle lors de l’instanciation.

### <a name="the-handled-property"></a>Propriété **gérée**

Plusieurs classes de données d’événement pour des événements routés spécifiques contiennent une propriété nommée **Handled**. Pour obtenir des exemples, consultez [**PointerRoutedEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**DragEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled). Dans tous les cas, **géré** est une propriété booléenne définissable.

La définition de la propriété **Handled** sur **true** influence le comportement du système d’événements. Quand **géré** a la **valeur true**, le routage s’arrête pour la plupart des gestionnaires d’événements ; l’événement ne continue pas sur l’itinéraire pour notifier à d’autres gestionnaires attachés de ce cas d’événement particulier. Ce que signifie « géré » dans le contexte de l’événement et la manière dont votre application répond à celle-ci dépend de vous. Fondamentalement, **Handled** est un protocole simple qui permet au code de l’application d’indiquer qu’une occurrence d’un événement n’a pas besoin de se propager à des conteneurs, votre logique d’application a pris en charge ce qui a été fait. À l’inverse, vous devez veiller à ne pas gérer les événements susceptibles de se propager afin que les comportements système ou de contrôle intégrés puissent agir. Par exemple, la gestion des événements de bas niveau dans les parties ou les éléments d’un contrôle de sélection peut être nuisible. Le contrôle de sélection peut rechercher des événements d’entrée pour savoir que la sélection doit changer.

Tous les événements routés ne peuvent pas annuler un itinéraire de cette façon et vous pouvez le dire car ils n’ont pas de propriété **gérée** . Par exemple, [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) et [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) do se propagent, mais ils se propagent toujours jusqu’à la racine, et leurs classes de données d’événement n’ont pas de propriété **gérée** qui peut influencer ce comportement.

##  <a name="input-event-handlers-in-controls"></a>Gestionnaires d’événements d’entrée dans les contrôles

Les contrôles Windows Runtime spécifiques utilisent parfois le concept **géré** pour les événements d’entrée en interne. Cela peut sembler qu’un événement d’entrée ne se produise jamais, car votre code utilisateur ne peut pas le gérer. Par exemple, la classe [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) comprend une logique qui gère délibérément l’événement d’entrée général [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed). Pour ce faire, les boutons déclenchent un événement [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) qui est initié par une entrée avec pointeur enfoncé, ainsi que par d’autres modes d’entrée tels que la gestion des clés comme la touche entrée qui peut appeler le bouton quand elle est axée. Dans le cadre de la conception de la classe du **bouton**, l’événement d’entrée brut est géré de manière conceptuelle, et les consommateurs de classe tels que votre code utilisateur peuvent interagir à la place avec l’événement **Click** approprié au contrôle. Les rubriques relatives aux classes de contrôle spécifiques dans le Windows Runtime référence de l’API remarquent souvent le comportement de gestion des événements que la classe implémente. Dans certains cas, vous pouvez modifier le comportement en remplaçant sur les méthodes **d'** _événement_ . Par exemple, vous pouvez modifier la façon dont votre classe dérivée de [**zone de texte**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) réagit à l’entrée de clé en substituant [**Control. OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Inscription de gestionnaires pour les événements routés déjà gérés

Nous avons dit précédemment que le paramètre **Handled** to **true** empêche l’appel de la plupart des gestionnaires. Toutefois, la méthode [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) fournit une technique dans laquelle vous pouvez attacher un gestionnaire qui est toujours appelé pour l’itinéraire, même si un autre gestionnaire plus haut dans l’itinéraire a défini la valeur de **Handled** sur **true** dans les données d’événement partagées. Cette technique est utile si un contrôle que vous utilisez a traité l’événement dans sa composition interne ou pour la logique propre au contrôle. Toutefois, vous souhaitez toujours y répondre à partir d’une instance de contrôle ou de l’interface utilisateur de votre application. Toutefois, utilisez cette technique avec précaution, car elle peut contredire l’objectif de **gérée** et, éventuellement, de rompre les interactions prévues d’un contrôle.

Seuls les événements routés qui ont un identificateur d’événement routé correspondant peuvent utiliser la technique de gestion des événements [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) , car l’identificateur est une entrée obligatoire de la méthode **AddHandler** . Consultez la documentation de référence pour [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) pour obtenir la liste des événements qui ont des identificateurs d’événements routés disponibles. Pour l’essentiel, il s’agit de la même liste d’événements routés que nous vous avons montrés précédemment. L’exception est que les deux derniers de la liste : [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) et [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) n’ont pas d’identificateur d’événement routé. vous ne pouvez donc pas utiliser **AddHandler** pour ceux-ci.

## <a name="routed-events-outside-the-visual-tree"></a>Événements routés en dehors de l’arborescence d’éléments visuels

Certains objets participent à une relation avec l’arborescence d’éléments visuels principale de manière conceptuelle, à l’instar d’une superposition sur les visuels principaux. Ces objets ne font pas partie des relations parent-enfant habituelles qui connectent tous les éléments d’arborescence à la racine visuelle. C’est le cas pour toutes les [**fenêtres contextuelles**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) ou [**info-bulles**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)affichées. Si vous souhaitez gérer des événements routés à partir d’une **fenêtre contextuelle** ou d’une **info-bulle**, placez les gestionnaires sur des éléments d’interface utilisateur spécifiques qui se trouvent dans la **fenêtre contextuelle** ou l' **info-** bulle, et non dans les éléments **Popup** ou **info-bulle eux-** mêmes. Ne vous fiez pas au routage dans une composition qui est effectuée pour le contenu de la **fenêtre contextuelle** ou de l' **info-bulle** . Cela est dû au fait que le routage d’événements pour les événements routés fonctionne uniquement le long de l’arborescence d’éléments visuels principale. Une **fenêtre contextuelle** ou une **info-bulle** n’est pas considérée comme un parent d’éléments d’interface utilisateur auxiliaires et ne reçoit jamais l’événement routé, même s’il essaie d’utiliser un élément comme l’arrière-plan par défaut de la **fenêtre contextuelle** comme zone de capture pour les événements d’entrée.

## <a name="hit-testing-and-input-events"></a>Test d’atteinte et événements d’entrée

Le fait de déterminer si et où dans l’interface utilisateur un élément est visible par l’entrée de souris, tactile et de stylet est appelé « *test de positionnement*». Pour les actions tactiles, ainsi que pour les événements spécifiques à l’interaction ou les événements de manipulation qui ont des conséquences sur une action tactile, un élément doit faire l’objet d’un test d’atteinte afin d’être la source de l’événement et déclencher l’événement associé à l’action. Sinon, l’action passe par l’élément à tous les éléments sous-jacents ou éléments parents dans l’arborescence d’éléments visuels qui peuvent interagir avec cette entrée. Il existe plusieurs facteurs qui affectent le test de positionnement, mais vous pouvez déterminer si un élément donné peut déclencher des événements d’entrée en vérifiant sa propriété [**IsHitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible) . Cette propriété retourne **true** uniquement si l’élément répond à ces critères :

- La valeur de la propriété [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) de l’élément est [**visible**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).
- La valeur de la propriété de **remplissage** ou d' **arrière-plan** de l’élément n’est pas **null**. Une valeur de [**pinceau**](/uwp/api/Windows.UI.Xaml.Media.Brush) **null** entraîne la transparence et l’invisibilité du test de positionnement. (Pour rendre un élément transparent, mais également testé, utilisez un pinceau [**transparent**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent) au lieu de **null**.)

**Notez**que l'  **arrière-plan** et le **remplissage** ne sont pas définis par [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)et qu’ils sont définis par différentes classes dérivées telles que [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) et [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). Toutefois, les implications des pinceaux que vous utilisez pour les propriétés de premier plan et d’arrière-plan sont les mêmes pour les tests de positionnement et les événements d’entrée, quelle que soit la sous-classe qui implémente les propriétés.

- Si l’élément est un contrôle, sa valeur de propriété [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) doit avoir la valeur **true**.
- L’élément doit avoir des dimensions réelles dans la disposition. Un élément où [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) et [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) sont 0 ne déclenche pas d’événements d’entrée.

Certains contrôles ont des règles spéciales pour le test de positionnement. Par exemple, [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) n’a aucune propriété d' **arrière-plan** , mais il continue d’effectuer un test de positionnement dans la région entière de ses dimensions. Les contrôles [**image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) et [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) sont testés par test sur leurs dimensions rectangle définies, quel que soit le contenu transparent, tel que le canal alpha dans le fichier source du média affiché. Les contrôles [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) ont un comportement de test de positionnement spécial, car l’entrée peut être gérée par le code html hébergé et déclencher des événements de script.

La plupart des classes et des [**bordures**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) de [**volets**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel) ne sont pas testés par correspondance, mais elles peuvent toujours gérer les événements d’entrée utilisateur qui sont routés à partir des éléments qu’ils contiennent.

Vous pouvez déterminer les éléments qui se trouvent à la même position qu’un événement d’entrée d’utilisateur, que les éléments soient testés ou non. Pour ce faire, appelez la méthode [**FindElementsInHostCoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) . Comme son nom l’indique, cette méthode recherche les éléments à un emplacement relatif à un élément hôte spécifié. Toutefois, les transformations appliquées et les modifications de disposition peuvent ajuster le système de coordonnées relatif d’un élément et, par conséquent, affecter les éléments qui se trouvent à un emplacement donné.

## <a name="commanding"></a>Commandes

Un petit nombre d’éléments d’interface utilisateur prennent en charge les *commandes*. L’exécution de commandes utilise des événements routés liés à l’entrée dans son implémentation sous-jacente et permet le traitement de l’entrée d’interface utilisateur associée (une certaine action de pointeur, une touche d’accès rapide spécifique) en appelant un gestionnaire de commandes unique. Si la commande est disponible pour un élément d’interface utilisateur, envisagez d’utiliser ses API de commande au lieu de n’importe quel événement d’entrée discret. En général, vous utilisez une référence de **liaison** dans les propriétés d’une classe qui définit le modèle de vue des données. Les propriétés contiennent des commandes nommées qui implémentent le modèle de commande **ICommand** propre au langage. Pour plus d’informations, consultez [**ButtonBase. Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Événements personnalisés dans le Windows Runtime

Dans le cadre de la définition d’événements personnalisés, la façon dont vous ajoutez l’événement et ce que cela signifie pour votre conception de classe dépend fortement du langage de programmation que vous utilisez.

- Pour C# et Visual Basic, vous définissez un événement CLR. Vous pouvez utiliser le modèle d’événement .NET standard, tant que vous n’utilisez pas d’accesseurs personnalisés (**ajouter**/**supprimer**). Conseils supplémentaires :
    - Pour le gestionnaire d’événements, il est judicieux d’utiliser [**System. EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1) , car il intègre une traduction vers le Windows Runtime [ **<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)de délégué d’événements générique.
    - Ne basez pas votre classe de données d’événement sur [**System. EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs) , car elle ne se traduit pas par le Windows Runtime. Utilisez une classe de données d’événement existante ou aucune classe de base.
    - Si vous utilisez des accesseurs personnalisés, consultez [événements personnalisés et accesseurs d’événement dans Windows Runtime composants](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Si vous n’êtes pas certain du modèle d’événement .NET standard, consultez [définition d’événements pour les classes Silverlight personnalisées](https://docs.microsoft.com/previous-versions/windows/). Ceci est écrit pour Microsoft Silverlight, mais il s’agit toujours d’une bonne somme du code et des concepts du modèle d’événement .NET standard.
- Pour C++/CX, consultez [événements (C++/CX)](https://docs.microsoft.com/cpp/cppcx/events-c-cx).
    - Utilisez des références nommées même pour vos propres utilisations d’événements personnalisés. N’utilisez pas de lambda pour les événements personnalisés. il peut créer une référence circulaire.

Vous ne pouvez pas déclarer un événement routé personnalisé pour Windows Runtime ; les événements routés sont limités à l’ensemble qui provient de la Windows Runtime.

La définition d’un événement personnalisé s’effectue généralement dans le cadre de l’exercice de définition d’un contrôle personnalisé. Il s’agit d’un modèle courant pour avoir une propriété de dépendance qui a un rappel modifié par la propriété, et pour définir également un événement personnalisé déclenché par le rappel de propriété de dépendance dans certains ou dans tous les cas. Les consommateurs de votre contrôle n’ont pas accès au rappel de modification de propriété que vous avez défini, mais un événement de notification est disponible. Pour plus d’informations, consultez [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Démarrage rapide : entrée tactile](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [Interactions du clavier](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [Événements et délégués .NET](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [Création de composants Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
