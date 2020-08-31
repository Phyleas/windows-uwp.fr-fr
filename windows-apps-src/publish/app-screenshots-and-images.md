---
Description: Vous pouvez sélectionner les captures d’écran, les logos et d’autres ressources artistiques (telles que les codes de fin et les images promotionnelles) à inclure dans la liste des boutiques de votre application.
title: Captures d’écran, images et bandes-annonces de l’application
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, code de fin, vidéo, capture d’écran, image, icône, liste des boutiques, images de la liste des boutiques
ms.localizationpriority: medium
ms.openlocfilehash: da9d6517a43550693596d15c735e3134c5c60a7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155313"
---
# <a name="app-screenshots-images-and-trailers"></a>Captures d’écran, images et bandes-annonces de l’application

Les images bien conçues constituent l’une des principales façons de représenter votre application à des clients potentiels dans le Store.

Vous pouvez fournir des [captures d’écran](#screenshots), des [logos](#store-logos), des [codes](#trailers)de fin et d’autres éléments d’art à inclure dans la liste des boutiques de votre application. Certaines d’entre elles sont requises, et certaines sont facultatives (bien que certaines des images facultatives soient importantes à inclure pour le meilleur affichage du magasin).

Au cours du [processus d’envoi](app-submissions.md)de l’application, vous fournissez ces ressources art à l’étape de liste des [boutiques](create-app-store-listings.md) . Notez que les images utilisées dans le magasin et leur mode d’affichage peuvent varier selon le système d’exploitation du client et d’autres facteurs.

Le magasin peut également utiliser l’icône de votre application et d’autres images que vous incluez dans le package de votre application. Exécutez le [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) pour déterminer si des images requises sont manquantes avant d’envoyer votre application. Pour obtenir des conseils et des recommandations sur ces images, consultez [icônes et logos d’application](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Captures d’écran.

Les captures d’écran sont les images de votre application que voient vos clients dans la description du Windows Store.

Vous avez la possibilité de fournir des captures d’écran pour les différentes familles d’appareils prises en charge par votre application afin que les captures d’écran appropriées s’affichent lorsqu’un client affiche la liste des magasins de votre application sur ce type d’appareil. 

Une seule capture d’écran (pour toutes les familles d’appareils) est requise pour votre soumission, même si vous pouvez en fournir plusieurs ; captures d’écran allant jusqu’à 9 postes de travail et jusqu’à 8 captures d’écran pour les autres familles d’appareils. Nous vous suggérons de fournir au moins quatre captures d’écran pour chaque famille d’appareils que votre application prend en charge afin que les utilisateurs puissent voir comment l’application s’affiche sur leur type d’appareil. (N’incluez pas de captures d’écran pour les familles d’appareils non prises en charge par votre application.) Notez que les captures d’écran du **Bureau** seront également présentées aux clients sur surface Hub appareils.

> [!NOTE]
> Microsoft Visual Studio fournit un [outil pour vous aider à capturer des captures d’écran](/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Chaque capture d’écran doit être un fichier. png dans l’orientation paysage ou portrait, et la taille du fichier ne peut pas être supérieure à 50 Mo.

Les exigences de taille varient en fonction de la famille d’appareils :
- Bureau : 1366 x 768 pixels ou plus. Prend en charge les images 4K (3840 x 2160). (S’affichera également pour les clients sur Surface Hub appareils).
- Mobile : les images doivent être de l’une des valeurs suivantes : 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 ou 480 x 800 pixels.
- Xbox : 3480 x 2160 pixels ou plus. Prend en charge les images 4K (3840 x 2160).
- Holographique : 1268 x 720 pixels ou plus. Prend en charge les images 4K (3840 x 2160).

Pour un affichage optimal, gardez à l’esprit les recommandations suivantes lors de la création de vos captures d’écran :
- Conservez les éléments visuels et le texte critiques dans les 3/4 premiers de l’image. Les superpositions de texte peuvent apparaître au bas de 1/4. 
- N’ajoutez pas de logos, d’icônes ou de messages marketing supplémentaires à vos captures d’écran.
- N’utilisez pas de couleurs extrêmement claires ou sombres ou des bandes à contraste élevé qui peuvent interférer avec la lisibilité des superpositions de texte.

Vous pouvez également fournir une brève légende qui décrit chaque capture d’écran en 200 caractères ou moins.

> [!TIP]
> Les captures d’écran sont affichées dans votre liste dans l’ordre. Après avoir téléchargé vos captures d’écran, vous pouvez les glisser-déplacer pour les réorganiser. 

Notez que si vous créez des listes de magasins pour [plusieurs langues](supported-languages.md), vous disposez d’une page de **liste de magasins** pour chacune d’elles. Vous devez charger des images pour chaque langue séparément (même si vous utilisez les mêmes images), et fournir des sous-titres à utiliser pour chaque langue. (Si vous avez des listes de magasins dans de nombreuses langues, il peut s’avérer plus facile de les mettre à jour en [exportant vos données d’annonce et en mode hors connexion](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Stocker des logos

Vous pouvez télécharger des logos de magasin pour créer un affichage plus personnalisé dans le Store. Nous vous recommandons de fournir ces images afin que le Listing de votre boutique s’affiche de manière optimale sur tous les appareils et versions de système d’exploitation pris en charge par votre application. Notez que si votre application est disponible pour les clients sur Xbox, certaines de ces images sont requises.

Vous pouvez fournir ces images en tant que fichiers. png (pas plus de 50 Mo), chacun d’eux devant suivre les instructions ci-dessous.

### <a name="23-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>2:3 image de l’affiche (720 x 1080 ou 1440 x 2160 pixels)

Utilisé comme image du logo principal pour les clients sur les appareils Windows 10 et Xbox, nous **vous recommandons vivement** de fournir cette image pour garantir un affichage correct. Votre annonce peut ne pas paraître correcte si vous ne l’incluez pas et ne sera pas cohérente avec les autres annonces que les clients voient lors de la navigation dans le Store. Cette image peut également être utilisée dans les résultats de la recherche ou dans des regroupements organisés de façon éditoriale.

Cette image doit inclure le nom de votre application, et tout texte sur l’image doit répondre aux exigences de lisibilité accessibles (taux de contraste 4,51). Notez que les superpositions de texte peuvent apparaître sur le trimestre inférieur de cette image. Assurez-vous que vous n’incluez pas de texte ou d’image clé ici.

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **obligatoire** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, car les superpositions de texte peuvent apparaître sur le trimestre inférieur de l’image.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 boîte (1080 x 1080 ou 2160 x 2160 pixels)

Cette image peut apparaître dans différentes pages du magasin pour Windows 10 (y compris la Xbox) et si vous ne fournissez pas l’image de l' **affiche 2:3** , elle sera utilisée comme logo principal. Cette image doit également inclure le nom de votre application. Les superpositions de texte peuvent apparaître sur le trimestre inférieur de cette image, donc n’incluez pas de texte ou d’image clé ici. Veillez à inclure le nom de votre application dans cette image. 

> [!NOTE]
> Si votre application est disponible pour les clients sur Xbox, cette image est **obligatoire** et doit inclure le titre du produit. Le titre doit apparaître dans les trois quarts supérieurs de l’image, car les superpositions de texte peuvent apparaître sur le trimestre inférieur de l’image.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>icône de vignette d’application 1:1 (300 x 300 pixels)

Cette image est nécessaire pour un affichage correct sur Windows Phone 8,1 et versions antérieures. Si votre application précédemment publiée prend en charge Windows Phone 8,1 ou une version antérieure et que vous ne fournissez pas cette image, les clients verront une icône vide avec la liste de votre application. (Cela s’applique également aux clients sur Windows 10 si votre application n’a que des packages ciblant Windows Phone 8,1 ou une version antérieure.)

Si votre envoi comprend *uniquement* des packages UWP, vous n’avez pas besoin de fournir cette image (sauf si vous activez la case à cocher pour les  **clients sur Windows 10 et Xbox, vous pouvez afficher des images de logos téléchargés à la place des images de mes packages**, comme décrit dans la section suivante).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Afficher uniquement les images de logo chargées dans le Store

Vous avez la possibilité d’empêcher le magasin d’utiliser les images de logos dans les packages de votre application lors de l’affichage de votre annonce auprès des clients sur Windows 10 (y compris la Xbox), et de faire en sorte que le magasin utilise uniquement les images que vous téléchargez. Cela vous permet de mieux contrôler l’apparence de votre application dans différents affichages dans le magasin pour les clients sur Windows 10 (y compris la Xbox). (Si votre application précédemment publiée prend en charge des versions de système d’exploitation antérieures, ces clients peuvent toujours voir les images de vos packages.)

Pour que le magasin utilise uniquement les images que vous téléchargez (pour les clients sur Windows 10, y compris la Xbox) et que vous n’utilisiez pas d’images de vos packages, cochez la case **pour les clients sur Windows 10 et Xbox, affichez les images de logos téléchargés à la place des images de mes packages**.

Lorsque vous activez cette case, une nouvelle section appelée **Affichage des images par le Windows Store** s’affiche. Ici, vous pouvez télécharger 3 images, y compris la taille de la vignette de l' **application 1:1 (300 x 300 pixels)** (si vous activez la case à cocher, le champ pour indiquer que l’image sera déplacée dans cette section). Nous vous recommandons de fournir les trois tailles d’image si vous utilisez cette option : 300 x 300, 150 x 150 et 71 x 71 pixels. Toutefois, seule la taille de 300 x 300 est requise.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Remorques et ressources supplémentaires

Cette section vous permet de fournir des illustrations pour vous aider à afficher votre produit plus efficacement dans le Store. Nous vous recommandons de fournir ces images pour créer une liste plus attrayante de magasins.

> [!TIP]
> L’image de [super héros 16:9](#windows-10-and-xbox-image-169-super-hero-art) est particulièrement recommandée si vous envisagez d’inclure des codes de fin [vidéo](#trailers) dans votre liste de magasins. Si vous ne l’incluez pas, vos codes de fin ne s’affichent pas en haut de votre liste.


### <a name="trailers"></a>Bandes-annonce

Les codes de fin sont des courtes vidéos qui offrent aux clients un moyen de voir votre produit en action, afin qu’ils puissent mieux comprendre ce à quoi il ressemble. Ils s’affichent en haut de la liste des boutiques de votre application (à condition que vous incluiez une image [art 16:9 super héros](#windows-10-and-xbox-image-169-super-hero-art) ). 

Les codes de fin sont encodés avec [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming), qui adapte la qualité d’un flux vidéo envoyé aux clients en temps réel en fonction de la bande passante et des ressources processeur disponibles.

> [!NOTE]
> Les codes de fin sont affichés uniquement aux clients sur Windows 10, version 1607 ou ultérieure (qui comprend la Xbox).

### <a name="upload-trailers"></a>Charger des codes de fin

Vous pouvez ajouter jusqu’à 15 codes de fin à votre annonce de boutique. Assurez-vous qu’ils remplissent toutes les [conditions requises](#trailer-requirements) listées ci-dessous.

Pour chaque code de fin que vous fournissez, vous devez télécharger un fichier vidéo (. MP4 ou. mov), une image miniature et un titre.

> [!IMPORTANT]
> Lorsque vous utilisez des codes de fin, vous devez également fournir une section d’image de [dessin super héros 16:9](#windows-10-and-xbox-image-169-super-hero-art) pour que vos codes de fin apparaissent en haut de la liste de votre boutique. Cette image s’affichera une fois que la durée de la diffusion de vos codes de fin est terminée.

Suivez ces recommandations pour rendre vos codes de fin efficaces :
- Les codes de fin doivent être de bonne qualité et de longueur minimale (60 secondes ou moins et 2 Go recommandés). 
- Utilisez une autre miniature pour chaque code de fin afin que les clients sachent qu’ils sont uniques.
- Étant donné que certaines dispositions de magasin peuvent légèrement rogner le haut et le bas de votre code de fin, vérifiez que les informations de clé apparaissent au centre de l’écran.
- La fréquence d’images et la résolution doivent correspondre au matériel source. Par exemple, la capture de contenu sur 720p60 doit être encodée et téléchargée sur 720p60. 

Vous devez également suivre les exigences indiquées ci-dessous.

**Pour ajouter des codes de fin à votre annonce :**
1. Téléchargez votre **fichier vidéo** de code de fin dans la zone indiquée. Une zone de liste déroulante s’affiche également au cas où vous souhaiteriez réutiliser un code de fin que vous avez lu en charge (peut-être pour une liste de magasins dans une autre langue).
2. Une fois que vous avez téléchargé le code de fin, vous devez télécharger une **image miniature** pour y accéder. Il doit s’agir d’un fichier. png de 1920 x 1080 pixels. il s’agit généralement d’une image continue du code de fin.
3. Cliquez sur l’icône de crayon pour ajouter un **titre** pour votre code de fin (255 caractères au maximum).
4. Si vous souhaitez ajouter d’autres codes de fin à la liste, cliquez sur **Ajouter un code de fin** et répétez les étapes ci-dessus.

> [!TIP]
> Si vous avez créé des listes de magasins dans plusieurs langues, vous pouvez sélectionner **choisir parmi les codes** de fin existants pour réutiliser les codes de fin que vous avez déjà téléchargés. Vous n’avez pas à les télécharger individuellement pour chaque langue.

Pour supprimer un code de fin d’une liste, cliquez sur le **symbole X** en regard de son nom de fichier. Vous pouvez choisir de le supprimer uniquement de la liste actuelle du magasin dans laquelle vous travaillez, ou de le supprimer de tous les listings du produit (dans chaque langue).


### <a name="trailer-requirements"></a>Spécifications de code de fin

Lorsque vous fournissez vos codes de fin, veillez à respecter les conditions suivantes :

- Le format vidéo doit être MOV ou MP4.
- La taille du fichier de la remorque ne doit pas dépasser 2 Go.
- La résolution vidéo doit être de 1920 x 1080 pixels.
- La miniature doit être un fichier PNG avec une résolution de 1920 x 1080 pixels.
- Le titre ne peut pas dépasser 255 caractères.
- N’incluez pas de classements d’âge dans vos codes de fin.

> [!WARNING]
> L’exception à la nécessité d’inclure les évaluations d’âge dans vos codes de fin s’applique **uniquement** aux codes de fin de la **Microsoft Store** affichés **sur la page du produit**. Tout code de fin publié en dehors de l’espace partenaires, qui n’est pas destiné à être affiché exclusivement sur la page de produit de Microsoft Store **doit** afficher des informations d’évaluation incorporées, le cas échéant, conformément aux directives de l’autorité d’évaluation appropriée.  

À l’instar des autres champs de la page de liste du Store, les codes de fin doivent être certifiés avant de pouvoir être publiés sur le Microsoft Store. Veillez à ce que vos codes de fin soient conformes aux [stratégies de Microsoft Store](store-policies.md).

Des exigences supplémentaires varient en fonction du type de fichier.

#### <a name="mov"></a>MOV

| Vidéo | Audio | 
| --- | --- | 
| <ul><li>1080p ProRes (HQ, le cas échéant)</li><li>Fréquence Native ; 29,97 FPS par défaut</li></ul> | <ul><li>Stéréo requis</li><li>Niveau audio recommandé :-16 Loudness/LUFS</li></ul> |


#### <a name="mp4"></a>MP4 

| Vidéo | Audio |
| --- | --- |
| <ul><li>Codec : [H. 264](/windows/desktop/DirectShow/h-264-video-types) (AVC1)  </li><li>Analyse progressive (aucune entrelacement)</li><li>Profil élevé</li><li>2 frames B consécutifs</li><li>Groupe d’images fermé. GOP de la moitié de la fréquence d’images</li><li>CABAC</li><li>50 Mo/s </li><li>Espace colorimétrique : 4.2.0</li></ul> | <ul><li>Codec : AAC-LC</li><li>Canaux : son stéréo ou surround</li><li>Taux d’échantillonnage : 48 KHz</li><li>Débit binaire audio : 384 Ko/s pour stéréo, 512 Ko/s pour le son surround</li></ul> |

> [!WARNING]
> Les clients ne peuvent pas entendre l’audio pour les fichiers MP4 encodés avec des codecs autres que AVC1.

Pour les fichiers mezzanine H. 264, nous vous recommandons les éléments suivants :
- Conteneur : MP4
- Aucune liste de modifications (ou vous risquez de perdre la synchronisation AV)
- Moov Atom au début du fichier (démarrage rapide)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Image Windows 10 et Xbox (16:9 super héros art)

Dans la section **image de Windows 10 et Xbox** , l’image **16:9 super hero art (1920 x 1080 ou 3840 x 2160 pixels)** est utilisée dans différentes dispositions du Microsoft Store sur tous les types d’appareils Windows 10 (y compris la Xbox). Nous vous recommandons de fournir cette image, quelle que soit la version du système d’exploitation ou le type d’appareil ciblé par votre application.

Cette image est *nécessaire* pour un affichage correct si votre liste comprend des codes de fin [vidéo](#trailers). Pour les clients sur Windows 10, version 1607 ou ultérieure (qui comprend la Xbox), elle est utilisée en tant qu’image principale en haut de la liste de votre boutique (ou apparaît après la fin de la lecture de toutes les remorques). Il peut également être utilisé pour présenter votre application dans des présentations promotionnelles dans le magasin. Notez que cette image ne doit pas inclure le titre du produit ou un autre texte.

Voici quelques conseils à prendre en compte lors de la conception de cette image :

- L’image doit être de type. png 1920 x 1080 pixels ou 3840 x 2160 pixels.
- Sélectionnez une image dynamique qui est associée à l’application pour la reconnaissance et la différenciation. Évitez les photos issues de banques d’images ou les images génériques.
- N’incluez pas de texte dans l’image.
- Évitez de placer des éléments visuels clés dans le tiers inférieur de l’image (car dans certaines dispositions, nous pouvons appliquer un dégradé sur cette partie).
- Placez les informations les plus importantes au centre de l’image (dans certaines dispositions, nous pouvons rogner l’image).
- Réduire l’espace vide.
- Éviter de montrer l’interface utilisateur de votre application et n’utilisez pas d’images spécifiques à des appareils.
- Évitez les thèmes politiques et nationaux, les drapeaux ou les symboles religieux.
- N’incluez pas d’images en rapport avec des gestes irrespectueux, la nudité, le jeu, l’argent, la drogue, le tabac ou l’alcool.
- N’utilisez pas d’armes pointant vers l’utilisateur ou une violence excessive.

Alors que cette image nous permet de prendre en compte votre application pour les opportunités promotionnelles proposées, elle ne garantit pas que votre application sera proposée. Pour plus d’informations, consultez [rendre votre application facile à promouvoir](make-your-app-easier-to-promote.md) .


### <a name="xbox-images"></a>Images Xbox

Ces images sont requises pour un affichage correct si vous publiez votre application sur Xbox. 

Vous pouvez télécharger 3 tailles différentes :
- **Image clé personnalisée, 584 x 800 pixels**: doit inclure le titre du produit. Une barre de personnalisation est requise sur cette image. Conservez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, car une superposition peut apparaître sur le trimestre inférieur.
- **Intitulé héros art, 1920 x 1080 pixels**: doit inclure le titre du produit. Conservez le titre et toutes les images clés dans les trois quarts supérieurs de l’image, car une superposition peut apparaître sur le trimestre inférieur.
- **Art du carré promotionnel, 1080 x 1080 pixels**: *ne doit pas* inclure le titre du produit.

> [!NOTE]
> Pour un meilleur affichage sur la Xbox, vous devez également fournir une image **9:16 (720 x 1080 ou 1440 x 2160 pixels)** dans la section [logos des boutiques](#store-logos) .


### <a name="holographic-image"></a>Image holographique

L’image **2:1 (2400 x 1200)** est utilisée uniquement si votre application prend en charge la famille d’appareils holographiques. Si c’est le cas, nous vous recommandons de fournir cette image.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Images uniquement pour Windows 8. x et/ou Windows Phone 8. x 

Si votre application envoyée précédemment prend en charge des versions de système d’exploitation antérieures (Windows 8. x et/ou Windows Phone 8. x), ces images doivent être fournies pour que nous puissions tenir compte de votre application dans des dispositions promotionnelles (bien qu’elles ne garantissent pas que votre application sera proposée). Si votre application ne prend pas en charge ces versions antérieures du système d’exploitation, ignorez cette section. (Cette section était autrefois appelée **images promotionnelles facultatives**.)

**Pour les Windows Phone 8,1 et versions antérieures**, deux tailles d’image peuvent être utilisées dans des présentations promotionnelles : **1000 x 800 pixels (5:4)** et **358 x 358 pixels (1:1)**. Si votre application s’exécute sur Windows Phone 8,1 ou une version antérieure, nous vous recommandons de fournir des images dans ces deux tailles.  

> [!TIP]
> Veillez à fournir une image d’icône de vignette d’application 300 x 300 dans la section [logos des boutiques](#store-logos) pour toute soumission prenant en charge Windows Phone 8,1 ou une version antérieure. Cela permet de s’assurer que votre application n’apparaît pas dans le magasin avec une icône vide.  

**Pour Windows 8.1 et versions antérieures**, certaines dispositions promotionnelles peuvent utiliser une image de la taille de **414 x 180** pixels. Si votre application s’exécute sur Windows 8.1 ou une version antérieure, nous vous recommandons de fournir une image de cette taille.