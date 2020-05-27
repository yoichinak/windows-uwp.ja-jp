---
title: WinUI 3.0 Preview 1 (2020 年 5 月)
description: WinUI 3.0 Preview の概要。
ms.date: 05/14/2020
ms.topic: article
ms.openlocfilehash: 3aac14807f8455eb9a9294c40ddc76ddfa224659
ms.sourcegitcommit: 7e8c7f89212c88dcc0274c69d2c3365194c0954a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2020
ms.locfileid: "83688487"
---
# <a name="windows-ui-library-30-preview-1-may-2020"></a>Windows UI ライブラリ 3.0 Preview 1 (2020 年 5 月)

Windows UI ライブラリ (WinUI) 3.0 は、Win32 から UWP に至るまで、すべての種類の Windows アプリで、WinUI を完全な UX フレームワークに変換する主要な更新プログラムです。

> [!Important]
> この WinUI 3.0 Preview リリースは、早期評価と、開発者コミュニティからのフィードバックの収集を目的としたもので、 実稼働アプリには使用**できません**。
>
> **[Preview 1 の制限事項と既知の問題](#preview-1-limitations-and-known-issues)** を参照してください。
## <a name="new-features-in-winui-30-preview-1"></a>WinUI 3.0 Preview 1 の新機能

- WinUI を使用してデスクトップ アプリを作成する機能 (Win32 アプリ用の [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) を含む)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView の更新](/windows/uwp/design/controls-and-patterns/tab-view)
- ダーク テーマの更新
- [WebView2](https://docs.microsoft.com/microsoft-edge/hosting/webview2) の機能強化と更新
  - 高 DPI のサポート
  - ウィンドウのサイズ変更と移動のサポート
  - より新しいバージョンの Edge を対象とするよう更新されました
  - WebView2 固有の Nuget パッケージの参照が不要になりました
- SwapChainPanel
- オープン ソースの移行に必要な機能強化

WinUI 3.0 および WinUI のロードマップと利点の詳細については、GitHub の「[Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)」を参照してください。

### <a name="provide-feedback-and-suggestions"></a>フィードバックやご提案の送信

[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)にフィードバックをお寄せください。

## <a name="try-winui-30-preview-1"></a>WinUI 3.0 Preview 1 を試す

開発環境を構成 (詳細な手順については「[開発環境の構成](#configure-your-dev-environment)」を参照) し、次のリンクから WinUI 3.0 Preview 1 VSIX をインストールして、WinUI 3.0 プロジェクト テンプレートをお試しください。

<table>
<tr>
<td align="center">
<a href="https://aka.ms/winui3/previewdownload"><img src="images/downloadbuttontx.png" alt="Download the WinUI 3.0 Preview 1 VSIX"/></a>
<!--
<br/>
<a href="https://aka.ms/winui3/previewdownload">Download the WinUI 3.0 Preview 1 VSIX</a>
-->
</td>
</tr>
</table>

[XAML コントロール ギャラリー](#xaml-controls-gallery-winui-30-preview-1-branch)の WinUI 3.0 Preview 1 バージョンを複製してビルドすることもできます。

### <a name="configure-your-dev-environment"></a>開発環境の構成

開発用コンピューターに Windows 10 April 2018 Update (バージョン 1803 - ビルド 17134) 以降がインストールされていることを確認します。

Visual Studio 2019 バージョン 16.7 Preview 1 をインストールします。 これは [Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview) からダウンロードできます。

Visual Studio Preview をインストールする際、次のワークロードを含める必要があります。

- .NET デスクトップ開発
- ユニバーサル Windows プラットフォームの開発

C++ アプリをビルドするには、次のワークロードも含める必要があります。

- C++ によるデスクトップ開発
- ユニバーサル Windows プラットフォーム ワークロードの "*C++ (v142) ユニバーサル Windows プラットフォーム ツール*" のオプション コンポーネント

### <a name="visual-studio-project-templates"></a>Visual Studio プロジェクト テンプレート

WinUI 3.0 Preview 1 とプロジェクト テンプレートにアクセスするには、 **https://aka.ms/winui3/previewdownload** に移動してください。

Visual Studio 拡張機能 (`.vsix`) をダウンロードして、WinUI プロジェクト テンプレートおよび NuGget パッケージを Visual Studio 2019 に追加します。

`.vsix` を Visual Studio に追加する方法については、[Visual Studio 拡張機能を見つけて使用する](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2019#install-without-using-the-manage-extensions-dialog-box)方法に関するページをご覧ください。

`.vsix` 拡張機能をインストールした後、"WinUI" を検索し、使用可能な C# または C++ テンプレートのいずれかを選択して、新しい WinUI 3.0 プロジェクトを作成することができます。

![WinUI 3.0 Visual Studio テンプレート](images/WinUI3Templates.png)<br/>
*WinUI 3.0 Visual Studio テンプレートの例*

### <a name="visual-studio-project-configuration"></a>Visual Studio プロジェクト構成

WinUI 3.0 Preview 1 テンプレートのいずれかを使用してプロジェクトを作成する場合は、**ターゲット バージョン**を Windows 10 バージョン 1903 (ビルド 18362) に設定し、**最小バージョン**を Windows 10 バージョン 1803 (ビルド 17134) に設定します。

プロジェクトを作成した後でこれらの値を変更するには、**ソリューション エクスプローラー** でプロジェクトを右クリックし、 **[プロパティ]** を選択します。

### <a name="creating-a-desktop-win32-app-with-winui-30-preview-1"></a>WinUI 3.0 Preview 1 を使用したデスクトップ Win32 アプリの作成

「[デスクトップ アプリ用の WinUI 3.0 の概要](get-started-winui3-for-desktop.md)」を参照してください。

以下で説明する制限や既知の問題を除いて、WinUI 3.0 Preview 1 を使用したアプリのビルドは、XAML と WinUI 2.x を使用した UWP アプリのビルドとよく似ているため、UWP アプリおよび `Windows.UI` API に関するほとんどのガイダンスとドキュメントを適用できます。

## <a name="preview-1-limitations-and-known-issues"></a>Preview 1 の制限事項と既知の問題

Preview 1 リリースはプレビュー版にすぎません。 デスクトップ Win32 アプリに関するシナリオは特に新しいものです。 バグ、制限事項、および問題があることを想定してください。

次の項目は、WinUI 3.0 Preview 1 に関する既知の問題の一部です。 以下に列挙していない問題が見つかった場合は、[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)で、既存の問題に投稿するか、または新規の問題として投稿して、Microsoft にお知らせください。

### <a name="platform-and-os-support"></a>プラットフォームと OS のサポート

WinUI 3.0 Preview 1 は、Windows 10 April 2018 Update (バージョン 1803 - ビルド 17134) 以降を実行する PC と互換性があります。

### <a name="developer-tools"></a>開発者ツール

- デスクトップ アプリは .NET 5 および C# 8 をサポートしています。また、パッケージ化する必要があります。
- UWP アプリは .NET Native および C# 7.3 をサポートしています
- Intellisense が不完全です
- ビジュアルなデザイナーがありません
- ホット リロードに対応していません
- ライブ ビジュアル ツリーには対応していません
- VS Code を使用した開発は未対応です
- 新しい C++ /cx アプリはサポートされていませんが、既存のアプリは引き続き機能します ( C++ /WinRT にできるだけ早く移行してください)
- WinUI 3.0 コンテンツは、プロセスごとに 1 つのウィンドウ、またはアプリごとに 1 つの ApplicationView でしか使用できません
- パッケージ化されていないデスクトップ展開はサポートしていません
- ARM64 はサポートしていません

### <a name="missing-platform-features"></a>ラットフォーム機能が不足しています

- Xbox には対応していません
- HoloLens には対応していません
- ウィンドウのポップアップはサポートしていません
- 手描き入力はサポートしていません
- 背景アクリル
- MediaElement および MediaPlayerElement
- RenderTargetBitmap
- MapControl
- NavigationView を使用した階層型ナビゲーション
- SwapChainPanel は透明度をサポートしていません
- グローバルな表示でフォールバック動作 (純色ブラシ) を使用します
- 今回のリリースでは XAML Islands はサポートしていません
- サードパーティ製エコシステム ライブラリは完全には機能しません
- IME は機能しません
- Windows.UI.Text 名前空間のメソッドを呼び出すことはできません

### <a name="known-issues"></a>既知の問題

- C# デスクトップ アプリ:
   - Windows オブジェクト (Xaml オブジェクトを含む) への弱い参照には、`System.WeakReference<T>` ではなく `WinRT.WeakReference<T>` を使用する必要があります。
   - [Point](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)、[Rect](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)、および [Size](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size) 構造体は、Double 型ではなく Float 型のメンバーを持ちます。


## <a name="xaml-controls-gallery-winui-30-preview-1-branch"></a>XAML コントロール ギャラリー (WinUI 3.0 Preview 1 ブランチ)

すべての WinUI 3.0 Preview 1 コントロールおよび機能を含むサンプル アプリについては、[XAML コントロール ギャラリーの WinUI 3.0 Preview 1 ブランチ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)を参照してください。

![WinUI 3.0 Preview 1 XAML コントロール ギャラリー アプリ](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3.0 Preview 1 XAML コントロール ギャラリー アプリの例*

サンプルをダウンロードするには、次のコマンドを使用して **winui3preview** ブランチを複製します。

> `git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git`

複製した後、確実に、ローカルの Git 環境で **winui3preview** ブランチに切り替えてください。

> `git checkout winui3preview`
