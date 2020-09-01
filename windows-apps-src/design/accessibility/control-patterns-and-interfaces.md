---
Description: Répertorie les modèles de contrôle Microsoft UI Automation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Modèles de contrôle et interfaces
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: adbe1556f48e2f9b362faa303be52586714c73d5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157023"
---
# <a name="control-patterns-and-interfaces"></a>Modèles de contrôle et interfaces  



Répertorie les modèles de contrôle Microsoft UI Automation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter.

Le tableau présenté dans cette rubrique décrit les modèles de contrôle Microsoft UI Automation. Par ailleurs, ce tableau recense les classes utilisées par les clients UI Automation pour accéder aux modèles de contrôle et aux interfaces utilisées par les fournisseurs UI Automation pour les implémenter. La colonne **Control pattern** indique le nom du modèle du point de vue du client UI Automation, sous la forme d’une valeur de constante répertoriée dans [**Control Pattern Availability Property Identifiers**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids). Du point de vue du fournisseur UI Automation, chacun de ces modèles est un nom de constante [**patternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) . La colonne **Class provider interface** indique le nom de l’interface que les fournisseurs implémentent pour proposer ce modèle pour un contrôle XAML personnalisé.

Pour plus d’informations sur l’implémentation d’homologues d’automatisation personnalisés qui exposent des modèles de contrôle et implémentent les interfaces, voir [Homologues d’automatisation personnalisés](custom-automation-peers.md).

Quand vous implémentez un modèle de contrôle, vous devez aussi consulter la documentation sur le fournisseur UI Automation qui décrit certaines des attentes qu’auront les clients concernant un modèle de contrôle, indépendamment de l’infrastructure de l’interface utilisateur utilisée pour l’implémenter. Certaines des informations répertoriées dans la documentation générale du fournisseur UI Automation influenceront votre implémentation de vos homologues et la prise en charge correcte de ce modèle. Reportez-vous à la rubrique [Implémentation de modèles de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns), et consultez la page qui documente le modèle que vous envisagez d’implémenter.

| Classe du modèle | Interface du fournisseur de classes | Description |
|-----------------|--------------------------|-------------|
| **Annotation** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | Utilisé pour exposer les propriétés d’une annotation dans un document. |
| **Ancrer** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | Utilisées pour les contrôles qui peuvent être ancrés dans un conteneur d’ancrage. Par exemple, les barres d’outils ou les palettes d’outils. |
| **Déplacez** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | Utilisé pour prendre en charge les contrôles pouvant être glissés, ou les contrôles qui comportent des éléments pouvant être glissés. |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | Utilisé pour prendre en charge les contrôles qui peuvent être la cible d’une opération glisser-déplacer. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | Utilisé pour prendre en charge les contrôles qui se développent visuellement pour afficher plus de contenu et qui se réduisent pour masquer du contenu. |
| **Grid** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | Utilisées pour les contrôles qui prennent en charge des fonctionnalités de grille telles que le dimensionnement et le déplacement vers une cellule spécifiée. La grille proprement dite n’implémente pas ce modèle, car bien qu’elle fournisse la disposition, il ne s’agit pas d’un contrôle. |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | Utilisées pour les contrôles dont les grilles contiennent des cellules. |
| **Appeler** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | Utilisé pour les contrôles qui peuvent être appelés, tels qu’un  [**bouton**](/uwp/api/Windows.UI.Xaml.Controls.Button). |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | Permet aux applications de rechercher un élément dans un conteneur, tel qu’une liste virtualisée. |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | Utilisées pour les contrôles qui peuvent basculer entre plusieurs représentations du même ensemble d’informations, de données ou d’enfants. |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | Utilisé pour exposer un pointeur à un modèle objet sous-jacent d’un document. |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | Utilisées pour les contrôles disposant d’une plage de valeurs qui peut s’appliquer au contrôle. Par exemple, un contrôle Spinner contenant des années peut avoir une plage s’échelonnant de 1900 à l’année en cours, pendant qu’un autre contrôle Spinner représentant les mois aura une plage allant de 1 à 12. |
| **Faire défiler** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | Utilisées pour les contrôles qui peuvent défiler. Par exemple, un contrôle disposant de barres de défilement qui sont actives lorsque la quantité d’informations est trop importante pour être affichée dans la zone affichable du contrôle. |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | Utilisées pour les contrôles qui disposent d’éléments individuels dans une liste déroulante. Par exemple, un contrôle de liste qui dispose d’éléments individuels dans la liste déroulante, comme un contrôle zone de liste déroulante. |
| **Sélection** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | Utilisées pour les contrôles conteneur de sélection. Par exemple, [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) et [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox). |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | Utilisées pour les éléments individuels dans les contrôles conteneur de sélection, tels que les zones de liste et zones de liste modifiables. |
| **Feuille de calcul** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | Utilisé pour exposer le contenu d’une feuille de calcul ou d’un autre document de type grille. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | Utilisé pour exposer les propriétés d’une cellule se trouvant dans une feuille de calcul ou un autre document de type grille. |
| **Styles** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | Utilisé pour décrire un élément d’interface utilisateur ayant un style, une couleur de remplissage, un motif de remplissage ou une forme spécifiques. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | Permet à des applications clientes UI Automation de diriger l’entrée de souris ou de clavier vers un élément spécifique de l’interface utilisateur. |
| **Table** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | Utilisées pour les contrôles qui disposent d’une grille ainsi que d’informations d’en-tête. Par exemple, un contrôle de calendrier tabulaire. |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | Utilisées pour les éléments d’une table. |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | Utilisées pour les contrôles d’édition et les documents qui exposent des informations textuelles. Voir aussi [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) et [**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | Utilisé pour accéder à l’ancêtre le plus proche d’un élément qui prend en charge le modèle de contrôle **Text**. |
| **TextEdit** | Aucune classe managée disponible | Fournit l’accès à un contrôle qui modifie du texte, par exemple un contrôle qui effectue une correction automatique ou permet une composition d’entrée via un éditeur de méthode d’entrée (IME). |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | Fournit l’accès à une étendue de texte continu au sein d’un conteneur de texte qui implémente [**ITextProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider). Voir aussi [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Bascule** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | Utilisées pour les contrôles dont l’état peut être activé et désactivé. Par exemple, [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) et les éléments de menu qui peuvent être activés. |
| **Transformer** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | Utilisées pour les contrôles qui peuvent être redimensionnés, déplacés et pivotés. Les utilisations courantes du modèle de contrôle Transform se font dans les concepteurs, les formulaires les éditeurs graphiques et les applications de dessin. |
| **Valeur** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | Permet aux clients d’obtenir ou de définir une valeur sur des contrôles qui ne prennent pas en charge une plage de valeurs. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | Expose les éléments figurant à l’intérieur de conteneurs qui sont virtualisés et doivent être entièrement accessibles en tant qu’éléments UI Automation. |
| **Window** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Expose des informations spécifiques aux fenêtres, concept fondamental du système d’exploitation Microsoft Windows. Les fenêtres et les boîtes de dialogue enfants sont des exemples de contrôles correspondant à des fenêtres. |

> [!NOTE]
> Vous ne trouverez pas nécessairement des implémentations de tous ces modèles dans les contrôles XAML existants. Certains de ces modèles ont des interfaces uniquement pour prendre en charge la parité avec la définition de l’infrastructure UI Automation générale des modèles, ainsi que pour prendre en charge des scénarios d’homologues d’automation qui nécessiteront une implémentation purement personnalisée pour prendre en charge ce modèle.

> [!NOTE]
> Les applications du Windows Phone Store ne prennent pas en charge tous les modèles de contrôle UI Automation répertoriés ici. **Annotation**, **Dock**, **Drag**, **DropTarget** et **ObjectModel** font partie des modèles non pris en charge.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Homologues d’automation personnalisés](custom-automation-peers.md)
* [Accessibilité](accessibility.md)