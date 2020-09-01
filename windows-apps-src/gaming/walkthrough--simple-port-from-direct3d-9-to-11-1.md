---
title: Procédure pas à pas--Portage de Direct3D 9 vers DirectX 11 et UWP
description: Cet exercice de portage indique comment faire passer une infrastructure de rendu simple de Direct3D 9 à Direct3D 11 et à la plateforme Windows universelle (UWP).
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, port, Direct3D 9, Direct3D 11
ms.localizationpriority: medium
ms.openlocfilehash: 2e194ab79b8ba0a5dc79d4ad24f808d3613a0c98
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158993"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>Procédure pas à pas : Porter une application Direct3D 9 simple vers DirectX 11 et la plateforme Windows universelle (UWP)



Cet exercice de portage indique comment faire passer une infrastructure de rendu simple de Direct3D 9 à Direct3D 11 et à la plateforme Windows universelle (UWP).
## 
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
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">Initialiser Direct3D 11</a></p></td>
<td align="left"><p>Montre comment convertir du code d’initialisation Direct3D 9 en Direct3D 11, notamment comment obtenir des handles vers le périphérique Direct3D et le contexte de périphérique, et comment utiliser DXGI pour configurer une chaîne d’échange.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">Convertir l’infrastructure de rendu</a></p></td>
<td align="left"><p>Montre comment convertir une infrastructure de rendu simple de Direct3D 9 à Direct3D 11, notamment comment porter des tampons de géométrie, comment compiler et charger des programmes de nuanceurs HLSL et comment implémenter la chaîne de rendu dans Direct3D 11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">Porter la boucle de jeu</a></p></td>
<td align="left"><p>Montre comment implémenter une fenêtre pour un jeu UWP et comment avancer la boucle de jeu, notamment comment créer un <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView"><strong>IFrameworkView</strong></a> pour contrôler un <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow"><strong>CoreWindow</strong></a>plein écran.</p></td>
</tr>
</tbody>
</table>

 

Cette rubrique examine progressivement deux chemins de code qui effectuent la même tâche graphique de base : afficher un cube en forme de vertex qui tourne. Dans les deux cas, le code couvre le processus suivant :

1.  Création d’un périphérique Direct3D et d’une chaîne d’échange.
2.  Création d’une mémoire tampon de vertex et d’un tampon d’index pour représenter un maillage de cube en couleur.
3.  Création d’un nuanceur de vertex qui transforme les vertex en espace d’écran, d’un nuanceur de pixels qui fusionne les valeurs de couleurs, compilation des nuanceurs et chargement des nuanceurs en tant que ressources Direct3D.
4.  Implémentation de la chaîne de rendu et présentation du cube dessiné à l’écran.
5.  Création d’une fenêtre, démarrage d’une boucle principale et traitement des messages de fenêtre.

À l’issue de cette procédure pas à pas, vous devrez connaître les différences élémentaires suivantes entre Direct3D 9 et Direct3D 11 :

-   Séparation du périphérique, du contexte de périphérique et de l’infrastructure graphique.
-   Processus de compilation des nuanceurs et chargement du bytecode de nuanceur au moment de l’exécution.
-   Manière de configurer des données par vertex pour le stade d’assembleur d’entrée (stade IA).
-   Manière d’utiliser un [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) pour créer un affichage CoreWindow.

Notez que cette procédure pas à pas utilise [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) pour des raisons de simplicité et ne couvre pas l’interopérabilité XAML.

## <a name="prerequisites"></a>Prérequis


Vous devez [Préparer votre environnement pour le développement de jeux UWP DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Vous n’avez pas encore besoin de modèle, mais Microsoft Visual Studio 2015 est nécessaire pour charger les exemples de code de cette procédure pas à pas.

Pour vous familiariser avec les concepts de programmation pour DirectX 11 et UWP présentés dans cette procédure pas à pas, voir les [concepts et considérations en matière de portage](porting-considerations.md).

## <a name="related-topics"></a>Rubriques connexes

**Direct3D**

* [Écriture de nuanceurs HLSL dans Direct3D 9](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Modèles de projet de jeu DirectX](user-interface.md)

**Microsoft Store**

* [**Microsoft :: WRL :: ComPtr**](/cpp/windows/comptr-class)
* [**Descripteur to Object, opérateur (^)**](/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)