---
Description: Le son vient compléter l’expérience utilisateur d’une application et offre à l’utilisateur cette touche audio supplémentaire qui l’aide à reconnaître Windows sur l’ensemble des plateformes.
label: Sound
title: Son
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c479a47a53c5f52bab1febf490957355264bfc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159873"
---
# <a name="sound"></a>Son

![Image Hero](images/header-sound.svg)

Il existe de nombreuses façons d’utiliser le son pour améliorer votre application. Vous pouvez utiliser des sons pour compléter d’autres éléments de l’interface utilisateur, afin de permettre aux utilisateurs de reconnaître les événements par un son. Le son peut être un élément d’interface utilisateur efficace pour les personnes souffrant de handicaps visuels. Vous pouvez utiliser le son pour créer une atmosphère qui immerge totalement l’utilisateur ; par exemple, vous pouvez lire une bande son distrayante en arrière-plan d’un jeu de puzzle ou utiliser des effets sonores menaçants pour un jeu d’horreur/de survie.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Sound">ouvrez l’application et voir l’effet Son en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="sound-global-api"></a>API Son globale

UWP fournit un système audio aisément accessible, qui vous permet de bénéficier d’une expérience audio immersive dans l’ensemble de votre application par le simple actionnement d’un commutateur.

[**ElementSoundPlayer**](/uwp/api/windows.ui.xaml.elementsoundplayer) est un système audio intégré dans le code XAML qui, une fois activé, déclenche la lecture automatique de sons par tous les contrôles par défaut.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer** peut prendre trois états : **On**, **Off** et **Auto**.

S’il est défini sur **Off**, aucun son ne sera émis, quelle que soit la plateforme sur laquelle votre application s’exécute. Si le système audio est défini sur **On**, les sons de votre application seront émis sur chaque plateforme.

L’activation de l’objet ElementSoundPlayer entraîne également l’activation automatique de l’audio spatial (son 3D). Pour désactiver le son 3D (tout en gardant le son activé), désactivez la propriété **SpatialAudioMode** de l’objet ElementSoundPlayer : 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

La propriété **SpatialAudioMode** peut accepter les valeurs suivantes : 
- **Auto** : l’audio spatial est activé quand le son est activé. 
- **Off** : l’audio spatial est toujours désactivé, même si le son est activé.
- **On** : l’audio spatial est toujours émis.

Pour plus d’informations sur l’audio spatial et la façon dont XAML le gère, voir [AudioGraph - Audio spatial](../../audio-video-camera/audio-graphs.md#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Son pour télévision et Xbox

Le son constitue un aspect essentiel de l’expérience d’interface à 3 mètres (« 10-foot ») et par défaut, l’état de **ElementSoundPlayer** est **Auto**, ce qui signifie que vous n’entendrez le son que si votre application est exécutée sur Xbox.
Pour plus d’informations sur la conception pour Xbox et la télévision, consultez [Conception pour Xbox et la télévision](../devices/designing-for-tv.md).

## <a name="sound-volume-override"></a>Remplacement du volume sonore

Tous les sons au sein de l’application peuvent être atténués avec le contrôle **Volume**. Cependant, le volume des sons de l’application ne peut pas être *plus élevé que le volume système*.

Pour définir le niveau de volume de l’application, appelez :
```C#
ElementSoundPlayer.Volume = 0.5;
```
où le volume maximal (par rapport au volume système) est de 1,0, et le volume minimal de 0,0 (silencieux).

## <a name="control-level-state"></a>État du niveau d’un contrôle

Si le son par défaut d’un contrôle n’est pas souhaité, il peut être désactivé. Ceci se fait via l’élément **ElementSoundMode** sur le contrôle.

L’élément **ElementSoundMode** peut prendre deux états : **Off** et **Default**. Quand il n’est pas défini, il a la valeur **Default**. S’il est défini sur **Off**, chaque son lu par le contrôle sera désactivé, *sauf pour le focus*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>Est-ce le son approprié ?

Quand vous créez un contrôle personnalisé ou que vous changez le son d’un contrôle existant, il est important de bien comprendre les utilisations de tous les sons fournis par le système.

Chaque son est associé à une interaction utilisateur simple spécifique, et même si vous pouvez personnaliser les sons pour n’importe quelle interaction, cette section montre les scénarios qui nécessitent l’utilisation de sons pour garantir une expérience cohérente dans l’ensemble des applications UWP.

### <a name="invoking-an-element"></a>Appel d’un élément

Le son le plus courant déclenché par un contrôle dans notre système actuel est le son **Invoke**. Ce son est émis quand un utilisateur appelle un contrôle via un appui, un clic, une sélection de la touche Entrée ou de la barre d’espace, ou un appui sur la touche « A » d’un boîtier de commande.

En règle générale, ce son est émis seulement quand un utilisateur cible explicitement un contrôle simple ou une partie d’un contrôle via un [périphérique d’entrée](../input/index.md).


Pour lire ce son à partir d’un événement de contrôle quelconque, il vous suffit d’appeler la méthode Play à partir de **ElementSoundPlayer** et de passer en entrée **ElementSound.Invoke** :
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Affichage et masquage de contenu

Le code XAML offre de nombreux menus volants, boîtes de dialogue et interfaces utilisateur révocables, et toute action déclenchant l’une de ces superpositions doit appeler un son **Show** ou **Hide**.

Quand une fenêtre de contenu superposée s’affiche, le son **Show** doit être appelé :

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
À l’inverse, quand une fenêtre de contenu superposée se ferme (ou n’est plus au premier plan), le son **Hide** doit être appelé :

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navigation dans une page

La navigation entre les panneaux ou les vues d’une page de l’application ([Onglets et sélecteurs de vue](../controls-and-patterns/pivot.md)) donne généralement lieu à un mouvement bidirectionnel. Cela signifie que vous pouvez accéder à la vue/au panneau suivants ou précédents sans quitter la page de l’application sur laquelle vous vous trouvez.

L’expérience audio associée à ce concept de navigation est représentée par les sons **MovePrevious** et **MoveNext**.

Lors de l’accès à une vue/un panneau considérés comme l’*élément suivant* d’une liste, appelez :

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Lors de l’accès à une vue/un panneau considérés comme l’*élément précédent* d’une liste, appelez :

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Navigation vers l’arrière

Lors de l’accès à la page précédente d’une application à partir d’une page donnée, le son **GoBack** doit être appelé :

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Placement du focus sur un élément

Le son **Focus** est le seul son implicite dans notre système. Cela signifie que quand un utilisateur n’interagit directement avec aucun élément, il entend quand même un son.

Le placement du focus se produit quand un utilisateur parcourt une application, que ce soit au moyen d’un boîtier de commande, d’un clavier, d’une télécommande ou de Kinect. En règle générale, le son **Focus***n’est pas émis lors des événements PointerEntered ou de pointage avec la souris*.

Pour configurer un contrôle afin qu’il lise le son **Focus** quand il reçoit le focus, appelez :

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Émission successive de différents sons de focus

En complément de l’appel de l’élément **ElementSound.Focus**, le système audio émet successivement par défaut 4 sons différents sur chaque déclencheur de navigation. Cela signifie que le système n’émettra pas deux fois de suite les deux mêmes sons de focus.

Cette fonctionnalité de lecture successive est destinée à empêcher que les sons de focus ne deviennent monotones ou gênants pour l’utilisateur ; les sons de focus sont ceux qui seront émis le plus souvent et doivent donc être les plus subtils.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

* [Conception pour Xbox et TV](../devices/designing-for-tv.md)
* [Documentation de la classe ElementSoundPlayer](/uwp/api/windows.ui.xaml.elementsoundplayer)