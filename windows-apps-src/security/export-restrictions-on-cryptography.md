---
title: Restrictions à l’exportation liées à l’utilisation du chiffrement
description: Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Microsoft Store.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: c647d91213ddf1fd8a3dafd80c6888a026cda576
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258936"
---
# <a name="export-restrictions-on-cryptography"></a>Restrictions à l’exportation liées à l’utilisation du chiffrement



Utilisez ces informations pour déterminer si votre application emploie un type de chiffrement qui pourrait l’empêcher de figurer dans le Microsoft Store.

Le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS) du Département du Commerce des États-Unis réglemente l’exportation des technologies qui utilisent certains types de chiffrements. Toutes les applications répertoriées dans le Microsoft Store doivent respecter ces lois et réglementations, car les fichiers d’application peuvent être stockés aux États-Unis. Même les applications qui sont chargées par les développeurs d’applications depuis d’autres pays en vue d’une diffusion hors des États-Unis doivent respecter ces réglementations. Lors de la soumission d’une application sur le Microsoft Store, tous les développeurs d’application doivent s’assurer que leurs applications ne contiennent pas de technologie limitée par ces réglementations.

> **Notez**  les informations fournies ici fournissent des instructions, mais il vous incombe en tant que développeur d’applications qui publie des applications dans le Microsoft Store de s’assurer que votre application est conforme à toutes les lois et réglementations applicables.

 

Pour obtenir plus d’informations sur le Département du Commerce des États-Unis et le « Bureau of Industry and Security » (Bureau de l’industrie et de la sécurité ou BIS), voir [À propos du Bureau de l’industrie et de la sécurité](https://www.bis.doc.gov/about/index.htm).

Pour plus d’informations sur la réglementation en vigueur aux États-Unis en matière de contrôle des exportations (« Export Administration Regulations » ou EAR) en ce qui concerne les technologies utilisant le chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="governed-uses"></a>Utilisations régies

Premièrement, déterminez si votre application utilise un type de chiffrement soumis à la réglementation en vigueur aux États-Unis en matière de contrôle des exportations (« Export Administration Regulations » ou EAR). La question inclut les exemples affichés dans la liste à cet endroit, mais rappelez-vous que cette liste ne reprend pas toutes les applications possibles du chiffrement.

> **Important**  envisagez non seulement le code que vous avez écrit pour votre application, mais également toutes les bibliothèques logicielles, les utilitaires et les composants du système d’exploitation que votre application contient ou auquel elle est liée.

-   Toute utilisation d’une signature numérique, telle que l’authentification ou la vérification de l’intégrité
-   Chiffrement de données ou de fichiers utilisés par votre application
-   Gestion de clés, gestion de certificats ou toute autre utilisation qui interagit avec une infrastructure à clé publique
-   Utilisation d’un canal de communication sécurisé, tel que NTLM, Kerberos, SSL (Secure Sockets Layer) ou TLS (Transport Layer Security)
-   Chiffrement de mots de passe ou autres formes de sécurité des informations
-   Protection contre la copie ou gestion des droits numériques (DRM)
-   Protection antivirus

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="non-restricted-uses"></a>Utilisations non restreintes

Notez que certaines applications de chiffrement ne sont pas restreintes. Les tâches non restreintes sont les suivantes :

-   Chiffrement de mot de passe
-   Protection contre la copie
-   Authentification
-   Gestion des droits numériques (DRM)
-   Utilisation de signatures numériques

Pour obtenir la liste complète et actualisée des applications de chiffrement, voir [Contrôles EAR des éléments utilisant le chiffrement](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

Si votre application appelle, prend en charge, contient ou utilise un chiffrement pour une tâche quelconque ne figurant pas dans cette liste, elle a besoin d’un numéro ECCN (Export Commodity Classification Number).

Si vous ne possédez pas de numéro ECCN, consultez l’article [Questions et réponses sur les numéros ECCN](https://www.bis.doc.gov/licensing/do_i_needaneccn.html).
