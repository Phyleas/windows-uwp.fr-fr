---
description: L’API JavaScript pour Microsoft Examen vous permet de sécuriser les évaluations. L’application Examen inclut un navigateur sécurisé qui empêche les étudiants d’utiliser un autre ordinateur ou des ressources Internet pendant l’examen.
title: API JavaScript Examen.
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, éducation
ms.localizationpriority: medium
ms.openlocfilehash: d4ed3bf3062deac308b9ed39ff7be709bcee4af3
ms.sourcegitcommit: 3be258523c5ee3666498d6a98ed2648b27b2907d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99973099"
---
# <a name="take-a-test-javascript-api"></a>API JavaScript Examen

[Un test](/education/windows/take-tests-in-windows-10) est une application UWP basée sur un navigateur qui rend les évaluations en ligne verrouillées pour les tests à grande importance, ce qui permet aux enseignants de se concentrer sur le contenu d’évaluation plutôt que sur la façon de fournir un environnement de test sécurisé. Pour ce faire, il utilise une API JavaScript qui peut être utilisée par n’importe quelle application Web. L’API Take-a-test prend en charge la [norme API du navigateur SBAC](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) pour les tests de base courants les plus importants.

Pour plus d’informations sur l’application elle-même, consultez la [référence technique de l’application Take a test](/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) . Pour obtenir de l’aide pour la résolution des problèmes, voir [Résoudre les problèmes de Microsoft Examen avec l’observateur d’événements](troubleshooting.md).

> [!NOTE]
> Cet article contient des références au terme liste rouge, un terme que Microsoft n’utilise plus. Lorsque le terme sera supprimé du logiciel, nous le supprimerons de cet article.


## <a name="reference-documentation"></a>Documentation de référence
Les API Take a test existent dans les espaces de noms suivants. Notez que toutes les API dépendent d’un objet global `SecureBrowser` .

| Espace de noms | Description |
|-----------|-------------|
|[espace de noms de sécurité](#security-namespace)|Contient des API qui vous permettent de verrouiller l’appareil pour le test et d’appliquer un environnement de test. |

### <a name="security-namespace"></a>Espace de noms de sécurité

L’espace de noms de sécurité vous permet de verrouiller l’appareil, de vérifier la liste des processus utilisateur et système, d’obtenir des adresses MAC et IP, ainsi que de supprimer les ressources Web mises en cache.

| Méthode | Description   |
|--------|---------------|
|[lockDown](#lockDown) | Verrouille l’appareil à des fins de test. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Détermine si le contexte de verrouillage est toujours appliqué à l’appareil. |
|[getDeviceInfo](#getDeviceInfo) | Obtient des détails sur la plateforme sur laquelle l’application de test s’exécute. |
|[examineProcessList](#examineProcessList)|Obtient la liste des processus utilisateur et système en cours d’exécution.|
|[close](#close) | Ferme le navigateur et déverrouille l’appareil. |
|[getPermissiveMode](#getPermissiveMode)|Vérifie si le mode permissif est activé ou désactivé.|
|[setPermissiveMode](#setPermissiveMode)|Active ou désactive le mode permissif.|
|[emptyClipBoard](#emptyClipBoard)|Efface le presse-papiers du système.|
|[getMACAddress](#getMACAddress)|Obtient la liste des adresses MAC pour l’appareil.|
|[getStartTime](#getStartTime) | Obtient l’heure à laquelle l’application de test a été démarrée. |
|[getCapability](#getCapability) | Interroge si une fonctionnalité est activée ou désactivée. |
|[setCapability](#setCapability)|Active ou désactive la fonctionnalité spécifiée.| 
|[isRemoteSession](#isRemoteSession) | Vérifie si la session active est connectée à distance. |
|[isVMSession](#isVMSession) | Vérifie si la session active est en cours d’exécution sur une machine virtuelle. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
Verrouille l’appareil. Également utilisé pour déverrouiller l’appareil. L’application Web de test appellera cet appel avant de permettre aux élèves de démarrer le test. L’implémenteur est tenu d’effectuer toutes les actions nécessaires pour sécuriser l’environnement de test. Les étapes prises pour sécuriser l’environnement sont spécifiques aux appareils et, par exemple, incluent des aspects tels que la désactivation des captures d’écran, la désactivation de la conversation vocale en mode sécurisé, l’effacement du presse-papiers du système, l’entrée en mode plein écran, la désactivation des espaces dans les appareils OSX 10.7 +, etc. L’application de test activera le verrouillage avant le début d’une évaluation et désactivera le verrouillage lorsque l’étudiant aura terminé l’évaluation et n’aura plus le test sécurisé.

**Syntaxe**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Paramètres**  
* `enable` - **true** pour exécuter l’application Take-a-test au-dessus de l’écran de verrouillage et appliquer les stratégies présentées dans ce [document](/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** arrête l’exécution de Take-a-test au-dessus de l’écran de verrouillage et le ferme à moins que l’application ne soit verrouillée ; dans ce cas, il n’y a aucun effet.  
* `onSuccess` -[facultatif] fonction à appeler après l’activation ou la désactivation du verrouillage. Elle doit être au format `Function(Boolean currentlockdownstate)` .  
* `onError` -[facultatif] fonction à appeler en cas d’échec de l’opération de verrouillage. Elle doit être au format `Function(Boolean currentlockdownstate)` .  

**Configuration requise**  
Windows 10, version 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Détermine si le contexte de verrouillage est toujours appliqué à l’appareil. L’application Web de test appellera cela avant de permettre aux élèves de commencer à tester et périodiquement à l’intérieur du test.

**Syntaxe**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Paramètres**  
* `callback` : Fonction à appeler lorsque cette fonction est terminée. Il doit être de la forme `Function(String state)` où `state` est une chaîne JSON contenant deux champs. La première est le `secure` champ, qui s’affiche `true` uniquement si tous les verrous nécessaires ont été activés (ou les fonctionnalités désactivées) pour activer un environnement de test sécurisé et qu’aucun d’entre eux n’a été compromis depuis que l’application est entrée dans le mode de verrouillage. L’autre champ, `messageKey` , comprend d’autres détails ou informations spécifiques au fournisseur. L’objectif ici est de permettre aux fournisseurs de placer des informations supplémentaires qui augmentent l' `secure` indicateur booléen :

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Configuration requise**  
Windows 10, version 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Obtient des détails sur la plateforme sur laquelle l’application de test s’exécute. Cela permet d’augmenter les informations qui étaient perceptibles par l’agent utilisateur.

**Syntaxe**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Paramètres**  
* `callback` : Fonction à appeler lorsque cette fonction est terminée. Il doit être de la forme `Function(String infoObj)` où `infoObj` est une chaîne JSON contenant plusieurs champs. Les champs suivants doivent être pris en charge :
    * `os` représente le type de système d’exploitation (par exemple : Windows, macOS, Linux, iOS, Android, etc.)
    * `name` représente le nom de la version du système d’exploitation, le cas échéant (par exemple, Sierra, Ubuntu).
    * `version` représente la version du système d’exploitation (par exemple : 10,1, 10 Pro, etc.)
    * `brand` représente la personnalisation de navigateur sécurisée (par exemple : OAKS, CA, SmarterApp, etc.)
    * `model` représente le modèle d’appareil pour les périphériques mobiles uniquement ; NULL/non utilisé pour les navigateurs de bureau.

**Configuration requise**  
Windows 10, version 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtient la liste de tous les processus en cours d’exécution sur l’ordinateur client appartenant à l’utilisateur. L’application de test l’appellera pour examiner la liste et la comparer à la liste des processus qui ont été jugés bloqués pendant le cycle de test. Cet appel doit être appelé à la fois au début d’une évaluation et périodiquement pendant que l’étudiant prend l’évaluation. Si un processus bloqué est détecté, l’évaluation doit être arrêtée pour préserver l’intégrité du test.

**Syntaxe**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Paramètres**  
* `blacklistedProcessList` : Liste des processus que l’application de test a bloqués.  
`callback` : Fonction à appeler une fois les processus actifs trouvés. Il doit se présenter sous la forme :, `Function(String foundBlacklistedProcesses)` où `foundBlacklistedProcesses` est au format : `"['process1.exe','process2.exe','processEtc.exe']"` . Il sera vide si aucun processus bloqué n’a été trouvé. Si la valeur est null, cela indique qu’une erreur s’est produite dans l’appel de fonction d’origine.

**Remarques** La liste n’inclut pas les processus système.

**Configuration requise**  
Windows 10, version 1709

---

<span id="close"/>

### <a name="close"></a>close
Ferme le navigateur et déverrouille l’appareil. L’application de test doit appeler cette méthode lorsque l’utilisateur choisit de quitter le navigateur.

**Syntaxe**  
`void SecureBrowser.security.close(restart);`

**Paramètres**  
* `restart` -Ce paramètre est ignoré, mais il doit être fourni.

**Remarques** Dans Windows 10, version 1607, l’appareil doit être verrouillé à l’origine. Dans les versions ultérieures, cette méthode ferme le navigateur, que l’appareil soit verrouillé ou non.

**Configuration requise**  
Windows 10, version 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
L’application Web de test doit appeler cette méthode pour déterminer si le mode permissif est activé ou désactivé. En mode permissif, un navigateur est supposé assouplir certains de ses hameçons de sécurité stricts pour permettre aux technologies d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les autres interfaces utilisateur d’application de s’en présenter peuvent souhaiter s’assouplir quand ils sont en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Paramètres**  
* `callback` : Fonction à appeler à la fin de cet appel. Il doit se présenter sous la forme : `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle n’est pas définie ou a la valeur null, une erreur s’est produite lors de l’opération d’extraction.

**Configuration requise**  
Windows 10, version 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
L’application Web de test doit appeler cette méthode pour activer ou désactiver le mode permissif. En mode permissif, un navigateur est supposé assouplir certains de ses hameçons de sécurité stricts pour permettre aux technologies d’assistance de fonctionner avec le navigateur sécurisé. Par exemple, les navigateurs qui empêchent de manière agressive les autres interfaces utilisateur d’application de s’en présenter peuvent souhaiter s’assouplir quand ils sont en mode permissif. 

**Syntaxe**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Paramètres**  
* `enable` -Valeur booléenne indiquant l’état du mode permissif prévu.  
* `callback` : Fonction à appeler à la fin de cet appel. Il doit se présenter sous la forme : `Function(Boolean permissiveMode)` où `permissiveMode` indique si le navigateur est actuellement en mode permissif. Si elle n’est pas définie ou a la valeur null, une erreur s’est produite dans l’opération de définition.

**Configuration requise**  
Windows 10, version 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Efface le presse-papiers du système. L’application de test doit appeler cette méthode pour forcer l’effacement des données qui peuvent être stockées dans le presse-papiers du système. La fonction de **[verrouillage](#lockDown)** effectue également cette opération.

**Syntaxe**  
`void SecureBrowser.security.emptyClipBoard();`

**Configuration requise**  
Windows 10, version 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtient la liste des adresses MAC pour l’appareil. L’application de test doit l’appeler pour faciliter le diagnostic. 

**Syntaxe**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Paramètres**  
* `callback` : Fonction à appeler à la fin de cet appel. Il doit se présenter sous la forme :, `Function(String addressArray)` où `addressArray` est au format : `"['00:11:22:33:44:55','etc']"` .

**Remarques**  
Il est difficile de faire la distinction entre les adresses IP source et les ordinateurs des utilisateurs finaux dans les serveurs de test, car les pare-feu/NAT/proxys sont couramment utilisés dans les écoles. Les adresses MAC permettent à l’application de distinguer les ordinateurs clients finaux situés derrière un pare-feu commun à des fins de diagnostic.

**Configuration requise**  
Windows 10, version 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtient l’heure à laquelle l’application de test a été démarrée.

**Syntaxe**  
`DateTime SecureBrowser.security.getStartTime();`

**Renvoi**  
Objet DateTime indiquant l’heure à laquelle l’application de test a été démarrée.

**Configuration requise**  
Windows 10, version 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Interroge si une fonctionnalité est activée ou désactivée. 

**Syntaxe**  
`Object SecureBrowser.security.getCapability(String feature)`

**Paramètres**  
`feature` -Chaîne permettant de déterminer la capacité à interroger. Les chaînes de fonctionnalité valides sont « screenMonitoring », « Printing » et « textSuggestions » (non-respect de la casse).

**Valeur de retour**  
Cette fonction retourne soit un objet JavaScript, soit un littéral au format : `{<feature>:true|false}` . **true** si la fonctionnalité d’interrogation est activée, **false** si la fonctionnalité n’est pas activée ou si la chaîne de capacité n’est pas valide.

**Configuration requise** Windows 10, version 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Active ou désactive une fonctionnalité spécifique dans le navigateur.

**Syntaxe**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Paramètres**  
* `feature` -Chaîne permettant de déterminer la capacité à définir. Les chaînes de fonctionnalité valides sont `"screenMonitoring"` , et (non-respect `"printing"` de la `"textSuggestions"` casse).  
* `value` -Paramètre prévu pour la fonctionnalité. Il doit s’agir `"true"` de ou `"false"` .  
* `onSuccess` -[facultatif] fonction à appeler une fois l’opération ensembliste terminée. Elle doit être au format `Function(String jsonValue)` où *jsonValue* se présente sous la forme : `{<feature>:true|false|undefined}` .  
* `onError` -[facultatif] fonction à appeler en cas d’échec de l’opération de définition. Elle doit être au format `Function(String jsonValue)` où *jsonValue* se présente sous la forme : `{<feature>:true|false|undefined}` .

**Remarques**  
Si la fonctionnalité ciblée est inconnue du navigateur, cette fonction passe une valeur `undefined` à la fonction de rappel.

**Configuration requise** Windows 10, version 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Vérifie si la session active est connectée à distance.

**Syntaxe**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valeur de retour**  
**true** si la session active est distante ; sinon, **false**.

**Configuration requise**  
Windows 10, version 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Vérifie si la session active est en cours d’exécution au sein d’une machine virtuelle.

**Syntaxe**  
`Boolean SecureBrowser.security.isVMSession();`

**Valeur de retour**  
**true** si la session active est en cours d’exécution sur un ordinateur virtuel, sinon **false**.

**Remarques**  
Cette vérification de l’API ne peut détecter que les sessions de machines virtuelles qui s’exécutent dans certains hyperviseurs qui implémentent les API appropriées

**Configuration requise**  
Windows 10, version 1709

---
