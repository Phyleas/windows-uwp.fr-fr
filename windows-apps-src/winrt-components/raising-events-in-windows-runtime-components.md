---
title: Déclenchement d’événements dans les composants Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Comment déclencher un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan afin que JavaScript puisse recevoir l’événement.
ms.date: 07/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5b5678ad1a0666e6f008a2ec69ba63c35441edf
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493514"
---
# <a name="raising-events-in-windows-runtime-components"></a>Déclenchement d’événements dans les composants Windows Runtime

> [!NOTE]
> Pour plus d’informations sur le déclenchement d’événements dans un composant [c++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Windows Runtime, consultez [créer des événements en c++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events).

Si votre composant Windows Runtime déclenche un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan (thread de travail) et que vous souhaitez que JavaScript puisse recevoir l’événement, vous pouvez l’implémenter et/ou le déclencher de l’une des manières suivantes.

-   (Option 1) Déclenchez l’événement via [**Windows. UI. Core. CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) pour marshaler l’événement vers le contexte de thread JavaScript. Bien qu’il s’agisse en général de la meilleure option, elle peut dans certains cas ne pas fournir des performances optimales.
-   (Option 2) Utilisez ** [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler-1) \<Object\> ** (mais perdez les informations sur le type d’événement). Si l’option 1 n’est pas possible, ou si ses performances ne sont pas adéquates, il s’agit d’un bon second choix, à condition que la perte d’informations de type soit acceptable. Si vous créez un composant de Windows Runtime C#, le type **Windows. Foundation. EventHandler \<Object\> ** n’est pas disponible ; à la place, ce type est projeté dans [**System. EventHandler**](/dotnet/api/system.eventhandler). vous devez donc l’utiliser à la place.
-   (Option nº 3) Créer vos propres proxy/stub pour le composant. Cette option est la plus difficile à implémenter, mais elle conserve les informations sur le type et peut offrir de meilleures performances que l’option nº 1 dans les scénarios exigeants.

Si vous déclenchez un événement sur un thread d’arrière-plan sans utiliser l’une de ces options, un client JavaScript ne recevra pas l’événement.

## <a name="background"></a>Arrière-plan

Tous les composants et applications Windows Runtime sont essentiellement des objets COM, quel que soit le langage utilisé pour les créer. Dans l’API Windows, la plupart des composants sont des objets COM agiles qui peuvent communiquer aussi bien avec les objets sur le thread d’arrière-plan qu’avec les objets sur le thread d’interface utilisateur. Si un objet COM ne peut pas être rendu agile, il requiert des objets d’assistance appelés proxys et stubs pour communiquer avec d’autres objets COM à travers la limite thread d’interface utilisateur-thread d’arrière-plan. (En termes COM, on parle de communication entre cloisonnements de threads.)

La plupart des objets de l’API Windows sont agiles ou ont des proxys et des stubs incorporés. Toutefois, les proxys et les stubs ne peuvent pas être créés pour les types génériques comme Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](/uwp/api/windows.foundation.typedeventhandler), car ces types ne sont pas complets tant que vous n’avez pas fourni l’argument de type. L’absence de proxys ou de stubs pose problème uniquement avec les clients JavaScript, mais si vous souhaitez que votre composant soit utilisable à partir de JavaScript, mais aussi de C++ ou d’un langage .NET, vous devez utiliser l’une des trois options suivantes.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Option nº 1) Déclencher l’événement via CoreDispatcher

Vous pouvez envoyer des événements de n’importe quel type délégué défini par l’utilisateur à l’aide de [Windows.UI.Core.CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher), et JavaScript sera en mesure de les recevoir. Si vous n’êtes pas certain de l’option à utiliser, essayez celle-ci en premier. Si la latence entre le déclenchement des événements et la gestion des événements devient un problème, essayez l’une des autres options.

L’exemple suivant montre comment utiliser CoreDispatcher pour déclencher un événement fortement typé. Notez que l’argument de type est Toast, et non Object.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(Option nº 2) Utiliser EventHandler&lt;Object&gt; mais perdre les informations sur le type

> [!NOTE]
> Si vous créez un composant de Windows Runtime C#, le type **Windows. Foundation. EventHandler \<Object\> ** n’est pas disponible ; à la place, ce type est projeté dans [**System. EventHandler**](/dotnet/api/system.eventhandler). vous devez donc l’utiliser à la place.

Une autre façon d’envoyer un événement à partir d’un thread d’arrière-plan consiste à utiliser l’objet [Windows. Foundation. EventHandler](/uwp/api/windows.foundation.eventhandler) &lt; &gt; comme type de l’événement. Windows fournit cette instanciation concrète du type générique, ainsi qu’un proxy et un stub pour celui-ci. L’inconvénient est que les informations sur le type de vos arguments et de votre émetteur d’événement sont perdues. Les clients C++ et .NET doivent connaître via la documentation le type vers lequel effectuer un cast lorsque l’événement est reçu. Les clients JavaScript n’ont pas besoin des informations sur le type d’origine. Ils recherchent les propriétés des arguments, selon leurs noms dans les métadonnées.

Cet exemple montre comment utiliser Windows.Foundation.EventHandler&lt;Object&gt; en C# :

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Cet événement est utilisé du côté JavaScript comme suit :

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(Option nº 3) Créer vos propres proxy/stub

Pour améliorer les performances potentielles sur les types d’événements définis par l’utilisateur qui présentent des informations entièrement préservées sur le type, vous devez créer vos propres objets proxy et stub et les inclure dans le package de votre application. En règle générale, vous devez utiliser cette option uniquement dans les rares cas où aucune des deux autres options n’est adaptée. En outre, il n’y a aucune garantie que cette option offrira de meilleures performances que les deux autres options. Les performances réelles dépendent de nombreux facteurs. Pour mesurer les performances réelles dans votre application et déterminer si l’événement est en fait un goulot d’étranglement, utilisez le profileur Visual Studio ou d’autres outils de profilage.

Le reste de cet article explique comment utiliser C# pour créer un composant Windows Runtime de base, puis comment utiliser C++ pour créer une DLL pour le proxy et le stub qui permettra à JavaScript d’utiliser un événement Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; déclenché par le composant dans une opération asynchrone. (Vous pouvez également utiliser C++ ou Visual Basic pour créer le composant. Les étapes liées à la création des proxys et des stubs sont identiques.) Cette procédure pas à pas est basée sur l’article Création d’un exemple de composant in-process Windows Runtime (C++/CX) et contribue à expliquer les différents rôles de ce dernier.

Cette procédure pas à pas comporte les éléments suivants.

-   Ici, vous allez créer deux classes Windows Runtime de base. Une classe expose un événement de type [Windows. Foundation. TypedEventHandler &lt; TSender, TResult &gt; ](/uwp/api/windows.foundation.typedeventhandler-2) et l’autre classe est le type qui est retourné à JavaScript comme argument pour TValue. Ces classes ne peuvent pas communiquer avec JavaScript tant que vous n’avez pas effectué les étapes suivantes.
-   Cette application active l’objet de classe principal, appelle une méthode et gère un événement déclenché par le composant Windows Runtime.
-   Ces éléments sont requis par les outils qui génèrent les classes proxy et stub.
-   Vous devez ensuite utiliser le fichier IDL pour générer le code source C pour le proxy et le stub.
-   Inscrivez les objets proxy-stub afin que le runtime COM puisse les trouver, puis référencez la DLL de proxy-stub dans le projet d’application.

## <a name="to-create-the-windows-runtime-component"></a>Pour créer le composant Windows Runtime

Dans Visual Studio, dans la barre de menus, choisissez **fichier &gt; nouveau projet**. Dans la boîte de dialogue **Nouveau projet**, développez **JavaScript &gt; Windows universel**, puis sélectionnez **Application vide**. Nommez le projet ToasterApplication, puis cliquez sur le bouton **OK**.

Ajoutez un composant Windows Runtime C# à la solution : dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Ajouter &gt; Nouveau projet**. Développez **Visual C# &gt; Microsoft Store** puis sélectionnez **Windows Runtime composant**. Nommez le projet ToasterComponent, puis cliquez sur le bouton **OK**. ToasterComponent sera l’espace de noms racine pour les composants que vous créerez lors des étapes ultérieures.

Dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Propriétés**. Dans la boîte de dialogue **Pages de propriétés**, sélectionnez **Propriétés de configuration** dans le volet gauche, puis en haut de la boîte de dialogue, définissez **Configuration** sur **Déboguer** et **Plateforme** sur x86, x64 ou ARM. Choisissez le bouton **OK**.

**Important**   Platform = Any CPU ne fonctionne pas, car il n’est pas valide pour la DLL Win32 de code natif que vous ajouterez ultérieurement à la solution.

Dans l’Explorateur de solutions, remplacez le nom class1.cs par ToasterComponent.cs afin qu’il corresponde au nom du projet. Visual Studio renomme automatiquement la classe dans le fichier pour qu’elle corresponde au nouveau nom de fichier.

Dans le fichier .cs, ajoutez une directive using pour l’espace de noms Windows.Foundation afin d’inclure TypedEventHandler dans la portée.

Lorsque vous avez besoin de proxys et de stubs, votre composant doit utiliser des interfaces pour exposer ses membres publics. Dans ToasterComponent.cs, définissez une interface pour le générateur de toasts et une autre pour le toast que le générateur produit.

**Remarque**   En C#, vous pouvez ignorer cette étape. À la place, créez d’abord une classe, puis ouvrez son menu contextuel et choisissez **Refactoriser &gt; Extraire l’interface**. Dans le code généré, donnez manuellement aux interfaces une accessibilité publique.

```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
```

L’interface IToast comporte qui peut être récupérée pour décrire le type de toast. L’interface IToaster dispose d’une méthode pour créer un toast et d’un événement pour indiquer que le toast est créé. Comme cet événement retourne le type particulier du toast, il est appelé événement typé.

Ensuite, nous avons besoin de classes qui implémentent ces interfaces, et qui soient publiques et verrouillées afin qu’elles soient accessibles depuis l’application JavaScript que vous programmerez ultérieurement.

```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
```

Dans le code précédent, nous créons le toast, puis accélérons un élément de travail de pool de threads pour déclencher la notification. Bien que l’IDE puisse suggérer que vous appliquez le mot clé await à l’appel asynchrone, cela n’est pas nécessaire dans ce cas, parce que la méthode n’effectue aucun travail dépendant des résultats de l’opération.

**Remarque**   L’appel asynchrone dans le code précédent utilise ThreadPool. RunAsync uniquement pour illustrer un moyen simple de déclencher l’événement sur un thread d’arrière-plan. Vous pouvez écrire cette méthode particulière comme indiqué dans l’exemple suivant, et elle s’exécutera correctement, car le planificateur de tâches .NET marshale automatiquement les appels asynchrones/d’attente vers le thread d’interface utilisateur.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Si vous générez le projet à présent, il doit être généré correctement.

## <a name="to-program-the-javascript-app"></a>Pour programmer l'application JavaScript

Maintenant nous pouvons ajouter un bouton à l'application JavaScript pour qu'elle utilise la classe que nous venons de définir pour créer un toast. Avant de le faire, nous devons ajouter une référence au projet ToasterComponent que nous venons de créer. Dans Explorateur de solutions, ouvrez le menu contextuel du projet ToasterApplication, choisissez **Ajouter des &gt; références**, puis choisissez le bouton **Ajouter une nouvelle référence** . Dans la boîte de dialogue Ajouter une référence, dans le volet gauche, sous Solution, sélectionnez le projet de composant, puis, dans le volet central, sélectionnez ToasterComponent. Choisissez le bouton **OK**.

Dans Explorateur de solutions, ouvrez le menu contextuel du projet ToasterApplication, puis choisissez **définir comme projet de démarrage**.

À la fin du fichier default.js, ajoutez un espace de noms pour contenir les fonctions permettant d'appeler le composant et d'être rappelé par celui-ci. L'espace de noms possède deux fonctions, une pour créer un toast et une pour gérer l'événement de fin de toast. L’implémentation de makeToast crée un objet toaster, inscrit le gestionnaire d’événements et crée le Toast. Jusqu'à présent, le gestionnaire d'événements ne fait pas grand-chose, comme illustré ci-après :

```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
```

La fonction makeToast doit être raccordée à un bouton. Mettez à jour le fichier default.html pour inclure un bouton et un espace pour sortir le résultat de la création du toast :

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Si nous n’utilisons pas de TypedEventHandler, nous pourrions maintenant être en mesure d’exécuter l’application sur l’ordinateur local et de cliquer sur le bouton pour créer un toast. Mais dans notre application, rien ne se produit. Pour savoir pourquoi, déboguez le code managé qui déclenche l’ToastCompletedEvent. Arrêtez le projet, puis dans la barre de menus, choisissez **déboguer les propriétés de l' &gt; application toaster**. Remplacez le **type de débogueur** par **managé uniquement**. À nouveau dans la barre de menus, choisissez **déboguer les &gt; exceptions**, puis sélectionnez **Exceptions Common Language Runtime**.

Maintenant, exécutez l'application et cliquez sur le bouton de création de toast. Le débogueur intercepte une exception de cast non valide. Bien que cela ne soit pas évident au vu de son message, cette exception se produit parce que les proxies sont manquants pour cette interface.

![proxy manquant](./images/debuggererrormissingproxy.png)

La première étape de la création d'un proxy et d'un stub pour un composant consiste à ajouter un identificateur unique ou GUID aux interfaces. Toutefois, le format de GUID à utiliser diffère selon que vous codez en C#, en Visual Basic ou autre langage .NET, ou en C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Pour générer des GUID pour les interfaces du composant (C# et d’autres langages .NET)

Dans la barre de menus, choisissez outils &gt; créer un GUID. Dans la boîte de dialogue, sélectionnez 5. \[Guid ("xxxxxxxx-xxxx... xxxx») \] . Sélectionnez le bouton Nouveau GUID, puis cliquez sur le bouton Copier.

![outil générateur de GUID](./images/guidgeneratortool.png)

Revenez à la définition d’interface, puis collez le nouveau GUID juste avant l’interface IToaster, comme illustré dans l’exemple suivant. N'utilisez pas le GUID de l'exemple. Chaque interface unique doit avoir son propre GUID.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Ajoutez une directive using pour l’espace de noms System. Runtime. InteropServices.

Répétez ces étapes pour l’interface IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Pour générer des GUID pour les interfaces du composant (C++)

Dans la barre de menus, choisissez outils &gt; créer un GUID. Dans la boîte de dialogue, sélectionnez 3. GUID de struct const static = {...}. Sélectionnez le bouton Nouveau GUID, puis cliquez sur le bouton Copier.

Collez le GUID juste avant la définition de l’interface IToaster. Une fois collé, le GUID doit ressembler à l'exemple suivant. N'utilisez pas le GUID de l'exemple. Chaque interface unique doit avoir son propre GUID.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Ajoutez une directive using pour Windows. Foundation. Metadata pour amener GuidAttribute dans la portée.

Convertissez ensuite manuellement le GUID const en un GuidAttribute afin qu'il soit mis en forme comme illustré dans l'exemple suivant. Notez que les accolades sont remplacées par des crochets et des parenthèses, et que le point-virgule de fin est supprimé.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Répétez ces étapes pour l’interface IToast.

Maintenant que les interfaces ont des ID uniques, nous pouvons créer un fichier IDL en alimentant le fichier. winmd dans l’outil de ligne de commande winmdidl, puis générer le code source C pour le proxy et le stub en alimentant ce fichier IDL dans l’outil en ligne de commande MIDL. Visual Studio le fait pour nous si nous créons des événements post-build comme indiqué dans les étapes suivantes.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Pour générer le code source du proxy et du stub

Pour ajouter un événement post-build personnalisé, dans l'Explorateur de solutions, ouvrez le menu contextuel du projet ToasterComponent, puis sélectionnez Propriétés. Dans le volet gauche des pages de propriétés, sélectionnez Événements de build, puis sélectionnez le bouton Modifier post-build. Ajoutez les commandes suivantes à la ligne de commande post-build. (Le fichier de commandes doit être appelé en premier pour définir les variables d’environnement afin de Rechercher l’outil winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Important**    Pour une configuration de projet ARM ou x64, remplacez le paramètre MIDL/env par x64 ou Arm32.

Pour vous assurer que le fichier IDL est régénéré chaque fois que le fichier. winmd est modifié, remplacez **exécuter l’événement après génération** par **lorsque la génération met à jour la sortie du projet.**
La page de propriétés événements de build doit ressembler à ceci : ![ événements de build](./images/buildevents.png)

Régénérez la solution pour générer et compiler le fichier IDL.

Vous pouvez vérifier que MIDL a correctement compilé la solution en recherchant ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c et dlldata.c dans le répertoire du projet ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Pour compiler le code de proxy et de stub dans une DLL

Maintenant que vous avez les fichiers requis, vous pouvez les compiler pour produire une DLL, qui est un fichier C++. Pour rendre cette opération aussi simple que possible, ajoutez un nouveau projet pour prendre en charge la génération des proxies. Ouvrez le menu contextuel de la solution ToasterApplication, puis choisissez **ajouter > nouveau projet**. Dans le volet gauche de la boîte de dialogue **nouveau projet** , développez **Visual C++ Windows &gt; &gt; universels**Windows, puis, dans le volet central, sélectionnez **dll (applications UWP)**. (Notez qu’il ne s’agit pas d’un projet de composant C++ Windows Runtime.) Nommez les proxies du projet, puis choisissez le bouton **OK** . Ces fichiers sont mis à jour par les événements post-build lorsque quelque chose change dans la classe C#.

Par défaut, le projet Proxies génère des fichiers d'en-tête .h et des fichiers C++ .cpp. Étant donné que la DLL est générée à partir des fichiers produits avec MIDL, les fichiers .h et .cpp ne sont pas requis. Dans Explorateur de solutions, ouvrez le menu contextuel correspondant, choisissez **supprimer**, puis confirmez la suppression.

Maintenant que le projet est vide, vous pouvez ajouter à nouveau les fichiers générés par MIDL. Ouvrez le menu contextuel du projet proxies, puis choisissez **ajouter > élément existant.** Dans la boîte de dialogue, naviguez jusqu'au répertoire du projet ToasterComponent et sélectionnez les fichiers suivants : ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c et dlldata.c. Choisissez le bouton **Ajouter**.

Dans le projet Proxies, créez un fichier .def pour définir les exportations de DLL décrites dans dlldata.c. Ouvrez le menu contextuel du projet, puis choisissez **ajouter > nouvel élément**. Dans le volet gauche de la boîte de dialogue, sélectionnez Code, puis, dans le volet central, sélectionnez Fichier de définition de module. Nommez le fichier proxies. def, puis cliquez sur le bouton **Ajouter** . Ouvrez ce fichier .def et modifiez-le pour inclure les EXPORTATIONS définies dans dlldata.c :

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Si vous générez le projet maintenant, il échouera. Pour compiler correctement ce projet, vous devez modifier la façon dont le projet est compilé et lié. Dans Explorateur de solutions, ouvrez le menu contextuel du projet proxies, puis choisissez **Propriétés**. Modifiez les pages de propriétés comme suit.

Dans le volet gauche, sélectionnez **C/C++ > préprocesseur**, puis dans le volet droit, sélectionnez définitions de **préprocesseur**, cliquez sur le bouton fléché vers le bas, puis sélectionnez **modifier**. Ajoutez ces définitions dans la zone :

```cpp
WIN32;_WINDOWS
```
Sous **C/C++ > en-têtes précompilés**, modifiez **en-tête précompilé** pour qu’il **n’utilise pas d’en-têtes précompilés**, puis choisissez le bouton **appliquer** .

Dans l' **éditeur de liens, > général**, remplacez la **bibliothèque d’importation** par la valeur **Yé**s, puis cliquez sur le bouton **appliquer** .

Sous **éditeur de liens > entrée**, sélectionnez **dépendances supplémentaires**, cliquez sur le bouton fléché vers le bas, puis sélectionnez **modifier**. Ajoutez ce texte dans la zone :

```cpp
rpcrt4.lib;runtimeobject.lib
```

Ne collez pas ces bibliothèques directement dans la ligne de liste. Utilisez la zone d' **édition** pour vous assurer que MSBuild dans Visual Studio conservera les dépendances supplémentaires appropriées.

Une fois ces modifications effectuées, cliquez sur le bouton **OK** dans la boîte de dialogue **pages de propriétés** .

Ensuite, prenez une dépendance sur le projet ToasterComponent. Cela garantit que le générateur de toasts sera généré avant le projet de proxy. Ceci est obligatoire, parce que le projet de générateur de toasts est chargé de générer les fichiers permettant de générer le proxy.

Ouvrez le menu contextuel du projet Proxies, puis sélectionnez Dépendances du projet. Activez les cases à cocher pour indiquer que le projet Proxies dépend du projet ToasterComponent, afin de vous assurer que Visual Studio les génère dans l'ordre adéquat.

Vérifiez que la solution est correctement générée en choisissant **générer > régénérer la solution** dans la barre de menus de Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Pour enregistrer le proxy et le stub

Dans le projet ToasterApplication, ouvrez le menu contextuel de package. appxmanifest, puis choisissez **Ouvrir avec**. Dans la boîte de dialogue Ouvrir avec, sélectionnez **éditeur de texte XML** , puis cliquez sur le bouton **OK** . Nous allons coller dans du code XML qui fournit une inscription d’extension Windows. activatableClass. proxyStub et qui sont basés sur les GUID dans le proxy. Pour rechercher les GUID à utiliser dans le fichier .appxmanifest, ouvrez ToasterComponent_i.c. Recherchez les entrées qui ressemblent à celles de l'exemple suivant. Notez également les définitions pour IToast, IToaster et une troisième interface : un gestionnaire d’événements typés qui a deux paramètres : toaster et Toast. Cela correspond à l’événement défini dans la classe toaster. Notez que les GUID pour IToast et IToaster correspondent aux GUID définis sur les interfaces dans le fichier C#. Étant donné que l'interface de gestionnaire d'événements typés est créée automatiquement, le GUID de cette interface l'est également.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Maintenant, nous copions les GUID, vous les collez dans package. appxmanifest dans un nœud que nous ajoutons et indiquons des extensions, puis nous les reformatons. L'entrée de manifeste ressemble à l'exemple suivant, mais, encore une fois, n'oubliez pas d'utiliser vos propres GUID. Notez que le GUID ClassId dans le fichier XML est identique à ITypedEventHandler2. C'est parce que ce GUID est le premier répertorié dans ToasterComponent_i.c. Les GUID ici ne respectent pas la casse. Au lieu de reformater manuellement les GUID pour IToast et IToaster, vous pouvez revenir aux définitions d’interface et obtenir la valeur GuidAttribute, qui a le format correct. En C++, il existe un GUID correctement mis en forme dans le commentaire. Dans tous les cas, vous devez reformater manuellement le GUID utilisé pour le ClassId et le gestionnaire d'événements.

```cpp
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
```

Collez le nœud XML des extensions en tant qu’enfant direct du nœud de package et un homologue de, par exemple, le nœud ressources.

Avant de continuer, il est important de vérifier que :

-   ProxyStub ClassId est défini sur le premier GUID dans le fichier ToasterComponent \_ i. c. Utilisez le premier GUID défini dans ce fichier pour le classId. (Cela peut être le même que le GUID pour ITypedEventHandler2.)
-   Le chemin d’accès est le chemin d’accès relatif du package du binaire du proxy. (Dans cette procédure pas à pas, proxies.dll se trouve dans le même dossier que ToasterApplication.winmd.)
-   Les GUID ont le format correct. (Il est facile de se tromper.)
-   Les ID d’interface dans le manifeste correspondent aux IID du \_ fichier ToasterComponent i. c.
-   Les noms d'interface sont uniques dans le manifeste. Étant donné que ceux-ci ne sont pas utilisés par le système, vous pouvez choisir les valeurs. Il est conseillé de choisir des noms d'interface qui correspondent clairement aux interfaces que vous avez définies. Pour des interfaces générées, les noms doivent évoquer les interfaces générées. Vous pouvez utiliser le \_ fichier ToasterComponent i. c pour vous aider à générer des noms d’interface.

Si vous essayez à présent d'exécuter la solution, vous obtiendrez une erreur indiquant que proxies.dll ne fait pas partie de la charge utile. Ouvrez le menu contextuel du dossier **références** dans le projet ToasterApplication, puis choisissez **Ajouter une référence**. Activez la case à cocher en regard du projet Proxies. En outre, assurez-vous que la case à cocher en regard de ToasterComponent est également cochée. Choisissez le bouton **OK**.

À présent, le projet doit se générer. Exécutez le projet et vérifiez que vous pouvez créer un toast.

## <a name="related-topics"></a>Rubriques connexes

* [Composants Windows Runtime avec C++/CX](creating-windows-runtime-components-in-cpp.md)
