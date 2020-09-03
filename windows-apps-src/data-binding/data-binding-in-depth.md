---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: データ バインディングの詳細
description: データ バインディングは、アプリの UI でデータを表示し、必要に応じてそのデータとの同期を保つ方法です。
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: a12fb133aed8706385480254f0371508c324107f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170096"
---
# <a name="data-binding-in-depth"></a>データ バインディングの詳細

**重要な API**

-   [ **{x:Bind} マークアップ拡張**](../xaml-platform/x-bind-markup-extension.md)
-   [**Binding クラス**](/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> このトピックでは、データ バインディングの機能について詳しく説明します。 簡潔で実用的な紹介については、「[データ バインディングの概要](data-binding-quickstart.md)」をご覧ください。

このトピックは、ユニバーサル Windows プラットフォーム (UWP) アプリケーションでのデータ バインディングに関するものです。 ここで説明する API は、[**Windows.UI.Xaml.Data** 名前空間](/uwp/api/windows.ui.xaml.data)に存在します。

データ バインディングは、アプリの UI でデータを表示し、必要に応じてそのデータとの同期を保つ方法です。 データ バインディングによって、UI の問題からデータの問題を切り離すことができるため、概念的なモデルが簡素化されると共に、アプリの読みやすさ、テストの容易性、保守容易性が向上します。

データ バインディングは、UI が最初に表示されたときにデータ ソースからの値を表示するだけの場合に使うことができ、それらの値の変化に応答するために使うことはできません。 これは "*1 回限り*" というバインディングのモードであり、実行中は変化しない値に適しています。 値を "監視" し、それらが変化したときに UI を更新するように選択することもできます。 このモードは "*一方向*" と呼ばれ、読み取り専用データに適しています。 最終的には、ユーザーが UI で値に対して行った変更が自動的にデータ ソースにプッシュバックされるように、監視と更新の両方を行うことを選択できます。 このモードは "*双方向*" と呼ばれ、読み取りと書き込みデータに適しています。 例をいくつか紹介します。

-   1 回限りのモードを使用して、[**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) を現在のユーザーの写真にバインドできます。
-   一方向モードを使用して、[**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) を、新聞セクション別にグループ化されたリアルタイムのニュース記事のコレクションにバインドできます。
-   双方向モードを使用して、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) をフォームの顧客の名前にバインドできます。

モードに関係なく、バインディングには 2 種類あり、いずれも通常は UI マークアップで宣言されます。 [{x:Bind} マークアップ拡張](../xaml-platform/x-bind-markup-extension.md)と [{Binding} マークアップ拡張](../xaml-platform/binding-markup-extension.md)のいずれを使うかを選択できます。 また、同じアプリや同じ UI 要素で、この 2 つを組み合わせて使うこともできます。 {x:Bind} は Windows 10 の新機能で、パフォーマンスが向上しています。 このトピックで説明されているすべての詳細は、特に明記していない限り、両方の種類のバインディングに適用されます。

**{x:Bind} の使い方を示すサンプル アプリ**

-   [{x:Bind} のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)。
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)。
-   [XAML UI の基本のサンプル](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)。

**{Binding} の使い方を示すサンプル アプリ**

-   [Bookstore1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10) アプリのダウンロード。
-   [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) アプリのダウンロード。

## <a name="every-binding-involves-these-pieces"></a>すべてのバインディングに関連する要素

-   *バインディング ソース*。 これは、バインディング用のデータのソースで、UI に表示する値を持つメンバーが含まれる、任意のクラスのインスタンスを使うことができます。
-   *バインディング ターゲット*。 これは、データを表示する UI の [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) の [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) です。
-   *バインディング オブジェクト*。 これは、ソースからターゲットに、および必要に応じてターゲットからソースに、データ値を転送する要素です。 バインディング オブジェクトは、XAML の読み込み時に [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) または [{Binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張から作成されます。

次のセクションでは、バインディング ソース、バインディング ターゲット、バインド オブジェクトについて詳しく見てみます。 また、以下のセクションでは、**HostViewModel** という名前のクラスに属する **NextButtonText** という名前の文字列プロパティに、ボタンのコンテンツをバインドする例へのリンクも示します。

### <a name="binding-source"></a>バインディング ソース

バインディング ソースとして使用できるクラスの非常に基本的な実装を次に示します。

[C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用している場合は、以下の C++/WinRT コード例の一覧に示すように名前が付けられた、新しい **Midl ファイル (.idl)** 項目をプロジェクトに追加します。 これらの新しいファイルの内容を、一覧に示されている [MIDL 3.0](/uwp/midl-3/intro) コードに置き換え、プロジェクトをビルドして `HostViewModel.h` と `.cpp` を生成してから、一覧に一致するように生成されたファイルにコードを追加します。 それらの生成されたファイルと、プロジェクトにそれらをコピーする方法の詳細については、「[XAML コントロール: C++/WinRT プロパティへのバインド](../cpp-and-winrt-apis/binding-property.md)」を参照してください。

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

**HostViewModel** の実装とその **NextButtonText** プロパティは、1 回限りのバインディングにのみ適しています。 ただし、一方向バインディングと双方向バインディングは非常に一般的であり、これらの種類のバインディングでは、UI はバインディング ソースのデータ値の変化に対応して自動的に更新されます。 これらの種類のバインディングが正常に動作するには、バインディング ソースをバインディング オブジェクトから "監視可能" にする必要があります。 この例では、**NextButtonText** プロパティに対して一方向または双方向バインディングを設定する場合、実行時にこのプロパティの値に対して発生するすべての変更を、バインディング オブジェクトから監視可能にする必要があります。

そのための方法の 1 つは、[**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) からバインディング ソースを表すクラスを派生させ、[**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) を通じてデータ値を公開することです。 これが、[**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) を監視可能にする方法です。 **FrameworkElements** は、そのまま利用できる優れたバインディング ソースです。

より簡単にクラスを監視可能にする方法 (および既に基底クラスがあるクラスで必要な方法) は、[**System.ComponentModel.INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged) を実装することです。 この方法は、**PropertyChanged** という名前の単一のイベントを実装するだけです。 **HostViewModel** を使った例を次に示します。

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

これで、**NextButtonText** プロパティは監視可能です。 そのプロパティへの一方向または双方向のバインディングを作成する場合 (方法については後述)、作成したバインディング オブジェクトは **PropertyChanged** イベントを受信登録します。 そのイベントが発生すると、バインディング オブジェクトのハンドラーは、変更されたプロパティの名前を含む引数を受け取ります。 このようにして、バインディング オブジェクトはどのプロパティの値が残っており、再び読み取る必要があるかを識別します。

上記のパターンを複数回実装する必要はありません。C# を使用している場合は、[QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) サンプル ("Common" フォルダー内) にある **BindableBase** 基底クラスから派生させるだけで済みます。 どのようになるかを示す例を以下に示します。

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> C++/WinRT の場合、基底クラスから派生する、アプリケーションで宣言するランタイム クラスはすべて、"*構成可能*" クラスと呼ばれます。 また、構成可能クラスに関しては制約があります。 Visual Studio および Microsoft Store が送信を検証するために使用する [Windows App Certification Kit](../debug-test-perf/windows-app-certification-kit.md) テストにアプリケーションが合格するには (そして結果的に、アプリケーションが Microsoft Store に正常に取り込まれるには)、構成可能クラスを最終的に Windows 基底クラスから派生させる必要があります。 つまり、継承階層の最上位にあるクラスは、Windows.* 名前空間から取得された型である必要があります。 基底クラスからランタイム クラスを派生させる必要がある場合 (たとえば、派生元のご自分のすべてのビュー モデルに対して **BindableBase** クラスを実装するために)、[**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject) から派生させることができます。

[**String.Empty**](/dotnet/api/system.string.empty) または **null** の引数を使って **PropertyChanged** イベントを発生させることは、オブジェクトのすべての非インデクサー プロパティを再び読み取る必要があることを示します。 特定のインデクサーの場合は "Item\[*indexer*\]" (ここで、*indexer* はインデックス値) の引数を、すべてのインデクサーの場合は "Item\[\]" の値を使って、オブジェクトのインデクサー プロパティが変更されたことを示すイベントを発生させることができます。

バインディング ソースは、プロパティにデータが含まれる単一のオブジェクト、またはオブジェクトのコレクションとして処理できます。 C# と Visual Basic コードでは、実行時に変更されないコレクションを表示する [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1) を実装するオブジェクトへの 1 回限りのバインディングを設定できます。 監視可能なコレクション (コレクションの項目の追加と削除を監視する) の場合、代わりに [**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) への一方向バインディングを設定します。 C++/CX コードでは、監視可能と監視可能でない両方のコレクションについて、[**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class) にバインドでき、C++/WinRT には独自の型があります。 独自のコレクション クラスにバインドするには、次の表をご覧ください。

|シナリオ|C# と VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|オブジェクトにバインドする。|どのオブジェクトでもかまいません。|どのオブジェクトでもかまいません。|オブジェクトで [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) を指定するか、[**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) を実装する必要があります。|
|バインドされたオブジェクトからプロパティ変更通知を取得します。|オブジェクトでは [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged) を実装する必要があります。| オブジェクトでは [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) を実装する必要があります。|オブジェクトでは [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) を実装する必要があります。|
|コレクションにバインドする。| [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1)|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)、または [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。 「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](../cpp-and-winrt-apis/binding-collection.md)」と「[C++/WinRT でのコレクション](../cpp-and-winrt-apis/collections.md)」を参照してください。| [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class)|
|バインドされたコレクションからコレクションの変更通知を取得します。|[**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)。 たとえば、[**winrt::single_threaded_observable_vector&lt;T&gt;** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) です。|[**IObservableVector&lt;T&gt;** ](/uwp/api/Windows.Foundation.Collections.IObservableVector_T_)。  [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class) でこのインターフェイスを実装します。|
|バインドをサポートするコレクションを実装する。|[  **List(Of T)** ](/dotnet/api/system.collections.generic.list-1) を拡張するか、[**IList**](/dotnet/api/system.collections.ilist)、[**IList**](/dotnet/api/system.collections.generic.ilist-1)(Of [**Object**](/dotnet/api/system.object))、[**IEnumerable**](/dotnet/api/system.collections.ienumerable)、または [**IEnumerable**](/dotnet/api/system.collections.generic.ienumerable-1)(Of **Object**) を実装します。 汎用の **IList(Of T)** と **IEnumerable(Of T)** へのバインドはサポートされていません。|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) を実装します。 「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](../cpp-and-winrt-apis/binding-collection.md)」と「[C++/WinRT でのコレクション](../cpp-and-winrt-apis/collections.md)」を参照してください。|[  **IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[**IBindableIterable**](/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable)、[**IVector**](/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[**Object**](/dotnet/api/system.object)^&gt;、[**IIterable**](/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;**Object**^&gt;、**IVector**&lt;[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt;、または **IIterable**&lt;**IInspectable**\*&gt; を実装します。 汎用の **IVector&lt;T&gt;** と **IIterable&lt;T&gt;** へのバインドはサポートされていません。|
| コレクションの変更通知をサポートするコレクションを実装します。 | [  **ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) を拡張するか、(汎用ではない) [**IList**](/dotnet/api/system.collections.ilist) と [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) を実装します。|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)、または [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) を実装します。|[  **IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) と [**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) を実装します。|
|段階的読み込みをサポートするコレクションを実装する。|[  **ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) を拡張するか、(汎用ではない) [**IList**](/dotnet/api/system.collections.ilist) と [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) を実装します。 さらに、[**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) を実装します。|[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)、または [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) を実装します。 さらに、[**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) を実装します|[  **IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector)、[**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) を実装します。|

段階的読み込みを使うと、任意の大きさのデータ ソースにリスト コントロールをバインドすると同時に、高パフォーマンスを実現できます。 たとえば、一度にすべての結果を読む込むことなく、Bing の画像クエリ結果にリスト コントロールをバインドすることができます。 この場合、すぐに読み込むのは一部の結果だけで、他の結果は必要に応じて読み込みます。 段階的読み込みをサポートするには、コレクション変更通知をサポートするデータ ソースに [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) を実装する必要があります。 データ バインディング エンジンがより多くのデータを要求する場合は、UI を更新するためにデータ ソースで適切な要求を行い、結果を統合して、適切な通知を送信する必要があります。

### <a name="binding-target"></a>バインディング ターゲット

以下の 2 つの例では、**Button.Content** プロパティはバインディング ターゲットであり、その値はバインディング オブジェクトを宣言するマークアップ拡張に設定されます。 最初に [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) を示し、次に [{Binding}](../xaml-platform/binding-markup-extension.md) を示します。 マークアップでバインディングを宣言する方法は一般的です (便利で、読みやすく、ツールで処理できます)。 ただし、必要な場合は、マークアップを使わずに、命令を使って (プログラムで) [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) クラスのインスタンスを作成できます。

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

C++/WinRT または Visual C++ コンポーネント拡張 (C++/CX) を使用している場合は、[{Binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張を使用するランタイム クラスに [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性を追加する必要があります。

> [!IMPORTANT]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用している場合、Windows SDK バージョン 10.0.17763.0 (Windows 10 バージョン 1809) 以降がインストールされていれば、[**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性を使用できます。 その属性がない場合、[{Binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張を使用できるようにするには、[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) および [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) インターフェイスを実装する必要があります。

### <a name="binding-object-declared-using-xbind"></a>{x:Bind} を使って宣言されたバインディング オブジェクト

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) マークアップを作成する前に必要な手順が 1 つあります。 マークアップのページを表すクラスからバインディング ソース クラスを公開する必要があります。 そのためには、**MainPage** ページ クラスに (この場合、型は **HostViewModel**) プロパティを追加します。

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

これが完了したら、バインディング オブジェクトを宣言するマークアップを詳しく見ていくことができます。 次の例では、前の「バインディング ターゲット」セクションで使用したものと同じ **Button.Content** バインディング ターゲットを使って、**HostViewModel.NextButtonText** プロパティにバインドされたバインディング ターゲットを示します。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

**Path** として指定している値に注意してください。 この値はページ自体のコンテキストで解釈されます。この場合、パスは、先ほど **MainPage** ページに追加した **ViewModel** プロパティを参照することによって開始されます。 このプロパティは **HostViewModel** インスタンスを返すため、そのオブジェクトにドットを付けて **HostViewModel.NextButtonText** プロパティにアクセスできます。 さらに、**Mode** を指定して、[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) の既定値である 1 回限りのバインディングをオーバーライドします。

[  **Path**](/uwp/api/windows.ui.xaml.data.binding.path) プロパティは、入れ子になったプロパティ、添付プロパティ、整数と文字列のインデクサーにバインドするためのさまざまな構文オプションをサポートしています。 詳しくは、「[Property-path 構文](../xaml-platform/property-path-syntax.md)」をご覧ください。 文字列のインデクサーにバインドすると、[**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) を実装しなくても動的プロパティにバインドする効果を得られます。 その他の設定については、「[{x:Bind} マークアップ拡張](../xaml-platform/x-bind-markup-extension.md)」をご覧ください。

**HostViewModel.NextButtonText** プロパティが実際に監視可能であることを示すには、ボタンに **Click** イベント ハンドラーを追加し、**HostViewModel.NextButtonText** の値を更新します。 ボタンをビルド、実行、およびクリックし、ボタンの **Content** 更新の値を確認します。

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text) に対する変更は、ユーザーのキーストロークのたびにではなく、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) からフォーカスが移動したときに、双方向バインディング ソースに送信されます。

**DataTemplate と x:DataType**

[  **DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) 内で (項目テンプレート、コンテンツ テンプレート、ヘッダー テンプレートのいずれとして使用される場合でも)、**Path** の値はページのコンテキストではなく、テンプレート化されたデータ オブジェクトのコンテキストで解釈されています。 {x:Bind} をデータ テンプレートで使用する場合、コンパイル時にバインディングを検証できるように (バインディング用に効率的なコードを生成できるように) するために、**DataTemplate** では、**x:DataType** を使って、データ オブジェクトの型を宣言する必要があります。 次に示す例は、**SampleDataGroup** オブジェクトのコレクションにバインドされている、項目コントロールの **ItemTemplate** として使うことができます。

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Path 内の厳密に型指定されていないオブジェクト**

たとえば、SampleDataGroup という名前の型があり、Title という名前の文字列プロパティを実装しているとします。 また、MainPage.SampleDataGroupAsObject プロパティがあり、これは型オブジェクトのプロパティですが、実際には SampleDataGroup のインスタンスを返すとします。 バインディング `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` は、型オブジェクトで Title プロパティが見つからないため、コンパイル エラーとなります。 これを解決するには、Path 構文に `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>` などのキャストを追加します。 Element がオブジェクトとして宣言されているが、実際には TextBlock である、`<TextBlock Text="{x:Bind Element.Text}"/>` という例を考えてみます。 この場合も、`<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>` のようにキャストによって問題が解決されます。

**データを非同期的に読み込む場合**

ページの部分クラスに、 **{x:Bind}** をサポートするコードがコンパイル時に生成されます。 これらのファイルは `obj` フォルダー内にあり、`<view name>.g.cs` (C# の場合) などの名前が付けられています。 生成されたコードには、ページの [**Loading**](/uwp/api/Windows.UI.Xaml.FrameworkElement) イベントのハンドラーが含まれており、このハンドラーが、ページのバインディングを表す生成されたクラスで **Initialize** メソッドを呼び出します。 次に、**Initialize** が **Update** を呼び出して、バインディング ソースとターゲットの間のデータの移動を開始します。 **Loading** は、ページまたはユーザー コントロールの最初の測定パスの直前に発生します。 したがって、データが非同期的に読み込まれる場合、**Initialize** が呼び出された時点で準備ができていない可能性があります。 そのため、データを読み込んだ後、`this.Bindings.Update();` を呼び出すことによって、1 回限りのバインディングを強制的に実行できます。 非同期的に読み込まれたデータについて 1 回限りのバインディングのみが必要な場合は、この方法でバインディングを初期化する方が、一方向のバインディングを使って変更をリッスンするよりもはるかに低コストです。 データがきめ細かく変更されない場合や、特定のアクションの一部として更新される可能性が高い場合は、バインディングを 1 回限りにし、いつでも **Update** を呼び出すことによって、強制的に手動更新を実行できます。

> [!NOTE]
> **{x:Bind}** は、JSON オブジェクトのディクショナリ構造内を移動する場合などの遅延バインディングのシナリオや、ダック タイピングには適していません。 "ダック タイピング" は、プロパティ名の語彙的な一致に基づく厳密ではない型指定です (つまり、"アヒルのように歩き、泳ぎ、鳴くならば、それはアヒルである")。 ダック タイピングでは、**Age** プロパティへのバインディングは、**Person** または **Wine** オブジェクトでも同様に満たされます (それぞれの型に **Age** プロパティがあると想定)。 これらのシナリオでは、 **{Binding}** マークアップ拡張を使用します。

### <a name="binding-object-declared-using-binding"></a>{Binding} を使って宣言されたバインディング オブジェクト

C++/WinRT または Visual C++ コンポーネント拡張 (C++/CX) を使っている場合に、[{Binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張を使用するには、バインド先のランタイム クラスに [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性を追加する必要があります。 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) を使用するために、その属性は必要ありません。

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用している場合、Windows SDK バージョン 10.0.17763.0 (Windows 10 バージョン 1809) 以降がインストールされていれば、[**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性を使用できます。 その属性がない場合、[{Binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張を使用できるようにするには、[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) および [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) インターフェイスを実装する必要があります。

[{Binding}](../xaml-platform/binding-markup-extension.md) は、既定で、マークアップ ページの [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) にバインドしていることを前提としています。 したがって、ページの **DataContext** を、バインディング ソース クラス (ここでは **HostViewModel** 型) のインスタンスに設定します。 次の例は、バインディング オブジェクトを宣言するマークアップを示しています。 前の「バインディング ターゲット」セクションで使用したものと同じ **Button.Content** バインディング ターゲットを使っており、**HostViewModel.NextButtonText** プロパティにバインドします。

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

**Path** として指定している値に注意してください。 この値は、ページの [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) で解釈されます。この例では、**HostViewModel** のインスタンスに設定されます。 パスは **HostViewModel.NextButtonText** プロパティを参照します。 [{Binding}](../xaml-platform/binding-markup-extension.md) の既定値である一方向のバインディングが適切であるため、**Mode** は省略できます。

UI 要素の [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) の既定値は、その親から継承された値です。 もちろん **DataContext** を明示的に設定することによってこの既定値 (さらに既定で子に継承される) をオーバーライドすることができます。 要素で明示的に **DataContext** を設定することは、同じソースを使う複数のバインディングが必要な場合に便利です。

バインディング オブジェクトには **Source** プロパティがあり、その既定値はバインディングが宣言されている UI 要素の [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) です。 この既定値をオーバーライドするには、バインディングで **Source**、**RelativeSource**、または **ElementName** を明示的に設定します (詳しくは、「[{Binding}](../xaml-platform/binding-markup-extension.md)」をご覧ください)。

[**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) 内では、[**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) はテンプレート化されるデータ オブジェクトに自動的に設定されます。 次の例は、**Title** と **Description** という名前の文字列プロパティを持つ任意の型のコレクションにバインドされている、項目コントロールの **ItemTemplate** として使うことができます

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> 既定では、[**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text) に対する変更は、[**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) からフォーカスが移動したときに、双方向バインディング ソースに送信されます。 変更をユーザーの各キーストロークの後に送信するには、マークアップのバインディングで **UpdateSourceTrigger** を **PropertyChanged** に設定します。 **UpdateSourceTrigger** を **Explicit** に設定することによって、変更が送信されるタイミングを完全に制御することもできます。 次に、テキスト ボックスでのイベント (通常 [**TextBox.TextChanged**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)) を処理し、ターゲットで [**GetBindingExpression**](/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression) を呼び出して [**BindingExpression**](/uwp/api/windows.ui.xaml.data.bindingexpression) オブジェクトを取得します。最後に、[**BindingExpression.UpdateSource**](/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource) を呼び出して、データ ソースをプログラムで更新します。

[  **Path**](/uwp/api/windows.ui.xaml.data.binding.path) プロパティは、入れ子になったプロパティ、添付プロパティ、整数と文字列のインデクサーにバインドするためのさまざまな構文オプションをサポートしています。 詳しくは、「[Property-path 構文](../xaml-platform/property-path-syntax.md)」をご覧ください。 文字列のインデクサーにバインドすると、[**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) を実装しなくても動的プロパティにバインドする効果を得られます。 [  **ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) プロパティは要素間のバインディングに便利です。 [  **RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) プロパティにはいくつかの用途があり、そのうちの 1 つが、[**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 内でバインディングをテンプレート化するためのより強力な方法としての用途です。 その他の設定については、「[{Binding} マークアップ拡張](../xaml-platform/binding-markup-extension.md)」と [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) クラスの説明をご覧ください。

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>ソースとターゲットが同じ型ではない場合

ブール型プロパティの値に基づいて UI 要素の表示を制御する場合、数値の範囲や傾向に応じた色で UI 要素をレンダリングする場合、または文字列を想定している UI 要素のプロパティに日付や時刻の値を表示する場合は、値の型を別の型に変換する必要があります。 バインディング ソース クラスから適切な型の別のプロパティを公開し、変換ロジックをカプセル化し、その中でテストできるようにしておくことが、適切なソリューションである場合があります。 ただし、この方法はソースとターゲットのプロパティの数が多くなり、組み合わせが多くなると、柔軟性も拡張性もありません。 その場合は、いくつかのオプションがあります。

* {X:Bind} を使用している場合、関数に直接バインドして変換を行うことができます
* または、変換を実行するためのオブジェクトである、値コンバーターを指定することができます 

## <a name="value-converters"></a>値コンバーター

[  **DateTime**](/dotnet/api/system.datetime) 値を月を含む文字列値に変換する、1 回限りまたは一方向のバインディングに適した値コンバーターを以下に示します。 このクラスは、[**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) を実装します。

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

次に、バインディング オブジェクトのマークアップでその値コンバーターを利用する方法を示します。

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

バインドの [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) パラメーターを定義した場合、[**Convert**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) メソッドと [**ConvertBack**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) メソッドがバインド エンジンによって呼び出されます。 ソースからデータが渡されると、バインド エンジンは、**Convert** を呼び出し、返すデータをターゲットに渡します。 ターゲットからデータが渡されると (双方向バインディングの場合)、バインド エンジンは、**ConvertBack** を呼び出し、返すデータをソースに渡します。

コンバーターには省略可能なパラメーターもあります。変換で使う言語を指定できる [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage)、および変換ロジックにパラメーターを渡せるようにする、[**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter) です。 コンバーター パラメーターの使用例については、「[**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter)」をご覧ください。

> [!NOTE]
> 変換でエラーが発生した場合は、例外をスローしないでください。 代わりに [**DependencyProperty.UnsetValue**](/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue) を返します。これにより、データ転送が中止されます。

バインディング ソースを解決できない場合に使用する既定値を表示するには、マークアップのバインディング オブジェクトで **FallbackValue** プロパティを設定します。 これは、変換エラーや書式エラーを処理する場合に便利です。 また、バインド時にソースのプロパティが、型が混在するバインド先のコレクションのどのオブジェクトにも見つからないときにも便利です。

テキスト コントロールを文字列でない値にバインドした場合、データ バインディング エンジンは値を文字列に変換します。 値が参照型である場合、データ バインディング エンジンは [**ICustomPropertyProvider.GetStringRepresentation**](/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) または [**IStringable.ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) を呼び出すことで文字列の値を取得します。取得できない場合は、[**Object.ToString**](/dotnet/api/system.object.tostring#System_Object_ToString) を呼び出します。 ただし、データ バインディング エンジンは基底クラスの実装を隠ぺいする **ToString** の実装を無視することに注意してください。 代わりに、サブクラスの実装で基底クラスの **ToString** メソッドをオーバーライドする必要があります。 同様に、ネイティブ言語では、すべてのマネージ オブジェクトが [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) と [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable) を実装しているように見えます。 ただし、**GetStringRepresentation** と **IStringable.ToString** のすべての呼び出しは、**Object.ToString** またはそのオーバーライドされたメソッドに割り振られます。基底クラスの実装を隠ぺいする新しい **ToString** メソッドの実装に割り振られることはありません。

> [!NOTE]
> Windows 10、バージョン1607 以降では、XAML フレームワークにブール値と Visibility 値のコンバーターが組み込まれています。 コンバーターは、**Visible** 列挙値に対して **true** を、**Collapsed** に対して **false** をマッピングします。これにより、コンバーターを作成せずに Visibility プロパティをブール値にバインドできます。 組み込みのコンバーターを使用するには、アプリの最小のターゲット SDK バージョンが 14393 以降である必要があります。 アプリがそれよりも前のバージョンの Windows 10 をターゲットとしている場合は使うことができません。 ターゲット バージョンについて詳しくは、「[バージョン アダプティブ コード](../debug-test-perf/version-adaptive-code.md)」をご覧ください。

## <a name="function-binding-in-xbind"></a>{X:Bind} の関数バインド

{x:Bind} は関数となるバインディング パスの最終ステップを有効化します。 これを使って変換を実行できます。また 1 つ以上のプロパティに依存するバインディングを実行できます。 「[**x:Bind の関数**](function-bindings.md)」を参照してください

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>要素間のバインド

ある XAML 要素のプロパティを、別の XAML 要素のプロパティにバインドできます。 マークアップでどのようになるかを示す例を以下に示します。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> C++/WinRT を使用する要素間のバインドに必要なワークフローについては、「[要素間のバインド](../cpp-and-winrt-apis/binding-property.md#element-to-element-binding)」を参照してください。

## <a name="resource-dictionaries-with-xbind"></a>リソース ディクショナリと {x:Bind}

[{x:Bind} マークアップ拡張](../xaml-platform/x-bind-markup-extension.md)はコードの生成に依存するため、**InitializeComponent** (生成されたコードを初期化する) を呼び出すコンストラクターを含むコード ビハインド ファイルが必要です。 ファイル名を参照する代わりに、その型をインスタンス化する (**InitializeComponent** が呼び出される) ことによって、リソース ファイルを再利用します。 次の例では、既存のリソース ディクショナリがあり、その中で {x:Bind} を使う場合の対処方法を示します。

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>イベント バインディングと ICommand

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) はイベント バインディングと呼ばれる機能をサポートしています。 この機能によって、バインディングを使用するイベントのハンドラーを指定できます。これは、コード ビハインド ファイルのメソッドによるイベント処理に対する追加のオプションです。 たとえば、**MainPage** クラスに **RootFrame** プロパティがあるとします。

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

次のように、**RootFrame** プロパティによって返される **Frame** オブジェクトのメソッドに、ボタンの **Click** イベントをバインドできます。 また、ボタンの **IsEnabled** プロパティを、同じ **Frame** の別のメンバーにもバインドします。

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

この方法では、オーバーロードされたメソッドを使ってイベントを処理することはできません。 また、イベントを処理するメソッドにパラメーターがある場合、すべてのパラメーターがそれぞれ、イベントのすべての型から代入できる必要があります。 この場合、[**Frame.GoForward**](/uwp/api/windows.ui.xaml.controls.frame.goforward) はオーバーロードされておらず、パラメーターはありません (ただし、2 つの **object** パラメーターを取る場合でも有効です)。 しかし、[**Frame.GoBack**](/uwp/api/windows.ui.xaml.controls.frame.goback) はオーバーロードされているため、この手法でそのメソッドを使うことはできません。

イベント バインディングの手法は、コマンドの実装と使用に似ています (コマンドは [**ICommand**](/uwp/api/windows.ui.xaml.input.icommand) インターフェイスを実装するオブジェクトを返すプロパティです)。 [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) と [{Binding}](../xaml-platform/binding-markup-extension.md) はいずれもコマンドで動作します。 コマンド パターンを何度も実装する必要はありません。[QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) サンプル (Common フォルダー) に含まれている **DelegateCommand** ヘルパー クラスを使うことができます。

## <a name="binding-to-a-collection-of-folders-or-files"></a>フォルダーやファイルのコレクションへのバインド

[  **Windows.Storage**](/uwp/api/Windows.Storage) 名前空間の API を使って、フォルダーとファイルのデータを取得できます。 ただし、**GetFilesAsync** メソッド、**GetFoldersAsync** メソッド、**GetItemsAsync** メソッドは、リスト コントロールへのバインドに適した値を返さないことがあります。 代わりに、[**FileInformationFactory**](/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory) クラスの [**GetVirtualizedFilesVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector) メソッド、[**GetVirtualizedFoldersVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector) メソッド、[**GetVirtualizedItemsVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector) メソッドの戻り値にバインドする必要があります。 [StorageDataSource と GetVirtualizedFilesVector のサンプルに関するページ](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/StorageDataSource%20and%20GetVirtualizedFilesVector%20sample)から抜粋した次のコード例は、一般的な使用パターンを示しています。 アプリ パッケージ マニフェストで **picturesLibrary** 機能を宣言し、ピクチャ ライブラリ フォルダーにピクチャがあることを確認します。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

通常はこの方法で、ファイルとフォルダーの情報の読み取り専用ビューを作成します。 たとえば、ユーザーが音楽ビューで曲を評価できるように、ファイルとフォルダーのプロパティへの双方向のバインドを作成できます。 ただし、適切な **SavePropertiesAsync** メソッド ([**MusicProperties.SavePropertiesAsync**](/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync) など) を呼び出すまで、変更は永続化されません。 項目からフォーカスが移動したときに選択のリセットがトリガーされるため、変更をコミットする必要があります。

この方法による双方向のバインドは、ミュージックなど、インデックス化された場所でのみ機能します。 ある場所がインデックス化されているかどうかを確認するには、[**FolderInformation.GetIndexedStateAsync**](/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync) メソッドを呼び出します。

仮想化されたベクターは、一部の項目に対して、値の設定の前に **null** を返す場合があることにも注意してください。 たとえば、仮想化されたベクターにバインドされたリスト コントロールの [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 値を使う前に、**null** をチェックする必要があります。または、代わりに [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) を使います。

## <a name="binding-to-data-grouped-by-a-key"></a>キーでグループ化されたデータへのバインド

フラットな項目のコレクション (たとえば、**BookSku** クラスで表される書籍) があり、共通のプロパティ (たとえば、**BookSku.AuthorName** プロパティ) をキーとして使って項目をグループ化する場合、結果はグループ化されたデータと呼ばれます。 データをグループ化すると、フラットなコレクションではなくなります。 グループ化されたデータはグループ オブジェクトのコレクションであり、各グループ オブジェクトには、

- キー、および
- プロパティがそのキーと一致する項目のコレクションがあります。

再び書籍の例で説明すると、書籍を著者名でグループ化した結果は、著者名グループのコレクションになり、各グループには、

- キー (著者名)、および
- **AuthorName** プロパティがグループのキーと一致する **BookSku** のコレクションが含まれます。

一般的に、コレクションを表示するには、項目コントロール [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) ([**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) や [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) など) を、コレクションを返すプロパティに直接バインドします。 項目のフラットなコレクションの場合は、何も特別なことをする必要はありません。 一方、グループ オブジェクトのコレクションの場合 (グループ化されたデータにバインドしている場合など) は、項目コントロールとバインディング ソースの間に存在する、[**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) と呼ばれる中間オブジェクトのサービスが必要です。 グループ化されたデータを返すプロパティに **CollectionViewSource** をバインドし、項目コントロールを **CollectionViewSource** にバンドします。 **CollectionViewSource** の追加の付加価値として現在の項目を追跡できるため、複数の項目コントロールをすべて同じ **CollectionViewSource** にバインドすることによって同期させることができます。 [**CollectionViewSource.View**](/uwp/api/windows.ui.xaml.data.collectionviewsource.view) プロパティによって返されるオブジェクトの [**ICollectionView.CurrentItem**](/uwp/api/windows.ui.xaml.data.icollectionview.currentitem) プロパティによって、現在の項目にプログラムでアクセスすることもできます。

[  **CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) のグループ化機能をアクティブにするには、[**IsSourceGrouped**](/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped) を **true** に設定します。 [  **ItemsPath**](/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath) プロパティも設定する必要があるかどうかは、グループ オブジェクトを作成する方法に依存します。 グループ オブジェクトを作成するには、"グループである" パターンと "グループを保持する" パターンの 2 つの方法があります。 "グループである" パターンでは、グループ オブジェクトはコレクション型から派生 (たとえば、**List&lt;T&gt;** ) から派生するため、グループ オブジェクトは実際にそれ自体が項目のグループです。 このパターンでは、**ItemsPath** を設定する必要はありません。 "グループを保持する" パターンでは、グループ オブジェクトはコレクション型 (**List&lt;T&gt;** など) の 1 つまたは複数のプロパティを持つため、グループは、プロパティの形式で項目のグループ (または複数のプロパティの形式で項目の複数のグループ) を "保持" します。 このパターンでは、**ItemsPath** を、項目のグループを含むプロパティの名前に設定する必要があります。

次の例は、"グループを保持する" パターンを示しています。 ページ クラスには [**ViewModel**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) という名前のプロパティがあります。このプロパティはビュー モデルのインスタンスを返します。 [  **CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) はビュー モデルの **Authors** プロパティにバインドされ (**Authors** はグループ オブジェクトのコレクション)、それがグループ化された項目を格納する **Author.BookSkus** プロパティであることも指定します。 最後に、[**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) は **CollectionViewSource** にバインドされ、グループ内の項目をレンダリングできるようにグループのスタイルが定義されています。

```xaml
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

"グループである" パターンは、2 つの方法のいずれかで実装できます。 1 つは、独自のグループ クラスを作成する方法です。 **List&lt;T&gt;** からクラスを派生させます (ここで *T* は項目の型です)。 たとえば、`public class Author : List<BookSku>` のように指定します。 もう 1 つは、**BookSku** 項目の同様のプロパティ値から、動的にグループ オブジェクト (とグループ クラス) を動的に作成する [LINQ](/previous-versions/bb397926(v=vs.140)) 式を使う方法です。 このアプローチ (項目のフラットな一覧のみを保持し、必要に応じてグループ化する) は、クラウド サービスのデータにアクセスするアプリで一般的です。 著者やジャンルなどに基づいて書籍を柔軟にグループ化することができます。**Author** や **Genre** などの特別なグループ クラスは必要ありません。

次の例は、[LINQ](/previous-versions/bb397926(v=vs.140)) を使用した "グループである" パターンを示しています。 今回は、書籍をジャンルでグループ化し、グループ ヘッダーにジャンル名と共に表示します。 これは、グループの [**Key**](/dotnet/api/system.linq.igrouping-2.key#System_Linq_IGrouping_2_Key) の値に関連する "Key" プロパティ パスによって示されます。

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) をデータ テンプレートと共に使う場合、**x:DataType** 値を設定することによって、バインド先の型を指定する必要があることに注意してください。 型がジェネリックである場合、マークアップでは表現できないため、グループ スタイル ヘッダー テンプレート内で代わりに [{Binding}](../xaml-platform/binding-markup-extension.md) を使う必要があります。

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

[  **SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) コントロールは、ユーザーがグループ化されたデータを表示したり、データ間を移動したりするための優れた方法です。 [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) サンプル アプリは、**SemanticZoom** の使い方を示しています。 このアプリでは、著者別にグループ化された書籍の一覧を表示する (拡大表示ビュー) ことも、縮小して著者のジャンプ リストを表示する (縮小表示ビュー) こともできます。 ジャンプ リストを使うと、書籍の一覧をスクロールするよりもすばやく移動することができます。 拡大表示ビューと縮小表示ビューは、実際には、同じ **CollectionViewSource** にバインドされた **ListView** または **GridView** コントロールです。

![SemanticZoom の図](images/sezo.png)

階層データ (カテゴリ内のサブカテゴリなど) にバインドする場合、一連の項目コントロールを使って、UI に階層レベルを表示できます。 1 つの項目コントロールで項目を選ぶと、後続する項目コントロールの内容に反映されます。 各リストをそれぞれの [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) インスタンスにバインドし、**CollectionViewSource** インスタンスをチェーン形式でバインドすることで、これらのリストの同期を保つことができます。 これは、マスター/詳細 (またはリスト/詳細) ビューと呼ばれます。 詳しくは、「[階層データにバインドし、マスター/詳細ビューを作る方法](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md)」をご覧ください。

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>データ バインディングに関する問題の診断とデバッグ

バインド マークアップには、プロパティ (と、C# では場合によってフィールドとメソッド) の名前が含まれています。 したがって、プロパティの名前を変更するときに、それを参照するすべてのバインディングを変更する必要があります。 この処理を忘れることは、データ バインディングのバグの一般的な例であり、アプリは正しくコンパイルまたは実行されません。

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) と [{Binding}](../xaml-platform/binding-markup-extension.md) によって作成されたバインディング オブジェクトは、ほとんど機能的に同等です。 ただし、{x:Bind} にはバインディング ソースの型情報が含まれ、コンパイル時にソース コードが生成されます。 {x:Bind} を使うと、コードの残りの部分で取得できるものと同じ種類の問題の検出を行うことができます。 これには、コンパイル時のバインド式の検証や、ページの部分クラスとして生成されたソース コード内でのブレークポイント設定によるデバッグが含まれます。 これらのクラスは `obj` フォルダー内のファイルにあり、`<view name>.g.cs` (C# の場合) などの名前が付けられています。 バインディングに問題がある場合は、Microsoft Visual Studio デバッガーで、 **[処理されない例外で中断]** をオンにします。 デバッガーはその時点での実行を中断し、問題のある点をデバッグすることができます。 {x:Bind} によって生成されたコードは、バインディング ソース ノードのグラフの各部分で同じパターンに従うため、この情報を **[コール スタック]** ウィンドウで使って、問題の原因となった呼び出しのシーケンスを特定できます。

[{Binding}](../xaml-platform/binding-markup-extension.md) には、バインディング ソースの型情報はありません。 ただし、デバッガーがアタッチされたアプリを実行すると、バインド エラーがある場合は Visual Studio の **[出力]** ウィンドウにそのエラーが表示されます。

## <a name="creating-bindings-in-code"></a>コードでのバインドの作成

**注**  このセクションは [{Binding}](../xaml-platform/binding-markup-extension.md) にのみ適用されます。これは、コードで [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) バインディングを作成できないためです。 ただし、{x:Bind} と同じメリットを、[**DependencyObject.RegisterPropertyChangedCallback**](/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) によって実現できます。これにより、すべての依存関係プロパティについての変更通知を登録できます。

XAML の代わりに手続き型コードを使っても UI 要素をデータに接続できます。 そのためには、新しい [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) オブジェクトを作成し、適切なプロパティを設定してから、[**FrameworkElement.SetBinding**](/uwp/api/windows.ui.xaml.frameworkelement.setbinding) または [**BindingOperations.SetBinding**](/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding) を呼び出します。 バインドをプログラムで作成すると、Binding プロパティの値を実行時に選択する場合や、複数のコントロール間で 1 つのバインドを共有する場合に便利です。 ただし、**SetBinding** の呼び出し後は Binding プロパティの値を変更できないことに注意してください。

次の例では、バインドをコードで実装する方法を示しています。

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>{x:Bind} と {Binding} の機能の比較

| 機能 | {x:Bind} | {Binding} | メモ |
|---------|----------|-----------|-------|
| Path が既定のプロパティである | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path プロパティ | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | x:Bind では、Path は既定で DataContext ではなく、Page をルートにします。 | 
| Indexer | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | コレクション内で指定した項目にバインドします。 整数ベースのインデックスのみがサポートされます。 | 
| 添付プロパティ | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 添付プロパティは、かっこを使って指定します。 プロパティが XAML 名前空間で宣言されていない場合は、そのプロパティの前に xml 名前空間を付けます。これはドキュメントの先頭でコード名前空間にマップする必要があります。 | 
| キャスト | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 必要ありません。 | キャストはかっこを使って指定します。 プロパティが XAML 名前空間で宣言されていない場合は、そのプロパティの前に xml 名前空間を付けます。これはドキュメントの先頭でコード名前空間にマップする必要があります。 | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | コンバーターは、Page/ResourceDictionary のルートまたは App.xaml で宣言する必要があります。 | 
| ConverterParameter、ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | コンバーターは、Page/ResourceDictionary のルートまたは App.xaml で宣言する必要があります。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | バインド式のリーフが null の場合に使用されます。 文字列値の場合は単一引用符を使用します。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | バインディングのパスの一部 (リーフを除く) が null の場合に使用されます。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | {x:Bind} によって、フィールドにバインドしています。Path は既定で Page をルートとするため、名前付きの要素はそのフィールドを使ってアクセスできます。 | 
| RelativeSource:Self (自己) | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | {x:Bind} では、要素に名前を付けて、Path でその名前を使います。 | 
| RelativeSource:TemplatedParent | 不要 | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | {x:Bind} では、ControlTemplate の TargetType は、テンプレートの親へのバインディングを示します。 {Binding} の場合、標準のテンプレート バインディングは、ほとんどのユーザーのコントロール テンプレートで使用できます。 ただし、コンバーターまたは双方向バインディングを使用する必要がある場合は TemplatedParent を使います。< | 
| ソース | 不要 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | {X:Bind} の場合、名前付き要素を直接使用することも、プロパティまたは静的パスを使用することもできます。 | 
| モード | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode には、OneTime、OneWay、TwoWay を指定できます。 {x:Bind} の既定値は OneTime で、{Binding} の既定値は OneWay です。 | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger には、Default、LostFocus、PropertyChanged を指定できます。 {x:Bind} では、UpdateSourceTrigger=Explicit はサポートされません。 {x:Bind} では、TextBox.Text を除くすべての場合に PropertyChanged 動作を使います。TextBox.Text の場合は LostFocus 動作を使います。 |