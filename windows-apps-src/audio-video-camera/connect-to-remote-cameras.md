---
ms.assetid: ''
description: Cet article vous montre comment vous connecter à des caméras distantes et comment obtenir un MediaFrameSourceGroup pour récupérer des images à partir de chaque caméra.
title: Se connecter à des caméras à distance
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fae76aea28ffb63f6cb0ad5af8c5eb6e3fac6e4
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494365"
---
# <a name="connect-to-remote-cameras"></a>Se connecter à des caméras à distance

Cet article vous montre comment vous connecter à une ou plusieurs caméras distantes et comment obtenir un objet [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) qui vous permet de lire des images à partir de chaque caméra. Pour plus d’informations sur la lecture des frames à partir d’une source multimédia, consultez [traiter des frames multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md). Pour plus d’informations sur le couplage avec des appareils, consultez [coupler des appareils](../devices-sensors/pair-devices.md).

> [!NOTE] 
> Les fonctionnalités présentées dans cet article sont disponibles à partir de Windows 10, version 1903.

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>Créer une classe DeviceWatcher pour surveiller les caméras distantes disponibles

La classe [**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher) surveille les périphériques disponibles pour votre application et informe votre application de l’ajout ou de la suppression d’appareils. Pour obtenir une instance de **DeviceWatcher** , appelez [**DeviceInformation. CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_), en passant une chaîne de syntaxe de requête avancée (AQS) qui identifie le type de périphériques que vous souhaitez analyser. La chaîne AQS spécifiant les périphériques de caméra réseau est la suivante :

```syntax
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> La méthode d’assistance [**MediaFrameSourceGroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) retourne une chaîne AQS qui surveille les caméras réseau distantes et connectées localement. Pour surveiller uniquement les caméras réseau, vous devez utiliser la chaîne AQS indiquée ci-dessus.

Quand vous démarrez le **DeviceWatcher** retourné en appelant la méthode [**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) , il déclenche l’événement [**added**](/uwp/api/windows.devices.enumeration.devicewatcher.added) pour chaque caméra réseau actuellement disponible. Tant que vous n’avez pas arrêté l’observateur en appelant [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop), l’événement **added** est déclenché quand de nouveaux périphériques de caméra réseau sont disponibles et l’événement [**removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) est déclenché quand un appareil photo n’est plus disponible.

Les arguments d’événement passés dans les gestionnaires d’événements **added** et **removed** sont un objet [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) ou [**DeviceInformationUpdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) , respectivement. Chacun de ces objets a une propriété **ID** qui est l’identificateur de la caméra réseau pour laquelle l’événement a été déclenché. Transmettez cet ID dans la méthode [**MediaFrameSourceGroup. FromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) pour obtenir un objet [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) que vous pouvez utiliser pour récupérer des frames à partir de l’appareil photo.

## <a name="remote-camera-pairing-helper-class"></a>Classe d’assistance de l’appariement de la caméra distante

L’exemple suivant montre une classe d’assistance qui utilise un **DeviceWatcher** pour créer et mettre à jour un **ObservableCollection** d’objets **MediaFrameSourceGroup** pour prendre en charge la liaison de données à la liste des caméras. Les applications typiques encapsulent le **MediaFrameSourceGroup** dans une classe de modèle personnalisée. Notez que la classe d’assistance gère une référence à l' [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) de l’application et met à jour la collection de caméras dans les appels à [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) pour s’assurer que l’interface utilisateur liée à la collection est mise à jour sur le thread d’interface utilisateur.

En outre, cet exemple gère l’événement [**DeviceWatcher. Updated**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) en plus des événements **added** et **removed** . Dans le gestionnaire **mis à jour** , l’appareil photo distant associé est supprimé de, puis rajouté à la collection.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::

```cppwinrt
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Media.Capture.Frames.h>
#include <winrt/Windows.UI.Core.h>
using namespace winrt;
using namespace winrt::Windows::Devices::Enumeration;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::Media::Capture::Frames;
using namespace winrt::Windows::UI::Core;

struct RemoteCameraPairingHelper
{
    RemoteCameraPairingHelper(CoreDispatcher uiDispatcher) :
        m_dispatcher(uiDispatcher)
    {
        m_remoteCameraCollection = winrt::single_threaded_observable_vector<MediaFrameSourceGroup>();
        auto remoteCameraAqs =
            LR"(System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"")"
            LR"(AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True)";
        m_watcher = DeviceInformation::CreateWatcher(remoteCameraAqs);
        m_watcherAddedAutoRevoker = m_watcher.Added(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Added });
        m_watcherRemovedAutoRevoker = m_watcher.Removed(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Removed });
        m_watcherUpdatedAutoRevoker = m_watcher.Updated(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Updated });
        m_watcher.Start();
    }
    ~RemoteCameraPairingHelper()
    {
        m_watcher.Stop();
    }
    IObservableVector<MediaFrameSourceGroup> FrameSourceGroups()
    {
        return m_remoteCameraCollection;
    }
    winrt::fire_and_forget Watcher_Added(DeviceWatcher /* sender */, DeviceInformation args)
    {
        co_await AddDeviceAsync(args.Id());
    }
    winrt::fire_and_forget Watcher_Removed(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
    }
    winrt::fire_and_forget Watcher_Updated(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
        co_await AddDeviceAsync(args.Id());
    }
    Windows::Foundation::IAsyncAction AddDeviceAsync(winrt::hstring id)
    {
        auto group = co_await MediaFrameSourceGroup::FromIdAsync(id);
        if (group)
        {
            co_await m_dispatcher;
            m_remoteCameraCollection.Append(group);
        }
    }
    Windows::Foundation::IAsyncAction RemoveDevice(winrt::hstring id)
    {
        co_await m_dispatcher;

        uint32_t ix{ 0 };
        for (auto const&& item : m_remoteCameraCollection)
        {
            if (item.Id() == id)
            {
                m_remoteCameraCollection.RemoveAt(ix);
                break;
            }
            ++ix;
        }
    }

private:
    CoreDispatcher m_dispatcher{ nullptr };
    DeviceWatcher m_watcher{ nullptr };
    IObservableVector<MediaFrameSourceGroup> m_remoteCameraCollection;
    DeviceWatcher::Added_revoker m_watcherAddedAutoRevoker;
    DeviceWatcher::Removed_revoker m_watcherRemovedAutoRevoker;
    DeviceWatcher::Updated_revoker m_watcherUpdatedAutoRevoker;
};
```

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Profils d’appareil photo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
