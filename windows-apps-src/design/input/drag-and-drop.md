---
description: Cet article explique comment ajouter une opération de glisser-déplacer dans votre application Windows.
title: Glisser-déposer
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9958aab20c13f0104ca1a52c6fccda33c00f6281
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159963"
---
# <a name="drag-and-drop"></a>Glisser-déposer

Le glisser-déplacer est un moyen intuitif pour transférer des données dans une application ou entre des applications sur le bureau Windows. La fonction glisser-déplacer permet à l’utilisateur de transférer des données entre des applications ou au sein d’une application à l’aide d’un mouvement standard (appuyez sur la pression et le panoramique avec le doigt ou la pression et le panoramique avec une souris ou un stylet).

> **API importantes**: [propriété CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag), [propriété AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop) 

La source de glissement, qui est l’application ou la zone dans laquelle le mouvement de glissement est déclenché, fournit les données à transférer en remplissant un objet de package de données qui peut contenir des formats de données standard, notamment du texte, du RTF, du HTML, des bitmaps, des éléments de stockage ou des formats de données personnalisés. La source indique également le type d’opérations qu’elle prend en charge : copie, déplacement ou liaison. Lorsque le pointeur est relâché, Drop se produit. La cible de déplacement, qui est l’application ou la zone située sous le pointeur, traite le package de données et retourne le type de l’opération effectuée.

Pendant le glisser-déplacer, l’interface utilisateur du glissement fournit une indication visuelle du type d’opération de glisser-déplacer qui a lieu. Ce commentaire visuel est initialement fourni par la source, mais peut être modifié par les cibles à mesure que le pointeur se trouve au-dessus de lui.

Le glisser-déplacer moderne est disponible sur tous les appareils qui prennent en charge UWP. Il permet le transfert de données entre ou dans n’importe quel type d’application, y compris les applications Windows classiques, bien que cet article se concentre sur l’API XAML pour le glisser-déplacer moderne. Une fois implémenté, le glisser-déplacer fonctionne parfaitement dans toutes les directions, notamment d’application à application, d’application à bureau et de bureau à application.

Voici une vue d’ensemble de ce que vous devez faire pour activer le glisser-déplacer dans votre application :

1. Activez le glissement sur un élément en affectant à sa propriété **CanDrag** la valeur true.  
2. Générez le package de données. Le système gère automatiquement les images et le texte, mais pour d’autres contenus, vous devez gérer les événements **DragStarted** et **DragCompleted** et les utiliser pour créer votre propre package de données. 
3. Activez la suppression en affectant à la propriété **AllowDrop** la **valeur true** pour tous les éléments qui peuvent recevoir du contenu supprimé. 
4. Gérez l’événement **DragOver** pour permettre au système de savoir quel type d’opérations de glisser l’élément peut recevoir. 
5. Traitez l’événement **Drop** pour recevoir le contenu déplacé. 



## <a name="enable-dragging"></a>Activer le glissement

Pour activer le glissement sur un élément, affectez à sa propriété [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag) la **valeur true**. Cela rend l’élément et les éléments qu’il contient, dans le cas de collections comme ListView--glisseable.

Soyez précis sur ce qui peut être glissé. Les utilisateurs ne veulent pas faire glisser tout dans votre application, mais uniquement certains éléments, tels que des images ou du texte. 

Voici comment définir [**CanDrag**](/uwp/api/windows.ui.xaml.uielement.candrag).

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

Vous n’avez besoin d’effectuer aucune autre action pour autoriser le glissement, sauf si vous souhaitez personnaliser l’interface utilisateur (le sujet est abordé plus loin dans cet article). L’opération Déplacer nécessite quelques étapes supplémentaires.

## <a name="construct-a-data-package"></a>Construire un package de données 

Dans la plupart des cas, le système créera un package de données pour vous. Le système gère automatiquement les éléments suivants :
* Images
* Text 

Pour d’autres contenus, vous devez gérer les événements **DragStarted** et **DragCompleted** et les utiliser pour construire votre propre [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage).

## <a name="enable-dropping"></a>Activer la suppression

Le balisage suivant montre comment définir une zone spécifique de l’application valide pour l’opération Déplacer à l’aide de l’élément [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) en XAML. Si un utilisateur tente d’effectuer le déplacement vers un autre emplacement, le système l’en empêche. Si vous souhaitez que les utilisateurs puissent déplacer des éléments n’importe où dans votre application, définissez l’ensemble de l’arrière-plan en tant que cible de l’opération Déplacer.

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>Gérer l’événement DragOver

L’événement [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover) se déclenche lorsqu’un utilisateur a fait glisser un élément sur votre application, mais pas encore de le supprimer. Dans ce gestionnaire, vous devez spécifier le type d’opérations que votre application prend en charge à l’aide de la propriété [**AcceptedOperation**](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation). L’opération Copier est la plus courante.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>Traiter l’événement Drop

L’événement [**Drop**](/uwp/api/windows.ui.xaml.uielement.drop) se produit lorsque l’utilisateur relâche des éléments dans une zone de dépôt valide. Traitez-les à l’aide de la propriété [**DataView**](/uwp/api/windows.ui.xaml.drageventargs.dataview).

Par souci de simplicité dans l’exemple ci-dessous, nous supposons que l’utilisateur a déposé une photo unique et y accède directement. En réalité, les utilisateurs peuvent déplacer plusieurs éléments de formats divers simultanément. Votre application doit gérer cette possibilité en vérifiant les types de fichiers qui ont été supprimés et le nombre de fichiers qui ont été supprimés, et traiter chacun d’eux en conséquence. Vous devez également envisager de notifier l’utilisateur s’il tente d’effectuer une action que votre application ne prend pas en charge.

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>Personnaliser l’interface utilisateur

Le système fournit une interface utilisateur par défaut pour le glisser-déposer. Toutefois, vous pouvez également choisir de personnaliser les différentes parties de l’interface utilisateur en définissant des légendes et des glyphes personnalisés, ou en choisissant de ne pas afficher d’interface utilisateur du tout. Pour personnaliser l’interface utilisateur, utilisez la propriété [**DragEventArgs.DragUIOverride**](/uwp/api/windows.ui.xaml.drageventargs.draguioverride).

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>Ouvrir un menu contextuel sur un élément que vous pouvez faire glisser avec une interface tactile

Quand vous utilisez une interface tactile, pour faire glisser un élément [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) et ouvrir son menu contextuel, vous utilisez des mouvements tactiles similaires qui commencent tous deux par un appui prolongé. Voici comment le système lève l’ambiguïté entre les deux actions pour les éléments de votre application qui prennent en charge les deux opérations : 

* Si un utilisateur appuie de façon prolongée sur un élément et commence à le faire glisser dans un intervalle de 500 millisecondes, l’élément est déplacé et le menu contextuel n’est pas affiché. 
* Si l’utilisateur appuie de façon prolongée sur l’élément, mais ne le fait pas glisser dans l’intervalle de 500 millisecondes, le menu contextuel est ouvert. 
* Une fois que le menu contextuel est ouvert, si l’utilisateur essaie de faire glisser l’élément (sans lever le doigt), le menu contextuel se ferme et le déplacement commence.

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>Désigner un élément dans un contrôle ListView ou GridView en tant que dossier

Vous pouvez spécifier un contrôle [**ListViewItem**](/uwp/api/Windows.UI.Xaml.Controls.ListViewItem) ou [**GridViewItem**](/uwp/api/Windows.UI.Xaml.Controls.GridViewItem) en tant que dossier. Ceci est particulièrement utile dans les scénarios TreeView et Explorateur de fichiers. Pour ce faire, définissez explicitement la propriété [**AllowDrop**](/uwp/api/windows.ui.xaml.uielement.allowdrop) sur **True** pour l’élément concerné. 

Le système montre automatiquement les animations appropriées pour le déplacement dans un dossier plutôt que dans un élément autre qu’un dossier. Votre code d’application doit continuer à gérer l’événement de [**déplacement**](/uwp/api/windows.ui.xaml.uielement.drop) sur l’élément de dossier (ainsi que sur l’élément qui n’est pas un dossier) afin de mettre à jour la source de données et d’ajouter l’élément déplacé dans le dossier cible.

## <a name="implementing-custom-drag-and-drop"></a>Implémentation du glisser-déplacer personnalisé

La classe [UIElement](/uwp/api/windows.ui.xaml.uielement) effectue la plupart des tâches d’implémentation du glisser-déplacer pour vous. Toutefois, si vous le souhaitez, vous pouvez implémenter votre propre version à l’aide des API de l' [espace de noms Windows. ApplicationModel. datatransfer. DragDrop. Core](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core).

| Fonctionnalité | API WinRT |
| --- | --- |
|  Activer le glissement | [CoreDragOperation](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  Créer un package de données | [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| Faire glisser vers l’interpréteur de commandes  | [CoreDragOperation.StartAsync](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| Recevoir des rejets de l’interpréteur de commandes  | [CoreDragDropManager](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>Voir aussi

* [Communication entre applications](index.md)
* [AllowDrop](/uwp/api/windows.ui.xaml.uielement.allowdrop)
* [CanDrag](/uwp/api/windows.ui.xaml.uielement.candrag)
* [DragOver](/uwp/api/windows.ui.xaml.uielement.dragover)
* [AcceptedOperation](/uwp/api/windows.ui.xaml.drageventargs.acceptedoperation)
* [DataView](/uwp/api/windows.ui.xaml.drageventargs.dataview)
* [DragUIOverride](/uwp/api/windows.ui.xaml.drageventargs.draguioverride)
* [Déplacez](/uwp/api/windows.ui.xaml.uielement.drop)
* [IsDragSource](/uwp/api/windows.ui.xaml.controls.listviewbase.isdragsource)