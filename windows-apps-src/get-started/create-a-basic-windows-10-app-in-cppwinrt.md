---
title: "\"Hello, World!\" アプリを C++/WinRT を使用して作成する"
description: このトピックでは、Windows 10 UWP の "Hello, World!" アプリを、C++/WinRT を使用して作成する手順を説明します。 アプリの UI は、Extensible Application Markup Language (XAML) を使用して定義します。
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: bb6a76f2e8096d63907daf5ededdb6a22eb72a6c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175206"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>"Hello, World!" アプリを C++/WinRT を使用して作成する

このトピックでは、Windows 10 ユニバーサル Windows プラットフォーム (UWP) の "Hello, World!" アプリを、C++/WinRT を使用して作成する手順を説明します。 アプリのユーザー インターフェイス (UI) は、Extensible Application Markup Language (XAML) を使用して定義します。

C++/WinRT は、Windows ランタイム (WinRT) API 用に完全に標準化された最新の C++17 言語プロジェクションです。 詳細情報、他のチュートリアル、コード例については、[C++/WinRT](../cpp-and-winrt-apis/index.md) のドキュメントを参照してください。 まずは、「[C++/WinRT の使用を開始する](../cpp-and-winrt-apis/get-started.md)」をご覧ください。

## <a name="set-up-visual-studio-for-cwinrt"></a>C++/WinRT 用に Visual Studio をセットアップする

&mdash;C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用など、&mdash;C++/WinRT 開発用に Visual Studio を設定する方法については、[Visual Studio での C++/WinRT のサポート](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

Visual Studio をダウンロードするには、「[のダウンロード](https://visualstudio.microsoft.com/downloads/)」を参照してください。

XAML の概要については、「[XAML の概要](../xaml-platform/xaml-overview.md)」を参照してください

## <a name="create-a-blank-app-helloworldcppwinrt"></a>空のアプリ (HelloWorldCppWinRT) を作成する

初めてのアプリである "Hello, World!" アプリでは、対話機能、レイアウト、スタイルに関するいくつかの基本的な機能を示します。

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **空のアプリ (C++/WinRT)** プロジェクトを作成し、*HelloWorldCppWinRT* という名前を付けます。 **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。

このトピックの後半で、プロジェクトをビルドするよう指示します (ただし、それまでビルドしないでください)。

### <a name="about-the-project-files"></a>プロジェクト ファイルについて

通常、プロジェクト フォルダーでは、`.xaml` (XAML マークアップ) ファイルごとに、対応する `.idl`、`.h`、`.cpp` ファイルがあります。 これらのファイルはまとめて、XAML ページ型にコンパイルされます。

UI 要素が作成されるように XAML マークアップ ファイルを変更したり、それらの要素をデータ ソースにバインドしたり ([データ バインディング](../data-binding/index.md)と呼ばれるタスク) することができます。 `.h` ファイルと `.cpp` ファイル (および場合によっては `.idl` ファイル) を変更して、XAML ページのカスタム ロジック (イベント ハンドラーなど) を追加します。

プロジェクト ファイルをいくつか見てみましょう。

- `App.idl`、`App.xaml`、`App.h`、`App.cpp`。 これらのファイルでは、[**Windows::UI::Xaml::Application**](/uwp/api/windows.ui.xaml.application) クラスをアプリ用に特殊化したものが表されます。これには、アプリのエントリ ポイントが含まれます。 `App.xaml` にはページ固有のマークアップは含まれませんが、ユーザー インターフェイス要素のスタイルと、すべてのページからアクセスできるようにするその他の要素を、そこに追加することができます。 `.h` ファイルと `.cpp` ファイルには、さまざまなアプリケーション ライフサイクル イベント用のハンドラーが含まれています。 通常、ここには、起動時にアプリを初期化したり、中断または終了時にクリーンアップ処理を実行したりするためのカスタム コードを追加します。
- `MainPage.idl`、`MainPage.xaml`、`MainPage.h`、`MainPage.cpp`。 アプリの既定のメイン (スタートアップ) ページ型 (**MainPage** ランタイム クラス) の XAML マークアップと実装が含まれています。 **MainPage** にはナビゲーションのサポートはありませんが、開始するための既定の UI とイベント ハンドラーがいくつか提供されています。
- `pch.h` と `pch.cpp`。 これらのファイルは、プロジェクトのプリコンパイル済みのヘッダー ファイルを表します。 `pch.h` には、頻繁に変更されないヘッダー ファイルが含まれます。プロジェクト内の他のファイルに `pch.h` をインクルードします。

## <a name="a-first-look-at-the-code"></a>初めてのコード

### <a name="runtime-classes"></a>ランタイム クラス

ご存じかもしれませんが、C# で記述されたユニバーサル Windows プラットフォーム (UWP) アプリのすべてのクラスは、Windows ランタイム型です。 ただし、C++/WinRT アプリケーションで型を作成するときは、その型が Windows ランタイム型であるか、または通常の C++ クラス、構造体、列挙であるかを選択できます。

プロジェクト内のすべての XAML ページ型は、Windows ランタイム型である必要があります。 したがって、**MainPage** は Windows ランタイム型です。 具体的には、それは "*ランタイム クラス*" です。 XAML ページによって使用されるすべての型も、Windows ランタイム型である必要があります。 [Windows ランタイム コンポーネント](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)を作成していて、別のアプリから使用できる型を作成する場合は、Windows ランタイム型を作成します。 それ以外の場合の型は、標準の C++ 型でかまいません。 一般に、Windows ランタイム型は、任意の Windows ランタイム言語で使用できます。

型が Windows ランタイム型であることは、たとえば、それがインターフェイス定義言語 (`.idl`) ファイル内の [Microsoft インターフェイス定義言語 (MIDL)](/uwp/midl-3/) で定義されていることでわかります。 例として、**MainPage** を見てみましょう。

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

次に示すのは、`MainPage.h` に含まれる、**MainPage** ランタイム クラスの実装の基本的な構造と、そのアクティベーション ファクトリです。

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

指定した型に対してランタイム クラスを作成するかどうかについて詳しくは、「[C++/WinRT での API の作成](../cpp-and-winrt-apis/author-apis.md)」をご覧ください。 ランタイム クラスと IDL (`.idl` ファイル) の間の接続に関する情報については、「[XAML コントロール: C++/WinRT プロパティへのバインド](../cpp-and-winrt-apis/binding-property.md)」トピックを参照して従ってください。 そのトピックでは、新しいランタイム クラスを作成する手順が示されています。その最初のステップでは、新しい **Midl ファイル (.idl)** 項目をプロジェクトに追加します。

次に、**HelloWorldCppWinRT** プロジェクトにいくつかの機能を追加します。

## <a name="step-1-modify-your-startup-page"></a>手順 1. スタートアップ ページを変更する

**ソリューション エクスプローラー**で `MainPage.xaml` を開いて、ユーザー インターフェイス (UI) を構成するコントロールを作成できるようにします。

既に存在する **StackPanel** とその内容を削除します。 その場所に、次の XAML を貼り付けます。

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

この新しい [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) には、ユーザーの名前の入力を求める [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、ユーザーの名前を受け取る [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)、[**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)、別の **TextBlock** 要素が含まれます。

*myButton* という名前の **Button** を削除したため、コードからその参照を削除する必要があります。 そのため、`MainPage.cpp` で、**MainPage::ClickHandler** 関数内のコード行を削除します。

ここまでの作業で、ごく基本的なユニバーサル Windows アプリが作成されました。 UWP アプリがどのように表示されるかを確認するため、アプリをビルドして実行します。

![コントロールがある UWP アプリ画面](images/xaml-hw-app2.png)

アプリでは、テキスト ボックスに入力できます。 しかし、ボタンをクリックしても何も実行されません。

## <a name="step-2-add-an-event-handler"></a>手順 2. イベント ハンドラーを追加する

`MainPage.xaml` で、*inputButton* という名前の **Button** を見つけ、その [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントに対するイベント ハンドラーを宣言します。 **Button** のマークアップは、次のようになります。

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

次のようなイベント ハンドラーを実装します。

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

詳細については、[デリゲートを使用したイベントの処理](../cpp-and-winrt-apis/handle-events.md)に関するページを参照してください。

実装では、テキスト ボックスからユーザーの名前が取得され、それを使用してあいさつ文が作成されて、*greetingOutput* テキスト ブロックに表示されます。

アプリをビルドして実行します。 テキスト ボックスに名前を入力し、ボタンをクリックします。 アプリによって、個人用の案内が表示されます。

![メッセージが表示されたアプリ画面](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>手順 3. スタートアップ ページのスタイルを設定する

### <a name="choose-a-theme"></a>テーマを選択する

アプリの外観は簡単にカスタマイズできます。 既定では、明るい色のスタイルのリソースがアプリで使用されます。 システム リソースには、暗い色のテーマも含まれています。

暗い色のテーマを試すには、`App.xaml` を編集し、[**Application::RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme) に値を追加します。

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

主に画像やビデオを表示するアプリには暗いテーマを、テキストが大量に含まれるアプリには明るいテーマをお勧めします。 カスタム配色を使用する場合は、アプリの外観に最もよく合ったテーマを使用してください。

> [!NOTE]
> テーマは、アプリの起動時に適用されます。 アプリの実行中に変更することはできません。

### <a name="use-system-styles"></a>システム スタイルを使用する

このセクションでは、テキストの外観を変更します (たとえば、フォント サイズを大きくします)。

`MainPage.xaml` で、"What's your name?" **TextBlock** を見つけます。 その [**Style**](/uwp/api/windows.ui.xaml.style) プロパティに、*BaseTextBlockStyle* システム リソース キーへの参照を設定します。

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

*BaseTextBlockStyle* は、`\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml` の [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) で定義されているリソースのキーです。 次に示すのは、そのスタイルによって設定されるプロパティ値です。

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

また、`MainPage.xaml` で、`greetingOutput` という名前の **TextBlock** を見つけます。 その **Style** も *BaseTextBlockStyle* に設定します。 ここでアプリをビルドして実行すると、両方のテキスト ブロックの外観が変更されていることがわかります (たとえば、フォント サイズが大きくなっています)。

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>手順 4. 異なるウィンドウ サイズに合わせて UI が調整されるようにする

次に、ウィンドウ サイズの変化に合わせて UI が動的に調整されるにして、ディスプレイが小さいデバイスでも適切に表示されるようにします。 これを行うには、[**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) セクションを `MainPage.xaml` に追加します。 異なるウィンドウ サイズに対して異なる表示状態を定義し、それらの各表示状態に適用するようにプロパティを設定します。

### <a name="adjust-the-ui-layout"></a>UI のレイアウトを調整する

この XAML のブロックを、ルート **StackPanel** 要素の最初の子要素として追加します。

```xaml
<StackPanel ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    ...
</StackPanel>
```

アプリをビルドして実行します。 UI は、ウィンドウが 641 DIP (デバイスに依存しないピクセル) より狭いサイズになるまでは、前と同じように表示されることに注意してください。 その時点で、*narrowState* 表示状態と、その状態に対して定義されているすべてのプロパティ セッターが適用されます。

`wideState` という名前の [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) で、[**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) の [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) プロパティが 641 に設定されています。 これは、ウィンドウの幅が 641 DIP という最小値以上である場合に限って、状態が適用されることを意味します。 この状態には [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) オブジェクトが定義されていないため、XAML でページのコンテンツに対して定義されているレイアウト プロパティが使用されます。

2 つ目の [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) である `narrowState` で、[**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) の [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) プロパティが 0 に設定されています。 この状態は、ウィンドウの幅が 0 より大きく 641 DIP より小さい場合に適用されます ちょうど 641 DIP になると、`wideState` が有効になります。 `narrowState` では、いくつかの [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) オブジェクトを設定して、UI のコントロールのレイアウト プロパティを変更します。

- *contentPanel* 要素の左側の余白を 120 から 20 に減らします。
- *inputPanel* 要素の [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) を、**Horizontal** から **Vertical** に変更します。
- *inputButton* 要素に、4 DIP の上余白を追加します。

## <a name="summary"></a>要約

このチュートリアルでは、Windows ユニバーサル アプリにコンテンツを追加する方法、対話機能を追加する方法、UI の見た目を変更する方法について説明しました。