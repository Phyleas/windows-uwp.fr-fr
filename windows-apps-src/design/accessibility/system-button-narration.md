---
description: 'En savoir plus sur : les lecteurs d’écran et les boutons système matériel'
title: Lecteurs d’écran et événements de bouton matériel
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: Windows 10, UWP, accessibilité, narrateur, lecteur d’écran
ms.localizationpriority: medium
ms.openlocfilehash: d4bf712c83b39a184247a4476db5a045f62a9482
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029852"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>Lecteurs d’écran et boutons système matériels

Les lecteurs d’écran, tels que le [narrateur](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator), doivent être en mesure de reconnaître et de gérer les événements de bouton du système matériel et de communiquer leur État aux utilisateurs. Dans certains cas, il se peut que le lecteur d’écran doive gérer ces événements de bouton matériel exclusivement et ne pas les laisser se propager aux autres gestionnaires.

À compter de Windows 10 version 2004, les applications UWP peuvent écouter et gérer les événements de bouton de système matériel **FN** de la même façon que les autres boutons matériels. Précédemment, ce bouton système a agi uniquement en tant que modificateur pour la façon dont d’autres boutons matériels ont signalé leurs événements et leur état.

> [!NOTE]
> La prise en charge des boutons FN est spécifique à l’OEM et peut inclure des fonctionnalités telles que la possibilité d’activer ou de désactiver la fonction de basculement/verrouillage (par opposition à une combinaison de touches de pression et de pression), ainsi qu’une lumière d’indicateur de verrou correspondante (qui peut ne pas être utile pour les utilisateurs aveugles ou malvoyants).

Les événements de bouton Fn sont exposés par le biais d’une nouvelle [classe SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) dans l’espace de noms [Windows. UI. Input](/uwp/api/windows.ui.input) . L’objet SystemButtonEventController prend en charge les événements suivants :

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> Le SystemButtonEventController ne peut pas recevoir ces événements s’ils ont déjà été gérés par un gestionnaire de priorité plus élevée.

## <a name="examples"></a>Exemples

Dans les exemples suivants, nous montrons comment créer un [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) basé sur un DispatcherQueue et gérer les quatre événements pris en charge par cet objet.

Il est courant que plus d’un des événements pris en charge se déclenchent lorsque le bouton Fn est enfoncé. Par exemple, si vous appuyez sur le bouton Fn sur un clavier de surface, vous déclenchez SystemFunctionButtonPressed, SystemFunctionLockChanged et SystemFunctionLockIndicatorChanged en même temps.

1. Dans ce premier extrait, nous incluons simplement les espaces de noms requis et indiquons des objets globaux, y compris le [DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) et les objets [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller) pour la gestion du thread [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   Nous spécifions ensuite les [jetons d’événements](/uwp/cpp-ref-for-winrt/event-token) retournés lors de l’inscription des délégués de gestion d’événements [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. Nous spécifions également un jeton d’événement pour l’événement [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) avec une valeur booléenne pour indiquer si l’application est en « mode d’apprentissage » (où l’utilisateur tente simplement d’explorer le clavier sans exécuter aucune fonction).

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. Ce troisième extrait comprend les délégués de gestionnaires d’événements correspondants pour chaque événement pris en charge par l’objet [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   Chaque gestionnaire d’événements annonce l’événement qui s’est produit. En outre, le gestionnaire FunctionLockIndicatorChanged contrôle également si l’application est en mode « Learning » ( `_isLearningMode` = true), ce qui empêche l’événement de se Proligner à d’autres gestionnaires et permet à l’utilisateur d’explorer les fonctionnalités du clavier sans réellement effectuer l’action.

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>Voir aussi

[SystemButtonEventController, classe](/uwp/api/windows.ui.input.systembuttoneventcontroller)
