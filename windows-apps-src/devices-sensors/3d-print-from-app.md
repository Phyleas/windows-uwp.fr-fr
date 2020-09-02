---
title: Impression 3D à partir de votre application
description: Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment lancer la boîte de dialogue d’impression 3D après s’être assuré que votre modèle 3D est imprimable et au format qui convient.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 3dprinting, impression 3D
ms.localizationpriority: medium
ms.openlocfilehash: 357d8bd3a460e61c436750fc4c9cbfbf8a8fcbfc
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362852"
---
# <a name="3d-printing-from-your-app"></a>Impression 3D à partir de votre application

**API importantes**

-   [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D)

Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment charger des données de géométrie 3D dans votre application et lancer la boîte de dialogue d’impression 3D après avoir vérifié que votre modèle 3D est imprimable et au format correct. Pour obtenir un exemple fonctionnel de ces procédures, consultez l ['exemple d’impression en 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> Dans l’exemple de code de ce guide, le rapport d’erreurs et la gestion sont considérablement simplifiés pour des raisons de simplicité.

## <a name="setup"></a>Installation


Dans votre classe d’application qui doit disposer de la fonctionnalité d’impression en 3D, ajoutez l’espace de noms [**Windows. Graphics. Printing3D**](/uwp/api/Windows.Graphics.Printing3D) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="Snippet3DPrintNamespace":::

Les espaces de noms supplémentaires suivants seront utilisés dans ce guide.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetOtherNamespaces":::

Ensuite, donnez à votre classe des champs de membre utiles. Déclarez un objet [**Print3DTask**](/uwp/api/Windows.Graphics.Printing3D.Print3DTask) pour représenter la tâche d’impression qui doit être transmise au pilote d’impression. Déclarez un objet [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) pour stocker le fichier de données 3D d’origine qui sera chargé dans l’application. Déclarez un objet [**Printing3D3MFPackage**](/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage) , qui représente un modèle 3D prêt pour l’impression avec toutes les métadonnées nécessaires.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetDeclareVars":::

## <a name="create-a-simple-ui"></a>Créer une interface utilisateur simple

Cet exemple présente trois contrôles utilisateur : un bouton charger qui place un fichier dans la mémoire du programme, un bouton corriger qui modifie le fichier en fonction des besoins et un bouton imprimer qui lance le travail d’impression. Le code suivant crée ces boutons (avec leurs gestionnaires d’événements de clic) dans le fichier XAML correspondant de la classe. cs.

:::code language="xml" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml" id="SnippetButtons":::

Ajoutez un **TextBlock** pour le commentaire de l’interface utilisateur.

:::code language="xml" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml" id="SnippetOutputText":::



## <a name="get-the-3d-data"></a>Obtenir les données 3D


La méthode par laquelle votre application acquiert les données de géométrie 3D à imprimer varie. Votre application peut récupérer des données à partir d’une analyse 3D, télécharger des données de modèle à partir d’une ressource Web ou générer un maillage 3D par programmation en utilisant des formules mathématiques ou une entrée utilisateur. Par souci de simplicité, ce guide montre comment charger un fichier de données 3D (d’un type de fichier courant) dans la mémoire du programme à partir d’un stockage de fichiers. La [bibliothèque de modèles 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) fournit plusieurs modèles que vous pouvez facilement télécharger sur votre appareil.

Dans votre `OnLoadClick` méthode, utilisez la classe [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour charger un fichier unique dans la mémoire de votre application.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetFileLoad":::

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Utiliser 3D Builder pour une conversion au format 3D Manufacturing Format (.3mf)

À ce stade, vous êtes en mesure de charger un fichier de données 3D dans la mémoire de votre application. Il existe de nombreux formats de données de géométrie 3D, mais ils ne sont pas tous adaptés à l’impression 3D. Windows 10 utilise le type de fichier 3D Manufacturing Format (.3mf) pour toutes les tâches d’impression 3D.

> [!NOTE]  
> Le type de fichier. 3mf offre plus de fonctionnalités que ce qui est abordé dans ce didacticiel. Pour en savoir plus sur les 3MF et les fonctionnalités qu’elle fournit aux producteurs et aux consommateurs de produits 3D, consultez la [spécification 3MF](https://3mf.io/what-is-3mf/3mf-specification/). Pour savoir comment utiliser ces fonctionnalités avec les API Windows 10, consultez le didacticiel [générer un package 3MF](./generate-3mf.md) .

L’application [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) peut ouvrir des fichiers des formats 3D les plus populaires et les enregistrer en tant que fichiers. 3mf. Dans cet exemple, où le type de fichier peut varier, une solution très simple consiste à ouvrir l’application de générateur 3D et à inviter l’utilisateur à enregistrer les données importées en tant que fichier. 3mf, puis à les recharger.

> [!NOTE]  
> En plus de convertir les formats de fichier, 3D Builder offre des outils simples pour modifier vos modèles, ajouter des données de couleur et effectuer d’autres opérations spécifiques de l’impression. C’est pourquoi il est souvent recommandé de l’intégrer à une application qui prend en charge l’impression 3D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetFileCheck":::

## <a name="repair-model-data-for-3d-printing"></a>Réparer des données de modèle pour l’impression 3D

Les données de modèle 3D ne sont pas toutes imprimables, même dans le type .3mf. Afin que l’imprimante puisse correctement déterminer les espaces à remplir et à laisser vides, le ou les modèles à imprimer doivent (chacun) constituer un ensemble unique et homogène, disposer de normales de surface orientées vers l’extérieur et d’une géométrie de collection. Les problèmes dans ces domaines peuvent se présenter sous différentes formes et peuvent être difficiles à repérer dans des formes complexes. Toutefois, les solutions logicielles modernes sont souvent adaptées à la conversion d’une géométrie brute en formes 3D imprimables. C’est ce qu’on appelle la *réparation* du modèle, qui est effectuée dans la méthode `OnFixClick`.

Le fichier de données 3D doit être converti pour implémenter [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream), qui peut ensuite être utilisé pour générer un objet [**Printing3DModel**](/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetRepairModel":::

L’objet **Printing3DModel** est maintenant réparé et imprimable. Utilisez [**SaveModelToPackageAsync**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) pour attribuer le modèle à l’objet **Printing3D3MFPackage** que vous avez déclaré pendant la création de la classe.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetSaveModel":::

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Exécuter la tâche d’impression : créer un gestionnaire TaskRequested


Par la suite, lorsque la boîte de dialogue d’impression 3D s’affiche et que l’utilisateur choisit de commencer l’impression, votre application doit transmettre les paramètres souhaités au pipeline d’impression 3D. L’API d’impression 3D déclenche l’événement **[TaskRequested](/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Vous devez écrire une méthode pour gérer cet événement de manière appropriée. Comme toujours, la méthode de gestionnaire doit être du même type que son événement : l’événement **TaskRequested** a un paramètre [**Print3DManager**](/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (une référence à son objet d’expéditeur) et un objet [**Print3DTaskRequestedEventArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs), qui contient la plupart des informations pertinentes.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetMyTaskTitle":::

L’objectif principal de cette méthode est d’utiliser le paramètre *args* pour envoyer un **Printing3D3MFPackage** dans le pipeline. Le type **Print3DTaskRequestedEventArgs** a une propriété : [**Request**](/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Elle est de type [**Print3DTaskRequest**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) et représente une demande de travail d’impression. Sa méthode [**CreateTask**](/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) autorise le programme à envoyer les informations correctes pour votre travail d’impression et retourne une référence à l’objet **Print3DTask** qui a été envoyé vers le pipeline d’impression 3D.

**CreateTask** a les paramètres d’entrée suivants : une chaîne pour le nom du travail d’impression, une chaîne pour l’ID de l’imprimante à utiliser, ainsi qu’un délégué [**Print3DTaskSourceRequestedHandler**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler). Le délégué est automatiquement appelé quand l’événement **3DTaskSourceRequested** est déclenché (cette opération est effectuée par l’API elle-même). Il est important de noter que ce délégué est appelé lorsqu’un travail d’impression est lancé et qu’il est responsable de la fourniture du package d’impression 3D approprié.

**Print3DTaskSourceRequestedHandler** prend un paramètre, un objet [**Print3DTaskSourceRequestedArgs**](/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) qui fournit les données à envoyer. [**SetSource**](/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), l’une des méthodes publiques de cette classe, accepte le package à imprimer. Implémentez un délégué **Print3DTaskSourceRequestedHandler** comme suit.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetSourceHandler":::

Ensuite, appelez **CreateTask**, à l’aide du délégué `sourceHandler` que vous venez de définir.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetCreateTask":::

L’objet **Print3DTask** renvoyé est attribué à la variable de classe déclarée au début. Vous pouvez maintenant (éventuellement) utiliser cette référence pour gérer certains événements levés par la tâche.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetOptional":::

> [!NOTE]  
> Vous devez implémenter des méthodes `Task_Submitting` et `Task_Completed` si vous voulez les inscrire à ces événements.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Exécuter la tâche d’impression : ouvrir la boîte de dialogue d’impression 3D


L’élément de code final nécessaire est celui qui lance la boîte de dialogue d’impression 3D. À l’instar d’une fenêtre de boîte de dialogue d’impression classique, la boîte de dialogue d’impression 3D fournit un certain nombre d’options d’impression de dernière minute et permet à l’utilisateur de choisir l’imprimante à utiliser (connectée par USB ou le réseau).

Inscrivez votre méthode `MyTaskRequested` à l’événement **TaskRequested**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetRegisterMyTaskRequested":::

Après avoir inscrit votre gestionnaire d’événements **TaskRequested**, vous pouvez appeler la méthode [**ShowPrintUIAsync**](/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), qui fait apparaître la boîte de dialogue d’impression 3D dans la fenêtre d’application active.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetShowDialog":::

Enfin, il est conseillé de désinscrire vos gestionnaires d’événements une fois que votre application reprend le contrôle.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/MainPage.xaml.cs" id="SnippetDeregisterMyTaskRequested":::

## <a name="related-topics"></a>Rubriques connexes

[Générer un package 3MF](./generate-3mf.md)  
[Exemple d’impression 3D UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
