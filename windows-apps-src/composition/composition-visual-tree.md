---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: Visuels de composition
description: Les éléments visuels de composition constituent la structure de l’arborescence des éléments visuels sur laquelle reposent toutes les autres fonctionnalités de l’API Composition. L’API permet aux développeurs de définir et de créer un ou plusieurs objets visuels, qui représentent chacun un nœud unique dans une arborescence d’éléments visuels.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85df48b4f43759013f80623595d919ac6c77337
ms.sourcegitcommit: ef3cdca5e9b8f032f46174da4574cb5593d32d56
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90593433"
---
# <a name="composition-visual"></a>Visuels de composition

Les éléments visuels de composition constituent la structure de l’arborescence des éléments visuels sur laquelle reposent toutes les autres fonctionnalités de l’API Composition. L’API permet aux développeurs de définir et de créer un ou plusieurs objets visuels, qui représentent chacun un nœud unique dans une arborescence d’éléments visuels.

## <a name="visuals"></a>Visuels

Il existe plusieurs types visuels qui composent la structure de l’arborescence visuelle plus une classe Brush de base avec plusieurs sous-classes qui affectent le contenu d’un visuel :

- [**Visuel**](/uwp/api/Windows.UI.Composition.Visual) : objet de base, la majorité des propriétés est ici et hérité par les autres objets visuels.
- [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) : dérive de [**Visual**](/uwp/api/Windows.UI.Composition.Visual), et ajoute la possibilité de créer des enfants.
  - [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : dérive de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual). A la possibilité d’associer un pinceau afin que le visuel puisse restituer des pixels, y compris des images, des effets ou une couleur unie.
  - [**LayerVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : dérive de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual). Les enfants du visuel sont aplatis en une seule couche.<br/>(_Introduit dans Windows 10, version 1607, SDK 14393._)
  - [**ShapeVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : dérive de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual). Nœud d’arborescence d’éléments visuels qui est la racine d’un CompositionShape.<br/>(_Introduit dans Windows 10, version 1803, SDK 17134._)
  - [**RedirectVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : dérive de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual). Le visuel obtient son contenu à partir d’un autre visuel.<br/>(_Introduit dans Windows 10, version 1809, SDK 17763._)
  - [**SceneVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) : dérive de [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual). Un visuel de conteneur pour les nœuds d’une scène 3D.<br/>(_Introduit dans Windows 10, version 1903, SDK 18362._)

Vous pouvez appliquer du contenu et des effets à SpriteVisuals à l’aide de [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) et de ses sous-classes, y compris [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush), [**CompositionSurfaceBrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) et [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush). Pour en savoir plus sur les pinceaux, consultez [**vue d’ensemble de CompositionBrush**](./composition-brushes.md).

## <a name="the-compositionvisual-sample"></a>Exemple CompositionVisual

Ici, nous examinerons un exemple de code qui illustre les trois types de visuels différents répertoriés précédemment. Bien que cet exemple ne couvre pas les concepts tels que les animations ou les effets plus complexes, il contient les blocs de construction que tous ces systèmes utilisent. (L’exemple de code complet est indiqué à la fin de cet article.)

Dans l’exemple se trouvent un certain nombre de carrés de couleur unie sur lesquels vous pouvez cliquer et faire glisser à propos de l’écran. Lorsque vous cliquez sur un carré, il s’affiche à l’avant, effectue une rotation de 45 degrés et devient opaque lorsque vous le faites glisser.

Cela illustre un certain nombre de concepts de base sur l’utilisation de l’API, notamment :

- Création d’un compositeur
- Création d’un SpriteVisual avec CompositionColorBrush
- Découpage du visuel
- Rotation du visuel
- Définition de l’opacité
- Modification de la position du visuel dans la collection.

## <a name="creating-a-compositor"></a>Création d’un compositeur

La création d’un [**compositeur**](/uwp/api/Windows.UI.Composition.Compositor) et son stockage dans une variable pour une utilisation en tant que fabrique sont une tâche simple. L’extrait de code suivant illustre la création d’un **Compositor** :

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>Création d’un SpriteVisual et d’un ColorBrush

À l’aide du [**compositeur**](/uwp/api/Windows.UI.Composition.Compositor) , il est facile de créer des objets chaque fois que vous en avez besoin, par exemple [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) et [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush):

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

Bien qu’il ne s’agit que de quelques lignes de code, il présente un concept puissant : les objets [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) sont le cœur du système d’effets. Le **SpriteVisual** permet une grande flexibilité et une interconnexion de création de couleur, d’image et d’effet. Le **SpriteVisual** est un type de visuel unique qui peut remplir un rectangle 2D avec un pinceau, dans ce cas, une couleur unie.

## <a name="clipping-a-visual"></a>Découpage d’un élément visuel

Le [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) permet également de créer des découpes dans un [**Visual**](/uwp/api/Windows.UI.Composition.Visual). Voici un exemple de l’utilisation de [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) pour ajuster chaque côté du visuel :

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

À l’instar des autres objets de l’API, [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) peut avoir des animations appliquées à ses propriétés.

## <a name="span-idrotating_a_clipspanspan-idrotating_a_clipspanspan-idrotating_a_clipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>Rotation d’un clip

Un [**visuel**](/uwp/api/Windows.UI.Composition.Visual) peut être transformé à l’aide d’une rotation. Notez que [**RotationAngle**](/uwp/api/windows.ui.composition.visual.rotationangle) prend en charge les radians et les degrés. Par défaut, il s’agit de radians, mais il est facile de spécifier des degrés comme indiqué dans l’extrait de code suivant :

```cs
child.RotationAngleInDegrees = 45.0f;
```

Rotation n’est qu’un des exemples des composants de transformation fournis par l’API qui facilitent ces tâches. D’autres incluent offset, Scale, orientation, RotationAxis et 4x4 TransformMatrix.

## <a name="setting-opacity"></a>Définition de l’opacité

La définition de l’opacité d’un élément visuel est une opération simple utilisant une valeur flottante. Dans l’exemple suivant, tous les carrés sont définis au départ sur l’opacité 0,8 :

```cs
visual.Opacity = 0.8f;
```

À l’instar de la rotation, la propriété [**Opacity**](/uwp/api/windows.ui.composition.visual.opacity) peut être animée.

## <a name="changing-the-visuals-position-in-the-collection"></a>Modification de la position de l’élément visuel dans la collection

L’API de composition permet de modifier la position d’un visuel dans un [**VisualCollection**](/uwp/api/windows.ui.composition.visualcollection) de plusieurs façons. Elle peut être placée au-dessus d’un autre visuel avec [**InsertAbove**](/uwp/api/windows.ui.composition.visualcollection.insertabove), placée sous [**InsertBelow**](/uwp/api/windows.ui.composition.visualcollection.insertbelow), déplacée vers le haut avec [**InsertAtTop**](/uwp/api/windows.ui.composition.visualcollection.insertattop), ou le bas avec [**InsertAtBottom**](/uwp/api/windows.ui.composition.visualcollection.insertatbottom).

Dans l’exemple, un [**visuel**](/uwp/api/Windows.UI.Composition.Visual) sur lequel l’utilisateur a cliqué est trié en haut :

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>Exemple complet

Dans l’exemple complet, tous les concepts ci-dessus sont utilisés ensemble pour construire et parcourir une arborescence simple d’objets [**visuels**](/uwp/api/Windows.UI.Composition.Visual) afin de modifier l’opacité sans utiliser XAML, WWA ou DirectX. Cet exemple illustre la création et l’ajout d’objets **Visual** enfants ainsi que la modification des propriétés.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using Windows.ApplicationModel.Core;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Core;

namespace compositionvisual
{
    class VisualProperties : IFrameworkView
    {
        //------------------------------------------------------------------------------
        //
        // VisualProperties.Initialize
        //
        // This method is called during startup to associate the IFrameworkView with the
        // CoreApplicationView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Initialize(CoreApplicationView view)
        {
            _view = view;
            _random = new Random();
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.SetWindow
        //
        // This method is called when the CoreApplication has created a new CoreWindow,
        // allowing the application to configure the window and start producing content
        // to display.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.SetWindow(CoreWindow window)
        {
            _window = window;
            InitNewComposition();
            _window.PointerPressed += OnPointerPressed;
            _window.PointerMoved += OnPointerMoved;
            _window.PointerReleased += OnPointerReleased;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerPressed
        //
        // This method is called when the user touches the screen, taps it with a stylus
        // or clicks the mouse.
        //
        //------------------------------------------------------------------------------

        void OnPointerPressed(CoreWindow window, PointerEventArgs args)
        {
            Point position = args.CurrentPoint.Position;

            //
            // Walk our list of visuals to determine who, if anybody, was selected
            //
            foreach (var child in _root.Children)
            {
                //
                // Did we hit this child?
                //
                Vector3 offset = child.Offset;
                Vector2 size = child.Size;

                if ((position.X >= offset.X) &&
                    (position.X < offset.X + size.X) &&
                    (position.Y >= offset.Y) &&
                    (position.Y < offset.Y + size.Y))
                {
                    //
                    // This child was hit. Since the children are stored back to front,
                    // the last one hit is the front-most one so it wins
                    //
                    _currentVisual = child as ContainerVisual;
                    _offsetBias = new Vector2((float)(offset.X - position.X),
                                              (float)(offset.Y - position.Y));
                }
            }

            //
            // If a visual was hit, bring it to the front of the Z order
            //
            if (_currentVisual != null)
            {
                ContainerVisual parent = _currentVisual.Parent as ContainerVisual;
                parent.Children.Remove(_currentVisual);
                parent.Children.InsertAtTop(_currentVisual);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerMoved
        //
        // This method is called when the user moves their finger, stylus or mouse with
        // a button pressed over the screen.
        //
        //------------------------------------------------------------------------------

        void OnPointerMoved(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual is selected, drag it with the pointer position and
            // make it opaque while we drag it
            //
            if (_currentVisual != null)
            {
                //
                // Set up the properties of the visual the first time it is
                // dragged. This will last for the duration of the drag
                //
                if (!_dragging)
                {
                    _currentVisual.Opacity = 1.0f;

                    //
                    // Transform the first child of the current visual so that
                    // the image is rotated
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngleInDegrees = 45.0f;
                        child.CenterPoint = new Vector3(_currentVisual.Size.X / 2, _currentVisual.Size.Y / 2, 0);
                        break;
                    }

                    //
                    // Clip the visual to its original layout rect by using an inset
                    // clip with a one-pixel margin all around
                    //
                    var clip = _compositor.CreateInsetClip();
                    clip.LeftInset = 1.0f;
                    clip.RightInset = 1.0f;
                    clip.TopInset = 1.0f;
                    clip.BottomInset = 1.0f;
                    _currentVisual.Clip = clip;

                    _dragging = true;
                }

                Point position = args.CurrentPoint.Position;
                _currentVisual.Offset = new Vector3((float)(position.X + _offsetBias.X),
                                                    (float)(position.Y + _offsetBias.Y),
                                                    0.0f);
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.OnPointerReleased
        //
        // This method is called when the user lifts their finger or stylus from the
        // screen, or lifts the mouse button.
        //
        //------------------------------------------------------------------------------

        void OnPointerReleased(CoreWindow window, PointerEventArgs args)
        {
            //
            // If a visual was selected, make it transparent again when it is
            // released and restore the transform and clip
            //
            if (_currentVisual != null)
            {
                if (_dragging)
                {
                    //
                    // Remove the transform from the first child
                    //
                    foreach (var child in _currentVisual.Children)
                    {
                        child.RotationAngle = 0.0f;
                        child.CenterPoint = new Vector3(0.0f, 0.0f, 0.0f);
                        break;
                    }

                    _currentVisual.Opacity = 0.8f;
                    _currentVisual.Clip = null;
                    _dragging = false;
                }

                _currentVisual = null;
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Load
        //
        // This method is called when a specific page is being loaded in the
        // application.  It is not used for this application.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Load(string unused)
        {

        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Run
        //
        // This method is called by CoreApplication.Run() to actually run the
        // dispatcher's message pump.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Run()
        {
            _window.Activate();
            _window.Dispatcher.ProcessEvents(CoreProcessEventsOption.ProcessUntilQuit);
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.Uninitialize
        //
        // This method is called during shutdown to disconnect the CoreApplicationView,
        // and CoreWindow from the IFrameworkView.
        //
        //------------------------------------------------------------------------------

        void IFrameworkView.Uninitialize()
        {
            _window = null;
            _view = null;
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.InitNewComposition
        //
        // This method is called by SetWindow(), where we initialize Composition after
        // the CoreWindow has been created.
        //
        //------------------------------------------------------------------------------

        void InitNewComposition()
        {
            //
            // Set up Windows.UI.Composition Compositor, root ContainerVisual, and associate with
            // the CoreWindow.
            //

            _compositor = new Compositor();

            _root = _compositor.CreateContainerVisual();



            _compositionTarget = _compositor.CreateTargetForCurrentView();
            _compositionTarget.Root = _root;

            //
            // Create a few visuals for our window
            //
            for (int index = 0; index < 20; index++)
            {
                _root.Children.InsertAtTop(CreateChildElement());
            }
        }

        //------------------------------------------------------------------------------
        //
        // VisualProperties.CreateChildElement
        //
        // Creates a small sub-tree to represent a visible element in our application.
        //
        //------------------------------------------------------------------------------

        Visual CreateChildElement()
        {
            //
            // Each element consists of three visuals, which produce the appearance
            // of a framed rectangle
            //
            var element = _compositor.CreateContainerVisual();
            element.Size = new Vector2(100.0f, 100.0f);

            //
            // Position this visual randomly within our window
            //
            element.Offset = new Vector3((float)(_random.NextDouble() * 400), (float)(_random.NextDouble() * 400), 0.0f);

            //
            // The outer rectangle is always white
            //
            //Note to preview API users - SpriteVisual and Color Brush replace SolidColorVisual
            //for example instead of doing
            //var visual = _compositor.CreateSolidColorVisual() and
            //visual.Color = Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF);
            //we now use the below

            var visual = _compositor.CreateSpriteVisual();
            element.Children.InsertAtTop(visual);
            visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
            visual.Size = new Vector2(100.0f, 100.0f);

            //
            // The inner rectangle is inset from the outer by three pixels all around
            //
            var child = _compositor.CreateSpriteVisual();
            visual.Children.InsertAtTop(child);
            child.Offset = new Vector3(3.0f, 3.0f, 0.0f);
            child.Size = new Vector2(94.0f, 94.0f);

            //
            // Pick a random color for every rectangle
            //
            byte red = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte green = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            byte blue = (byte)(0xFF * (0.2f + (_random.NextDouble() / 0.8f)));
            child.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, red, green, blue));

            //
            // Make the subtree root visual partially transparent. This will cause each visual in the subtree
            // to render partially transparent, since a visual's opacity is multiplied with its parent's
            // opacity
            //
            element.Opacity = 0.8f;

            return element;
        }

        // CoreWindow / CoreApplicationView
        private CoreWindow _window;
        private CoreApplicationView _view;

        // Windows.UI.Composition
        private Compositor _compositor;
        private CompositionTarget _compositionTarget;
        private ContainerVisual _root;
        private ContainerVisual _currentVisual;
        private Vector2 _offsetBias;
        private bool _dragging;

        // Helpers
        private Random _random;
    }


    public sealed class VisualPropertiesFactory : IFrameworkViewSource
    {
        //------------------------------------------------------------------------------
        //
        // VisualPropertiesFactory.CreateView
        //
        // This method is called by CoreApplication to provide a new IFrameworkView for
        // a CoreWindow that is being created.
        //
        //------------------------------------------------------------------------------

        IFrameworkView IFrameworkViewSource.CreateView()
        {
            return new VisualProperties();
        }


        //------------------------------------------------------------------------------
        //
        // main
        //
        //------------------------------------------------------------------------------

        static int Main(string[] args)
        {
            CoreApplication.Run(new VisualPropertiesFactory());

            return 0;
        }
    }
}
```