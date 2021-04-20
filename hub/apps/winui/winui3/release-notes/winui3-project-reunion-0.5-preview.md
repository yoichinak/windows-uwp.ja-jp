---
title: Windows UI ライブラリ 3 - Project Reunion 0.5 Preview (2021 年 3 月) リリース ノート
description: Windows UI ライブラリ 3 - Project Reunion 0.5 Preview (2021 年 3 月) リリースのリリース ノート。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: e6d6f1a2007ab09b0295de24b1761d3dbb0c9094
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730666"
---
# <a name="windows-ui-library-3---project-reunion-05-preview-march-2021-release-notes"></a>Windows UI ライブラリ 3 - Project Reunion 0.5 Preview (2021 年 3 月) リリース ノート

Windows UI ライブラリ (WinUI) 3 は、最新の Windows アプリのビルドに使用するネイティブのユーザー エクスペリエンス (UX) プラットフォームです。 WinUI 3 のこのプレビューは、デスクトップ/Win32 と UWP アプリの両方で動作します。このプレビューには、WinUI ベースのユーザー インターフェイスを備えたアプリのビルドを始めるときに役立つ Visual Studio プロジェクト テンプレートと、WinUI ライブラリが収められた NuGet パッケージが含まれています。

**WinUI 3 - Project Reunion 0.5 Preview** は、Project Reunion パッケージに含まれる WinUI 3 の最初のリリースです。 この変更と共に、このプレビュー リリースには、重要なバグ修正、安定性の向上、その他の一般的な機能強化が含まれます ( **[このリリースで導入される主な変更点](#major-changes-introduced-in-this-release)** に関するセクションを参照してください)。

> [!Important]
> この WinUI 3 Preview は、早期評価と開発者コミュニティからフィードバックを収集することを目的としています。 実稼働アプリには使用 **できません**。
>
> Project Reunion 0.5 は 3 月下旬に提供する予定で、WinUI 3 の最初の安定したサポート バージョンが含まれます。
>
> [WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml)を使用して、フィードバックを提供し、提案と問題をログに記録してください。

## <a name="install-winui-3---project-reunion-05-preview"></a>WinUI 3 Project - Reunion 0.5 Preview のインストール

このバージョンの WinUI 3 は、Project Reunion 0.5 Preview の一部として入手できます。 

インストールするには、Project Reunion の「[開発環境を設定する](../../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)」ガイドの指示に従ってください。 

WinUI 3 の以前のプレビュー バージョンとは異なり、WinUI VSIX パッケージではなく、Project Reunion VSIX パッケージをダウンロードします。 Project Reunion VSIX には、WinUI 3 アプリのビルドに使用する [WinUI プロジェクト テンプレート](../winui-project-templates-in-visual-studio.md)が含まれています。 インストール完了後の WinUI 3 アプリの開発エクスペリエンスは変わりません。

> [!NOTE]
> [XAML コントロール ギャラリー](#xaml-controls-gallery-winui-3-preview-branch)の WinUI 3 Preview バージョンをクローンしてビルドすることもできます。

> [!NOTE]
> ライブ ビジュアル ツリー、ホット リロード、ライブ プロパティ エクスプローラーなどの WinUI 3 のツールを使用するには、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio のプレビュー機能で WinUI 3 ツールを有効にする必要があります。

開発環境を設定したら、[WinUI 3 プロジェクトの作成](../winui-project-templates-in-visual-studio.md)に関するページを参照して、使用可能な Visual Studio プロジェクトと項目テンプレートについてご確認ください。 

WinUI プロジェクト テンプレートの概要については、次の記事を参照してください。

- [デスクトップ アプリ用の WinUI 3 の概要](../get-started-winui3-for-desktop.md)
- [UWP アプリ用の WinUI 3 の概要 (プレビュー)](../get-started-winui3-for-uwp.md)

[制限と既知の問題](#limitations-and-known-issues)を除くと、WinUI プロジェクトを使用したアプリの構築は、XAML と WinUI 2.x を使用した UWP アプリの構築と似ています。 そのため、Windows SDK の UWP アプリと **Windows.UI** WinRT 名前空間の [ガイダンス ドキュメント](/windows/uwp/design/)のほとんどを適用できます。

WinUI 3 API リファレンス ドキュメントについては、[WinUI 3 API リファレンス](/windows/winui/api)を参照してください

WinUI 3 Preview 4 を使用してプロジェクトを作成した場合は、Project Reunion 0.5 Preview を使用するようにプロジェクトをアップグレードできます。 詳細な手順については、[WinUI の GitHub リポジトリ](https://aka.ms/winui3/upgrade-instructions)を参照してください。

### <a name="webview2"></a>WebView2
この WinUI 3 Preview で WebView2 を使用するには、WebView2 ランタイムがまだインストールされていない場合は、[こちらのページ](https://developer.microsoft.com/microsoft-edge/webview2/)にある Evergreen Bootstrapper または Evergreen Standalone インストーラーをダウンロードしてください。 

### <a name="windows-community-toolkit"></a>Windows Community Toolkit

Windows コミュニティ ツールキットを使用している場合は、[最新バージョンをダウンロード](https://aka.ms/wct-winui3)してください。

### <a name="visual-studio-support"></a>Visual Studio のサポート

ホット リロード、ライブ ビジュアル ツリー、ライブ プロパティ エクスプローラーなど、WinUI 3 に追加された最新のツール機能を利用するには、Visual Studio の最新 **プレビュー** バージョンを最新の WinUI 3 プレビューと共に使用し、[こちらの手順](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)で説明されているように、Visual Studio Preview 機能で WinUI ツールを有効にする必要があります。 次の表は、将来のバージョンと WinUI 3 - Project Reunion 0.5 Preview の互換性を示したものです。

| VS バージョン  | WinUI 3 - Project Reunion 0.5 Preview  |
|---|---|
| 16.8 RTM  | いいえ   |
| 16.9 プレビュー  | はい (ツールあり)  | 
| 16.9 RTM  | はい。ただし、ホット リロード、ライブ ビジュアル ツリー、ライブ プロパティ エクスプローラーには非互換   |
| 16.10 プレビュー  | はい (ツールあり)   |

## <a name="major-changes-introduced-in-this-release"></a>このリリースで導入される主な変更点

- WinUI 3 は、Project Reunion パッケージに含まれるようになりました。これは、今後サポートされるリリースの提供方法にも適用されます。

- アプリ内アクリルがサポートされるようになります。

- ピボット コントロールはサポートされなくなり、WinUI 3 では非推奨とされています。 アプリ内ナビゲーション シナリオには [NavigationView コントロール](/windows/uwp/design/controls-and-patterns/navigationview)を使用することをお勧めします。

- WinUI 3 および Project Reunion は、Windows 10 バージョン 1809 以降でのみサポートされます。ビルド 17763 以降が必要です。

- プレビュー機能が試験段階としてマークされるようになりました。 
  - プレビュー機能は、引き続き WinUI 3 プレビューに含まれますが、次の WinUI 3 サポート リリースには含まれなくなります。 
  - プレビュー機能には、WinUI 2.6 プレビューの一部である試験的な API も含まれます。
  - プレビュー機能を使用するアプリをビルドすると、アプリにより警告がスローされます。 


## <a name="list-of-bugs-fixed-in-winui-3---project-reunion-05-preview"></a>WinUI 3 - Project Reunion 0.5 Preview で修正されるバグの一覧

Preview 3 以降にチームが修正したユーザーに関するバグの一覧を次に示します。 他にも、安定化とテストの改良に関する多くの作業が行われています。

- WinUI 3 エラー メッセージは次の内容に置き換える必要がある: "'Windows.metadata' を解決できません。  Windows ソフトウェア開発キットをインストールしてください。 Windows SDK は、Visual Studio と共にインストールされます。"
- Windows の既定のテーマを選択すると、アプリを再起動するまで、Windows のテーマの変更に応答しない
- XamlDirect.CreateInstance の呼び出しで例外が発生
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3509) に提出してくださった @BorzillaR に感謝します。
- ProgressBar で一時停止とエラー オプションの違いが表示されない
- StandardUICommand リスト ビューの項目のポップアップが間違った位置に表示される。
- ListView 項目の順序をタッチして変更しようとすると、デスクトップの XamlControlsGallery でクラッシュする
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3694) に提出してくださった @j0shuams に感謝します。
- RichTextBlock を押したままにすると、ポップアップが誤った場所に配置される
- RadioButtons 内のフォーカスが左/右矢印を使って移動しない
  - この[問題を GitHub](https://github.com/microsoft/microsoft-ui-xaml/issues/3385) に提出してくださった @vmadurga に感謝します。
- ユーザーが DatePicker で下/上方向キーを押して次/前の月、年、日を選択しても、ナレーターがサイレント状態のままである

- NavigationView light-dismiss が WinUI 3 で機能しない


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>過去の WinUI 3 プレビューで導入された新機能

次の機能は、WinUI 3 Preview 1 から 4 で導入され、WinUI 3 - Project Reunion 0.5 Preview で引き続きサポートされます。

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

WinUI 3 および WinUI のロードマップと利点の詳細については、GitHub の「[Windows UI Library Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)」を参照してください。

### <a name="provide-feedback-and-suggestions"></a>フィードバックやご提案の送信

[WinUI GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)にフィードバックをお寄せください。

### <a name="whats-coming-next"></a>今後予定された機能

詳細な[機能のロードマップ](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)を参照して、特定の機能が WinUI 3 に導入されるタイミングを確認してください。 

## <a name="limitations-and-known-issues"></a>制限事項と既知の問題

WinUI 3 - Project Reunion 0.5 Preview はまだプレビューです。 デスクトップ アプリに関するシナリオは特に新しいものです。 バグ、制限事項、およびその他の問題があることを想定してください。

次の項目は、WinUI 3 - Project Reunion 0.5 Preview の既知の問題の一部です。 以下に列挙していない問題が見つかった場合は、[WinUI の GitHub リポジトリ](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)で、既存の問題に投稿するか、または新規の問題として投稿して、Microsoft にお知らせください。

### <a name="platform-and-os-support"></a>プラットフォームと OS のサポート

WinUI 3 - Project Reunion 0.5 Preview は、Windows 10 October 2018 Update (バージョン 1809 - ビルド 17763) 以降を実行する PC と互換性があります。

### <a name="developer-tools"></a>開発者用ツール

- C# および C++/WinRT アプリのみがサポートされています
- デスクトップ アプリは .NET 5 および C# 9 をサポートしており、MSIX アプリ内でパッケージ化する必要があります
- UWP アプリは .NET Native および C# 7.3 をサポートしています
- Visual Studio で開発者ツールと Intellisense が正しく機能しない可能性があります。
- XAML デザイナーはサポートされていません
- 新しい C++ /cx アプリはサポートされていませんが、既存のアプリは引き続き機能します ( C++ /WinRT にできるだけ早く移行してください)
- デスクトップ アプリの複数ウィンドウのサポートは進行中ですが、まだ完全でなく安定していません。
  - 複数のウィンドウの動作に新しい問題や不具合が見つかった場合は、リポジトリにバグを報告してください。
- パッケージ化されていないデスクトップ展開はサポートしていません
- F5 キーを使用してデスクトップ アプリを実行する場合は、パッケージ プロジェクトを実行していることを確認してください。 アプリ プロジェクトで F5 キーを押すと、パッケージ化されていないアプリが実行されます。これは、WinUI 3 ではまだサポートされていません。

### <a name="missing-platform-features"></a>不足しているプラットフォーム機能

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

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>デスクトップ アプリの CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher

Preview 4 の新機能で今後は標準になる [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)、
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher)、およびそれらの依存関係は、デスクトップ アプリでは使用できません。 たとえば、[Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) プロパティは常に null ですが、Window.DispatcherQueue プロパティを代わりに使用できます。

これらの API は、UWP アプリでのみ機能します。 以前のプレビューでは、デスクトップ アプリでも部分的に機能していましたが、Preview 4 では完全に無効になっています。 これらの API は、スレッドごとにウィンドウが 1 つだけ存在する UWP の場合向けに設計されており、WinUI 3 の機能の 1 つは、複数を有効にすることです。

これらの API の存在に内部的に依存する API があり、そのためデスクトップ アプリではサポートされません。 通常、これらの API には静的な `GetForCurrentView` メソッドがあります。 たとえば、[UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView) などです。

影響を受ける API の詳細、およびこれらの API の回避策と代替方法については、「[WinRT API changes for desktop apps](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)」 (デスクトップ アプリの WinRT API の変更) を参照してください

### <a name="known-issues"></a>既知の問題

- UWP アプリが Windows 10 バージョン 1809 で起動できないという問題が発生しています。 このバグを次のプレビュー リリースで修正するため、チームは積極的に取り組んでいます。

- Alt + F4 を押しても、デスクトップ アプリ ウィンドウが閉じません。

- [UISettings.ColorValuesChanged Event](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged) と [AccessibilitySettings.HighContrastChanged Event](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged) は、デスクトップ アプリでサポートされなくなります。 これにより、Windows テーマでの変更を検出するためにそれを使用している場合、問題が発生する可能性があります。 

- このリリースには、使用時にビルド警告をスローする試験的な API がいくつか含まれます。 これらはチームによって完全にはテストされていないため、不明な問題が発生する可能性があります。 問題が発生した場合は、リポジトリで[バグを報告](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)してください。 

- 以前は、CompositionCapabilities インスタンスを取得するには、[CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview) を呼び出していました。 しかし、この呼び出しから返される機能は、ビューに依存して "*いませんでした*"。 これを解決して反映するため、このリリースでは静的な GetForCurrentView () を削除したので、[CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) オブジェクトを直接作成できるようになりました。

- C# UWP アプリの場合:

  WinUI 3 フレームワークは、C++ (C++/WinRT を使用) または C# から使用できる WinRT コンポーネントのセットです。 C# を使用する場合、アプリ モデルに応じて 2 つのバージョンの .NET があります: UWP アプリで WinUI 3 を使用する場合は .NET Native を使用し、デスクトップ アプリで使用する場合は .NET 5 (および C#/WinRT) を使用します。

  UWP で WinUI 3 アプリに C# を使用する場合は、WinUI 3 デスクトップ アプリまたは C# WinUI 2 アプリでの C# と比較して、いくつかの API 名前空間の違いがあります: 一部の種類は `System` 名前空間ではなく `Microsoft` 名前空間にあります。 たとえば、`INotifyPropertyChanged` インターフェイスは、`System.ComponentModel` 名前空間ではなく、`Microsoft.UI.Xaml.Data` 名前空間にあります。 

  この方法は、次の対象に適用されます。
    - `INotifyPropertyChanged` (および関連する型)
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 名前空間バージョンは依然として存在しますが、WinUI 3 では使用できません。 これは、`ObservableCollection` が、WinUI 3 C# UWP アプリではそのまま機能しないことを意味します。 この回避策については、[XAML コントロール ギャラリーのサンプル](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)の [CollectionsInterop のサンプル](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)を参照してください。

## <a name="xaml-controls-gallery-winui-3-preview-branch"></a>XAML コントロール ギャラリー (WinUI 3 Preview ブランチ)

WinUI 3 - Project Reunion 0.5 Preview に含まれるすべてのコントロールと機能を使用するサンプル アプリについては、「[WinUI 3 Preview branch of the XAML Controls Gallery](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)」 (XAML コントロールギャラリーの WinUI 3 Preview ブランチ) を参照してください。

:::image type="content" source="../images/WinUI3XamlControlsGallery.png" alt-text="WinUI 3 Preview XAML コントロール ギャラリー アプリ":::<br/>
*WinUI 3 Preview XAML コントロール ギャラリー アプリの例*

サンプルをダウンロードするには、次のコマンドを使用して **winui3preview** ブランチを複製します。

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

複製した後、確実に、ローカルの Git 環境で **winui3preview** ブランチに切り替えてください。 

```
git checkout winui3preview
```
