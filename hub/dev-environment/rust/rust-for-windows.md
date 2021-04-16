---
title: Windows 用 Rust と *windows* クレート
description: '*windows* クレートの使用と Windows API の呼び出し。'
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: rust、windows 10、microsoft、rust の学習、windows での rust (初心者向け)、vs code を使用した rust、windows 用の rust
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 1ba0acf298a3bd492ceac9adccafcac857f67cba
ms.sourcegitcommit: f7c7a2ae6367e114a8b9d438963082440cd24043
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2021
ms.locfileid: "107315065"
---
# <a name="rust-for-windows-and-the-windows-crate"></a>Windows 用 Rust と *windows* クレート

&nbsp;
> [!VIDEO https://www.youtube.com/embed/LajquCjHXK4]

## <a name="introducing-rust-for-windows"></a>Windows 用 Rust の概要

「[Rust を使用した Windows での開発の概要](overview.md)」トピックでは、*Hello, world!* メッセージを出力する簡単なアプリについて説明しました。 メッセージで応答します。 しかし、Windows "*で*" Rust を使用できるだけでなく、Rust を使用して Windows "*のための*" アプリを記述することもできます。

Windows 用 Rust (現時点ではプレビュー形式) は、Windows のための最新の言語計画です。 Windows 用 Rust を使用すると、[*windows* クレート](https://crates.io/crates/windows)を介して、過去、現在、将来の任意の Windows API を直接シームレスに使用できるようになります (*クレート* は、バイナリ、ライブラリ、1 つにビルドされるソース コードを表す Rust の用語です)。

それが [CreateEventW](/windows/win32/api/synchapi/nf-synchapi-createeventw) や [WaitForSingleObject](/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject) などのタイムレス関数であるか、[Direct3D](/windows/win32/direct3d12/directx-12-programming-guide) などの強力なグラフィックス エンジンであるか、[CreateWindowExW](/windows/win32/api/winuser/nf-winuser-createwindowexw) や [DispatchMessageW](/windows/win32/api/winuser/nf-winuser-dispatchmessagew) などの従来のウィンドウ関数であるか、[Composition](/uwp/api/windows.ui.composition) や [Xaml](/uwp/api/windows.ui.xaml) などの最新のユーザー インターフェイス (UI) フレームワークであるかを問わず、[*windows* クレート](https://crates.io/crates/windows)で対応できます。

この [win32metadata](https://github.com/microsoft/win32metadata) プロジェクトは、Win32 API のメタデータを提供することを目的としています。 このメタデータは、厳密に型指定された API の署名、パラメーター、型など、API サーフェイスについて記述するものです。 これにより、Rust (および C# や C++ などの言語) での使用のために、自動化された完全な方法で Windows API 全体を計画することができます。 また、「[より多くの言語からの Win32 API へのアクセスの実現](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/)」を参照してください。

Rust の開発者は、Cargo (Rust のパッケージ管理ツール) と共に `https://crates.io` (Rust コミュニティのクレート レジストリ) を使用して、プロジェクト内の依存関係を管理します。 Rust アプリから [*windows* クレート](https://crates.io/crates/windows) を参照したらすぐに、Windows API の呼び出しを開始できるのが利点です。 [*windows* クレートに関する Rust のドキュメント](https://docs.rs/windows/0.3.1/windows/)は、`https://docs.rs` でも参照できます。

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) と同様に、[Windows 用 Rust](https://github.com/microsoft/windows-rs) は、GitHub で開発されているオープン ソースの言語計画です。 Windows 用 Rust について質問がある場合や、問題を報告する場合は、[Windows 用 Rust](https://github.com/microsoft/windows-rs) のリポジトリを使用します。

Windows 用 Rust のリポジトリには、参考にできる[いくつかの簡単な例](https://github.com/microsoft/windows-rs/tree/master/examples)も収録されています。 また、Robert Mikhayelyan の[マインスイーパー](https://github.com/robmikh/minesweeper-rs)という形で、優れたサンプル アプリがあります。

## <a name="contribute-to-rust-for-windows"></a>Windows 用 Rust に貢献する

[Windows 用 Rust](https://github.com/microsoft/windows-rs) では、皆さんの貢献が歓迎されています。

* [ソース コード](https://github.com/microsoft/windows-rs/tree/master/src)内のバグを特定して修正する

## <a name="rust-documentation-for-the-windows-api"></a>Windows API 用の Rust ドキュメント

Windows 用 Rust は、Rust 開発者が利用している洗練されたツールチェーンの恩恵を受けています。 しかし、Windows API 全体を自在に扱うことが少し難しすぎるように思える場合は、[Windows API についての Rust ドキュメント](https://microsoft.github.io/windows-docs-rs/doc/bindings/Windows/)も用意されています。

このリソースは、基本的に、Windows の API と型が、独特な Rust にどのように投影されるかを文書化するものです。 これを使用して、それについて知る必要があり、呼び出す方法を知る必要がある API の参照や検索を行ってください。

## <a name="writing-an-app-with-rust-for-windows"></a>Windows 用 Rust を使用したアプリの記述

次のトピックは、[RSS リーダーのチュートリアル](rss-reader-rust-for-windows.md)です。ここでは、Windows 用 Rust を使用して簡単なアプリを記述する手順について説明します。

## <a name="related"></a>関連項目

* [Rust を使用した Windows での開発の概要](overview.md)
* [RSS リーダーのチュートリアル](rss-reader-rust-for-windows.md)
* [*windows* クレート](https://crates.io/crates/windows)
* [*windows* クレートに関するドキュメント](https://docs.rs/windows/0.3.1/windows/)
* [Win32 メタデータ](https://github.com/microsoft/win32metadata)
* [より多くの言語からの Win32 API へのアクセスの実現](https://blogs.windows.com/windowsdeveloper/2021/01/21/making-win32-apis-more-accessible-to-more-languages/)
* [Windows API 用の Rust ドキュメント](https://microsoft.github.io/windows-docs-rs/doc/bindings/windows/)
* [Windows 用 Rust](https://github.com/microsoft/windows-rs)
* [マインスイーパー サンプル アプリ](https://github.com/robmikh/minesweeper-rs)
