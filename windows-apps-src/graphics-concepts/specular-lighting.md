---
title: Éclairage spéculaire
description: L’éclairage spéculaire identifie les surbrillances spéculaires brillantes qui se produisent lorsque la lumière atteint une surface d’objet et renvoie vers l’appareil photo.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Éclairage spéculaire
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60bd4019f330058d4396a5b0d75d00f90ecff09
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750185"
---
# <a name="specular-lighting"></a>Éclairage spéculaire


L' *éclairage spéculaire* identifie les surbrillances spéculaires brillantes qui se produisent lorsque la lumière atteint une surface d’objet et renvoie vers l’appareil photo. L’éclairage spéculaire est plus intense que la lumière diffuse et s’arrête plus rapidement sur la surface de l’objet. Le calcul de l’éclairage spéculaire prend plus de temps que l’éclairage diffus, mais l’avantage de son utilisation est qu’il ajoute des détails significatifs à une surface.

La modélisation de la réflexion spéculaire nécessite que le système sache dans quel sens le feu est en déplacement et dans le sens de l’oeil du spectateur. Le système utilise une version simplifiée du modèle de réflexion spéculaire Phong, qui utilise un vecteur à mi-chemin pour rapprocher l’intensité de la réflexion spéculaire.

L’état d’éclairage par défaut ne calcule pas les surbrillances spéculaires.

## <a name="span-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Équation d’éclairage spéculaire


L’éclairage spéculaire est décrit par l’équation suivante.

> Éclairage spéculaire = cs \* Sum \[ ls \* (N · H)<sup>P</sup> \* atten \*\]

Les variables, leurs types et leurs plages sont les suivants :

| Paramètre    | Valeur par défaut | Type                                                             | Description                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| C           | (0, 0, 0, 0)     | Transparence rouge, vert, bleu et alpha (valeurs à virgule flottante) | Couleur spéculaire.                                                                                        |
| Sum          | N/A           | N/A                                                              | Somme du composant spéculaire de chaque lumière.                                                          |
| N            | N/A           | vecteur 3D (valeurs à virgule flottante x, y et z)                    | Normale au vertex.                                                                                         |
| H            | N/A           | vecteur 3D (valeurs à virgule flottante x, y et z)                    | Vecteur à demi-sens. Consultez la section sur le vecteur à mi-chemin.                                                |
| <sup>P</sup> | 0.0           | Virgule flottante                                                   | Puissance de réflexion spéculaire. La plage est comprise entre 0 et + infini                                                     |
| LS           | (0, 0, 0, 0)     | Transparence rouge, vert, bleu et alpha (valeurs à virgule flottante) | Couleur spéculaire clair.                                                                                  |
| Atten        | N/A           | Virgule flottante                                                   | Valeur d’atténuation de la lumière. Consultez [facteurs d’atténuation et de focalisation](attenuation-and-spotlight-factor.md). |
| Zone         | N/A           | Virgule flottante                                                   | Facteur Gong. Consultez [facteurs d’atténuation et de focalisation](attenuation-and-spotlight-factor.md).        |

 

La valeur de CS est l’une des suivantes :

-   couleur du vertex 1, si la source de matériau spéculaire est la couleur de vertex diffuse, et la première couleur de vertex est fournie dans la déclaration de vertex.
-   couleur du Vertex 2, si la source de matériau spéculaire est la couleur de vertex spéculaire, et la deuxième couleur de vertex est fournie dans la déclaration de vertex.
-   couleur spéculaire

**Remarque**    Si l’option source de matériau spéculaire est utilisée et que la couleur du vertex n’est pas fournie, la couleur spéculaire est utilisée.

 

Les composants spéculaires sont ancrés à une valeur comprise entre 0 et 255, après que toutes les lumières ont été traitées et interpolées séparément.

## <a name="span-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>Vecteur à mi-chemin


Le vecteur à mi-chemin (H) existe à mi-chemin entre deux vecteurs : le vecteur d’un sommet d’objet à la source de lumière et le vecteur d’un sommet d’objet à la position de la caméra. Direct3D offre deux façons de calculer le vecteur à mi-chemin. Lorsque les surbrillances spéculaires relatives à l’appareil photo sont activées (au lieu des surbrillances spéculaires orthogonales), le système calcule le vecteur à mi-chemin à l’aide de la position de la caméra et la position du vertex, ainsi que le vecteur de direction de la lumière. La formule suivante illustre cela.

> H = normal (normal (CP-VP) + L<sub>Rép</sub>)

 

| Paramètre       | Valeur par défaut | Type                                          | Description                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| CP              | N/A           | vecteur 3D (valeurs à virgule flottante x, y et z) | Position de la caméra.                                             |
| VP              | N/A           | vecteur 3D (valeurs à virgule flottante x, y et z) | Position du vertex.                                             |
| <sub>Rép</sub> . | N/A           | vecteur 3D (valeurs à virgule flottante x, y et z) | Vecteur de direction de la position du vertex à la position de la lumière. |

 

La détermination du vecteur à mi-chemin de cette manière peut nécessiter de nombreuses ressources de calcul. En guise d’alternative, l’utilisation de surbrillances spéculaires orthogonales (au lieu des surbrillances spéculaires relatives à l’appareil photo) indique au système d’agir comme si le point de vue était à distance infiniment sur l’axe z. Cela se reflète dans la formule suivante.

> H = normal ((0, 0, 1) + L<sub>répertoire</sub>)

Ce paramètre est moins gourmand en calculs, mais beaucoup moins précis. il est donc mieux utilisé par les applications qui utilisent la projection orthogonale.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Tels


Dans cet exemple, l’objet est coloré à l’aide de la couleur de la lumière spéculaire et d’une couleur spéculaire.

D’après l’équation, la couleur résultante pour les vertex d’objet est une combinaison de la couleur de l’élément et de la couleur claire.

L’illustration suivante montre la couleur de matériau spéculaire, grise, et la couleur d’éclairage spéculaire, qui est blanche.

![illustration d’une sphère grise](images/amb1.jpg)![illustration d’une sphère blanche](images/lightwhite.jpg)

La surbrillance spéculaire qui en résulte est présentée dans l’illustration suivante.

![illustration de la surbrillance spéculaire](images/lights.jpg)

La combinaison de la surbrillance spéculaire avec l’éclairage ambiant et diffus produit l’illustration suivante. Avec les trois types d’éclairage appliqués, cela ressemble plus clairement à un objet réaliste.

![illustration de la combinaison de la mise en surbrillance spéculaire, de l’éclairage ambiant et de l’éclairage diffus](images/lightads.jpg)

L’éclairage spéculaire est plus gourmand en calcul que l’éclairage diffus. Il est généralement utilisé pour fournir des indices visuels sur le matériau en surface. La mise en surbrillance spéculaire varie en fonction de la taille et de la couleur.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




