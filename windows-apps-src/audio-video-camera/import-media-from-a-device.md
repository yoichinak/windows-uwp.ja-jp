---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: この記事では、利用可能なメディア ソースの検索、写真やサイドカー ファイルなどのファイルのインポート、ソース デバイスからインポートしたファイルの削除など、デバイスからメディアをインポートする方法について説明します。
title: メディアのインポート
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 626a80b1c3962f5bf12d7a906a61f2f600da5eed
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362535"
---
# <a name="import-media-from-a-device"></a>デバイスからのメディアのインポート

この記事では、利用可能なメディア ソースの検索、ビデオ、写真、サイドカー ファイルなどのファイルのインポート、ソース デバイスからインポートしたファイルの削除など、デバイスからメディアをインポートする方法について説明します。

> [!NOTE] 
> この記事のコードは、[**MediaImport UWP アプリ サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)を基にしています。 このサンプルを[**ユニバーサル Windows アプリ サンプル Git リポジトリ**](https://github.com/Microsoft/Windows-universal-samples)から複製またはダウンロードすると、コンテキスト内のコードを確認できます。独自のアプリの出発点として使うこともできます。

## <a name="create-a-simple-media-import-ui"></a>シンプルなメディア インポート UI の作成
この記事の例では、メディア インポートのコア シナリオを実現する最小限の UI を使用します。 メディア インポート アプリ用のより強力な UI を作成する方法については、[**MediaImport サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)をご覧ください。 次の XAML では、次のコントロールを持つスタック パネルを作成します。
* メディアをインポートできるソースの検索を開始するための [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)。
* 見つかったメディア インポート ソースを一覧にして選択するための [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)。
* 選択したインポート ソースのメディア項目を表示して選択するための [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) コントロール。
* 選択したソースからメディア項目のインポートを開始するための **Button**。
* 選択したソースからインポートされた項目の削除を開始するための **Button**。
* 非同期メディア インポート操作を取り消すための **Button**。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml" id="SnippetImportXAML":::

## <a name="set-up-your-code-behind-file"></a>分離コード ファイルの設定
*using* ディレクティブを追加して、既定のプロジェクト テンプレートにまだ含まれていない、この例で使用する名前空間を含めます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetUsing":::

## <a name="set-up-task-cancellation-for-media-import-operations"></a>メディア インポート操作のタスク取り消しの設定

メディア インポート操作には長い時間がかかる場合があるため、操作は [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) を使用して非同期で実行します。 ユーザーがキャンセル ボタンをクリックした場合に、実行中の操作の取り消しに使用する、[**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource) 型のクラス メンバー変数を宣言します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareCts":::

キャンセル ボタンのハンドラーの実装 この記事の後で示す例では、**CancellationTokenSource** を操作が始まったときに初期化して、操作が完了したときに null に設定します。 このキャンセル ボタン ハンドラーでは、トークンが null かどうかを確認し、null でない場合は [**Cancel**](/dotnet/api/system.threading.cancellationtokensource.cancel#System_Threading_CancellationTokenSource_Cancel) を呼び出して操作を取り消します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetOnCancel":::

## <a name="data-binding-helper-classes"></a>データ バインディング ヘルパー クラス

一般的なメディア インポート シナリオでは、インポートできるメディア項目の一覧をユーザーに提示します。選択するメディア ファイルの数は非常に多い可能性があり、通常、各メディア項目のサムネイルを表示します。 このため、この例では 3 つのヘルパー クラスを使用して、ユーザーが一覧をスクロールしていくと ListView コントロールにエントリが段階的に読み込まれるようにします。

* **IncrementalLoadingBase** クラス - [**IList**](/dotnet/api/system.collections.ilist)、[**ISupportIncrementalLoading**](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)、および [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) を実装し、段階的な読み込みの基本動作を提供します。
* **GeneratorIncrementalLoadingClass** クラス - 段階的な読み込みの基底クラスの実装を提供します。
* **ImportableItemWrapper** クラス - [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem) クラスのシン ラッパーで、インポートされる各項目のサムネイル イメージに対するバインディング可能な [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) プロパティを追加します。

これらのクラスは [**MediaImport サンプル**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)に用意されており、変更なしでプロジェクトに追加できます。 ヘルパー クラスをプロジェクトに追加したら、後でこの例で使用する**GeneratorIncrementalLoadingClass** 型のクラス メンバー変数を宣言します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetGeneratorIncrementalLoadingClass":::


## <a name="find-available-sources-from-which-media-can-be-imported"></a>メディアをインポートできる利用可能なソースを見つける

ソース検索ボタンのクリック ハンドラーで、静的メソッド [**PhotoImportManager.FindAllSourcesAsync**](/uwp/api/windows.media.import.photoimportmanager.findallsourcesasync) を呼び出し、メディアをインポート可能なデバイスのシステム検索を開始します。 操作の完了を待機した後、返される一覧内の各 [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource) オブジェクトをループ処理し、エントリを **ComboBox** に追加します。ソース オブジェクト自体に **Tag** プロパティを設定して、ユーザーが選択するときに簡単にソース オブジェクトを取得できるようにします。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetFindSourcesClick":::

ユーザーが選択したインポート ソースを格納するクラス メンバー変数を宣言します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImportSource":::

インポート ソース **ComboBox** の [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) ハンドラーで、クラス メンバー変数を選択したソースに設定します。その後、この記事の広範で示す **FindItems** ヘルパー メソッドを呼び出します。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetSourcesSelectionChanged":::

## <a name="find-items-to-import"></a>インポートする項目を見つける

この後の手順で使用する [**PhotoImportSession**](/uwp/api/Windows.Media.Import.PhotoImportSession) 型および [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) 型のクラス メンバー 変数を追加します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImport":::

**FindItems**メソッドで、 **CancellationTokenSource**変数を初期化して、必要に応じて検索操作を取り消すために使用できるようにします。 **try** ブロック内で、ユーザーが選択した [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource) オブジェクトで [**CreateImportSession**](/uwp/api/windows.media.import.photoimportsource.createimportsession) を呼び出して、新しいインポート セッションを作成します。 検索操作の進行状況を表示するためのコールバックを提供するために、新しい [**Progress**](/dotnet/api/system.progress-1) オブジェクトを作成します。 次に、**[FindItemsAsync](/uwp/api/windows.media.import.photoimportsession.finditemsasync)** を呼び出して、検索操作を開始します。 [**PhotoImportContentTypeFilter**](/uwp/api/Windows.Media.Import.PhotoImportContentTypeFilter) の値を提供して、写真、ビデオ、またはその両方を返す必要があるかどうかを指定します。 [**PhotoImportItemSelectionMode**](/uwp/api/Windows.Media.Import.PhotoImportItemSelectionMode) の値を提供して、[**IsSelected**](/uwp/api/windows.media.import.photoimportitem.isselected) プロパティが true に設定されているときに、すべてのメディア項目を返すのか、メディア項目を何も返さないのか、新しいメディア項目のみを返すのかを指定します。 このプロパティは、ListBox 項目テンプレートの、各メディア項目のチェック ボックスにバインドされます。

**FindItemsAsync** は [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) を返します。 拡張メソッド [**AsTask**](/dotnet/api/system) は、待機可能で、キャンセル トークンを使用して取り消し可能であり、提供された **Progress** オブジェクトを使用して進行状況を報告するタスクの作成に使用されます。

次に、データ バインディング ヘルパー クラス **GeneratorIncrementalLoadingClass** が初期化されます。 **FindItemsAsync** は、待機から返されると、[**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) オブジェクトを返します。 このオブジェクトには、操作の成功、見つかったさまざまな種類のメディア項目の数など、検索操作の状態情報が含まれます。 [**FoundItems**](/uwp/api/windows.media.import.photoimportfinditemsresult.founditems) プロパティには、見つかったメディア項目を表す [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem) オブジェクトの一覧が含まれます。 **GeneratorIncrementalLoadingClass** コンストラクターは、段階的に読み込まれる項目の合計数と、必要に応じて読み込まれる新規項目を生成する関数を引数として受け取ります。 この場合、指定されたラムダ式によって、**ImportableItemWrapper** の新しいインスタンスが作成されます。これは、**PhotoImportItem** をラップして各項目のサムネイルを含めます。 段階的読み込みクラスが初期化されたら、それを UI に含まれる **ListView** コントロールの [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) プロパティに設定します。 これで、見つかったメディア項目が段階的に読み込まれ、一覧に表示されます。

次に、検索操作の状態情報が出力されます。 一般的なアプリでは、この情報が UI でユーザーに表示されます。しかし、この例では、単純に情報をデバッグ コンソールに出力します。 最後に、操作が完了したのでキャンセル トークンを null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetFindItems":::

## <a name="import-media-items"></a>メディア項目のインポート

インポート操作を実装する前に、インポート操作の結果を格納する [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) オブジェクトを宣言します。 これは後で、ソースから正常にインポートされたメディア項目を削除するために使用します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImportResult":::

メディアのインポート操作を開始する前に、**CancellationTokenSource** 変数を初期化し、[**ProgressBar**](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) コントロールの値を 0 に設定します。

**ListView** コントロールで選択されている項目がない場合、インポートするものはありません。 それ以外の場合は、進行状況バー コントロールを更新する進行状況コールバックを提供するために、[**Progress**](/dotnet/api/system.progress-1) オブジェクトを初期化します。 検索操作によって返される [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) の [**ItemImported**](/uwp/api/windows.media.import.photoimportfinditemsresult.itemimported) イベント用のハンドラーを登録します。 このイベントは、この例の場合、項目がインポートされ、インポートされた各ファイルの名前がデバッグ コンソールに出力されるときに発生します。

[**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) を呼び出して、インポート操作を開始します。 検索操作と同様に、[**AsTask**](/dotnet/api/system) 拡張メソッドは、返された操作を、待機可能で、進行状況を報告し、取り消し可能なタスクに変換するために使用されます。

インポート操作が完了したら、[**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) によって返される [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) オブジェクトから操作の状態を取得できます。 この例では、状態情報をデバッグ コンソールに出力して、最後にキャンセル トークンを null に設定します。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetImportClick":::

## <a name="delete-imported-items"></a>インポートされた項目の削除
ソースから正常にインポートされた項目をインポート元から削除するには、まず、削除操作を取り消せるようにキャンセル トークンを初期化して、進行状況バーの値を 0 に設定します。 [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) から返される [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) が null でないことを確認します。 そうでない場合は、削除操作の進行状況コールバックを提供するために、[**Progress**](/dotnet/api/system.progress-1) オブジェクトをもう一度作成します。 [**DeleteImportedItemsFromSourceAsync**](/uwp/api/windows.media.import.photoimportimportitemsresult.deleteimporteditemsfromsourceasync) を呼び出して、インポートされた項目の削除を開始します。 **AsTask** を使用して、進行状況の報告機能と取り消し機能を持った待機可能なタスクに結果を変換します。 待機後、返された [**PhotoImportDeleteImportedItemsFromSourceResult**](/uwp/api/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) オブジェクトを使用して、削除操作に関する状態情報を取得および表示できます。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeleteClick":::








