---
description: C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête.
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: b3ad3a204b2eb4b288e3d3c313a0832bea124de0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219892"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en employant n’importe quel compilateur C++17 conforme aux normes. Le SDK Windows inclut C++/WinRT. Il a été introduit dans la version 10.0.17134.0 (Windows 10, version 1803).

C++/WinRT est destiné à tous les développeurs intéressés pour écrire du code sublime et rapide pour Windows. Voici pourquoi.

## <a name="the-case-for-cwinrt"></a>Les arguments en faveur de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

Le langage de programmation C++ est utilisé à la fois dans les segments des entreprises *et* des éditeurs de logiciels indépendants (ISV) pour les applications où des niveaux élevés d’exactitude, de qualité et de performances sont importants. Par exemple : programmation de systèmes, systèmes mobiles et embarqués avec restriction de ressources, jeux et graphiques, pilotes de périphériques et applications médicales, scientifiques et industrielles, pour n’en citer que quelques-uns.

D’un point de vue du langage, C++ a toujours été destiné à la création et l’utilisation d’abstractions qui sont à la fois riches en types et légères. Cependant, le langage a changé considérablement depuis les pointeurs bruts, les boucles brutes et l’allocation et la libération laborieuses de mémoire de C++98. Le C++ moderne (à partir de C++11) s’attache à l’expression claire des idées, à la simplicité, à la lisibilité et à limiter l’introduction de bogues.

Pour la création et l’utilisation d’API Windows Runtime avec C++, il existe C++/WinRT. Il s’agit du remplacement recommandé par Microsoft pour la projection de langage [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) et la [bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

Vous utilisez des types de données, des algorithmes et des mots clés C++ standard lorsque vous utilisez C++/WinRT. La projection a ses propres types de données personnalisés, mais dans la plupart des cas vous n’avez pas besoin de les apprendre, car ils fournissent des conversions appropriées vers et depuis les types standard. De cette façon, vous pouvez continuer à utiliser les fonctionnalités de langage C++ standard auxquelles vous êtes habitué, et le code source que vous avez déjà. C++/WinRT facilite beaucoup l’appel des API Windows Runtime dans n’importe quelle application C++, de Win32 à UWP.

C++/WinRT offre de meilleures performances et génère des fichiers binaires plus petits que toute autre option de langage pour Windows Runtime. Il surpasse même le code manuscrit en utilisant directement les interfaces ABI. Cela est dû au fait que les abstractions utilisent des idiomes C++ modernes que le compilateur Visual C++ est conçu pour optimiser. Cela inclut magic statics, les classes de base vides, l’élision **strlen** ainsi que de nombreuses optimisations plus récentes de la dernière version de Visual C++ destinée spécifiquement à améliorer les performances de C++/WinRT.

Il existe des moyens d’introduire progressivement C++/WinRT dans vos projets. Vous pouvez utiliser les [composants Windows Runtime](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) ou interopérer avec C++/CX. Pour plus d’informations, consultez [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md).

Pour plus d’informations sur le portage vers C++/WinRT, consultez ces ressources.

- [Passer de C++/CX à C++/WinRT](./move-to-winrt-from-cx.md)
- [Passer de WRL à C++/WinRT](./move-to-winrt-from-wrl.md)
- [Passer de C# à C++/WinRT](./move-to-winrt-from-csharp.md)

Consultez aussi [Où trouver des exemples d’applications C++/WinRT ?](./faq.md#where-can-i-find-cwinrt-sample-apps).

### <a name="topics-about-cwinrt"></a>Rubriques sur C++/WinRT

| Rubrique | Description |
| - | - |
| [Présentation de C++/WinRT](./intro-to-using-cpp-with-winrt.md) | Présentation de C++/WinRT &mdash; une projection de langage C++ standard pour les API Windows Runtime. |
| [Prise en main de C++/WinRT](./get-started.md) | Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code. |
| [Nouveautés de C++/WinRT](./news.md) | Nouveautés et modifications apportées à C++/WinRT. |
| [Questions fréquentes](./faq.md) | Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT. |
| [Dépannage](./troubleshooting.md) | Le tableau de résolution des problèmes et des solutions de cette rubrique peut vous être utile, que vous coupiez un nouveau code ou portiez une application existante. |
| [Exemple d’application C++/WinRT Photo Editor](./photo-editor-sample.md) | Photo Editor est un exemple d’application UWP qui illustre le développement à l’aide de la projection de langage C++/WinRT. L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque **Images**, puis de modifier l’image sélectionnée avec des effets de photo assortis. | 
| [Gestion des chaînes](./strings.md) | Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues C++ standard, ou vous pouvez utiliser le type [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Types de données C++ standard et C++/WinRT](./std-cpp-data-types.md) | Avec C++/WinRT, vous pouvez appeler les API Windows Runtime à l’aide de types de données C++ standard. |
| [Conversions boxing et unboxing de valeurs scalaires vers IInspectable](./boxing.md) | Une valeur scalaire doit être encapsulée dans un objet de classe de référence avant d’être transmise à une fonction qui attend **IInspectable**. Ce processus d’encapsulation est appelé *boxing* de la valeur. |
| [Utiliser des API avec C++/WinRT](./consume-apis.md) | Cette rubrique montre comment utiliser des API C++/WinRT, qu’elles soient implémentées par Windows, un fournisseur de composants tiers ou par vous-même. |
| [Créer des API avec C++/WinRT](./author-apis.md) | Cette rubrique montre comment créer des API C++/WinRT à l’aide du struct de base **winrt::implements**, directement ou indirectement. |
| [Gestion des erreurs avec C++/WinRT](./error-handling.md) | Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec C++/WinRT. |
| [Gérer des événements en utilisant des délégués](./handle-events.md) | Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de C++/WinRT. |
| [Créer des événements](./author-events.md) | Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements. |
| [Collections avec C++/WinRT](./collections.md) | C++/WinRT fournit des fonctions et classes de base qui vous permettent d’économiser du temps et des efforts quand vous souhaitez implémenter et/ou transmettre des collections. |
| [Concurrence et opérations asynchrones](./concurrency.md) | Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT. |
| [Concurrence et opérations asynchrones plus avancées](./concurrency-2.md) | Scénarios de concurrence et d’opérations asynchrones plus avancés dans C++/WinRT. |
| [Contrôles XAML ; liaison à une propriété C++/WinRT](./binding-property.md) | Une propriété qui peut être efficacement liée à un contrôle XAML est appelée propriété *observable*. Cette rubrique montre comment implémenter et utiliser une propriété observable, et comment y lier un contrôle XAML. |
| [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](./binding-collection.md) | Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée collection *observable*. Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML. |
| [Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT](./xaml-cust-ctrl.md) | Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé simple à l’aide de C++/WinRT. Vous pouvez vous baser sur les informations présentées ici pour créer vos propres contrôles d’interface utilisateur riches en fonctionnalités et personnalisables. |
| [Passage de paramètres à la frontière ABI](./pass-parms-to-abi.md) | C++/ WinRT simplifie le passage de paramètres à la frontière ABI en fournissant des conversions automatiques pour les cas courants. |
| [Utiliser des composants COM avec C++/WinRT](./consume-com.md) | Cette rubrique utilise un exemple de code complet Direct2D pour montrer comment utiliser C++/WinRT pour consommer des classes et interfaces COM. |
| [Créer des composants COM avec C++/WinRT](./author-coclasses.md) | C++/WinRT peut vous aider à créer des composants COM classiques, comme il vous aide à créer des classes Windows Runtime. |
| [Passer de C++/CX à C++/WinRT](./move-to-winrt-from-cx.md) | Cette rubrique décrit les détails techniques impliqués dans le portage du code source dans un projet [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers son équivalent dans [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md) | Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Asynchronisme, interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx-async.md) | Il s’agit d’une rubrique avancée relative au portage progressif de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Elle montre comment les coroutines et les tâches PPL (Parallel Patterns Library) peuvent exister côte à côte dans le même projet. |
| [Passer de WRL à C++/WinRT](./move-to-winrt-from-wrl.md) | Cette rubrique montre comment porter du code de la [bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) vers son équivalent en [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Exemple de portage du Presse-papiers vers C++/WinRT à partir de C#&mdash;étude de cas](./clipboard-to-winrt-from-csharp.md) | Cette rubrique présente une étude de cas de portage de l’un des [exemples d’application de plateforme Windows universelle (UWP)](https://github.com/microsoft/Windows-universal-samples) à partir de [C#](/visualstudio/get-started/csharp) vers [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Vous pouvez bénéficier d’une pratique et d’une expérience de portage en suivant la procédure pas à pas et en portant l’exemple pour vous-même au fur et à mesure. |
| [Passer de C# à C++/WinRT](./move-to-winrt-from-csharp.md) | Cette rubrique catalogue tous les détails techniques impliqués dans le portage du code source dans un projet [C#](/visualstudio/get-started/csharp) vers son équivalent en [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Interopérabilité entre C++/WinRT et ABI](./interop-winrt-abi.md) | Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) et C++/WinRT. |
| [Références fortes et faibles en C++/WinRT](./weak-references.md) | L’environnement Windows Runtime est un système avec décompte des références ; dans un tel système, il est important de connaître la signification des références fortes et faibles, et de faire la distinction entre elles. |
| [Objets agiles](./agile-objects.md) | Un objet agile est un objet qui est accessible à partir de n’importe quel thread. Vos types C++/WinRT sont agiles par défaut, mais vous pouvez le refuser. |
| [Diagnostic des allocations directes](./diag-direct-alloc.md) | Cette rubrique décrit de façon détaillée la fonctionnalité C++/WinRT 2.0 qui vous permet de diagnostiquer l’erreur de créer un objet de type implémentation sur la pile, plutôt que d’utiliser la famille d’assistants [**winrt::make**](/uwp/cpp-ref-for-winrt/make), comme vous le devriez. |
| [Points d’extension pour vos types d’implémentation](./details-about-destructors.md) | Ces points d’extension en C++/WinRT 2.0 vous permettent de différer la destruction de vos types d’implémentation, d’interroger en toute sécurité pendant la destruction et de raccorder l’entrée et la sortie de vos méthodes projetées. |
| [Exemple de bibliothèque d’IU Windows C++/WinRT simple](./simple-winui-example.md) | Cette rubrique vous guide dans le processus d’ajout d’une prise en charge simple de WinUI dans un projet C++/WinRT. |
| [Composants Windows Runtime avec C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) | Cette rubrique montre comment utiliser C++/WinRT pour créer et consommer un composant Windows Runtime (composant pouvant être appelé à partir d’une application Windows universelle générée avec n’importe quel langage Windows Runtime). |

### <a name="topics-about-the-c-language"></a>Rubriques sur le langage C++

| Rubrique | Description |
| - | - |
| [Catégories de valeurs et références à celles-ci](./cpp-value-categories.md) | Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous aurez sans doute entendu parler de lvalues et rvalues, mais il existe aussi d’autres types. |

## <a name="important-apis"></a>API importantes
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Bibliothèque de modèles Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)