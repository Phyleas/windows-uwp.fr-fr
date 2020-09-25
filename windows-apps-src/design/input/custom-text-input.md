---
Description: Les API de texte de base de l’espace de noms Windows. UI. Text. Core permettent à une application Windows de recevoir des entrées de texte de n’importe quel service de texte pris en charge sur les appareils Windows.
title: Vue d’ensemble de la saisie de texte personnalisé
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: clavier, texte, Core Text, texte personnalisé, Text Services Framework, entrées, interactions utilisateur
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8a6183dcc690a8fe3b9d13cfa0e471f41f04ff30
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220572"
---
# <a name="custom-text-input"></a>Saisie de texte personnalisé



Les API de texte de base de l’espace de noms [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core) permettent à une application Windows de recevoir des entrées de texte de n’importe quel service de texte pris en charge sur les appareils Windows. Ces API sont similaires aux API [Structure des services de texte](/windows/desktop/TSF/text-services-framework), dans la mesure où l’application n’a pas besoin de connaissances détaillées des services de texte. Cela permet à l’application de recevoir du texte dans n’importe quelle langue et à partir de n’importe quel type d’entrée, comme la saisie sur clavier, la saisie vocale ou la saisie à l’aide d’un stylet.

> **API importantes**: [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core), [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>Pourquoi utiliser les API Core Text ?


Pour de nombreuses applications, les contrôles de zone de texte XAML ou HTML sont suffisants pour la saisie et l’édition de texte. Toutefois, si votre application gère les scénarios de texte complexes, comme une application de traitement de texte, vous aurez peut-être besoin d’un contrôle d’édition de texte personnalisé. Vous pouvez utiliser les API de clavier [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) pour créer votre contrôle d’édition de texte, mais celles-ci ne permettent pas de recevoir une entrée de texte basée sur une composition, ce qui est nécessaire pour prendre en charge les langues d’Extrême-Orient.

Si vous avez besoin de créer un système de contrôle d’édition de texte, utilisez plutôt les API [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core). Ces API sont conçues pour vous offrir une grande souplesse dans le traitement de la saisie de texte, dans n’importe quelle langue. Elles vous permettent également de bénéficier de l’expérience de texte la plus adaptée à votre application. Les contrôles de saisie et d’édition de texte conçus avec les API Core Text peuvent recevoir du texte à partir de toutes les méthodes de saisie de texte qui existent sur les appareils Windows, des éditeurs de méthode d’entrée (IME) basés sur la [structure des services de texte](/windows/desktop/TSF/text-services-framework) et de l’écriture manuscrite sur PC au clavier WordFlow (qui offre des fonctionnalités de correction automatique, de prédiction et de dictée) sur les appareils mobiles.

## <a name="architecture"></a>Architecture


Voici une représentation simple du système de saisie de texte.

-   « Application » représente une application Windows hébergeant un contrôle d’édition personnalisé créé à l’aide des API de texte de base.
-   Les API [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core) facilitent la communication avec les services de texte via Windows. La communication entre le contrôle d’édition de texte et les services de texte est gérée principalement via un objet [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) qui fournit les méthodes et événements visant à faciliter la communication.

![diagramme de l’architecture core text](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Sélection et plages de texte


Les contrôles d’édition fournissent un espace pour la saisie de texte et les utilisateurs peuvent modifier du texte n’importe où dans cet espace. Cet article vise à présenter le système de positionnement de texte utilisé par l’API Core Text, ainsi que la représentation des plages et des sélections dans ce système.

### <a name="application-caret-position"></a>Position d’insertion de l’application

Les plages de texte utilisées avec les API Core Text sont exprimées en termes de position d’insertion. La « position d’insertion de l’application (ACP) » est un nombre (basé sur zéro) indiquant le nombre de caractères à partir du début du texte, juste avant le point d’insertion, comme indiqué ici.

![exemple de diagramme de flux de texte](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Sélection et plages de texte

Les plages de texte et les sélections sont représentées par la structure [**CoreTextRange**](/uwp/api/Windows.UI.Text.Core.CoreTextRange) qui contient deux champs :

| Champ                  | Type de données                                                                 | Description                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Nombre** \[ JavaScript\] | **System. Int32** \[ .net\] | **Int32** \[ C++\] | La position de départ d’une plage correspond à l’ACP située juste avant le premier caractère. |
| **EndCaretPosition**   | **Nombre** \[ JavaScript\] | **System. Int32** \[ .net\] | **Int32** \[ C++\] | La position de fin d’une plage correspond à l’ACP située juste après le dernier caractère.     |

 

Par exemple, dans la plage de texte indiquée précédemment, la plage \[ 0, 5 \] spécifie le mot « hello ». **StartCaretPosition** doit toujours être inférieur ou égal à **EndCaretPosition**. La plage \[ 5, 0 \] n’est pas valide.

### <a name="insertion-point"></a>Point d’insertion

La position d’insertion actuelle, souvent appelée « point d’insertion », est représentée par la définition d’un champ **StartCaretPosition** égal au champ **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Sélection non contiguë

Certains contrôles d’édition prennent en charge les sélections non contiguës. Par exemple, les applications Microsoft Office prennent en charge les sélections arbitraires multiples, et de nombreux éditeurs de code source permettent la sélection de colonnes. Toutefois, les API Core Text n’acceptent pas les sélections non contiguës. Les contrôles d’édition doivent uniquement signaler une sélection contiguë, qui correspond le plus souvent à la sous-plage active des sélections non contiguës.

Prenons l’exemple du flux de texte suivant :

![exemple de diagramme de flux de texte ](images/coretext/stream-2.png) il y a deux sélections : \[ 0, 1 \] et \[ 6, 11 \] . Le contrôle d’édition ne doit signaler qu’un seul d’entre eux ; \[0, 1 \] ou \[ 6, 11 \] .

## <a name="working-with-text"></a>Utilisation du texte


La classe [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) permet un flux de texte entre Windows et les contrôles d’édition via l’événement [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), l’événement [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) et la méthode [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Votre système de contrôle d’édition reçoit le texte via les événements [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) qui sont générés lorsque les utilisateurs utilisent les méthodes de saisie de texte telles que les claviers, la saisie vocale ou les éditeurs IME.

Lorsque vous modifiez du texte dans le contrôle d’édition, par exemple en collant du texte dans le contrôle, vous devez notifier Windows en appelant [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Si le service de texte a besoin du nouveau texte, un événement [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) est déclenché. Vous devez indiquer le nouveau texte dans le gestionnaire d’événements **TextRequested**.

### <a name="accepting-text-updates"></a>Accepter des mises à jour de texte

Votre système de contrôle d’édition accepte généralement les demandes de mises à jour de texte, dans la mesure où elles contiennent le texte que l’utilisateur souhaite saisir. Dans le gestionnaire d’événements [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) , ces actions sont attendues de votre contrôle d’édition :

1.  Insérez le texte spécifié dans [**CoreTextTextUpdatingEventArgs. Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) à la position spécifiée dans [**CoreTextTextUpdatingEventArgs. Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range).
2.  Placer la sélection à la position indiquée dans [**CoreTextTextUpdatingEventArgs.NewSelection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection).
3.  Indiquer au système que la mise à jour a été correctement effectuée en définissant [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) sur [**CoreTextTextUpdatingResult.Succeeded**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Par exemple, voici l’état d’un contrôle d’édition avant que l’utilisateur tape « d ». Le point d’insertion est \[ 10, 10 \] .

![exemple de diagramme de flux de texte ](images/coretext/stream-3.png) quand l’utilisateur tape « d », un événement [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) est déclenché avec les données [**CoreTextTextUpdatingEventArgs**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs) suivantes :

-   [**Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)  =  Plage \[ 10, 10\]
-   [**Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**NewSelection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)  =  NewSelection \[ 11, 11\]

Dans votre système de contrôle d’édition, appliquez les modifications indiquées et définissez [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) sur **Succeeded**. Voici l’état du contrôle une fois que les modifications sont appliquées.

![exemple de diagramme de flux de texte](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Refuser des mises à jour de texte

Il vous est parfois impossible d’appliquer les mises à jour de texte, car la plage concernée est une zone du contrôle d’édition qui ne doit pas être modifiée. Dans ce cas, vous ne devez pas appliquer les modifications. Au lieu de cela, indiquez au système que la mise à jour a échoué en définissant [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) sur [**CoreTextTextUpdatingResult.Failed**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Par exemple, imaginons un système de contrôle d’édition qui accepte uniquement une adresse de messagerie. Les espaces doivent être rejetés, car les adresses de messagerie ne peuvent pas contenir d’espaces. De ce fait, quand des événements [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) sont déclenchés pour la touche Espace, il vous suffit de définir [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) sur **Failed** dans votre système de contrôle d’édition.

### <a name="notifying-text-changes"></a>Signaler des modifications de texte

Parfois, votre système de contrôle d’édition apporte des modifications au texte lorsque le texte est collé ou corrigé automatiquement. Dans ce cas, vous devez notifier les services de texte de ces modifications en appelant la méthode [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) .

Par exemple, voici l’état d’un contrôle d’édition avant que l’utilisateur colle le mot « World ». Le point d’insertion est à \[ 6, 6 \] .

![exemple de diagramme de flux de texte ](images/coretext/stream-5.png) l’utilisateur exécute l’action coller et le contrôle d’édition finit par le texte suivant :

![exemple ](images/coretext/stream-4.png) de diagramme de flux de texte lorsque cela se produit, vous devez appeler [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) avec les arguments suivants :

-   *modifiedRange*  =  modifiedRange \[ 6, 6\]
-   *newLength* = 5
-   *newSelection*  =  newSelection \[ 11, 11\]

Un ou plusieurs événements [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) sont suivis, que vous gérez pour mettre à jour le texte avec lequel les services de texte travaillent.

### <a name="overriding-text-updates"></a>Ignorer des mises à jour de texte

Dans votre système de contrôle d’édition, vous souhaiterez peut-être ignorer une mise à jour de texte pour utiliser des fonctionnalités de correction automatique.

Par exemple, imaginons un système de contrôle d’édition qui fournit une fonctionnalité de correction qui formalise les contractions. Voici l’état du contrôle d’édition avant que l’utilisateur appuie sur la touche Espace pour déclencher la correction. Le point d’insertion est à \[ 3, 3 \] .

![exemple de diagramme de flux ](images/coretext/stream-6.png) de texte l’utilisateur appuie sur la touche espace et un événement [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) correspondant est déclenché. Le système de contrôle d’édition accepte la mise à jour de texte. Voici l’état que le contrôle d’édition affiche pendant un court instant avant la fin de la correction. Le point d’insertion est à \[ 4, 4 \] .

![exemple ](images/coretext/stream-7.png) de diagramme de flux de texte en dehors du gestionnaire d’événements [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) , le contrôle d’édition effectue la correction suivante. Voici l’état du contrôle d’édition après la fin de la correction. Le point d’insertion est à \[ 5, 5 \] .

![exemple ](images/coretext/stream-8.png) de diagramme de flux de texte lorsque cela se produit, vous devez appeler [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) avec les arguments suivants :

-   *modifiedRange*  =  modifiedRange \[ 1, 2\]
-   *newLength* = 2
-   *newSelection*  =  newSelection \[ 5, 5\]

Un ou plusieurs événements [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) sont suivis, que vous gérez pour mettre à jour le texte avec lequel les services de texte travaillent.

### <a name="providing-requested-text"></a>Fournir le texte demandé

Les services de texte doivent disposer du texte approprié pour proposer des fonctionnalités comme la correction automatique ou la prédiction, en particulier si le texte existait déjà dans le système de contrôle d’édition, par exemple parce qu’il avait été créé lors du chargement d’un document, ou parce qu’il avait été inséré par le système de contrôle d’édition comme expliqué dans les sections précédentes. Par conséquent, chaque fois qu’un événement [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) est déclenché, vous devez fournir le texte actuellement dans votre contrôle d’édition pour la plage spécifiée.

Il y aura des occurrences de la [**plage**](/uwp/api/windows.ui.text.core.coretexttextrequest.range) dans [**CoreTextTextRequest**](/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) spécifie une plage qui ne peut pas être prise en compte par votre contrôle d’édition. C’est par exemple le cas si le **Range** est supérieur à la taille du contrôle d’édition au moment de l’événement [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) ou si la fin du **Range** est hors limites. Dans ces cas, vous devez indiquer la plage adéquate, qui correspond généralement à un sous-ensemble de la plage requise.

## <a name="related-articles"></a>Articles connexes

### <a name="samples"></a>Exemples

- [Exemple de contrôle d’édition personnalisé](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>Exemples d’archive

- [Exemple de modification de texte XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
