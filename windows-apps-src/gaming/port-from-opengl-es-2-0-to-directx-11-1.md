---
title: Portage d’OpenGL ES 2.0 sur Direct3D 11
description: Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, opengl, direct3d 11 , portage, graphiques
ms.localizationpriority: medium
ms.openlocfilehash: 9215c06c97fb8e45a445ccd3691f484a6b2ad513
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258452"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>Portage d’OpenGL ES 2.0 sur Direct3D 11



Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime.

> **Notez**   une étape intermédiaire pour le portage de votre projet OpenGL ES 2,0 est l’utilisation de l’angle pour les Microsoft Store. ANGLE pour Windows Store vous permet d’exécuter le contenu OpenGL ES sous Windows en convertissant les appels d’API OpenGL ES en appels d’API DirectX 11. Pour plus d’informations sur ANGLE, voir le [Wiki ANGLE pour Microsoft Store](https://github.com/microsoft/angle/wiki).

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">Mapper OpenGL ES 2,0 à Direct3D 11,1</a></p></td>
<td align="left"><p>Si vous vous apprêtez à porter votre architecture graphique OpenGL ES 2.0 sur Direct3D pour la première fois, commencez par vous familiariser avec les principales différences entre les API. Les rubriques de cette section vous aideront à planifier votre stratégie de portage ainsi que les différentes modifications à apporter en vue du transfert de votre processus de traitement graphique sur Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">Comment : porter un convertisseur OpenGL ES 2,0 simple vers Direct3D 11,1</a></p></td>
<td align="left"><p>Cet exercice de portage permet de mettre en pratique une notion de base : porter un convertisseur simple d’OpenGL ES 2.0 sur Direct3D, afin d’adapter un cube en rotation inclus dans un nuanceur de vertex au modèle d’application DirectX 11 (Windows universelle) fourni dans Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">Référence OpenGL ES 2,0 vers Direct3D 11,1</a></p></td>
<td align="left"><p>Vous trouverez dans ces rubriques de référence un index de mappage d’API ainsi que des exemples de code simples destinés à vous aider lors du portage d’OpenGL ES 2.0 vers Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

 

 




