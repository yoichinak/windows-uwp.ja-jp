---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: 非同期プログラミング
description: このトピックでは、ユニバーサル Windows プラットフォーム (UWP) での非同期プログラミングと、C#、Microsoft Visual Basic .NET、C++、JavaScript でのその表現について説明します。
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10、UWP、非同期
ms.localizationpriority: medium
ms.openlocfilehash: 8e7cdffd484c426faa9b877240f45f122ccc5ec4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161756"
---
# <a name="asynchronous-programming"></a>非同期プログラミング
このトピックでは、ユニバーサル Windows プラットフォーム (UWP) での非同期プログラミングと、C#、Microsoft Visual Basic .NET、C++、JavaScript でのその表現について説明します。

非同期プログラミングを使うと、アプリが時間のかかる可能性がある操作を実行しているときでも、アプリの応答性を保つことができます。 たとえば、インターネットからコンテンツをダウンロードするアプリは、コンテンツが到着するまで数秒待機する可能性があります。 UI スレッドで同期メソッドを使ってコンテンツを取得すると、アプリはメソッドから制御が返されるまでブロックされます。 アプリが操作に応答せず、応答していないと思ったユーザーが苛立ちを感じる可能性があります。 これよりもはるかに優れた方法は非同期プログラミングを使うことです。非同期プログラミングでは、アプリは実行を続けて、操作が完了するまで待機している間も UI に応答します。

処理の完了までに時間がかかるメソッドの場合、UWP で非同期プログラミングを使うのは標準で、例外ではありません。 JavaScript、C#、Visual Basic、および C++ はそれぞれ、非同期メソッドの言語サポートを提供します。

## <a name="asynchronous-programming-in-the-uwp"></a>UWP での非同期プログラミング
[**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) Api や[**StorageFile**](/uwp/api/Windows.Storage.StorageFile) api などの多くの UWP 機能は、非同期 api として公開されています。 慣例により、非同期 Api の名前は、制御が呼び出し元に返された後に実行される可能性があることを示すために、"Async" で終了します。

ユニバーサル Windows プラットフォーム (UWP) アプリで非同期 API を使う場合、コードは一貫した方法で非ブロック呼び出しを行います。 これらの非同期パターンを独自の API 定義に実装すると、呼び出し元はコードを理解し、予測可能な方法で使うことができます。

ここでは、非同期 Windows ランタイム Api の呼び出しを必要とする一般的なタスクをいくつか紹介します。

-   メッセージ ダイアログの表示

-   ファイル システムの操作 (ファイル ピッカーの表示)

-   インターネットを介したデータの送受信。

-   ソケット、ストリーム、接続の使用

-   予定、連絡先、カレンダーの操作

-   ファイルの種類の操作 (Portable Document Format (PDF) ファイルを開く、画像やメディア形式のデコードなど)。

-   デバイスやサービスの操作

UWP の非同期パターンを利用すると、スレッドの明示的な管理を完全に回避できる場合があります。 各プログラミング言語では、次に示すように独自の方法で UWP の非同期パターンがサポートされます。

| プログラミング言語 | 非同期表現           |
|----------------------|---------------------------------------|
| C#                   | **async** キーワード、**await** 演算子 |
| Visual Basic         | **Async** キーワード、 **Await** 演算子 |
| C++/WinRT            | コルーチンおよび **co_await** 演算子  |
| C++/CX               | **task** クラス、**.then** メソッド      |
| JavaScript           | promise オブジェクト、**then** 関数     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>C# と Visual Basic を使った UWP での非同期パターン
C# または Visual Basic で書かれたコードのセグメントは、通常は同期して実行されます。つまり、ある行が実行されるときには、その行は次の行が実行される前に完了します。 以前は非同期実行の Microsoft .NET プログラミング モデルがありましたが、作成されたコードでは、コードで実行しようとしているタスクではなく、非同期コードを実行するしくみに重点が置かれる傾向があります。 UWP、.NET framework、C# と Visual Basic のコンパイラでは、コードから非同期のしくみを取り出す機能が追加されました。 これにより、.NET や UWP を使う場合、いつどのように達成するかではなく何を達成するかに重点を置いた非同期コードを記述できます。 記述される非同期コードは、同期コードに類似しています。 詳しくは、「[C# または Visual Basic での非同期 API の呼び出し](call-asynchronous-apis-in-csharp-or-visual-basic.md)」をご覧ください。

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>C++/WinRT での UWP の非同期パターン
C++/WinRT では、コルーチンと **co_await** 演算子を使用します。 詳細とコード例については、「 [C++/WinRT での非同期プログラミング](../cpp-and-winrt-apis/concurrency.md)」を参照してください。

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>C++/CX での UWP の非同期パターン
C++/CX では、非同期プログラミングは [**task クラス**](/cpp/parallel/concrt/reference/task-class) とその [**then メソッド**](/cpp/parallel/concrt/reference/task-class?view=vs-2017) に基づいています。 構文は JavaScript の promise の構文に似ています。 **task クラス**とそれに関連する型は、スレッド コンテキストの取り消しと管理に使われる機能を提供します。 詳細については、「 [C++/cx での非同期プログラミング](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)」を参照してください。

[**Create \_ async 関数**](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017)は、JAVASCRIPT または UWP をサポートするその他の言語から使用できる非同期 api の生成をサポートします。 詳細については、「 [C++/cx での非同期操作の作成](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)」を参照してください。

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>JavaScript を使った UWP での非同期パターン
JavaScript の非同期プログラミングでは、[Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A) 提唱の標準に従って、非同期メソッドで promise オブジェクトを返します。 Promise は、UWP と JavaScript 用 Windows ライブラリの両方で使われます。

promise オブジェクトは、将来取得されたときに値を表します。 UWP では、promise オブジェクトをファクトリ関数から取得します。ファクトリ関数には、慣例により "Async" で終わる名前が付いています。

多くの場合、非同期関数の呼び出しは従来の関数の呼び出しと同じくらい簡単です。 違いは、結果またはエラーに対するハンドラーの割り当てと操作の開始に [**then**](/previous-versions/windows/apps/br229728(v=win.10)) メソッドまたは [**done**](/previous-versions/windows/apps/hh701079(v=win.10)) メソッドを使うことです。

## <a name="related-topics"></a>関連トピック
* [C# または Visual Basic での非同期 API の呼び出し](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Async および Await を使用した非同期プログラミング (C# および Visual Basic)](/previous-versions/visualstudio/visual-studio-2012/hh191443(v=vs.110))
* [リバーシの機能のシナリオ: 非同期コード](/previous-versions/windows/apps/jj712233(v=win.10))