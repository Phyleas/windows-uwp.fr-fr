---
title: Optimisation et Rubriques avancées pour les jeux DirectX
description: Consultez les articles qui fournissent des informations sur l’optimisation des performances de votre jeu DirectX et d’autres rubriques avancées.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Game, DirectX, optimiser, échantillonnage multiple, chaînes de permutation
ms.localizationpriority: medium
ms.openlocfilehash: f19081a2618ea683c7790db3cd7517078f19269c
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053649"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimisation et Rubriques avancées pour les jeux DirectX

Cette section fournit des informations sur l’optimisation des performances de votre jeu DirectX et d’autres rubriques avancées.

La rubrique programmation asynchrone pour les jeux décrit les différents points à prendre en compte lorsque vous souhaitez utiliser la programmation asynchrone pour paralléliser certains composants et utiliser le multithreading pour maximiser l’utilisation d’un GPU puissant.

Gérer les scénarios de suppression d’appareils dans la rubrique Direct3D 11 utilise une procédure pas à pas pour expliquer comment les jeux développés à l’aide de Direct3D 11 peuvent détecter et répondre aux situations dans lesquelles la carte graphique est réinitialisée, supprimée ou modifiée.

L’échantillonnage multiple dans les applications UWP fournit une vue d’ensemble de l’utilisation de l’anticrénelage à plusieurs échantillons, une technique graphique pour réduire l’apparence des bords avec des alias dans les jeux UWP créés avec Direct3D.

La rubrique optimiser l’entrée et la boucle de rendu fournit des conseils sur la façon de choisir l’option de traitement d’événement d’entrée appropriée pour gérer la latence d’entrée et optimiser la boucle de rendu.

La rubrique réduire la latence avec les chaînes d’échange DXGI 1,3 explique comment réduire la latence de trame effective en attendant que la chaîne de permutation signale le moment approprié pour commencer le rendu d’une nouvelle trame.

La rubrique relative à la mise à l’échelle et aux superpositions de chaîne de permutation explique comment améliorer les temps de rendu en utilisant des chaînes de permutation mises à l’échelle pour afficher le contenu du jeu en temps réel à une résolution inférieure à celle que l’affichage est en mode natif. Elle explique également comment créer des chaînes d’échange de superposition pour les appareils dotés de la fonctionnalité de superposition matérielle. Cette technique peut être utilisée pour atténuer le problème d’une interface utilisateur avec montée en puissance parallèle résultant de l’utilisation de la mise à l’échelle de la chaîne de permutation.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">Programmation asynchrone pour les jeux</a></p></td>
<td align="left"><p>Comprendre la programmation asynchrone et le Threading avec DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Gérer des scénarios de suppression de périphériques dans Direct3D 11</a></p></td>
<td align="left"><p>Recréez la chaîne d’interface de l’appareil Direct3D et DXGI lorsque la carte graphique est supprimée ou réinitialisée.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">Échantillonnage multiple dans les applications UWP</a></p></td>
<td align="left"><p>Utilisez l’échantillonnage multiple dans les jeux UWP créés à l’aide de Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">Optimiser la boucle d’entrée et de rendu</a></p></td>
<td align="left"><p>Réduisez la latence d’entrée et optimisez la boucle de rendu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">Réduire la latence avec des chaînes d’échange DXGI 1.3</a></p></td>
<td align="left"><p>Utilisez DXGI 1,3 pour réduire la latence de trame effective.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">Mise à l’échelle et superpositions de chaînes d’échange</a></p></td>
<td align="left"><p>Créez des chaînes de permutation mises à l’échelle pour un rendu plus rapide sur les appareils mobiles et utilisez des chaînes d’échange de superposition pour augmenter la qualité visuelle.</p></td>
</tr>
</tbody>
</table>