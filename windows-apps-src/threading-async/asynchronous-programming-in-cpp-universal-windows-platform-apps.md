---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Cet article décrit la meilleure façon d’utiliser des méthodes asynchrones dans les extensions des composants Visual C++ (C++/CX) à l’aide de la classe task qui est définie dans l’espace de noms concurrency dans ppltasks.h.
title: Programmation asynchrone en C++
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, threads, asynchrone, C++
ms.localizationpriority: medium
ms.openlocfilehash: e08a73c7617a5b24af49d5b3665303124e28d257
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750155"
---
# <a name="asynchronous-programming-in-ccx"></a>Programmation asynchrone en C++/CX
> [!NOTE]
> Cette rubrique existe pour vous aider à gérer votre application C++/CX. Toutefois, nous vous recommandons d’utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour les nouvelles applications. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne.

Cet article décrit la meilleure façon d’utiliser des méthodes asynchrones dans les extensions des composants Visual C++ (C++/CX) à l’aide de la classe `task` qui est définie dans l’espace de noms `concurrency` dans ppltasks.h.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Types asynchrones de plateforme Windows universelle (UWP)
Les fonctionnalités de plateforme Windows universelle UWP comprennent un modèle bien défini pour l’appel de méthodes asynchrones, et fournissent les types dont vous avez besoin pour consommer de telles méthodes. Si vous n’êtes pas familiarisé avec le modèle UWP asynchrone, lisez [programmation asynchrone][AsyncProgramming] avant de lire le reste de cet article.

Bien que vous puissiez consommer les API de Windows Runtime asynchrones directement en C++, l’approche recommandée consiste à utiliser la [**classe de tâche**][task-class] et ses types et fonctions connexes, qui sont contenus dans l’espace de noms d' [**accès concurrentiel**][concurrencyNamespace] et définis dans `<ppltasks.h>` . Le type **concurrency::task** est un type à usage général, mais lorsque le commutateur du compilateur **/ZW**, requis pour les applications et composants de plateforme Windows universelle (UWP), est utilisé, la classe task encapsule les types asynchrones UWP, ce qui facilite les opérations suivantes :

-   chaîner plusieurs opérations asynchrones et synchrones

-   gérer des exceptions dans des chaînes de tâches

-   effectuer des annulations dans des chaînes de tâches

-   garantir que des tâches individuelles s’exécutent dans le contexte de thread ou le cloisonnement de threads approprié

Cet article fournit des indications de base sur la façon d’utiliser la classe **task** avec les API asynchrones UWP. Pour obtenir une documentation plus complète sur les **tâches** et les méthodes associées, notamment [**créer une \_ tâche**][createTask], consultez [parallélisme des tâches (Runtime d’accès concurrentiel)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Consommation d’une opération asynchrone à l’aide d’une tâche
L’exemple suivant montre comment utiliser la classe Task pour consommer une méthode **Async** qui retourne une interface [**IAsyncOperation**][IAsyncOperation] et dont l’opération produit une valeur. Les étapes de base sont les suivantes :

1.  Appelez la méthode `create_task` et passez-lui l’objet **IAsyncOperation^**.

2.  Appelez la fonction membre [**Task :: Then**][taskThen] sur la tâche et fournissez une expression lambda qui sera appelée lorsque l’opération asynchrone se termine.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

La tâche qui est créée et retournée par la fonction [**Task :: Then**][taskThen] est appelée *continuation*. L’argument d’entrée (dans le cas présent) pour l’expression lambda fournie par l’utilisateur est le résultat que l’opération de la tâche produit quand elle se termine. C’est la même valeur que celle qui serait récupérée en appelant la méthode [**IAsyncOperation::GetResults**](/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) si vous utilisiez directement l’interface **IAsyncOperation**.

La méthode [**Task :: Then**][taskThen] est retournée immédiatement, et son délégué ne s’exécute pas jusqu’à ce que le travail asynchrone se termine correctement. Dans cet exemple, si l’opération asynchrone entraîne la levée d’une exception, ou se termine dans l’état d’annulation suite à une demande d’annulation, la continuation ne s’exécute jamais. Nous expliquerons ultérieurement comment écrire des continuations qui s’exécutent même si la tâche précédente a été annulée ou a échoué.

Bien que vous déclariez la variable de la tâche sur la pile locale, elle gère sa durée de vie de façon à ne pas être supprimée tant que toutes ses opérations ne sont pas terminées et que toutes les références à celle-ci ne sortent pas de l’étendue, même si la méthode retourne un résultat avant que les opérations soient terminées.

## <a name="creating-a-chain-of-tasks"></a>Création d’une chaîne de tâches
Dans la programmation asynchrone, il est courant de définir une séquence d’opérations, également appelée *chaîne de tâches*, dans laquelle chaque continuation s’exécute uniquement si la précédente est terminée. Dans certains cas, la tâche précédente (aussi appelée *antécédent*) produit une valeur que la continuation accepte comme entrée. À l’aide de la méthode [**Task :: Then**][taskThen] , vous pouvez créer des chaînes de tâches de manière intuitive et simple. la méthode retourne une **tâche <T> ** où **T** est le type de retour de la fonction lambda. Vous pouvez composer plusieurs continuations dans une chaîne de tâches : `myTask.then(…).then(…).then(…);`

Les chaînes de tâches sont particulièrement utiles lorsqu’une continuation crée une nouvelle opération asynchrone ; une telle tâche est appelée « tâche asynchrone ». L’exemple suivant illustre une chaîne de tâches comportant deux continuations. La tâche initiale acquiert le handle vers un fichier existant, et lorsque cette opération se termine, la première continuation démarre une nouvelle opération asynchrone pour supprimer le fichier. Lorsque l’opération se termine, la deuxième continuation s’exécute, et génère un message de confirmation.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

L’exemple précédent illustre quatre points importants :

-   La première continuation convertit l’objet [**IAsyncAction ^**][IAsyncAction] en **tâche <void> ** et retourne la **tâche**.

-   La deuxième continuation n’effectue aucune gestion des erreurs, et par conséquent, prend **void** et non pas **task<void>** comme entrée. C’est une continuation basée sur les valeurs.

-   La deuxième continuation ne s’exécute pas tant que l’opération [**DeleteAsync**][deleteAsync] n’est pas terminée.

-   Étant donné que la deuxième continuation est basée sur la valeur, si l’opération qui a été démarrée par l’appel à [**DeleteAsync**][deleteAsync] lève une exception, la deuxième continuation ne s’exécute pas du tout.

**Remarque**    La création d’une chaîne de tâches n’est qu’une des façons d’utiliser la classe **Task** pour composer des opérations asynchrones. Vous pouvez également composer des opérations à l’aide des opérateurs Join et Choice **&&** et **||** . Pour plus d’informations, consultez [parallélisme des tâches (Runtime d’accès concurrentiel)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Types de retour de la fonction lambda et types de retour d’une tâche
Dans une continuation de tâche, le type de retour de la fonction lambda est encapsulé dans un objet **task**. Si l’expression lambda retourne une valeur **double**, le type de la tâche de continuation est **task<double>**. Toutefois, l’objet task est conçu de façon à ne pas produire inutilement des types de retour imbriqués. Si une expression lambda retourne un **IAsyncOperation&lt;SyndicationFeed^&gt;^**, la continuation retourne un **task&lt;SyndicationFeed^&gt;**, et non un **task&lt;task&lt;SyndicationFeed^&gt;&gt;** ou **task&lt;IAsyncOperation&lt;SyndicationFeed^&gt;^&gt;^**. Ce processus, qui porte le nom de *désencapsulation asynchrone*, garantit également l’achèvement de l’opération asynchrone à l’intérieur de la continuation avant l’appel de la continuation suivante.

Dans l’exemple précédent, Notez que la tâche retourne une **tâche <void> ** , même si son lambda a retourné un objet [**IAsyncInfo**][IAsyncInfo] . Le tableau suivant résume les conversions de types qui se produisent entre une fonction lambda et la tâche englobante :

| type de retour de la fonction lambda | `.then` type de retour |
| ------------------ | ------------------- |
| TResult                                                | tâche<TResult> |
| IAsyncOperation<TResult>^                        | tâche<TResult> |
| IAsyncOperationWithProgress&lt;TResult, TProgress&gt;^ | tâche<TResult> |
|IAsyncAction^                                           | tâche<void>    |
| IAsyncActionWithProgress<TProgress>^             |tâche<void>     |
| tâche<TResult>                                    |tâche<TResult>  |


## <a name="canceling-tasks"></a>Annulation des tâches
Il est souvent conseillé de donner à l’utilisateur la possibilité d’annuler une opération asynchrone. Et dans certains cas, vous pouvez avoir besoin d’annuler une opération par programme depuis l’extérieur de la chaîne de tâches. Bien que chaque \* type de retour **asynchrone** ait une méthode [**Cancel**][IAsyncInfoCancel] dont il hérite de [**IAsyncInfo**][IAsyncInfo], il est difficile de l’exposer à des méthodes externes. La méthode recommandée pour prendre en charge l’annulation dans une chaîne de tâches consiste à utiliser une [** \_ \_ source de jeton d’annulation**](/cpp/parallel/concrt/reference/cancellation-token-source-class) pour créer un [** \_ jeton d’annulation**](/cpp/parallel/concrt/reference/cancellation-token-class), puis à passer le jeton au constructeur de la tâche initiale. Si une tâche asynchrone est créée avec un jeton d’annulation et que le [**jeton d’annulation \_ \_ source :: Cancel**](/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017) est appelé, la tâche appelle automatiquement **Cancel** sur l’opération **IAsync \* ** et passe la requête d’annulation dans sa chaîne de continuation. Le pseudo-code suivant présente l’approche de base.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

Lorsqu’une tâche est annulée, une exception de [**tâche \_ annulée**][taskCanceled] est propagée vers le dessous de la chaîne de tâches. Les continuations basées sur des valeurs ne s’exécutent tout simplement pas, mais les continuations basées sur les tâches provoquent la levée de l’exception quand [**Task :: obten**][taskGet] est appelé. Si vous avez une continuation de gestion des erreurs, assurez-vous qu’elle intercepte l’exception de la **tâche \_ annulée** explicitement. (Cette exception n’est pas dérivée de [**Platform::Exception**](/cpp/cppcx/platform-exception-class).)

L’annulation est coopérative. Si votre continuation effectue un travail de longue durée au-delà du simple appel de méthode UWP, il vous incombe de vérifier régulièrement l’état du jeton d’annulation et d’arrêter l’exécution s’il est annulé. Après avoir nettoyé toutes les ressources qui ont été allouées dans la continuation, appelez [**annuler la \_ \_ tâche actuelle**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) pour annuler cette tâche et propager l’annulation vers toutes les continuations basées sur une valeur qui la suivent. Voici un autre exemple : vous pouvez créer une chaîne de tâches qui représente le résultat d’une opération [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker). Si l’utilisateur choisit le bouton **Annuler** , la méthode [**IAsyncInfo :: Cancel**][IAsyncInfoCancel] n’est pas appelée. Au lieu de cela, l’opération réussit mais renvoie **nullptr**. La continuation peut tester le paramètre d’entrée et appeler **annuler la \_ \_ tâche actuelle** si l’entrée est **nullptr**.

Pour plus d’informations, voir [Annulation dans la bibliothèque de modèles parallèles](/cpp/parallel/concrt/cancellation-in-the-ppl)

## <a name="handling-errors-in-a-task-chain"></a>Gestion des erreurs dans une chaîne de tâches
Si vous souhaitez qu’une continuation s’exécute même si l’antécédent a été annulé ou a levé une exception, effectuez la continuation d’une continuation basée sur les tâches en spécifiant l’entrée de sa fonction lambda en tant que **tâche <TResult> ** ou **tâche <void> ** si l’expression lambda de la tâche antécédente retourne un [**IAsyncAction ^**][IAsyncAction].

Pour gérer les erreurs et l’annulation dans une chaîne de tâches, il n’est pas nécessaire de transformer chaque continuation en continuation basée sur les tâches ou de placer chaque opération qui pourrait être déclenchée dans un bloc `try…catch`. Au lieu de cela, vous pouvez ajouter une continuation basée sur les tâches à la fin de la chaîne et gérer toutes les erreurs à cet emplacement. Toute exception (y compris une exception de [**tâche \_ annulée**][taskCanceled] ) se propage dans la chaîne de tâches et ignore toutes les continuations basées sur les valeurs, afin que vous puissiez la gérer dans la continuation basée sur les tâches de gestion des erreurs. Vous pouvez récrire l’exemple précédent pour utiliser une continuation de gestion des erreurs basée sur les tâches :

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

Dans une continuation basée sur des tâches, nous appelons la fonction membre [**Task :: obtenir**][taskGet] pour obtenir les résultats de la tâche. Nous devons toujours appeler **Task :: obtenir** même si l’opération était un [**IAsyncAction**][IAsyncAction] qui ne produit aucun résultat, car **Task :: obtenir** obtient également toutes les exceptions qui ont été transportées vers la tâche. Si la tâche d’entrée stocke une exception, celle-ci est levée lors de l’appel à **task::get**. Si vous n’appelez pas **Task :: obtenir**ou si vous n’utilisez pas de continuation basée sur des tâches à la fin de la chaîne, ou si vous n’interceptez pas le type d’exception qui a été levé, une ** \_ \_ exception de tâche** non prise en compte est levée lorsque toutes les références à la tâche ont été supprimées.

Interceptez uniquement les exceptions que vous pouvez gérer. Si votre application rencontre une erreur de laquelle vous ne pouvez pas récupérer, il est préférable de laisser l’application se bloquer plutôt que de laisser son exécution se poursuivre dans un état inconnu. En outre, en général, ne tentez pas d’intercepter l' ** \_ \_ exception de tâche** non prise en même. Cette exception est principalement destinée aux opérations de diagnostic. Quand une ** \_ \_ exception de tâche non observée** est levée, elle indique généralement un bogue dans le code. La cause est souvent soit une exception qui doit être gérée, soit une exception irrécupérable qui est provoquée par une autre erreur dans le code.

## <a name="managing-the-thread-context"></a>Gestion du contexte de thread
L’interface utilisateur d’une application UWP s’exécute dans un thread unique cloisonné (STA). Une tâche dont l’expression lambda retourne un [**IAsyncAction**][IAsyncAction] ou un [**IAsyncOperation**][IAsyncOperation] est compatible Apartment. Sauf spécification contraire, si la tâche est créée dans le thread unique cloisonné (STA), toutes les continuations qu’elle contient s’exécuteront également par défaut. En d’autres termes, l’ensemble de la chaîne de tâches hérite de la prise en charge du cloisonnement de la tâche parente. Ce comportement permet de simplifier les interactions avec les contrôles d’interface utilisateur, lesquels sont accessibles uniquement à partir du thread unique cloisonné (STA).

Par exemple, dans une application UWP, dans la fonction membre d’une classe qui représente une page XAML, vous pouvez remplir un contrôle [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) à partir d’une méthode [**Task :: Then**][taskThen] sans avoir à utiliser l’objet [**répartiteur**](/uwp/api/Windows.UI.Core.CoreDispatcher) .

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

Si une tâche ne retourne pas de [**IAsyncAction**][IAsyncAction] ou de [**IAsyncOperation**][IAsyncOperation], elle ne prend pas en charge la cloisonnement et, par défaut, ses continuations sont exécutées sur le premier thread d’arrière-plan disponible.

Vous pouvez remplacer le contexte de thread par défaut pour chaque type de tâche à l’aide de la surcharge de [**Task :: Then**][taskThen] qui prend un [** \_ \_ contexte de continuation de tâche**](/cpp/parallel/concrt/reference/task-continuation-context-class). Par exemple, dans certains cas, il peut être souhaitable de planifier la continuation d’une tâche prenant en charge le cloisonnement sur un thread d’arrière-plan. Dans ce cas, vous pouvez passer le [** \_ contexte de continuation de tâche \_ :: use \_ arbitraire**][useArbitrary] pour planifier le travail de la tâche sur le prochain thread disponible dans un cloisonnement multithread. Cela peut améliorer les performances de la continuation, car son travail n’a pas besoin d’être synchronisé avec un autre travail qui se produit sur le thread d’interface utilisateur.

L’exemple suivant montre qu’il est utile de spécifier l' [**option \_ Context de continuation de tâche \_ :: use \_ arbitraire**][useArbitrary] . il montre également comment le contexte de continuation par défaut est utile pour synchroniser des opérations simultanées sur des collections non thread-safe. Dans ce code, nous parcourons en boucle une liste d’URL pour des flux RSS, et pour chaque URL, nous démarrons une opération asynchrone pour récupérer les données de flux. Nous ne pouvons pas contrôler l’ordre dans lequel les flux sont récupérés, et cela n’a pas vraiment d’importance. Quand chaque opération [**RetrieveFeedAsync**](/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) se termine, la première continuation accepte l’objet [**SyndicationFeed^**](/uwp/api/Windows.Web.Syndication.SyndicationFeed) et l’utilise pour initialiser un objet `FeedData^` défini par l’application. Étant donné que chacune de ces opérations est indépendante des autres, nous pouvons potentiellement accélérer les choses en spécifiant le ** \_ contexte de continuation de tâche \_ :: use \_ arbitraire** Context continuation. Toutefois, une fois que chaque `FeedData` objet est initialisé, nous devons l’ajouter à un [**vecteur**](/cpp/cppcx/platform-collections-vector-class), qui n’est pas une collection thread-safe. Par conséquent, nous créons une continuation et spécifions le [** \_ contexte de continuation de tâche \_ :: use \_ Current**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) pour garantir que tous les appels à [**Append**](/uwp/api/windows.foundation.collections.ivector_t_.append) se produisent dans le même contexte de cloisonnement de thread unique (ASTA) de l’application. Étant donné que le contexte de [** \_ continuation de tâche \_ :: use \_ default**](/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) est le contexte par défaut, nous n’avons pas à le spécifier explicitement, mais nous le faisons ici pour des raisons de clarté.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

Les tâches imbriquées, qui sont de nouvelles tâches créées à l’intérieur d’une continuation, n’héritent pas de la prise en charge du cloisonnement de la tâche initiale.

## <a name="handing-progress-updates"></a>Gestion des mises à jour de l’avancement
Les méthodes qui prennent en charge [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) ou [**IAsyncActionWithProgress**](/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) fournissent régulièrement des mises à jour de l’avancement pendant que l’opération est en cours, avant qu’elle ne se termine. Le signalement de l’avancement est indépendant de la notion de tâches et de continuations. Vous fournissez simplement le délégué pour la propriété [**Progress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) de l’objet. Le délégué est généralement utilisé pour mettre à jour une barre de progression dans l’interface utilisateur.

## <a name="related-topics"></a>Rubriques connexes
* [Création d’opérations asynchrones en C++/CX pour les applications UWP](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Informations de référence en matière de langage Visual C++](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Programmation asynchrone][AsyncProgramming]
* [Parallélisme des tâches (runtime d’accès concurrentiel)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: ./asynchronous-programming-universal-windows-platform-apps.md "AsyncProgramming"
[concurrencyNamespace]: /cpp/parallel/concrt/reference/concurrency-namespace "Espace de noms d’accès concurrentiel"
[createTask]: /cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task "CreateTask"
[deleteAsync]: /uwp/api/Windows.Storage.StorageFile "DeleteAsync"
[IAsyncAction]: /uwp/api/Windows.Foundation.IAsyncAction "IAsyncAction"
[IAsyncOperation]: /uwp/api/windows.foundation.iasyncoperation-1 "IAsyncOperation"
[IAsyncInfo]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfo"
[IAsyncInfoCancel]: /uwp/api/Windows.Foundation.IAsyncInfo "IAsyncInfoCancel"
[taskCanceled]: /cpp/parallel/concrt/reference/task-canceled-class "TaskCancelled"
[task-class]: /cpp/parallel/concrt/reference/task-class#get "Classe de tâche"
[taskGet]: /cpp/parallel/concrt/reference/task-class "TaskGet"
[taskParallelism]: /cpp/parallel/concrt/task-parallelism-concurrency-runtime "Parallélisme des tâches"
[taskThen]: /cpp/parallel/concrt/reference/task-class#then "TaskThen"
[useArbitrary]: /cpp/parallel/concrt/reference/task-continuation-context-class "UseArbitrary"