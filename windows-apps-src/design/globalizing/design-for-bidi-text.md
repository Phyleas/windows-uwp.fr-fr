---
description: Concevez votre application pour fournir une prise en charge du texte bidirectionnel afin de pouvoir combiner le script de systèmes d’écriture de gauche à droite (LTR) et de droite à gauche (RTL), qui contiennent généralement différents types d’alphabets.
title: Concevoir votre application pour le texte bidirectionnel
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, UWP, globalisation, adaptabilité, localisation, RTL, LTR
ms.localizationpriority: medium
ms.openlocfilehash: f09aa1ac2b56c83b502e54ce631e46d2f4054943
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829609"
---
# <a name="design-your-app-for-bidirectional-text"></a>Concevoir votre application pour le texte bidirectionnel

Concevez votre application pour fournir une prise en charge du texte bidirectionnel afin de pouvoir combiner les scripts de droite à gauche (RTL) et les systèmes d’écriture de gauche à droite (LTR), qui contiennent généralement différents types d’alphabets.

Les systèmes d’écriture de droite à gauche, tels que ceux utilisés au Moyen-Orient, au centre et à l’Asie du Sud et en Afrique, ont des exigences de conception uniques. Ces systèmes d’écriture nécessitent une prise en charge du texte bidirectionnel (bidirectionnel). La prise en charge BiDi est la possibilité d’entrer et d’afficher la disposition du texte dans l’ordre de droite à gauche (RTL) ou de gauche à droite (LTR).

Au total, neuf langues BiDi sont incluses avec Windows.
- Deux langues entièrement localisées. Arabe et hébreu.
- Sept packs d’interface linguistique pour les marchés émergents. Persan, ourdou, dari, kurde central, Sindhi, pendjabi (Pakistan) et ouïgour.

Cette rubrique contient la philosophie de conception de Windows BiDi et des études de cas qui présentent des considérations relatives à la conception BiDi.

## <a name="bidi-design-elements"></a>Éléments de conception bidi

Quatre éléments influencent les décisions de conception BiDi dans Windows.

- **Mise en miroir de l’interface utilisateur (IU)**. Le workflow de l’interface utilisateur permet de présenter le contenu de droite à gauche dans sa disposition native. La conception de l’interface utilisateur semble être locale sur des marchés BiDi.
- **Cohérence dans l’expérience utilisateur**. La conception semble naturelle dans l’orientation de droite à gauche. Les éléments d’interface utilisateur partagent un sens de disposition cohérent et s’affichent lorsque l’utilisateur les attend.
- **Optimisation tactile**. Semblable à l’interface utilisateur tactile dans une interface utilisateur non mise en miroir, les éléments sont faciles à atteindre et ils sont naturels à toucher l’interaction.
- **Prise en charge du texte mixte**. La prise en charge de l’orientation du texte permet une bonne présentation du texte mixte (texte en anglais sur les builds BiDi, et vice versa).

## <a name="feature-design-overview"></a>Vue d’ensemble de la conception de fonctionnalités

Windows prend en charge les quatre éléments de conception BiDi. Examinons quelques-unes des principales fonctionnalités importantes de Windows et fournissons un contexte sur la manière dont elles affectent votre application.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Naviguer dans la direction qui semble naturelle

Windows ajuste la direction de la grille typographique de sorte qu’elle se passe de droite à gauche, ce qui signifie que la première vignette sur la grille est placée dans le coin supérieur droit et la dernière vignette en bas à gauche. Cela correspond au modèle RTL de publications imprimées, telles que les livres et les magazines, où le modèle de lecture commence toujours dans l’angle supérieur droit et progresse vers la gauche.

![Menu Démarrer bidi menu Démarrer bidi ](images/56283_BIDI_01_startscreen_resized.png)
 ![ avec les icônes](images/56283_BIDI_02_startscreen_charm_resized.png)

Pour conserver un workflow cohérent de l’interface utilisateur, le contenu sur les mosaïques conserve une disposition de droite à gauche, ce qui signifie que le nom et le logo de l’application sont positionnés dans le coin inférieur droit de la vignette, quelle que soit la langue de l’interface utilisateur de l’application.

#### <a name="bidi-tile"></a>Vignette BiDi

![Vignette BiDi](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Vignette anglais

![Vignette anglais](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Recevoir des notifications de vignette qui se lisent correctement

Les vignettes prennent en charge le texte mixte. La zone de notification offre une flexibilité intégrée pour ajuster l’alignement du texte en fonction du langage de notification.  Lorsqu’une application envoie des notifications en arabe, en Hébreu ou en autres langues BiDi, le texte est aligné à droite. Et lorsqu’une notification anglais (ou une autre LTR) arrive, elle s’aligne sur la gauche.

![Notifications de vignette](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Une expérience utilisateur de droite cohérente et facile à toucher

Chaque élément de l’interface utilisateur de Windows s’ajuste à l’orientation de droite à gauche. Les icônes et les lanceurs ont été positionnés sur le bord gauche de l’écran de sorte qu’ils ne chevauchent pas les résultats de recherche ou ne dégradent pas l’optimisation tactile. Ils peuvent être facilement atteints avec les curseurs.

![Capture d’écran de la bidirectionnel montrant la capture d’écran de recherche redimensionnée dans le menu volant ](images/56286_BIDI_05_search_flyout_resized.png)
 ![ de bidi montrant le menu volant d’impression redimensionné](images/56286_BIDI_06_print_flyout_resized.png)

![Capture d’écran de la barre bidimensionnelle montrant la ](images/56286_BIDI_07_settings_flyout_resized.png)
 ![ capture d’écran des paramètres du menu décrivant le menu bidirectionnel montrant les barres de l’application redimensionnées](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Entrée de texte dans n’importe quelle direction

Windows offre un clavier tactile à l’écran, propre et sans encombrement. Pour les langues BiDi, il existe une clé de contrôle d’orientation du texte qui permet de basculer la direction d’entrée de texte en fonction des besoins.

![Clavier tactile pour la langue BiDi](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Utiliser n’importe quelle application dans n’importe quel langage

Installez et utilisez vos applications favorites dans n’importe quel langage. Les applications apparaissent et fonctionnent comme elles le feraient sur des versions non-BiDi de Windows. Les éléments dans les applications sont toujours placés dans une position cohérente et prévisible.

![Application en anglais avec contenu BiDi](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Afficher les parenthèses correctement

Avec l’introduction de l’algorithme de parenthèses bibidis (BPA), les parenthèses jumelées apparaissent toujours correctement, quelles que soient les propriétés d’alignement du texte ou de la langue.

#### <a name="incorrect-parentheses"></a>Parenthèses incorrectes

![Application BiDi avec parenthèses incorrectes](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Parenthèses correctes

![Application BiDi avec parenthèses correctes](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Typographie

Windows utilise la police de Segoe UI pour toutes les langues BiDi. Cette police est mise en forme et mise à l’échelle pour l’interface utilisateur de Windows.

![Capture d’écran montrant la police Segoe UI dans la capture d’écran de démarrage ](images/56290_BIDI_13_start_screen_segoe.png)
 ![ montrant la police arabe Segoe dans l’écran d’accueil](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Étude de cas #1 : application de musique BiDi

### <a name="overview"></a>Vue d’ensemble

Les applications multimédias font l’essentiel d’un défi de conception intéressant, car les contrôles multimédias sont généralement censés avoir une mise en page de gauche à droite similaire à celle des langues non BiDi.

![Contrôles multimédias de gauche à droite](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Contrôles multimédias de droite à gauche](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Définition de la direction de l’interface utilisateur

La conservation du workflow de l’interface utilisateur de droite à gauche est importante pour une conception cohérente des marchés BiDi. Il est difficile d’ajouter des éléments qui ont un flux de gauche à droite dans ce contexte, car certains éléments de navigation tels que le bouton précédent peuvent contredire l’orientation directionnelle du bouton précédent dans les contrôles audio.

![Page de suivi de l’application musicale](images/56292_BIDI_16_app_layout_callouts_resized.png)

Cette application musicale conserve une grille orientée de droite à gauche. Cela donne à l’application une apparence très naturelle pour les utilisateurs qui parcourent déjà cette direction à travers l’interface utilisateur de Windows. Le Flow est conservé en garantissant que les éléments principaux ne sont pas simplement triés de droite à gauche, mais également correctement alignés dans les en-têtes de section pour aider à maintenir le workflow de l’interface utilisateur.

![Page de l’album de l’application musicale](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Gestion du texte

La bio-artiste de la capture d’écran ci-dessus est alignée à gauche, tandis que les autres éléments textuels liés à l’artiste tels que les noms d’albums et de pistes conservent l’alignement à droite. Le champ bio est un élément de texte assez grand, qui se lit mal lorsqu’il est aligné sur la droite, simplement parce qu’il est difficile d’effectuer le suivi entre les lignes tout en lisant un bloc de texte plus large. En général, tout élément de texte comportant plus de deux ou trois lignes contenant cinq mots ou plus doit être pris en compte pour les exceptions d’alignement similaires, où l’alignement du bloc de texte est opposé à celui de la disposition générale de l’application.

La manipulation de l’alignement au sein de l’application peut sembler simple, mais elle expose souvent certaines des limites et limitations des moteurs de rendu en termes de positionnement des caractères neutres entre les chaînes BiDi. Par exemple, la chaîne suivante peut être affichée différemment en fonction de l’alignement.

| | Chaîne anglaise (LTR) | Chaîne hébraïque (RTL) |
| -------------- | ------------------- | ------------------- |
| **Alignement à gauche** | Hello, World! | בוקר טוב! |
| **Alignement à droite** | ! Hello World | !בוקר טוב |

Pour vous assurer que les informations de l’artiste sont correctement affichées dans l’application musicale, l’équipe de développement a séparé les propriétés de disposition du texte de l’alignement. En d’autres termes, les informations sur l’artiste peuvent être affichées comme alignées à droite dans la plupart des cas, mais la modification de la disposition des chaînes est définie en fonction du traitement personnalisé en arrière-plan. Le traitement en arrière-plan détermine le meilleur paramètre de disposition directionnelle en fonction du contenu de la chaîne.

![Page de l’artiste de l’application musicale](images/56292_BIDI_18_app_layout_callouts_resized.png)

Par exemple, sans traitement de la disposition des chaînes personnalisées, le nom de l’artiste « The contoso Band ». s’affiche sous la forme «. La bande de contoso».

### <a name="specialized-string-direction-preprocessing"></a>Prétraitement spécialisé de la direction des chaînes

Lorsque l’application contacte le serveur pour les métadonnées de média, elle prétraite chaque chaîne avant de l’afficher à l’utilisateur. Au cours de ce prétraitement, l’application effectue également une transformation pour assurer la cohérence de la direction du texte. Pour ce faire, il vérifie s’il existe des caractères neutres aux extrémités de la chaîne. En outre, si la direction du texte de la chaîne est opposée à la direction de l’application définie par les paramètres de langue Windows, elle ajoute (et parfois insère) des marqueurs de direction Unicode. La fonction de transformation ressemble à ceci.

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

Les caractères Unicode ajoutés ont une largeur nulle, donc ils n’ont pas d’impact sur l’espacement des chaînes. Ce code entraîne une baisse potentielle des performances, car la détection de la direction d’une chaîne requiert l’exécution de la chaîne jusqu’à ce qu’un caractère non neutre soit rencontré. Chaque caractère dont la neutralité est vérifiée est d’abord comparé à plusieurs plages Unicode. il ne s’agit donc pas d’un contrôle trivial.

## <a name="case-study-2-a-bidi-mail-app"></a>Étude de cas #2 : application de courrier bidirectionnel

### <a name="overview"></a>Vue d’ensemble

En termes de spécifications de disposition d’interface utilisateur, un client de messagerie est relativement simple à concevoir. L’application de messagerie dans Windows est mise en miroir par défaut. Du point de vue de la gestion de texte, l’application de messagerie doit disposer d’un affichage de texte et de fonctionnalités de composition plus robustes pour prendre en charge les scénarios de texte mixte.

### <a name="establishing-ui-directionality"></a>Définition de la direction de l’interface utilisateur

La disposition de l’interface utilisateur de l’application de messagerie est mise en miroir. Les trois volets ont été reorienté afin que le volet des dossiers soit positionné sur le bord droit de l’écran, puis sur le volet de la liste des éléments de messagerie à gauche, puis dans le volet de composition du courrier électronique.

![Application de messagerie en miroir](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Des éléments supplémentaires ont été reciblés pour correspondre à l’ensemble du workflow de l’interface utilisateur et à l’optimisation tactile. Celles-ci incluent la barre d’application et les icônes compose, répondre et supprimer.

![Application de messagerie mise en miroir avec la barre d’application](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Gestion du texte

#### <a name="ui"></a>UI

L’alignement du texte dans l’interface utilisateur est généralement aligné à droite. Cela comprend le volet dossier et le volet éléments. Le volet élément est limité à deux lignes de texte (adresse et titre). Cela est important pour conserver l’alignement de droite à gauche, sans introduire de bloc de texte difficile à lire lorsque la direction du contenu est opposée au déroulement de l’interface utilisateur.

#### <a name="text-editing"></a>Modification de texte

La modification de texte nécessite la possibilité de composer le formulaire de droite à gauche et de gauche à droite. En outre, la disposition de la composition doit être conservée à l’aide d’un format &mdash; tel que le texte enrichi &mdash; qui permet d’enregistrer les informations d’orientation.

![Application de messagerie de gauche à droite](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Application de messagerie de droite à gauche](images/56294_BIDI_22_email_orientation_RtL_resized.png)
