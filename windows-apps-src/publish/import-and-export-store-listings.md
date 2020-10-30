---
description: Vous pouvez créer des listes de magasins pour vos applications sans utiliser l’espace partenaires en exportant vos listes dans un fichier. csv, en entrant vos informations et ressources, puis en important le fichier mis à jour.
title: Importer et exporter des descriptions dans le Store
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, importer les listes du magasin, exporter les listes du magasin, importer l’exportation, stocker les listes CSV
ms.localizationpriority: medium
ms.openlocfilehash: 426d5f84056347f5adb28cafa8aedfe5da255fdc
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029682"
---
# <a name="import-and-export-store-listings"></a>Importer et exporter des descriptions dans le Store

Au lieu d' [entrer des informations pour vos listes de magasins directement dans l’espace partenaires](create-app-store-listings.md), vous avez la possibilité d’ajouter ou de mettre à jour des informations en exportant vos listes dans un fichier. csv, en entrant vos informations et ressources, puis en important le fichier mis à jour. Vous pouvez utiliser cette méthode pour créer des listes de toutes pièces ou pour mettre à jour des listes que vous avez déjà créées.

Cette option est particulièrement utile si vous souhaitez créer ou mettre à jour des listes de magasins pour votre produit dans plusieurs langues, car vous pouvez copier/coller les mêmes informations dans plusieurs champs et apporter facilement toutes les modifications qui doivent s’appliquer à des langues spécifiques. Toutefois, vous ne pouvez pas utiliser cette méthode pour créer ou mettre à jour des [listes de magasins spécifiques](create-platform-specific-store-listings.md) à la plateforme pour les applications précédemment publiées qui prennent en charge des versions de système d’exploitation plus anciennes. 

> [!TIP]
> Vous pouvez également utiliser cette fonctionnalité pour importer et exporter les détails d’une liste de magasins pour un module complémentaire. Pour les modules complémentaires, le processus fonctionne de la même façon, sauf que [seuls les champs pertinents pour les modules](#add-ons) complémentaires sont inclus.

N’oubliez pas que vous pouvez toujours créer ou mettre à jour des listes directement dans l’espace partenaires (même si vous avez déjà utilisé la méthode d’importation/exportation). Il peut être plus facile de mettre à jour directement dans l’espace partenaires lorsque vous effectuez simplement une modification simple, mais vous pouvez utiliser l’une ou l’autre méthode à tout moment.

## <a name="export-listings"></a>Exporter les listings

Sur la page vue d’ensemble de l’envoi d’une application, cliquez sur **Exporter la liste** (dans la section **listes des boutiques** ) pour générer un fichier. csv encodé au format UTF-8. Enregistrez ce fichier dans un emplacement sur votre ordinateur.

Vous pouvez utiliser Microsoft Excel ou un autre éditeur pour modifier ce fichier. Notez que Microsoft 365 versions d’Excel vous permettent d’enregistrer un fichier. csv au format **CSV UTF-8 (délimité par des virgules) (*. csv)** , mais il est possible que d’autres versions ne prennent pas en charge cette opération. Vous trouverez plus d’informations sur les versions d’Excel qui prennent en charge cette fonctionnalité dans le [Bulletin excel 2016 New Features](https://support.office.com/article/what-s-new-in-excel-for-office-365-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73?ui=en-US&rs=en-001&ad=US), ainsi que des informations sur l’encodage UTF-8 dans [divers éditeurs.](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16)
      
Si vous n’avez pas encore créé de liste pour votre produit, le fichier. csv que vous avez exporté ne contient pas de données personnalisées. Vous verrez des colonnes pour le **champ** , l' **ID** , le **type** et la **valeur par défaut** , et les lignes qui correspondent à chaque élément qui peut apparaître dans une liste de magasins.

Si vous avez déjà créé des listes (ou si vous avez téléchargé des packages), vous verrez également des colonnes étiquetées avec des codes de paramètres régionaux qui correspondent à la langue de chaque liste que vous avez créée (ou que nous avons détectés dans vos packages), ainsi qu’à toutes les informations d’annonce que vous avez fournies précédemment.
     
Voici une vue d’ensemble de ce qui est contenu dans chacune des colonnes du fichier. csv exporté :
- La colonne **champ** contient un nom qui est associé à chaque partie d’une liste de magasins. Celles-ci correspondent aux mêmes éléments que ceux que vous pouvez fournir lors de la création de listes de magasins dans l’espace partenaires, bien que certains noms soient légèrement différents. Pour les éléments dans lesquels vous pouvez entrer plus d’un type d’élément, vous verrez plusieurs lignes, jusqu’au nombre maximal que vous pouvez fournir. Par exemple, pour les **fonctionnalités d’application** , vous verrez **Feature1** , **feature2** , etc., jusqu’à **Feature20** (dans la mesure où vous pouvez fournir jusqu’à 20 fonctionnalités d’application).
- La colonne **ID** contient un numéro que Partner Center associe à chaque champ. 
- La colonne **type** fournit des indications générales sur le type d’informations à fournir pour ce champ, telles que **texte** ou **chemin d’accès relatif (ou URL vers un fichier dans l’espace partenaires)** . 
- La colonne **par défaut** (et toutes les colonnes étiquetées avec des codes de paramètres régionaux) représentent le texte ou les éléments multimédias associés à chaque partie de la liste des magasins. Vous pouvez modifier les champs de ces colonnes pour effectuer des mises à jour de vos listes de magasins.

>[!IMPORTANT]
> Ne modifiez aucune des informations contenues dans les colonnes **Field** , **ID** ou **type** . Les informations contenues dans ces colonnes doivent rester inchangées pour que votre fichier importé puisse être traité.

## <a name="update-listing-info"></a>Mettre à jour les informations de liste

Une fois que vous avez exporté vos listes et enregistré votre fichier. csv, vous pouvez modifier vos informations de référencement directement dans le fichier. csv. 

Avec la colonne **par défaut** , chaque langue pour laquelle vous avez créé une liste possède sa propre colonne. Les modifications que vous apportez dans une colonne seront appliquées à votre description dans cette langue. Vous pouvez créer des listes pour les nouvelles langues en ajoutant le code de paramètres régionaux dans la colonne vide suivante dans la ligne du haut. Pour obtenir la liste des codes de paramètres régionaux valides, consultez [langues prises en charge](supported-languages.md).

Vous pouvez utiliser la colonne **par défaut** pour entrer les informations que vous souhaitez partager dans toutes les descriptions de votre application. Si le champ d’une langue donnée n’est pas renseigné, les informations de la colonne par défaut seront utilisées pour cette langue. Vous pouvez remplacer ce champ pour une langue particulière en entrant différentes informations pour cette langue.

La plupart des champs de la liste des magasins sont facultatifs. La **Description** et une capture d’écran sont requises pour chaque liste. pour les langues qui n’ont pas de packages associés, vous devez également fournir un **titre** pour indiquer les noms d’application réservés à utiliser pour cette liste. Pour tous les autres champs, vous pouvez laissez le champ vide si vous ne souhaitez pas l’inclure dans votre liste. N’oubliez pas que si vous laissez un champ vide pour une langue donnée, nous allons vérifier s’il existe des informations dans ce champ dans la colonne par défaut. Si c’est le cas, ces informations sont utilisées. 

Prenons l’exemple suivant : 

![Exemple d’énumération exportée](images/listingimport.png)
     
- Le texte « Description par défaut » sera utilisé pour le champ de **Description** dans les listes en-US et fr-fr. Toutefois, le champ **Description** de la liste es-es utilise le texte « espagnol Description ». 
- Pour le champ **ReleaseNotes** , le texte « notes de publication en anglais » sera utilisé pour en-US, et le texte « notes de publication françaises » sera utilisé pour fr-fr. Toutefois, aucune note de publication ne s’affiche pour es-es.

Si vous ne souhaitez pas apporter de modifications à un champ particulier, vous pouvez supprimer toute la ligne de la feuille de calcul, **à l’exception des lignes des codes de fin et des miniatures et des titres associés** . En dehors de ces éléments, la suppression d’une ligne n’a pas d’impact sur les données associées à ce champ dans vos annonces. Cela vous permet de supprimer toutes les lignes que vous n’envisagez pas de modifier. vous pouvez ainsi vous concentrer sur les champs dans lesquels vous apportez des modifications.

La suppression des informations dans un champ pour une langue, sans supprimer la ligne entière, fonctionne différemment, en fonction du champ. Pour les champs dont le **type** est **texte** , la suppression des informations dans un champ supprimera simplement cette entrée de la liste dans cette langue.  Toutefois, la suppression des informations dans un champ pour une image, telle qu’une capture d’écran ou un logo, n’aura aucun effet. l’image précédente sera toujours utilisée, sauf si vous la supprimez directement dans l’espace partenaires. La suppression des informations pour un champ de code de fin supprime réellement ce code de fin de l’espace partenaires. Assurez-vous de disposer d’une copie des fichiers nécessaires avant de le faire.

La plupart des champs de vos listes exportées requièrent une entrée de texte, comme celles de l’exemple ci-dessus, **Description** et **ReleaseNotes** . Pour ces types de champs, il vous suffit d’entrer le texte approprié dans le champ pour chaque langue. Veillez à respecter la longueur et les autres exigences pour chaque champ. Pour plus d’informations sur ces conditions requises, consultez [créer des listes App Store](create-app-store-listings.md).

Fournir des informations pour les champs qui correspondent aux ressources, telles que les images et les codes de fin, est un peu plus compliqué. Au lieu de **texte** , le **type** de ces ressources est **chemin d’accès relatif (ou URL vers un fichier dans l’espace partenaires)** . 
     
Si vous avez déjà téléchargé des ressources pour vos listes de magasins, ces ressources seront ensuite représentées par une URL. Ces URL peuvent être réutilisées dans plusieurs descriptions pour un produit, ou même sur différents produits au sein du même compte de développeur. vous pouvez ainsi copier ces URL pour les réutiliser dans un autre champ si vous le souhaitez.

> [!TIP]
> Pour confirmer quelle ressource correspond à une URL, vous pouvez entrer l’URL dans un navigateur pour afficher l’image (ou télécharger la vidéo de code de fin).  Vous devez être connecté à votre compte espace partenaires pour que cette URL fonctionne.

Si vous souhaitez utiliser un nouvel élément multimédia que vous n’avez pas encore ajouté à l’espace partenaires, vous pouvez le faire en important vos listes en tant que dossier, plutôt qu’en tant que fichier. csv. Vous devez créer un dossier contenant votre fichier. csv. Ensuite, ajoutez vos images dans le même dossier, soit dans le dossier racine, soit dans un sous-dossier. Vous devez entrer le chemin d’accès complet, y compris le nom du dossier racine, dans le champ.

> [!TIP]
> Pour obtenir de meilleurs résultats lors de l’importation de vos listes en tant que dossier, assurez-vous d’utiliser la dernière version de Microsoft Edge, chrome ou Firefox.

Par exemple, si votre dossier racine est nommé **my_folder** et que vous souhaitez utiliser une image appelée **screenshot1.png** pour **DesktopScreenshot1** , vous pouvez ajouter screenshot1.png à la racine de ce dossier, puis entrer **my_folder/screenshot1.png** dans le champ **DesktopScreenshot1** . Si vous avez créé un dossier images dans votre dossier racine, puis que vous l’avez placé screenshot1.jpg ici, vous devez entrer **my_folder screenshot1.png/images/** . Notez qu’après avoir importé vos listes à l’aide d’un dossier, les chemins d’accès à vos images sont convertis en URL vers les fichiers dans l’espace partenaires la prochaine fois que vous exportez vos annonces. Vous pouvez copier et coller ces URL pour les réutiliser (par exemple, pour utiliser les mêmes ressources dans plusieurs langages de liste). 

> [!IMPORTANT]
> Si votre liste exportée inclut des codes de fin, sachez que la suppression de l’URL du code de fin ou de son image miniature de votre fichier. csv supprimera complètement le fichier supprimé de l’espace partenaires et vous ne pourrez plus y accéder (sauf s’il est également utilisé dans une autre liste où il n’a pas été supprimé). 

## <a name="import-listings"></a>Importer les listings

Une fois que vous avez entré toutes vos modifications dans le fichier. csv (et inclus les ressources que vous souhaitez télécharger), vous devez enregistrer votre fichier avant de le charger. Si vous utilisez une version de Microsoft Excel qui prend en charge l’encodage UTF-8, veillez à sélectionner **Enregistrer sous** et à utiliser le format **CSV UTF-8 (délimité par des virgules) (*. csv)** . Si vous utilisez un autre éditeur pour afficher et modifier votre fichier. csv, assurez-vous que le fichier. csv est encodé au format UTF-8 avant le téléchargement.

Lorsque vous êtes prêt à charger le fichier. csv mis à jour et à importer vos données de référencement, sélectionnez **Importer les listes** sur votre page vue d’ensemble de la soumission. Si vous importez uniquement un fichier. csv, choisissez **Importer. csv** , accédez à votre fichier, puis cliquez sur **ouvrir** . Si vous importez un dossier avec des fichiers image, choisissez Importer un dossier, accédez à votre dossier, puis cliquez sur **Sélectionner un dossier** . Assurez-vous qu’il n’existe qu’un seul fichier. csv dans votre dossier, ainsi que les ressources que vous chargez. 

Lors du traitement de votre fichier. csv importé, une barre de progression reflétant l’état de l’importation et de la validation s’affiche. Cela peut prendre un certain temps, surtout si vous avez beaucoup de listes et/ou de fichiers image. 

Si nous détectons des problèmes, vous verrez une remarque indiquant que vous devrez effectuer toutes les mises à jour nécessaires, puis réessayer. Sélectionnez le lien **afficher les erreurs** pour voir les champs non valides et pourquoi. Vous devez corriger ces problèmes dans votre fichier. csv (ou remplacer les ressources non valides), puis réimporter vos listes.

> [!TIP]
> Vous pouvez accéder à ces informations ultérieurement via le lien **afficher les erreurs pour la dernière importation** .

Aucune des informations de votre fichier. csv n’est enregistrée dans l’espace partenaires tant que toutes les erreurs de votre fichier n’ont pas été résolues, même pour les champs sans erreurs. Une fois que vous avez importé un fichier. csv qui n’a pas d’erreurs, les informations relatives à la liste que vous avez fournies seront enregistrées dans l’espace partenaires et seront utilisées pour cette soumission.

Vous pouvez continuer à effectuer des mises à jour de vos annonces en important un autre fichier. csv mis à jour ou en apportant des modifications directement dans l’espace partenaires.

## <a name="add-ons"></a>Modules complémentaires

Pour les modules complémentaires, l’importation et l’exportation de listes de magasins utilisent le même processus que celui décrit ci-dessus, à ceci près que vous verrez uniquement les trois champs pertinents pour les [listes du magasin de modules complémentaires](create-add-on-store-listings.md): **Description** , **titre** et **StoreLogo300x300** (sous forme d' **icône** dans la page de la liste des boutiques dans l’espace partenaires). Le champ **titre** est obligatoire et les deux autres champs sont facultatifs.

Notez que vous devez importer et exporter des listes de magasins séparément pour chaque module complémentaire de votre application en accédant à la page de présentation de la soumission du module complémentaire.


