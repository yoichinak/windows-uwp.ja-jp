---
title: WinUI 3 Preview 4 (2021 年 2 月)
description: WinUI 3 Preview 4 リリースの概要。
ms.date: 02/09/2021
ms.topic: article
ms.openlocfilehash: a6c74ac64e3384b5a1f5cdc466b4faf441f14445
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824086"
---
# <a name="windows-ui-library-3-preview-4-february-2021"></a>Windows UI ライブラリ 3 Preview 4 (2021 年 2 月)

Windows UI ライブラリ (WinUI) 3 は、Windows デスクトップと UWP の両アプリに対応したネイティブ ユーザー エクスペリエンス (UX) フレームワークです。

**WinUI 3 Preview 4** は安定性プレビュー リリースであり、重要なバグ修正とその他の一般的な機能強化が含まれます (「 **[Preview 4 で導入された機能](#capabilities-introduced-in-preview-4)** 」を参照してください)。

> [!Important]
> この WinUI 3 Preview リリースは、早期評価と、開発者コミュニティからのフィードバックの収集を目的としています。 実稼働アプリには使用 **できません**。
>
> WinUI 3 のプレビュー リリースは 2021 年も引き続き提供され、その後 2021 年 3 月に最初の公式にサポートされたリリースが提供されます。
>
> [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、フィードバックを提供し、提案と問題をログに記録してください。

## <a name="install-winui-3-preview-4"></a>WinUI 3 Preview 4 をインストールする

WinUI 3 Preview 4 には、WinUI ベースのユーザー インターフェイスを備えたアプリの構築を始めるときに役立つ Visual Studio プロジェクト テンプレートと、WinUI ライブラリが収められた NuGet パッケージが含まれています。 WinUI 3 Preview 4 をインストールするには、次の手順のようにします。

> [!NOTE]
> [XAML コントロール ギャラリー](#xaml-controls-gallery-winui-3-preview-4-branch)の WinUI 3 Preview 4 バージョンをクローンして構築することもできます。

1. 開発用コンピューターに Windows 10 バージョン 1803 (ビルド 17134) 以降がインストールされていることを確認します。

2. [Visual Studio 2019 バージョン 16.9 Preview](https://visualstudio.microsoft.com/vs/preview/) をインストールします。 **最新のプレビュー** をダウンロードして、ワークロードに必要なすべての更新プログラム (.NET 5 など) を確実に入手します。

    Visual Studio をインストールする際、次のワークロードを含める必要があります。
    - ユニバーサル Windows プラットフォームの開発

    .NET アプリを構築するには、次のワークロードも含める必要があります。
    - .NET デスクトップ開発 (これにより、後で必要になる .NET 5 の最新バージョンもインストールされます)

    C++ アプリを構築するには、次のワークロードも含める必要があります。
    - C++ によるデスクトップ開発
    - ユニバーサル Windows プラットフォーム ワークロード用の *C++ (v142) ユニバーサル Windows プラットフォーム ツール* のオプション コンポーネント (右ペインにある [ユニバーサル Windows プラットフォーム開発] セクションの [インストールの詳細] を参照してください)

3. **nuget.org** に対して NuGet パッケージ ソースがシステムで有効になっていることを確認します。詳細については、「[一般的な NuGet 構成](/nuget/consume-packages/configuring-nuget-behavior)」を参照してください。

4. [WinUI 3 Preview 4 VSIX パッケージ](https://aka.ms/winui3/preview4-download)をダウンロードしてインストールします。 これにより、WinUI 3 プロジェクト テンプレートと、WinUI 3 ライブラリを含む NuGet パッケージの両方が Visual Studio 2019 に追加されます。

    VSIX パッケージを Visual Studio に追加する方法については、[Visual Studio 拡張機能を見つけて使用する方法](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)に関するページをご覧ください。

5. ライブ ビジュアル ツリー、ホット リロード、ライブ プロパティ エクスプローラーなどの WinUI 3 のツールを使用するには、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI 3 ツールを有効にする必要があります。

#### <a name="webview2"></a>WebView2
WinUI 3 Preview 4 で WebView2 を使用するには、[こちらのページ](https://developer.microsoft.com/microsoft-edge/webview2/)にある Evergreen Bootstrapper または Evergreen Standalone Installer をダウンロードしてください。 

#### <a name="windows-community-toolkit"></a>Windows Community Toolkit

Windows コミュニティ ツールキットを使用している場合は、[最新バージョンをダウンロード](https://aka.ms/wct-winui3)してください。

## <a name="create-winui-projects"></a>WinUI プロジェクトを作成する

WinUI 3 Preview 4 VSIX パッケージをインストールすると、Visual Studio の WinUI プロジェクト テンプレートのいずれかを使用して新しいプロジェクトを作成できるようになります。 **[新しいプロジェクトの作成]** ダイアログで WinUI プロジェクト テンプレートにアクセスするには、言語を **C++** または **C#** に、プラットフォームを **Windows** に、プロジェクトの種類を **WinUI** にフィルター処理します。 または、*WinUI* を検索して、使用できる C# または C++ テンプレートのいずれかを選択することもできます。

![WinUI プロジェクト テンプレート](images/winui-projects-csharp.png)

WinUI プロジェクト テンプレートの概要については、次の記事を参照してください。

- [デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)
- [UWP アプリ用の WinUI 3 の概要](get-started-winui3-for-uwp.md)

[制限と既知の問題](#limitations-and-known-issues)を除くと、WinUI プロジェクトを使用したアプリの構築は、XAML と WinUI 2.x を使用した UWP アプリの構築と似ています。 そのため、Windows SDK の UWP アプリと **Windows.UI** WinRT 名前空間の [ガイダンス ドキュメント](/windows/uwp/design/)のほとんどを適用できます。

このリリースの API リファレンス ドキュメントは近日公開予定です。 利用できるようになったら、ここでリンクを提供します。 それまでの間は、[Preview 3 用の WinUI 3 API リファレンス ドキュメント](/windows/winui/api/)を自由にご覧ください。

WinUI 3 Preview 3 を使用してプロジェクトを作成した場合、Preview 4 を使用するようにプロジェクトをアップグレードすることができます。 詳細な手順については、[WinUI の GitHub リポジトリ](https://aka.ms/winui3/upgrade-instructions)を参照してください。

### <a name="project-templates-for-winui-3"></a>WinUI 3 用のプロジェクト テンプレート

これらの WinUI プロジェクト テンプレートを使用してアプリを作成することができます。

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| [Blank App, Packaged (WinUI in Desktop)]\(空のアプリ、パッケージ (WinUI in Desktop)\) | C++ および C# | WinUI ベースのユーザー インターフェイスを使用して、デスクトップ .NET 5 (C#) またはネイティブ Win32 (C++) アプリを作成します。 生成されるプロジェクトには、UI の構築を始める際に使用できる WinUI ライブラリの **Microsoft.UI.Xaml.Window** クラスから派生する基本ウィンドウが含まれています。 このプロジェクトの種類の詳細については、「[デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)」を参照してください。<p></p>また、このソリューションには、アプリを [MSIX パッケージ](/windows/msix/overview)に組み込むよう構成されている [Windows アプリケーション パッケージ プロジェクト](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)も含まれています。 これによって、最新のデプロイ エクスペリエンスがもたらされ、パッケージ拡張機能を使用して Windows 10 の機能と統合できるなど、さまざまなメリットがあります。  |
| 空のアプリ (UWP の WinUI)  | C++ および C# | WinUI ベースのユーザー インターフェイスを備えた UWP アプリを作成します。 生成されるプロジェクトには、UI の構築を始める際に使用できる、WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Page** クラスから派生した基本的なページが含まれています。 このプロジェクトの種類の詳細については、「[UWP アプリ用の WinUI 3 の概要](get-started-winui3-for-uwp.md)」を参照してください。 |

これらの WinUI プロジェクト テンプレートを使用すると、WinUI ベースのアプリで読み込んで使用できるコンポーネントを構築することができます。

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| [Class Library (WinUI in Desktop)]\(クラス ライブラリ (WinUI in Desktop)\) | C# のみ | WinUI ベースのユーザー インターフェイスを備えた他の .NET 5 デスクトップ アプリで使用できる .NET 5 マネージド クラス ライブラリ (DLL) を C# で作成します。  |
| クラス ライブラリ (UWP の WinUI)  | C# のみ | WinUI ベースのユーザー インターフェイスを備えた他の UWP アプリで使用できるマネージド クラス ライブラリ (DLL) を C# で作成します。 |
| Windows ランタイム コンポーネント (WinUI) | C++ | C++/WinRT で記述された [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。これは、アプリの記述に使用したプログラミング言語に関係なく、WinUI ベースのユーザー インターフェイスを備えた任意の UWP またはデスクトップ アプリで使用できます。 |
| Windows ランタイム コンポーネント (UWP) | C# | C# で記述された [Windows ランタイム コンポーネント](/windows/uwp/winrt-components/)を作成します。これは、アプリの記述に使用したプログラミング言語に関係なく、WinUI ベースのユーザー インターフェイスを備えた任意の UWP アプリで使用できます。 |

### <a name="item-templates-for-winui-3"></a>WinUI 3 用の項目テンプレート

次の項目テンプレートは、WinUI プロジェクトで使用できます。 これらの WinUI 項目テンプレートにアクセスするには、**ソリューション エクスプローラー** でプロジェクト ノードを右クリックし、 **[追加]**  ->  **[新しい項目]** を選択し、 **[新しい項目の追加]** ダイアログの **[WinUI]** をクリックします。

![WinUI の項目テンプレート](images/winui-items-csharp.png)

| テンプレート | Language | 説明 |
|----------|----------|-------------|
| 空白のページ (WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Page** クラスから派生する新しいページを定義する XAML ファイルとコード ファイルを追加します。 |
| 空のウィンドウ (デスクトップの WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Window** クラスから派生する新しいウィンドウを定義する XAML ファイルとコード ファイルを追加します。 |
| カスタム コントロール (WinUI) | C++ および C# | 既定スタイルを使用してテンプレート化されたコントロールを作成するためのコード ファイルを追加します。 テンプレート化されたコントロールは、WinUI ライブラリの **Microsoft.UI.Xaml.Controls.Control** クラスから派生します。<p></p>この項目テンプレートの使用方法を示すチュートリアルについては、「[C++/WinRT を使用した UWP および WinUI 3 アプリ用のテンプレート化された XAML コントロール](xaml-templated-controls-cppwinrt-winui-3.md)」および「[C# を使用した UWP および WinUI 3 アプリ用のテンプレート化された XAML コントロール](xaml-templated-controls-csharp-winui-3.md)」を参照してください。 テンプレート化されたコントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |
| リソース ディクショナリ (WinUI) | C++ および C# | 空の、XAML リソースのキー付きコレクションを追加します。 詳細については、「[ResourceDictionary と XAML リソースの参照](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)」を参照してください。 |
| リソース ファイル (WinUI) | C++ および C# | アプリ用の文字列および条件付きリソースを格納するファイルを追加します。 この項目を使用して、アプリをローカライズすることができます。 詳細については、「[UI とアプリ パッケージ マニフェスト内の文字列をローカライズする](/windows/uwp/app-resources/localize-strings-ui-manifest)」を参照してください。 |
| ユーザー コントロール (WinUI) | C++ および C# | WinUI ライブラリの **Microsoft.UI.Xaml.Controls.UserControl** クラスから派生するユーザー コントロールを作成するための XAML ファイルとコード ファイルを追加します。 通常、ユーザー コントロールによって関連する既存のコントロールがカプセル化され、独自のロジックが提供されます。<p></p>ユーザー コントロールの詳細については、「[カスタム XAML コントロール](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)」を参照してください。 |

### <a name="visual-studio-support"></a>Visual Studio のサポート

ホット リロード、ライブ ビジュアル ツリー、ライブ プロパティ エクスプローラーなど、WinUI 3 Preview 4 に追加された最新のツール機能を利用するには、Visual Studio の最新プレビュー バージョンを最新の WinUI 3 プレビューと共に使用し、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI ツールを有効にする必要があります。 次の表は、将来のバージョンと WinUI 3 Preview 4 の互換性を示したものです。

| VS バージョン  | WinUI 3 Preview 4  |
|---|---|
| 16.8 RTM  | いいえ   |
| 16.9 プレビュー  | はい  | 
| 16.9 RTM  | いいえ   |
| 16.10 プレビュー  | はい   |

## <a name="capabilities-introduced-in-preview-4"></a>Preview 4 で導入された機能

- WinUI 2.5 との均衡 (InfoBarコントロール、ProgressRing と NavigationView の新機能、バグ修正など)
- カスタム タイトル バー機能: 開発者がデスクトップ アプリでカスタム タイトル バーを作成できる新しい Window.ExtendsContentIntoTitleBar および Window.SetTitleBar API。
- VirtualSurfaceImageSource のサポート

## <a name="list-of-bugs-fixed-in-preview-4"></a>Preview 4 で修正されたバグの一覧

Preview 3 以降にチームが修正したユーザーに関するバグの一覧を次に示します。 他にも、安定化とテストの改良に関する多くの作業が行われています。

- このリリースには新しいバージョンの CS/WinRT と Windows SDK が組み込まれており、次のバグが修正されました。
  - {Binding} を使用して URI のプロパティにバインドしたときのクラッシュ
  - .NET 5 と正しく相互運用されない C#/WinRT のマーシャリング関数

- Windows Insider ビルドで実行したときの WinUI 3 のクラッシュ
  - GitHub でこのバグを報告してくださったコミュニティの複数の共同作成者に感謝します。 
- WebView2 によって、ホスト アプリの言語とロケールが CoreWebView2Environment に適用されない
- Windows コミュニティ ツールキットの DataGrid コントロールにより、開始時およびスクロール バーの表示時にアプリがクラッシュする
  - GitHub でこのバグを報告してくださったコミュニティの複数の共同作成者に感謝します。
- 表示モードが変更されるとページ レンダリングが不適切な状態になる
- CalendarView で言語 ComboBox を使用するとクラッシュする
- WinUI 3 デスクトップ: Tab キーを押して WebView2 から移動できない
- WinUI 3 デスクトップ: 派生した TreeViewNodes が含まれる TreeView がクラッシュする 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2699) に提出してくださった @eleanorleffler に感謝します。
- WinUI 3 デスクトップ: ContentDialog 内の TextBox にテキストを入力できない 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2704) に提出してくださった @eleanorleffler に感謝します。
- WinUI 3 デスクトップ: ALT と F6 が機能しない
- 削除された古い SwapChainPanel が新しい SwapChain 上にレンダリングされる
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2942) に提出してくださった @dotMorten に感謝します。
- WinUI 3 デスクトップ: トラックパッドを使用してスクロールできない
- 同じスレッドの複数のウィンドウで NavigationView コントロールを使用するとクラッシュする
- アクセシビリティの問題: WinUI デスクトップ アプリの起動時にフォーカスを示す四角形が表示される
- DataGrid 内でスクロールしている間のアクセス違反
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2946) に提出してくださった @TroelsL に感謝します。
- WinUI 3 デスクトップ: タブの循環が機能しない 
- WinUI XAML Islands を使用するデスクトップ アプリケーションで、GridView でのドラッグ アンド ドロップが失敗する
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3871) に提出してくださった @smk2007 に感謝します。
- アクセシビリティの問題: WinUI 3 デスクトップで PageUp および PageDown キーを使用してスクロールできない
- WebView2 のビューポートのサイズが正しくない
- MenuFlyout アウトを開いた後でクリックすると WebView2 がクラッシュする
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3729) に提出してくださった @sudongg に感謝します。
- WinUI 3 デスクトップ: DropDownButton または SplitButton のポップアップを開こうとすると、アプリがクラッシュする
- WebView2: マウスの右ボタンをダブルクリックするとクラッシュする
- ToggleSplitButton をクリックすると、アプリケーションがクラッシュする
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3641) に提出してくださった @lhak に感謝します。
- WinUI 3 デスクトップ: 空の DesktopWindowXamlSource ウィンドウがタスク バーに表示される
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3698) に提出してくださった @bridgesquared に感謝します。
- WinUI 3 デスクトップ: DataGrid が表示されない
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2703) に提出してくださった @eleanorleffler に感謝します。
- WinUI 3 デスクトップ: ファイルをグリッドにドロップできない 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/2715) に提出してくださった @eleanorleffler に感謝します。
- WinUI 3 デスクトップ: WinUI 3 Preview 2 で ItemsRepeater がクラッシュする 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3007) に提出してくださった @hshristov に感謝します。
- バインドを更新すると AccessViolationException がスローされる
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3680) に提出してくださった @WamWooWam に感謝します。
- WinUI 3 デスクトップ: NavigationView をスクロールするとアプリがクラッシュする
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3598) に提出してくださった @Berkunath に感謝します。
- ItemsSource コレクション内の項目を動的に追加または削除したときに、その ItemsControl が更新されない。 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3517) に提出してくださった @VigneshRameshh に感謝します。
- C++ 準拠モードが有効になっている場合の、App.xaml.g.h でのコンパイル エラー C2760 
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3716) に提出してくださった @boostafazoo に感謝します。


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>過去の WinUI 3 プレビューで導入された新機能

次の機能は、WinUI 3 Preview 1 - 3 で導入されたものであり、WinUI 3 Preview 4 で引き続きサポートされます。

- WinUI を使用してデスクトップ アプリを作成する機能 (Win32 アプリ用の [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0) を含む)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView の更新](/windows/uwp/design/controls-and-patterns/tab-view)
- ダーク テーマの更新
- [WebView2](/microsoft-edge/hosting/webview2) の機能強化と更新
  - 高 DPI のサポート
  - ウィンドウのサイズ変更と移動のサポート
  - より新しいバージョンの Edge を対象とするよう更新されました
  - WebView2 固有の Nuget パッケージの参照が不要になりました
- SwapChainPanel
- MRT Core のサポート
  - これにより、スタートアップ時にアプリの速度と軽量化が向上し、リソース検索が迅速になります。
- ARM64 のサポート
- アプリの内部および外部でのドラッグ アンド ドロップ
- RenderTargetBitmap (現時点では XAML コンテンツのみ - SwapChainPanel コンテンツなし)
- カスタム カーソル サポート
- オフスレッド入力
- ツール/開発者エクスペリエンスの向上:
  - ライブ ビジュアル ツリー、ホット リロード、ライブ プロパティ エクスプローラー、および類似のツール
  - WinUI 3 用の IntelliSense
- オープン ソースの移行に必要な機能強化

WinUI 3 および WinUI のロードマップと利点の詳細については、GitHub の「[Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)」を参照してください。

### <a name="provide-feedback-and-suggestions"></a>フィードバックやご提案の送信

[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)にフィードバックをお寄せください。

### <a name="whats-coming-next"></a>今後予定された機能

詳細な[機能のロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)を参照して、特定の機能が WinUI 3 に導入されるタイミングを確認してください。 

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

Preview 4 リリースはプレビュー版にすぎません。 デスクトップ アプリに関するシナリオは特に新しいものです。 バグ、制限事項、およびその他の問題があることを想定してください。

次の項目は、WinUI 3 Preview 4 に関する既知の問題の一部です。 以下に列挙していない問題が見つかった場合は、[WinUI の GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)で、既存の問題に投稿するか、または新規の問題として投稿して、Microsoft にお知らせください。

### <a name="platform-and-os-support"></a>プラットフォームと OS のサポート

WinUI 3 Preview 4 は、Windows 10 April 2018 Update (バージョン 1803 - ビルド 17134) 以降を実行する PC と互換性があります。

### <a name="developer-tools"></a>開発者用ツール

- C# および C++/WinRT アプリのみがサポートされています
- デスクトップ アプリは .NET 5 および C# 9 をサポートしており、MSIX アプリ内でパッケージ化する必要があります
- UWP アプリは .NET Native および C# 7.3 をサポートしています
- Visual Studio バージョン 16.8 では、開発者ツールと Intellisense が正しく機能しない可能性があります。
- XAML デザイナーはサポートされていません
- 新しい C++ /cx アプリはサポートされていませんが、既存のアプリは引き続き機能します ( C++ /WinRT にできるだけ早く移行してください)
- デスクトップ アプリでの複数のウィンドウのサポートは進行中ですが、まだ完全でなく安定していません。
  - 複数のウィンドウの動作に新しい問題や不具合が見つかった場合は、リポジトリにバグを報告してください。
- パッケージ化されていないデスクトップ展開はサポートしていません
- F5 キーを使用してデスクトップ アプリを実行する場合は、パッケージ プロジェクトを実行していることを確認してください。 アプリ プロジェクトで F5 キーを押すと、パッケージ化されていないアプリが実行されます。これは、WinUI 3 ではまだサポートされていません。

### <a name="missing-platform-features"></a>ラットフォーム機能が不足しています

- Xbox のサポート
- HoloLens のサポート
- ウィンドウありのポップアップ
  - 具体的には、プロパティの値に関係なく、`ShouldConstrainToRootBounds` プロパティは常に、`true` に設定されているかのように動作します。
- 手描き入力のサポート
- アクリル
- MediaElement および MediaPlayerElement
- MapControl
- SwapChainPanel および XAML 以外のコンテンツ用の RenderTargetBitmap
- SwapChainPanel は透明度をサポートしていません
- グローバルな表示でフォールバック動作 (純色ブラシ) を使用します
- 今回のリリースでは XAML Islands はサポートしていません
- サードパーティ製エコシステム ライブラリは完全には機能しません
- IME は機能しません
- CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher、およびそれらの依存関係は、デスクトップ アプリではサポートされていません (下記参照)

#### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>デスクトップ アプリの CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher

Preview 4 での新機能である [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)、
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher)、およびそれらの依存関係は、デスクトップ アプリでは使用できません。

たとえば、[Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) プロパティは常に null ですが、Window.DispatcherQueue プロパティを代わりに使用できます。

これらの API は、UWP アプリでのみ機能します。
以前のプレビューでは、デスクトップ アプリでも部分的に機能していましたが、Preview 4 では完全に無効になっています。
これらの API は、スレッドごとにウィンドウが 1 つだけ存在する UWP の場合向けに設計されており、WinUI 3 の機能の 1 つは、複数を有効にすることです。

これらの API の存在に内部的に依存する API があり、そのためデスクトップ アプリではサポートされません。 通常、これらの API には静的な `GetForCurrentView` メソッドがあります。 たとえば、[UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView) などです。


### <a name="known-issues"></a>既知の問題

- Alt + F4 を押したとき、デスクトップ アプリ ウィンドウは閉じません。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow) が変更されたため、次の WinRT API は **デスクトップ** アプリで意図したとおりに動作しなくなる可能性があります。
  - [`ApplicationView`](/uwp/api/windows.ui.viewmanagement.applicationview) および関連するすべての API は動作しなくなります。
  - [`CoreApplicationView`](/uwp/api/windows.applicationmodel.core.coreapplicationview) および関連するすべての API は動作しなくなります。
  - すべての `GetForCurrentView` API は、サポートされない可能性があります ([`CoreInputView.GetForCurrentView`](/uwp/api/Windows.UI.ViewManagement.Core.CoreInputView.GetForCurrentView) など)。
  - [`CoreWindow.GetForCurrentThread`](/uwp/api/Windows.UI.Core.CoreWindow.GetForCurrentThread) は null を返すようになります。

  WinUI 3 デスクトップ アプリでの WinRT API の使用に関する詳細については、「[デスクトップ アプリで使用できる Windows ランタイム API](../../desktop/modernize/desktop-to-uwp-supported-api.md
)」を参照してください。

- [UISettings.ColorValuesChanged Event](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) と [AccessibilitySettings.HighContrastChanged Event](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) は、デスクトップ アプリでサポートされなくなります。 これにより、Windows テーマでの変更を検出するためにそれを使用している場合、問題が発生する可能性があります。 

- このリリースには、試験的な API がいくつか含まれています。 これらはチームによって完全にはテストされていないため、不明な問題が発生する可能性があります。 問題が発生した場合は、リポジトリで[バグを報告](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)してください。 

- 以前は、CompositionCapabilities インスタンスを取得するには、[CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview) を呼び出していました。 しかし、この呼び出しから返される機能は、ビューに依存して "*いませんでした*"。 これを解決して反映するため、このリリースでは静的な GetForCurrentView () を削除したので、[CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) オブジェクトを直接作成できるようになりました。

- C# UWP アプリの場合:

  WinUI 3 フレームワークは、C++ (C++/WinRT を使用) または C# から使用できる WinRT コンポーネントのセットです。 C# を使用する場合、アプリ モデルに応じて 2 つのバージョンの .NET があります: UWP アプリで WinUI 3 を使用する場合は .NET Native を使用し、デスクトップ アプリで使用する場合は .NET 5 を使用しています (および C#/WinRT)。

  UWP で WinUI 3 アプリに C# を使用する場合は、WinUI 3 デスクトップ アプリまたは C# WinUI 2 アプリでの C# と比較して、いくつかの API 名前空間の違いがあります: 一部の種類は `System` 名前空間ではなく `Microsoft` 名前空間です。 たとえば、`INotifyPropertyChanged` インターフェイスは、`System.ComponentModel` 名前空間ではなく、`Microsoft.UI.Xaml.Data` 名前空間にあります。 

  この方法は、次の対象に適用されます。
    - `INotifyPropertyChanged` (および関連する型)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 名前空間バージョンは依然として存在しますが、WinUI 3 では使用できません。 これは、`ObservableCollection` が、WinUI 3 C# UWP アプリではそのまま機能しないことを意味します。 この回避策については、[XAML コントロール ギャラリーのサンプル](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)の [CollectionsInterop のサンプル](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)を参照してください。

## <a name="xaml-controls-gallery-winui-3-preview-4-branch"></a>XAML コントロール ギャラリー (WinUI 3 Preview 4 ブランチ)

すべての WinUI 3 Preview 4 コントロールおよび機能を含むサンプル アプリについては、[XAML コントロール ギャラリーの WinUI 3 Preview 4 ブランチ](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)を参照してください。

![WinUI 3 Preview 4 XAML コントロール ギャラリー アプリ](images/WinUI3XamlControlsGallery.PNG)<br/>
*WinUI 3 Preview 4 XAML コントロール ギャラリー アプリの例*

サンプルをダウンロードするには、次のコマンドを使用して **winui3preview** ブランチを複製します。

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製した後、確実に、ローカルの Git 環境で **winui3preview** ブランチに切り替えてください。 

```
git checkout winui3preview
```