---
title: Liste des packages NuGet pour la bibliothèque d’IU Windows
description: Liste les packages NuGet pour la bibliothèque d’interface utilisateur Windows
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, sdk kit de ressources
ms.openlocfilehash: ca3f2915d77bb83f45744e90bd86e82edba013b8
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492884"
---
# <a name="windows-ui-library-nuget-packages"></a>Packages NuGet de la bibliothèque d’IU Windows

NuGet est un gestionnaire de package standard pour les applications .Net qui est intégré à Visual Studio. Dans votre solution ouverte, choisissez le menu *Outils*, *Gestionnaire de package NuGet*, *Gérer les packages NuGet pour la solution...* afin d’ouvrir l’interface utilisateur.  Entrez l’un des noms de packages ci-dessous pour le rechercher en ligne.

| Nom du package NuGet | Description |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | Contrôles pour les applications UWP. Comprend les API de ces espaces de noms : [Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml), [Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers), [Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls), [Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives), [Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes), [Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media), [Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | Vous permet d’utiliser des API [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) sur des versions antérieures de Windows 10 sans avoir à écrire de code spécial pour gérer plusieurs versions cibles de Windows 10. |


## <a name="search-in-visual-studio"></a>Rechercher dans Visual Studio

En recherchant dans le gestionnaire de package Visual Studio, vous devriez voir une liste semblable à celle-ci (les numéros de version peuvent être différents, mais les noms devraient être identiques).

![Gestionnaire de package NuGet](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>Mettre à jour les packages NuGet

Nous mettons régulièrement à jour la bibliothèque d’IU Windows avec de nouveaux contrôles, services, API, et plus important encore, avec de nouvelles corrections de bogues. Pour vérifier que vous disposez de la dernière version, ouvrez votre projet dans Visual Studio, choisissez le menu **Outils**, sélectionnez **Gestionnaire de package NuGet** -> **Gérer les packages NuGet pour la solution...** et sélectionnez l’onglet *Mises à jour*. Sélectionnez le package que vous souhaitez mettre à jour, puis cliquez sur Installer pour effectuer une mise à jour vers la version la plus récente.

## <a name="getting-started"></a>Prise en main

Pour plus d’informations sur l’utilisation de ces packages NuGet dans vos propres projets, consultez [Bien démarrer avec la bibliothèque d’interface utilisateur Windows](getting-started.md).
