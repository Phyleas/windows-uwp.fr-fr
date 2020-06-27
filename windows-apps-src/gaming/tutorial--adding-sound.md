---
title: Ajouter du son
description: Développez un moteur audio simple à l’aide des API XAudio2 pour lire la musique et les effets sonores du jeu.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, son
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409648"
---
# <a name="add-sound"></a>Ajouter du son

> [!NOTE]
> Cette rubrique fait partie de la série de didacticiels [créer un jeu de plateforme Windows universelle simple (UWP) avec DirectX](tutorial--create-your-first-uwp-directx-game.md) . La rubrique de ce lien définit le contexte de la série.

Dans cette rubrique, nous créons un moteur audio simple utilisant des API [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) . Si vous débutez avec __XAudio2__, nous avons inclus une brève présentation des [concepts audio](#audio-concepts).

>[!Note]
>Si vous n’avez pas téléchargé le dernier code de jeu pour cet exemple, accédez à l' [exemple de jeu Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Cet exemple fait partie d’une grande collection d’exemples de fonctionnalités UWP. Pour obtenir des instructions sur le téléchargement de l’exemple, consultez [obtenir les exemples UWP à partir de GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objectif

Ajoutez des sons à l’exemple de jeu à l’aide de [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Définir le moteur audio

Dans l’exemple de jeu, les objets audio et les comportements sont définis dans trois fichiers :

* __[Audio. h](#audioh)/.cpp__: définit l’objet __audio__ , qui contient les ressources __XAudio2__ pour la lecture audio. Il définit également une méthode pour interrompre et reprendre la lecture audio si le jeu est en pause ou désactivé.
* __ [MediaReader. h](#mediareaderh)/.cpp__: définit les méthodes de lecture des fichiers audio. wav à partir du stockage local.
* __ [SoundEffect. h](#soundeffecth)/.cpp__: définit un objet pour la lecture audio dans le jeu.

## <a name="overview"></a>Vue d’ensemble

La configuration de la lecture audio dans votre jeu se compose de trois parties principales.

1. [Créer et initialiser les ressources audio](#create-and-initialize-the-audio-resources)
2. [Charger le fichier audio](#load-audio-file)
3. [Associer le son à l’objet](#associate-sound-to-object)

Ils sont tous définis dans la méthode [Simple3DGame :: Initialize](#simple3dgameinitialize-method) . Commençons par examiner cette méthode, puis nous étudierons plus en détail chacune des sections.

Après la configuration de, nous apprenons à déclencher les effets sonores pour jouer. Pour plus d’informations, consultez [la lecture du son](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame :: Initialize, méthode

Dans __Simple3DGame :: Initialize__, où __m \_ Controller__ et le __ \_ convertisseur m__ sont également initialisés, nous allons configurer le moteur audio et le préparer à lire des sons.

 * Créez __m \_ audioController__, qui est une instance de la classe [audio](#audioh) .
 * Créez les ressources audio nécessaires à l’aide de la méthode [audio :: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Ici, deux __XAudio2__ objets XAudio2 &mdash; un objet de moteur musical et un objet de moteur son, et une voix de mastérisation pour chacun d’eux a été créée. L’objet moteur musical peut être utilisé pour lire de la musique en arrière-plan pour votre jeu. Le moteur son peut être utilisé pour jouer des effets sonores dans votre jeu. Pour plus d’informations, consultez [créer et initialiser les ressources audio](#create-and-initialize-the-audio-resources).
 * Créez __mediaReader__, qui est une instance de la classe [mediaReader](#mediareaderh) . [MediaReader](#mediareaderh), qui est une classe d’assistance pour la classe [SoundEffect](#soundeffecth) , lit les petits fichiers audio de façon synchrone à partir de l’emplacement du fichier et retourne les données sonores sous la forme d’un tableau d’octets.
 * Utilisez [MediaReader :: LoadMedia](#mediareaderloadmedia-method) pour charger des fichiers audio à partir de son emplacement et créer une variable __targetHitSound__ pour contenir les données audio. wav chargées. Pour plus d’informations, consultez [charger un fichier audio](#load-audio-file). 

Les effets sonores sont associés à l’objet de jeu. Ainsi, lorsqu’une collision se produit avec cet objet de jeu, l’effet sonore est déclenché. Dans cet exemple de jeu, nous avons des effets sonores pour le AMMO (ce que nous utilisons pour prendre des cibles avec) et pour la cible. 
    
* Dans la classe __gameobject__ , une propriété __HitSound__ est utilisée pour associer l’effet sonore à l’objet.
* Créez une nouvelle instance de la classe [SoundEffect](#soundeffecth) et initialisez-la. Pendant l’initialisation, une voix source pour l’effet sonore est créée. 
* Cette classe émet un son à l’aide d’une voix de mastérisation fournie à partir de la classe [audio](#audioh) . Les données audio sont lues à partir de l’emplacement du fichier à l’aide de la classe [MediaReader](#mediareaderh) . Pour plus d’informations, consultez [associer un son à un objet](#associate-sound-to-object).

>[!Note]
>Le déclencheur réel de lecture du son est déterminé par le mouvement et la collision de ces objets de jeu. Par conséquent, l’appel pour lire réellement ces sons est défini dans la méthode [Simple3DGame :: UpdateDynamics](#simple3dgameupdatedynamics-method) . Pour plus d’informations, consultez [la lecture du son](#play-the-sound).

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Créer et initialiser les ressources audio

* Utilisez [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), une API XAudio2, pour créer deux nouveaux objets XAudio2 qui définissent les moteurs d’effets sonores et musicaux. Cette méthode retourne un pointeur vers l’interface [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) de l’objet qui gère tous les États du moteur audio, le thread de traitement audio, le graphique vocal, et bien plus encore.
* Une fois les moteurs instanciés, utilisez [IXAudio2 :: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) pour créer une voix de mastérisation pour chacun des objets du moteur audio.

Pour plus d’informations, consultez [Comment : initialiser XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Audio :: CreateDeviceIndependentResources, méthode

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Charger le fichier audio

Dans l’exemple de jeu, le code de lecture des fichiers de format audio est défini dans [MediaReader. h](#mediareaderh)/cpp__.  Pour lire un fichier audio. wav encodé, appelez [MediaReader :: LoadMedia](#mediareaderloadmedia-method), en passant le nom de fichier du fichier. wav comme paramètre d’entrée.

### <a name="mediareaderloadmedia-method"></a>MediaReader :: LoadMedia, méthode

Cette méthode utilise les API [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) pour la lecture du fichier audio .wav en tant que tampon de modulation par impulsions codées (PCM).

#### <a name="set-up-the-source-reader"></a>Configurer le lecteur source

1. Utilisez [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) pour créer un lecteur de source multimédia ([IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Utilisez [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) pour créer un objet de type[de média (](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)_MediaType_). Il représente une description d’un format de média. 
3. Spécifiez que la sortie décodée du _MediaType_est un fichier audio PCM, qui est un type audio que __XAudio2__ peut utiliser.
4. Définit le type de média de sortie décodé pour le lecteur source en appelant [IMFSourceReader :: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Pour plus d’informations sur la raison pour laquelle nous utilisons le lecteur source, accédez à [lecteur source](/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Décrire le format de données du flux audio

1. Utilisez [IMFSourceReader :: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) pour obtenir le type de média actuel pour le flux.
2. Utilisez [IMFMediaType :: MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) pour convertir le type de média audio actuel en mémoire tampon [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) , en utilisant les résultats de l’opération antérieure comme entrée. Cette structure spécifie le format de données du flux audio Wave qui est utilisé après le chargement de l’audio. 

Le format __WAVEFORMATEX__ peut être utilisé pour décrire la mémoire tampon PCM. Par rapport à la structure [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) , elle ne peut être utilisée que pour décrire un sous-ensemble de formats Wave audio. Pour plus d’informations sur les différences entre __WAVEFORMATEX__ et __WAVEFORMATEXTENSIBLE__, consultez [descripteurs de format Wave extensible](/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Lire le flux audio

1.  Obtient la durée, en secondes, du flux audio en appelant [IMFSourceReader :: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) , puis convertit la durée en octets.
2.  Lisez le fichier audio dans en tant que flux en appelant [IMFSourceReader :: ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lit l’exemple suivant à partir de la source du média.
3.  Utilisez [IMFSample :: ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) pour copier le contenu de la mémoire tampon d’exemple audio (_Sample_) dans un tableau (_mediaBuffer_).

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>Associer le son à l’objet

L’Association de sons à l’objet a lieu lors de l’initialisation du jeu, dans la méthode [Simple3DGame :: Initialize](#simple3dgameinitialize-method) .

Résumé :
* Dans la classe __gameobject__ , une propriété __HitSound__ est utilisée pour associer l’effet sonore à l’objet.
* Créez une nouvelle instance de l’objet de classe [SoundEffect](#soundeffecth) et associez-la à l’objet de jeu. Cette classe joue un son à l’aide des API __XAudio2__ .  Elle utilise une voix de mastérisation fournie par la classe [audio](#audioh) . Les données audio peuvent être lues à partir de l’emplacement du fichier à l’aide de la classe [MediaReader](#mediareaderh) .

[SoundEffect :: Initialize](#soundeffectinitialize-method) est utilisé pour initialiser l’instance __SoundEffect__ avec les paramètres d’entrée suivants : pointeur vers l’objet de moteur son (objets IXAudio2 créés dans la méthode [audio :: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), pointeur vers le format du fichier. wav à l’aide de __MediaReader :: GetOutputWaveFormatEx__et données audio chargées à l’aide de la méthode [MediaReader :: LoadMedia](#mediareaderloadmedia-method) . Pendant l’initialisation, la voix source pour l’effet sonore est également créée.

### <a name="soundeffectinitialize-method"></a>SoundEffect :: Initialize, méthode

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Lire le son

Les déclencheurs permettant de lire les effets sonores sont définis dans la méthode [Simple3DGame :: UpdateDynamics](#simple3dgameupdatedynamics-method) , car c’est là que le déplacement des objets est mis à jour et que les collisions entre les objets sont déterminées.

Étant donné que l’interaction entre les objets diffère sensiblement, en fonction du jeu, nous n’allons pas aborder la dynamique des objets de jeu ici. Si vous souhaitez comprendre son implémentation, accédez à la méthode [Simple3DGame :: UpdateDynamics](#simple3dgameupdatedynamics-method) .

En principe, lorsqu’une collision se produit, elle déclenche l’effet sonore à émettre en appelant **SoundEffect ::P laysound**. Cette méthode arrête tous les effets sonores en cours de diffusion et met en file d’attente la mémoire tampon en mémoire avec les données audio souhaitées. Il utilise la voix source pour définir le volume, envoyer des données audio et démarrer la lecture.

### <a name="soundeffectplaysound-method"></a>SoundEffect ::P méthode laySound

* Utilise l’objet Voice source **m \_ sourceVoice** pour démarrer la lecture de la mémoire tampon de données Sound **m \_ soundData**
* Crée une [ \_ mémoire tampon XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), à laquelle elle fournit une référence à la mémoire tampon de données audio, puis l’envoie avec un appel à [IXAudio2SourceVoice :: SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Avec les données audio en file d’attente, **SoundEffect::PlaySound** démarre la lecture en appelant [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame :: UpdateDynamics, méthode

La méthode __Simple3DGame :: UpdateDynamics__ prend soin de l’interaction et des collisions entre les objets de jeu. Lorsque des objets sont en conflit (ou se croisent), l’effet sonore associé est déclenché.

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
        // Start playing the sounds for the impact between the two balls.
        m_ammo[one]->PlaySound(impact, m_player->Position());
        m_ammo[two]->PlaySound(impact, m_player->Position());
    }
}
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.

    if (impact > 0.0f)
    {
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>Étapes suivantes

Nous avons abordé l’infrastructure UWP, les graphiques, les contrôles, l’interface utilisateur et l’audio d’un jeu Windows 10. La partie suivante de ce didacticiel, qui [étend l’exemple de jeu](tutorial-resources.md), explique d’autres options qui peuvent être utilisées lors du développement d’un jeu.

## <a name="audio-concepts"></a>Concepts audio

Pour le développement de jeux Windows 10, utilisez XAudio2 version 2,9. Cette version est fournie avec Windows 10. Pour plus d’informations, consultez [versions de XAudio2](/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ est une API de bas niveau qui fournit le traitement des signaux et la Fondation. Pour plus d’informations, consultez [concepts clés de XAudio2](/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>Voix XAudio2

Il existe trois types d’objets vocaux XAudio2 : les voix source, de mixage secondaire et de mastérisation. Les voix sont les objets utilisés par XAudio2 pour traiter, manipuler et lire des données audio. 
* Les voix source opèrent sur les données audio fournies par le client. 
* Les voix source et sous-mixées envoient leur sortie vers une ou plusieurs voix sous-mixées ou de matriçage. 
* Les voix sous-mixées et de matriçage mixent les signaux provenant de toutes les voix qui les alimentent et opèrent sur le résultat. 
* La maîtrise des voix reçoit des données des voix et des voix de mixage source et envoie ces données au matériel audio.

Pour plus d’informations, accédez à [XAudio2 Voices](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Graphique audio

Le graphique audio est une collection de [voix XAudio2](/windows/desktop/xaudio2/xaudio2-voices). L’audio commence à un côté d’un graphique audio dans les voix source, passe éventuellement par une ou plusieurs voix de mixage et se termine à une voix de mastérisation. Un graphique audio contient une voix source pour chaque son en cours de lecture, zéro, une ou plusieurs voix de mixage et une voix de mastérisation. Le graphique audio le plus simple, et le minimum nécessaire pour faire un bruit dans XAudio2, est une seule voix source qui s’effectue directement sur une voix de mastérisation. Pour plus d’informations, accédez à [graphiques audio](/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Documentation supplémentaire

* [Procédure : initialiser XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Procédure : charger des fichiers de données audio dans XAudio2](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Procédure : lire un son avec XAudio2](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Fichiers audio. h de clé

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```
