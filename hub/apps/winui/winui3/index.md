---
title: WinUI 3.0 Preview 1 (Mai 2020)
description: Vue d’ensemble de WinUI 3.0 Preview.
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 3aac14807f8455eb9a9294c40ddc76ddfa224659
ms.sourcegitcommit: 7e8c7f89212c88dcc0274c69d2c3365194c0954a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83688488"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Bibliothèque d’interface utilisateur Windows 3.0 Preview 1 (Mai 2020)

La bibliothèque d’interface utilisateur Windows (WinUI) 3.0 est une mise à jour majeure qui transforme WinUI en un framework d’expérience utilisateur complet pour tous les types d’applications Windows, de Win32 à UWP.

> [!Important]
> Cette préversion de WinUI 3.0 est destinée à une évaluation anticipée et à la collecte de commentaires de la communauté des développeurs. Il ne doit **PAS** être utilisé pour les applications de production.
>
> **Consultez [Limitations et problèmes connus dans Preview 1](#preview-1-limitations-and-known-issues)** .
## <a name="new-features-in-winui-30-preview-1"></a>Nouvelles fonctionnalités de Windows 3.0 Preview 1

- Possibilité de créer des applications de bureau avec WinUI, y compris [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) pour les applications Win32
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [Mises à jour de TabView](/windows/uwp/design/controls-and-patterns/tab-view)
- Mises à jour de thèmes sombres
- Améliorations et mises à jour de [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2)
  - Prise en charge des résolutions élevées
  - Prise en charge du redimensionnement et du déplacement de fenêtre
  - Mise à jour pour cibler une version plus récente d’Edge
  - Vous n’avez plus besoin de référencer un package NuGet propre à WebView2
- SwapChainPanel
- Améliorations requises pour la migration open source

Pour plus d’informations sur les avantages de WinUI 3.0, consultez la [feuille de route de la bibliothèque d’interface utilisateur Windows](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) sur GitHub.

### <a name="provide-feedback-and-suggestions"></a>Fournir des commentaires et des suggestions

Nous serions heureux de recevoir vos commentaires dans le [dépôt GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

## <a name="try-winui-30-preview-1"></a>Essayer WinUI 3.0 Preview 1

Configurez votre environnement de développement (pour obtenir des instructions détaillées, consultez [Configurer votre environnement de développement](#configure-your-dev-environment)), installez le VSIX WinUI 3.0 Preview 1 à partir du lien suivant et essayez les modèles de projet WinUI 3.0.

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

Vous pouvez également cloner et générer la version WinUI 3.0 Preview 1 de la [Galerie de contrôles XAML](#xaml-controls-gallery-winui-30-preview-1-branch).

### <a name="configure-your-dev-environment"></a>Configurer votre environnement de développement

Vérifiez que votre ordinateur de développement dispose de la mise à jour d’avril 2018 de Windows 10 (version 1803 - build 17134) ou d’une version plus récente.

Installez Visual Studio 2019, version 16.7 Preview 1. Vous pouvez le télécharger à partir de la page [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview).

Vous devez inclure les charges de travail suivantes lors de l’installation de Visual Studio Preview :

- Développement .NET Desktop
- Développement pour la plateforme Windows universelle

Pour créer des applications C++, vous devez également inclure les charges de travail suivantes :

- Développement Desktop en C++
- Le composant facultatif *Outils de plateforme Windows universelle C++ (v142)* pour la charge de travail Plateforme Windows universelle

### <a name="visual-studio-project-templates"></a>Modèles de projet Visual Studio

Pour accéder aux modèles de projet et WinUI 3.0 Preview 1, rendez-vous sur **https://aka.ms/winui3/previewdownload** .

Téléchargez l’extension Visual Studio (`.vsix`) pour ajouter les modèles de projet WinUI et le package NuGet à Visual Studio 2019.

Pour obtenir des instructions sur la façon d’ajouter le fichier `.vsix` à Visual Studio, consultez [Rechercher et installer des extensions Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box).

Après avoir installé l’extension `.vsix`, vous pouvez créer un projet WinUI 3.0 en recherchant « WinUI » et en sélectionnant l’un des modèles C++ ou C# disponibles.

![Modèles Visual Studio WinUI 3.0](images/WinUI3Templates.png)<br/>
*Exemples de modèles Visual Studio WinUI 3.0*

### <a name="visual-studio-project-configuration"></a>Configuration de projet Visual Studio

Quand vous créez un projet à l’aide de l’un des modèles WinUI 3.0 Preview 1, définissez Windows 10, version 1903 (build 18362) comme **Version cible** et Windows 10, version 1803 (build 17134) comme **Version minimale**.

Pour changer ces valeurs après avoir créé un projet, cliquez avec le bouton droit sur le projet dans l’**Explorateur de solutions** et sélectionnez **Propriétés**.

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>Création d’une application de bureau Win32 avec WinUI 3.0 Preview 1

Consultez [Bien démarrer avec WinUI 3.0 pour les applications de bureau](get-started-winui3-for-desktop.md).

Hormis les limitations et les problèmes connus décrits ci-dessous, la création d’une application à l’aide de WinUI 3.0 Preview 1 est très similaire à la création d’une application UWP avec XAML et WinUI 2.x. Par conséquent, la plupart des instructions et de la documentation pour les applications UWP et les API `Windows.UI` sont applicables.

## <a name="preview-1-limitations-and-known-issues"></a>Limitations et problèmes connus dans Preview 1

La version Preview 1 n’est, comme son nom l’indique, qu’une préversion. Les scénarios concernant les applications de bureau Win32 sont particulièrement nouveaux. Attendez-vous à des bogues, des limitations et des problèmes.

Voici quelques-uns des problèmes connus avec WinUI 3.0 Preview 1. Si vous détectez un problème qui ne figure pas dans la liste ci-dessous, faites-le nous savoir en contribuant à un problème existant ou en consignant un nouveau problème dans le dépôt [GitHub WinUI](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose).

### <a name="platform-and-os-support"></a>Prise en charge de plateforme et de système d’exploitation

WinUI 3.0 Preview 1 est compatible avec les PC qui exécutent la mise à jour d’avril 2018 de Windows 10 (version 1803 - build 17134) et versions ultérieures.

### <a name="developer-tools"></a>Outils de développement

- Les applications de bureau prennent en charge .NET 5 et C# 8, et doivent être empaquetées
- Les applications UWP prennent en charge .NET Native et C# 7.3
- IntelliSense est incomplet
- Aucun concepteur visuel
- Aucun rechargement à chaud
- Aucune arborescence visuelle dynamique
- Le développement avec VS Code n’est pas encore pris en charge
- Les nouvelles applications C++/CX ne sont pas prises en charge. Toutefois, vos applications existantes continueront à fonctionner (veuillez passer à C++/WinRT dès que possible)
- Le contenu WinUI 3.0 ne peut être que dans une fenêtre par processus ou un ApplicationView par application
- Le déploiement d’appareils de bureau sans package n’est pas pris en charge
- Aucune prise en charge d’ARM64

### <a name="missing-platform-features"></a>Fonctionnalités de plateforme manquantes

- Aucune prise en charge de Xbox
- Aucune prise en charge de HoloLens
- Aucune prise en charge des fenêtres contextuelles
- Aucune prise en charge du mode d’entrée manuscrite
- Acrylique en arrière-plan
- MediaElement et MediaPlayerElement
- RenderTargetBitmap
- MapControl
- Navigation hiérarchique avec NavigationView
- SwapChainPanel ne prend pas en charge la transparence
- Global Reveal utilise le comportement de secours, un pinceau plein
- XAML Islands n’est pas pris en charge dans cette version
- Les bibliothèques d’écosystèmes tiers ne fonctionneront pas complètement
- Les éditeurs IME ne fonctionnent pas
- Les méthodes sur l’espace de noms Windows.UI.Text ne peuvent pas être appelées

### <a name="known-issues"></a>Problèmes connus

- Dans les applications de bureau en C# :
   - Vous devez utiliser `WinRT.WeakReference<T>` plutôt que `System.WeakReference<T>` pour les références faibles aux objets Windows (notamment les objets XAML).
   - Les structs [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) et [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) ont des membres qui sont de type Float plutôt que de type Double.


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>Galerie de contrôles XAML (Branche WinUI 3.0 Preview 1)

Consultez la [branche WinUI 3.0 Preview 1 de la galerie de contrôles XAML](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview) pour obtenir un exemple d’application qui comprend tous les contrôles et fonctionnalités WinUI 3.0 Preview 1.

![Application XAML Controls Gallery WinUI 3.0 Preview 1](images/WinUI3XamlControlsGallery.png)<br/>
*Exemple de l’application XAML Controls Gallery WinUI 3.0 Preview 1*

Pour télécharger l’exemple, clonez la branche **winui3preview** à l’aide de la commande suivante :

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

Après le clonage, veillez à basculer vers la branche **winui3preview** dans votre environnement Git local :

> `git checkout winui3preview`
