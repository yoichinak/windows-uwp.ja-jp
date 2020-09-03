---
description: このトピックでは、C++/WinRT を使用して、シンプルなカスタム コントロールを作成する手順について説明します。 ここに示されている情報に基づき、豊富な機能のカスタム可能な独自の UI コントロールを作成することができます。
title: C++/WinRT による XAML カスタム (テンプレート化) コントロール
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, XAML, カスタム, テンプレート化, コントロール
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 805e9db834e4428f8db5815b54b8d1d669310611
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154226"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>C++/WinRT による XAML カスタム (テンプレート化) コントロール

> [!IMPORTANT]
> [C++/WinRT](./intro-to-using-cpp-with-winrt.md) でランタイム クラスを使用および作成する方法についての理解をサポートするために重要な概念と用語については、「[C++/WinRT での API の使用](consume-apis.md)」と「[C++/WinRT での API の作成](author-apis.md)」を参照してください。

ユニバーサル Windows プラットフォーム (UWP) の最も強力な機能の 1 つは、XAML [**Control**](/uwp/api/windows.ui.xaml.controls.control) 型に基づいてカスタム コントロールを作成できるユーザー インターフェイス (UI) スタックの柔軟性です。 XAML UI フレームワークには、[カスタム依存関係プロパティ](../xaml-platform/custom-dependency-properties.md)、[添付プロパティ](../xaml-platform/custom-attached-properties.md)、[コントロール テンプレート](../design/controls-and-patterns/control-templates.md)などの機能が用意されており、豊富な機能でカスタマイズ可能なコントロールを簡単に作成できます。 このトピックでは、C++ /WinRT を使用してカスタム (テンプレート) コントロールを作成する手順について説明します。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>空のアプリを作成する (BgLabelControlApp)

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **空のアプリ (C++/WinRT)** を作成し、名前を *BgLabelControlApp* にして、(フォルダー構造がチュートリアルと一致するように) **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。

このトピックの後半で、プロジェクトをビルドするよう指示します (ただし、それまでビルドしないでください)。

> [!NOTE]
> &mdash;C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用など、&mdash;C++/WinRT 開発用に Visual Studio を設定する方法については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

これから、カスタム (テンプレート) コントロールを表す新しいクラスを作成します。 同じコンパイル ユニット内のクラスを作成および使用しています。 ただし、このクラスを XAML マークアップからインスタンス化できるようにするため、ランタイム クラスにします。 また、この作成と使用のどちらにも C++/WinRT を使用します。

新しいランタイム クラスの作成の最初の手順では、新しい **Midl ファイル (.idl)** 項目をプロジェクトに追加します。 これに `BgLabelControl.idl` という名前をつけます。 `BgLabelControl.idl` の既定のコンテンツを削除し、このランタイム クラスの宣言に貼り付けます。

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上記の一覧は、依存関係プロパティ (DP) を宣言するときに従うパターンを示しています。 各 DP には 2 つの部分があります。 まず、型 [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty) の読み取り専用の静的プロパティを宣言します。 これは、実際の DP の名前に *Property* を加えたものです。 実装ではこの静的プロパティを使用します。 次に、DP の型と名前持つ読み取りおよび書き込みインスタンス プロパティを宣言します。 (DP ではなく) *添付プロパティ*を作成する場合は、「[カスタム添付プロパティ](../xaml-platform/custom-attached-properties.md)」のコード例を参照してください。

> [!NOTE]
> 浮動小数点型の DP が必要な場合は、`double` ([MIDL 3.0](/uwp/midl-3/) では `Double`) にします。 型 `float` (MIDL では `Single`) の DP を宣言して実装し、XAML マークアップでその DP の値を設定すると、エラー "*テキスト '<NUMBER>' から 'Windows.Foundation.Single' を作成できませんでした*" が発生します。

ファイルを保存します。 現時点ではプロジェクトは完成するまでビルドされませんが、ここでビルドすることは有用です。それによって、**BgLabelControl** ランタイム クラスを実装するためのソース コード ファイルが生成されるからです。 それでは、今すぐビルドしてみましょう (この段階で表示されることが予想されるビルド エラーは、"未解決の外部シンボル" に関係します)。

ビルド プロセス中に、`midl.exe` ツールが実行されて、ランタイム クラスを記述する Windows ランタイム メタデータ ファイル (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) が作成されます。 次に、`cppwinrt.exe` ツールが実行され、ランタイム クラスの作成と使用をサポートするソース コード ファイルが生成されます。 これらのファイルには、IDL で宣言した **BgLabelControl** ランタイム クラスの実装を開始するためのスタブが含まれています。 これらのスタブは `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` と `BgLabelControl.cpp` です。

スタブ ファイル `BgLabelControl.h` と `BgLabelControl.cpp` を `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` からプロジェクト フォルダー `\BgLabelControlApp\BgLabelControlApp\` にコピーします。 **ソリューション エクスプローラー**で、 **[すべてのファイルを表示]** がオンであることを確認します。 コピーしたスタブ ファイルを右クリックし、 **[プロジェクトに含める]** をクリックします。

`BgLabelControl.h` と `BgLabelControl.cpp` の先頭に `static_assert` がありますが、これを削除する必要があります。 これで、プロジェクトがビルドされます。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>**BgLabelControl** カスタム コントロール クラスを実装する
ここで、`\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` と `BgLabelControl.cpp` を開いてランタイム クラスを実装してみましょう。 `BgLabelControl.h` で、コンストラクターを変更して既定のスタイル キーを設定し、**Label** と **LabelProperty** を実装し、**OnLabelChanged** という名前の静的イベント ハンドラーを追加して依存関係プロパティの値の変更を処理し、**LabelProperty** のバッキング フィールドを格納するプライベート メンバーを追加します。

これらを追加すると、`BgLabelControl.h` は次のようになります。 次のコード リストをコピーして貼り付け、`BgLabelControl.h` の内容を置き換えることができます。

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl>
    {
        BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

        winrt::hstring Label()
        {
            return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
        }

        void Label(winrt::hstring const& value)
        {
            SetValue(m_labelProperty, winrt::box_value(value));
        }

        static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Windows::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

`BgLabelControl.cpp` で、このような静的なメンバーを定義します。 次のコード リストをコピーして貼り付け、`BgLabelControl.cpp` の内容を置き換えることができます。

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#include "BgLabelControl.g.cpp"

namespace winrt::BgLabelControlApp::implementation
{
    Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Windows::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```

このチュートリアルでは、**OnLabelChanged** を使用しません。 ただし、これは、依存関係プロパティをプロパティが変更されたコールバックに登録する方法を確認できるようにするためにあります。 **OnLabelChanged** の実装は、プロジェクションが実行された基本型からプロジェクションが実行された派生型を取得する方法を示しています (この場合、プロジェクションが実行された基本型は **DependencyObject** です)。 また、プロジェクションが実行された型を実装する型へのポインターを取得する方法を示しています。 その 2 番目の操作は、当然ながら、プロジェクションが実行された型を実装するプロジェクト (つまり、ランタイム クラスを実装するプロジェクト) でのみ可能です。

> [!NOTE]
> Windows SDK バージョン 10.0.17763.0 (Windows 10 バージョン 1809) 以降をインストールしていない場合は、[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) ではなく、上記のイベント ハンドラーを変更して依存関係プロパティで [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) を呼び出す必要があります。

## <a name="design-the-default-style-for-bglabelcontrol"></a>**BgLabelControl** の既定のスタイルを設計する

そのコンストラクター内で、**BgLabelControl** によってそれ自体の既定のスタイル キーが設定されます。 しかし、既定のスタイル*とは*何でしょうか。 カスタム (テンプレート) コントロールには、コントロールのコンシューマーがスタイルやテンプレートを設定しない場合にレンダリングに使用できる&mdash;既定のコントロール テンプレートを含む&mdash;既定のスタイルを指定する必要があります。 このセクションでは、既定のスタイルを含むマークアップ ファイルをプロジェクトに追加します。

(**ソリューション エクスプローラー**で) **[すべてのファイルを表示]** がまだオンになっていることを確認します。 プロジェクト ノードで、新しいフォルダー (フィルターではなくフォルダー) を作成し、"Themes" という名前を付けます。 `Themes` 以下に、種類が **Visual C++**  > **XAML** > **XAML ビュー**の新しい項目を追加し、"Generic.xaml" という名前を付けます。 XAML フレームワークでカスタム コントロールの既定のスタイルを検出するには、フォルダーとファイルの名前をこのようにする必要があります。 `Generic.xaml` の既定のコンテンツを削除し、以下のマークアップを貼り付けます。

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

この場合、既定のスタイルに設定される唯一のプロパティはコントロール テンプレートです。 テンプレートは 1 つの正方形 (背景は XAML [**Control**](/uwp/api/windows.ui.xaml.controls.control) 型のすべてのインスタンスが持つ **Background** プロパティにバインドされています) と、1 つのテキスト要素 (テキストは **BgLabelControl::Label** 依存プロパティにバインドされています) から構成されます。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>メイン UI ページに **BgLabelControl** のインスタンスを追加します

メイン UI ページの XAML マークアップが含まれている `MainPage.xaml` を開きます。 **Button** 要素の直後 (**StackPanel** 内) に次のマークアップを追加します。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

また、**MainPage** 型 (XAML マークアップと命令型コードのコンパイルの組み合わせ) が **BgLabelControl** カスタム コントロール型を認識するように、`MainPage.h` に次の include ディレクティブを追加します。 他の XAML ページから **BgLabelControl** を使用する場合は、そのページのヘッダー ファイルにも同じ include ディレクティブを追加します。 または、単にプリコンパイル済みヘッダー ファイルに 1 つの include ディレクティブを追加します。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

ここでプロジェクトをビルドして実行します。 既定のコントロール テンプレートが、マークアップの **BgLabelControl** インスタンスの背景ブラシとラベルにバインドされることがわかります。

このチュートリアルでは、C++/WinRT のカスタム (テンプレート) コントロールの簡単な例を示しました。 必要に応じて、ご自身のカスタム コントロールをリッチで全機能を備えたものにすることができます。 たとえば、カスタム コントロールは、編集可能なデータ グリッド、ビデオ プレーヤー、または 3D ジオメトリのビジュアライザーのように複雑な形式にすることができます。

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>**MeasureOverride** や **OnApplyTemplate** など、*オーバーライド可能な*関数を実装しています

カスタム コントロールは [**Control**](/uwp/api/windows.ui.xaml.controls.control) ランタイム クラスから派生し、それ自体も基本ランタイム クラスから派生します。 また、派生クラスでオーバーライドできる **Control**、[**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)、[**UIElement**](/uwp/api/windows.ui.xaml.uielement) のオーバーライド可能なメソッドがあります。 この方法を示すコード例を次に示します。

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

*オーバーライド可能な*関数は、さまざまな言語プロジェクションで通常と異なる場合があります。 たとえば、C# では、オーバーライド可能な関数は通常、保護された仮想関数として表示されます。 C++/WinRT では、これらは仮想でも保護対象でもありませんが、上記のように、それらをオーバーライドして独自の実装を提供できます。

## <a name="important-apis"></a>重要な API
* [コントロール クラス](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty クラス](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement クラス](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement クラス](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>関連トピック
* [コントロール テンプレート](../design/controls-and-patterns/control-templates.md)
* [カスタム依存関係プロパティ](../xaml-platform/custom-dependency-properties.md)