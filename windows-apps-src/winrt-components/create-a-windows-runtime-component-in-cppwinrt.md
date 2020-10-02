---
title: Composants Windows Runtime avec C++/WinRT
description: Cette rubrique montre comment utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour créer et consommer un composant Windows Runtime &mdash; un composant pouvant être appelé à partir d’une application Windows universelle créée à l’aide de n’importe quel langage de Windows Runtime.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, composant, composants Windows Runtime composant, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646292"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Composants Windows Runtime avec C++/WinRT

Cette rubrique montre comment utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour créer et consommer un composant Windows Runtime &mdash; un composant pouvant être appelé à partir d’une application Windows universelle créée à l’aide de n’importe quel langage de Windows Runtime.

Il existe plusieurs raisons pour créer un composant Windows Runtime en C++/WinRT.
- Pour bénéficier de l’avantage en termes de performances de C++ dans les opérations complexes ou nécessitant de nombreuses ressources de calcul.
- Pour réutiliser du code C++ standard déjà écrit et testé.
- Pour exposer les fonctionnalités Win32 à une application plateforme Windows universelle (UWP) écrite dans, par exemple, C#.

En général, lorsque vous créez votre composant C++/WinRT, vous pouvez utiliser des types de la bibliothèque C++ standard et des types intégrés, sauf à la limite ABI (application binary interface) où vous passez des données vers et depuis du code dans un autre `.winmd` Package. Au niveau de l’ABI, utilisez les types de Windows Runtime. En outre, dans votre code C++/WinRT, utilisez des types tels que Delegate et Event pour implémenter des événements qui peuvent être déclenchés à partir de votre composant et gérés dans un autre langage. Consultez [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour plus d’informations sur c++/WinRT.

Le reste de cette rubrique vous explique comment créer un composant Windows Runtime en C++/WinRT, puis comment le consommer à partir d’une application.

Le composant Windows Runtime que vous allez générer dans cette rubrique contient une classe d’exécution représentant un thermomètre. La rubrique illustre également une application principale qui utilise la classe de Runtime thermomètre et appelle une fonction pour ajuster la température.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](../cpp-and-winrt-apis/consume-apis.md) et [Créer des API avec C++/WinRT](../cpp-and-winrt-apis/author-apis.md).

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>Créer un composant Windows Runtime (ThermometerWRC)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Windows Runtime Component (C++/WinRT)** , puis nommez-le *ThermometerWRC* (pour « thermomètre Windows Runtime Component »). Vérifiez que l’option **Placer la solution et le projet dans le même répertoire** n’est pas cochée. Ciblez la dernière version en disponibilité générale (autrement dit, pas la préversion) du SDK Windows. L’attribution d’un nom au projet *ThermometerWRC* vous offre l’expérience la plus simple avec les autres étapes de cette rubrique. 

Ne générez pas encore le projet.

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Dans l’Explorateur de solutions, renommez ce fichier `Thermometer.idl` (le fait de renommer le fichier `.idl` a aussi pour effet de renommer automatiquement les fichiers dépendants `.h` et `.cpp`). Remplacez le contenu de l’élément `Thermometer.idl` par le listing ci-dessous.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

Enregistrez le fichier. Le projet ne s’exécutera pas jusqu’à la fin à ce moment-là, mais la génération est une chose utile, car il génère les fichiers de code source dans lesquels vous allez implémenter la classe d’exécution du **thermomètre** . Continuons et générons le projet maintenant (les erreurs de génération auxquelles vous pouvez vous attendre à ce stade sont liées à des éléments `Class.h` et `Class.g.h` introuvables).

Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer le fichier de métadonnées Windows Runtime de votre composant (à savoir `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent des stubs qui vous permettent de commencer à implémenter la classe Runtime **thermomètre** que vous avez déclarée dans votre IDL. Ces stubs sont `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` et `Thermometer.cpp`.

Cliquez avec le bouton droit de la souris sur le nœud du projet, puis cliquez sur **Ouvrir le dossier dans l'Explorateur de fichiers**. Le dossier du projet s’ouvre dans l'Explorateur de fichiers. De là, copiez les fichiers stub `Thermometer.h` et `Thermometer.cpp` du dossier `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` vers le dossier contenant vos fichiers projet, c’est-à-dire `\ThermometerWRC\ThermometerWRC\`, puis remplacez les fichiers dans la destination. Maintenant, nous allons ouvrir `Thermometer.h` et `Thermometer.cpp`, et implémenter notre classe runtime. Dans `Thermometer.h` , ajoutez un nouveau membre privé à l’implémentation (et*non* à l’implémentation de fabrique) du **thermomètre**.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

Dans `Thermometer.cpp` , implémentez la méthode **AdjustTemperature** comme indiqué dans la liste ci-dessous.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

Vous devrez aussi supprimer `static_assert` des deux fichiers.

Si un avertissement vous empêche de procéder à la génération, corrigez les erreurs ou définissez la propriété de projet **C/C++**  > **Général** > **Considérer les avertissements comme des erreurs** sur **Non (/WX-)** et générez de nouveau le projet.

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>Créer une application principale (ThermometerCoreApp) pour tester le composant Windows Runtime

Créez maintenant un projet (dans votre solution *ThermometerWRC* ou dans un nouveau projet). Créez un projet d' **application principale (C++/WinRT)** et nommez-le *ThermometerCoreApp*. Définissez *ThermometerCoreApp* comme projet de démarrage si les deux projets se trouvent dans la même solution.

> [!NOTE]
> Comme mentionné précédemment, le fichier de métadonnées Windows Runtime de votre composant Windows Runtime (dont vous avez nommé le projet *ThermometerWRC*) est créé dans le dossier `\ThermometerWRC\Debug\ThermometerWRC\` . Le premier segment de ce chemin correspond au nom du dossier qui contient votre fichier de solution, le segment suivant est le sous-répertoire du projet nommé `Debug` et le dernier segment est le sous-répertoire du projet nommé de votre composant Windows Runtime. Si vous n’avez pas nommé votre projet *ThermometerWRC*, votre fichier de métadonnées se trouve dans le dossier `\<YourProjectName>\Debug\<YourProjectName>\` .

À présent, dans votre projet d’application principal (*ThermometerCoreApp*), ajoutez une référence, puis accédez `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` (ou ajoutez une référence de projet à un projet, si les deux projets se trouvent dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. Créez maintenant *ThermometerCoreApp*. Dans le cas improbable où vous voyez une erreur indiquant que le fichier `readme.txt` de charge utile n’existe pas, excluez ce fichier du Windows Runtime projet de composant, régénérez-le, puis régénérez *ThermometerCoreApp*.

Pendant le processus de génération, l’outil `cppwinrt.exe` est exécuté pour traiter le fichier `.winmd` référencé dans les fichiers de code source contenant les types projetés afin de vous aider à utiliser votre composant. L’en-tête pour les types projetés des classes runtime de votre composant &mdash; nommé `ThermometerWRC.h` &mdash; est généré dans le dossier `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\`.

Incluez cet en-tête dans `App.cpp`.

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

Dans `App.cpp` , ajoutez également le code suivant pour instancier un objet **thermomètre** (à l’aide du constructeur par défaut du type projeté) et appelez une méthode sur l’objet thermomètre.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

Chaque fois que vous cliquez sur la fenêtre, vous incrémentez la température de l’objet thermomètre. Vous pouvez définir des points d’arrêt si vous souhaitez parcourir le code pour confirmer que l’application appelle vraiment le composant Windows Runtime.

## <a name="next-steps"></a>Étapes suivantes

Pour ajouter encore plus de fonctionnalités, ou de nouveaux types de Windows Runtime, à votre composant C++/WinRT Windows Runtime, vous pouvez suivre les mêmes modèles que ceux indiqués ci-dessus. Tout d’abord, utilisez IDL pour définir les fonctionnalités que vous souhaitez exposer. Générez ensuite le projet dans Visual Studio pour générer une implémentation de stub. Puis terminez l’implémentation selon le cas. Toutes les méthodes, propriétés et événements que vous définissez dans IDL sont visibles par l’application qui consomme votre composant Windows Runtime. Pour plus d’informations sur IDL, consultez [Introduction à Microsoft Interface Definition Language 3,0](/uwp/midl-3/intro).

Pour obtenir un exemple d’ajout d’un événement à votre composant Windows Runtime, consultez [créer des événements en C++/WinRT](../cpp-and-winrt-apis/author-events.md).

## <a name="troubleshooting"></a>Dépannage

| Symptôme | Solution |
|---------|--------|
|Dans une application C++/WinRT, lors de l’utilisation d’un [composant Windows Runtime C#](./creating-windows-runtime-components-in-csharp-and-visual-basic.md) qui utilise XAML, le compilateur génère une erreur au format «  *'MyNamespace_XamlTypeInfo' : n’est pas un membre de 'winrt::MyNamespace'*  », &mdash;où *MyNamespace* est le nom de l’espace de noms du composant Windows Runtime. | Dans `pch.h` dans l’application qui utilise C++/WinRT, ajoutez `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash;remplacer *MyNamespace* si nécessaire. |
