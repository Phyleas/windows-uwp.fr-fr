---
title: Ajouter des fonctionnalités de version de démonstration commerciale (RDX) à votre application
description: Préparez votre application pour le mode de démonstration de la vente au détail, en vous aidant à présenter votre application sur la vente au détail.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: application de démonstration Windows 10, UWP, de la version commerciale
ms.localizationpriority: medium
ms.openlocfilehash: 39f1cb7439c02f215824c6c632fb2e2fc6afdb39
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164533"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Ajouter des fonctionnalités de version de démonstration commerciale (RDX) à votre application

Incluez un mode de version de démonstration commerciale dans votre application Windows pour que les clients qui essaient des ordinateurs et des appareils sur le point de vente puissent y accéder.

Lorsque les clients se trouvent dans un magasin de vente au détail, ils s’attendent à pouvoir essayer des démonstrations de PC et d’appareils. Ils consacrent souvent beaucoup de temps à la découverte des applications grâce à l' [expérience de démonstration de la vente au détail (RDX)](/windows-hardware/customize/desktop/retail-demo-experience).

Vous pouvez configurer votre application pour fournir différentes expériences en mode _normal_ ou _au détail_ . Par exemple, si votre application commence par un processus d’installation, vous pouvez ignorer cette dernière en mode de vente au détail et préremplir l’application avec les exemples de données et les paramètres par défaut pour qu’ils puissent accéder directement à.

Du point de vue de nos clients, il n’y a qu’une seule application. Pour aider les clients à faire la distinction entre les deux modes, nous recommandons que, lorsque votre application est en mode de vente au détail, elle affiche le mot « Retail » en évidence dans la barre de titre ou dans un emplacement approprié.

Outre les exigences Microsoft Store pour les applications, les applications prenant en charge la fonction RDX doivent également être compatibles avec les processus de configuration, de nettoyage et de mise à jour de RDX pour s’assurer que les clients bénéficient d’une expérience positive constante dans le magasin de vente au détail.

## <a name="design-principles"></a>Principes de conception

* **Affichez votre meilleure**. Utilisez l’expérience de démonstration de la vente au détail pour illustrer la raison pour laquelle votre application est Rock. C’est probablement la première fois que votre client verra votre application. Affichez-la donc la meilleure pièce !

* **Affichez-le rapidement**. Les clients sont parfois impatients. Plus un utilisateur peut constater rapidement les points forts de votre application, mieux c’est.

* **Gardez l’histoire simple**. L’expérience de démonstration de la vente au détail est un pas d’ascenseur pour la valeur de votre application.

* **Concentrez-vous sur l’expérience**. Donnez à l’utilisateur le temps d’assimiler votre contenu. S’il est essentiel de les amener rapidement à découvrir les points forts, il faut également aménager des pauses pour leur permettre de profiter pleinement de l’expérience.

## <a name="technical-requirements"></a>Exigences techniques

Étant donné que les applications prenant en charge l’interface RDX sont conçues pour présenter le meilleur de votre application aux clients de détail, elles doivent répondre aux exigences techniques et respecter les réglementations en matière de confidentialité que le Microsoft Store a pour toutes les applications de démonstration de la version commerciale.

Il peut être utilisé comme liste de vérification pour vous aider à préparer le processus de validation et à fournir une clarté dans le processus de test. Notez que ces exigences doivent être satisfaites non seulement pendant le processus de validation, mais également pendant toute la durée de vie de l’application de démonstration commerciale tant qu’elle est exécutée sur les appareils de démonstration commerciale.

### <a name="critical-requirements"></a>Exigences critiques

Les applications prenant en charge les RDX qui ne répondent pas à ces exigences critiques seront supprimées de tous les appareils de démonstration de la version commerciale dès que possible.

* **Ne pas demander d’informations d’identification personnelle (PII)**. Cela comprend les informations de connexion, les informations de compte Microsoft ou les coordonnées.

* **Expérience sans erreur**. Votre application doit s’exécuter sans erreur. En outre, aucune fenêtre ou notification d’erreur ne doit s’afficher lorsque les clients utilisent les appareils de démonstration commerciale. Les erreurs se répercutent négativement sur l’application elle-même, sur la personnalisation, sur la réputation de l’appareil, sur la manufacturer’s de l’appareil et sur la réputation de Microsoft.

* Les **applications payantes doivent avoir un mode d’évaluation**. Votre application doit être gratuite ou inclure un [mode d’évaluation](./exclude-or-limit-features-in-a-trial-version-of-your-app.md). Les clients ne souhaitent pas payer pour une expérience en magasin.

### <a name="high-priority-requirements"></a>Exigences de haute priorité

Les applications prenant en charge les RDX qui ne répondent pas à ces exigences de haute priorité doivent être examinées immédiatement pour résoudre le problème. En l’absence de correctif applicable, cette application sera probablement supprimée des appareils de démonstration commerciale.

* **Expérience hors connexion mémorable**. Votre application doit présenter une expérience hors connexion exceptionnelle, car environ 50% des appareils sont hors connexion aux emplacements de vente au détail. Vous devez vous assurer que les clients qui interagissent avec votre application hors connexion vivent une expérience positive et significative.

* **Expérience du contenu mis à jour**. Votre application ne doit jamais être invité à fournir des mises à jour en ligne. Si des mises à jour sont nécessaires, elles doivent être exécutées en mode silencieux.

* **Aucune communication anonyme**. Étant donné qu’un client utilisant un appareil de démonstration de la vente au détail est un utilisateur anonyme, il ne doit pas être en mesure de messageer ou de partager du contenu à partir de l’appareil.

* **Offrez des expériences cohérentes en utilisant le processus de nettoyage**. Chaque client doit vivre la même expérience lorsqu’il utilise un appareil de démonstration commerciale. Votre application doit utiliser le [processus de nettoyage](#cleanup-process) pour revenir au même État par défaut après chaque utilisation. Nous ne voulons pas que le client suivant voit ce que le dernier client a quitté. Cela inclut les scores, les réussites et les déverrouillages.

* **Vieillit le contenu approprié**. Tout le contenu de l’application doit se voir attribuer une catégorie d’un adolescent ou d’une évaluation inférieure. Pour plus d’informations, consultez [obtention de votre application évaluée par IARC](https://www.globalratings.com/for-developers.aspx) et [ESRB ratings](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Exigences de priorité moyenne

L’équipe commerciale Windows Store peut contacter directement les développeurs pour discuter de la manière de corriger ces problèmes.

* **Possibilité de s’exécuter correctement sur une plage d’appareils**. Les applications doivent s’exécuter correctement sur tous les appareils, y compris les appareils avec des spécifications de bas de gamme. Si l’application est installée sur des appareils qui ne respectent pas les spécifications minimales, l’application doit clairement en informer l’utilisateur. La configuration minimale requise pour l’appareil doit être connue des clients afin qu’elle soit toujours exécutée avec des performances élevées.

* **Répondez aux exigences de taille des applications du magasin de vente au détail**. L’application doit être inférieure à 800 Mo. Pour plus d’informations, contactez l’équipe du Windows Retail Store, si votre application compatible RDX ne répond pas aux exigences de taille.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>API RetailInfo : préparation de votre code en mode démo

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
La propriété [**IsDemoModeEnabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) dans la classe de l’utilitaire [**RetailInfo**](/uwp/api/Windows.System.Profile.RetailInfo) , qui fait partie du [Windows.SysTEM. ](/uwp/api/windows.system.profile) Espace de noms de profil dans le kit de développement logiciel (SDK) Windows 10, est utilisé comme indicateur booléen pour spécifier le chemin de code du code sur lequel votre application s’exécute, le mode _normal_ ou le mode de _vente au détail_ .

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo. Properties

Quand [**IsDemoModeEnabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) retourne la valeur true, vous pouvez interroger un ensemble de propriétés sur l’appareil à l’aide de [**RetailInfo. Properties**](/uwp/api/windows.system.profile.retailinfo.properties) pour créer une expérience de démonstration de la vente au détail plus personnalisée. Ces propriétés incluent [**ManufacturerName**](/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](/uwp/api/windows.system.profile.knownretailinfoproperties.memory) et ainsi de suite.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>MIDL

```cpp
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>Processus de nettoyage

Le nettoyage commence deux minutes après qu’un client a cessé d’interagir avec l’appareil. La démonstration de la vente au détail est jouée et Windows commence à réinitialiser les exemples de données dans les contacts, les photos et d’autres applications. En fonction de l’appareil, la réinitialisation de tous les éléments en mode normal peut prendre entre 1-5 minutes. Cela permet de s’assurer que chaque client dans le magasin de vente au détail peut aller jusqu’à un appareil et avoir la même expérience lors de l’interaction avec l’appareil.

Étape 1 : nettoyage
* Toutes les applications Win32 et du Windows Store sont fermées
* Tous les fichiers des dossiers connus comme __Images__, __Vidéos__, __Musique__, __Documents__, __Photos enregistrées__, __Pellicule__, __Bureau__ et __Téléchargements__ sont supprimés.
* Les états d’itinérance non structurés et structurés sont supprimés
* Les états locaux structurés sont supprimés

Étape 2 : installation
* Pour les appareils hors connexion : les dossiers restent vides
* Pour les appareils en ligne : les ressources de démonstration de la vente au détail peuvent être transmises à l’appareil à partir de la Microsoft Store

### <a name="store-data-across-user-sessions"></a>Stocker des données entre les sessions utilisateur

Pour stocker des données entre les sessions utilisateur, vous pouvez stocker des informations dans __ApplicationData. Current. temporaryFolder__ , car le processus de nettoyage par défaut ne supprime pas automatiquement les données de ce dossier. Notez que les informations stockées à l’aide de *LocalState* sont supprimées pendant le processus de nettoyage.

### <a name="customize-the-cleanup-process"></a>Personnaliser le processus de nettoyage

Pour personnaliser le processus de nettoyage, implémentez `Microsoft-RetailDemo-Cleanup` app service dans votre application.

Les scénarios dans lesquels une logique de nettoyage personnalisée est nécessaire incluent l’exécution d’une installation complète, le téléchargement et la mise en cache de données, ou la suppression des données *LocalState* .

Étape 1 : déclarer le service _Microsoft-RetailDemo-Cleanup_ dans le manifeste de votre application.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Étape 2 : implémenter votre logique de nettoyage personnalisée sous la fonction de cas _AppdataCleanup_ à l’aide de l’exemple de modèle ci-dessous.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Liens connexes

* [Stocker et récupérer des données d’application](../design/app-settings/store-and-retrieve-app-data.md)
* [Comment créer et utiliser un service d’application](../launch-resume/how-to-create-and-consume-an-app-service.md)
* [Localiser les contenus d’une application](../design/globalizing/globalizing-portal.md)
* [Expérience de démonstration de la vente au détail (RDX)](/windows-hardware/customize/desktop/retail-demo-experience)