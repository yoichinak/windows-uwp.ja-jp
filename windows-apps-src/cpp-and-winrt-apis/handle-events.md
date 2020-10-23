---
description: このトピックでは、C++/WinRT を使用したイベント処理デリゲートの登録方法と取り消し方法について説明します。
title: C++/WinRT でのデリゲートを使用したイベントの処理
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、プロジェクション、処理、イベント、デリゲート
ms.localizationpriority: medium
ms.openlocfilehash: 884f61e877b1d7ff9f5c4567dfc329d59610b773
ms.sourcegitcommit: 14e79119aacc75382de9940fb5abaf7a618ad843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/20/2020
ms.locfileid: "92210606"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>C++/WinRT でのデリゲートを使用したイベントの処理

このトピックでは、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) を使用したイベント処理デリゲートの登録方法と取り消し方法について説明します。 標準的な C++ 関数のようなオブジェクトを使用してイベントを処理できます。

> [!NOTE]
> C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>Visual Studio 2019 を使用してイベント ハンドラーを追加する

プロジェクトにイベント ハンドラーを追加する便利な方法は、Visual Studio 2019 の XAML デザイナーのユーザー インターフェイス (UI) を使用することです。 XAML デザイナーで XAML ページを開き、イベントを処理するコントロールを選択します。 そのコントロールのプロパティ ページで、稲妻アイコンをクリックして、そのコントロールが発生元のすべてのイベントを一覧表示します。 その後、処理するイベントをダブルクリックします (たとえば *OnClicked*)。

XAML デザイナーにより、適切なイベント ハンドラー関数のプロトタイプ (およびスタブの実装) がソース ファイルに追加され、独自の実装に置き換えることができるようになります。

> [!NOTE]
> 通常、イベント ハンドラーを Midl ファイル (`.idl`) で記述する必要はありません。 そのため、XAML デザイナーでは、Midl ファイルにイベント ハンドラー関数のプロトタイプが追加されることはありません。 `.h` ファイルと `.cpp` ファイルにだけ追加されます。

## <a name="register-a-delegate-to-handle-an-event"></a>デリゲートを登録してイベントを処理する

次に、ボタンのクリック イベントの処理例を示します。 通常、イベントを処理するメンバー関数を登録するには、次のように XAML マークアップを使用します。

```xaml
// MainPage.xaml
<Button x:Name="myButton" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.h
void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */)
{
    myButton().Content(box_value(L"Clicked"));
}
```

上記のコードは、Visual Studio の **空のアプリ (C++/WinRT)** プロジェクトから取られたものです。 コード `myButton()` により、生成されたアクセサー関数が呼び出されます。これにより、*myButton* という名前が付けられた **Button** が返されます。 その **Button** 要素の `x:Name` を変更すると、生成されるアクセサー関数の名前も変更されます。

> [!NOTE]
> この場合、イベント ソース (イベントを発生させるオブジェクト) は、*myButton* という名前の **Button** です。 そして、イベント受信側 (イベントを処理するオブジェクト) は、**MainPage** のインスタンスです。 イベント ソースとイベント受信側の有効期間の管理については、このトピックの後半で詳しく説明します。

マークアップで宣言する代わりに、イベントを処理するメンバー関数を命令を使って登録することができます。 以下のコード例からは分かりにくいかもしれませんが、[**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 呼び出しの引数は [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) デリゲートのインスタンスです。 この場合、メンバー関数へのオブジェクトとポインターを取得する **RoutedEventHandler** コンストラクター オーバーロードを使用しています。

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    myButton().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> デリゲートを登録するとき、上のコード例では (現在のオブジェクトを指す) 生の *this* ポインターが渡されます。 現在のオブジェクトに対する強い参照または弱い参照を確立する方法については、「[デリゲートとしてメンバー関数を使用する場合](weak-references.md#if-you-use-a-member-function-as-a-delegate)」をご覧ください。

静的メンバー関数を使用する例を次に示します。さらにシンプルな構文に注目します。

```cppwinrt
// MainPage.h
static void ClickHandler(
    winrt::Windows::Foundation::IInspectable const& sender,
    winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    myButton().Click( MainPage::ClickHandler );
}
void MainPage::ClickHandler(
    IInspectable const& /* sender */,
    RoutedEventArgs const& /* args */) { ... }
```

**RoutedEventHandler** の作成には他の方法もあります。 次に、[**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) のドキュメント内にある構文ブロックを示します (Web ページの右上の **[言語]** ドロップダウンから [*C++/WinRT*] を選択します)。 さまざまなコンストラクターがあることに注意してください。ラムダ、自由関数、メンバー関数へのオブジェクトとポインター (上記で使用したもの) を受け取ります。

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

また、関数呼び出し演算子の構文も確認に役立ちます。 必要なデリケートのパラメーターを通知します。 ご覧のように、この場合、関数呼び出し演算子の構文は **MainPage::ClickHandler** のパラメーターと一致します。

> [!NOTE]
> 特定のイベントについて、そのデリゲートの詳細とそのデリゲートのパラメーターを把握するには、まずイベント自体のドキュメント トピックに進みます。 例として [UIElement.KeyDown イベント](/uwp/api/windows.ui.xaml.uielement.keydown)を見てみましょう。 そのトピックにアクセスし、 **[言語]** ドロップ ダウンから [*C++ /WinRT*] を選択します。 トピックの冒頭にある構文ブロックで、これを確認します。
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> その情報から、**UIElement.KeyDown** イベント (現在のトピック) には **KeyEventHandler** のデリゲート型があることがわかります。これは、このイベント型にデリゲートを登録するときに渡す型だからです。 次は、このトピックの [KeyEventHandler デリゲート](/uwp/api/windows.ui.xaml.input.keyeventhandler)型のリンクをたどりましょう。 この構文ブロックには関数呼び出し演算子が含まれています。 また、前述のように、必要なデリケートのパラメーターを通知します。
>
>```cppwinrt
>void operator()(
>   winrt::Windows::Foundation::IInspectable const& sender,
>   winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
>```
>
>  ご覧のとおり、送信者として **IInspectable** を、引数として [KeyRoutedEventArgs クラス](/uwp/api/windows.ui.xaml.input.keyroutedeventargs)のインスタンスを受け取るようにデリゲートを宣言する必要があります。
>
> 別の例を見るために、[Popup.Closed イベント](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed)を見てみましょう。 そのデリゲート型は [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler) です。 そのため、デリゲートは送信者として **IInspectable** を、引数として **IInspectable** を受け取ります (これは **EventHandler** の型パラメーターであるため)。

イベント ハンドラーで多くの処理を行っていない場合は、メンバー関数の代わりにラムダ関数を使用できます。 以下のコード例からはわかりにくいかもしれませんが、**RoutedEventHandler** デリゲートはラムダ関数から作成されているため、前述の関数呼び出し演算子の構文ともう一度一致させる必要があります。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    myButton().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        myButton().Content(box_value(L"Clicked"));
    });
}
```

デリゲートを作成する場合にもう少し明示的に指定することができます。 たとえば、次のように渡したり、何回も使用したりする場合です。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    myButton().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>登録済みデリゲートの取り消し

デリゲートを登録すると、通常、トークンがユーザーに返されます。 その後、このトークンを使用してデリゲートを取り消すことができます。つまり、このトークンはイベントから登録解除され、イベントが再び発生しても呼び出されることはありません。

説明を簡単にするために、上記の例にはその方法を示すコードは含まれていません。 ただし、次のコード例では、トークンを構造体のプライベート データ メンバーに格納し、デストラクターの該当するハンドラーを取り消しています。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

上の例のような強参照の代わりに、弱参照をボタンに格納することができます (「[C++/WinRT の強参照と弱参照](weak-references.md)」を参照してください)。

> [!NOTE]
> イベント ソースでそのイベントが同期的に生成される場合は、ハンドラーを取り消して、それ以上イベントを受け取ることはないという確信を持つことができます。 ただし、非同期イベントの場合は、取り消し後 (特にデストラクター内で取り消す場合) でも、破棄が開始された後に実行中のイベントがオブジェクトに到着する可能性があります。 破棄の前に登録を解除する場所を見つけることで、問題が軽減される可能性があります。または、堅牢なソリューションについて、「[イベント処理デリゲートで *this* ポインターに安全にアクセスする](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)」を参照してください。

または、デリゲートを登録する場合、**winrt::auto_revoke** (型 [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t) の値) を指定して ([**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker) 型の) イベント リボーカーを要求できます。 イベント リボーカーにより、イベント ソース (イベントを発生させるオブジェクト) への弱参照が保持されます。 **event_revoker::revoke** メンバー関数を呼び出して手動で取り消すことができますが、イベント リボーカーは参照が範囲外になったときに自動的にその関数自体を呼び出します。 **revoke** 関数は、イベント ソースがまだ存在するかどうかを確認し、存在する場合は、デリケートを取り消します。 次の例では、イベント ソースを格納する必要がないため、デストラクターは必要ありません。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(
            winrt::auto_revoke,
            [this](IInspectable const& /* sender */,
            RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

次の構文ブロックは、[**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) イベントのドキュメント トピックからの抜粋です。 3 つの異なる登録と取り消し関数を示しています。 3 番目のオーバーロードから宣言する必要があるイベント リボーカーの型を正確に確認できます。 そして、同じ種類のデリゲートを、*register* と *revoke with event_revoker* の両方のオーバーロードに渡すことができます。

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> 上記のコード例では、`Button::Click_revoker` は `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>` の型エイリアスです。 同じようなパターンがすべての C++/WinRT イベントに適用されます。 各 Windows ランタイム イベントには、イベント リボーカーを返す失効関数オーバーロードがあり、そのリボーカーの型はイベント ソースのメンバーです。 そのため、別の例を挙げると、[**CoreWindow:: SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) イベントには、型 **CoreWindow::SizeChanged_revoker** の値を返す登録関数オーバーロードがあります。

ページのナビゲーションのシナリオでハンドラーの取り消しを検討します。 あるページへの移動を繰り返す場合、そのページから移動する際にハンドラーを取り消すことができます。 または、同じページ インスタンスを再使用しているときは、トークンの値を確認し、(`if (!m_token){ ... }`) がまだ設定されていない場合のみ登録します。 3 番目のオプションでは、ページにイベント リボーカーをデータ メンバーとして格納します。 このトピック後半で紹介する 4 番目のオプションでは、ラムダ関数内の*この*オブジェクトの強参照または弱参照をキャプチャします。

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>自動取り消しのデリゲートの登録が失敗する場合

デリゲートを登録するときに [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) を指定しようとして、結果が [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 例外である場合、通常それはイベント ソースで弱い参照がサポートされていないことを意味します。 たとえば、[**Windows.UI.Composition**](/uwp/api/windows.ui.composition) 名前空間ではよくあることです。 このような場合は、自動取り消し機能を使用できません。 イベント ハンドラーの手動取り消しにフォールバックする必要があります。

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>非同期アクションと非同期操作のデリゲート型

上記の例では、**RoutedEventHandler** デリゲート型を使用していますが、他にも多くのデリゲート型があります。 たとえば、非同期アクションと非同期操作 (進行状況ありとなし) には、対応するデリゲート型を必要とする完了イベントと進行状況イベントがあります。 たとえば、進行状況ありの非同期操作の進行状況イベント ([**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) を実装する任意のイベント) には、[**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler-2) のデリゲート型が必要です。 次に、ラムダ関数を使用してこの型のデリゲートを作成するコード例を示します。 この例は、[**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler-2) デリゲートの作成方法も示しています。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& /* sender */,
            RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](
            IAsyncOperationWithProgress<SyndicationFeed,
            RetrievalProgress> const& sender,
            AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

上記の "コルーチン" のコメントからも分かるように、非同期アクションと非同期操作の完了イベントでデリゲートを使用するよりも、コルーチンを使用するほうがより自然です。 詳細とコード例については、「[C++/WinRT を使用した同時実行操作と非同期操作](concurrency.md)」を参照してください。

> [!NOTE]
> 非同期アクションまたは操作に、複数の*完了ハンドラー*を実装するのは誤りです。 完了したイベントに対して 1 つのデリゲートを持つか、それを `co_await` することができます。 両方ある場合、2 つ目は失敗します。

コルーチンではなくデリゲートにこだわる場合は、もっと簡単な構文を選択できます。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>値を返すデリゲート型

一部のデリゲート型では自身に値を戻す必要があります。 たとえば、[**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler) は文字列を返します。 次に、このような型のデリゲートの作成例を示します (ラムダ関数が値を返します)。

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>イベント処理デリゲートで *this* ポインターに安全にアクセスする

オブジェクトのメンバー関数でイベントを処理する場合、あるいはオブジェクトのメンバー関数内にあるラムダ関数内からイベントを処理する場合、イベント受信側 (イベントを処理するオブジェクト) とイベント ソース (イベントを発生させるオブジェクト) の相対的な有効期間を考慮する必要があります。 詳細とコード例については、「[C++/WinRT の強参照と弱参照](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)」を参照してください。

## <a name="important-apis"></a>重要な API
* [winrt::auto_revoke_t マーカー構造体](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak 関数](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::implements::get_strong 関数](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>関連トピック
* [C++/WinRT でのイベントの作成](./author-events.md)
* [C++/WinRT を使用した同時開催操作と非同期操作](./concurrency.md)
* [C++/WinRT の強参照と弱参照](./weak-references.md)