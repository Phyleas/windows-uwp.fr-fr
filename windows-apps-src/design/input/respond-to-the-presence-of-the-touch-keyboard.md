---
Description: Découvrez comment personnaliser l’interface utilisateur de votre application lorsque le clavier tactile est affiché ou masqué.
title: Répondre à la présence du clavier tactile
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: clavier, accessibilité, navigation, Focus, texte, entrée, interactions de l’utilisateur
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: 76b468eedd136522a4af9fb5880049278548865d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970264"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Répondre à la présence du clavier tactile

Découvrez comment personnaliser l’interface utilisateur de votre application lorsque le clavier tactile est affiché ou masqué.

### <a name="important-apis"></a>API importantes

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![clavier tactile en mode de disposition classique](images/keyboard/default.png)

<sup>Clavier tactile en mode de disposition par défaut</sup>

Le clavier tactile permet l’entrée de texte pour les appareils qui prennent en charge les fonctions tactiles. Les contrôles d’entrée de texte de l’application Windows appellent le clavier tactile par défaut lorsqu’un utilisateur appuie sur un champ d’entrée modifiable. Le clavier tactile reste généralement visible pendant que l’utilisateur navigue entre les contrôles d’un formulaire, mais ce comportement peut varier selon les autres types de contrôles que contient le formulaire.

Pour prendre en charge le comportement de clavier tactile correspondant dans un contrôle de saisie de texte personnalisé qui ne dérive pas d’un contrôle de saisie de texte standard, vous devez utiliser la classe <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> pour exposer vos contrôles à Microsoft UI Automation et mettre en œuvre les modèles de contrôles UI Automation appropriés. Voir [Accessibilité du clavier](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) et [Homologues d’automation personnalisés](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

Une fois cette prise en charge ajoutée à votre contrôle personnalisé, vous pouvez répondre de manière appropriée à la présence du clavier tactile.

**Configuration requise :**

Cette rubrique s’appuie sur l’article [Interactions avec le clavier](keyboard-interactions.md).

Vous devez posséder des connaissances de base sur les interactions avec le clavier standard, sur la gestion de la saisie au clavier et des événements de clavier et sur UI Automation.

Si vous débutez dans le développement d’applications d’application Windows, consultez ces rubriques pour vous familiariser avec les technologies abordées ici.

- [Créer votre première application](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Instructions relatives à l’expérience utilisateur :**

Pour obtenir des conseils utiles sur la conception d’une application utile et attrayante, optimisée pour l’entrée au clavier, consultez interactions avec le [clavier](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Clavier tactile et interface utilisateur personnalisée

Voici quelques recommandations de base concernant les contrôles de saisie de texte personnalisés.

- Affichez le clavier tactile tout au long de l’interaction avec votre formulaire.

- Assurez-vous que vos contrôles personnalisés possèdent le [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) d’automatisation d’interface utilisateur approprié pour le clavier lorsque le focus se déplace d’un champ d’entrée de texte dans le contexte d’une entrée de texte. Par exemple, si vous avez un menu qui est ouvert au milieu d’un scénario d’entrée de texte et que vous souhaitez que le clavier reste affiché, le menu doit avoir le **AutomationControlType** de Menu.

- Ne manipulez pas les propriétés UI Automation pour contrôler le clavier tactile. D’autres outils d’accessibilité reposent sur la précision des propriétés UI Automation.

- Assurez-vous que les utilisateurs peuvent toujours voir le champ d’entrée avec lequel ils interagissent.

    Étant donné que le clavier tactile occludes une grande partie de l’écran, Windows garantit que le champ d’entrée avec le focus défile dans la vue lorsqu’un utilisateur parcourt les contrôles du formulaire, y compris les contrôles qui ne sont pas actuellement en vue.

    Lorsque vous personnalisez votre interface utilisateur, fournissez un comportement similaire sur l’apparence du clavier tactile en gérant l' [affichage](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) et le [masquage](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) des événements exposés par l’objet [**InputPane**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) .

    ![formulaire avec et sans clavier tactile apparent](images/touch-keyboard-pan1.png)

    Dans certains cas, il existe des éléments d’interface utilisateur qui doivent rester tout le temps à l’écran. Concevez l’interface utilisateur de sorte que les contrôles de formulaire se trouvent dans une région panoramique et que les éléments d’interface utilisateur importants soient statiques. Par exemple :

    ![formulaire contenant des zones devant toujours rester affichées](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Gestion des événements d’affichage et de masquage

Voici un exemple de liaison de gestionnaires d’événements pour l' [illustration](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) et le [masquage](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) des événements du clavier tactile.

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>Articles connexes

- [Interactions du clavier](keyboard-interactions.md)
- [Accessibilité du clavier](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)
- [Homologues d’automation personnalisés](https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers)

### <a name="samples"></a>Exemples

- [Exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

### <a name="archive-samples"></a>Exemples d’archive

- [Entrée : exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [Exemple de réponse à l’apparition du Clavier visuel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [Exemple de modification de texte XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
- [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
