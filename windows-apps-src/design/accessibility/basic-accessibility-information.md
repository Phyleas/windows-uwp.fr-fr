---
description: 'Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations de base nécessaires aux technologies d’assistance.'
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: Présenter des informations d’accessibilité élémentaires
label: Expose basic accessibility information
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09dfb92f53105d7c8718ff12f1a0d5634ba6a75d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032562"
---
# <a name="expose-basic-accessibility-information"></a>Présenter des informations d’accessibilité élémentaires  



Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations de base nécessaires aux technologies d’assistance.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>Nom accessible  
Le nom accessible est une chaîne de texte courte et descriptive qui est utilisée par les lecteurs d’écran pour présenter un élément d’interface utilisateur. Définissez le nom accessible des éléments d’interface utilisateur dont la signification est importante pour comprendre le contenu ou interagir avec l’interface utilisateur. Ces éléments incluent généralement les images, les champs d’entrée, les boutons, les contrôles et les régions.

Le tableau suivant décrit comment définir ou obtenir un nom accessible pour différents types d’éléments dans une interface utilisateur XAML.

| Type d'élément | Description |
|--------------|-------------|
| Texte statique | Pour les éléments [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) et [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), un nom accessible est déterminé automatiquement à partir du texte visible (interne). Tout le texte dans cet élément est utilisé comme nom. Voir [Nom du texte interne](#name_from_inner_text). |
| Images | L’élément [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) XAML n’a pas d’analogue direct pour l’attribut HTML **alt** de type **img** et éléments similaires. Vous devez utiliser [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) pour fournir un nom ou utiliser la technique de légendage. Voir [Noms accessibles pour les images](#images). |
| Éléments du formulaire | Le nom accessible pour un élément de formulaire doit être identique à l’étiquette affichée pour cet élément. Voir [Étiquettes et LabeledBy](#labels). |
| Boutons et liens | Par défaut, le nom accessible d’un bouton ou d’un lien est basé sur le texte visible, les mêmes règles que celles décrites dans [Nom du texte interne](#name_from_inner_text) étant appliquées. Dans les cas où un bouton contient uniquement une image, utilisez [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) pour fournir un équivalent texte-uniquement de l’action prévue du bouton. |

La plupart des éléments de conteneur tels que les panneaux n’effectuent pas la promotion de leur contenu comme nom accessible, car c’est le contenu de l’élément qui doit indiquer un nom et un rôle correspondant, et non son conteneur. L’élément conteneur peut indiquer qu’il s’agit d’un élément ayant des enfants dans une représentation Microsoft UI Automation, de telle sorte que la logique de technologie d’assistance puisse le traverser. Toutefois, les informations sur les conteneurs ne sont généralement pas utiles aux utilisateurs de technologies d’assistance et la plupart des conteneurs ne sont pas nommés.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>Rôle et valeur  
Les contrôles et autres éléments d’interface utilisateur qui font partie du vocabulaire XAML mettent en œuvre la prise en charge d’UI Automation pour signaler les rôles et les valeurs dans le cadre de leurs définitions. Vous pouvez utiliser des outils UI Automation pour examiner les informations sur les rôles et les valeurs pour les contrôles, ou vous pouvez lire la documentation relative aux implémentations [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) de chaque contrôle. Les rôles disponibles dans une infrastructure UI Automation sont définis dans l’énumération [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType). Les clients UI Automation tels que les technologies d’assistance peuvent obtenir des informations sur les rôles en appelant les méthodes que l’infrastructure UI Automation expose à partir de l’élément **AutomationPeer** du contrôle.

Les contrôles n’ont pas tous une valeur. Ceux qui en ont une signalent ces informations à UI Automation par le biais des homologues et modèles qui sont pris en charge par ce contrôle. Par exemple, un élément de formulaire de [**zone de texte**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) a une valeur. Une technologie d’assistance peut être un client UI Automation et découvrir par conséquent qu’il existe une valeur et quelle est cette valeur. Dans ce cas particulier, **TextBox** prend en charge le modèle [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) via les définitions [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer).

> [!NOTE]
> Dans les cas où vous utilisez [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) ou d’autres techniques pour fournir explicitement le nom accessible, n’incluez pas le même texte que celui utilisé par le rôle de contrôle ou les informations de type dans le nom accessible. Par exemple, n’incluez pas des chaînes telles que « bouton » ou « liste » dans le nom. Les informations de type et de rôle proviennent d’une propriété UI Automation différente ( **LocalizedControlType** ) qui est fournie par la prise en charge du contrôle par défaut pour UI Automation. De nombreuses technologies d’assistance ajoutent l’élément **LocalizedControlType** au nom accessible. Le fait de dupliquer le rôle dans le nom accessible peut donc provoquer une répétition de mots inutile. Par exemple, si vous attribuez à un contrôle [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) le nom accessible « bouton » ou incluez « bouton » à la fin du nom, les lecteurs d’écran risquent de lire « bouton bouton ». Nous vous conseillons de tester cet aspect de votre accessibilité à l’aide du Narrateur.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>Influence sur les vues de l’arborescence UI Automation  
La conception de l’infrastructure UI Automation repose sur des arborescences, où les clients UI Automation peuvent récupérer les relations entre les éléments d’une interface utilisateur à l’aide de trois affichages possibles : brut, de contrôle et de contenu. L’affichage de contrôle est souvent utilisé par les clients UI Automation, car il fournit une bonne représentation et une organisation efficace des éléments interactifs d’une interface utilisateur. Les outils de test vous permettent généralement de choisir l’arborescence à utiliser quand l’outil présente l’organisation des éléments.

Par défaut, toute classe dérivée de [**contrôle**](/uwp/api/Windows.UI.Xaml.Controls.Control) et quelques autres éléments s’affichent dans la vue de contrôle lorsque l’infrastructure UI Automation représente l’interface utilisateur d’une application Windows. Cependant, il se peut que vous ne souhaitiez pas qu’un élément apparaisse dans l’affichage de contrôle en raison de la composition de l’interface utilisateur, où cet élément duplique des informations ou présente des informations qui ne sont pas importantes pour les scénarios d’accessibilité. Utilisez la propriété jointe [**AutomationProperties.AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) pour modifier la façon dont les éléments sont exposés dans les arborescences. Si vous placez un élément dans l’arborescence **Raw** , la plupart des technologies d’assistance n’indiquent pas cet élément dans le cadre de leurs affichages. Pour voir quelques exemples de ce fonctionnement dans des contrôles existants, ouvrez le fichier XAML de référence de la conception generic.xaml dans un éditeur de texte et recherchez **AutomationProperties.AccessibilityView** dans les modèles.

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>Nom du texte interne  
Pour simplifier l’utilisation de chaînes qui existent déjà dans l’interface utilisateur visible pour les valeurs de noms accessibles, une grande partie des contrôles et autres éléments d’interface utilisateur permettent de déterminer automatiquement un nom accessible par défaut, en fonction du texte interne de l’élément ou de valeurs de chaînes de propriétés de contenu.

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) et **RichTextBlock** effectuent chacun la promotion de la valeur de la propriété **Text** comme nom accessible par défaut.
* Toute sous-classe [**ContentControl**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) utilise une technique « ToString » itérative pour rechercher des chaînes dans sa valeur de [**contenu**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) , et promeut ces chaînes en tant que nom accessible par défaut.

> [!NOTE]
> Comme l’impose UI Automation, la longueur du nom accessible ne doit pas dépasser 2 048 caractères. Si une chaîne utilisée pour la détermination automatique du nom accessible dépasse cette limite, le nom accessible est tronqué à ce niveau.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>Noms accessibles pour les images
Pour prendre en charge les lecteurs d’écran et pour fournir les informations d’identification de base pour chaque élément dans l’interface utilisateur, vous devez parfois spécifier des alternatives textuelles à des informations non textuelles telles que des images et des graphiques (à l’exclusion des éléments purement décoratifs ou structurels). Ces éléments n’ayant pas de texte interne, aucune valeur n’est calculée pour le nom accessible. Vous pouvez définir directement le nom accessible à l’aide de la propriété attachée [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name), comme Illustré dans l’exemple.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

En guise d’alternative, vous pouvez inclure une légende qui s’affiche dans l’interface utilisateur visible et qui sert également d’informations d’accessibilité associées à l’étiquette pour le contenu de l’image. Voici un exemple :

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>Étiquettes et LabeledBy.  
Le meilleur moyen d’associer une étiquette à un élément de formulaire consiste à utiliser un objet [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) avec un **x:Name** comme texte d’étiquette, puis à définir la propriété jointe [**AutomationProperties.LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) sur l’élément de formulaire pour faire référence à l’étiquetage **TextBlock** par son nom XAML. Si vous utilisez ce modèle, quand l’utilisateur clique sur une étiquette, le focus bascule vers le contrôle associé et les technologies d’assistance peuvent utiliser le texte d’étiquette comme nom accessible pour le champ de formulaire. Voici un exemple qui illustre cette technique.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>Description accessible (facultative)  
Une description accessible fournit des informations d’accessibilité supplémentaires sur un élément d’interface utilisateur particulier. Vous fournissez généralement une description accessible lorsqu’un nom accessible seul ne transmet pas de façon adéquate l’objectif d’un élément.

Le lecteur d’écran Narrateur lit la description accessible d’un élément uniquement lorsque l’utilisateur demande davantage d’informations sur l’élément en appuyant sur Verr. maj+F.

Le nom accessible est censé identifier le contrôle plutôt que documenter son comportement de manière exhaustive. Si une brève description n’est pas suffisante pour expliquer le contrôle, vous pouvez définir la propriété jointe [**AutomationProperties. HelpText**](/dotnet/api/system.windows.automation.automationproperties.helptext) en plus de [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name).

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>Test anticipé et régulier de l’accessibilité  
Pour finir, la meilleure approche de la prise en charge des lecteurs d’écran consiste à tester vous-même votre application à l’aide d’un lecteur d’écran. Cela vous permettra de savoir comment le lecteur d’écran se comporte et d’identifier les informations d’accessibilité éventuellement manquantes dans l’application. Vous pourrez alors ajuster les valeurs de propriétés d’interface utilisateur ou UI Automation en conséquence. Pour plus d’informations, voir [Tests d’accessibilité](accessibility-testing.md).

L’un des outils que vous pouvez utiliser pour tester l’accessibilité s’appelle **AccScope** . L’outil **AccScope** est particulièrement utile car il vous permet d’afficher des représentations visuelles de votre interface utilisateur qui indiquent comment les technologies d’assistance peuvent voir votre application comme une arborescence d’automatisation. Il existe en particulier un mode Narrateur qui donne une vue de la façon dont le Narrateur obtient du texte de votre application et dont il organise les éléments dans l’interface utilisateur. AccScope est conçu de façon à pouvoir être utilisé et utile tout au long du cycle de développement d’une application, même pendant la phase de conception préliminaire. Pour plus d’informations, voir [AccScope](/windows/desktop/WinAuto/accscope).

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>Noms accessibles tirés de données dynamiques  
Windows prend en charge de nombreux contrôles qui peuvent servir à afficher des valeurs provenant d’une source de données associée, grâce à une fonctionnalité appelée *liaison de données* . Lors du remplissage de listes avec des éléments de données, vous devrez peut-être appliquer une technique qui définit les noms accessibles des éléments de listes liés aux données une fois la liste initiale remplie. Pour plus d’informations, voir « Scénario 4 » dans l’[Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample).

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>Noms accessibles et localisation  
Pour vous assurer que le nom accessible est également un élément localisé, vous devez appliquer des techniques correctes pour le stockage de chaînes localisables telles que les ressources, puis faire référence aux connexions de ressources avec des valeurs [directive x:Uid](../../xaml-platform/x-uid-directive.md). Si le nom accessible provient d’une utilisation [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) définie de manière explicite, assurez-vous que la chaîne est également localisable.

Notez que les propriétés jointes telles que les propriétés [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) utilisent une syntaxe de qualification spéciale pour le nom de la ressource, afin que la ressource fasse référence à la propriété jointe telle qu’elle est appliquée à un élément spécifique. Par exemple, le nom de ressource pour [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) appliqué à un élément d’interface utilisateur nommé `MediumButton` est : `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name` .

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name)
* [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Test de l’accessibilité](accessibility-testing.md)
