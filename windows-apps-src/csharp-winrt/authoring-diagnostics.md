---
description: Messages d’erreur de diagnostic/WinRT C# pour les composants créés
title: Diagnostiquer les erreurs du composant/WinRT C#
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230016"
---
# <a name="diagnose-cwinrt-component-errors"></a>Diagnostiquer les erreurs du composant/WinRT C#

Cet article fournit des informations supplémentaires sur les restrictions relatives aux composants Windows Runtime écrits en C#/WinRT. Il développe les informations fournies dans les messages d’erreur à partir de C#/WinRT lorsqu’un auteur génère son composant. Pour les composants .NET Native gérés UWP existants, les métadonnées des composants WinRT C# sont générées à l’aide d' [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool), un outil .net. Maintenant que la prise en charge de Windows Runtime est découplée à partir de .NET, C#/WinRT fournit les outils permettant de générer un fichier. winmd à partir de votre composant. La Windows Runtime a davantage de contraintes sur le code qu’une bibliothèque de classes C#, et l’analyseur de diagnostic/WinRT C# vous en avertit avant de générer un fichier. winmd.

Cet article traite des erreurs signalées dans votre build à partir de C#/WinRT. Cet article fait office de version mise à jour des informations sur les restrictions applicables aux [composants gérés .net Native UWP existants](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) qui utilisent l’outil Winmdexp.exe.

Recherchez le texte du message d’erreur (en omettant les valeurs spécifiques des espaces réservés) ou le numéro du message. Si vous ne trouvez pas les informations dont vous avez besoin ici, vous pouvez nous aider à améliorer la documentation à l’aide du bouton commentaires à la fin de cet article. Dans vos commentaires, veuillez inclure le message d’erreur. Vous pouvez également signaler un bogue dans le [référentiel C#/WinRT](https://github.com/microsoft/CsWinRT).

Cet article organise les messages d’erreur par scénario.

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>Implémentation d’interfaces qui ne sont pas des interfaces Windows Runtime valides

Les composants/WinRT C# ne peuvent pas implémenter certaines interfaces Windows Runtime, telles que les interfaces Windows Runtime qui représentent des actions ou des opérations asynchrones (**IAsyncAction**, **\<TProgress\> IAsyncActionWithProgress**, **IAsyncOperation \<TResult\>** ou **IAsyncOperationWithProgress \<TResult,TProgress\>**). Utilisez plutôt la classe **AsyncInfo** pour générer des opérations asynchrones dans les composants Windows Runtime. Remarque : ce ne sont pas les seules interfaces non valides, par exemple une classe ne peut pas implémenter **System. exception**.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>Windows Runtime type de composant {0} ne peut pas implémenter {1} l’interface, car l’interface n’est pas une interface Windows Runtime valide</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>Erreurs liées aux attributs

Dans le Windows Runtime, les méthodes surchargées peuvent avoir le même nombre de paramètres uniquement si une surcharge est spécifiée en tant que surcharge par défaut. Utilisez l’attribut **Windows. Foundation. Metadata. DefaultOverload** (CsWinRT1015, 1016).

Lorsque des tableaux sont utilisés en tant qu’entrées ou sorties dans des fonctions ou des propriétés, ils doivent être en lecture seule ou en écriture seule (CsWinRT 1025). Les attributs **System. Runtime. InteropServices. WindowsRuntime. ReadOnlyArray** et **System. Runtime. InteropServices. WindowsRuntime. WriteOnlyArray** sont fournis pour que vous les utilisiez. Les attributs fournis sont uniquement destinés à être utilisés sur les paramètres du type de tableau (CsWinRT1026), et un seul doit être appliqué par paramètre (CsWinRT1023).

Vous n’avez pas besoin d’appliquer un attribut à un paramètre de **tableau marqué,** car il est supposé être en écriture seule. Il y a un message d’erreur si vous le décorez en lecture seule dans ce cas (CsWinRT1024).

Les attributs **System. Runtime. InteropServices. InAttribute** et **System. Runtime. InteropServices. OutAttribute** ne doivent pas être utilisés sur des paramètres de tout type (CsWinRT1021, 1022).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>Dans la classe {2} : {0} les surcharges de plusieurs paramètres de' {1} 'sont décorées avec Windows. Foundation. Metadata. DefaultOverloadAttribute. L’attribut ne peut être appliqué qu’à une seule surcharge de la méthode.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>Dans {2} la classe :, les {0} surcharges de paramètre de {1} doivent avoir une seule méthode spécifiée en tant que surcharge par défaut en la décorant avec l’attribut Windows. Foundation. Metadata. DefaultOverloadAttribute.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'qui est un tableau, et qui a un System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. Dans le Windows Runtime, les paramètres de tableau doivent avoir soit ReadOnlyArray, soit WriteOnlyArray. Supprimez les attributs suivants ou remplacez-les par l’attribut de Windows Runtime approprié si nécessaire.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'avec un System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. outattribute. Windows Runtime ne prend pas en charge le marquage des paramètres avec System. Runtime. InteropServices. InAttribute ou System. Runtime. InteropServices. OutAttribute. Supprimez System.Runtime.InteropServices.InAttribute et remplacez System.Runtime.InteropServices.OutAttribute par le modificateur « out ».</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'qui est un tableau, et qui a à la fois ReadOnlyArray et WriteOnlyArray. Dans le Windows Runtime, les paramètres du tableau de contenu doivent être accessibles en lecture ou en écriture. Supprimez l’un des attributs de « {1} ».</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>La méthode' {0} 'a un paramètre de sortie' {1} 'qui est un tableau, mais qui a l’attribut ReadOnlyArray. Dans le Windows Runtime, le contenu des tableaux de sortie est accessible en écriture.  Supprimez l’attribut de « {1} ».</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'qui est un tableau. Dans le Windows Runtime, le contenu des paramètres du tableau doit être accessible en lecture ou en écriture. Appliquez ReadOnlyArray ou WriteOnlyArray à' {1} '.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'qui n’est pas un tableau et qui a un attribut ReadOnlyArray ou un attribut WriteOnlyArray. Windows Runtime ne prend pas en charge le marquage des paramètres autres que des tableaux avec ReadOnlyArray ou WriteOnlyArray.»</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Erreurs d’espace de noms et noms non valides pour le fichier de sortie

Dans le Windows Runtime, tous les types publics dans un fichier de métadonnées Windows (. winmd) doivent se trouver dans un espace de noms qui partage le nom de fichier. winmd, ou dans des sous-espaces de noms du nom de fichier. Par exemple, si votre projet Visual Studio est nommé A. B (autrement dit, votre composant Windows Runtime est. B. winmd), il peut contenir des classes publiques A. B. classe1 et A. B. C. Classe2, mais pas un. class3 ou D. Class4.

> [!NOTE]
> Ces restrictions s'appliquent uniquement aux types publics, pas aux types privés utilisés dans votre implémentation.

Dans le cas d’un. class3, vous pouvez déplacer class3 vers un autre espace de noms ou remplacer le nom du composant Windows Runtime par un. winmd. Dans l’exemple précédent, le code qui appelle A.B.winmd ne pourra pas localiser A.Class3.

Dans le cas de D. Class4, aucun nom de fichier ne peut contenir à la fois D. Class4 et des classes dans l’espace de noms A. B. par conséquent, la modification du nom du composant Windows Runtime n’est pas une option. Vous pouvez déplacer D. Class4 vers un autre espace de noms ou le placer dans un autre composant de Windows Runtime.

Le système de fichiers ne peut pas faire la distinction entre les majuscules et les minuscules, de sorte que les espaces de noms qui diffèrent par la casse ne sont pas autorisés (CsWinRT1002).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>Un type public a un espace de noms (« {1} ») qui ne partage aucun préfixe commun avec d’autres espaces de noms (« {0} »). Tous les types d’un fichier de métadonnées Windows doivent exister dans un sous-espace de noms de l’espace de noms impliqué par le nom de fichier.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>Plusieurs espaces de noms trouvés avec le nom' {0} '; les noms d’espaces de noms ne peuvent pas différer uniquement par la casse dans le Windows Runtime.</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>Exportation des types qui ne sont pas des types Windows Runtime

L’interface publique de votre composant doit exposer uniquement les types de Windows Runtime. Toutefois, .NET fournit des mappages pour plusieurs types couramment utilisés qui sont légèrement différents dans .NET et le Windows Runtime. Cela permet au développeur .NET de travailler avec des types familiers au lieu d’en apprendre de nouveaux. Vous pouvez utiliser ces types .NET Framework mappés dans l’interface publique de votre composant. Pour plus d’informations, consultez [déclaration de types dans les composants Windows Runtime](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components), [passage de Windows Runtime types à du code managé](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)et [mappages .net de types Windows Runtime](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

Bon nombre de ces mappages sont des interfaces. Par exemple, IList est \<T\> mappé à l’interface Windows Runtime IVector \<T\> . Si vous utilisez List \<string\> au lieu de IList \<string\> comme type de paramètre, C#/WinRT fournit une liste d’alternatives qui inclut toutes les interfaces mappées implémentées par List \<T\> . Si vous utilisez des types génériques imbriqués, tels que List \<Dictionary\<int, string\> \> , C#/WinRT offre des choix pour chaque niveau d’imbrication. Ces listes peuvent considérablement s’allonger.

En général, le meilleur choix est l’interface qui est la plus proche du type. Par exemple, pour \<int, string\> le dictionnaire, le meilleur choix est le plus approprié \<int, string\> .

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>Le membre' {0} 'a le type' ' {1} dans sa signature. Le type' {1} 'n’est pas un type de Windows Runtime valide. Pourtant, le type (ou ses paramètres génériques) implémente des interfaces qui sont des types de Windows Runtime valides. Envisagez de remplacer le type' {1} dans la signature de membre par l’un des types suivants de System. Collections. Generic : {2} .</p></td>
</tr>
</tbody>
</table>

Dans le Windows Runtime, les tableaux dans les signatures de membre doivent être unidimensionnels avec une limite inférieure de 0 (zéro). Les types de tableaux imbriqués tels que `myArray[][]` (CsWinRT1017) et `myArray[,]` (CsWinRT1018) ne sont pas autorisés.

> [!NOTE]
> Cette restriction ne s'applique pas aux tableaux utilisés en interne dans votre implémentation.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>{0}La méthode a un tableau imbriqué de type {1} dans sa signature. Les tableaux dans Windows Runtime signature de méthode ne peuvent pas être imbriqués.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>La méthode' {0} 'a un tableau multidimensionnel de type' {1} 'dans sa signature. Les tableaux dans les signatures de méthode Windows Runtime doivent être unidimensionnels.</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Structures contenant des champs de types non autorisés

Dans le Windows Runtime, une structure peut contenir uniquement des champs, et seules les structures peuvent contenir des champs. Ces champs doivent être publics. Les types de champ valides incluent des énumérations, des structures et des types primitifs.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>La structure {0} ne contient pas de champs publics. Les structures de Windows Runtime doivent contenir au moins un champ public.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>La structure {0} a un champ non public. Tous les champs doivent être publics pour les structures de Windows Runtime.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>La structure {0} a un champ const. Les constantes ne peuvent apparaître que dans les énumérations Windows Runtime.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>{0}La structure contient un champ de type {1} ; {1} n’est pas un type de champ Windows Runtime valide. Les champs d’une structure Windows Runtime doivent uniquement avoir la valeur UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum, ou bien correspondre à une structure.</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>Le nom de paramètre est en conflit avec le code généré

Dans le Windows Runtime, les valeurs de retour sont considérées comme des paramètres de sortie, et les noms des paramètres doivent être uniques. Par défaut, C#/WinRT donne le nom à la valeur de retour `__retval` . Si votre méthode a un paramètre nommé `__retval` , vous obtiendrez l’erreur CsWinRT1010. Pour corriger cela, donnez à votre paramètre un nom autre que `__retvalue` .

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>Le nom du paramètre {1} dans la méthode {0} est le même que le nom du paramètre de valeur de retour utilisé dans l’interopérabilité/WinRT C# généré ; utilisez un autre nom de paramètre.</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>Divers

Les autres restrictions dans un composant créé par C#/WinRT sont les suivantes :

- Vous ne pouvez pas exposer les opérateurs surchargés sur les types publics.
- Les classes et les interfaces ne peuvent pas être génériques.
- Les classes doivent être sealed.
- Les paramètres ne peuvent pas être passés par référence, par exemple à l’aide du mot clé **ref** .
- Les propriétés doivent avoir une méthode de récupération publique.
- Il doit y avoir au moins un type public (classe ou interface) dans l’espace de noms de votre composant.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>' {0} 'est une surcharge d’opérateur. Les types managés ne peuvent pas exposer les surcharges de l’opérateur dans le Windows Runtime.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>Le type {0} est Generic. Les types de Windows Runtime ne peuvent pas être génériques.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>L’exportation des types non scellés n’est pas prise en charge dans CsWinRT. Veuillez marquer le type {0} comme sealed.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>La méthode' {0} 'a le paramètre' {1} 'marqué `ref` . Les paramètres de référence ne sont pas autorisés dans Windows Runtime.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>La propriété' {0} 'n’a pas de méthode Getter publique. Windows Runtime ne prend pas en charge les propriétés Setter uniquement.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Les composants Windows Runtime doivent avoir au moins un type public</p></td>
</tr>
</tbody>
</table>

Dans le Windows Runtime, une classe ne peut avoir qu’un seul constructeur avec un nombre donné de paramètres. Par exemple, vous ne pouvez pas avoir un constructeur qui a un seul paramètre de type **String** et un autre constructeur qui a un seul paramètre de type **int**. La seule solution consiste à utiliser un nombre différent de paramètres pour chaque constructeur.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Numéro d'erreur</p></th>
<th><p>Texte du message</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>Les classes ne peuvent pas avoir plusieurs constructeurs de la même arité dans le Windows Runtime, la classe {0} a des {1} constructeurs à arité multiple.</p></td>
</tr>
</tbody>
</table>


