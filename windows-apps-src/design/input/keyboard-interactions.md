---
Description: Apprenez à concevoir et optimiser vos applications Windows afin qu’elles fournissent la meilleure expérience possible pour les utilisateurs du clavier et pour ceux qui ont des handicaps et d’autres exigences en matière d’accessibilité.
title: Interactions avec le clavier
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: clavier, accessibilité, navigation, Focus, texte, entrée, interactions utilisateur, boîtier de commande, à distance
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 1d883243b60b2b2693fbf0f21315008e556b5743
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970754"
---
# <a name="keyboard-interactions"></a>Interactions avec le clavier

![image Hero du clavier](images/keyboard/keyboard-hero.jpg)

Apprenez à concevoir et optimiser vos applications Windows afin qu’elles fournissent la meilleure expérience possible pour les utilisateurs du clavier et pour ceux qui ont des handicaps et d’autres exigences en matière d’accessibilité.

Sur les appareils, l’entrée au clavier est un élément important de l’expérience d’interaction des applications Windows globale. Une expérience de clavier bien conçue permet aux utilisateurs de naviguer efficacement dans l’interface utilisateur de votre application et d’accéder à ses fonctionnalités complètes sans jamais soulever les mains du clavier.

![image de clavier et de manette](images/keyboard/keyboard-gamepad.jpg)

***Les modèles d’interaction courants sont partagés entre le clavier et le boîtier***

Dans cette rubrique, nous nous concentrons spécifiquement sur la conception d’applications Windows pour les entrées au clavier sur les PC. Toutefois, une expérience de clavier bien conçue est importante pour la prise en charge d’outils d’accessibilité tels que le narrateur Windows, à l’aide de [claviers logiciels](#software-keyboard) tels que le clavier tactile et le clavier visuel (OSK), ainsi que pour la gestion d’autres types de périphériques d’entrée, tels que le boîtier de commande Xbox et le contrôle à distance.

La plupart des instructions et recommandations présentées ici, y compris les éléments [visuels de focus](#focus-visuals), les [clés d’accès](#access-keys)et la [navigation dans l’interface utilisateur](#navigation), s’appliquent également à ces autres scénarios.

**Remarque**  Alors que les claviers matériels et logiciels sont utilisés pour l’entrée de texte, l’objectif de cette rubrique est la navigation et l’interaction.

## <a name="built-in-support"></a>Prise en charge intégrée

Avec la souris, le clavier est le périphérique le plus largement utilisé sur les PC et, par conséquent, est une partie fondamentale de l’expérience du PC. Les utilisateurs de PC attendent une expérience complète et cohérente du système et des applications individuelles en réponse à l’entrée au clavier.

Tous les contrôles UWP incluent une prise en charge intégrée de riches expériences clavier et des interactions avec les utilisateurs, tandis que la plateforme elle-même fournit une base complète pour la création d’expériences clavier qui vous semblent mieux adaptées à vos applications et contrôles personnalisés.

![clavier avec une image de téléphone](images/keyboard/keyboard-phone.jpg)

***UWP prend en charge le clavier avec n’importe quel appareil***

## <a name="basic-experiences"></a>Expériences de base
![Appareils basés sur le focus](images/keyboard/focus-based-devices.jpg)

Comme mentionné précédemment, les périphériques d’entrée tels que le boîtier de commande Xbox et le contrôle à distance, ainsi que les outils d’accessibilité tels que le narrateur, partagent une grande partie de l’expérience d’entrée au clavier pour la navigation et les commandes. Cette expérience commune sur les types d’entrée et les outils réduit le travail supplémentaire de votre part et contribue à l’objectif « générer une fois, exécuter en tout lieu » du plateforme Windows universelle.

Le cas échéant, nous allons identifier les principales différences que vous devez connaître et décrire les mesures d’atténuation à prendre en compte.

Voici les appareils et les outils présentés dans cette rubrique :

| Appareil/outil                       | Description     |
|-----------------------------------|-----------------|
|Clavier (matériel et logiciels)   |En plus du clavier matériel standard, les applications Windows prennent en charge deux claviers logiciels : le [clavier tactile (ou logiciel)](#software-keyboard) et le [clavier visuel](#on-screen-keyboard).|
|Boîtier de commande et télécommande         |Le boîtier de commande Xbox et le contrôle à distance sont des périphériques d’entrée fondamentaux dans [une expérience de 10 mètres](../devices/designing-for-tv.md). Pour obtenir des informations spécifiques sur la prise en charge de Windows pour le boîtier et le contrôle à distance, consultez [le boîtier et les interactions de contrôle à distance](gamepad-and-remote-interactions.md).|
|Lecteurs d’écran (narrateur)          |Narrator est un lecteur d’écran intégré pour Windows qui fournit des expériences et des fonctionnalités d’interaction uniques, tout en s’appuyant sur une navigation et une entrée de clavier de base. Pour plus d’informations sur le narrateur, consultez [prise en](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)main du narrateur.|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Des expériences personnalisées et un clavier efficace
Comme nous l’avons vu précédemment, la prise en charge du clavier est essentielle pour garantir que vos applications fonctionnent bien pour les utilisateurs ayant des compétences, des aptitudes et des attentes différentes. Nous vous recommandons de classer par ordre de priorité les éléments suivants.
- Prendre en charge la navigation et l’interaction avec le clavier
    - Vérifiez que les éléments actionnables sont identifiés comme des taquets de tabulation (et non pas des éléments qui ne sont pas actionnables) et que l’ordre de navigation est logique et prévisible (voir [taquets de tabulation](#tab-stops))
    - Définir le focus initial sur l’élément le plus logique (voir le [focus initial](#initial-focus))
    - Fournir une navigation par touche de direction pour les « navigations internes » (voir [navigation](#navigation))
- Raccourcis clavier de prise en charge
    - Fournir des touches accélérateur pour les actions rapides (voir [accélérateurs](#accelerators))
    - Fournir des touches d’accès pour naviguer dans l’interface utilisateur de votre application (voir [clés d’accès](access-keys.md))

### <a name="focus-visuals"></a>Éléments visuels de focus

La UWP prend en charge une conception visuelle de focalisation unique qui fonctionne bien pour tous les types d’entrée et les expériences.
![Visuel du focus](images/keyboard/focus-visual.png)

Un visuel de Focus :

- S’affiche quand un élément d’interface utilisateur reçoit le focus d’un clavier et/ou d’un contrôle à distance
- Est rendu sous la forme d’une bordure en surbrillance autour de l’élément d’interface utilisateur pour indiquer qu’une action peut être effectuée
- Aide un utilisateur à naviguer dans l’interface utilisateur d’une application sans se perdre
- Peut être personnalisé pour votre application (voir [visuels de focus de visibilité élevée](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**Remarque** Le visuel de focus UWP n’est pas le même que le rectangle de focus du narrateur.

### <a name="tab-stops"></a>Taquets de tabulation

Pour utiliser un contrôle (y compris des éléments de navigation) avec le clavier, il faut que le focus de celui-ci soit positionné sur le contrôle. Pour qu’un contrôle puisse recevoir le focus clavier, vous pouvez le rendre accessible via la navigation par onglets en l’identifiant sous la forme d’un taquet de tabulation dans l’ordre de tabulation de votre application.

Pour qu’un contrôle soit inclus dans l’ordre de tabulation, la propriété [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) doit avoir la valeur **true** et la propriété [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) doit avoir la valeur **true**.

Pour exclure spécifiquement un contrôle de l’ordre de tabulation, affectez la valeur **false**à la propriété [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) .

Par défaut, l’ordre de tabulation reflète l’ordre dans lequel les éléments d’interface utilisateur sont créés. Par exemple, si un `StackPanel` contient un `Button`, un `Checkbox`et un `TextBox`, l’ordre de tabulation `Button`est `Checkbox`, et `TextBox`.

Vous pouvez remplacer l’ordre de tabulation par défaut en définissant la propriété [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) .

#### <a name="tab-order-should-be-logical-and-predictable"></a>L’ordre de tabulation doit être logique et prévisible

Un modèle de navigation de clavier bien conçu, utilisant un ordre de tabulation logique et prévisible, rend votre application plus intuitive et permet aux utilisateurs d’explorer, de découvrir et d’accéder plus efficacement aux fonctionnalités.

Tous les contrôles interactifs doivent avoir des taquets de tabulation (sauf s’ils appartiennent à un [groupe](#control-group)), alors que les contrôles non interactifs, tels que les étiquettes, ne le doivent pas.

Évitez un ordre de tabulation personnalisé qui permet de se concentrer sur votre application. Par exemple, une liste de contrôles dans un formulaire doit avoir un ordre de tabulation qui s’enchaîne de haut en bas et de gauche à droite (en fonction des paramètres régionaux).

Pour plus d’informations sur la personnalisation des taquets de tabulation, consultez [accessibilité du clavier](../accessibility/keyboard-accessibility.md) .

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Essayer de coordonner l’ordre des onglets et l’ordre visuel

La coordination de l’ordre de tabulation et de l’ordre visuel (également appelée ordre de lecture ou ordre d’affichage) permet de réduire la confusion pour les utilisateurs lorsqu’ils parcourent l’interface utilisateur de votre application.

Essayez de classer et de présenter les commandes, les contrôles et le contenu les plus importants en premier dans l’ordre de tabulation et l’ordre visuel. Toutefois, la position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés des éléments enfants qui influencent la disposition. Plus précisément, les dispositions qui utilisent une métaphore de grille ou une métaphore de table peuvent avoir un ordre visuel très différent de l’ordre de tabulation.

**Remarque** L’ordre visuel dépend également des paramètres régionaux et de la langue.

### <a name="initial-focus"></a>Focus initial

Focus initial spécifie l’élément d’interface utilisateur qui reçoit le focus lorsqu’une application ou une page est lancée ou activée pour la première fois. Lors de l’utilisation d’un clavier, il provient de cet élément qu’un utilisateur commence à interagir avec l’interface utilisateur de votre application.

Pour les applications UWP, le focus initial est défini sur l’élément avec la valeur [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) la plus élevée qui peut recevoir le focus. Les éléments enfants des contrôles conteneurs sont ignorés. En liaison, le premier élément de l’arborescence d’éléments visuels reçoit le focus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Définir le focus initial sur l’élément le plus logique

Définissez le focus initial sur l’élément d’interface utilisateur pour la première action, ou primaire, que les utilisateurs sont les plus susceptibles de prendre lors du lancement de votre application ou de la navigation vers une page. Voici quelques exemples :
-   Une application photo dans laquelle le focus est défini sur le premier élément d’une galerie
-   Une application musicale dans laquelle le focus est défini sur le bouton de lecture

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Ne définissez pas le focus initial sur un élément qui expose un résultat potentiellement négatif, voire désastreux

Ce niveau de fonctionnalité doit être le choix d’un utilisateur. Le fait de définir le focus initial sur un élément avec un résultat significatif peut entraîner une perte de données involontaire ou un accès système. Par exemple, ne définissez pas le focus sur le bouton supprimer lors de la navigation vers un message électronique.

Pour plus d’informations sur la substitution de l’ordre de tabulation, consultez [focus navigation](focus-navigation.md) .

### <a name="navigation"></a>Navigation

La navigation au clavier est généralement prise en charge via les touches Tab et les touches de direction.

![touches de tabulation et de direction](images/keyboard/tab-and-arrow.png)

Par défaut, les contrôles UWP suivent les comportements de clavier de base suivants :
-   Les **touches de tabulation** permettent de naviguer entre les contrôles actionnables/actifs dans l’ordre de tabulation.
-   **MAJ + TAB** permet de parcourir les contrôles dans l’ordre inverse de tabulation. Si l’utilisateur a navigué à l’intérieur du contrôle à l’aide de la touche de direction, le focus est défini sur la dernière valeur connue à l’intérieur du contrôle.
-   Les **touches de direction** exposent la « navigation interne » spécifique au contrôle quand l’utilisateur entre « navigation interne », les touches de direction ne quittent pas un contrôle. Voici quelques exemples :
    -   Touche flèche haut/bas déplace le focus `ListView` à l’intérieur et`MenuFlyout`
    -   Modifiez les valeurs actuellement sélectionnées `Slider` pour et.`RatingsControl`
    -   Déplacer le signe insertion à l’intérieur`TextBox`
    -   Développer/réduire des éléments à l’intérieur`TreeView`

Utilisez ces comportements par défaut pour optimiser la navigation au clavier de votre application.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Utiliser la « navigation interne » avec des ensembles de contrôles connexes

La navigation dans les touches fléchées dans un ensemble de contrôles connexes renforce leur relation au sein de l’organisation globale de l’interface utilisateur de votre application.

Par exemple, le `ContentDialog` contrôle présenté ici fournit par défaut une navigation interne pour une ligne horizontale de boutons (pour les contrôles personnalisés, consultez la section [groupe de contrôles](#control-group) ).

![exemple de dialogue](images/keyboard/dialog.png)

***L’interaction avec une collection de boutons associés est facilitée grâce à la navigation par touche de direction***

Si les éléments sont affichés dans une seule colonne, la touche de direction haut/bas parcourt les éléments. Si les éléments sont affichés sur une seule ligne, la touche de direction droite/gauche parcourt les éléments. Si les éléments sont de plusieurs colonnes, les quatre touches de direction se défilent.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Définir un seul taquet de tabulation pour une collection de contrôles connexes

En définissant un seul taquet de tabulation pour une collection de contrôles associés ou complémentaires, vous pouvez réduire le nombre de taquets de tabulation globaux dans votre application.

Par exemple, les images suivantes montrent deux `ListView` contrôles empilés. L’image de gauche montre la navigation par touche de direction utilisée avec un taquet de tabulation `ListView` pour naviguer entre les contrôles, tandis que l’image à droite montre comment la navigation entre les éléments enfants peut être facilitée et plus efficace en éliminant la nécessité de parcourir les contrôles parents avec une touche Tab.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***L’interaction avec deux contrôles ListView empilés peut être facilitée et plus efficace en éliminant le taquet de tabulation et en naviguant avec les touches de direction.***

Consultez la section [groupe de contrôle](#control-group) pour savoir comment appliquer les exemples d’optimisation à l’interface utilisateur de votre application.

### <a name="interaction-and-commanding"></a>Interaction et commande

Une fois qu’un contrôle a le focus, un utilisateur peut interagir avec lui et appeler toutes les fonctionnalités associées à l’aide d’une entrée au clavier spécifique.

#### <a name="text-entry"></a>Entrée de texte

Pour les contrôles spécifiquement conçus pour l’entrée de texte `TextBox` , `RichEditBox`tels que et, toute entrée au clavier est utilisée pour l’entrée ou la navigation dans du texte, qui est prioritaire par rapport à d’autres commandes du clavier. Par exemple, le menu déroulant d’un `AutoSuggestBox` contrôle ne reconnaît pas la clé d' **espace** comme une commande de sélection.

![entrée de texte](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Espace clé

Lorsqu’il n’est pas en mode de saisie de texte, la touche **espace** appelle l’action ou la commande associée au contrôle ayant le focus (tout comme un TAP avec Touch ou un clic avec une souris).

![espace clé](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Entrée (touche)

La touche **entrée** peut effectuer une série d’interactions utilisateur courantes, en fonction du contrôle ayant le focus :
-   Active des contrôles de `Button` commande tels que ou `Hyperlink`. Pour éviter toute confusion entre l’utilisateur final, la touche **entrée** active également les contrôles qui ressemblent `ToggleButton` à `AppBarToggleButton`des contrôles tels que ou.
-   Affiche l’interface utilisateur du sélecteur pour les `ComboBox` contrôles `DatePicker`tels que et. La touche **entrée** valide également et ferme l’interface utilisateur du sélecteur.
-   Active les contrôles de liste tels `ListView`que `GridView`, et `ComboBox`.
    -   La touche **entrée** effectue l’action de sélection en tant que clé d' **espace** pour les éléments de liste et de grille, sauf s’il existe une action supplémentaire associée à ces éléments (ouverture d’une nouvelle fenêtre).
    -   Si une action supplémentaire est associée au contrôle, la touche **entrée** effectue l’action supplémentaire et la touche **espace** effectue l’action de sélection.

**Remarque** La touche **entrée** et la touche **espace** n’effectuent pas toujours la même action, mais elles sont souvent les mêmes.

![entrer une clé](images/keyboard/enter-key.png)

La touche Échap permet à un utilisateur d’annuler l’interface utilisateur temporaire (ainsi que toutes les actions en cours dans cette interface).

Voici quelques exemples de cette expérience :
-   L’utilisateur ouvre `ComboBox` une avec une valeur sélectionnée et utilise les touches de direction pour déplacer la sélection de focus vers une nouvelle valeur. Appuyer sur la touche Échap ferme `ComboBox` et rétablit la valeur sélectionnée à sa valeur d’origine.
-   L’utilisateur appelle une action de suppression permanente pour un e-mail et est invité `ContentDialog` à confirmer l’action. L’utilisateur décide qu’il ne s’agit pas de l’action prévue et appuie sur la touche **Échap** pour fermer la boîte de dialogue. Comme la touche **Échap** est associée au bouton **Annuler** , la boîte de dialogue est fermée et l’action est annulée. La touche **Échap** n’affecte que l’interface utilisateur temporaire, elle ne se ferme pas ou n’effectue pas de navigation dans l’interface utilisateur de l’application.

![Touche Échap](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Touches Origine et Fin

Les touches **origine** et **fin** permettent à un utilisateur de faire défiler le début ou la fin d’une région de l’interface utilisateur.

Voici quelques exemples de cette expérience :
-   Pour `ListView` les `GridView` contrôles et, la touche **début** déplace le focus sur le premier élément et le fait défiler dans la vue, tandis que la touche **fin** déplace le focus sur le dernier élément et le fait défiler dans la vue.
-   Pour un `ScrollView` contrôle, la touche **début** défile vers le haut de la région, tandis que la touche **fin** défile vers le bas de la région (le focus n’est pas modifié).

![clés de début et de fin](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Touches PG. suiv et PG. suiv.

Les touches de **page** permettent à un utilisateur de faire défiler une zone d’interface utilisateur en incréments discrets.

Par exemple, pour `ListView` les `GridView` contrôles et, la touche **PG. haut** fait défiler la région d’une « page » (généralement la hauteur de la fenêtre d’affichage) et déplace le focus vers le haut de la zone. Sinon, la touche **PG. suiv** fait défiler la zone vers le bas d’une page et déplace le focus vers le bas de la région.

![touche PG. suiv et PG. suiv.](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Raccourcis clavier

Les raccourcis clavier peuvent faciliter l’utilisation de votre application en offrant une prise en charge améliorée de l’accessibilité et une meilleure efficacité pour les utilisateurs du clavier.

Outre la prise en charge de la navigation et de l’activation du clavier dans votre application, il est également conseillé de fournir des raccourcis pour les fonctionnalités de votre application. La navigation par onglets constitue un bon niveau de prise en charge du clavier, mais avec une interface utilisateur plus complexe, vous souhaiterez peut-être également ajouter la prise en charge des touches de raccourci. 

Un raccourci est une combinaison de touches qui améliore la productivité en fournissant à l’utilisateur un moyen efficace d’accéder aux fonctionnalités de l’application. Il existe deux types de raccourcis :
-   Les [accélérateurs](#accelerators) sont des raccourcis qui appellent une commande d’application. Votre application peut ou non fournir une interface utilisateur spécifique qui correspond à la commande. Les accélérateurs se composent généralement de la touche Ctrl plus une touche lettre.
-   Les [clés d’accès](#access-keys) sont des raccourcis qui définissent le focus sur une interface utilisateur spécifique dans votre application. Les clés d’accès habituelles se composent de la touche Alt et d’une touche lettre.

Fournir des raccourcis clavier cohérents qui prennent en charge des tâches similaires entre les applications les rend plus utiles et puissantes et aide les utilisateurs à les mémoriser.

#### <a name="accelerators"></a>Accélérateurs

Les accélérateurs aident les utilisateurs à effectuer des actions courantes dans une application bien plus rapidement et plus efficacement. 

Exemples d’accélérateurs :
-   Appuyez sur CTRL + N n’importe où dans l’application de **messagerie** pour lancer un nouvel élément de messagerie.
-   L’appui sur la touche Ctrl + E n’importe où dans Microsoft Edge (et de nombreuses applications Microsoft Store) lance la recherche.

Les accélérateurs présentent les caractéristiques suivantes :
-   Ils utilisent principalement les séquences de touches Ctrl et fonction (les touches de raccourci système Windows utilisent également les touches Alt + non alphanumériques et la touche du logo Windows).
-   Elles sont attribuées uniquement aux commandes les plus couramment utilisées.
-   Elles sont destinées à être mémorisées et sont indiquées uniquement dans les menus, les info-bulles et l’aide.
-   Elles sont effectives dans toute l’application, lorsqu’elles sont prises en charge.
-   Elles doivent être affectées de manière cohérente, car elles sont mémorisées et ne sont pas directement documentées.

#### <a name="access-keys"></a>Clés d'accès

Pour obtenir des informations plus détaillées sur la prise en charge des clés d’accès avec UWP, consultez la page [clés d’accès](access-keys.md) .

Les clés d’accès aident les utilisateurs ayant des handicaps de fonction moteur à appuyer sur une touche à la fois pour agir sur un élément spécifique de l’interface utilisateur. En outre, les clés d’accès peuvent être utilisées pour communiquer des touches de raccourci supplémentaires pour aider les utilisateurs expérimentés à effectuer des actions rapidement.

Les touches d’accès rapide présentent les caractéristiques suivantes :
-   Elles utilisent la touche Alt associée à une touche alphanumérique.
-   Elles visent principalement l’accessibilité.
-   Elles sont documentées directement dans l’interface utilisateur, en regard du contrôle, à l’aide des [touches accélératrices](access-keys.md).
-   Leur incidence se limite à la fenêtre active et elles permettent d’accéder à l’option de menu ou au contrôle correspondant.
-   Les clés d’accès doivent être affectées de manière cohérente aux commandes couramment utilisées (notamment les boutons de validation), dans la mesure du possible.
-   Elles sont localisées.

#### <a name="common-keyboard-shortcuts"></a>Raccourcis clavier courants

Le tableau suivant est un petit exemple de raccourcis clavier fréquemment utilisés. 

| Action                               | Combinaison de touches                                      |
|--------------------------------------|--------------------------------------------------|
| Sélectionner tout                           | Ctrl+A                                           |
| Sélection continue                  | Maj+touche de direction                                  |
| Enregistrer                                 | Ctrl+S                                           |
| Rechercher                                 | Ctrl+F                                           |
| Print                                | Ctrl+P                                           |
| Copier                                 | Ctrl+C                                           |
| Couper                                  | Ctrl+X                                           |
| Coller                                | Ctrl+V                                           |
| Annuler                                 | Ctrl+Z                                           |
| Onglet suivant                             | Ctrl+Tab                                         |
| Fermer l’onglet                            | CTRL + F4 ou CTRL + W                                |
| Zoom sémantique                        | Ctrl++ ou Ctrl+-                                 |

Pour obtenir la liste complète des raccourcis système Windows, consultez [raccourcis clavier pour Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). Pour les raccourcis d’application courants, consultez [raccourcis clavier pour les applications Microsoft](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Expériences avancées

Dans cette section, nous aborderons certaines des expériences d’interaction clavier plus complexes prises en charge par les applications UWP, ainsi que certains des comportements que vous devez connaître quand votre application est utilisée sur différents appareils et avec des outils différents.

### <a name="control-group"></a>Groupe de contrôles

Vous pouvez regrouper un ensemble de contrôles associés ou complémentaires dans un « groupe de contrôles » (ou une zone directionnelle), ce qui permet la « navigation interne » à l’aide des touches de direction. Le groupe de contrôles peut être un taquet de tabulation unique, ou vous pouvez spécifier plusieurs taquets de tabulation dans le groupe de contrôles.

#### <a name="arrow-key-navigation"></a>Navigation dans les touches de direction

Les utilisateurs s’attendent à prendre en charge la navigation dans les touches de direction lorsqu’il existe un groupe de contrôles similaires, associés dans une région d’interface utilisateur :
-   `AppBarButtons`dans un`CommandBar`
-   `ListItems`ou `GridItems` à `ListView` l’intérieur ou`GridView`
-   `Buttons`dans`ContentDialog`

Les contrôles UWP prennent en charge la navigation par touche de direction par défaut. Pour les dispositions personnalisées et les groupes de `XYFocusKeyboardNavigation="Enabled"` contrôles, utilisez pour fournir un comportement similaire.

Envisagez d’ajouter la prise en charge de la navigation par touche de direction lorsque vous utilisez les contrôles suivants :

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Boutons de boîte de dialogue</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems et GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Taquets de tabulation

Selon les fonctionnalités et la disposition de votre application, la meilleure option de navigation pour un groupe de contrôles peut être un taquet de tabulation unique avec navigation vers les éléments enfants, plusieurs taquets de tabulation ou une combinaison.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Utiliser plusieurs taquets de tabulation et touches de direction pour les boutons

Les utilisateurs d’accessibilité s’appuient sur des règles de navigation de clavier bien établies, qui n’utilisent généralement pas de touches de direction pour naviguer dans une collection de boutons. Toutefois, les utilisateurs dépourvus de troubles de la vision peuvent avoir le sentiment que le comportement est naturel.

Voici un exemple de comportement UWP par défaut `ContentDialog`. Si vous pouvez utiliser les touches de direction pour naviguer entre les boutons, chaque bouton est également un taquet de tabulation.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Affecter un taquet de tabulation unique à des modèles d’interface utilisateur familiers

Dans les cas où votre disposition suit un modèle d’interface utilisateur connu pour les groupes de contrôle, l’affectation d’un taquet de tabulation unique au groupe peut améliorer l’efficacité de la navigation pour les utilisateurs.

Voici quelques exemples :
-   `RadioButtons`
-   Plusieurs `ListViews` qui ressemblent et se comportent comme un seul`ListView`
-   Toute interface utilisateur faite pour regarder et se comporter comme une grille de vignettes (comme les vignettes du menu Démarrer)

#### <a name="specifying-control-group-behavior"></a>Spécification du comportement du groupe de contrôles

Utilisez les API suivantes pour prendre en charge le comportement des groupes de contrôles personnalisés (tous sont décrits plus en détail plus loin dans cette rubrique) :

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) active la navigation par touche de direction entre les contrôles
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) indique s’il y a plusieurs taquets de tabulation ou taquets de tabulation unique
-   [FindFirstFocusableElement et FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) définit le focus sur le premier élément avec la clé de **démarrage** et le dernier élément avec la clé de **fin**

L’illustration suivante montre un comportement de navigation de clavier intuitif pour un groupe de contrôles de cases d’option associées. Dans ce cas, nous vous recommandons d’utiliser un seul taquet de tabulation pour le groupe de contrôles, la navigation interne entre les cases d’option à l’aide des touches de direction, de la clé de **démarrage** liée à la première case d’option et de la clé de **fin** liée à la dernière case d’option.

![ensemble](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Clavier et narrateur

Narrator est un outil d’accessibilité de l’interface utilisateur conçu pour les utilisateurs du clavier (d’autres types d’entrée sont également pris en charge). Toutefois, les fonctionnalités du narrateur vont au-delà des interactions de clavier prises en charge par les applications UWP et une attention supplémentaire est requise lors de la conception de votre application UWP pour le narrateur. (La [page de base du narrateur](https://support.microsoft.com/help/22808/windows-10-narrator-basics) vous guide tout au long de l’expérience utilisateur du narrateur.)

Les différences entre les comportements du clavier UWP et celles prises en charge par Narrator sont les suivantes :
-   Combinaisons de touches supplémentaires pour la navigation vers des éléments d’interface utilisateur qui ne sont pas exposés par le biais de la navigation au clavier standard, tels que Verr. Maj + touches de direction pour lire des étiquettes de contrôle.
-   Navigation vers les éléments désactivés. Par défaut, les éléments désactivés ne sont pas exposés via la navigation au clavier standard.
-   Contrôle des « vues » pour une navigation plus rapide en fonction de la granularité de l’interface utilisateur. Les utilisateurs peuvent accéder aux éléments, aux caractères, aux mots, aux lignes, aux paragraphes, aux liens, aux en-têtes, aux tableaux, aux points de repère et aux suggestions. La navigation au clavier standard expose ces objets sous la forme d’une liste plate, ce qui peut compliquer la navigation, sauf si vous fournissez des touches de raccourci.

#### <a name="case-study--autosuggestbox-control"></a>Étude de cas – contrôle AutoSuggestBox

Le bouton de recherche pour `AutoSuggestBox` le n’est pas accessible à la navigation au clavier standard à l’aide des touches de tabulation et de direction, car l’utilisateur peut appuyer sur la touche **entrée** pour envoyer la requête de recherche. Toutefois, il est accessible par le narrateur quand l’utilisateur appuie sur Verr. Maj + une touche de direction.

![suggestion automatique du focus clavier](images/keyboard/auto-suggest-keyboard.png)

*Avec le clavier, les utilisateurs appuient sur la* touche ***entrée*** *pour envoyer une requête de recherche*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>Avec Narrator, les utilisateurs appuient sur la touche <strong>entrée</strong> pour envoyer une requête de recherche</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>Avec Narrator, les utilisateurs peuvent également accéder au bouton de recherche à l’aide de la <strong>touche VERR. Maj + Flèche droite</strong>, puis en appuyant sur la touche <strong>espace</strong></em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Clavier et manette Xbox et contrôle à distance

Les boîtiers de commande Xbox et les contrôles à distance prennent en charge de nombreux comportements et expériences du clavier UWP. Toutefois, en raison de l’absence de différentes options clés disponibles sur un clavier, le boîtier de commande et le contrôle à distance manquent de nombreuses optimisations du clavier (le contrôle à distance est encore plus limité que le boîtier de commande).

Pour plus d’informations sur la prise en charge UWP et l’entrée de contrôle à distance, consultez les [interactions du boîtier et du contrôle à distance](gamepad-and-remote-interactions.md) .

L’exemple suivant montre des mappages de clés entre le clavier, le boîtier de commande et le contrôle à distance.

| **Clavier**  | **Boîtier de commande**                         | **Contrôle à distance**  |
|---------------|-------------------------------------|---------------------|
| Espace         | Bouton A                            | Bouton Sélectionner       |
| Entrez         | Bouton A                            | Bouton Sélectionner       |
| Caractère d'échappement        | Bouton B                            | Bouton Précédent         |
| Début/fin      | N/A                                 | N/A                 |
| Page vers le haut/vers le haut  | Bouton de déclenchement pour le défilement vertical, bouton du pare-chocs pour le défilement horizontal   | N/A                 |

Voici quelques-unes des principales différences à prendre en compte lors de la conception de votre application UWP pour une utilisation avec le boîtier de commande et l’utilisation du contrôle à distance :
-   La saisie de texte oblige l’utilisateur à appuyer sur un pour activer un contrôle de texte.
-   La navigation au focus n’est pas limitée aux groupes de contrôles, les utilisateurs peuvent accéder librement à n’importe quel élément d’interface utilisateur pouvant être actif dans l’application.

    **Remarque** Le focus peut se déplacer vers n’importe quel élément d’interface utilisateur dans le sens de la frappe, sauf s’il se trouve dans une interface utilisateur de superposition ou si le [focus](gamepad-and-remote-interactions.md#focus-engagement) est spécifié, ce qui empêche le focus d’entrer/quitter une région jusqu’à ce qu’elle soit engagée/débrayée avec le bouton a. Pour plus d’informations, consultez la section [navigation directionnelle](#directional-navigation) .
-   Les boutons D-Pad et Stick Stick permettent de déplacer le focus entre les contrôles et la navigation interne.

    **Remarque** Le boîtier de commande et le contrôle à distance naviguent uniquement vers les éléments qui se trouvent dans le même ordre visuel que la touche directionnelle enfoncée. La navigation est désactivée dans cette direction lorsqu’il n’existe aucun élément suivant qui peut recevoir le focus. Selon la situation, les utilisateurs du clavier n’ont pas toujours cette contrainte. Pour plus d’informations, consultez la section [optimisation du clavier intégré](#built-in-keyboard-optimization) .

#### <a name="directional-navigation"></a>Navigation directionnelle

La navigation directionnelle est gérée par une classe d’assistance du [Gestionnaire de focus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) UWP, qui prend la touche directionnelle enfoncée (touche de direction, D-PAD) et tente de déplacer le focus dans la direction visuelle correspondante.

Contrairement au clavier, quand une application opte pour le [mode souris](gamepad-and-remote-interactions.md#mouse-mode), la navigation directionnelle est appliquée à l’ensemble de l’application pour le boîtier et le contrôle à distance. Pour plus d’informations sur l’optimisation de la navigation directionnelle, consultez le [boîtier et les interactions de contrôle à distance](gamepad-and-remote-interactions.md) .

**Remarque** La navigation à l’aide de la touche Tab n’est pas considérée comme une navigation directionnelle. Pour plus d’informations, consultez la section [arrêter les tabulations](#tab-stops) .

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Navigation directionnelle prise en charge</strong></br>À l’aide des touches de direction (flèches du clavier, boîtier de commande et contrôle à distance D), l’utilisateur peut naviguer entre les différents contrôles.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Navigation directionnelle non prise en charge</strong> </br>L’utilisateur ne peut pas naviguer entre les différents contrôles à l’aide des touches directionnelles. Les autres méthodes de navigation entre les contrôles (touche TAB) ne sont pas affectées.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Optimisation du clavier intégré

Selon la disposition et les contrôles utilisés, les applications UWP peuvent être optimisées spécifiquement pour l’entrée au clavier.

L’exemple suivant montre un groupe d’éléments de liste, d’éléments de grille et d’éléments de menu qui ont été assignés à un seul taquet de tabulation (voir la section des [taquets de tabulation](#tab-stops) ). Lorsque le groupe a le focus, la navigation interne est effectuée à l’aide des touches de direction directionnelle dans l’ordre visuel correspondant (voir la section [navigation](#navigation) ).

![navigation dans la touche de direction d’une seule colonne](images/keyboard/single-column-arrow.png)

***Navigation dans la touche de direction d’une seule colonne***

![navigation sur une seule touche de flèche d’une seule ligne](images/keyboard/single-row-arrow.png)

***Navigation sur une seule touche de flèche d’une seule ligne***

![navigation sur plusieurs flèches de colonne et de ligne](images/keyboard/multiple-column-and-row-navigation.png)

***Navigation entre les touches de direction sur plusieurs colonnes/lignes***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Encapsulation d’éléments de liste et de vue de grille homogènes

La navigation directionnelle n’est pas toujours la manière la plus efficace de parcourir plusieurs lignes et colonnes d’éléments de liste et GridView.

**Remarque** Les éléments de menu sont généralement des listes de colonnes uniques, mais des règles de focus spéciales peuvent s’appliquer dans certains cas (consultez [interface utilisateur contextuelle](#popup-ui)).

Les objets de liste et de grille peuvent être créés avec plusieurs lignes et colonnes. Celles-ci sont généralement dans la ligne principale (où les éléments remplissent la ligne entière avant de remplir la ligne suivante) ou colonne-principal (où les éléments remplissent la colonne tout d’abord avant de remplir la colonne suivante). L’ordre majeur des lignes ou des colonnes dépend de la direction de défilement et vous devez vous assurer que l’ordre des éléments n’est pas en conflit avec cette direction.

Dans l’ordre ligne-principal (où les éléments sont remplis de gauche à droite, de haut en bas), lorsque le focus se trouve sur le dernier élément d’une ligne et que la touche de direction droite est enfoncée, le focus est déplacé vers le premier élément de la ligne suivante. Ce même comportement se produit dans l’ordre inverse : lorsque le focus est défini sur le premier élément d’une ligne et que la touche de direction gauche est enfoncée, le focus est déplacé vers le dernier élément de la ligne précédente.

Dans l’ordre des colonnes (dans le cas où les éléments remplissent de haut en bas, de gauche à droite), lorsque le focus se trouve sur le dernier élément d’une colonne et que l’utilisateur appuie sur la touche de direction bas, le focus est déplacé vers le premier élément de la colonne suivante. Ce même comportement se produit dans l’ordre inverse : lorsque le focus est défini sur le premier élément d’une colonne et que la touche de direction haut est enfoncée, le focus est déplacé vers le dernier élément de la colonne précédente.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Navigation de clavier de ligne principale</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Navigation de clavier de colonne principale</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Interface utilisateur contextuelle

Comme nous l’avons vu, vous devez essayer de garantir que la navigation directionnelle correspond à l’ordre visuel des contrôles dans l’interface utilisateur de votre application.

Certains contrôles (tels que le menu contextuel, le menu CommandBar Overflow et le menu suggestion automatique) affichent un menu contextuel dans un emplacement et un sens (vers le bas par défaut) par rapport au contrôle principal et à l’espace écran disponible. Notez que le sens d’ouverture peut être affecté par divers facteurs au moment de l’exécution.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

Pour ces contrôles, lorsque le menu est ouvert pour la première fois (et qu’aucun élément n’a été sélectionné par l’utilisateur), la touche de direction bas définit toujours le focus sur le premier élément tandis que la touche de direction haut définit toujours le focus sur le dernier élément du menu. 

Si le dernier élément a le focus et que la touche de direction bas est enfoncée, le focus se déplace vers le premier élément du menu. De même, si le premier élément a le focus et que la touche de direction haut est enfoncée, le focus se déplace vers le dernier élément du menu. Ce comportement est appelé *cycle* et est utile pour naviguer dans les menus contextuels qui peuvent s’ouvrir dans des directions imprévisibles.

> [!NOTE]
> Le cycle doit être évité dans les interfaces utilisateur non contextuelles, où les utilisateurs peuvent se sentir sentir dans une boucle sans fin. 

Nous vous recommandons d’émuler ces mêmes comportements dans vos contrôles personnalisés. Vous trouverez un exemple de code sur la façon d’implémenter ce comportement dans la documentation de la [navigation de focus par programmation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) .

## <a name="test-your-app"></a>Test de l'application

Testez votre application avec tous les périphériques d’entrée pris en charge pour vous assurer que les éléments d’interface utilisateur peuvent être parcourus de manière cohérente et intuitive et qu’aucun élément inattendu n’interfère avec l’ordre de tabulation souhaité.

## <a name="related-articles"></a>Articles connexes
* [Événements de clavier](keyboard-events.md)
* [Identification des périphériques d’entrée](identify-input-devices.md)
* [Répondre à la présence du clavier tactile](respond-to-the-presence-of-the-touch-keyboard.md)
* [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

## <a name="appendix"></a>Annexe

### <a name="software-keyboard"></a>Clavier logiciel

Le clavier logiciel est un clavier qui s’affiche à l’écran que l’utilisateur peut utiliser à la place du clavier physique pour taper et entrer des données à l’aide de la fonction tactile, de la souris, du stylet/du stylet ou d’un autre dispositif de pointage (un écran tactile n’est pas nécessaire). Sur l’écran tactile, ces claviers peuvent être directement manipulés pour entrer du texte. Sur les appareils Xbox One, les clés doivent être sélectionnées en déplaçant le focus visuel ou en utilisant les touches de raccourci à l’aide du dispositif de contrôle à distance ou du boîtier.

![Clavier tactile Windows 10](images/keyboard/default.png)

***Clavier tactile Windows 10***

![Clavier Xbox One à l’écran](images/keyboard/xbox-onscreen-keyboard.png)

***Clavier Xbox One à l’écran***

En fonction de l’appareil, le clavier logiciel apparaît lorsqu’un champ de texte ou un autre contrôle de texte modifiable est activé, ou lorsque l’utilisateur l’active manuellement par le biais du **Centre de notifications**:

![icône du clavier tactile dans le Centre de notification](images/keyboard/touch-keyboard-notificationcenter.png)

Si votre application définit le focus par programme sur un contrôle d’entrée de texte, le clavier tactile n’est pas appelé. Cela permet d’éliminer les comportements inattendus non provoqués directement par l’utilisateur. Toutefois, le clavier est automatiquement masqué lorsque le focus est déplacé par programme vers un contrôle autre que d’entrée de texte.

En général, le clavier tactile ne disparaît pas automatiquement tant que l’utilisateur parcourt les contrôles dans un formulaire. Ce comportement peut varier en fonction des autres types de contrôle du formulaire.

Voici la liste des contrôles autres que d’édition qui peuvent recevoir le focus pendant une session d’entrée de texte à l’aide du clavier tactile sans que cela ait pour effet de masquer celui-ci. Au lieu de perturber inutilement l’interface utilisateur au risque de désorienter l’utilisateur, le clavier tactile reste bien en vue, car l’utilisateur va vraisemblablement passer des contrôles à l’entrée de texte à l’aide du clavier tactile.

-   Case à cocher
-   Zone de liste modifiable
-   Radio button
-   Barre de défilement
-   Arborescence
-   Élément d’arborescence
-   Menu
-   Barre de menus
-   Élément de menu
-   Barre d'outils
-   List
-   Élément de liste

Voici quelques exemples des différents modes disponibles pour le clavier tactile. La première image représente la disposition classique, la seconde représente la disposition ergonomique (qui n’est pas forcément disponible dans toutes les langues).

![clavier tactile en mode de disposition classique](images/keyboard/default.png)

***Clavier tactile en mode de disposition par défaut***

![clavier tactile en mode de disposition étendue](images/keyboard/extendedview.png)

***Clavier tactile en mode de disposition développé***

Des interactions réussies avec le clavier permettent aux utilisateurs d’accomplir des scénarios d’application de base uniquement à l’aide du clavier. Autrement dit, les utilisateurs peuvent atteindre tous les éléments interactifs et activer leur fonctionnalité par défaut. Plusieurs facteurs peuvent affecter le degré de réussite, tels que la navigation à l’aide du clavier, les touches d’accès pour l’accessibilité et les touches d’accès rapide (ou de raccourci) pour les utilisateurs expérimentés.

**Notez**  que le clavier tactile ne prend pas en charge le basculement et la plupart des commandes système.

#### <a name="on-screen-keyboard"></a>Clavier visuel
À l’instar du clavier logiciel, le clavier visuel est un clavier visuel et logiciel que vous pouvez utiliser au lieu du clavier physique pour taper et entrer des données à l’aide de la fonction tactile, de la souris, du stylet/du stylet ou d’un autre dispositif de pointage (un écran tactile n’est pas nécessaire). Le Clavier visuel est fourni pour les systèmes qui ne possèdent pas de clavier physique ou pour les utilisateurs qui connaissent des problèmes de mobilité les empêchant d’utiliser les périphériques d’entrée physiques classiques. Le clavier visuel émule la plupart, sinon la totalité, des fonctionnalités d’un clavier matériel.

Il peut être activé depuis la page Clavier dans Paramètres &gt; Options d’ergonomie.

**Remarque** Le clavier visuel est prioritaire sur le clavier tactile, qui ne s’affiche pas si le clavier visuel est présent.

![clavier visuel](images/keyboard/osk.png)

***Clavier visuel***

Visitez la [page du clavier visuel](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) pour plus d’informations sur le clavier visuel.

## <a name="related-articles"></a>Articles connexes

- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)
