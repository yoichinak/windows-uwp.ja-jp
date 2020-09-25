---
description: C++/WinRT は、ヘッダー ファイル ベースのライブラリとして実装された、Windows ランタイム (WinRT) API 用に完全に標準化された最新の C++17 言語プロジェクションです。
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10、uwp、標準、c++、cpp、winrt、プロジェクション
ms.localizationpriority: medium
ms.openlocfilehash: b3ad3a204b2eb4b288e3d3c313a0832bea124de0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219895"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) は Windows ランタイム (WinRT) API 用に完全に標準化された最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 C++/WinRT の場合、標準に準拠した C++17 のコンパイラを使用して Windows ランタイム API を作成および使用できます。 Windows SDK には C++/WinRT が含まれます。バージョン 10.0.17134.0 (Windows 10、バージョン 1803) で導入されました。

C++/WinRT は、Windows 用の美しく高速なコードの作成に興味のある開発者向けです。 次に、その理由を示します。

## <a name="the-case-for-cwinrt"></a>C++/WinRT の場合
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

C++ プログラミング言語は、企業*と*アプリケーションの独立系ソフトウェア ベンダー (ISV) セグメントの両方で使用されています。これらでは、正確性、品質、パフォーマンスが高く評価されています。 たとえば、システム プログラミング、リソースが限られた埋め込みシステムやモバイル システム、ゲームやグラフィックス、デバイス ドライバー、工業用、科学技術用、医療用医療アプリケーションなどです。

言語の観点から見ると、C++ では、型が豊富で軽量な抽象化を常に作成および使用します。 ただし、RAW ポインター、RAW ループ、煩雑なメモリ割り当て、C++98 のリリース以降、言語は急激に変化しました。 最新の C++ (C++11 以降) は、アイデアの明確な表現、シンプルさ、読みやすさ、バグ発生の大幅な軽減が中心です。

C++ を使用して Windows ランタイム API を作成および使用する場合は、C++/WinRT を使用します。 これは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 言語プロジェクション、および [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) に代わる、Microsoft で推奨されているものです。

C++/WinRT を使用する場合は、標準的な C++ データ型、アルゴリズム、およびキーワードを使用します。 プロジェクションに独自のカスタム データ型はありませんが、多くの場合、標準型から適切に変換されるため、学習する必要はありません。 このため、使い慣れた標準的な C++ 言語機能と使用中のソース コードを使用し続けることができます。 C++/WinRT では、C++ アプリケーションで Windows ランタイム API を Win32 から UWP に非常に簡単に呼び出すことができます。

Windows ランタイムの他の言語オプションに比べて、C++/WinRT はパフォーマンスが向上し、小さなバイナリを生成します。 ABI インターフェイスを直接使用した手書きのコードよりも優れています。 このため、抽象化では、Visual C++ コンパイラが最適化するように設計されている最新の C++ イディオムを使用します。 これには、C++/WinRT のパフォーマンス向上に特化した最新の Visual C++ の多くの新しい最適化機能だけでなく、マジック static、空の基底クラス、**strlen** の省略も含まれます。

C++/WinRT をお客様のプロジェクトに段階的に導入する方法がいくつかあります。 [Windows ランタイム コンポーネント](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)を使用することも、C++/CX と相互運用することもできます。 詳細については、[C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md)をご覧ください。

C++/WinRT への移植の詳細については、これらのリソースをご覧ください。

- [C++/CX から C++/WinRT への移行](./move-to-winrt-from-cx.md)
- [WRL から C++/WinRT への移行](./move-to-winrt-from-wrl.md)
- [C# から C++/WinRT への移行](./move-to-winrt-from-csharp.md)

「[C++/WinRT サンプル アプリはどこにありますか](./faq.md#where-can-i-find-cwinrt-sample-apps)」もご覧ください。

### <a name="topics-about-cwinrt"></a>C++/WinRT に関するトピック

| トピック | 説明 |
| - | - |
| [C++/WinRT の概要](./intro-to-using-cpp-with-winrt.md) | C++/WinRT の紹介 &mdash; Windows ランタイム API 用の標準的な C++ 言語プロジェクション。 |
| [C++/WinRT の使用を開始する](./get-started.md) | C++/WinRT の使用をすぐに開始できるように、このトピックでは、単純なコード例について説明します。 |
| [C++/WinRT の新機能](./news.md) | C++/WinRT に関するニュースと変更内容です。 |
| [よく寄せられる質問](./faq.md) | C++/WinRT での Windows ランタイム API の作成と使用に関する質問への回答です。 |
| [トラブルシューティング](./troubleshooting.md) | このトピックにある現象のトラブルシューティングおよび対処法に関する表は、新しいコードを作成しているか既存のアプリを移植しているかにはかかわらず役立つ可能性があります。 |
| [フォト エディター C++/WinRT サンプル アプリケーション](./photo-editor-sample.md) | フォト エディターは、C++/WinRT 言語プロジェクションでの開発を紹介する UWP のサンプル アプリケーションです。 サンプル アプリケーションを使用すると、**画像**ライブラリから写真を取得し、関連する写真効果で選択したイメージを編集できます。 | 
| [文字列の処理](./strings.md) | C++/WinRT では、標準的な C++ ワイド文字列型を使用して Windows ランタイム API を呼び出すか、[**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 型を使用することができます。 |
| [標準的な C++ のデータ型と C++/WinRT](./std-cpp-data-types.md) | C++/WinRT では、標準的な C++ データ型を使用して Windows ランタイム API を呼び出すことができます。 |
| [IInspectable へのスカラー値のボックス化とボックス化解除](./boxing.md) | スカラー値は、**IInspectable** を想定している関数に渡す前に、参照クラス オブジェクト内にラップする必要があります。 このラッピング プロセスは、値の*ボックス化*と呼ばれます。 |
| [C++/WinRT で API を使用する](./consume-apis.md) | このトピックでは、C++/WinRT API を実装する Windows、サードパーティ コンポーネント ベンダー、またはユーザー自身に応じた使用方法について説明します。 |
| [C++/WinRT で API を作成する](./author-apis.md) | このトピックでは、直接的または間接的に **winrt::implements** 基本構造体を使用して、C++/WinRT API を作成する方法を示します。 |
| [C++/WinRT でのエラー処理](./error-handling.md) | このトピックでは、C++/WinRT でのプログラミング時にエラーを処理するための方法について説明します。 |
| [デリゲートを使用したイベントの処理](./handle-events.md) | このトピックでは、C++/WinRT を使用したイベント処理デリゲートの登録方法と取り消し方法について説明します。 |
| [イベントの作成](./author-events.md) | このトピックでは、イベントを発生させるランタイム クラスを含む、Windows ランタイム コンポーネントを作成する方法を示します。 コンポーネントを使用してイベントを処理するアプリも示します。 |
| [C++/WinRT でのコレクション](./collections.md) | C++/WinRT では、コレクションを実装したり、渡すときの時間と手間を大幅に減らすことができる、関数と基本クラスが提供されます。 |
| [同時開催操作と非同期操作](./concurrency.md) | このトピックでは、C++/WinRT を使用した Windows ランタイムの非同期オブジェクトの作成方法と利用方法について説明します。 |
| [より高度な同時実行操作と非同期操作](./concurrency-2.md) | C++/WinRT での同時実行操作と非同期操作を使用したより高度なシナリオを示します。 |
| [XAML コントロール: C++/WinRT プロパティへのバインド](./binding-property.md) | XAML コントロールに効果的にバインドできるプロパティは、*監視可能な*プロパティと呼ばれます。 このトピックでは、監視可能なプロパティを実装および使用する方法と、XAML コントロールをバインドする方法を示します。 |
| [XAML アイテム コントロール: C++/WinRT コレクションへのバインド](./binding-collection.md) | XAML アイテム コントロールに効果的にバインドできるコレクションは、*監視可能な*コレクションと呼ばれます。 このトピックでは、監視可能なコレクションを実装および使用する方法と、それに XAML アイテム コントロールをバインドする方法を示します。 |
| [C++/WinRT による XAML カスタム (テンプレート化) コントロール](./xaml-cust-ctrl.md) | このトピックでは、C++/WinRT を使用して、シンプルなカスタム コントロールを作成する手順について説明します。 ここに示されている情報に基づき、豊富な機能のカスタム可能な独自の UI コントロールを作成することができます。 |
| [ABI 境界へのパラメーターの受け渡し](./pass-parms-to-abi.md) | C++/WinRT では、一般的なケースの自動変換を提供することによって、ABI 境界へのパラメーターの受け渡しが簡略化されます。 |
| [C++/WinRT での COM コンポーネントの使用](./consume-com.md) | このトピックでは、完全な Direct2D コードの例を使用し、C++/WinRT を使って COM クラスとインターフェイスを利用する方法を示します。 |
| [C++/WinRT での COM コンポーネントの作成](./author-coclasses.md) | C++/WinRT は、Windows Runtime クラスを作成するのに役立つのと同様に、従来の COM コンポーネントを作成するのに役立ちます。 |
| [C++/CX から C++/WinRT への移行](./move-to-winrt-from-cx.md) | このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) プロジェクト内のソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植することに関する技術的な詳細を取り上げています。 |
| [C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx.md) | このトピックでは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) と [C++/WinRT](./intro-to-using-cpp-with-winrt.md) オブジェクト間の変換に使用できる 2 つのヘルパー関数について説明します。 |
| [非同期性、および C++/WinRT と C++/CX 間の相互運用](./interop-winrt-cx-async.md) | これは、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) から [C++/WinRT](./intro-to-using-cpp-with-winrt.md) への段階的な移植に関する高度なトピックです。 ここでは、並列パターン ライブラリ (PPL) のタスクとコルーチンを同じプロジェクトに並べて配置できる方法について説明します。 |
| [WRL から C++/WinRT への移行](./move-to-winrt-from-wrl.md) | このトピックでは、[Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) のコードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植する方法について説明します。 |
| [C# から C++/WinRT へのクリップボード サンプルの移植&mdash;ケース スタディ](./clipboard-to-winrt-from-csharp.md) | このトピックでは、[ユニバーサル Windows プラットフォーム (UWP) アプリのサンプル](https://github.com/microsoft/Windows-universal-samples)のいずれかを [C#](/visualstudio/get-started/csharp) から [C++/WinRT](./intro-to-using-cpp-with-winrt.md) へ移植するケース スタディについて説明します。 チュートリアルの手順を実行し、サンプルを自分で移植することにより、移植を練習し経験を得ることができます。 |
| [C# から C++/WinRT への移行](./move-to-winrt-from-csharp.md) | このトピックでは、[C#](/visualstudio/get-started/csharp) プロジェクト内のソース コードを [C++/WinRT](./intro-to-using-cpp-with-winrt.md) の同等のコードに移植することに関する技術的な詳細を総合的に分類します。 |
| [C++/WinRT と ABI 間の相互運用](./interop-winrt-abi.md) | このトピックでは、アプリケーション バイナリ インターフェイス (ABI) と C++/WinRT オブジェクト間の変換方法について説明します。 |
| [C++/WinRT の強参照と弱参照](./weak-references.md) | Windows ランタイムは参照カウント システムです。このようなシステムでは、強参照と弱参照の重要性とこれらの違いを認識することが重要です。 |
| [アジャイル オブジェクト](./agile-objects.md) | アジャイル オブジェクトは、いずれかのスレッドからアクセスできます。 お使いの C++/WinRT 型は既定ではアジャイルですが、オプトアウトできます。 |
| [直接割当ての診断](./diag-direct-alloc.md) | このトピックでは、ヘルパーの [**winrt::make**](/uwp/cpp-ref-for-winrt/make) ファミリを使用するのではなく、スタック上に実装型のオブジェクトを作成するという間違いを診断するのに役立つ C++/WinRT 2.0 機能について詳しく説明します。 |
| [実装の種類の拡張ポイント](./details-about-destructors.md) | C++/WinRT 2.0 のこれらの拡張ポイントを使用すると、実装の種類の破棄を延期したり、破棄中に安全にクエリを実行したり、プロジェクションが実行された方法に対してエントリをフックしたりすることができます。 |
| [単純な C++/WinRT Windows UI ライブラリの例](./simple-winui-example.md) | このトピックでは、C++/WinRT プロジェクト内に WinUI の単純なサポートを追加する処理を順を追って説明します。 |
| [C++/WinRT を使用した Windows ランタイム コンポーネント](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) | このトピックでは、C++/WinRT を使用して Windows ランタイム コンポーネントを作成および使用する方法を示します。このコンポーネントは、任意の Windows ランタイム言語を使用してビルドされたユニバーサル Windows アプリから呼び出すことができます。 |

### <a name="topics-about-the-c-language"></a>C++ 言語に関するトピック

| トピック | 説明 |
| - | - |
| [値のカテゴリと、その参照](./cpp-value-categories.md) | このトピックでは、C++ に存在する値のさまざまなカテゴリについて説明します。 lvalues と rvalues については聞いたことがあると思いますが、それ以外の種類もあります。 |

## <a name="important-apis"></a>重要な API
* [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>関連トピック
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)