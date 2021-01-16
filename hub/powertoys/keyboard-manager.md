---
title: Utilitaire du gestionnaire de clavier PowerToys pour Windows 10
description: Utilitaire qui permet aux utilisateurs de redéfinir des touches sur le clavier
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8ffd782a1b23d1e439be0462300ebdf20593913
ms.sourcegitcommit: 375cf20e0583335805ec246d65819dc1674a2e32
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98240999"
---
# <a name="keyboard-manager-utility"></a>Utilitaire du gestionnaire de clavier

Le gestionnaire de clavier PowerToys vous permet de redéfinir les touches du clavier.

Par exemple, vous pouvez échanger la lettre <kbd>A</kbd> pour la lettre <kbd>D</kbd> sur votre clavier. Lorsque vous sélectionnez la touche <kbd>a</kbd> , un <kbd>D</kbd> s’affiche.

![Capture d’écran des clés de remappage du gestionnaire de clavier PowerToys](../images/powertoys-keyboard-remap.png)

Vous pouvez également échanger des combinaisons de touches de raccourci. Par exemple, la touche de raccourci <kbd>CTRL</kbd> + <kbd>C</kbd>copiera le texte dans Microsoft Word. Avec l’utilitaire du gestionnaire de clavier PowerToys, vous pouvez échanger ce raccourci pour <kbd>⊞ Win</kbd> + <kbd>C</kbd>). Maintenant, <kbd>⊞ Win</kbd> + <kbd>C</kbd>) copie le texte. Si vous ne spécifiez pas d’application ciblée dans le gestionnaire de clavier PowerToys, l’échange de raccourcis sera appliqué globalement sur Windows.

Le gestionnaire de clavier PowerToys doit être activé (avec les PowerToys exécutés en arrière-plan) pour les clés et les raccourcis remappés à appliquer. Si PowerToys n’est pas en cours d’exécution, le remappage de clé ne sera plus appliqué.

![Capture d’écran des raccourcis de remappage du gestionnaire de clavier PowerToys](../images/powertoys-keyboard-shortcuts.png)

> [!NOTE]
> Certaines touches de raccourci sont réservées pour le système d’exploitation et ne peuvent pas être remplacées. Les clés qui ne peuvent pas être remappées sont les suivantes :
> - `⊞ Win`+`L`et `Ctrl`  +  `Alt`  +  `Del` ne peuvent pas être remappés, car ils sont réservés par le système d’exploitation Windows.
> - La `Fn` clé (fonction) ne peut pas être remappée (dans la plupart des cas). Les `F1` - `F12` clés (et `F13` - `F24` ) peuvent être mappées.
> - `Pause` enverra uniquement un événement KeyOut Sngle. Par conséquent, si vous le mappez à la touche Retour arrière, par exemple et que vous appuyez sur + Holding, vous ne supprimerez qu’un seul caractère.

## <a name="settings"></a>Paramètres

Pour créer des mappages à l’aide du gestionnaire de clavier, vous devez ouvrir les paramètres PowerToys (recherchez l’application PowerToys dans le menu Démarrer de Windows, puis sélectionnez cette option pour ouvrir la fenêtre Paramètres PowerToys). Dans les paramètres PowerToys, sous l’onglet Gestionnaire de clavier, vous verrez les options suivantes :

- Lancez la fenêtre Paramètres de clavier de remappage en sélectionnant <kbd>remapper une clé</kbd>
- Ouvrez la fenêtre Paramètres de raccourcis de remappage en sélectionnant le <kbd>raccourci de remappage</kbd>

## <a name="remap-keys"></a>Remapper les clés

Pour remapper une clé, en la remplaçant par une nouvelle valeur, lancez la fenêtre Paramètres de clavier de remappage avec le bouton <kbd>remapper une clé</kbd> . Lors du premier lancement, aucun mappage prédéfini ne s’affiche. Vous devez sélectionner le <kbd>+</kbd> bouton pour ajouter un nouveau remappage.

Une fois qu’une nouvelle ligne de remappage s’affiche, sélectionnez la clé dont vous souhaitez **modifier** la sortie _ dans la colonne « clé ». Sélectionnez la nouvelle valeur de clé à assigner dans la colonne « mappé à ».

Par exemple, si vous souhaitez appuyer sur <kbd>un</kbd> et que <kbd>B</kbd>  apparaît :

- Clé : « A »
- Mappé à : "B"

Pour échanger des positions clés entre les clés « A » et « B », ajoutez un autre remappage avec :

- Clé : "B"
- Mappé à : "A"

![Capture d’écran des touches de remappage du clavier](../images/powertoys-keyboard-remap-a-b.png)

## <a name="key-to-shortcut"></a>Touche de raccourci

Pour remapper une clé à un raccourci (combinaison de touches), entrez la combinaison de touches de raccourci dans la colonne « mappé à ».

Par exemple, si vous souhaitez sélectionner la clé « C » et la faire se produire « Ctrl + V » :

- Clé : "C"
- Mappé à : « Ctrl + V »

> [!NOTE]
> Le remappage de clé sera conservé même si la clé remappée est utilisée dans un autre raccourci. Par exemple, si vous entrez le raccourci « ALT + C », vous obtenez la valeur « ALT + CTRL + V », car la touche C a été remappée à « Ctrl + V ».

## <a name="remap-shortcuts"></a>Raccourcis de remappage

Pour remapper une combinaison de touches de raccourci, par exemple « Ctrl + v », sélectionnez <kbd>remapper un raccourci</kbd> pour ouvrir la fenêtre Paramètres de raccourcis de remappage.

Lors du premier lancement, aucun mappage prédéfini ne s’affiche. Vous devez sélectionner le <kbd>+</kbd> bouton pour ajouter un nouveau remappage.

Une fois qu’une nouvelle ligne de remappage s’affiche, sélectionnez la clé dont vous souhaitez _*_modifier_*_ la sortie dans la colonne « raccourci ». Sélectionnez la nouvelle valeur de raccourci à assigner dans la colonne « mappé à ».

Par exemple, le raccourci <kbd>CTRL</kbd> + <kbd>C</kbd> copie le texte que vous avez sélectionné. Pour remapper les raccourcis afin d’utiliser la touche <kbd>ALT</kbd> , plutôt que la touche <kbd>CTRL</kbd> :

- Raccourci : « Ctrl » + « C »
- Mappé à : "alt" + "C"

![Capture d’écran du raccourci de remappage du clavier](../images/powertoys-keyboard-remap-shortcut.png)

Il existe quelques règles à suivre pour remapper les raccourcis (ces règles s’appliquent uniquement à la colonne « raccourci ») :

- Les raccourcis doivent commencer par une touche de modification : <kbd>CTRL</kbd>, <kbd>MAJ</kbd>, <kbd>ALT</kbd>ou <kbd>⊞ Win</kbd>
- Les raccourcis doivent se terminer par une clé d’action (toutes les clés qui ne sont pas des modificateurs) : A, B, C, 1, 2, 3, etc.
- Les raccourcis ne peuvent pas dépasser 3 clés  

## <a name="remap-a-shortcut-to-a-single-key"></a>Remapper un raccourci sur une clé unique

Il est possible de remapper un raccourci (combinaison de touches) à une seule pression sur une touche.

Par exemple, pour remplacer la touche de raccourci <kbd>⊞ Win</kbd>  +  <kbd>D</kbd> (afficher/masquer les applications de bureau Windows) par une seule pression de touche, <kbd>ALT</kbd>:

- Raccourci : « ⊞ Win » (touche Windows) + « D »
- Mappé à : "alt"

> [!NOTE]
> Le remappage des raccourcis sera conservé même si la clé remappée est utilisée dans un autre raccourci. Par exemple, si vous entrez le raccourci « Alt » + « Tab » après avoir remappé la touche « Alt » comme indiqué ci-dessus, vous obtiendrez « ⊞ Win » + « D » + « Tab ».

## <a name="app-specific-shortcuts"></a>Raccourcis spécifiques à l’application

Le gestionnaire de clavier vous permet de remapper les raccourcis pour des applications spécifiques uniquement (plutôt que globalement sur Windows).

Par exemple, dans l’application de messagerie Outlook, le raccourci « Ctrl + E » est défini par défaut pour rechercher un e-mail. Si vous préférez définir « Ctrl + F » pour effectuer une recherche dans votre courrier électronique (plutôt que de transférer un e-mail comme défini par défaut), vous pouvez remapper le raccourci avec « Outlook » défini comme « application cible ».

Le gestionnaire de clavier utilise les noms de processus (et non les noms d’application) pour cibler les applications. Par exemple, Microsoft Edge est défini sur « msedge » (nom de processus) et non sur « Microsoft Edge » (nom de l’application). Pour rechercher le nom du processus d’une application, ouvrez PowerShell, entrez la commande `get-process` ou ouvrez l’invite de commandes et entrez la commande `tasklist` . Cela génère une liste de noms de processus pour toutes les applications que vous avez actuellement ouvertes. Voici une liste de quelques noms de processus d’application populaires.

  | _ *Application**   | **Nom du processus**|
  | ------------------| --------------|
  | Microsoft Edge    |  msedge.exe   |
  | OneNote           |  onenote.exe  |
  | Outlook           |  outlook.exe  |
  | Teams             |  Teams.exe    |
  | Adobe Photoshop   |  Photoshop.exe|
  | Explorateur de fichiers     |  explorer.exe |
  | Musique Spotify     |  spotify.exe  |
  | Google Chrome     |  chrome.exe   |
  | Excel             |  excel.exe    |
  | Word              |  winword.exe  |
  | PowerPoint        |  powerpnt.exe |

### <a name="keys-that-cannot-be-remapped"></a>Clés qui ne peuvent pas être remappées

Certaines touches de raccourci ne sont pas autorisées pour le remappage. Il s’agit, entre autres, des suivantes :

- <kbd>CTRL</kbd> + <kbd>ALT</kbd> +  <kbd>Del</kbd> (commande interupt)
- <kbd>⊞ Win</kbd> + <kbd>L</kbd> (verrouillage de votre ordinateur)
- La touche de fonction, <kbd>FN</kbd>, ne peut pas être remappée (dans la plupart des cas), mais la touche <kbd>F1</kbd> - <kbd>F12</kbd> peut être mappée.

## <a name="how-to-select-a-key"></a>Comment sélectionner une clé

Pour sélectionner une touche ou un raccourci à remapper, vous pouvez :

- Utilisez le bouton <kbd>type de clé</kbd> .
- Utilisez le menu déroulant.

Une fois que vous avez sélectionné le bouton de <kbd>type clé/raccourci</kbd> , une boîte de dialogue s’affiche, dans laquelle vous pouvez entrer la touche ou le raccourci à l’aide de votre clavier. Une fois que vous êtes satisfait de la sortie, appuyez sur la touche <kbd>entrée</kbd> pour continuer. Si vous souhaitez conserver la boîte de dialogue, maintenez le bouton <kbd>Échap</kbd> enfoncé.

À l’aide du menu déroulant, vous pouvez effectuer une recherche avec le nom de clé et des valeurs de liste déroulante supplémentaires s’affichent à mesure que vous progressez. Toutefois, vous ne pouvez pas utiliser la fonctionnalité de clé de type lorsque le menu déroulant est ouvert.

## <a name="orphaning-keys"></a>Clés orphelines

L’orpheline d’une clé signifie que vous l’avez mappée à une autre clé et qu’aucune n’est mappée à celle-ci.

Par exemple, si la clé est remappée à partir d’un-> B, alors une clé n’existe plus sur votre clavier qui se traduit par un.

Pour résoudre ce problème, utilisez + pour créer une autre clé remappée qui est mappée pour générer un. Pour s’assurer que cela ne se produit pas par accident, un avertissement s’affiche pour toutes les clés orphelines.

![Clé orpheline du gestionnaire de clavier PowerToys](../images/powertoys-keyboard-remap-orphaned.png)

## <a name="frequently-asked-questions"></a>Forum aux questions

### <a name="i-remapped-the-wrong-keys-how-can-i-stop-it-quickly"></a>J’ai remappé les mauvaises clés, comment puis-je l’arrêter rapidement ?

Pour que le remappage de clé fonctionne, les PowerToys doivent s’exécuter en arrière-plan et le gestionnaire de clavier doit être activé. Pour arrêter les clés remappées, fermez les PowerToys ou désactivez le gestionnaire de clavier dans les paramètres PowerToys.

### <a name="can-i-use-keyboard-manager-at-my-log-in-screen"></a>Puis-je utiliser le gestionnaire de clavier sur mon écran de connexion ?

Non, le gestionnaire de clavier n’est disponible que lorsque PowerToys est en cours d’exécution et ne fonctionne pas sur un écran de mot de passe, notamment exécuter en tant qu’administrateur.

### <a name="do-i-have-to-turn-off-my-computer-for-the-remapping-to-take-effect"></a>Dois-je désactiver mon ordinateur pour que le remappage soit pris en compte ?

Non, le remappage doit se produire immédiatement quand vous appuyez sur **appliquer**.

### <a name="where-are-the-maclinux-profiles"></a>Où se trouvent les profils Mac/Linux ?

Actuellement, les profils Mac et Linux ne sont pas inclus.

### <a name="will-this-work-on-video-games"></a>Cela fonctionne-t-il sur les jeux vidéo ?

Cela dépend de la façon dont le jeu accède à vos clés. Certaines API de clavier ne fonctionnent pas avec le gestionnaire de clavier.

### <a name="will-remapping-work-if-i-change-my-input-language"></a>Remappera-t-il le travail si je change ma langue d’entrée ?

Oui. À ce stade, si vous remappez <kbd>un</kbd> à <kbd>B</kbd> sur le clavier anglais (US), puis que vous modifiez le paramètre de langue en français, si vous tapez <kbd>un</kbd> sur le clavier français (<kbd>Q</kbd> sur le clavier physique anglais des États-Unis), le résultat est <kbd>B</kbd>, ce qui est cohérent avec la manière dont Windows gère les entrées multilingues.

## <a name="troubleshooting"></a>Dépannage

Si vous avez essayé de remapper une clé ou un raccourci et que vous rencontrez des problèmes, il peut s’agir de l’un des problèmes suivants :

- **Exécuter en tant qu’administrateur :** Le remappage ne fonctionne pas dans une application/fenêtre si cette fenêtre s’exécute en mode administrateur (avec élévation de privilèges) et que les PowerToys ne s’exécutent pas en tant qu’administrateur. Essayez d’exécuter PowerToys en tant qu’administrateur.

- **Non-interception des clés :** Le gestionnaire de clavier intercepte des crochets de clavier pour remapper vos clés. Certaines applications qui le font également et peuvent interférer avec le gestionnaire de clavier. Pour résoudre ce problème, accédez aux paramètres et désactivez puis réactivez le gestionnaire de clavier.

## <a name="known-issues"></a>Problèmes connus

- [L’indicateur de lumière majuscule ne bascule pas correctement](https://github.com/microsoft/PowerToys/issues/1692)

- [Remappages non opérationnels pour FancyZones et le Guide de raccourci](https://github.com/microsoft/PowerToys/issues/3079)

- [Le remappage de clés comme Win, CTRL, ALT ou Maj peut rompre des mouvements et certains boutons spéciaux](https://github.com/microsoft/PowerToys/issues/3703)

Consultez la liste des [problèmes Open Keyboard Manager](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3A%22Product-Keyboard+Shortcut+Manager%22).
