---
title: ファイルへの書き込みに関するベスト プラクティス
description: FileIO および PathIO クラスのさまざまなファイル書き込みメソッドの使用に関するベスト プラクティスについて学習します。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 844e1da1a4108673c353e91b8624376d9b98e976
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173256"
---
# <a name="best-practices-for-writing-to-files"></a>ファイルへの書き込みに関するベスト プラクティス

**重要な API**

* [**FileIO クラス**](/uwp/api/Windows.Storage.FileIO)
* [**PathIO クラス**](/uwp/api/windows.storage.pathio)

開発者がファイル システム I/O 操作を実行するために [**FileIO**](/uwp/api/Windows.Storage.FileIO) および [**PathIO**](/uwp/api/windows.storage.pathio) クラスの **Write** メソッドを使用するときに、一般的な一連の問題が発生することがあります。 たとえば、一般的な問題には以下のようなものが含まれます。

* ファイルが部分的に書き込まれる。
* アプリで、メソッドのいずれかを呼び出すときに例外を受け取る。
* 操作で、ターゲット ファイルの名前と似たようなファイル名の .TMP ファイルが残る。

[**FileIO**](/uwp/api/Windows.Storage.FileIO) および [**PathIO**](/uwp/api/windows.storage.pathio) クラスの **Write** メソッドには、以下のものが含まれます。

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 この記事では、開発者がこれらのメソッドをいつどのように使用するかをよりよく理解できるように、それらの動作について詳しく説明します。 この記事はガイドラインを提供するものであり、考えられるすべてのファイル I/O に関する問題の解決策の提供を試みるものではありません。 

> [!NOTE]
> この記事では、**FileIO** メソッドを例とし、重点的に説明します。 しかし、**PathIO** メソッドでは同様のパターンに従い、この記事のガイダンスのほとんどがそれらにも適用されます。 

## <a name="convenience-vs-control"></a>利便性と制御

[**StorageFile**](/uwp/api/windows.storage.storagefile) オブジェクトは、ネイティブの Win32 プログラミング モデルのようなファイル ハンドルではありません。 むしろ、[**StorageFile**](/uwp/api/windows.storage.storagefile) は、コンテンツを操作するためのメソッドを含むファイルを表したものです。

この概念を理解することは、**StorageFile** で I/O を実行する場合に役立ちます。 たとえば、「[ファイルへの書き込み](quickstart-reading-and-writing-files.md#writing-to-a-file)」セクションでは、ファイルへの 3 とおりの書き込み方法が示されています。

* [**FileIO.WriteTextAsync**](/uwp/api/windows.storage.fileio.writetextasync) メソッドを使用する。
* バッファーを作成してから、[**FileIO.WriteBufferAsync**](/uwp/api/windows.storage.fileio.writebufferasync) メソッドを呼び出す。
* ストリームを使用する 4 ステップのモデル:
  1. ファイルを[開き](/uwp/api/windows.storage.storagefile.openasync)、ストリームを取得します。
  2. 出力ストリームを[取得](/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)します。
  3. [**DataWriter**](/uwp/api/windows.storage.streams.datawriter) オブジェクトを作成し、対応する **Write** メソッドを呼び出します。
  4. データ ライターでデータを[コミット](/uwp/api/windows.storage.streams.datawriter.storeasync)し、出力ストリームを[フラッシュ](/uwp/api/windows.storage.streams.ioutputstream.flushasync)します。

最初の 2 つのシナリオは、アプリで最もよく使用されるものです。 単一操作でのファイルへの書き込みは、コーディングして保守するより簡単です。また、アプリでファイル I/O の多くの複雑さに対処しなくても済むようになります。 しかし、この利便性の代償として、操作全体の制御と、特定の時点のエラーのキャッチができなくなります。

## <a name="the-transactional-model"></a>トランザクション モデル

[**FileIO**](/uwp/api/Windows.Storage.FileIO) および [**PathIO**](/uwp/api/windows.storage.pathio) クラスの **Write** メソッドでは、レイヤーが追加され、上記の 3 番目の書き込みモデルの手順がラップされます。 このレイヤーは、ストレージ トランザクションでカプセル化されます。

データの書き込み中に問題が発生した場合に元のファイルの整合性を保護するために、**Write** メソッドでは [**OpenTransactedWriteAsync**](/uwp/api/windows.storage.storagefile.opentransactedwriteasync) を使ってファイルを開き、トランザクション モデルを使用します。 このプロセスで [**StorageStreamTransaction**](/uwp/api/windows.storage.storagestreamtransaction) オブジェクトが作成されます。 このトランザクション オブジェクトが作成された後、API では、[ファイル アクセス](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) サンプル、または [**StorageStreamTransaction**](/uwp/api/windows.storage.storagestreamtransaction) に関する記事のコード例と同じような方法に従って、データを書き込みます。

次の図では、成功した書き込み操作の**WriteTextAsync** メソッドで実行された基になるタスクを示しています。 この図では、操作の簡易ビューを提供しています。 たとえば、さまざまなスレッドでテキストのエンコードや非同期完了などの手順は省略されています。

![ファイルへの書き込みに関する UWP API 呼び出しのシーケンス図](images/file-write-call-sequence.svg)

ストリームを使用する、より複雑な 4 ステップ モデルではなく、[**FileIO**](/uwp/api/Windows.Storage.FileIO) および [**PathIO**](/uwp/api/windows.storage.pathio) クラスの **Write** メソッドを使用する利点は、次のとおりです。

* エラーを含む、すべての中間ステップを処理する 1 回の API 呼び出し。
* 問題が発生した場合に、元のファイルが保持される。
* システム状態が可能な限りクリーンな状態に保たれるようにする。

しかし、考えられる中間単一障害点が非常に多く存在するため、障害の可能性が高くなります。 エラーが発生したときに、プロセスが失敗した場所を把握するのが難しくなる場合があります。 次のセクションでは、**Write** メソッドを使用するときに発生する可能性のある障害をいくつか示し、考えられる解決策を提供します。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO および PathIO クラスの Write メソッドの一般的なエラー コード

この表には、アプリ開発者が **Write** メソッドを使用するときに発生する一般的なエラー コードが示されています。 表の手順は、前の図の手順に対応しています。

|  エラー名 (値)  |  手順  |  原因  |  ソリューション  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  場合によっては前の操作から、削除する対象として元のファイルにマークが付けられる可能性があります。  |  操作を再試行します。</br>ファイルへのアクセスが同期されていることを確認します。  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  元のファイルが別の排他的書き込みによって開かれています。   |  操作を再試行します。</br>ファイルへのアクセスが同期されていることを確認します。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19 から 20  |  元のファイル (file.txt) が使用中のため、置換できませんでした。 置換の前に、別のプロセスまたは操作でファイルにアクセスされました。  |  操作を再試行します。</br>ファイルへのアクセスが同期されていることを確認します。  |
|  ERROR_DISK_FULL (0x80070070)  |  7、14、16、20  |  トランザクション処理されたモデルでは余分なファイルが作成され、これにより、余分なストレージが消費されています。  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14、16  |  これは、複数の未処理の I/O 操作または大きなファイル サイズが原因で発生する場合があります。  |  ストリームを制御することによる、より詳細な方法でエラーが解決される可能性があります。  |
|  E_FAIL (0x80004005) |  Any  |  その他  |  操作を再試行します。 それでも失敗する場合は、プラットフォームのエラーである可能性があります。アプリが不整合な状態であるため、終了する必要があります。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>エラーにつながる可能性のあるファイルの状態に関するその他の考慮事項

**Write** メソッドによって返されたエラーとは別に、ここでは、ファイルへの書き込み時にアプリで予期される内容に関するガイダンスをいくつか示します。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>操作が完了した場合にのみ、データがファイルに書き込まれる

アプリでは、書き込み操作の進行中にファイル内のデータについて何も想定しません。 操作が完了する前にファイルにアクセスしようとすると、整合性のないデータになる可能性があります。 アプリでは、未処理の I/O の追跡を担当する必要があります。

### <a name="readers"></a>Readers

書き込まれているファイルが正当な閲覧者によって使用もされている (つまり、[**FileAccessMode.Read**](/uwp/api/Windows.Storage.FileAccessMode) で開かれている) 場合、以降の読み取りは ERROR_OPLOCK_HANDLE_CLOSED (0x80070323) エラーで失敗します。 アプリでは、**書き込み**操作の進行中に、再度読み取るファイルを開くために再試行される場合があります。 これにより、競合状態となる可能性があり、元のファイルを置換できないため、そのファイルの上書きが試行されると、**書き込み**は最終的に失敗します。

### <a name="files-from-knownfolders"></a>KnownFolders からのファイル

自分のアプリが、[**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) のいずれかに存在するファイルにアクセスしようとしている唯一のアプリではない場合があります。 操作が成功しても、アプリでファイルに書き込まれた内容が、ファイルの読み取りの次回の試行時に一定のままである保証はありません。 また、このシナリオでは、共有またはアクセス拒否エラーがより一般的になります。

### <a name="conflicting-io"></a>競合する I/O

アプリでローカル データのファイルに対して **Write** メソッドを使用すると、同時開催エラーの可能性は低くなる場合がありますが、それでもいくつか注意が必要です。 複数の**書き込み**操作が同時にファイルに送信されている場合、ファイルのデータの最終的な状態については保証されません。 これを軽減するために、アプリでファイルへの**書き込み**操作をシリアル化することをお勧めします。

### <a name="tmp-files"></a>~TMP ファイル

場合によっては、操作が強制的に中断されると (たとえば、アプリが OS によって中断または終了された場合)、トランザクションがコミットされないか、適切に閉じられません。 これにより、(.~TMP) 拡張子が付いたファイルが残る可能性があります。 アプリのアクティブ化を処理するときに、(アプリのローカル データに存在する場合は) これらの一時ファイルの削除を検討してください。

## <a name="considerations-based-on-file-types"></a>ファイルの種類に基づく考慮事項

ファイルの種類、アクセス頻度、およびファイルのサイズに応じて、一部のエラーがより一般的になる場合があります。 一般に、アプリからアクセスできるファイルには 3 つのカテゴリがあります。

* アプリのローカル データ フォルダーで、ユーザーによって作成および編集されたファイル。 これらはアプリの使用時にのみ、作成および編集され、アプリ内にのみ存在します。
* アプリのメタデータ。 アプリではこれらのファイルを使用して、自身の状態を追跡します。
* アプリでアクセスする機能が宣言された、ファイル システムの場所にあるその他のファイル。 これらは、[**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) のいずれかに最も一般的に配置されます。

アプリでは最初の 2 つのカテゴリのファイルを完全に制御できます。これらは、アプリのパッケージ ファイルの一部であり、アプリのみでアクセスされるためです。 最後のカテゴリのファイルの場合、アプリでは、他のアプリと OS サービスによってファイルに同時にアクセスされる可能性があることに注意する必要があります。

アプリに応じて、ファイルへのアクセスは頻度が異なる場合があります。

* 非常に低い。 通常、これらはアプリが起動したときに一度開かれ、アプリが中断されたときに保存されるファイルです。
* 低い。 これらは、特にユーザーによって (保存や読み込みなど) 操作されるファイルです。
* 中程度または高い。 これらは、アプリで常にデータを更新する (自動保存機能や定期的なメタデータ追跡など) 必要があるファイルです。

ファイルのサイズについては、**WriteBytesAsync** メソッドに関する以下のグラフのパフォーマンス データを見ていきます。 このグラフでは、制御された環境でのファイル サイズごとの 10000 操作の平均パフォーマンスを超える、ファイル サイズと操作を完了するまでの時間を比較します。

![WriteBytesAsync のパフォーマンス](images/writebytesasync-performance.png)

ハードウェアおよび構成によって得られる絶対時刻値が異なるため、このグラフの y 軸の時間値は意図的に省略されています。 しかし、マイクロソフトによるテストではこのような傾向を一貫して監視しています。

* 非常に小さなファイル (1 MB 以下) の場合:操作が完了するまでの時間は、一貫して短くなります。
* 大きなファイル (1 MB を超える) の場合:操作が完了するまでの時間が、指数関数的に増え始めます。

## <a name="io-during-app-suspension"></a>アプリの中断時の I/O

以降のセッションで使用する状態情報またはメタデータを保持する必要がある場合は、中断を処理するようにアプリを設計する必要があります。 アプリの中断に関する背景情報については、[アプリのライフサイクル](../launch-resume/app-lifecycle.md)に関するページと[こちらのブログ記事](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)を参照してください。

OS でアプリに延長実行が許可されない限り、アプリが中断された場合に、そのすべてのリソースが解放され、そのデータが保存されるまで 5 秒かかります。 最高の信頼性とユーザー エクスペリエンスのために、中断タスクを処理する必要がある時間が限られていることを常に前提とします。 中断タスクを処理する 5 秒間は、以下のガイドラインに注意してください。

* フラッシュおよびリリース操作によって発生する競合状態を回避するために、I/O を最小限に抑えるようにしてください。
* 書き込みに数百ミリ秒以上の時間を要するファイルの書き込みは避けてください。
* アプリで **Write** メソッドが使用されている場合は、これらのメソッドで必要なすべての中間ステップに注意してください。

中断時にアプリで少量の状態データを操作するのであれば、ほとんどの場合、**Write** メソッドを使用して、データをフラッシュすることができます。 しかし、アプリで大量の状態データを使用する場合は、ストリームを使ってデータを直接格納することを検討してください。 これは、**Write** メソッドのトランザクション モデルによって発生する遅延を減らすのに役立つ場合があります。 

例については、[BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) サンプルに関するページを参照してください。

## <a name="other-examples-and-resources"></a>その他の例とリソース

特定のシナリオでのいくつかの例と他のリソースを以下に示します。

### <a name="code-example-for-retrying-file-io-example"></a>ファイル I/O の例を再試行するためのコード例

ユーザーが保存するファイルを選んだ後に書き込みが行われることを前提とし、書き込みを再試行する方法の疑似コード例 (C#) を以下に示します。

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>ファイルへのアクセスを同期する

[.NET での並列プログラミングに関するブログ](https://devblogs.microsoft.com/pfxteam/)は、並列プログラミングについてのガイダンスの優れたリソースです。 具体的には、[AsyncReaderWriterLock についての投稿](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)で、同時読み取りアクセスを許可している間に、書き込みのためのファイルへの排他的アクセスを維持する方法について説明されています。 I/O のシリアル化がパフォーマンスに影響することに注意してください。

## <a name="see-also"></a>関連項目

* [ファイルの作成、書き込み、および読み取り](quickstart-reading-and-writing-files.md)