---
title: Utilisation de la couche visuelle avec WPF
description: Découvrez les techniques d’utilisation des API de la couche visuelle avec le contenu WPF existant pour créer des effets et des animations avancés.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215154"
---
# <a name="using-the-visual-layer-with-wpf"></a>Utilisation de la couche visuelle avec WPF

Vous pouvez utiliser des API de composition Windows Runtime (autrement dit, la [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications WPF (Windows Presentation Foundation) pour créer des expériences modernes dédiées aux utilisateurs de Windows 10.

Le code complet de ce tutoriel est disponible sur GitHub : [Exemple WPF HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Prérequis

L’API d’hébergement XAML UWP a les prérequis suivants.

- Nous partons du principe que vous êtes familiarisé avec le développement d’applications à l’aide de WPF et d’UWP. Pour plus d’informations, voir :
  - [Bien démarrer (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Bien démarrer avec les applications Windows 10](/windows/uwp/get-started/)
  - [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 ou version ultérieure
- Windows 10 version 1803 ou ultérieure
- SDK Windows 10 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-in-wpf"></a>Comment utiliser les API de composition dans WPF

Dans ce tutoriel, vous allez créer une interface utilisateur d’application WPF et y ajouter des éléments de composition animés. Les composants WPF et de composition sont simples, mais le code interop indiqué est le même, quelle que soit la complexité des composants. L’application terminée ressemble à ceci.

![Interface utilisateur de l’application exécutée](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Créer un projet WPF

La première étape consiste à créer le projet d’application WPF, qui comprend une définition d’application et la page XAML pour l’interface utilisateur.

Pour créer un projet d’application WPF en Visual C# nommé _HelloComposition_ :

1. Ouvrez Visual Studio, puis sélectionnez **Fichier** > **Nouveau** > **Projet**.

    La boîte de dialogue **Nouveau projet** s’affiche.
1. Dans la catégorie **Installé**, développez le nœud **Visual C#** , puis sélectionnez **Bureau Windows**.
1. Sélectionnez le modèle **Application WPF (.NET Framework)** .
1. Entrez le nom _HelloComposition,_ sélectionnez Framework **.NET Framework 4.7.2**, puis cliquez sur **OK**.

    Visual Studio crée le projet et ouvre le concepteur pour la fenêtre d’application par défaut nommée MainWindow.xaml.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour qu’il utilise des API Windows Runtime

Pour utiliser des API Windows Runtime (WinRT) dans votre application WPF, vous devez configurer votre projet Visual Studio pour qu’il accède à Windows Runtime. De plus, comme les API de composition utilisent quantité de vecteurs, vous devez ajouter les références requises pour utiliser les vecteurs.

Des packages NuGet sont disponibles pour répondre à ces deux besoins. Installez les dernières versions de ces packages pour ajouter les références nécessaires à votre projet.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (le format de gestion de package par défaut doit être défini sur PackageReference)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Nous vous recommandons d’utiliser les packages NuGet pour configurer votre projet, mais il est possible d’ajouter manuellement les références requises. Pour plus d’informations, consultez [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). Le tableau ci-dessous liste les fichiers auxquels vous devez ajouter des références.

|File|Emplacement|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Configurer le projet pour la prise en charge DPI par moniteur

Le contenu de la couche visuelle que vous ajoutez à votre application n’est pas automatiquement mis à l’échelle selon les paramètres DPI de l’écran sur lequel il est affiché. Vous devez activer la prise en charge DPI par moniteur dans votre application, puis vous assurer que le code utilisé pour créer le contenu de la couche visuelle prend en compte la mise à l’échelle DPI actuelle quand l’application est exécutée. Ici, nous configurons le projet pour la prise en charge DPI. Dans des sections ultérieures, nous montrerons comment utiliser les paramètres DPI pour mettre à l’échelle le contenu de la couche visuelle.

Par défaut, les applications WPF permettent la prise en charge DPI système. Elles doivent cependant déclarer explicitement leur prise en charge DPI par moniteur dans un fichier app.manifest. Pour activer la prise en charge DPI par moniteur de niveau Windows dans le fichier manifeste de l’application :

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_.
1. Dans le menu contextuel, sélectionnez **Ajouter** > **Nouvel élément...** .
1. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez « Fichier manifeste de l’application », puis cliquez sur **Ajouter**. (Vous pouvez laisser le nom par défaut.)
1. Dans le fichier app.manifest, recherchez ce code XML et décommentez-le :

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Ajoutez ce paramètre après la balise `<windowsSettings>` ouvrante :

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. Vous devez également définir le paramètre **DoNotScaleForDpiChanges** dans le fichier App.config.

    Ouvrez App.config et ajoutez ce code XML à l’intérieur de l’élément `<configuration>` :

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** ne peut être défini qu’une seule fois. Si votre application en a déjà un défini, vous devez délimiter ce commutateur par un point-virgule à l’intérieur de l’attribut value.

(Pour plus d’informations, consultez le [Guide du développeur et les exemples de DPI par moniteur](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) sur GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Créer une classe dérivée HwndHost pour y héberger les éléments de composition

Pour héberger le contenu que vous créez avec la couche visuelle, vous devez créer une classe dérivée de [HwndHost](/dotnet/api/system.windows.interop.hwndhost). C’est là que vous effectuez la majeure partie de la configuration pour l’hébergement des API de composition. Dans cette classe, vous utilisez [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) et [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) pour intégrer des API de composition dans votre application WPF. Pour plus d’informations sur PInvoke et COM Interop, consultez [Interopération avec du code non managé](/dotnet/framework/interop/index).

> [!TIP]
> Si nécessaire, reportez-vous au code complet fourni à la fin du tutoriel pour vous assurer que tout le code se trouve à la bonne place à mesure que vous avancez dans le tutoriel.

1. Ajoutez à votre projet le fichier de la nouvelle classe qui dérive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_.
    - Dans le menu contextuel, sélectionnez **Ajouter** > **Classe...** .
    - Dans la boîte de dialogue **Ajouter un nouvel élément**, nommez la classe _CompositionHost.cs_, puis cliquez sur **Ajouter**.
1. Dans CompositionHost.cs, modifiez la définition de classe pour qu’elle dérive de **HwndHost**.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. Ajoutez le code et le constructeur suivants à la classe.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. Substituez les méthodes **BuildWindowCore** et **DestroyWindowCore**.
    > [!NOTE]
    > Dans BuildWindowCore, vous appelez les méthodes _InitializeCoreDispatcher_ et _InitComposition_. Vous créerez ces méthodes aux étapes suivantes.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) et [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) nécessitent une déclaration PInvoke. Placez cette déclaration à la fin du code pour la classe.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. Initialisez un thread avec un [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). Le répartiteur principal est chargé de traiter les messages de fenêtre et de répartir les événements pour les API WinRT. Les nouvelles instances de **CoreDispatcher** doivent être créées sur un thread avec CoreDispatcher.
    - Créez une méthode nommée _InitializeCoreDispatcher_ et ajoutez du code pour configurer la file d’attente du répartiteur.

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - La file d’attente du répartiteur nécessite également une déclaration PInvoke. Placez cette déclaration à l’intérieur de la région _PInvoke declarations_ que vous avez créée à l’étape précédente.

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    La file d’attente du répartiteur est maintenant prête et peut commencer à initialiser et à créer du contenu de composition.

1. Initialisez la classe [Compositor](/uwp/api/windows.ui.composition.compositor). Compositor est une fabrique qui crée divers types dans l’espace de noms [Windows.UI.Composition](/uwp/api/windows.ui.composition) couvrant les visuels, le système d’effets et le système d’animation. La classe Compositor gère aussi la durée de vie des objets créés à partir de la fabrique.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** et **ICompositionTarget** requièrent les importations COM. Placez ce code après la classe _CompositionHost_, mais à l’intérieur de la déclaration d’espace de noms.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Créer un contrôle utilisateur pour ajouter votre contenu à l’arborescence de visuels WPF

La dernière étape pour configurer l’infrastructure qui hébergera le contenu de composition consiste à ajouter HwndHost à l’arborescence de visuels WPF.

### <a name="create-a-usercontrol"></a>Créer un contrôle utilisateur

Un contrôle utilisateur (UserControl) est un moyen pratique de créer un package de votre code qui crée et gère le contenu de composition, et d’ajouter facilement le contenu à votre XAML.

1. Ajoutez un nouveau fichier de contrôle utilisateur à votre projet.
    - Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_.
    - Dans le menu contextuel, sélectionnez **Ajouter** > **Contrôle utilisateur...** .
    - Dans la boîte de dialogue **Ajouter un nouvel élément**, nommez le contrôle utilisateur _CompositionHostControl.xaml_, puis cliquez sur **Ajouter**.

    Les deux fichiers CompositionHostControl.xaml et CompositionHostControl.xaml.cs sont créés et ajoutés à votre projet.
1. Dans CompositionHostControl.xaml, remplacez les balises `<Grid> </Grid>` par cet élément **Border**, qui est le conteneur XAML où sera stocké votre HwndHost.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

Dans le code du contrôle utilisateur, créez une instance de la classe CompositionHost que vous avez créée à l’étape précédente, puis ajoutez-la en tant qu’élément enfant de _CompositionHostElement_, l’élément Border que vous avez créé dans la page XAML.

1. Dans CompositionHostControl.xaml.cs, ajoutez des variables private pour les objets que vous utiliserez dans votre code de composition. Ajoutez ces variables après la définition de classe.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Ajoutez un gestionnaire pour l’événement **Loaded** du contrôle utilisateur. C’est là que vous configurez votre instance CompositionHost.

    - Dans le constructeur, raccordez le gestionnaire d’événement comme indiqué ici (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Ajoutez la méthode du gestionnaire d’événement appelée *CompositionHostControl_Loaded*.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    Dans cette méthode, vous configurez les objets que vous utiliserez dans votre code de composition. Voici un aperçu rapide du processus.

    - Tout d’abord, assurez-vous que la configuration n’a pas déjà été effectuée, en recherchant d’éventuelles instances de CompositionHost existantes.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Obtenez le paramètre DPI actuel. Ce paramètre est utilisé pour mettre à l’échelle vos éléments de composition de manière appropriée.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Créez une instance de CompositionHost et affectez-la en tant qu’enfant de l’élément Border appelé _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Obtenez l’élément Compositor à partir du CompositionHost.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Utilisez l’élément Compositor pour créer un visuel de conteneur. Il s’agit du conteneur de composition auquel vous ajoutez vos éléments de composition.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Une fois l’infrastructure en place, vous pouvez générer le contenu de composition que vous souhaitez montrer.

Pour cet exemple, vous ajoutez du code qui crée et anime un carré simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Ajoutez un élément de composition. Dans CompositionHostControl.xaml.cs, ajoutez ces méthodes à la classe CompositionHostControl.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>Gérer les modifications de DPI

Le code qui ajoute et anime un élément prend en compte l’échelle DPI actuelle quand des éléments sont créés, mais vous devez également tenir compte des modifications de DPI durant l’exécution de l’application. Vous pouvez gérer l’événement [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) pour être averti des modifications et adapter vos calculs en fonction de la nouvelle DPI.

1. Dans la méthode CompositionHostControl_Loaded, après la dernière ligne, ajoutez ce code pour raccorder le gestionnaire de l’événement DpiChanged.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Ajoutez la méthode du gestionnaire d’événement appelée _CompositionHostDpiChanged_. Ce code adapte l’échelle et le décalage de chaque élément, puis recalcule toutes les animations qui ne sont pas terminées.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>Ajouter le contrôle utilisateur à votre page XAML

À présent, vous pouvez ajouter le contrôle utilisateur à votre interface utilisateur XAML.

1. Dans MainWindow.xaml, définissez la hauteur et la largeur de la fenêtre sur 600 et 840, respectivement.
1. Ajoutez le code XAML pour l’interface utilisateur. Dans MainWindow.xaml, ajoutez ce code XAML entre les balises `<Grid> </Grid>` à la racine.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. Gérez l’événement Button_Click pour créer des éléments. (L’événement Click est déjà raccordé dans le code XAML.)

    Dans MainWindow.xaml.cs, ajoutez cette méthode du gestionnaire d’événement *Button_Click*. Ce code appelle _CompositionHost.AddElement_ pour créer un élément avec une taille et un décalage générés de manière aléatoire.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Vous pouvez maintenant générer et exécuter votre application WPF. Si nécessaire, reportez-vous au code complet fourni à la fin du tutoriel pour vous assurer que tout le code se trouve à la bonne place.

Quand vous exécutez l’application et cliquez sur le bouton, vous devez voir les carrés animés ajoutés à l’interface utilisateur.

## <a name="next-steps"></a>Étapes suivantes

Pour voir un exemple plus complet basé sur la même infrastructure, consultez l’[exemple d’intégration de couche visuelle WPF](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) sur GitHub.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Bien démarrer (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interopération avec du code non managé](/dotnet/framework/interop/) (.NET)
- [Bien démarrer avec les applications Windows 10](/windows/uwp/get-started/) (UWP)
- [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espace de noms Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Code complet

Vous trouverez ci-dessous le code complet de ce tutoriel.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```