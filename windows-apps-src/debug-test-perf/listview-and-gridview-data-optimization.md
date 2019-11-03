---
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: ListView と GridView のデータ仮想化
description: データ仮想化によって ListView と GridView のパフォーマンスと起動時間を向上させます。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d00a41c5a58935a4ecfe623c71a1264a2dc1132
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339617"
---
# <a name="listview-and-gridview-data-virtualization"></a>ListView と GridView のデータ仮想化


**注**   詳細については、「[ユーザーが GridView および ListView で大量のデータを操作するときに](https://channel9.msdn.com/Events/Build/2013/3-158)、build/session を使用してパフォーマンスを大幅に向上させる」を参照してください。

データ仮想化によって [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) と [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) のパフォーマンスと起動時間を向上させます。 UI の仮想化、要素の削減、項目の段階的な更新については、「[ListView と GridView の UI の最適化](optimize-gridview-and-listview.md)」をご覧ください。

データ仮想化のメソッドは、大きすぎてメモリに一度に格納できないか、すべてを格納する必要がないデータ セットで必要です。 最初の部分を (ローカル ディスク、ネットワーク、またはクラウドから) メモリに読み込んで、その部分的なデータ セットに UI の仮想化を適用します。 データは、後から段階的に読み込むことも、マスター データ セット内の任意の位置からオンデマンドで読み込むこともできます (ランダム アクセス)。 データ仮想化が適しているかどうかは、多数の要因によって決まります。

-   データ セットのサイズ
-   各項目のサイズ
-   データ セットのソース (ローカル ディスク、ネットワーク、またはクラウド)
-   アプリの総合的なメモリ消費量

**注**   機能が既定で有効になっていることに注意してください。 ListView と GridView では、ユーザーがすばやくパン/スクロールするときに、一時的なプレースホルダービジュアルが表示されます。 これらのプレース ホルダーの視覚効果は、データが読み込まれると項目テンプレートに置き換えられます。 この機能は、[**ListViewBase.ShowsScrollingPlaceholders**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) を false に設定することによって無効にできますが、その場合は、x:Phase 属性を使って項目テンプレートの要素を段階的にレンダリングすることをお勧めします。 詳しくは、「[GridView と ListView の項目を段階的に更新する](optimize-gridview-and-listview.md#update-items-incrementally)」をご覧ください。

以降では、段階的なデータ仮想化とランダム アクセスのデータ仮想化の手法について詳しく説明します。

## <a name="incremental-data-virtualization"></a>段階的なデータ仮想化

段階的なデータ仮想化では、データを連続的にダウンロードします。 段階的なデータ仮想化を実行する [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) を使って、数百万の項目のコレクションを表示できますが、最初は 50 個の項目だけが読み込まれます。 ユーザーがパン/スクロールすると、次の 50 個の項目が読み込まれます。 項目が読み込まれると、スクロール バーのサムはサイズが小さくなります。 この種のデータ仮想化では、次のインターフェイスを実装するデータ ソース クラスを記述します。

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist)
-   [INotifyCollectionChanged](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) (C#/VB) または[**IObservableVector&lt;T&gt;**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) (C++/cx)
-   [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)

このようなデータ ソースは、継続的に拡張できるメモリ内リストです。 項目コントロールは、標準的な [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist) インデクサーとカウント プロパティを使って項目を要求します。 カウントは、データセットの実際のサイズではなく、ローカルでの項目の数を表す必要があります。

項目コントロールは、既存のデータの終わりに近づいたときに [**ISupportIncrementalLoading.HasMoreItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems) を呼び出します。 **true** が返された場合は、アドバタイズされた読み込む項目数を渡す [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) を呼び出します。 データの読み込み元 (ローカル ディスク、ネットワーク、またはクラウド) に応じて、アドバタイズされた項目数とは異なる数の項目を読み込むことができます。 たとえば、サービスは 50 項目のバッチをサポートしているが、項目コントロールは 10 項目のみを要求している場合、50 項目を読み込むことができます。 バックエンドからデータを読み込んでリストに追加した後、[**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) または [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) 経由で変更通知を発行して、項目コントロールが新しい項目を認識できるようにします。 さらに、実際に読み込んだ項目の数を返します。 アドバタイズされた数よりも少ない項目を読み込むか、項目コントロールが途中でさらにパン/スクロールされた場合は、データ ソースをもう一度呼び出して、さらに項目を読み込むサイクルが続けられます。 詳細については、Windows 8.1 用の[XAML データバインディングのサンプル](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5)をダウンロードし、Windows 10 アプリでソースコードを再利用する方法に関するページを参照してください。

## <a name="random-access-data-virtualization"></a>ランダム アクセスのデータ仮想化

ランダム アクセスのデータ仮想化を使うと、データ セット内の任意の位置からデータを読み込むことができます。 ランダム アクセスのデータ仮想化を実行する [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) を、100 万の項目があるコレクションを表示するために使うと、100,000 番目から 100,050 番目の項目を読み込むことができます。 ユーザーが一覧の先頭に移動すると、コントロールは 1 番目から 50 番目の項目を読み込みます。 スクロール バーのサムは、常に **ListView** に 100 万の項目が含まれていることを示します。 スクロール バーのサムの位置は、表示されている項目がコレクションのデータ セット全体で相対的にどこに位置しているかを示します。 この種のデータ仮想化は、必要なメモリを大幅に減らし、コレクションの読み込み時間を大きく短縮します。 これを有効にするには、データをオンデマンドで取得し、ローカル キャッシュを管理し、次のインターフェイスを実装するデータ ソース クラスを記述する必要があります。

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist)
-   [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) (C#/VB) または[**IObservableVector&lt;T&gt;**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) (C++/cx)
-   (必要に応じて) [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)
-   (必要に応じて) [**ISelectionInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISelectionInfo)

[**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)は、コントロールがアクティブに使用している項目に関する情報を提供します。 項目コントロールはビューが変更されるたびにこのメソッドを呼び出し、その中には 次の 2 つの範囲のセットが含まれます。

-   ビューポート内の項目のセット。
-   項目コントロールが使う項目で、ビューポートに表示されない可能性がある、仮想化されていない項目のセット。
    -   タッチ パンをスムーズに行えるようにするために項目コントロールが保持している、ビューポートの周囲の項目のバッファー。
    -   フォーカスが置かれている項目。
    -   先頭の項目。

[**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) を実装することで、データ ソースは、どの項目をフェッチしてキャッシュする必要があり、不要になったキャッシュ データをいつ除去するかがわかります。 **IItemsRangeInfo** は、[**ItemIndexRange**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ItemIndexRange) オブジェクトを使って、コレクション内のインデックスに基づいてオブジェクトのセットを記述します。 これは、正しくないか安定していない可能性がある項目ポインターを使わないようにするためです。 **IItemsRangeInfo** は、項目コントロールの状態情報に頼っているため、項目コントロールの 1 つのインスタンスでのみ使われるように設計されています。 複数の項目コントロールが同じデータにアクセスする必要がある場合は、それぞれに対してデータ ソースの個別のインスタンスが必要です。 それらは共通のキャッシュを共有できますが、キャッシュから消去するためのロジックはもっと複雑です。

ランダム アクセスのデータ仮想化データ ソースのための基本的な戦略を次に示します。

-   項目を要求されたとき
    -   メモリ内の項目を利用できる場合はその項目を返します。
    -   利用できない場合は、null またはプレースホルダー項目を返します。
    -   項目に対する要求 (または [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) からの範囲要求) を使って、どの項目が必要であるかを調べ、バックエンドから項目のデータを非同期的に取得します。 データを取得した後、[**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) または [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) 経由で変更通知を発行して、項目コントロールが新しい項目を認識できるようにします。
-   (必要に応じて) 項目コントロールのビューポートが変更されるときに、[**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) の実装を通してどの項目がデータ ソースから必要であるかを識別します。

その他のいつデータ項目を読み込むか、いくつ読み込むか、そしてどの項目をメモリに保持するかは、アプリケーションにまかされます。 いくつかの一般的な考慮事項を次に示します。

-   データは非同期に要求します。UI スレッドをブロックしないでください。
-   項目を取得するバッチのサイズのスイート スポットを探します。 ブロックで処理するようにします。 小さな要求を何度も繰り返すほど小さくなく、取得するまで時間がかかりすぎるほど大きくないサイズにします。
-   同時に保留中にする要求の数を検討します。 簡単なのは一度に 1 つずつ実行することですが、完了までの時間がかかる場合は遅くなりすぎる可能性があります。
-   データの要求を取り消すことができるかどうか。
-   ホストされるサービスを使っている場合は、トランザクションごとにコストが発生するかどうか。
-   クエリの結果が変更されるときにサービスによって提供される通知の種類は何か。 項目がンデックス 33 に挿入されたことがわかるか。 サービスがキーとオフセットに基づくクエリをサポートする場合は、インデックスだけを使うよりも適している可能性があります。
-   項目のプリフェッチをいかにスマートに実行するか。 必要な項目を予測するためにスクロールの方向と速度を追跡する予定ですか。
-   キャッシュの消去をどの程度積極的に行うか。 これはメモリとエクスペリエンスのトレードオフです。




