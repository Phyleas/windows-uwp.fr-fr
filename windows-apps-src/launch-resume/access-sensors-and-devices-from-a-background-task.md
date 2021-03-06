---
title: Accéder à des capteurs et des appareils à partir d’une tâche en arrière-plan
description: DeviceUseTrigger permet à votre application Windows universelle d’accéder aux capteurs et aux périphériques en arrière-plan, même si votre application au premier plan est suspendue.
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 1939766c2fbb215980143c33f82bca669dc4df0e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167933"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>Accéder à des capteurs et des appareils à partir d’une tâche en arrière-plan




[**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) permet à votre application Windows universelle d’accéder aux capteurs et aux appareils en arrière-plan, même si votre application d’avant-plan est interrompue. Par exemple, en fonction du lieu où votre application s’exécute, elle peut utiliser une tâche en arrière-plan pour synchroniser les données et les périphériques ou surveiller les capteurs. Pour préserver l’autonomie de la batterie et garantir le consentement de l’utilisateur approprié, l’utilisation de [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) est soumise aux stratégies décrites dans cette rubrique.

Pour accéder aux capteurs ou aux périphériques en arrière-plan, créez une tâche en arrière-plan qui utilise [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). Pour voir comment procéder sur un PC, consultez l’article [Exemple de périphérique USB personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomUsbDeviceAccess). Pour voir comment procéder sur un téléphone, consultez l’article [Exemple de capteurs en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundSensors).

> [!Important]
> **DeviceUseTrigger** ne peut pas être utilisé avec des tâches en arrière-plan intégrées au processus. Les informations fournies dans cette rubrique s’appliquent uniquement aux tâches en arrière-plan qui s’exécutent en dehors du processus.

## <a name="device-background-task-overview"></a>Présentation de la tâche d’appareil en arrière-plan

Lorsque l’utilisateur ne voit plus votre application, Windows la suspend ou l’arrête pour demander de la mémoire et des ressources processeur. Les autres applications peuvent ainsi s’exécuter au premier plan, et la consommation de la batterie s’en trouve réduite. Lorsque cela se produit, sans l’aide d’une tâche en arrière-plan, tous les événements de données en cours seront perdus. Windows fournit le déclencheur de tâche en arrière-plan, [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), qui permet à votre application d’exécuter une longue synchronisation et de surveiller les opérations sur les périphériques et les capteurs de manière sécurisée en arrière-plan, même si votre application est suspendue. Pour plus d’informations sur le cycle de vie des applications, voir [Lancement, reprise et tâches en arrière-plan](index.md). Pour plus d’informations sur les tâches en arrière-plan, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

**Remarque**    Dans une application Windows universelle, la synchronisation d’un appareil en arrière-plan requiert que votre utilisateur ait approuvé la synchronisation en arrière-plan par votre application. L’appareil doit aussi être connecté au PC ou y être couplé, avec les E/S actives, et a droit à un maximum de 10 minutes d’activité en arrière-plan. Vous trouverez plus de détails sur l’application de la stratégie plus loin dans cette rubrique.

### <a name="limitation-critical-device-operations"></a>Limitation : opérations de périphérique critiques

Certaines opérations d’appareil critiques, telles que les mises à jour de microprogrammes longues, ne peuvent pas être effectuées avec le [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). De telles opérations ne peuvent être effectuées que sur le PC, et uniquement par une application privilégiée utilisant le [**DeviceServicingTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger). Une *application privilégiée* est une application autorisée par le fabricant de l’appareil à effectuer ces opérations. Les métadonnées de périphérique permettent de spécifier l’application définie, le cas échéant, comme application privilégiée d’un appareil. Pour plus d’informations, consultez [synchronisation et mise à jour des appareils pour Microsoft Store applications pour appareils](/windows-hardware/drivers/devapps/device-sync-and-update-for-uwp-device-apps).

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>Protocoles/API pris en charge dans une tâche en arrière-plan DeviceUseTrigger

Les tâches en arrière-plan qui utilisent [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) permettent à votre application de communiquer sur de nombreux protocoles/API, dont la plupart ne sont pas pris en charge par les tâches en arrière-plan déclenchées par le système. Les protocoles suivants sont pris en charge dans les applications Windows universelles.

| Protocole         | DeviceUseTrigger dans une application Windows universelle                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| HID              | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth GATT   | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| Réseau câblé    | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| Réseau Wi-Fi    | ![ce protocole est pris en charge.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![deviceservicingtrigger prend en charge ideviceiocontrol](images/ap-tools.png)                                                                                                                       |
| API pour les capteurs      | ![deviceservicingtrigger prend en charge les API de capteurs universels ](images/ap-tools.png) (limitées aux capteurs de la [famille d’appareils universels](../get-started/universal-application-platform-guide.md)) |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>Inscription des tâches en arrière-plan dans le manifeste du package d’application

Votre application effectue les opérations de synchronisation et de mise à jour dans le code qui s’exécute dans le cadre d’une tâche en arrière-plan. Ce code est incorporé dans une classe Windows Runtime qui implémente [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) (ou dans une page JavaScript dédiée pour les applications JavaScript). Pour utiliser une tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), vous devez la déclarer dans le fichier manifeste d’une application au premier plan, comme dans le cas des tâches en arrière-plan déclenchées par le système.

Dans cet exemple de fichier manifeste du package d’application, **DeviceLibrary.SyncContent** est le point d’entrée d’une tâche en arrière-plan qui utilise [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger).

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>Introduction à l’utilisation de DeviceUseTrigger

Pour utiliser [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), suivez ces étapes de base. Pour plus d’informations sur les tâches en arrière-plan, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

1.  Votre application inscrit sa tâche en arrière-plan dans le manifeste de l’application et intègre le code de la tâche en arrière-plan dans une classe Windows Runtime qui implémente IBackgroundTask ou dans une page JavaScript dédiée des applications JavaScript.
2.  Quand votre application démarre, elle crée et configure un objet déclencheur de type [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), puis stocke l’instance du déclencheur en vue d’une utilisation future.
3.  Votre application vérifie si la tâche en arrière-plan a été préalablement inscrite et, si tel n’est pas le cas, l’inscrit par rapport au déclencheur. Notez que votre application n’est pas autorisée à définir de conditions sur la tâche associée au déclencheur.
4.  Lorsque votre application doit déclencher la tâche en arrière-plan, elle doit d’abord appeler [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) pour vérifier si l’application est en mesure de demander une tâche en arrière-plan.
5.  Si l’application peut demander la tâche en arrière-plan, elle appelle la méthode d’activation [**RequestAsync**](/uwp/api/windows.applicationmodel.background.deviceusetrigger.requestasync) sur l’objet de déclenchement de l’appareil.
6.  Votre tâche en arrière-plan n’est pas limitée comme les autres tâches en arrière-plan du système (aucun quota de temps processeur), mais elle s’exécute avec une priorité réduite pour que les applications au premier plan demeurent réactives.
7.  Windows valide alors, en fonction du type de déclencheur, que les stratégies nécessaires ont été satisfaites, y compris la demande de l’accord de l’utilisateur pour l’opération avant le démarrage de la tâche en arrière-plan.
8.  Windows surveille les conditions système et l’exécution de la tâche et, si nécessaire, annule la tâche si les conditions requises ne sont plus satisfaites.
9.  Quand les tâches en arrière-plan signalent une progression ou un achèvement, votre application reçoit ces événements via des événements en cours et terminés sur la tâche inscrite.

**Important**    Tenez compte des points importants suivants lors de l’utilisation de [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger):

-   La possibilité de déclencher par programmation des tâches en arrière-plan qui utilisent le [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) a été introduite pour la première fois dans Windows 8.1 et Windows Phone 8,1.

-   Certaines stratégies sont appliquées par Windows pour s’assurer de l’accord de l’utilisateur lors de la mise à jour des appareils périphériques sur le PC.

-   Des stratégies supplémentaires sont appliquées pour préserver l’autonomie de la batterie de l’utilisateur lors de la synchronisation et de la mise à jour des appareils périphériques.

-   Les tâches en arrière-plan qui utilisent [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) peuvent être annulées par Windows lorsque certaines spécifications de la stratégie ne sont plus satisfaites, y compris la durée maximale en arrière-plan (temps horloge). Il importe de prendre en compte ces spécifications de la stratégie lors de l’utilisation de ces tâches en arrière-plan pour interagir avec votre appareil périphérique.

**Conseil**    Pour voir comment ces tâches en arrière-plan fonctionnent, téléchargez un exemple. Pour voir comment procéder sur un PC, consultez l’article [Exemple de périphérique USB personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomUsbDeviceAccess). Pour voir comment procéder sur un téléphone, consultez l’article [Exemple de capteurs en arrière-plan](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundSensors).
 
## <a name="frequency-and-foreground-restrictions"></a>Restrictions de fréquence et de premier plan

Il n’existe aucune restriction quant à la fréquence à laquelle votre application peut initier des opérations, mais votre application ne peut exécuter qu’une seule opération de tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) à la fois (cela n’affecte pas les autres types de tâches en arrière-plan) et peut lancer une tâche en arrière-plan uniquement lorsque votre application est au premier plan. Lorsque votre application ne se trouve pas au premier plan, elle ne peut pas initier une tâche en arrière-plan avec **DeviceUseTrigger**. Votre application ne peut pas initier une seconde tâche **DeviceUseTrigger** en arrière-plan tant que la première tâche en arrière-plan n’est pas terminée.

## <a name="device-restrictions"></a>Restrictions d’appareil

Alors que chaque application est limitée à l’inscription et à l’exécution d’une seule tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), le périphérique (sur lequel votre application s’exécute) peut autoriser plusieurs applications à inscrire et à exécuter des tâches en arrière-plan **DeviceUseTrigger**. Selon le périphérique, il peut exister une limite quant au nombre total de tâches en arrière-plan **DeviceUseTrigger** de toutes les applications. Cela permet d’économiser la batterie sur les appareils avec restriction de ressources. Pour plus d’informations, voir le tableau suivant.

À partir d’une même tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), votre application peut accéder à un nombre illimité de périphériques ou de capteurs, la seule limite étant celle des API et protocoles pris en charge répertoriés dans le tableau précédent.

## <a name="background-task-policies"></a>Stratégies de tâche en arrière-plan

Windows applique des stratégies quand votre application utilise une tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) . Si ces stratégies ne sont pas satisfaites, la tâche en arrière-plan peut être annulée. Il importe de prendre en compte ces spécifications de stratégie lors de l’utilisation de ce type de tâche en arrière-plan pour interagir avec les périphériques ou les capteurs.

### <a name="task-initiation-policies"></a>Stratégies d’initiation de tâche

Ce tableau indique les stratégies d’initiation de tâche applicables à une application Windows universelle.

| Stratégie | DeviceUseTrigger dans une application Windows universelle |
|--------|---------------------------------------------|
| Votre application est au premier plan lors du déclenchement de la tâche en arrière-plan. | ![la stratégie s’applique](images/ap-tools.png) |
| Le périphérique est attaché au système (ou dans la plage d’un périphérique sans fil). | ![la stratégie s’applique](images/ap-tools.png) |
| Le périphérique est accessible à l’application utilisant les API de périphérique prises en charge (API Windows Runtime pour USB, HID, Bluetooth, Capteurs, etc). Si votre application ne peut pas accéder au périphérique ou au capteur, l’accès à la tâche en arrière-plan est refusé. | ![la stratégie s’applique](images/ap-tools.png) |
| Le point d’entrée de la tâche en arrière-plan fourni par l’application est inscrit dans le manifeste du package d’application. | ![la stratégie s’applique](images/ap-tools.png) |
| Une seule tâche en arrière-plan [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) est exécutée par application. | ![la stratégie s’applique](images/ap-tools.png) |
| Le nombre maximal de tâches en arrière-plan [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) n’a pas encore été atteint sur le périphérique (sur lequel votre application s’exécute). | **Famille d’appareils de bureau**: un nombre illimité de tâches peut être enregistré et exécuté en parallèle. **Famille d’appareils mobiles**: 1 tâche sur un appareil 512 Mo ; dans le cas contraire, 2 tâches peuvent être inscrites et exécutées en parallèle. |
| Nombre maximal de périphériques ou de capteurs auxquels votre application peut accéder à partir d’une seule et même tâche en arrière-plan [DeviceUseTrigger](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger), lors de l’utilisation des API/protocoles pris en charge. | illimitée |
| Votre tâche en arrière-plan consomme 400 ms de temps processeur (dans l’hypothèse d’un processeur 1 GHz) toutes les minutes quand l’écran est verrouillé ou toutes les cinq minutes dans le cas contraire. L’impossibilité de satisfaire cette stratégie peut entraîner une annulation de votre tâche. | ![la stratégie s’applique](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>Contrôles de stratégie runtime

Windows applique les spécifications de stratégie runtime suivantes tandis que votre tâche s’exécute en arrière-plan. Si l’une des spécifications de la stratégie runtime cesse d’être vraie, Windows annule la tâche en arrière-plan de votre périphérique.

Ce tableau indique les stratégies runtime applicables à une application Windows universelle.

| Contrôle de la stratégie | DeviceUseTrigger dans une application Windows universelle |
|--------------|:-------------------------------------------:|
| Le périphérique est attaché au système (ou dans la plage d’un périphérique sans fil). | ![le contrôle de stratégie s’applique](images/ap-tools.png) |
| La tâche exécute des E/S régulières sur le périphérique (1 E/S toutes les 5 secondes). | ![le contrôle de stratégie s’applique](images/ap-tools.png) |
| L’application n’a pas annulé la tâche. | ![le contrôle de stratégie s’applique](images/ap-tools.png) |
| Limite de temps horloge : durée totale pendant laquelle la tâche de votre application peut s’exécuter en arrière-plan. | **Famille d’appareils de bureau**: 10 minutes. **Famille d’appareils mobiles**: aucune limite de durée. Pour économiser les ressources, le nombre de tâches exécutées simultanément doit être limité à 1 ou 2. |
| L’application ne s’est pas terminée. | ![le contrôle de stratégie s’applique](images/ap-tools.png) |

## <a name="best-practices"></a>Meilleures pratiques

Voici les meilleures pratiques pour les applications qui utilisent les tâches d’arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) .

### <a name="programming-a-background-task"></a>Programmation d’une tâche en arrière-plan

L’utilisation de la tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) de votre application garantit que les opérations de synchronisation ou de surveillance démarrées à partir de votre application au premier plan continuent de s’exécuter à l’arrière-plan si vos utilisateurs changent d’application et que votre application au premier plan est suspendue par Windows. Nous vous recommandons de suivre ce modèle global pour l’inscription, le déclenchement et l’annulation de l’inscription de vos tâches en arrière-plan :

1.  Appelez [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) pour vérifier si l’application est en mesure de demander une tâche en arrière-plan. Cela doit être fait avant l’inscription d’une tâche en arrière-plan.

2.  Inscrivez la tâche en arrière-plan avant de demander le déclencheur.

3.  Connectez les gestionnaires des événements de progression et de fin à votre déclencheur. Lorsque votre application reprend après une suspension, Windows lui fournit les événements de progression ou de fin en file d’attente qui peuvent être utilisés pour déterminer l’état de vos tâches en arrière-plan.

4.  Fermez les objets capteur ou périphérique ouverts lorsque vous déclenchez votre tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) afin que ces périphériques ou capteurs puissent être librement ouverts et utilisés par votre tâche en arrière-plan.

5.  Inscrivez le déclencheur.

6.  Considérez soigneusement l’impact sur la batterie de l’accès à un périphérique ou à un capteur depuis une tâche en arrière-plan. Par exemple, l’exécution trop fréquente de l’intervalle de rapport d’un capteur pourrait entraîner une exécution de la tâche si fréquente qu’elle viderait rapidement la batterie du téléphone.

7.  Lorsque votre tâche en arrière-plan est terminée, annulez son inscription.

8.  Inscrivez les événements d’annulation à partir de votre classe de tâches en arrière-plan. L’inscription des événements d’annulation permet au code de votre tâche d’exécution d’arrêter proprement l’exécution de votre tâche en arrière-plan quand elle est annulée par Windows ou votre application en premier plan.

9.  À la sortie de l’application (pas à sa suspension), annulez l’inscription des tâches en cours d’exécution, ainsi que les tâches elles-mêmes, si votre application n’en a plus besoin. Sur les systèmes avec restriction de ressources, comme les téléphones disposant de peu de mémoire, cela permet aux autres applications d’utiliser une tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger).

    -   Lorsque vous quittez l’application, annulez l’inscription des tâches en cours d’exécution, ainsi que les tâches elles-mêmes.

    -   Lorsque vous quittez l’application, vos tâches en arrière-plan sont annulées et les gestionnaires d’événement existants sont déconnectés de vos tâches en arrière-plan existantes. Cela vous empêche de déterminer l’état de vos tâches en arrière-plan. L’annulation de l’inscription et l’annulation de la tâche en arrière-plan permettent à votre code d’annulation d’arrêter proprement vos tâches en arrière-plan.

### <a name="cancelling-a-background-task"></a>Annulation d’une tâche en arrière-plan

Pour annuler une tâche s’exécutant en arrière-plan de votre application au premier plan, utilisez la méthode Unregister sur l’objet [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) que vous utilisez dans votre application pour inscrire la tâche en arrière-plan [**DeviceUseTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). L’annulation de l’inscription de votre tâche en arrière-plan à l’aide de la méthode [**Unregister**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister) sur **BackgroundTaskRegistration** entraîne l’annulation de votre tâche en arrière-plan par l’infrastructure des tâches en arrière-plan.

La méthode [**Unregister**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister) prend en outre une valeur booléenne true ou false pour indiquer si les instances en cours d’exécution de votre tâche en arrière-plan doivent être annulées sans les autoriser à se terminer. Pour plus d’informations, voir la référence d’API pour la méthode **Unregister**.

En plus d' [**Annuler l’inscription**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.unregister), votre application doit également appeler [**BackgroundTaskDeferral. Complete**](/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete). Le système est ainsi informé que l’opération asynchrone associée à une tâche en arrière-plan a pris fin.