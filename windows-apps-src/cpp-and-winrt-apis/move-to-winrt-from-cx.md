---
description: Cette rubrique décrit les détails techniques impliqués dans le portage du code source dans un projet [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers son équivalent dans [C++/WinRT](./intro-to-using-cpp-with-winrt.md).
title: Passer de C++/CX à C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 94ffa80700cea640d63f63344991144a2ac00ab6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157313"
---
# <a name="move-to-cwinrt-from-ccx"></a>Passer de C++/CX à C++/WinRT

Cette rubrique est la première d’une série qui décrit comment vous pouvez déplacer le code source dans votre projet [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) vers son équivalent dans [C++/WinRT](./intro-to-using-cpp-with-winrt.md).

Si votre projet utilise également des types [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consultez [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md).

## <a name="strategies-for-porting"></a>Stratégies de déplacement

Il est utile de savoir que ce déplacement de C++/CX vers C++/WinRT est généralement simple, à l’exception du déplacement depuis des tâches [Bibliothèque de modèles parallèles (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) vers des coroutines. Les modèles sont différents. Il n’existe pas de mappage un-à-un naturel entre les tâches PPL et les coroutines et il n’existe aucun moyen simple de déplacer mécaniquement le code qui fonctionne pour tous les cas. Pour obtenir de l’aide sur cet aspect spécifique du déplacement et sur les options d’interopérabilité entre les deux modèles, consultez [Asynchronisme et interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx-async.md).

Les équipes de développement signalent régulièrement qu’une fois qu’elles dépassent le seuil de déplacement de leur code asynchrone, le reste du travail de déplacement est essentiellement mécanique.

### <a name="porting-in-one-pass"></a>Déplacement en une seule passe

Si vous êtes en mesure de déplacer votre projet entier en une seule passe, vous n’aurez besoin que de cette rubrique pour obtenir les informations dont vous avez besoin (et vous n’aurez pas besoin des rubriques *Interopérabilité* qui suivent celle-ci). Nous vous recommandons de commencer par créer un nouveau projet dans Visual Studio à l’aide de l’un des modèles de projet C++/WinRT (consultez [Support Visual Studio pour C++ /WinRT](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Déplacez ensuite vos fichiers de code source vers ce nouveau projet et déplacez tout le code source C++/CX vers C++/WinRT, comme vous le feriez.

Sinon, si vous préférez effectuer le déplacement dans votre projet C++/CX existant, vous devez y ajouter le support C++/WinRT. Les étapes que vous suivez sont décrites dans [Prise en charge du projet et ajout du support C++/WinRT](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support). Pendant que vous avez effectué le déplacement, vous avez transformé un pur projet C++/CX en pur projet C++/WinRT.

> [!NOTE]
> Si vous avez un projet de composant Windows Runtime, le déplacement en une seule passe est votre seule option. Un projet de composant Windows Runtime écrit dans C++ doit contenir soit tous les codes sources C++/CX ou tous les codes sources C++/WinRT. Ils ne peuvent pas coexister dans ce type de projet.

### <a name="porting-a-project-gradually"></a>Déplacement progressif d’un projet

Excepté les projets de composant Windows Runtime, comme indiqué dans la section précédente, si la taille ou la complexité de votre codebase rend le déplacement de votre projet progressif nécessaire, vous aurez besoin d’un processus de déplacement dans lequel le code C++/CX et C++/WinRT existent côte à côte pendant un moment dans le même projet. En plus de lire cette rubrique, consultez également [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md) et [Asynchronisme et l’interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx-async.md). Ces rubriques fournissent des informations et des exemples de code montrant comment inter opérer entre les deux projections de langage.

Pour préparer un projet en vue d’un processus de déplacement progressif, vous pouvez ajouter le support C++/WinRT à votre projet C++/CX. Les étapes que vous suivez sont décrites dans [Prise en charge du projet et ajout du support C++/WinRT](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support). Vous pouvez ensuite procéder à un déplacement progressif à partir de là.

Une autre option consiste à créer un nouveau projet dans Visual Studio à l’aide de l’un des modèles de projet C++/WinRT (consultez [Support Visual Studio pour C++ /WinRT](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Puis ajoutez le support C++/CX à ce projet. Les étapes que vous suivez sont décrites dans [Prise en charge du projet C++/WinRT et ajout du support C++/CX](./interop-winrt-cx.md#taking-a-cwinrt-project-and-adding-cx-support). Vous pouvez alors commencer à déplacer votre code source et déplacer *certains* codes sources C++/CX vers C++/WinRT ce faisant.

Dans les deux cas, vous interagissez (des deux façons) entre votre code C++/WinRT et tout code C++/CX que vous n’avez pas encore déplacé.

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) et le SDK Windows déclarent tous les deux les types dans l’espace de noms racine **Windows**. Un type Windows projeté en C++/WinRT a le même nom complet que le type Windows, mais il est placé dans l'espace de noms C++ **winrt**. Ces espaces de noms distincts vous permettent de porter le code C++/CX vers C++/WinRT à votre propre rythme.

#### <a name="porting-a-xaml-project-gradually"></a>Déplacement progressif d’un projet XAML

> [!IMPORTANT]
> Pour un projet qui utilise XAML, à tout moment, tous vos types de pages XAML doivent être entièrement C++/CX ou entièrement C++/WinRT. Vous pouvez toujours mélanger C++/CX et C++/WinRT *en dehors* des types de pages XAML dans le même projet (dans vos modèles et vues de modèles et ailleurs).

Pour ce scénario, le flux de travail que nous recommandons est de créer un nouveau projet C++/WinRT et de copier le code source et le balisage à partir du projet C++/CX. Tant que tous vos types de pages XAML sont C++/WinRT, vous pouvez ajouter de nouvelles pages XAML avec **Projet** \> **Ajouter un nouvel élément...** \> **Visual C++**  > **Page vierge (C++/WinRT)** .

Sinon, vous pouvez utiliser un composant Windows Runtime pour factoriser le code du projet C++/CX XAML lorsque vous le déplacez.

- Vous pouvez créer un nouveau projet WRC C++/CX, déplacer autant de code C++/CX que vous le pouvez dans ce projet, puis modifier le projet XAML en C++/WinRT.
- Ou vous pouvez créer un nouveau projet WRC C++/WinRT, laisser le projet XAML au format C++/CX et commencer le déplacement du code C++/CX vers C++/WinRT en déplaçant le code résultat hors du projet XAML et dans le projet de composant.
- Vous pouvez également utiliser un projet de composant C++/CX parallèlement à un projet de composant C++/WinRT dans la même solution, référencer ces deux projets à partir de votre projet d’application, puis effectuer un portage progressif de l’un à l’autre. Une fois de plus , consultez [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md) pour plus d’informations sur l’utilisation des deux projections de langage dans le même projet.

## <a name="first-steps-in-porting-a-ccx-project-to-cwinrt"></a>Premières étapes du portage d’un projet C++/CX vers C++/WinRT

Quelle que soit votre stratégie de déplacement (déplacement en une seule passe ou déplacement graduel), la première étape consiste à préparer votre projet pour le déplacement. Voici un récapitulatif de ce que nous avons décrit dans [Stratégies de déplacement](#strategies-for-porting) en termes de type de projet sur lequel vous allez commencer à travailler et comment le configurer.

- **Déplacement en une seule passe**. Créez un nouveau projet dans Visual Studio à l’aide de l’un des exemples de projet C++/WinRT. Déplacez les fichiers de votre projet C++/CX vers ce nouveau projet, puis déplacez le code source C++/CX.
- **Déplacement progressif d’un projet non XAML**. Vous pouvez choisir d’ajouter le support C++/WinRT vers votre projet C++/CX (consultez [Prise en charge d’un projet C++/CX et ajout du support C++/WinRT](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)) et déplacement progressif. Vous pouvez choisir de créer un nouveau projet C++/WinRT et d’y ajouter le support C++/CX (consultez [Prise en charge d’un projet C++/WinRT et ajout du support C++/CX](./interop-winrt-cx.md#taking-a-cwinrt-project-and-adding-cx-support)), y déplacer des fichiers de façon progressive.
- **Déplacement progressif d’un projet XAML**. Créez un nouveau projet C++/WinRT, y déplacez des fichiers de façon progressive. À un moment donné, vos types de pages XAML doivent être *soit* tous les C++/WinRT *soit* tous les C++/CX.

Le reste de cette rubrique s’applique quelle que soit la stratégie de déplacement choisie. Elle contient un catalogue de détails techniques impliqués dans le déplacement du code source de C++/CX vers C++/WinRT. Si vous effectuez un déplacement progressif, vous souhaiterez peut-être également consulter [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md) et [Asynchronisme et interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx-async.md).

## <a name="file-naming-conventions"></a>Conventions d’affectation de noms de fichier

### <a name="xaml-markup-files"></a>Fichiers de balisage XAML

| Origine du fichier | C++/CX | C++/WinRT |
| - | - | - |
| **Fichiers XAML pour les développeurs** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (voir ci-dessous) |
| **Fichiers XAML générés** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

Notez que C++/WinRT supprime l’extension `.xaml` des noms de fichier `*.h` et `*.cpp`.

C++/WinRT ajoute un fichier pour les développeurs supplémentaire, le **fichier Midl (.idl)** . C++/CX génère automatiquement ce fichier en interne en y ajoutant tous les membres publics et protégés. Dans C++/WinRT, vous ajoutez et créez le fichier vous-même. Pour obtenir plus d’informations et d’exemples de code ainsi qu’un guide pas à pas sur la création d’IDL, consultez [Contrôles XAML ; liaison à une propriété C++/WinRT](./binding-property.md).

Consultez également [Factorisation des classes runtime dans des fichiers Midl (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)

### <a name="runtime-classes"></a>Classes runtime

C++/CX n’impose pas de restrictions sur les noms de vos fichiers d’en-tête. Il est courant de placer plusieurs définitions de classe runtime dans un seul fichier d’en-tête, en particulier pour les petites classes. Toutefois, C++/WinRT exige que chaque classe runtime ait son propre fichier d’en-tête nommé d’après le nom de la classe. 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

Dans C++/CX, utiliser des fichiers d’en-tête nommés différemment pour les contrôles personnalisés XAML est une pratique moins courante (mais toujours légale). Vous devrez renommer le fichier d’en-tête pour qu’il corresponde au nom de la classe.

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>Exigences relatives aux fichiers d’en-tête

C++/CX n’exige pas d’inclure des fichiers d’en-tête spéciaux, car il génère automatiquement des fichiers d’en-tête en interne à partir de fichiers `.winmd`. Dans C++/CX, il est courant d’utiliser des directives `using` pour les espaces de noms que vous utilisez par nom.

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

La directive `using namespace Windows::Media::Playback` nous permet d’écrire `MediaPlaybackItem` sans préfixe d’espace de noms. Nous avons également touché l’espace de noms `Windows.Media.Core`, car `item->VideoTracks->GetAt(0)` retourne un fichier **Windows.Media.Core.VideoTrack**. Cependant, nous n’avons jamais dû entrer le nom **VideoTrack**, donc nous n’avions pas besoin d’une directive `using Windows.Media.Core`.

Toutefois, C++/WinRT vous oblige à inclure un fichier d’en-tête correspondant à chaque espace de noms que vous utilisez, même si vous ne le nommez pas.

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

D’autre part, même si l’événement **MediaPlaybackItem.AudioTracksChanged** est de type **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** , nous n’avons pas besoin d’inclure `winrt/Windows.Foundation.Collections.h` car nous n’avons pas utilisé cet événement.

C++/WinRT exige également que vous ajoutiez des fichiers d’en-tête pour les espaces de noms qui sont utilisés par le balisage XAML.

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

Si vous utilisez la classe **Rectangle**, vous devez ajouter l’élément suivant.

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

Si vous oubliez un fichier d’en-tête, la compilation sera effectuée correctement mais vous recevrez des erreurs relatives aux éditeurs de liens car les classes `consume_` sont manquantes.

## <a name="parameter-passing"></a>Passage de paramètres

Lorsque vous écrivez du code source C++/CX, vous transmettez des types C++/CX en tant que paramètres de fonction sous forme de références accent circonflexe (\^).

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, pour les fonctions synchrones, vous devez utiliser les paramètres `const&` par défaut. Cela évitera une surcharge liée aux copies et imbrications. Mais vos coroutines doivent utiliser passer par valeur pour être sûr qu’elles capturent par valeur et éviter les problèmes de durée de vie (pour plus d’informations, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

L'objet C++/WinRT est fondamentalement une valeur qui contient un pointeur d’interface vers l'objet Windows Runtime de sauvegarde. Lorsque vous copiez un objet C++/WinRT, le compilateur copie le pointeur d’interface encapsulé, en incrémentant son nombre de références. Une destruction éventuelle de la copie implique de décrémenter le nombre de références. Par conséquent, ne faites subir la surcharge liée à une copie que lorsque c'est nécessaire.

## <a name="variable-and-field-references"></a>Références de variable et de champ

Lorsque vous écrivez du code source C++/CX, vous utilisez des variables accent circonflexe (\^) pour référencer des objets Windows Runtime et l'opérateur flèche (-&gt;) pour supprimer la référence à une variable accent circonflexe.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Lors du portage vers le code C++/WinRT équivalent, vous pouvez vous faciliter la tâche en supprimant les accents circonflexes et en remplaçant l’opérateur flèche (-&gt;) par l’opérateur point (.). Les types C++/WinRT projetés sont des valeurs et non des pointeurs.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

Le constructeur par défaut pour une référence d’accent circonflexe C++/CX s’initialise à la valeur null. Voici un exemple de code C++/CX dans lequel nous créer une variable/un champ de type correct, mais sans l’initialiser. En d’autres termes, il ne fait pas référence initialement à un **TextBlock**; nous comptons lui assigner une référence plus tard.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Pour l’équivalent en C++/WinRT, consultez [Initialisation différée](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Propriétés

Les extensions de langage C++/CX incluent le concept de propriétés. Lorsque vous écrivez du code source C++/CX, vous pouvez accéder à une propriété comme s’il s’agissait d’un champ. Le code C++ standard n’a pas de concept de propriété, donc, en C++/WinRT, vous appelez les fonctions get et set.

Dans les exemples qui suivent, **XboxUserId**, **UserState**, **PresenceDeviceRecords** et **Taille** sont toutes des propriétés.

### <a name="retrieving-a-value-from-a-property"></a>Récupération de la valeur d’une propriété

Voici comment obtenir une valeur de propriété en C++/CX.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

Le code source C++/WinRT équivalent appelle une fonction qui a le même nom que la propriété, mais sans paramètres.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Notez que la fonction **PresenceDeviceRecords** renvoie un objet Windows Runtime qui a lui-même une fonction **Taille**. Comme l’objet renvoyé est également un type projeté C++/WinRT, nous supprimons la référence à l’aide de l’opérateur point pour appeler **Taille**.

### <a name="setting-a-property-to-a-new-value"></a>Définition d’une propriété sur une nouvelle valeur

La définition d’une propriété sur une nouvelle valeur suit un modèle similaire. Tout d'abord, en C++/CX.

```cppcx
record->UserState = newValue;
```

Pour effectuer l’équivalent en C++ /WinRT, vous appelez une fonction portant le même nom que la propriété et vous passez un argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Création d’une instance d’une classe

Vous travaillez avec un objet C++/CX via un handle vers lui, communément appelé référence accent circonflexe (\^). Vous créez un objet via le mot clé `ref new`, qui à son tour appelle [**RoActivateInstance**](/windows/desktop/api/roapi/nf-roapi-roactivateinstance) pour activer une nouvelle instance de la classe runtime.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

L'objet C++/WinRT est une valeur ; par conséquent, vous pouvez l’allouer sur la pile, ou en tant que champ d’un objet. Vous n'utilisez *jamais*`ref new` (ni `new`) pour allouer un objet C++/WinRT. En arrière-plan, **RoActivateInstance** est toujours appelé.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si une ressource est coûteuse à initialiser, il est courant de retarder son initialisation jusqu'à ce qu'elle soit réellement nécessaire. Comme nous l’avons mentionné précédemment, le constructeur par défaut pour une référence d’accent circonflexe C++/CX s’initialise à la valeur null.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

Le même code porté en C++/WinRT. Notez l’utilisation du constructeur **std::nullptr_t**. Pour plus d’informations sur ce constructeur, consultez [Initialisation différée](consume-apis.md#delayed-initialization).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="how-the-default-constructor-affects-collections"></a>Comment le constructeur par défaut affecte-t-il les collections ?

Les types de collections C++ utilisent le constructeur par défaut, ce qui peut entraîner une construction d’objet involontaire.

| Scénario | C++/CX | C++/WinRT (incorrect) | C++/WinRT (correct) |
| - | - | - | - |
| Variable locale, initialement vide | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| Variable de membre, initialement vide | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| Variable globale, initialement vide | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| Vecteur de références vides | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| Définir une valeur dans un mappage | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| Tableau de références vides | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| Coupler | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>En savoir plus sur les collections de références vides

Quand vous avez un **Platform::Array\^** (voir [Port **Platform::Array\^** ](#port-platformarray)) dans C++/CX, vous avez la possibilité de le porter vers un **std::vector** dans C++/WinRT (en fait, n’importe quel conteneur contigu) au lieu de le conserver comme tableau. Faire le choix d’utiliser **std::vector** présente des avantages.

Par exemple, alors qu’il existe un raccourci pour créer un vecteur de taille fixe de références vides (voir le tableau ci-dessus), il n’en existe pas pour créer un *tableau* de références vides. Vous devez donc répéter `nullptr` pour chaque élément dans un tableau. Si vous n’en avez pas assez, les éléments supplémentaires sont construits par défaut.

S’il s’agit d’un vecteur, vous pouvez le remplir avec des références vides soit au moment de l’initialisation (comme dans le tableau ci-dessus), soit après l’initialisation en utilisant un code comme celui-ci.

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>En savoir plus sur l’exemple **std::map**

L’opérateur d’indice `[]` pour **std::map** se comporte comme suit.

- Si la clé est trouvée dans le mappage, retournez une référence à la valeur existante (vous pourrez la remplacer).
- Si la clé n’est pas trouvée dans le mappage, créez une entrée dans le mappage qui comprend la clé (déplacée, si elle est déplaçable) et une *valeur construite par défaut*, puis retournez une référence à la valeur (vous pourrez la remplacer ultérieurement).

En d’autres termes, l’opérateur `[]` crée toujours une entrée dans le mappage, ce qui est différent de C#, Java et JavaScript.

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Conversion d’une classe runtime de base vers une classe dérivée

Il est courant d’utiliser une référence à la base qui fait référence à un objet d’un type dérivé. Dans C++/CX, vous utilisez `dynamic_cast` pour *diffuser* la référence à la base dans une référence à un objet dérivé. Le `dynamic_cast` est simplement un appel masqué à [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Voici un exemple typique&mdash;vous gérez un événement de modification d’une propriété de dépendance et souhaitez diffuser à partir de **DependencyObject** vers le type réel qui possède la propriété de dépendance.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

Le code C++/WinRT équivalent remplace `dynamic_cast` par un appel à la fonction [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function), qui encapsule **QueryInterface**. Vous avez également la possibilité d’appeler [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), qui lève une exception si l’interrogation de l’interface requise (interface par défaut du type que vous demandez) n’est pas retournée. Voici un exemple de code C++/WinRT.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>Classes dérivées

La classe de base doit être *composable* pour être dérivée à partir d’une classe runtime. C++/CX ne nécessite pas de procédure spéciale pour que vos classes soient composables, contrairement à C++/WinRT. Vous utilisez le [mot clé unsealed](/uwp/midl-3/intro#base-classes) pour indiquer que vous souhaitez que votre classe soit utilisable comme classe de base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

Dans votre classe d’en-tête d’implémentation, vous devez ajouter le fichier d’en-tête de la classe de base avant d’inclure l’en-tête généré automatiquement pour la classe dérivée. Dans le cas contraire, vous obtiendrez des erreurs telles que « Utilisation non conforme de ce type comme expression ».

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Gestion des événements avec un délégué

Voici un exemple type de gestion d’un événement en C++/CX, en utilisant une fonction lambda en tant que délégué dans ce cas.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

C’est l’équivalent en C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Au lieu d’une fonction lambda, vous pouvez choisir d’implémenter votre délégué sous forme d’une fonction libre ou en tant que pointeur-vers-fonction-membre. Pour plus d'informations, voir [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md).

Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne (pas sur les fichiers binaires), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) vous aidera à répliquer ce modèle en C++/WinRT. Consultez également [Délégués paramétrés, signaux simples et rappels au sein d'un projet](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Révocation d’un délégué

Dans C++/CX, vous utilisez l'opérateur `-=` pour révoquer l'inscription d'un événement préalable.

```cppcx
myButton->Click -= token;
```

C’est l’équivalent en C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Pour plus d’informations et d'options, voir [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate).

## <a name="boxing-and-unboxing"></a>Boxing et unboxing

C++/CX effectue automatiquement une conversion boxing des scalaires en objets. C++/WinRT vous oblige à appeler la fonction [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manière explicite. Les deux langages nécessitent un unboxing explicite. Consultez [Conversions boxing et unboxing avec C++/WinRT](./boxing.md).

Dans les tableaux suivants, nous allons utiliser ces définitions.

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| Opération | C++/CX | C++/WinRT|
|-|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX et C# génèrent des exceptions si vous essayez d’effectuer une conversion unboxing d’un pointeur null en un type de valeur. C++/WinRT considère qu’il s’agit d’une erreur de programmation, ce qui provoque un blocage. Dans C++/WinRT, utilisez la fonction [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si vous souhaitez prendre en charge le cas avec l’objet qui n’est pas du type que vous pensiez.

| Scénario | C++/CX | C++/WinRT|
|-|-|-|-|
| Conversion unboxing d’un entier connu | `i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o est null | `Platform::NullReferenceException` | Se bloquer |
| Si o n’est pas un entier converti par boxing | `Platform::InvalidCastException` | Se bloquer |
| Conversion unboxing d’un entier, utiliser la valeur de secours si la valeur est null ; se bloquer si autre chose | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Conversion unboxing d’un entier si possible ; utiliser la valeur de secours pour toute autre chose | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversions boxing et unboxing d’une chaîne

Une chaîne est parfois un type de valeur et parfois un type de référence. C++/CX et C++/WinRT traitent les chaînes différemment.

Le type ABI [**HSTRING**](/windows/win32/winrt/hstring) est un pointeur vers une chaîne avec décompte des références. Toutefois, il ne dérive pas à partir de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Techniquement, il ne s’agit donc pas d’un *objet*. En outre, un pointeur **HSTRING** null représente la chaîne vide. La conversion boxing d’éléments non dérivés à partir de **IInspectable** est effectuée en les encapsulant dans une interface [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_). De plus, Windows Runtime fournit une implémentation standard sous la forme de l’objet [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (les types personnalisés sont signalés sous la forme [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C++/CX représente une chaîne Windows Runtime sous la forme d’un type de référence, tandis que C++/WinRT projette une chaîne sous la forme d’un type de valeur. Cela signifie qu’une chaîne null convertie par boxing peut avoir des représentations différentes selon la façon dont vous l’avez obtenue.

En outre, C++/CX vous permet de déréférencer une chaîne null **String^** . Auquel cas, elle se comporte comme la chaîne `""`.

| Comportement | C++/CX | C++/WinRT|
|-|-|-|
| Déclarations | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| Catégorie de type de chaîne | Type de référence | Type de valeur |
| **HSTRING** null projette sous la forme | `(String^)nullptr` | `hstring{}` |
| Est-ce que null et `""` sont identiques ? | Oui | Oui |
| Validité de la valeur null | `s = nullptr;`<br>`s->Length == 0` (valide) | `s = hstring{};`<br>`s.size() == 0` (valide) |
| Si vous affectez une chaîne null à l’objet | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Si vous affectez `""` à l’objet | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Boxing et unboxing de base.

| Opération | C++/CX | C++/WinRT|
|-|-|-|
| Conversion boxing d’une chaîne | `o = s;`<br>La chaîne vide devient nullptr. | `o = box_value(s);`<br>La chaîne vide devient un objet non null. |
| Conversion unboxing d’une chaîne connue | `s = (String^)o;`<br>L’objet null devient une chaîne vide.<br>InvalidCastException si ce n’est pas une chaîne. | `s = unbox_value<hstring>(o);`<br>L’objet null plante.<br>Plantage si ce n’est pas une chaîne. |
| Unboxing d’une chaîne possible | `s = dynamic_cast<String^>(o);`<br>L’objet null ou la non-chaîne devient une chaîne vide. | `s = unbox_value_or<hstring>(o, fallback);`<br>Null ou non-chaîne devient la valeur de secours.<br>Chaîne vide conservée. |

## <a name="concurrency-and-asynchronous-operations"></a>Concurrence et opérations asynchrones

La bibliothèque de modèles parallèles (PPL) ([**concurrency::task**](/cpp/parallel/concrt/reference/task-class), par exemple) a été mise à jour pour prendre en charge les références d’accent circonflexe C++/CX.

Pour C++/WinRT, utilisez plutôt des coroutines et `co_await`. Pour plus d’informations et des exemples de code, voir [Opérations concurrentes et asynchrones avec C++/WinRT](./concurrency.md).

## <a name="consuming-objects-from-xaml-markup"></a>Utilisation d’objets à partir du balisage XAML

Dans un projet C++/CX, vous pouvez utiliser des éléments nommés et des membres privés à partir du balisage XAML. Toutefois, dans C++/WinRT, toutes les entités utilisées via [**l’extension de balisage {x:Bind}** ](../xaml-platform/x-bind-markup-extension.md) XAML doivent être exposées publiquement dans IDL.

De plus, une liaison à un booléen affiche `true` ou `false` en C++/CX, mais affiche **Windows.Foundation.IReference`1\<Boolean\>** en C++/WinRT.

Pour obtenir plus d’informations et d’exemples de code, consultez [Utilisation d’objets à partir du balisage](./binding-property.md#consuming-objects-from-xaml-markup).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Mappage de types **Platform** C++/CX sur des types C++/WinRT

C++/CX fournit plusieurs types de données dans l'espace de noms **Platform**. Ces types ne sont pas en C++ standard, donc vous ne pouvez les utiliser que lorsque vous activez les extensions de langage Windows Runtime (propriété de projet Visual Studio **C/C++**  > **Général** > **Consommer l'extension Windows Runtime** > **Oui (/ZW)** ). Le tableau ci-dessous vous aide à porter des types **Platform** vers leurs équivalents en C++/WinRT. Une fois que c'est fait, puisque C++/WinRT est en C++ standard, vous pouvez désactiver l'option `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Voir [Port **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>Porter **Platform::Agile\^** vers **winrt::agile_ref**

Le type **Platform::Agile\^** en C++/CX représente une classe Windows Runtime accessible à partir de n’importe quel thread. L’équivalent en C++/WinRT est [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Port **Platform::Array\^**

Dans les cas où C++/CX exige l’utilisation d’un tableau, C++/WinRT vous permet d’utiliser n’importe quel conteneur contigu. Consultez [Comment le constructeur par défaut affecte-t-il les collections ?](#how-the-default-constructor-affects-collections) pour connaître les avantages d’utiliser un **std::vector**.

Ainsi, quand vous avez un **Platform::Array\^** dans C++/CX, les options de portage disponibles incluent l’utilisation d’une liste d’initialiseurs, d’un **std::array** ou d’un **std::vector**. Pour plus d’informations et des exemples de code, consultez [Listes d’initialiseurs standard](./std-cpp-data-types.md#standard-initializer-lists) et [Vecteurs et tableaux standard](./std-cpp-data-types.md#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresult_error"></a>Porter **Platform::Exception\^** vers **winrt::hresult_error**

Le type **Platform::Exception\^** se produit en C++/CX quand une API Windows Runtime retourne un HRESULT autre que S\_OK. L’équivalent en C++/WinRT est [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Pour le portage vers C++/WinRT, modifiez tout le code qui utilise **Platform::Exception\^** afin d'utiliser **winrt::hresult_error**.

En C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

En C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT fournit ces classes d’exceptions.

| Type d'exception | Classe de base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | call [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Notez que chaque classe (via la classe de base **hresult_error**) fournit une fonction [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) qui retourne le HRESULT de l’erreur, et une fonction [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) qui retourne la représentation de chaîne de ce HRESULT.

Voici un exemple de levée d'exception en C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Et voici l’équivalent en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Porter **Platform::Object\^** vers **winrt::Windows::Foundation::IInspectable**

Comme tous les types C++/WinRT, **winrt::Windows::Foundation::IInspectable** est un type de valeur. Voici comment vous initialisez une variable de ce type sur la valeur Null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Porter **Platform::String\^** vers **winrt::hstring**

**Platform::String\^** équivaut au type Windows Runtime HSTRING ABI. Pour C++/WinRT, l’équivalent est [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Mais avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues de la bibliothèque C++ standard comme **std::wstring** et/ou des littéraux de chaîne étendue. Pour obtenir plus d’informations et des exemples de code, voir [Gestion des chaînes en C++/WinRT](strings.md).

Avec C++/CX, vous pouvez accéder à la propriété [**Platform::String::Data**](/cpp/cppcx/platform-string-class?view=vs-2019#data) pour récupérer la chaîne en tant que tableau **const wchar_t\*** de style C (par exemple, pour le passer à **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Pour faire de même avec C++/WinRT, vous pouvez utiliser la fonction [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) pour obtenir une version de la chaîne de style C terminée par Null, exactement comme avec **std::wstring**.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Lorsqu’il s’agit d'implémenter des API qui utilisent ou retournent des chaînes, généralement, vous modifiez tout code C++/CX qui utilise **Platform::String\^\^** pour utiliser **winrt::hstring** à la place.

Voici un exemple d'API C++/CX qui utilise une chaîne.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Pour C++/WinRT, vous pouvez déclarer cette API dans [MIDL 3.0](/uwp/midl-3) comme suit.

```idl
// LogType.idl
void LogWrapLine(String str);
```

La chaîne d’outils C++/WinRT génère pour vous le code source, qui se présente comme suit.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

Les types C++/CX fournissent la méthode [Object::ToString](/cpp/cppcx/platform-object-class#tostring).

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT ne fournit pas directement cette fonctionnalité, mais vous disposez d’alternatives.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT prend également en charge [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) pour certains types. Vous devez ajouter des surcharges pour tous les types supplémentaires que vous souhaitez convertir en chaîne.

| Language | Convertir un entier en chaîne | Convertir une valeur enum en chaîne |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

Si vous souhaitez convertir une valeur enum en chaîne, vous devez fournir l’implémentation de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Ces conversions en chaîne sont souvent utilisées implicitement par la liaison de données.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Ces liaisons effectuent la conversion **winrt::to_hstring** de la propriété liée. Pour le deuxième exemple (**StatusEnum**), vous devez fournir votre propre surcharge de **winrt::to_hstring**. Dans le cas contraire, vous recevrez une erreur relative au compilateur.

#### <a name="string-building"></a>Génération de chaîne

C++/CX et C++/WinRT se basent sur le type **std:: wstringstream** standard pour la génération de chaînes.

| Opération | C++/CX | C++/WinRT |
|-|-|-|
| Ajout de chaîne, conservation des valeurs null | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| Ajout de chaîne, arrêt à la première valeur null | `stream << s->Data();` | `stream << s.c_str();` |
| Extraire le résultat | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>Autres exemples

Dans les exemples ci-dessous , *ws* est une variable de type **std::wstring**. En outre, C++/CX peut construire **Platform::String** à partir d’une chaîne de 8 bits, contrairement à C++/WinRT.

| Opération | C++/CX | C++/WinRT |
| - | - | - |
| Construction d’une chaîne à partir d’un littéral | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| Conversion à partir de **std::wstring**, conservation des valeurs null | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| Conversion à partir de **std::wstring**, arrêt à la première valeur null | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| Conversion vers **std::wstring**, conservation des valeurs null | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| Conversion vers **std::wstring**, arrêt à la première valeur null | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| Transmission du littéral à la méthode | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| Transmission de **std::wstring** à la méthode | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>API importantes

* [Modèle de structure winrt::delegate](/uwp/cpp-ref-for-winrt/delegate)
* [Structure winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes

* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Créer des événements en C++/WinRT](./author-events.md)
* [Opérations concurrentes et asynchrones avec C++/WinRT](./concurrency.md)
* [Utiliser des API avec C++/WinRT](./consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](./handle-events.md)
* [Interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx.md)
* [Asynchronisme, interopérabilité entre C++/WinRT et C++/CX](./interop-winrt-cx-async.md)
* [Référence de Microsoft Interface Definition Language 3.0](/uwp/midl-3)
* [Passer de WRL à C++/WinRT](./move-to-winrt-from-wrl.md)
* [Gestion des chaînes en C++/WinRT](./strings.md)