---
title: Communication interprocessus (IPC)
description: Cette rubrique décrit les différentes façons d’effectuer des communications interprocessus (IPC) entre des applications plateforme Windows universelle (UWP) et des applications Win32.
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256379"
---
# <a name="interprocess-communication-ipc"></a>Communication interprocessus (IPC)

Cette rubrique décrit les différentes façons d’effectuer des communications interprocessus (IPC) entre des applications plateforme Windows universelle (UWP) et des applications Win32.

## <a name="app-services"></a>Services d’application

Les services d’application permettent aux applications d’exposer des services qui acceptent et retournent des conteneurs de propriétés de primitives ([**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)) en arrière-plan. Les objets enrichis peuvent être passés s’ils sont [sérialisés](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu).

Les services d’application peuvent s’exécuter [hors processus](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) en tant que tâche en arrière-plan ou [dans le processus](/windows/uwp/launch-resume/convert-app-service-in-process) au sein de l’application de premier plan.

Les services d’application conviennent mieux pour le partage de petites quantités de données lorsque la latence en temps quasi réel n’est pas nécessaire.

## <a name="com"></a>COM

[Com](/windows/win32/com/component-object-model--com--portal) est un système orienté objet distribué permettant de créer des composants logiciels binaires capables d’interagir et de communiquer. En tant que développeur, vous utilisez COM pour créer des composants logiciels réutilisables et des couches d’automatisation pour une application. Les composants COM peuvent être en cours de traitement ou hors processus, et ils peuvent communiquer via un modèle [client et serveur](/windows/win32/com/com-clients-and-servers) . Les serveurs COM out-of-process ont longtemps été utilisés pour la [communication entre les objets](/windows/win32/com/inter-object-communication).

Les applications empaquetées avec la fonctionnalité [runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) peuvent inscrire des serveurs com hors processus pour IPC via le [manifeste du package](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension). C’est ce qu’on appelle [com empaqueté](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/).

## <a name="filesystem"></a>FileSystem

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

Les applications empaquetées peuvent exécuter IPC à l’aide du système de fichiers large en déclarant la fonctionnalité restreinte [broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations) . Cette fonctionnalité accorde aux API [Windows. Storage](/uwp/api/Windows.Storage) et aux API Win32 [xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) l’accès au système de fichiers large.

Par défaut, IPC via le système de fichiers pour les applications empaquetées est limité aux autres mécanismes décrits dans cette section.

### <a name="publishercachefolder"></a>PublisherCacheFolder

Le [PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder) permet aux applications empaquetées de déclarer des dossiers dans leur manifeste qui peuvent être partagés avec d’autres packages par le même serveur de publication.

Le dossier de stockage partagé a les exigences et restrictions suivantes :

* Les données du dossier de stockage partagé ne sont pas sauvegardées ou itinérantes.
* L’utilisateur peut effacer le contenu du dossier de stockage partagé.
* Vous ne pouvez pas utiliser le dossier de stockage partagé pour partager des données entre des applications provenant de différents serveurs de publication.
* Vous ne pouvez pas utiliser le dossier de stockage partagé pour partager des données entre différents utilisateurs.
* Le dossier de stockage partagé ne dispose pas de la gestion des versions.

Si vous publiez plusieurs applications et que vous recherchez un mécanisme simple pour partager des données entre elles, PublisherCacheFolder est une option simple basée sur un système de fichiers.

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) est utilisé conjointement avec les services d’application, les activations de protocole (par exemple, LaunchUriForResultsAsync), etc., pour partager des StorageFiles via des jetons.

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

Avec la fonctionnalité [runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) , les applications empaquetées peuvent [lancer des processus de confiance totale](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) dans le même package.

Dans les scénarios où les restrictions de package sont fastidieuses, ou si les options IPC manquent, une application peut utiliser un processus de confiance totale en tant que proxy pour interagir avec le système, puis IPC avec le processus de confiance totale par le biais d’app services ou d’un autre mécanisme IPC bien pris en charge.

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results) est utilisé pour l’échange de données simple ([ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)) avec d’autres applications empaquetées qui implémentent le contrat d’activation [ProtocolForResults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) . Contrairement à app services, qui s’exécute généralement en arrière-plan, l’application cible est lancée au premier plan.

Les fichiers peuvent être partagés en passant des jetons [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) à l’application via ValueSet.

## <a name="loopback"></a>Bouclage

Le bouclage est le processus de communication avec un serveur réseau qui écoute sur localhost (l’adresse de bouclage).

Pour assurer la sécurité et l’isolement réseau, les connexions de bouclage pour IPC sont bloquées par défaut pour les applications empaquetées. Vous pouvez activer les connexions de bouclage entre les applications empaquetées approuvées à l’aide de [fonctionnalités](/previous-versions/windows/apps/hh770532(v=win.10)) et de [Propriétés de manifeste](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules).

* Toutes les applications empaquetées qui participent à des connexions de bouclage doivent déclarer la `privateNetworkClientServer` fonctionnalité dans leurs [manifestes de package](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Deux applications empaquetées peuvent communiquer par boucle en déclarant des [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) dans leurs manifestes de package.
    * Chaque application doit répertorier l’autre dans son [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). Le client déclare une règle « out » pour le serveur, et le serveur déclare des règles « in » pour ses clients pris en charge.

> [!NOTE]
> Le nom de la famille de packages requis pour identifier une application dans ces règles est disponible via l’éditeur de manifeste de package dans Visual Studio pendant le développement, via l' [espace partenaires](/windows/uwp/publish/view-app-identity-details) pour les applications publiées via le Microsoft Store, ou via la commande PowerShell [AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) pour les applications déjà installées.

Les applications et les services non empaquetés n’ont pas d’identité de package et ne peuvent donc pas être déclarés dans [LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules). Vous pouvez configurer une application empaquetée pour qu’elle se connecte par le biais du bouclage avec des applications et des services non empaquetés via [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)). Toutefois, cela n’est possible que pour les scénarios chargement ou de débogage dans lesquels vous disposez d’un accès local à l’ordinateur, et vous disposez de privilèges d’administrateur.

* Toutes les applications empaquetées participant à des connexions de bouclage doivent déclarer la `privateNetworkClientServer` fonctionnalité dans leurs [manifestes de package](/uwp/schemas/appxpackage/uapmanifestschema/element-capability).
* Si une application empaquetée se connecte à une application ou à un service décompressé, exécutez `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` pour ajouter une exemption de bouclage pour l’application empaquetée.
* Si une application ou un service décompressé se connecte à une application empaquetée, exécutez `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` pour permettre à l’application empaquetée de recevoir des connexions de bouclage entrantes.
    * [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) doit s’exécuter en continu pendant que l’application empaquetée écoute les connexions.
    * L' `-is` indicateur a été introduit dans Windows 10, version 1607 (10,0 ; Build 14393).

> [!NOTE]
> Le nom de famille de packages requis pour l' `-n` indicateur de [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) est disponible via l’éditeur de manifeste de package dans Visual Studio pendant le développement, via l' [espace partenaires](/windows/uwp/publish/view-app-identity-details) pour les applications publiées via le Microsoft Store ou via la commande PowerShell [AppxPackage](/powershell/module/appx/get-appxpackage?view=win10-ps) pour les applications qui sont déjà installées.

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) est également utile pour [déboguer les problèmes d’isolement réseau](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues).

## <a name="pipes"></a>Canaux

Les [canaux](/windows/win32/ipc/pipes) permettent une communication simple entre un serveur de canal et un ou plusieurs clients de canal.

Les canaux [nommés](/windows/win32/ipc/named-pipes) et les [canaux anonymes](/windows/win32/ipc/anonymous-pipes) sont pris en charge avec les contraintes suivantes :

* Par défaut, les canaux nommés dans les applications empaquetées sont pris en charge uniquement entre les processus au sein du même package, sauf si un processus est de confiance totale.
* Les canaux nommés peuvent être partagés entre les packages en suivant les instructions relatives au [partage des objets nommés](/windows/uwp/communication/sharing-named-objects).
* Les canaux nommés dans les applications empaquetées doivent utiliser la syntaxe du `\\.\pipe\LOCAL\` nom du canal.

## <a name="registry"></a>Registre

L’utilisation [du Registre](/windows/win32/sysinfo/registry-functions) pour IPC est généralement déconseillée, mais elle est prise en charge pour le code existant. Les applications empaquetées peuvent accéder uniquement aux clés de Registre auxquelles elles sont autorisées à accéder.

Les [applications de bureau empaquetées comme MSIX](/windows/msix/desktop/desktop-to-uwp-root) tirent parti de la [virtualisation du registre](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) , de sorte que les écritures de Registre globales sont contenues dans une ruche privée au sein du package MSIX. Cela permet la compatibilité du code source tout en minimisant l’impact global du Registre, et peut être utilisé pour IPC entre les processus dans le même package. Si vous devez utiliser le registre, ce modèle est préférable à la manipulation du registre global.

## <a name="rpc"></a>RPC

[RPC](/windows/win32/rpc/rpc-start-page) peut être utilisé pour connecter une application empaquetée à un point de terminaison RPC Win32, à condition que l’application empaquetée dispose des fonctionnalités appropriées pour faire correspondre les listes de contrôle d’accès au point de terminaison RPC.

Les fonctionnalités personnalisées permettent aux fabricants d’ordinateurs OEM et aux IHV de [définir des fonctionnalités arbitraires](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability), d’établir une [liste de contrôle d’accès à leurs points de terminaison RPC](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability), puis [d’accorder ces fonctionnalités à des applications clientes autorisées](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file). Pour obtenir un exemple d’application complet, consultez l’exemple [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) .

Les points de terminaison RPC peuvent également être gérée à des applications packagées spécifiques pour limiter l’accès au point de terminaison aux seules applications sans nécessiter la surcharge de gestion des fonctionnalités personnalisées. Vous pouvez utiliser l’API [DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) pour dériver un SID d’un nom de famille de packages, puis la liste de contrôle d’accès du point de terminaison RPC avec le SID, comme indiqué dans l’exemple [CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) .

## <a name="shared-memory"></a>Mémoire partagée

Le [mappage de fichier](/windows/win32/memory/sharing-files-and-memory) peut être utilisé pour partager un fichier ou une mémoire entre deux ou plusieurs processus avec les contraintes suivantes :

* Par défaut, les mappages de fichiers dans les applications empaquetées sont pris en charge uniquement entre les processus dans le même package, sauf si un processus est de confiance totale.
* Les mappages de fichiers peuvent être partagés entre les packages en suivant les instructions relatives au [partage des objets nommés](/windows/uwp/communication/sharing-named-objects).

La mémoire partagée est recommandée pour partager et manipuler efficacement de grandes quantités de données.
