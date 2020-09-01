---
title: Ajout d’audio à l’exemple Marble Maze
description: Ce document décrit les pratiques clés à prendre en compte quand vous utilisez du son et comment appliquer ces pratiques à l’exemple Marble Maze.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, audio, jeux, exemple
ms.localizationpriority: medium
ms.openlocfilehash: df1be02cd962f086e52609550e9cae65e5280b71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172133"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Ajout d’audio à l’exemple Marble Maze

Ce document décrit les pratiques clés à prendre en compte quand vous utilisez du son et comment appliquer ces pratiques à l’exemple Marble Maze. Le labyrinthe utilise [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) pour charger des ressources audio à partir de fichiers, et [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) pour mélanger et lire des données audio et pour appliquer des effets à l’audio.

Le labyrinthe de marbre lit la musique en arrière-plan et utilise également des sons de jeu pour indiquer les événements de jeu, par exemple quand le marbre atteint un mur. Dans son implémentation, Marble Maze utilise un effet de réverbération, ou écho, pour reproduire le son de la bille quand elle rebondit. L’implémentation de l’effet de réverbération permet à l’écho de s’entendre plus rapidement et plus fort dans des petites pièces et d’être plus sourd et de s’entendre moins fort dans des pièces de grandes dimensions.

> [!NOTE]
> L’exemple de code qui correspond à ce document se trouve dans l' [exemple de jeu de labyrinthe DirectX Marble](https://github.com/microsoft/Windows-appsample-marble-maze).

Voici quelques éléments clés présentés dans ce document que vous devez prendre en compte quand vous utilisez du son dans votre jeu :

- Utilisez Media Foundation pour décoder les éléments audio et XAudio2 pour lire l’audio. Cependant, si vous disposez déjà d’un mécanisme de chargement d’éléments audio qui fonctionne dans une application de la plateforme Windows universelle (UWP), vous pouvez l’utiliser.

- Un graphique audio contient une voix source pour chaque son actif, zéro ou plusieurs voix prémixées et une voix mastérisée. Les voix sources peuvent être utilisées dans les voix prémixées et/ou dans la voix mastérisée. Les voix prémixées peuvent être utilisées dans d’autres voix prémixées ou dans la voix mastérisée.

- Si la taille de vos fichiers de musique de fond est importante, diffusez la musique en continu dans des mémoires tampons de plus petites tailles afin d’économiser la mémoire.

- Vous pouvez également décider de suspendre l’audio quand l’application perd le focus, n’est plus visible ou est suspendue. Relancez l’audio quand l’application retrouve le focus, est de nouveau visible ou est relancée.

- Définissez les catégories audio de sorte qu’elles reflètent le rôle de chaque son. Par exemple, vous utilisez généralement **AudioCategory \_ GameMedia** pour l’audio d’arrière-plan du jeu et **AudioCategory \_ GameEffects** pour les effets sonores.

- Gérez les changements de périphériques, notamment les casques, en libérant et en recréant toutes les ressources et interfaces audio.

- Pensez à compresser les fichiers audio quand vous devez économiser l’espace disque et réduire les coûts de diffusion. Dans le cas contraire, ne compressez pas l’audio pour un chargement plus rapide.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>Présentation de XAudio2 et Microsoft Media Foundation

XAudio2 est une bibliothèque audio de bas niveau pour Windows qui prend en charge tout particulièrement l’audio de jeu. Il fournit un moteur de traitement du signal numérique (DSP) et de graphique audio pour les jeux. XAudio2 se base sur ses prédécesseurs, DirectSound et XAudio, pour prendre en charge les tendances informatiques telles que les architectures à virgule flottante SIMD et l’audio HD. Il prend également en charge les demandes de traitement audio plus complexes des jeux actuels.

Le document [Concepts principaux de XAudio2](/windows/desktop/xaudio2/xaudio2-key-concepts) explique les concepts clés de l’utilisation de XAudio2. En résumé, ces concepts sont les suivants :

- L’interface [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) est au cœur du moteur XAudio2. Marble Maze utilise cette interface pour créer des voix et recevoir une notification quand le périphérique de sortie change ou ne fonctionne pas.

- Une **voix** traite, ajuste et lit les données audio.

- Une **voix source** est une collection de canaux audio (mono, 5,1, etc.) et représente un flux de données audio. Dans XAudio2, le traitement de l’audio commence au niveau de la voix source. En général, les données sonores sont chargées à partir d’une source externe, par exemple un fichier ou un réseau, puis sont envoyées à une voix source. Marble Maze utilise [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) pour charger les données audio à partir de fichiers. Media Foundation est présenté plus loin dans ce document.

- Une **voix de mixage secondaire** traite les données audio. Ce traitement peut inclure la modification du flux audio ou la combinaison de plusieurs flux en un seul. Marble Maze utilise les prémixages pour créer l’effet de réverbération.

- Une **voix de mastérisation** combine les données des voix source et de mixage secondaire et envoie ces données au matériel audio.

- Un **graphique audio** contient une voix source pour chaque son actif, zéro, une ou plusieurs voix de mixage, et une seule voix de mastérisation.

- Un **rappel** informe le code client qu’un événement s’est produit dans une voix ou dans un objet moteur. En utilisant des rappels, vous pouvez réutiliser la mémoire quand XAudio2 a terminé d’utiliser une mémoire tampon, vous pouvez réagir quand le périphérique audio change (par exemple, quand vous connectez ou déconnectez un casque), etc. La [gestion des modifications de l’appareil et du casque](#handling-headphones-and-device-changes) plus loin dans ce document explique comment le labyrinthe de marbre utilise ce mécanisme pour gérer les modifications des appareils.

Marble Maze utilise deux moteurs audio (c’est-à-dire, deux objets [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)) pour traiter l’audio. Un moteur traite la musique en arrière-plan, tandis que l’autre traite les sons de jeu.

Marble Maze doit également créer une voix mastérisée pour chaque moteur. N’oubliez pas qu’un moteur mastérisé associe les flux audio en un seul flux et envoie ce flux au matériel audio. Le flux de musique de fond, une voix source, envoie les données à une voix mastérisée et à deux voix prémixées. Les voix prémixées réalisent l’effet de réverbération.

Media Foundation est une bibliothèque multimédia qui prend en charge de nombreux formats audio et vidéo. XAudio2 et Media Foundation se complètent. Le labyrinthe de marbre utilise Media Foundation pour charger des ressources audio à partir de fichiers et utilise XAudio2 pour lire l’audio. Vous n’avez pas besoin d’utiliser Media Foundation pour charger les éléments audio. Si vous disposez déjà d’un mécanisme de chargement d’éléments audio qui fonctionne dans les applications de la plateforme Windows universelle (UWP), vous pouvez l’utiliser. L' [audio, la vidéo et l’appareil photo](../audio-video-camera/index.md) décrivent différentes façons d’implémenter l’audio dans une application UWP.

Pour plus d’informations sur XAudio2, voir le [Guide de programmation](/windows/desktop/xaudio2/programming-guide). Pour plus d’informations sur Media Foundation, voir [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk).

## <a name="initializing-audio-resources"></a>Initialisation des ressources audio

Les labyrinthe de marbre utilisent un fichier Windows Media Audio (. WMA) pour la musique d’arrière-plan et des fichiers WAV (. wav) pour les sons de jeu. Ces formats sont pris en charge par Media Foundation. Même si le format de fichier .wav est pris en charge en mode natif par XAudio2, un jeu doit analyser le format de fichier manuellement pour remplir les structures de données XAudio2 appropriées. Marble Maze utilise Media Foundation pour traiter plus facilement les fichiers .wav. Pour obtenir la liste complète des formats multimédias pris en charge par Media Foundation, voir [Formats multimédias pris en charge dans Media Foundation](/windows/desktop/medfound/supported-media-formats-in-media-foundation). Marble Maze n’utilise pas des formats audio distincts au moment de la conception et de l’exécution. Il n’utilise pas non plus la prise en charge de la compression ADPCM XAudio2. Pour plus d’informations sur la compression ADPCM dans XAudio2, voir [Vue d’ensemble d’ADPCM](/windows/desktop/xaudio2/adpcm-overview).

La méthode **audio :: CreateResources** , qui est appelée à partir de **MarbleMazeMain :: LoadDeferredResources**, charge les flux audio à partir des fichiers, initialise les objets du moteur XAudio2 et crée les voix source, de mixage secondaire et de mastérisation.

### <a name="creating-the-xaudio2-engines"></a>Création des moteurs XAudio2

N’oubliez pas que Marble Maze crée un objet [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) pour représenter chaque moteur audio qu’il utilise. Pour créer un moteur audio, appelez la méthode [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) . L’exemple suivant montre comment Marble Maze crée le moteur audio qui traite la musique de fond.

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

Le labyrinthe de marbre effectue une étape similaire pour créer le moteur audio qui lit les sons de jeu.

L’utilisation de l’interface [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) dans une application UWP diffère de son utilisation dans une application de bureau de deux façons. En premier lieu, vous ne devez pas appeler [CoInitializeEx](/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) avant d’appeler [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create). Par ailleurs, **IXAudio2** ne prend plus en charge l’énumération de périphériques. Pour plus d’informations sur la façon d’énumérer des périphériques audio, voir [Énumération des périphériques](/previous-versions/windows/apps/hh464977(v=win.10)).

### <a name="creating-the-mastering-voices"></a>Création des voix mastérisées

L’exemple suivant montre comment la méthode **audio :: CreateResources** crée la voix de mastérisation pour la musique d’arrière-plan à l’aide de la méthode [IXAudio2 :: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) . Dans cet exemple, **m \_ musicMasteringVoice** est un objet [IXAudio2MasteringVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) . Nous spécifions deux canaux d’entrée ; Cela simplifie la logique de l’effet de réverbération. 

Nous spécifions 48000 comme taux d’échantillonnage d’entrée. Nous avons choisi ce taux d’échantillonnage car il représente un bon équilibre entre la qualité audio et la quantité de traitement d’UC nécessaire. Un taux d’échantillonnage supérieur aurait requis davantage de traitement de l’UC sans amélioration notable de la qualité. 

Enfin, nous spécifions **AudioCategory_GameMedia** comme catégorie de flux audio pour que les utilisateurs puissent écouter de la musique à partir d’une application différente au fur et à mesure qu’ils lisent le jeu. Quand une application musicale est lue, Windows désactive les voix créées par l’option **AudioCategory \_ GameMedia** . L’utilisateur entend toujours entendre les sons de jeu, car ils sont créés par l’option **AudioCategory \_ GameEffects** . Pour plus d’informations sur les catégories audio, consultez [ \_ \_ catégorie Audio Stream](/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category).

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice,
        2,
        48000,
        0,
        nullptr,
        nullptr,
        AudioCategory_GameMedia
        )
);
```

La méthode **audio :: CreateResources** effectue une étape similaire pour créer la voix de mastérisation des sons de jeu, à la différence qu’elle spécifie **AudioCategory \_ GameEffects** pour le paramètre *StreamCategory* , qui est la valeur par défaut.

### <a name="creating-the-reverb-effect"></a>Création de l’effet de réverbération

Pour chaque voix, vous pouvez utiliser XAudio2 pour créer des séquences d’effets qui traitent l’audio. Ces séquences sont appelées « chaînes d’effets ». Utilisez les chaînes d’effets quand vous voulez appliquer un ou plusieurs effets à une voix. Les chaînes d’effets peuvent être destructrices. Autrement dit, chaque effet de la chaîne peut remplacer la mémoire tampon audio. Cette propriété est importante, car XAudio2 ne garantit pas l’initialisation des mémoires tampons de sortie en mode silence. Les objets d’effets sont représentés dans XAudio2 par des objets de traitement audio multiplateforme (XAPO). Pour plus d’informations sur les objets XAPO, voir [Vue d’ensemble des objets XAPO](/windows/desktop/xaudio2/xapo-overview).

Pour créer une chaîne d’effets, procédez comme suit :

1. Créez l’objet d’effet.

2. Remplir une structure de [ \_ \_ descripteur d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) avec des données d’effet.

3. Remplir une structure de [ \_ \_ chaîne d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) avec des données.

4. Appliquez la chaîne d’effets à une voix.

5. Remplissez une structure de paramètre d’effet et appliquez-la à l’effet.

6. Désactivez ou activez l’effet en fonction des besoins.

La classe **Audio** définit la méthode **CreateReverb** pour créer la chaîne d’effets qui implémente la réverbération. Cette méthode appelle la méthode [XAudio2CreateReverb](/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) pour créer un **objet &lt; IUnknown &gt; ComPtr** , **soundEffectXAPO**, qui joue le rôle de voix de mixage secondaire pour l’effet de réverbération.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

La structure du [ \_ \_ descripteur d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) contient des informations sur un XAPO à utiliser dans une chaîne d’effet, par exemple, le nombre cible de canaux de sortie. La méthode **audio :: CreateReverb** crée un **objet \_ \_ descripteur d’effet XAUDIO2** , **soundEffectdescriptor**, qui est défini sur l’état désactivé, utilise deux canaux de sortie et référence **soundEffectXAPO** pour l’effet de réverbération. **soundEffectdescriptor** démarre dans un état désactivé, car le jeu doit définir des paramètres avant que l’effet commence à modifier les sons du jeu. Marble Maze utilise deux canaux de sortie pour simplifier la logique de l’effet de réverbération.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Si votre chaîne d’effets comporte plusieurs effets, chaque effet nécessite un objet. La structure de la [ \_ \_ chaîne d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) contient le tableau d’objets de [ \_ \_ descripteur d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) qui participent à l’effet. L’exemple suivant montre comment la méthode **Audio::CreateReverb** spécifie l’effet permettant d’implémenter la réverbération.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

La méthode **Audio::CreateReverb** appelle la méthode [IXAudio2::CreateSubmixVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) pour créer la voix prémixée pour l’effet. Elle spécifie l’objet de [ \_ \_ chaîne d’effet XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) , **SoundEffectChain**, pour le paramètre *pEffectChain* afin d’associer la chaîne d’effet à la voix. Marble Maze spécifie également deux canaux de sortie et un taux d’échantillonnage de 48 kilohertz.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> Si vous souhaitez attacher une chaîne d’effets existante à une voix de mixage secondaire existante, ou si vous souhaitez remplacer la chaîne d’effet active, utilisez la méthode [IXAudio2Voice :: SetEffectChain](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) .

La méthode **audio :: CreateReverb** appelle [IXAudio2Voice :: SetEffectParameters](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) pour définir des paramètres supplémentaires associés à l’effet. Cette méthode utilise une structure de paramètre spécifique à l’effet. Un objet de [ \_ \_ paramètres de réverbération XAUDIO2FX](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) , **m_reverbParametersSmall**, qui contient les paramètres d’effet pour le réverbe, est initialisé dans la méthode **audio :: Initialize** , car chaque effet de réverbération partage les mêmes paramètres. L’exemple suivant montre comment la méthode **Audio::Initialize** initialise les paramètres de réverbération en champ proche.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

Cet exemple utilise les valeurs par défaut pour la plupart des paramètres de réverbération, mais il affecte à **DisableLateField** la valeur TRUE pour spécifier la réverbération en champ proche, à **EarlyDiffusion** la valeur 4 pour simuler des surfaces proches planes et à **LateDiffusion** la valeur 15 pour simuler des surfaces éloignées très diffuses. Les surfaces proches planes permettent aux échos de vous atteindre plus rapidement et plus fort. Les surfaces éloignées diffuses permettent aux échos d’être plus silencieux et de vous atteindre plus lentement. Vous pouvez faire des essais avec les valeurs de réverbération pour tirer le meilleur profit de votre jeu ou utiliser la méthode **ReverbConvertI3DL2ToNative** pour utiliser les paramètres standard I3DL2 (Interactive 3D audio Guidelines Level 2,0).

L’exemple suivant montre comment **Audio::CreateReverb** définit les paramètres de réverbération. **newSubmix** est un objet [IXAudio2SubmixVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)* *. **Parameters** est un objet de [ \_ \_ paramètres de réverbération XAUDIO2FX](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)*.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

La méthode **audio :: CreateReverb** se termine en activant l’effet à l’aide de [IXAudio2Voice :: EnableEffect](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) si l’indicateur **EnableEffect** est défini. Il définit également son volume à l’aide de [IXAudio2Voice :: setVolume](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) et de la matrice de sortie à l’aide de [IXAudio2Voice :: SetOutputMatrix](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix). Cette partie définit le volume sur complet (1.0), puis définit la matrice de volume en mode silence pour les entrées droite et gauche et les haut-parleurs de sortie droit et gauche. Nous procédons ainsi car un autre code se fond entre les deux réverbérations (en simulant la transition entre le fait d’être près d’un mur et celui d’être dans une grande salle) ou désactive le son des deux réverbérations si nécessaire. Quand le chemin de réverbération est ensuite restauré, le jeu définit une matrice {1.0f, 0.0f, 0.0f, 1.0f} pour acheminer la sortie de réverbération gauche vers l’entrée gauche de la voix mastérisée et la sortie de réverbération droite vers l’entrée droite de la voix mastérisée.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Le labyrinthe de marbre appelle la méthode **audio :: CreateReverb** quatre fois : deux fois pour la musique d’arrière-plan et deux fois pour les sons du jeu. L’exemple suivant montre comment Marble Maze appelle la méthode **CreateReverb** pour la musique de fond.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Pour obtenir la liste des sources d’effets possibles utilisables avec XAudio2, voir [Effets audio XAudio2](/windows/desktop/xaudio2/xaudio2-audio-effects).

### <a name="loading-audio-data-from-file"></a>Chargement des données audio à partir d’un fichier

Le labyrinthe de marbre définit la classe **MediaStreamer** , qui utilise Media Foundation pour charger des ressources audio à partir de fichiers. Marble Maze utilise un objet **MediaStreamer** pour charger chaque fichier audio.

Marble Maze appelle la méthode **MediaStreamer::Initialize** pour initialiser chaque flux audio. Voici comment la méthode **Audio::CreateResources** appelle **MediaStreamer::Initialize** pour initialiser le flux audio pour la musique de fond :

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

La méthode **MediaStreamer :: Initialize** commence par appeler la méthode [MFStartup](/windows/desktop/api/mfapi/nf-mfapi-mfstartup) pour initialiser Media Foundation. **MF_VERSION** est une macro définie dans **mfapi. h**et est ce que nous spécifions comme version de Media Foundation à utiliser.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** appelle ensuite [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) pour créer un objet [IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader). Un objet **IMFSourceReader** , **m_reader**, lit les données multimédias à partir du fichier spécifié par l' **URL**.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

La méthode **MediaStreamer :: Initialize** crée ensuite un objet [IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) à l’aide de [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) pour décrire le format du flux audio. Un format audio a deux types : un type principal et un sous-type. Le type principal définit le format général des données multimédias, par exemple vidéo, audio, script, etc. Le sous-type définit le format, par exemple PCM, ADPCM ou WMA.

La méthode **MediaStreamer :: Initialize** utilise la méthode [IMFAttributes :: SetGUID](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) pour spécifier le type principal ([MF_MT_MAJOR_TYPE](/windows/desktop/medfound/mf-mt-major-type-attribute)) comme audio (**MFMediaType \_ audio**) et le type secondaire ([MF_MT_SUBTYPE](/windows/desktop/medfound/mf-mt-subtype-attribute)) en tant que fichier audio PCM non compressé (**MFAudioFormat \_ PCM**). **MF_MT_MAJOR_TYPE** et **MF_MT_SUBTYPE** sont des [attributs de Media Foundation](/windows/desktop/medfound/media-foundation-attributes). **MFMediaType_Audio** et **MFAUDIOFORMAT_PCM** sont des GUID de type et de sous-type ; Pour plus d’informations, consultez [types de média audio](/windows/desktop/medfound/audio-media-types) . La méthode [IMFSourceReader::SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) associe le type de média au lecteur de flux.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

La méthode **MediaStreamer :: Initialize** obtient ensuite le format de média de sortie complet à partir de Media Foundation à l’aide de [IMFSourceReader :: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) et appelle la méthode [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) pour convertir le type de média audio Media Foundation en une structure [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) . La structure **WAVEFORMATEX** définit le format des données audio .wav. Marble Maze utilise cette structure pour créer les voix sources et appliquer le filtre passe-bas au son de roulement de la bille.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> [!IMPORTANT]
> La méthode [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) utilise **CoTaskMemAlloc** pour allouer l’objet [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) . Par conséquent, veillez à appeler **CoTaskMemFree** quand vous avez terminé d’utiliser cet objet.

 

La méthode **MediaStreamer :: Initialize** se termine par le calcul de la longueur du flux, **m \_ maxStreamLengthInBytes**, en octets. Pour ce faire, il appelle la méthode [IMFSourceReader :: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) pour connaître la durée du flux audio en unités de 100 nanosecondes, convertit la durée en sections, puis multiplie par le taux moyen de transfert de données en octets par seconde. Le labyrinthe de marbre utilise ensuite cette valeur pour allouer la mémoire tampon qui contient chaque son de jeu.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>Création des voix sources

Marble Maze crée des voix sources XAudio2 pour lire chacun de ses sons et sa musique de jeu dans les voix sources. La classe **audio** définit un objet [IXAudio2SourceVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) pour la musique d’arrière-plan et un tableau d’objets **SoundEffectData** pour contenir les sons de jeu. La structure **SoundEffectData** contient l’objet **IXAudio2SourceVoice** correspondant à un effet et définit également d’autres données liées à l’effet, telles que la mémoire tampon audio. **Audio. h** définit l’énumération **SoundEvent** . Le labyrinthe de marbre utilise cette énumération pour identifier chaque son de jeu. La classe **audio** utilise également cette énumération pour indexer le tableau d’objets **SoundEffectData** .

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

Le tableau suivant montre la relation entre chacune de ces valeurs, le fichier qui contient les données de son associées et une courte description de ce que chaque son représente. Les fichiers audio se trouvent dans le dossier ** \\ Media \\ audio** .

| Valeur SoundEvent  | Nom de fichier      | Description                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Lu quand la bille roule.                              |
| FallingEvent      | MarbleFall.wav | Lu quand la bille sort (tombe) du labyrinthe.               |
| CollisionEvent    | MarbleHit.wav  | Lu quand la bille entre en collision avec le labyrinthe.           |
| CheckpointEvent   | Checkpoint.wav | Lu quand la bille passe par un point de contrôle.         |
| MenuChangeEvent   | MenuChange.wav | Lu lorsque l’utilisateur modifie l’élément de menu actuel. |
| MenuSelectedEvent | MenuSelect.wav | Lu lorsque l’utilisateur sélectionne un élément de menu.           |

 

L’exemple suivant montre comment la méthode **Audio::CreateResources** crée la voix source pour la musique de fond. La structure du [ \_ \_ descripteur d’envoi XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) définit la voix de destination cible à partir d’une autre voix et spécifie si un filtre doit être utilisé. Le labyrinthe de marbre appelle la méthode **audio :: SetSoundEffectFilter** pour utiliser les filtres afin de changer le son de la balle au fur et à mesure. La structure [XAUDIO2 Voice Send définit \_ \_ ](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) le jeu de voix pour recevoir des données à partir d’une seule voix de sortie. Marble Maze envoie les données de la voix source à la voix mastérisée (pour la partie inchangée d’un son en cours de lecture) et aux deux voix prémixées qui implémentent la partie réverbérante d’un son en cours de lecture.

La méthode [IXAudio2::CreateSourceVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) crée et configure une voix source. Elle prend une structure [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) qui définit le format des mémoires tampons audio envoyées à la voix. Comme indiqué précédemment, Marble Maze utilise le format PCM.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>Lecture de la musique de fond


Une voix source est créée dans l’état arrêté. Marble Maze démarre la musique de fond dans la boucle de jeu. Le premier appel à **MarbleMazeMain :: Update** appelle **audio :: Start** pour démarrer la musique d’arrière-plan.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

La méthode **Audio::Start** appelle [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) pour démarrer le traitement de la voix source pour la musique de fond.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

La voix source passe ces données audio à l’étape suivante du graphique audio. Dans le cas de Marble Maze, l’étape suivante contient deux voix prémixées qui appliquent les deux effets de réverbération à l’audio. Une voix prémixée applique une réverbération proche à champ tardif. La seconde applique une réverbération lointaine à champ tardif.

La quantité selon laquelle chaque voix prémixée contribue à la combinaison finale est déterminée par la taille et la forme de la salle. La réverbération en champ proche est plus importante quand la bille est près d’un mur ou dans une petite salle. La réverbération à champ tardif est plus importante quand la bille se trouve dans un grand espace. Cette technique produit un effet d’écho plus réaliste quand la bille se déplace dans le labyrinthe. Pour en savoir plus sur la façon dont Marble Maze implémente cet effet, voir **Audio::SetRoomSize** et **Physics::CalculateCurrentRoomSize** dans le code source de Marble Maze.

> [!NOTE]
> Dans un jeu dans lequel la plupart des tailles d’espace sont relativement identiques, vous pouvez utiliser un modèle de réverbération plus basique. Par exemple, vous pouvez utiliser un paramètre de réverbération pour toutes les pièces ou vous pouvez créer un paramètre de réverbération prédéfini pour chaque pièce.

La méthode **Audio::CreateResources** utilise Media Foundation pour charger la musique de fond. Cependant, à ce stade, la voix source ne dispose pas de données audio à utiliser. De plus, comme la musique de fond passe en boucle, la voix source doit être régulièrement mise à jour avec les données pour que la musique puisse continuer à être jouée.

Pour que la voix source dispose de ces données, la boucle du jeu met à jour les mémoires tampons audio à chaque image. La méthode **MarbleMazeMain :: Render** appelle **audio :: Render** pour traiter la mémoire tampon audio de musique en arrière-plan. La classe **audio** définit un tableau de trois mémoires tampons audio, **m \_ audioBuffers**. Chaque mémoire tampon contient 64 Ko (65 536 octets) de données. La boucle lit les données à partir de l’objet Media Foundation et écrit ces données dans la voix source jusqu’à ce que cette dernière ait trois mémoires tampons mises en file d’attente.

> [!CAUTION]
> Bien que le labyrinthe de marbre utilise une mémoire tampon de 64 Ko pour contenir des données musicales, vous devrez peut-être utiliser une mémoire tampon plus grande ou plus petite. La taille dépend des exigences du jeu.

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer],
                STREAMING_BUFFER_SIZE,
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

La boucle gère également le moment où l’objet Media Foundation atteint la fin du flux. Dans ce cas, elle appelle la méthode [IMFSourceReader :: SetCurrentPosition](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) pour réinitialiser la position de la source audio.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Pour implémenter la boucle audio pour une seule mémoire tampon (ou pour un son entier entièrement chargé en mémoire), vous pouvez définir le champ [XAUDIO2_BUFFER](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: LoopCount sur **XAUDIO2 \_ Loop \_ infinite** lorsque vous initialisez le son. Marble Maze utilise cette technique pour lire le son de la bille qui roule.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

Cependant, pour la musique de fond, Marble Maze gère les mémoires tampons directement, pour obtenir un meilleur contrôle sur la quantité de mémoire utilisée. Quand la taille de vos fichiers de musique est importante, vous pouvez diffuser les données de musique dans des mémoires tampons plus petites. Cela permet d’équilibrer la taille de la mémoire et la fréquence de traitement et de diffusion des données audio du jeu.

> [!TIP]
> Si votre jeu a une fréquence d’images faible ou variable, le traitement de l’audio sur le thread principal peut produire des pauses ou des pop inattendus dans le son, car le moteur audio ne dispose pas de données audio mises en mémoire tampon pour fonctionner avec. Si votre jeu subit ce problème, traitez l’audio dans un thread séparé qui n’effectue pas de rendu. Cette approche est particulièrement utile sur les ordinateurs à plusieurs processeurs, car votre jeu peut utiliser des processeurs inactifs.

## <a name="reacting-to-game-events"></a>Réaction aux événements du jeu

La classe **audio** fournit des méthodes telles **que PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch**et **SetSoundEffectFilter** pour permettre au jeu de contrôler le moment où les sons sont lus et arrêtés, ainsi que de contrôler les propriétés de son, telles que le volume et la tonalité. Par exemple, si le marbre sort du labyrinthe, **MarbleMazeMain :: Update** appelle la méthode **audio ::P laysoundeffect** pour lire le son **FallingEvent** .

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

La méthode **Audio::PlaySoundEffect** appelle la méthode [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) pour démarrer la lecture du son. Si la méthode **IXAudio2SourceVoice::Start** a déjà été appelée, elle n’est pas redémarrée. **Audio::PlaySoundEffect** exécute ensuite une logique personnalisée pour certains sons.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
        // If there's an error, then we'll recreate the engine on the next
        // render pass.
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();

    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up,
    // and allow up to two collisions to be queued up. 
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Pour les sons autres que le roulement, la méthode **Audio::PlaySoundEffect** appelle [IXAudio2SourceVoice::GetState](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) pour déterminer le nombre de mémoires tampons que la voix source lit. Elle appelle [IXAudio2SourceVoice::SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) pour ajouter les données audio du son à la file d’attente d’entrée de la voix si aucune mémoire tampon n’est active. La méthode **Audio::PlaySoundEffect** permet également au son de collision d’être lu deux fois successivement. Cela se produit, par exemple, quand la bille heurte un angle du labyrinthe.

Comme décrit précédemment, la classe audio utilise l’indicateur de ** \_ boucle \_ infinie XAUDIO2** quand elle Initialise le son pour l’événement de substitution. Le son démarre la lecture en boucle la première fois que la méthode **Audio::PlaySoundEffect** est appelée pour cet événement. Pour simplifier la logique de lecture pour le son de roulement, Marble Maze désactive le son au lieu de l’arrêter. Quand la vitesse de la bille change, Marble Maze modifie le pas et le volume du son pour lui donner un effet plus réaliste. L’exemple suivant montre comment la méthode **MarbleMazeMain :: Update** met à jour la hauteur et le volume du marbre en fonction de la rapidité de son changement et de la façon dont il désactive le son en affectant à son volume la valeur zéro lorsque le marbre s’arrête.

```cpp
// Play the roll sound only if the marble is actually rolling.
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly
    // ramp up the low-pass filter the faster we go.
    // We also reduce the Q-value of the filter, starting with a
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent,
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## <a name="reacting-to-suspend-and-resume-events"></a>Réaction aux événements d’interruption et de reprise

La [structure d’application du labyrinthe de marbre](marble-maze-application-structure.md) décrit comment le labyrinthe de marbre prend en charge l’interruption et la reprise. Quand le jeu est interrompu, l’audio est mis en pause. Quand le jeu reprend, l’audio recommence là où il a été arrêté. Cela correspond à la meilleure pratique qui consiste à ne pas utiliser de ressources quand cela n’est pas nécessaire.

La méthode **Audio::SuspendAudio** est appelée quand le jeu est suspendu. Cette méthode appelle la méthode [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) pour arrêter l’ensemble du son. Même si **IXAudio2::StopEngine** arrête immédiatement toutes les sorties audio, elle conserve le graphique audio et ses paramètres d’effet (par exemple, l’effet de réverbération appliqué quand la bille rebondit).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }

    m_isAudioStarted = false;
}
```

La méthode **Audio::ResumeAudio** est appelée quand le jeu reprend. Cette méthode utilise la méthode [IXAudio2::StartEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) pour redémarrer le son. Dans la mesure où l’appel à [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) conserve le graphique audio et ses paramètres d’effet, la sortie audio reprend là où elle a été arrêtée.

```cpp
// Restarts the audio streams. A call to this method must match a previous call
// to SuspendAudio. This method causes audio to continue where it left off.
// If there is a problem with the restart, the m_engineExperiencedCriticalError
// flag is set. The next call to Render will recreate all the resources and
// reset the audio pipeline.
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## <a name="handling-headphones-and-device-changes"></a>Gestion des casques et des changements de périphériques

Le labyrinthe de marbre utilise des rappels de moteur pour gérer les défaillances du moteur XAudio2, par exemple lorsque le périphérique audio change. La connexion ou la déconnexion d’un casque par l’utilisateur est une source de changement de périphérique. Nous recommandons d’implémenter le rappel de moteur qui gère les changements de périphériques. Dans le cas contraire, votre jeu ne lira plus les sons quand l’utilisateur connecte ou retire un casque, tant que le jeu n’est pas redémarré.

**Audio. h** définit la classe **AudioEngineCallbacks** . Cette classe implémente l’interface [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback).

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private:
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

L’interface [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) permet à votre code d’être notifié quand des événements de traitement du son se produisent et quand le moteur rencontre une erreur critique. Pour s’inscrire aux rappels, le labyrinthe de marbre appelle la méthode [IXAudio2 :: RegisterForCallbacks](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) dans **audio :: CreateResources**, après avoir créé l’objet [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) pour le moteur Music.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze ne requiert pas de notification quand le traitement audio démarre ou se termine. Par conséquent, il implémente les méthodes [IXAudio2EngineCallback::OnProcessingPassStart](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) et [IXAudio2EngineCallback::OnProcessingPassEnd](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) pour qu’elles ne fassent rien. Pour la méthode [IXAudio2EngineCallback :: OnCriticalError](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) , le labyrinthe de marbre appelle la méthode **SetEngineExperiencedCriticalError** , qui définit l’indicateur **m \_ engineExperiencedCriticalError** .

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Quand une erreur critique se produit, le traitement audio s’arrête et tous les appels suivants à XAudio2 échouent. Pour résoudre ce problème, vous devez libérer l’instance XAudio2 et en créer une nouvelle. La méthode **audio :: Render** , qui est appelée à partir de la boucle de jeu chaque frame, vérifie tout d’abord l’indicateur **m \_ engineExperiencedCriticalError** . Si cet indicateur est défini, elle l’efface, libère l’instance XAudio2 actuelle, initialise les ressources et démarre la musique de fond.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Le labyrinthe de marbre utilise également l’indicateur **m \_ engineExperiencedCriticalError** pour se protéger contre l’appel à XAudio2 quand aucun périphérique audio n’est disponible. Par exemple, la méthode **MarbleMazeMain :: Update** ne traite pas l’audio pour les événements de substitution ou de collision lorsque cet indicateur est défini. L’application tente de réparer le moteur audio à chaque trame, si nécessaire. Toutefois, l’indicateur **m \_ engineExperiencedCriticalError** peut toujours être défini si l’ordinateur n’a pas de périphérique audio ou si le casque est débranché et qu’il n’y a pas d’autre périphérique audio disponible.

> [!CAUTION]
> En règle générale, n’effectuez pas d’opérations bloquantes dans le corps d’un rappel de moteur. Cela peut entraîner des problèmes de performances. Marble Maze définit un indicateur dans le rappel **OnCriticalError** et gère l’erreur ultérieurement pendant la phase régulière de traitement du son. Pour plus d’informations sur les rappels XAudio2, voir [Rappels XAudio2](/windows/desktop/xaudio2/xaudio2-callbacks).

## <a name="conclusion"></a>Conclusion

Cela englobe l’exemple de jeu de labyrinthe de marbre ! Bien qu’il s’agisse d’un jeu relativement simple, il contient un grand nombre des éléments importants qui entrent dans n’importe quel jeu DirectX UWP et constitue un bon exemple à suivre pour créer votre propre jeu.

Maintenant que vous avez terminé, essayez de vous prépasser du code source et de voir ce qui se passe. Ou découvrez [créer un jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md), un autre exemple de jeu DirectX UWP.

Vous êtes prêt à aller plus loin avec DirectX ? Consultez ensuite nos guides sur la [programmation DirectX](directx-programming.md).

Si vous êtes intéressé par le développement de jeux sur UWP en général, consultez la documentation relative à la [programmation de jeux](index.md).

## <a name="related-topics"></a>Rubriques connexes

* [Ajout d’entrées et d’interactivité à l’exemple Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Développement de Marble Maze, jeu pour UW en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)