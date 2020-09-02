---
title: Shell compatible avec Voice (VES) sur Xbox
description: Découvrez comment ajouter une prise en charge du contrôle vocal à vos applications de plateforme Windows universelle (UWP) sur Xbox en utilisant le shell compatible avec Voice (VES).
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox, Speech, Shell avec voix activée
ms.openlocfilehash: 38afa2473dd74ab580cf38cc21d1f2b192f9b72a
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304651"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Utilisation de la reconnaissance vocale pour appeler des éléments d’interface utilisateur

Le shell compatible avec Voice (VES) est une extension de la plateforme Windows Speech qui permet une expérience vocale de premier ordre dans les applications, ce qui permet aux utilisateurs d’utiliser la reconnaissance vocale pour appeler des contrôles à l’écran et insérer du texte par le biais de la dictée. Le système VES s’efforce de fournir une expérience de bout en bout de bout en bout sur l’ensemble des shells et appareils Windows, avec un minimum d’effort requis des développeurs d’applications.  Pour ce faire, il tire parti de la plateforme Microsoft Speech et de l’infrastructure UI Automation (UIA).

## <a name="user-experience-walkthrough"></a>Démonstration de l’expérience utilisateur ##
Vous trouverez ci-dessous une vue d’ensemble de ce qu’un utilisateur peut rencontrer lors de l’utilisation de VES sur Xbox, et il devrait vous aider à définir le contexte avant de plonger dans les détails du fonctionnement du système VES.

- L’utilisateur active la console Xbox et souhaite parcourir ses applications pour trouver un intérêt :

        User: "Hey Cortana, open My Games and Apps"

- L’utilisateur reste en mode d’écoute active (ALM), ce qui signifie que la console est à l’écoute de l’utilisateur pour appeler un contrôle visible à l’écran, sans qu’il soit nécessaire de dire « Hey Cortana » à chaque fois.  L’utilisateur peut maintenant basculer vers afficher les applications et faire défiler la liste des applications :

        User: "applications"

- Pour faire défiler la vue, l’utilisateur peut simplement indiquer :

        User: "scroll down"

- L’utilisateur voit l’image de la boîte de l’application qui l’intéresse, mais a oublié le nom.  L’utilisateur demande l’affichage des étiquettes de pourboires vocaux :

        User: "show labels"

- Maintenant qu’il est clair que vous pouvez indiquer, l’application peut être lancée :

        User: "movies and TV"

- Pour quitter le mode d’écoute active, l’utilisateur demande à Xbox d’arrêter l’écoute :

        User: "stop listening"

- Plus tard, une nouvelle session d’écoute active peut être démarrée avec :

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dépendance UI Automation ##
Le système VES est un client UI Automation et s’appuie sur les informations exposées par l’application par le biais de ses fournisseurs UI Automation. Il s’agit de la même infrastructure que celle déjà utilisée par la fonctionnalité narrateur sur les plateformes Windows.  UI Automation permet l’accès par programmation aux éléments de l’interface utilisateur, y compris le nom du contrôle, son type et les modèles de contrôle qu’il implémente.  Lorsque l’interface utilisateur est modifiée dans l’application, VES réagit aux événements de mise à jour UIA et réanalyse l’arborescence UI Automation mise à jour pour trouver tous les éléments actionnables, à l’aide de ces informations pour générer une grammaire de reconnaissance vocale. 

Toutes les applications UWP ont accès à l’infrastructure UI Automation et peuvent exposer des informations sur l’interface utilisateur, indépendamment de l’infrastructure graphique sur laquelle elles sont basées (XAML, DirectX/Direct3D, Xamarin, etc.).  Dans certains cas, comme XAML, la majeure partie du travail est effectuée par l’infrastructure, ce qui réduit considérablement le travail requis pour prendre en charge Narrator et VES.

Pour plus d’informations sur UI Automation, consultez [notions de base d’UI Automation](/dotnet/framework/ui-automation/ui-automation-fundamentals "Notions de base d'UI Automation").

## <a name="control-invocation-name"></a>Nom de l’appel de contrôle ##
Le système VES utilise l’heuristique suivante pour déterminer quelle phrase doit s’inscrire auprès du module de reconnaissance vocale comme nom du contrôle (par exemple, ce que l’utilisateur doit parler pour appeler le contrôle).  Il s’agit également de l’expression qui s’affiche dans l’étiquette de Conseil vocal.

Source de nom par ordre de priorité :

1. Si l’élément a une `LabeledBy` propriété jointe, ves utilise le `AutomationProperties.Name` de cette étiquette de texte.
2. `AutomationProperties.Name` de l’élément.  En XAML, le contenu de texte du contrôle sera utilisé comme valeur par défaut pour `AutomationProperties.Name` .
3. Si le contrôle est un ListItem ou un bouton, le système VES recherche le premier élément enfant avec un valide `AutomationProperties.Name` .

## <a name="actionable-controls"></a>Contrôles actionnables ##
Le système VES considère qu’un contrôle est exploitable s’il implémente l’un des modèles de contrôle Automation suivants :

- **InvokePattern** (par exemple, Button)-représente les contrôles qui initialisent ou exécutent une action unique et non ambiguë et ne maintiennent pas l’État lorsqu’ils sont activés.

- **TogglePattern** (par exemple, Case à cocher) : représente un contrôle qui peut parcourir un ensemble d’États et conserver un État une fois défini.

- **SelectionItemPattern** (par exemple, Zone de liste déroulante) : représente un contrôle qui agit comme un conteneur pour une collection d’éléments enfants sélectionnables.

- **ExpandCollapsePattern** (par exemple, Zone de liste déroulante)-représente les contrôles qui se développent visuellement pour afficher le contenu et réduire pour masquer le contenu.

- **ScrollPattern** (par exemple, List)-représente les contrôles qui jouent le rôle de conteneurs à défilement pour une collection d’éléments enfants.

## <a name="scrollable-containers"></a>Conteneurs à défilement ##
Pour les conteneurs pouvant faire l’objet d’un défilement qui prennent en charge ScrollPattern, le système VES écoute les commandes vocales telles que « faire défiler vers la gauche », « faire défiler vers la droite », etc., et appelle le défilement avec les paramètres appropriés lorsque l’utilisateur déclenche l’une de ces commandes.  Les commandes de défilement sont injectées en fonction de la valeur des `HorizontalScrollPercent` `VerticalScrollPercent` Propriétés et.  Par exemple, si `HorizontalScrollPercent` est supérieur à 0, le défilement vers la gauche est ajouté, s’il est inférieur à 100, « Scroll Right » est ajouté, et ainsi de suite.

## <a name="narrator-overlap"></a>Chevauchement du narrateur ##
L’application narrateur est également un client UI Automation et utilise la `AutomationProperties.Name` propriété comme l’une des sources du texte qu’elle lit pour l’élément d’interface utilisateur actuellement sélectionné.  Pour offrir une meilleure expérience d’accessibilité, de nombreux développeurs d’applications ont recouru à la surcharge de la `Name` propriété avec du texte descriptif long, avec l’objectif de fournir plus d’informations et de contexte lorsqu’ils sont lus par Narrator.  Toutefois, cela provoque un conflit entre les deux fonctionnalités : le système VES a besoin d’expressions courtes qui correspondent ou correspondent étroitement au texte visible du contrôle, tandis que le narrateur bénéficie de plus longues expressions descriptives pour offrir un meilleur contexte.

Pour résoudre ce dernier, à compter de Windows 10 Creators Update, Narrator a été mis à jour pour examiner également la `AutomationProperties.HelpText` propriété.  Si cette propriété n’est pas vide, Narrator parle son contenu en plus de `AutomationProperties.Name` .  Si `HelpText` est vide, Narrator lira uniquement le contenu du nom.  Cela permet d’utiliser des chaînes descriptives plus longues lorsque cela est nécessaire, mais conserve une expression plus rapide et conviviale de reconnaissance vocale dans la `Name` propriété.

![](images/ves_narrator.jpg)

Pour plus d’informations [, consultez Propriétés d’Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "Propriétés d’Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur").

## <a name="active-listening-mode-alm"></a>Mode d’écoute active (ALM) ##
### <a name="entering-alm"></a>Entrée de ALM ###
Sur Xbox, le système VES n’écoute pas constamment les entrées vocales.  L’utilisateur doit entrer explicitement le mode d’écoute actif en disant :

- « Hey Cortana, SELECT » ou
- « Hey Cortana, effectuer une sélection »

Il existe plusieurs autres commandes Cortana qui laissent également l’utilisateur actif écouter à l’achèvement, par exemple « Hey Cortana, se connecter » ou « Hey Cortana, Go chez moi ». 

L’entrée de ALM aura les effets suivants :

- La superposition Cortana s’affiche dans l’angle supérieur droit, indiquant à l’utilisateur qu’il peut dire ce qu’il voit.  Pendant que l’utilisateur parle, les fragments d’expression reconnus par le module de reconnaissance vocale s’affichent également à cet emplacement.
- VES analyse l’arborescence UIA, recherche tous les contrôles actionnables, inscrit son texte dans la grammaire de la reconnaissance vocale et démarre une session d’écoute continue.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Quitter ALM ###
Le système restera en ALM pendant que l’utilisateur interagit avec l’interface utilisateur à l’aide de la voix.  Il existe deux façons de quitter ALM :

- L’utilisateur dit explicitement « arrêter l’écoute », ou
- Un dépassement de délai se produit s’il n’y a pas de reconnaissance positive dans les 17 secondes qui ont été entrées dans la partie ALM ou depuis la dernière reconnaissance positive

## <a name="invoking-controls"></a>Appeler des contrôles ##
Dans ALM, l’utilisateur peut interagir avec l’interface utilisateur à l’aide de la voix.  Si l’interface utilisateur est correctement configurée (avec les propriétés de nom correspondant au texte visible), l’utilisation de la voix pour effectuer des actions doit être une expérience naturelle et transparente.  L’utilisateur doit pouvoir simplement indiquer ce qu’il voit à l’écran.

## <a name="overlay-ui-on-xbox"></a>Superposition de l’interface utilisateur sur Xbox ##
Le nom VES dérivé pour un contrôle peut être différent du texte visible réel dans l’interface utilisateur.  Cela peut être dû à la `Name` propriété du contrôle ou `LabeledBy` à l’élément attaché explicitement défini sur une chaîne différente.  Ou bien, le contrôle n’a pas de texte d’interface utilisateur graphique, mais uniquement un élément icône ou image.

Dans ces cas, les utilisateurs ont besoin d’un moyen de voir ce qui doit être dit pour appeler ce type de contrôle.  Par conséquent, une fois dans l’écoute active, vous pouvez afficher des conseils vocaux en disant « afficher les étiquettes ».  Cela entraîne l’affichage des étiquettes de pourboires vocaux au-dessus de chaque contrôle actionnable.

Il existe une limite de 100 étiquettes. par conséquent, si l’interface utilisateur de l’application a plus de contrôles actionnables que 100, cela signifie que les étiquettes de Conseil vocal ne sont pas affichées.  Dans ce cas, les étiquettes choisies ne sont pas déterministes, car elles dépendent de la structure et de la composition de l’interface utilisateur actuelle, comme d’abord énumérée dans l’arborescence UIA.

Une fois que les étiquettes de Conseil vocal sont affichées, il n’y a aucune commande permettant de les masquer, elles restent visibles jusqu’à ce que l’un des événements suivants se produise :

- l’utilisateur appelle un contrôle
- l’utilisateur quitte la scène actuelle
- l’utilisateur dit « arrêter l’écoute »
- dépassement du délai d’attente en mode d’écoute active

## <a name="location-of-voice-tip-labels"></a>Emplacement des étiquettes de Conseil vocal ##
Les étiquettes de pourboire vocal sont centrées horizontalement et verticalement dans le BoundingRectangle du contrôle.  Lorsque les contrôles sont petits et étroitement regroupés, les étiquettes peuvent se chevaucher/être masquées par d’autres et VES tente de pousser ces étiquettes pour les séparer et s’assurer qu’elles sont visibles.  Toutefois, il n’est pas garanti qu’il fonctionne 100% du temps.  S’il existe une interface utilisateur très encombrée, il est probable que certaines étiquettes soient masquées par d’autres. Vérifiez votre interface utilisateur avec « afficher les étiquettes » pour vous assurer qu’il y a suffisamment d’espace pour la visibilité de l’info-bulle.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Zones de liste modifiable ##
Lorsqu’une zone de liste déroulante est développée, chaque élément individuel de la zone de liste déroulante reçoit son propre étiquette de Conseil vocale et, souvent, les contrôles existants de la liste déroulante.  Pour éviter de présenter un muddle encombré et confus d’étiquettes (où les étiquettes des éléments de zone de liste déroulante sont mélangées avec les étiquettes des contrôles situés derrière la zone de liste déroulante) lorsqu’une zone de liste déroulante est développée, seules les étiquettes de ses éléments enfants sont affichées.  toutes les autres étiquettes de Conseil vocal sont masquées.  L’utilisateur peut ensuite sélectionner l’un des éléments de liste déroulante ou « fermer » la zone de liste déroulante.

- Étiquettes sur les zones de liste déroulante réduites :

    ![](images/ves_combo_closed.png)

- Étiquettes sur la zone de liste déroulante développée :

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Contrôles à défilement ##
Pour les contrôles à défilement, les conseils vocaux pour les commandes de défilement sont centrés sur chacun des bords du contrôle.  Les conseils vocaux s’affichent uniquement pour les directions de défilement qui sont actionnables. ainsi, par exemple, si le défilement vertical n’est pas disponible, le fait de faire défiler vers le haut et de faire défiler vers le haut ne s’affiche pas.  Lorsque plusieurs régions défilantes sont présentes, le système VES utilise des ordinaux pour les différencier (par exemple, « Faire défiler vers la droite 1 », « faire défiler vers la droite 2 », etc.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Lever les ambiguïtés ##
Quand plusieurs éléments d’interface utilisateur ont le même nom ou que le module de reconnaissance vocale a mis en correspondance plusieurs candidats, le système VES entrera en mode de désambiguïsation.  Dans ce mode, les étiquettes vocales s’affichent pour les éléments impliqués afin que l’utilisateur puisse sélectionner celui qui convient. L’utilisateur peut annuler le mode de désambiguation en disant « annuler ».

Par exemple :

- En mode d’écoute active, avant toute ambiguïté ; l’utilisateur dit « AM I ambigu » :

    ![](images/ves_disambig1.png) 

- Les deux boutons correspondent ; désambiguation démarrée :

    ![](images/ves_disambig2.png) 

- Indication de l’action de clic lorsque l’option « Sélectionner 2 » a été choisie :

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Exemple d’interface utilisateur ##
Voici un exemple d’interface utilisateur XAML, qui définit le AutomationProperties.Name de différentes manières :

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


L’exemple ci-dessus présente l’interface utilisateur, ainsi que les étiquettes vocales.
 
- En mode d’écoute active, sans étiquettes affichées :

    ![](images/ves_alm_nolabels.png) 

- En mode d’écoute active, après que l’utilisateur a dit « afficher les étiquettes » :

    ![](images/ves_alm_labels.png) 

Dans le cas de `button1` , XAML remplit automatiquement la `AutomationProperties.Name` propriété à l’aide du texte du contenu textuel visible du contrôle.  C’est pour cette raison qu’il existe une étiquette de Conseil vocal, bien qu’il n’y ait pas de `AutomationProperties.Name` jeu explicite.

Avec `button2` , nous définissons explicitement le `AutomationProperties.Name` sur une valeur autre que le texte du contrôle.

Avec `comboBox` , nous avons utilisé la `LabeledBy` propriété pour faire référence à la `label1` source de l’automatisation `Name` , et dans `label1` nous définissons `AutomationProperties.Name` sur une expression plus naturelle que celle qui est affichée à l’écran (« jour de la semaine » au lieu de « sélectionner le jour de la semaine »).

Enfin, avec `button3` , le système VES récupère le du `Name` premier élément enfant, car il `button3` n’a pas de `AutomationProperties.Name` jeu.

## <a name="see-also"></a>Voir aussi
- [Notions de base d’UI Automation](/dotnet/framework/ui-automation/ui-automation-fundamentals "Notions de base d'UI Automation")
- [Propriétés d’Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ff400332(v=vs.95) "Propriétés d’Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur")
- [Forum aux questions](frequently-asked-questions.md)
- [UWP sur Xbox One](index.md)