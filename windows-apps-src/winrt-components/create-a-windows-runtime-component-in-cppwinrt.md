---
title: Composants Windows Runtime avec C++/WinRT
description: Cette rubrique montre comment utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour créer et consommer un composant Windows Runtime &mdash; un composant pouvant être appelé à partir d’une application Windows universelle créée à l’aide de n’importe quel langage de Windows Runtime.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, composant, composants Windows Runtime composant, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: adf13308b1a2c360d7db53ded4edfe866de6c260
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220312"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Composants Windows Runtime avec C++/WinRT

Cette rubrique montre comment utiliser [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour créer et consommer un composant Windows Runtime &mdash; un composant pouvant être appelé à partir d’une application Windows universelle créée à l’aide de n’importe quel langage de Windows Runtime.

Il existe plusieurs raisons pour créer un composant Windows Runtime en C++/WinRT.
- Pour bénéficier de l’avantage en termes de performances de C++ dans les opérations complexes ou nécessitant de nombreuses ressources de calcul.
- Pour réutiliser du code C++ standard déjà écrit et testé.
- Pour exposer les fonctionnalités Win32 à une application plateforme Windows universelle (UWP) écrite dans, par exemple, C#.

En général, lorsque vous créez votre composant C++/WinRT, vous pouvez utiliser des types de la bibliothèque C++ standard et des types intégrés, sauf à la limite ABI (application binary interface) où vous passez des données vers et depuis du code dans un autre `.winmd` Package. Au niveau de l’ABI, utilisez les types de Windows Runtime. En outre, dans votre code C++/WinRT, utilisez des types tels que Delegate et Event pour implémenter des événements qui peuvent être déclenchés à partir de votre composant et gérés dans un autre langage. Consultez [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) pour plus d’informations sur c++/WinRT.

Le reste de cette rubrique vous explique comment créer un composant Windows Runtime en C++/WinRT, puis comment le consommer à partir d’une application.

Le composant Windows Runtime que vous allez générer dans cette rubrique contient une classe d’exécution représentant un compte bancaire. La rubrique illustre également une application principale qui utilise la classe d’exécution du compte bancaire et appelle une fonction pour ajuster le solde.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](../cpp-and-winrt-apis/consume-apis.md) et [Créer des API avec C++/WinRT](../cpp-and-winrt-apis/author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Créer un composant Windows Runtime (BankAccountWRC)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Composant Windows Runtime (C++/WinRT)** et nommez-le *BankAccountWRC* (pour « composant Windows Runtime de compte bancaire »). Vérifiez que l’option **Placer la solution et le projet dans le même répertoire** n’est pas cochée. Ciblez la dernière version en disponibilité générale (autrement dit, pas la préversion) du SDK Windows. Si vous nommez le projet *BankAccountWRC*, vous suivrez le reste des étapes de cette rubrique le plus facilement possible. 

Ne générez pas encore le projet.

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Dans l’Explorateur de solutions, renommez ce fichier `BankAccount.idl` (le fait de renommer le fichier `.idl` a aussi pour effet de renommer automatiquement les fichiers dépendants `.h` et `.cpp`). Remplacez le contenu de l’élément `BankAccount.idl` par le listing ci-dessous.

> [!NOTE]
> Inutile de préciser que vous n’implémentez pas de logiciel financier de production de cette manière ; `Single` dans cet exemple, nous utilisons uniquement pour des raisons pratiques.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

Enregistrez le fichier. Le projet ne sera pas généré complètement pour le moment, mais cette étape est utile car elle régénère les fichiers de code source dans lesquels vous allez implémenter la classe runtime **BankAccount**. Continuons et générons le projet maintenant (les erreurs de génération auxquelles vous pouvez vous attendre à ce stade sont liées à des éléments `Class.h` et `Class.g.h` introuvables).

Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer le fichier de métadonnées Windows Runtime de votre composant (à savoir `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BankAccount** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`.

Cliquez avec le bouton droit de la souris sur le nœud du projet, puis cliquez sur **Ouvrir le dossier dans l'Explorateur de fichiers**. Le dossier du projet s’ouvre dans l'Explorateur de fichiers. De là, copiez les fichiers stub `BankAccount.h` et `BankAccount.cpp` du dossier `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` vers le dossier contenant vos fichiers projet, c’est-à-dire `\BankAccountWRC\BankAccountWRC\`, puis remplacez les fichiers dans la destination. Maintenant, nous allons ouvrir `BankAccount.h` et `BankAccount.cpp`, et implémenter notre classe runtime. Dans `BankAccount.h` , ajoutez un nouveau membre privé à l’implémentation (et*non* à l’implémentation de la fabrique) de **BankAccount**.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

Dans `BankAccount.cpp` , implémentez la méthode **AdjustBalance** comme indiqué dans la liste ci-dessous.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

Vous devrez aussi supprimer `static_assert` des deux fichiers.

Si un avertissement vous empêche de procéder à la génération, corrigez les erreurs ou définissez la propriété de projet **C/C++**  > **Général** > **Considérer les avertissements comme des erreurs** sur **Non (/WX-)** et générez de nouveau le projet.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime

Créez à présent un nouveau projet (dans votre solution *BankAccountWRC* ou dans une nouvelle solution). Créez un projet **Application de base (C++/WinRT)** et nommez-le *BankAccountCoreApp*. Définissez *BankAccountCoreApp* comme projet de démarrage si les deux projets se trouvent dans la même solution.

> [!NOTE]
> Comme mentionné précédemment, le fichier de métadonnées Windows Runtime de votre composant Windows Runtime (dont vous avez nommé le projet *BankAccountWRC*) est créé dans le dossier `\BankAccountWRC\Debug\BankAccountWRC\`. Le premier segment de ce chemin correspond au nom du dossier qui contient votre fichier de solution, le segment suivant est le sous-répertoire du projet nommé `Debug` et le dernier segment est le sous-répertoire du projet nommé de votre composant Windows Runtime. Si vous n’avez pas nommé votre projet *BankAccountWRC*, votre fichier de métadonnées se trouve dans le dossier `\<YourProjectName>\Debug\<YourProjectName>\`.

Maintenant, dans votre projet Core App (*BankAccountCoreApp*), ajoutez une référence, puis accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou ajoutez une référence de projet à projet, si les deux projets se trouvent dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez *BankAccountCoreApp*. Dans le cas improbable où vous voyez une erreur indiquant que le fichier de charge utile `readme.txt` n’existe pas, excluez ce fichier du projet Composant Windows Runtime, régénérez-le, puis regénérez *BankAccountCoreApp*.

Pendant le processus de génération, l’outil `cppwinrt.exe` est exécuté pour traiter le fichier `.winmd` référencé dans les fichiers de code source contenant les types projetés afin de vous aider à utiliser votre composant. L’en-tête pour les types projetés des classes runtime de votre composant &mdash; nommé `BankAccountWRC.h` &mdash; est généré dans le dossier `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluez cet en-tête dans `App.cpp`.

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

Dans `App.cpp` , ajoutez également le code suivant pour instancier un objet **BankAccount** (à l’aide du constructeur par défaut du type projeté) et appelez une méthode sur l’objet compte bancaire.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

Chaque fois que vous cliquez sur la fenêtre, vous incrémentez le solde de l’objet de compte bancaire. Vous pouvez définir des points d’arrêt si vous souhaitez parcourir le code pour confirmer que l’application appelle vraiment le composant Windows Runtime.

## <a name="next-steps"></a>Étapes suivantes

Pour ajouter encore plus de fonctionnalités, ou de nouveaux types de Windows Runtime, à votre composant C++/WinRT Windows Runtime, vous pouvez suivre les mêmes modèles que ceux indiqués ci-dessus. Tout d’abord, utilisez IDL pour définir les fonctionnalités que vous souhaitez exposer. Générez ensuite le projet dans Visual Studio pour générer une implémentation de stub. Puis terminez l’implémentation selon le cas. Toutes les méthodes, propriétés et événements que vous définissez dans IDL sont visibles par l’application qui consomme votre composant Windows Runtime. Pour plus d’informations sur IDL, consultez [Introduction à Microsoft Interface Definition Language 3,0](/uwp/midl-3/intro).

Pour obtenir un exemple d’ajout d’un événement à votre composant Windows Runtime, consultez [créer des événements en C++/WinRT](../cpp-and-winrt-apis/author-events.md).

## <a name="troubleshooting"></a>Dépannage

| Symptôme | Solution |
|---------|--------|
|Dans une application C++/WinRT, lors de l’utilisation d’un [composant C# Windows Runtime](./creating-windows-runtime-components-in-csharp-and-visual-basic.md) qui utilise XAML, le compilateur génère une erreur au format «*MyNamespace_XamlTypeInfo » : n’est pas membre de « WinRT :: MyNamespace »*, &mdash; où *MyNamespace* est le nom de l’espace de noms du composant Windows Runtime. | Dans `pch.h` , dans l’application/WinRT C++ consommatrice, ajoutez `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; en remplaçant *MyNamespace* , le cas échéant. |