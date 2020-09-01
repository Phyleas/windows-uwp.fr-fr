---
description: Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique des entrées de l’utilisateur et des sorties vers ce dernier.
title: Portage d’une application Silverlight pour Windows Phone vers UWP pour le modèle d’E/S, d’appareil et d’application
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09b192f38a5bedaad491ade322df252d4b9e5971
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162183"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Portage Windows Phone Silverlight vers UWP pour les e/s, l’appareil et le modèle d’application


Rubrique précédente : [Portage du balisage XAML et de la couche interface utilisateur](wpsl-to-uwp-porting-xaml-and-ui.md).

Le code qui s’intègre à l’appareil proprement dit et à ses capteurs implique des entrées de l’utilisateur et des sorties vers ce dernier. Il peut également impliquer le traitement des données. Néanmoins, ce code n’est généralement pas pensé comme la couche interface utilisateur ni comme la couche de données. Ce code inclut l’intégration au contrôleur de vibrations, à l’accéléromètre, au gyroscope, au microphone et au haut-parleur (qui rejoignent la reconnaissance et la synthèse vocales), à la (géo)localisation et aux modalités d’entrée telles que l’écran tactile, la souris, le clavier et le stylet.

## <a name="application-lifecycle-process-lifetime-management"></a>Cycle de vie des applications (gestion de la durée de vie des processus)

Votre application Silverlight pour Windows Phone contient du code permettant d’enregistrer et de restaurer l’état de l’application et son état d’affichage, afin qu’elle puisse être désactivée, puis réactivée. Le cycle de vie des applications de plateforme Windows universelle (UWP) présente de nombreuses similitudes avec celui des applications Silverlight pour Windows Phone. En effet, elles sont dans les deux cas conçues avec le même objectif : l’optimisation des ressources disponibles pour l’application que l’utilisateur a choisi de mettre au premier plan, à un moment donné. Vous constaterez que votre code s’adapte assez facilement au nouveau système.

**Remarque**    Appuyez sur le bouton matériel **retour** pour terminer automatiquement une application Windows Phone Silverlight. Si vous appuyez sur le bouton matériel **Précédent** d’un appareil mobile, une application UWP *ne s’arrête pas* automatiquement. Au lieu de cela, l’application est suspendue, puis peut être arrêtée par la suite. Toutefois, ces détails sont transparents pour une application qui réagit de façon appropriée aux événements touchant le cycle de vie de l’application.

Une « fenêtre de réponse » correspond au laps de temps qui s’écoule entre le moment où l’application devient inactive et le moment où le système déclenche l’événement de suspension. Dans le cas d’une application UWP, ce type de fenêtre n’existe pas : l’événement de suspension est déclenché dès que l’application devient inactive.

Pour plus d’informations, consultez [cycle de vie des applications](../launch-resume/app-lifecycle.md).

## <a name="camera"></a>Appareil photo

Le code de capture d’appareil photo de Silverlight pour Windows Phone utilise les classes **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** ou **Microsoft.Phone.Tasks.CameraCaptureTask**. Pour porter ce code vers le plateforme Windows universelle (UWP), vous pouvez utiliser la classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . La rubrique [**CapturePhotoToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync) contient un exemple de code. Cette méthode vous permet de capturer une photo dans un fichier de stockage. Elle nécessite la définition des fonctionnalités **microphone** et **webcam** à l’aide d’éléments  [**DeviceCapability**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) dans le manifeste du package d’application.

Une autre option correspond à la classe [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI), qui requiert également la définition des fonctionnalités **microphone** et **webcam** à l’aide d’éléments  [**DeviceCapability**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability).

Les applications de filtre ne sont pas prises en charge pour les applications UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Détection de la plateforme d’exécution de votre application

La façon d’envisager le ciblage d’application change avec Windows 10. Selon le nouveau modèle conceptuel, une application cible la plateforme Windows universelle (UWP) et s’exécute sur tous les appareils Windows. Elle peut ensuite choisir d’activer des fonctionnalités exclusives à certaines familles d’appareils. Si nécessaire, l’application a également la possibilité de restreindre son ciblage à une ou plusieurs familles d’appareils spécifiques. Pour plus d’informations sur les familles d’appareils et savoir comment déterminer les familles d’appareils à cibler, voir le [Guide des applications UWP](../get-started/universal-application-platform-guide.md).

**Remarque**    Nous vous recommandons de ne pas utiliser le système d’exploitation ou la famille d’appareils pour détecter la présence de fonctionnalités. En règle générale, l’identification de la famille d’appareils ou du système d’exploitation actuel ne constitue pas le meilleur moyen de déterminer si une fonctionnalité particulière du système d’exploitation ou de la famille d’appareils est présente. Plutôt que de détecter le système d’exploitation ou la famille d’appareils (et le numéro de version), vérifiez directement la présence de la fonctionnalité à l’aide d’un test (voir [Compilation conditionnelle et code adaptatif](wpsl-to-uwp-porting-to-a-uwp-project.md)). Si vous devez exiger un système d’exploitation ou une famille d’appareils spécifique, veillez à l’utiliser comme une version minimale prise en charge plutôt que de concevoir le test pour cette version particulière.

Pour adapter l’interface utilisateur à différents appareils, plusieurs techniques sont recommandées. Continuez à utiliser les éléments à dimensionnement automatique et les panneaux à disposition dynamique que vous utilisez depuis toujours. Dans le balisage XAML, continuez à utiliser les tailles en pixels effectifs (auparavant appelés « pixels d’affichage ») afin que votre interface utilisateur s’adapte à différentes résolutions et différents facteurs d’échelle (voir [Pixels d’affichage/effectifs, distance d’affichage et facteurs d’échelle](wpsl-to-uwp-porting-xaml-and-ui.md)). Utilisez également les déclencheurs adaptatifs et les méthodes setter du Gestionnaire d’état visuel pour adapter votre interface utilisateur à la taille de la fenêtre (voir le [Guide des applications UWP](../get-started/universal-application-platform-guide.md)).

Toutefois, si l’un de vos scénarios implique nécessairement la détection de la famille d’appareils, vous pouvez mettre celle-ci en œuvre. Dans cet exemple, nous utilisons la classe [**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) pour accéder à une page adaptée à la famille de périphériques mobiles, le cas échéant, et nous nous assurons de revenir à une page par défaut.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Votre application peut également déterminer la famille d’appareils sur laquelle elle est exécutée à partir des facteurs de sélection de ressources en vigueur. L’exemple ci-dessous montre comment effectuer cette opération de manière impérative, et la rubrique [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) décrit le cas d’usage plus courant pour la classe dans le chargement de ressources spécifiques à la famille d’appareils en fonction du facteur de famille d’appareils.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Voir également [Compilation conditionnelle et code adaptatif](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>État de l'appareil

Une application Silverlight pour Windows Phone peut utiliser la classe **Microsoft.Phone.Info.DeviceStatus** pour obtenir des informations sur l’appareil sur lequel l’application est en cours d’exécution. S’il n’existe pas d’équivalent UWP direct pour l’espace de noms **Microsoft.Phone.Info**, voici certains événements et propriétés que vous pouvez utiliser dans une application UWP à la place des appels aux membres de la classe **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propriétés **ApplicationCurrentMemoryUsage** et **ApplicationCurrentMemoryUsageLimit** | Propriétés [**memorymanager. AppMemoryUsage**](/uwp/api/windows.system.memorymanager.appmemoryusage) et [**AppMemoryUsageLimit**](/uwp/api/windows.system.memorymanager.appmemoryusagelimit)                                                                                                                                    |
| Propriété **ApplicationPeakMemoryUsage**                                                 | Utilisez les outils de profilage de mémoire de Visual Studio. Pour plus d’informations, voir [Analyser l’utilisation de la mémoire](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).                                                                                                                                                                          |
| Propriété **DeviceFirmwareVersion**                                                      | Propriété [**EasClientDeviceInformation.SystemFirmwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion) (famille d’appareils de bureau uniquement)                                                                                                                                                                             |
| Propriété **DeviceHardwareVersion**                                                      | [**EasClientDeviceInformation.Syspropriété temHardwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion) (famille d’appareils de bureau uniquement)                                                                                                                                                                             |
| Propriété **DeviceManufacturer**                                                         | [**EasClientDeviceInformation.Syspropriété temManufacturer**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer) (famille d’appareils de bureau uniquement)                                                                                                                                                                                |
| Propriété **DeviceName**                                                                 | [**EasClientDeviceInformation.Syspropriété temProductName**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname) (famille d’appareils de bureau uniquement)                                                                                                                                                                                 |
| Propriété **DeviceTotalMemory**                                                          | Pas d'équivalent                                                                                                                                                                                                                                                                                                                      |
| Propriété **IsKeyboardDeployed**                                                         | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Propriété **IsKeyboardPresent**                                                          | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Événement **KeyboardDeployedChanged**                                                       | Aucun équivalent. Cette propriété fournit des informations sur les claviers matériels pour appareils mobiles, qui sont rarement utilisés.                                                                                                                                                                                                        |
| Propriété **PowerSource**                                                                | Pas d'équivalent                                                                                                                                                                                                                                                                                                                      |
| Événement **PowerSourceChanged**                                                            | Gérer l’événement [**RemainingChargePercentChanged**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) (famille d’appareils mobiles uniquement). L’événement est déclenché lorsque la valeur de la propriété [**RemainingChargePercent**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) (famille d’appareils mobiles uniquement) diminue de 1 %. |

## <a name="location"></a>Emplacement

Lorsqu’une application déclarant la fonctionnalité de localisation dans son manifeste de package d’application s’exécute sur Windows 10, le système demande le consentement de l’utilisateur final. Si votre application affiche sa propre invite de consentement personnalisée ou qu’elle fournit une bascule de type activation/désactivation, vous devrez donc supprimer ces éléments pour que l’utilisateur final ne soit invité qu’une seule fois à autoriser cette fonctionnalité.

## <a name="orientation"></a>Orientation

L’équivalent d’application UWP des propriétés **PhoneApplicationPage.SupportedOrientations** et **Orientation** correspond à l’élément [**uap:InitialRotationPreference**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) dans le manifeste de package d’application. Sélectionnez l’onglet **Application** s’il ne l’est pas déjà, puis cochez une ou plusieurs cases sous **Rotations prises en charge** pour enregistrer vos préférences.

Toutefois, vous êtes invité à concevoir l’interface utilisateur de votre application UWP de façon à en optimiser l’apparence indépendamment de l’orientation de l’appareil et de la taille de l’écran. Pour plus d’informations, voir la rubrique [Portage pour différents facteurs de forme et expériences utilisateur](wpsl-to-uwp-form-factors-and-ux.md).

Rubrique suivante : [Portage des couches métier et des couches de données](wpsl-to-uwp-business-and-data.md).