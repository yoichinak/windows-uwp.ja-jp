---
description: C++/WinRT の紹介 &mdash; Windows ランタイム API 用の標準的な C++ 言語プロジェクション。
title: C++/WinRT の概要
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, プロジェクション, 概要
ms.localizationpriority: medium
ms.openlocfilehash: 39606a1797f56e8bb63f0afb99d7c86d78934662
ms.sourcegitcommit: 6009896ead442b378106d82870f249dc8b55b886
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/10/2020
ms.locfileid: "89643789"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT の概要
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT は Windows ランタイム (WinRT) API の標準的な最新の C++17 言語プロジェクションで、ヘッダー ファイル ベースのライブラリとして実装され、最新の Windows API への最上位アクセス権を提供するように設計されています。 C++/WinRT の場合、標準に準拠した C++17 のコンパイラを使用して Windows ランタイム API を作成および使用できます。 Windows SDK には C++/WinRT が含まれます。バージョン 10.0.17134.0 (Windows 10、バージョン 1803) で導入されました。

C++/WinRT は、[C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 言語プロジェクション、および [Windows ランタイム C++ テンプレート ライブラリ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) に代わる、Microsoft で推奨されているものです。 [C++/WinRT に関するトピック](index.md#topics-about-cwinrt)の完全な一覧には、C++/CX と WRL との相互運用、C++/CX と WRL からの移行の両方に関する情報が含まれます。

> [!IMPORTANT]
> 知っておくべき C++/WinRT の一部の最も重要な部分は、「[C++/WinRT の SDK サポート](#sdk-support-for-cwinrt)」と「[C++/WinRT、XAML、VSIX 拡張機能、NuGet パッケージの Visual Studio のサポート](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)」のセクションで説明されています。

「[C++/WinRT サンプル アプリはどこにありますか?](/windows/uwp/cpp-and-winrt-apis/faq#where-can-i-find-cwinrt-sample-apps)」も参照してください。

## <a name="language-projections"></a>言語プロジェクション
Windows ランタイムは、コンポーネント オブジェクト モデル (COM) API に基づいており、*言語プロジェクション*を使用してアクセスするよう設計されています。 プロジェクションは、COM の詳細を隠し、特定の言語により自然なプログラミング エクスペリエンスを提供します。

### <a name="the-cwinrt-language-projection-in-the-windows-runtime-api-reference-content"></a>Windows ランタイム API リファレンス コンテンツにおける C++/WinRT 言語プロジェクション
[Windows ランタイム API](/uwp/api/) の閲覧中に、右上隅の **[言語]** ボックスをクリックして **[C++/WinRT]** を選択すると、C++/WinRT 言語プロジェクションで使用された API 構文ブロックを表示できます。

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>C++/WinRT、XAML、VSIX 拡張機能、NuGet パッケージの Visual Studio のサポート
Visual Studio のサポートでは、Visual Studio 2019 または Visual Studio 2017 が必要になります (バージョン 15.6 以上、Microsoft では 15.7 以上をお勧めします)。 Visual Studio インストーラーから、**ユニバーサル Windows プラットフォーム開発**ワークロードをインストールします。 まだ行っていない場合は、 **[インストールの詳細]**  >  **[ユニバーサル Windows プラットフォーム開発]** で **[C++ (v14x) ユニバーサル Windows プラットフォーム ツール]** オプションを選択します。 また、Windows の **[設定]**  >  **[更新 \& セキュリティ]**  >  **[開発者向け]** で、 **[アプリのサイドロード]** オプションではなく、 **[開発者モード]** オプションを選びます。

最新バージョンの Visual Studio と Windows SDK で開発することをお勧めしますが、10.0.17763.0 (Windows 10、バージョン 1809) より前の Windows SDK に付属する C++/WinRT のバージョンを使用している場合、上記で示した Windows 名前空間ヘッダーを使用するには、10.0.17134.0 (Windows 10、バージョン 1803) のプロジェクトで最小の Windows SDK ターゲット バージョンが必要になります。

[Visual Studio Marketplace](https://marketplace.visualstudio.com/) から最新バージョンの [C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) をダウンロードしてインストールする必要があります。

- VSIX 拡張機能では、C++/WinRT 開発を開始できるように、Visual Studio で C++/WinRT プロジェクトとアイテム テンプレートが提供されます。
- さらに、これにより、C++/WinRT の投影された型の Visual Studio ネイティブのデバッグの視覚化 (NatVis) が提供され、C# デバッグと同様のエクスペリエンスを実現します。 Natvis はデバッグ ビルドで自動で行われます。 シンボル WINRT_NATVIS を定義することで、リリース ビルドを選択できます。

C++/WinRT 用の Visual Studio プロジェクト テンプレートは、以下のセクションで説明されます。 最新バージョンの VSIX 拡張機能がインストールされた新しい C++/WinRT プロジェクトを作成する場合、新しい C++/WinRT プロジェクトで自動的に [Microsoft.Windows.CppWinRT NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)がインストールされます。 **Microsoft.Windows.CppWinRT** NuGet パッケージでは、C++/WinRT のビルド (MSBuild プロパティとターゲット) がサポートされ、自分のプロジェクトを (NuGet パッケージのみ (VSIX 拡張機能以外) がインストールされる) 開発マシンとビルド エージェントとの間で移植できるようになります。

または、**Microsoft.Windows.CppWinRT** NuGet パッケージを手動でインストールすることで、既存のプロジェクトを変換できます。 最新バージョンの VSIX 拡張機能をインストール (または更新) した後、Visual Studio で既存のプロジェクトを開いて、 **[プロジェクト]** \> **[NuGet パッケージの管理]** \> **[参照]** をクリックし、検索ボックスに「**Microsoft.Windows.CppWinRT**」を入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックして、そのプロジェクトのパッケージをインストールします。 そのパッケージを追加したら、`cppwinrt.exe` ツールの呼び出しを含む、プロジェクトの C++/WinRT MSBuild サポートを取得できます。

> [!IMPORTANT]
> 1\.0.190128.4 より前の VSIX 拡張機能のバージョンで作成された (作業するためにアップグレードされた) プロジェクトがある場合は、「[VSIX 拡張機能の以前のバージョン](#earlier-versions-of-the-vsix-extension)」を参照してください。 そのセクションには、プロジェクトの構成に関する重要な情報が含まれ、最新バージョンの VSIX 拡張機能を使用するためにアップグレードする必要があるものを把握します。

- C++/WinRT では C++17 標準の機能が使用されるので、NuGet パッケージでは Visual Studio でプロジェクト プロパティの **[C/C++]**  >  **[言語]**  >  **[C++ 言語標準]**  >  **[ISO C++17 標準 (/std:c++17)]** を設定します。
- これにより、[/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file) コンパイラ オプションも追加されます。
- `co_await` を有効にするために、[/await](/cpp/build/reference/await-enable-coroutine-support) コンパイラ オプションが追加されます。
- C++/WinRT codegen を出力するよう、XAML コンパイラに指示されます。
- **Conformance mode:Yes (/permissive-)** を設定する可能性もあり、これはコードを標準に準拠しているようにさらに制限します。
- 注意すべきもう 1 つのプロジェクト プロパティは、 **[C/C++]**  >  **[全般]**  >  **[警告をエラーとして扱う]** です。 これをユーザーの好みに合わせて **[はい (/WX)]** または **[いいえ (/WX-)]** に設定します。 場合によっては、`cppwinrt.exe` ツールによって生成されたソース ファイルは、それらに実装を追加するまで警告を生成します。

上記で説明されているように設定されたシステムでは、Visual Studio で C++/WinRT プロジェクトを作成してビルドし (または開いて)、展開することができます。

バージョン 2.0 の **Microsoft.Windows.CppWinRT** NuGet パッケージには、`cppwinrt.exe` ツールが含まれます。 `cppwinrt.exe` ツールは Windows ランタイム メタデータ (`.winmd`) ファイルでポイントして、C++/WinRT コードから使用するためのメタデータに記述されている API を*投影する*ヘッダー ファイル ベースの標準的な C++ ライブラリを生成することができます。 Windows ランタイム メタデータ (`.winmd`) ファイルは、Windows ランタイム API サーフェスを記述する正規の方法を提供します。 メタデータで `cppwinrt.exe` をポイントすることで、セカンド パーティまたはサード パーティの Windows ランタイム コンポーネントに実装された、または独自のアプリケーションに実装された任意のランタイム クラスで使用するためのライブラリを生成することができます。 詳細については、「[C++/WinRT での API の使用](consume-apis.md)」を参照してください。

C++/WinRT では、COM スタイルのプログラミングを使用せずに、標準的な C++ を使用して独自のランタイム クラスを実装することもできます。 ランタイム クラスでは、IDL ファイルで型を記述するだけです。`midl.exe` および `cppwinrt.exe` が実装のスケルトン ソース コード ファイルを自動的に作成します。 または、C++/WinRT の基本クラスから派生することでインターフェイスを実装することもできます。 詳細については、「[C++/WinRT での API の作成](author-apis.md)」を参照してください。

プロジェクトのプロパティによって設定される `cppwinrt.exe` ツールのカスタマイズ オプションの一覧については、Microsoft.Windows.CppWinRT NuGet パッケージの [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) をご覧ください。

プロジェクト内にインストールされた **Microsoft.Windows.CppWinRT** NuGet パッケージのプレゼンスによって、C++/WinRT MSBuild サポートを使用するプロジェクトを識別できます。

VSIX 拡張機能によって提供される Visual Studio プロジェクト テンプレートを次に示します。

### <a name="blank-app-cwinrt"></a>空のアプリ (C++/WinRT)
XAML ユーザー インターフェイスを持つユニバーサル Windows プラットフォーム (UWP) アプリのプロジェクト テンプレートです。

Visual Studio では、各 XAML マークアップ ファイルの背後にあるインターフェイス定義言語 (IDL) (`.idl`) ファイルから実装とヘッダーのスタブを生成するために XAML コンパイラ サポートを提供します。 IDL ファイルで、アプリの XAML ページ内で参照する任意のローカルのランタイム クラスを定義してから、プロジェクトを 1 回ビルドして `Generated Files` で実装テンプレート、`Generated Files\sources` でスタブ型定義を生成します。 次に、ローカルのランタイム クラスの実装への参照にこれらのスタブ型定義を使用します。 「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」を参照してください。

C++/WinRT に対する Visual Studio 2019 での XAML デザイン サーフェスのサポートは、C# でのパリティに近いです。 Visual Studio 2019 では、 **[プロパティ]** ウィンドウの **[イベント]** タブを使用して、C++/WinRT プロジェクト内にイベント ハンドラーを追加できます。 また、自分のコードに手動でイベント ハンドラーを追加することもできます&mdash;詳細については、「[C++/WinRT でのデリゲートを使用したイベントの処理](handle-events.md)」を参照してください。

### <a name="core-app-cwinrt"></a>コア アプリ (C++/WinRT)
XAML を使用しないユニバーサル Windows プラットフォーム (UWP) アプリのプロジェクト テンプレートです。

代わりに、Windows.ApplicationModel.Core 名前空間に C++/WinRT Windows 名前空間ヘッダーを使用します。 ビルドおよび実行した後に、空の領域をクリックして色付きの正方形を追加し、色付きの正方形をクリックしてそれをドラッグします。

### <a name="windows-console-application-cwinrt"></a>Windows コンソール アプリケーション (C++/WinRT)
コンソールのユーザー インターフェイスを含む、Windows デスクトップの C++/WinRT クライアント アプリケーションのプロジェクト テンプレートです。

### <a name="windows-desktop-application-cwinrt"></a>Windows デスクトップ アプリケーション (C++/WinRT)
Windows Desktop 用の C++/WinRT クライアント アプリケーションに対するプロジェクト テンプレートでは、Win32 **MessageBox** 内に Windows ランタイム [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) が表示されます。

### <a name="windows-runtime-component-cwinrt"></a>Windows ランタイム コンポーネント (C++/WinRT)
通常はユニバーサル Windows プラットフォーム (UWP) から使用するための、コンポーネントのプロジェクト テンプレートです。

このテンプレートは、Windows ランタイム メタデータ (`.winmd`) が IDL から生成され、実装とヘッダーのスタブが Windows ランタイム メタデータから生成される、`midl.exe` > `cppwinrt.exe` ツール チェーンを示します。

IDL ファイルでは、コンポーネント、それらの既定インターフェイス、およびそれらが実装している他のすべてのインターフェイスのランタイム クラスを定義します。 プロジェクトを 1 回ビルドして `module.g.cpp`、`module.h.cpp`、`Generated Files` の実装テンプレート、および `Generated Files\sources` のスタブ型定義を生成します。 次にコンポーネント内のランタイム クラスの実装への参照にこれらのスタブ型定義を使用します。 「[ランタイム クラスを Midl ファイル (.idl) にファクタリングする](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)」を参照してください。

ビルドした Windows ランタイム コンポーネントのバイナリとその `.winmd` を、それらを使用する UWP アプリとバンドルします。

## <a name="earlier-versions-of-the-vsix-extension"></a>VSIX 拡張機能の以前のバージョン
最新バージョンの [VSIX 拡張機能](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)をインストール (またはアップグレード) することをお勧めします。 既定では、それ自体で更新されるように構成されています。 これを行い、1.0.190128.4 以前の VSIX 拡張機能のバージョンで作成されたプロジェクトがある場合、このセクションには新しいバージョンで動作するように、これらのプロジェクトのアップグレードに関する重要な情報が含まれます。 更新しない場合は、引き続きこのセクションでの情報が役に立つことがわかります。

サポートされる Windows SDK と Visual Studio のバージョン、Visual Studio の構成に関して、上記の「[C++/WinRT、XAML、VSIX 拡張機能、NuGet パッケージの Visual Studio のサポート](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)」セクションの情報は、以前のバージョンの VSIX 拡張機能に適用されます。 下記の情報は、以前のバージョンで作成された (または作業を行うためにアップグレードされた) 動作とプロジェクトの構成に関して重要な違いについて説明します。

### <a name="created-earlier-than-101810022"></a>1\.0.181002.2 より前に作成
プロジェクトが 1.0.181002.2 より前の VSIX 拡張機能のバージョンで作成された場合、C++/WinRT ビルドのサポートはそのバージョンの VSIX 拡張機能に組み込まれます。 ご利用のプロジェクトには、`.vcxproj` ファイルに `<CppWinRTEnabled>true</CppWinRTEnabled>` プロパティ セットがあります。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

**Microsoft.Windows.CppWinRT** NuGet パッケージを手動でインストールすることで、自分のプロジェクトをアップグレードできます。 最新バージョンの VSIX 拡張機能をインストール (またはアップグレード) した後、Visual Studio で自分のプロジェクトを開いて、 **[プロジェクト]** \> **[NuGet パッケージの管理]** \> **[参照]** をクリックし、検索ボックスに「**Microsoft.Windows.CppWinRT**」を入力するか貼り付けます。検索結果の項目を選択し、 **[インストール]** をクリックして自分のプロジェクトのパッケージをインストールします。

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>1\.0.181002.2 から 1.0.190128.3 の間で作成 (またはアップグレード)
プロジェクトが 1.0.181002.2 から 1.0.190128.3 (含む) の間の VSIX 拡張機能のバージョンで作成された場合、**Microsoft.Windows.CppWinRT** NuGet パッケージはプロジェクト テンプレートによってプロジェクトに自動的にインストールされています。 この範囲の VSIX 拡張機能のバージョンを使用するために、以前のプロジェクトをアップグレードしている可能性もあります。 この操作を行った場合、&mdash;ビルドのサポートは引き続きこの範囲内の VSIX 拡張機能のバージョンでも表されるため&mdash;ご利用のアップグレードされたプロジェクトでは **Microsoft.Windows.CppWinRT** NuGet パッケージをインストールしている、またはしていない可能性があります。

プロジェクトをアップグレードするには、前のセクションの手順に従って、自分のプロジェクトに確実に **Microsoft.Windows.CppWinRT** NuGet パッケージをインストールします。

### <a name="invalid-upgrade-configurations"></a>無効なアップグレードの構成
最新バージョンの VSIX 拡張機能では、**Microsoft.Windows.CppWinRT** NuGet パッケージもインストールされていない場合は、プロジェクトで `<CppWinRTEnabled>true</CppWinRTEnabled>` プロパティを含めることはできません。 この構成のプロジェクトでは、ビルド エラー メッセージの "The C++/WinRT VSIX no longer provides project build support.  Please add a project reference to the Microsoft.Windows.CppWinRT Nuget package" (C++/WinRT VSIX ではプロジェクトのビルドがサポートされなくなりました。Microsoft.Windows.CppWinRT Nuget パッケージへのプロジェクト参照を追加してください) が生成されます。

上記で説明したように、C++/WinRT プロジェクトでは NuGet パッケージがインストールされている必要があるようになりました。

`<CppWinRTEnabled>` 要素は古くなったため、必要に応じて `.vcxproj` を編集して、要素を削除できます。 厳密には必要ではありませんが、これはオプションです。

また、`.vcxproj` には `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>` が含まれる場合は、C++/WinRT VSIX 拡張機能をインストールすることなく、ビルドできるように、これを削除できます。

## <a name="sdk-support-for-cwinrt"></a>C++/WinRT の SDK サポート
これは互換性の理由のためのみで示されていますが、バージョン 10.0.17134.0 (Windows 10 バージョン 1803) の時点で、Windows SDK には、ファーストパーティ Windows API (Windows 名前空間の Windows ランタイム API) を使用するためのヘッダー ファイル ベースの標準的な C++ ライブラリが含まれています。 それらのヘッダーは `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` フォルダー内にあります。 Windows SDK バージョン 10.0.17763.0 (Windows 10、バージョン 1809) の時点で、これらのヘッダーは、プロジェクト内の *$(GeneratedFilesDir)* フォルダー内で生成されます。

繰り返しになりますが、互換性のために、Windows SDK は `cppwinrt.exe` ツールにも付属しています。 しかし、代わりに最新バージョンの `cppwinrt.exe` をインストールして使用することをお勧めします。これは、**Microsoft.Windows.CppWinRT** NuGet パッケージに含まれています。 そのパッケージ (および `cppwinrt.exe`) は、上記のセクションで説明されています。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT プロジェクションにおけるカスタム型
自分の C++/WinRT プログラミングで、標準 C++ 言語機能および[標準 C++ データ型と C++/WinRT](std-cpp-data-types.md) を使用できます&mdash;一部の C++ 標準ライブラリのデータ型を含みます。 ただし、プロジェクションでいくつかのカスタム データ型を認識するようになり、それらを使用することもできます。 たとえば、[C++/WinRT の概要](get-started.md) のクイックスタートのコード例では [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) を使用しています。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) は、あるポイントで使用する可能性が高い別の型です。 ただし、[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) などの型を直接使用する可能性は低いです。 または、対応する型が C++ 標準ライブラリに現れた場合に変更すべきコードがないように、使用しないことを選択する場合もあります。

> [!WARNING]
> C++/WinRT Windows 名前空間ヘッダーをよく調査すると見つかる可能性がある型もあります。 例として **winrt::param::hstring** がありますが、コレクションの例もあります。 これらは入力パラメーターのバインディングを最適化するためにのみ存在し、大幅に改善したパフォーマンスをもたらし、関連する標準的な C++ の型とコンテナーでほとんどの呼び出しパターンが "そのまま機能する" ようにします。 これらの型は、ほとんどの値を追加する場合にプロジェクションでのみ使用されます。 高度に最適化され、一般的な用途で使用するものではありません。それらの型を自分で使用しないようにしてください。 また、`winrt::impl` 名前空間からは何も使用しないでください。それらは実装型であるため、変更されることがあります。 引き続き標準型を使用するか、または [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)の型を使用する必要があります。
>
> また、「[ABI 境界へのパラメーターの受け渡し](./pass-parms-to-abi.md)」もご覧ください。

## <a name="important-apis"></a>重要な API
* [winrt::hstring 構造体](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 名前空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>関連トピック
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 拡張機能 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [C++/WinRT の使用を開始する](get-started.md)
* [標準的な C++ のデータ型と C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT での文字列の処理](strings.md)
* [Windows ランタイム API](/uwp/api/)