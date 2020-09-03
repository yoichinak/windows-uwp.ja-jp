---
description: このトピックでは、C++/WinRT でのプログラミング時にエラーを処理するための方法について説明します。
title: C++/WinRT でのエラー処理
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, エラー, 処理, 例外
ms.localizationpriority: medium
ms.openlocfilehash: c721c70f19533053821139429b9bbd8bcd17f68a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170226"
---
# <a name="error-handling-with-cwinrt"></a>C++/WinRT でのエラー処理

このトピックでは、[C++/WinRT](./intro-to-using-cpp-with-winrt.md) でのプログラミング時にエラーを処理するための方法について説明します。 一般的な情報、および背景については、「[エラーと例外の処理 (最新の C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)」を参照してください。

## <a name="avoid-catching-and-throwing-exceptions"></a>例外のキャッチとスローの回避
引き続き[例外安全なコード](/cpp/cpp/how-to-design-for-exception-safety)を記述することをお勧めしますが、可能な限り、例外のキャッチとスローを回避します。 例外のハンドラーがない場合、Windows は (クラッシュのミニダンプを含む) エラー レポートを自動的に生成します。このレポートは、問題のある場所を追跡するのに役立ちます。

キャッチすることが予想される例外をスローしないでください。 また、予想されるエラーに対して例外を使用しないでください。 ** 予期しないランタイム エラーが発生したときにのみ&mdash;例外をスローし、それ以外はすべてエラー コードまたは結果コードで、エラーの発生源に近いところで直接処理します。 これにより、例外がスロー*された*ときに、原因がコード内のバグであるか、またはシステム内の例外的なエラー状態のいずれかであることがわかります。

Windows レジストリにアクセスするためのシナリオを検討してください。 アプリがレジストリから値を読み取ることができなかった場合は、それが予想されることであり、適切に処理する必要があります。 例外をスローしないで、その例外と、値が読み取られなかった理由を示す `bool` または `enum` の値を返します。 一方、レジストリへの値の*書き込み*に失敗すると、アプリケーションで適切に処理できないほどの大きな問題があることが示される可能性があります。 そのような場合は、アプリケーションを続行させたくないため、結果としてエラー レポートを生じさせる例外は、アプリケーションが問題を起こさないようにする最も速い方法です。

別の例として、[**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) への呼び出しからサムネイル画像を取得し、そのサムネイルを [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_) に渡すことを検討します。 その呼び出しのシーケンスにより、`nullptr` を **SetSourceAsync** に渡し (イメージ ファイルは読み取ることができません。おそらく、そのファイル拡張子のためファイルにイメージ データが含まれているように見えて実際には含まれていません)、無効なポインター例外がスローされることになります。 コードでそのようなケースが見つかったら、そのケースを例外としてキャッチして処理するのではなく **GetThumbnailAsync** から返される `nullptr` かどうかを確認します。

例外をスローすることは、エラー コードを使用するよりも遅くなる傾向があります。 致命的なエラーが発生した場合にのみ例外をスローする場合は、すべてがうまく行けばパフォーマンスの代償を払うことはありません。

ただし、より可能性の高いパフォーマンスの影響として、例外がスローされる万が一のイベントで適切なデストラクターが呼び出されるのを確認することによる実行時のオーバーヘッドがあります。 この保証に関する代償は、例外が実際にスローされるかどうかで決まります。 そのため、どの関数が例外をスローする可能性があるかをコンパイラで十分に把握していることを確認する必要があります。 コンパイラが特定の関数 (`noexcept` 仕様) からの例外が発生しないことを証明できれば、生成されるコードを最適化できます。

## <a name="catching-exceptions"></a>例外のキャッチ
[Windows ランタイム ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) レイヤーで発生するエラー状態は、HRESULT 値の形式で返されます。 ただし、コードで HRESULT を処理する必要はありません。 使用する側で API のために生成された C++/WinRT プロジェクション コードにより、ABI レイヤーで HRESULT エラー コードが検出され、そのコードが [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 例外に変換されます。この例外はキャッチして処理できます。 HRESULTS を*処理したい*場合は、**winrt::hresult** 型を使用します。

たとえば、アプリケーションによるそのコレクションの反復処理中に、ユーザーが画像ライブラリのイメージを削除してしまった場合、プロジェクションにより例外がスローされます。 また、このケースでは、その例外をキャッチして処理する必要があります。 このケースを示すコード例を次に示します。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.code(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

`co_await` された関数を呼び出すときにコルーチンでこれと同じパターンを使用します。 この HRESULT から例外への変換の別の例として、コンポーネント API が E_OUTOFMEMORY を返すときに **std::bad_alloc** がスローされます。

HRESULT コードを単に確認するだけなら、[**winrt::hresult_error::code**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorcode-function) を使用してください。 一方で、[**winrt::hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 関数を使用すると、COM エラー オブジェクトに変換され、状態が COM スレッド ローカル ストレージにプッシュされます。

## <a name="throwing-exceptions"></a>例外のスロー
特定の関数への呼び出しが失敗した場合に、アプリケーションが回復できない (予想どおりに機能することを当てにできない) ように決定する場合があります。 次のコード例では、[**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 値を [**CreateEvent**](/windows/desktop/api/synchapi/nf-synchapi-createeventa) から返された HANDLE 全体のラッパーとして使用します。 次にハンドルを (そこから `bool` 値を作成して) [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 関数テンプレートに渡します。 **winrt::check_bool** は、`bool` または `false` (エラーの状態) または `true` (成功の状態) と読み替えることができる任意の値と連携します。

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

[  **winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) に渡す値が false である場合、次の一連の処理が実行されます。

- **winrt::check_bool** が [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 関数を呼び出す。
- 呼び出しスレッドの最終エラー コード値を取得するために **winrt::throw_last_error** が [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) を呼び出し、次に [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) 関数を呼び出す。
- **winrt::throw_hresult** が、エラー コードを表す [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) オブジェクト (または標準のオブジェクト) を使用して例外をスローする。

Windows API では、さまざまな戻り値の型を使用して実行時エラーをレポートするため、**winrt::check_bool** 以外にも、値をチェックして例外をスローするためのその他の便利なヘルパー関数がいくつかあります。

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)。 HRESULT コードがエラーを表すかどうかをチェックし、エラーを表す場合は **winrt::throw_hresult** を呼び出します。
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt)。 コードがエラーを表すかどうかをチェックし、エラーを表す場合は **winrt::throw_hresult** を呼び出します。
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)。 ポインター が null かどうかをチェックし、null の場合は **winrt::throw_last_error** を呼び出します。
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32)。 コードがエラーを表すかどうかをチェックし、エラーを表す場合は **winrt::throw_hresult** を呼び出します。

一般的なリターン コードの種類にこれらのヘルパー関数を使用するか、または任意のエラー状態に応答して、[**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) または [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) を呼び出すことができます。 

## <a name="throwing-exceptions-when-authoring-an-api"></a>API を作成するときの例外のスロー
[Windows ランタイム アプリケーション バイナリ インターフェイス](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types)の境界 (ABI の境界) はすべて *noexcept*&mdash; である必要があります。これは、例外がここで絶対にエスケープされないことを意味します。 API を作成するときは、常に、ABI の境界を C++ `noexcept` キーワードでマークしてください。 `noexcept` には C++ で固有の動作があります。 C++ の例外で `noexcept` 境界がヒットした場合、プロセスは **std::terminate** でフェイル ファストします。 ハンドルされない例外はほとんどの場合プロセスの不明な状態を意味するので、この動作は通常は望ましい動作です。

例外は ABI の境界を越えてはならないため、実装で発生するエラー状態は、HRESULT エラー コードの形式で ABI レイヤーを介して返されます。 C++/WinRT を使用して API を作成している場合、実装でスロー*する*例外を HRESULT に変換するためのコードが生成されます。 このようなパターンで生成されたコードで [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 関数が使用されます。

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) では、**std::exception** から派生した例外、および [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) とその派生型を処理します。 実装では、API のユーザーが詳細なエラー情報を受け取るように、**winrt::hresult_error** または派生型を使用することをお勧めします。 (E_FAIL にマップされる) **std::exception** は、標準テンプレート ライブラリを使用したことで例外が発生した場合にサポートされます。

### <a name="debuggability-with-noexcept"></a>noexcept によるデバッグ容易性
前述のように、C++ の例外で `noexcept` 境界がヒットすると、**std::terminate** でフェイル ファストします。 これはデバッグには適していません。**std:: terminate** では、コルーチンが関係する場合は、多くの、またはすべてのエラーやスローされた例外コンテキストが失われることがよくあるためです。

そのため、このセクションでは、ABI メソッド (`noexcept` で適切に注釈が付けられています) で `co_await` を使用して非同期 C++/WinRT プロジェクション コードを呼び出すケースについて説明します。 C++/WinRT プロジェクション コードへの呼び出しは、**winrt::fire_and_forget** 内にラップすることをお勧めします。 これにより、ハンドルされない例外を格納された例外として適切に記録するための場所が提供され、デバッグ容易性が大幅に増加します。

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt:: fire_and_forget** には組み込みの `unhandled_exception` メソッド ヘルパーがあります。これは **winrt::terminate** を呼び出し、さらにこれが **RoFailFastWithErrorContext** を呼び出します。 これにより、すべてのコンテキスト (格納された例外、エラー コード、エラー メッセージ、スタック バックトレースなど) がライブ デバッグ用または事後検証ダンプ用に保持されることが保証されます。 便宜上、fire-and-forget の部分を、**winrt::fire_and_forget** を返してからそれを呼び出す別の関数に組み入れることができます。

### <a name="synchronous-code"></a>同期コード
場合によっては、ABI メソッド (この場合も、`noexcept` で適切に注釈が付けられています) は同期コードだけを呼び出します。 つまり、非同期 Windows ランタイムメソッドを呼び出したり、フォアグラウンド スレッドとバックグラウンド スレッドを切り替えたりするために、`co_await` を使用することはありません。 その場合でも、fire_and_forget 手法は引き続き機能しますが、効率的ではありません。 代わりに、次のように処理できます。

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>フェイル ファスト
前のセクションのコードでは、引き続きフェイル ファストします。 記載されているように、そのコードは例外を処理しません。 ハンドルされない例外が発生すると、プログラムは終了します。

しかし、この形式によりデバッグ容易性が確保されるため、より優れています。 まれに、`try/catch` で、特定の例外を処理することが必要になる場合があります。 しかし、これはまれなケースです。このトピックで説明するように、予想される条件のフロー制御メカニズムとして例外を使用することをお勧めします。

ハンドルされない例外によってネイキッド `noexcept` コンテキストをエスケープできるようにすることは、適切な方法ではありません。 その条件下では C++ ランタイムは **std::terminate** でプロセスを呼び出し、これにより、C++/WinRT で慎重に記録された格納済みの例外情報が失われます。

## <a name="assertions"></a>アサーション
アプリケーションの内部の前提として、アサーションが用意されています。 可能な限り、コンパイル時の検証に **static_assert** を使用することをお勧めします。 実行時の条件には、ブール式で `WINRT_ASSERT` を使います。 `WINRT_ASSERT` はマクロ定義であり、[_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros) に展開されます。

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT は、リテール ビルドでコンパイルされます。デバッグ用のビルドでは、アサーションがあるコード行でデバッガーでアプリケーションが停止します。

デストラクターで例外を使用しないでください。 そのため、少なくともデバッグ ビルドでは、WINRT_VERIFY (ブール式を含む) と WINRT_VERIFY_ (期待される結果とブール式を含む) を使用してデストラクターからの関数の呼び出しの結果をアサートできます。

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>重要な API
* [winrt::check_bool 関数テンプレート](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult 関数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_nt 関数テンプレート](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer 関数テンプレート](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32 関数テンプレート](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle 構造体](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error 構造体](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult 関数](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error 関数](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult 関数](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>関連トピック
* [エラーと例外処理 (最新の C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* 「[方法: 例外の安全性の設計](/cpp/cpp/how-to-design-for-exception-safety)