---
description: Une fois vos packages téléchargés, vous verrez une table qui indique quels packages seront proposés à des familles d’appareils Windows 10 spécifiques (et des versions de système d’exploitation antérieures, le cas échéant), dans l’ordre de classement.
title: Disponibilité de la famille d’appareils
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP, packages, téléchargement, disponibilité de la famille d’appareils
ms.localizationpriority: medium
ms.openlocfilehash: 48cbb0fd9ecf27c9926d55e22abc17d039d3674b
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411963"
---
# <a name="device-family-availability"></a>Disponibilité de la famille d’appareils

Une fois que vos packages ont été correctement téléchargés sur la page **packages** , la section disponibilité de la **famille d’appareils** affiche un tableau qui indique quels packages seront proposés à des familles d’appareils Windows 10 spécifiques (et, le cas échéant, versions antérieures du système d’exploitation), dans l’ordre de classement. Cette section vous permet également de choisir s’il faut ou non offrir la soumission aux clients sur des familles d’appareils Windows 10 spécifiques.

> [!NOTE]
> Si vous n’avez pas encore téléchargé de packages, la section disponibilité de la **famille d’appareils** affiche les familles d’appareils Windows 10 avec des cases à cocher qui vous permettent d’indiquer si la soumission sera ou non proposée aux clients sur ces familles d’appareils. La table s’affiche une fois que vous avez téléchargé un ou plusieurs packages.

Cette section comprend également une case à cocher dans laquelle vous pouvez indiquer si vous souhaitez autoriser Microsoft à mettre l’application à la disposition des prochaines familles d’appareils Windows 10. Nous recommandons de conserver cette case à cocher activée pour que votre application puisse être disponible pour davantage de clients potentiels à mesure que de nouvelles familles d’appareils seront introduites.


## <a name="choosing-which-device-families-to-support"></a>Sélection des familles d’appareils prises en charge

Si vous téléchargez des packages ciblant une famille de périphériques individuels, nous allons activer la case à cocher pour mettre ces packages à la disposition des nouveaux clients sur ce type d’appareil. Par exemple, si un package cible Windows. Desktop, la boîte de dialogue **Windows 10 Desktop** est vérifiée pour ce package (et vous ne pouvez pas activer les cases à cocher pour les autres familles d’appareils).

Les packages ciblant la famille de périphériques Windows. Universal peuvent s’exécuter sur n’importe quel appareil Windows 10 (y compris Xbox One). Par défaut, nous allons mettre ces packages à la disposition des nouveaux clients sur tous les types d’appareils *, à l’exception* de Xbox.

Vous pouvez décocher la case en regard d’une famille d’appareils Windows 10 si vous ne souhaitez pas proposer la soumission aux clients sur ce type d’appareils. Si la case associée à une famille d’appareils est décochée, les nouveaux clients sur ce type d’appareils ne pourront pas acquérir l’application (bien que les clients qui possèdent déjà l’application puissent toujours l’utiliser, et bénéficient des mises à jour soumises).

Si votre application les prend en charge, nous vous recommandons de garder toutes les cases cochées, sauf si vous avez une raison particulière de limiter les types de périphériques Windows 10 pouvant acquérir votre application. Par exemple, si vous savez que votre application n’offre pas une bonne expérience sur [surface Hub](https://developer.microsoft.com/windows/surfacehub) et/ou [Microsoft HoloLens](https://developer.microsoft.com/mixed-reality), vous pouvez décocher la case **Windows 10 Team** et/ou **Windows 10 holographique** . Cela empêche les nouveaux clients d’acquérir l’application sur ces appareils. Si vous décidez ultérieurement de l’offrir à ces clients, vous pouvez créer une nouvelle soumission avec les cases à cocher activées.

<span id="xbox" />

La seule famille d’appareils Windows 10 qui n’est pas activée par défaut pour Windows. les packages universels est **Windows 10 Xbox**. Si votre application n’est pas un jeu (ou s’il s’agit d’un jeu et que vous avez activé le [programme de créateurs de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) ou franchir le processus d' [approbation du concept](../gaming/concept-approval.md) ) et que votre envoi comprend des packages UWP neutres et/ou x64 compilés à l’aide du kit de développement logiciel (SDK) **Windows 10 version** 14393 ou ultérieure

> [!IMPORTANT]
> Pour que votre application puisse être lancée sur des appareils Xbox, vous devez inclure un package indépendant ou x64 qui est compilé avec SDK Windows version 14393 ou ultérieure. Toutefois, si vous activez la case à cocher **Xbox Windows 10**, votre package avec version supérieure applicable à la Xbox (c’est-à-dire un package neutre ou x64 qui cible la famille de périphériques Xbox ou Universal Device Family) sera toujours proposé aux clients sur Xbox, même s’il est compilé avec une version du SDK antérieure. Pour cette raison, il est essentiel de vous assurer que le package présentant la version la plus élevée applicable à Xbox soit compilé avec la version 14393 du SDK Windows ou une version supérieure. Si ce n’est pas le cas, vous verrez un message d’erreur indiquant que les clients Xbox ne seront pas en mesure de lancer votre application. 
> 
> Pour résoudre cette erreur, vous pouvez effectuer l’une des opérations suivantes :
> - Remplacez les packages applicables par les instances compilées à l’aide de la version 14393 du SDK Windows ou une version ultérieure.
> - Si vous disposez déjà d’un package qui prend en charge Xbox et qui est compilé à l’aide de la version 14393 du SDK Windows ou une version ultérieure, augmentez son numéro de version, de manière à en faire le package présentant la version la plus élevée de la soumission.
> - Désélectionnez la case à cocher **Xbox Windows 10**.
>   
> SI vous ne parvenez toujours pas à résoudre le problème, contactez le support technique.

Si vous soumettez une application UWP pour Windows 10 IoT Core, vous ne devez pas modifier les sélections par défaut après avoir chargé vos packages. Il n’existe pas de case à cocher distincte pour Windows 10 IoT. Pour plus d’informations sur la publication d’applications UWP d’IoT Core, consultez [Microsoft Store prise en charge des applications UWP de base UWP](/windows/iot-core/commercialize-your-device/installingandservicing).

Si votre soumission pour une application précédemment publiée comprend des packages qui peuvent s’exécuter sur **Windows 8/8.1** et **Windows Phone 8. x et versions antérieures**, ces packages seront mis à la disposition des clients sur ces versions de système d’exploitation. Pour cesser d’offrir votre application à ces clients, supprimez les packages correspondants de votre envoi.

> [!IMPORTANT]
> Pour empêcher complètement une famille spécifique d’appareils Windows 10 d’obtenir votre soumission, mettez à jour l’élément [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) dans votre manifeste pour cibler uniquement la famille d’appareils que vous souhaitez prendre en charge (par exemple, Windows. mobile ou Windows. Desktop), plutôt que de le laisser comme valeur Windows. Universal (pour la famille d’appareils universels) que Microsoft Visual Studio inclure dans le manifeste par défaut.

Il est important de savoir que les sélections que vous effectuez dans la section disponibilité de la **famille d’appareils** s’appliquent uniquement aux nouvelles acquisitions. Toute personne qui dispose déjà de votre application peut continuer à l’utiliser et obtiendra les mises à jour que vous soumettez, même si vous supprimez la famille de périphériques ici. Cela s’applique même aux clients ayant acquis votre application avant la mise à niveau vers Windows 10. Par exemple, si vous avez une application publiée avec des packages Windows Phone 8,1, et que vous ajoutez un package Windows 10 (UWP) ciblant la famille de périphériques Windows. Universal, les clients Windows 10 mobile qui disposaient de votre package Windows Phone 8,1 recevront une mise à jour de ce package Windows 10 (UWP), même si vous avez désactivé la case à cocher pour **Windows 10 Mobile**.

Pour plus d’informations sur les familles d’appareils, consultez [programmation avec les kits de développement logiciel (SDK) d’extension](/uwp/extension-sdks/device-families-overview).


## <a name="understanding-ranking"></a>Compréhension du classement

Hormis le fait que vous pouvez indiquer les familles d’appareils Windows 10 qui peuvent télécharger votre soumission, la section disponibilité de la **famille d’appareils** affiche les packages spécifiques qui seront mis à la disposition de différentes familles d’appareils. Si vous possédez plusieurs packages pouvant s’exécuter sur une famille d’appareils spécifique, le tableau indique l’ordre de proposition des packages, en fonction de leur numéro de version. Pour plus d’informations sur le classement des packages en fonction de leur numéro de version, consultez la section [Numérotation des versions de packages](package-version-numbering.md). 

Par exemple, imaginons que vous disposez de deux packages : Package_A.appxupload et Package_B.appxupload. Pour une famille d’appareils donnée, si Package_A.appxupload est classé au premier rang et que Package_B.appxupload est classé au second rang, lorsqu’un client sur ce type d’appareil acquiert votre application, le Windows Store tentera dans un premier temps d’offrir Package_A.appxupload. Si l’appareil du client n’est pas en mesure d’exécuter Package_A.appxupload, le Windows Store propose Package_B.appxupload. Si l’appareil du client ne peut pas exécuter l’un des packages pour cette famille d’appareils (par exemple, si le **MinVersion** pris en charge par votre application est supérieur à la version sur l’appareil du client), le client ne pourra pas télécharger l’application sur cet appareil.

> [!NOTE]
> Les numéros de version dans les packages. xap (pour les applications précédemment publiées) ne sont pas pris en compte lors de la détermination du package à fournir à un client donné. Pour cette raison, si vous disposez de plusieurs packages de rang égal, vous verrez un astérisque en lieu et place d’un numéro ; les clients peuvent recevoir les deux packages. Pour mettre à jour les clients d’un package .xap vers une version plus récente, veillez à supprimer l’ancien .xap dans la nouvelle soumission.