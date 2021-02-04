---
description: この記事では、C++/WinRT で WinUI 3 の XAML テンプレート コントロールを作成する手順について説明します。
title: C++/WinRT を使用した WinUI 3 アプリ用のテンプレート化された XAML コントロール
ms.date: 07/09/2020
ms.topic: article
keywords: Windows 10, UWP, カスタム コントロール, テンプレート コントロール, WinUI, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: d319374791eeb0a02b0291c66f25f55e31bcbc4b
ms.sourcegitcommit: 6759309a3fbb6ede498c95c04c05f57a074ab070
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2021
ms.locfileid: "99069169"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>C++/WinRT を使用した WinUI 3 アプリ用のテンプレート化された XAML コントロール

この記事では、C++/WinRT で WinUI 3 のテンプレート化された XAML コントロールを作成する手順について説明します。 テンプレート化されたコントロールは **Microsoft.UI.Xaml.Controls.Control** から継承され、XAML コントロール テンプレートを使用してカスタマイズできる視覚的な構造と視覚的な動作を持っています。 この記事では、記事「[C++/WinRT による XAML カスタム (テンプレート化) コントロール](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)」と同じシナリオについて説明しますが、WinUI 3 を使用するように調整されています。

この記事の手順を実行する前に、開発環境が WinUI 3 アプリを作成するように構成されていることを確認する必要があります。 セットアップ情報については、「[デスクトップ アプリ用の WinUI 3 の概要](./get-started-winui3-for-desktop.md)」を参照してください。 また、[Visual Studio Marketplace](https://marketplace.visualstudio.com) から最新バージョンの [C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) をダウンロードしてインストールする必要もあります。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>空のアプリを作成する (BgLabelControlApp)

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 [`Create a new project`] ダイアログで、 **[空のアプリ (UWP 内の WinUI)]** プロジェクト テンプレートを選択します。このとき、必ず C++ 言語バージョンを選択します。 プロジェクト名を "BgLabelControlApp" に設定して、ファイル名が次の例のコードと一致するようにします。 **[ターゲット バージョン]** を Windows 10 バージョン 1903 (ビルド 18362) に、 **[最小バージョン]** を Windows 10 バージョン 1803 (ビルド 17134) に設定します。 このチュートリアルは、 **[Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (デスクトップの WinUI)\)** プロジェクト テンプレートを使用して作成するデスクトップ アプリでも有効です。**BgLabelControlApp (デスクトップ)** のすべての手順は必ず実行してください。

![空のアプリ プロジェクト テンプレート](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>テンプレート コントロールをアプリに追加する

テンプレート コントロールを追加するには、ツールバーの **[プロジェクト]** メニューをクリックするか、**ソリューション エクスプローラー** でプロジェクトを右クリックし、 **[新しい項目の追加]** を選択します。 **[Visual C++] -> [WinUI]** の下で、 **[カスタム コントロール (WinUI)]** テンプレートを選択します。 新しいコントロールに "BgLabelControl" という名前を指定し、 *[追加]* をクリックします。 これにより、3 つの新しいファイルがプロジェクトに追加されます。 `BgLabelControl.h` はコントロールの宣言を含むヘッダーで、`BgLabelControl.cpp` にはこのコントロールの C++/WinRT 実装が含まれています。 `BgLabelControl.idl` は、コントロールをランタイム クラスとしてインスタンス化できるようにするインターフェイス定義ファイルです。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>BgLabelControl カスタム コントロール クラスを実装する

以下の手順で、ランタイム クラスを実装するために、プロジェクト ディレクトリ内の `BgLabelControl.idl`、`BgLabelControl.h`、`BgLabelControl.cpp` ファイルのコードを更新します。 


テンプレート コントロール クラスは XAML マークアップからインスタンス化されます。そのため、ランタイム クラスになります。 完成したプロジェクトをビルドすると、MIDL コンパイラ (midl.exe) により `BgLabelControl.idl` ファイルを使用してコントロールの Windows ランタイム メタデータ ファイル (.winmd) が生成されます。これが、コンポーネントのコンシューマーによって参照されます。 ランタイム クラスの作成の詳細については、「[C++/WinRT での API の作成](/windows/uwp/cpp-and-winrt-apis/author-apis)」をご覧ください。

ここで作成しているテンプレート コントロールでは、コントロールのラベルとして使用される文字列である、1 つのプロパティが公開されます。 `BgLabelControl.idl` の内容を次のコードに置き換えます。

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上記の一覧は、依存関係プロパティ (DP) を宣言するときに従うパターンを示しています。 各 DP には 2 つの部分があります。 まず、DependencyProperty 型の読み取り専用の静的プロパティを宣言します。 名前は、お使いの DP に Property を加えたものになります。 実装ではこの静的プロパティを使用します。 次に、DP の型と名前持つ読み取りおよび書き込みインスタンス プロパティを宣言します。 (DP ではなく) 添付プロパティを作成する場合は、「[カスタム添付プロパティ](/windows/uwp/xaml-platform/custom-attached-properties)」のコード例を参照してください。

上記のコードで参照されている XAML クラスは、Microsoft.UI.Xaml 名前空間に存在していることに注意してください。 これによってこれらは Windows.UI.XAML 名前空間に定義される UWP XAML コントロールではなく、WinUI コントロールとして区別されます。

BgLabelControl.h の内容を次のコードに置き換えます。

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

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

上に示されているコードでは、**Label** プロパティと **LabelProperty** プロパティを実装し、依存関係プロパティの値の変更を処理するための **OnLabelChanged** という名前の静的イベント ハンドラーを追加し、**LabelProperty** のバッキング フィールドを格納するためのプライベート メンバーを追加しています。 ここでも、ヘッダー ファイルで参照される XAML クラスは、UWP UI フレームワークによって使用される Windows.UI.Xaml 名前空間ではなく、WinUI 3 フレームワークに属する Microsoft.UI.Xaml 名前空間に存在していることに注意してください。


次に、BgLabelControl.cpp の内容を次のコードに置き換えます。

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"
#endif

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
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
このチュートリアルでは **OnLabelChanged** コールバックは使用しませんが、プロパティ変更コールバックを使用して依存関係プロパティを登録する方法を確認できるようにするためにこれが用意されています。 **OnLabelChanged** の実装では、投影された基本型から投影された派生型を取得する方法も示しています (ここでは、投影された基本型は DependencyObject です)。 また、プロジェクションが実行された型を実装する型へのポインターを取得する方法を示しています。 その 2 番目の操作は、当然ながら、プロジェクションが実行された型を実装するプロジェクト (つまり、ランタイム クラスを実装するプロジェクト) でのみ可能です。

WinUI 3 プロジェクト テンプレートに既定で含まれていない [xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) 関数は、Windows.UI.Xaml.Interop 名前空間によって提供されます。 この名前空間に関連付けられているヘッダー ファイルを含めるために、プロジェクトのプリコンパイル済みヘッダー ファイル `pch.h` に 1 行追加します。

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>BgLabelControl の既定のスタイルを定義する

そのコンストラクター内で、**BgLabelControl** によってそれ自体の既定のスタイル キーが設定されます。 テンプレート コントロールには、コントロールのコンシューマーがスタイルやテンプレートを設定しない場合にレンダリングするために使用できる既定のコントロール テンプレートが含まれている、既定のスタイルを指定する必要があります。 このセクションでは、既定のスタイルを含むマークアップ ファイルをプロジェクトに追加します。

(**ソリューション エクスプローラー** で) **[すべてのファイルを表示]** がまだオンになっていることを確認します。 プロジェクト ノードで、新しいフォルダー (フィルターではなくフォルダー) を作成し、"Themes" という名前を付けます。 `Themes` の下に、種類が **[Visual C++] > [XAML] > [リソース ディクショナリ (WinUI)]** である新しい項目を追加し、"Generic.xaml" という名前を付けます。 XAML フレームワークでテンプレート コントロールの既定のスタイルを検出するために、フォルダーとファイルの名前をこのようにする必要があります。 Generic.xaml の既定のコンテンツを削除し、以下のマークアップを貼り付けます。

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

この場合、既定のスタイルに設定される唯一のプロパティはコントロール テンプレートです。 テンプレートは、1 つの正方形 (背景は XAML **Control** 型のすべてのインスタンスに設定される **Background** プロパティにバインドされています) と、1 つのテキスト要素 (テキストは **BgLabelControl::Label** 依存関係プロパティにバインドされています) で構成されます。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>メイン UI ページに BgLabelControl のインスタンスを追加する

メイン UI ページの XAML マークアップが含まれている `MainPage.xaml` を開きます。 **Button** 要素の直後 (**StackPanel** 内) に次のマークアップを追加します。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

また、**MainPage** 型 (XAML マークアップと命令型コードのコンパイルの組み合わせ) で **BgLabelControl** テンプレート コントロール型が認識されるように、`MainPage.h` に次の include ディレクティブを追加します。 他の XAML ページから **BgLabelControl** を使用する場合は、そのページのヘッダー ファイルにも同じ include ディレクティブを追加します。 または、単にプリコンパイル済みヘッダー ファイルに 1 つの include ディレクティブを追加します。

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

ここでプロジェクトをビルドして実行します。 既定のコントロール テンプレートが、マークアップの **BgLabelControl** インスタンスの背景ブラシとラベルにバインドされることがわかります。

![テンプレート コントロールの結果](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>**MeasureOverride** や **OnApplyTemplate** などのオーバーライド可能な関数の実装

テンプレート コントロールは **Control** ランタイム クラスから派生し、それ自体も基本ランタイム クラスから派生します。 また、派生クラスでオーバーライドできる **Control**、**FrameworkElement**、および **UIElement** というオーバーライド可能なメソッドがあります。 この方法を示すコード例を次に示します。

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

*オーバーライド可能な* 関数は、さまざまな言語プロジェクションで通常と異なる場合があります。 たとえば、C# では、オーバーライド可能な関数は通常、保護された仮想関数として表示されます。 C++/WinRT では、これらは仮想でも保護対象でもありませんが、上記のように、それらをオーバーライドして独自の実装を提供できます。



## <a name="generating-the-control-source-files-without-using-a-template"></a>テンプレートを使用せずにコントロールのソース ファイルを生成する。

このセクションでは、**カスタム コントロール** の項目テンプレートを使用せずに、カスタム コントロールを作成するために必要なソース ファイルを生成する方法について説明します。 

最初に、新しい Midl ファイル (.idl) 項目をプロジェクトに追加します。 **[プロジェクト]** メニューで、 **[新しい項目の追加...]** を選択し、検索ボックスに「MIDL」と入力して、.idl ファイル項目を検索します。 新しいファイルに `BgLabelControl.idl` という名前を付けて、この記事の手順で使用される名前と同じになるようにします。 `BgLabelControl.idl` の既定のコンテンツを削除し、上記の手順のランタイム クラス宣言を貼り付けます。


新しい .idl ファイルを保存したら、次の手順として、Windows ランタイム メタデータ ファイル (.winmd) と、テンプレート コントロールを実装するために使用する実装ファイル (.cpp と .h) のスタブを生成します。 これらのファイルは、ソリューションをビルドすることで生成します。これにより、MIDL コンパイラ (midl.exe) によって、作成した .idl ファイルがコンパイルされます。 ソリューションは正常にビルドされず、Visual Studio によって出力ウィンドウにビルド エラーが表示されますが、必要なファイルは生成されることに注意してください。

スタブ ファイルの BgLabelControl.h と BgLabelControl.cpp を \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\ からプロジェクト フォルダーにコピーします。 **ソリューション エクスプローラー** で、[すべてのファイルを表示] がオンであることを確認します。 コピーしたスタブ ファイルを右クリックし、 **[プロジェクトに含める]** をクリックします。

コンパイラでは、生成されるファイルがコンパイルされないように、BgLabelControl.h と BgLabelControl.cpp の先頭に static_assert 行が置かれます。 コントロールを実装するときに、プロジェクト ディレクトリに配置したファイルから、これらの行を削除する必要があります。 このチュートリアルでは、ファイルの内容全体を、上で示したコードで上書きすることができます。
