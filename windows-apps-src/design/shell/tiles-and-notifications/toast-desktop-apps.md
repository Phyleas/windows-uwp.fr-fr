---
Description: Découvrez les différentes options disponibles pour les applications de bureau Win32 pour l’envoi de notifications Toast
title: Notifications toast à partir d’applications de bureau
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 05/01/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, Desktop, notifications Toast, Desktop Bridge, msix, package Sparse, options pour envoyer des toasts, serveur com, activateur com, com, com factice, com, sans com, envoyer un toast
ms.localizationpriority: medium
ms.openlocfilehash: 020cdeb1aaddac7fe879e91d18e258aea1b387ea
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970984"
---
# <a name="toast-notifications-from-desktop-apps"></a>Notifications toast à partir d’applications de bureau

Les applications de bureau (y compris les applications [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empaquetées, les applications qui utilisent des [packages éparss](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) pour obtenir l’identité du package et les applications Win32 non empaquetées classiques) peuvent envoyer des notifications de Toast interactif comme les applications d’application Windows. Toutefois, il existe différentes options pour les applications de bureau en raison des différents schémas d’activation.

Dans cet article, nous répertorions les options dont vous disposez pour envoyer une notification Toast sur Windows 10. Toutes les options prennent entièrement en charge...

* Persistance dans le centre de notifications
* En cours d’activation à la fois dans le menu contextuel et dans le centre de notifications
* Être activable pendant que votre EXE n’est pas en cours d’exécution

## <a name="all-options"></a>Toutes les options

Le tableau ci-dessous illustre vos options de prise en charge des toasts dans votre application de bureau et les fonctionnalités prises en charge correspondantes. Vous pouvez utiliser le tableau pour sélectionner la meilleure option pour votre scénario.<br/><br/>

| Option | Objets visuels | Actions | Entrées | Active in-process |
| -- | -- | -- | -- | -- |
| [Activateur COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Aucun CLSID COM/stub](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Option préférée-activateur COM

Il s’agit de l’option recommandée qui fonctionne pour les applications de bureau et prend en charge toutes les fonctionnalités de notification. N’hésitez pas à utiliser l’activateur COM. Nous disposons d’une bibliothèque [pour les applications C#](send-local-toast-desktop.md) et [C++](send-local-toast-desktop-cpp-wrl.md) , ce qui est très simple, même si vous n’avez jamais écrit de serveur com auparavant.<br/><br/>

| Objets visuels | Actions | Entrées | Active in-process |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Avec l’option COM Activator, vous pouvez utiliser les modèles de notification et les types d’activation suivants dans votre application.<br/><br/>

| Type de modèle et d’activation | Package MSIX/Sparse | Win32 classique |
| -- | -- | -- |
| Premier plan ToastGeneric | ✔️ | ✔️ |
| Arrière-plan ToastGeneric | ✔️ | ✔️ |
| Protocole ToastGeneric | ✔️ | ✔️ |
| Modèles hérités | ✔️ | ❌ |

> [!NOTE]
> Si vous ajoutez l’activateur COM à votre application de package MSIX/Sparse existante, le premier plan/l’arrière-plan et les activations de notification héritées activent désormais votre activateur COM au lieu de votre ligne de commande.

Pour savoir comment utiliser cette option, consultez [Envoyer une notification Toast locale à partir d’applications Desktop C#](send-local-toast-desktop.md) ou [Envoyer une notification Toast locale à partir d’applications Desktop C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Option alternative-aucun CLSID COM/stub

Il s’agit d’une autre option si vous ne pouvez pas implémenter un activateur COM. Toutefois, vous allez sacrifier quelques fonctionnalités, telles que la prise en charge des entrées (zones de texte sur les toasts) et l’activation in-process.<br/><br/>

| Objets visuels | Actions | Entrées | Active in-process |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Avec cette option, si vous prenez en charge Win32 classique, vous êtes bien plus limité dans les modèles de notification et les types d’activation que vous pouvez utiliser, comme indiqué ci-dessous.<br/><br/>

| Type de modèle et d’activation | Package MSIX/Sparse | Win32 classique |
| -- | -- | -- |
| Premier plan ToastGeneric | ✔️ | ❌ |
| Arrière-plan ToastGeneric | ✔️ | ❌ |
| Protocole ToastGeneric | ✔️ | ✔️ |
| Modèles hérités | ✔️ | ❌ |

Pour les applications [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empaquetées et les applications qui utilisent des [packages éparss](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps), il vous suffit d’envoyer des notifications de Toast comme une application UWP. Quand l’utilisateur clique sur votre toast, votre application est lancée à partir de la ligne de commande avec les arguments Launch que vous avez spécifiés dans le Toast.

Pour les applications Win32 classiques, configurez le identifiant AUMID afin de pouvoir envoyer des toasts, puis spécifiez un CLSID sur votre raccourci. Il peut s’agir d’un GUID aléatoire. N’ajoutez pas le serveur/l’activateur COM. Vous ajoutez un CLSID COM « stub », ce qui entraîne la conservation de la notification par le centre de maintenance. Notez que vous ne pouvez utiliser que des toasts d’activation de protocole, car le CLSID stub interrompt l’activation de toute autre activation Toast. Par conséquent, vous devez mettre à jour votre application pour prendre en charge l’activation de protocole et faire en sorte que le protocole toasts active votre propre application.


## <a name="resources"></a>Ressources

* [Envoyer une notification Toast locale à partir d’applications Desktop C#](send-local-toast-desktop.md)
* [Envoyer une notification Toast locale à partir d’applications Desktop C++ WRL](send-local-toast-desktop-cpp-wrl.md)
* [Documentation sur le contenu Toast](adaptive-interactive-toasts.md)
