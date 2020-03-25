---
description: Cet article explique comment héberger un contrôle UWP personnalisé dans une C++ application Win32 à l’aide de l’API d’hébergement XAML.
title: Héberger un contrôle UWP personnalisé dans C++ une application Win32 à l’aide de l’API d’hébergement XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, îlots XAML, contrôles personnalisés, contrôles utilisateur, contrôles hôtes
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226313"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Héberger un contrôle UWP personnalisé dans C++ une application Win32

Cet article montre comment utiliser l' [API d’hébergement XAML UWP](using-the-xaml-hosting-api.md) pour héberger un contrôle XAML UWP personnalisé dans une C++ nouvelle application Win32. Si vous disposez déjà C++ d’un projet d’application Win32, vous pouvez adapter ces étapes et exemples de code pour votre projet.

Pour héberger un contrôle XAML UWP personnalisé, vous allez créer les projets et les composants suivants dans le cadre de cette procédure pas à pas :

* **Projet d’application de bureau Windows**. Ce projet implémente une application C++ de bureau Win32 native. Vous allez ajouter du code à ce projet qui utilise l’API d’hébergement XAML UWP pour héberger un contrôle XAML UWP personnalisé.

* **Projet d’application UWPC++(/WinRT)** . Ce projet implémente un contrôle XAML UWP personnalisé. Il implémente également un fournisseur de métadonnées racine pour le chargement des métadonnées pour les types XAML UWP personnalisés dans le projet.

## <a name="requirements"></a>Configuration requise

* Visual Studio 2019 version 16.4.3 ou ultérieure.
* Windows 10, version 1903 SDK (version 10.0.18362) ou version ultérieure.
* [/WinRT extension Visual Studio (VSIX) est installé avec Visual Studio. C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Pour plus d’informations, consultez [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Créer un projet d’application de bureau

1. Dans Visual Studio, créez un projet d' **application de bureau Windows** nommé **MyDesktopWin32App**. Ce modèle de projet est disponible sous **C++** les filtres de projet, **Windows**et de **Bureau** .

2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, cliquez sur **recibler la solution**, sélectionnez le **10.0.18362.0** ou une version ultérieure du kit de développement logiciel (SDK), puis cliquez sur **OK**.

3. Installez le package NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) pour activer la prise en charge de [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) dans votre projet :

    1. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur le projet **MyDesktopWin32App** , puis choisissez **gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) et installez la dernière version de ce package.

4. Dans la fenêtre **gérer les packages NuGet** , installez les packages NuGet supplémentaires suivants :

    * [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (version v 6.0.0 ou ultérieure). Ce package fournit plusieurs ressources de génération et d’exécution qui permettent aux îlots XAML de fonctionner dans votre application.
    * [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v 6.0.0 ou ultérieure).
    * [Microsoft. VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Générez la solution et vérifiez qu’elle est correctement générée.

## <a name="create-a-uwp-app-project"></a>Créer un projet d’application UWP

Ensuite, ajoutez un projet d’application **UWP (C++/WinRT)** à votre solution et apportez des modifications de configuration à ce projet. Plus loin dans cette procédure pas à pas, vous ajouterez du code à ce projet pour implémenter un contrôle XAML UWP personnalisé et définir une instance de la classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le kit de connaissances de la communauté Windows. Votre application utilisera cette classe comme fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et sélectionnez **Ajouter** -> **nouveau projet**.

2. Ajoutez un projet **application videC++(/WinRT)** à votre solution. Nommez le projet **MyUWPApp** et assurez-vous que la version cible et la version minimale sont toutes deux définies sur **Windows 10, version 1903** ou ultérieure.

3. Installez le package NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) dans le projet **MyUWPApp** :

    1. Cliquez avec le bouton droit sur le projet **MyUWPApp** , puis choisissez **gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) , puis installez v 6.0.0 ou une version ultérieure de ce package.

4. Cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **Propriétés**. Dans la page **Propriétés communes** ->  **C++/WinRT** , définissez les propriétés suivantes, puis cliquez sur **appliquer**.

    * Affectez à **verbosity** la valeur **normal**.
    * Définissez l' **optimisation** sur **non**.

    Lorsque vous avez terminé, la page Propriétés doit ressembler à ceci.

    ![C++Propriétés du projet/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. Dans la **page Propriétés de configuration** -> **général** de la fenêtre Propriétés, définissez **type de configuration** sur **bibliothèque dynamique (. dll)** , puis cliquez sur **OK** pour fermer la fenêtre Propriétés.

    ![Propriétés générales du projet](images/xaml-islands/xaml-island-cpp-2.png)

6. Ajoutez un fichier exécutable d’espace réservé au projet **MyUWPApp** . Ce fichier exécutable d’espace réservé est requis pour que Visual Studio génère les fichiers projet requis et génère correctement le projet.

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de projet **MyUWPApp** et sélectionnez **Ajouter** -> **nouvel élément**.
    2. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **utilitaire** dans la page de gauche, puis sélectionnez **fichier texte (. txt)** . Entrez le nom de l' **espace réservé. exe** , puis cliquez sur **Ajouter**.
      ![ajouter un fichier texte](images/xaml-islands/xaml-island-cpp-3.png)
    3. Dans **Explorateur de solutions**, sélectionnez le fichier **. exe de l’espace réservé** . Dans la fenêtre **Propriétés** , assurez-vous que la propriété **contenu** a la valeur **true**.
    4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **Package. appxmanifest** dans le projet **MyUWPApp** , sélectionnez **Ouvrir avec**, puis sélectionnez **éditeur XML (texte)** , puis cliquez sur **OK**.
    5. Recherchez l’élément **&lt;Application&gt;** et remplacez l’attribut **exécutable** par la valeur `placeholder.exe`. Lorsque vous avez terminé, l’élément de&gt;de l' **Application&lt;** doit ressembler à ceci.

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

    6. Enregistrez et fermez le fichier **Package. appxmanifest** .

7. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **décharger le projet**.
8. Cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **modifier MyUWPApp. vcxproj**.
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
11. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **MyUWPApp** et sélectionnez **recharger le projet**.

## <a name="configure-the-solution"></a>Configurer la solution

Dans cette section, vous allez mettre à jour la solution qui contient les deux projets pour configurer les dépendances du projet et les propriétés de génération requises pour que les projets soient générés correctement.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et ajoutez un nouveau fichier XML nommé **solution. props**.
2. Ajoutez le code XML suivant au fichier **solution. props** .

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

3. Dans le menu **affichage** , cliquez sur **Gestionnaire de propriétés** (en fonction de votre configuration, elle peut être sous **affichage** -> **autres fenêtres**).
4. Dans la fenêtre **Gestionnaire de propriétés** , cliquez avec le bouton droit sur **MyDesktopWin32App** et sélectionnez Ajouter une **feuille de propriétés existante**. Accédez au fichier **solution. props** que vous venez d’ajouter, puis cliquez sur **ouvrir**.
5. Répétez l’étape précédente pour ajouter le fichier **solution. props** au projet **MyUWPApp** dans la fenêtre **Gestionnaire de propriétés** .
6. Fermez la fenêtre de **Gestionnaire de propriétés** .
7. Vérifiez que les modifications apportées à la feuille de propriétés ont été enregistrées correctement. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyDesktopWin32App** et choisissez **Propriétés**. Cliquez **sur Propriétés de Configuration** -> **Genneral**et vérifiez que les propriétés **Répertoire de sortie** et **Répertoire intermédiaire** contiennent les valeurs que vous avez ajoutées au fichier **solution. props** . Vous pouvez également vérifier les mêmes pour le projet **MyUWPApp** .
    ![les propriétés du projet](images/xaml-islands/xaml-island-cpp-4.png)

8. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution et choisissez **dépendances du projet**. Dans la liste déroulante **projets** , assurez-vous que **MyDesktopWin32App** est sélectionné, puis sélectionnez **MyUWPApp** dans la liste **dépend de** .
    ![les dépendances du projet](images/xaml-islands/xaml-island-cpp-5.png)

9. Cliquez sur **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Ajouter du code au projet d’application UWP

Vous êtes maintenant prêt à ajouter du code au projet **MyUWPApp** pour effectuer les tâches suivantes :

* Implémentez un contrôle XAML UWP personnalisé. Plus loin dans cette procédure pas à pas, vous ajouterez du code qui héberge ce contrôle dans le projet **MyDesktopWin32App** .
* Définissez un type qui dérive de la classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) dans la boîte à outils de la communauté Windows. Cette classe joue le rôle de fournisseur de métadonnées racine pour le chargement de métadonnées pour les types XAML UWP personnalisés.

### <a name="define-a-custom-uwp-xaml-control"></a>Définir un contrôle XAML UWP personnalisé

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **MyUWPApp** et sélectionnez **Ajouter** -> **nouvel élément**. Sélectionnez le **nœud C++ visuel** dans le volet gauche, sélectionnez **contrôle utilisateur vide (C++/WinRT)** , nommez-le **MyUserControl**, puis cliquez sur **Ajouter**.
2. Dans l’éditeur XAML, remplacez le contenu du fichier **MyUserControl. Xaml** par le code XAML suivant, puis enregistrez le fichier.

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

Ensuite, révisez la classe d' **application** par défaut dans le projet **MyUWPApp** pour qu’elle dérive de la classe [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le kit de connaissances de la communauté Windows. Plus loin dans cette procédure pas à pas, vous allez mettre à jour le projet de bureau pour créer une instance de cette classe en tant que fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés.

  > [!NOTE]
  > Chaque solution qui utilise des îlots XAML ne peut contenir qu’un seul projet qui définit un objet `XamlApplication`. Tous les contrôles XAML UWP personnalisés de votre application partagent le même objet `XamlApplication`. 

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier **MainPage. Xaml** dans le projet **MyUWPApp** . Cliquez sur **supprimer** , puis sur **supprimer** pour supprimer ce fichier permamently du projet.
2. Dans le projet **MyUWPApp** , développez le fichier **app. Xaml** .
3. Remplacez le contenu des fichiers **app. Xaml**, **app. cpp**, **app. h**et **app. idl** par le code suivant.

    * **App. Xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App. idl**:

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

    * **App. h**:

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

    * **App. cpp**:

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

4. Ajoutez un nouveau fichier d’en-tête au projet **MyUWPApp** nommé **app. base. h**.
5. Ajoutez le code suivant au fichier **app. base. h** , enregistrez le fichier, puis fermez-le.

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

Avant que l’application **MyDesktopWin32App** puisse héberger un contrôle XAML UWP personnalisé dans un îlot XAML, elle doit être configurée pour utiliser des types de contrôles personnalisés à partir du projet **MyUWPApp** . Il existe deux façons de procéder, et vous pouvez choisir l’une des options au fur et à mesure que vous effectuez cette procédure pas à pas.

### <a name="option-1-package-the-app-using-msix"></a>Option 1 : empaqueter l’application à l’aide de MSIX

Vous pouvez empaqueter l’application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement. MSIX est la technologie d’empaquetage d’applications moderne pour Windows, qui est basée sur une combinaison de MSI,. AppX, App-V et des technologies d’installation ClickOnce.

1. Ajoutez un nouveau [projet d’empaquetage d’applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. Au fur et à mesure que vous créez le projet, nommez-le **MyDesktopWin32Project** et sélectionnez **Windows 10, version 1903 (10,0 ; Build 18362)** pour la **version cible** et la **version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **applications** et choisissez **Ajouter une référence**. Dans la liste des projets, activez la case à cocher en regard du projet **MyDesktopWin32App** , puis cliquez sur **OK**.
    ![de référence du projet](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour le déploiement, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

### <a name="option-2-create-an-application-manifest"></a>Option 2 : créer un manifeste d’application

Vous pouvez ajouter un [manifeste d’application](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre application.

1. Cliquez avec le bouton droit sur le projet **MyDesktopWin32App** et sélectionnez **Ajouter** -> **nouvel élément**. 
2. Dans la boîte de dialogue **Ajouter un nouvel élément** , cliquez sur **Web** dans le volet gauche et sélectionnez **fichier XML (. Xml)** . 
3. Nommez le nouveau fichier **app. manifest** , puis cliquez sur **Ajouter**.
4. Remplacez le contenu du nouveau fichier par le code XML suivant. Ce code XML inscrit les types de contrôles personnalisés dans le projet **MyUWPApp** .

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

## <a name="configure-additional-desktop-project-properties"></a>Configurer des propriétés de projet de bureau supplémentaires

Ensuite, mettez à jour le projet **MyDesktopWin32App** pour définir une macro pour des répertoires Include supplémentaires et configurer des propriétés supplémentaires.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MyDesktopWin32App** et sélectionnez **décharger le projet**.

2. Cliquez avec le bouton droit sur **MyDesktopWin32App (déchargé)** et sélectionnez **modifier MyDesktopWin32App. vcxproj**.

3. Ajoutez le code XML suivant juste avant la balise `</Project>` fermante à la fin du fichier. Ensuite, enregistrez et fermez le fichier.

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

4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **MyDesktopWin32App (déchargé)** et sélectionnez **recharger le projet**.

5. Cliquez avec le bouton droit sur **MyDesktopWin32App**, sélectionnez **Propriétés**, puis cliquez sur le nœud **C/C++**  dans le volet gauche. Vérifiez que la macro **autres répertoires Include** a été définie à partir de la modification apportée au fichier projet à l’étape précédente.
    ![les paramètresC++ C/Project](images/xaml-islands/xaml-island-cpp-7.png)

6. Dans la boîte de dialogue **pages de propriétés** , développez **outil manifeste** -> **entrée et sortie**. Définissez la propriété prise en charge des **PPP** sur **par moniteur**. Si vous ne définissez pas cette propriété, vous risquez de rencontrer une erreur de configuration de manifeste dans certains scénarios haute résolution.
    ![les paramètresC++ C/Project](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Héberger le contrôle XAML UWP personnalisé dans le projet de bureau

Enfin, vous êtes prêt à ajouter du code au projet **MyDesktopWin32App** pour héberger le contrôle XAML UWP personnalisé que vous avez défini précédemment dans le projet **MyUWPApp** .

1. Dans le projet **MyDesktopWin32App** , ouvrez le fichier **Framework. h** et commentez la ligne de code suivante. Enregistrez le fichier une fois que vous avez terminé.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Ouvrez le fichier **MyDesktopWin32App. h** et remplacez le contenu de ce fichier par le code suivant pour référencer C++les fichiers d’en-tête/WinRT nécessaires. Enregistrez le fichier une fois que vous avez terminé.

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

3. Ouvrez le fichier **MyDesktopWin32App. cpp** et ajoutez le code suivant à la section `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. Dans le même fichier, ajoutez le code suivant à la section `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. Dans le même fichier, ajoutez le code suivant immédiatement après l' `TODO: Place code here.` commentaire dans la fonction `wWinMain`.

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

8. Dans le même fichier, recherchez la fonction `WndProc`. Remplacez le gestionnaire d' `WM_DESTROY` par défaut dans l’instruction switch par le code suivant.

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

## <a name="test-the-app"></a>Tester l’application

Exécutez la solution et vérifiez que **MyDesktopWin32App** s’ouvre avec la fenêtre suivante.

![Application MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Étapes suivantes :

De nombreuses applications de bureau qui hébergent des îlots XAML doivent gérer des scénarios supplémentaires pour offrir une expérience utilisateur fluide. Par exemple, les applications de bureau peuvent avoir besoin de gérer l’entrée au clavier dans les îlots XAML, de se concentrer sur la navigation entre les îlots XAML et d’autres éléments d’interface utilisateur et les modifications de disposition.

Pour plus d’informations sur la gestion de ces scénarios et des pointeurs vers les exemples de code associés, consultez [scénarios avancés pour les îlots XAML dans C++ les applications Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md)
* [Utilisation de l’API d’hébergement XAML UWP C++ dans une application Win32](using-the-xaml-hosting-api.md)
* [Héberger un contrôle UWP standard dans C++ une application Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Scénarios avancés pour les îlots C++ XAML dans les applications Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Exemples de code des îlots XAML](https://github.com/microsoft/Xaml-Islands-Samples)
