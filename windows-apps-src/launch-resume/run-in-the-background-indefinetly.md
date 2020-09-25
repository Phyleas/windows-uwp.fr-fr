---
title: Exécuter indéfiniment en arrière-plan
description: Utilisez la fonctionnalité extendedExecutionUnconstrained pour exécuter une tâche en arrière-plan ou une session d’exécution étendue en arrière-plan indéfiniment.
ms.assetid: 6E48B8B6-D3BF-4AE2-85FB-D463C448C9D3
keywords: tâche en arrière-plan, exécution étendue, ressources, limites, tâche en arrière-plan
ms.date: 10/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f843c23a4a1e0738cfc05e96009b2597f4919809
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217652"
---
# <a name="run-in-the-background-indefinitely"></a>Exécuter indéfiniment en arrière-plan

Pour offrir la meilleure expérience aux utilisateurs, Windows impose des limites de ressources sur les applications de plateforme Windows universelle (UWP). Les applications de premier plan disposent de la plus grande partie de la mémoire et de la durée d’exécution. les applications en arrière-plan sont moins. Les utilisateurs sont donc protégés contre les performances médiocres des applications de premier plan et la charge importante de la batterie.

Toutefois, les développeurs qui écrivent des applications UWP à des fins personnelles (autrement dit, des applications qui ne seront pas publiées dans le Microsoft Store) ou des développeurs écrivant des applications UWP d’entreprise peuvent utiliser toutes les ressources disponibles sur l’appareil sans aucun arrière-plan ou limitation d’exécution étendue. Les applications UWP professionnelles et personnelles peuvent utiliser des API dans Windows Creators Update (version 1703) pour désactiver la limitation. N’oubliez pas que vous ne pouvez pas placer une application dans le Microsoft Store si elle utilise ces API.

## <a name="run-while-minimized"></a>Exécution en mode réduit

Les applications UWP passent à l’état suspendu lorsqu’elles ne s’exécutent pas au premier plan. Sur le bureau, cela se produit lorsqu’un utilisateur minimise l’application. Les applications utilisent une session d’exécution étendue pour continuer à s’exécuter pendant qu’elle est réduite. Les API d’exécution étendues acceptées par le Microsoft Store sont détaillées dans [reporter la suspension d’application avec une exécution étendue](./run-minimized-with-extended-execution.md).

Si vous développez une application qui n’est pas destinée à être envoyée dans le Microsoft Store, vous pouvez utiliser le [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) avec la `extendedExecutionUnconstrained` fonctionnalité restreinte afin que votre application puisse continuer à s’exécuter tout en minimisant, quel que soit l’état d’énergie de l’appareil.  

La `extendedExecutionUnconstrained` fonctionnalité est ajoutée en tant que fonctionnalité restreinte dans le manifeste de votre application. Pour plus d’informations sur les fonctionnalités restreintes, consultez déclarations de fonctionnalités d' [application](../packaging/app-capability-declarations.md) .

> [!NOTE]
> Ajoutez la déclaration d’espace de noms XML *xmlns : ResCap* et utilisez le préfixe *ResCap* pour déclarer la fonctionnalité.
>
> Pour plus d’informations, consultez la section fonctionnalités restreintes des [déclarations de fonctionnalités d’application](../packaging/app-capability-declarations.md).
>

_Package.appxmanifest_

```xml
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
  ...
  <Capabilities>
    <rescap:Capability Name="extendedExecutionUnconstrained"/>
  </Capabilities>
</Package>
```

Lorsque vous utilisez la `extendedExecutionUnconstrained` fonctionnalité, [ExtendedExecutionForegroundSession](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession) et [ExtendedExecutionForegroundReason](/uwp/api/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason) sont utilisés au lieu de [ExtendedExecutionSession](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionsession) et [ExtendedExecutionReason](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason). Le même modèle pour la création de la session, la définition de membres et la demande de l’extension de manière asynchrone s’applique toujours : 

```cs
var newSession = new ExtendedExecutionForegroundSession();
newSession.Reason = ExtendedExecutionForegroundReason.Unconstrained;
newSession.Description = "Long Running Processing";
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();
switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```

Vous pouvez demander cette session d’exécution étendue dès que l’application arrive au premier plan. Les sessions d’exécution étendues non restreintes ne sont pas limitées par les quotas d’énergie ou par l’économiseur de batterie du système d’exploitation. Tant qu’il existe une référence à l’objet de session, l’application reste à l’État en cours d’exécution et ne passe pas à l’état suspendu. Si l’application est fermée par l’utilisateur, la session est révoquée.

L’inscription de l’événement **révoqué** permettra à votre application d’effectuer toutes les tâches de nettoyage requises. Dans l’état de suspension, vous pouvez créer une session d’exécution étendue avec   [ExtendedExecutionReason. SavingData](/uwp/api/windows.applicationmodel.extendedexecution.extendedexecutionreason) pour enregistrer les données utilisateur avant que l’application ne soit terminée et supprimée de la mémoire.

## <a name="run-background-tasks-indefinitely"></a>Exécuter des tâches en arrière-plan indéfiniment

Dans le plateforme Windows universelle, les tâches en arrière-plan sont des processus qui s’exécutent en arrière-plan sans aucune forme d’interface utilisateur. Les tâches en arrière-plan peuvent généralement s’exécuter pendant vingt-cinq secondes au maximum avant d’être annulées. Certaines des tâches dont l’exécution est plus longue ont également une vérification pour s’assurer que la tâche en arrière-plan n’est pas inactive ou utilise la mémoire. Dans Windows Creators Update (version 1703), la fonctionnalité [extendedBackgroundTaskTime](../packaging/app-capability-declarations.md) Restricted a été introduite pour supprimer ces limites. La fonctionnalité **extendedBackgroundTaskTime** est ajoutée en tant que fonctionnalité restreinte dans le fichier manifeste de votre application :

> [!NOTE]
> Ajoutez la déclaration d’espace de noms XML *xmlns : ResCap* et utilisez le préfixe *ResCap* pour déclarer la fonctionnalité.
>
> Pour plus d’informations, consultez la section fonctionnalités restreintes des [déclarations de fonctionnalités d’application](../packaging/app-capability-declarations.md).
>

_Package.appxmanifest_

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp rescap">
...
  <Capabilities>
    <rescap:Capability Name="extendedBackgroundTaskTime"/>
  </Capabilities>
</Package>
```

Cette fonctionnalité supprime les limites de temps d’exécution et la surveillance des tâches inactives. Une fois qu’une tâche en arrière-plan a démarré, qu’il s’agisse d’un déclencheur ou d’un appel App service, une fois qu’elle a effectué un report sur le [BackgroundTaskInstance](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) fourni par la méthode **Run** , elle peut s’exécuter indéfiniment. Si l’application est configurée pour être **gérée par Windows**, il est possible qu’un quota d’énergie lui soit appliqué et que ses tâches en arrière-plan ne soient pas activées lorsque l’économiseur de batterie est actif.Cela peut être modifié avec les paramètres de système d’exploitation. Des informations supplémentaires sont disponibles dans optimisation de l' [activité en arrière-plan](../debug-test-perf/optimize-background-activity.md).

Le plateforme Windows universelle surveille l’exécution des tâches en arrière-plan pour garantir une bonne autonomie de la batterie et une expérience d’application de premier plan lisse. Toutefois, les applications personnelles et les applications métier d’entreprise peuvent utiliser l’exécution étendue et la fonction **extendedBackgroundTaskTime** pour créer des applications qui s’exécuteront aussi longtemps que nécessaire, quelle que soit la disponibilité des ressources de l’appareil.

Sachez que les fonctionnalités **extendedExecutionUnconstrained** et **extendedBackgroundTaskTime** peuvent remplacer la stratégie par défaut pour les applications UWP et peuvent entraîner une vidange importante de la batterie. Avant d’utiliser ces fonctionnalités, vérifiez d’abord que les stratégies d’exécution étendue et de temps de tâche en arrière-plan par défaut ne répondent pas à vos besoins et effectuez des tests dans des conditions restreintes à la batterie pour comprendre l’impact que votre application aura sur un appareil.

## <a name="see-also"></a>Voir aussi

[Supprimer les restrictions de ressources de tâche en arrière-plan](/windows/application-management/enterprise-background-activity-controls)