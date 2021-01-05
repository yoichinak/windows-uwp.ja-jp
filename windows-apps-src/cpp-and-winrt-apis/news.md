---
description: 最近の追加機能と改善点、および C++/WinRT 2.0 と Windows SDK バージョン 10.0.17763.0 についての新機能と変更点をご確認ください。
title: C++/WinRT の新機能
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 新機能
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f5cfe5dc66df98e3dd4d4290023cac1874ae797a
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860288"
---
# <a name="whats-new-in-cwinrt"></a>C++/WinRT の新機能

C++/WinRT の後続バージョンがリリースされると、このトピックに新機能と変更点が記載されます。

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>2020 年 3 月現在における最新の機能強化および追加のロールアップ

### <a name="up-to-23-shorter-build-times"></a>ビルド時間を最大 23% 短縮

C++/WinRT と C++ コンパイラ チームは、ビルド時間短縮のために可能なすべてのことを共同で行ってきました。 コンパイラ分析に注力し、C++ コンパイラでコンパイル時のオーバーヘッドを解消できるように C++/WinRT の内部を再構築する方法と、C++/WinRT ライブラリを処理するために C++ コンパイラ自体を改善する方法を見つけ出しました。 C++/WinRT はコンパイラ用に最適化されており、コンパイラも C++/WinRT 用に最適化されています。

たとえば、すべての C++/WinRT プロジェクション名前空間ヘッダーを含むプリコンパイル済みヘッダー (PCH) を構築するという最悪のシナリオを考えてみましょう。

| バージョン | PCH のサイズ (バイト) | 回数 |
| - | - | - |
| 7 月からの C++/WinRT、および Visual C++ 16.3 | 3,004,104,632 | 31 |
| バージョン 2.0.200316.3 の C++/WinRT、および Visual C++ 16.5 | 2,393,515,336 | 24 |

サイズが 20% 削減され、ビルド時間が 23% 短縮されます。

### <a name="improved-msbuild-support"></a>MSBuild サポートの向上

さまざまなシナリオを対象とした [MSBuild](/visualstudio/msbuild/msbuild) サポートを向上させるために、多くの取り組みを行ってきました。

### <a name="even-faster-factory-caching"></a>ファクトリ キャッシュのさらなる高速化

インライン ホット パスを向上させるために、ファクトリ キャッシュのインライン化を改善し、より高速な実行を実現しました。

&mdash;後述の[最適化された EH コード生成](#optimized-exception-handling-eh-code-generation)の説明にあるように、この改善はコード サイズには影響せず、アプリケーションで C++ 例外処理を頻繁に使用する場合は、Visual Studio 2019 16.3 以降で作成された新しいプロジェクトで既定でオンになっている `/d2FH4` オプションを使用してバイナリを縮小できます。

### <a name="more-efficient-boxing"></a>より効率的なボックス化

XAML アプリケーションで使用する場合、[**winrt:: box_value**](/uwp/cpp-ref-for-winrt/box-value) がより効率的になりました ([ボックス化とボックス化解除](./boxing.md)に関するページを参照してください)。 また、多数のボックス化を実行するアプリケーションでは、コード サイズも小さくなります。

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>IInspectable を実装する COM インターフェイスの実装のサポート

[**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) を実装するためだけに (Windows ランタイム以外の) COM インターフェイスを実装する必要がある場合は、C++/WinRT を使用してそうすることができます。 [IInspectable を実装する COM インターフェイス](https://github.com/microsoft/xlang/pull/603)に関するページを参照してください。

### <a name="module-locking-improvements"></a>モジュール ロックの機能強化

モジュール ロックの制御により、カスタム ホスティング シナリオと、モジュール レベル ロックの完全な排除の両方が可能になりました。 [モジュール ロックの機能強化](https://github.com/microsoft/xlang/pull/583)に関するページを参照してください。

### <a name="support-for-non-windows-runtime-error-information"></a>Windows ランタイム以外のエラー情報のサポート

一部の API では (一部の Windows ランタイム API でも)、Windows ランタイム エラー発生 API を使用せずにエラーが報告されます。 このような場合、C++/WinRT では COM エラー情報が使用されるようになりました。 [WinRT 以外のエラー情報に対する C++/WinRT サポート](https://github.com/microsoft/xlang/pull/582)に関するページを参照してください。

### <a name="enable-c-module-support"></a>C++ モジュール サポートの有効化 

C++ モジュール サポートが再度有効になりましたが、まだ試験的な段階にすぎません。 この機能は、C++ コンパイラではまだ完全ではありません。

### <a name="more-efficient-coroutine-resumption"></a>コルーチン再開のさらなる効率化

C++/WinRT コルーチンは既に良好なパフォーマンスを発揮していますが、さらに向上させるための方法の調査は引き続き行われています。 [コルーチン再開のスケーラビリティの向上](https://github.com/microsoft/xlang/pull/546)に関するページを参照してください。

### <a name="new-when_all-and-when_any-async-helpers"></a>新しい非同期ヘルパー **when_all** および **when_any**

**when_all** ヘルパー関数は、指定されたすべての awaitable が完了したときに完了する [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) オブジェクトを作成します。 **when_any** ヘルパーは、指定された awaitable のいずれかが完了したときに完了する **IAsyncAction** を作成します。 

[when_any 非同期ヘルパーの追加](https://github.com/microsoft/xlang/pull/520)および [when_all 非同期ヘルパーの追加](https://github.com/microsoft/xlang/pull/516)に関するページを参照してください。

### <a name="other-optimizations-and-additions"></a>その他の最適化と追加

さらに、多数のバグ修正、軽度の最適化、および追加が導入されました。これには、デバッグを簡略化し、内部および既定の実装を最適化するためのさまざまな機能強化が含まれています。 完全な一覧については、こちらのリンクを参照してください。[https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed)

## <a name="news-and-changes-in-cwinrt-20"></a>C++/WinRT 2.0 の新機能と変更点

[C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)、[Microsoft.Windows.CppWinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)、`cppwinrt.exe` ツールの入手やインストール方法などの詳細については、「[Visual Studio のサポートの C +/cli WinRT、XAML、VSIX 拡張機能と NuGet パッケージ](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)」を参照してください。

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>バージョン 2.0 の C++/WinRT Visual Studio Extension (VSIX) の変更点

- デバッグ ビジュアライザーは、引き続き Visual Studio 2017 をサポートするのに加えて、Visual Studio 2019 をサポートするようになります。
- 多くのバグ修正が行われました。

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>バージョン 2.0 の Microsoft.Windows.CppWinRT NuGet パッケージの変更点

- 必要に応じてプロジェクトごとにプラットフォーム プロジェクション ヘッダーを生成する `cppwinrt.exe` ツールは、Microsoft.Windows.CppWinRT NuGet パッケージに含まれるようになります。 その結果、`cppwinrt.exe` ツールは Windows SDK に依存しなくなります (ただし、互換性の理由から、ツールには引き続き SDK が付属しています)。
- 並列ビルドを有効にするため、`cppwinrt.exe` では各プラットフォーム/構成固有の中間フォルダー ($IntDir) の下に、プロジェクション ヘッダーが生成されるようになりました。
- プロジェクト ファイルを手動でカスタマイズする場合のため、C++/WinRT のビルドのサポート (プロパティ/ターゲット) が完全にドキュメント化されるようになりました。 Microsoft.Windows.CppWinRT NuGet パッケージの [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) をご覧ください。
- 多くのバグ修正が行われました。

### <a name="changes-to-cwinrt-for-version-20"></a>バージョン 2.0 での C++/WinRT の変更点

#### <a name="open-source"></a>オープン ソース

`cppwinrt.exe` ツールでは、Windows ランタイム メタデータ (`.winmd`) ファイルが取得され、それに基づいて、メタデータで記述されている API を "*プロジェクト*" するヘッダー ファイル ベースの標準 C++ ライブラリが生成されます。 それにより、C++/WinRT のコードからそれらの API を使用できます。

このツールは完全なオープン ソース プロジェクトになり、GitHub で入手できます。 [Microsoft\/cppwinrt](https://github.com/microsoft/cppwinrt) にアクセスしてください。

#### <a name="xlang-libraries"></a>xlang ライブラリ

完全に移植可能なヘッダーのみのライブラリ (Windows ランタイムで使用される ECMA-335 メタデータ形式の解析用) により、今後のすべての Windows ランタイムおよび xlang ツールの基礎が形成されます。 特に、`cppwinrt.exe` ツールは xlang ライブラリを使ってまったく新しく書き直しました。 これにより、メタデータのより正確なクエリが提供されるようになり、C++/WinRT 言語プロジェクションに関する長期にわたるいくつかの問題が解決されます。

#### <a name="fewer-dependencies"></a>依存関係の数の削減

xlang メタデータ リーダーにより、`cppwinrt.exe` ツール自体の依存関係の数が少なくなります。 これにより、柔軟性が増し、より多くのシナリオ (特に、制限の厳しいビルド環境) で使用できるようになります。 特に、`RoMetadata.dll` に依存しなくなります。
 
`cppwinrt.exe` 2.0 での依存関係は次のとおりです。
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

Windows 10 だけでなく、Windows 7 までの旧バージョンや Windows Vista でも、これらの DLL をすべて利用できます。 そうしたければ、Windows 7 を実行中の古いビルド サーバーでも、`cppwinrt.exe` を実行して、プロジェクトの C++ ヘッダーを生成できるようになりました。 関心がある場合は、手間はかかりますが、[Windows 7 上で C++/WinRT を実行する](https://github.com/kennykerr/win7)こともできます。

上記の一覧を、次の `cppwinrt.exe` 1.0 の依存関係と比べてください。

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows ランタイムの `noexcept` 属性

Windows ランタイムの新しい `[noexcept]` 属性を使うと、[MIDL 3.0](/uwp/midl-3/predefined-attributes) のメソッドとプロパティを修飾することができます。 この属性が存在すると、実装で例外がスローされない (エラー HRESULT も返されない) ことをサポート ツールに示します。 これにより、言語プロジェクションでは、失敗する可能性のあるアプリケーション バイナリ インターフェイス (ABI) の呼び出しをサポートするために必要な例外処理のオーバーヘッドを回避することで、コードの生成を最適化することができます。

C++/WinRT では、使用コードと作成コード両方の C++ `noexcept` の実装を生成することによって、これを利用します。 エラーが発生しない API のメソッドまたはプロパティがあり、コードのサイズを考慮することが必要な場合は、この属性を検討できます。

#### <a name="optimized-code-generation"></a>最適化されたコード生成

C++ コンパイラで可能な限り小さくて効率的なバイナリ コードを生成できるよう、C++/WinRT ではいっそう効率的な C++ ソース コードが (バックグラウンドで) 生成されるようになります。 不要なアンワインド情報を回避することで、例外処理のコストの削減を目的とする多くの機能強化が行われています。 大量の C++WinRT コードを使うバイナリでは、コードのサイズが約 4% 削減されます。 また、命令の数が減るため、コードがいっそう効率的に (より速く実行されるように) なります。

これらの機能強化は、使用可能な新しい相互運用機能にも依存します。 リソース所有者であるすべての C++/WinRT 型には、前の 2 つのステップのアプローチを回避し、所有権を直接取得するためのコンストラクターが含まれます。

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>最適化された例外処理 (EH) コード生成

この変更は、Microsoft C++ オプティマイザー チームによって行われた作業を補完して、例外処理のコストを削減します。 アプリケーション バイナリ インターフェイス (ABI) (COM など) をコード内で大量に使う場合、このパターンに従うコードを多く目にします。

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/WinRT 自体では、実装されているすべての API に対してこのパターンが生成されます。 何千もの API 関数において、ここでの最適化が重要である可能性があります。 これまで、オプティマイザーではそれらの catch ブロックがすべて同じであることは検出されなかったため、各 ABI で多くのコードが重複していました (それは、システム コードで例外を使うと大きなバイナリが生成されるという確信の原因になっていました)。 しかし、Visual Studio 2019 以降の C++ コンパイラでは、それらのすべての catch funclet が折りたたまれていて、固有のものだけが格納されます。 結果として、このパターンに大きく依存するバイナリのコード サイズは全体的としてさらに 18% 減少します。 EH コードがリターン コードを使うより効率的になっただけでなく、大きいバイナリ ファイルに関する心配は過去のものになりました。

#### <a name="incremental-build-improvements"></a>インクリメンタル ビルドの機能強化

`cppwinrt.exe` ツールでは、生成されたヘッダー/ソース ファイルの出力がディスク上の既存のファイルの内容と比較されて、ファイルが実際に変更されている場合にのみ、ファイルが書き込まれます。 これにより、ディスク I/O の時間がかなり短縮され、C++ コンパイラによってファイルが "ダーティ" と見なされないことが保証されます。 結果として、多くの場合に、再コンパイルが回避または縮小されます。

#### <a name="generic-interfaces-are-now-all-generated"></a>ジェネリック インターフェイスがすべて生成されるようになった

xlang メタデータ リーダーのため、C++/WinRT では、すべてのパラメーター化された、またはジェネリック インターフェイスがメタデータから生成されるようになりました。 [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) などのインターフェイスは、`winrt/base.h` の手書きからではなく、メタデータから生成されるようになります。 その結果、`winrt/base.h` のサイズが半分になり、最適化がコードですぐに生成されます (手書きのアプローチではこれを行うのは大変でした)。

> [!IMPORTANT]
> 示されている例のようなインターフェイスは、`winrt/base.h` ではなく、それぞれの名前空間のヘッダーに存在するようになります。 そのため、まだそのようにしていない場合、インターフェイスを使うには、適切な名前空間のヘッダーをインクルードする必要があります。

#### <a name="component-optimizations"></a>コンポーネントの最適化

この更新では、以下のセクションで説明するように、C++/WinRT に対して複数の追加オプトイン最適化のサポートが追加されています。 これらの最適化は破壊的変更なので (サポートするために小さな変更を行う必要があります)、明示的に有効にする必要があります。 Visual Studio で、プロジェクトのプロパティ **[共通プロパティ]**  >  **[C++/WinRT]**  >  **[最適化]** を *[はい]* に設定します。 その効果として、プロジェクト ファイルに `<CppWinRTOptimized>true</CppWinRTOptimized>` が追加されます。 また、コマンド ラインから `cppwinrt.exe` を呼び出すときに `-opt[imize]` スイッチを追加するのと同じ効果があります。

(プロジェクト テンプレートからの) 新しいプロジェクトでは、`-opt` が既定で使われます。

##### <a name="uniform-construction-and-direct-implementation-access"></a>均一コンストラクション、実装への直接アクセス

これら 2 つの最適化により、コンポーネントでは、プロジェクションが実行された型のみを使う場合であっても、独自の実装型に直接アクセスできます。 パブリック API サーフェスを使うだけの場合は、[**make**](/uwp/cpp-ref-for-winrt/make)、[**make_self**](/uwp/cpp-ref-for-winrt/make-self)、[**get_self**](/uwp/cpp-ref-for-winrt/get-self) を使う必要はありません。 呼び出しは実装の直接呼び出しにコンパイルされ、完全にインライン化される可能性さえあります。

詳細とコード例については、[均一の構築と実装への直接アクセスへのオプトイン](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access)に関する記事を参照してください。

##### <a name="type-erased-factories"></a>型消去されたファクトリ

この最適化により、`module.g.cpp` に #include の依存関係がなくなり、1 つの実装クラスで変更が発生するたびに再コンパイルをする必要がなくなります。 結果として、ビルドのパフォーマンスが向上します。

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>複数のライブラリを含む大規模なプロジェクトに対する `module.g.cpp` のスマートさと効率の向上

`module.g.cpp` ファイルには、**winrt_can_unload_now** および **winrt_get_activation_factory** という名前の 2 つの追加構成可能なヘルパーも含まれるようになっています。 これらは、それぞれに独自のランタイム クラスが含まれる多数のライブラリで DLL が構成される、大規模なプロジェクト向けに設計されています。 そのような状況では、DLL の **DllGetActivationFactory** と **DllCanUnloadNow** を手動でまとめる必要があります。 これらのヘルパーにより、見かけ上の実行元のエラーが回避されて、それを行うのがはるかに簡単になります。 `cppwinrt.exe` ツールの `-lib` フラグを使って個々のライブラリに (`winrt_xxx` ではなく) 独自のプリアンブルを提供し、各ライブラリの関数に個別の名前を付けて、明確に組み合わせられるようにすることもできます。

#### <a name="coroutine-support"></a>コルーチンのサポート

コルーチンのサポートは、自動的に組み込まれます。 これまでは、サポートは複数の場所に存在しており、制限が厳しすぎると考えられていました。 その後、V2.0 では一時的に `winrt/coroutine.h` ヘッダー ファイルが必要でしたが、それはもう必要なくなっています。 Windows ランタイムの非同期インターフェイスは現在では手書きではなく生成されるようになったため、`winrt/Windows.Foundation.h` に存在しています。 保守性とサポート性の向上とは別に、[**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) などのコルーチン ヘルパーを特定の名前空間ヘッダーの末尾に付加する必要がなくなったことを意味します。 代わりに、もっと自然に依存関係を組み込むことができます。 これにより、さらに **resume_foreground** では特定の [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) での再開をサポートできるだけでなく、特定の [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) での再開もサポートできるようになります。 以前は、定義は 1 つの名前空間にのみ存在できたため、両方ではなく、1 つのみをサポートできました。

**DispatcherQueue** のサポートの例を次に示します。

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

コルーチン ヘルパーも `[[nodiscard]]` で修飾されるようになったため、使いやすさが向上しています。 それらが動作するための `co_await` を忘れた (または、必要であることを知らなかった) 場合、`[[nodiscard]]` のため、このような誤りではコンパイラの警告が発生します。

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>直接 (スタック) 割り当ての診断でのヘルプ

投影されたクラスと実装クラスの名前は (既定では) 同じで、名前空間のみが異なるため、間違えることがあり、ヘルパーの [**make**](/uwp/cpp-ref-for-winrt/make) ファミリではなく、スタック上に実装を誤って作成する可能性があります。 これは、未解決の参照がまだ処理中にオブジェクトが破棄される可能性があるため、場合によっては診断が困難です。 デバッグ ビルドでは、アサーションによってこれが選択されるようになりました。 アサーションではコルーチン内のスタック割り当ては検出されませんが、それでもそのような誤りのほとんどを把握するのに役立ちます。

詳細については、「[直接割当ての診断](./diag-direct-alloc.md)」を参照してください。

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>強化されたキャプチャ ヘルパーと可変個引数デリゲート

この更新では、プロジェクションが実行された型もサポートすることによって、キャプチャ ヘルパーに関する制限が修正されています。 これは、プロジェクションが実行された型を返すときに、Windows ランタイムの相互運用 API で発生することがあります。

また、この更新では、可変個引数 (非 Windows ランタイム) のデリゲートを作成するときの [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) と [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) のサポートも追加されています。

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>遅延破棄および破棄中の安全な QI のサポート

ランタイム クラスのオブジェクトのデストラクターでは、参照カウントを一時的に増加させるメソッドが呼び出されることは珍しくありません。 参照カウントが 0 に戻ると、オブジェクトで 2 回目の破棄が行われます。 XAML アプリケーションでは、階層の上位または下位にあるクリーンアップの実装を呼び出すために、デストラクター内で [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) を実行することが必要な場合があります。 しかし、オブジェクトの参照カウントは既に 0 になっているので、その QI も参照カウントのバウンスを構成します。

この更新には、0 に達した後で再実行されないようにする、参照カウントのデバウンスのサポートが追加されています。それでも、破棄中に必要な一時的な QI は許可されます。 この手順は特定の XAML アプリケーション/コントロールでは避けられず、C++/WinRT はそれに対する回復力を持つようになっています。

実装型で静的な **final_release** 関数を提供することにより、破棄を遅延できます。 最後に残ったオブジェクトへのポインターは、**std::unique_ptr** の形式で、**final_release** に渡されます。 その後、そのポインターの所有権を他のコンテキストに移動できます。 二重破棄をトリガーすることなくポインターに対して QI を行っても安全です。 ただし、オブジェクトを破棄する時点では、参照カウントに対する正味の変更が 0 になっている必要があります。

**final_release** の戻り値は、`void`、[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) などの非同期操作オブジェクト、または **winrt::fire_and_forget** にすることができます。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

次の例では、**MainPage** が解放されると (最終回で)、**final_release** が呼び出されます。 その関数は (スレッド プールで) 5 秒間待機してから、ページの **ディスパッチャー** を使って再開します (動作するには QI/AddRef/Release が必要です)。 その後、その UI スレッドでリソースをクリーンアップします。 最後に、**unique_ptr** がクリアされることで、**MainPage** のデストラクターが実際に呼び出されます。 そのデストラクター内でも、**DataContext** が呼び出され、**IFrameworkElement** に対する QI が必要です。

コルーチンとしてとして独自の **final_release** を実装する必要はありません。 しかし、それは動作し、非常に簡単に破棄を別のスレッドに移動できます。この例では、それが行われています。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

詳細については、[遅延破棄](./details-about-destructors.md#deferred-destruction)に関する記事を参照してください。

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>COM スタイルの単一インターフェイス継承に対するサポートの向上

Windows ランタイム プログラミングだけでなく、C++/WinRT は COM 専用 API の作成と使用にも使われます。 この更新では、インターフェイス階層が存在する COM サーバーを実装できるようになります。 これには Windows ランタイムには必要ありませんが、一部の COM 実装には必要です。

#### <a name="correct-handling-of-out-params"></a>`out` パラメーターの適切な処理

`out` パラメーターの処理は難しいことがあります。Windows ランタイム配列の場合は特にそうです。 この更新では、`out` のパラメーターと配列に関する誤りに対する C++/WinRT の信頼性と回復性がかなり高くなっています。それらのパラメーターが言語プロジェクションからのものか、生の ABI を使用している COM 開発者からのものか、変数の初期化が一貫していない誤りを誰が行ったかには関係ありません。 いずれの場合も、C++WinRT では、ABI に対するプロジェクションが実行された型の引き渡し (リソースの解放を記憶することで)、および ABI で到着したパラメーターのゼロ化またはクリアが、適切に処理されます。

#### <a name="events-now-handle-invalid-tokens-reliably"></a>イベントで確実に処理されるようになった無効なトークン

[**winrt::event**](/uwp/cpp-ref-for-winrt/event) の実装では、無効なトークン値 (配列に存在しない値) で **remove** メソッドが呼び出された場合が正しく処理されるようになります。

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>コルーチンが戻る前にコルーチンのローカル変数が破棄される

コルーチン型の従来の実装方法では、コルーチン内のローカル変数を (最後の中断の前ではなく) コルーチンのリターン/完了の "*後*" で破棄することができました。 この問題を回避するためと他のメリットのため、待機処理の再開は最後の中断まで延期されるようになりました。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Windows SDK バージョン 10.0.17763.0 (Windows 10 バージョン 1809) での新機能と変更点

次の表では、Windows SDK バージョン 10.0.17763.0 (Windows 10 バージョン 1809) での C++/WinRT の新機能と変更点を示します。

| 新機能または変更された機能 | 詳細情報 |
| - | - |
| **破壊的変更**。 コンパイルで、C++/WinRT は Windows SDK からのヘッダーに依存しません。 | 後の「[Windows SDK ヘッダー ファイルからの分離](#isolation-from-windows-sdk-header-files)」をご覧ください。 |
| Visual Studio プロジェクト システムの形式が変更されました。 | 後の「[C++/WinRT プロジェクトのターゲットを Windows SDK の後のバージョンに変更する方法](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)」をご覧ください。 |
| Windows ランタイム関数にコレクション オブジェクトを渡すため、または独自のコレクション プロパティとコレクション型を実装するための、新しい関数と基底クラスがあります。 | 「[C++/WinRT でのコレクション](collections.md)」をご覧ください。 |
| C++/WinRT ランタイム クラスで [{binding}](../xaml-platform/binding-markup-extension.md) マークアップ拡張機能を使用できます。 | 詳細とコード例については、「[データ バインディングの概要](../data-binding/data-binding-quickstart.md)」をご覧ください。 |
| コルーチンの取り消しのサポートにより、取り消しコールバックを登録できます。 | 詳細とコード例については、「[非同期操作の取り消しとキャンセル コールバック](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks)」をご覧ください。 |
| メンバー関数を指し示すデリゲートを作成するとき、ハンドラーを登録する時点で、現在のオブジェクト (生の *this* ポインターのインスタンス) に対する強い参照または弱い参照を確立できます。 | 詳細およびコード例については、「[イベント処理デリゲートで *this* ポインターに安全にアクセスする](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)」セクションの「**デリゲートとしてメンバー関数を使用する場合**」サブセクションをご覧ください。 |
| Visual Studio の C++ 標準への適合性が向上することによって発見されたバグが修正されました。 C++/WinRT の標準準拠の検証に対する LLVM および Clang ツールチェーンの利用も向上しています。 | 次の記事で説明されいている問題が発生しなくなります。[Why won't my new project compile?I'm using Visual Studio 2017 (version 15.8.0 or higher), and SDK version 17134 (新しいプロジェクトがコンパイルされない理由: Visual Studio 2017 (バージョン 15.8.0 以降) と SDK バージョン 17134 を使用している場合)](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

その他の変更点。

- **破壊的変更**。 [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) で、`HSTRING` ではなく `void*` が返されるようになります。 `static_cast<HSTRING>(get_abi(my_hstring));` を使って HSTRING を取得できます。 「[ABI の HSTRING との相互運用](interop-winrt-abi.md#interoperating-with-the-abis-hstring)」をご覧ください。
- **破壊的変更**。 [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) で、`HSTRING*` ではなく `void**` が返されるようになります。 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` を使って HSTRING* を取得できます。 「[ABI の HSTRING との相互運用](interop-winrt-abi.md#interoperating-with-the-abis-hstring)」をご覧ください。
- **破壊的変更**。 HRESULT が **winrt::hresult** として投影されるようになります。 HRESULT が必要な場合 (型チェックの実行または型の特徴のサポートのため)、**winrt::hresult** を `static_cast` できます。 それ以外の場合、C++/WinRT ヘッダーをインクルードする前に `unknwn.h` がインクルードされていると、**winrt::hresult** は HRESULT に変換されます。
- **破壊的変更**。 GUID が **winrt::guid** として投影されるようになります。 実装する API の場合、GUID パラメーターに対して **winrt::guid** を使用する必要があります。 それ以外の場合、C++/WinRT ヘッダーをインクルードする前に `unknwn.h` がインクルードされていると、**winrt::guid** は GUID に変換されます。 「[ABI の GUID 構造体との相互運用](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct)」をご覧ください。
- **破壊的変更**。 [**winrt::handle_type コンストラクター**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) は、明示的にすることで強化されています (それで不適切なコードを記述することが難しくなっています)。 生のハンドル値を割り当てる必要がある場合は、代わりに [**handle_type::attach 関数**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) を呼び出します。
- **破壊的変更**。 **WINRT_CanUnloadNow** と **WINRT_GetActivationFactory** のシグネチャが変更されています。 これらの関数は宣言しないでください。 代わりに、`winrt/base.h` をインクルードしてこれらの関数の宣言を含めます (いずれかの C++/WinRT Windows 名前空間ヘッダー ファイルをインクルードすると、自動的にインクルードされます)。
- [**winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock) では、**from_FILETIME/to_FILETIME** は非推奨になり、**from_file_time/to_file_time** を代わりに使用します。
- **IBuffer** パラメーターを必要とする簡素化された API。 ほとんどの API ではコレクションまたは配列が優先されますが、 **IBuffer** に依存する API を簡単に呼び出せるようにする必要がありました。 この更新では、C++ 標準ライブラリ コンテナーによって使用されるのと同じデータ名前付け規則を使用して、**IBuffer** 実装の背後にあるデータに直接アクセスできます。 この規則により、慣例的に大文字で始まるメタデータの名前との競合も回避されます。
- コード生成の向上: コード サイズの削減、インライン化の向上、ファクトリ キャッシュの最適化に関するさまざまな改善。
- 不要な再帰を削除しました。 コマンド ラインで特定の `.winmd` ではなくフォルダーを参照するとき、`cppwinrt.exe` ツールで `.winmd` ファイルが再帰的に検索されなくなります。 `cppwinrt.exe` ツールでは重複の処理もいっそうインテリジェントになり、ユーザー エラーや不適切な形式の `.winmd` ファイルに対する回復性が向上しています。
- 強化されたスマート ポインター。 以前は、新しい値を移動割り当てすると、イベント リボーカーは失効に失敗しました。 これは、スマート ポインター クラスが自己割り当てを確実に処理しない問題を発見するのに役立ちました。原因は [**winrt::com_ptr 構造体テンプレート**](/uwp/cpp-ref-for-winrt/com-ptr)でした。 **winrt::com_ptr** を修正しました。また、イベント リボーカーが移動セマンティクスを正しく処理するように修正され、割り当て時に失効するようになりました。

> [!IMPORTANT]
> バージョン 1.0.181002.2 とバージョン 1.0.190128.4 の両方で、[C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) に対して重要な変更が行われました。 これらの変更の詳細および既存プロジェクトへの影響については、[C++/WinRT の Visual Studio サポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関するページおよび「[VSIX 拡張機能の以前のバージョン](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)」をご覧ください。

### <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK ヘッダー ファイルからの分離

これは、コードに対する破壊的変更になる可能性があります。

C++/WinRT は、コンパイルで Windows SDK からのヘッダー ファイルに依存しなくなります。 C ランタイム ライブラリ (CRT) および C++ 標準テンプレート ライブラリ (STL) のヘッダー ファイルでも、Windows SDK ヘッダーがインクルードされなくなります。 それにより、標準への準拠が向上し、不注意による依存関係が回避され、防ぐ必要のあるマクロの数が大幅に減ります。

この独立性は、C++/WinRT の移植性と標準準拠の向上を意味し、クロスコンパイラおよびクロスプラットフォーム ライブラリになる可能性を促進します。 また、C++/WinRT ヘッダーがマクロに悪影響を及ぼさないことも意味します。

以前はプロジェクトへの Windows ヘッダーのインクルードを C++/WinRT に任せていた場合、自分でインクルードすることが必要になります。 いずれの場合も常に、依存するヘッダーを明示的にインクルードし、別のライブラリによって自動的にインクルードされるままにしないのが、ベスト プラクティスです。

現時点では、Windows SDK ヘッダー ファイルの分離に対する例外は、組み込み関数と数値だけです。 これらの最後に残っている依存関係に関する既知の問題はありません。

プロジェクトでは、必要に応じて、Windows SDK ヘッダーとの相互運用を再度有効にできます。 たとえば、COM インターフェイスを実装することが必要な場合があります、(原因は [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown))。 その例では、すべての C++/WinRT ヘッダーをインクルードする前に、`unknwn.h` をインクルードします。 そのようにすると、C++/WinRT の基本ライブラリで、クラシック COM インターフェイスをサポートするためのさまざまなフックが有効になります。 コードの例については、「[C++/WinRT での COM コンポーネントの作成](author-coclasses.md)」をご覧ください。 同様に、呼び出そうとする型や関数が宣言されている他の Windows SDK ヘッダーを明示的にインクルードします。

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>C++/WinRT プロジェクトのターゲットを Windows SDK の後のバージョンに変更する方法

コンパイラとリンカーの問題が最も少なくなるようにプロジェクトのターゲットを変更する方法は、手間も最もかかります。 その方法では、新しいプロジェクトを (選択した Windows SDK のバージョンをターゲットにして) 作成した後、古いプロジェクトから新しいプロジェクトにファイルをコピーします。 古い `.vcxproj` および `.vcxproj.filters` ファイルには、コピーして Visual Studio のファイルに保存して追加するだけでよいセクションがあります。

ただし、Visual Studio でプロジェクトのターゲットを変更するには、他に 2 つの方法があります。

- プロジェクトのプロパティ **[全般]** \> **[Windows SDK バージョン]** に移動し、 **[すべての構成]** と **[すべてのプラットフォーム]** を選択します。 **[Windows SDK バージョン]** を、ターゲットにするバージョンに設定します。
- **ソリューション エクスプローラー** で、プロジェクト ノードを右クリックし、 **[プロジェクトの再ターゲット]** をクリックして、ターゲットにするバージョンを選択してから、 **[OK]** をクリックします。

これら 2 つの方法のいずれかを使用した後で、コンパイラ エラーまたはリンカー エラーが発生する場合は、もう一度ビルドを試みる前に、ソリューションをクリーニングしてみることができます ( **[ビルド]**  >  **[ソリューションのクリーン]** を選択するか、すべての一時フォルダーとファイルを手動で削除します)。

C++ コンパイラで "*エラー C2039: 'IUnknown': '\`<グローバル名前空間>'' のメンバーではありません*" が発生する場合は、`#include <unknwn.h>` を `pch.h` ファイルの先頭に追加します (すべての C++/WinRT ヘッダーをインクルードする前)。

その後で、`#include <hstring.h>` の追加も必要な場合があります。

C++ リンカーで "*エラー LNK2019:関数 _VSDesignerCanUnloadNow@0 で参照されている外部シンボル _WINRT_CanUnloadNow@0 は未解決です*" が発生する場合は、`#define _VSDESIGNER_DONT_LOAD_AS_DLL` を `pch.h` ファイルに追加することで解決できます。
