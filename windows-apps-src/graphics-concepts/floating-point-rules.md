---
title: Règles en matière de virgule flottante
description: Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini des règles à virgule flottante simple précision IEEE 754 32 bits.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Règles en matière de virgule flottante
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7832d4ad344e425bc479d52e1516ae0c63dff624
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168063"
---
# <a name="span-iddirect3dconceptsfloating-point_rulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Règles en matière de virgule flottante


Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini des règles à virgule flottante simple précision IEEE 754 32 bits.

## <a name="span-idalpha_32_bitspanspan-idalpha_32_bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>règles à virgule flottante 32 bits


Il existe deux ensembles de règles : ceux qui se conforment à IEEE-754 et ceux qui s’écartent de la norme.

### <a name="span-idalpha_754_rulesspanspan-idalpha_754_rulesspanspan-idalpha_754_rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>Règles IEEE-754 honorées

Certaines de ces règles sont une option unique où IEEE-754 offre des choix.

-   La division par 0 produit +/-INF, à l’exception de 0/0, ce qui donne NaN.
-   le journal de (+/-) 0 produit-INF.  

    le journal d’une valeur négative (autre que-0) produit une valeur NaN.
-   La racine carrée réciproque (rsq) ou la racine carrée (sqrt) d’un nombre négatif produit une valeur NaN.  

    L’exception est-0 ; sqrt (-0) génère-0, et rsq (-0) produit-INF.
-   INF-INF = NaN
-   (+/-) INF/(+/-) INF = NaN
-   (+/-) INF \* 0 = Nan
-   NaN (any OP) any-value = NaN
-   Les comparaisons EQ, GT, GE, LT et LE, lorsque l’un ou l’autre ou les deux opérandes sont NaN, retourne **false**.
-   Les comparaisons ignorent le signe 0 (par conséquent + 0 est égal à-0).
-   La comparaison ne, lorsque l’un des opérandes ou les deux, est NaN retourne la **valeur true**.
-   Les comparaisons de toute valeur non NaN par rapport à +/-INF renvoient le résultat correct.

### <a name="span-idalpha_754_deviationsspanspan-idalpha_754_deviationsspanspan-idalpha_754_deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Écarts ou exigences supplémentaires des règles IEEE-754

-   IEEE-754 nécessite des opérations à virgule flottante pour produire un résultat qui est la valeur représentable la plus proche d’un résultat à précision infinie, connu sous le nom d’arrondi à le plus proche, même.

    Direct3D 11 et up définissent la même exigence que IEEE-754 : les opérations à virgule flottante 32 bits produisent un résultat qui se trouve dans 0,5 unité-Last-place (ULP) du résultat infiniment précis. Cela signifie que, par exemple, le matériel est autorisé à tronquer les résultats à 32 bits plutôt qu’à effectuer un arrondi vers le plus proche, même si cela entraînerait une erreur d’au plus 0,5 ULP. Cette règle s’applique uniquement à l’addition, la soustraction et la multiplication.

    Les versions antérieures de Direct3D définissent une exigence plus faible que IEEE-754 : les opérations à virgule flottante 32 bits produisent un résultat qui se trouve dans une unité-Last-place (1 ULP) du résultat à précision infinie. Cela signifie que, par exemple, le matériel est autorisé à tronquer les résultats à 32 bits plutôt qu’à effectuer un arrondi vers le plus proche, même si cela entraînerait une erreur d’au plus un ULP.

-   Il n’existe aucune prise en charge des exceptions à virgule flottante, des bits d’État ou des interruptions.
-   Les dénormes sont vidées du zéro protégé contre la signature lors de l’entrée et de la sortie d’une opération mathématique à virgule flottante. Des exceptions sont faites pour toute opération de déplacement de données ou d’e/s qui ne manipule pas les données.
-   Les États qui contiennent des valeurs à virgule flottante, telles que les valeurs Viewport MinDepth/MaxDepth ou BorderColor, peuvent être fournis en tant que valeurs dénormales et peuvent ou non être vidés avant que le matériel ne les utilise.
-   Les opérations min ou Max vident les dénormes pour la comparaison, mais le résultat peut ou non être vidé de la norme.
-   L’entrée NaN d’une opération produit toujours NaN à la sortie. Toutefois, le modèle binaire exact de la valeur NaN n’est pas requis pour rester le même (sauf si l’opération est une instruction Move brute, qui ne modifie pas les données).
-   Les opérations min ou Max pour lesquelles un seul opérande est NaN retournent l’autre opérande en tant que résultat (contrairement aux règles de comparaison que nous avons consultées précédemment). Il s’agit d’une règle IEEE 754R.

    La spécification IEEE-754R pour les opérations min et Max à virgule flottante indique que si l’une des entrées de min ou Max est une valeur QNaN silencieuse, le résultat de l’opération est l’autre paramètre. Par exemple :

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Une révision de la spécification IEEE-754R a adopté un comportement différent pour min et Max lorsqu’une entrée est une valeur SNaN « signaling » et une valeur QNaN :

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    En règle générale, Direct3D suit les normes pour les opérations arithmétiques : IEEE-754 et IEEE-754R. Mais dans ce cas, nous avons un écart.

    Les règles arithmétiques dans Direct3D 10 et les versions ultérieures ne font aucune distinction entre les valeurs NaN et les valeurs NaN de signalisation (QNaN contre SNaN). Toutes les valeurs NaN sont gérées de la même façon. Dans le cas de min et Max, le comportement Direct3D pour toute valeur NaN est semblable à la façon dont QNaN est géré dans la définition IEEE-754R. (Pour l’exhaustivité : si les deux entrées sont NaN, toute valeur NaN est retournée.)

-   Une autre règle IEEE 754R est que min (-0, + 0) = = min (+ 0,-0) = =-0 et Max (-0, + 0) = = max (+ 0,-0) = = + 0, qui honore le signe, contrairement aux règles de comparaison pour le zéro signé (comme nous l’avons vu précédemment). Direct3D recommande le comportement IEEE 754R ici, mais ne l’applique pas. Il est possible que le résultat de la comparaison des zéros dépende de l’ordre des paramètres, à l’aide d’une comparaison qui ignore les signes.
-   x \* 1.0 f aboutit toujours à x (sauf denorme Flush).
-   x/1.0 a toujours pour résultat x (à l’exception de la dénorme Flush).
-   x +/-0.0 f produit toujours x (à l’exception de la dénorme Flush). Mais-0 + 0 = + 0.
-   Les opérations fusionnées (par exemple, Mad, DP3) produisent des résultats qui ne sont pas moins précis que le plus mauvais ordonnancement en série de l’évaluation de l’expansion déroutée de l’opération. La définition du pire classement possible, dans le but de la tolérance, n’est pas une définition fixe pour une opération fusionnée donnée. elle dépend des valeurs particulières des entrées. Les étapes individuelles de l’expansion sans fusible sont chacune autorisées 1 tolérance ULP (ou pour toutes les instructions Direct3D appelle avec une tolérance plus Lax que 1 ULP, la tolérance la plus LAX est autorisée).
-   Les opérations fusionnées adhèrent aux mêmes règles NaN que les opérations non fusionnées.
-   sqrt et RCP ont 1 tolérance ULP. Les instructions de la racine carrée de nuanceur réciproque et réciproque, [**RCP**](/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) et [**rsq**](/windows/desktop/direct3dhlsl/rsq--sm4---asm-), ont leur propre exigence de précision souple distincte.
-   La multiplication et la Division de chacune fonctionnent au niveau de précision à virgule flottante 32 bits (précision à 0,5 ULP pour multiplier, 1,0 ULP pour réciproque). Si x/y est implémenté directement, les résultats doivent être d’une précision supérieure ou égale à celle d’une méthode en deux étapes.

## <a name="span-iddouble_prec_64_bitspanspan-iddouble_prec_64_bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>règles à virgule flottante 64 bits (double précision)


Le matériel et les pilotes d’affichage prennent éventuellement en charge la virgule flottante double précision. Pour indiquer la prise en charge, quand vous appelez [**ID3D11Device :: CheckFeatureSupport**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) avec [**d3d11 \_ Feature \_ double**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature), le pilote affecte la valeur true à **DoublePrecisionFloatShaderOps** de données de la [** \_ fonctionnalité \_ \_ d3d11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles) . Le pilote et le matériel doivent ensuite prendre en charge toutes les instructions à virgule flottante double précision.

Les instructions à double précision suivent les spécifications de comportement IEEE 754R.

La prise en charge de la génération de valeurs dénormalisées est requise pour les données à double précision (aucun comportement de vidage à zéro). De même, les instructions ne lisent pas les données dénormalisées comme un zéro signé, elles respectent la valeur de la norme.

## <a name="span-idalpha_16_bitspanspan-idalpha_16_bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>règles à virgule flottante 16 bits


Direct3D prend également en charge les représentations 16 bits des nombres à virgule flottante.

Format:

-   1 bit (s) de signe dans la position de bit du MSB
-   5 bits d’exposant biaisé (e)
-   10 bits de fraction (f), avec un bit masqué supplémentaire

Une valeur float16 (v) suit les règles suivantes :

-   Si e = = 31 et f ! = 0, alors v est NaN indépendamment de s
-   Si e = = 31 et f = = 0, v = (-1) s \* infini (infini signé)
-   Si e est compris entre 0 et 31, v = (-1) s \* 2 (e-15) \* (1. f)
-   Si e = = 0 et f ! = 0, alors v = (-1) s \* 2 (e-14) \* (0. f) (nombres dénormalisés)
-   Si e = = 0 et f = = 0, v = (-1) s \* 0 (zéro signé)

les règles à virgule flottante 32 bits sont également conservées pour les nombres à virgule flottante 16 bits, ajustées pour la disposition en bits décrite précédemment. Les exceptions à cette règle sont les suivantes :

-   Précision : les opérations non ancrées sur les nombres à virgule flottante 16 bits produisent un résultat qui est la valeur représentable la plus proche d’un résultat à précision infinie (arrondi au plus proche pair, par IEEE-754, appliqué aux valeurs 16 bits). les règles à virgule flottante 32 bits adhèrent à 1 tolérance ULP, les règles à virgule flottante de 16 bits adhèrent à 0,5 ULP pour les opérations non fusionnées et 0,6 ULP pour les opérations fusionnées.
-   les nombres à virgule flottante 16 bits préservent les dénormes.

## <a name="span-idalpha_11_bitspanspan-idalpha_11_bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>règles à virgule flottante 11 bits et 10 bits


Direct3D prend également en charge les formats à virgule flottante 11 bits et 10 bits.

Format:

-   Aucun bit de signe
-   5 bits d’exposant biaisé (e)
-   6 bits de fraction (f) pour un format 11 bits, 5 bits de fraction (f) pour un format 10 bits, avec un bit masqué supplémentaire dans les deux cas.

Une valeur float11/float10 (v) respecte les règles suivantes :

-   Si e = = 31 et f ! = 0, alors v est NaN
-   Si e = = 31 et f = = 0, alors v = + Infinity
-   Si e est compris entre 0 et 31, v = 2 (e-15) \* (1. f)
-   Si e = = 0 et f ! = 0, alors v = \* 2 (e-14) \* (0. f) (nombres dénormalisés)
-   Si e = = 0 et f = = 0, alors v = 0 (zéro)

les règles à virgule flottante 32 bits contiennent également des nombres à virgule flottante de 11 et 10 bits, ajustés pour la disposition en bits décrite précédemment. Voici certaines exceptions :

-   Précision : les règles à virgule flottante 32 bits adhèrent à 0,5 ULP.
-   les nombres à virgule flottante 10/11 bits préservent les dénormes.
-   Toute opération qui aboutirait à un nombre inférieur à zéro est ancrée à zéro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

[Ressources](/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[Textures](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 