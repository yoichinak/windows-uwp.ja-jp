---
description: XAML コントロールに効果的にバインドできるプロパティは、*監視可能な*プロパティと呼ばれます。 このトピックでは、監視可能なプロパティを実装および使用する方法と、XAML コントロールをバインドする方法を示します。
title: 'XAML コントロール: C++/WinRT プロパティへのバインド'
ms.date: 06/21/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, XAML, コントロール, バインド, プロパティ
ms.localizationpriority: medium
ms.openlocfilehash: b6e663ec77c66d4a018d388da350794771312b77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154386"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML コントロール: C++/WinRT プロパティへのバインド

XAML コントロールに効果的にバインドできるプロパティは、*監視可能な*プロパティと呼ばれます。 この概念は、*オブザーバー パターン*と呼ばれるソフトウェアの設計パターンに基づいています。 このトピックでは、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) で監視可能なプロパティを実装する方法と、これらに XAML コントロールをバインドする方法を示します (背景情報については、「[データ バインディング](../data-binding/index.md)」をご覧ください)。

> [!IMPORTANT]
> C++/WinRT でランタイム クラスを使用および作成する方法についての理解をサポートするために重要な概念と用語については、「[C++/WinRT での API の使用](consume-apis.md)」と「[C++/WinRT での作成者 API](author-apis.md)」を参照してください。

## <a name="what-does-observable-mean-for-a-property"></a>プロパティの*監視可能*とはどういう意味ですか?

たとえば、**BookSku** という名前のランタイム クラスに **Title** という名前のプロパティがあるとします。 **Title** の値が変わるたびに **BookSku** が [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) イベントを発生させることを選択する場合、**Title** は監視可能なプロパティです。 そのプロパティ (あれば) のどれが監視可能かを決定するのは **BookSku** の動作 (イベントを発生させるまたは発生させない) です。

XAML テキスト要素、またはコントロールでは、更新された値を取得して、新しい値を表示するためにそれ自体を更新することで、これらのイベントをバインドし、処理することができます。

> [!NOTE]
> C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

## <a name="create-a-blank-app-bookstore"></a>空のアプリ (Bookstore) を作成する

まず、Microsoft Visual Studio で、新しいプロジェクトを作成します。 **空のアプリ (C++/WinRT)** プロジェクトを作成し、*Bookstore* と名前を付けます。 **[ソリューションとプロジェクトを同じディレクトリに配置する]** がオフになっていることを確認します。 Windows SDK の最新の一般公開された (プレビュー以外の) バージョンを対象とします。

監視可能なタイトルのプロパティを持つブックを表すための新しいクラスを作成します。 同じコンパイル ユニット内のクラスを作成および使用しています。 ただし、XAML からこのクラスへバインドできるようにしたいため、ランタイム クラスにします。 また、この作成と使用のどちらにも C++/WinRT を使用します。

新しいランタイム クラスの作成の最初の手順では、新しい **Midl ファイル (.idl)** 項目をプロジェクトに追加します。 これに `BookSku.idl` という名前をつけます。 `BookSku.idl` の既定のコンテンツを削除し、このランタイム クラスの宣言に貼り付けます。

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> 自分のビュー モデル クラス (実際に、自分のアプリケーション内で宣言したランタイム クラス) は、基底クラスから派生させる必要はありません。 上記で宣言されている **BookSku** クラスはその一例です。 このクラスではインターフェイスが実装されますが、これは基底クラスから派生したものではありません。
>
> アプリケーション内で宣言したランタイム クラスのうち、基底クラスから派生*した*ものは、"*構成可能*" クラスと呼ばれます。 また、構成可能クラスに関しては制約があります。 Visual Studio および Microsoft Store が送信を検証するために使用する [Windows App Certification Kit](../debug-test-perf/windows-app-certification-kit.md) テストにアプリケーションが合格するには (そして結果的に、アプリケーションが Microsoft Store に正常に取り込まれるには)、構成可能クラスを最終的に Windows 基底クラスから派生させる必要があります。 つまり、継承階層の最上位にあるクラスは、Windows.* 名前空間から取得された型である必要があります。 基底クラスからランタイム クラスを派生させる必要がある場合 (たとえば、派生元のご自分のすべてのビュー モデルに対して **BindableBase** クラスを実装するために)、[**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) から派生させることができます。
>
> ビュー モデルはビューを抽象化したものであるため、ビュー (XAML マークアップ) に直接バインドされています。 データ モデルはデータを抽象化したものであり、自分のビュー モデルからのみ使用され、XAML に直接バインドされることはありません。 そのため、自分のデータ モデルをランタイム クラスとしてではなく、C++ の構造体またはクラスとして宣言することができます。 それらを MIDL 内で宣言する必要はなく、任意の継承階層を自由に使用することができます。

ファイルを保存し、プロジェクトをビルドします。 ビルド プロセス中に、`midl.exe` ツールが実行されて、ランタイム クラスを記述する Windows ランタイム メタデータ ファイル (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) が作成されます。 次に、`cppwinrt.exe` ツールが実行され、ランタイム クラスの作成と使用をサポートするソース コード ファイルが生成されます。 これらのファイルには、IDL で宣言した **BookSku** ランタイム クラスの実装を開始するためのスタブが含まれています。 これらのスタブは `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` と `BookSku.cpp` です。

プロジェクト ノードを右クリックし、 **[エクスプローラーでフォルダーを開く]** をクリックします。 これにより、エクスプローラーでプロジェクト フォルダーが開きます。 そこで、スタブ ファイル `BookSku.h` および `BookSku.cpp` をフォルダー `\Bookstore\Bookstore\Generated Files\sources\` からプロジェクト フォルダー `\Bookstore\Bookstore\` にコピーします。 **ソリューション エクスプローラー**で、プロジェクト ノードを選択した状態で、 **[すべてのファイルを表示]** がオンであることを確認します。 コピーしたスタブ ファイルを右クリックし、 **[プロジェクトに含める]** をクリックします。

## <a name="implement-booksku"></a>**BookSku** を実装する
ここで、`\Bookstore\Bookstore\BookSku.h` と `BookSku.cpp` を開いてランタイム クラスを実装してみましょう。 `BookSku.h` で、次の変更を行います。

- [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 値を受け取るコンストラクターを追加します。 この値はタイトル文字列です。
- このタイトル文字列を格納するプライベート メンバーを追加します。
- タイトルが変更されたときに発生させるイベント用に、別のプライベート メンバーを追加します。

これらの変更を行うと、`BookSku.h` は次のようになります。

```cppwinrt
// BookSku.h
#pragma once
#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

`BookSku.cpp` では、次のように関数を実装します。

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"
#include "BookSku.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

**Title** ミューテーター関数では、現在の値と異なる値が設定されているかどうかを確認します。 そのようになっている場合は、タイトルを更新して、変更されたプロパティの名前に等しい引数で [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) イベントも発生させます。 これにより、ユーザー インターフェイス (UI) はどのプロパティの値を再クエリするのか分かるようになります。

## <a name="declare-and-implement-bookstoreviewmodel"></a>**BookstoreViewModel** を宣言および実装する
メイン XAML ページが、メイン ビュー モデルにバインドします。 またそのビュー モデルは、**BookSku** 型のいずれかを含む、いくつかのプロパティを持つようになります。 この手順では、メイン ビュー モデル ランタイム クラスを宣言および実装します。

`BookstoreViewModel.idl` という名前の新しい **Midl ファイル (.idl)** 項目を追加します。 ただし、「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」も参照してください。

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

保存してビルドします。 `Generated Files\sources` フォルダーから `BookstoreViewModel.h` と `BookstoreViewModel.cpp` をプロジェクト フォルダーにコピーし、プロジェクトに追加します。 これらのファイルを開き、下に示すようにランタイム クラスを実装します。 `BookstoreViewModel.h` で、**BookSku** の実装型 (**winrt::Bookstore::implementation::BookSku**) を宣言する `BookSku.h` をインクルードする方法に注意してください。 そして、既定のコンストラクターから、`= default` を削除します。

```cppwinrt
// BookstoreViewModel.h
#pragma once
#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"
#include "BookstoreViewModel.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> `m_bookSku` の型は投影型 (**winrt::Bookstore::BookSku**) であり、[**winrt::make**](/uwp/cpp-ref-for-winrt/make) で使用するテンプレート パラメーターは実装型 (**winrt::Bookstore::implementation::BookSku**) です。 この場合も、 **[make]** は投影型のインスタンスを返します。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>**BookstoreViewModel** 型のプロパティを **MainPage** に追加する
`MainPage.idl` を開きます。ここでメインの UI ページを表すランタイム クラスを宣言しています。 インポート ステートメントを追加して `BookstoreViewModel.idl` をインポートし、**BookstoreViewModel** 型の MainViewModel という名前の読み取り専用プロパティを追加します。 **MyProperty** プロパティも削除します。 以下のリスト内の `import` ディレクティブにも注意してください。

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

ファイルを保存します。 現時点ではプロジェクトは完成までビルドされませんが、ここでビルドすることは有用です。それにより、**MainPage** ランタイム クラスを実装するソース コード ファイル (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` および `MainPage.cpp`) が再生成されるからです。 それでは、今すぐビルドしてみましょう。 この段階で表示されることが予想されるビルド エラーは、 **'MainViewModel': は 'winrt::Bookstore::implementation::MainPage' のメンバーではありません**というものです。

`BookstoreViewModel.idl` のインクルードを省略した場合 (上記の `MainPage.idl` のリストを参照)、エラー **expecting \< near "MainViewModel"** が表示されます。 もう 1 つのヒントは、同じ名前空間 (コード リストに表示されている名前空間) 内にすべての型が残されているか確認することです。

表示されると予想されるエラーを解決するには、生成されたファイル (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` および `MainPage.cpp`) から **MainViewModel** プロパティ用のアクセサ スタブを `\Bookstore\Bookstore\MainPage.h` および `MainPage.cpp` にコピーする必要があります。 その手順は次のとおりです。

`\Bookstore\Bookstore\MainPage.h` で、**BookstoreViewModel** の実装型 (**winrt::Bookstore::implementation::BookstoreViewModel**) を宣言する `BookstoreViewModel.h` をインクルードします。 ビュー モデルを格納するプライベート メンバーを追加します。 プロパティ アクセサー関数 (およびメンバー m_mainViewModel) は、**BookstoreViewModel** の投影型 (**Bookstore::BookstoreViewModel**) について実装されていることに注意してください。 実装型はアプリケーションと同じプロジェクト (コンパイル ユニット) にあるため、**std::nullptr_t** を使うコンストラクターのオーバーロードによって m_mainViewModel を作成します。 **MyProperty** プロパティも削除します。

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

`\Bookstore\Bookstore\MainPage.cpp` で、下記のリストに示すように、次の変更を行います。 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) を (**BookstoreViewModel** 実装型で) 呼び出して、投影型 **BookstoreViewModel** の新しいインスタンスを *m_mainViewModel* に割り当てます。 前に説明したように、**BookstoreViewModel** コンストラクターでは、新しい **BookSku** オブジェクトがプライベート データ メンバーとして作成され、そのタイトルは最初に `L"Atticus"` に設定されます。 ボタンのイベント ハンドラー (**ClickHandler**) では、本のタイトルを公開されたタイトルに更新します。 そして最後に、**MainViewModel** プロパティのアクセサーを実装します。 **MyProperty** プロパティも削除します。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>**Title** プロパティにボタンをバインドする
メイン UI ページの XAML マークアップが含まれている `MainPage.xaml` を開きます。 下記のリストに示すように、ボタンから名前を削除し、その **Content** プロパティ値を文字式からバインド式に変更します。 バインド式の `Mode=OneWay` プロパティをメモします (ビュー モデルから UI へ一方向)。 そのプロパティが無いと、UI はプロパティの変更イベントに応答しません。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

ここでプロジェクトをビルドして実行します。 ボタンをクリックして**クリック** イベント ハンドラーを実行します。 そのハンドラーがブックのタイトル ミューテーター関数を呼び出します。そのミューテーターがイベントを発生させるので、UI は **Title** プロパティが変更されたことがわかります。ボタンはそのプロパティの値を再クエリして、そのボタンの **Content** 値を更新します。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>C++/WinRT で {Binding} マークアップ拡張を使用する
現在リリースされている C++/WinRT のバージョンの場合、{Binding} マークアップ拡張を使用できるようにするには、[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) および [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) インターフェイスを実装する必要があります。

## <a name="element-to-element-binding"></a>要素間のバインド

ある XAML 要素のプロパティを、別の XAML 要素のプロパティにバインドできます。 マークアップでどのようになるかを示す例を以下に示します。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

Midl ファイル (.idl) で読み取り専用プロパティとして名前付き XAML エンティティ `myTextBox` を宣言する必要があります。

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

これが必要な理由を次に示します。 XAML コンパイラで検証する必要があるすべての型は ([{X:bind}](../xaml-platform/x-bind-markup-extension.md) で使われるものを含みます)、Windows メタデータ (WinMD) から読み取られます。 開発者が行う必要があるのは、読み取り専用のプロパティを Midl ファイルに追加することだけです。 自動的に生成される XAML コードビハインドで実装が提供されるので、実装は行わないでください。

## <a name="consuming-objects-from-xaml-markup"></a>XAML マークアップからのオブジェクトの使用

XAML [ **{x:Bind} マークアップ拡張**](../xaml-platform/x-bind-markup-extension.md)の使用によって使用されるすべてのエンティティは、IDL で公開されている必要があります。 さらに、XAML マークアップに、マークアップにも含まれる別の要素への参照が含まれている場合は、そのマークアップの getter が IDL に存在する必要があります。

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

*ChangeColorButton* 要素は、バインディングによって *UseCustomColorCheckBox* 要素を参照します。 したがって、このページの IDL は、バインディングにアクセスできるようにするためには *UseCustomColorCheckBox* という名前の読み取り専用プロパティを宣言する必要があります。

*UseCustomColorCheckBox* の Click イベント ハンドラー デリゲートは従来の XAML デリゲート構文を使用するため、IDL のエントリは必要ありません。実装クラスでパブリックにする必要があるだけです。 一方、*ChangeColorButton* には `{x:Bind}` Click イベント ハンドラーもあり、これは IDL に移動する必要もあります。

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

**UseCustomColorCheckBox** プロパティの実装を指定する必要はありません。 これは XAML コード ジェネレーターによって行われます。

### <a name="binding-to-boolean"></a>ブール値へのバインド

これは診断モードで行うことができます。

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

これにより C++/CX では `true` または `false` が表示されますが、C++/WinRT では **Windows.Foundation.IReference`1<Boolean>** が表示されます。

ブール値にバインドするときは、`x:Bind` を使用します。

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>重要な API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 関数テンプレート](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT で API を使用する](consume-apis.md)
* [C++/WinRT で API を作成する](author-apis.md)