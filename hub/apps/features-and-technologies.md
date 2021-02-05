---
description: Cette section permet de comprendre de quelle manière les fonctionnalités Windows sont prises en charge sur les différentes plateformes d’applications et comment commencer à utiliser ces fonctionnalités dans votre code.
title: Fonctionnalités et technologies
ms.topic: article
ms.date: 01/29/2021
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: 9a77b10fe22d1d0706fd6cf913f19e40c3c37390
ms.sourcegitcommit: 6759309a3fbb6ede498c95c04c05f57a074ab070
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99069176"
---
# <a name="features-and-technologies-for-windows-apps"></a>Fonctionnalités et technologies des applications Windows

Quel que soit le type d’application que vous créez ou le périphérique que vous ciblez, Windows prend en charge de nombreuses fonctionnalités qui constituent des blocs de construction clés pour les scénarios d’application importants. Certaines de ces fonctionnalités sont exposées à Windows Runtime (WinRT) et à la plateforme Windows universelle (UWP), Win32 (API Windows) et à d’autres plateformes d’application de différentes manières. Les articles suivants vous aident à comprendre comment certaines fonctionnalités de Windows sont prises en charge dans différentes plateformes d’application et comment commencer à utiliser les fonctionnalités de votre code.

Cet article fournit une liste personnalisée d’articles pour découvrir plus en détail comment vous pouvez accéder aux fonctionnalités et technologies Windows dans les plateformes d’applications WinRT, Win32 (API Windows), WPF et Windows Forms. Pour obtenir des informations complètes sur les fonctionnalités de développement de chaque plateforme, consultez les ressources suivantes :

* [Documentation WinRT/UWP](/windows/uwp/index)
* [Documentation Win32 (API Windows)](/windows/desktop/index)
* [Documentation WPF](/dotnet/framework/wpf/index)
* [Documentation Windows Forms](/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Principales fonctionnalités et technologies Windows

Les sections suivantes mettent en évidence plusieurs fonctionnalités et technologies importantes de Windows qui permettent de proposer des expériences modernes et attrayantes aux clients.

### <a name="windows-ink"></a>Windows Ink

![Stylet Surface](images/hero-small.png)  

La plateforme Windows Ink, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

Pour plus d’informations sur les différentes façons d’utiliser Windows Ink dans les applications Windows, consultez [Windows Ink.](/windows/uwp/design/input/pen-and-stylus-interactions)

### <a name="speech-interactions"></a>Interactions vocales

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

Windows propose différents moyens d’intégrer la reconnaissance vocale et la conversion de texte par synthèse vocale (également appelée TTS ou synthèse vocale) directement dans l’expérience utilisateur de votre application. Les fonctions vocales peuvent permettre aux utilisateurs d’interagir avec votre application de manière fiable et agréable. Elles peuvent aussi compléter, voire remplacer le clavier, la souris, l’écran tactile et les mouvements.

Pour plus d’informations sur les différentes façons d’utiliser les interactions vocales dans les applications Windows, consultez [Parole, voix et conversation dans Windows 10](speech.md).

### <a name="windows-ai"></a>Windows IA

![Windows IA](images/windows-ai.png)

Nous proposons plusieurs solutions IA différentes que vous pouvez utiliser pour améliorer vos applications Windows. Avec Windows Machine Learning, vous pouvez intégrer des modèles de Machine Learning entraînés à vos applications et les exécuter localement sur l’appareil. Windows Vision Skills permet d’utiliser des bibliothèques prédéfinies pour effectuer des tâches de traitement d’image courantes ou de créer vos propres solutions personnalisées. DirectML fournit des API de type DirectX de bas niveau qui vous permettent de tirer pleinement parti du matériel.

Pour plus d’informations sur les différentes façons d’utiliser Windows l’IA dans les applications Windows, consultez [IA Windows.](/windows/ai/)

## <a name="features-and-technologies-by-platform"></a>Fonctionnalités et technologies par plateforme

Les sections suivantes fournissent des liens utiles pour en savoir plus sur l’intégration avec les fonctionnalités et technologies principales de Windows à partir de nos principales plateformes d’applications : WinRT/UWP, Win32 (API Windows), WPF et Windows Forms.

### <a name="user-interface-and-accessibility"></a>Interface utilisateur et accessibilité

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Concevez](/windows/uwp/design/basics/)<br/><br/>[Disposition](/windows/uwp/design/layout/)<br/><br/>[Contrôles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Entrée](/windows/uwp/design/input/)<br/><br/>[Vignettes](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/)<br/><br/>[Lancement, reprise et tâches en arrière-plan](/windows/uwp/launch-resume/)<br/><br/>[Accessibilité de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>[Interactions vocales](/windows/uwp/design/input/speech-interactions)<br/><br/> |  [Interface utilisateur de bureau](/windows/desktop/windows-application-ui-development)<br/><br/>[Environnement et shell de bureau](/windows/desktop/user-interface)<br/><br/>[Contrôles Windows](/windows/desktop/controls/window-controls)<br/><br/>[Contrôles UWP dans les applications de bureau (XAML Islands)](./desktop/modernize/xaml-islands.md)<br/><br/>[Couche visuelle dans les applications de bureau](./desktop/modernize/visual-layer-in-desktop-apps.md)<br/><br/>[Windows et messages](/windows/desktop/winmsg/windowing)<br/><br/>[Menus et autres ressources](/windows/desktop/menurc/resources)<br/><br/>[Haute résolution](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accessibilité](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform - SDK (version 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[SDK Microsoft Speech version 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [Intégration du format Windows au format WPF](/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Vue d’ensemble de la navigation](/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[Intégration du format XAML au format WPF](/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Contrôles](/dotnet/framework/wpf/controls/)<br/><br/>[Programmation de la couche visuelle](/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Entrée](/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accessibilité](/dotnet/framework/ui-automation/)<br/><br/>[Guide de programmation System.Speech pour le .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Créer un formulaire Windows](/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Contrôles](/dotnet/framework/winforms/controls/)<br/><br/>[Boîtes de dialogue](/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrée de l’utilisateur](/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accessibilité de Windows Forms](/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[Guide de programmation System.Speech pour le .NET Framework](/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vidéo et graphismes

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vidéo et appareil photo](/windows/uwp/audio-video-camera/)<br/><br/>[Lecture de contenu multimédia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/) |  [Audio et vidéo](/windows/desktop/audio-and-video)<br/><br/>[Graphismes et jeux ](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Graphismes](/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimédia](/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Graphismes et dessin](/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer, classe](/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Accès aux données et ressources de l’application

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Accès aux données](/windows/uwp/data-access/)<br/><br/>[Liaison de données](/windows/uwp/data-binding/)<br/><br/>[Fichiers, dossiers et bibliothèques](/windows/uwp/files/)<br/><br/>[Ressources de l’application](/windows/uwp/app-resources/) |  [Accès aux données et stockage](/windows/desktop/data-access-and-storage)<br/><br/>[Systèmes de fichiers locaux](/windows/desktop/fileio/file-systems)<br/><br/>[Vues d’ensemble des ressources](/windows/desktop/menurc/resources-overviews)</li>  |  [Données et modélisation](/dotnet/framework/data/)<br/><br/>[Liaison de données](/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressources dans les applications .NET](/dotnet/framework/resources/)<br/><br/>[Ressource, contenu et fichiers de données d’une application](/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Données et modélisation](/dotnet/framework/data/)<br/><br/>[Liaison de données](/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressources dans les applications .NET](/dotnet/framework/resources/)<br/><br/>[Paramètres d’application](/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Appareils, documents et impression

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Activer les fonctionnalités d’un appareil](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Détecteurs](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impression et numérisation](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de capteur](/windows/desktop/sensorsapi/portal)<br/><br/>[Impression](/desktop/printdocs/printdocs-printing)<br/><br/>[API UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Impression et gestion du système d’impression](/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Prise en charge de l’impression](/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Système, réseau et alimentation

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtenir des informations sur la batterie](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threads et programmation asynchrone](/windows/uwp/threading-async/)<br/><br/>[Mise en réseau et services web](/windows/uwp/networking/) | [Services système](/windows/desktop/system-services)<br/><br/>[Gestion de la mémoire](/windows/desktop/memory/memory-management)<br/><br/>[Gestion de l’alimentation](/windows/desktop/power/power-management-portal)<br/><br/>[Processus et threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Mise en réseau et Internet](/windows/desktop/networking)<br/><br/>[Informations système Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modèle de thread](/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programmation réseau dans le .NET Framework](/dotnet/framework/network-programming/)  |  [Informations système](/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Gestion de l’alimentation](/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programmation réseau dans le .NET Framework](/dotnet/framework/network-programming/)<br/><br/>[Fonction de mise en réseau des Windows Forms](/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="security"></a>Sécurité

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Sécurité](/windows/uwp/security)<br/><br/>[Authentification et identité des utilisateurs](/windows/uwp/security/authentication-and-user-identity)<br/><br/>[Gestionnaire de comptes web](/windows/uwp/security/web-account-manager)<br/><br/>[Service Broker d’authentification web](/windows/uwp/security/web-authentication-broker)<br/><br/>[Cryptographie](/windows/uwp/security/cryptography) | [Sécurité et identité](/windows/win32/security)<br/><br/>[Authentification](/win32/secauthn/authentication-portal)<br/><br/>[Cryptographie](/windows/win32/seccng/cng-portal) |  [Sécurité dans .NET](/dotnet/standard/security/)<br/><br/>[Sécurité (WPF)](/dotnet/desktop/wpf/security-wpf)  |  [Sécurité dans .NET](/dotnet/standard/security/)<br/><br/>[Sécurité des Windows Forms](/dotnet/desktop/winforms/windows-forms-security)  |

### <a name="debugging-and-performance"></a>Débogage et performances

|  WinRT/UWP  |  Win32 (API Windows) |  WPF et Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Débogage, tests et analyse des performances](/windows/uwp/debug-test-perf)<br/><br/>[Déploiement et débogage des applications UWP](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Kit de certification des applications Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[Performances](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [Débogage et gestion des erreurs](/windows/win32/debugging-and-error-handling)<br/><br/>[Outils de débogage pour Windows](/windows-hardware/drivers/debugger/)<br/><br/>[Suivi d’événements pour Windows (ETW)](/windows/win32/etw/event-tracing-portal)<br/><br/>[API .NET TraceProcessing](./trace-processing/index.yml)<br/><br/>[TraceLogging](/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[Compteurs de performance](/windows/win32/perfctrs/performance-counters-portal) |  [Débogage, traçage et profilage](/dotnet/framework/debug-trace-profile/)<br/><br/>[Traçage et instrumentation d’applications](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[Diagnostiquer les erreurs avec les Assistants Débogage managé](/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[Profilage de runtime](/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[Compteurs de performance](/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[Déploiement ClickOnce pour les Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>Déploiement et packaging

|  WinRT/UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetage d’applications](/windows/uwp/packaging/)<br/><br/>[MSIX](/windows/msix/)<br/><br/>[Schéma de manifeste du package de l’application](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Installation d’application et maintenance](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](/dotnet/framework/deployment/)<br/><br/>[Déploiement d’une application WPF](/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](/dotnet/framework/deployment/)<br/><br/>[Déploiement ClickOnce pour les Windows Forms](/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
