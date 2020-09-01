---
title: Bonnes pratiques concernant les activités utilisateur
description: Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités des utilisateurs.
keywords: activité de l’utilisateur, activités de l’utilisateur, chronologie, Cortana reprendre là où vous l’avez quittée, Cortana reprend là où je l’ai quitté, le projet Rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f4e41ac4f340491e551fccc1c1d4700bbe8d8004
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172953"
---
# <a name="user-activities-best-practices"></a>Bonnes pratiques concernant les activités utilisateur

Ce guide décrit les pratiques recommandées pour la création et la mise à jour des activités des utilisateurs. Pour obtenir une vue d’ensemble de la fonctionnalité d’activités utilisateur sur Windows, consultez [poursuivre l’activité des utilisateurs, même sur plusieurs appareils](./useractivities.md). Ou consultez la [section activités](/windows/project-rome/user-activities/) de l’utilisateur de Project Rome pour les implémentations d’activités sur d’autres plateformes de développement.

## <a name="when-to-create-or-update-user-activities"></a>Quand créer ou mettre à jour des activités de l’utilisateur

Étant donné que chaque application est différente, il revient à chaque développeur de déterminer la meilleure façon de mapper les actions au sein de l’application aux activités de l’utilisateur. Vos activités utilisateur seront présentées dans Cortana et chronologie, qui se concentrent sur l’amélioration de la productivité et de l’efficacité des utilisateurs en leur aidant à revenir au contenu qu’ils ont consulté dans le passé.

**Recommandations générales**

* **Enregistrez une activité unique pour un groupe d’actions utilisateur associées.** Cela s’applique particulièrement aux sélections musicales ou aux émissions TV : une seule activité peut être mise à jour à intervalles réguliers pour refléter la progression de l’utilisateur. Dans ce cas, vous aurez une seule activité utilisateur avec plusieurs éléments d’historique représentant des périodes d’engagement sur plusieurs jours ou semaines. Il en va de même pour les activités basées sur les documents sur lesquelles l’utilisateur effectue une progression progressive dans votre application.
* **Stocker les données utilisateur dans le Cloud.** Si vous souhaitez prendre en charge des activités inter-appareils, vous devez vous assurer que le contenu requis pour réactiver cette activité est stocké dans un emplacement du Cloud. Les activités spécifiques à l’appareil s’affichent dans la chronologie de l’appareil sur lequel l’activité a été créée, mais elles peuvent ne pas s’afficher sur d’autres appareils.
* **Ne créez pas d’activités pour les actions que les utilisateurs n’auront pas besoin de reprendre.** Si votre application est utilisée pour effectuer des opérations ponctuelles simples qui ne persistent pas, vous n’avez probablement pas besoin de créer une activité utilisateur.
* **Ne créez pas d’activités pour les actions effectuées par d’autres utilisateurs.** Si un compte externe envoie un message à l’utilisateur ou @-mentions à l’intérieur de votre application, vous ne devez pas créer d’activité pour ce. Ce type d’action est mieux servi par les notifications du centre de notifications.
  * Les scénarios de collaboration sont une exception : si plusieurs utilisateurs travaillent simultanément sur la même activité (par exemple, un document Word), il y aura des cas où un autre utilisateur aura apporté des modifications après votre utilisateur. Dans ce cas, vous souhaiterez peut-être mettre à jour l’activité existante pour refléter les modifications apportées au document. Cela implique la mise à jour des données de contenu de l’activité de l’utilisateur existant sans créer un nouvel élément d’historique.

**Instructions pour des types spécifiques d’applications**

Même si chaque application est différente, la plupart des applications sont classées dans l’un des modèles d’interaction suivants.
* **Applications basées sur un document** : créez une activité par document, avec un ou plusieurs éléments d’historique reflétant les périodes d’utilisation. Il est important de mettre à jour votre activité au fur et à mesure que des modifications sont apportées au document.
* **Jeux** : créez une activité pour chaque jeu enregistré ou monde. Si votre jeu ne prend en charge qu’une seule séquence de niveaux, vous pouvez republier la même activité dans le temps, bien que vous souhaitiez mettre à jour les données de contenu pour afficher la progression ou les réalisations les plus récentes.
* **Applications Utilitaires** : s’il n’y a rien dans votre application que les utilisateurs doivent quitter et reprendre, vous n’avez pas besoin d’utiliser les activités de l’utilisateur. Un bon exemple est une application simple comme Calculator.
* **Applications métier** : de nombreuses applications existent pour gérer des tâches ou des flux de travail simples. Créez une activité pour chaque Workflow distinct accessible par le biais de votre application (par exemple, les notes de frais seraient chacune une activité distincte, afin que l’utilisateur puisse cliquer sur une activité pour voir si un rapport particulier a été approuvé).
* **Applications de lecture multimédia** : créez une activité par regroupement logique de contenu (par exemple, une sélection, un programme ou du contenu autonome). La question sous-jacente pour les développeurs d’applications est de savoir si une partie de contenu (épisode TV, chanson) est comptabilisée comme contenu autonome ou comme partie d’une collection. En règle générale, si l’utilisateur choisit de lire une collection ou un contenu séquentiel, la collection est l’ensemble de l’activité. S’ils choisissent de lire un seul élément de contenu, il s’agit de l’activité. Consultez les instructions plus spécifiques ci-dessous.
  * **Musique : album/artiste/genre** : si l’utilisateur sélectionne un album, un artiste ou un genre et atteint la **lecture**, cette collection est l’activité ; n’écrivez pas une activité distincte pour chaque chanson. Pour les collections courtes comme un album unique ou les collections en cours de lecture dans un ordre aléatoire, il est possible que vous n’ayez pas besoin de mettre à jour l’activité pour refléter la position actuelle de l’utilisateur. Pour une lecture séquentielle longue, telle qu’un album ou une sélection, il peut être judicieux d’enregistrer votre position dans l’album.
  * **Musique : playlists intelligentes** : les applications qui lisent de la musique dans un ordre aléatoire doivent enregistrer une activité unique pour cette sélection. Si l’utilisateur lit la sélection une deuxième fois, vous créez des enregistrements d’historique supplémentaires pour la même activité. L’enregistrement de la position actuelle de l’utilisateur dans la sélection n’est pas nécessaire, car le classement est aléatoire.
  * **Série TV** : Si votre application est configurée pour jouer le prochain épisode après l’achèvement de la série active, vous devez écrire une seule activité pour la série TV. Lorsque vous jouez aux différents épisodes de plusieurs sessions d’affichage, vous mettez à jour votre activité pour refléter la position actuelle dans la série, et plusieurs enregistrements d’historique sont créés.
  * **Film** : un film est un élément de contenu unique et doit avoir son propre enregistrement d’historique. Si l’utilisateur cesse de regarder la vidéo au cours de la procédure, il est souhaitable d’enregistrer sa position. Lorsqu’ils souhaitent le reprendre à l’avenir, l’activité peut reprendre le film là où ils se sont arrêtés, ou même demander à l’utilisateur s’il souhaite reprendre ou démarrer au début.

## <a name="user-activity-design"></a>Conception de l’activité utilisateur

Les activités de l’utilisateur se composent de trois composants : un URI d’activation, des données visuelles et des métadonnées de contenu.
* L’URI d’activation est un URI qui peut être passé à une application ou à une expérience afin de reprendre l’application avec un contexte spécifique. En règle générale, ces liens prennent la forme d’un gestionnaire de protocole pour un schéma (par exemple, « My-app://page2 ? action = Edit »). Il revient au développeur de déterminer comment les paramètres d’URI seront gérés par leur application. Pour plus d’informations, consultez [gérer l’activation d’URI](./handle-uri-activation.md) .
* Les données visuelles, composées d’un ensemble de propriétés obligatoires et facultatives (par exemple : titre, description ou éléments de carte adaptative), permettent aux utilisateurs d’identifier visuellement une activité. Voir ci-dessous pour obtenir des instructions sur la création de visuels de carte adaptative pour votre activité.
* Les métadonnées de contenu sont des données JSON qui peuvent être utilisées pour regrouper et récupérer des activités dans un contexte spécifique. En général, cela prend la forme de http://schema.org données. Voir ci-dessous pour obtenir des instructions sur la saisie de ces données.

### <a name="adaptive-card-design-guidelines"></a>Instructions pour la conception de cartes adaptatives

Lorsque les activités s’affichent dans la chronologie, elles s’affichent à l’aide de l' [infrastructure de carte adaptative](/adaptive-cards/). Si le développeur ne fournit pas de carte adaptative pour chaque activité, la chronologie crée automatiquement une carte simple basée sur le nom de l’application/l’icône, le champ titre requis et le champ description facultative. 

Les développeurs d’applications sont encouragés à fournir des cartes personnalisées à l’aide du schéma JSON de carte adaptative simple. Pour obtenir des instructions techniques sur la construction d’objets carte adaptative, consultez la documentation sur les [cartes adaptatives](/adaptive-cards/authoring-cards/getting-started) . Reportez-vous aux instructions ci-dessous pour concevoir des cartes adaptatives dans les activités des utilisateurs.
* Utiliser des images
  * Utilisez une image unique pour chaque activité, si possible. Le nom et l’icône de votre application s’affichent automatiquement en regard de la carte de votre activité ; des images supplémentaires aideront les utilisateurs à localiser l’activité qu’ils recherchent.
  * Les images ne doivent pas inclure de texte que l’utilisateur est censé lire. Ce texte ne sera pas disponible pour les utilisateurs ayant des besoins d’accessibilité et ne peut pas être recherché.
  * Si l’image ne contient pas de texte et peut être rognée à un ratio de 2:1, vous devez l’utiliser comme une image d’arrière-plan. Il en résulte une carte d’activité en gras qui sera mise en évidence dans la chronologie. L’image sera légèrement obscurcie pour s’assurer que le texte reste visible sur la carte. dans ce cas, il est recommandé d’utiliser uniquement le nom de l’activité, car un texte plus petit peut devenir difficile à lire.
  * Si l’image ne peut pas être rognée à 2:1, vous devez la placer dans la carte d’activité.  
    * Si les proportions sont carrées ou portrait, ancrez l’image sur le côté droit de la carte sans marges.
    * Si les proportions sont paysage, ancrez l’image dans le coin supérieur droit de la carte.
* Chaque activité est requise pour fournir un nom d’activité, qui doit toujours être affiché.
  * Ce nom doit être affiché dans l’angle supérieur gauche de la carte à l’aide de l’option de texte long gras. Il est important que le nom soit facilement identifiable, car il s’agit de la seule partie que les utilisateurs verront quand l’activité est affichée dans les scénarios Cortana. L’indication du même nom dans la chronologie permet aux utilisateurs de parcourir plus facilement un grand nombre d’activités.
* Utilisez le même style visuel pour toutes les activités de votre application, afin que les utilisateurs puissent facilement localiser les activités de votre application dans la chronologie.
  * Par exemple, les activités doivent toutes utiliser la même couleur d’arrière-plan.
* Utilisez les informations de texte supplémentaires avec modération. 
  * Évitez de remplir la carte avec du texte et utilisez uniquement des informations supplémentaires qui aident les utilisateurs à trouver l’activité appropriée ou qui reflète des informations d’État (telles que la progression actuelle d’une tâche particulière).

### <a name="content-metadata-guidelines"></a>Instructions relatives aux métadonnées de contenu

Les activités des utilisateurs peuvent également contenir des métadonnées de contenu, que Windows et Cortana utilisent pour catégoriser les activités et générer des inférences. Les activités peuvent ensuite être regroupées autour d’un sujet particulier, tel qu’un emplacement (si l’utilisateur recherche des vacances), un objet (si l’utilisateur recherche un article) ou une action (si l’utilisateur fait des achats pour un produit particulier sur différentes applications et sites Web). Il est judicieux de représenter à la fois les noms et les verbes impliqués dans une activité. 

Dans l’exemple suivant, le code JSON des métadonnées de contenu, suivant les normes de [Schema.org](https://schema.org/), représente le scénario : « John a joué à des oiseaux en colère avec Steve ».

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>API clés

* [Espace de noms UserActivities](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Rubriques connexes

* [Activités de l’utilisateur (documents de Rome du projet)](/windows/project-rome/user-activities/)
* [Cartes adaptatives](/adaptive-cards/)
* [Visualiseur de cartes adaptatives, exemples](https://adaptivecards.io/)
* [Gérer l’activation des URI](./handle-uri-activation.md)
* [En contact avec vos clients sur n’importe quelle plateforme à l’aide du Microsoft Graph, du flux d’activités et des cartes adaptatives](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)