---
title: Comparez les fonctionnalités de la plateforme sous iOS, Android et Windows 10.
description: Affichez une comparaison détaillée des concepts de développement et des fonctionnalités de plateforme entre iOS, Android et le plateforme Windows universelle (UWP) sur Windows 10.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 082736c8-2ac3-41b3-b246-e705edc23f34
ms.localizationpriority: medium
ms.openlocfilehash: 21cb4c105cc4c95c3a14c4c5bd0049265682f91c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220582"
---
# <a name="windows-apps-concept-mapping-for-android-and-ios-developers"></a>Mappage de concepts d’applications Windows pour développeurs iOS et Android

Si vous êtes un développeur doté de compétences relatives aux systèmes d’exploitation Android ou iOS et au code et que vous souhaitez migrer vers Windows 10 et la plateforme Windows universelle (UWP), cette ressource vous permettra de mapper les fonctionnalités de la plateforme, et vos connaissances, entre les trois plateformes.

Voir également le contenu de portage dans [Migrer d’iOS vers UWP](ios-to-uwp-root.md). Ce document est également disponible en [téléchargement](https://www.microsoft.com/download/details.aspx?id=52041).

## <a name="user-interface-ui"></a>Interface utilisateur (UI)


<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Langage de conception.</strong><br><br>Ensemble de conventions qui prévoient la manière dont les applications sur la plateforme doivent se présenter et se comporter.</td>
<td align="left">Les règles de <strong>conception de matériel Android</strong> fournissent un langage visuel pour les développeurs et les concepteurs Android.</td>
<td align="left">Les <strong>instructions relatives à l’interface humaine</strong> fournissent des conseils pour les développeurs et les concepteurs iOS.</td>
<td align="left">La <a href="https://developer.microsoft.com/windows/apps/design"><strong>conception d’applications Windows UWP</strong></a> vous montre comment créer une application qui semble fantastique sur tous les appareils Windows 10. Vous y trouverez des notions de base en matière de conception d’interface utilisateur (UI), des techniques de conception réactives et une liste complète de recommandations détaillées.<br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Langage de balisage de l’interface utilisateur.</strong> <br><br>Langage de balisage effectuant le rendu et décrivant une interface utilisateur et ses composants. Chaque plateforme fournit un éditeur à des fins de modification visuelle et du balisage.<br/></td>
<td align="left"><strong>Dispositions XML</strong>, modifiées à l’aide de <strong>Android Studio</strong> ou <strong>Eclipse</strong>.</td>
<td align="left"><strong>XIB</strong> et <strong>storyboards</strong> modifiés à l’aide de <strong>Interface Builder</strong> à l’intérieur de Xcode.</td>
<td align="left"><strong><a href="/windows/uwp/xaml-platform/xaml-overview">XAML</a></strong>, modifié à l’aide de <strong><a href="https://visualstudio.microsoft.com/">Microsoft Visual Studio</a></strong> et <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend pour Visual Studio</a></strong>.<br/><br/><a href="/windows/uwp/xaml-platform/index">Plateforme XAML</a><br/><br/><a href="/windows/uwp/design/basics/xaml-basics-ui">Créer une interface utilisateur avec XAML</a><br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Définir des dispositions avec XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Contrôles d’interface utilisateur intégrés.</strong> <br><br>Éléments d’interface utilisateur réutilisables fournis par la plateforme, tels que des boutons, des contrôles de liste et des contrôles de texte.</td>
<td align="left">Classes <strong>view</strong> et <strong>view group</strong> prédéfinies désignées sous les termes widgets, dispositions, champs de texte, conteneurs, contrôles date/heure et contrôles d’expert.</td>
<td align="left"><strong>Affichages</strong> et <strong>contrôles</strong> trouvés dans la bibliothèque d’objets Xcode et listés dans le catalogue de l’interface utilisateur uikit. Les vues comprennent les vues d’images, les vues de sélecteur et les vues de défilement. Les contrôles comprennent les boutons, les sélecteurs de dates et les champs de texte.</td>
<td align="left">La plateforme XAML propose un vaste ensemble de <strong>contrôles intégrés</strong>, tels que des boutons, des contrôles de liste, des panneaux, des contrôles de texte, des barres de commandes, des sélecteurs, du contenu multimédia et l’entrée manuscrite.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Ajouter des contrôles et gérer les événements</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Contrôler la gestion des événements.</strong> <br><br>Définition de la logique qui s’exécute lorsque des événements sont déclenchés dans des contrôles d’interface utilisateur.</td>
<td align="left">Les <strong>gestionnaires d’événements</strong> et les écouteurs d' <strong>événements</strong> sont ajoutés au format XML ou par programme.</td>
<td align="left">Les contrôles envoient des messages d’<strong>action</strong> aux <strong>cibles</strong>.</td>
<td align="left">Vous pouvez définir des méthodes pour gérer les événements d’un contrôle XAML dans un <strong>fichier code-behind</strong> attaché à la page XAML. Les <strong>gestionnaires d’événements</strong> sont toujours écrits dans le code. Toutefois, vous pouvez associer ces gestionnaires à des événements dans le balisage XAML ou le code.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Ajouter des contrôles et gérer les événements</a><br/><br/><a href="/windows/uwp/xaml-platform/events-and-routed-events-overview">Vue d’ensemble des événements et des événements routés</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Liaison de données.</strong> <br><br>Modèle de conception logicielle qui permet à l’interface utilisateur de votre application d’afficher des données et éventuellement de rester synchronisé avec celles-ci.</td>
<td align="left">Une <strong>bibliothèque de liaison de données</strong> est fournie, bien qu’elle soit toujours en version bêta.</td>
<td align="left">Il n’existe aucun système de liaison intégré sur iOS. L’observation de la <strong>valeur clé</strong> peut être basée sur pour effectuer la liaison de données, soit avec l’utilisation d’une bibliothèque tierce, soit en écrivant du code supplémentaire. Les contrôles utilisent une approche délégué/rappel pour l’obtention des données.</td>
<td align="left">La plateforme UWP traite pour vous la <strong>liaison de données</strong>. Vous utilisez l’extension de balisage <strong><a href="/windows/uwp/xaml-platform/x-bind-markup-extension">{x :bind}</a></strong> pour tirer parti de la liaison hautes performances ou <strong><a href="/windows/uwp/xaml-platform/binding-markup-extension">{Binding}</a></strong> pour tirer parti d’autres fonctionnalités. Il suffit alors de configurer votre liaison pour choisir si la plateforme utilise une <strong>liaison à sens unique</strong> pour afficher les valeurs provenant d’une source de données dans votre interface utilisateur, ou si elle observe ces valeurs et met à jour votre interface utilisateur lors du passage en <strong>liaison bidirectionnelle</strong>.<br/><br/><a href="/windows/uwp/data-binding/index">Liaison de données</a></td>
</tr>
<tr class="even">
<td align="left"><strong>UI Automation.</strong> <br><br>Accès par programmation aux éléments d’interface utilisateur, rendant les applications accessibles aux produits de technologie d’assistance et permettant aux scripts automatisés d’interagir avec votre interface utilisateur.</td>
<td align="left">Les <strong>étiquettes de texte</strong>, <strong>contentDescription</strong> et les valeurs d' <strong>indicateur</strong> permettent de s’assurer que les éléments d’interface utilisateur sont accessibles par Automation. Android Studio vous permet d’écrire des tests d’interface utilisateur à l’aide des infrastructures de test <strong>UI Automator</strong> et <strong>Espresso</strong>.</td>
<td align="left">L’<strong>instrument d’automatisation</strong> vous permet d’écrire des scripts de test d’interface utilisateur automatisés qui identifient les éléments à l’aide de paramètres d’<strong>accessibilité</strong> ou de la position de l’élément dans la <strong>hiérarchie d’éléments</strong>.</td>
<td align="left">Vous bénéficiez d’un accès par programmation aux éléments d’interface utilisateur intégrés dans la série UWP avec <strong><a href="/windows/desktop/WinAuto/uiauto-uiautomationoverview">UI Automation</a></strong>.<br/>Les <strong><a href="/windows/uwp/accessibility/custom-automation-peers">homologues Automation personnalisés</a></strong> vous permettent de fournir une prise en charge de l’automatisation pour vos propres classes d’interface utilisateur personnalisées. Le <strong><a href="/visualstudio/test/use-ui-automation-to-test-your-code?view=vs-2015">projet de test codé de l’interface utilisateur</a></strong> dans Visual Studio vous permet de tester automatiquement l’ensemble de votre application par le biais de l’interface utilisateur ou de tester l’interface utilisateur de manière isolée.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification de l’apparence d’un contrôle.</strong> <br><br>Modification de la taille, de la couleur et d’autres attributs.</td>
<td align="left">Les contrôles possèdent des <strong>propriétés</strong> qui peuvent être modifiées à l’aide de l’outil de conception, dans le balisage XML ou par programmation.</td>
<td align="left">Les contrôles possèdent des <strong>attributs</strong> que vous pouvez modifier à l’aide de l’<strong>Attributes Inspector</strong> dans Interface Builder ou par programmation.</td>
<td align="left">Vous pouvez modifier les <strong>propriétés</strong> des contrôles dans le balisage XAML ou par programmation, à l’aide de Visual Studio et de Blend pour Visual Studio.<br/><br/><a href="/windows/uwp/controls-and-patterns/controls-and-events-intro">Ajouter des contrôles et gérer les événements</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Styles visuels réutilisables.</strong> <br><br>Application de modifications visuelles à un certain nombre de contrôles, dans un format réutilisable.</td>
<td align="left">Les <strong>styles XML</strong> sont des ensembles de propriétés qui sont appliqués à un ou plusieurs contrôles.</td>
<td align="left">iOS ne prend pas en charge les styles visuels réutilisables fournis, toutefois le protocole UIAppearance permet à plusieurs contrôles de partager des attributs communs.</td>
<td align="left">Vous pouvez créer des <strong><a href="/uwp/api/Windows.UI.Xaml.Style">styles</a></strong>réutilisables, qui peuvent être appliqués à plusieurs contrôles et stockés dans un <strong><a href="/uwp/api/Windows.UI.Xaml.ResourceDictionary">ResourceDictionary</a></strong> pour faciliter leur réutilisation.<br/><br/><a href="/previous-versions/windows/apps/hh465381(v=win.10)">Démarrage rapide : application de styles aux contrôles (XAML)</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification de la structure visuelle des contrôles.</strong> <br><br>Personnaliser la structure visuelle d’un contrôle au-delà de la simple modification des propriétés ou des attributs, par exemple, en déplaçant le texte de la case à cocher sous la case à cocher.</td>
<td align="left">Il n’existe aucune méthode de modification simple de la structure visuelle des contrôles dans Android.</td>
<td align="left">Il n’existe aucune méthode de modification simple de la structure visuelle des contrôles dans iOS.</td>
<td align="left">Pour personnaliser la structure visuelle d’un contrôle, vous pouvez copier et modifier son <strong><a href="/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate">modèle de contrôle</a></strong> dans le balisage XAML.<br/><br/><a href="/previous-versions/windows/apps/hh465374(v=win.10)">Démarrage rapide : modèles de contrôle (XAML)</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Gestes tactiles intégrés.</strong> <br><br>Prise en charge personnalisée de la fonctionnalité tactile par la gestion des événements de mouvement abstraits de niveau élevé, tels que l’appui et le double appui dans les vues et les contrôles.</td>
<td align="left">Les <strong>détecteurs de mouvement</strong> détectent les gestes tactiles courants, notamment le défilement, la pression longue, le TAP, le double-tap et Fling.</td>
<td align="left">L’infrastructure UIKit fournit des <strong>outils de reconnaissance de mouvement</strong> qui détectent les mouvements tactiles, tels que l’appui, le pincement, le mouvement panoramique, le balayage, la rotation et l’appui prolongé.</td>
<td align="left">Les <strong>éléments d’interface utilisateur</strong> vous permettent de gérer les <strong>événements de mouvement statiques</strong> , notamment le TAP, le double-clic, le robinet et le maintien, ainsi que les <strong>événements de mouvement de manipulation</strong> , y compris la diapositive, le balayage, le tournage, le pincement et l’étirement. Les événements de mouvement sont des <strong>événements routés</strong> et peuvent être gérés par des objets parent contenant l’élément d’interface utilisateur enfant.<br/><br/><a href="/windows/uwp/input-and-devices/touch-interactions">Interactions tactiles</a><br/><br/><a href="/windows/uwp/design/layout/index">Interactions utilisateur personnalisées (mouvements, manipulations et interactions)</a></td>
</tr>
</tbody>
</table>
<h2 id="navigation-and-app-structure">Navigation et structure de l’application</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Dispositions.</strong> <br><br>La disposition définit la structure de l’interface utilisateur.</td>
<td align="left">La disposition se compose de <strong>groupes d’affichages</strong>, tels que <strong>LinearLayout</strong> et <strong>RelativeLayout</strong> qui peuvent imbriquer d’autres groupes d’affichages ou vues.</td>
<td align="left">La disposition est constituée d’un <strong>UIViewController</strong> contenant des <strong>UIView</strong> qui peuvent être imbriqués.</td>
<td align="left">XAML qui fournit un système de disposition flexible composé <strong>de classes de panneau de disposition</strong> telles que <strong><a href="/uwp/api/windows.ui.xaml.controls.canvas">Canvas</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.grid">Grid</a></strong>, <strong><a href="/uwp/api/windows.ui.xaml.controls.relativepanel">RelativePanel</a></strong> et <strong><a href="/uwp/api/windows.ui.xaml.controls.stackpanel">StackPanel</a></strong> pour des dispositions statiques et réactives. Les <strong><a href="/visualstudio/ide/reference/properties-window?view=vs-2015">Propriétés</a></strong> sont utilisées pour contrôler la taille et la position des éléments.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Définir des dispositions avec XAML</a><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigation homologue.</strong> <br><br>Présentation à l’utilisateur de méthodes de navigation entre les pages d’importance hiérarchique équivalente.</td>
<td align="left">Les <strong>onglets</strong>, les <strong>vues balayées</strong> et les <strong>tiroirs de navigation</strong> permettent une <strong>navigation latérale</strong>.</td>
<td align="left">Les contrôleurs de <strong>barre d’onglets</strong>, les contrôleurs de <strong>vue fractionnée</strong> et les <strong>contrôleurs de vue de page</strong> permettent de naviguer entre les vues de la même</td>
<td align="left">Vous pouvez afficher une liste permanente de liens/onglets au-dessus du contenu à l’aide d' <strong><a href="/windows/uwp/controls-and-patterns/tabs-pivot">onglets/de tableaux croisés dynamiques</a></strong>. Le <strong><a href="/windows/uwp/controls-and-patterns/split-view">volet de navigation/le mode fractionné</a></strong> vous permet d’afficher une liste de liens à côté du contenu.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Naviguer entre deux pages</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Navigation hiérarchique.</strong> <br><br>Navigation entre les pages parent et enfant d’une hiérarchie.</td>
<td align="left">Les <strong>listes</strong>et les <strong>listes de grilles</strong>, les <strong>boutons</strong> et autres contrôles fournissent une <strong>navigation descendante</strong> lorsqu’ils sont utilisés avec des <strong>intentions</strong> pour charger d’autres <strong>activités</strong>.</td>
<td align="left">Les <strong>contrôleurs de navigation</strong> permettent aux utilisateurs de naviguer entre les niveaux d’une hiérarchie.</td>
<td align="left">Les <strong><a href="/windows/uwp/controls-and-patterns/hub">concentrateurs</a></strong> vous permettent de montrer à l’utilisateur un aperçu du contenu qui peut être sélectionné pour accéder aux pages enfants. <strong><a href="/windows/uwp/controls-and-patterns/master-details">Maître/détails</a></strong> permet aux utilisateurs de choisir parmi une liste de résumés d’éléments qui s’affichent en regard de la section détail correspondante.<br/><br/><a href="/windows/uwp/layout/navigation-basics">Navigation</a><br/><br/><a href="/windows/uwp/layout/navigate-between-two-pages">Naviguer entre deux pages</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Navigation dans le bouton précédent.</strong> <br><br>Retour à une application.</td>
<td align="left">Les boutons <strong>précédent</strong> et <strong>haut</strong> situés dans la barre d’actions permettent une navigation par <strong>ancêtres</strong> et <strong>temporelle</strong> à l’aide de la <strong>pile Back</strong>.</td>
<td align="left">Le <strong>contrôleur de navigation</strong> peut se voir ajouter un bouton Précédent.<br/></td>
<td align="left">Vous pouvez facilement gérer les appuis sur les boutons matériels ou logiciels à l’aide de la <strong><a href="/uwp/api/windows.ui.xaml.controls.frame.backstack">propriété pile Back</a></strong> qui permet aux utilisateurs de traverser l’<strong>historique de navigation</strong>.<br/><br/><a href="/windows/uwp/layout/navigation-history-and-backwards-navigation">Navigation via le bouton Précédent</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Écran de démarrage.</strong> <br><br>Affiche d’une image au lancement de l’application, principalement utilisé pour la personnalisation.</td>
<td align="left">Les écrans de démarrage ne sont pas fournis par défaut et sont implémentés en modifiant l’arrière-plan du <strong>thème des premières activités</strong>.</td>
<td align="left">Les applications doivent disposer d’une <strong>image de lancement statique</strong> ou d’un <strong>fichier de lancement XIB/de table de montage séquentiel</strong>.</td>
<td align="left">Vous créez un écran de démarrage à l’aide d’une <strong>image</strong> et d’un arrière-plan coloré. L’heure de l' <a href="/windows/uwp/launch-resume/create-a-customized-splash-screen">écran de démarrage peut être étendue</a>.<br/><br/><a href="/windows/uwp/launch-resume/add-a-splash-screen">Ajouter un écran de démarrage</a></td>
</tr>
</tbody>
</table>
<h2 id="custom-inputs">Entrées personnalisées</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Facture.</strong> <br><br>Reconnaissance vocale pour la saisie vocale et fonctionnalités vocales supplémentaires.</td>
<td align="left">La saisie vocale peut être assurée par toute application implémentant une classe <strong>RecognizerIntent</strong>, tel que la <strong>recherche vocale Google</strong>. La classe <strong>SpeechRecognizer</strong> permet aux applications d’utiliser l’API de reconnaissance vocale de Google.</td>
<td align="left">Les applications peuvent utiliser la classe <strong>SFSpeechRecognizer</strong> pour implémenter la reconnaissance vocale et la reconnaissance vocale.</td>
<td align="left">Vous pouvez utiliser l’API de <strong><a href="/windows/uwp/input-and-devices/speech-recognition">reconnaissance vocale</a></strong> pour interagir avec votre application au premier plan. Vous pouvez utiliser les <strong><a href="/windows/uwp/input-and-devices/cortana-interactions">interactions Cortana</a></strong> basées sur la parole pour lancer des applications au premier plan ou en arrière-plan, et interagir avec les applications en arrière-plan.<br/><br/><a href="/windows/uwp/input-and-devices/speech-interactions">Interactions vocales</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Entrées utilisateur personnalisées.</strong> <br><br>Gestion du clavier, de la souris, du stylet et d’autres entrées.</td>
<td align="left">Prise en charge de l’interaction via la fonctionnalité <strong>tactile</strong>, le <strong>pavé tactile</strong>, le <strong>stylet</strong>, la <strong>souris</strong> et le <strong>clavier</strong>. Les mouvements et entrées sont considérés comme des mouvements tactiles, toutefois, il est possible de détecter plus d’informations concernant le <strong>périphérique d’entrée</strong>.</td>
<td align="left">La prise en charge de la fonctionnalité <strong>tactile</strong>, de l’<strong>Apple Pencil</strong> et du <strong>clavier</strong> est assurée.</td>
<td align="left">Vous trouverez la prise en charge d’une large gamme d’interactions, notamment la <strong><a href="/windows/uwp/input-and-devices/touch-interactions">saisie tactile</a></strong>, le <strong><a href="/windows/uwp/input-and-devices/touchpad-interactions">pavé tactile</a></strong>, le <strong><a href="/windows/uwp/input-and-devices/pen-and-stylus-interactions">stylet/stylet</a></strong> avec l’encre numérique, <strong><a href="/windows/uwp/input-and-devices/mouse-interactions">la souris</a></strong> et le <strong><a href="/windows/uwp/input-and-devices/keyboard-interactions">clavier</a></strong>. Vos applications peuvent gérer les données sans avoir besoin de savoir quel périphérique d’entrée a été utilisé, et les données de périphérique d’entrée brutes sont accessibles si nécessaire.<br/><br/><a href="/windows/uwp/input-and-devices/handle-pointer-input">Gestion des entrées du pointeur</a><br/><br/><a href="/windows/uwp/design/layout/index">Interactions utilisateur personnalisées</a></td>
</tr>
</tbody>
</table>
<h2 id="data">Données</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Données de l’application locale.</strong> <br><br>Stockage local des paramètres et des fichiers associés à votre application.</td>
<td align="left">Les fichiers locaux peuvent être enregistrés à l’aide de <strong>openFileOutput</strong> et <strong>openFileInput</strong>. Les paramètres d’un <strong>fichier de préférences partagé</strong> sont accessibles à l’aide de <strong>getSharedPreferences</strong>.</td>
<td align="left">Les fichiers locaux peuvent être stockés dans le répertoire <strong>prise en charge de l’application</strong>, accessible via la classe <strong>NSFileManager</strong>. Les paramètres des fichiers de <strong>préférences</strong> sont accessibles via la classe <strong>NSUserDefaults</strong>.</td>
<td align="left">Les classes <strong><a href="/uwp/api/Windows.Storage">Windows. Storage</a></strong> gèrent le stockage de données local pour vous de manière unifiée. Vous stockez les paramètres en tant qu’objet <strong><a href="/uwp/api/Windows.Storage.ApplicationDataContainer">ApplicationDataContainer</a></strong> , accessible via la propriété <strong><a href="/uwp/api/windows.storage.applicationdata.localsettings">ApplicationData. LocalSettings</a></strong> . Vous stockez les fichiers dans un objet <strong><a href="/uwp/api/windows.storage.storagefolder">StorageFolder</a></strong> accessible via la propriété <strong><a href="/uwp/api/windows.storage.applicationdata.localfolder">ApplicationData. LocalFolder</a></strong> .<br/><br/><a href="/windows/uwp/app-settings/store-and-retrieve-app-data">Stocker et récupérer des paramètres et autres données d’application</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Stockage de base de données local.</strong> <br><br>Stockage des données d’application dans une base de données relationnelle, avec ORM, le cas échéant.</td>
<td align="left">La base de données <strong>SQLite</strong> est fournie. Aucun ORM n’est intégré. Les requêtes SQL sont exécutées à l’aide de la classe <strong>SQLiteDatabase</strong>.</td>
<td align="left">La base de données <strong>SQLite</strong> est fournie. <strong>CoreData</strong> est l’infrastructure graphique d’objets intégrée qui peut être utilisée avec SQLite et fournir des fonctionnalités comparables à un ORM.</td>
<td align="left">Vous pouvez stocker des données à l’aide <strong>SQLite</strong>. <strong><a href="/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps">Entity Framework</a></strong> est un ORM intégré qui élimine la nécessité d’écrire un grand nombre de code d’accès aux données et vous permet d’interroger facilement la base de données sans écrire SQL. Vous pouvez exécuter des requêtes SQL directement avec la <a href="/windows/uwp/data-access/sqlite-databases">bibliothèque SQLite</a>.<br/><br/><a href="/windows/uwp/data-access/index">Accès aux données</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Bibliothèques HTTP pour l’accès REST.</strong> <br><br>Bibliothèques intégrées qui vous permettent de communiquer avec les services web et les serveurs web à l’aide du protocole HTTP(S).<br/></td>
<td align="left">Bibliothèques HTTP <strong>HttpURLConnection</strong> et <strong>Volley</strong>.</td>
<td align="left"><strong>Passer</strong>, <strong>NSURLConnection</strong> et <strong>NSURLDownload</strong>.</td>
<td align="left">Vous pouvez utiliser l’API <strong><a href="/uwp/api/Windows.Web.Http.HttpClient">httpclient</a></strong> intégrée pour accéder aux fonctionnalités http courantes, notamment les informations d’extraction, de suppression, put, de publication, les modèles d’authentification courants, le protocole SSL, les cookies et les informations de progression.</td>
</tr>
<tr class="even">
<td align="left"><strong>Services de sauvegarde Cloud.</strong> <br><br>Services de sauvegarde fournis par la plate-forme pour les données d’application.</td>
<td align="left">Le <strong>gestionnaire de sauvegarde</strong> d’Android gère la sauvegarde des données d’application dans le <strong>service de sauvegarde Android</strong> de Google.</td>
<td align="left">la <strong>sauvegarde icloud</strong> peut être configurée par un utilisateur pour gérer ses sauvegardes, y compris les données d’application. Les applications utilisant des <strong>données principales</strong> compatibles iCloud, le <strong>magasin clé-valeur iCloud</strong> et le <strong>stockage de documents iCloud</strong>.</td>
<td align="left">Toutes les données d’application que vous stockez à l’aide des <strong><a href="/uwp/api/windows.storage.applicationdata">API ApplicationData</a></strong> itinérantes (y compris <strong><a href="/uwp/api/windows.storage.applicationdata.roamingfolder">RoamingFolder</a></strong> et <a href="/uwp/api/windows.storage.applicationdata.roamingsettings"><strong>RoamingSettings</strong></a>) sont automatiquement synchronisées dans le Cloud et sur les autres appareils de l’utilisateur. La synchronisation s’effectue par le biais du compte Microsoft de l’utilisateur.<br/><br/><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">Recommandations en matière de données d’application itinérantes</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Téléchargements de fichiers HTTP.</strong> <br><br>Téléchargement de fichiers de petite et grande taille sur HTTP.</td>
<td align="left"><strong>URLConnection</strong> et <strong>HTTPURLConnection</strong> sont utilisés pour le téléchargement via http et FTP. il est également possible d’utiliser le gestionnaire de <strong>Téléchargement</strong> système pour le téléchargement en arrière-plan.</td>
<td align="left"><strong>Passer</strong> et <strong>NSURLConnection</strong> peuvent être utilisés pour télécharger des fichiers via http et FTP.</td>
<td align="left">L' <strong><a href="/uwp/api/windows.networking.backgroundtransfer">API de transfert en arrière-plan</a></strong> vous permet de transférer de manière fiable des fichiers sur http (S) et FTP, en tenant compte de la suspension de l’application, de la perte de connectivité et de l’ajustement en fonction de la connectivité et de la durée Vous pouvez également utiliser <strong><a href="/uwp/api/windows.web.http.httpclient">httpclient</a></strong> qui est idéal pour les fichiers plus petits.<br/><br/><a href="/windows/uwp/networking/which-networking-technology">Quelle technologie de réseau ?</a><br/><br/><a href="/windows/uwp/networking/background-transfers">Transferts en arrière-plan</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Sockets.</strong> <br><br>Création de datagrammes UDP et de sockets TCP de faible niveau pour communiquer avec d’autres appareils à l’aide de votre propre protocole.</td>
<td align="left">La classe <strong>Socket</strong> fournit des sockets TCP, la classe <strong>DatagramSocket</strong> fournit un socket UDP.</td>
<td align="left"><strong>NSStream</strong> et <strong>CFStream</strong> fournissent des sockets TCP, <strong>CFSOCKET</strong> fournit des sockets UDP.</td>
<td align="left">Vous pouvez utiliser la classe <strong><a href="/uwp/api/Windows.Networking.Sockets.DatagramSocket">DatagramSocket</a></strong> pour communiquer à l’aide d’un socket de datagramme UDP et de la classe <strong><a href="/uwp/api/Windows.Networking.Sockets.StreamSocket">StreamSocket</a></strong> pour communiquer via TCP ou Bluetooth RFCOMM.<br/><br/><a href="/windows/uwp/networking/networking-basics">Notions de base en matière de réseau</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">Quelle technologie de réseau ?</a><br/><br/><a href="/windows/uwp/networking/sockets">Vue d’ensemble de sockets</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>WebSockets.</strong> <br><br>Assurer une communication bidirectionnelle entre un client et un serveur, ce qui permet le transfert de données en temps réel.</td>
<td align="left">Aucune bibliothèque WebSockets intégrée n’existe sur Android.</td>
<td align="left">Aucune bibliothèque WebSockets intégrée n’existe sur iOS.</td>
<td align="left">Il est possible d’établir des connexions sécurisées aux serveurs qui prennent en charge WebSockets avec la classe <strong><a href="/uwp/api/windows.networking.sockets.messagewebsocket">MessageWebSocket</a></strong> pour les messages plus petits avec notifications de réception et <strong><a href="/uwp/api/windows.networking.sockets.streamwebsocket">StreamWebSocket</a></strong> pour les transferts de fichiers binaires plus volumineux qui peuvent être lus dans les sections.<br/><br/><a href="/windows/uwp/networking/networking-basics">Notions de base en matière de réseau</a><br/><br/><a href="/windows/uwp/networking/which-networking-technology">Quelle technologie de réseau ?</a><br/><br/><a href="/windows/uwp/networking/websockets">Vue d’ensemble de WebSockets</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Bibliothèques OAuth.</strong> <br><br>Bibliothèques OAuth permettant d’accéder à des fournisseurs OAuth tiers et à toute gestion de compte intégrée à la plateforme.</td>
<td align="left">Aucune bibliothèque OAuth générique n’est fournie. La classe <strong>GoogleAuthUtil</strong> est fournie pour l’authentification OAuth avec les Services Google Play.<br/></td>
<td align="left">Aucune bibliothèque OAuth générique n’est fournie. L’<strong>infrastructure des comptes</strong> permet d’accéder aux comptes d’utilisateurs déjà stockés sur l’appareil, tels que Facebook et Twitter.</td>
<td align="left">Le <strong><a href="/windows/uwp/security/web-authentication-broker">Service Broker d’authentification Web</a></strong> de la bibliothèque OAuth générique vous permet de vous connecter à des services de fournisseur d’identité tiers. Le dispositif de <strong><a href="/windows/uwp/security/credential-locker">verrouillage des informations d’identification</a></strong> permet à vos utilisateurs d’enregistrer leur connexion et de les utiliser sur plusieurs appareils. L’espace de noms <strong><a href="/previous-versions/office/developer/onedrive-live-sdk-reference/dn896755(v=office.15)">Microsoft. Live</a></strong> vous permet d’accéder facilement au kit de développement logiciel (SDK) Live pour l’accès aux services Microsoft.<br/><br/><a href="/windows/uwp/security/authentication-and-user-identity">Authentification et identité des utilisateurs</a><br/><br/><a href="/uwp/api/windows.security.authentication.web">Documentation sur l’API Windows.Security.Authentication.Web</a><br/><br/><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker">Exemple de code WebAuthenticationBroker</a></td>
</tr>
</tbody>
</table>
<h2 id="tooling">Outillage</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>IDE.</strong> <br><br>Ensemble d’outils utilisé pour créer votre application.</td>
<td align="left"><strong>Android Studio</strong> et <strong>Eclipse</strong>, avec Google pousse les développeurs à utiliser des Android Studio.</td>
<td align="left"><strong>Xcode</strong></td>
<td align="left"><strong><a href="https://visualstudio.microsoft.com/features/universal-windows-platform-vs">Visual Studio</a></strong> et <strong><a href="/visualstudio/designers/creating-a-ui-by-using-blend-for-visual-studio?view=vs-2015">Blend pour Visual Studio</a></strong> dispose de tous les outils dont vous avez besoin pour coder, concevoir, connecter, déboguer, analyser, optimiser et tester des applications UWP. Visual Studio fournit également des <strong><a href="/windows/uwp/debug-test-perf/test-with-the-emulator">émulateurs</a></strong> pour les appareils Windows 10, ce qui vous permet de tester votre application sur une plage d’appareils émulés.<br/><br/><a href="https://developer.microsoft.com/windows/downloads">Téléchargements et outils pour UWP</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Organisation du code.</strong> <br><br>Structure de dossiers de base d’une application, souvent créée à partir d’un modèle d’origine.</td>
<td align="left"><strong>Fichier fichier AndroidManifest</strong> , dossier <strong>java</strong> contenant les fichiers sources, dossier <strong>res</strong> avec les ressources, notamment les mises en page et les valeurs, <strong>Gradle</strong> générer des scripts dans Android Studio et des scripts de génération <strong>ant</strong> dans Eclipse.</td>
<td align="left">Fichiers sources et <strong>fichiers de prise en charge</strong>, fichier <strong>Info.plist</strong>, <strong>Main.storyboard</strong> et <strong>LaunchScreen.storyboard</strong>. Les images sont stockées dans les <strong>bibliothèques de ressources</strong>.</td>
<td align="left">Votre application UWP contient des fichiers XAML et de code pour votre application intitulée Example.xaml et Example.xaml.cs, différentes images dans le <strong>dossier de ressources</strong>, une page de démarrage comme <strong>MainPage.xaml</strong> et <strong>MainPage.xaml.cs</strong> et un manifeste.<br/><br/><a href="/windows/uwp/get-started/create-a-hello-world-app-xaml-universal">Créer une application Hello World</a></td>
</tr>
</tbody>
</table>
<h2 id="app-lifecycle">Cycle de vie de l’application</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Cycle de vie des applications.</strong> <br><br>Gestion des événements lors du lancement, de la suspension, de la reprise et de la fermeture d’une application permettant d’enregistrer/de restaurer l’état de l’application et d’exécuter d’autres tâches.</td>
<td align="left">Chaque activité possède son propre <strong>cycle de vie d’activité</strong> avec des états de type <strong>reprise</strong>. Les <strong>rappels de cycle de vie</strong> , tels que <strong>onResume</strong> , sont implémentés dans vos <strong>classes d’activité</strong>.</td>
<td align="left">Le <strong>cycle de vie de l’application</strong> peut avoir des états de type <strong>suspendue</strong>. Les méthodes telles que <strong>applicationDidEnterBackground:</strong> sont implémentées dans l’<strong>objet délégué d’application</strong> pour exécuter du code lors d’un changement d’état.</td>
<td align="left">Votre application dispose des <strong>états d’exécution</strong> suivants : NotRunning, Activated, Running, Suspending, Suspended et Resuming.<br/><br/>Vous pouvez implémenter les méthodes de <strong><a href="/uwp/api/windows.ui.xaml.application">classe d’application</a></strong> OnLaunched, OnActivated, suspension ou reprise dans votre application pour exécuter le code lorsque l’état change.<br/><br/><a href="/windows/uwp/launch-resume/app-lifecycle">Cycle de vie de l’application</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Tâches en arrière-plan.</strong> <br><br>Tâches qui effectuent des opérations en arrière-plan et qui continuent à s’exécuter lorsque l’application n’est plus au premier plan.</td>
<td align="left">Les applications peuvent lancer des <strong>services</strong> qui effectuent des opérations en arrière-plan lorsque l’application n’est plus au premier plan. Les services ont leur propre <strong>cycle de vie</strong> et sont enregistrés dans le manifeste.</td>
<td align="left"><strong>L’exécution en arrière-plan</strong> est autorisée uniquement pour des types de tâches spécifiques.<br/><br/>Les applications déclarent les<strong> tâches en arrière-plan prises en charge</strong> dans le fichier Info.plist à l’aide de <strong>UIBackgroundModes</strong>.<br/><br/>Le système contrôle l’exécution des tâches en arrière-plan et leur durée.</td>
<td align="left">Vous pouvez créer une tâche en arrière-plan en implémentant l’interface <strong><a href="/uwp/api/windows.applicationmodel.background.ibackgroundtask">IBackgroundTask</a></strong> et en inscrivant la tâche dans le manifeste de l’application. Vous pouvez définir une tâche à déclencher à l’aide d' <a href="/windows/uwp/launch-resume/run-a-background-task-on-a-timer-"><strong>un</strong></a>déclencheur, d’un <a href="/uwp/api/windows.applicationmodel.background.systemtriggertype"><strong>déclencheur système</strong></a>et d’un <a href="/windows/uwp/launch-resume/use-a-maintenance-trigger"><strong>déclencheur de maintenance</strong></a>.<br/><br/><a href="/windows/uwp/launch-resume/support-your-app-with-background-tasks">Prendre en charge votre application avec des tâches en arrière-plan</a><br/><br/><a href="/windows/uwp/launch-resume/create-and-register-a-background-task">Créer et inscrire une tâche en arrière-plan</a><br/><br/><a href="/windows/uwp/launch-resume/guidelines-for-background-tasks">Recommandations relatives aux tâches en arrière-plan</a></td>
</tr>
</tbody>
</table>
<h2 id="performance">Performances</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Meilleures pratiques en matière de performances.</strong> <br><br>Recommandations en matière de création d’applications rapides, réactives, préservant l’autonomie de la batterie et offrant un démarrage rapide.</td>
<td align="left">Android fournit le guide de formation <strong>Meilleures pratiques en matière de performances</strong>.</td>
<td align="left">iOS fournit le document <strong>Vue d’ensemble des performances</strong>.</td>
<td align="left">Vous pouvez lire le guide détaillé sur les <strong><a href="/windows/uwp/debug-test-perf/performance-and-xaml-ui">performances</a></strong> avec des sections couvrant des sujets tels que ; définition des objectifs de performances, mesure des performances, gestion de la mémoire, animations lisses, accès efficace au système de fichiers et outils disponibles pour le profilage et les performances.</td>
</tr>
<tr class="even">
<td align="left"><strong>Afficher l’optimisation d’une interface utilisateur réactive.</strong> <br><br>Amélioration des performances par l’optimisation des vues.</td>
<td align="left">L’optimisation des <strong>hiérarchies de disposition</strong> à l’aide de l’outil Visionneuse de la hiérarchie, la <strong>réutilisation des dispositions</strong> et le chargement des <strong>vues à la demande</strong>, sont des techniques qui permettent d’assurer la réactivité du thread et d’éviter les boîtes de dialogue &quot;L’application ne répond pas&quot; (<strong>ANR</strong>).<br/></td>
<td align="left">La résolution des problèmes liés à l’interface utilisateur avec le <strong>rendu hors écran</strong>, le <strong>mélange des couches</strong> et la <strong>rastérisation</strong> à l’aide de l’outil <strong>Core Animation</strong> assure la réactivité du thread de l’interface utilisateur.</td>
<td align="left">Vous pouvez facilement <strong>optimiser</strong> le <strong>balisage</strong> et les <strong>dispositions</strong> XAML en suivant quelques étapes simples. Les techniques incluent la réduction de la structure de la disposition, du nombre d’éléments et du surdessin. <br/><br/><a href="/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive">Assurer la réactivité du thread de l’interface utilisateur</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-xaml-loading">Optimiser votre balisage XAML</a><br/><br/><a href="/windows/uwp/debug-test-perf/optimize-your-xaml-layout">Optimiser votre disposition XAML</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Threading.</strong> <br><br>Utilisation de threads pour assurer la <strong>réactivité de l’interface utilisateur</strong> et exécuter plusieurs <strong>tâches en parallèle</strong>.</td>
<td align="left">Les threads sont obtenus à l’aide des classes <strong>Runnable</strong>, <strong>Handler</strong>, <strong>ThreadPoolExecutor</strong> et <strong>AsyncTask</strong> de niveau supérieur.</td>
<td align="left">Les threads sont obtenus à l’aide des classes <strong>NSThread</strong>, <strong>Grand Central Dispatch</strong> et <strong>NSOperation</strong> de niveau supérieur.</td>
<td align="left">Vous pouvez utiliser des threads en envoyant des <strong>éléments de travail</strong> au <strong>ThreadPool</strong> avec <strong><a href="/uwp/api/windows.system.threading.threadpool.runasync">RunAsync</a></strong>. Vous pouvez utiliser un minuteur pour envoyer un élément de travail avec <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createtimer">CreateTimer</a></strong> et créer un élément de travail répétitif avec <strong><a href="/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer">CreatePeriodicTimer</a></strong>.<br/><br/><a href="/windows/uwp/threading-async/submit-a-work-item-to-the-thread-pool">Envoyer un élément de travail au pool de threads</a><br/><br/><a href="/windows/uwp/threading-async/use-a-timer-to-submit-a-work-item">Utiliser un minuteur pour envoyer un élément de travail</a><br/><br/><a href="/windows/uwp/threading-async/create-a-periodic-work-item">Créer un élément de travail périodique</a><br/><br/><a href="/windows/uwp/threading-async/best-practices-for-using-the-thread-pool">Meilleures pratiques pour l’utilisation du pool de threads</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Programmation asynchrone.</strong> <br><br>Éviter la complexité des threads en tirant parti des modèles de programmation asynchrones pour garantir la réactivité du thread de l’interface utilisateur.</td>
<td align="left">L’utilisation des <strong>threads est requise</strong> pour créer vos propres classes asynchrones. Certaines classes intégrées sont asynchrones.</td>
<td align="left">L’utilisation des <strong>threads est requise</strong> pour créer vos propres classes asynchrones. Certaines classes intégrées sont asynchrones.</td>
<td align="left">Vous pouvez utiliser des modèles asynchrones pour éviter de bloquer le thread principal lorsque vous créez vos propres API, par exemple en utilisant <strong>Async</strong> et <strong>await</strong> en C# et Visual Basic. Vous pouvez utiliser les API asynchrones intégrées qui se terminent par le mot <strong>Async</strong>.<br/><br/><a href="/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Programmation asynchrone</a><br/><br/><a href="/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic">Appeler des API asynchrones en C# ou Visual Basic</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Optimisation de la vue liste.</strong> <br><br>Modèles intégrés permettant d’optimiser des listes de données, qui sont souvent peu performantes pour l’affichage de grandes quantités de données</td>
<td align="left">Le modèle de conception <strong>ViewHolder</strong> est utilisé pour éviter les multiples recherches de vues, et vous permet d’utiliser des éléments d’interface utilisateur réutilisables.</td>
<td align="left">Diverses optimisations peuvent être effectuées pour améliorer les performances de <strong>UITableView</strong>. Rien n’est intégré.</td>
<td align="left">Vous pouvez utiliser les contrôles <a href="/uwp/api/windows.ui.xaml.controls.listview">ListView</a> et <a href="/uwp/api/windows.ui.xaml.controls.gridview">GridView</a> qui assurent la <strong>virtualisation de l’interface utilisateur</strong> offrant un mouvement panoramique fluide, une expérience de défilement expérience et un démarrage plus rapide. Vous pouvez également implémenter <a href="/dotnet/api/system.collections.ilist">IList</a> et <a href="/dotnet/api/system.collections.specialized.inotifycollectionchanged">INotifyCollectionChanged</a> dans votre source de données, pour <strong>virtualiser les données</strong> et améliorer les performances.<br/><br/><a href="/windows/uwp/debug-test-perf/optimize-gridview-and-listview">Optimisation des options d’interface ListView et GridView</a><br/><br/><a href="/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization">Virtualisation des données ListView et GridView</a></td>
</tr>
</tbody>
</table>
<h2 id="monetization">Monétisation</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Achats dans l’application.</strong> <br><br>Fonctionnalités de la plateforme permettant aux utilisateurs d’effectuer des achats dans vos applications.</td>
<td align="left">La <strong>facturation dans l’application</strong> est fournie par Google services. Les produits sont ajoutés à la <strong>Console développeur de Google Play</strong>. Les achats dans l’application sont implémentés à l’aide de la <strong>bibliothèque de facturation Google Play</strong>.</td>
<td align="left">Les produits sont ajoutés à <strong>iTunes Connect</strong>. Les achats dans l’application sont implémentés à l’aide de l’infrastructure <strong>StoreKit</strong>.<br/><br/>Les produits sont achetés à l’aide de <strong>SKMutablePayment</strong> et <strong>SKPaymentQueue</strong>.</td>
<td align="left">Vous créez des achats de produits in-app pour votre application en les <a href="/windows/uwp/publish/iap-submissions">ajoutant à votre application et les soumettant au Windows Store</a>. <br/><br/>Vous utilisez la <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp">classe CurrentApp</a></strong> pour définir des achats dans l’application. <br/><br/>Vous utilisez <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync">CurrentApp. RequestProductPurchaseAsync</a></strong> pour afficher l’interface utilisateur qui permet aux clients d’acheter le produit.<br/><br/><a href="/windows/uwp/monetize/enable-in-app-product-purchases">Activer les achats de produits in-app</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Achats dans l’application consommables.</strong> <br><br>Produits intégrés à l’application pouvant être achetés, utilisés, puis de nouveau achetés.</td>
<td align="left">Les achats de consommables sont activés par un achat régulier, puis sa consommation à l’aide de <strong>consumePurchase</strong>, qui lui permet d’être acheté, utilisé, puis acheté de nouveau.</td>
<td align="left">Les produits consommables sont <strong>définis sous forme de produits consommables</strong> dans iTunes Connect.</td>
<td align="left">Vous pouvez prendre en charge des consommables en <a href="/windows/uwp/publish/enter-iap-properties">définissant leur type de produit sur Consommable lorsque vous les envoyez au</a> Windows Store. Vous appelez ensuite <strong><a href="/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync">CurrentApp. ReportConsumableFulfillmentAsync</a></strong> après l’achat d’un consommable pour permettre au client d’y accéder.<br/><br/><a href="/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Activer les achats de consommables dans l’application</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Test des achats dans l’application.</strong> <br><br>Permet de tester le code d’achat dans l’application sans placer votre application dans le Windows Store.</td>
<td align="left">Le <strong>bac à sable de facturation dans l’application</strong> est utilisé pour le test.</td>
<td align="left">Les <strong>comptes de testeur sandbox</strong> sont utilisés à des fins de test.</td>
<td align="left">Vous pouvez tester des achats dans l’application en utilisant simplement la classe <strong><a href="/uwp/api/windows.applicationmodel.store.currentappsimulator">CurrentAppSimulator</a></strong> à la place de CurrentApp.<br/><br/></td>
</tr>
<tr class="even">
<td align="left"><strong>Cliniques.</strong> <br><br>Permet de limiter facilement le contenu ou de supprimer des publicités en fonction de la version d’évaluation d’une application.</td>
<td align="left">Google Play <strong>ne prend pas en charge officiellement les versions d’évaluation</strong>. Les essais gratuits ou la suppression des publicités s’obtient en créant un achat dans l’application et en prenant le chemin d’accès au code approprié lors de la confirmation de la réussite de l’achat.</td>
<td align="left">L’App Store <strong>ne prend pas en charge officiellement les essais gratuits d’applications</strong>. Les essais gratuits ou la suppression des publicités s’obtient en créant un achat dans l’application et en prenant le chemin d’accès au code approprié lors de la confirmation de la réussite de l’achat.</td>
<td align="left">Vous pouvez proposer une version d’évaluation gratuite de votre application à l’aide de l' <strong><a href="/windows/uwp/publish/set-app-pricing-and-availability">option « version d’évaluation gratuite »</a></strong> lors de la soumission de votre application au Windows Store. Vous pouvez ensuite utiliser <strong><a href="/uwp/api/windows.applicationmodel.store.licenseinformation.istrial">LicenseInformation. IsTrial</a></strong> pour vérifier l’état d’évaluation de l’application et présenter les différents chemins de code en conséquence. Vous pouvez vous inscrire à l’<a href="/uwp/api/windows.applicationmodel.store.licenseinformation.licensechanged">événement LicenseChanged</a> pour être averti lorsque l’utilisateur modifie l’état de l’essai lors de l’exécution de l’application.<br/><br/><a href="/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app">Exclure ou limiter des fonctionnalités de la version d’évaluation</a></td>
</tr>
</tbody>
</table>
<h2 id="adapting-to-multiple-platforms">Adaptation aux différentes plateformes</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface utilisateur adaptative : dispositions flexibles.</strong> <br><br>Prise en charge de différentes tailles d’écran avec une hauteur et une largeur flexibles.</td>
<td align="left">Des dispositions flexibles peuvent être obtenues à l’aide des valeurs <strong>wrap_content</strong> et <strong>match_parent</strong> des objets LinearLayout, ou en utilisant des objets RelativeLayout pour l’alignement.</td>
<td align="left">Des dispositions flexibles peuvent être obtenues à l’aide du <strong>modèle adaptatif</strong> avec des tables de montage séquentiel universelles, en utilisant la <strong>Disposition automatique</strong> avec des <strong>contraintes</strong> et des <strong>caractéristiques</strong> telles que horizontalSizeClass et displayScale qui sont appliquées aux contrôleurs d’affichage.</td>
<td align="left">Vous pouvez créer une disposition fluide à l’aide de <strong>propriétés</strong> et de <strong>panneaux de disposition</strong> avec une combinaison de dimensionnement fixe et dynamique.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Définir des dispositions avec XAML - Propriétés et panneaux de disposition</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Conception dynamique 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Interface utilisateur adaptative : dispositions personnalisées.</strong> <br><br>Prise en charge de différentes tailles d’écran avec des dispositions ciblées distinctes.</td>
<td align="left">Le fait de fournir d’autres fichiers de disposition pour différentes configurations d’écran dans le répertoire de ressources utilisant des <strong>qualificateurs de configuration</strong> tels que <strong>small</strong>, <strong>large</strong>, <strong>ldpi</strong> et <strong>hdpi</strong> vous permet de cibler des dispositions personnalisées pour les écrans de différentes tailles et densités.</td>
<td align="left">Définissez une <strong>table de montage séquentiel pour iPhone et iPad distincte</strong> afin d’adapter les dispositions à différentes familles d’appareils dans une application universelle.</td>
<td align="left">Vous pouvez créer une disposition personnalisée en définissant <strong>différents fichiers de balisage XAML</strong> par famille d’appareils.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Définir des dispositions avec XAML - Dispositions personnalisées</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Interface utilisateur adaptative : dispositions réactives.</strong> <br><br>Réponse aux changements de taille d’écran, par exemple, à la rotation, ou à une modification de la taille d’une fenêtre.</td>
<td align="left">L’utilisation de dispositions flexibles avec <strong>LinearLayout</strong> et <strong>RelativeLayout</strong>, ou le fait de proposer d’autres fichiers de disposition pour différentes orientations offre des dispositions dynamiques.</td>
<td align="left">Lors du changement de la <strong>taille</strong> ou des <strong>caractéristiques</strong> d’une vue, les <strong>contraintes</strong> spécifiées dans les tables de montage séquentiel s’appliquent.</td>
<td align="left">Vous pouvez facilement redistribuer, repositionner, redimensionner, afficher ou remplacer des sections de votre interface utilisateur au moment de l’exécution en réponse aux modifications de la taille de la fenêtre à l’aide de <strong><a href="/uwp/api/windows.ui.xaml.visualstate">VisualState</a></strong>, de <strong><a href="/uwp/api/windows.ui.xaml.visualstatemanager">VisualStateManager</a></strong> et de <strong><a href="/uwp/api/windows.ui.xaml.adaptivetrigger">AdaptiveTrigger</a></strong>.<br/><br/><a href="/windows/uwp/layout/layouts-with-xaml">Définir des dispositions avec XAML - États visuels et déclencheurs d’état</a><br/><br/><a href="/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Conception dynamique 101</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Prise en charge de différentes fonctionnalités d’appareil.</strong> <br><br>Exploitation des fonctionnalités matérielles avancées tout en assurant la prise en charge des appareils qui n’en sont pas dotés.</td>
<td align="left">Le fait de tester des fonctionnalités d’appareil au moment de l’exécution à l’aide de <strong>PackageManager.hasSystemFeature</strong> vous permet de déterminer si un code spécifique d’un matériel peut s’exécuter.</td>
<td align="left">Il n’existe <strong>aucune vérification unique</strong> à effectuer lors de l’exécution pour tester les fonctionnalités d’un appareil. Chaque fonctionnalité est testée de manière spécifique pour déterminer si le code propre à un matériel peut être exécuté.</td>
<td align="left">Vous pouvez ajouter des <strong>Kits de développement logiciel (SDK) d’extension de plate-forme</strong> à votre package pour cibler les fonctionnalités supplémentaires trouvées dans différentes familles d’appareils : téléphone, ordinateur de bureau et IoT. Vous utilisez l' <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> pour tester la présence de types et de membres au moment de l’exécution, et vous pouvez appeler ces types et membres uniquement s’ils sont présents.</td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Prise en charge de différentes fonctionnalités d’appareil.</strong> <br><br>Exploitation des fonctionnalités matérielles avancées tout en assurant la prise en charge des appareils qui n’en sont pas dotés.</td>
<td align="left">La <strong>bibliothèque de prise en charge Android</strong> peut être fournie avec votre application afin de mettre à disposition quelques nouvelles API aux utilisateurs d’anciennes versions d’Android. Le test du niveau d’API lors de l’exécution peut être effectué à l’aide de <strong>Build.Version.SDK_INT</strong>.</td>
<td align="left">Les vérifications à l’exécution standard permettent de déterminer si des API sont disponibles, comme la méthode <strong>class</strong> qui vérifie l’existence d’une classe et <strong>respondsToSelector:</strong> qui recherche les méthodes sur les classes.</td>
<td align="left">Vous pouvez utiliser <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent">ApiInformation. IsApiContractPresent</a></strong> pour déterminer si un contrat d’API avec un nombre majeur et secondaire spécifié est présent. Vous pouvez également utiliser l' <strong><a href="/uwp/api/windows.foundation.metadata.apiinformation">API ApiInformation</a></strong> pour tester la présence de types et de membres au moment de l’exécution et appeler ces types et membres uniquement s’ils sont présents.</td>
</tr>
</tbody>
</table>
<h2 id="notifications">Notifications</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Vignettes et badges.</strong> <br><br>Présentation des mises à jour aux utilisateurs sur l’écran d’accueil.</td>
<td align="left">Les <strong>widgets</strong> d’application sont des vues de votre application qui peuvent être incorporées dans l’écran d’accueil et peuvent recevoir des mises à jour périodiques. <strong>Aucun système de badge n'</strong> existe sur Android. Il n’existe aucun système identique aux vignettes.</td>
  <td align="left">Les <strong>widgets</strong> sur iOS mappear dans le centre de notifications et sont implémentés en tant qu' <strong>extensions d’application</strong>. Vous pouvez également ajouter un <strong>badge</strong> à votre icône avec un numéro qui peut changer en réponse à des notifications locales ou distantes. Il n’y a aucun système de vignettes.</td>
<td align="left">Votre application dispose d’une <strong>vignette</strong> qui peut être épinglée à l’écran de démarrage et permet d’afficher les textes, images et un <strong>badge</strong> de votre choix avec des glyphes et des chiffres. Vous pouvez mettre à jour le contenu des vignettes à partir de l’application par le biais de notifications Push ou selon un calendrier prédéfini. Les vignettes peuvent être adaptatives et varier en fonction de l’endroit où elles sont affichées.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Créer des vignettes</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-create-adaptive-tiles">Créer des vignettes adaptatives</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Choisir une méthode de remise de notification</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Recommandations en matière de vignettes et de badges</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Affichage des notifications.</strong> <br><br>Types de notifications pouvant être affichées.</td>
<td align="left">Les notifications peuvent être affichées dans la <strong>zone de notification</strong> et le <strong>panneau de notification</strong>. Les <strong>notifications d’avertissement</strong> s’affichent dans une petite fenêtre flottante. La définition d’un <strong>PendingIntent</strong> permet d’ajouter des actions aux notifications.</td>
<td align="left">Les notifications contextuelles apparaissent sous forme de <strong>bannières</strong> ou d’<strong>alertes</strong>. Vous pouvez ajouter des boutons d’action personnalisés aux <strong>notifications interactives</strong> définis avec <strong>UIMutableUserNotificationAction</strong>.</td>
<td align="left">Vous pouvez créer des notifications contextuelles adaptatives appelées <strong>notifications toast</strong>. Vous pouvez définir des notifications toast en XML avec du contenu visuel, des <strong>actions</strong> qui peuvent être des boutons, ou des entrées et du son.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notifications toast adaptatives et interactives</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-choosing-a-notification-delivery-method">Choisir une méthode de remise de notification</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-badges-notifications">Recommandations en matière de notifications toast</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Planification des notifications locales.</strong> <br><br>Notifications locales envoyées par votre application à une heure prévue.</td>
<td align="left">Les notifications et actions sont définies à l’aide d’un <strong>NotificationCompat.Builder</strong> et peuvent être planifiées et gérées dans l’application à l’aide d’<strong>AlarmManager</strong> et de <strong>BroadcastReceiver</strong>.</td>
<td align="left">Les notifications locales sont créées à l’aide de <strong>UILocalNotification</strong>et peuvent être planifiées avec <b> UILocalNotification. scheduleLocalNotification :<strong>. | Vous pouvez planifier une notification toast à l’aide de </strong> <a href="/uwp/api/Windows.UI.Notifications.ScheduledToastNotification">ScheduledToastNotification</a><strong>. Vous pouvez envoyer une notification de vignette à partir de votre application à l’aide de la </strong> <a href="/uwp/api/Windows.UI.Notifications.TileNotification">classe TileNotification</a> <strong> , ou planifier une notification par vignette avec <a href="/uwp/api/Windows.UI.Notifications.ScheduledTileNotification">ScheduledTileNotification</a>.<br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts">Notifications toast adaptatives et interactives</a><br/><br/><a href="/windows/uwp/controls-and-patterns/tiles-and-notifications-sending-a-local-tile-notification">Envoyer une notification de vignette locale</a> | | </strong>Envoi de notifications push.</b> Notification envoyée à partir d’un serveur de notifications push et éventuellement gérée dans l’application.</td>
<td align="left"><strong>Google Cloud Messaging</strong> fournit une prise en charge des notifications push pour Android.</td>
</tr>
</tbody>
</table>
<h2 id="media-capture-and-rendering">Capture et rendu multimédia</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Média de capture.</strong> <br><br>Enregistrement d’un contenu audio et visuel.</td>
<td align="left">L’utilisation de <strong>intent</strong>, tel que MediaStore.ACTION_VIDEO_CAPTURE permet la capture multimédia avec une application de caméra existante. L’utilisation de la bibliothèque <strong>android.hardware.camera2</strong> ou <strong>camera</strong> permet d’implémenter une interface de caméra personnalisée. <strong>MediaRecorder</strong> Les API peuvent être utilisées pour capturer l’audio.</td>
<td align="left"><strong>UIImagePickerController</strong> permet la capture de vidéos et de photos avec l’interface utilisateur du système. Les classes <strong>AVFoundation</strong> telles que <strong>AVCaptureSession</strong> permettent d’accéder directement à la caméra. <br/>La classe <strong>AVAudioRecorder</strong> permet d’enregistrer du son.</td>
<td align="left">Vous pouvez capturer des photos et de la vidéo tout en utilisant l’interface utilisateur de l’appareil photo intégré avec la <strong><a href="/uwp/api/Windows.Media.Capture.CameraCaptureUI">classe CameraCaptureUI</a></strong>. Vous pouvez interagir avec la caméra à un niveau bas et capturer des données audio avec des classes dans <strong><a href="/uwp/api/Windows.Media.Capture">Windows. Media. capture</a></strong> , telles que l' <strong><a href="/uwp/api/Windows.Media.Capture.MediaCapture">API MediaCapture</a></strong>. <br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-cameracaptureui">Capturer des photos et des vidéos à l’aide de CameraCaptureUI</a><br/><br/><a href="/windows/uwp/audio-video-camera/capture-photos-and-video-with-mediacapture">Capturer des photos et des vidéos à l’aide de MediaCapture</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Lecture du média.</strong> <br><br>Lecture de fichiers audio et vidéo.</td>
<td align="left">Les classes <strong>MediaPlayer</strong> et <strong>AudioManager</strong> permettent de lire des fichiers audio et vidéo.</td>
<td align="left">L’<strong>infrastructure AVKit</strong>, <strong>AVAudioPlayer</strong> et l’<strong>infrastructure du lecteur multimédia</strong> permettent de lire des fichiers audio et vidéo.</td>
<td align="left">Vous pouvez utiliser la <strong><a href="/uwp/api/Windows.Media.Core.MediaSource">classe MediaSource</a></strong>, le <strong><a href="/uwp/api/windows.ui.xaml.controls.mediaelement">MediaElement</a></strong>et les classes <strong><a href="/uwp/api/windows.media.playback.mediaplayer">MediaPlayer</a></strong> pour lire des données audio et vidéo à partir de sources telles que des fichiers locaux et distants.<br/><br/><a href="/windows/uwp/audio-video-camera/media-playback-with-mediasource">Lecture de contenu multimédia avec MediaSource</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modification du média.</strong> <br><br>Composition de nouveaux fichiers multimédias à partir d’enregistrements existants et application d’effets spéciaux.</td>
<td align="left">Les classes de faible niveau, telles que <strong>MediaCodec</strong>, <strong>MediaMuxer</strong> et <strong>android.media.effect</strong> peuvent être utilisées pour modifier du contenu.</td>
<td align="left">Les classes de l’infrastructure <strong>AV Foundation</strong>, telles que <strong>AVMutableComposition</strong>, <strong>AVMutableVideoComposition</strong> et <strong>AVMutableAudioMix</strong> peuvent être utilisées pour modifier du contenu.</td>
<td align="left">Vous pouvez utiliser les API <strong><a href="/uwp/api/windows.media.editing">Windows. Media. Editing</a></strong> telles que <strong><a href="/uwp/api/windows.media.editing.mediacomposition">MediaComposition</a></strong> et <strong><a href="/uwp/api/windows.media.editing.mediaclip">MediaClip</a></strong> pour créer des compositions de média à partir de fichiers audio et vidéo. Vous pouvez ajouter des superpositions d’images et de vidéos, combiner des clips vidéo, ajouter du son en arrière-plan et appliquer des effets audio et vidéo.<br/><br/><a href="/windows/uwp/audio-video-camera/media-compositions-and-editing">Compositions multimédias et modification</a></td>
</tr>
</tbody>
</table>
<h2 id="sensors">Capteurs</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Sens.</strong> <br><br>Détection des propriétés environnementales, de position et de mouvement de l’appareil.</td>
<td align="left">L’<strong>infrastructure de capteur</strong> permet d’accéder aux capteurs matériels et logiciels avec des classes, telles que <strong>SensorManager</strong> et <strong>SensorEvent</strong>.</td>
<td align="left">L<strong>’infrastructure Core Motion</strong> permet d’accéder aux données des capteurs bruts et traités.</td>
<td align="left">Vous pouvez utiliser des classes dans <strong><a href="/uwp/api/windows.devices.sensors">Windows. Devices. Sensors</a></strong> pour accéder aux relevés de capteur et aux événements déclenchés quand de nouvelles données de lecture sont reçues du capteur.<br/><br/><a href="/windows/uwp/devices-sensors/sensors">Détecteurs</a></td>
</tr>
</tbody>
</table>
<h2 id="location-and-mapping">Localisation et mappage</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Emplacement.</strong> <br><br>Recherche de l’emplacement <strong>actuel</strong> de l’appareil et suivi des <strong>modifications</strong>.</td>
<td align="left">Les API de localisation des services Google Play offrent un accès de haut niveau au <strong>dernier emplacement connu</strong> avec le <strong>fournisseur de localisation fusionné</strong> à l’aide des méthodes <strong>getLastLocation</strong> et <strong>requestLocationUpdates</strong>. <strong>LocationManager</strong> offre un accès de bas niveau dans les bibliothèques Android.</td>
<td align="left">La classe <strong>CLLocationManager</strong> <strong> de Core Location</strong> permet de surveiller l’emplacement d’un appareil, avec <strong>startUpdatingLocation</strong> pour le service de localisation standard et <strong>startMonitoringSignificantLocationChanges</strong> pour le service de localisation en cas de <strong>changement notable</strong>.</td>
<td align="left">Vous pouvez suivre l’emplacement de l’appareil avec des classes dans <strong><a href="/uwp/api/windows.devices.geolocation">Windows. Devices. géolocalisation</a></strong>. Utilisez <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync">géolocator. GetGeopositionAsync</a></strong> pour une lecture ponctuelle. Utilisez <strong><a href="/uwp/api/windows.devices.geolocation.geolocator.positionchanged">géolocateur. PositionChanged</a></strong> pour obtenir l’emplacement régulièrement à l’aide d’un minuteur ou être informé de la modification de l’emplacement.<br/><br/><a href="/windows/uwp/maps-and-location/get-location">Obtenir la localisation de l’utilisateur</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Affichage des mappages.</strong> <br><br>Affichage d’une <strong>carte interactive intégrée</strong> et ajout de <strong>centres d’intérêt</strong>.</td>
<td align="left">Les classes <strong>GoogleMap</strong>, <strong>MapFragment</strong> et <strong>MapView</strong> de l’<strong>API Google Maps Android</strong> permet d’intégrer des cartes dans des applications. Il est possible d’afficher des centres d’intérêt à l’aide de <strong>marqueurs</strong> et de la classe <strong>Marker</strong> personnalisable.</td>
<td align="left">Les cartes sont intégrées aux applications iOS à l’aide de la classe <strong>MKMapView</strong> de l’<strong>infrastructure MapKit</strong>. Les <strong>Annotations</strong> peuvent être ajoutées aux applications pour afficher des points d’intérêt à l’aide de classes d’objet telles que <strong>MKPointAnnotation</strong> et des classes d’affichage telles que <strong>MKPinAnnotationView</strong>.</td>
<td align="left">Vous pouvez incorporer des mappages dans vos applications à l’aide du contrôle XAML <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapcontrol">collection MapControl</a></strong> intégré qui fournit des vues 2D, 3D et Streetside. Vous pouvez ajouter des points d’intérêt avec une <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mapicon">punaise, une</a></strong>image ou une forme à l’aide de classes telles que <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolygon">maMapPolygon</a></strong> et <strong><a href="/uwp/api/windows.ui.xaml.controls.maps.mappolyline">polyligne</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/display-maps">Afficher des cartes avec des vues 2D, 3D et Streetside</a><br/><br/><a href="/windows/uwp/maps-and-location/display-poi">Afficher des POI (centres d’intérêt) sur une carte</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Géorepérage.</strong> <br><br>Surveillance de l’entrée et de la sortie d’une région géographique particulière.</td>
<td align="left">Les limites géographiques sont surveillées à l’aide des <strong>services de localisation</strong> du Kit de développement logiciel (SDK) des services Google Play.</td>
<td align="left">Les régions sont surveillées avec la classe <strong>CLCircularRegion</strong> et enregistrées à l’aide de <strong>CLLocationManager.startMonitoringForRegion:</strong>.</td>
<td align="left">Vous pouvez créer une limite géographique avec la classe de la <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofence">limite de géolimite</a></strong> et définir vos <strong>États surveillés</strong> , tels que l’entrée ou la sortie d’une région. Gérez les événements géolimiter au premier plan avec la <strong><a href="/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor">classe GeofenceMonitor</a></strong>, et en arrière-plan avec la <strong><a href="/uwp/api/windows.applicationmodel.background.locationtrigger">classe d’arrière-plan LocationTrigger</a></strong>.<br/><br/><a href="/windows/uwp/maps-and-location/set-up-a-geofence">Définir une limite géographique</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Géocodage et Géocodage inversé.</strong> <br><br>Conversion d’adresses en emplacements géographiques (géocodage) et inversement (géocodage inverse).<br/></td>
<td align="left">La classe <strong>Geocoder</strong> est utilisée pour le géocodage et le géocodage inverse.</td>
<td align="left">La classe <strong>CLGeocoder</strong> est utilisée pour le géocodage.</td>
<td align="left">Vous pouvez effectuer le géocodage à l’aide de la <strong><a href="/uwp/api/windows.services.maps.maplocationfinder">classe MapLocationFinder</a></strong> dans <strong><a href="/uwp/api/windows.services.maps">Windows. services. Maps</a></strong>. Vous utilisez <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync">FindLocationsAsync</a></strong> pour le géocodage et <strong><a href="/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync">FindLocationsAtAsync</a></strong> pour le géocodage inversé.<br/><br/><a href="/windows/uwp/maps-and-location/geocoding">Effectuer un géocodage et un géocodage inverse</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Itinéraires et directions.</strong> <br><br>Itinéraires, indications et distances entre deux emplacements géographiques.</td>
<td align="left">Google propose l’<strong>API Google Maps Directions</strong> du service web qui peut être utilisée sur Android même si aucun Kit de développement logiciel n’est fourni.</td>
<td align="left">Map Kit fournit l’API <strong>MKDirections</strong> qui peut être utilisée pour récupérer des informations relatives à un itinéraire et des indications.</td>
<td align="left">Vous pouvez demander une route ou conduire avec la classe <strong><a href="/uwp/api/windows.services.maps.maproutefinder">MapRouteFinder</a></strong> dans <strong><a href="/uwp/api/windows.services.maps">Windows. services. Maps</a></strong>. Les itinéraires sont retournés sous la forme d’une instance <strong><a href="/uwp/api/windows.services.maps.maproute">MapRoute</a></strong> qui peut être facilement affichée sur un collection MapControl. Les directions sont retournées à l’intérieur de l’objet <strong><a href="/uwp/api/windows.services.maps.maproutemaneuver">MapRouteManeuver</a></strong> .<br/><br/><a href="/windows/uwp/maps-and-location/routes-and-directions">Afficher des itinéraires et indications sur une carte</a></td>
</tr>
</tbody>
</table>
<h2 id="app-to-app-communication">Communication entre les applications</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Appel d’une autre application.</strong> <br><br>Lancement d’une autre application et éventuellement partage de données, telles que des liens, du texte, des photos, des vidéos et des fichiers.</td>
<td align="left">Une <strong>intention implicite</strong> permet de lancer une autre application en définissant une <strong>action</strong> et des données facultatives dans une<strong> intention</strong> et en l’appelant avec <strong>startActivityForResult</strong>.<br/></td>
<td align="left">Les <strong>extensions d’application</strong> peuvent être utilisées pour fournir un accès aux données d’application à une autre application. Les <strong>schémas d’URL</strong> permettent de transmettre une URL à une autre application.</td>
<td align="left">Vous pouvez lancer une autre application qui s’est inscrite pour un URI avec <strong><a href="/uwp/api/windows.system.launcher.launchuriasync">Launcher. LaunchUriAsync</a></strong>ou <strong><a href="/uwp/api/windows.system.launcher.launchuriforresultsasync">Launcher. LaunchUriForResultsAsync</a></strong> pour lancer les résultats et récupérer les données à partir de l’application lancée. Vous pouvez utiliser <strong><a href="/uwp/api/windows.system.launcher.launchfileasync">Launcher. LaunchFileAsync</a></strong> pour transmettre un fichier à une autre application à gérer.<br/><br/>Vous pouvez utiliser un <strong>contrat de partage</strong> pour partager facilement des données entre des applications.<br/><br/><a href="/windows/uwp/launch-resume/launch-default-app">Lancer l’application par défaut pour un URI</a><br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Lancer une application pour obtenir des résultats</a><br/><br/><a href="/windows/uwp/launch-resume/launch-the-default-app-for-a-file">Lancer l’application par défaut pour un fichier</a><br/><br/><a href="/windows/uwp/app-to-app/share-data">Partager les données</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Autorisation de l’appel de votre application.</strong> <br><br>Autorisation de votre application à répondre à une demande issue d’une autre application.</td>
<td align="left">Les applications enregistrent une <strong>activité de gestion d’intention</strong> avec un <strong>filtre d’intention</strong> pour répondre à une intention implicite à partir d’une autre application.</td>
<td align="left">La création d’un package d’une <strong>extension d’application</strong> permet de partager des données avec d’autres applications. Les applications peuvent enregistrer un <strong>schéma d’URL personnalisé</strong> à l’aide de la clé <strong>CFBundleURLTypes</strong> d’Info.plist.</td>
<td align="left">Vous pouvez enregistrer votre application comme gestionnaire par défaut d’un <strong>nom de schéma d’URI</strong> en enregistrant un <strong><a href="/uwp/api/windows.applicationmodel.activation.activationkind#Protocol">protocole</a></strong> dans le manifeste du package et en mettant à jour le gestionnaire d’événements <strong><a href="/uwp/api/windows.ui.xaml.application.onactivated">Application.OnActivated</a></strong>, qui renvoie éventuellement des résultats. De la même façon, vous pouvez inscrire votre application en tant que gestionnaire par défaut pour certains types de fichiers en ajoutant une déclaration dans le manifeste du package et en gérant l’événement <strong><a href="/uwp/api/windows.ui.xaml.application.onfileactivated">application. OnFileActivated</a></strong> .<br/><br/>Vous pouvez gérer les demandes de contrat de partage en inscrivant votre application en tant que cible de partage dans le manifeste et en gérant l’événement <strong><a href="/uwp/api/windows.ui.xaml.application.onsharetargetactivated">application. OnShareTargetActivated</a></strong> .<br/><br/><a href="/windows/uwp/launch-resume/how-to-launch-an-app-for-results">Lancer une application pour obtenir des résultats</a><br/><br/><a href="/windows/uwp/launch-resume/handle-file-activation">Gérer l’activation des fichiers</a><br/><br/><a href="/windows/uwp/app-to-app/receive-data">Recevoir des données</a></td>
</tr>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Copier et coller.</strong> <br><br>Copie et collage de texte et d’autres contenus entre les applications.</td>
<td align="left">L’<strong>infrastructure du Presse-papiers</strong> peut servir à implémenter la fonction de copier-coller à l’aide des classes <strong>ClipboardManager</strong> et <strong>ClipData</strong>.</td>
<td align="left"><strong>UIPasteboard</strong>, <strong>UIMenuController</strong> et <strong>UIResponderStandardEditActions</strong> peuvent servir à implémenter la fonction de copier-coller.</td>
<td align="left">De nombreux contrôles XAML par défaut prennent déjà en charge la fonction de copier-coller. Vous pouvez implémenter la copie et le collage vous-même à l’aide des classes <strong><a href="/uwp/api/windows.applicationmodel.datatransfer.datapackage">DataPackage</a></strong> et <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard">Clipboard</a></strong> dans <strong><a href="/uwp/api/Windows.ApplicationModel.DataTransfer">Windows. ApplicationModel. datatransfer</a></strong>.<br/><br/><a href="/windows/uwp/app-to-app/copy-and-paste">Copier et coller</a></td>
</tr>
<tr class="even">
<td align="left"><strong>Glisser-déplacer.</strong> <br><br>Glisser-déplacer du contenu entre plusieurs applications.</td>
<td align="left">La fonction de glisser-déplacer peut être implémentée au sein d’une seule application à l’aide de l’<strong>infrastructure de glisser-déplacer Android</strong>.</td>
<td align="left">Aucune API de glisser-déplacer de haut niveau n’est fournie par iOS.</td>
<td align="left">Vous pouvez implémenter la fonction de glisser-déplacer dans votre application pour permettre de glisser-déplacer du contenu d’une application à une autre, du Bureau vers une application et inversement. Vous implémentez la prise en charge du glisser-déplacer dans la classe UIElement avec les propriétés <strong><a href="/uwp/api/windows.ui.xaml.uielement.allowdrop">AllowDrop</a></strong>, et <strong><a href="/uwp/api/windows.ui.xaml.uielement.candrag">CanDrag</a></strong> , ainsi que les événements <strong><a href="/uwp/api/windows.ui.xaml.uielement.dragover">DragOver</a></strong>et <strong><a href="/uwp/api/windows.ui.xaml.uielement.drop">Drop</a></strong> .<br/><br/><a href="/windows/uwp/app-to-app/drag-and-drop">Glisser-déplacer</a></td>
</tr>
</tbody>
</table>
<h2 id="software-design">Conception de logiciels</h2>
<table style="width:100%">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="40%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><strong>Concept général</strong></th>
<th align="left"><strong>Android</strong></th>
<th align="left"><strong>iOS</strong></th>
<th align="left"><strong>UWP Windows 10</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd" style="background-color: #f2f2f2">
<td align="left"><strong>Modèles de conception de logiciels.</strong> <br><br>Modèles recommandés ou bien adaptés pour la plateforme.</td>
<td align="left">Aucun modèle officiel n’a été recommandé ou fourni pour le développement Android, bien que la version bêta de l’infrastructure de liaison de données puisse permettre une utilisation plus généralisée du modèle <strong>Model-View-ViewModel (MVVM)</strong>. Un certain nombre d’articles et d’infrastructures tiers recommandent les approches <strong>Model-View-Presenter (MVP)</strong> et <strong>MVVM</strong>.</td>
<td align="left">Le modèle <strong>MVC (Model-View-Controller)</strong> est un modèle courant utilisé avec iOS et est intégré à la plateforme.</td>
<td align="left">Vous n’êtes pas limité à un modèle spécifique lorsque vous créez des logiciels pour UWP.<br/><br/>Vous pouvez utiliser le modèle de <a href="/windows/uwp/data-binding/index">liaison de données</a> intégré pour garantir une séparation nette des problèmes liés aux données et de ceux liés à l’interface utilisateur et éviter d’avoir à coder les gestionnaires d’événements de l’interface utilisateur qui mettent ensuite à jour les valeurs des propriétés.<br/><br/>Vous pouvez étendre la liaison de données de manière à suivre le modèle <strong>Model-View-ViewModel (MVVM)</strong> soit en utilisant des bibliothèques MVVM tierces, telles que le <a href="https://archive.codeplex.com/?p=mvvmlight">Kit de ressources MVVM Light</a>, ou en déployant votre propre liaison et en maintenant la logique en dehors de code-behind.<br/><br/><a href="/previous-versions/msp-n-p/hh848246(v=pandp.10)">Le modèle MVVM</a><br/><br/><a href="https://github.com/Windows-XAML/Template10/wiki">Modèles de projet Visual Studio Template 10</a></td>
</tr>
</tbody>
</table>