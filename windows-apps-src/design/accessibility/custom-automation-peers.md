---
Description: Décrit le concept d’homologues Automation pour Microsoft UI Automation et la façon dont vous pouvez assurer la prise en charge de l’automatisation pour votre propre classe d’interface utilisateur personnalisée.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Homologues Automation personnalisés
label: Custom automation peers
template: detail.hbs
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c6bc6996e80aacbd9eec0f37127a1dd24ec0e24e
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690407"
---
# <a name="custom-automation-peers"></a>Homologues Automation personnalisés  

Décrit le concept d’homologues Automation pour Microsoft UI Automation et la façon dont vous pouvez assurer la prise en charge de l’automatisation pour votre propre classe d’interface utilisateur personnalisée.

UI Automation fournit une infrastructure que les clients Automation peuvent utiliser pour examiner ou utiliser les interfaces utilisateur de diverses plateformes et infrastructures d’interface utilisateur. Si vous écrivez une application plateforme Windows universelle (UWP), les classes que vous utilisez pour votre interface utilisateur fournissent déjà la prise en charge d’Automation d’interface utilisateur. Vous pouvez dériver des classes existantes non scellées pour définir un nouveau type de contrôle d’interface utilisateur ou de classe de prise en charge. Dans ce cas, votre classe peut ajouter un comportement qui doit avoir une prise en charge de l’accessibilité, mais que la prise en charge d’UI Automation par défaut ne couvre pas. Dans ce cas, vous devez étendre la prise en charge d’Automation d’interface utilisateur existante en dérivant de la classe [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) utilisée par l’implémentation de base, en ajoutant la prise en charge nécessaire à l’implémentation de votre homologue et en informant le plateforme Windows universelle (UWP). infrastructure de contrôle qu’il doit créer votre homologue.

UI Automation permet non seulement aux applications d’accessibilité et aux technologies d’assistance, telles que les lecteurs d’écran, mais également au code d’assurance qualité (test). Dans les deux cas, les clients UI Automation peuvent examiner les éléments de l’interface utilisateur et simuler l’interaction de l’utilisateur avec votre application à partir d’un autre code en dehors de votre application. Pour plus d’informations sur UI Automation sur toutes les plateformes et dans sa signification plus étendue, consultez [vue d’ensemble d’UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview).

Il existe deux audiences distinctes qui utilisent l’infrastructure UI Automation.

* Les  ***clients* UI Automation** appellent les API UI Automation pour en savoir plus sur toute l’interface utilisateur actuellement affichée pour l’utilisateur. Par exemple, une technologie d’assistance, telle qu’un lecteur d’écran, agit comme un client UI Automation. L’interface utilisateur est présentée sous la forme d’une arborescence d’éléments d’automatisation associés. Le client UI Automation peut être intéressé par une seule application à la fois ou dans toute l’arborescence. Le client UI Automation peut utiliser des API UI Automation pour naviguer dans l’arborescence et lire ou modifier des informations dans les éléments Automation.
* Les  ***fournisseurs* UI Automation** apportent des informations à l’arborescence UI Automation en implémentant des API qui exposent les éléments de l’interface utilisateur qu’ils ont introduits dans le cadre de leur application. Lorsque vous créez un nouveau contrôle, vous devez maintenant agir en tant que participant dans le scénario du fournisseur UI Automation. En tant que fournisseur, vous devez vous assurer que tous les clients UI Automation peuvent utiliser l’infrastructure UI Automation pour interagir avec votre contrôle à des fins d’accessibilité et de test.

En général, il existe des API parallèles dans l’infrastructure UI Automation : une API pour les clients UI Automation et une autre qui porte le même nom API pour les fournisseurs UI Automation. Dans la plupart des cas, cette rubrique couvre les API pour le fournisseur UI Automation et, en particulier, les classes et les interfaces qui activent l’extensibilité du fournisseur dans cette infrastructure d’interface utilisateur. Parfois, nous mentionnons les API UI Automation utilisées par les clients UI Automation, pour fournir un certain point de vue ou fournir une table de recherche qui met en corrélation les API du client et du fournisseur. Pour plus d’informations sur le point de vue du client, consultez le [Guide du programmeur du client UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-clientportal).

> [!NOTE]
> Les clients UI Automation n’utilisent généralement pas de code managé et ne sont généralement pas implémentés en tant qu’application UWP (il s’agit généralement d’applications de bureau). UI Automation est basé sur une implémentation ou une infrastructure standard et non spécifique. De nombreux clients UI Automation existants, y compris les produits de technologie d’assistance tels que les lecteurs d’écran, utilisent des interfaces COM (Component Object Model) pour interagir avec UI Automation, le système et les applications qui s’exécutent dans les fenêtres enfants. Pour plus d’informations sur les interfaces COM et sur l’écriture d’un client UI Automation à l’aide de COM, consultez [notions de base d’UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Détermination de l’état existant de la prise en charge d’UI Automation pour votre classe d’interface utilisateur personnalisée  
Avant de tenter d’implémenter un homologue Automation pour un contrôle personnalisé, vous devez vérifier si la classe de base et son homologue Automation fournissent déjà la prise en charge de l’accessibilité ou de l’automatisation dont vous avez besoin. Dans de nombreux cas, la combinaison des implémentations de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) , de pairs spécifiques et des modèles qu’elles implémentent peut fournir une expérience d’accessibilité de base mais satisfaisante. La valeur de cette propriété dépend du nombre de modifications que vous avez apportées au modèle d’objet pour l’exposition à votre contrôle par rapport à sa classe de base. En outre, cela dépend du fait que vos ajouts à la fonctionnalité de classe de base correspondent aux nouveaux éléments d’interface utilisateur dans le contrat de modèle ou à l’apparence visuelle du contrôle. Dans certains cas, vos modifications peuvent introduire de nouveaux aspects de l’expérience utilisateur qui nécessitent une prise en charge supplémentaire de l’accessibilité.

Même si vous utilisez la classe homologue de base existante pour la prise en charge de l’accessibilité de base, il est toujours recommandé de définir un homologue pour pouvoir déclarer des informations de **className** précises à UI Automation pour les scénarios de test automatisé. Cette considération est particulièrement importante si vous écrivez un contrôle destiné à une consommation tierce.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Classes homologues Automation  
La UWP s’appuie sur les techniques et conventions d’automatisation d’interface utilisateur existantes utilisées par les anciennes infrastructures d’interface utilisateur de code managé, telles que Windows Forms, Windows Presentation Foundation (WPF) et Microsoft Silverlight. La plupart des classes de contrôle, ainsi que leur fonction et leur objectif, ont également leur origine dans une infrastructure d’interface utilisateur précédente.

Par Convention, les noms de classes homologues commencent par le nom de la classe de contrôle et se terminent par « AutomationPeer ». Par exemple, [**ButtonAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer) est la classe homologue pour la classe de contrôle [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) .

> [!NOTE]
> Dans le cadre de cette rubrique, nous considérons les propriétés liées à l’accessibilité comme étant plus importantes lorsque vous implémentez un homologue de contrôle. Toutefois, pour un concept plus général de la prise en charge d’UI Automation, vous devez implémenter un homologue conformément aux recommandations, comme indiqué par le [Guide du programmeur du fournisseur UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-providerportal) et les [notions de base d’UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview). Ces rubriques ne couvrent pas les API [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) spécifiques que vous utiliseriez pour fournir les informations dans l’infrastructure UWP pour UI Automation, mais elles décrivent les propriétés qui identifient votre classe ou fournissent d’autres informations ou une interaction.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Homologues, modèles et types de contrôle  
Un *modèle de contrôle* est une implémentation d’interface qui expose un aspect particulier des fonctionnalités d’un contrôle à un client UI Automation. Les clients UI Automation utilisent les propriétés et les méthodes exposées par le biais d’un modèle de contrôle pour récupérer des informations sur les fonctionnalités du contrôle, ou pour manipuler le comportement du contrôle au moment de l’exécution.

Les modèles de contrôle permettent de catégoriser et d’exposer les fonctionnalités d’un contrôle indépendamment du type de contrôle ou de l’apparence du contrôle. Par exemple, un contrôle qui présente une interface tabulaire utilise le modèle de contrôle **Grid** pour exposer le nombre de lignes et de colonnes de la table, et pour permettre à un client UI Automation de récupérer des éléments de la table. À titre d’autre exemple, le client UI Automation peut utiliser le modèle de contrôle **Invoke** pour les contrôles qui peuvent être appelés, tels que les boutons, et le modèle de contrôle **Scroll** pour les contrôles qui ont des barres de défilement, tels que les zones de liste, les affichages de liste ou les zones de liste modifiable. Chaque modèle de contrôle représente un type distinct de fonctionnalité et les modèles de contrôle peuvent être combinés pour décrire l’ensemble des fonctionnalités prises en charge par un contrôle particulier.

Les modèles de contrôle sont liés à l’interface utilisateur, car les interfaces sont liées aux objets COM. Dans COM, vous pouvez interroger un objet pour demander les interfaces qu’il prend en charge, puis utiliser ces interfaces pour accéder aux fonctionnalités. Dans UI Automation, les clients UI Automation peuvent interroger un élément UI Automation pour connaître les modèles de contrôle qu’il prend en charge, puis interagir avec l’élément et son contrôle homologué via les propriétés, les méthodes, les événements et les structures exposées par le pris en charge. modèles de contrôle.

L’un des principaux objectifs d’un homologue Automation est de signaler à un client UI Automation les modèles de contrôle que l’élément d’interface utilisateur peut prendre en charge via son homologue. Pour ce faire, les fournisseurs UI Automation implémentent de nouveaux homologues qui modifient le comportement de la méthode [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) en substituant la méthode [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) . Les clients UI Automation effectuent des appels que le fournisseur UI Automation mappe pour appeler **GetPattern**. Les clients UI Automation interrogent chaque modèle spécifique avec lequel ils veulent interagir. Si l’homologue prend en charge le modèle, il retourne une référence d’objet à lui-même ; Sinon, elle retourne la **valeur null**. Si le retour n’est pas **null**, le client UI Automation s’attend à ce qu’il puisse appeler des API de l’interface de modèle en tant que client, afin d’interagir avec ce modèle de contrôle.

Un *type de contrôle* est un moyen de définir globalement la fonctionnalité d’un contrôle que l’homologue représente. Il s’agit d’un concept différent d’un modèle de contrôle, car tandis qu’un modèle informe l’UI Automation des informations qu’il peut obtenir ou des actions qu’il peut effectuer par le biais d’une interface particulière, le type de contrôle existe un niveau au-dessus de celui-ci. Chaque type de contrôle a des conseils sur ces aspects d’UI Automation :

* Modèles de contrôle UI Automation : un type de contrôle peut prendre en charge plusieurs modèles, chacun représentant une classification d’informations ou d’interaction différente. Chaque type de contrôle a un ensemble de modèles de contrôle que le contrôle doit prendre en charge, un ensemble facultatif et un ensemble que le contrôle ne doit pas prendre en charge.
* Valeurs de propriété UI Automation : chaque type de contrôle a un ensemble de propriétés que le contrôle doit prendre en charge. Il s’agit des propriétés générales, décrites dans [vue d’ensemble des propriétés UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-propertiesoverview), et non de celles qui sont spécifiques au modèle.
* Événements UI Automation : chaque type de contrôle a un ensemble d’événements que le contrôle doit prendre en charge. Là encore, celles-ci sont générales, pas spécifiques au modèle, comme décrit dans [vue d’ensemble des événements UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-eventsoverview).
* Structure d’arborescence UI Automation : chaque type de contrôle définit la manière dont le contrôle doit apparaître dans l’arborescence UI Automation.

Quel que soit le mode d’implémentation des homologues Automation pour le Framework, la fonctionnalité du client UI Automation n’est pas liée au UWP. en fait, il est probable que les clients UI Automation existants, tels que les technologies d’assistance, utilisent d’autres modèles de programmation, tels que COM. Dans COM, les clients peuvent exécuter **QueryInterface** pour l’interface de modèle de contrôle com qui implémente le modèle demandé ou l’infrastructure d’automatisation d’interface utilisateur générale pour les propriétés, les événements ou l’examen d’arborescence. Pour les modèles, l’infrastructure UI Automation marshale ce code d’interface dans le code UWP qui s’exécute sur le fournisseur UI Automation de l’application et l’homologue concerné.

Lorsque vous implémentez des modèles de contrôle pour une infrastructure de code managé telle qu’une application UWP à l’aide de C\# ou Microsoft Visual Basic, vous pouvez utiliser des interfaces .NET Framework pour représenter ces modèles au lieu d’utiliser la représentation de l’interface COM. Par exemple, l’interface de modèle UI Automation pour une implémentation de fournisseur Microsoft .NET du modèle **Invoke** est [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider).

Pour obtenir la liste des modèles de contrôle, les interfaces de fournisseur et leurs objectifs, consultez [modèles de contrôle et interfaces](control-patterns-and-interfaces.md). Pour obtenir la liste des types de contrôle, consultez [UI Automation Control types Overview](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-controltypesoverview).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Conseils sur la façon d’implémenter des modèles de contrôle  
Les modèles de contrôle et ce à quoi ils sont destinés font partie d’une plus grande définition de l’infrastructure UI Automation et ne s’appliquent pas uniquement à la prise en charge de l’accessibilité pour une application UWP. Lorsque vous implémentez un modèle de contrôle, vous devez vous assurer que vous l’implémentez de manière à ce qu’il corresponde aux instructions décrites dans ces documents et également dans la spécification UI Automation. Si vous recherchez de l’aide, vous pouvez généralement utiliser la documentation de Microsoft et ne pas avoir à vous référer à la spécification. Des instructions pour chaque modèle sont documentées ici : [implémentation des modèles de contrôle UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Vous remarquerez que chaque rubrique de cette zone contient une section « conventions et directives d’implémentation » et la section « membres requis ». L’aide fait généralement référence à des API spécifiques de l’interface de modèle de contrôle appropriée dans les [interfaces de modèle de contrôle pour](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-cpinterfaces) la référence des fournisseurs. Ces interfaces sont les interfaces natives/COM (et leurs API utilisent la syntaxe de style COM). Mais tout ce que vous voyez, c’est qu’il existe un équivalent dans l’espace de noms [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) .

Si vous utilisez les homologues Automation par défaut et que vous développez sur leur comportement, ces homologues ont déjà été écrits conformément aux instructions d’UI Automation. S’ils prennent en charge les modèles de contrôle, vous pouvez vous appuyer sur cette prise en charge de modèle conforme à l’aide à [l’implémentation des modèles de contrôle UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Si un homologue de contrôle indique qu’il est représentatif d’un type de contrôle défini par UI Automation, les conseils documentés dans la [prise en charge des types de contrôle UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) ont été suivis par ce pair.

Néanmoins, vous pouvez avoir besoin d’aide supplémentaire pour les modèles de contrôle ou les types de contrôle afin de suivre les recommandations UI Automation dans votre implémentation d’homologue. Cela est particulièrement vrai si vous implémentez une prise en charge du modèle ou du type de contrôle qui n’existe pas encore comme une implémentation par défaut dans un contrôle UWP. Par exemple, le modèle pour les annotations n’est pas implémenté dans les contrôles XAML par défaut. Toutefois, vous pouvez avoir une application qui utilise largement des annotations et, par conséquent, vous souhaitez que cette fonctionnalité soit accessible. Pour ce scénario, votre homologue doit implémenter [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) et doit probablement se rapporter lui-même en tant que type de contrôle de **document** avec les propriétés appropriées pour indiquer que vos documents prennent en charge l’annotation.

Nous vous recommandons d’utiliser les instructions que vous voyez pour les modèles sous [implémentation de modèles de contrôle UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) ou de types de contrôle sous [prise en charge des types de contrôle UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) comme orientation et aide générale. Vous pouvez même essayer de suivre certains liens d’API pour obtenir des descriptions et des remarques sur l’objectif des API. Toutefois, pour les caractéristiques de syntaxe nécessaires à la programmation d’applications UWP, recherchez l’API équivalente dans l’espace de noms [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) et utilisez ces pages de référence pour plus d’informations.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Classes homologues Automation intégrées  
En général, les éléments implémentent une classe homologue Automation s’ils acceptent l’activité de l’interface utilisateur de l’utilisateur, ou s’ils contiennent des informations requises par les utilisateurs de technologies d’assistance qui représentent l’interface utilisateur interactive ou significative des applications. Tous les éléments visuels UWP n’ont pas d’homologues Automation. Les exemples de classes qui implémentent des homologues Automation sont [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) et [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Les exemples de classes qui n’implémentent pas d’homologues Automation sont les [**bordures**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) et les classes basées sur le [**panneau**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel), telles que [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) et [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas). Un **panneau** n’a pas d’homologue, car il fournit un comportement de disposition visuel uniquement. Il n’existe aucune méthode relative à l’accessibilité permettant à l’utilisateur d’interagir avec un **panneau**. Quels que soient les éléments enfants d’un **panneau** , ils sont signalés aux arborescences UI Automation en tant qu’éléments enfants du parent disponible suivant dans l’arborescence qui a une représentation d’élément ou d’homologue.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>UI Automation et limites de processus UWP  
En règle générale, le code client UI Automation qui accède à une application UWP s’exécute hors processus. L’infrastructure UI Automation Framework permet d’accéder à des informations à travers la limite de processus. Ce concept est expliqué plus en détail dans la [rubrique notions de base d’UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview).

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Toutes les classes qui dérivent de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) contiennent la méthode virtuelle protégée [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer). La séquence d’initialisation d’objet pour les homologues Automation appelle **OnCreateAutomationPeer** pour obtenir l’objet homologue Automation pour chaque contrôle et créer ainsi une arborescence UI Automation pour une utilisation au moment de l’exécution. Le code UI Automation peut utiliser l’homologue pour obtenir des informations sur les caractéristiques et les fonctionnalités d’un contrôle et simuler l’utilisation interactive au moyen de ses modèles de contrôle. Un contrôle personnalisé qui prend en charge Automation doit substituer **OnCreateAutomationPeer** et retourner une instance d’une classe qui dérive de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer). Par exemple, si un contrôle personnalisé dérive de la classe [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) , l’objet retourné par **OnCreateAutomationPeer** doit dériver de [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Si vous écrivez une classe de contrôle personnalisée et que vous envisagez de fournir également un nouvel homologue Automation, vous devez substituer la méthode [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) pour votre contrôle personnalisé afin qu’il retourne une nouvelle instance de votre homologue. Votre classe pair doit dériver directement ou indirectement de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer).

Par exemple, le code suivant déclare que le contrôle personnalisé `NumericUpDown` doit utiliser les `NumericUpDownPeer` homologues à des fins d’automatisation d’interface utilisateur.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> L’implémentation de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) ne doit faire rien de plus que d’initialiser une nouvelle instance de votre homologue Automation personnalisé, en passant le contrôle appelant comme propriétaire et en retournant cette instance. N’essayez pas une logique supplémentaire dans cette méthode. En particulier, toute logique susceptible d’entraîner la destruction des [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) dans le même appel peut entraîner un comportement inattendu du Runtime.

Dans les implémentations classiques de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer), le *propriétaire* est spécifié comme **This** ou **me** , car la substitution de méthode se trouve dans la même portée que le reste de la définition de la classe de contrôle.

La définition réelle de la classe pair à pair peut être effectuée dans le même fichier de code que le contrôle ou dans un fichier de code séparé. Les définitions homologues existent toutes dans l’espace de noms [**Windows. UI. Xaml. Automation. Peers**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers) qui est un espace de noms distinct des contrôles pour lesquels ils fournissent des homologues. Vous pouvez également choisir de déclarer vos pairs dans des espaces de noms distincts, à condition que vous référençiez les espaces de noms nécessaires pour l’appel de la méthode [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) .

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Choix de la classe de base d’homologue appropriée  
Assurez-vous que votre [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) est dérivé d’une classe de base qui vous donne la meilleure correspondance pour la logique d’homologue existante de la classe de contrôle à partir de laquelle vous dérivez. Dans le cas de l’exemple précédent, étant donné que `NumericUpDown` dérive de [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase), une classe [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) est disponible pour laquelle vous devez baser votre homologue. En utilisant la classe homologue correspondante la plus proche en parallèle à la façon dont vous dérivez le contrôle lui-même, vous pouvez éviter de substituer au moins une partie de la fonctionnalité [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) , car la classe homologue de base l’implémente déjà.

La classe de [**contrôle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) de base n’a pas de classe homologue correspondante. Si vous avez besoin d’une classe homologue pour correspondre à un contrôle personnalisé qui dérive de **Control**, dérivez la classe homologue personnalisée de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer).

Si vous dérivez directement de [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) , cette classe n’a aucun comportement d’homologue Automation par défaut, car aucune implémentation de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) ne fait référence à une classe homologue. Par conséquent, assurez-vous d’implémenter **OnCreateAutomationPeer** pour utiliser votre propre homologue ou d’utiliser [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) en tant qu’homologue si ce niveau de prise en charge de l’accessibilité est adapté à votre contrôle.

> [!NOTE]
> En général, vous ne dérivez pas de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) au lieu de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer). Si vous avez effectué la dérivation directement à partir de **AutomationPeer** , vous devez dupliquer une grande partie de la prise en charge de l’accessibilité de base qui serait autrement fournie par **FrameworkElementAutomationPeer**.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Initialisation d’une classe homologue personnalisée  
L’homologue Automation doit définir un constructeur de type sécurisé qui utilise une instance du contrôle propriétaire pour l’initialisation de base. Dans l’exemple suivant, l’implémentation passe la valeur de *propriétaire* à la base [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) , et il s’agit finalement du [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) qui utilise en fait le *propriétaire* pour définir [ **FrameworkElementAutomationPeer. Owner**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Méthodes principales de AutomationPeer  
Pour les raisons de l’infrastructure UWP, les méthodes substituables d’un homologue Automation font partie d’une paire de méthodes : la méthode d’accès public que le fournisseur UI Automation utilise comme point de transfert pour les clients UI Automation et la méthode de personnalisation « Core » protégée. qu’une classe UWP peut être substituée pour influencer le comportement. La paire de méthodes est câblée ensemble par défaut de telle sorte que l’appel à la méthode d’accès appelle toujours la méthode « Core » parallèle qui a l’implémentation du fournisseur, ou en tant que secours, appelle une implémentation par défaut à partir des classes de base.

Lors de l’implémentation d’un homologue pour un contrôle personnalisé, substituez les méthodes « Core » de la classe homologue Automation de base où vous souhaitez exposer le comportement qui est unique à votre contrôle personnalisé. Le code UI Automation obtient des informations sur votre contrôle en appelant des méthodes publiques de la classe homologue. Pour fournir des informations sur votre contrôle, remplacez chaque méthode par un nom qui se termine par « Core » lorsque l’implémentation et la conception de votre contrôle créent des scénarios d’accessibilité ou d’autres scénarios d’automatisation d’interface utilisateur qui diffèrent de ce qui est pris en charge par l’Automation de base classe homologue.

Au minimum, chaque fois que vous définissez une nouvelle classe pair, implémentez la méthode [**GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) , comme indiqué dans l’exemple suivant.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Vous pouvez stocker les chaînes en tant que constantes plutôt que directement dans le corps de la méthode, mais c’est à vous de choisir. Pour [**GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), vous n’avez pas besoin de localiser cette chaîne. La propriété **LocalizedControlType** est utilisée chaque fois qu’une chaîne localisée est requise par un client UI Automation, et non **className**.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Certaines technologies d’assistance utilisent la valeur [**GetAutomationControlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) directement lors de la création de rapports sur les caractéristiques des éléments d’une arborescence UI Automation, sous la forme d’informations supplémentaires au-delà du **nom**UI Automation. Si votre contrôle est très différent du contrôle à partir duquel vous dérivez et que vous souhaitez signaler un autre type de contrôle par rapport à ce qui est signalé par la classe pair de base utilisée par le contrôle, vous devez implémenter un homologue et substituer [**GetAutomationControlTypeCore** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore)dans l’implémentation de votre homologue. Cela est particulièrement important si vous dérivez d’une classe de base généralisée telle que [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) ou [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl), où l’homologue de base ne fournit pas d’informations précises sur le type de contrôle.

Votre implémentation de [**GetAutomationControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) décrit votre contrôle en retournant une valeur [**AutomationControlType**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) . Bien que vous puissiez retourner **AutomationControlType. Custom**, vous devez retourner l’un des types de contrôle plus spécifiques s’ils décrivent avec précision les scénarios principaux de votre contrôle. Voici un exemple.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> À moins que vous ne spécifiiez [**AutomationControlType. Custom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), vous n’êtes pas obligé d’implémenter [**GetLocalizedControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) pour fournir une valeur de propriété **LocalizedControlType** aux clients. L’infrastructure commune UI Automation fournit des chaînes traduites pour chaque valeur **AutomationControlType** possible autre que **AutomationControlType. Custom**.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern et GetPatternCore  
L’implémentation d’un homologue de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) retourne l’objet qui prend en charge le modèle demandé dans le paramètre d’entrée. Plus précisément, un client UI Automation appelle une méthode qui est transférée à la méthode [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) du fournisseur, et spécifie une valeur d’énumération [**patternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) qui nomme le modèle demandé. Votre remplacement de **GetPatternCore** doit retourner l’objet qui implémente le modèle spécifié. Cet objet est l’homologue lui-même, car l’homologue doit implémenter l’interface de modèle correspondante chaque fois qu’il signale qu’il prend en charge un modèle. Si votre homologue n’a pas d’implémentation personnalisée d’un modèle, mais que vous savez que la base de l’homologue n’implémente pas le modèle, vous pouvez appeler l’implémentation du type de base de **GetPatternCore** à partir de votre **GetPatternCore**. Le **GetPatternCore** d’un homologue doit retourner la **valeur null** si un modèle n’est pas pris en charge par l’homologue. Toutefois, au lieu de retourner **null** directement à partir de votre implémentation, vous vous fiez généralement à l’appel à l’implémentation de base pour retourner la **valeur null** pour tout modèle non pris en charge.

Lorsqu’un modèle est pris en charge, l’implémentation de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut retourner **This** ou **me**. L’attente est que le client UI Automation effectue un cast de la valeur de retour [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) vers l’interface de modèle demandée chaque fois qu’elle n’est pas **null**.

Si une classe homologue hérite d’un autre homologue et que tous les rapports de prise en charge et de modèle nécessaires sont déjà gérés par la classe de base, l’implémentation de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) n’est pas nécessaire. Par exemple, si vous implémentez un contrôle de plage qui dérive de [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase), et que votre homologue dérive de [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer), cet homologue se retourne lui-même pour [**patternInterface. RangeValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) et a des implémentations opérationnelles de interface [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) qui prend en charge le modèle.

Bien qu’il ne s’agisse pas du code littéral, cet exemple se rapproche de l’implémentation de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) déjà présente dans [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Si vous implémentez un homologue dans lequel vous ne disposez pas de toute la prise en charge dont vous avez besoin à partir d’une classe homologue de base, ou que vous souhaitez modifier ou ajouter à l’ensemble de modèles hérités de base que votre homologue peut prendre en charge, vous devez remplacer [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour activer UI Automation. clients qui utilisent les modèles.

Pour obtenir la liste des modèles de fournisseur disponibles dans l’implémentation UWP de la prise en charge d’UI Automation, consultez [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider). Chacun de ces modèles a une valeur correspondante de l’énumération [**patternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) , qui est la manière dont les clients UI Automation demandent le modèle dans un appel [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) .

Un homologue peut signaler qu’il prend en charge plusieurs modèles. Dans ce cas, la substitution doit inclure la logique de chemin de retour pour chaque valeur [**patternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) prise en charge et retourner l’homologue dans chaque cas de correspondance. Il est prévu que l’appelant ne demande qu’une seule interface à la fois et qu’il revient à l’appelant d’effectuer un cast vers l’interface attendue.

Voici un exemple de remplacement de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour un homologue personnalisé. Il signale la prise en charge de deux modèles, [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) et [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider). Le contrôle ici est un contrôle d’affichage de média qui peut s’afficher en plein écran (mode bascule) et qui comporte une barre de progression dans laquelle les utilisateurs peuvent sélectionner une position (le contrôle Range). Ce code provient de l' [exemple d’accessibilité XAML](https://go.microsoft.com/fwlink/p/?linkid=238570).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Transférer des modèles à partir de sous-éléments  
Une implémentation de méthode [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut également spécifier un sous-élément ou un composant en tant que fournisseur de modèles pour son hôte. Cet exemple imite la manière dont [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) transfère la gestion des modèles de défilement à l’homologue de son contrôle [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) interne. Pour spécifier un sous-élément pour la gestion des modèles, ce code obtient l’objet de sous-élément, crée un homologue pour le sous-élément à l’aide de la méthode [**FrameworkElementAutomationPeer. CreatePeerForElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) et retourne le nouvel homologue.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Autres méthodes principales  
Votre contrôle peut avoir besoin de prendre en charge les équivalents du clavier pour les scénarios principaux. Pour plus d’informations sur la raison pour laquelle cela peut être nécessaire, consultez [accessibilité du clavier](keyboard-accessibility.md). L’implémentation de la prise en charge des clés fait nécessairement partie du code de contrôle et non du code homologue, car elle fait partie de la logique d’un contrôle, mais votre classe homologue doit substituer les méthodes [**GetAcceleratorKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) et [**GetAccessKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) pour signaler à l’interface utilisateur Les clients Automation qui utilisent des clés. Tenez compte du fait que les chaînes qui signalent des informations de clé peuvent avoir besoin d’être localisées et qu’elles doivent donc provenir de ressources, et non de chaînes codées en dur.

Si vous fournissez un homologue pour une classe qui prend en charge une collection, il est préférable de dériver à la fois des classes fonctionnelles et des classes homologues qui ont déjà ce type de prise en charge de collection. Si vous ne le faites pas, les homologues pour les contrôles qui maintiennent des collections enfants devront peut-être substituer la méthode homologue associée à la collection [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) pour signaler correctement les relations parent-enfant à l’arborescence UI Automation.

Implémentez les méthodes [**IsContentElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) et [**IsControlElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) pour indiquer si votre contrôle contient du contenu de données ou s’il respecte un rôle interactif dans l’interface utilisateur (ou les deux). Par défaut, les deux méthodes retournent **true**. Ces paramètres améliorent la convivialité des technologies d’assistance, telles que les lecteurs d’écran, qui peuvent utiliser ces méthodes pour filtrer l’arborescence Automation. Si votre méthode [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfère la gestion des modèles à un homologue de sous-élément, la méthode **IsControlElementCore** de l’homologue de sous-élément peut retourner la **valeur false** pour masquer l’homologue de sous-élément de l’arborescence Automation.

Certains contrôles peuvent prendre en charge des scénarios d’étiquetage, où une partie de l’étiquette de texte fournit des informations pour une partie non textuelle, ou un contrôle est destiné à se trouver dans une relation d’étiquetage connue avec un autre contrôle dans l’interface utilisateur. S’il est possible de fournir un comportement utile basé sur les classes, vous pouvez substituer [**GetLabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) pour fournir ce comportement.

[**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) et [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) sont principalement utilisés pour les scénarios de test automatisé. Si vous souhaitez prendre en charge les tests automatisés pour votre contrôle, vous souhaiterez peut-être substituer ces méthodes. Cela peut être souhaité pour les contrôles de type plage, dans lesquels vous ne pouvez pas suggérer un point unique, car l’utilisateur clique dans l’espace de coordonnées a un effet différent sur une plage. Par exemple, l’homologue Automation [**ScrollBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar) par défaut se substitue à **GetClickablePointCore** pour retourner une valeur de [**point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) « not a Number ».

[**GetLiveSettingCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influence le contrôle par défaut pour la valeur **LiveSetting** pour UI Automation. Vous souhaiterez peut-être la remplacer si vous souhaitez que votre contrôle retourne une valeur autre que [**AutomationLiveSetting. OFF**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting). Pour plus d’informations sur ce que **LiveSetting** représente, consultez [**AutomationProperties. LiveSetting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty).

Vous pouvez substituer [**GetOrientationCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) si votre contrôle a une propriété d’orientation définissable qui peut être mappée à [**AutomationOrientation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation). Les classes [**ScrollBarAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) et [**SliderAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) le font.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implémentation de base dans FrameworkElementAutomationPeer  
L’implémentation de base de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) fournit des informations UI Automation qui peuvent être interprétées à partir de différentes propriétés de disposition et de comportement qui sont définies au niveau de l’infrastructure.

* [**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): retourne une structure [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) basée sur les caractéristiques de disposition connues. Retourne un **Rect** de valeur 0 si [**IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) a la valeur **true**.
* [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): retourne une structure de [**points**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) basée sur les caractéristiques de disposition connues, à condition qu’il y ait un **BoundingRectangle**différent de zéro.
* [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): comportement plus étendu que celui qui peut être résumé ici ; consultez [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore). Fondamentalement, elle tente une conversion de chaîne sur tout contenu connu d’une classe [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) ou associée qui a du contenu. En outre, s’il existe une valeur pour [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)), la valeur **Name** de cet élément est utilisée comme **nom**.
* [**HasKeyboardFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): évaluée en fonction des propriétés [**FocusState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focusstate) et [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) du propriétaire. Les éléments qui ne sont pas des contrôles retournent toujours **false**.
* [**IsEnabledCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): évaluée en fonction de la propriété [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) du propriétaire s’il s’agit d’un [**contrôle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control). Les éléments qui ne sont pas des contrôles retournent toujours **true**. Cela ne signifie pas que le propriétaire est activé dans le sens d’interaction conventionnel. Cela signifie que l’homologue est activé en dépit du fait que le propriétaire n’a pas de propriété **IsEnabled** .
* [**IsKeyboardFocusableCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): retourne la **valeur true** Si owner est un [**contrôle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control); dans le cas contraire, la **valeur est false**.
* [**IsOffscreenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): une [**visibilité**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) de [**réduit**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visibility) sur l’élément propriétaire ou sur l’un de ses parents équivaut à une valeur **true** pour [**IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Exception : un objet [**Popup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) peut être visible même si les parents de son propriétaire ne le sont pas.
* [**SetFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): appelle [**le focus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focus).
* [**GetParent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): appelle [**FrameworkElement. parent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent) à partir du propriétaire et recherche l’homologue approprié. Il ne s’agit pas d’une paire de substitution avec une méthode « Core ». vous ne pouvez donc pas modifier ce comportement.

> [!NOTE]
> Les pairs UWP par défaut implémentent un comportement à l’aide d’un code natif interne qui implémente la UWP, pas nécessairement à l’aide du code UWP réel. Vous ne pouvez pas voir le code ou la logique de l’implémentation par le biais de la réflexion common language runtime (CLR) ou d’autres techniques. Vous ne verrez pas non plus des pages de référence distinctes pour les remplacements spécifiques aux sous-classes du comportement de l’homologue de base. Par exemple, il peut y avoir un comportement supplémentaire pour [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore) d’un [**TextBoxAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer), qui n’est pas décrit dans la page de référence **AutomationPeer. GetNameCore** , et il n’existe aucune page de référence pour  **TextBoxAutomationPeer. GetNameCore**. Il n’y a même pas de page de référence **TextBoxAutomationPeer. GetNameCore** . Au lieu de cela, lisez la rubrique de référence pour la classe homologue la plus immédiate et recherchez les notes d’implémentation dans la section Notes.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Pairs et AutomationProperties  
Votre homologue Automation doit fournir les valeurs par défaut appropriées pour les informations relatives à l’accessibilité de votre contrôle. Notez que tout code d’application qui utilise le contrôle peut remplacer certains de ce comportement en incluant des valeurs de propriété attachée [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) sur les instances de contrôle. Les appelants peuvent le faire soit pour les contrôles par défaut, soit pour les contrôles personnalisés. Par exemple, le code XAML suivant crée un bouton qui a deux propriétés UI Automation personnalisées : `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Pour plus d’informations sur les propriétés jointes [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) , consultez [informations d’accessibilité de base](basic-accessibility-information.md).

Certaines méthodes [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) existent en raison du contrat général de la façon dont les fournisseurs UI Automation sont censés signaler des informations, mais ces méthodes ne sont généralement pas implémentées dans les homologues de contrôle. Cela est dû au fait que les informations doivent être fournies par les valeurs [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) appliquées au code d’application qui utilise les contrôles dans une interface utilisateur spécifique. Par exemple, la plupart des applications définissent la relation d’étiquetage entre deux contrôles différents dans l’interface utilisateur en appliquant une valeur [**AutomationProperties. LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) . Toutefois, [**LabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) est implémenté dans certains pairs qui représentent des relations de données ou d’éléments dans un contrôle, telles que l’utilisation d’une partie d’en-tête pour étiqueter une partie de champ de données, l’étiquetage des éléments avec leurs conteneurs, ou des scénarios similaires.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implémentation de modèles  
Examinons comment écrire un homologue pour un contrôle qui implémente un comportement de développement et de réduction en implémentant l’interface de modèle de contrôle pour Expand-collapse. L’homologue doit activer l’accessibilité pour le comportement Expand-collapse en se retournant à chaque fois que [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) est appelé avec une valeur de [**patternInterface. ExpandCollapse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface). L’homologue doit hériter de l’interface du fournisseur pour ce modèle ([**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) et fournir des implémentations pour chacun des membres de cette interface de fournisseur. Dans ce cas, l’interface a trois membres à substituer : [**expand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse), [**ExpandCollapseState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Il est utile de planifier l’accessibilité dans la conception de l’API de la classe elle-même. Chaque fois que vous avez un comportement potentiellement demandé suite à des interactions classiques avec un utilisateur qui travaille dans l’interface utilisateur ou par le biais d’un modèle de fournisseur Automation, fournissez une méthode unique que la réponse de l’interface utilisateur ou le modèle d’automatisation peut appeler. Par exemple, si votre contrôle a des parties de bouton qui ont des gestionnaires d’événements câblés qui peuvent développer ou réduire le contrôle, et qui a des équivalents clavier pour ces actions, les gestionnaires d’événements appellent-ils la même méthode que celle que vous appelez à partir du corps du [**développement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand)ou de [**réduire**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) les implémentations de [**IExpandCollapseProvider**](https://docs.microsoft.com/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider) dans l’homologue. L’utilisation d’une méthode logique commune peut également être utile pour s’assurer que les États visuels de votre contrôle sont mis à jour pour afficher l’état logique d’une manière uniforme, quelle que soit la façon dont le comportement a été appelé.

Une implémentation classique est que les API du fournisseur appellent d’abord le [**propriétaire**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) pour accéder à l’instance de contrôle au moment de l’exécution. Ensuite, les méthodes de comportement nécessaires peuvent être appelées sur cet objet.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Une autre implémentation est que le contrôle lui-même peut référencer son homologue. Il s’agit d’un modèle courant si vous déclenchez des événements d’Automation à partir du contrôle, car la méthode [**RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) est une méthode homologue.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Événements UI Automation  

Les événements UI Automation appartiennent aux catégories suivantes.

| Événement | Description |
|-------|-------------|
| Modification de propriété | Se déclenche lorsqu’une propriété d’un élément UI Automation ou d’un modèle de contrôle change. Par exemple, si un client doit surveiller le contrôle de case à cocher d’une application, il peut s’inscrire pour écouter un événement de modification de propriété sur la propriété [**ToggleState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) . Lorsque le contrôle de case à cocher est activé ou désactivé, le fournisseur déclenche l’événement et le client peut agir en fonction des besoins. |
| Action d’élément | Se déclenche lorsqu’une modification de l’interface utilisateur résulte de l’activité de l’utilisateur ou de la programmation ; par exemple, lorsqu’un utilisateur clique sur un bouton ou l’appelle par le biais du modèle **Invoke** . |
| Modification de la structure | Se déclenche lorsque la structure de l’arborescence UI Automation change. La structure change quand de nouveaux éléments d’interface utilisateur deviennent visibles, masqués ou supprimés sur le bureau. |
| Modification globale | Se déclenche lorsque des actions d’intérêt global pour le client se produisent, par exemple quand le focus passe d’un élément à un autre, ou lorsqu’une fenêtre enfant se ferme. Certains événements ne signifient pas nécessairement que l’état de l’interface utilisateur a changé. Par exemple, si l’utilisateur passe à un champ d’entrée de texte, puis clique sur un bouton pour mettre à jour le champ, un événement [**TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) se déclenche même si l’utilisateur n’a pas réellement modifié le texte. Lors du traitement d’un événement, il peut être nécessaire pour une application cliente de vérifier si quelque chose a changé avant de prendre une mesure. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificateurs AutomationEvents  
Les événements UI Automation sont identifiés par des valeurs [**AutomationEvents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents) . Les valeurs de l’énumération identifient de façon unique le type d’événement.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Déclencher des événements  
Les clients UI Automation peuvent s’abonner à des événements d’Automation. Dans le modèle d’homologue Automation, les homologues des contrôles personnalisés doivent signaler les modifications apportées à l’état du contrôle qui sont pertinentes pour l’accessibilité en appelant la méthode [**RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) . De même, lorsqu’une valeur de propriété Automation d’interface utilisateur de clé change, les homologues de contrôle personnalisé doivent appeler la méthode [**RaisePropertyChangedEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) .

L’exemple de code suivant montre comment obtenir l’objet homologue dans le code de définition de contrôle et comment appeler une méthode pour déclencher un événement à partir de cet homologue. En guise d’optimisation, le code détermine s’il existe des écouteurs pour ce type d’événement. Le déclenchement de l’événement et de la création de l’objet homologue uniquement lorsqu’il existe des écouteurs évite une surcharge inutile et aide le contrôle à rester réactif.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Navigation entre homologues  
Après avoir localisé un homologue Automation, un client UI Automation peut naviguer dans la structure homologue d’une application en appelant les méthodes [**GetChildren**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) et [**GetParent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) de l’objet homologue. La navigation entre les éléments d’interface utilisateur dans un contrôle est prise en charge par l’implémentation de l’homologue de la méthode [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) . Le système UI Automation appelle cette méthode pour générer une arborescence de sous-éléments contenus dans un contrôle ; par exemple, les éléments de liste dans une zone de liste. La méthode **GetChildrenCore** par défaut dans [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) parcourt l’arborescence d’éléments visuels pour créer l’arborescence des homologues Automation. Les contrôles personnalisés peuvent substituer cette méthode pour exposer une représentation différente des éléments enfants aux clients Automation, en retournant les homologues Automation des éléments qui communiquent des informations ou autorisent l’interaction de l’utilisateur.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Prise en charge d’Automation native pour les modèles de texte  
Certains des homologues d’automatisation d’application UWP par défaut fournissent la prise en charge du modèle de contrôle pour le modèle de texte ([**patternInterface. Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)). Mais ils offrent cette prise en charge par le biais de méthodes natives, et les homologues impliqués ne noteront pas l’interface [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) dans l’héritage (managé). Toutefois, si un client UI Automation managé ou non managé interroge l’homologue pour les modèles, il signale la prise en charge du modèle de texte et fournit le comportement pour les parties du modèle lorsque les API clientes sont appelées.

Si vous envisagez de dériver de l’un des contrôles de texte d’application UWP et de créer également un homologue personnalisé qui dérive de l’un des homologues liés au texte, consultez les sections Notes pour l’homologue pour en savoir plus sur la prise en charge native des modèles. Vous pouvez accéder au comportement de base natif dans votre homologue personnalisé si vous appelez l’implémentation de base à partir de vos implémentations d’interface de fournisseur managé, mais il est difficile de modifier ce que fait l’implémentation de base, car les interfaces natives sur l’homologue et son le contrôle propriétaire n’est pas exposé. En général, vous devez utiliser les implémentations de base telles quelles (base d’appel uniquement) ou remplacer complètement les fonctionnalités par votre propre code managé et ne pas appeler l’implémentation de base. Ce dernier est un scénario avancé, vous aurez besoin d’une bonne connaissance de l’infrastructure de services de texte utilisée par votre contrôle afin de prendre en charge les exigences d’accessibilité lors de l’utilisation de ce Framework.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties. AccessibilityView  
En plus de fournir un homologue personnalisé, vous pouvez également ajuster la représentation d’arborescence pour toute instance de contrôle en définissant [**AutomationProperties. AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) en XAML. Cette opération n’est pas implémentée dans le cadre d’une classe homologue, mais nous la mentionnerons ici, car elle est en vigueur dans la prise en charge de l’accessibilité globale pour les contrôles personnalisés ou pour les modèles que vous personnalisez.

Le principal scénario d’utilisation de **AutomationProperties. AccessibilityView** consiste à omettre délibérément certains contrôles dans un modèle à partir des vues UI Automation, car ils ne contribuent pas de manière significative à la vue d’accessibilité de l’ensemble du contrôle. Pour éviter cela, affectez à **AutomationProperties. AccessibilityView** la valeur « RAW ».

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Levée d’exceptions à partir d’homologues Automation  
Les API que vous implémentez pour la prise en charge de l’homologue Automation sont autorisées à lever des exceptions. Il est prévu que tous les clients UI Automation qui écoutent soient suffisamment robustes pour continuer après la levée de la plupart des exceptions. Dans tous les cas, l’écouteur examine une arborescence d’automatisation complète qui comprend des applications autres que les vôtres, et il s’agit d’une conception cliente inacceptable pour faire apparaître l’intégralité du client uniquement parce qu’une zone de l’arborescence a levé une exception basée sur l’homologue lorsque le client a appelé ses API.

Pour les paramètres passés à votre homologue, il est acceptable de valider l’entrée et, par exemple, de lever [**ArgumentNullException**](https://docs.microsoft.com/dotnet/api/system.argumentnullexception) si elle a été passée **null** et qu’il ne s’agit pas d’une valeur valide pour votre implémentation. Toutefois, si des opérations ultérieures sont effectuées par votre homologue, n’oubliez pas que les interactions de l’homologue avec le contrôle d’hébergement ont un caractère asynchrone. Tout ce qu’un homologue ne bloquera pas nécessairement le thread d’interface utilisateur dans le contrôle (et cela ne devrait probablement pas). Par conséquent, vous pouvez avoir des situations où un objet était disponible ou avait certaines propriétés lorsque l’homologue a été créé ou lorsqu’une méthode d’homologue Automation était appelée pour la première fois, mais dans l’intervalle, l’état du contrôle a changé. Dans ce cas, il existe deux exceptions dédiées qu’un fournisseur peut lever :

* Levez [**ElementNotAvailableException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotavailableexception) si vous ne parvenez pas à accéder au propriétaire de l’homologue ou à un élément homologue associé en fonction des informations d’origine transmises par votre API. Par exemple, vous pouvez avoir un homologue qui tente d’exécuter ses méthodes, mais le propriétaire a depuis été supprimé de l’interface utilisateur, par exemple une boîte de dialogue modale qui a été fermée. Pour un client non-.NET, cela correspond à [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes).
* Levez [**ElementNotEnabledException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotenabledexception) s’il existe encore un propriétaire, mais que ce propriétaire est dans un mode tel que [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)`=`**false** qui bloque certaines des modifications de programmation spécifiques que votre homologue tente d’accomplir. Pour un client non-.NET, cela correspond à [**UIA\_E\_ELEMENTNOTENABLED**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes).

Au-delà, les homologues doivent être relativement conservateurs en ce qui concerne les exceptions qu’ils lèvent de leur prise en charge d’homologue. La plupart des clients ne sont pas en mesure de gérer les exceptions des pairs et de les convertir en choix exploitables que leurs utilisateurs peuvent effectuer lors de l’interaction avec le client. Par conséquent, il arrive parfois qu’une absence d’opération, et l’interception des exceptions sans nouvelle levée dans vos implémentations d’homologue, constituent une meilleure stratégie que la levée d’exceptions chaque fois que l’homologue tente de ne pas fonctionner. Envisagez également que la plupart des clients UI Automation ne sont pas écrits en code managé. La plupart sont écrits en COM et vérifient simplement les **\_OK** dans un **HRESULT** chaque fois qu’ils appellent une méthode de client UI Automation qui finit par accéder à votre homologue.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Aux](accessibility.md)
* [Exemple d’accessibilité XAML](https://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md)
