---
description: Cet article explique comment héberger un contrôle UWP personnalisé dans une application Win32 C++ à l’aide de l’API d’hébergement XAML.
title: Héberger un contrôle UWP personnalisé dans une application C++ Win32 à l’aide de l’API d’hébergement XAML
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, C++, Win32, xaml islands, contrôles personnalisés, contrôles utilisateur, contrôles hôtes
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226313"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Héberger un contrôle UWP personnalisé dans une application Win32 C++

Cet article explique comment utiliser l’[API d’hébergement XAML UWP](using-the-xaml-hosting-api.md) pour héberger un contrôle XAML UWP personnalisé dans une nouvelle application Win32 C++. Si vous disposez déjà d’un projet d’application Win32 C++, vous pouvez adapter ces étapes et exemples de code à votre projet.

Pour héberger un contrôle XAML UWP personnalisé, vous allez créer les projets et composants suivants dans le cadre de cette procédure pas à pas :

* **Projet d’application de bureau Windows**. Ce projet implémente une application de bureau Win32 C++ native. Vous allez ajouter du code à ce projet qui utilise l’API d’hébergement XAML UWP pour héberger un contrôle XAML UWP personnalisé.

* **Projet d’application UWP (C++/WinRT)** . Ce projet implémente un contrôle XAML UWP personnalisé. Il implémente également un fournisseur de métadonnées racine pour le chargement de métadonnées pour les types XAML UWP personnalisés dans le projet.

## <a name="requirements"></a>Conditions requises

* Visual Studio 2019 version 16.4.3 ou ultérieure.
* Windows 10, SDK version 1903 (version 10.0.18362) ou ultérieure.
* [Extension Visual Studio C++/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) installée avec Visual Studio. C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d’en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Pour plus d’informations, voir [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Créer un projet d'application de bureau

1. Dans Visual Studio, créez un projet **Application de bureau Windows** nommé **MyDesktopWin32App**. Ce modèle de projet est disponible sous les filtres de projet **C++** , **Windows** et **Desktop**.

2. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, cliquez sur **Recibler la solution**, sélectionnez la version **10.0.18362.0** ou une version ultérieure du kit de développement logiciel (SDK), puis cliquez sur **OK**.

3. Installez le package NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour activer la prise en charge de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) dans votre projet :

    1. Cliquez avec le bouton droit sur le projet **MyDesktopWin32App** dans **Explorateur de solutions**, puis choisissez **Gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir**, recherchez le package [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), installez la version la plus récente de ce package.

4. Dans la fenêtre **Gérer les packages NuGet**, installez les packages NuGet supplémentaires suivants :

    * [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (version 6.0.0 ou ultérieure). Ce package fournit plusieurs ressources de génération et d’exécution qui permettent à XAML Islands de fonctionner dans votre application.
    * [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version 6.0.0 ou ultérieure).
    * [Microsoft.VCRTForwarders.140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Générez la solution et vérifiez qu’elle est correctement générée.

## <a name="create-a-uwp-app-project"></a>Créer un projet d’application UWP

Ajoutez ensuite un projet d’application **UWP (C++/WinRT)** à votre solution et apportez des modifications de configuration à ce projet. Plus loin dans cette procédure pas à pas, vous ajouterez du code à ce projet pour implémenter un contrôle XAML UWP personnalisé et définir une instance de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le kit de ressources Communauté Windows. Votre application utilisera cette classe comme fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés.

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Ajouter** -> **Nouveau projet**.

2. Ajoutez un projet **Blank App (C++/WinRT)** à votre solution. Nommez le projet **MyUWPApp** et assurez-vous que la version cible et la version minimale sont toutes les deux définies sur **Windows 10, version 1903** ou ultérieure.

3. Installez le package NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) dans le projet **MyUWPApp** :

    1. Cliquez avec le bouton droit sur le projet **MyUWPApp** et choisissez **Gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir**, recherchez le package [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication), puis installez la version 6.0.0 ou une version ultérieure de ce package.

4. Cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **Propriétés**. Sur la page **Propriétés communes** -> **C++/WinRT**, définissez les propriétés suivantes, puis cliquez sur **Appliquer**.

    * Définissez **Verbosité** sur **normal**.
    * Définissez **Optimisé** sur **Non**.

    Lorsque vous avez terminé, la page Propriétés devrait ressembler à ceci.

    ![Propriétés du projet C++/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. Sur la page **Propriétés de configuration** -> **Général** de la fenêtre des propriétés, définissez **Type de configuration** sur **Bibliothèque dynamique (.dll)** , puis cliquez sur **OK** pour fermer la fenêtre des propriétés.

    ![Propriétés générales du projet](images/xaml-islands/xaml-island-cpp-2.png)

6. Ajoutez un fichier exécutable d’espace réservé au projet **MyUWPApp**. Ce fichier exécutable d’espace réservé est requis pour permettre à Visual Studio de créer les fichiers projet nécessaires et de générer correctement le projet.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de projet **MyUWPApp**, puis sélectionnez **Ajouter** -> **Nouvel élément**.
    2. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Utilitaire** dans la page gauche, puis choisissez **Fichier texte (.txt)** . Entrez le nom **placeholder.exe**, puis cliquez sur **Ajouter**.
      ![Ajouter un fichier texte](images/xaml-islands/xaml-island-cpp-3.png)
    3. Dans **Explorateur de solutions**, sélectionnez le fichier **placeholder.exe**. Dans la fenêtre **Propriétés**, assurez-vous que la propriété **Contenu** est définie sur **Vrai**.
    4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **Package.appxmanifest** dans le projet **MyUWPApp**, sélectionnez **Ouvrir avec**, choisissez **Éditeur XML (Texte)** , puis cliquez sur **OK**.
    5. Recherchez l’élément **&lt;Application&gt;** et remplacez l’attribut **Exécutable** par la valeur `placeholder.exe`. Lorsque vous avez terminé, l'élément **&lt;Application&gt;** devrait ressembler à ceci.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. Enregistrez puis fermez le fichier **Package.appxmanifest**.

7. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **MyUWPApp**, puis sélectionnez **Décharger le projet**.
8. Cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **Modifier MyUWPApp.vcxproj**.
9. Recherchez l’élément `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` et remplacez-le par le code XML suivant. Ce code XML ajoute plusieurs nouvelles propriétés immédiatement avant l’élément.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Enregistrez et fermez le fichier projet.
11. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **MyUWPApp**, puis sélectionnez **Recharger le projet**.

## <a name="configure-the-solution"></a>Configurer la solution

Dans cette section, vous allez mettre à jour la solution qui contient les deux projets pour configurer les dépendances du projet et les propriétés de génération requises afin que les projets soient correctement créés.

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis ajoutez un nouveau fichier XML nommé **Solution.props**.
2. Ajoutez le code XML suivant au fichier **Solution.props**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. Dans le menu **Affichage**, cliquez sur **Gestionnaire de propriétés** (selon votre configuration, cette option peut apparaître sous **Affichage** -> **Autres fenêtres**).
4. Dans la fenêtre **Gestionnaire de propriétés**, cliquez avec le bouton droit sur **MyDesktopWin32App**, puis sélectionnez **Ajouter une feuille de propriétés existante**. Accédez au fichier **Solution.props** que vous venez d’ajouter, puis cliquez sur **Ouvrir**.
5. Répétez l’étape précédente pour ajouter le fichier **Solution.props** au projet **MyUWPApp** dans la fenêtre **Gestionnaire de propriétés**.
6. Fermez la fenêtre **Gestionnaire de propriétés**.
7. Vérifiez que les modifications apportées à la feuille de propriétés ont été correctement enregistrées. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyDesktopWin32App**, puis choisissez **Propriétés**. Cliquez sur **Propriétés de configuration** -> **Général**, puis confirmez que les propriétés **Répertoire de sortie** et **Répertoire intermédiaire** affichent les valeurs que vous avez ajoutées au fichier **Solution.props**. Vous pouvez également confirmer les mêmes propriétés pour le projet **MyUWPApp**.
    ![Propriétés du projet](images/xaml-islands/xaml-island-cpp-4.png)

8. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis choisissez **Dépendances du projet**. Dans la liste déroulante **Projets**, vérifiez que l’option **MyDesktopWin32App** est sélectionnée, puis choisissez **MyUWPApp** dans la liste **Dépend de**.
    ![Dépendances du projet](images/xaml-islands/xaml-island-cpp-5.png)

9. Cliquez sur **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Ajouter du code au projet d’application UWP

Vous pouvez maintenant ajouter du code au projet **MyUWPApp** afin d’effectuer les tâches suivantes :

* Implémenter un contrôle XAML UWP personnalisé. Plus loin dans cette procédure pas à pas, vous ajouterez le code qui héberge ce contrôle dans le projet **MyDesktopWin32App**.
* Définir un type dérivé de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) dans le Kit de ressources Communauté Windows. Cette classe servira de fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés.

### <a name="define-a-custom-uwp-xaml-control"></a>Définir un contrôle XAML UWP personnalisé

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **MyUWPApp**, puis sélectionnez **Ajouter** -> **Nouvel élément**. Sélectionnez le nœud **Visual C++** dans le volet gauche, choisissez **Contrôle utilisateur vide (C++/WinRT)** , nommez-le **MyUserControl**, puis cliquez sur **Ajouter**.
2. Dans l’éditeur XAML, remplacez le contenu du fichier **MyUserControl.xaml** par le code XAML suivant, puis enregistrez le fichier.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>Définir une classe XamlApplication

Modifiez ensuite la classe **App** par défaut du projet **MyUWPApp** afin de la dériver de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le Kit de ressources Communauté Windows. Plus loin dans cette procédure pas à pas, vous allez mettre à jour le projet de bureau pour créer une instance de cette classe en tant que fournisseur de métadonnées racine afin de charger les métadonnées des types XAML UWP personnalisés.

  > [!NOTE]
  > Chaque solution qui utilise XAML Islands ne peut contenir qu’un seul projet définissant un objet `XamlApplication`. Tous les contrôles XAML UWP personnalisés de votre application partagent le même objet `XamlApplication`. 

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **MainPage.xaml** dans le projet **MyUWPApp**. Cliquez sur **Retirer** puis **Supprimer** pour supprimer définitivement ce fichier du projet.
2. Dans le projet **MyUWPApp**, développez le fichier **App.xaml**.
3. Remplacez le contenu des fichiers **App.xaml**, **App.cpp**, **App.h** et **App.idl** par le code suivant.

    * **App.xaml** :

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App.idl** :

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.h** :

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.cpp** :

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. Ajoutez un nouveau fichier d’en-tête au projet **MyUWPApp** nommé **app.base.h**.
5. Ajoutez le code suivant au fichier **app.base.h**, enregistrez le fichier, puis fermez-le.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. Générez la solution et vérifiez qu’elle est correctement générée.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Configurer le projet de bureau pour utiliser des types de contrôles personnalisés

Pour que l’application **MyDesktopWin32App** puisse héberger un contrôle XAML UWP personnalisé dans un îlot XAML, l’application doit être configurée pour utiliser des types de contrôles personnalisés provenant du projet **MyUWPApp**. Il existe deux façons de procéder, et vous pouvez choisir l’une ou l’autre de ces deux options à mesure que vous effectuez cette procédure pas à pas.

### <a name="option-1-package-the-app-using-msix"></a>Option 1 : Empaqueter l’application à l’aide de MSIX

Vous pouvez empaqueter l’application dans un package [MSIX](https://docs.microsoft.com/windows/msix) en vue du déploiement. MSIX est une technologie d’empaquetage moderne pour Windows, basée sur une combinaison des technologies d’installation MSI, .appx, App-V et ClickOnce.

1. Ajoutez un nouveau [projet d’empaquetage d’application Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. À mesure que vous créez le projet, nommez-le **MyDesktopWin32Project** et sélectionnez **Windows 10, version 1903 (10.0 ; Build 18362)** à la fois pour la **version cible** et la **version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **Applications**, puis choisissez **Ajouter une référence**. Dans la liste des projets, activez la case à cocher en regard du projet **MyDesktopWin32App**, puis cliquez sur **OK**.
    ![Projet de référence](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour la déployer, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

### <a name="option-2-create-an-application-manifest"></a>Option 2 : Créer un manifeste d'application

Vous pouvez ajouter un [manifeste d’application](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre application.

1. Cliquez avec le bouton droit sur le projet **MyDesktopWin32App**, puis sélectionnez **Ajouter** -> **Nouvel élément**. 
2. Dans la boîte de dialogue **Ajouter un nouvel élément**, cliquez sur **Web** dans le volet gauche, puis sélectionnez **Fichier XML (.xml)** . 
3. Nommez le nouveau fichier **app.manifest**, puis cliquez sur **Ajouter**.
4. Remplacez le contenu du nouveau fichier par le code XML ci-dessous. Ce code XML inscrit les types de contrôles personnalisés dans le projet **MyUWPApp**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>Configurer d’autres propriétés de projet de bureau

Mettez à jour le projet **MyDesktopWin32App** afin de définir une macro pour des répertoires Include supplémentaires et de configurer d’autres propriétés.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyDesktopWin32App**, puis choisissez **Décharger le projet**.

2. Cliquez avec le bouton droit sur **MyDesktopWin32App (déchargé)** , puis sélectionnez **Modifier MyDesktopWin32App.vcxproj**.

3. Ajoutez le code XML suivant juste avant la balise de fermeture `</Project>` à la fin du fichier. Enregistrez et fermez le fichier.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **MyDesktopWin32App (déchargé)** , puis sélectionnez **Recharger le projet**.

5. Cliquez avec le bouton droit sur **MyDesktopWin32App**, sélectionnez **Propriétés**, puis cliquez sur le nœud **C/C++** dans le volet gauche. Vérifiez que la macro **Autres répertoires Include** a été définie à l’aide de la modification du fichier projet apportée à l’étape précédente.
    ![Paramètres de projet C/C++](images/xaml-islands/xaml-island-cpp-7.png)

6. Dans la boîte de dialogue **Pages de propriétés**, développez **Outil Manifeste** -> **Entrée et sortie**. Définissez la propriété **Prise en charge DPI** sur **Reconnaissant les résolutions élevées par moniteur**. Si vous ne définissez pas cette propriété, vous risquez de rencontrer une erreur de configuration de manifeste dans certains scénarios impliquant des résolutions élevées.
    ![Paramètres de projet C/C++](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Héberger le contrôle XAML UWP personnalisé dans le projet de bureau

Vous êtes enfin prêt à ajouter du code au projet **MyDesktopWin32App** pour héberger le contrôle XAML UWP personnalisé que vous avez défini précédemment dans le projet **MyUWPApp**.

1. Dans le projet **MyDesktopWin32App**, ouvrez le fichier **framework.h** et commentez la ligne de code suivante. Enregistrez le fichier une fois que vous avez terminé.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Ouvrez le fichier **MyDesktopWin32App.h** et remplacez le contenu de ce fichier par le code suivant pour référencer les fichiers d’en-tête C++/WinRT requis. Enregistrez le fichier une fois que vous avez terminé.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. Ouvrez le fichier **MyDesktopWin32App.cpp** et ajoutez le code suivant à la section `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. Dans le même fichier, ajoutez le code suivant à la section `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. Dans le même fichier, ajoutez le code suivant immédiatement après le commentaire `TODO: Place code here.` dans la fonction `wWinMain`.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. Dans le même fichier, remplacez la fonction `InitInstance` par défaut par le code suivant.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. Dans le même fichier, ajoutez la nouvelle fonction suivante à la fin du fichier.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. Dans le même fichier, recherchez la fonction `WndProc`. Remplacez le gestionnaire `WM_DESTROY` par défaut dans l’instruction switch par le code suivant.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. Enregistrez le fichier.
10. Générez la solution et vérifiez qu’elle est correctement générée.

## <a name="test-the-app"></a>Tester l'application

Exécutez la solution et vérifiez que **MyDesktopWin32App** s’ouvre dans la fenêtre suivante.

![Application MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Étapes suivantes

De nombreuses applications de bureau qui hébergent XAML Islands doivent gérer des scénarios supplémentaires pour offrir une expérience utilisateur fluide. Par exemple, des applications de bureau peuvent nécessiter la gestion de la saisie au clavier dans XAML Islands, la navigation en mode focus entre XAML Islands et d’autres éléments d’interface utilisateur, et des modifications de disposition.

Pour plus d’informations sur la gestion de ces scénarios et les pointeurs des exemples de code connexes, consultez [Scénarios avancés pour XAML Islands dans les applications Win32 C++](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans des applications de bureau (XAML Islands)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++](using-the-xaml-hosting-api.md)
* [Héberger un contrôle UWP standard dans une application Win32 C++](host-standard-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour XAML Islands dans les applications Win32 C++](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples)
