---
title: WinUI 3 Project Reunion 0.5 (2021 年 3 月)
description: WinUI 3 Project Reunion 0.5 の概要。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 8e6efbe97e37d7184555b96099952f6b32ea9cb2
ms.sourcegitcommit: df14e7768acdb243190e3418db5afa5d65c5ff88
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2021
ms.locfileid: "107574637"
---
# <a name="windows-ui-library-3---project-reunion-05-march-2021"></a>Windows UI ライブラリ 3 - Project Reunion 0.5 (2021 年 3 月)

Windows UI ライブラリ (WinUI) 3 は、最新の Windows アプリのビルドに使用するネイティブのユーザー エクスペリエンス (UX) フレームワークです。  これは、Windows オペレーティング システムとは別に、[Poject Reunion](../../project-reunion/index.md) の一部として出荷されます。  Project Reunion 0.5 リリースには、WinUI 3 ベースのユーザー インターフェイスを使用したアプリのビルドの開始に役立つ [Visual Studio プロジェクト テンプレート](https://aka.ms/projectreunion/vsixdownload)が用意されています。

**WinUI 3 - Project Reunion 0.5** は、Microsoft Store に発行できる実稼働アプリの作成に使用できる、最初のサポートされている安定したバージョンの WinUI 3 です。 このリリースは、WinUI 3 の上位互換と本稼動を可能にする、安定性の更新と全般的な機能強化で構成されています。


## <a name="install-winui-3---project-reunion-05"></a>WinUI 3 - Project Reunion 0.5 をインストールする

この新しいバージョンの WinUI 3 は、Project Reunion 0.5 の一部として利用できます。 インストールするには、次を参照してください。

**[Project Reunion 0.5 のインストール手順](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)**

WinUI は Project Reunion の一部として出荷されるので、作業を開始するには Project Reunion Visual Studio Extension (VSIX) をダウンロードします。これには、開発者用のツールとコンポーネントのセットが含まれています。 Project Reunion パッケージの詳細については、「[Project Reunion を使用するアプリを展開する](../../project-reunion/deploy-apps-that-use-project-reunion.md)」を参照してください。 Project Reunion VSIX には、WinUI 3 アプリのビルドに使用する [WinUI プロジェクト テンプレート](winui-project-templates-in-visual-studio.md)が含まれています。 

> [!NOTE]
> WinUI 3 のコントロールと機能の動作を確認するには、GitHub から WinUI 3 バージョンの [XAML コントロール ギャラリー](#winui-3-controls-gallery)を複製してビルドします。

> [!NOTE]
> ライブ ビジュアル ツリー、ホット リロード、ライブ プロパティ エクスプローラーなどの WinUI 3 のツールを使用するには、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI 3 ツールを有効にする必要があります。

開発環境を設定したら、[Visual Studio の WinUI 3 プロジェクト テンプレート](winui-project-templates-in-visual-studio.md)を参照して、使用可能な Visual studio プロジェクトと項目テンプレートについて理解してください。

WinUI 3 アプリのビルドの概要については、次の記事を参照してください。

- [デスクトップ アプリ用の WinUI 3 の概要](get-started-winui3-for-desktop.md)
- [デスクトップ用の基本的な WinUI 3 アプリを構築する](desktop-build-basic-winui3-app.md)

[制限と既知の問題](#limitations-and-known-issues)を除くと、WinUI プロジェクトを使用したアプリの構築は、XAML と WinUI 2.x を使用した UWP アプリの構築と似ています。 そのため、Windows SDK の UWP アプリと **Windows.UI** WinRT 名前空間の [ガイダンス ドキュメント](/windows/uwp/design/)のほとんどを適用できます。

WinUI 3 API リファレンス ドキュメントについては、[WinUI 3 API リファレンス](/windows/winui/api)を参照してください

### <a name="webview2"></a>WebView2

この WinUI 3 リリースで WebView2 を使用するには、WebView2 ランタイムがまだインストールされていない場合は、[こちらのページ](https://developer.microsoft.com/microsoft-edge/webview2/)にある Evergreen Bootstrapper または Evergreen Standalone インストーラーをダウンロードしてください。 

### <a name="windows-community-toolkit"></a>Windows Community Toolkit

Windows コミュニティ ツールキットを使用している場合は、[最新バージョンをダウンロード](https://aka.ms/wct-winui3)してください。

### <a name="visual-studio-support"></a>Visual Studio のサポート

ホット リロード、ライブ ビジュアル ツリー、ライブ プロパティ エクスプローラーなど、WinUI 3 に追加された最新のツール機能を利用するには、Visual Studio の最新のプレビュー バージョンを使用し、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI ツールを有効にする必要があります。 次の表は、Visual Studio 2019 のバージョンと WinUI 3 - Project Reunion 0.5 の互換性を示したものです。

| VS バージョン  | WinUI 3 - Project Reunion 0.5  |
|---|---|
| 16.8  | いいえ   |
| 16.9  | はい、ただし、ホット リロード、ライブ ビジュアル ツリー、ライブ プロパティ エクスプローラーは含まれません  |
| 16.10 プレビュー  | はい、すべての WinUI 3 ツールが含まれます   |

## <a name="update-your-existing-winui-3-app"></a>既存の WinUI 3 アプリの更新

以前のプレビューまたはリリース バージョンの WinUI 3 を使用してアプリを作成した場合、最新リリースの WinUI 3 - Project Reunion 0.5 を使用するようにプロジェクトを更新できます。 手順については、「[既存のプロジェクトを最新リリースの Project Reunion に更新する](../../project-reunion/update-existing-projects-to-the-latest-release.md)」をご覧ください。

## <a name="major-changes-introduced-in-this-release"></a>このリリースで導入される主な変更点

### <a name="stable-features"></a>安定した機能

このリリースでは、Microsoft Store に出荷できる実稼働アプリに、WinUI 3 を適合させるための安定性とサポートが提供されます。 これには、過去のプレビューで導入されたほとんどの機能のサポートと上位互換性が含まれます。

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
- カスタム タイトル バー機能: 開発者がデスクトップ アプリでカスタム タイトル バーを作成できる新しい [Window.ExtendsContentIntoTitleBar](/windows/winui/api/microsoft.ui.xaml.window.extendscontentintotitlebar) および [Window.SetTitleBar](/windows/winui/api/microsoft.ui.xaml.window.settitlebar) API。
- VirtualSurfaceImageSource のサポート
- アプリ内アクリル

### <a name="preview-features"></a>プレビュー機能

これは安定版のリリースであるため、このバージョンの WinUI 3 からプレビュー機能が削除されました。 最新の[プレビュー バージョンの WinUI 3](release-notes/winui3-project-reunion-0.5-preview.md) を使用すると、これらの機能に引き続きアクセスできます。 次の重要な機能はまだプレビュー段階であり、安定版に向けた作業が続いています。

- UWP サポート
  - つまり、WinUI 3 - Project Reunion 0.5 VSIX を使用して UWP アプリをビルドまたは実行することはできません。 [WinUI 3 - Project Reunion 0.5 Preview VSIX](https://aka.ms/projectreunion/previewdownload) を使用し、「[Project Reunion の概要](../../project-reunion/get-started-with-project-reunion.md)」にある開発環境の設定手順の残りに従います。 詳細については、「[Windows UI ライブラリ 3 - Project Reunion 0.5 Preview (2021 年 3 月) リリース ノート](release-notes/winui3-project-reunion-0.5-preview.md)」を参照してください。

- デスクトップ アプリでの複数ウィンドウのサポート

- 入力の検証

### <a name="provide-feedback-and-suggestions"></a>フィードバックやご提案の送信

[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)にフィードバックをお寄せください。

### <a name="whats-coming-next"></a>今後予定された機能

具体的な機能がいつ予定されているのかの詳細については、GitHub の[機能のロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)を参照してください。

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

次の項目は、WinUI 3 - Project Reunion 0.5 の既知のイシューの一部です。 以下に列挙していない問題が見つかった場合は、[WinUI の GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)で、既存の問題に投稿するか、または新規の問題として投稿して、Microsoft にお知らせください。

### <a name="platform-and-os-support"></a>プラットフォームと OS のサポート

WinUI 3 - Project Reunion 0.5 は、Windows 10 October 2018 Update (バージョン 1809 - ビルド 17763) 以降を実行する PC と互換性があります。

### <a name="developer-tools"></a>開発者用ツール

- C# および C++/WinRT アプリのみがサポートされています
- デスクトップ アプリは .NET 5 および C# 9 をサポートしており、MSIX アプリ内でパッケージ化する必要があります
- XAML デザイナーはサポートされていません
- 新しい C++ /cx アプリはサポートされていませんが、既存のアプリは引き続き機能します ( C++ /WinRT にできるだけ早く移行してください)
- パッケージ化されていないデスクトップ展開はサポートしていません
- F5 キーを使用してデスクトップ アプリを実行する場合は、パッケージ プロジェクトを実行していることを確認してください。 アプリ プロジェクトで F5 キーを押すと、パッケージ化されていないアプリが実行されます。これは、WinUI 3 ではまだサポートされていません。

### <a name="missing-platform-features"></a>ラットフォーム機能が不足しています

- Xbox のサポート
- HoloLens のサポート
- ウィンドウありのポップアップ
  - 具体的には、プロパティの値に関係なく、`ShouldConstrainToRootBounds` プロパティは常に、`true` に設定されているかのように動作します。
- インク サポート (以下を含む):
  - [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)
  - [HandwritingView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HandwritingView)
  - [InkPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)
- 背景アクリル
- MediaElement および MediaPlayerElement
- MapControl
- SwapChainPanel は透明度をサポートしていません
- グローバルな表示でフォールバック動作 (純色ブラシ) を使用します
- 今回のリリースでは XAML Islands はサポートしていません
- 既存の WinUI 以外のデスクトップ アプリで直接 WinUI 3 を使用すると、次のような制限があります。既存のアプリを移行するために現在使用できる方法は、ソリューションに **新しい** WinUI 3 プロジェクトを追加し、必要に応じてロジックを調整またはリファクタリングすることです。

- Application.Suspending は、デスクトップ アプリでは呼び出されません。 詳細については、[Application.Suspending イベント](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true )の API リファレンス ドキュメントを参照してください。 

- CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher、およびそれらの依存関係は、デスクトップ アプリではサポートされていません (下記参照)

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>デスクトップ アプリの CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher

WinUI 3 Preview 4 の新機能で今後は標準になる [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView、](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher)、およびそれらの依存関係は、デスクトップ アプリでは使用できません。 たとえば、[Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) プロパティは常に **null** ですが、**Window.DispatcherQueue** プロパティを代わりに使用できます。

これらの API は、UWP アプリでのみ機能します。 以前のプレビューでは、デスクトップ アプリでも部分的に機能していましたが、Preview 4 以降では完全に無効になっています。 これらの API は、スレッドごとにウィンドウが 1 つだけ存在する UWP の場合向けに設計されており、WinUI 3 の機能の 1 つは、複数を今後有効にすることです。

これらの API の存在に内部的に依存する API があり、そのためデスクトップ アプリではサポートされません。 通常、これらの API には静的な `GetForCurrentView` メソッドがあります。 たとえば、[UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView) などです。

影響を受ける API の詳細、およびこれらの API の回避策と代替方法については、「[WinRT API changes for desktop apps](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)」 (デスクトップ アプリの WinRT API の変更) を参照してください

### <a name="known-issues"></a>既知の問題

- [UISettings.ColorValuesChanged Event](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) と [AccessibilitySettings.HighContrastChanged Event](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) は、デスクトップ アプリでサポートされなくなります。 これにより、Windows テーマでの変更を検出するためにそれを使用している場合、問題が発生する可能性があります。 

- 以前は、CompositionCapabilities インスタンスを取得するには、[CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview) を呼び出していました。 しかし、この呼び出しから返される機能は、ビューに依存して "*いませんでした*"。 これを解決して反映するため、このリリースでは静的な GetForCurrentView () を削除したので、[CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) オブジェクトを直接作成できるようになりました。

- .NET SDK と winrt.runtime.dll のバージョンが一致しないことによるビルド エラーが発生することがあります。 回避策として、以下を試すことができます。

    - .NET SDK を最新バージョンに明示的に設定します。 これを行うには、.csproj ファイルに次の項目グループを追加してから、プロジェクトを保存します。

      ```xml
      <ItemGroup>            
          <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" RuntimeFrameworkVersion="10.0.18362.16" />
          <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" TargetingPackVersion="10.0.18362.16" />
      </ItemGroup>
      ```

    .NET 5.0.6 が 5 月に利用可能になると、これらの行を削除できることに注意してください。 


## <a name="winui-3-controls-gallery"></a>WinUI 3 コントロール ギャラリー

WinUI 3 - Project Reunion 0.5 に含まれるすべてのコントロールと機能が含まれるサンプル アプリについては、「WinUI 3 コントロール ギャラリー (以前の「_XAML コントロール ギャラリー - WinUI 3 バージョン_」)」を参照してください。

![WinUI 3 コントロール ギャラリー アプリ](images/WinUI3XamlControlsGallery.png)<br/>
*WinUI 3.0 コントロール ギャラリー アプリの例*

GitHub リポジトリを複製すると、サンプルをダウンロードできます。 これを行うには、次のコマンドを使用して **winui3** ブランチを複製します。

```
git clone --single-branch --branch winui3 https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製した後、必ずローカルの Git 環境で **winui3** ブランチに切り替えてください。 

```
git checkout winui3
```
