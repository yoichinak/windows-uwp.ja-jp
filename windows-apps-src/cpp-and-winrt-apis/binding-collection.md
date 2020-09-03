---
description: XAML アイテム コントロールに効果的にバインドできるコレクションは、*監視可能な*コレクションと呼ばれます。 このトピックでは、監視可能なコレクションを実装および使用する方法と、それに XAML アイテム コントロールをバインドする方法を示します。
title: 'XAML アイテム コントロール: C++/WinRT コレクションへのバインド'
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、XAML、コントロール、バインド、コレクション
ms.localizationpriority: medium
ms.openlocfilehash: c87fcec62c9177ddfccaa14e97294ebcd78ded5f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154516"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>XAML アイテム コントロール: C++/WinRT コレクションへのバインド

XAML アイテム コントロールに効果的にバインドできるコレクションは、*監視可能な*コレクションと呼ばれます。 この概念は、*オブザーバー パターン*と呼ばれるソフトウェアの設計パターンに基づいています。 このトピックでは、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) で監視可能なコレクションを実装する方法と、これらに XAML コントロールをバインドする方法を示します (背景情報については、「[データ バインディング](../data-binding/index.md)」をご覧ください)。

このトピックを理解するには、「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」に記載されているプロジェクトを最初に作成することをお勧めします。 このトピックでは、そのプロジェクトにさらにコードを追加し、そのトピックで説明されている概念が補足されます。

> [!IMPORTANT]
> C++/WinRT でランタイム クラスを使用および作成する方法についての理解をサポートするために重要な概念と用語については、「[C++/WinRT での API の使用](consume-apis.md)」と「[C++/WinRT での作成者 API](author-apis.md)」を参照してください。

## <a name="what-does-observable-mean-for-a-collection"></a>コレクションの*監視可能*とはどういう意味ですか?

コレクションを表すランタイム クラスが、要素が追加されるまたは削除されるたびに [**IObservableVector&lt;T&gt;:: VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) イベントを発生することを選択する場合、そのランタイム クラスは監視可能なコレクションです。 XAML アイテム コントロールでは、更新されたコレクションを取得して、現在の要素を表示するためにそれ自体を更新することで、これらのイベントをバインドし、処理することができます。

> [!NOTE]
> C++/WinRT Visual Studio Extension (VSIX) と NuGet パッケージ (両者が連携してプロジェクト テンプレートとビルドをサポート) のインストールと使用については、[Visual Studio での C++/WinRT のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>**BookSkus** コレクションを **BookstoreViewModel** に追加する

「[XAML コントロール、C++/WinRT プロパティへのバインド](binding-property.md)」では、**BookSku** 型のプロパティをメイン ビュー モデルに追加しました。 この手順では、[**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) ファクトリ関数テンプレートを使用します。これは、同じビュー モデルに対して **BookSku** の監視可能なコレクションを実装するのに役に立ちます。

`BookstoreViewModel.idl` で新しいプロパティを宣言します。

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> 上記の MIDL 3.0 のリストでは、**BookSkus** プロパティの型が **BookSku** の [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) であることに注意してください。 このトピックの次のセクションでは、[**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) の項目ソースを **BookSkus** にバインドします。 リスト ボックスは項目コントロールです。[**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティを正しく設定するには、それを **IObservableVector** 型の値、**IVector** 型の値、または [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) などの相互運用性型の値に設定する必要があります。

> [!WARNING]
> このトピックで示すコードの適用対象は、C++/WinRT バージョン 2.0.190530.8 以降です。 それより前のバージョンを使っている場合は、示されているコードを若干調整する必要があります。 上記の MIDL 3.0 のリストでは、**BookSkus** プロパティを [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) の [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) に変更します。 その後、実装でも (**BookSku** の代わりに) **IInspectable** を使います。

保存してビルドします。 `\Bookstore\Bookstore\Generated Files\sources` フォルダー内の `BookstoreViewModel.h` および `BookstoreViewModel.cpp` からアクセサー スタブをコピーします (詳細については、前のトピック「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」を参照してください)。 それらのアクセサー スタブを次のように実装します。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>**BookSkus** プロパティに ListBox をバインドします。

メイン UI ページの XAML マークアップが含まれている `MainPage.xaml` を開きます。 **Button** と同じ **StackPanel** 内に次のマークアップを追加します。

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

`MainPage.cpp` で、**クリック** イベント ハンドラーにコードの行を追加してブックをコレクションに追加します。

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

ここでプロジェクトをビルドして実行します。 ボタンをクリックして**クリック** イベント ハンドラーを実行します。 **Append** の実装によりイベントが発生し、コレクションが変更されたことを UI が把握できるようにすることが分かります。**ListBox** はその独自の **Items** 値を更新するためにコレクションを再クエリします。 前と同様に、ブックのいずれかのタイトルが変わります。このタイトル変更は、ボタンとリスト ボックス内の両方に反映されます。

## <a name="important-apis"></a>重要な API

* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [winrt::make 関数テンプレート](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>関連トピック

* [C++/WinRT で API を使用する](consume-apis.md)
* [C++/WinRT で API を作成する](author-apis.md)