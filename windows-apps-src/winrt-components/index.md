---
title: Windows ランタイム コンポーネント
description: Windows ランタイム コンポーネントは自己完結型オブジェクトで、C#、Visual Basic、JavaScript、C++ など、すべての言語からインスタンス化して使用することができます。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76026c4f499a068bab689eadf050e0ec6277bbe8
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393683"
---
# <a name="windows-runtime-components"></a>Windows ランタイム コンポーネント
Windows ランタイム コンポーネントは自己完結型オブジェクトで、C#、Visual Basic、JavaScript、C++ など、すべての言語からインスタンス化して使用することができます。

Visual Studio と C#、Visual Basic、または C++ を使って、ユニバーサル Windows プラットフォーム (UWP) アプリで使用できる Windows ランタイム コンポーネントを作成できます。

| トピック | 説明 |
|-------|-------------|
| [C++/CX を使用した Windows ランタイム コンポーネント](creating-windows-runtime-components-in-cpp.md) | このトピックでは、C++/CX を使って Windows ランタイム コンポーネントを作成する方法を示します。このコンポーネントは、Windows ランタイム言語を使って構築しされたユニバーサル Windows アプリから呼び出すことができます。 |
| [C++/CX Windows ランタイム コンポーネントの作成と JavaScript または C# からの呼び出しに関するチュートリアル](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | このチュートリアルでは、JavaScript、C#、または Visual Basic から呼び出すことができる基本的な Windows ランタイム コンポーネント DLL を作成する方法について説明します。 このチュートリアルを開始する前に、抽象バイナリ インターフェイス (ABI)、ref クラス、Visual C++ コンポーネント拡張などの概念を必ず理解しておいてください。ref クラスの操作が容易になります。 詳しくは、「[C++ での Windows ランタイム コンポーネントの作成](creating-windows-runtime-components-in-cpp.md)」および「[Visual C++ の言語リファレンス (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)」をご覧ください。 |
| [C# および Visual Basic を使用した Windows ランタイム コンポーネント](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | マネージ コードを使って独自の Windows ランタイム型を作成し、Windows ランタイム コンポーネントにパッケージ化することができます。 また、C++、JavaScript、Visual Basic、C# を利用することで、ユニバーサル Windows プラットフォーム (UWP) アプリでコンポーネントを使うことができます。 このトピックでは、コンポーネントを作成するための規則を示し、Windows ランタイム向けの .NET のサポートをいくつか説明します。 このサポートは、通常、.NET のプログラマが意識しなくても利用できるように設計されています。 ただし、JavaScript や C++ で使うコンポーネントを作成する場合は、これらの言語が Windows ランタイムをサポートする方法の違いに注意する必要があります。 |
| [C# または Visual Basic Windows ランタイム コンポーネントの作成と JavaScript からの呼び出しに関するチュートリアル](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | このチュートリアルでは、Visual Basic または C# で .NET を使って、Windows ランタイム コンポーネントにパッケージ化される独自の Windows ランタイム型を作成する方法と、JavaScript を使って Windows 用にビルドされたユニバーサル Windows アプリからコンポーネントを呼び出す方法について説明します。 |
| [Windows ランタイム コンポーネントでイベントを生成する](raising-events-in-windows-runtime-components.md) | Windows ランタイム コンポーネントを使って、ユーザー定義のデリゲート型のイベントをバック グラウンド スレッド (ワーカー スレッド) で発生させ、このイベントを JavaScript で受け取る場合、以下のいずれかの方法でイベントを実装し、発生させることができます。 | 
| [サイド ローディングされた UWP アプリのための Windows ランタイム コンポーネント ブローカー](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | このトピックでは、Windows 10 更新プログラム以降でサポートされるエンタープライズ向け機能について説明します。この機能では、ビジネス クリティカルな操作を実行する既存のコードを、タッチ対応の .NET アプリで使用できるようにします。 |
