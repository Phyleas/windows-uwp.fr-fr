---
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: Créer une application de carte à puce NFC
description: Auparavant, Windows Phone 8.1 prenait en charge les applications d’émulation de carte NFC à l’aide d’un élément sécurisé sur carte SIM, mais ce modèle nécessitait le couplage fort d’applications de paiement sécurisé avec les opérateurs de réseau mobile.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4726f2169750cf69bba91f2494c0d01e609d6715
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339707"
---
# <a name="create-an-nfc-smart-card-app"></a>Créer une application de carte à puce NFC

> [!Important]
> Cette rubrique s’applique uniquement à Windows 10 mobile.

Auparavant, Windows Phone 8.1 prenait en charge les applications d’émulation de carte NFC à l’aide d’un élément sécurisé sur carte SIM, mais ce modèle nécessitait le couplage fort d’applications de paiement sécurisé avec les opérateurs de réseau mobile. Cette configuration éliminait de facto le recours aux solutions de paiement proposées par d’autres négociants ou développeurs ne présentant aucun couplage avec les opérateurs de réseau mobile. Dans Windows 10 Mobile, nous avons introduit une nouvelle technologie d’émulation de carte, appelée HCE (Host Card Emulation, émulation de carte hôte). Grâce à la technologie HCE, votre application peut directement interagir avec un lecteur de cartes NFC. Cette rubrique illustre le fonctionnement de la technologie HCE sur les appareils Windows 10 Mobile et vous explique comment développer une application HCE permettant à vos clients d’accéder à vos services sur leur téléphone, plutôt que via une carte physique, sans aucune collaboration avec un opérateur de réseau mobile.

## <a name="what-you-need-to-develop-an-hce-app"></a>Éléments nécessaires au développement d’une application HCE

Pour développer une application d’émulation de carte HCE pour Windows 10 Mobile, vous devez obtenir votre configuration d’environnement de développement. Pour ce faire, vous pouvez installer Microsoft Visual Studio 2015, qui comprend les outils des développeurs Windows et l’émulateur Windows 10 Mobile prenant en charge l’émulation NFC. Pour plus d’informations sur la configuration, voir [Se préparer](/windows/apps/get-started/get-set-up).

Si vous le souhaitez, il est possible de tester un véritable appareil Windows 10 Mobile sans recourir à l’émulateur Windows 10 Mobile. Pour ce faire, vous avez besoin des éléments suivants :

- un appareil Windows 10 Mobile avec prise en charge NFC HCE. Actuellement, les appareils Lumia 730, 830, 640 et le modèle 640 XL disposent du matériel prenant en charge les applications NFC HCE ;
- un terminal de lecteur prenant en charge les protocoles ISO/IEC 14443-4 et ISO/IEC 7816-4.

Windows 10 Mobile implémente un service HCE qui offre les fonctionnalités suivantes.

- Les applications peuvent inscrire les identificateurs d’applet (AID) associés aux cartes devant faire l’objet d’une émulation.
- La résolution des conflits et le routage de la commande et de la réponse APDU (Application Protocol Data Unit) sont couplés à l’une des applications inscrites, en fonction de la carte de lecteur externe choisie et de la préférence utilisateur.
- Gestion des événements et des notifications sur les applications, suite aux actions de l’utilisateur.

Windows 10 prend en charge l’émulation de cartes à puce utilisant le protocole ISO-DEP (ISO-IEC 14443-4) et communique via les commandes APDU définies dans la spécification ISO-IEC 7816-4. Windows 10 prend en charge la technologie ISO/IEC 14443-4 de type A pour les applications HCE. Les technologies de type B, de type F et non ISO-DEP (comme MIFARE) sont routées vers la carte SIM par défaut.

Seuls les appareils Windows 10 Mobile sont activés avec la fonction d’émulation de carte. L’émulation de carte HCE et SIM n’est pas disponible sur les autres versions de Windows 10.

L’architecture de la prise en charge de l’émulation de cartes HCE et SIM est indiquée dans le diagramme ci-dessous.

![Architecture pour l’émulation de cartes HCE et SIM](./images/nfc-architecture.png)

## <a name="app-selection-and-aid-routing"></a>Sélection des applications et routage des identificateurs d’applet

Pour développer une application HCE, vous devez comprendre comment les appareils Windows 10 Mobile dirigent les identificateurs d’applet vers une application spécifique, dans la mesure où les utilisateurs peuvent installer différentes applications HCE. Chaque application peut inscrire plusieurs cartes HCE et SIM. Les applications Windows Phone 8.1 héritées qui sont basées sur la carte SIM continueront à fonctionner sur Windows 10 Mobile, tant que l’utilisateur sélectionne l’option Carte SIM en tant que carte de paiement par défaut dans le menu des paramètres NFC. Il s’agit de la configuration établie par défaut lors du premier allumage de l’appareil.

Lorsque l’utilisateur appuie vers un terminal sur son appareil Windows 10 Mobile, les données sont automatiquement routées vers l’application appropriée installée sur l’appareil. Ce routage est basé sur les identificateurs d’applet (AID), qui sont des identificateurs de 5-16 octets. Lors d’un appui, le terminal externe transmet une commande SELECT APDU afin de spécifier l’identificateur d’applet vers lequel diriger l’ensemble des commandes APDU suivantes. Les commandes SELECT suivantes modifieront de nouveau le routage. En fonction des identificateurs d’applet inscrits par les applications et des paramètres utilisateurs, le trafic APDU est dirigé vers une application spécifique, qui renvoie une réponse APDU. N’oubliez pas qu’un terminal peut tenter de communiquer avec plusieurs applications durant le même appui. Ainsi, vous devez vous assurer que la tâche en arrière-plan de votre application se ferme le plus rapidement possible lorsqu’elle est désactivée, afin de libérer de l’espace pour une autre tâche en arrière-plan d’application, utilisée pour répondre à la commande APDU. Les tâches en arrière-plan seront évoquées ultérieurement dans cette rubrique.

Les applications HCE doivent s’inscrire avec les identificateurs d’applet spécifiques pris en charge, afin de configurer la réception des commandes APDU associées. Les applications utilisent des groupes d’identificateurs d’applet pour communiquer les identificateurs d’applet traités. D’un point de vue conceptuel, un groupe d’identificateurs d’applet équivaut à une carte physique individuelle. Par exemple, une carte de crédit est communiquée avec un groupe donné d’identificateurs d’application et une seconde carte de crédit d’une banque différente est communiquée avec un groupe différent d’identificateurs d’applet, même si les deux peuvent prendre en charge des identificateurs d’applet identiques.

## <a name="conflict-resolution-for-payment-aid-groups"></a>Résolution de conflits pour les groupes d’identificateurs d’applet de paiement

Lorsqu’une application inscrit des cartes physiques (groupes d’identificateurs d’applet), elle peut communiquer les catégories « Paiement » ou « Autre » de groupe d’identificateurs d’applet. Si plusieurs groupes d’identificateurs d’applet de paiement peuvent être inscrits simultanément, seul l’un d’entre eux peut être pris en charge à la fois pour le paiement au toucher. Cette sélection est effectuée par l’utilisateur. Ce comportement permet à l’utilisateur de disposer d’un contrôle total sur son choix. Qu’il s’agisse d’une carte de paiement unique ou d’une carte de crédit, il est assuré de ne pas payer à l’aide d’une autre carte choisie aléatoirement lorsqu’il sélectionne un terminal au toucher sur son appareil.

Cependant, plusieurs groupes d’identificateurs d’applet inscrits dans la catégorie « Autre » peuvent être activés simultanément sans aucune interaction de l’utilisateur. Ce comportement existe, car d’autres types de cartes comme des cartes de fidélité, des bons de réduction ou des cartes de transport public sont censés fonctionner d’office, sans autre action ou invite après la pression sur le téléphone.

L’ensemble des groupes d’identificateurs d’applet qui sont inscrits dans la catégorie « Paiement » apparaissent dans la liste des cartes de la page des paramètres NFC, sur laquelle l’utilisateur peut sélectionner sa carte de paiement par défaut. Lorsqu’une carte de paiement par défaut est sélectionnée, l’application ayant inscrit ce groupe d’identificateurs d’applet de paiement devient l’application de paiement par défaut. Les applications de paiement par défaut peuvent activer ou désactiver les groupes d’identificateurs d’applet de paiement pris en charge sans aucune interaction utilisateur. Si l’utilisateur décline l’invite de l’application de paiement par défaut, l’application de paiement par défaut actuellement définie (le cas échéant) demeure la valeur utilisée par défaut. La capture suivante représente la page des paramètres NFC.

![Capture d’écran de la page des paramètres NFC](./images/nfc-settings.png)

À l’aide de l’exemple de capture ci-dessus, si l’utilisateur remplace sa carte de paiement par défaut par une autre carte qui n’est pas inscrite par « HCE Application 1 », le système crée une invite de confirmation destinée à l’utilisateur. Toutefois, si l’utilisateur remplace sa carte de paiement par défaut par une autre carte inscrite par « HCE Application 1 », le système ne crée aucune invite de confirmation, car « HCE Application 1 » est déjà l’application de paiement par défaut.

## <a name="conflict-resolution-for-non-payment-aid-groups"></a>Résolution de conflits pour les groupes d’identificateurs d’applet sans paiement

Les cartes sans paiement de la catégorie « Autre » n’apparaissent pas dans la page des paramètres NFC.

Votre application peut créer, inscrire et activer des groupes d’identificateurs d’applet sans paiement sans rien changer de la procédure utilisée pour les groupes d’identificateurs d’applet de paiement. Toutefois, et il s’agit ici de la principale différence, la catégorie d’émulation des groupes d’identificateurs d’applet sans paiement est configurée sur « Autre », non sur « Paiement ». Après avoir inscrit le groupe d’identificateurs d’applet auprès du système, vous devez activer la réception du trafic NFC sur le groupe. Lorsque vous essayez d’activer la réception du trafic sur un groupe d’identificateurs d’applet sans paiement, aucune invite de confirmation n’est transmise à l’utilisateur, sauf s’il existe un conflit avec l’un des identificateurs d’applet déjà inscrit dans le système par une application différente. Si un conflit est identifié, un message est transmis à l’utilisateur, afin de lui communiquer des informations sur la carte et l’application associée qui seront désactivées si l’utilisateur décide d’activer le groupe d’identificateurs d’applet nouvellement inscrit.

### <a name="coexistence-with-sim-based-nfc-applications"></a>Coexistence avec des applications NFC basées sur SIM

Dans Windows 10 Mobile, le système configure la table de routage du contrôleur NFC qui est utilisée pour prendre les décisions de routage sur la couche du contrôleur. La table contient les informations de routage pour les éléments suivants.

- Itinéraires des identificateurs d’applet.
- Itinéraire de protocole (ISO-DEP).
- Itinéraire technologique (NFC-A/B/F).

Lorsqu’un lecteur externe envoie une commande « SELECT AID », le contrôleur NFC recherche une correspondance d’itinéraire d’identificateur d’applet dans la table de routage. Si aucune correspondance n’est identifiée, le contrôleur utilise l’itinéraire de protocole en tant qu’itinéraire par défaut pour le trafic ISO-DEP (14443-4-A). Pour tous les autres trafics non ISO-DEP, il utilise le routage technologique.

Dans la page des paramètres NFC, Windows 10 Mobile offre une option de menu « Carte SIM » qui permet de continuer à utiliser les applications héritées sur carte SIM de Windows Phone 8.1, qui n’inscrivent pas leurs identificateurs d’applet auprès du système. Si l’utilisateur sélectionne « Carte SIM » en tant que carte de paiement par défaut, l’itinéraire ISO-DEP est défini sur UICC. Pour toutes les autres sélections du menu déroulant, l’itinéraire ISO-DEP est défini sur l’hôte.

L’itinéraire ISO-DEP est défini sur « Carte SIM » pour les appareils qui présentent une carte SIM compatible SE lorsque l’appareil est démarré pour la première fois avec Windows 10 Mobile. Lorsque l’utilisateur installe une application HCE et que cette application active des inscriptions de groupes d’identificateurs d’applet HCE, l’itinéraire ISO-DEP est dirigé vers l’hôte. Les nouvelles applications sur cartes SIM doivent inscrire les identificateurs d’applet sur la carte SIM afin de renseigner les itinéraires d’identificateur d’applet dans la table de routage du contrôleur.

## <a name="creating-an-hce-based-app"></a>Création d’une application HCE

Votre application HCE est composée de deux parties :

- la principale application au premier plan, dédiée à l’interaction utilisateur ;
- une tâche en arrière-plan déclenchée par le système pour traiter les commandes APDU associées à un identificateur d’applet donné.

En raison des exigences extrêmement strictes de performances associées au chargement de votre tâche en arrière-plan en réponse à une pression NFC, nous vous recommandons d’implémenter l’intégralité de votre tâche en arrière-plan en code natif C++/CX (y compris les dépendances, les références ou les bibliothèques dont vous dépendez), plutôt qu’en C# ou en code géré. Si C# et le code géré fonctionnent habituellement correctement, ils sont associés à une surcharge de temps évitable. Par exemple, le chargement de .NET CLR est absent avec l’écriture en C++/CX.

## <a name="create-and-register-your-background-task"></a>Créer et inscrire votre tâche en arrière-plan

Vous devez créer une tâche en arrière-plan dans votre application HCE, afin de traiter les commandes APDU émise par le système, et d’y répondre. Lors du premier lancement de votre application, le premier plan enregistre une tâche en arrière-plan HCE qui implémente l’interface [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) , comme indiqué dans le code suivant.

```cppcx
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

Notez que le déclencheur de tâche est défini sur [**SmartCardTriggerType**](/uwp/api/Windows.Devices.SmartCards.SmartCardTriggerType). **EmulatorHostApplicationActivated**. Ainsi, chaque fois qu’une commande SELECT AID APDU est reçue par le système d’exploitation résolu sur votre application, votre tâche en arrière-plan est lancée.

## <a name="receive-and-respond-to-apdus"></a>Recevoir les commandes APDU et y répondre

Quand une commande APDU est dirigée vers votre application, le système lance votre tâche en arrière-plan. Votre tâche en arrière-plan reçoit la valeur APDU transmise via la propriété [**CommandApdu**](/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu) de l’objet [**SmartCardEmulatorApduReceivedEventArgs**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorApduReceivedEventArgs) et répond au APDU à l’aide de la méthode [**TryRespondAsync**](/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.tryrespondwithcryptogramsasync) du même objet. Pour des raisons de performance, songez à conserver votre tâche en arrière-plan pour les opérations légères. Par exemple, répondez immédiatement aux commandes APDU et fermez votre tâche en arrière-plan à l’issue du traitement. En raison de la nature des transactions NFC, les utilisateurs ont tendance à maintenir leur appareil contre le lecteur uniquement sur une courte période de temps. Votre tâche en arrière-plan continuera à recevoir le trafic à partir du lecteur jusqu’à ce que votre connexion soit désactivée. dans ce cas, vous recevrez un objet [**SmartCardEmulatorConnectionDeactivatedEventArgs**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorConnectionDeactivatedEventArgs) . Votre connexion peut être désactivée pour les raisons suivantes, comme indiqué dans la propriété [**SmartCardEmulatorConnectionDeactivatedEventArgs. Reason**](/uwp/api/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason) .

- Si la connexion est désactivée avec la valeur **ConnectionLost** , cela signifie que l’utilisateur a retiré son appareil du lecteur. Si votre application nécessite que l’utilisateur appuie plus longuement sur le terminal, vous pouvez envisager de lui transmettre des commentaires. Vous devez arrêter rapidement votre tâche en arrière-plan (en exécutant votre report), ceci pour garantir qu’aucun délai associé à la fermeture de la tâche en arrière-plan précédente ne sera respecté en cas de nouvel appui de l’utilisateur.
- Si la connexion est désactivée avec l’élément **ConnectionRedirected** , cela signifie que le terminal a dirigé une nouvelle commande SELECT AID APDU vers un identificateur d’applet différent. Dans ce cas, votre application doit fermer immédiatement la tâche en arrière-plan (en exécutant votre report) afin de permettre l’exécution d’une autre tâche en arrière-plan.

La tâche en arrière-plan doit également s’inscrire à l' [**événement Canceled**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) sur l' [**interface IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)et quitter rapidement la tâche en arrière-plan (en terminant votre différé), car cet événement est déclenché par le système lorsqu’il est terminé avec votre tâche en arrière-plan. Voici le code associé à une tâche en arrière-plan d’une application HCE.

```cppcx
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"**_ HCE Activation Background Task Started _*_");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```

## <a name="create-and-register-aid-groups"></a>Créer et inscrire des groupes d’identificateurs d’applet

Durant la mise en service de la carte, intervenant lors du premier lancement de votre application, vous créez des groupes d’identificateurs d’applet, que vous inscrivez auprès du système. Le système identifie l’application possédant un lecteur système avec laquelle interagir et dirige en conséquence les commandes ADPU en fonction des identificateurs d’applet inscrits et des paramètres utilisateur.

La plupart des cartes de paiement sont inscrites auprès des mêmes identificateurs d’applet (PPSE) et identificateurs d’applet supplémentaires de carte réseau de paiement. Chaque groupe d’identificateurs d’applet représente une carte. Lorsque l’utilisateur active la carte, l’ensemble des identificateurs d’applet du groupe sont activés. De la même manière, lorsque l’utilisateur désactive la carte, l’ensemble des identificateurs d’applet du groupe sont désactivés.

Pour inscrire un groupe d’aides, vous devez créer un objet [_ *SmartCardAppletIdGroup* *](/uwp/api/Windows.Devices.SmartCards.SmartCardAppletIdGroup) et définir ses propriétés de façon à ce qu’il s’agit d’une carte de paiement basée sur HCE. Votre nom complet doit être descriptif pour l’utilisateur, car il apparaît dans le menu des paramètres NFC, ainsi que dans les invites utilisateur. Pour les cartes de paiement HCE, la propriété [**SmartCardEmulationCategory**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) doit être définie sur **Payment** et la propriété [**SmartCardEmulationType**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) doit être définie sur **Host**.

```cppcx
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

Pour les cartes HCE sans paiement, la propriété [**SmartCardEmulationCategory**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) doit être définie sur **Other** et la propriété [**SmartCardEmulationType**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) doit être définie sur **Host**.

```cppcx
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

Vous pouvez inclure jusqu’à 9 identificateurs d’applet (d’une longueur comprise entre 5 et 16 octets) par groupe d’identificateurs d’applet.

Utilisez la méthode [**RegisterAppletIdGroupAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.registerappletidgroupasync) pour inscrire votre groupe d’aide auprès du système, qui retourne un objet [**SmartCardAppletIdGroupRegistration**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) . Par défaut, la propriété [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de l’objet d’inscription est définie sur **Disabled**. Cela signifie que même si vos identificateurs d’applet sont inscrits auprès du système, ils ne sont pas encore activés et ne reçoivent aucun trafic.

```cppcx
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

Vous pouvez activer vos cartes inscrites (groupes d’aides) à l’aide de la méthode [**RequestActivationPolicyChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de la classe [**SmartCardAppletIdGroupRegistration**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) , comme indiqué ci-dessous. Dans la mesure où une seule carte de paiement peut être activée à la fois sur le système, la définition de l’élément [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) d’un groupe d’identificateurs d’applet de carte de paiement sur **Enabled** équivaut à la définition de la carte de paiement par défaut. L’utilisateur est invité à autoriser cette carte en tant que carte de paiement par défaut, indépendamment du fait qu’une carte de paiement par défaut soit déjà sélectionnée ou non. Cette allégation n’est pas vraie si votre application est déjà l’application de paiement par défaut, et qu’elle est modifiée simplement entre ses propres groupes d’identificateurs d’applet. Vous pouvez enregistrer jusqu’à 10 groupes d’identificateurs d’applet par application.

```cppcx
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

Vous pouvez interroger les groupes d’identificateurs d’applet inscrits de votre application avec le système d’exploitation et contrôler leur stratégie d’activation à l’aide de la méthode [**GetAppletIdGroupRegistrationsAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.getappletidgroupregistrationsasync).

Une invite est transmise aux utilisateurs lorsque vous modifiez la stratégie d’activation d’une carte de paiement de **Disabled** en **Enabled** , seulement si votre application n’est pas déjà l’application de paiement par défaut. Une invite est transmise aux utilisateurs uniquement lorsque vous modifiez la stratégie d’activation d’une carte sans paiement de **Disabled** en **Enabled** , en cas de conflit d’identificateur d’applet.

```cppcx
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

### <a name="event-notification-when-activation-policy-change"></a>Notification d’événement en cas de modification de la stratégie d’activation

Dans votre tâche en arrière-plan, vous pouvez configurer la réception des événements intervenant lorsque la stratégie d’activation de l’une de vos inscriptions de groupes d’identificateurs d’applet est modifiée en dehors de votre application. Par exemple, l’utilisateur peut modifier l’application de paiement par défaut via le menu des paramètres NFC, en remplaçant l’une de vos cartes par une autre carte hébergée par une autre application. S’il est nécessaire de signaler cette modification à votre application pour des raisons de configuration interne, comme pour la modification de vignettes dynamiques, vous pouvez recevoir des notifications d’événements associés et agir en conséquence dans votre application.

```cppcx
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## <a name="foreground-override-behavior"></a>Comportement de remplacement au premier plan

Vous pouvez remplacer l’élément [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de l’une de vos inscriptions de groupes d’identificateurs d’applet par **ForegroundOverride** lorsque votre application est au premier plan, sans inviter l’utilisateur. Lorsque l’utilisateur appuie vers un terminal sur son appareil lorsque l’application est au premier plan, le trafic est dirigé vers votre application, même si aucune de vos cartes de paiement n’a été choisie par l’utilisateur en tant que carte de paiement par défaut. Lorsque vous modifiez une stratégie d’activation de carte en **ForegroundOverride** , cette modification temporaire est conservée jusqu’à ce que votre application quitte le premier plan. Elle ne modifie en rien la carte de paiement par défaut actuellement définie par l’utilisateur. Pour modifier l’élément **ActivationPolicy** de vos cartes (avec ou sans paiement) à partir de votre application de premier plan, procédez comme suit : Notez que la méthode [**RequestActivationPolicyChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) peut être appelée uniquement à partir d’une application de premier plan, et pas d’une tâche en arrière-plan.

```cppcx
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

En outre, vous pouvez inscrire un groupe d’identificateurs d’applet composé d’un seul identificateur de longueur nulle. Dans cette configuration, le système dirige l’ensemble des commandes APDU indépendamment de l’identificateur, y compris les commandes transmises avant la réception d’une commande SELECT AID. Toutefois, un groupe d’identificateurs de ce type fonctionne uniquement lorsque votre application est au premier plan, car il peut être uniquement défini sur **ForegroundOverride** , et ne peut pas être activé en permanence. Par ailleurs, ce mécanisme fonctionne à la fois pour les valeurs **Host** et **UICC** de l’énumération [**SmartCardEmulationType**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType), afin de diriger l’ensemble du trafic vers votre tâche en arrière-plan HCE ou vers la carte SIM.

```cppcx
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## <a name="check-for-nfc-and-hce-support"></a>Vérifier les prises en charge NFC et HCE

Votre application doit vérifier si un appareil présente le matériel NFC, prend en charge la fonction d’émulation de carte et l’émulation de carte hôte afin de proposer de telles fonctionnalités à l’utilisateur.

La fonction d’émulation de carte à puce NFC est activée uniquement sur Windows 10 Mobile. En conséquence, vous provoquerez des erreurs si vous tentez d’utiliser les API d’émulateur de carte à puce dans d’autres versions de Windows 10. Pour vérifier la prise en charge de l’API de carte à puce, consultez l’extrait de code suivant.

```cppcx
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

Vous pouvez également vérifier si l’appareil possède un matériel NFC capable d’effectuer une certaine forme d’émulation de carte en vérifiant si la méthode [**SmartCardEmulator. GetDefaultAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.getdefaultasync) retourne la valeur null. Si tel est le cas, l’appareil ne prend en charge aucune émulation de carte NFC.

```cppcx
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

La prise en charge du routage UICC AID et HCE est disponible uniquement sur les appareils lancés récemment, comme les modèles Lumia 730, 830, 640, et 640 XL. Tous les nouveaux appareils compatibles NFC exécutant Windows 10 Mobile et les versions ultérieures doivent prendre en charge HCE. Pour vérifier la prise en charge HCE, votre application peut procéder de la façon suivante.

```cppcx
Smartcardemulator.IsHostCardEmulationSupported();
```

## <a name="lock-screen-and-screen-off-behavior"></a>Comportement de l’écran de verrouillage et de l’écran éteint

Windows 10 Mobile dispose de paramètres d’émulation de carte de niveau appareil, qui peuvent être définis par l’opérateur mobile ou le fabricant de l’appareil. Par défaut, le commutateur Toucher pour payer est désactivé et la stratégie d’activation au niveau appareil est définie sur Toujours, sauf si l’opérateur mobile ou le fabricant remplacent ces valeurs.

Votre application peut interroger la valeur du [**EnablementPolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorEnablementPolicy) au niveau de l’appareil et prendre des mesures pour chaque cas, en fonction du comportement souhaité de votre application dans chaque État.

```cppcx
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;

 case Always:
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked";
}
```

Votre tâche en arrière-plan d’application est lancée, même si le téléphone est verrouillé et/ou si l’écran est éteint, uniquement si le lecteur externe sélectionne un identificateur d’applet qui se résout sur votre application. Vous pouvez répondre aux commandes du lecteur dans votre tâche en arrière-plan. Cependant, si vous sollicitez une saisie de l’utilisateur ou si vous souhaitez afficher un message à l’utilisateur, vous pouvez lancer votre application de premier plan avec quelques arguments. Votre tâche en arrière-plan peut lancer votre application de premier plan des manières suivantes :

- sous l’écran de verrouillage de l’appareil (l’utilisateur aura accès à votre application de premier plan uniquement après le déverrouillage de l’appareil) ;
- au-dessus de l’écran de verrouillage de l’appareil (après la fermeture de votre application par l’utilisateur, l’appareil est toujours verrouillé).

```cppcx
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        }
```

## <a name="aid-registration-and-other-updates-for-sim-based-apps"></a>Inscription d’identificateur d’applet et autres mises à jour pour les applications sur carte SIM

Les applications d’émulation de cartes qui utilisent la carte SIM en tant qu’élément sécurisé peut s’inscrire auprès du service Windows afin de communiquer les identificateurs d’applet pris en charge sur la carte SIM. Cette inscription est très similaire à l’inscription d’application HCE. La seule différence est [**SmartCardEmulationType**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType), qui doit être défini sur UICC pour les applications SIM. Suite à l’inscription de la carte de paiement, le nom d’affichage de celle-ci apparaît également dans le menu des paramètres NFC.

```cppcx
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

> [!IMPORTANT]
> La prise en charge héritée de l’interception des SMS binaires de Windows Phone 8.1 a été supprimée et remplacée par une nouvelle prise en charge élargie des SMS dans Windows 10 Mobile, mais l’ensemble des applications Windows Phone 8.1 héritées utilisant cette fonctionnalité doivent être mises à jour pour prendre en charge les nouvelles API SMS Windows 10 Mobile.