---
title: Lancer la capture d’écran
description: Cette rubrique décrit les schémas d’URI MS-screenclip et MS-screensketch. Votre application peut utiliser ces schémas d’URI pour lancer l’application de capture & esquisse ou pour ouvrir une nouvelle capture.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP, Uri, capture, croquis
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2d7471f414922eb1e4923079082ee6abfd8418bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167813"
---
# <a name="launch-screen-snipping"></a>Lancer la capture d’écran

Les schémas **MS-screenclip :** et **MS-screensketch :** URI vous permettent d’initialiser les captures d’écran de capture ou de modification.

## <a name="open-a-new-snip-from-your-app"></a>Ouvrir une nouvelle capture à partir de votre application

L’URI **MS-screenclip :** permet à votre application d’ouvrir automatiquement et de démarrer une nouvelle capture. La capture obtenue est copiée dans le presse-papiers de l’utilisateur, mais n’est pas automatiquement repassée à l’application d’ouverture.

**MS-screenclip :** accepte les paramètres suivants :

| Paramètre | Type | Obligatoire | Description |
| --- | --- | --- | --- |
| source | string | non | Chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| delayInSeconds | int | Non | Valeur entière comprise entre 1 et 30. Spécifie le délai, en secondes entières, entre l’appel de l’URI et le début de l’capture. |
| callbackformat | string | non | Ce paramètre n’est pas disponible. |

## <a name="launching-the-snip--sketch-app"></a>Lancement de l’application d’esquisse & de capture

L’URI **MS-screensketch :** vous permet de lancer par programmation l’application de capture & esquisse et d’ouvrir une image spécifique dans cette application pour l’annotation.

**MS-screensketch :** accepte les paramètres suivants :

| Paramètre | Type | Obligatoire | Description |
| --- | --- | --- | --- |
| sharedAccessToken | string | non | Jeton identifiant le fichier à ouvrir dans l’application d’esquisse & de capture. Récupéré à partir de [SharedStorageAccessManager. AddFile](/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si ce paramètre est omis, l’application est lancée sans fichier ouvert. |
| secondarySharedAccessToken | string | non | Chaîne identifiant un fichier JSON avec les métadonnées relatives à la capture. Les métadonnées peuvent inclure un champ **clipPoints** avec un tableau de coordonnées x, y et/ou un [userActivity](/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| source | string | non | Chaîne de forme libre pour indiquer la source qui a lancé l’URI. |
| isTemporary | bool | Non | Si la valeur est true, l’esquisse d’écran tente de supprimer le fichier après l’avoir ouvert. |

L’exemple suivant appelle la méthode [LaunchUriAsync](/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) pour envoyer une image en vue de la capture & dessin à partir de l’application de l’utilisateur.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

L’exemple suivant illustre ce qu’un fichier spécifié par le paramètre **secondarySharedAccessToken** de **MS-screensketch** peut contenir :

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```