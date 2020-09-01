---
Description: Découvrez comment les applications Win32 C++ WRL peuvent envoyer des notifications Toast locales et gérer l’utilisateur en cliquant sur le Toast.
title: Envoyer une notification toast locale depuis des applications WRL de bureau en C++
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, Desktop, notifications Toast, envoyer un toast, envoyer un toast local, Desktop Bridge, msix, package fragmenté, C++, CPP, Cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e1aae390cf9047c8c93b4d24084c87bc90af8d80
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172303"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>Envoyer une notification toast locale depuis des applications WRL de bureau en C++

Les applications de bureau (y compris les applications [MSIX](/windows/msix/desktop/source-code-overview) empaquetées, les applications qui utilisent des [packages éparss](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) pour obtenir l’identité du package et les applications Win32 non empaquetées classiques) peuvent envoyer des notifications de Toast interactives comme des applications Windows. Toutefois, il existe quelques étapes spéciales pour les applications de bureau en raison des différents schémas d’activation et de l’absence potentielle d’identité de package si vous n’utilisez pas MSIX ou un package fragmenté.

> [!IMPORTANT]
> Si vous écrivez une application UWP, consultez la [documentation UWP](send-local-toast.md). Pour les autres langues du bureau, consultez [Desktop C#](send-local-toast-desktop.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Étape 1 : activer le kit de développement logiciel (SDK) Windows 10

Si vous n’avez pas activé le kit de développement logiciel (SDK) Windows 10 pour votre application Win32, vous devez d’abord le faire. Il existe quelques étapes clés...

1. Ajouter `runtimeobject.lib` aux **dépendances supplémentaires**
2. Cibler le kit de développement logiciel (SDK) Windows 10

Cliquez avec le bouton droit sur votre projet et sélectionnez **Propriétés**.

Dans le menu de **configuration** supérieur, sélectionnez **toutes les configurations** afin que la modification suivante soit appliquée à la fois au débogage et à la mise en version.

Sous **éditeur de liens-> entrée**, ajoutez `runtimeobject.lib` aux **dépendances supplémentaires**.

Ensuite, sous **général**, assurez-vous que la **version de SDK Windows** est définie sur une valeur supérieure ou égale à 10,0 (non Windows 8.1).


## <a name="step-2-copy-compat-library-code"></a>Étape 2 : copier le code de bibliothèque de compatibilité

Copiez le fichier [DesktopNotificationManagerCompat. h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) et [DesktopNotificationManagerCompat. cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) de GitHub dans votre projet. La bibliothèque de compatibilité résume la majeure partie de la complexité des notifications sur le bureau. Les instructions suivantes nécessitent la bibliothèque de compatibilité.

Si vous utilisez des en-têtes précompilés, assurez-vous de `#include "stdafx.h"` la première ligne du fichier DesktopNotificationManagerCompat. cpp.


## <a name="step-3-include-the-header-files-and-namespaces"></a>Étape 3 : inclure les espaces de noms et les fichiers d’en-tête

Incluez le fichier d’en-tête de bibliothèque de compatibilité, ainsi que les fichiers d’en-tête et les espaces de noms associés à l’utilisation des API Toast Windows.

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>Étape 4 : implémenter l’activateur

Vous devez implémenter un gestionnaire pour l’activation de Toast, de sorte que lorsque l’utilisateur clique sur votre toast, votre application peut effectuer une opération. Cela est nécessaire pour que votre toast soit conservé dans le centre de notifications (dans la mesure où le Toast peut être cliqué plus tard pendant la fermeture de votre application). Cette classe peut être placée n’importe où dans votre projet.

Implémentez l’interface **INotificationActivationCallback** comme indiqué ci-dessous, y compris un UUID, et appelez également **CoCreatableClass** pour signaler votre classe comme pouvant être créée par com. Pour votre UUID, créez un GUID unique à l’aide de l’un des nombreux générateurs GUID en ligne. Ce GUID CLSID (identificateur de classe) indique comment le centre de maintenance connaît la classe à activer par COM.

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>Étape 5 : s’inscrire auprès de la plateforme de notification

Ensuite, vous devez vous inscrire auprès de la plateforme de notification. Différentes étapes varient selon que vous utilisez des packages MSIX/épars ou Win32 classique. Si vous prenez en charge les deux, vous devez effectuer les deux étapes (Toutefois, vous n’avez pas besoin de répliquer votre code, notre bibliothèque le gère pour vous !).


### <a name="msixsparse-package"></a>Package MSIX/Sparse

Si vous utilisez [MSIX](/windows/msix/desktop/source-code-overview) ou un [package fragmenté](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (ou si vous prenez en charge les deux), dans votre **Package. appxmanifest**, ajoutez :

1. Déclaration pour **xmlns : com**
2. Déclaration pour **xmlns : Desktop**
3. Dans l’attribut **ignorablenamespaces en spécifiant** , **com** et **Desktop**
4. **com : extension** pour l’activateur com à l’aide du GUID de l’étape #4. Veillez à inclure le `Arguments="-ToastActivated"` pour que vous sachiez que votre lancement s’est bien fait d’un toast
5. **Desktop : extension** pour **Windows. toastNotificationActivation** pour déclarer le CLSID de votre activateur Toast (le GUID de l’étape #4).

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32 classique

Si vous utilisez Win32 classique (ou si vous prenez en charge les deux), vous devez déclarer l’ID de modèle utilisateur de l’application (identifiant AUMID) et le CLSID de l’activateur Toast (le GUID à partir de l’étape #4) sur le raccourci de votre application dans Démarrer.

Choisissez un identifiant AUMID unique qui identifie votre application Win32. Il se présente généralement sous la forme [CompanyName]. [AppName], mais vous voulez vous assurer qu’il est unique pour toutes les applications (n’hésitez pas à ajouter des chiffres à la fin).

#### <a name="step-51-wix-installer"></a>Étape 5,1 : programme d’installation de WiX

Si vous utilisez WiX pour votre programme d’installation, modifiez le fichier **Product. wxs** pour ajouter les deux propriétés de raccourci au raccourci du menu Démarrer, comme indiqué ci-dessous. Assurez-vous que le GUID de l’étape #4 est inséré `{}` comme indiqué ci-dessous.

**Product. wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Pour utiliser réellement les notifications, vous devez installer votre application par le biais du programme d’installation avant de procéder au débogage normalement, afin que le raccourci de démarrage avec votre identifiant AUMID et CLSID soit présent. Une fois le raccourci de démarrage présent, vous pouvez déboguer à l’aide de la touche F5 à partir de Visual Studio.


#### <a name="step-52-register-aumid-and-com-server"></a>Étape 5,2 : inscrire identifiant AUMID et le serveur COM

Ensuite, quel que soit votre programme d’installation, dans le code de démarrage de votre application (avant d’appeler les API de notification), appelez la méthode **RegisterAumidAndComServer** , en spécifiant votre classe d’activateur de notification à partir de l’étape #4 et votre identifiant aumid utilisé ci-dessus.

```cpp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

Si vous prenez en charge à la fois le package MSIX/Sparse et le Win32 classique, n’hésitez pas à appeler cette méthode, quelle que soit la valeur. Si vous utilisez un package MSIX ou Sparse, cette méthode sera simplement retournée immédiatement. Vous n’avez pas besoin de dupliquer votre code.

Cette méthode vous permet d’appeler les API de compatibilité pour envoyer et gérer des notifications sans avoir à fournir constamment vos identifiant AUMID. Elle insère la clé de registre LocalServer32 pour le serveur COM.


## <a name="step-6-register-com-activator"></a>Étape 6 : inscrire COM Activator

Pour le package MSIX/Sparse et les applications Win32 classiques, vous devez inscrire votre type d’activateur de notification, afin de pouvoir gérer les activations Toast.

Dans le code de démarrage de votre application, appelez la méthode **RegisterActivator** suivante. Cette méthode doit être appelée pour que vous puissiez recevoir des activations Toast.

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>Étape 7 : envoyer une notification

L’envoi d’une notification est identique aux applications UWP, sauf que vous allez utiliser **DesktopNotificationManagerCompat** pour créer un **ToastNotifier**. La bibliothèque de compatibilité gère automatiquement la différence entre le package MSIX/Sparse et le Win32 classique, ce qui vous permet de ne pas avoir à dupliquer votre code. Pour le Win32 classique, la bibliothèque de compatibilité met en cache votre identifiant AUMID que vous avez fournie lors de l’appel de **RegisterAumidAndComServer** afin que vous n’ayez pas à vous soucier du moment auquel fournir ou ne pas fournir le identifiant aumid.

Veillez à utiliser la liaison **ToastGeneric** comme indiqué ci-dessous, car les modèles de notification Windows 8.1 Toast hérités n’activeront pas votre activateur de notification com que vous avez créé à l’étape #4.

> [!IMPORTANT]
> Les images http sont uniquement prises en charge dans les applications de package MSIX/Sparse qui disposent de la fonctionnalité Internet dans leur manifeste. Les applications Win32 classiques ne prennent pas en charge les images http ; vous devez télécharger l’image dans vos données d’application locales et la référencer localement.

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> Les applications Win32 classiques ne peuvent pas utiliser les modèles Toast hérités (comme ToastText02). L’activation des modèles hérités échouera lorsque le CLSID COM sera spécifié. Vous devez utiliser les modèles ToastGeneric Windows 10 comme indiqué ci-dessus.


## <a name="step-8-handling-activation"></a>Étape 8 : gestion de l’activation

Quand l’utilisateur clique sur votre toast ou sur les boutons du Toast, la méthode **Activate** de votre classe **NotificationActivator** est appelée.

À l’intérieur de la méthode Activate, vous pouvez analyser les arguments que vous avez spécifiés dans le Toast et obtenir l’entrée utilisateur tapée ou sélectionnée par l’utilisateur, puis activer votre application en conséquence.

> [!NOTE]
> La méthode **Activate** est appelée sur un thread separt de votre thread principal.

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

Pour une prise en charge correcte du lancement pendant la fermeture de votre application, dans votre fonction WinMain, vous devez déterminer si vous êtes en train de lancer à partir d’un toast ou non. S’il est lancé à partir d’un toast, il y aura un ARG de lancement de « -ToastActivated ». Quand vous voyez cela, vous devez arrêter d’exécuter un code d’activation de lancement normal et autoriser votre **NotificationActivator** à gérer le lancement de Windows si nécessaire.

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>Séquence d’activation des événements

La séquence d’activation est la suivante...

Si votre application est déjà en cours d’exécution :

1. **Activer** dans votre **NotificationActivator** est appelé

Si votre application n’est pas en cours d’exécution :

1. L’exécutable de votre application est lancé, vous recevez les arguments de ligne de commande « -ToastActivated »
2. **Activer** dans votre **NotificationActivator** est appelé


### <a name="foreground-vs-background-activation"></a>Activation au premier plan et en arrière-plan
Pour les applications de bureau, le premier plan et l’activation en arrière-plan sont gérés de la même façon : votre activateur COM est appelé. Il s’agit du code de votre application pour décider s’il faut afficher une fenêtre ou simplement effectuer un travail, puis quitter. Par conséquent, la spécification d’un **activationType** de l' **arrière-plan** dans votre contenu Toast ne change pas le comportement.


## <a name="step-9-remove-and-manage-notifications"></a>Étape 9 : supprimer et gérer les notifications

La suppression et la gestion des notifications sont identiques aux applications UWP. Toutefois, nous vous recommandons d’utiliser notre bibliothèque de compatibilité pour obtenir un **DesktopNotificationHistoryCompat** . vous n’avez donc pas à vous soucier de la fourniture de identifiant aumid si vous utilisez le Win32 classique.

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>Étape 10 : déploiement et débogage

Pour déployer et déboguer votre application de package MSIX/Sparse, consultez [exécuter, déboguer et tester une application de bureau empaquetée](/windows/msix/desktop/desktop-to-uwp-debug).

Pour déployer et déboguer votre application Win32 classique, vous devez installer votre application par le biais du programme d’installation avant de déboguer normalement, afin que le raccourci de démarrage avec votre identifiant AUMID et CLSID soit présent. Une fois le raccourci de démarrage présent, vous pouvez déboguer à l’aide de la touche F5 à partir de Visual Studio.

Si vos notifications ne s’affichent pas dans votre application Win32 classique (et qu’aucune exception n’est levée), cela signifie probablement que le raccourci de démarrage n’est pas présent (installer votre application via le programme d’installation), ou que le identifiant AUMID que vous avez utilisé dans le code ne correspond pas à identifiant AUMID dans votre raccourci de début.

Si vos notifications s’affichent mais ne sont pas conservées dans le centre de maintenance (disparition après la fermeture de la fenêtre contextuelle), cela signifie que vous n’avez pas implémenté correctement l’activateur COM.

Si vous avez installé à la fois votre package MSIX/Sparse et l’application Win32 classique, Notez que l’application de package MSIX/Sparse remplace l’application Win32 classique lors du traitement des activations Toast. Cela signifie que les toasts de l’application Win32 classique continueront de lancer l’application de package MSIX/Sparse quand vous cliquez dessus. La désinstallation de l’application de package MSIX/Sparse repasse l’activation à l’application Win32 classique.

Si vous recevez `HRESULT 0x800401f0 CoInitialize has not been called.` , veillez à appeler `CoInitialize(nullptr)` dans votre application avant d’appeler les API.

Si vous recevez `HRESULT 0x8000000e A method was called at an unexpected time.` lors de l’appel des API de compatibilité, cela signifie probablement que vous n’avez pas pu appeler les méthodes de Registre requises (ou si une application de package MSIX/Sparse, vous n’exécutez actuellement pas votre application dans le contexte MSIX/Sparse).

Si vous recevez de nombreuses `unresolved external symbol` Erreurs de compilation, vous avez probablement oublié d’ajouter `runtimeobject.lib` aux **dépendances supplémentaires** à l’étape #1 (ou vous l’avez ajoutée uniquement à la configuration Debug et non à la configuration Release).


## <a name="handling-older-versions-of-windows"></a>Gestion des versions antérieures de Windows

Si vous prenez en charge la Windows 8.1 ou une valeur inférieure, vous pouvez vérifier au moment de l’exécution si vous utilisez Windows 10 avant d’appeler des API **DesktopNotificationManagerCompat** ou d’envoyer des toasts ToastGeneric.

Windows 8 a introduit les notifications Toast, mais a utilisé les [modèles Toast hérités](/previous-versions/windows/apps/hh761494(v=win.10)), tels que ToastText01. L’activation a été gérée par l’événement **activé** en mémoire sur la classe **ToastNotification** , car les toasts n’étaient que de courtes fenêtres contextuelles qui n’étaient pas conservées. Windows 10 a introduit les [toasts ToastGeneric interactifs](adaptive-interactive-toasts.md)et a également introduit le centre de maintenance dans lequel les notifications sont rendues persistantes pendant plusieurs jours. L’introduction du centre de maintenance nécessitait l’introduction d’un activateur COM, afin que votre toast puisse être activé jours après sa création.

| Système d''exploitation | ToastGeneric | Activateur COM | Modèles Toast hérités |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | Pris en charge | Pris en charge | Pris en charge (mais n’active pas le serveur COM) |
| Windows 8.1/8 | N/A | N/A | Pris en charge |
| Windows 7 et versions antérieures | N/A | N/A | N/A |

Pour vérifier si vous exécutez sur Windows 10, incluez l' `<VersionHelpers.h>` en-tête et vérifiez la méthode **IsWindows10OrGreater** . Si cette propriété retourne la valeur true, continuez à appeler toutes les méthodes décrites dans cette documentation. 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>Problèmes connus

Problème **résolu : l’application ne devient pas focalisée après avoir cliqué sur Toast**: dans les builds 15063 et antérieures, les droits de premier plan n’ont pas été transférés à votre application lorsque nous avons activé le serveur com. Par conséquent, votre application clignoterait simplement quand vous avez essayé de la déplacer au premier plan. Il n’existe aucune solution de contournement pour ce problème. Nous avons résolu ce problème dans les versions 16299 et supérieures.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notifications toast à partir d’applications de bureau](toast-desktop-apps.md)
* [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)