---
description: このトピックにある現象のトラブルシューティングおよび対処法に関する表は、新しいコードを作成しているか既存のアプリを移植しているかにはかかわらず役立つ可能性があります。
title: C++/WinRT に関する問題のトラブルシューティング
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10、uwp、標準、c++、cpp、winrt、プロジェクション、トラブルシューティング、HRESULT、エラー
ms.localizationpriority: medium
ms.openlocfilehash: 268792dfe8053feca8c1e6fcb486bede4b26ee6a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297727"
---
# <a name="troubleshooting-cwinrt-issues"></a>C++/WinRT に関する問題のトラブルシューティング

> [!NOTE]
> [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) (プロジェクト テンプレート サポートを提供) のインストールと使用については、[C++/WinRT の Visual Studio サポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関するページをご覧ください。

このトピックは、すぐに認識していただくための先行情報です。まだ必要としていない場合も同様です。 以下の症状のトラブルシューティングおよび対処法に関する表は、新しいコードを作成しているか既存のアプリを移植しているかにはかかわらず役立つ可能性があります。 移植中であり、進展させてプロジェクトのビルドおよび実行の段階に達することを急いでいる場合は、問題を引き起こしている重要でないコードにコメントアウトまたはスタブ挿入を適用して、一時的に進展させることができます。その後、元に戻ってその借りを解消することになります。

よく寄せられる質問の一覧については、[よく寄せられる質問](faq.md)に関する記事をご覧ください。

## <a name="tracking-down-xaml-issues"></a>XAML に関する問題の検出
XAML 解析例外は診断が難しい場合があります。特に、わかりやすいエラー メッセージが例外に含まれていない場合は、診断が難しくなります。 デバッガーが初回例外をキャッチするように構成されていることを確してください (早い段階で解析例外のキャッチを試行するため)。 デバッガーで例外変数を調べて、HRESULT やメッセージ内に役立つ情報が含まれているかどうかを確認できます。 また、XAML パーサーを使って、Visual Studio の出力ウィンドウを調べ、エラー メッセージの出力を確認することもできます。

アプリが終了し、XAML マークアップの解析中に処理不能な例外がスローされたことのみがわかっている場合、存在しないリソースへの (キーによる) 参照の結果である可能性があります。 または、UserControl、カスタム コントロール、カスタム レイアウト パネルの内部で例外がスローされたことも考えられます。 最終手段として、バイナリ分割を使うことができます。 XAML ページからマークアップのおよそ半分を削除し、アプリを再実行します。 これによって、エラーが削除した半分で発生しているか (いずれの場合でも、削除した部分はここで元に戻す必要があります)、または削除しなかった半分で発生しているかがわかります。 問題が特定されるまで、エラーを含む半分をさらに分割するプロセスを繰り返します。

## <a name="symptoms-and-remedies"></a>現象と対処法
| 症状 | 解決方法 |
|---------|--------|
| 実行時に REGDB_E_CLASSNOTREGISTERED の HRESULT 値で例外がスローされます。 | 「["クラスが登録されていません" という例外が発生するのはなぜですか?](faq.md#why-am-i-getting-a-class-not-registered-exception)」を参照してください。 |
| C++ コンパイラーは、"*'implements_type': は、'&lt;プロジェクションされた型&gt;'* の直接的または間接的な基底クラスのメンバーではありません" というエラーを生成します。 | これは、実装型 (たとえば **MyRuntimeClass**) の未修飾名前空間名を使用して、その型のヘッダーを含めずに **make** を呼び出すと発生する可能性があります。 コンパイラーは、**MyRuntimeClass** をプロジェクションされた型として解釈します。 解決策は、実装型のヘッダーを含めることです (たとえば `MyRuntimeClass.h`)。 |
| C++ コンパイラーが、"*削除された関数を参照しようとしています*" というエラーを生成します。 | これは、**make** を呼び出し、テンプレート パラメーターとして渡す実装型の既定のコンストラクターが `= delete` である場合に発生する可能性があります。 実装型のヘッダー ファイルを編集し、`= delete` を `= default` に変更してください。 ランタイム クラスの IDL にコンストラクターを追加することもできます。 |
| [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) を実装しましたが、XAML バインドが更新されません (そのため、UI が [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) にサブスクライブしません)。 | XAML マークアップのバインド式で必ず `Mode=OneWay` (または TwoWay) を設定してください。 「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」を参照してください。 |
| XAML アイテム コントロールを監視可能なコレクションにバインドしていますが、実行時に "パラメーターが正しくありません" というメッセージで例外がスローされます。 | IDL および実装で、監視可能なコレクションを型 **Windows.Foundation.Collections.IVector<IInspectable>** として宣言します。 ただし、**Windows.Foundation.Collections.IObservableVector<T>** を実装するオブジェクトを返します。T は要素型です。 「[XAML アイテム コントロール: C++/WinRT コレクションへのバインド](binding-collection.md)」を参照してください。  |
| C++ コンパイラーが、"*'MyImplementationType_base&lt;MyImplementationType&gt;': 使用可能な適切な既定コンストラクターがありません*" という形式のエラーを生成します。|これは、非自明なコンストラクターを持つ型から派生させた場合に発生する可能性があります。 派生型のコンストラクターは、基本型のコンストラクターが必要とするパラメーターを渡す必要があります。 実証済みの例については、「[非自明なコンストラクタを持つ型からの派生](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)」を参照してください。|
| C++ コンパイラーが、" *'const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;' から 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'* に変換できません" というエラーを生成します。|これは、コレクションを予期している Windows ランタイム API に std::wstring の std::vector を渡すときに発生する可能性があります。 詳細については、「[標準的な C++ のデータ型と C++/WinRT](std-cpp-data-types.md)」を参照してください。|
| C++ コンパイラーが、" *'const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;' から 'const winrt::param::async_iterable&lt;winrt::hstring&gt; &'* に変換できません" というエラーを生成します。|これは、コレクションを予期している非同期 Windows ランタイム API に winrt::hstring の std::vector を渡すときに、非同期呼び出し先へのベクトルのコピーも移動も行っていない場合に発生する可能性があります。 詳細については、「[標準的な C++ のデータ型と C++/WinRT](std-cpp-data-types.md)」を参照してください。|
| プロジェクトを開くときに、Visual Studio が "*プロジェクトのアプリケーションはインストールされていません*" というエラーを生成します。|まだ行っていない場合は、Visual Studio の **[新しいプロジェクト]** ダイアログから **C++ での開発用の Windows ユニバーサル ツール**をインストールする必要があります。 それでも問題が解決しない場合は、プロジェクトが C++/WinRT Visual Studio Extension (VSIX) に依存している可能性があります ([C++/WinRT の Visual Studio サポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)に関するページを参照してください)。|
| Windows アプリ認定キットのテストが、ランタイム クラスの 1 つについて、"*Windows 基底クラスから派生しません。すべての構成可能クラスは最終的に、Windows 名前空間内の型から派生する必要があります*" というエラーを生成します。|基底クラスから派生する任意のランタイム クラス (アプリケーション内で宣言) は*構成可能*クラスと呼ばれます。 構成可能クラスの最終的な基底クラスは、Windows.* 名前空間からの型でなければなりません (例: [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject))。 詳細情報については、「[XAML コントロール: C++/WinRT プロパティへのバインド](binding-property.md)」を参照してください。|
| C++ コンパイラにより、EventHandler または TypedEventHandler のデリゲート特殊化に関して "*T は WinRT 型である必要があります*" というエラーが生成されます。|代わりに **winrt::delegate&lt;...T&gt;** を使用することを考慮してください。 「[C++/WinRT でのイベントの作成](author-events.md)」を参照してください。|
| C++ コンパイラにより、Windows ランタイムの非同期操作の特殊化に関して "*T は WinRT 型である必要があります*" というエラーが生成されます。|代わりに並列パターン ライブラリ (PPL) の [**task**](/cpp/parallel/concrt/reference/task-class) を返すことを考慮してください。 「[同時実行操作と非同期操作](concurrency.md)」を参照してください。|
| [**winrt::xaml_typename**](/uwp/cpp-ref-for-winrt/xaml-typename) を呼び出すと、C++ コンパイラによって "*T は WinRT 型である必要があります*" というエラーが生成されます。|**winrt::xaml_typename** により、投影された型を使用し (たとえば **BgLabelControlApp::BgLabelControl** を使用する)、実装型を使用しないようにします (たとえば **BgLabelControlApp::implementation::BgLabelControl** を使用しない)。 [XAML カスタム (テンプレート化) コントロール](xaml-cust-ctrl.md)に関するページをご覧ください。|
| C++ コンパイラーが、"*エラー C2220: 警告がエラーとして扱われました - 'オブジェクト' ファイルは生成されませんでした*" を生成します。|警告を解決するか、または **[C/C++]**  >  **[全般]**  >  **[警告をエラーとして扱う]** を **[いいえ (/WX-)]** に設定します。|
| オブジェクトが破棄された後で C++/WinRT オブジェクトのイベント ハンドラーが呼び出されるため、アプリがクラッシュします。|「[イベント処理デリゲートで *this* ポインターに安全にアクセスする](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)」を参照してください。|
| C++ コンパイラーが "*エラー C2338: これは弱参照サポート専用です*" を生成します。|**テンプレート引数として winrt::no_weak_ref** マーカー構造体を基底クラスに渡した型の、弱参照を要求しています。 「[弱参照サポートの除外](weak-references.md#opting-out-of-weak-reference-support)」を参照してください。|
| C++ リンカーで次のエラーが発生します。"*エラー LNK2019: 外部シンボルは未解決です*"|「[リンカーで "LNK2019: 外部シンボルは未解決です" というエラーが発生するのはなぜですか?](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)」を参照してください。|
| C++/WinRT と共に使用した場合、LLVM と Clang のツールチェーンでエラーが生成されます。|C++/WinRT では LLVM と Clang のツールチェーンはサポートされていませんが、内部でどのように使用されているかエミュレートしたい場合は、「[LLVM/Clang を使用して C++/WinRT でコンパイルすることはできますか](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)」に記載されているような実験を試してみることができます。|
| C++ コンパイラーで、投影された型に対して "*既定のコンストラクターがありません*" が生成されます。 | ランタイム クラス オブジェクトの初期化を遅らせたり、同じプロジェクト内のランタイム クラスを使用および実装したりしようとしている場合は、**std::nullptr_t** コンストラクターを呼び出す必要があります。 詳細については、「[C++/WinRT での API の使用](consume-apis.md)」を参照してください。 |
| C++ コンパイラで、"*エラー C3861: 'from_abi': 識別子が見つかりません*" と、*base.h* から発生する他のエラーが生成されます。 このエラーは、Visual Studio 2017 (バージョン 15.8.0 以上) を使用しており、Windows SDK バージョン 10.0.17134.0 (Windows 10 バージョン 1803) を対象としている場合に発生することがあります。 | より新しい (より準拠した) バージョンの Windows SDK をターゲットにするか、プロジェクトのプロパティを **[C/C++]**  >  **[言語]**  >  **[準拠モード]:[いいえ]** に設定します (また、プロジェクトのプロパティの **[その他のオプション]** の **[C/C++]**  >  **[言語]**  >  **[コマンド ライン]** に **/permissive-** が表示される場合は、それを削除します)。 |
| C++ コンパイラで次のエラーが発生します。"*error C2039: 'IUnknown': '\`グローバル名前空間'' のメンバーではありません*"。 | 「[C++/WinRT プロジェクトのターゲットを Windows SDK のより新しいバージョンに変更する方法](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)」を参照してください。 |
| C++ リンカーで "*エラー LNK2019: 未解決の外部シンボル _WINRT_CanUnloadNow@0 が関数 _VSDesignerCanUnloadNow@0 で参照されました*" が生成されます。 | 「[C++/WinRT プロジェクトのターゲットを Windows SDK のより新しいバージョンに変更する方法](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)」を参照してください。 |
| ビルド プロセスで、次のエラー メッセージが発生します。*The C++/WinRT VSIX no longer provides project build support. Please add a project reference to the Microsoft.Windows.CppWinRT Nuget package* (C++/WinRT VSIX ではプロジェクトのビルドがサポートされなくなりました。Microsoft.Windows.CppWinRT Nuget パッケージへのプロジェクト参照を追加してください)。 | **Microsoft.Windows.CppWinRT** NuGet パッケージをプロジェクトにインストールします。 詳細については、「[VSIX 拡張機能の以前のバージョン](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)」を参照してください。 |
| C++ リンカーが、*winrt::impl::consume_Windows_Foundation_Collections_IVector* に言及して、"*エラー LNK2019: 外部シンボルは未解決です*" を生成します。 | [C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20) の時点で、Windows Runtime コレクションで範囲ベースの `for` を使用している場合は、`#include <winrt/Windows.Foundation.Collections.h>` を実行する必要があります。 |
| C++ コンパイラで次のエラーが発生します。"*エラー C4002: 関数に似たマクロ呼び出し 'GetCurrentTime' の引数が多すぎます*"。 | 「[GetCurrentTime および TRY でのあいまいさを解決するにはどうすればよいですか?](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)」を参照してください。 |
| C++ コンパイラで次のエラーが発生します。"*エラー C2334: '{' の前に予期しないトークンがありました。関数の本体は無視されます*"。 | 「[GetCurrentTime および TRY でのあいまいさを解決するにはどうすればよいですか?](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)」を参照してください。 |
| C++ コンパイラで次のエラーが発生します。"*winrt::impl::produce&lt;D,I&gt; cannot instantiate abstract class, due to missing GetBindingConnector*" ('winrt::impl::produce<D,I>': GetBindingConnector がないため、抽象クラスをインスタンス化できません)。 | `#include <winrt/Windows.UI.Xaml.Markup.h>` が必要です。 |
| C++ コンパイラで次のエラーが発生します。"*エラー C2039: 'promise_type': 'std::experimental::coroutine_traits<void>' のメンバーではありません*"。 | コルーチンでは、非同期操作オブジェクトまたは **winrt::fire_and_forget** を返す必要があります。 「[同時実行操作と非同期操作](concurrency.md)」を参照してください。 |
| プロジェクトで "*'PopulatePropertyInfoOverride' へのアクセスがあいまいです*" が発生します。 | このエラーは、IDL で宣言した 1 つの基底クラスが XAML マークアップの基底クラスと異なると、発生する場合があります。 |
| C++/WinRT ソリューションを初めて読み込むと、次のエラーが発生します。"*プロジェクト 'MyProject.vcxproj'、構成 'Debug\|x86' のデザイン時のビルドに失敗しました。IntelliSense を利用できない可能性があります。* "。 | この IntelliSense の問題は、初めてのビルドの後で解決されます。 |
| デリゲートを登録するときに [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) を指定しようとすると、[**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 例外が発生します。 | 「[自動取り消しのデリゲートの登録が失敗する場合](handle-events.md#if-your-auto-revoke-delegate-fails-to-register)」を参照してください。 |
|C++/WinRT アプリで、XAML を使用する [C# Windows ランタイム コンポーネント](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)を使用すると、" *'MyNamespace_XamlTypeInfo' は 'winrt::MyNamespace' のメンバーではありません*" という形式のエラーがコンパイラによって生成されます&mdash;この *MyNamespace* は、Windows ランタイム コンポーネントの名前空間の名前です。 | 使用する側の C++/WinRT アプリの `pch.h` で、`#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` を追加します&mdash;必要に応じて *MyNamespace* を置き換えます。 |

> [!NOTE]
> このトピックで質問の答えが見つからなかった場合は、[Visual Studio C++ 開発者コミュニティ](https://developercommunity.visualstudio.com/spaces/62/index.html)にアクセスするか、[`c++-winrt` タグを Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt) で使用することでヘルプが得られる場合があります。