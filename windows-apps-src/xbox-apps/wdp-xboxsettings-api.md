---
title: Référence des API des paramètres de développement Xbox Device Portal
description: Découvrez comment accéder aux paramètres Xbox One qui sont utiles pour le développement à l’aide de l’API REST du portail d’appareils Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168983"
---
# <a name="developer-settings-api-reference"></a>Informations de référence sur les API des paramètres de développement

Cette API vous permet d’accéder aux paramètres Xbox One utiles pour le développement.

## <a name="get-all-developer-settings-at-once"></a>Obtenir tous les paramètres de développement à la fois

**Requête**

Vous pouvez utiliser la requête suivante pour obtenir tous les paramètres de développement en une seule requête.

Méthode      | URI de demande
:------     | :-----
GET | /ext/settings

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**

- Aucune

**Lutte**   
La réponse est un tableau de paramètres JSON contenant tous les paramètres. Chaque paramètre inclut les champs suivants :

* Name : (chaîne) le nom du paramètre.
* Value : (chaîne) la valeur du paramètre.
* RequiresReboot : (« Oui » | « Non ») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
* Désactivé-(« Oui » | « Non ») ce champ indique si le paramètre est désactivé et ne peut pas être modifié.
* Category : (chaîne) catégorie du paramètre.
* Type-("texte" | « Nombre » | « Bool » | « SELECT ») ce champ indique le type de paramètre : entrée de texte, valeur booléenne (« true » ou « false »), nombre avec min et Max ou SELECT avec une liste de valeurs spécifique.

Si le paramètre est un nombre :

* Min-(Number) ce champ indique la valeur numérique minimale du paramètre.
* Max-(nombre) ce champ indique la valeur numérique maximale du paramètre.

Si le paramètre est sélectionné :

* OptionsVariable-(« Oui » | « Non ») ce champ indique si les options de paramétrage sont variables, si les options valides peuvent changer sans redémarrage.
* Options : tableau JSON contenant les options Select valides en tant que chaînes.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="get-settings-one-at-a-time"></a>Obtenir les paramètres un par un

Les paramètres peuvent également être récupérés individuellement.

**Requête**

Vous pouvez utiliser la requête suivante pour obtenir des informations sur un paramètre individuel.

Méthode      | URI de demande
:------     | :-----
GET | /ext/settings/\<setting name\>

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**

- Aucune

**Lutte**   
La réponse est un objet JSON avec les champs suivants :

* Name : (chaîne) le nom du paramètre.
* Value : (chaîne) la valeur du paramètre.
* RequiresReboot : (« Oui » | « Non ») ce champ indique si un redémarrage est nécessaire pour que le paramètre prenne effet.
* Désactivé-(« Oui » | « Non ») ce champ indique si le paramètre est désactivé et ne peut pas être modifié.
* Category : (chaîne) catégorie du paramètre.
* Type-("texte" | « Nombre » | « Bool » | « SELECT ») ce champ indique le type de paramètre : entrée de texte, valeur booléenne (« true » ou « false »), nombre avec min et Max ou SELECT avec une liste de valeurs spécifique.

Si le paramètre est un nombre :

* Min-(Number) ce champ indique la valeur numérique minimale du paramètre.
* Max-(nombre) ce champ indique la valeur numérique maximale du paramètre.

Si le paramètre est sélectionné :

* OptionsVariable-(« Oui » | « Non ») ce champ indique si les options de paramétrage sont variables, si les options valides peuvent changer sans redémarrage.
* Options : tableau JSON contenant les options Select valides en tant que chaînes.

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

## <a name="set-the-value-of-a-setting"></a>Définir la valeur d’un paramètre

Vous pouvez définir la valeur d’un paramètre.

**Requête**

Vous pouvez utiliser la requête suivante pour définir la valeur d’un paramètre.

Méthode      | URI de demande
:------     | :-----
PUT | /ext/settings/\<setting name\>

**Paramètres d’URI**

- Aucune

**En-têtes de requête**

- Aucune

**Corps de la demande**   
Le corps de la requête est un objet JSON contenant le champ suivant :   
Value : (chaîne) la nouvelle valeur du paramètre.

**Réponse**   

- Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d'état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

**Familles d’appareils disponibles**

* Windows Xbox