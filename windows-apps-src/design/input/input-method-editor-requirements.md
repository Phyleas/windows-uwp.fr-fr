---
description: Développez un éditeur de méthode d’entrée (IME) personnalisé pour aider un texte d’entrée utilisateur dans une langue qui ne peut pas être représentée facilement sur un clavier AZERTY standard.
title: Spécifications de l’éditeur de méthode d’entrée (IME)
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: IME, éditeur de méthode d’entrée, entrée, interaction
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 74c223aefa525bb6109521c8b91a9a849e2f5586
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030132"
---
# <a name="custom-input-method-editor-ime-requirements"></a>Configuration requise de l’éditeur de méthode d’entrée (IME) personnalisé

Ces instructions et exigences peuvent vous aider à développer un éditeur de méthode d’entrée (IME) personnalisé pour aider un texte d’entrée d’utilisateur dans une langue qui ne peut pas être représentée facilement sur un clavier AZERTY standard.

Pour obtenir une vue d’ensemble des éditeurs de méthode d’entrée, consultez [éditeur de méthode d’entrée (IME)](input-method-editors.md).

## <a name="default-ime"></a>IME par défaut

Un utilisateur peut sélectionner l’un de ses IME actifs ( **paramètres-> heure & langue-> langue-> langues préférées-> module linguistique-options** ) comme IME par défaut pour la langue de votre choix.

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="Paramètre de langue par défaut":::

Sélectionnez le clavier par défaut dans l’écran Paramètres options de langue pour la langue par défaut.

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="Paramètre de langue par défaut":::

> [!Important]
> Nous vous déconseillons d’écrire directement dans le registre pour définir le clavier par défaut de votre IME personnalisé.

## <a name="compatibility-requirements"></a>Spécifications de compatibilité

Voici les conditions de compatibilité de base pour un IME personnalisé.

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME doit être compatible avec les applications Windows

Utilisez [Text Services Framework (TSF)](/windows/win32/tsf/text-services-framework) pour implémenter des IME. Auparavant, vous aviez la possibilité d’utiliser le [Gestionnaire de méthode d’entrée (imm32)](/windows/win32/intl/input-method-manager) pour les services d’entrée. Désormais, le système bloque les IME qui sont implémentés à l’aide du gestionnaire de méthode d’entrée (IMM32).

Lorsqu’une application démarre, TSF charge la DLL IME pour l’IME actuellement sélectionné par l’utilisateur. Lorsqu’un IME est chargé, il est soumis aux mêmes restrictions de conteneur d’application que l’application. Par exemple, un IME ne peut pas accéder à Internet si une application n’a pas demandé l’accès à Internet dans son manifeste. Ce comportement garantit que les éditeurs de la sécurité ne peuvent pas violer les contrats de sécurité.

TSF est l’intermédiaire entre l’application et votre IME. TSF communique les événements d’entrée à l’IME et reçoit les caractères d’entrée de l’IME une fois que l’utilisateur a sélectionné un caractère.

Ce comportement est identique à celui des versions précédentes de Windows, mais le chargement dans une application Windows affecte les fonctionnalités potentielles d’un IME.

Si votre IME doit fournir des fonctionnalités ou une interface utilisateur différentes entre les applications Windows et les applications de bureau, assurez-vous que la DLL chargée par TSF vérifie le type d’application dans lequel elle est chargée. Appelez la méthode [ITfThreadMgrEx :: GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) dans votre IME et vérifiez l’indicateur TF_TMF_IMMERSIVEMODE, afin que votre IME déclenche une logique d’application différente selon le résultat.

Les applications Windows ne prennent pas en charge les IME du service de texte de table (TTS).

> [!NOTE]
> Certains outils pour générer des IME TTS produisent des IME marqués comme programmes malveillants par Windows.

### <a name="ime-must-be-compatible-with-the-system-tray"></a>IME doit être compatible avec la barre d’état système

Il n’existe aucune barre de langue pour héberger les icônes IME. Au lieu de cela, un indicateur d’entrée s’affiche sur la barre d’état système qui indique l’option d’entrée actuelle. L’indicateur d’entrée affiche uniquement l’icône de personnalisation de l’éditeur IME pour indiquer l’IME en cours d’exécution. En outre, il existe une icône en mode IME qui s’affiche à gauche de l’icône de personnalisation de l’éditeur IME, qui permet aux utilisateurs d’exécuter le commutateur mode IME le plus couramment utilisé, comme l’activation ou la désactivation de l’IME.

L’indicateur d’entrée affiche l’icône de personnalisation de l’IME et l’icône de mode uniquement pour les IME compatibles. Les IME qui ne sont pas compatibles n’ont pas l’icône de personnalisation et l’icône de mode affichée dans la barre d’état système. Au lieu de cela, l’indicateur d’entrée affiche l’abréviation de la langue à la place de l’icône de personnalisation de l’éditeur IME.

Stockez les icônes IME dans un fichier DLL ou EXE, au lieu d’un fichier. ico autonome. La conception des icônes IME doit suivre les instructions décrites dans la section indications pour la conception d’interface utilisateur.

### <a name="ime-branding-icon"></a>Icône de personnalisation de l’éditeur IME

L’indicateur d’entrée obtient l’icône de personnalisation de l’IME à partir de la DLL IME en utilisant l’ID de ressource défini par l’IME lorsqu’il a été inscrit sur le système.

### <a name="ime-mode-icon"></a>Icône mode IME

Certains IME peuvent avoir besoin de s’appuyer sur l’indicateur d’entrée affiché dans la barre d’état système pour afficher l’icône mode IME. Dans ce cas, l’IME passe l’icône du mode IME à l’indicateur d’entrée à l’aide de GUID_LBI_INPUTMODE.

Lorsque vous passez les icônes mode IME à l’indicateur d’entrée sur la barre d’état système, la taille par défaut de l’icône mode IME est de 16x16 pixels. La mise à l’échelle de l’interface utilisateur suit DPI.

Lors du passage de l’icône du mode IME à l’indicateur d’entrée sur UAC (contrôle de compte d’utilisateur dans le Bureau sécurisé), la taille par défaut de l’icône du mode IME est 20x20 pixels. L’icône de mise à l’échelle de l’interface utilisateur pour le mode IME sur UAC suit les PPP.

## <a name="ime-must-work-in-app-container"></a>L’IME doit fonctionner dans le conteneur d’application

Certaines fonctions IME sont affectées dans un conteneur d’application.

- **Fichiers de dictionnaire** -souvent, les éditeurs de la liste de prédéfinis ont des fichiers dictionnaire en lecture seule pour mapper les entrées utilisateur à des caractères spécifiques. Pour accéder à ces fichiers à partir d’un conteneur d’application, votre IME doit les placer sous les fichiers programme ou les répertoires Windows. Par défaut, ces répertoires peuvent être lus à partir d’un conteneur d’application, de sorte que les éditeurs IME peuvent accéder aux fichiers de dictionnaire stockés à ces emplacements. Si votre IME doit stocker le fichier de dictionnaire ailleurs, il doit manipuler de manière explicite les [listes de Access Control (ACL)](/windows/win32/secauthz/access-control-lists) des fichiers de dictionnaire pour autoriser l’accès à partir des conteneurs d’applications.
- **Mise à jour Internet** : Si votre IME doit mettre à jour ses dictionnaires à l’aide de données provenant d’Internet, il ne peut pas le faire de manière fiable dans un conteneur d’application, car l’accès à Internet n’est pas toujours autorisé. Au lieu de cela, votre IME doit exécuter un processus de bureau distinct chargé de mettre à jour les fichiers de dictionnaire avec des données provenant d’Internet.
- **Apprentissage à la volée** : si un IME s’exécute dans un conteneur d’application qui a accès à Internet, il n’existe aucune restriction sur les points de terminaison avec lesquels l’IME peut communiquer. Dans ce cas, un IME peut utiliser un serveur Cloud pour fournir des services d’apprentissage à la volée. Certains IME téléchargent et téléchargent des entrées utilisateur à la volée, pendant que l’utilisateur tape. Étant donné que l’accès à Internet n’est pas garanti dans un conteneur d’application, cela n’est pas toujours autorisé.
- **Partage d’informations entre processus** : les éditeurs de logiciels peuvent devoir partager des données sur les préférences d’entrée de l’utilisateur entre les applications qui se trouvent dans des conteneurs d’applications différents. Utilisez un service Web pour partager des données entre des applications.

> [!Important]
> Si vous essayez de contourner les règles de sécurité du conteneur d’application, votre IME peut être traité comme un programme malveillant et bloqué.

## <a name="ime-and-touch-keyboard"></a>IME et clavier tactile

Votre IME doit s’assurer que l’interface utilisateur du volet candidat et d’autres éléments d’interface utilisateur ne sont pas dessinés sous le clavier tactile. Le clavier tactile est affiché dans une bande d’ordre de plan supérieure à celle de toutes les applications, et l’interface utilisateur de l’éditeur IME est affichée dans la même bande de l’ordre de plan que l’application dans laquelle elle est active. Par conséquent, le clavier tactile peut se chevaucher et masquer l’interface utilisateur de l’IME. Dans la plupart des cas, l’application doit redimensionner sa fenêtre pour tenir compte du clavier tactile. Si une application ne se redimensionne pas, l’IME peut toujours utiliser l’API [InputPane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) pour recevoir la position du clavier tactile. L’IME interroge la propriété d' [emplacement](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) ou inscrit un gestionnaire pour les événements d’affichage et de masquage du clavier tactile. L’événement Show est déclenché chaque fois que l’utilisateur appuie sur un champ d’édition, même si le clavier tactile est actuellement affiché. Votre IME peut utiliser cette API pour récupérer l’espace à l’écran utilisé par le clavier tactile avant que l’IME ne dessine l’interface utilisateur candidate (ou autre), et pour redistribuer l’interface utilisateur des éditeurs pour éviter le dessin sous le clavier tactile.

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>Spécification de la disposition de clavier tactile par défaut

L’IME peut spécifier la disposition de clavier tactile à utiliser, et l’IME est activé pour fonctionner avec des dispositions tactiles optimisées. Cette fonctionnalité est limitée aux IME pour les langues d’entrée traditionnelles en coréen, japonais, chinois simplifié et chinois traditionnel.

Il existe sept mises en page prises en charge par le clavier tactile, dont trois sont des mises en page classiques et quatre, qui sont des dispositions tactiles optimisées. Les dispositions classiques apparaissent et se comportent comme un clavier physique.

Toutes les trois dispositions classiques sont destinées à l’entrée de chinois traditionnel sous différentes formes :

- Entrée phonétique
- Entrée ChangJie
- Entrée-dayi

Outre les dispositions classiques, il existe une disposition tactile optimisée pour chacun des langages d’entrée coréen, japonais, chinois simplifié et chinois traditionnel.

Pour utiliser cette fonctionnalité, votre IME doit implémenter l’interface [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) , qui est exportée par l’IME à l’aide de l’API [ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) de Text Services Framework.

Si votre IME ne prend pas en charge l’interface ITfFnGetPreferredTouchKeyboardLayout, l’utilisation de l’IME entraîne la mise en page classique par défaut pour la langue affichée par le clavier tactile.

Si votre IME doit définir l’une des dispositions classiques comme mise en page par défaut, aucun travail supplémentaire n’est requis côté IME au-delà de la prise en charge des interfaces ITfFnGetPreferredTouchKeyboardLayout et ITfFunctionProvider. Toutefois, un travail supplémentaire est requis dans l’IME pour fonctionner avec les dispositions tactiles, ce qui est décrit dans la section suivante.

### <a name="touch-optimized-layout"></a>Disposition tactile optimisée

Les claviers orientés tactiles pour le coréen, le japonais, le chinois simplifié et les langues d’entrée en chinois traditionnel affichent une disposition différente pour les modes de conversion IME activé et IME. Il y a une touche sur le clavier tactile pour définir le mode de conversion de l’IME sur activé ou désactivé, mais le mode IME du clavier peut également changer en fonction des changements de focus parmi les contrôles d’édition.

Les claviers orientés tactiles pour les langues d’entrée en japonais, en chinois simplifié et en chinois traditionnel contiennent une clé, ou des clés, que l’IME utilise pour naviguer dans les pages candidates. Pour le japonais et le chinois simplifié, la clé de la page candidate s’affiche sur la disposition tactile. Pour le chinois traditionnel, il existe des clés distinctes pour les pages candidates précédentes et suivantes.

Lorsque ces touches sont enfoncées, le clavier tactile appelle la fonction [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) pour envoyer les caractères de zone d’utilisation privée Unicode suivants à l’application ciblée, que l’IME peut intercepter et agir :

- **Page suivante (0xF003)** -envoyé lorsque la clé de la page candidate est enfoncée sur le clavier tactile pour le japonais et le chinois simplifié, ou lorsque la touche page suivante est enfoncée sur le clavier tactile pour le chinois traditionnel.
- **Page précédente (0xF004)** -envoyé quand la touche de la page candidate est enfoncée en même temps que la touche Maj sur le clavier optimisé pour le japonais et le chinois simplifié, ou lorsque la touche page précédente est enfoncée sur le clavier optimisé pour le chinois traditionnel.

Ces caractères sont envoyés en tant qu’entrée Unicode. Le paragraphe suivant explique en détail comment extraire les informations sur les caractères au cours des notifications du récepteur d’événements clés que l’éditeur de méthode d’entrée de l’infrastructure Text Services recevra. Ces valeurs de caractères ne sont pas définies dans un fichier d’en-tête. vous devrez donc les définir dans votre code.

Pour intercepter l’entrée au clavier, votre IME doit s’inscrire en tant que récepteur d’événements Key. Pour une entrée Unicode générée à l’aide de la fonction SendInput, le paramètre WPARAM des rappels [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) (OnKeyDown, OnKeyUp, OnTestKeyDown, OnTestKeyUp) contient toujours la clé virtuelle VK_PACKET et n’identifie pas directement le caractère.

Implémentez la séquence d’appels suivante pour accéder au caractère :

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>Intégration de la recherche IME

Fournissez aux utilisateurs des fonctionnalités de recherche via le contrat de recherche et l’intégration au volet de recherche.

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="Paramètre de langue par défaut":::<br/>
*Volet de recherche et suggestions IME*

Le volet de recherche est un emplacement central où les utilisateurs peuvent effectuer des recherches dans l’ensemble de leurs applications. Pour les utilisateurs de l’IME, Windows fournit une expérience de recherche unique qui permet d’intégrer des éditeurs IME compatibles avec Windows afin d’améliorer l’efficacité et la convivialité.

Les utilisateurs qui tapent avec un IME compatible avec la recherche bénéficient de deux principaux avantages :

- Interaction transparente entre l’IME et l’expérience de recherche. Les candidats IME s’affichent en ligne sous la zone de recherche sans les suggestions de recherche d’occlusion. L’utilisateur peut utiliser le clavier pour naviguer de manière transparente entre la zone de recherche, les candidats de conversion IME et les suggestions de recherche.
- Accès plus rapide aux résultats et suggestions pertinents fournis par les applications. L’application a accès à tous les candidats de conversion actuels pour fournir des suggestions plus pertinentes. Pour mieux hiérarchiser les suggestions de recherche, les conversions sont accordées aux applications par ordre de pertinence. Les utilisateurs recherchent et sélectionnent le résultat souhaité sans conversion, simplement en tapant phonétique.

Un IME est compatible avec l’expérience de recherche intégrée s’il répond aux critères suivants :

- Compatible avec l’interpréteur de commandes de style Windows.
- Implémentez les API de mode UILess TSF. Pour plus d’informations, consultez [vue d’ensemble du mode UILess](/windows/win32/tsf/uiless-mode-overview).
- Implémentez les API d’intégration de recherche TSF, [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider) et [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement).

Lorsqu’il est activé dans le volet de recherche, un IME compatible est placé en mode UIless et ne peut pas afficher son interface utilisateur. Au lieu de cela, il envoie des candidats de conversion à Windows, qui les affiche dans le contrôle de liste de candidat Inline, comme indiqué dans la capture d’écran précédente.

En outre, l’IME envoie les candidats qui doivent être utilisés pour exécuter la recherche actuelle. Ces candidats peuvent être les mêmes que les candidats à la conversion, ou ils peuvent être personnalisés pour la recherche.

Les bons candidats à la recherche répondent aux critères suivants :

- Aucun chevauchement de préfixe. Par exemple, les and北京 北京大学 sont redondants, car l’un d’eux est un préfixe de l’autre.
- Aucun candidat redondant. Tout candidat redondant n’est pas utile pour la recherche, car il ne permet pas de filtrer les résultats. Par exemple, tout résultat qui correspond à 北京大学 correspond également à 北京.
- Pas de prédiction candidate, uniquement conversion. Par exemple, si l’utilisateur tape « a », l’IME peut retourner 北 comme candidat, mais pas 北京大学. En règle générale, les candidats aux prédictions sont trop restrictifs.

Les IME qui ne répondent pas aux critères ne sont pas compatibles avec l’affichage de la recherche de la même façon que les autres contrôles et ne peuvent pas tirer parti de l’intégration de l’interface utilisateur et des candidats à la recherche. Les applications reçoivent des requêtes uniquement une fois que l’utilisateur a terminé la composition.

Lorsqu’une application qui prend en charge le contrat de recherche reçoit une requête, l’événement de requête contient un tableau « queryTextAlternatives » qui contient toutes les alternatives connues, classées du plus pertinent (probablement) au moins pertinent (improbable).

Lorsque des alternatives sont fournies, l’application doit traiter chaque alternative comme une requête et retourner tous les résultats qui correspondent à l’une des alternatives. L’application doit se comporter comme si l’utilisateur avait émis plusieurs requêtes en même temps, en émettant une requête « ou » au service qui fournit les résultats. Pour des raisons de performances, les applications limitent souvent la correspondance à entre 5 et 20 des alternatives les plus pertinentes.

## <a name="ui-design-guidelines"></a>Indications pour la conception d’interface utilisateur

Tous les IME doivent suivre les instructions d’expérience utilisateur décrites dans [conception et code applications Windows](../index.md).

### <a name="dont-use-sticky-windows"></a>N’utilisez pas de fenêtres rémanentes

Vos fenêtres IME doivent apparaître uniquement lorsque cela est nécessaire, et elles ne doivent pas être visibles tout le temps. Quand les utilisateurs n’ont pas besoin de taper, les fenêtres IME ne s’affichent pas. La fenêtre IME ne doit pas être une fenêtre plein écran. Les fenêtres IME ne doivent pas se chevaucher. Les fenêtres doivent être conçues dans un style Windows et suivre la mise à l’échelle de l’interface utilisateur.

### <a name="ime-icons"></a>Icônes IME

Il existe deux types d’icônes d’IME : les icônes de personnalisation et les icônes de mode. Toutes les icônes IME doivent être conçues uniquement avec des couleurs noires et blanches. Les nouvelles icônes IME empruntent l’apparence glyphe des icônes de la barre d’état système. Ce style a été créé afin que tous les langages puissent l’utiliser pour compléter l’apparence de la familial tout en se différenciant les unes des autres.

Le format de fichier pour les icônes IME est ICO. Vous devez fournir les tailles d’icônes suivantes.

- 16x16 pixels
- 20x20 pixels
- 24x24 pixels
- 32 x 32 pixels
- 40 x 40 pixels
- 48 pixels

Assurez-vous que les icônes 32 bits avec canal alpha sont fournies dans toutes les résolutions.

Les icônes de la personnalisation IME sont définies par une zone blanche dans laquelle un glyphe typographique rendu dans une police moderne est placé. Chaque définition de glyphe est choisie par chaque équipe de langage. Le glyphe est noir. La zone comprend un trait extérieur de 1 pixel en noir à 50% d’opacité. Les nouvelles versions sont définies par un angle arrondi dans le coin supérieur gauche de la zone.

Les icônes en mode IME sont définies par un glyphe typographique blanc dans une police moderne qui comprend un trait extérieur de 1 pixel en noir à 50% d’opacité.

| Icône | Description |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="Paramètre de langue par défaut"::: | Exemple d’icône de personnalisation IME pour le chinois traditionnel ChangeJie. |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="Paramètre de langue par défaut"::: | Exemple d’icône de personnalisation IME pour le chinois traditionnel ChangeJie. |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="Paramètre de langue par défaut"::: | Exemple d’icône mode IME. |

### <a name="owned-window"></a>Fenêtre propriétaire

Pour afficher l’interface utilisateur candidate, un IME doit définir sa fenêtre de sorte qu’elle soit propriétaire de la fenêtre, afin qu’elle puisse s’afficher sur l’application en cours d’exécution. Utilisez la méthode [ITfContextView :: GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd) pour récupérer la fenêtre à laquelle il appartient. Si GetWnd retourne une erreur ou un NULLHWND, appelez la fonction [getFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) .

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>Interaction de la fenêtre candidate IME avec les surfaces d’écart clair

Le modèle de masquage des fenêtres contextuelles est appelé « ignorer la lumière », car il est facile pour un utilisateur de fermer ces fenêtres. Pour que les éditeurs IME fonctionnent correctement dans le modèle d’interaction Windows, les fenêtres IME doivent participer au modèle d’ignorer la lumière.

Pour participer au modèle d’ignorer la lumière, votre IME doit déclencher trois nouveaux événements Windows à l’aide de la fonction [NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent) ou d’une fonction similaire. Ces nouveaux événements sont les suivants :

- **EVENT_OBJECT_IME_SHOW** -déclenche cet événement lorsque l’IME devient visible.
- **EVENT_OBJECT_IME_HIDE** -déclenche cet événement lorsque l’IME est masqué.
- **EVENT_OBJECT_IME_CHANGE** : déclenche cet événement lorsque l’IME se déplace ou change de taille.

### <a name="declaring-compatibility"></a>Déclaration de compatibilité

Les éditeurs de la déclaration déclarent qu’ils sont compatibles en inscrivant la catégorie GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT pour leur IME à l’aide de [ITfCategoryMgr :: RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory).

### <a name="set-the-default-ime-mode-to-on"></a>Définir le mode IME par défaut sur activé

Nous fournissons une meilleure expérience utilisateur pour les IME.

## <a name="dpi-scaling-support-for-desktop-applications"></a>Prise en charge de la mise à l’échelle DPI pour les applications de bureau

La prise en charge de la mise à l’échelle PPP améliorée permet d’interroger le niveau de détection PPP déclaré de chaque processus de bureau pour déterminer s’il doit mettre à l’échelle l’interface utilisateur. Dans un scénario à plusieurs écrans, Windows met à l’échelle l’interface utilisateur de manière appropriée pour les différents paramètres PPP sur chaque analyse.

Étant donné que votre IME s’exécute dans le contexte du processus de chaque application, vous ne devez pas déclarer un niveau de sensibilisation PPP pour votre IME. Cela garantit que votre IME s’exécute au niveau de reconnaissance PPP du processus actuel.

Pour vous assurer que tous les éléments d’interface utilisateur IME ont une parité de mise à l’échelle avec les éléments d’interface utilisateur du processus dans lequel vous exécutez, vous devez répondre de manière appropriée à différentes valeurs DPI.

> [!NOTE]
> Pour garantir la parité avec les nouvelles applications de bureau, votre IME doit prendre en charge par moniteur – la prise en charge DPI, mais ne doit pas déclarer un niveau de sensibilisation lui-même. Le système détermine les exigences de mise à l’échelle appropriées dans chaque scénario.

Pour plus d’informations sur les exigences de prise en charge de la mise à l’échelle DPI pour les applications de bureau, consultez [haute résolution](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows).

## <a name="ime-installation"></a>Installation IME

Si vous générez votre IME à l’aide de Microsoft Visual Studio, créez une expérience d’installation pour votre IME à l’aide d’un programme d’installation tiers, comme InstallShield à partir du logiciel Flexera.

Les étapes suivantes montrent comment utiliser InstallShield pour créer un projet d’installation pour votre DLL IME.

- Installez Visual Studio.
- Démarrez Visual Studio.
- Dans le menu **fichier** , pointez sur **nouveau** , puis sélectionnez **projet** . La boîte de dialogue **Nouveau projet** s’affiche.
- Dans le volet gauche, accédez à **modèles > d’autres types de projets > le programme d’installation et le déploiement** , cliquez sur **activer InstallShield Limited Edition** , puis cliquez sur **OK** . Suivez les instructions d'installation.
- Démarrez Visual Studio.
- Ouvrez le fichier de solution IME (. sln).
- Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution, pointez sur **Ajouter** , puis sélectionnez **nouveau projet** . La boîte de dialogue **Ajouter un nouveau projet** s’ouvre.
- Dans le contrôle d’arborescence de gauche, accédez à **modèles > d’autres types de projets > InstallShield Limited Edition** .
- Dans la fenêtre centrale, cliquez sur **projet InstallShield Limited Edition** .
- Dans la zone de texte **nom** , tapez « SetupIME », puis cliquez sur **OK** .
- Dans la boîte de dialogue **Assistant projet** , cliquez sur informations sur l' **application** .
- Renseignez le nom de votre société et les autres champs.
- Cliquez sur **fichiers d’application** .
- Dans le volet gauche, cliquez avec le bouton droit sur le dossier **[INSTALLDIR]** , puis sélectionnez **nouveau dossier** . Nommez le dossier « Plug-ins ».
- Cliquez sur **Ajouter des fichiers** . Accédez à votre DLL IME et ajoutez-la au dossier **plugins** . Répétez cette étape pour le dictionnaire IME.
- Cliquez avec le bouton droit sur la DLL IME et sélectionnez **Propriétés** . La boîte de dialogue **Propriétés** s’ouvre.
- Dans la boîte de dialogue **Propriétés** , cliquez sur l’onglet **paramètres com & .net** .
- Sous **type d’inscription** , sélectionnez **inscription automatique** , puis cliquez sur **OK** .
- Générez la solution. La DLL IME est générée et InstallShield crée un fichier setup.exe qui permet aux utilisateurs d’installer votre IME sur Windows.

Pour créer votre propre expérience d’installation, appelez la méthode [ITfInputProcessorProfileMgr :: RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) pour inscrire l’IME au cours de l’installation. N’écrivez pas d’entrées de Registre directement.

Si l’IME doit être utilisable immédiatement après l’installation, appelez [InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) pour ajouter l’IME aux méthodes d’entrée activées par l’utilisateur, en utilisant le format suivant pour le paramètre PSZ :

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>Accessibilité IME

Implémentez la Convention suivante pour rendre vos IME conformes aux exigences d’accessibilité et pour utiliser le narrateur. Pour rendre les listes de candidats accessibles, vos IME doivent respecter cette Convention.

- La liste de candidats doit avoir une **UIA_AutomationIdPropertyId** égale à « IME_Candidate_Window » pour les listes de candidats à la conversion ou « IME_Prediction_Window » pour les listes de candidats de prédiction.
- Quand la liste candidate apparaît et disparaît, elle déclenche des événements de type **UIA_MenuOpenedEventId** et **UIA_MenuClosedEventId** , respectivement
- Lorsque le candidat actuellement sélectionné change, la liste de candidats lève une **UIA_SelectionItem_ElementSelectedEventId** . L’élément sélectionné doit avoir une propriété **UIA_SelectionItemIsSelectedPropertyId** égale à **true** .
- La **UIA_NamePropertyId** pour chaque élément de la liste candidate doit être le nom du candidat. Si vous le souhaitez, vous pouvez fournir des informations supplémentaires pour lever l’ambiguïté des candidats par le biais de **UIA_HelpTextPropertyId** .

## <a name="related-topics"></a>Rubriques connexes

- [Éditeur de méthode d'entrée (IME)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [Accessibilité](../accessibility/accessibility.md)
