---
description: C++/WinRT での Windows ランタイム API の作成と使用に関する質問への回答です。
title: C++/WinRT についてよく寄せられる質問
ms.date: 04/23/2019
ms.topic: article
keywords: wwindows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 頻繁, 質問, 質問, faq
ms.localizationpriority: medium
ms.openlocfilehash: b1a05dbb666b33739a083c517395359a64fee9df
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297656"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>C++/WinRT についてよく寄せられる質問
[C++/WinRT](./intro-to-using-cpp-with-winrt.md) での Windows ランタイム API の作成と使用に関する質問への回答です。

> [!IMPORTANT]
> C++/WinRT に関するリリース ノートについては、「[C++/WinRT 2.0 の新機能と変更点](news.md#news-and-changes-in-cwinrt-20)」を参照してください。

> [!NOTE]
> 質問の内容が、表示されたエラー メッセージに関するものである場合は、「[C++/WinRT に関する問題のトラブルシューティング](troubleshooting.md)」のトピックも参照してください。

## <a name="where-can-i-find-cwinrt-sample-apps"></a>C++/WinRT サンプル アプリはどこにありますか?
[C++/WinRT サンプル アプリ](/samples/browse/?languages=cppwinrt)に関するページを参照してください。

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>C++/WinRT プロジェクトのターゲットを Windows SDK の後のバージョンに変更するにはどうすればよいですか?
「[C++/WinRT プロジェクトのターゲットを Windows SDK のより新しいバージョンに変更する方法](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)」を参照してください。

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>C++/WinRT 2.0 に移行した後、新しいプロジェクトがコンパイルされないのはなぜですか?
詳細な変更点 (破壊的変更を含む) については、「[News, and changes, in C++/WinRT 2.0　(C++/WinRT 2.0 の新機能と変更点)](news.md#news-and-changes-in-cwinrt-20)」を参照してください。 たとえば、Windows ランタイム コレクションで範囲ベースの `for` を使用している場合は、`#include <winrt/Windows.Foundation.Collections.h>` が必要になります。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>新しいプロジェクトがコンパイルされないのはなぜですか? 私は Visual Studio 2017 (Version 15.8.0 以降) と SDK Version 17134 を使用しています
Visual Studio 2017 (Version 15.8.0 以降) を使用し、Windows SDK Version 10.0.17134.0 (Windows 10 Version 1803) をターゲットにしている場合は、C++/WinRT プロジェクトを新しく作成すると、エラー " *'エラー C3861: 'from_abi': 識別子が見つかりませんでした*" と、*base.h* から発生する他のエラーでコンパイルに失敗することがあります。 これを解決するには、より新しい (より準拠した) バージョンの Windows SDK をターゲットにするか、プロジェクトのプロパティを **[C/C++]**  >  **[言語]**  >  **[準拠モード]:[いいえ]** に設定します (また、プロジェクトのプロパティの **[その他のオプション]** の **[C/C++]**  >  **[コマンド ライン]** に **/permissive-** が表示される場合は、それを削除します)。

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>ビルド エラー "The C++/WinRT VSIX no longer provides project build support.  Please add a project reference to the Microsoft.Windows.CppWinRT Nuget package" (C++/WinRT VSIX はプロジェクトのビルドをサポートしなくなりました。Microsoft.Windows.CppWinRT Nuget パッケージへのプロジェクト参照を追加してください) を解決するにはどうすればよいですか?
**Microsoft.Windows.CppWinRT** NuGet パッケージをプロジェクトにインストールします。 詳細については、「[VSIX 拡張機能の以前のバージョン](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)」を参照してください。

## <a name="how-do-i-customize-the-build-support-in-the-nuget-package"></a>NuGet パッケージでビルド サポートをカスタマイズするにはどうすればよいですか?

C++/WinRT ビルド サポート (プロパティ/ターゲット) については、Microsoft.Windows.CppWinRT NuGet パッケージの [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) に記載されています。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>C++/WinRT Visual Studio Extension (VSIX) の要件は何ですか?
VSIX 拡張機能の Version 1.0.190128.4 以降については、[C++/WinRT の Visual Studio のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。 その他のバージョンについては、「[VSIX 拡張機能の以前のバージョン](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)」を参照してください。

## <a name="whats-a-runtime-class"></a>*ランタイム クラス*とは
ランタイム クラスは、通常実行可能な境界を越えて、モダン COM インターフェイス経由でアクティブ化および使用可能な型です。 ただし、ランタイム クラスは、それを実装するコンパイル ユニット内でも使用できます。 インターフェイス定義言語 (IDL) でランタイム クラスを宣言し、C++/WinRT を使用した標準の C++ で実装することができます。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*プロジェクションされる型*と*実装型*とは
Windows ランタイム クラス (ランタイム クラス) を*使用*する場合のみ、*プロジェクションされる型*を排他的に処理します。 C++/WinRT は*言語のプロジェクション*であるため、プロジェクションされる型は、C++/WinRT を使用した C++ に*プロジェクション*される Windows ランタイムの画面に表示されます。 詳細については、「[C++/WinRT での API の使用](consume-apis.md)」を参照してください。

*実装型*にはランタイム クラスの実装が含まれるため、ランタイム クラスを実装するプロジェクトでのみ利用可能です。 ランタイム クラス (Windows ランタイム コンポーネント プロジェクト、つまり XAML UI を使用するプロジェクト) を実装するプロジェクトで作業している場合、ランタイム クラスの実装型と C++/WinRT にプロジェクションされたランタイム クラスを表すプロジェクションされる型との違いを習熟することが重要です。 詳細については、「[C++/WinRT での API の作成](author-apis.md)」を参照してください。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>使用するランタイム クラスの IDL でコンストラクターを宣言する必要性
ランタイム クラスが実装するコンパイル ユニット (Windows ランタイム クライアント アプリで一般的に使用するための Windows ランタイム コンポーネント) 以外で使用されるように設計されている場合のみ IDL のコンストラクターを宣言する際の目的と影響の詳細については、「[ランタイム クラス コンストラクター](author-apis.md#runtime-class-constructors)」を参照してください。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>リンカーで "LNK2019:Unresolved external symbol" (外部シンボルは未解決です) エラーが発生するのはなぜですか?
未解決のシンボルが (**winrt** 名前空間内の) C++/WinRT プロジェクションの Windows 名前空間ヘッダーからの API である場合、その API は含まれているヘッダー内で事前宣言されていますが、その定義は含まれていないヘッダー内にあります。 API の名前空間で付けられた名前のヘッダーを含めてから、リビルドしてください。 詳細については、「[C++/WinRT プロジェクション ヘッダー](consume-apis.md#cwinrt-projection-headers)」を参照してください。

未解決のシンボルが [RoInitialize](/windows/desktop/api/roapi/nf-roapi-roinitialize) などの Windows ランタイムの自由関数である場合、[WindowsApp.lib](/uwp/win32-and-com/win32-apis) の包括的なライブラリを明示的にプロジェクトにリンクする必要があります。 C++/WinRT プロジェクションは、これらの一部の自由 (非メンバー) 関数とエントリ ポイントに依存します。 アプリケーションでいずれかの [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) プロジェクト テンプレートを使用する場合は、`WindowsApp.lib` が自動的にリンクされます。 それ以外の場合、プロジェクトのリンク設定を使用して含めるか、またはソース コードでそれを行うことができます。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

代替の静的リンク ライブラリではなく **WindowsApp.lib** をリンクしてリンカー エラーを解決することが重要です。そうしないと、アプリケーションは、Visual Studio および Microsoft Store で提出の検証に使用される [Windows アプリ認定キット](../debug-test-perf/windows-app-certification-kit.md) テストに合格しません (つまり、結果として、アプリケーションは Microsoft Store に正常に取り込まれません)。

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>"クラスが登録されていません" という例外が発生するのはなぜですか?

この場合の現象としては、ランタイム クラスを構築するか、または静的メンバーにアクセスすると、実行時に HRESULT の値が REGDB_E_CLASSNOTREGISTERED である例外がスローされます。

原因の 1 つは、Windows ランタイム コンポーネントを読み込むことができないことです。 コンポーネントの Windows ランタイム メタデータ ファイル (`.winmd`) の名前がコンポーネント バイナリ (`.dll`) の名前と同じであることを確認してください。この名前は、プロジェクトの名前、およびルート名前空間の名前でもあります。 また、Windows ランタイム メタデータとバイナリが、ビルド プロセスによって使用中のアプリの `Appx` フォルダに正しくコピーされていることを確認してください。 さらに、使用中のアプリの `AppxManifest.xml` (および `Appx` フォルダ内) に、アクティブ化可能なクラスとバイナリ名を正しく宣言している **&lt;InProcessServer&gt;** 要素が含まれていることを確認してください。

### <a name="uniform-construction"></a>均一の構築

このエラーは、投影された型のコンストラクターのいずれか (**std::nullptr_t** コンストラクター以外) を使ってローカルに実装されているランタイム クラスをインスタンス化しようとした場合にも、発生する可能性があります。 それを行うには、均一の構築と呼ばれることがよくある C++/WinRT 2.0 の機能が必要です。 その機能にオプトインする場合は、詳細とコード例について、「[均一コンストラクション、実装への直接アクセス](./author-apis.md#opt-in-to-uniform-construction-and-direct-implementation-access)」を参照してください。

均一の構築を必要と "*しない*" ローカルに実装されたランタイム クラスをインスタンス化する方法については、「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」をご覧ください。

## <a name="should-i-implement-windowsfoundationiclosable-and-if-so-how"></a>[  **Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable) を実装するかどうかとその方法
自身のデストラクターのリソースを解放するランタイム クラスを使用し、そのランタイム クラスが実装するコンパイル ユニット (Windows ランタイム クライアント アプリで一般的に使用するための Windows ランタイム コンポーネント) 以外で使用されるように設計されている場合、確定終了処理が不足する言語でランタイム クラスの使用をサポートするために、**IClosable** も実装することをお勧めします。 デストラクター、[**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)、または両方が呼び出されたときにリソースが解放されるようにしてください。 **IClosable::Close** は必要に応じて呼び出すことができます。

## <a name="do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume"></a>使用するランタイム クラスで [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) を読み出す必要性
**IClosable** は確定終了処理が不足する言語をサポートします。 そのため、一般に、C++/WinRT から **IClosable::Close** を呼び出す必要はありません。 ただし、その一般的な規則に対して以下の例外を考慮してください。
- **IClosable::Close** の呼び出しが必要になるような、シャットダウンの競合や破壊的に近い支配が関係する場合はほとんどありません。 たとえば、**Windows.UI.Composition** を使用していて、設定順序でオブジェクトを破棄する場合、その代わりとして、C++/WinRT ラッパーを破棄することができます。
- オブジェクトに対する残りの最後の参照を自分が持っていることが確実でない場合 (オブジェクトを他の API に渡し、そこで参照が保持されている可能性があるため)、**IClosable::Close** を呼び出すのはよいアイデアです。
- 不明な場合は、破棄時にラッパーによって呼び出されるのを待つのではなく、手動で **IClosable::Close** を呼び出すのが安全です。

それで、自分が最後の参照を持っていることが分かっている場合は、ラッパー デストラクターに処理させることができます。 最後の参照が消える前にクローズする必要がある場合は、**Close** を呼び出す必要があります。 例外を適切に処理するには、resource-acquisition-is-initialization (RAII) 型で **Close** を実行する必要があります (アンワインド時にクローズされるようにする)。 C++/WinRT には **unique_close** ラッパーはありませんが、自分で作成することができます。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>LLVM/Clang を使用して C++/WinRT でコンパイルすることはできますか。
C++/WinRT の LLVM および Clang ツール チェーンはサポートしていませんが、C++/WinRT の標準への準拠を検証するためにそれを内部で使用します。 たとえば、マイクロソフトが内部で行っている内容をエミュレートする場合は、次に説明するような実験を試してみることができます。

[LLVM ダウンロード ページ](https://releases.llvm.org/download.html)に移動し、 **[Download LLVM 6.0.0] (LLVM 6.0.0 のダウンロード)**  >  **[Pre-Built Binaries] (事前ビルドされたバイナリ)** を探し、 **[Clang for Windows (64-bit)] (Windows の Clang (64 ビット))** をダウンロードします。 インストール中に、コマンド プロンプトから起動できるように、PATH システム変数に LLVM を追加することを選択します。 この実験の目的上、"Failed to find MSBuild toolsets directory (MSBuild ツールセット ディレクトリの検索に失敗しました)" または "MSVC integration install failed (MSVC 統合インストールに失敗しました)" というエラーが表示された場合には無視できます。 LLVM/Clang を起動するさまざまな方法があります。次の例は、1 つの方法のみを示しています。

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

C++/WinRT では C++17 標準の機能を使用するため、そのサポートを受けるために必要なコンパイラ フラグを使用する必要があります。そのようなフラグはコンパイラによって異なります。

Visual Studio は、マイクロソフトがサポートし、C++/WinRT 用に推奨する開発ツールです。 [C++/WinRT の Visual Studio のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関する記事を参照してください。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>読み取り専用プロパティ用に生成された実装関数に `const` 修飾子がないのはなぜですか?
[MIDL 3.0](/uwp/midl-3/) で読み取り専用プロパティを宣言すると、`cppwinrt.exe` ツールによって `const` で修飾された実装関数 (*this* ポインターを定数として扱う const 関数) が自動的に生成されることが期待されるかもしれません。

確かに、可能な限り const を使用することをお勧めしますが、`cppwinrt.exe` ツール自体では、どの実装関数がおそらく const で、どれがそうでないかについて推論が試行されません。 この例のように、任意の実装関数を const にすることを選択できます。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

実装で何らかのオブジェクトの状態を変更する必要があると判断した場合は、**ToString** の `const` 修飾子を削除できます。 ただし、各メンバー関数を const または non-const (両方ではなく) にします。 つまり、`const` の実装関数をオーバーロードしないでください。

実装関数とは別に、const が関与するもう 1 つの場所として Windows ランタイム関数のプロジェクションがあります。 このコードを見てみましょう。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

前述の **ToString** の呼び出しの場合、Visual Studio の **[宣言へ移動]** コマンドを実行すると、Windows ランタイム **IStringable::ToString** から C++/WinRT へのプロジェクションはこのように表示されます。

```cppwinrt
winrt::hstring ToString() const;
```

プロジェクションに対する関数は、それらの実装をどのように修飾するかにかかわらず、const です。 バックグラウンドでは、プロジェクションによってアプリケーション バイナリ インターフェイス (ABI) が呼び出されます。これは COM インターフェイス ポインターを介した呼び出しに相当します。 プロジェクションが実行された **ToString** が対話する唯一の状態は、その COM インターフェイス ポインターです。また、確かにそのポインターを修正する必要がないので、関数は const です。 そのため、呼び出している **IStringable** の参照については何も変わらないことが保証されます。また、**IStringable** への const 参照があっても確実に **ToString** を呼び出すことができます。

これらの `const` の例は、C++/WinRT プロジェクションの実装の詳細と実装です。これらによって、有益なコードの検疫が構成されます。 COM と Windows ランタイム ABI (メンバー関数用) のどちらにも `const` のようなものはありません。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>C++/WinRT バイナリのコード サイズを減らすための推奨事項はありますか?
Windows ランタイム オブジェクトを使用する場合は、必要以上に多くのバイナリ コードが生成され、アプリケーションに悪影響が及ぶ可能性があるため、次のようなコーディング パターンは避けてください。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

Windows ランタイム環境では、`c()` の値、または間接指定 ('.') を介して呼び出される各メソッドのインターフェイスをコンパイラでキャッシュすることはできません。 介入しないと、結果として仮想呼び出しと参照カウントのオーバーヘッドが増えます。 上記のパターンによって、厳密に必要なコードの 2 倍が容易に生成される可能性があります。 代わりに、できる限り次のパターンをお勧めします。 生成されるコードがはるかに少なくなり、実行時のパフォーマンスも大幅に向上します。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

上記の推奨パターンは、C++/WinRT だけでなく、すべての Windows ランタイム言語のプロジェクションにも当てはまります。

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>(たとえばナビゲーションのために) 文字列を型に変換するにはどうすればよいですか?
[ナビゲーション ビューのコード例](../design/controls-and-patterns/navigationview.md#code-example) (主に C#) の末尾には、この実行方法を示す C++/WinRT コード スニペットがあります。

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>GetCurrentTime および TRY でのあいまいさを解決するにはどうすればよいですか?

ヘッダー ファイル `winrt/Windows.UI.Xaml.Media.Animation.h` では **GetCurrentTime** という名前のメソッドが宣言されているのに対し、`windows.h` では (`winbase.h` を介して) **GetCurrentTime** という名前のマクロが定義されています。 その 2 つが競合すると、C++ コンパイラで次のエラーが発生します。"*エラー C4002: 関数に似たマクロ呼び出し 'GetCurrentTime' の引数が多すぎます*"。

同様に、`winrt/Windows.Globalization.h` では **TRY** という名前のメソッドが宣言されているのに対し、`afx.h` では **TRY** という名前のマクロが定義されています。 これらが競合すると、C++ コンパイラで次のエラーが発生します。"*エラー C2334: '{' の前に予期しないトークンがありました。関数の本体は無視されます*"。

一方または両方の問題を解決するには、次のようにすることができます。

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

## <a name="how-do-i-speed-up-symbol-loading"></a>シンボルの読み込みを高速化するにはどうすればよいですか?
Visual Studio で、 **[ツール]**  >  **[オプション]**  >  **[デバッグ]**  >  **[シンボル]** に移動し、" *[指定したモジュールのみ]* " のチェック ボックスをオンにします。 その後スタックの一覧で DLL を右クリックして、個々のモジュールを読み込むことができます。

> [!NOTE]
> このトピックで質問の答えが見つからなかった場合は、[Visual Studio C++ 開発者コミュニティ](https://developercommunity.visualstudio.com/spaces/62/index.html)にアクセスするか、[`c++-winrt` タグを Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt) で使用することでヘルプが得られる場合があります。
