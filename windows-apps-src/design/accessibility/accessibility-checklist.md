---
description: Fournit une liste de vérification pour vous aider à vous assurer que votre application Windows est accessible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Liste de vérification de l’accessibilité
label: Accessibility checklist
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd16c93b3914987741a486e4f40d4b60e0b274ee
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032712"
---
# <a name="accessibility-checklist"></a>Liste de vérification de l’accessibilité

Fournit une liste de vérification pour vous aider à vous assurer que votre application Windows est accessible.

Nous fournissons ici une liste de vérification qui vous permet de vous assurer que votre application est accessible.

1. Définissez le nom accessible (obligatoire) et la description accessible (facultative) du contenu et des éléments d’interface utilisateur interactifs de votre application.

    Le nom accessible est une chaîne de texte courte et descriptive qui est utilisée par les lecteurs d’écran pour présenter un élément d’interface utilisateur. Certains éléments d’interface utilisateur tels que [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) et [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) effectuent la promotion de leur contenu texte comme nom accessible par défaut ; voir [Informations d’accessibilité élémentaires](basic-accessibility-information.md#name_from_inner_text).

    Vous devez définir le nom accessible de manière explicite pour les images ou autres contrôles qui n’effectuent pas la promotion du contenu de texte interne comme nom accessible implicite. Vous devez utiliser des étiquettes pour les éléments de formulaires afin que le texte d’étiquette puisse être utilisé comme cible [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) dans le modèle Microsoft UI Automation pour la corrélation entre les étiquettes et les entrées. Si vous souhaitez fournir davantage d’instructions dans l’interface utilisateur que celles normalement fournies par le nom accessible, des descriptions accessibles et des info-bulles aident les utilisateurs à mieux comprendre l’interface utilisateur.

    Pour plus d’informations, voir les sections [Nom accessible](basic-accessibility-information.md#accessible_name) et [Description accessible](basic-accessibility-information.md).

2. Mettez en œuvre l’accessibilité du clavier :

    * Testez l’ordre d’index de tabulation par défaut pour une interface utilisateur. Ajustez l’ordre d’index de tabulation si nécessaire, ce qui peut exiger l’activation ou la désactivation de certains contrôles ou la modification des valeurs par défaut de [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) sur certains éléments d’interface utilisateur.
    * Utilisez des contrôles qui prennent en charge la navigation à l’aide des touches de direction pour les éléments composites. Pour les contrôles par défaut, la navigation à l’aide des touches de direction est en général déjà implémentée.
    * Utilisez des contrôles qui prennent en charge l’activation du clavier. Pour les contrôles par défaut, en particulier ceux qui prennent en charge le modèle [**Invoke**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) UI Automation, l’activation du clavier est généralement disponible ; vérifiez la documentation de ce contrôle.
    * Définissez des touches d’accès rapide ou mettez en œuvre des touches accélérateur pour les parties spécifiques de l’interface utilisateur qui prennent en charge l’interaction.
    * Pour tout contrôle personnalisé que vous utilisez dans votre interface utilisateur, vérifiez que vous avez mis en œuvre ces contrôles avec la prise en charge [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) correcte pour l’activation et que vous avez défini des substitutions pour la gestion des touches selon les besoins pour prendre en charge l’activation, la traversée et les touches d’accès rapide ou accélérateur.

    Pour plus d’informations, voir [Interactions avec le clavier](../input/keyboard-interactions.md).

3. Vérifier que le texte est une taille lisible

    * Windows comprend différents outils et paramètres d’accessibilité dont les utilisateurs peuvent tirer parti et s’adapter à leurs propres besoins et préférences pour lire du texte. Elles incluent notamment :
        * L’outil loupe, qui agrandit la zone sélectionnée de l’interface utilisateur. Vous devez vous assurer que la disposition du texte dans votre application ne complique pas l’utilisation de la loupe pour la lecture.
        * Les paramètres de mise à l’échelle et de résolution globaux dans **paramètres->>système d’affichage->de l’échelle et de la disposition** . Les options de dimensionnement disponibles peuvent varier en fonction des capacités du périphérique d’affichage.
        * Paramètres de taille du texte dans **paramètres->facilité d’accès->affichage** . Ajustez le paramètre **agrandir le texte** pour spécifier uniquement la taille du texte dans les contrôles de prise en charge pour l’ensemble des applications et écrans (tous les contrôles de texte UWP prennent en charge l’expérience de mise à l’échelle du texte sans personnalisation ni création de modèles).
        > [!NOTE]
        > Le paramètre **rendre tout** le plus grand permet à un utilisateur de spécifier la taille préférée du texte et des applications en général sur leur écran principal uniquement.

4. Vérifiez visuellement votre interface utilisateur pour vous assurer que le contraste du texte est suffisant, que le rendu des éléments est correct dans les thèmes à contraste élevé et que les couleurs sont utilisées correctement.

    * Utilisez un outil d’analyse des couleurs pour vérifier que le coefficient de contraste de texte visuel est au moins de 4,5 pour 1.
    * Basculez vers un thème à contraste élevé et vérifiez que l’interface utilisateur de votre application est lisible et utilisable.
    * Assurez-vous que votre interface utilisateur n’utilise pas la couleur comme seule façon de transmettre les informations.

    Pour plus d’informations, voir les rubriques [Thèmes à contraste élevé](high-contrast-themes.md) et [Exigences de texte accessible](accessible-text-requirements.md).

5. Exécutez les outils d’accessibilité, traitez les problèmes signalés et vérifiez l’expérience de lecture d’écran.

    Utilisez des outils tels que [**Inspect**](/windows/desktop/WinAuto/inspect-objects) pour vérifier l’accès par programme, exécutez des outils de diagnostic tels que [**AccChecker**](/windows/desktop/WinAuto/ui-accessibility-checker) pour identifier les erreurs courantes et vérifiez l’expérience de lecture d’écran avec le Narrateur.

    Pour plus d’informations, voir [Tests d’accessibilité](accessibility-testing.md).

6. Assurez-vous que vos paramètres de manifeste d’application respectent les recommandations en matière d’accessibilité.

7. Déclarez votre application comme accessible dans le Microsoft Store.

    Si vous avez implémenté la prise en charge de l’accessibilité de base, la déclaration de votre application comme accessible dans le Microsoft Store peut aider à atteindre davantage de clients et à obtenir des évaluations supplémentaires.

    Pour plus d’informations, voir [Accessibilité dans le Windows Store](accessibility-in-the-store.md).

## <a name="related-topics"></a>Rubriques connexes  

* [Exigences de texte accessible](accessible-text-requirements.md)
* [Mise à l’échelle du texte](../input/text-scaling.md)
* [Accessibilité](accessibility.md)
* [Concevoir des applications pour l’accessibilité](./accessibility-overview.md)
* [Pratiques à éviter](practices-to-avoid.md)
