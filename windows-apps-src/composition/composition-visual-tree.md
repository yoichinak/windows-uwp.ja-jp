---
ms.assetid: f1297b7d-1a10-52ae-dd84-6d1ad2ae2fe6
title: コンポジションのビジュアル
description: コンポジションのビジュアル オブジェクト ツリー構造は、コンポジション API の他のすべての機能でベースとして使われます。 この API により、開発者は 1 つまたは複数のビジュアル オブジェクトを作成して定義できます。それぞれがビジュアル オブジェクト ツリーの 1 つのノードを表します。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85df48b4f43759013f80623595d919ac6c77337
ms.sourcegitcommit: ef3cdca5e9b8f032f46174da4574cb5593d32d56
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2020
ms.locfileid: "90593436"
---
# <a name="composition-visual"></a>コンポジションのビジュアル

コンポジションのビジュアル オブジェクト ツリー構造は、コンポジション API の他のすべての機能でベースとして使われます。 この API により、開発者は 1 つまたは複数のビジュアル オブジェクトを作成して定義できます。それぞれがビジュアル オブジェクト ツリーの 1 つのノードを表します。

## <a name="visuals"></a>ビジュアル

ビジュアルツリー構造を構成するいくつかの種類のビジュアルと、ビジュアルのコンテンツに影響を与える複数のサブクラスを持つ基本ブラシクラスがあります。

- [**Visual**](/uwp/api/Windows.UI.Composition.Visual) – ベース オブジェクト。プロパティの大半はここにあり、他のビジュアル オブジェクトによって継承されます。
- [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) – [**Visual**](/uwp/api/Windows.UI.Composition.Visual) から派生し、子ビジュアル オブジェクトを作成できます。
  - [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)から派生します。 には、画像、効果、または純色を含むピクセルを表示できるように、ブラシを関連付ける機能があります。
  - [**レイヤービジュアル**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)から派生します。 ビジュアルの子は、1つのレイヤーにフラット化されます。<br/>(_Windows 10、バージョン1607、SDK 14393 で導入され_ました)。
  - [**図形ビジュアル**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)から派生します。 CompositionShape のルートであるビジュアルツリーノード。<br/>(_Windows 10、バージョン1803、SDK 17134 で導入され_ました)。
  - [**Redirectvisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)から派生します。 ビジュアルは、別のビジュアルからコンテンツを取得します。<br/>(_Windows 10、バージョン1809、SDK 17763 で導入され_ました)。
  - [**SceneVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) – [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual)から派生します。 3D シーンのノードのコンテナービジュアル。<br/>(_Windows 10、バージョン1903、SDK 18362 で導入され_ました)。

[**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) とそのサブクラスである [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush)、[**CompositionSurfaceBrush**](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)、[**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) を使用して、コンテンツと効果を SpriteVisual に適用できます。 ブラシの詳細については、「 [**CompositionBrush の概要**](./composition-brushes.md)」を参照してください。

## <a name="the-compositionvisual-sample"></a>CompositionVisual のサンプル

ここでは、前に説明した 3 つの種類のビジュアル タイプを使用する、いくつかのサンプル コードについて説明します。 このサンプルでは、アニメーションや複雑な効果などの概念については説明していませんが、すべてのシステムが使用する構成要素が含まれています。 (完全なサンプル コードは、この記事の最後に示されています。)

このサンプルでは、画面でクリックしてドラッグできる複数の単色の正方形を使います。 正方形がクリックされると、前面に移動して 45 度回転し、ドラッグされると不透明になります。

ここでは、次のように API の操作について多数の基本的な概念を示します。

- コンポジターの作成
- SpriteVisual と CompositionColorBrush の作成
- ビジュアル オブジェクトのクリッピング
- ビジュアル オブジェクトの回転
- 不透明度の設定
- コレクション内のビジュアルの位置を変更します。

## <a name="creating-a-compositor"></a>コンポジターの作成

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) を作成し、ファクトリ用に変数に格納するのは簡単です。 次のスニペットでは、新しい **Compositor** の作成方法を示しています。

```cs
_compositor = new Compositor();
```

## <a name="creating-a-spritevisual-and-colorbrush"></a>SpriteVisual と ColorBrush の作成

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) を使って、必要なときにオブジェクト、たとえば [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) や [**CompositionColorBrush**](/uwp/api/Windows.UI.Composition.CompositionColorBrush) を作成するのは簡単です。

```cs
var visual = _compositor.CreateSpriteVisual();
visual.Brush = _compositor.CreateColorBrush(Color.FromArgb(0xFF, 0xFF, 0xFF, 0xFF));
```

これはわずか数行のコードですが、強力な概念を示しており、[**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) オブジェクトは効果システムの中核となります。 **SpriteVisual** を使うと、色、画像、効果の作成で高い柔軟性と関係性を得られます。 **SpriteVisual**は、ブラシ (この場合は純色) で2d 四角形に塗りつぶすことができる1つのビジュアル型です。

## <a name="clipping-a-visual"></a>ビジュアル オブジェクトのクリップ

[**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) は、[**Visual**](/uwp/api/Windows.UI.Composition.Visual) に対するクリップを作成するためにも使えます。 次に示しているのは、ビジュアル オブジェクトの両側をトリミングする [**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) を使ったサンプルからの例です。

```cs
var clip = _compositor.CreateInsetClip();
clip.LeftInset = 1.0f;
clip.RightInset = 1.0f;
clip.TopInset = 1.0f;
clip.BottomInset = 1.0f;
_currentVisual.Clip = clip;
```

API の他のオブジェクトと同様、[**InsetClip**](/uwp/api/Windows.UI.Composition.InsetClip) のプロパティにもアニメーションを適用できます。

## <a name="span-idrotating_a_clipspanspan-idrotating_a_clipspanspan-idrotating_a_clipspanrotating-a-clip"></a><span id="Rotating_a_Clip"></span><span id="rotating_a_clip"></span><span id="ROTATING_A_CLIP"></span>クリップの回転

[**Visual**](/uwp/api/Windows.UI.Composition.Visual) は回転により変換できます。 [**RotationAngle**](/uwp/api/windows.ui.composition.visual.rotationangle) では、ラジアンと度の両方がサポートされています。 既定値はラジアンですが、次のスニペットに示すように、角度を簡単に指定できます。

```cs
child.RotationAngleInDegrees = 45.0f;
```

Rotation は、変換が簡単になるように API に用意された一連の変換コンポーネントのほんの一例です。 そのほかにも Offset、Scale、Orientation、RotationAxis、4x4 TransformMatrix などがあります。

## <a name="setting-opacity"></a>不透明度の設定

ビジュアル オブジェクトの不透明度の設定も簡単で、浮動小数値を使って指定するだけです。 たとえば、このサンプルでは、すべての正方形の不透明度は .8 から始めています。

```cs
visual.Opacity = 0.8f;
```

Rotation と同様、[**Opacity**](/uwp/api/windows.ui.composition.visual.opacity) のプロパティにもアニメーションを適用できます。

## <a name="changing-the-visuals-position-in-the-collection"></a>コレクション内のビジュアル オブジェクトの位置変更

コンポジション API を使うと、[**VisualCollection**](/uwp/api/windows.ui.composition.visualcollection) でのビジュアルの位置を多くの方法で変更できます。 たとえば、[**InsertAbove**](/uwp/api/windows.ui.composition.visualcollection.insertabove) を使うと、別のビジュアルの上に、[**InsertBelow**](/uwp/api/windows.ui.composition.visualcollection.insertbelow) を使うと、下に配置できます。[**InsertAtTop**](/uwp/api/windows.ui.composition.visualcollection.insertattop) を使うと、先頭に、[**InsertAtBottom**](/uwp/api/windows.ui.composition.visualcollection.insertatbottom) を使うと、末尾に移動できます。

このサンプルでは、クリックされた [**Visual**](/uwp/api/Windows.UI.Composition.Visual) は先頭に並べ替えられています。

```cs
parent.Children.InsertAtTop(_currentVisual);
```

## <a name="full-example"></a>完全な例

完全なサンプルでは、これまで説明した概念のすべてを一緒に使って、[**Visual**](/uwp/api/Windows.UI.Composition.Visual) オブジェクトの単純なツリーを作成してたどり、XAML、WWA、または DirectX を使わずに不透明度を変更しています。 このサンプルでは、どのように子 **Visual** オブジェクトが作成されて追加され、プロパティが変更されるかを示しています。

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