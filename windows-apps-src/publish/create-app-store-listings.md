---
Description: La section répertorier les listes du processus d’envoi de l’application vous permet de fournir le texte et les images que les clients verront lors de l’affichage de la liste de votre application dans la Microsoft Store.
title: Créer des annonces d’application dans le Windows Store
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP, liste, description, page Store, notes de publication, title
ms.localizationpriority: medium
ms.openlocfilehash: 6e8ba40129197f77b2bbbd3950a6bcf214bae787
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493014"
---
# <a name="create-app-store-listings"></a>Créer des annonces d’application dans le Windows Store

La section répertorier les **listes** du [processus d’envoi](app-submissions.md) de l’application vous permet de fournir le texte et les [images](app-screenshots-and-images.md) que les clients verront lors de l’affichage de la liste de votre application dans la Microsoft Store.

La plupart des champs d’une **liste de magasins** sont facultatifs, mais nous vous suggérons de fournir plusieurs images et autant d’informations que possible pour faire ressortir votre annonce. La valeur minimale requise pour l’étape de **liste des boutiques** à considérer comme étant terminée est une description textuelle et au moins une [capture d’écran](app-screenshots-and-images.md#screenshots).

> [!TIP]
> Vous pouvez éventuellement [Importer et exporter des listes de magasins](import-and-export-store-listings.md) si vous préférez saisir vos informations de référencement en mode hors connexion dans un fichier. csv, plutôt que de fournir des informations et de charger des fichiers directement dans l’espace partenaires. L’utilisation de l’option d’importation et d’exportation peut être particulièrement pratique si vous avez des listes dans de nombreux langages, car elle vous permet d’effectuer plusieurs mises à jour à la fois.

Si votre application précédemment publiée prend en charge Windows 8. x et/ou Windows Phone 8. x ou une version antérieure, vous pouvez [créer des listes de magasins spécifiques](create-platform-specific-store-listings.md) à la plateforme à afficher à ces clients.

## <a name="store-listing-languages"></a>Langues des descriptions dans le Windows Store

Vous devez remplir la page **Description dans le Windows Store** dans une langue au minimum. Nous vous recommandons de fournir une description dans le Windows Store dans chaque langue prise en charge par vos packages. Vous avez cependant la possibilité de supprimer les langues que vous ne souhaitez pas afficher sur cette page. Vous pouvez également fournir des descriptions dans le Windows Store dans des langues supplémentaires, qui ne sont pas prises en charge par vos packages.

> [!NOTE]
> Si votre soumission contient déjà des packages, nous afficherons les [langues](supported-languages.md) prises en charge dans vos packages sur la page vue d’ensemble de la soumission (sauf si vous les supprimez).

Pour ajouter ou supprimer des langues pour les annonces de votre boutique, cliquez sur **Ajouter/supprimer des langues** dans la page vue d’ensemble de la soumission. Si vous avez déjà chargé des packages, leurs langues sont répertoriées dans la section **Langues prises en charge par vos packages**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer**. Si vous décidez plus tard d’inclure une langue précédemment supprimée de cette section, vous pouvez cliquer sur **Ajouter**.

Dans la section **Langues supplémentaires de description dans le Windows Store**, vous pouvez cliquer sur **Gérer les langues supplémentaires** pour ajouter ou supprimer des langues qui ne sont *pas* incluses dans vos packages. Cochez les cases pour les langues que vous souhaitez ajouter, puis cliquez sur **Mettre à jour**. Les langues que vous avez sélectionnées s’afficheront dans la section **Langues supplémentaires de description dans le Windows Store**. Pour supprimer une ou plusieurs de ces langues, cliquez sur **Supprimer** (ou cliquez sur **Gérer les langues supplémentaires** et décochez la case pour les langues que vous souhaitez supprimer).

Lorsque vous avez terminé vos sélections, cliquez sur **Enregistrer** pour revenir à la page de présentation de la soumission.

## <a name="add-and-edit-store-listing-info"></a>Ajouter et modifier les informations de la liste des boutiques

Pour modifier un listing de la boutique, sélectionnez le nom de la langue dans la page vue d’ensemble de la soumission. Vous devez modifier chaque langue séparément, sauf si vous choisissez d’exporter les listes du Store et de travailler hors connexion, puis d’importer toutes les données de la liste en même temps. Pour plus d’informations sur la façon dont cela fonctionne, consultez [Importer et exporter des listes de magasins](import-and-export-store-listings.md).

Les champs disponibles sont décrits ci-dessous.

## <a name="product-name"></a>Nom du produit

Cette zone de liste déroulante vous permet de spécifier le nom à utiliser dans la liste des magasins (si vous avez réservé plus d’un nom pour l’application).

Si vous avez téléchargé des packages dans la même langue que la liste des boutiques sur laquelle vous travaillez, le nom utilisé dans ces packages est sélectionné. Si vous devez [Renommer l’application](manage-app-names.md#rename-an-app-that-has-already-been-published) une fois qu’elle a déjà été publiée, vous pouvez sélectionner un autre nom réservé ici lorsque vous créez une soumission, après avoir téléchargé des packages qui utilisent le nouveau nom.

Si vous n’avez pas téléchargé de packages pour la langue sur laquelle vous travaillez et que vous avez réservé plusieurs noms, vous devez sélectionner l’un de vos noms d’applications réservés, car il n’existe aucun package associé dans cette langue à partir duquel extraire le nom.

> [!NOTE]
> Le **nom de produit** que vous sélectionnez s’applique uniquement à la liste des boutiques dans la langue dans laquelle vous travaillez. Elle n’a pas d’impact sur le nom affiché lorsqu’un client installe l’application ; ce nom provient du manifeste du package qui est installé. Pour éviter toute confusion, nous vous recommandons de faire en sorte que les packages et les listes des magasins de chaque langue utilisent le même nom.

## <a name="description"></a>Description

Le champ de description est l’emplacement où vous pouvez indiquer aux clients ce que fait votre application. Ce champ obligatoire accepte jusqu’à 10 000 caractères.

Pour obtenir des conseils sur la rédaction d’une description attrayante, consultez l’article [Rédiger une description convaincante de l’application](write-a-great-app-description.md).

<a name="release-notes"></a>

## <a name="whats-new-in-this-version"></a>Nouveautés de cette version

Si c’est la première fois que vous envoyez votre application, laissez ce champ vide. Pour une mise à jour d’une application existante, c’est là que vous pouvez permettre aux clients de savoir ce qui a changé dans la dernière version. Ce champ est limité à 1 500 caractères. (Auparavant, ce champ était appelé **notes de publication**).

## <a name="product-features"></a>Fonctionnalités du produit

Il s’agit de courts résumés des principales fonctionnalités de votre application. Ils sont affichés au client sous la forme d’une liste à puces dans la section **fonctionnalités** de la liste des boutiques de votre application, en plus de la **Description**. Chacune de ces informations est limitée à 200 caractères. Vous pouvez spécifier jusqu’à 20 fonctionnalités.

> [!NOTE]
> Ces fonctionnalités apparaîtront à puces dans la liste de votre boutique. n’ajoutez donc pas vos propres puces.

## <a name="screenshots"></a>Captures d’écran.

Une capture d’écran est nécessaire pour soumettre votre application. Nous vous recommandons de fournir au moins quatre captures d’écran pour chaque type d’appareil pris en charge par votre application afin que les utilisateurs puissent voir comment l’application s’affiche sur leur type d’appareil.

Pour plus d’informations, voir l’article [Images et captures d’écran de l’application](app-screenshots-and-images.md#screenshots).

## <a name="store-logos"></a>Stocker des logos

Les logos de stockage sont des images facultatives que vous pouvez télécharger pour améliorer la façon dont votre application est affichée aux clients. Vous pouvez également spécifier que seules les images que vous téléchargez ici doivent être utilisées dans la liste des magasins de votre application pour les clients sur Windows 10 (y compris la Xbox), au lieu d’autoriser le magasin à utiliser des images de logo de vos packages d’application.

> [!IMPORTANT]
> Si votre application prend en charge la Xbox, ou si elle prend en charge Windows Phone 8,1 ou une version antérieure, vous devez fournir certaines images ici pour que la liste apparaisse correctement dans le magasin.

Pour plus d’informations, consultez [stocker des logos](app-screenshots-and-images.md#store-logos).

## <a name="trailers-and-additional-assets"></a>Remorques et ressources supplémentaires

Vous pouvez envoyer des ressources supplémentaires pour votre produit, y compris des codes de fin vidéo et des images promotionnelles. Ils sont tous facultatifs, mais nous vous recommandons de les télécharger autant que possible. Ces images peuvent aider les clients à mieux comprendre ce que fait votre produit et à faire une liste plus attrayante.

Pour plus d’informations, consultez codes de fin [et ressources supplémentaires](app-screenshots-and-images.md#trailers-and-additional-assets).

<a name="supplemental-information"></a>

## <a name="supplemental-fields"></a>Champs supplémentaires

Les champs de cette section sont tous facultatifs. Passez en revue les informations ci-dessous pour déterminer si ces informations sont pertinentes pour votre envoi. En particulier, la **brève description** est recommandée pour la plupart des soumissions. Les autres champs peuvent vous aider à fournir une expérience optimale pour votre produit dans différents scénarios.

### <a name="short-title"></a>Titre abrégé

Version plus petite du nom de votre produit. S’il est fourni, ce nom plus petit peut apparaître à différents emplacements sur Xbox 1 (lors de l’installation, dans les réalisations, etc.) à la place du titre complet de votre produit.

Ce champ a une limite de 50 caractères.

### <a name="sort-title"></a>Titre du tri

Si votre produit peut être alphabétique ou épelé de différentes manières, vous pouvez entrer une autre version ici. Cela permet aux clients de trouver votre produit plus rapidement s’ils tapent cette version dans lors de la recherche.

Ce champ a une limite de 255 caractères.

### <a name="voice-title"></a>Titre de la voix

Un autre nom pour votre produit qui, s’il est fourni, peut être utilisé dans l’expérience audio sur Xbox 1 lors de l’utilisation de Kinect ou d’un casque.

Ce champ a une limite de 255 caractères.

### <a name="short-description"></a>Description courte

Une description plus rapide qui peut être utilisée en haut du listing du magasin de votre produit. S’il n’est pas fourni, le premier paragraphe (jusqu’à 500 caractères) de votre [Description](#description) plus longue sera utilisé à la place. Étant donné que votre description apparaît également sous ce texte, nous vous recommandons de fournir une brève description avec un texte différent pour que la liste des boutiques ne soit pas répétitive.

Pour les jeux, la brève description peut également apparaître dans la section informations du Hub de jeu sur Xbox One.

Pour de meilleurs résultats, conservez votre brève description sous 270 caractères. Le champ a une limite de 1000 caractères, mais dans certains affichages, seuls les 270 premiers caractères seront affichés (avec un lien disponible pour afficher le reste de la brève description).

### <a name="additional-system-requirements"></a>Configuration système supplémentaire requise

Si nécessaire, vous pouvez décrire les configurations matérielles requises par votre application pour fonctionner correctement (en plus des informations fournies dans la section **Configuration système requise** de [Propriétés de l’application](enter-app-properties.md#system-requirements). Ces informations sont particulièrement importantes si votre application nécessite du matériel qui peut ne pas être présent sur tous les ordinateurs. Par exemple, si votre application fonctionne correctement avec un matériel USB externe tel qu’une imprimante ou un microcontrôleur 3D, nous vous suggérons de les entrer ici. Les informations que vous entrez s’affichent pour les clients qui affichent le Listing de votre application sur Windows 10, version 1607 ou ultérieure (y compris la Xbox), ainsi que les conditions que vous avez indiquées sur la page de propriétés du produit.

Vous pouvez entrer jusqu’à 11 éléments pour les deux champs **Matériel minimum** et **Matériel recommandé**. Celles-ci sont présentées au client sous la forme d’une liste à puces dans votre liste de boutiques. Chacune de ces informations est limitée à 200 caractères.

> [!NOTE]
> Votre configuration requise supplémentaire apparaîtra à la liste de votre boutique, donc n’ajoutez pas vos propres puces.

<a name="shared-fields"></a>

## <a name="additional-information"></a>Informations supplémentaires

Les éléments décrits ci-dessous aident les clients à découvrir et à comprendre votre produit. (Anciennement appelée **champs partagés**).

### <a name="search-terms"></a>Termes de recherche

Les termes de recherche (autrefois appelés Mots clés) sont des mots uniques ou des expressions courtes qui ne sont pas visibles pour les clients, mais ils peuvent vous aider à rendre votre application détectable dans le magasin lorsque les clients recherchent à l’aide de ces termes. Vous pouvez inclure jusqu’à 7 termes de recherche avec un maximum de 30 caractères chacun et ne peut pas utiliser plus de 21 mots distincts pour tous les termes de recherche.

Lorsque vous ajoutez des termes de recherche, réfléchissez aux mots que les clients peuvent utiliser pour rechercher des applications comme les vôtres, en particulier s’ils ne font pas partie du nom de votre application. Veillez à ne pas utiliser de termes de recherche qui ne sont pas réellement pertinents pour votre application.

### <a name="copyright-and-trademark-info"></a>Informations de copyright et de marque déposée

Si vous voulez fournir des informations supplémentaires de droits d’auteur et/ou de marque, entrez-les ici. Ce champ est limité à 200 caractères.

### <a name="additional-license-terms"></a>Termes de licence supplémentaires

Laissez ce champ vide si vous voulez que votre application possède une licence conforme aux **Termes du contrat de licence d’application standard** (vers lesquels pointe un lien situé dans le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)).

Si vos termes de contrat de licence diffèrent de ceux du **Contrat de licence d’application standard**, entrez-les ici.

Si vous entrez une URL dans ce champ, elle apparaît aux clients sous la forme d’un lien sur lequel ces derniers peuvent cliquer pour lire les termes supplémentaires du contrat de licence. Ceci est utile si les termes supplémentaires de votre contrat de licence sont très longs, ou si vous souhaitez inclure des liens hypertexte fonctionnels ou une mise en forme dans les termes supplémentaires de votre contrat de licence.

Vous pouvez également saisir jusqu’à 10 000 caractères dans ce champ. Dans ce cas, ces termes supplémentaires du contrat de licence s’afficheront aux clients sous la forme de texte brut.

### <a name="developed-by"></a>Développé par

Entrez du texte ici si vous souhaitez inclure un champ **développé par** dans la liste des magasins de votre application. (Le champ **publié par** répertorie le nom complet de l’éditeur associé à votre compte, que vous fournissiez ou non une valeur pour le champ **développé par** .)

Ce champ a une limite de 255 caractères.
 
<a name="privacy-policy"></a>

> [!NOTE]
> Les **champs stratégie de confidentialité**, **site Web**et **informations de contact du support technique** se trouvent maintenant dans la page [Propriétés](enter-app-properties.md) .
