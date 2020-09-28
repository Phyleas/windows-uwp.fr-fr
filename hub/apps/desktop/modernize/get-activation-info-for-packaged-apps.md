---
description: Découvrez comment obtenir certains types d’informations d’activation pour les applications .NET empaquetées et C++/Win32 natives
title: Obtenir des informations d’activation pour les applications empaquetées
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 748646ce335e9e68ee7a22131ebd305db94e9801
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90799848"
---
# <a name="get-activation-info-for-packaged-apps"></a>Obtenir des informations d’activation pour les applications empaquetées

À compter de la version 1809 de Windows 10, les applications de bureau empaquetées peuvent appeler la méthode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) pour récupérer certains types d’informations d’activation des applications lors du démarrage. Par exemple, vous pouvez appeler cette méthode pour obtenir des informations relatives à l’activation des applications concernant l’ouverture d’un fichier, un clic sur un toast interactif ou l’utilisation d’un protocole. À compter de la version 2004 de Windows 10, cette fonctionnalité est également prise en charge dans les applications qui utilisent des [packages partiellement alloués](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps).

> [!NOTE]
> En plus de récupérer certains types d’informations d’activation à l’aide de la méthode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) comme le décrit cet article, vous pouvez également récupérer les informations d’activation relatives aux tâches en arrière-plan en définissant une classe COM. Pour en savoir plus, consultez [Créer et inscrire une tâche en arrière-plan COM winmain](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task).

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment appeler la méthode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) à partir de la fonction **Main** dans une application Windows Forms. Pour chaque type d’activation pris en charge par votre application, convertissez la valeur de retour `args` en type d’argument d’événement correspondant. Dans cet exemple de code, les méthodes `Handlexxx` sont supposées être du code de gestionnaire d’activation dédié que vous avez défini ailleurs.

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>Types d’activation pris en charge

Vous pouvez utiliser la méthode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) pour récupérer les informations d’activation à partir du jeu d’objets d’arguments d’événement pris en charge répertoriés dans le tableau suivant. Certains de ces types d’activation requièrent l’utilisation d’une extension de package dans le manifeste du package.

Les informations d’activation [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) sont prises en charge uniquement sur Windows 10 version 2004 et les versions ultérieures. Tous les autres types d’informations d’activation sont pris en charge sur Windows 10 version 1809 et les versions ultérieures.

| Types d’arguments d’événement | Extension du package | Documents connexes | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [Faire de votre application de bureau une cible de partage](/windows/apps/desktop/modernize/desktop-to-uwp-extend#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [Démarrer votre application à l’aide d’un protocole](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [Notifications toast à partir d’applications de bureau](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps) |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [Démarrer un fichier exécutable lorsque les utilisateurs se connectent à Windows](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [Associer votre application empaquetée à un ensemble de types de fichiers](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | None | [Gérer l’activation et exécuter les commandes vocales](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | None |  |
