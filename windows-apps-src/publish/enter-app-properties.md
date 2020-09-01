---
Description: La page Propriétés de l’application du processus de soumission d’application vous permet de définir la catégorie de votre application, ainsi que les préférences matérielles ou d’autres déclarations.
title: Entrer les propriétés d’une application
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, paramètres de jeu, mode d’affichage, configuration requise, configuration matérielle requise, matériel minimal, matériel recommandé, politique de confidentialité, informations de contact du support, site Web d’application, informations de support
ms.localizationpriority: medium
ms.openlocfilehash: f945b9908a86d660bde9713ca353f1f3a438bf90
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167403"
---
# <a name="enter-app-properties"></a>Entrer les propriétés d’une application

La page **Propriétés** du [processus d’envoi](app-submissions.md) de l’application vous permet de définir la catégorie de votre application et d’entrer d’autres informations et déclarations. Veillez à fournir des détails complets et précis sur votre application sur cette page.


## <a name="category-and-subcategory"></a>Catégorie et sous-catégorie

Vous devez indiquer la catégorie (et sous-catégorie/genre, le cas échéant) que le magasin doit utiliser pour classer votre application. Vous devez définir une catégorie pour être en mesure de soumettre votre application.

Pour plus d’informations, voir l’article [Tableau des catégories et sous-catégories](category-and-subcategory-table.md).


## <a name="support-info"></a>Informations de support technique

Cette section vous permet de fournir des informations pour aider les clients à en savoir plus sur votre application et sur la façon d’obtenir un support technique.

### <a name="privacy-policy-url"></a>URL de la politique de confidentialité

Vous êtes tenu de vous assurer que votre application est conforme aux lois et réglementations en matière de confidentialité, et de fournir une URL de politique de confidentialité valide ici, si nécessaire.

Dans cette section, vous devez indiquer si votre application accède à, collecte ou transmet des [informations personnelles](/legal/windows/agreements/store-policies#105-personal-information). Si vous répondez **Oui**, une URL de politique de confidentialité est requise. Dans le cas contraire, il est facultatif (Cependant, si nous déterminons que votre application nécessite une stratégie de confidentialité et que vous n’en avez pas fourni, votre soumission risque d’échouer).

> [!NOTE]
> Si nous détectons que vos packages déclarent des [capacités](../packaging/app-capability-declarations.md) qui pourraient permettre l’accès, la transmission ou la collecte d’informations personnelles, nous allons marquer cette question comme **Oui**et vous devrez entrer une URL de politique de confidentialité.

Pour vous aider à déterminer si votre application requiert une stratégie de confidentialité, consultez le [contrat de développement d’application](/legal/windows/agreements/app-developer-agreement) et les stratégies de [Microsoft Store](/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> Microsoft ne fournit pas de politique de confidentialité par défaut pour votre application. De même, votre application n’est couverte par aucune politique de confidentialité Microsoft. 


### <a name="website"></a>Site web

Entrez l’URL de la page web de votre application. Cette URL doit pointer vers une page de votre propre site web, et non vers la description web de votre application dans le Windows Store. Ce champ est facultatif, mais recommandé.

### <a name="support-contact-info"></a>Coordonnées du support technique

Entrez l’URL de la page Web à laquelle vos clients peuvent accéder pour prendre en charge votre application, ou une adresse de messagerie que les clients peuvent contacter pour obtenir de l’aide. Nous vous recommandons d’inclure ces informations pour toutes les soumissions, afin que vos clients sachent comment obtenir un support technique s’ils en ont besoin. Notez que Microsoft ne permet pas à vos clients de prendre en charge votre application.

> [!IMPORTANT]
> Le champ **informations de contact du support** est obligatoire si votre application ou jeu est disponible sur Xbox. Dans le cas contraire, il est facultatif (mais recommandé).


## <a name="game-settings"></a>Paramètres du jeu

Cette section s’affiche uniquement si vous avez sélectionné **jeux** comme catégorie de votre produit. Ici, vous pouvez spécifier les fonctionnalités prises en charge par votre jeu. Les informations que vous fournissez dans cette section s’affichent dans la liste des boutiques du produit.

Si votre jeu prend en charge l’une des options multijoueur, veillez à indiquer le nombre minimal et maximal de joueurs pour une session. Vous ne pouvez pas entrer plus de 1 000 lecteurs minimum ou maximum.

Le **mode multijoueur multiplateforme** signifie que le jeu prend en charge les sessions multijoueur entre les joueurs sur les PC Windows 10 et la Xbox.


## <a name="display-mode"></a>Mode d’affichage

Cette section vous permet d’indiquer si votre produit est conçu pour s’exécuter dans une vue immersif (non 2D) pour [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) sur des appareils PC et/ou HoloLens. Si vous indiquez qu’il s’agit de, vous devez également effectuer les opérations suivantes :
- Sélectionnez **matériel minimum** ou **matériel recommandé** pour le **casque immersif Windows Mixed realisation** dans la section [Configuration requise](#system-requirements) qui s’affiche en bas de la page **Propriétés** .
- Spécifiez la **Configuration des limites** (si le PC est sélectionné) afin que les utilisateurs sachent s’ils sont destinés à être utilisés uniquement en position assise ou debout, ou s’ils autorisent (ou nécessitent) l’utilisateur à se déplacer tout en l’utilisant. 

Si vous avez sélectionné des **jeux** comme catégorie de produit, vous verrez des options supplémentaires dans la sélection du **mode d’affichage** qui vous permettent d’indiquer si votre produit prend en charge la sortie vidéo à 4 Ko, la sortie vidéo HDR (High dynamique Range) ou la fréquence d’actualisation des variables.

Si votre produit ne prend pas en charge l’une de ces options de mode d’affichage, laissez toutes les cases décochées.


## <a name="product-declarations"></a>Déclarations de produit

Les cases à cocher de cette section vous permettent d’indiquer les déclarations qui s’appliquent à votre application. Votre sélection affecte le mode d'affichage de votre application, le public auquel elle est proposée ou la façon dont les clients peuvent l'utiliser.

Pour plus d’informations, consultez [déclarations de produits](./product-declarations.md).

## <a name="system-requirements"></a>Configuration requise

Cette section vous permet d’indiquer si certaines fonctionnalités matérielles sont requises ou recommandées pour exécuter votre application et interagir avec cette dernière. Lorsque vous souhaitez spécifier **Matériel minimum** et/ou **Matériel recommandé** pour un composant matériel, activez la case à cocher (ou indiquez l’option appropriée).

Si vous effectuez des sélections pour **Matériel recommandé**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel recommandé pour les clients utilisant Windows 10, version 1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations.

Si vous effectuez des sélections pour **Matériel minimum**, ces composants sont affichés dans la description du produit sur le Windows Store en tant que matériel minimum requis pour les clients utilisant Windows 10, version 1607 ou ultérieure. Les clients utilisant des versions antérieures du système d’exploitation ne voient pas ces informations. Le Windows Store peut également afficher un avertissement pour les clients qui consultent la description de votre application sur un appareil ne disposant pas du matériel requis. Les clients peuvent quand même télécharger votre application sur des appareils non équipés du matériel nécessaire, mais ils ne sont pas en mesure d’évaluer ou de commenter votre application sur ces appareils. 

Le comportement des clients varie selon les configurations requises spécifiques et la version de Windows du client :

- **Pour les clients utilisant Windows 10, version 1607 ou ultérieure :**
     - La configuration minimale requise et recommandée est affichée dans la description du Windows Store.
     - Le Windows Store vérifie la configuration minimale requise et affiche un avertissement si l’appareil du client ne satisfait pas ces exigences.
- **Pour les clients utilisant des versions antérieures de Windows 10 :**
     - Pour la plupart des clients, la configuration matérielle minimale requise et recommandée s’affiche dans la description au sein du Windows Store (toutefois les clients utilisant une version antérieure du client Windows Store voient seulement la configuration matérielle minimale requise).
     - Le Windows Store tente de vérifier les composants que vous spécifiez dans **Matériel minimum**, à l’exception de **Mémoire**, **DirectX**, **Mémoire vidéo**, **Carte graphique** et **Processeur**. Aucun de ces composants n’est vérifié, et les clients ne voient aucun avertissement sur les appareils qui ne satisfont pas ces exigences. 
- **Pour les clients utilisant Windows 8.x et des versions antérieures ou Windows Phone 8.x et des versions antérieures :**
     - Si vous activez la case à cocher **Matériel minimum** pour **Écran tactile**, cette configuration requise s’affiche dans la description de votre application sur le Windows Store, et les clients dont les appareils n’ont pas d’écran tactile voient un avertissement s’ils tentent de télécharger l’application. Aucune autre configuration requise n’est vérifiée ou affichée dans votre description au sein du Windows Store.

Nous vous recommandons également d’ajouter des vérifications d’exécution pour le matériel spécifié dans votre application, car il peut arriver que le Windows Store ne puisse pas détecter que l’appareil d’un client ne dispose pas des fonctionnalités sélectionnées. Dans ce cas, le client peut toujours télécharger votre application, même si un avertissement s’affiche. Si vous souhaitez empêcher complètement le téléchargement de votre application UWP sur un appareil qui ne répond pas à la configuration minimale requise pour la mémoire ou le niveau DirectX, vous pouvez indiquer la configuration minimale requise dans un [fichier XML StoreManifest](/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Si votre produit nécessite des éléments supplémentaires qui ne sont pas répertoriés dans cette section pour s’exécuter correctement, tels que des imprimantes 3D ou des périphériques USB, vous pouvez également entrer [des configurations système supplémentaires](create-app-store-listings.md#additional-system-requirements) pour la création de votre liste de magasins.