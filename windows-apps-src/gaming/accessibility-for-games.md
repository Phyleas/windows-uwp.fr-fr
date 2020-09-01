---
title: Proposer des jeux accessibles
description: Découvrez comment assurer l’accessibilité de vos jeux. Appliquez le principe de conception de jeux inclusifs pour garantir l’accessibilité des jeux.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, accessibilité, jeux
ms.localizationpriority: medium
ms.openlocfilehash: f90f976f696d5c49e7f772627bbb7d0e3e3d0908
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159433"
---
#  <a name="making-games-accessible"></a>Proposer des jeux accessibles

L’accessibilité contribue à accroître les capacités de l’ensemble des individus et des organisations de la planète, et cela se vérifie également dans le domaine des jeux vidéo. Cet article est conçu pour les développeurs de jeux, les concepteurs de jeux et les producteurs. Il offre une vue d’ensemble des recommandations en matière de conception de jeux accessibles émanant de diverses organisations (répertoriées dans la section de références à la fin de cet article) et présente le principe de conception de jeux inclusifs à appliquer pour améliorer l’accessibilité des jeux.

## <a name="gaming-for-everyone"></a>Jeux tous publics

Chez Microsoft, nous pensons que les jeux doivent être amusants pour tout le monde. Nous avons pensé à faire plus pour faire du jeu un environnement inclusif qui adoptait tout le monde. Nous pensons fondamentalement que ce que nous créons pour nos fans et le mode d’affichage, à l’intérieur et à l’extérieur des murs de Microsoft, est la réflexion de ce que nous faisons. Nous avons conçu le programme pour qu’il reflète les valeurs de base dont nous disposons en tant qu’organisation et que le programme puisse apporter des modifications positives, non seulement dans notre espace de travail, mais aussi dans les produits que nous créons pour les joueurs que nous utilisons. (Billet de[blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) de Phil Spencer)

Nous souhaitons créer un environnement amusant, diversifié et inclusif dans lequel tout le monde peut jouer. «Pour réellement avoir un impact durable, il faut un décalage de culture, qui ne se produira pas au lendemain. Toutefois, notre équipe s’est engagée à être plus performante chaque jour, pour s’adapter à notre processus décisionnel et réfléchir à la diversité incroyable des besoins, des aptitudes et des intérêts entre les joueurs dans le monde entier.» (Billet de[blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) de Phil Spencer)

Nous espérons que vous nous rejoignerez à ce voyage pour faire [des jeux pour tout le monde](https://news.microsoft.com/gamingforeveryone/) . 

##  <a name="why-make-games-accessible"></a>Pourquoi proposer des jeux accessibles ?

### <a name="increased-gamer-base"></a>Élargissement de la communauté des joueurs

À la base, la justification économique de l’accessibilité repose sur une formule d’une grande simplicité :

Nombre d’utilisateurs pouvant jouer à votre jeu x niveau d’excellence du jeu = augmentation des ventes du jeu

Si vous créez un jeu spectaculaire, mais si complexe ou alambiqué que seule une poignée de joueurs peuvent en profiter, vous limiterez votre nombre de ventes. De même, si vous concevez un jeu inutilisable par des personnes présentant des troubles physiques, sensoriels ou cognitifs, vous raterez des opportunités de ventes. En supposant que [19% des personnes du États-Unis présentent une certaine forme de handicap](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [14% des adultes estimés aux États-Unis ont des difficultés à lire](https://nces.ed.gov/naal/estimates/overview.aspx)et [10% des mâles ont une forme de défaillance de la vision des couleurs](https://www.aao.org/eye-health/diseases/color-blindness-risk), cela peut avoir un impact important sur le chiffre d’affaires de votre titre. 

Pour découvrir d’autres justifications commerciales, voir [Proposer des jeux vidéo accessibles](/windows/desktop/DxTechArts/accessibility-best-practices).

### <a name="better-games"></a>Amélioration des jeux

La création d’un jeu plus accessible permet d’offrir un produit plus abouti au final. 

Les jeux qui proposent des sous-titres en constituent un bon exemple. Par le passé, les jeux prenaient rarement en charge le sous-titrage des dialogues. Aujourd’hui, il est d’usage de proposer des jeux incluant des sous-titres et des légendes. Cette évolution n’a pas été inspirée par des joueurs souffrant de handicaps ou d’invalidités. Au lieu de cela, il a été piloté par la localisation, mais il est devenu populaire avec un large éventail de joueurs qui préfèrent simplement jouer avec des sous-titres, car l’expérience de jeu a été améliorée. Les joueurs activent les sous-titres et les légendes lorsqu’ils jouent dans un environnement présentant un bruit de fond important, lorsqu’ils éprouvent des difficultés d’audition des voix accompagnées de divers effets sonores ou bruits ambiants, ou simplement lorsqu’ils doivent maintenir le volume à bas niveau pour ne pas déranger leur entourage. L’affichage de sous-titres et de légendes a non seulement contribué à améliorer l’expérience de jeu des utilisateurs, mais a également étendu cette expérience aux personnes souffrant de handicaps ou d’invalidités.

Pour les mêmes raisons, la reconfiguration des manettes est une autre fonctionnalité progressivement adoptée en standard par l’industrie des jeux électroniques. Elle est généralement proposée en tant qu’avantage pour tous les joueurs. Certains joueurs apprécient la personnalisation de leurs expériences de jeux et d’autres préfèrent simplement les différences par rapport à ce que les concepteurs ont à l’esprit. Ce que la plupart des gens ne se rendent pas, c’est que la possibilité de remapper des boutons sur un appareil d’entrée est en fait également une fonctionnalité d’accessibilité qui a été conçue pour rendre un jeu lisible pour les personnes ayant différents types de handicaps moteurs, qui sont physiquement incapables de faire fonctionner certaines zones du contrôleur.

Au final, la réflexion que vous aurez menée pour améliorer l’accessibilité de votre jeu se traduira généralement par l’obtention d’un jeu plus abouti, car vous offrirez à vos utilisateurs une expérience plus conviviale et personnalisable.

### <a name="social-space-and-quality-of-life"></a>Espace social et qualité de vie

Les jeux vidéo sont l’une des formes les plus élevées de divertissement et les jeux peuvent offrir des heures de joie. Toutefois, pour certaines personnes, le jeu permet non seulement de se divertir, mais également d’échapper à un lit d’hôpital, à une douleur chronique ou à une anxiété sociale invalidante. Les joueurs sont transportés au sein d’un univers qui leur offre la possibilité de devenir les personnages principaux du jeu vidéo. Grâce aux jeux, ils peuvent donner vie et prendre part à un espace social qui leur est destiné et qui leur fait oublier agréablement leur combat quotidien contre le handicap dont ils souffrent tout en leur offrant l’opportunité de communiquer avec des personnes avec lesquelles ils n’auraient peut-être pas la possibilité d’échanger dans d’autres circonstances. 

Les jeux sont également une culture. La possibilité de prendre part à la même chose que l’ensemble de vos amis est quelque chose qui peut être très utile pour la qualité de vie d’un utilisateur.

##  <a name="is-the-game-you-are-making-today-accessible"></a>Le jeu que vous proposez aujourd’hui est-il accessible ?

Si vous envisagez pour la première fois de rendre votre jeu accessible, voici quelques questions que vous devez vous poser :

* Pouvez-vous exécuter le jeu avec une seule main ? 
* La prise en main de votre jeu peut-elle être effectuée par une personne lambda ?
* Pouvez-vous jouer facilement au jeu sur un moniteur ou un téléviseur de petite taille à une certaine distance ?
* Prenez-vous en charge plusieurs types de périphériques d’entrée utilisables tout au long du jeu ?
* Pouvez-vous jouer au jeu avec le son désactivé ?
* Pouvez-vous jouer au jeu avec votre moniteur configuré en noir et blanc ?
* Lorsque vous chargez votre dernier jeu enregistré au bout d’un mois, pouvez-vous facilement déterminer où vous êtes dans le jeu et savoir ce que vous devez faire pour progresser ?

Si vous répondez non à la plupart de ces questions ou que vous n’en connaissez pas les réponses, il est temps de passer à la vitesse supérieure et de garantir l’accessibilité de votre jeu.

## <a name="defining-disability"></a>Définition de la notion de handicap/invalidité

Le terme « handicap/invalidité » désigne une incompatibilité entre les besoins de l’individu et le service, le produit ou l’environnement proposés. ([Vidéo Inclusive](https://www.microsoft.com/design/inclusive/), Microsoft.com.) Cela signifie que tout le monde peut souffrir d’un handicap ou d’une invalidité et qu’il peut s’agir d’une situation à court terme ou circonstancielle. Envisagez toutes les difficultés que les joueurs atteints de tels handicaps risquent de rencontrer en utilisant votre jeu, et réfléchissez à la façon dont vous pouvez améliorer la conception de votre jeu à leur intention. Voici quelques-uns des handicaps ou invalidités que vous devez prendre en compte :

### <a name="vision"></a>Vision

*   Affections médicales à long terme telles que le glaucome, la cataracte, le daltonisme, la myopie et la rétinopathie diabétique
*   Conditions de situation, à court terme, telles qu’une petite taille d’écran ou d’écran, un écran à faible résolution ou un reflet de l’écran en raison de sources lumineuses vives comme le soleil sur un moniteur ou un écran mobile
        
### <a name="hearing"></a>Ouïe

* Affections médicales à long terme telles qu’une surdité totale ou une perte d’audition partielle découlant de maladies ou de facteurs génétiques
* Conditions à court terme, comme un bruit d’arrière-plan excessif, une qualité audio de qualité inférieure ou un volume restreint pour éviter d’en perturber d’autres
        
### <a name="motor"></a>Motricité

* Affections médicales à long terme telles que la maladie de Parkinson, la sclérose latérale amyotrophique (SLA), l’arthrite et la dystrophie musculaire
* Situations circonstancielles à court terme comme une blessure à la main ou le fait de tenir une boisson ou de porter un enfant dans ses bras
  
### <a name="cognitive"></a>Cognitif

* Affections médicales à long terme telles que la dyslexie, l’épilepsie, le trouble déficitaire de l’attention avec hyperactivité (TDAH), la démence et l’amnésie
* Conditions de situation, à court terme, telles que la consommation d’alcool, l’absence de sommeil ou les distractions temporaires comme sirène d’un véhicule d’urgence qui se dirige par la maison

### <a name="speech"></a>Speech

* Affections médicales à long terme telles qu’une lésion des cordes vocales, une dysarthrie ou une apraxie
* Situations circonstancielles à court terme comme des soins dentaires ou le fait de manger et de boire


## <a name="how-to-make-games-more-accessible"></a>Comment proposer des jeux plus accessibles ?

### <a name="design-shift-inclusive-game-design-approach"></a>Évolution des pratiques de conception : adoptez une approche de conception de jeux inclusive

La conception inclusive est axée sur la création de produits et de services plus accessibles à un plus vaste éventail de clients, y compris aux personnes souffrant de handicaps ou d’invalidités.

Pour réussir, les concepteurs de jeux actuels doivent réfléchir à la création de jeux qu’ils apprécient. Les concepteurs de jeux doivent savoir comment leurs décisions de conception ont un impact sur l’accessibilité générale du jeu. la lecture du jeu pour l’ensemble de son public cible, y compris les personnes handicapées.

Les paradigmes de conception de jeux traditionnels doivent donc évoluer afin de prendre en compte le concept de conception de jeux inclusive. La conception de jeux inclusive implique de dépasser le processus élémentaire de développement de jeux uniquement axé sur le divertissement du public cible en créant des personnages supplémentaires ou modifiés afin d’englober un plus large éventail de joueurs. Vous devez être conscient de la conception des barrières dans votre jeu et vous assurer qu’elles n’ajoutent pas d’obstacles inutiles qui ne sont pas amusants de l’expérience prévue.

En identifiant les lacunes, vous pouvez optimiser, effectuer une itération sur le concept de conception d’origine et le rendre plus efficace, ce qui permet à d’autres personnes d’expérimenter votre vision. Lorsque vous prenez le temps de mettre en place un processus de conception de jeux plus inclusif, vous améliorez l’accessibilité de votre jeu au final. Aucun jeu ne peut fonctionner pour tout le monde, la définition du jeu requiert un certain degré de défi, mais en prenant en compte l’accessibilité, vous pouvez vous assurer que personne n’est inutilement exclu.

### <a name="empower-gamers-give-gamers-options"></a>Accroissement des capacités des joueurs : élargissez les options des utilisateurs

Presque toutes les solutions d’accessibilité sont en passe de l’un des deux principes. La première offre à vos joueurs les options pour personnaliser leur expérience de jeux. Si vos jeux connaissent déjà une immense popularité, il est possible qu’une part non négligeable de vos fans refuse toute modification de l’expérience de jeu. Cela ne pose aucun problème. Offrez à vos joueurs la possibilité d’activer ou de désactiver les fonctionnalités d’accessibilité et de configurer ces dernières individuellement. Vous devez permettre aux utilisateurs d’expérimenter le jeu de la manière qui convient le mieux à leurs propres besoins et préférences.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Renforcement : communiquer des informations de plusieurs façons

Le deuxième principe est l’endroit où se trouve le concept de conception universelle, une seule approche qui, non seulement, offre un plus grand nombre de joueurs mais améliore également l’expérience de tous. Par exemple, une image et du texte, un symbole et une couleur. Une carte basée sur une plage de marqueurs de couleur différents est non seulement impossible pour les joueurs aveugles de couleurs d’utiliser, mais elle est également frustrante pour tous les autres personnes qui doivent se souvenir de ce que tout correspond à. L’ajout de symboles offre une meilleure expérience à tous.

### <a name="innovate-be-creative"></a>Innovation : faites preuve de créativité

Vous disposez de nombreux moyens pour améliorer l’accessibilité de votre jeu. Soyez inventif et inspirez-vous des autres jeux accessibles existants. Si vous avez déjà créé un jeu, efforcez-vous d’en identifier les fonctionnalités que vous pourriez améliorer tout en conservant les mécanismes clés du jeu et l’expérience de jeu initiale. Comme indiqué précédemment, l’accessibilité en matière de jeux repose entièrement sur l’offre de possibilités de personnalisation de l’expérience de jeu. Il peut s’agir d’un renforcement ou d’une communication d’informations de plusieurs façons. 

La prise en compte de l’accessibilité vous permet d’aborder la conception à partir d’un nouvel angle et éventuellement des idées que vous n’auriez pas pensé autrement. Cette approche de la conception a abouti non seulement à des concepts intéressants, mais a créé des produits qui ont une adoption étendue ou un succès commercial du marché de masse. Citons notamment le texte prédictif, la reconnaissance vocale, les coupes, le haut-parleur, la machine à écrire et la reconnaissance optique de caractères (OCR). Les idées de ces produits proviennent de ceux qui ont commencé à réfléchir aux solutions pour l’accessibilité.

### <a name="adopt-quality-means-accessible-features"></a>Adopter : la qualité signifie des fonctionnalités accessibles

L’accessibilité est une mesure de la qualité. Il doit s’agir d’une spécification de fonctionnalité et non d’un élément de travail approprié. Par exemple, « adapter minicarte for colourblindness » n’est pas considéré comme un élément de travail de faible priorité auquel vous accédez si vous avez plus de temps. Si cet élément de travail n’est pas terminé, cela signifie simplement que la totalité de la fonctionnalité minicarte est incomplète et ne peut pas être expédiée.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Sensibilisation : mettez l’accent sur l’accessibilité dans votre studio de jeu

Le développement de jeux étant fréquemment soumis à des délais serrés, votre capacité à hisser l’accessibilité au rang de vos priorités contribuera à simplifier ce processus. Un bon moyen de procéder consiste à concevoir vos jeux à partir de zéro en gardant l’accessibilité à l’esprit. Plus tôt vous considérez l’accessibilité, plus l’opération est simple et économique. 

Partagez vos connaissances en matière d’accessibilité avec votre équipe, partagez les justifications de l’entreprise et dissiper les conceptages courants, qu’elle n’offre pas beaucoup de gens, elle dilue votre mécanicien et il est difficile et coûteux à implémenter.

### <a name="review-constantly-evaluate-your-game"></a>Vérification : évaluez continuellement votre jeu

Au cours du développement, vous pouvez introduire un processus de vérification destiné à vous assurer que chaque étape de la conception est axée sur l’accessibilité. Établissez une liste de contrôle semblable à celle ci-dessous pour aider votre équipe à déterminer continuellement si le jeu que vous créez est ou non accessible.

| Liste de contrôle                                         | Fonctionnalités d’accessibilité                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Séquences animées au sein du jeu                                | Présentation de sous-titres et de légendes, test de photosensibilité des animations                                                                           |
| Conception graphique globale (graphismes 2D et 3D)              | Couleurs et options aveugles de couleur, non dépendantes entièrement de la couleur pour l’identification, mais utilisant également des formes et des modèles|
| Écran d’accueil, menu de paramètres et autres menus       | Possibilité de lecture des options à voix haute, possibilité de mémorisation des paramètres, méthode d’entrée de commande alternative, possibilité de réglage de la taille des polices de l’interface utilisateur  |
| Expérience de jeu                                          | Vaste choix de niveaux de difficulté, affichage de sous-titres et de légendes, retour visuel et audio pertinent pour le joueur                           |
| Affichage tête haute                                       | Position de l’écran réglable, taille de police réglable, option conviviale des couleurs                                                  |        
| Entrée de contrôle                                     | Possibilité de reconfigurer des contrôles sur le périphérique d’entrée, prise en charge de manettes personnalisées, autorisation d’entrées simplifiées pour le jeu                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Test du jeu et itération : recueillez les commentaires des joueurs

Lorsque vous organisez des sessions de test du jeu, invitez des personnes souffrant de handicaps ou d’invalidités à y participer afin de vérifier l’accessibilité de votre jeu. N’oubliez pas d’inclure les questions d’accessibilité dans le test bêta questionaires. Les groupes d’infirmités locaux sont une source importante de participants. Observez la façon dont ces personnes utilisent votre jeu et recueillez leurs commentaires. Déterminez les changements à apporter afin d’améliorer votre jeu.

Utilisez des réseaux sociaux et le Forum de votre jeu pour écouter les fonctionnalités d’accessibilité les plus importantes et comment elles doivent être implémentées. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Promotion : signalez l’accessibilité de votre jeu au monde entier

Les utilisateurs ont besoin de savoir si votre jeu est manipulable par des personnes souffrant de handicaps ou d’invalidités. Indiquez clairement l’accessibilité du jeu sur le site Web du jeu, les communiqués de presse et l’empaquetage pour vous assurer que les consommateurs savent à quoi s’attendre quand ils achètent votre jeu. Pensez également à assurer l’accessibilité de votre site web et de tous les circuits de vente de votre jeu. Plus important encore, faites la promotion de votre jeu auprès de la communauté de joueurs concernés par l’accessibilité.

## <a name="game-accessibility-features"></a>Fonctionnalités d’accessibilité des jeux

Cette section décrit certaines des fonctionnalités vous permettant d’améliorer l’accessibilité de votre jeu. Ces fonctionnalités sont dérivées des instructions tirées du site Web sur les [recommandations relatives](http://gameaccessibilityguidelines.com/) à l’accessibilité des jeux. Cette ressource représente les résultats d’un groupe collaboratif de Studios, de spécialistes et d’universitaires.

### <a name="color-blind-friendly-graphics-and-user-interface"></a>Graphismes et interface utilisateur avec aveugles couleurs

La rétine comporte deux types de cellules photoréceptrices : les cônes qui permettent de percevoir la lumière, et les bâtonnets qui facilitent la vision dans des conditions de faible luminosité. 

Il existe trois types de cônes (rouges, verts et bleus) qui nous aident à visualiser correctement les couleurs. Le daltonisme se produit lorsqu’un ou plusieurs de ces trois types de cônes légers ne fonctionnent pas comme prévu. Le degré de cécité des couleurs peut aller de la perception de la couleur presque normale à une sensibilité réduite au rouge, au vert ou à la lumière bleue, à une impossibilité complète de percevoir une couleur du tout. 

Étant donné qu’il est moins courant d’avoir une sensibilité réduite à la lumière bleue, lors de la conception de l’aveugle, la sélection des couleurs est destinée aux personnes qui sont aveugles ou vertes en couleur verte :
 
  + Utilisez des combinaisons de couleurs qui peuvent être différenciées par les personnes avec le daltonisme de couleur rouge/vert :
  
    * couleurs apparaissant comme similaires : toutes les nuances de rouge et de vert, y compris le marron et l’orange ;
    * couleurs qui ressortent : le bleu et le jaune.
    
  + Ne vous fiez pas uniquement à la couleur pour communiquer ou distinguer les objets de jeu. Utilisez également des formes et des modèles.
  + Si vous devez vous appuyer uniquement sur des couleurs, combinez les présélections avec une sélection de couleurs libre, afin qu’elles puissent être entièrement personnalisables par les joueurs qui en ont besoin et ne pas créer de travail supplémentaire pour les joueurs qui n’en ont pas besoin.
  + Utilisez un simulateur de couleur aveugle pour tester vos conceptions afin de pouvoir afficher vos conceptions grâce aux yeux des aveugles. Cela peut vous aider à éviter les problèmes de contraste courants. La [couleur Oracle](https://www.colororacle.org) est un simulateur borgne de couleur gratuit qui peut simuler les trois types les plus courants d’insuffisance de la vision des couleurs : Deuteranopia, protanopia et tritanopia.
  
### <a name="closed-captioning-and-subtitles"></a>Sous-titres et légendes

Lorsque vous concevez les sous-titres et les légendes de votre jeu, vous devez offrir la possibilité d’activer des sous-titres lisibles pour permettre aux utilisateurs de jouer à votre jeu sans le son. Vous devez faire en sorte que certains composants du jeu, tels que les dialogues et les animations et effets sonores, soient affichables sous forme de texte à l’écran.

Voici quelques recommandations de base à prendre en compte lors de la conception des sous-titres et des légendes :

*   Sélectionnez une police lisible simple.
*   Choisissez une police suffisamment grande ou envisagez de proposer une option de taille de police ajustable afin d’améliorer la flexibilité. (La taille de police idéale dépend de la taille de l’écran, de l’éloignement de l’écran, etc.)
*   Créez un contraste élevé entre la couleur d’arrière-plan et la couleur de police. Utilisez le contour et les ombres fortes pour le texte. Utilisez une superposition d’arrière-plan sombre pour les légendes et n’oubliez pas de fournir des options pour qu’elles soient activées ou désactivées. (Pour plus d’informations, voir [Coefficients de contraste](../design/accessibility/accessible-text-requirements.md).)
* Affichez des phrases courtes à l’écran, 38 caractères maximum par ligne et 2-3 lignes maximum à un moment donné. (Veillez à ne pas gâcher le suspense de votre jeu en affichant le texte avant que l’événement correspondant ne se produise.)
*   Indiquez clairement la provenance de l’effet sonore ou le nom du personnage en train de parler. (Par exemple : « Daniel : Bonjour ! »)
*   Offrez la possibilité d’activer et de désactiver les sous-titres et les légendes. (Fonctionnalité supplémentaire : possibilité de sélection de la quantité d’informations sonores affichées en fonction de leur importance.)

### <a name="game-chat-transcription"></a>Transcription de conversation de jeu

Si votre titre autorise les joueurs à communiquer à l’aide de la voix et à envoyer des messages texte les uns aux autres, les fonctionnalités de conversion de texte par synthèse vocale et de synthèse vocale doivent être disponibles en option.

Les personnes qui n’ont pas de microphones attachés à leur appareil de jeu peuvent toujours avoir une conversation vocale avec quelqu’un qui parle. Ils peuvent taper du texte dans la fenêtre de conversation et faire en sorte que ces messages soient convertis en voix. Il permet également à quelqu’un qui ne peut pas entendre très bien de lire les messages texte transcrits de la personne avec laquelle ils ont une conversation vocale.

Pour les développeurs dans le ID@Xbox programme et les partenaires gérés, les fonctionnalités de synthèse vocale et de synthèse vocale sont disponibles dans le cadre des [fonctionnalités d’accessibilité de Game chat 2](/gaming/xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) dans le service Xbox Live. Pour plus d’informations, consultez [vue d’ensemble de Game chat 2](/gaming/xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Retour audio

Le son fournit un retour au joueur en complément du retour visuel. La conception d’une structure audio efficace peut améliorer l’accessibilité de votre jeu pour les joueurs souffrant d’une déficience visuelle. Voici quelques recommandations à prendre en compte :

*   Utilisez des signaux audio 3D pour fournir des informations spatiales complémentaires.
* Séparez les contrôles de volume de la musique, de la voix et des effets sonores.
*   Choisissez des formulations qui fournissent des informations pertinentes pour les joueurs. (Par exemple : « Les ennemis sont en train d’entrer par la porte de derrière » plutôt que « Les ennemis sont en approche ».)
*   Assurez-vous que la vitesse d’élocution est correcte et proposez un contrôle permettant d’ajuster cette vitesse pour améliorer l’accessibilité.

### <a name="fully-mappable-controls"></a>Contrôles entièrement configurables

Certaines entreprises et organisations, telles que [Special Effect](https://www.specialeffect.org.uk/), conçoivent des manettes de jeu personnalisées qui sont utilisables avec différents systèmes de jeu, comme Windows et Xbox One. Cette personnalisation permet à des personnes atteintes de différentes formes de handicap ou d’invalidité de jouer à des jeux qu’ils n’auraient pas pu expérimenter sans cela. Pour découvrir des exemples d’utilisateurs qui sont désormais en mesure de jouer à des jeux de façon autonome grâce aux manettes personnalisées, consultez la page présentant [les personnes ayant bénéficié de l’aide de Special Effect](https://www.specialeffect.org.uk/who-we-helped).

En tant que développeur de jeux, vous pouvez améliorer l’accessibilité de votre jeu en autorisant les contrôles entièrement configurables afin d’offrir aux joueurs la possibilité de brancher leurs manettes personnalisées et de reconfigurer les touches selon leurs besoins.

Le fait de disposer de contrôles entièrement mappables est également bénéfique aux personnes qui utilisent des contrôleurs standard. Vos joueurs peuvent concevoir une disposition qui répond à leurs propres besoins individuels.

Les manettes Xbox One standard et Xbox Elite sont personnalisables pour les jeux de précision. Pour utliize entièrement leurs fonctionnalités de remappage, __il est recommandé que les développeurs incluent le remappage directement dans le jeu__. Pour plus d’informations, voir [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) et [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Large éventail de niveaux de difficulté

Le rôle des jeux vidéo est de divertir les utilisateurs. La difficulté pour les développeurs consiste à ajuster le niveau de difficulté afin de proposer au joueur le défi le mieux adapté. Pour commencer, l’habilité et les capacités des joueurs diffèrent selon les individus. L’élargissement de l’éventail de niveaux de difficulté est donc plus susceptible de satisfaire aux attentes précises des joueurs en matière de défi à relever. Parallèlement, le fait d’offrir un choix de niveaux de difficulté plus étendu contribue également à optimiser l’accessibilité de votre jeu en permettant à un plus grand nombre de personnes handicapées d’utiliser votre jeu. N’oubliez pas que les joueurs sont motivés par la perspective de relever les différents défis d’un jeu et d’en être récompensés. Ils ne veulent pas d’un jeu impossible à gagner.

La mise au point du niveau de difficulté de votre jeu constitue un processus délicat. Si votre jeu est trop simple, les joueurs risquent de s’en lasser. S’il est trop difficile, il est possible que les utilisateurs finissent par baisser les bras et par abandonner votre jeu. Trouver le juste équilibre entre les deux relève à la fois de l’art et de la science. Il existe de nombreuses façons de concevoir un jeu offrant le niveau de difficulté approprié. Certains jeux proposent des entrées simplifiées, comme la possibilité de jouer par une simple pression sur un bouton, une option de rembobinage et de recommencement de la partie pour obtenir une nouvelle chance de gagner, ou une possibilité de diminuer le nombre ou la force des adversaires afin de faciliter la progression dans le jeu après plusieurs tentatives.

### <a name="photosensitivity-epilepsy-testing"></a>Test contre les risques d’épilepsie photosensible

L’épilepsie (PSE) est une condition dans laquelle les crises sont déclenchées par des stimuli visuels, y compris des expositions à des lampes clignotantes ou à certains modèles et modèles visuels mobiles. Ce type de trouble touche près de trois pour cent de la population et survient plus fréquemment chez les enfants et les adolescents. En termes de nombres, nous examinons environ [1 4000 personnes âgées de 5-24](https://www.epilepsy.com/learn/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-0).

De nombreux facteurs peuvent entraîner une réaction photosensible lors de l’utilisation d’un jeu vidéo, comme la durée de la partie, la fréquence des clignotements, l’intensité lumineuse, le contraste de l’arrière-plan et des motifs lumineux, la distance entre l’écran et le joueur, ainsi que la longueur d’onde de la lumière.

De nombreuses personnes découvrent qu’elles ont une cessation d’épilepsie. Les joueurs peuvent et ont leurs premières crises par le biais de videogames, ce qui peut entraîner des blessures physiques. En tant que développeur, voici quelques conseils pour la conception d’un jeu afin de réduire le risque de crises en raison de l’épilepsie photosensible.

Évitez les éléments suivants :
* Avoir des lumières clignotantes d’une fréquence de 5 à 30 clignotements par seconde (Hertz) parce que les lumières clignotantes de cette plage sont susceptibles de déclencher des crises.
* Toute séquence d’images clignotantes qui dure plus de 5 secondes
* Plus de trois clignotements en une seule seconde, couvrant 25% + de l’écran
* Déplacement de modèles répétés ou de texte uniforme, couvrant 25% et plus de l’écran
* Répétitions statiques ou texte uniforme, couvrant 40% et plus de l’écran
* Une haute modification instantanée de la luminosité/du contraste (y compris des coupes rapides) ou vers/à partir de la couleur rouge
* Plus de cinq rayures répétées avec un contraste élevé uniformément espacées : lignes ou colonnes telles que les grilles et checkerboards, qui peuvent être composées d’éléments réguliers plus petits tels que Polkadots
* Plus de cinq lignes de texte mises en majuscules uniquement, avec un peu d’espace entre les lettres, et l’interligne de la même hauteur que les lignes elles-mêmes, ce qui la rend en fait un contraste élevé de manière uniforme

Utilisez un système automatisé pour rechercher dans votre jeu la présence éventuelle de stimuli risquant de provoquer une épilepsie photosensible. (Exemple : [Harding test](https://www.hardingtest.com/index.php?page=test) et [Harding Flash and Pattern Analyzer (FPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/) développé par Cambridge Research System Ltd et professeur Graham Harding.) 

L’option **d’activation/désactivation du clignotement** et la **désactivation** du **clignotement** par défaut. Ce faisant, vous protégez les joueurs qui ne savent pas encore qu’ils sont susceptibles de se lancer.

Introduisez des pauses entre les niveaux de jeu afin d’inciter les joueurs à s’arrêter de temps en temps au lieu de jouer pendant plusieurs heures d’affilée.

## <a name="other-accessibility-resources"></a>Autres ressources en matière d’accessibilité

Vous trouverez ci-après quelques sites externes fournissant des informations supplémentaires sur l’accessibilité des jeux.

### <a name="game-accessibility-guidelines"></a>Recommandations en matière de conception de jeux accessibles
* [Instructions d’accessibilité aux jeux](http://gameaccessibilityguidelines.com/) (utilisées comme référence dans cette rubrique)
* [AbleGamers Foundation Guidelines](https://accessible.games/accessible-player-experiences/) (utilisé comme référence dans cette rubrique)
* [Concevoir des jeux universellement accessibles (en anglais)](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Manettes de jeu personnalisées
* [Special Effect (en anglais)](https://www.specialeffect.org.uk/)
* [Warfighter Engaged (en anglais)](https://www.warfighterengaged.org/)

### <a name="other-references-used"></a>Autres références utilisées
* [Color Blind Awareness, entreprise d’intérêt communautaire (en anglais)](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Comment faire des sous-titres bien &mdash; un article de blog sur Gamasutra de Ian Hamilton](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovation for All Programme (en anglais)](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Épilepsie-Fondation](https://www.epilepsy.com/)

### <a name="related-links"></a>Liens connexes
* [Conception inclusive](https://www.microsoft.com/design/inclusive/)
* [Hub de développeurs axés sur l’accessibilité Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Développement d’applications UWP accessibles](../design/accessibility/accessibility.md)
* [Livre électronique sur la conception de logiciels accessibles](https://www.microsoft.com/download/details.aspx?id=19262)