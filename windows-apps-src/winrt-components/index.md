---
title: Windows ランタイム コンポーネント
description: Windows ランタイム コンポーネントは自己完結型オブジェクトで、C#、Visual Basic、JavaScript、C++ など、すべての言語からインスタンス化して使用することができます。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b01aaff3a0a273ccbf5327d54bb01ae67dd3774e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174246"
---
# <a name="windows-runtime-components"></a>Windows ランタイム コンポーネント

Windows ランタイム コンポーネントは、任意の Windows ランタイム言語 (C#、C++/WinRT、Visual Basic、JavaScript、C++/CX など) を使用して、作成、参照、および使用できる自己完結型のソフトウェア モジュールです。 Visual Studio を使用して、ユニバーサル Windows プラットフォーム (UWP) アプリで使用できる Windows ランタイム コンポーネントを作成できます。

> [!NOTE]
> C++ 開発者の場合、新しいアプリケーションには [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) を使用することをお勧めします。 C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 C++/WinRT を使用して Windows ランタイム コンポーネントを作成する方法については、「[C++/WinRT を使用した Windows Runtime コンポーネント](./create-a-windows-runtime-component-in-cppwinrt.md)」を参照してください。

| トピック | 説明 |
|-------|-------------|
| [C++/WinRT を使用した Windows ランタイム コンポーネント](./create-a-windows-runtime-component-in-cppwinrt.md) | このトピックでは、C++/WinRT を使用して Windows ランタイム コンポーネントを作成および使用する方法を示します。このコンポーネントは、任意の Windows ランタイム言語を使用してビルドされたユニバーサル Windows アプリから呼び出すことができます。 |
| [C++/CX を使用した Windows ランタイム コンポーネント](creating-windows-runtime-components-in-cpp.md) | このトピックでは、C++/CX を使って Windows ランタイム コンポーネントを作成する方法を示します。このコンポーネントは、Windows ランタイム言語を使って構築しされたユニバーサル Windows アプリから呼び出すことができます。 |
| [C++/CX Windows ランタイム コンポーネントの作成と JavaScript または C# からの呼び出しに関するチュートリアル](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | このチュートリアルでは、JavaScript、C#、または Visual Basic から呼び出すことができる基本的な Windows ランタイム コンポーネント DLL を作成する方法について説明します。 このチュートリアルを開始する前に、抽象バイナリ インターフェイス (ABI)、ref クラス、Visual C++ コンポーネント拡張などの概念を必ず理解しておいてください。ref クラスの操作が容易になります。 詳しくは、「[C++ での Windows ランタイム コンポーネントの作成](creating-windows-runtime-components-in-cpp.md)」および「[Visual C++ の言語リファレンス (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx)」をご覧ください。 |
| [C# および Visual Basic を使用した Windows ランタイム コンポーネント](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | マネージ コードを使って独自の Windows ランタイム型を作成し、Windows ランタイム コンポーネントにパッケージ化することができます。 また、C++、JavaScript、Visual Basic、C# を利用することで、ユニバーサル Windows プラットフォーム (UWP) アプリでコンポーネントを使うことができます。 このトピックでは、コンポーネントを作成するための規則を示し、Windows ランタイム向けの .NET のサポートをいくつか説明します。 このサポートは、通常、.NET のプログラマが意識しなくても利用できるように設計されています。 ただし、JavaScript や C++ で使うコンポーネントを作成する場合は、これらの言語が Windows ランタイムをサポートする方法の違いに注意する必要があります。 |
| [C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | このチュートリアルでは、Visual Basic または C# で .NET を使って、Windows ランタイム コンポーネントにパッケージ化される独自の Windows ランタイム型を作成する方法と、JavaScript を使って Windows 用にビルドされたユニバーサル Windows アプリからコンポーネントを呼び出す方法について説明します。 |
| [Windows ランタイム コンポーネントでイベントを生成する](raising-events-in-windows-runtime-components.md) | Windows ランタイム コンポーネントを使って、ユーザー定義のデリゲート型のイベントをバック グラウンド スレッド (ワーカー スレッド) で発生させ、このイベントを JavaScript で受け取る場合、以下のいずれかの方法でイベントを実装し、発生させることができます。 | 
| [サイド ローディングされた UWP アプリのための Windows ランタイム コンポーネント ブローカー](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | このトピックでは、Windows 10 更新プログラム以降でサポートされるエンタープライズ向け機能について説明します。この機能では、ビジネス クリティカルな操作を実行する既存のコードを、タッチ対応の .NET アプリで使用できるようにします。 |