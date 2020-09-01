---
title: Lancer l’application Contacts
description: Cette rubrique décrit le schéma d’URI ms-people. Votre application peut utiliser ce schéma d’URI afin de lancer l’application Contacts pour effectuer des actions spécifiques.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd1048ad0d3c5d542c5d7fb398261f3e29316396
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162613"
---
# <a name="launch-the-people-app"></a>Lancer l’application Contacts

Cette rubrique décrit le schéma d’URI **ms-people:**. Votre application peut utiliser ce schéma d’URI afin de lancer l’application Contacts pour effectuer des actions spécifiques.

## <a name="ms-people-uri-scheme-reference"></a>Référence de schéma d’URI ms-people:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Résultats</th>
<th align="left">Schéma d’URI</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Permet à d’autres applications de lancer la page Principaux de l’application Contacts.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permet à d’autres applications de lancer la page Paramètres de l’application Contacts.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permet à d’autres applications de fournir une chaîne de recherche qui lance l’application Contacts avec la page de résultats de la recherche.
<div class="alert">
<p>Les paramètres respectent la casse.</p>
<p>Si vous n’entrez pas la syntaxe correctement ou si la valeur de chaîne de recherche est manquante, le comportement par défaut consiste à renvoyer la liste complète des contacts sans aucun filtrage.</p>
</div>
<div>
</div></td>
<td align="left">MS-People : Rechercher ? SearchString = &lt; contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Ouvre une carte de visite existante si le contact est trouvé. Ou bien ouvre une carte de visite temporaire si aucun contact n’est détecté. Si aucun paramètre d’entrée n’est fourni, nous lançons l’application Contacts avec une liste de contacts.
<div class="alert">
<p>Les paramètres respectent la casse.</p>
<p>Peu importe l’ordre des paramètres.</p>
<p>S’il existe plusieurs correspondances, nous renvoyons la première correspondance du contact.</p>
</div>
<div> 
</div></td>
<td align="left">MS-People : viewcontact ? ContactId = &lt; ContactID &gt; &amp; AggregatedId = &lt; aggid &gt; &amp; PhoneNumber = &lt; phoneNum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; name &gt; &amp; contact = &lt; contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Ouvre une page d’enregistrement de contact dans l’application Contacts, permettant d’enregistrer le contact donné avec le numéro de téléphone ou l’adresse électronique fournis.
<div class="alert">
<p>Les paramètres respectent la casse.</p>
<p>Peu importe l’ordre des paramètres.</p>
</div>
<div>
</div></td>
<td align="left">MS-People : savetocontact ? PhoneNumber = &lt; phoneNum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; Name&gt;</td>
</tr>
<tr class="even">
<td align="left">Lance sur la page Ajouter un nouveau contact de l’application personnes pour enregistrer le contact donné.
<div class="alert"><p>Utilisez <a href="/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> pour ouvrir la page enregistrer le nouveau contact. L’utilisation de <strong>LaunchUriAsync</strong> lance uniquement la page principale de l’application People.</p>
<p>Les paramètres respectent la casse.</p>
<p>Peu importe l’ordre des paramètres.</p>
<p>Vous pouvez utiliser n’importe quelle combinaison de paramètres pris en charge.</p>
</div>
<div>
</div></td>
<td align="left">MS-People : savecontacttask ? PhoneNumber = &lt; phoneNum &gt; &amp; email = &lt; email &gt; &amp; ContactName = &lt; Name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>Référence de paramètre ms-people:search:

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>Optionnel.</p>
<p>Chaîne de recherche pour les informations de recherche de contact.</p>
<p>Numéro de téléphone ou nom du contact.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>Référence de paramètre ms-people:viewcontact:

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>Optionnel.</p>
<p>ID de contact du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Optionnel.</p>
<p>Numéro de téléphone du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>E-mail</b></td>
<td align="left"><p>Optionnel.</p>
<p>Adresse électronique du contact.</p></td>
<td align="left"><p>MS-People : viewcontact ? E-mail =johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Optionnel.</p>
<p>Nom du contact.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Contact</b></td>
<td align="left"><p>Optionnel.</p>
<p>Objet Contact.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>Référence de paramètre ms-people:savetocontact:

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>
<th align="left">Exemple</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Optionnel.</p>
<p>Numéro de téléphone du contact.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>E-mail</b></td>
<td align="left"><p>Optionnel.</p>
<p>Adresse électronique du contact.</p></td>
<td align="left"><p>MS-People : savetocontact ? E-mail =johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Optionnel.</p>
<p>Nom du contact.</p></td>
<td align="left"><p>MS-People : savetocontact ? Email = johnsmith@contsco.com &amp; ContactName = John %20% Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>MS-People : savecontacttask : référence de paramètre

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Paramètre</th>
<th align="left">Description</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Société</b></td>
<td align="left"><p>Optionnel.</p>
<p>Nom de la société du contact.</p></td>

</tr>
<tr class="even">
<td align="left"><b>FirstName</b></td>
<td align="left"><p>Optionnel.</p>
<p>Prénom du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>Optionnel.</p>
<p>Ville de l’adresse personnelle.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>Optionnel.</p>
<p>Pays de l’adresse personnelle.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>Optionnel.</p>
<p>État de l’adresse personnelle.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>Optionnel.</p>
<p>Rue de l’adresse personnelle.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>Optionnel.</p>
<p>Code postal de l’adresse personnelle.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>Optionnel.</p>
<p>Téléphone du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>Optionnel.</p>
<p>Poste du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>LastName</b></td>
<td align="left"><p>Optionnel.</p>
<p>Nom du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>Optionnel.</p>
<p>Deuxième prénom du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>Optionnel.</p>
<p>Numéro de téléphone mobile du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Mon</b></td>
<td align="left"><p>Optionnel.</p>
<p>Surnom du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Notes</b></td>
<td align="left"><p>Optionnel.</p>
<p>Remarques sur le contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>Optionnel.</p>
<p>Autre adresse de messagerie du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>Optionnel.</p>
<p>Adresse E-mail personnelle du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffixe</b></td>
<td align="left"><p>Optionnel.</p>
<p>Suffixe du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Titre</b></td>
<td align="left"><p>Optionnel.</p>
<p>Titre du contact.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Site web</b></td>
<td align="left"><p>Optionnel.</p>
<p>Site Web du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>Optionnel.</p>
<p>Ville de l’adresse professionnelle.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>Optionnel.</p>
<p>Pays de l’adresse professionnelle.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>Optionnel.</p>
<p>État de l’adresse de travail.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>Optionnel.</p>
<p>Rue de l’adresse professionnelle.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>Optionnel.</p>
<p>Code postal de l’adresse professionnelle.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkEmail</b></td>
<td align="left"><p>Optionnel.</p>
<p>Adresse de messagerie professionnelle du contact.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkPhone</b></td>
<td align="left"><p>Optionnel.</p>
<p>Numéro de téléphone professionnel du contact.</p></td>
</tr>
</tbody>
</table>